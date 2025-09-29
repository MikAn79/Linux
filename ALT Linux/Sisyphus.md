#altlinux

## Настройка sudo

`su -`

`control sudowheel enabled`

## Переход на Sisyphus

## список команд

0 - входим под рутом (если ***sudo*** настроено, то данный шаг не обязателен)  
***su*** - если настроено sudo, то дальнейшие команды выполняем через sudo - перед командами дописываем sudo

1 - обновляем источники пакетов `apt-get update`

2 - обновлем пакеты до последних версий в текущем репозитории `apt-get dist-upgrade`

3 - переключаем ветку ядра на std-def если используется un-def   `update-kernel -t std-def`

4 - проверяем, установлен ли пакет ***apt repo*** `y`

выполняем отключение репозиториев стабильной платформы `apt-repo rm all`

5 - подключаем репозиторий сизиф `apt-repo set sisyphus`

6 - редактируем файл ==***/etc/rpm/macros***==  
 6.1 - открываем его `mcedit /etc/rpm/macros`  
 6.2 дописываем туда следующее с помощью комбинации ctrl + shift + v    ==%\_priority_distbranch sisyphus==  
 6.3 сохраняем - f2 и выходим f10

7 - очищаем кеш апт `apt-get clean`

8 - обновляем источники пакетов - чтоб подгрузилась информация о свежих пакетах из сизифа `apt-get update`

9 - обновляем систему из сизифа  
   9,1 - загружаем новые пакеты `apt-get dist-upgrade -d`  
   9.2 - устанавливаем эти самые свежие пакеты `apt-get dist-upgrade`

10 - снова обновляем ядро, теперь уже из сизифа `update-kernel -t std-def`

11 - перезагружаем систему и получаем ролинг релиз на базе сизифа с всеми свежайшими пакетами, которые только могут быть в альт линукс

12 - до полного счастья обновляем ядро с стабильного до наисвежайшей версии `update-kernel -t un-def`

13 - перезагружаемся и наслаждаемся освежённой ситемой

> сайт, где смотреть какие пакеты находятся в том или ином репозитории [https://packages.altlinux.org/ru/sisy...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa0VGYUVsdDZ3dUJNb1VJcGlGRUhSOVNRVnJ6d3xBQ3Jtc0tsbFhjT3JnOHIydzc2YV9pSXJfSm1QdGJUSjN2UDE3em1uQktldXpFbTZMVGpVOEF1VGFuNlZMR0FqZFdlVHZFdndYSk9Lend1NGQ2cERwVnNWcm85NURReFFaVUtKaG80ZmhJR3E2ZExrYXlDWk9pYw&q=https%3A%2F%2Fpackages.altlinux.org%2Fru%2Fsisyphus%2F&v=vUhTQ-ai1UY)
> 
> список изменений в сизифе [http://nightly.altlinux.org/sisyphus/...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbUlHb1JPenJ6V3Y1bkZzbEtnV09CS0lNYWJJUXxBQ3Jtc0ttbExYNUxSbzZERUhrRzVocEwydHNhT2txZnBmbTl1Y3lMa0M2VmFONnBwbk4wUTFhUTJJaWthOFBKY2Flc1REcWQ0NzU2SDVjQS1BUGx6dWdYSGlpWVM0NXN1Tk5FV0FGM2RrNlkxMTBnby1NcmE0QQ&q=http%3A%2F%2Fnightly.altlinux.org%2Fsisyphus%2FChangeLog&v=vUhTQ-ai1UY)
> 
> скачать реуглярные сборки последние [http://nightly.altlinux.org/sisyphus/...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbWV1dWtMZDA4SVU1cmtaT1FqMnNZYjVxbDc2d3xBQ3Jtc0tudDBjX2M1NFNfb1BhZ0tFa2JwZU1RQzk2NWZ3ckJFX1hqSFo1YzdFdno4YkZYMVk1UUVwTEF5Tzd5TkpSWkZILTNLTlVLeTdZOW5fZmVfVDN5RnBMTE81TFMyaDBLV0diaFlzRnkyd09xeHp0bkhwcw&q=http%3A%2F%2Fnightly.altlinux.org%2Fsisyphus%2Fcurrent%2F&v=vUhTQ-ai1UY)
> 
> тестированные [http://nightly.altlinux.org/sisyphus/...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa2xJTnp1S3R4U2FwRlZoYzNjdzQ5MDlTZjJkUXxBQ3Jtc0ttTkcyYnBDdS1XQUVKWmd6ZGFNNWhPS28tRFB1WnQzYnlIV1F2R29FUEl3Z1VSNlhxZnluUWRqMEc0VVNMSFhmMXVDNldrTFNySjk1eUJaNVNKeUZHM2o4VUNadTlXUTNjZXR0TUp0SENsd0U2MEktRQ&q=http%3A%2F%2Fnightly.altlinux.org%2Fsisyphus%2Ftested%2F&v=vUhTQ-ai1UY)