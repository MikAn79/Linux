И не только.

Для начала выполняем шаги, описанные в руководстве:

- [Создание ресурсов общего доступа от имени обычного пользователя](https://wiki.archlinux.org/index.php/samba_%28%D0%A0%D1%83%D1%81%D1%81%D0%BA%D0%B8%D0%B9%29#.D0.A1.D0.BE.D0.B7.D0.B4.D0.B0.D0.BD.D0.B8.D0.B5_.D1.80.D0.B5.D1.81.D1.83.D1.80.D1.81.D0.BE.D0.B2_.D0.BE.D0.B1.D1.89.D0.B5.D0.B3.D0.BE_.D0.B4.D0.BE.D1.81.D1.82.D1.83.D0.BF.D0.B0_.D0.BE.D1.82_.D0.B8.D0.BC.D0.B5.D0.BD.D0.B8_.D0.BE.D0.B1.D1.8B.D1.87.D0.BD.D0.BE.D0.B3.D0.BE_.D0.BF.D0.BE.D0.BB.D1.8C.D0.B7.D0.BE.D0.B2.D0.B0.D1.82.D0.B5.D0.BB.D1.8F), или
- [Creating usershare path](https://wiki.archlinux.org/index.php/samba#Creating_usershare_path)

Если коротко и коспективно:

1.  Создаём директорию `usershare`: `mkdir -p /var/lib/samba/usershare`
2.  Создаём группу `sambashare`: `groupadd sambashare`
3.  Правим права доступа к директории:\`\`\` chown root:sambashare /var/lib/samba/usershare chmod 1770 /var/lib/samba/usershare

```
  1. Проверяем конфигурацию `smb.conf`:```
...
[global]
  usershare path = /var/lib/samba/usershare  usershare max shares = 100  usershare allow guests = yes  usershare owner only = yes  ...
```

1.  Разрешаем вашему пользователю создавать шары: `usermod -a -G sambashare $USER`
2.  Рестартуем демоны `smbd` и `nmbd`
3.  Перелогиниваемся в систему

Как минимум теперь вы сможете управлять пользовательскими шарами, используя командную строку:

Для использования этого функционала через Dolphin, потребуется поставить пакет `kdenetwork-filesharing`: pacman -S kdenetwork-filesharing

После чего в свойствах директории появится вкладка, ответственная за общий доступ к содержимому директории.