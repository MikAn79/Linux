## Основные команды[](#basic-commands "Permalink to this heading")

В этом разделе описаны основные команды, необходимые для установки, запуска и управления приложениями Flatpa Чтобы получить полный список команд Flatpak, выполните `flatpak --help` или посмотрите:flatpak-command-reference.

### Список удалённых компьютеров[](#list-remotes "Permalink to this heading")

Чтобы вывести список удалённых компьютеров, которые вы настроили в своей системе, запустите:

```
$ flatpak remotes

```

Это дает список существующих удалённых, которые были добавлены. Список Список указывает, был ли добавлен каждый удалённый компьютер для каждого пользователя или в масштабе всей системы.

### Добавить удалённый компьютер[](#add-a-remote "Permalink to this heading")

Самый удобный способ добавить удалённый компьютер \- использовать файл `.flatpakrepo`,который включает в себя как сведения о удалённом, так и его ключ GPG

```
$ flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

```

Здесь `flathub` \- это локальное имя, присвоенное удаленному устройству. URL-адрес указывает на удаленный файл `.flatpakrepo` `--if-not-exists` останавливает команду от выдачи ошибки, если удалённый компьютер уже существуе

### Удалить удалённый компьютер[](#remove-a-remote "Permalink to this heading")

Чтобы удалить удалённый компьютер, запустите:

```
$ flatpak remote-delete flathub

```

В этом случае flathub - это локальное имя удалённого компьютера.

### Поиск[](#search "Permalink to this heading")

Приложения можно найти в любом из ваших удалённых компьютеров с помощью команды \[[](#)`](#id1)search`Например:

```
$ flatpak search gimp

```

Поиск вернет все приложения, соответствующие условиям поиска. Каждый результат поиска включает идентификатор приложения и удалённый компьютер,на котором находится приложение. В этом примере поисковым запросом является `gimp`.

- ## Install Flatpak
    
    To install Flatpak on Kubuntu 18.10 (Cosmic Cuttlefish), simply run:
    
    ```
     $ sudo apt install flatpak 
    ```
    
    With older Kubuntu versions, the official Flatpak PPA is the recommended way to install Flatpak. To install it, run the following in a terminal:
    
    $ `sudo add-apt-repository ppa:alexlarsson/flatpak`
    $ `sudo apt update`
    $ `sudo apt install flatpak`
    
- ## Install the Discover Flatpak backend
    
    The Flatpak plugin for the Software app makes it possible to install apps without needing the command line (available on Kubuntu 18.04 and newer). To install on 18.04, run:
    
    ```
     $ sudo apt install plasma-discover-flatpak-backend 
    ```
    
    On Kubuntu 20.04 or later, you should run this instead:
    
    ```
     $ sudo apt install plasma-discover-backend-flatpak 
    ```
    
- ## Add the Flathub repository
    
    Flathub is the best place to get Flatpak apps. To enable it, run:
    
    ```
     $ flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo 
    ```
    
- ## Restart
    
    To complete setup, restart your system. Now all you have to do is [install some apps](https://flathub.org/)!
    

### Установка приложений[](#install-applications "Permalink to this heading")

Чтобы установить приложение, запустите:

```
$ flatpak install flathub org.gimp.GIMP

```

Здесь «flathub» - это имя удаленного компьютера, с которого должно быть установлено приложение, а `org.gimp.GIMP` \- это идентификатор приложения.

Иногда приложению требуется определенная среда выполнения, и она будет установлена перед приложением.

Детали устанавливаемого приложения также могут быть предоставлены в файле с расширением `.flatpakref`, который может быть удаленным или локальным. Чтобы указать `.flatpakref` вместо того, чтобы вручную указывать удаленный идентификатор и идентификатор приложения,выполните:

```
$ flatpak install https://flathub.org/repo/appstream/org.gimp.GIMP.flatpakref

```

Если в файле `.flatpakref`. указано, что приложение должно быть установлено с удаленного компьютера, который еще не был добавлен, перед установкой приложения вам будет предложено добавить его.

Начиная с Flatpak 1.2, команда `install` может искать приложения. просто:

```
$ flatpak install gimp

```

подтвердит удалённый компьютер и приложение и перейдет к установке.

### Запущенные приложения[](#running-applications "Permalink to this heading")

После того, как приложение установлено, его можно запустить с помощью команды `run` и идентификатора приложения:

```
$ flatpak run org.gimp.GIMP

```

### Обновление[](#updating "Permalink to this heading")

Чтобы обновить все установленные приложения и среды выполнения до последней версии, запустите:

```
$ flatpak update

```

### Список установленных приложений[](#list-installed-applications "Permalink to this heading")

Чтобы вывести список установленных приложений и сред выполнения, запустите:

```
$ flatpak list

```

для того, чтобы просто вывести список установленных приложений, запустите:

```
$ flatpak list --app

```

### Удалить приложение[](#remove-an-application "Permalink to this heading")

Чтобы удалить приложение, запустите:

```
$ flatpak uninstall org.gimp.GIMP

```

### Исправление проблем[](#troubleshooting "Permalink to this heading")

У Flatpak есть несколько команд, которые могут помочь вам снова заставить все работать, когда что-то пойдет не так.

Чтобы удалить среды выполнения и расширения, которые не используются установленными приложениями, используйте:

```
$ flatpak uninstall --unused

```

Чтобы исправить несоответствия с вашей локальной установкой, используйте:

```
$ flatpak repair

```

Flatpak также имеет ряд команд для управления разрешениями портала установленных приложений. Чтобы сбросить все разрешения портала для приложения, используйте команду `flatpak permission-reset`:

```
$ flatpak permission-reset org.gimp.GIMP

```

Чтобы узнать, какие изменения были внесены в вашу установку Flatpak с течением времени, вы можете просмотреть журналы (начиная с 1.2):

```
$ flatpak history
```