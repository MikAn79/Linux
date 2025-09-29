---
tags:
  - vmware
  - виртуализация
---

## Установка
[Link](https://www.if-not-true-then-false.com/2024/fedora-vmware-install/#14-change-root-user)
 [[https://www.if-not-true-then-false.com/2024/fedora-vmware-install/#1-install-vmware-workstation-pro-on-fedora-414039Fedora 41/40/39 VMware Install Guide \[VMware Workstation Pro 17.6.1\] :: If Not True Then False](https://www.if-not-true-then-false.com/2024/fedora-vmware-install/#1-install-vmware-workstation-pro-on-fedora-414039)]()
 
### Build VMware Modules


```/usr/bin/vmware-modconfig --console --install-all```

### Скрипт для авто сборки модулей

[https://github.com/jhenriquelc/fedora-vmware-installer]()

#### Usage

[](https://github.com/jhenriquelc/fedora-vmware-installer#usage)

To use the script, run the following commands:

```shell
# after having installed VMware from their official installer
git clone https://github.com/jhenriquelc/fedora-vmware-installer.git
cd fedora-vmware-installer
sudo ./install.sh
```

## Uninstall

[](https://github.com/jhenriquelc/fedora-vmware-installer#uninstall)

To uninstall the modules' reinstaller script:

- Delete the service at `/etc/systemd/system/vm-host-modules-reinstall.service`.
- Delete the modules source folder and reinstall script at `/opt/vm-host-modules`.

###  Vmware workstation ошибка

[ССылка](https://errors24.ru/2023/06/22/vmware-workstation-oshibka-pri-vklyuchenii-virtualnoj-mashiny/)