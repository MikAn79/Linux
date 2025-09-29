---
page-title: Правильная установка драйверов NVIDIA в Fedora — Официальный сайт EasyCoding Team
url: https://www.easycoding.org/2017/01/11/pravilnaya-ustanovka-drajverov-nvidia-v-fedora.html
date: 2024-12-06 20:18:41
---
[[Мониторинг и управление GPU NVIDIA с nvidia-smi • ИТ Решения — ИП Кривошеин Алексей Сергеевич]]
[[NVIDIA Drivers on Fedora]]
[[NVIDIA-Latest-Beta driver]]
## Установка обычного драйвера
> 
> Обычный проприетарный драйвер NVIDIA доступен в репозиториях RPM Fusion, поэтому нам потребуется подключить их если они ещё не подключены (необходимы как free, так и nonfree):
> 
```
sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```
> 
> Теперь мы должны установить набор сборки драйвера: компилятор GCC и заголовочные файлы ядра, исходники ядерного модуля, а также сам драйвер.
> 
#### Установка для современных видеокарт
> 
> Вариант для современных видеокарт NVIDIA (серии 800 (ноутбуки), 900 и 1000, 2000, 1600, 3000 и более современные):
> 
```
sudo dnf install gcc kernel-headers kernel-devel akmod-nvidia xorg-x11-drv-nvidia xorg-x11-drv-nvidia-libs xorg-x11-drv-nvidia-power
```
> 
> Если используется 64-битная ОС, но требуется запускать ещё и Steam и 32-битные версии игр, то установим также 32-битный драйвер (устанавливать сразу после предыдущих):
> 
```
sudo dnf install xorg-x11-drv-nvidia-libs.i686
```

####  Действия по окончании установки

>  По окончании установки необходимо убедиться, что модули ядра были успешно собраны и установлены корректно:
>
```
sudo akmods --force
```

