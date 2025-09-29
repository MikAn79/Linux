---
tags:
  - fedora
  - rpm
  - repo
---
## Включить RPM Fusion
[[Fedora_Linux_Начало]]
RPM Fusion предлагает пакеты, которые не могут быть предложены в официальных репозиториях Fedora по различным причинам, таким как несвободное или проприетарное лицензирование. Эти инструкции любезно заимствованы у [RPM Fusion](https://rpmfusion.org/Configuration). Также доступен RPM, который активирует эти репозитории. Эта команда активирует свободные и несвободные репозитории.

```
sudo dnf install -y https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```