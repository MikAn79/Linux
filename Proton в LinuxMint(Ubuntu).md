[Другой мир](https://dzen.ru/gothicserge)

вы подписаны

# Ставим Proton без Steam в LinuxMint(Ubuntu).

Для начала владельцы видеокарт от Nvidia должны обновить драйвер, у меня такого нет, значит иду дальше. Нам нужно установить нужные зависимости.

Открываем терминал и копируем все туда, по строчкам, ввод и следующию строчку.

`sudo dpkg --add-architecture i386`

`sudo add-apt-repository multiverse`

`sudo apt install curl file libc6 libnss3 policykit-1 xz-utils zenity bubblewrap curl icoutils tar libvulkan1 libvulkan1:i386 wget zenity zstd cabextract xdg-utils openssl bc libgl1-mesa-glx libgl1-mesa-glx:i386`

*Это одна строчка*

Теперь идем [на гитхаб и качаем deb фал](https://github.com/Castro-Fidel/PortProton_dpkg/releases/tag/portproton_1.0-2_amd64), я специально даю ссылку не на файл, а не страницу, потому что версия может обновиться.

### Важный момент

ПортПротон будет пытаться установить не туда, куда надо.

<img width="600" height="433" src="../_resources/scale_1200_945f254973cf4c25a7d73e9a7ed52e61.png" class="jop-noMdConv">

Нам нужно указать "правильный" адрес:

<img width="600" height="444" src="../_resources/scale_1200_5b39124d80d84280bfcd7718580876d2.png" class="jop-noMdConv">

Зачем так сделано \- не знаю, если установить по умолчанию у вас программа запуститься только один раз, потому что установиться во временную папку. Скорее всего это для запуска программ не требующих инсталляции.

### Где программа и как пользоваться?

Найти установленную программу можно по адресу :

/home/ваш домашний каталог/PortWINE/PortProton/prefixes/DEFAULT/drive_c/Program Files (x86)/программа

<img width="600" height="209" src="../_resources/scale_1200_74476805a57f460d902f749a4dda5eb5.png" class="jop-noMdConv">

Далее находим exe шник и запускаем его по правой кнопке с помощью портпротон. Так же можно создать ярлык и потом вынести его на рабочий стол.

<img width="600" height="380" src="../_resources/scale_1200_117111c0a15c46ffa6d262ef22d0c52b.png" class="jop-noMdConv">

Вот, пользуйтесь. Еще одна вам альтернатива классического wine