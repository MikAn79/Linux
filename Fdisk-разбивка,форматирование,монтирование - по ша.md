## [Notes](https://evgenbl.github.io/)

- [Archive](https://evgenbl.github.io/archive.html)
- [ALT Regular Gnome](https://evgenbl.github.io/alt.html)
- [NixOS](https://evgenbl.github.io/nixos)
- [RosaFresh](https://evgenbl.github.io/rosafresh.html)
- [Slackware](https://evgenbl.github.io/slackware.html)
- [Trisquel](https://evgenbl.github.io/trisquel.html)
- [Windows](https://evgenbl.github.io/windows.html)
- [Linux](https://evgenbl.github.io/linux.html)
- [Program](https://evgenbl.github.io/program.html)
- [ConsoleUtilities](https://evgenbl.github.io/console.html)
- [Markdown](https://evgenbl.github.io/markdown.html)
- [LaTeX](https://evgenbl.github.io/latex.html)
- [Diagnostics](https://evgenbl.github.io/diagnostics.html)
- [HTML||CSS](https://evgenbl.github.io/html.html)
- [VirtualBox](https://evgenbl.github.io/virtualbox)
- [Video_help](https://evgenbl.github.io/Video_help.html)
- [Figma plugins](https://evgenbl.github.io/figma.html)
- [Jekyll](https://evgenbl.github.io/jekyll.html)
- [Interesting](https://evgenbl.github.io/interesting.html)
- [About Me](https://evgenbl.github.io/about_me.html)

# Fdisk-разбивка,форматирование,монтирование - по шагово!

8 January, 2023

  

***1\. Процесс разбивки диска по шагам***

*Здесь показан полный процесс разбивки диска по шагам*

[***Ручная установка Arch Linux***](https://disk.yandex.ru/i/qwqRT9DzrBew7A) \- ***“ссылка на видео”***

*Ссылка на авторский канал:*

[***Computer Genius***](https://www.youtube.com/@Arch4u)

*Создаем MBR таблицу (Для UEFI - GPT командой g)*

- o

Жмем **enter**

*Создаем новый диск*

- n

Жмем **enter**

*Выбираем primary (основной) или extented (расширенный) По умолчанию стоит primary (основной)*

поэтому просто жмем **enter**

*Выбор номера диска, по умолчанию подставляется следующий номер*

Просто жмем **enter**

*Запрос на первый сектор диска*

Просто жмем **enter**

*Запрос на последний сектор диска (Ставим + и объем памяти. Пример: +100M)*

- +100M

*Повторяем все шаги снова для всех нужных разделов диска*

***(LEGACY)***

Для **/boot** не забываем указать **a** и поставить **1** для установки его загрузочным

***(UEFI)***

Для **efi** не забываем указать, что это **efi** раздел **t** и поставить **1** для установки его загрузочным

*Как все разметили не забываем все записать командой*

- **w**

*В итоге можете проверить, что у вас получилось командой*

- **fdisk -l**

***Должно получиться примерно так***:

*Legacy разметка*

![](../_resources/legacy_03758a9ee4ff416e825a25bced95220d.png)

*UEFI разметка*

<img width="662" height="320" src="../_resources/uefi_dfac99b4e467414b80b307e895a2e899.png"/>

***2\. Форматирование и монтирование разделов***

* * *

**Разделы с BIOS**

*Форматирование дисков*

- mkfs.ext2 /dev/sda1 -L boot
    
- mkfs.ext4 /dev/sda2 -L root
    
- mkswap /dev/sda3 -L swap
    
- mkfs.ext4 /dev/sda4 -L home
    

*Монтируем* - **/**

mount /dev/sda2 /mnt

**Создаем директорию boot и home в mnt**

- mkdir /mnt/{boot,home}

*Монтируем* - **boot**

- mount /dev/sda1 /mnt/boot

*Монтируем* - **swap**

- swapon /dev/sda3

*Монтируем* - **/home**

- mount /dev/sda4 /mnt/home

* * *

**Разделы с UEFI**

*Форматирование дисков*

- mkfs.fat -F32 /dev/sda1
    
- mkfs.ext4 /dev/sda2
    
- mkfs.ext4 /dev/sda3
    

*Монтирование дисков*

- mount /dev/sda2 /mnt
    
- mkdir /mnt/home
    
- mkdir -p /mnt/boot/efi
    
- mount /dev/sda1 /mnt/boot/efi
    
- mount /dev/sda3 /mnt/home
    

* * *

## Создание разделов с BIOS

- \# /boot 100M - выставить флаг boot командой a и затем 1
    
- \# / 20G
    
- \# swap 1024M
    
- \# /home весь остаток
    

## Создание разделов с UEFI

- cfdisk /dev/sda
    
- /dev/sda1 - 500M EFI - выставить флаг EFI командой t и затем 1
    
- /dev/sda2 - 30G root Linux File System
    
- /dev/sda3 - Весь остаток home Linux file System
    

* * *

## Процесс разбивки диска по шагам

- См. видео https://vk.cc/7S7OMg

\# Создаем MBR таблицу (Для UEFI - GPT командой g)

- o или - g
    
- Жмем enter
    

\# Создаем новый диск

- n
    
- Жмем enter
    

\# Выбираем primary (основной) или extented (расширенный)

По умолчанию стоит primary (основной) поэтому просто жмем enter

- Жмем enter

\# Выбор номера диска, по умолчанию подставляется следующий номер

Просто жмем enter

\# Запрос на первый сектор диска

Просто жмем enter

\# Запрос на последний сектор диска (Ставим + и объем памяти. Пример: +100M) +100M

### Повторяем все шаги снова для всех нужных разделов диска

- (LEGACY) Для /boot не забываем указать a и поставить 1 для установки его загрузочным
    
- (UEFI) Для efi не забываем указать, что это efi раздел t и поставить 1
    

Как все разметили не забываем все записать командой

- w
    
- Жмем enter
    

В итоге можете проверить, что у вас получилось командой:

- fdisk -l

Должно получиться примерно так:

Legacy разметка http://i.imgur.com/pgej0Nt.png

UEFI разметка https://i.imgur.com/O7Yn0MK.png

* * *

## Форматирование и монтирование разделов

## Разделы с BIOS

- mkfs.ext2 /dev/sda1 -L boot
    
- mkfs.ext4 /dev/sda2 -L root
    
- mkswap /dev/sda3 -L swap
    
- mkfs.ext4 /dev/sda4 -L home
    

## Монтируем /

- mount /dev/sda2 /mnt

## Создаем директорию boot и home в mnt

- mkdir /mnt/{boot,home}

## Монтируем boot

- mount /dev/sda1 /mnt/boot

## Монтируем swap

- swapon /dev/sda3

## Монтируем /home

- mount /dev/sda4 /mnt/home

## Разделы с UEFI

## Форматирование дисков

- mkfs.fat -F32 /dev/sda1
    
- mkfs.ext4 /dev/sda2
    
- mkfs.ext4 /dev/sda3
    

## Монтирование дисков

- mount /dev/sda2 /mnt
    
- mkdir /mnt/home
    
- mkdir -p /mnt/boot/efi
    
- mount /dev/sda1 /mnt/boot/efi
    
- mount /dev/sda3 /mnt/home