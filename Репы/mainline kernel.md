1.) Чтобы добавить PPA, откройте терминал, и выполните команду:

`
sudo add-apt-repository ppa:cappelikan/ppa
`

2.) Затем проверьте обновления и установите инструмент с помощью команд:

`
sudo apt update
`
`
sudo apt install mainline
`

### Как удалить Mainline:

Чтобы удалить PPA, выполните команду:

`
sudo add-apt-repository --remove ppa:cappelikan/ppa
`

Чтобы удалить установщик ядра Ubuntu Mainline, выполните команду:

`
sudo apt remove mainline
`