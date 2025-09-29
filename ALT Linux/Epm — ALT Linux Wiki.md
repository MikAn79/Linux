#altlinux

[Перейти к содержанию](#content)

Переключить боковую панель [**ALT Linux Wiki**](https://www.altlinux.org/%D0%93%D0%BB%D0%B0%D0%B2%D0%BD%D0%B0%D1%8F_%D1%81%D1%82%D1%80%D0%B0%D0%BD%D0%B8%D1%86%D0%B0)

Персональные инструменты collapsed

- [Статья](https://www.altlinux.org/Epm "Просмотр основной страницы [alt-shift-c]")
- [Обсуждение](https://www.altlinux.org/%D0%9E%D0%B1%D1%81%D1%83%D0%B6%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5:Epm "Обсуждение основной страницы [alt-shift-t]")

- [Читать](https://www.altlinux.org/Epm)
- [Просмотр кода](/index.php?title=Epm&action=edit "Эта страница защищена от изменений.
    Вы можете посмотреть её исходный текст. [alt-shift-e]")
- [История](https://www.altlinux.org/index.php?title=Epm&action=history "Журнал изменений страницы [alt-shift-h]")

<a id="top"></a>

# Epm

epm - единая команда управления пакетами, разработанная в компании Etersoft<sup>[\[1\]](#cite_note-1)</sup>. Основное предназначение: унифицировать управление пакетами в дистрибутивах с разными пакетными менеджерами. Кроме того, сейчас в epm заскриптованы некоторые типовые операции, которые, например, в случае использования apt в ALT, потребовали бы ввода более одной команды.

Установка в ALT Linux:

```
apt-get install eepm

```

Описание на сайте разработчика: https://wiki.etersoft.ru/Epm

Пополнения рецептов принимаются по pull request в http://github.com/Etersoft/eepm

## Содержание

- [1 Команды](#Команды)
    - [1.1 Пример использования](#Пример_использования)
    - [1.2 Пример установки Яндекс Браузера](#Пример_установки_Яндекс_Браузера)
- [2 Примечания](#Примечания)

# 

<a id="&Kcy;&ocy;&mcy;&acy;&ncy;&dcy;&ycy;"></a>Команды

| Описание операции | Команда epm | Альтернативная команда epm | Команда ALT Linux |
| --- | --- | --- | --- |
| Установка пакета по названию в систему | epm -i (package) | epm install (package) или epmi (package) | apt-get install (package) |
| Установка пакета с конвертацией |     | epm install --repack (package) |     |
| Установка файла пакета в систему | epm -i (package file) | epm install (package file) или epmi (package file) | apt-get install (package file) |
| Удаление пакета из системы | epm -e (package) | epm remove (package) или epme (package) | apt-get remove (package) |
| Поиск пакета в репозитории | epm -s (text) | epm search (text) или epms (text) | apt-cache search (text) |
| Проверка наличия пакета в системе | epm -q (package) | epm installed (package) или epmq (package) | rpm -qa (pipe) grep (package) |
| Список установленных пакетов | epm -qa | epm packages или epm list или epmqa | rpm -qa |
| Поиск по названиям установленных пакетов | epm -qp &lt;word&gt; | epmqp | grep &lt;word&gt; |
| Принадлежность файла к (установленному) пакету | epm -qf (file) | epmqf (file) | rpm -qf (file) или rpmqf из etersoft-build-utils |
| Поиск, в каком пакете есть указанный файл | epm -sf &lt;file&gt; | epm filesearch | apf search &lt;file&gt; |
| Список файлов в (установленном) пакете | epm -ql (package) | epm filelist &lt;package&gt; | rpm -ql (package) |
| Вывести информацию о пакете | epm -qi (package) | epm info (package) | apt-cache show (package) |
| Обновить дистрибутив | epm upgrade | epm dist-upgrade | apt-get dist-upgrade |
| Обновить систему и ядро | epm full-upgrade |     | apt-get dist-upgrade && update-kernel |
| Добавить i586-пакеты в систему | epm play i586-fix |     | См. [Biarch](https://www.altlinux.org/Biarch "Biarch") |
| Показать доступные к установке пакеты | epm play |     | GUI в Р10 и выше - appinstall[\[1\]](http://packages.altlinux.org/en/Sisyphus/srpms/appinstall) |
| Обновить epm с сервера Etersoft | epm ei |     |     |

## 

<a id="&Pcy;&rcy;&icy;&mcy;&iecy;&rcy;_&icy;&scy;&pcy;&ocy;&lcy;&softcy;&zcy;&ocy;&vcy;&acy;&ncy;&icy;&yacy;"></a>Пример использования

Конкретный случай :

```


epm play sublime
 # bash /etc/eepm/play.d/sublime.sh --run
 # /usr/bin/wget -q -O- https://www.sublimetext.com/download
FATAL: Can't get package URL

```

Должно было скачать и установить пакет sublime, но что-то пошло не так.

Идём на сайт https://www.sublimetext.com/, поменялся URL скачивания пакета, или ещё что-то, но wget не скачивает.

Скачиваем пакет с странички скачивания : https://www.sublimetext.com/download . смотрим в скрипте /etc/eepm/play.d/sublime.sh, что должно было скачаться - файл с tar.xz Скачиваем ( прямая ссылка https://www.sublimetext.com/download_thanks?target=x64-tar)

Потом делаем :

```
epm repack /...путь_до.../sublime_text_build_4126_x64.tar.xz 

```

И устанавливаем перепакованный пакет:

```
epmi  /..путь../sublime_text_build-4126-alt1.repacked.with.epm.2.x86_64.rpm

```

## 

<a id="&Pcy;&rcy;&icy;&mcy;&iecy;&rcy;_&ucy;&scy;&tcy;&acy;&ncy;&ocy;&vcy;&kcy;&icy;_&YAcy;&ncy;&dcy;&iecy;&kcy;&scy;_&Bcy;&rcy;&acy;&ucy;&zcy;&iecy;&rcy;&acy;"></a>Пример установки Яндекс Браузера

[Установка Яндекс Браузера с помощью epm](https://www.altlinux.org/%D0%AF%D0%BD%D0%B4%D0%B5%D0%BA%D1%81_%D0%91%D1%80%D0%B0%D1%83%D0%B7%D0%B5%D1%80 "Яндекс Браузер")

# 

<a id="&Pcy;&rcy;&icy;&mcy;&iecy;&chcy;&acy;&ncy;&icy;&yacy;"></a>Примечания

1.  [↑](#cite_ref-1 "Обратно к тексту") [Etersoft выпустил универсальное средство управления пакетами EPM](https://etersoft.ru/about/news/418-epm)

Программное обеспечение

[Alternatives](https://www.altlinux.org/Alternatives "Alternatives") • [Appimage](https://www.altlinux.org/Appimage "Appimage") • [Autoinstall](https://www.altlinux.org/Autoinstall "Autoinstall") • Epm • [Folding@Home](https://www.altlinux.org/Folding@Home "Folding@Home") • [RPM-repair](https://www.altlinux.org/RPM-repair "RPM-repair") • [SoftwareCenter](https://www.altlinux.org/SoftwareCenter "SoftwareCenter") • [Synaptic](https://www.altlinux.org/Synaptic "Synaptic") • [Vim учебник](https://www.altlinux.org/Vim_%D1%83%D1%87%D0%B5%D0%B1%D0%BD%D0%B8%D0%BA "Vim учебник") • [Vim-подключение плагинов](https://www.altlinux.org/Vim-%D0%BF%D0%BE%D0%B4%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%B8%D0%B5_%D0%BF%D0%BB%D0%B0%D0%B3%D0%B8%D0%BD%D0%BE%D0%B2 "Vim-подключение плагинов") • [Восстановление поврежденной RPM-базы](https://www.altlinux.org/%D0%92%D0%BE%D1%81%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5_%D0%BF%D0%BE%D0%B2%D1%80%D0%B5%D0%B6%D0%B4%D0%B5%D0%BD%D0%BD%D0%BE%D0%B9_RPM-%D0%B1%D0%B0%D0%B7%D1%8B "Восстановление поврежденной RPM-базы") • [Где и как искать программы](https://www.altlinux.org/%D0%93%D0%B4%D0%B5_%D0%B8_%D0%BA%D0%B0%D0%BA_%D0%B8%D1%81%D0%BA%D0%B0%D1%82%D1%8C_%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D1%8B "Где и как искать программы") • [Замена](https://www.altlinux.org/%D0%97%D0%B0%D0%BC%D0%B5%D0%BD%D0%B0 "Замена") • [Карманы](https://www.altlinux.org/%D0%9A%D0%B0%D1%80%D0%BC%D0%B0%D0%BD%D1%8B "Карманы") • [Неактуальные уязвимости](https://www.altlinux.org/%D0%9D%D0%B5%D0%B0%D0%BA%D1%82%D1%83%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D0%B5_%D1%83%D1%8F%D0%B7%D0%B2%D0%B8%D0%BC%D0%BE%D1%81%D1%82%D0%B8 "Неактуальные уязвимости") • [Управление пакетами](https://www.altlinux.org/%D0%A3%D0%BF%D1%80%D0%B0%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5_%D0%BF%D0%B0%D0%BA%D0%B5%D1%82%D0%B0%D0%BC%D0%B8 "Управление пакетами")

[Категории](https://www.altlinux.org/%D0%A1%D0%BB%D1%83%D0%B6%D0%B5%D0%B1%D0%BD%D0%B0%D1%8F:%D0%9A%D0%B0%D1%82%D0%B5%D0%B3%D0%BE%D1%80%D0%B8%D0%B8 "Служебная:Категории"):

- [Пользователю](https://www.altlinux.org/%D0%9A%D0%B0%D1%82%D0%B5%D0%B3%D0%BE%D1%80%D0%B8%D1%8F:%D0%9F%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D0%B5%D0%BB%D1%8E "Категория:Пользователю")
- [Программное обеспечение](https://www.altlinux.org/%D0%9A%D0%B0%D1%82%D0%B5%D0%B3%D0%BE%D1%80%D0%B8%D1%8F:%D0%9F%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%BD%D0%BE%D0%B5_%D0%BE%D0%B1%D0%B5%D1%81%D0%BF%D0%B5%D1%87%D0%B5%D0%BD%D0%B8%D0%B5 "Категория:Программное обеспечение")

- Содержание доступно по лицензии [CC-BY-SA-3.0](https://www.altlinux.org/ALT_Linux_Wiki:Copyright "ALT Linux Wiki:Copyright") (если не указано иное).

- [Политика конфиденциальности](https://www.altlinux.org/ALT_Linux_Wiki:%D0%9F%D0%BE%D0%BB%D0%B8%D1%82%D0%B8%D0%BA%D0%B0_%D0%BA%D0%BE%D0%BD%D1%84%D0%B8%D0%B4%D0%B5%D0%BD%D1%86%D0%B8%D0%B0%D0%BB%D1%8C%D0%BD%D0%BE%D1%81%D1%82%D0%B8 "ALT Linux Wiki:Политика конфиденциальности")
- [О ALT Linux Wiki](https://www.altlinux.org/ALT_Linux_Wiki:%D0%9E%D0%BF%D0%B8%D1%81%D0%B0%D0%BD%D0%B8%D0%B5 "ALT Linux Wiki:Описание")
- [Отказ от ответственности](https://www.altlinux.org/ALT_Linux_Wiki:%D0%9E%D1%82%D0%BA%D0%B0%D0%B7_%D0%BE%D1%82_%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D1%81%D1%82%D0%B2%D0%B5%D0%BD%D0%BD%D0%BE%D1%81%D1%82%D0%B8 "ALT Linux Wiki:Отказ от ответственности")

- ![CC-BY-SA-3.0](../../_resources/cc-by-sa_86d31ddf1f6d41eaa01bb16721e74192.png)
- [![Powered by MediaWiki](https://www.altlinux.org/resources/assets/poweredby_mediawiki_88x31.png)](https://www.mediawiki.org/)