> Если возникла ошибка, то подробный журнал можно найти в каталоге **/var/cache/akmods/nvidia/**.
> 
> Теперь вырежем из образа [initrd](https://ru.wikipedia.org/wiki/Initrd) драйвер nouveau и добавим NVIDIA:

```
sudo dracut --force
 ```

### Активируем systemd-юниты для корректной работы ждущего и спящего режимов:
> 
```
sudo systemctl enable nvidia-{suspend,resume,hibernate}
```
#### Удаление драйверов

> Если возникли какие-то проблемы, либо драйверы NVIDIA более не требуются, то их всегда можно удалить штатным способом:
> 
```
sudo dnf remove \*nvidia\* --no autoremove
```

> По окончании удаления необходимо в обязательном порядке пересобрать образ initrd, чтобы вернуть в него удалённый при установке свободный драйвер nouveau:
```
sudo dracut --force
```


#### При возникновении чёрного экрана

> Если по окончании установки и перезагрузки вместо окна входа в систему нас встретит чёрный экран, то в загрузчике добавим через пробел следующие параметры ядра:

```
rd.drivers.blacklist=nouveau nouveau.modeset=0
```
> 
> Также нужно в обязательном порядке зайти в модуль настройки UEFI компьютера или ноутбука и отключить [UEFI Secure Boot](https://ru.wikipedia.org/wiki/Secure_boot) (сама Fedora поддерживает работу с Secure Boot, однако модули ядра проприетарного драйвера не имеют цифровой подписи, поэтому не могут быть загружены в данном режиме и, как следствие, пользователь увидит чёрный экран), а также перевести его из режима **Windows Only** в **Other OS**.
> 
> Работа с включённым UEFI Secure Boot поддерживается начиная с Fedora 36, однако [требуется настройка](https://www.easycoding.org/2022/05/31/nastraivaem-podderzhku-uefi-secure-boot-dlya-drajverov-nvidia.html).
> 
> ## Установка драйверов для NVIDIA Optimus
> 
> Начиная с Fedora 31 и версии проприетарного драйвера 435.xx, технология NVIDIA Optimus, используемая в ноутбуках с гибридной графикой, поддерживается в полной мере «из коробки». К сожалению, старые поколения видеокарт (ниже серии 700) им не поддерживаются и поэтому работать не будут.
> 
> Подключим репозитории RPM Fusion:
> 
> <table><tbody><tr><td><p>1</p></td><td><div><p><code>sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm</code></p></div></td></tr></tbody></table>
> 
> Установим стандартный драйвер NVIDIA для современных видеокарт:
> 
> <table><tbody><tr><td><p>1</p></td><td><div><p><code>sudo dnf install gcc kernel-headers kernel-devel akmod-nvidia xorg-x11-drv-nvidia xorg-x11-drv-nvidia-libs</code></p></div></td></tr></tbody></table>
> 
> Если используется 64-битная ОС, но требуется запускать ещё и Steam и 32-битные версии игр, то установим также 32-битный драйвер (устанавливать сразу после предыдущих):
> 
> <table><tbody><tr><td><p>1</p></td><td><div><p><code>sudo dnf install xorg-x11-drv-nvidia-libs.i686</code></p></div></td></tr></tbody></table>
> 
> #### Действия по окончании установки
> 
> По окончании установки необходимо убедиться, что модули ядра были успешно собраны и установлены корректно:
> 
> Если возникла ошибка, то подробный журнал можно найти в каталоге **/var/cache/akmods/nvidia/**.
> 
> Теперь вырежем из образа [initrd](https://ru.wikipedia.org/wiki/Initrd) драйвер nouveau и добавим NVIDIA:
> 
> #### При возникновении чёрного экрана
> 
> Если по окончании установки и перезагрузки вместо окна входа в систему нас встретит чёрный экран, то в загрузчике добавим через пробел следующие параметры ядра:
> 
> <table><tbody><tr><td><p>1</p></td><td><div><p><code>rd.drivers.blacklist=nouveau nouveau.modeset=0</code></p></div></td></tr></tbody></table>
> 
> Также нужно в обязательном порядке зайти в модуль настройки UEFI компьютера или ноутбука и отключить [UEFI Secure Boot](https://ru.wikipedia.org/wiki/Secure_boot) (сама Fedora поддерживает работу с Secure Boot, однако модули ядра проприетарного драйвера не имеют цифровой подписи, поэтому не могут быть загружены в данном режиме и, как следствие, пользователь увидит чёрный экран), а также перевести его из режима **Windows Only** в **Other OS**.
> 
> #### Работа с NVIDIA Optimus
> 
> По умолчанию будет использоваться интегрированное решение, но для запуска приложения с использованием дискретной видеокарты необходимо передавать особые переменные окружения:
> 
> <table><tbody><tr><td><p>1</p></td><td><div><p><code>__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia application [параметры запуска приложения]</code></p></div></td></tr></tbody></table>
> 
> Пример запуска панели управления NVIDIA для Optimus конфигураций:
> 
> <table><tbody><tr><td><p>1</p></td><td><div><p><code>__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia nvidia-settings -c :8</code></p></div></td></tr></tbody></table>
> 
> Пример запуска приложения app.exe через Wine на Optimus:
> 
> <table><tbody><tr><td><p>1</p></td><td><div><p><code>__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia wine app.exe</code></p></div></td></tr></tbody></table>
> 
> #### Удаление драйверов
> 
> Если возникли какие-то проблемы, либо драйверы NVIDIA более не требуются, то их всегда можно удалить штатным способом:
> 
> <table><tbody><tr><td><p>1</p></td><td><div><p><code>sudo dnf remove \*nvidia\*</code></p></div></td></tr></tbody></table>
> 
> По окончании удаления необходимо в обязательном порядке пересобрать образ initrd, чтобы вернуть в него удалённый при установке свободный драйвер nouveau:
> 
> ## Пользователям Gnome с Wayland
> 
> Если используется Gnome с GDM, то необходимо отключить поддержку Wayland ибо проприетарные драйверы NVIDIA на момент написания данной статьи работают с ним некорректно. Для этого нужно отредактировать файл **/etc/gdm/custom.conf**, добавив или расскомментировав строку **WaylandEnable=false**:
> 
> <table><tbody><tr><td><p>1</p></td><td><div><p><code>sudo vim /etc/gdm/custom.conf</code></p></div></td></tr></tbody></table>
> 
> Если этот шаг пропустить, то при следующей загрузки увидим серый экран с надписью «Упс, что-то пошло не так». Пользователи KDE, XFCE и других DE могут смело его пропускать ибо в Fedora используется Wayland пока только для Gnome.
