---
page-title: Пакетный менеджер (ПМ) Arch Linux - Справочная система PuppyRus Linux
url: https://wiki.puppyrus.org/setups/pacman
date: 2025-05-19 14:21:22
tags:
  - pacman
---
### **−**Содержание

## Пакетный менеджер (ПМ) Arch Linux

## Rolling\_release

-   Arch Linux является непрерывно обновляемым дистрибутивом и не имеет фиксированной версии: [Rolling\_release](http://ru.wikipedia.org/wiki/Rolling_release "http://ru.wikipedia.org/wiki/Rolling_release").
    

## Arch Rollback Machine ("заморозка репозитория")

-   Имеется возможность «заморозить» состояние репозитария Arch Linux на определенную дату (см. [Arch\_Rollback\_Machine](https://wiki.archlinux.org/index.php/Arch_Rollback_Machine "https://wiki.archlinux.org/index.php/Arch_Rollback_Machine")), т.е. зафиксировать состояние репозитария на момент сборки базовых модулей
    
-   Это очень удобно для FRUGAL установки : обновления не накапливаются в сохраненке
    

## pacman

## Шпаргалка по ключам

### \-S синхронизация (--sync)

-   pacman -S pkg1 pkg2 устанавливает или обновляет пакеты вместе с их зависимостями
    
-   pacman -S repo/pkg устанавливает пакет из указанного репозитория (когда пакет имеет несколько версий в разных репозиториях, например, extra и testing)
    
-   pacman -S «pkg>=version» устанавливает пакет требуемой версии
    

-   pacman -Sw pkg скачивает пакет, но не устанавливает его
    
-   pacman -Sp pkg устанавливает пакет и выводит для него ссылку на скачивание вместе с зависимостями
    
-   pacman -Sf pkg устанавливает пакет, пропуская проверку конфликтов
    
-   pacman -Sd pkg устанавливает пакет, пропуская проверку зависимостей
    

-   pacman -Syu обновляет все пакеты системы (предварительно синхронизировав базы данных репозиториев)
    
-   pacman -Su обновляет все устаревшие пакеты (предпочтительнее предыдущая команда)
    
-   pacman -Suu обновляет пакеты с возможностью даунгрейда (если были, например, отключены репозитории testing и требуется откатиться на более старые версии)
    

-   pacman -Ss name ищет пакеты в базе данных по имени и описанию
    
-   pacman -Ssq pkg выводит в результатах поиска только имена пакетов
    
-   pacman -Si pkg показывает информацию о пакете
    
-   pacman -Sg group показывает пакеты, входящие в указанную группу
    
-   pacman -Sl repo показывает все пакеты из репозитория
    

-   pacman -Sc удаляет из кэша пакеты, которые уже были удалены (кэш хранится в /var/cache/pacman/pkg/)
    
-   pacman -Scc полная очистка кэша пакетов
    

### \-R Удаление (--remove)

-   pacman -R pkg удаляет пакет, оставляя зависимости в системе
    
-   pacman -Rs pkg удаляет пакет вместе с зависимостями, если они не используются другими пакетами
    
-   pacman -Rn pkg удаляет пакет и резервные копии его конфигурационных файлов (по-умолчаню, они сохраняются с добавлением расширения \*.pacsave при удалении приложений)
    

### \-Q запрос (--query)

-   pacman -Q - установленные пакеты с версией (-Qq - без версии)
    
-   pacman -Qs name ищет пакеты среди установленных
    
-   pacman -Qi pkg показывает информацию об установленном пакете
    
-   pacman -Ql pkg показывает список файлов установленного пакета
    
-   pacman -Qc pkg показывает список изменений пакета (если пакет его имеет)
    
-   pacman -Qg group показывает все пакеты из группы
    
-   pacman -Qo /path/to/file показывает какой пакет является владельцем указанного файла
    

-   pacman -Qet отображение списка явно установленных, но не требующихся другим пакетам, пакетов
    
-   pacman -Qdt перечисляет все пакеты, больше не требуемые как зависимости
    
-   pacman -Qt - -Qet и -Qdt
    
-   pacman -Qu выводит список устаревших пакетов
    
-   pacman -Qk pkg проверяет, все ли файлы, принадлежащие данному пакету присутствуют в системе
    

-   pacman -U path/to/pkg.tar.gz устанавливает локальный пакет (или из интернета, если как путь будет прописана интернет-ссылка)
    
-   pacman -T pkg выводит список зависимостей, которые не удовлетворены в системе для указанного пакета
    

### Обновление базы пакетов

-   Для принудительного обновления списка пакетов : *pacman -Syy*
    

### Поиск

-   Найти все зависимости локального пакета: *pacman -U –print-format »%n %v»*
    
-   Найти пакет : *pacman -Ss имя\_или\_описание\_пакета*
    
-   Все пакеты репозитория : *pacman -Sl имя\_репозитория*. -Slq - без версий
    
-   Доступные обновления: *pacman -Qu*
    
-   Показать все пакеты, не используемые ни одним пакетом: *pacman -Qt*
    
-   Установленные пакеты: *pacman -Qu*. -Qq - без версий
    
-   [Найти](https://archlinux.org/pacman/pacman.8.html#_file_options_apply_to_em_f_em_a_id_fo_a "https://archlinux.org/pacman/pacman.8.html#_file_options_apply_to_em_f_em_a_id_fo_a") пакет по имени файла : *pacman -F file*
    
-   Найти пакеты установленных из репозитория имя\_репозитория : *comm -12 <(pacman -Qq | sort) <(pacman -Sql имя\_репозитория | sort)*
    
    -   с версией пакета: *pacman -Sl имя\_репозитория |grep «\\\[»*
        

### Установка, удаление

-   Устанавливать пакет с заменой файлов системы: *pacman –force -S имя\_пакета* . В свежих версиях : *pacman -S имя\_пакета –overwrite «\*»*
    
-   Устанавливать пакет из указанного репозитория: *pacman -S репозиторий/имя\_пакета*
    
-   Скачать пакет, но не устанавливать его: *pacman -Sw имя\_пакета*
    
-   Установить локальный пакет : *pacman -U путь/имя\_пакета*
    
-   Удалить пакет со всеми зависимостями : *pacman -Rs имя\_пакета*
    
-   Переустановить все пакеты из arch репозитория: *pacman -S $(pacman -Qq | grep -v «$(pacman -Qmq)»)*
    

Для получения списка файлов неустановленного пакета можно использовать утилиту pkgfile из состава пакета pkgtools или [найти](https://archlinux.org/pacman/pacman.8.html#_file_options_apply_to_em_f_em_a_id_fo_a "https://archlinux.org/pacman/pacman.8.html#_file_options_apply_to_em_f_em_a_id_fo_a") пакет по имени файла : *pacman -F file*

### Справочная информация

-   pacman -V показывает версию pacman
    
-   pacman -h показывает синтаксис команды (если добавить опцию, то синтаксис для заданной опции, например pacman -Qh)
    
-   man pacman полный ман по командам pacman
    
-   man pacman.conf полный ман по файлу настроек pacman
    

## Создание пакета

-   [AUR](https://wiki.archlinux.org/index.php/Arch_User_Repository_%28%D0%A0%D1%83%D1%81%D1%81%D0%BA%D0%B8%D0%B9%29 "https://wiki.archlinux.org/index.php/Arch_User_Repository_%28%D0%A0%D1%83%D1%81%D1%81%D0%BA%D0%B8%D0%B9%29"): packer часть\_названия\_пакета
    
-   [ABS](https://wiki.archlinux.org/index.php/Arch_Build_System#How_to_use_ABS "https://wiki.archlinux.org/index.php/Arch_Build_System#How_to_use_ABS") : asp export пакет
    

## packer

[Описание](https://github.com/keenerd/packer/wiki "https://github.com/keenerd/packer/wiki")

В отличие от pacman работает еще и с AUR. Типовое использование:

1.  В LF дистрибутивах подключить модуль DEVX\*.pfs. В arch , manjaro : sudo pacman -Sy base-devel
    
2.  *packer имя\_или\_описание\_пакета* - найти и установить
    
3.  *packer -G имя\_пакета* - скачать и распаковать tarball с PKGBUILD.
    
4.  [makepkg](https://wiki.archlinux.org/index.php/Makepkg_%28%D0%A0%D1%83%D1%81%D1%81%D0%BA%D0%B8%D0%B9%29 "https://wiki.archlinux.org/index.php/Makepkg_%28%D0%A0%D1%83%D1%81%D1%81%D0%BA%D0%B8%D0%B9%29") (запускать не от root)
    

## pkgfile