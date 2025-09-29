[TOC]

# Запуск Android приложений на Linux

Обновлено: 22 июня, 2018 Опубликовано: 21 июня, 2018 от [Drako](https://losst.pro/author/drako "Просмотр всех записей Drako") , 33 комменариев, время чтения: 7 минут, 27 секунд

Эмуляторами и виртуальными машинами на сегодняшний день сложно кого-то удивить. Зачастую это уже не инструмент системного администратора, который в свитере и с бородой укрощает демонов. Эмуляторы используют разработчики и геймеры. На них тестируют приложения Android до выхода в свет - это проще, чем держать под рукой дюжину моделей смартфонов.

Сегодня мы поговорим о том, как можно запустить приложения Android на Linux, а именно в операционной системе Ubuntu 18.04. Оказывается, для этого есть несколько путей.


## Запуск Android-приложений в ОС Linux

### 1\. Установка эмулятора Anbox

Первый метод запуска приложения Android на Linux — это использование Anbox. Это эмулятор Android с открытым исходным кодом, который устанавливается несколькими командами и позволяет запускать Android-приложения на Ubuntu.

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.12.53.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.12.53.png)

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.14.08.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.14.08.png)

Для начала заходим в терминал и обновляем список пакетов следующей командой:

`sudo apt-get update`

После этого начинаем установку Anbox. Сначала подключаем репозиторий:

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.36.24.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.36.24.png)
`sudo add-apt-repository ppa:morphis/anbox-support`

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.37.09.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.37.09.png)

Когда система спросит — нажимаем Enter. Ещё раз выполняем обновление списка пакетов:

`sudo apt update`

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.38.58.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.38.58.png)

После этого устанавливаем ядро Anbox

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.41.54.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.41.54.png)
`sudo apt install anbox-modules-dkms`

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.42.36.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.42.36.png)

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.44.24.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.44.24.png)

После установки нужно один раз вручную запустить модули ядра. В будущем они будут стартовать сами.

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.45.21.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.45.21.png)

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.45.52.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.45.52.png)

Выполняем:
`sudo modprobe ashmem_linux`
`sudo modprobe binder_linux`

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.46.34.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.46.34.png)

Устанавливаем Android Debug Bridge. Он нужен для корректной работы Android-приложений:

`sudo apt install android-tools-adb android-tools-fastboot`

Теперь устанавливаем, собственно, Anbox. Важно отметить, что пока это бета-версия, потому в ней возможны ошибки.

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.47.30.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.47.30.png)

А установка производится в виде snap-, а не классических пакетов. Выполняем в терминале:
`snap install --devmode --beta anbox`
[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.48.26.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.48.26.png)

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.50.08.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.50.08.png)

Маловероятно, но система может ругнуться «snap not found». Установите snap командой:

`sudo apt install snapd`

И попробуйте ещё раз. В процессе установки будет затребован пароль.

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.47.57.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-10.47.57.png)

### 2\. Использование эмулятора Anbox

После установки можно добавлять или удалять файлы с расширением APK. Это установочные пакеты программ на Android.

Для установки программы Android в Linux используем:

`sudo adb install /the/location/of/file.apk`

Для удаления, соответственно:

`sudo adb uninstall /the/location/of/file.apk`

Запускать Anbox можно как из списка программ, так и из терминала:

`anbox session-manager`

### 3\. Установка эмулятора Genymotion

Второй эмулятор на сегодня весьма интересен. Начнём с того, что он — облачный. Во-вторых, он платный для коммерческого использования. Хотя в нашем случае хватит и бесплатной лицензии.

Наконец, Genymotion предлагает эмуляцию уже готовых моделей смартфонов и планшетов, что очень хорошо для разработчиков. Можно сразу протестировать программу в разных режимах.

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.05.59.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.05.59.png)

Для начала идём на [сайт Genymotion](https://www.genymotion.com), регистрируемся там и получаем на почту сообщение с подтверждением о регистрации. В нём кликаем по ссылке, чтобы подтвердить адрес и активизировать учётку.

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.07.05.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.07.05.png)

После тыкаем по красной кнопке **Trial** в верхнем правом углу.

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.16.34-1024x513.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.16.34.png)

Нас перебрасывает на новую страницу, откуда можно скачать необходимый файл. В нашем случае это файл с расширением bin. Запускаем закачку, а пока она идёт, установим VirtualBox (если вдруг он не установлен).

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.25.45.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.25.45.png)

В терминале набираем:

`sudo apt install virtualbox`

