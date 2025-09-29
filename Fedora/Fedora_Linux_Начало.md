---
tags:
  - fedora
---
# Fedora Linux

**Ссылки
[[Руководство по последующей Установке Fedora 42]]
[[Шрифты]]**
[[Очистка KDE Plasma]]
[[Руководство после установки Fedora 40]]
[[VirtualBox iso Guest. Гостевые]]
[[DNF5 Commands]]
[[Основные команды DNF и утилиты dnf repoquery]]
[Что нужно сделать после установки | Руководство по установке Fedora Workstation](../../Linux/Fedora/Что%20нужно%20сделать%20после%20установки%20_%20Руководство%20по.md)
[Обслуживание системы Fedora Linux](../../Linux/Fedora/Обслуживание%20системы%20Fedora%20Linux.md)
[[Правильная установка драйверов NVIDIA в Fedora]]
[Samba-доступ к каталогу в Fedora Linux](../../Linux/Fedora/Samba-доступ%20к%20каталогу%20в%20Fedora%20Linux.md)
[[SSH  Fedora]]
[KVM на Fedora](../../Linux/Fedora/KVM%20на%20Fedora.html)
[[Установка VMware сборка модулей ядра]]
[Обновление до Beta | Fedora Zero](../../Linux/Fedora/Обновление%20до%20Beta%20_%20Fedora%20Zero.md)
[[Заметки]]
[[файл подкачки]]
[Linux_Fedora_mini_FAQ](https://linux-faq.ru/page/fedora)
[[Шпаргалка по горячим клавишам в терминале!]]
[[Visual Studio Code on Linux]]
[[Пост-инсталл Fedora ]] (Скрипт) [Streamlit](https://nattdf.streamlit.app/)
## Fedora FAQ

![[fedora-faq-ru.pdf]]
## **Установка всех доступных обновлений**
>[!note] Внимание
>В первую очередь обновления, потом — все остальное...

## Команды для подключения репозиториев проекта RPM Fusion

	A. Программные компоненты с открытым исходным кодом (мультимедийные кодеки и приложения):

```
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
```

	B. Программные компоненты с закрытым исходным кодом (драйверы, кодеки, архиваторы RAR, ACE, LZH, Steam и.т.д.):

```
sudo dnf install https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```


`sudo dnf -y upgrade --refresh`

`sudo dnf install kernel-headers kernel-devel dkms`

`sudo dnf group upgrade core`

`sudo dnf install akmod-nvidia`
## Русификация приложений

В прошлых версиях дистрибутива некоторые приложения, такие, как LibreOffice, не были в полной степени русифицированы сразу же после установки дистрибутива. Хотя сейчас это не особо актуально, следует убедиться в корректности установки пакетов русификации. Для этого достаточно нажать на кнопку «Обзор» на верхней панели, ввести запрос «терминал» в поле поиска в верхней части экрана, выбрать первое предложенное приложение «Терминал» и выполнить с помощью него следующую команду:


```
sudo dnf install langpacks-ru man-pages-ru
```

**настройка DNF:**  

```
sudo nano /etc/dnf/dnf.conf
```

```
fastestmirror=True      
max_parallel_downloads=10  
defaultyes=True  
deltarpm=True
```

## Как очистить кэш плагина dnf fastestmirror?
#кэш_зеркал
Удалим файл с кэшем плагина fastestmirror:

```
sudo rm -f /var/cache/dnf/fastestmirror.cache
```

## Медиакодеки


Установите их, чтобы обеспечить правильное воспроизведение мультимедиа.
```
sudo dnf swap 'ffmpeg-free' 'ffmpeg' --allowerasing # Switch to full FFMPEG.
sudo dnf group upgrade multimedia
sudo dnf upgrade @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin # Installs gstreamer components. Required if you use Gnome Videos and other dependent applications.
sudo dnf group install -y sound-and-video # Installs useful Sound and Video complement packages.
```


```
sudo dnf install gstreamer1-plugins-{bad-*,good-*,base} gstreamer1-plugin-openh264 gstreamer1-libav --exclude=gstreamer1-plugins-bad-free-devel --exclude=gstreamer1-plugins-bad-free  
world --exclude=proj-data-*
```
### Декодирование видео в формате H / W с помощью VA-API

```
sudo dnf install ffmpeg-libs libva libva-utils
```
## Архивные инструменты

```
sudo dnf install -y unzip p7zip p7zip-plugins unrar
```

## Установить имя хоста

```
hostnamectl set-hostname YOUR_HOSTNAME
```

### Отключить NetworkManager-wait-online.service

- Отключение этой функции может сократить время загрузки как минимум на 15–20 секунд:
```
sudo systemctl disable NetworkManager-wait-online.service
```
## Отключение SELinux

Отключить навсегда

Чтобы SELinux не запускался после перезагрузки, открываем на редактирование следующий файл:

```
sudoedit /etc/selinux/config
```

И редактируем следующую строку:

SELINUX=disabled

\*\* **disabled** отключает selinux, **enforcing** — включает.\*

### Навсегда одной командой

Для этого достаточно выполнить замену строки в вышеописанном конфигурационном файле следующей командой:

```
sed -i 's/^SELINUX=.*/SELINUX=disabled/g' /etc/selinux/config
```


## Обновление до нового релиза

```
sudo -i

dnf upgrade --refresh
dnf install dnf-plugin-system-upgrade
dnf system-upgrade download --releasever=$(($(rpm -E %fedora) + 1)) --setopt='module_platform_id=platform:f$(($(rpm -E %fedora) + 1))' --allowerasing
dnf system-upgrade reboot
```

## Установка KDE-Beta

Installation Instructions
How to use:

```
sudo dnf copr enable @kdesig/kde-beta
sudo dnf update
```

How to revert back to stable: This is not a generally supported use-case. It may not always work well.

```
sudo dnf copr disable @kdesig/kde-beta
sudo dnf distro-sync
```

[[Как безопасно очистить Fedora Workstation _ Linux ]]

[[fedora]]
