[[cron]]

Есть много полезных консольных команд и приложений.Одна из них Cron(crontab).

```
#|  hour (0-23),
#|  |   day of the month (1-31),
#|  |   |   month of the year (1-12),
#|  |   |   |   day of the week (0-6 with 0=Sunday).
#|  |   |   |   |   commands
```

- crontab -e – открывает конфигурационный файл
    
- crontab -l – показывает список задач из конфигурационного файла (все, что было запланировано).
    
- crontab -r – удаляет конфигурационный файл вместе со всеми запланированными задачами.
    
- сrontab -v – показывает, когда в последний раз открывался конфигурационный файл.
    

#### crontab -e создать задание(работает от имени пользователя,без sudo)[](#crontab--e-%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D1%82%D1%8C-%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%D0%B5%D1%82-%D0%BE%D1%82-%D0%B8%D0%BC%D0%B5%D0%BD%D0%B8-%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D0%B5%D0%BB%D1%8F%D0%B1%D0%B5%D0%B7-sudo)

### Cron.Его основная задача выполнять нужные процессы в нужное время.(Планировщик заданий)[](#cron.%D0%B5%D0%B3%D0%BE-%D0%BE%D1%81%D0%BD%D0%BE%D0%B2%D0%BD%D0%B0%D1%8F-%D0%B7%D0%B0%D0%B4%D0%B0%D1%87%D0%B0-%D0%B2%D1%8B%D0%BF%D0%BE%D0%BB%D0%BD%D1%8F%D1%82%D1%8C-%D0%BD%D1%83%D0%B6%D0%BD%D1%8B%D0%B5-%D0%BF%D1%80%D0%BE%D1%86%D0%B5%D1%81%D1%81%D1%8B-%D0%B2-%D0%BD%D1%83%D0%B6%D0%BD%D0%BE%D0%B5-%D0%B2%D1%80%D0%B5%D0%BC%D1%8F.%D0%BF%D0%BB%D0%B0%D0%BD%D0%B8%D1%80%D0%BE%D0%B2%D1%89%D0%B8%D0%BA-%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B9)

#### На моем примере:[](#%D0%BD%D0%B0-%D0%BC%D0%BE%D0%B5%D0%BC-%D0%BF%D1%80%D0%B8%D0%BC%D0%B5%D1%80%D0%B5)

- crontab -e
    
- 00 09 16 \* \* echo ‘date’ > /home/jenit/Desktop/date.txt
    

### В Astra Linux так делать запись:[](#%D0%B2-astra-linux-%D1%82%D0%B0%D0%BA-%D0%B4%D0%B5%D0%BB%D0%B0%D1%82%D1%8C-%D0%B7%D0%B0%D0%BF%D0%B8%D1%81%D1%8C)

- sudo crontab -u jenit -e
    
- 00 09 16 \* \* echo ‘date’ > /home/jenit/Desktop/date.txt
    

*(Расшифровка: в ноль минут,в девять часов,шестнадцатого числа,каждый месяц отпралять сообщение со словом data на рабочий стол,в файл data.txt.)*

00 - минуты

09 - часы

16 - дата,(день месяца)

\* - месяц

\* - день недели

#### Мой crontab -e[](#%D0%BC%D0%BE%D0%B9-crontab--e)

- 00 09 16 \* \* echo ‘date’ > /home/jenit/‘Рабочий стол’/date.txt
    
- 00 08 05 03 \* echo ‘Birthday’ > /home/jenit/‘Рабочий стол’/date-Sl.txt
    

*Посмотрите в свое файловом менеджере как у Вас пишется “Рабочий стол” или “Desktop”*

<img width="895" height="449" src="../_resources/cron1_fb05a71a7dcd452dbe90dec1b277a802.jpg" class="jop-noMdConv"> <img width="895" height="449" src="../_resources/cron2_957705230b3e449bb5a09da66fc6a1b7.jpg" class="jop-noMdConv">

> Enter - сохранить

#### Если нужно по расписанию проиграть звуковой файл.[](#%D0%B5%D1%81%D0%BB%D0%B8-%D0%BD%D1%83%D0%B6%D0%BD%D0%BE-%D0%BF%D0%BE-%D1%80%D0%B0%D1%81%D0%BF%D0%B8%D1%81%D0%B0%D0%BD%D0%B8%D1%8E-%D0%BF%D1%80%D0%BE%D0%B8%D0%B3%D1%80%D0%B0%D1%82%D1%8C-%D0%B7%D0%B2%D1%83%D0%BA%D0%BE%D0%B2%D0%BE%D0%B9-%D1%84%D0%B0%D0%B9%D0%BB.)

- 00 08 \* \* \* /usr/bin/mpg123 /home/jenit/Music/mahnem.mp3

*У меня почему то корректно работает только с mpg123(консольный аудио проигрыватель)*