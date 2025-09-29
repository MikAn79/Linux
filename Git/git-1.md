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

# git

1 February, 2023

  

* * *

#### Клонируем репозиторий на свой комп

~$ mkdir mygit

~$ git clone https://github.com/username/username.github.io

теперь он в директории /mygit

Идём туда(папку) и смотрим что там:

~$ cd mygit

~$ ls

cheat.sh \_config.yml LICENSE Makefile README.md или что там есть..

* * *

#### Можем править файлы

$ echo -e “\\nGo to [username.github.io](https://github.com/username/username.github.io) \\for get some tips! “ » README.md

$ echo -e “или пишем что-то другое” » README.md

(лучше все делать в специальном редакторе VS code,PyCharm…)

***Фиксируем изменения и коммитим:***

$ git add .

$ git commit -m “ ‘1 2 3’ или любой другой commit”

* * *

#### Push it on github

***Отправляем изменения на GIT:***

~$ git add –all

~$ git commit -m ‘1 2 3’

~$ git push -u origin master

git remote add origin:

* * *

## Иногда нужно:

***Правим,редактируем…***

• Hello World

*Enter the project folder and add an index.html file:*

~ $cd username.github.io

~ $echo “Hello World” > index.html

* * *

***Отправляем изменения на GIT:***

• Push it

*Add, commit, and push your changes:*

~ $git add –all

~ $git commit -m “Initial commit”

~ $git push -u origin main