Жмём Enter, вводим пароль, когда система попросит подтверждения, нажимаем **y**. По окончании установки набираем в терминале:

`ls`

Эта команда выведет список директорий. Нам нужна **Downloads** или та, куда у вас по умолчанию скачиваются файлы. Вводим:

`cd Downloads/`

Жмём Enter, нас перебрасывает в папку. Снова вводим:

`ls`

Это даёт нам список файлов в папке. Нас интересует genymotion-2.12.1-linux\_x64.bin (в случае 32-разрядной системы genymotion-2.12.1-linux\_x32.bin). Вводим:

`sudo chmod +x genymotion-2.12.1-linux_x64.bin`

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.31.59.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.31.59.png)

Здесь можно ввести первые буквы имени файла и нажать **Tab**. Жмём Enter. Иначе это можно сделать, зайдя в папку Downloads, кликнуть правой кнопкой по файлу и дать ему права на выполнение.

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.33.17.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.33.17.png)

После этого устанавливаем:

`sudo ./genymotion-2.12.1-linux_x64.bin`

Нажимаем Enter, потом **y**, далее:

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.34.13.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.34.13.png)

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.35.57.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.35.57.png)
`cd /opt/genymobile/genymotion` `./genymotion`

По нажатию Enter программа запускается. Теперь выбираем **Sign in** и авторизуемся с теми данными, с которыми мы регистрировались на сайте. Можно сразу выбрать **Personal Use** и авторизоваться уже там.

Принимаем условия **EULA** и — вуаля! У нас готовая к использованию система. Теперь вы знаете, как запустить Android приложение на Linux.
<a id="Chrome"></a>

### 4\. Запуск Android-приложений в Google Chrome Linux

В браузере для этого используется ряд расширений — chromeos-apk и ARChon. Сама же эмуляция функционирует, используя библиотеку Chrome App Runtime. Первоначально она появилась в Chrome OS, когда туда добавили поддержку Android. Для начала  устанавливаем именно нестабильную версию Chrome (до релиза). В Терминале выполняем:

`sudo nano /etc/apt/sources.list.d/google-chrome.list`

Откроется окно Nano, туда вписываем следующее:

`deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main`

Записываем изменения комбинацией **Ctrl+O**, выходим \- **Ctrl+X**. Важно! При выходе без предварительной записи данные **не сохраняются!**

Далее в терминале вводим:

`wget https://dl.google.com/linux/linux_signing_key.pub`

Эта команда скачает ключ для доступа к deb-пакету. Теперь вводим команду, которая ниже, это добавит ключ:

`sudo apt-key add linux_signing_key.pub`

Обновляем список пакетов:

`sudo apt update`

После этого устанавливаем уже нестабильную версию браузера:

`sudo apt install google-chrome-unstable`

[![](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.50.33.png)](https://losst.pro/wp-content/uploads/2018/06/Ubuntu-Ubuntu-kodeki-Rabotaet-Oracle-VM-VirtualBox-2018-06-16-11.50.33.png)

Первый запуск браузера будет долгим, это нормально. Можно установить его по умолчанию, если хочется.

- После запуска [скачиваем](https://archon-runtime.github.io) и извлекаем из архива содержимое ARChon в любую директорию (можно даже по умолчанию);
- Открываем Chrome и в настройках активируем **Developer mode**. Для этого идём в chrome://extensions/ или же открываем меню (которое с тремя точками);
- Кликаем по **Load unpacked extension** и указываем распакованный ARChon. После этого расширение устанавливается.

Однако это ещё не всё. Теперь необходимо подготовить APK для установки (а вы думали, всё будет просто?). Сначала установим расширение chromeos-apk. Для этого инсталлируем библиотеку lib32stdc++6 командой в терминале:

`sudo apt install lib32stdc++6`

Сhromeos-apk устанавливаем, используя менеджер пакетов npm в том же терминале:

`npm install chromeos-apk -g`

Готово, теперь можно корректно распаковывать APK с помощью chromeos-apk и переводить его в расширение. В терминале вводим команду:

`chromeos-apk path/to/file.apk`

У нас получилось расширение для браузера Chrome. Открываем его в chrome://extensions/, после чего запускаем. Если вы всё сделали правильно, новенькая программа должна запуститься.

Важно, что в этом варианте будут работать далеко не все приложения. Ведь, по сути, это пока тестовая возможность, а не релиз. Скорее всего «не заведутся» игры, системные приложения и просто «тяжёлые» программы.
