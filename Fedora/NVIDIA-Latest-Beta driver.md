---
url: https://rpmfusion.org/Howto/NVIDIA?highlight=%28%5CbCategoryHowto%5Cb%29#Latest.2FBeta_driver
date: 2025-02-17 07:40:31
tags:
  - nvidia
  - gpu
---
[[Fedora_Linux_Начало]]
[[Правильная установка драйверов NVIDIA в Fedora]]
> ### Latest/Beta driver
> 
> You can install the latest drivers from Rawhide using the following command:
> 
> sudo dnf install "kernel-devel-uname-r >= $(uname -r)"
> sudo dnf update -y
> sudo dnf copr enable kwizart/nvidia-driver-rawhide -y
> sudo dnf install rpmfusion-nonfree-release-rawhide -y
> sudo dnf --enablerepo=rpmfusion-nonfree-rawhide install akmod-nvidia xorg-x11-drv-nvidia xorg-x11-drv-nvidia-cuda --nogpgcheck
> 
> Or if you want to grab it from the latest fedora stable release:
> 
> sudo dnf install "kernel-devel-uname-r == $(uname -r)"
> sudo dnf update -y
> sudo dnf --releasever=30 install akmod-nvidia xorg-x11-drv-nvidia --nogpgcheck
> 
> ### nvidia-xconfig
> 
> This tool is only meant to be used as a sample to create a xorg.conf files. But don't use this directly as the generated xorg.conf is known to broke with many default Fedora/RHEL Xorg server options. ad, you should probably start with :
> 
> sudo cp /usr/share/X11/xorg.conf.d/nvidia.conf /etc/X11/xorg.conf.d/nvidia.conf
