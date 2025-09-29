#rsiync #резерв #резервное #копирование


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

# Rsync - резервное копирование.

24 January, 2024

### Rsync - резервное копирование.

(на локальной машине)

*Создание начальной копии файлов займет какое-то время,остальные копии записывают только изменения и новые файлы.*

Ввести в терминале нужную команду:

```
- /  (корневой каталог)

rsync -aAXvzh / --exclude={"/home/*","/dev/*","/proc/*","/sys/*","/tmp/*","/run/*","/mnt/*","/media/*","/lost+found"} /home/user/root_copy/



sudo rsync -aAXvzh / --exclude={"/home/*","/dev/*","/proc/*","/sys/*","/tmp/*","/run/*","/mnt/*","/media/*","/lost+found"} /home/user/root_copy/ 

--- 

- /home  (домашний каталог)

rsync -aAXvzh /home/user --exclude={'/user/.cache','/user/.local','/user/мое_фото','/user/мое_видео','/user/музыка_видео','/user/Музыка'} /run/media/user/da6b9bec-08ef-4205-ac88-d373c65699bd/home_copy/ 


sudo rsync -aAXvzh /home/usert --exclude={'/user/.cache','/user/.local','/user/мое_фото','/user/мое_видео','/user/музыка_видео','/user/Музыка'} /run/media/user/da6b9bec-08ef-4205-ac88-d373c65699bd/home_copy/ 
```

### Расшифровка:

*чем(rsync) - опции(-aAXvzh) - откуда(/ или /home/user) - исключения(–exclude=) - куда(/home/user/root_copy/ или /run/media/user/da6b9bec-08ef-4205-ac88-d373c65699bd/home_copy/)*

- \-a - Режим архивирования, когда сохраняются все атрибуты оригинальных файлов
    
- \-A - сохранить списки управления доступом
    
- \-X - сохранение расширенных атрибутов
    
- \-v - Выводить подробную информацию о процессе копирования
    
- \-z - Сжимать файлы перед передачей
    
- \-h - выводите числа в удобочитаемом формате
    
- –exclude - исключенные каталоги
    

### Можно сделать простой скрипт:

```
#!/bin/bash

sudo rsync -aAXvzh / --exclude={"/home/*","/dev/*","/proc/*","/sys/*","/tmp/*","/run/*","/mnt/*","/media/*","/lost+found"} /home/user/root_copy/
```

Сохранить с именем:

- script.sh (или Ваше имя)

Сделать исполняемым:

- chmod + x script.sh

### Запуск скрипта без sudo

Нужно отредактировать **/etc/sudoers** и разрешить запускать файл с от рута.

Allow members of group sudo to execute any command(Разрешить членам группы sudo выполнять любую команду)

Запустить в терминале:

- sudo nano /etc/sudoers

Добавить запись:

(имя скрипта и место нахождения вставьте свое)

- ALL=(ALL:ALL) ALL user ALL = NOPASSWD: /home/user/Documents/script.sh

&nbsp;

### Rsync - Опции

Теперь давайте кратко рассмотрим параметры rsync. Здесь перечислены не все опции. Для более подробной информации смотрите man rsync:

- \-v - Выводить подробную информацию о процессе копирования;
    
- \-q - Минимум информации;
    
- \-c - Проверка контрольных сумм для файлов;
    
- \-a - Режим архивирования, когда сохраняются все атрибуты оригинальных файлов;
    
- \-R - Относительные пути;
    
- \-b - Создание резервной копии;
    
- \-u - Не перезаписывать более новые файлы;
    
- \-l - Копировать символьные ссылки;
    
- \-L - Копировать содержимое ссылок;
    
- \-H - Копировать жесткие ссылки;
    
- \-p - Сохранять права для файлов;
    
- \-g - Сохранять группу;
    
- \-t - Сохранять время модификации;
    
- \-x - Работать только в этой файловой системе;
    
- \-e - Использовать другой транспорт, например, ssh;
    
- \-z - Сжимать файлы перед передачей;
    
- \-h - выводите числа в удобочитаемом формате
    
- \-A - сохранить списки управления доступом
    
- \-X - сохранение расширенных атрибутов
    
- –delete - Удалять файлы которых нет в источнике;
    
- –exclude - Исключить файлы по шаблону;
    
- –include - Включить файлы по шаблону
    
- –recursive - Перебирать директории рекурсивно;
    
- –no-recursive - Отключить рекурсию;
    
- –progress - Выводить прогресс передачи файла;
    
- –stat - Показать статистику передачи;
    
- –version - Версия утилиты.