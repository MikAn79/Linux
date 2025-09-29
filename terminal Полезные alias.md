[TOC]

## Команда alias

Чтобы посмотреть какие псевдонимы linux команд уже заданны в вашей системе просто выполните:

`alias`

В выводе вы увидите что-то подобное:

[![](https://losst.pro/wp-content/uploads/2016/05/Snimok-ekrana-ot-2020-09-21-18-40-06-1024x576.png)](https://losst.pro/wp-content/uploads/2016/05/Snimok-ekrana-ot-2020-09-21-18-40-06.png)

Команда покажет все alias команд linux определенные для текущего пользователя. Вывод очень сильно зависит от вашего дистрибутива. Общий синтаксис команды выглядит следующим образом:

**$ alias имя="значение"**

**$ alias имя="команда аргумент1 аргумент2"**

**$ alias имя="/путь/к/исполняемому/файлу"**

Вы можете создавать новые псевдонимы, просто выполняя эту команду в терминале. Но созданные таким образом алиасы linux будут работать только в этом терминале и только до его закрытия.

Давайте для примера создадим alias linux для такой часто используемой команды, как clear (очистить вывод терминала):

`alias c='clear'`

Теперь, чтобы очистить терминал достаточно выполнить:

`с`

Удалить созданный alias можно с помощью команды unalias:

`unalias c`

Но как я уже сказал, такие alias команд linux сохраняются только до закрытия терминала. Поэтому их необходимо создавать в начале каждой терминальной сессии. Для этого можно записать все нужные команды в **~/.bashrc**. При запуске терминала, каждый раз выполняется этот скрипт, чтобы установить переменные окружения и подготовить оболочку. Таким образом добавив нужные строки в конец файла мы получим работающие alias linux в каждом терминале.

Продолжим создание alias для команды clear:

`vi ~/.bashrc`

Добавьте эту строку в конец файла:

`alias c='clear'`

Затем сохраните и закройте редактор (:wq). Чтобы проверить работоспособность запустите новый терминал. Если вы хотите чтобы ваши алиасы linux были доступны для всех пользователей, необходимо использовать файл /etc/bashrc.

Поскольку **.bashrc**, это обычный bash скрипт, перед нами открываются большие возможности. Например мы можем добавить alias команд с использованием sudo, если текущий пользователь не root:

`if [ $UID -ne 0 ]; then alias reboot='sudo reboot' alias update='sudo apt-get upgrade' fi`

Ещё мы можем менять команды алиасов в зависимости от дистрибутива:

`_myos="$(uname)" case $_myos in Linux) alias foo='/path/to/linux/bin/foo';; FreeBSD|OpenBSD) alias foo='/path/to/bsd/bin/foo' ;; SunOS) alias foo='/path/to/sunos/bin/foo' ;; *) ;; esac`

Чтобы удалить alias достаточно просто удалить запись о нем, из того файла в который вы её добавили. Мы рассмотрели основы добавления alias linux, теперь давайте перейдем к списку полезных алиасов linux.

## Полезные alias Linux

Вы можете добавить в своей системе любые или даже все эти алиасы linux чтобы повысить продуктивность своей работы в терминале.

### 1\. Вывод ls

Цветной вывод:

`alias ls='ls --color=auto'`

Показывать скрытые файлы и представлять вывод в виде списка:

`alias ll='ls -la'`

Показать только скрытые файлы:

`alias l.='ls -d .* --color=auto'`

### 2\. Перемещение по каталогам

Исправляем опечатку:

`alias cd..='cd ..'`

Быстрое перемещение от текущей директории:

`alias ..='cd ..' alias ...='cd ../../../' alias ....='cd ../../../../' alias .....='cd ../../../../' alias .4='cd ../../../../' alias .5='cd ../../../../..'`

### 3\. Вывод grep

Делаем вывод цветным:

`alias grep='grep --color=auto' alias egrep='egrep --color=auto' alias fgrep='fgrep --color=auto'`

### 4\. Калькулятор

Запускать калькулятор с поддержкой стандартной библиотеки mathlib:

`alias bc='bc -l'`

### 5\. Создание каталогов

Создавать дерево каталогов, если оно не существует:

`alias mkdir='mkdir -pv'`

### 6\. Вывод diff

Делаем вывод diff цветным:

`alias diff='colordiff'`

### 7\. Вывод mount

Сделаем вывод mount читаемым:

`alias mount='mount | column -t'`

### 8\. История

Сократим команды для экономии времени:

`alias h='history' alias j='jobs -l'`

### 9\. Информация и дата

`alias path='echo -e ${PATH//:/\\n}' alias now='date +"%T"' alias nowtime=now alias nowdate='date +"%d-%m-%Y"'`

### 10\. Редактор Vim

alias команд linux для использования редактора vim по умолчанию:

`alias vi=vim alias svi='sudo vi' alias vis='vim "+set si"' alias edit='vim'`

### 11\. Ping

Посылать только пять запросов:

`alias ping='ping -c 5'`

Интервал между запросами одна секунда:

`alias fastping='ping -c 100 -s.2'`

### 12\. Открытые порты

`alias ports='netstat -tulanp'`

### 13\. Wakeup

Будим серверы в режиме сна по mac адресу с помощью утилиты wakeonlan:

`alias wakeupnas01='/usr/bin/wakeonlan 00:11:32:11:15:FC' alias wakeupnas02='/usr/bin/wakeonlan 00:11:32:11:15:FD' alias wakeupnas03='/usr/bin/wakeonlan 00:11:32:11:15:FE'`

### 14\. Управление iptables

`alias iptlist='sudo /sbin/iptables -L -n -v --line-numbers' alias iptlistin='sudo /sbin/iptables -L INPUT -n -v --line-numbers' alias iptlistout='sudo /sbin/iptables -L OUTPUT -n -v --line-numbers' alias iptlistfw='sudo /sbin/iptables -L FORWARD -n -v --line-numbers' alias firewall=iptlist`

### 15\. Утилита curl

Получить заголовки сервера:

`alias header='curl -I'`

Проверять поддержку сжатия на сервере:

`alias headerc='curl -I --compress'`

### 16\. Работа с файлами

Не удалять корень и предупреждать об удалении файлов:

`alias rm='rm -I --preserve-root'`

Предупреждения:

`alias mv='mv -i' alias cp='cp -i' alias ln='ln -i'`

Защита от изменения прав для /:

`alias chown='chown --preserve-root' alias chmod='chmod --preserve-root' alias chgrp='chgrp --preserve-root'`

### 17\. Обновление Debian

Установка пакета:

`alias apt="sudo apt" alias updatey="sudo apt --yes"`

Обновление одной командой:

`alias update='sudo apt update && sudo apt upgrade'`

### 18\. Обновление RedHat

В семействе дистрибутивов Red Hat используется пакетный менеджер yum:

`alias update='yum update' alias updatey='yum -y update'`

### 19\. Стать суперпользователем

`alias root='sudo -i' alias su='sudo -i'`

### 20\. Выключение

Выполнять команды выключения через sudo:

`alias reboot='sudo /sbin/reboot' alias poweroff='sudo /sbin/poweroff' alias halt='sudo /sbin/halt' alias shutdown='sudo /sbin/shutdown'`

### 21\. Управление серверами

`alias nginxreload='sudo /usr/bin/nginx -s reload' alias nginxtest='sudo /usr/bin/nginx -t' alias lightyload='sudo systemctl reload lighttpd' alias lightytest='sudo /usr/sbin/lighttpd -f /etc/lighttpd/lighttpd.conf -t' alias httpdreload='sudo /usr/sbin/apachectl -k graceful' alias httpdtest='sudo /usr/sbin/apachectl -t && /usr/sbin/apachectl -t -D DUMP_VHOSTS'`

### 22\. Мультимедиа

Открыть видео в текущей директории:

`alias playavi='mplayer *.avi' alias vlc='vlc *.avi'`

Добавить в плейлист музыку из текущей директории:

`alias playwave='for i in *.wav; do mplayer "$i"; done' alias playogg='for i in *.ogg; do mplayer "$i"; done' alias playmp3='for i in *.mp3; do mplayer "$i"; done'`

Открыть музыку из устройства nas:

`alias nplaywave='for i in /nas/multimedia/wave/*.wav; do mplayer "$i"; done' alias nplayogg='for i in /nas/multimedia/ogg/*.ogg; do mplayer "$i"; done' alias nplaymp3='for i in /nas/multimedia/mp3/*.mp3; do mplayer "$i"; done'`

### 22\. Системное администрирование

Работать с интерфейсом eth1:

`alias dnstop='dnstop -l 5 eth1' alias vnstat='vnstat -i eth1' alias iftop='iftop -i eth1' alias tcpdump='tcpdump -i eth1' alias ethtool='ethtool eth1'`

Работать с интерфейсом wlan0 по умолчанию:

`alias iwconfig='iwconfig wlan0'`

### 23\. Информация о системе

Использование памяти:

`alias meminfo='free -m -l -t'`

Показать процессы потребляющие больше всего памяти:

`alias psmem='ps auxf | sort -nr -k 4' alias psmem10='ps auxf | sort -nr -k 4 | head -10'`

Показать процессы использующие процессор:

`alias pscpu='ps auxf | sort -nr -k 3' alias pscpu10='ps auxf | sort -nr -k 3 | head -10'`

Информация о процессоре:

`alias cpuinfo='lscpu'`

Посмотреть память видеокарты:

`alias gpumeminfo='grep -i --color memory /var/log/Xorg.0.log'`

### 25\. Утилита wget

Продолжать незавершенную загрузку по умолчанию:

`alias wget='wget -c'`

### 26\. Браузеры

Сокращения:

`alias ff='/usr/bin/firefox' alias chrome='/usr/bin/google-chrome' alias opera='/usr/bin/opera' alias chromium='/usr/bin/chromium'`

Браузер по умолчанию:

`alias browser=chrome`

### 27\. Правильные единицы измерения

Правильное отображение данных для free, df и du:

×

`alias df='df -H' alias du='du -ch' alias free='free -h'`

