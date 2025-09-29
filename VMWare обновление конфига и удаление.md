## VMWare обновление конфига после обновы ядра

`sudo vmware-modconfig --console --install-all`

## VMWare удаление

Чтобы проверить какие продукты VMware  у нас установленны, вводим в консоли:

`vmware-installer -l`

[![](https://xaxatyxa.ru/wp-content/uploads/2012/03/vmware_unistal_1.png "vmware_unistal_1")](https://xaxatyxa.ru/wp-content/uploads/2012/03/vmware_unistal_1.png)

Удаляем

`sudo vmware-installer -u vmware-player`

На всякий случай сохраняем конфигурационные файлы, выбираем «Yes»

[![](https://xaxatyxa.ru/wp-content/uploads/2012/03/vmware_unistal_2.png "vmware_unistal_2")](https://xaxatyxa.ru/wp-content/uploads/2012/03/vmware_unistal_2.png)

Ждём пока всё удалиться

[![](https://xaxatyxa.ru/wp-content/uploads/2012/03/vmware_unistal_3.png "vmware_unistal_3")](https://xaxatyxa.ru/wp-content/uploads/2012/03/vmware_unistal_3.png)

[![](https://xaxatyxa.ru/wp-content/uploads/2012/03/vmware_unistal_5.png "vmware_unistal_5")](https://xaxatyxa.ru/wp-content/uploads/2012/03/vmware_unistal_5.png)

Вот собственно и всё.