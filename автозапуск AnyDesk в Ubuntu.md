Я нашел следующий способ отключить службу AnyDesk. Таким образом, вы можете запустить его вручную.

`systemctl disable anydesk.service`

Вы также можете проверить его сервисный статус:

`systemctl status anydesk`

NB: вы можете сначала остановить его (или после), если он запущен:

`service anydesk stop`