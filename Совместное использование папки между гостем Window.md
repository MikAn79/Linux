# Совместное использование папки между гостем Windows и хостом Linux в KVM с использованием virtiofs

5 мин

* * *

[![Ариндам](https://secure.gravatar.com/avatar/817f1324a0121daed334e248723abb17?s=74&d=wavatar&r=g)](https://www.debugpoint.com/author/admin1/)

к [Ариндам](https://www.debugpoint.com/author/admin1/) 4 месяца назад4 дня назад

[19](https://www.debugpoint.com/kvm-share-folder-windows-guest/#comments)

11,5 тыс.Просмотры

**В этом руководстве вы узнаете, как совместно использовать папку между гостевыми системами Windows, работающими под управлением хоста Linux, например Fedora, Ubuntu или Linux Mint, с помощью KVM.**

Приложение [virt-manager](https://virt-manager.org/) (с [libvirt](https://libvirt.org/manpages/libvirtd.html) ) и пакеты предоставляют гибкий набор инструментов для управления виртуальными машинами в Linux. Он бесплатный, с открытым исходным кодом и используется для виртуальных машин KVM и других гипервизоров.

В предыдущей статье я объяснил, [как совместно использовать папки между гостем Linux и хостом Linux](https://www.debugpoint.com/share-folder-virt-manager/) . Однако, когда вы пытаетесь создать общую папку с помощью гостя Windows и хоста Linux, это немного трудный и сложный процесс. Потому что обе операционные системы работают по-разному, и требуется много настроек.

Следуйте приведенным ниже инструкциям, чтобы открыть общий доступ к папке между гостем Windows и хостом Linux.

Оглавление

- [Заметка о виртиофах](#A_note_about_virtiofs "Заметка о виртиофах")
- [Совместное использование папки между гостем Windows и хостом Linux с помощью KVM](#Share_folder_between_Windows_guest_and_Linux_host_using_KVM "Совместное использование папки между гостем Windows и хостом Linux с помощью KVM")
    - [Настройте тег монтирования в virt-manager](#Set_up_a_mount_tag_in_virt-manager "Настройте тег монтирования в virt-manager")
    - [Настройка WinFSP – FUSE для Windows](#Set_up_WinFSP_%E2%80%93_FUSE_for_Windows "Настройка WinFSP – FUSE для Windows")
    - [Создайте VirtIO-FS как услугу.](#Create_VirtIO-FS_as_a_service "Создайте VirtIO-FS как услугу.")
- [Заключение](#Conclusion "Заключение")

Заметка о виртиофах

Для совместного использования файлов и папок используется общая файловая система libvirt, называемая virtiofs. Он предоставляет все функции и параметры для доступа к дереву каталогов на хост-компьютере. Поскольку большинство конфигураций виртуальных машин virt-manager преобразуются в XML, общие файлы/папки также могут быть указаны в XML-файле.

Примечание. Если вы ищете общий доступ к файлам с помощью KVM между **двумя компьютерами Linux** (гостевой и хостовой), [прочтите эту статью](https://www.debugpoint.com/share-folder-virt-manager/) .

Совместное использование папки между гостем Windows и хостом Linux с помощью KVM

Следующие инструкции предполагают, что вы установили Windows в virt-manager на любом хосте Linux. Если нет, вы можете прочитать это [полное руководство по установке Windows в Linux](https://www.debugpoint.com/install-windows-ubuntu-virt-manager/) .

Настройте тег монтирования в virt-manager

- Сначала убедитесь, что ваша гостевая виртуальная машина выключена. В графическом интерфейсе virt-manager выберите виртуальную машину и нажмите «Открыть», чтобы открыть настройки консоли.

[![Откройте настройки консоли](https://www.debugpoint.com/wp-content/uploads/2023/06/Open-the-console-settings.jpg)](https://www.debugpoint.com/wp-content/uploads/2023/06/Open-the-console-settings.jpg)

Откройте настройки консоли

- Нажмите на значок с надписью «Показать сведения о виртуальном оборудовании» на панели инструментов. А затем нажмите **«Память»** на левой панели.
- Выберите опцию « **Включить общую память** ». Нажмите Применить.
- Убедитесь, что XML показывает «режим доступа = общий», как показано ниже на вкладке XML.

&lt;memoryBacking&gt; &lt; тип источника = "memfd" /&gt; **<** **режим доступа** **=** **"shared"** **/>** &lt;/memoryBacking&gt;

[![Включить общую память](https://www.debugpoint.com/wp-content/uploads/2023/06/Enable-shared-memory.jpg)](https://www.debugpoint.com/wp-content/uploads/2023/06/Enable-shared-memory.jpg)

Включить общую память

- Нажмите «Добавить оборудование» внизу.
    
- Выберите **«Файловая система»** на левой панели в окне добавления нового оборудования.
    
- Затем выберите **Driver=virtiofs** на вкладке сведений. Нажмите и **выберите путь к хосту** в вашей системе Linux.`browse > browse local`
    
- В целевом пути укажите любое имя, которое хотите. Это просто тег файла, который будет использоваться во время монтирования. Это имя в целевом пути будет смонтировано как Диск в Windows — Мой компьютер в Проводнике.
    
- Я добавил «linux_pictures» в качестве целевого тега монтирования.
    
- Итак, если я хочу получить доступ к папке «Изображения» ( ), примеры настроек могут быть следующими:`/home/debugpoint/Pictures`
    
- Нажмите «Готово».
    

[![Добавить монтирование файловой системы для Windows](https://www.debugpoint.com/wp-content/uploads/2023/06/Add-a-file-system-mount-for-windows.jpg)](https://www.debugpoint.com/wp-content/uploads/2023/06/Add-a-file-system-mount-for-windows.jpg)

Добавить монтирование файловой системы для Windows

Настройки XML приведены ниже для указанной выше конфигурации. Вы можете найти его на вкладке XML.

< тип файловой системы = "mount" accessmode = " **passthrough** " \> &lt; тип драйвера = "virtiofs" /&gt; **<исходный** **каталог** **=** **"/home/debugpoint/Pictures"** **/>** **<целевой** **каталог** **=** **"linux_pictures"** **/>** &lt; тип адреса = " PCI" домен = "0x0000" шина = "0x05" слот = "0x00" функция = "0x0" /&gt; &lt;aя=42&gt;&lt;/файловая система&gt;

В главном окне virt-manager щелкните правой кнопкой мыши виртуальную машину Windows и выберите «Выполнить», чтобы запустить виртуальную машину. Обязательно нажмите «показать графическую консоль» (значок монитора на панели инструментов) — если виртуальная машина не отображается.

Настройка WinFSP – FUSE для Windows

Убедитесь, что виртуальная машина Windows (гостевая) запущена.

- Сначала нам нужно настроить WinFSP или прокси-сервер файловой системы Windows — FUSE для Windows. Это позволяет вам без проблем смонтировать любую UNIX-подобную файловую систему.
- Откройте приведенную ниже страницу в WinFSP GitHub **с гостевой** машины Windows.
- Загрузите установщик WinFSP .msi.

[Скачать установщик WinFSP](https://github.com/winfsp/winfsp/releases/)

- Установите пакет на виртуальную машину Windows. Обязательно выберите «Core» при установке пакета. Завершите установку.

[![WinFSP настроен.](https://www.debugpoint.com/wp-content/uploads/2023/06/WinFSP-set-up.jpg)](https://www.debugpoint.com/wp-content/uploads/2023/06/WinFSP-set-up.jpg)

WinFSP настроен.

Создайте VirtIO-FS как услугу.

- Загрузите **virtio-win-guest-tools.exe** по указанному ниже пути, зайдя в папку **стабильного-virtio** .

[Загрузите virtio-win-guest-tools](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/)

[![Загрузите гостевые инструменты](https://www.debugpoint.com/wp-content/uploads/2023/06/Download-guest-tools-1024x576.jpg)](https://www.debugpoint.com/wp-content/uploads/2023/06/Download-guest-tools.jpg)

- Установите пакет на виртуальную машину Windows.

[![Установка Virtio-Win-драйвера](https://www.debugpoint.com/wp-content/uploads/2023/06/Virtio-Win-driver-installation.jpg)](https://www.debugpoint.com/wp-content/uploads/2023/06/Virtio-Win-driver-installation.jpg)

Установка Virtio-Win-драйвера

- После завершения установки **перезагрузите** виртуальную машину Windows.
- После перезагрузки откройте «Диспетчер устройств», выполнив поиск в меню «Пуск».
- Перейдите к системным устройствам и найдите «Устройство VirtIO FS». Он должен быть распознан, а драйвер должен быть подписан Red Hat.
- **Примечание** . (необязательно) Если вы видите восклицательный знак, т. е. драйвер не обнаружен, следуйте инструкциям [здесь](https://virtio-fs.gitlab.io/howto-windows.html) о том, как загрузить файл ISO, смонтировать его и вручную обнаружить драйвер.

[![Убедитесь, что драйвер Virt IO подписан и установлен.](https://www.debugpoint.com/wp-content/uploads/2023/06/Make-sure-the-Virt-IO-driver-is-signed-and-installed.jpg)](https://www.debugpoint.com/wp-content/uploads/2023/06/Make-sure-the-Virt-IO-driver-is-signed-and-installed.jpg)

Убедитесь, что драйвер Virt IO подписан и установлен.

- Откройте меню «Пуск» и найдите «Службы».
- Прокрутите вниз, чтобы найти «Службу VirtIO-FS». Щелкните правой кнопкой мыши и нажмите «Пуск», чтобы запустить службу.
- Кроме того, вы можете запустить приведенную ниже команду из PowerShell/командной строки от имени администратора, чтобы запустить службу.

sc create VirtioFsSvc binpath = "C:\\Program Files\\Virtio-Win\\VioFS\\virtiofs.exe" start = auto depend = "WinFsp.Launcher/VirtioFsDrv" DisplayName = "Служба Virtio FS"

sc запустить VirtioFsSvc

[![Запустите службу Virt IO.](https://www.debugpoint.com/wp-content/uploads/2023/06/Start-the-Virt-IO-Service.jpg)](https://www.debugpoint.com/wp-content/uploads/2023/06/Start-the-Virt-IO-Service.jpg)

Запустите службу Virt IO.

- После запуска службы откройте Проводник, и вы должны увидеть тег монтирования, созданный вами на первом шаге выше, который должен быть сопоставлен как диск Z. См. ниже.
- Теперь вы можете получить доступ ко всей папке Linux с измененными разрешениями в соответствии с вашими потребностями.

[![Тег монтирования отображается как диск Z в Windows.](https://www.debugpoint.com/wp-content/uploads/2023/06/The-mount-tag-is-mapped-as-Z-drive-in-windows-1024x640.jpg)](https://www.debugpoint.com/wp-content/uploads/2023/06/The-mount-tag-is-mapped-as-Z-drive-in-windows.jpg)

Тег монтирования отображается как диск Z в Windows.

Вот параллельное сравнение одной и той же папки, доступ к которой осуществляется в гостевой системе Linux Mint и Windows.

[![Доступ и общий доступ к папке на гостевой ОС Windows и хосте Linux](https://www.debugpoint.com/wp-content/uploads/2023/06/Access-and-share-folder-in-Windows-guest-and-Linux-host-1024x640.jpg)](https://www.debugpoint.com/wp-content/uploads/2023/06/Access-and-share-folder-in-Windows-guest-and-Linux-host.jpg)

Доступ и общий доступ к папке на гостевой ОС Windows и хосте Linux

Заключение

Надеюсь, теперь вы сможете совместно использовать папку между гостевой системой Windows и хост-системой Linux. В этой статье описанный выше метод протестирован в Linux Mint. Это должно работать и для Ubuntu, и для Fedora.

Если описанный выше метод работает, оставьте комментарий ниже для пользы других.

***Рекомендации***

- https://virtio-fs.gitlab.io/howto-windows.html
- https://docs.fedoraproject.org/en-US/quick-docs/creating-windows-virtual-machines-using-virtio-drivers/
- https://github.com/virtio-win/virtio-win-pkg-scripts/blob/master/README.md
- https://github.com/virtio-win/kvm-guest-drivers-windows/issues/473

https://www.linux.org.ru/forum/desktop/15962644?cid=15966007