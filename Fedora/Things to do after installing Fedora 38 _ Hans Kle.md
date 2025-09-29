# (#things-to-do-after-installing-fedora-38-workstationThings to do after installing Fedora 38 Workstation

*2023-04-24 private version - unfinished*

*A brief tutorial on configuring a secure and reliable operating system.*

This guide is aimed at users who have an intermediate-level of technical knowledge. It is intended for those who are interested in transitioning from Microsoft Windows to a safer, more open, and stable operating system. The information provided below is based solely on my personal experience and is intended to serve as a suggestion. It’s important to think for yourself, conduct your own research, and enjoy the process. I will continue to improve the guide over time, adding more steps and detailed instructions.

**Добро пожаловать в Linux - добро пожаловать на свободу!**

Процесс установки прост и не требует рекомендаций даже для менее осведомленных пользователей. Вы можете обратиться к [официальной документации](https://docs.fedoraproject.org/en-US/quick-docs/creating-and-using-a-live-installation-image) для получения дополнительной информации. Я настоятельно рекомендую вам использовать шифрование диска в процессе разделения, чтобы обезопасить ваши данные.

После установки Fedora не забудьте включить сторонние репозитории при первой конфигурации загрузки.

## [](#configure-dnf-package-manager)**Настройка DNF Package Manager**

Первый шаг - открыть конфигурацию dnf с помощью следующей команды:

```
sudo nano /etc/dnf/dnf.conf
```

Чтобы включить параллельную загрузку в DNF, добавьте следующую строку в конец вашего файла /etc/dnf /dnf.conf:

```
max_parallel_downloads=10
```

Добавьте следующее, чтобы настроить самое быстрое зеркало:

```
fastestmirror=True
deltarpm=True
```

Хорошая идея - запустить обновление DNF, и вы сначала заметите, что диспетчер пакетов DNF теперь определяет самые быстрые зеркала в выходных данных. Кроме того, чтобы убедиться, что у вас установлены последние версии пакетов после новой установки, выполните следующую команду в вашем терминале:

```
sudo dnf upgrade --refresh
```

После **первого** обновления рекомендуется перезагрузить компьютер:

```
sudo reboot
```

## [](#enable-the-dnf-autoupdate)**Включите автоматическое обновление DNF**

```
sudo dnf install dnf-automatic -y
```

Загружается только конфигурация по умолчанию, но не применяются никакие пакеты. Чтобы изменить или добавить какие-либо конфигурации, откройте файл .conf от имени пользователя root (или с помощью sudo) в окне терминала.

```
sudo nano /etc/dnf/automatic.conf
```

как только вы будете готовы к настройке, включите автозапуск:

```
sudo systemctl enable --now dnf-automatic.timer
```

## [](#reduce-the-systemd-timeout-for-services)**Сократите время ожидания Systemd для служб**

Чтобы уменьшить время ожидания Systemd, отредактируйте следующий файл конфигурации:

```
sudo nano /etc/systemd/system.conf
```

измените значение в секундах на `15s`:

```
DefaultTimeoutStartSec=15s
DefaultTimeoutStopSec=15s
```

## [](#enable-the-rpm-fusion-repositories)**Включите репозитории RPM Fusion**

```
sudo dnf install \
  https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
```

При необходимости включите несвободный репозиторий:

```
sudo dnf install \
  https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

При первой попытке установки пакетов из этих репозиториев утилита dnf предложит вам подтвердить подпись репозиториев. Подтвердите это.

Включите данные Appstream для RPM Fusion:

```
sudo dnf group update core
```

## [](#get-yourself-some-juice)**Налейте себе немного сока!**

Вот несколько важных приложений, которыми я пользуюсь ежедневно. Вы можете изучить их в Интернете и решить, устанавливать ли их с помощью DNF, через Flatpak или не устанавливать вообще. В качестве альтернативы вы можете довериться мне и выполнить команду как есть. Пожалуйста, обратите внимание, что, хотя репозиторий Fedora RPM может немного отставать от Flathub, такие пакеты, как GIMP и Inkscape, часто имеют больший вес по сравнению с RPM. В конечном итоге выбор остается за вами.

```
sudo dnf install mc bpytop inxi ncdu neofetch cmatrix gnome-tweaks syncthing qbittorrent thunderbird keepassxc gimp inkscape -y
```

### [](#enable-syncthing-autorun)**Enable Syncthing autorun**

Syncthing is a free and open-source file synchronization program that allows users to keep files in sync across multiple devices. The program operates on a peer-to-peer network, meaning that files are transferred directly between devices without relying on a central server. Syncthing is available for most major operating systems, including Windows, macOS, Linux, and Android.

It is important to enable Syncthing to run as a service so that it starts automatically when the system boots up, and continues to run even when the user is not logged in. This ensures that files remain in sync at all times, without requiring manual intervention.

```
sudo systemctl enable --now syncthing@[user].service
```

replace \[user\] with your username

To access the Syncthing web UI, simply navigate to http://localhost:8384 in your web browser. For more information on how to use Syncthing, refer to the official documentation.

### [](#do-it-right-with-libreoffice)**Do it right with LibreOffice**

LibreOffice is a free and open-source office suite that provides a range of tools for document editing, spreadsheet management, and presentation creation. The program is available for most major operating systems, including Windows, macOS, and Linux.

Remove .rpm instances to avoid mess and conflicts

```
sudo dnf remove libreoffice*
```

Installing LibreOffice via Flathub means that you will always have the latest release of the software available.

```
flatpak install flathub org.libreoffice.LibreOffice
```

Reinstall the module for locale and language aids to work properly with LibreOffice

```
flatpak install --reinstall org.freedesktop.Platform.Locale
```

TIP: Don’t forget to set the correct locale in the LibreOffice settings; otherwise, you risk incorrect formatting of documents and measurements.

### [](#install-some-flatpak-goodies)**Install some Flatpak goodies**

Starting from Fedora 38, the Flathub repository is unrestricted, so there’s no need to enable it manually. Now, you can proceed to install software from the official Flathub repository.

Take a moment to review which apps you would like to install.

#### [](#extensions-manager)[Extensions Manager](https://beta.flathub.org/apps/com.mattjakeman.ExtensionManager)

The Extension Manager is a tool for managing GNOME Shell extensions, allowing users to customize the appearance and functionality of their desktop environment.

```
flatpak install flathub com.mattjakeman.ExtensionManager
```

#### [](#telegram-desktop)[Telegram Desktop](https://beta.flathub.org/apps/org.telegram.desktop)

Telegram Desktop is a fast and secure messaging app that enables users to send text messages, make voice and video calls, and share files with other Telegram users. The app supports end-to-end encryption for added security and offers a range of features, including chat groups, channels, and bots.

```
flatpak install flathub org.telegram.desktop
```

#### [](#joplin)[Joplin](https://beta.flathub.org/apps/net.cozic.joplin_desktop)

Joplin is a free, open-source note-taking app with end-to-end encryption. It supports Markdown formatting, file attachments, and tagging, and is available on Windows, macOS, Linux, and mobile devices.

```
flatpak install flathub net.cozic.joplin_desktop
```

*Looking for an alternative? Check* [*Notesnook*](https://beta.flathub.org/apps/com.notesnook.Notesnook) *and* [*MarkText*](https://beta.flathub.org/apps/com.github.marktext.marktext)*.*

#### [](#onlyoffice-desktop)[OnlyOffice Desktop](https://beta.flathub.org/apps/org.onlyoffice.desktopeditors)

OnlyOffice Desktop is a free and open-source office suite that provides a range of tools for document editing, spreadsheet management, and presentation creation. The program offers full compatibility with Microsoft Office formats, making it a popular alternative for users who want to avoid the cost of proprietary software.

```
flatpak install flathub org.onlyoffice.desktopeditors
```

#### [](#bottles)[Bottles](https://beta.flathub.org/apps/com.usebottles.bottles)

Bottles is a free and open-source application that allows users to run Windows software on Linux and macOS operating systems. It does this by creating a virtual environment where Windows programs can be installed and run without the need for a full Windows installation.

```
flatpak install flathub com.usebottles.bottles
```

#### [](#pika-backup)[Pika Backup](https://beta.flathub.org/apps/org.gnome.World.PikaBackup)

Pika Backup is an open-source backup utility that allows users to easily create and manage backups of their files and directories.

```
flatpak install flathub org.gnome.World.PikaBackup
```

#### [](#pulsar)[Pulsar](https://beta.flathub.org/apps/dev.pulsar_edit.Pulsar)

A Community-led hyper-hackable text editor. Pulsar aims to not only reach feature parity with the original Atom, but to bring Pulsar into the 21st century by updating the underlying architecture, and supporting modern features.

```
flatpak install flathub dev.pulsar_edit.Pulsar
```

#### [](#remmina)[Remmina](https://beta.flathub.org/apps/org.remmina.Remmina)

Remmina is a free and open-source remote desktop client for Linux that allows users to connect to and control remote computers from a local system.

```
flatpak install flathub org.remmina.Remmina
```

#### [](#freetube)[FreeTube](https://beta.flathub.org/apps/io.freetubeapp.FreeTube)

FreeTube is a privacy-focused YouTube client for desktop that allows users to watch and download YouTube videos without ads or tracking.

```
flatpak install flathub io.freetubeapp.FreeTube
```

#### [](#onionshare)[OnionShare](https://beta.flathub.org/apps/org.onionshare.OnionShare)

OnionShare is a free, open-source tool that allows users to securely and anonymously share files or host websites over the Tor network. By creating a temporary, encrypted Tor hidden service, OnionShare enables the sender and recipient to bypass traditional methods of file sharing, such as email attachments or cloud-based services, ensuring privacy and security.

```
flatpak install flathub org.onionshare.OnionShare
```

## [](#install-appimage-launcher)**Install AppImage Launcher**

Integrate AppImages to your application launcher with one click, and manage, update and remove them from there. Double-click AppImages to open them, without having to make them executable first.

AppImageLauncher plays well with other applications managing AppImages, for example app stores. However, it doesn’t depend on any of those, and can run completely standalone.

```
https://github.com/TheAssassin/AppImageLauncher/releases/download/v2.2.0/appimagelauncher-2.2.0-travis995.0f91801.x86_64.rpm
```

```
sudo dnf install ./appimagelauncher-travis995.0f91801.x86_64.rpm
```

Applications for Linux without installation: [appimage.github.io](https://hansklepitko.github.io/app/joplin-desktop/resources/app/appimage.github.io)

## [](#settings-in-the-graphical-interface)**Settings in the graphical interface**

When launching the GNOME 44 Settings, it is highly recommended to go through all available sections and explore various settings. Don’t hesitate to experiment with different options and determine which ones require modification or customization. Don’t be intimidated, as it’s not as challenging as it may seem. Just keep an open mind and be willing to learn. As one of the first things you may consider to enable ‘Remote Desktop’. This is useful if you need to access your graphical desktop from remote locations. You can make additional changes in the ‘Sharing’ settings according to your needs. If you run multiple computers on your network or want to personalize your machine name, it’s also recommended to change the device name. ![](https://hansklepitko.github.io/fedora/images/share.png)

If you don’t require ‘Remote Login’ access, it’s recommended not to enable this option. Enabling remote login allows you to access your computer over SSH protocol, which may not be necessary for all users. If you’re not familiar with SSH and how it works, it’s best to leave this option disabled.

Check out the various settings in the ‘Settings’ Gnome App, as there are many options you may want to customize to your liking. Some of the settings you might want to adjust include the desktop appearance, privacy, power, region and language, users, keyboard layouts, and shortcuts. Ultimately, it’s up to your personal preference and needs.

## [](#gnome-appearance)**GNOME Appearance**

<img width="736" height="382" src="../../_resources/vanila-desktop_1646e981469141d2bec2e3c0f08a7665.png" class="jop-noMdConv">

> The Vanilla appearance of Gnome 44 desktop

Gnome is one of the two most popular Linux user environments, and it is known for its solid workflow. It suits my preference for a close-to-vanilla setup. However, for users migrating from Windows may necessitate some learning, which can be mitigated by the use of certain extensions. We typically use the [We10x icon theme](https://github.com/yeyushengfan258/We10X-icon-theme), GNOME Tweaks, and the following extensions for this:

- ArcMenu
- Dash-to-Panel
- Desktop Icons NG (DING)
- AppIndicator and KStatusNotifierItem Support

<img width="736" height="382" src="../../_resources/desktop-01_560b966157424c5db9b8462516264831.png" class="jop-noMdConv">

> A customized version with a Windows-like look and feel

<img width="736" height="382" src="../../_resources/desktop-02_7cd56579e8e24804aa622aa0fcccbefc.png" class="jop-noMdConv">

> Dash to Panel, ArcMenu, Desktop Icons NG and LibreOffice with tabbed user interface

## [](#additional-configuration)**Additional configuration**

In this section, I’ll go over some extra settings and apps that aren’t strictly necessary but can greatly improve your system’s user experience, usability, and privacy.

### [](#windows-fonts)**Windows fonts**

To begin, make sure that you have the required packages installed on your system. Most systems already have these packages, but you can install them by running the following command in the terminal:

```
sudo dnf install curl cabextract xorg-x11-font-utils fontconfig
```

Once you have the required packages, you can proceed to download and install the Microsoft Core Fonts package. Simply run the following command in the terminal:

```
sudo rpm -i https://downloads.sourceforge.net/project/mscorefonts2/rpms/msttcore-fonts-installer-2.6-1.noarch.rpm
```

After the installation process completes, you should receive a message confirming that the Microsoft Fonts have been successfully installed.

Instead of using the first method, you can also copy the fonts directory directly from your Windows instance to a new directory `/home/[username]/.fonts`, with `[username]` replaced by your actual username. I prefer this method because it offers better cross-platform compatibility, especially if you frequently work with text documents created on Windows.

### [](#vivaldi)**Vivaldi**

Choosing a web browser is a personal decision that depends on individual preferences. Personally, I prefer to use Vivaldi, a feature-rich browser with a user-friendly interface. However, it’s important to note that Vivaldi is a closed-source program, which may be a trade-off for some users.

Go to Vivaldi website and download, then install the RPM package, or do the following:

```
wget https://downloads.vivaldi.com/stable/vivaldi-stable-5.7.2921.65-1.x86_64.rpm
```

```
sudo apt install ./vivaldi-stable-5.7.2921.65-1.x86_64.rpm
```

*At the time of writing, version v5.7.2921.65-1 is the latest release.*

### [](#tor-service-and-tor-browser)**Tor service and Tor browser**

The Tor network is a free and open-source software that enables anonymous communication on the internet by encrypting and routing traffic through a network of relays run by volunteers worldwide. The Tor network provides several features that make it a popular choice for privacy-conscious users, including:

- Anonymous browsing: Tor hides your IP address and online activity from anyone trying to track your movements online, including your internet service provider (ISP), advertisers, or government agencies.
- Access to blocked content: Tor allows you to bypass censorship and access content that may be blocked in your region or by your ISP.
- Enhanced security: Tor’s encryption and routing help protect against certain types of online attacks, such as man-in-the-middle attacks and some forms of malware. Onion services are websites or services hosted on the Tor network that are only accessible through the Tor browser. Discovering onion services can be beneficial to your privacy because they provide an additional layer of anonymity. When you access an onion service, your connection is encrypted and routed through the Tor network, making it difficult for anyone to trace your activity back to your physical location or IP address. By utilizing the Tor network and onion services, you can take control of your online privacy and browse the web with confidence.

Let’s install the Tor network:

```
sudo dnf install tor
```

```
sudo systemctl enable --now tor
```

### [](#zerotier)**ZeroTier**

ZeroTier is a software-defined networking tool that allows users to create virtual Ethernet networks and connect devices, servers, and containers to these networks regardless of their physical location or network configuration. It offers secure end-to-end encrypted communication over the internet, making it a useful tool for remote teams, gaming, and IoT devices. ZeroTier is available for a wide range of platforms including Windows, macOS, Linux, Android, and iOS, and can be used for both personal and commercial purposes.

To install ZeroTier, simply execute the command below:

```
curl -s https://install.zerotier.com | sudo bash
```

For further information on configuring and using your ZeroTier network, visit the official [ZeroTier documentation](https://docs.zerotier.com).

### [](#ztncui)**ztncui**

[ztncui](https://github.com/key-networks/ztncui) is a web-based user interface for the ZeroTier network controller (ZTNC) API, which allows users to manage ZeroTier networks and devices. It provides a graphical interface for adding, deleting, and managing networks and devices, as well as viewing network statistics and device information. It is designed to be used alongside the ZeroTier network virtualization service, which allows users to create and manage virtual networks that can be accessed securely over the internet.

To manage a virtual network with over 25 devices, it is recommended to install the ztncui controller on a dedicated Debian/Ubuntu server. However, if you have fewer devices in your virtual network, you can use the controller provided by ZeroTier, Inc. This controller can be accessed after logging in on their official website.

### [](#tabby)**Tabby**

Tabby is an infinitely customizable cross-platform terminal app for local shells, serial, SSH and Telnet connections. You can download the latest release from: [github.com/Eugeny/tabby/releases](https://github.com/Eugeny/tabby/releases)

```
wget https://github.com/Eugeny/tabby/releases/download/v1.0.196/tabby-1.0.196-linux-x64.rpm
```

```
sudo dnf install ./tabby-1.0.196-linux-x64.rpm
```

*At the time of writing, version v1.0.196 is the latest release.*

### [](#nomachine)**NoMachine**

NoMachine is a remote desktop software that allows users to access and control their computer or server from anywhere, using any device. It offers a secure and high-performance remote desktop experience with features such as file transfer, screen sharing, and virtual desktops. NoMachine supports various platforms including Windows, Linux, macOS, Android, and iOS.

```
wget https://download.nomachine.com/download/8.4/Linux/nomachine_8.4.2_1_x86_64.rpm
```

```
sudo dnf install ./nomachine_8.4.2_1_x86_64.rpm
```

*At the time of writing, version v8.4.2 is the latest release.*

### [](#waydroid)**Waydroid**

Waydroid is a compatibility layer that enables running Android applications natively on a GNU/Linux distribution. Unlike traditional Android emulators, Waydroid does not rely on virtualization technology or emulation. Instead, it leverages the Linux container technology to provide a lightweight and efficient solution for running Android apps on desktop Linux. This allows users to seamlessly integrate Android apps into their Linux workflow without compromising performance or security.

Waydroid can be installed from the official package repository.

```
sudo dnf install waydroid
```

```
sudo systemctl enable --now waydroid-container
```

After installing, launch Waydroid from the applications menu and proceed with the initialization by pasting these URLs in the OTA fields:

![](../../_resources/waydroid_180fc16c0d3e45e79d2d36961151ede3.png)

System OTA: `https://ota.waydro.id/system`

Vendor OTA: `https://ota.waydro.id/vendor`

During the initialization process, you can select between two types of Android: **VANILLA** and **GAPPS**. The VANILLA option refers to a basic Android system with no pre-installed apps or services, whereas the GAPPS option includes pre-installed Google apps and services such as Google Play Store, Google Maps, and Gmail. Your preference and needs will determine which of these two options you choose. You can select VANILLA if you prefer a more minimalist approach or do not require Google apps and services. If you rely on Google services, the GAPPS option is preferable.

If you’ve decided to use the GAPPS version of Waydroid, you’ll need to get it certified. The certification process is described in the following link: https://docs.waydro.id/faq/google-play-certification

## [](#bonus-section)**Bonus section**

In this section, I will introduce advanced settings and apps that go beyond the regular user configuration and require additional knowledge and technical expertise. These settings and apps can greatly enhance your system’s functionality, security, and privacy, but they may also require some technical skills and tinkering. So if you’re a tech-savvy user looking to explore some of the more advanced features of your system, this section is for you!

### [](#brave-search)**Brave Search**

[Brave Search](https://search.brave.com) is a privacy-focused search engine that provides users with a safer and more reliable search experience. Unlike other widely used search engines that track user data and show personalized results, Brave Search does not collect or share user data and provides completely anonymous search results. The engine uses its own index and algorithms to deliver results, ensuring that users receive unbiased and unfiltered search results. Additionally, Brave Search allows users to customize their search experience with features such as region-specific results and ad-free search results. While we suggest giving Brave Search a try, the choice of search engine is always in the hands of the user.

### [](#fish)**Fish**

Fish (friendly interactive shell) is a smart and user-friendly command line shell that works on Linux, MacOS, and other operating systems. Use it for everyday work in your terminal and for scripting. Scripts written in fish are less cryptic than their equivalent bash versions.

https://fedoramagazine.org/fish-a-friendly-interactive-shell/

### [](#sparrow-bitcoin-wallet)**Sparrow Bitcoin Wallet**

[Sparrow](https://sparrowwallet.com) is a Bitcoin wallet for those who value financial self sovereignty. Sparrow’s emphasis is on security, privacy and usability. Sparrow does not hide information from you - on the contrary it attempts to provide as much detail as possible about your transactions and UTXOs, but in a way that is manageable and usable.

Download the latest release of the software, which at the time of writing is version v1.7.4-1.

```
# wget https://github.com/sparrowwallet/sparrow/releases/download/1.7.4/sparrow-1.7.4-1.x86_64.rpm

# sudo dnf install ./sparrow-1.7.4-1.x86_64.rpm
```

Now it’s time to delve into the world of Bitcoin. Personally, I can recommend a few good books to get you started:

- Nik Bhatia - Layered Money
- Saifedean Ammous - The Bitcoin Standard
- Andreas Antonopoulos - The Internet of Money
- Gigi - 21 Lessons
- Yan Pritzker - Inventing Bitcoin

### [](#the-power-of-tor)**The Power of Tor**

In the `/etc/tor/torrc` config file, you can make several additional configurations to customize your Tor installation. Some examples of additional configurations you can make are:

- **Enabling Tor to act as a relay**: If you want to contribute to the Tor network, you can configure your installation to act as a relay by uncommenting the ORPort and Nickname lines and adding your own values.
- **Limiting the bandwidth**: By default, Tor uses a significant amount of bandwidth, which may be a concern for some users. You can limit the bandwidth usage by uncommenting the RelayBandwidthRate and RelayBandwidthBurst lines and setting your own values.
- **Changing the exit policy**: By default, Tor allows connections to all destinations, but you can customize the exit policy to restrict connections to specific domains or ports. You can do this by uncommenting the ExitPolicy line and adding your own rules.
- **Enabling onion services**: If you want to host your own onion services, you can enable this feature by uncommenting the HiddenServiceDir and HiddenServicePort lines and adding your own values.
- **Enabling IPv6 support**: By default, Tor only supports IPv4 connections. If you want to enable IPv6 support, you can uncomment the IPv6Exit and IPv6Traffic lines.

It’s important to note that making changes to the torrc file requires some technical knowledge and should be done carefully to avoid breaking your Tor installation or compromising your anonymity.

#### [](#ssh-over-tor)**SSH over Tor**

Enabling SSH over Tor involves configuring Tor to listen on a specific port and configuring SSH to use that port to connect to the Tor network. Here are the general steps:

1.  Install Tor and OpenSSH on the server.
2.  Edit the Tor configuration file `/etc/tor/torrc` to add the following lines:

```
HiddenServiceDir /var/lib/tor/ssh/
HiddenServicePort 22 127.0.0.1:22
```

This configures Tor to listen on port 22 on the local machine and forward traffic to that port to an onion service hosted on Tor. 3. Restart Tor to apply the changes

```
sudo systemctl restart tor
```

1.  Retrieve the hostname and private key of the newly generated onion service by looking at the file `/var/lib/tor/ssh/hostname`.
2.  Edit the SSH configuration file (/etc/ssh/sshd_config) to add the following lines:

```
ListenAddress 127.0.0.1
Port 22
```

This configures SSH to listen on port 22 on the local machine. 6. Restart the SSH daemon to apply the changes

```
sudo systemctl restart sshd
```

1.  Connect to the server using the onion address and the private key. For example, if the onion address is abc123.onion and the private key is stored in id_ed25519, use the following command:

```
ssh -i /path/to/id_ed25519 user@abc123.onion
```

By connecting to the server through the Tor network, your connection will be encrypted and anonymized, providing an additional layer of privacy and security.

…

### [](#pihole-and-pivpn)Pihole and pivpn

…

### [](#veracrypt)VeraCrypt

…

### [](#matrix-and-element)Matrix and Element

…

### [](#umbrel-and-citadel)Umbrel and Citadel

…

* * *

*Reference:*

[*https://www.linuxcapable.com/increase-dnf-speed-on-fedora-linux*](https://www.linuxcapable.com/increase-dnf-speed-on-fedora-linux)

[*https://docs.fedoraproject.org/en-US/quick-docs/autoupdates*](https://docs.fedoraproject.org/en-US/quick-docs/autoupdates)

[*https://docs.fedoraproject.org/en-US/quick-docs/setup_rpmfusion*](https://docs.fedoraproject.org/en-US/quick-docs/setup_rpmfusion)

[*https://firewalld.org/documentation*](https://firewalld.org/documentation)

[*https://docs.syncthing.net*](https://docs.syncthing.net)

[*https://key-networks.com/ztncui/#installation*](https://key-networks.com/ztncui/#installation)

[*https://docs.zerotier.com*](https://docs.zerotier.com)

[*https://github.com/yeyushengfan258/We10X-icon-theme*](https://github.com/yeyushengfan258/We10X-icon-theme)

[*https://techviewleo.com/install-fish-shell-oh-my-fish-on-fedora*](https://techviewleo.com/install-fish-shell-oh-my-fish-on-fedora)

[*https://techviewleo.com/configure-fish-shell-with-oh-my-fish*](https://techviewleo.com/configure-fish-shell-with-oh-my-fish)

[*https://tabby.sh*](https://tabby.sh)

[*https://www.bitcoinerbooks.com/books*](https://www.bitcoinerbooks.com/books)