> **GQemoo**
> GUI для qemoo— скрипт-оболочка для qemu для запуска и установки гостевых систем.
> 
> Возможно, самый простой и удобный тандем для работы с виртуальными машинами в QEMU. Минимальные параметры для запуска и установки ВМ: RAM, SIZE(размер диска для установки) и количество ядер ЦП (-smp X и другие параметры) в поле QEMUADD. Идеально подходит для тестирования/установки любых дистрибутивов Linux/Windows.
> https://github.com/AKotov-dev/gqemoo

# Как установить и настроить QEMU/KVM на Ubuntu 20.04/22.04

- [![walle9054](https://linuxthebest.net/wp-content/uploads/rcl-uploads/avatars/3227-70x70.png?ver=1679483599)](https://linuxthebest.net/account-2/?user=3227)[walle9054](https://linuxthebest.net/account-2/?user=3227 "Записи пользователя walle9054")
- 2022-10-29
- [Инструкции](https://linuxthebest.net/terminal/), [Обзоры](https://linuxthebest.net/review/), [Программы](https://linuxthebest.net/programmy/)

Виртуализация является одной из наиболее широко используемых технологий как в корпоративной, так и в домашней среде. Являетесь ли вы опытным ИТ-специалистом, программистом или новичком в ИТ, виртуализация может стать одним из ваших лучших друзей.

Виртуализация — это абстрагирование аппаратных ресурсов компьютера с помощью программного приложения, известного как гипервизор. Гипервизор создает уровень абстракции над компьютерным оборудованием и виртуализирует различные компоненты системы, включая, помимо прочего, память, процессор, хранилище, USB-устройства и т. д.

При этом он позволяет создавать виртуальные компьютеры, также известные как виртуальные машины, из виртуализированных элементов, и каждая виртуальная машина, также известная как гость, работает независимо от хост-системы.

**KVM**, сокращенно от «Виртуальная машина на основе ядра», представляет собой гипервизор типа 1 с открытым исходным кодом (гипервизор «голого железа»), интегрированный в ядро ​​​​Linux. Он позволяет создавать и управлять виртуальными машинами под управлением Windows, Linux или вариантов UNIX, таких как FreeBSD и OpenBSD.

Как упоминалось ранее, каждая виртуальная машина имеет свои собственные виртуальные ресурсы, такие как хранилище, память, ЦП, сетевые интерфейсы, интерфейсы USB и видеографика, и это лишь некоторые из них.

**QEMU** (Quick Emulator) — программный модуль, эмулирующий различные компоненты компьютерного оборудования. Он поддерживает полную виртуализацию и работает вместе с KVM, обеспечивая целостный опыт виртуализации.

## Шаг 1\. Проверьте, включена ли виртуализация в Ubuntu

Для начала проверьте, поддерживает ли ваш процессор технологию виртуализации. В вашей системе должен быть процессор Intel VT-x (vmx) или процессор AMD-V (svm).

Чтобы убедиться в этом, выполните следующую команду egrep.

```
$ egrep -c '(vmx|svm)' /proc/cpuinfo
```

Если виртуализация поддерживается, вывод должен быть больше 0, например, 2,4,6 и т. д.

Кроме того, вы можете запустить следующую команду grep, чтобы отобразить тип процессора, который поддерживает ваша система. В нашем случае мы используем Intel VT-x, обозначенный параметром vmx.

```
$ grep -E --color '(vmx|svm)' /proc/cpuinfo
```

<img width="890" height="440" src="../_resources/enable-virtualization-in-ubuntu_24882bdaebe440f1ad.png" class="jop-noMdConv">

Не менее важно проверить, поддерживается ли виртуализация KVM, выполнив следующую команду:

```
$ kvm-ok
```

<img width="890" height="168" src="../_resources/check-kvm-virtualization-in-ubun_b9336844b3774c21b.png" class="jop-noMdConv">

Если утилита kvm-ok отсутствует, установите пакет cpu-checker следующим образом.

```
$ sudo apt install cpu-checker -y
```

Теперь, когда мы убедились, что наша система поддерживает виртуализацию KVM, давайте продолжим и установим QEMU.

## Шаг 2\. Установите QEMU/KVM на Ubuntu 20.04/22.04.

Затем обновите списки пакетов и репозитории следующим образом.

```
$ sudo apt update
```

После этого установите QEMU/KVM вместе с другими пакетами виртуализации следующим образом:

```
$ sudo apt install qemu-kvm virt-manager virtinst libvirt-clients bridge-utils libvirt-daemon-system -y
```

<img width="890" height="235" src="../_resources/install-qemu-in-ubuntu_22456bb198c8466d9fbfdb638ce.png" class="jop-noMdConv">

Давайте рассмотрим, какую роль играет каждый из этих пакетов.

- **qemu-kvm** — это эмулятор с открытым исходным кодом, который эмулирует аппаратные ресурсы компьютера.
- **virt-manager** — графический интерфейс на основе Qt для создания и управления виртуальными машинами с помощью демона libvirt.
- **virtinst** — набор утилит командной строки для создания и внесения изменений в виртуальные машины.
- **libvirt-clients** — API и клиентские библиотеки для управления виртуальными машинами из командной строки.
- **bridge-utils** — набор инструментов командной строки для управления мостовыми устройствами.
- **libvirt-daemon-system** — предоставляет файлы конфигурации, необходимые для запуска службы виртуализации.

На данный момент мы установили QEMU и все необходимые пакеты виртуализации. Следующим шагом является запуск и включение демона виртуализации libvirtd.

Итак, выполните следующие команды:

```
$ sudo systemctl enable --now libvirtd
$ sudo systemctl start libvirtd
```

Затем проверьте, работает ли служба виртуализации, как показано.

```
$ sudo systemctl status libvirtd
```

<img width="890" height="324" src="../_resources/start-libvirtd-virtualization-se_a1d4aaed721b469c8.png" class="jop-noMdConv">

Судя по выходным данным выше, демон libvirtd запущен и работает, как и ожидалось. Кроме того, добавьте пользователя, вошедшего в систему, в группы kvm и libvirt, как показано ниже.

```
$ sudo usermod -aG kvm $USER
$ sudo usermod -aG libvirt $USER
```

## Шаг 3: Запустите диспетчер виртуальных машин в Ubuntu

Следующим шагом является запуск графического инструмента QEMU/KVM, который является диспетчером виртуальных машин.

```
$ sudo virt-manager
```

Диспетчер виртуальных машин появится, как показано на рисунке. Отсюда вы можете начать создавать виртуальные машины и управлять ими, как мы вскоре продемонстрируем.

![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)

## Шаг 4: Создайте виртуальную машину с помощью QEMU/KVM в Ubuntu

В этом разделе мы покажем, как создать виртуальную машину с помощью ISO-образа. В демонстрационных целях мы будем использовать ISO-образ Fedora Live. Вы можете использовать ISO-образ предпочитаемой ОС и следовать инструкциям.

Чтобы начать, щелкните значок в верхнем левом углу, как показано ниже.

![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)

Поскольку мы создаем виртуальную машину из файла ISO, выберите первый вариант — «Локальный установочный носитель (образ ISO или компакт-диск)». Затем нажмите «Вперед».

![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)

Затем нажмите «Обзор», чтобы перейти к местоположению файла ISO.

![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)

Поскольку файл ISO сохраняется локально в вашей системе, мы нажмем «Обзор локально».

![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)

Обязательно перейдите к местоположению вашего файла ISO. Нажмите на нее, а затем нажмите «Открыть».

![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)

Прежде чем продолжить, убедитесь, что вы выбрали операционную систему в раскрывающемся меню. Затем нажмите «Вперед».

![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)

Нажмите «Да» во всплывающем окне, чтобы предоставить эмулятору разрешения на поиск файла ISO.

![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)

Затем выберите размер памяти и количество ядер ЦП и нажмите «Вперед».

![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)

На следующем шаге включите хранилище для виртуальной машины и укажите размер виртуального диска. Затем нажмите «Вперед».

![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)

Наконец, проверьте все настройки, которые вы определили, и, если все выглядит хорошо, нажмите «Готово», чтобы создать виртуальную машину. В противном случае нажмите «Назад» и внесите необходимые изменения.

![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)

Как только вы нажмете «Готово», диспетчер виртуальных машин начнет создавать виртуальную машину на основе заданных конфигураций.

![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)

И через несколько секунд появится мастер установки виртуальной машины. Вы можете продолжить установку так же, как и в физической системе.

![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)

Кроме того, ваша виртуальная машина будет указана в диспетчере виртуальных машин, как показано ниже. Щелкнув правой кнопкой мыши на своей виртуальной машине, вы можете выполнять различные задачи, включая приостановку, перезагрузку, сброс и удаление виртуальной машины среди многих других.

![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)

### *Похожее*

[![KVM](https://i0.wp.com/losst.ru/wp-content/uploads/2017/03/big_machine-300x100.png?resize=350%2C200&ssl=1)](https://linuxthebest.net/chto-takoe-virtualizatsiya-kvm/?relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=0&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=0&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=0&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=0 "Что такое виртуализация KVM")

#### [Что такое виртуализация KVM](https://linuxthebest.net/chto-takoe-virtualizatsiya-kvm/?relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=0&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=0&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=0&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=0 "Что такое виртуализация KVM")

2019-08-28

В "Обзоры"

[![qemu](https://i0.wp.com/linuxthebest.net/wp-content/uploads/2016/10/qemu.jpg?resize=350%2C200)](https://linuxthebest.net/qemu-v-ubuntu-16-04linux-mint-18/?relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=1&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=1&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=1&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=1 "Установить Qemu/KVM в Ubuntu 16.04/Linux Mint 18")

#### [Установить Qemu/KVM в Ubuntu 16.04/Linux Mint 18](https://linuxthebest.net/qemu-v-ubuntu-16-04linux-mint-18/?relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=1&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=1&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=1&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=1 "Установить Qemu/KVM в Ubuntu 16.04/Linux Mint 18")

2016-10-13

В "Инструкции"

[![](https://i0.wp.com/linuxthebest.net/wp-content/uploads/2022/07/000-6.png?resize=350%2C200&ssl=1)](https://linuxthebest.net/kak-ustanovyt-qemu-na-ubuntu-dlya-nastrojky-vyrtualnoj-mashyny/?relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=2&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=2&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=2&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=2 "Как установить QEMU на Ubuntu для настройки виртуальной машины")

#### [Как установить QEMU на Ubuntu для настройки виртуальной машины](https://linuxthebest.net/kak-ustanovyt-qemu-na-ubuntu-dlya-nastrojky-vyrtualnoj-mashyny/?relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=2&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=2&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=2&relatedposts_hit=1&relatedposts_origin=24805&relatedposts_position=2 "Как установить QEMU на Ubuntu для настройки виртуальной машины")

2022-07-26

В "Инструкции"

[\# kvm](https://linuxthebest.net/tag/kvm/)[\# qemu](https://linuxthebest.net/tag/qemu/)[\# ubuntu](https://linuxthebest.net/tag/ubuntu/)

Поделитесь с друзьями

[![walle9054](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)](https://linuxthebest.net/account-2/?user=3227)

##### walle9054

[Статей: 118](https://linuxthebest.net/account-2/?user=3227)

[![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)<br>Пред. Запись Обзор Ubuntu Unity 22.10: многообещающий «официальный» старт](https://linuxthebest.net/oglyad-ubuntu-unity-22-10-perspektyvnyj-oficzijnyj-start/)[След. Запись Zorin OS 16.2 выходит с расширенной поддержкой приложений для Windows<br>![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)](https://linuxthebest.net/zorin-os-16-2-vyhodyt-s-rasshyrennoj-podderzhkoj-prylozhenyj-dlya-windows/)

##### Вам может понравится

[![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)](https://linuxthebest.net/10-luchshyh-prylozhenyj-linux-dlya-muzykantov/)

#### [10 лучших приложений Linux для музыкантов](https://linuxthebest.net/10-luchshyh-prylozhenyj-linux-dlya-muzykantov/)

- [ViGo](https://linuxthebest.net/account-2/?user=1501 "Записи пользователя ViGo")
- [Обзоры](https://linuxthebest.net/review/), [Программы](https://linuxthebest.net/programmy/)
- [1 коммент](https://linuxthebest.net/10-luchshyh-prylozhenyj-linux-dlya-muzykantov/#comments)

[![](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)](https://linuxthebest.net/testirovanie-linux-bez-klassicheskoj-ustanovki/)

#### [Тестирование Linux без классической установки](https://linuxthebest.net/testirovanie-linux-bez-klassicheskoj-ustanovki/)

- [ViGo](https://linuxthebest.net/account-2/?user=1501 "Записи пользователя ViGo")
- [Инструкции](https://linuxthebest.net/terminal/), [Обзоры](https://linuxthebest.net/review/)

[![daw for linux](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)](https://linuxthebest.net/najkrashhi-daw-czifrovi-audio-robochi-stanczi%d1%97-dostupni-dlya-nastilnih-kompyuteriv-linux/)

#### [Лучшие DAW (цифровые аудио рабочие станции), доступные для настольных компьютеров Linux](https://linuxthebest.net/najkrashhi-daw-czifrovi-audio-robochi-stanczi%d1%97-dostupni-dlya-nastilnih-kompyuteriv-linux/)

- [FOX](https://linuxthebest.net/account-2/?user=60 "Записи пользователя FOX")
- [Обзоры](https://linuxthebest.net/review/), [Программы](https://linuxthebest.net/programmy/)
- [3 комментария](https://linuxthebest.net/najkrashhi-daw-czifrovi-audio-robochi-stanczi%d1%97-dostupni-dlya-nastilnih-kompyuteriv-linux/#comments)

## Язык

[Вход](https://linuxthebest.net/wp-login.php?redirect_to=%2F "Вход")

[Регистрация](https://linuxthebest.net/wp-login.php?action=register "Регистрация")

## Вход через соцсети

## Последние комментарии

<img width="44" height="44" src="../_resources/049387b75a23b501edfd8d6b543e6f6c_b49b416ec5fb49558.png" class="jop-noMdConv">

[Ubuntu Desktop и Ubuntu…](https://linuxthebest.net/ubuntu-desktop-y-ubuntu-server-v-chem-raznycza/#comment-6163)

3 годин тому от костя вашкевич николаич

[я люблю сосать хуи](https://linuxthebest.net/ubuntu-desktop-y-ubuntu-server-v-chem-raznycza/#comment-6163)

* * *

<img width="44" height="44" src="../_resources/049387b75a23b501edfd8d6b543e6f6c_b49b416ec5fb49558.png" class="jop-noMdConv">

[Ubuntu Desktop и Ubuntu…](https://linuxthebest.net/ubuntu-desktop-y-ubuntu-server-v-chem-raznycza/#comment-6162)

3 годин тому от костя вашкевич

[БАМ БАМ БАМ Фура едет по холмам](https://linuxthebest.net/ubuntu-desktop-y-ubuntu-server-v-chem-raznycza/#comment-6162)

* * *

<img width="44" height="44" src="../_resources/049387b75a23b501edfd8d6b543e6f6c_b49b416ec5fb49558.png" class="jop-noMdConv">

[Ubuntu Desktop и Ubuntu…](https://linuxthebest.net/ubuntu-desktop-y-ubuntu-server-v-chem-raznycza/#comment-6161)

4 годин тому от костя вашкевич

[Я еблан](https://linuxthebest.net/ubuntu-desktop-y-ubuntu-server-v-chem-raznycza/#comment-6161)

* * *

<img width="44" height="44" src="../_resources/049387b75a23b501edfd8d6b543e6f6c_b49b416ec5fb49558.png" class="jop-noMdConv">

[Ubuntu Desktop и Ubuntu…](https://linuxthebest.net/ubuntu-desktop-y-ubuntu-server-v-chem-raznycza/#comment-6160)

4 годин тому от костя вашкевич

[хули ржешь](https://linuxthebest.net/ubuntu-desktop-y-ubuntu-server-v-chem-raznycza/#comment-6160)

* * *

<img width="44" height="44" src="../_resources/e4079090d3751af835d4ead7b27d7370_e123a87d0355455db.png" class="jop-noMdConv">

[Linux-программы для резервного копирования](https://linuxthebest.net/linux-programmy-dlya-rezervnogo-kopirovaniya/#comment-6159)

1 день тому от Ирина

[Я устанавливаю программу для резервного копирования Time Vault на другом…](https://linuxthebest.net/linux-programmy-dlya-rezervnogo-kopirovaniya/#comment-6159)

* * *

Підключитися з

[Вхід](https://linuxthebest.net/login-registraciya/)

![guest](data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==)

0 комментариев

© 2010 - 2023 **[UALinux](https://ualinux.com)**