На работе сетевой МФУ. Сканируя документы он складывает готовые файлы в настроенный заранее каталог. Делать это он может по протоколу SMB или FTP. Я 

уже писал про [настройку Samba в Ubuntu](https://d1mon.com/n/1684). Теперь про Fedora. Здесь есть свои особенности.

Установка и активация Samba:

```
sudo dnf install samba
sudo systemctl enable smb --now
```

Настройка фаервола:

```
firewall-cmd --get-active-zones
sudo firewall-cmd --permanent --zone=FedoraWorkstation --add-service=samba
sudo firewall-cmd --reload
```

Как и в предыдущей статье я создаю для доступа специального пользователя и создаю сам каталог для складирования. Каталог размещаю за пределами домашнего каталога этого пользователя, хотя может правильнее было бы в нём. В 

общем не принципиально, делайте, как удобнее.

Создаём пользователя `share-user` без домашнего каталога:

```
sudo useradd -d /dev/null share-user
```

Задаём пароль этому пользователю:

```
sudo passwd share-user
```

Пользователя и пароль также внесём в базу данных Samba:

```
sudo smbpasswd -a share-user
```

Создаём каталог, например, `/mnt/share`, обязательно от имени и группы этого пользователя.

Лучше сразу добавить своего обычного пользователя в группу share-user, чтобы проще было потом работать с файлами, для применения изменений нужно перезайти или перезапустить компьютер:

```
sudo usermod -aG share-user имя_вашего_пользователя
```

Настраиваем SELinux, иначе каталог будет недоступен для записи:

```
sudo semanage fcontext --add --type "samba_share_t" /mnt/share
sudo restorecon -R /mnt/share
```

Пример конфига `/etc/samba/smb.conf`:

```
[global]
security = user
passdb backend = tdbsam
workgroup = WORKGROUP
server min protocol = NT1
server string = MySMB

[share]
comment =  MySMB Share
path = /mnt/share
valid users = @share-user
force group = share-user
create mask = 0771
directory mask = 0771
writable = yes
browseable = yes
write list = user
```

Группу `workgroup` укажите в соответствии со своей.

Параметр `server min protocol = NT1` лучше не ставить, если и без него у вас будет работать. Дело в том, что есть разные версии протокола SMB. Часто бывает, что даже не сильно старые МФУ не поддерживают новые версии протокола. Данным параметром мы разрешаем подключение с использованием SMB v1.

Перезапуск Samba для применения изменений:

```
sudo systemctl restart smb
```

Стоит попробовать подключиться. Можно прямо со своего же компьютера, через программу «Файлы». Ссылка для подключения имеет вид `smb://pc-name/share`. При этом вам надо будет указать пользователя `share-user` и его пароль. Если подключение будет успешным, обязательно проверьте возможность записи в каталоге. Просто создайте, например, пустой каталог. Все ОК? Тогда можно добавлять данные SMB-доступа в МФУ.

<img width="972" height="736" src="../../_resources/smb_1b9f8fdd09d147678893f6b421fc343b.png"/>

Дополнительную информацию можно пискать [здесь](https://docs.fedoraproject.org/ru/quick-docs/samba/).