
# Разбираемся с UEFI и GPT: установка Windows и Kubuntu на один диск


Помните те времена, когда BIOS был 16-битным с адресным пространством в 1 Мб, а вся информация о загрузчиках писалась в MBR? На смену уже давно пришли более гибкие технологии: UEFI (замена BIOS), и GPT (замена MBR).
Предыстория: Понадобилось мне недавно на свой домашний десктоп поставить 2 системы, чтобы разграничить окружение. Kubuntu для разработки на Ruby on Rails (ибо работаю удаленно), и Windows для всяких игрушек в свободное время. Хочу заметить, что несколько лет назад это было достаточно просто: один раздел для винды и один раздел для линукса, загрузчик записывался в MBR. Однако, технологии не стоят на месте, и оказалось, что настройка dual boot'а теперь несколько изменилась.
<a id="habracut"></a>
Итак, начнем.

#### Терминология

**[UEFI](https://ru.wikipedia.org/wiki/Extensible_Firmware_Interface)** (Unified Extensible Firmware Interface, Единый расширяемый интерфейс прошивки) разрабатывался компанией Intel как замена BIOS (Basic Input Output System). В отличие от 16-битного BIOS'а UEFI работает в 32- или 64-битном режиме, что позволяет использовать намного больше памяти для сложных процессов. Кроме того, UEFI приятно выглядит и там есть поддержка мышки.

**Внешний вид:**

**[GPT](https://ru.wikipedia.org/wiki/%D0%A2%D0%B0%D0%B1%D0%BB%D0%B8%D1%86%D0%B0_%D1%80%D0%B0%D0%B7%D0%B4%D0%B5%D0%BB%D0%BE%D0%B2_GUID)** (GUID Partition Table, Таблица разделов GUID) — часть спецификации UEFI. UEFI использует GPT так же как BIOS использует [MBR](https://ru.wikipedia.org/wiki/%D0%93%D0%BB%D0%B0%D0%B2%D0%BD%D0%B0%D1%8F_%D0%B7%D0%B0%D0%B3%D1%80%D1%83%D0%B7%D0%BE%D1%87%D0%BD%D0%B0%D1%8F_%D0%B7%D0%B0%D0%BF%D0%B8%D1%81%D1%8C).
Главным отличием GPT от MBR, на мой взгляд, являются:

- *Количество разделов*: MBR поддерживает только 4 раздела. Можно и больше, но только через extended partition, что является просто хаком ограничений. GPT поддерживает до 128 разделов.
- *Размер диска*: MBR поддерживает диски до 2Тб, в то время как GPT — до 9.4 Зеттабайт (=9.4 × 10^21 байт, или условно 1000 Тб)
- ***Порядок загрузки***: раньше BIOS загружал MBR, и в нем содержались адреса загрузчиков для каждого раздела диска. Теперь UEFI считывает GPT, находит в таблице все разделы типа efi (на них содержатся загрузчики), и подгружает их в память. Разберем это на примере немного позже.

#### Что делаем:

Устанавливаем следующие ОС на пустой HDD размером в 1 Тб.

- **Windows 8.1 x64**. Windows поддерживает загрузку с GPT начиная с Windows 8 для 32 битной архитектуры и с Windows Server 2003 и Windows Vista для 64 бит ([Источник](https://en.wikipedia.org/wiki/GUID_Partition_Table#Windows:_32-bit_versions)).
- **Kubuntu 15.04**. По идее подойдет любой дистрибутив, который поддерживает Grub2, лично я предпочитаю Kubuntu.

NB: Материнская плата поддерживает UEFI

#### Разбивка диска

Сначала устанавливаем Windows 8, т.к. она автоматически будет использовать GPT.
Разбивка будет выглядеть так (пардон за кривой снимок):
<img width="740" height="429" src="../_resources/78b12bab2ade4ef99a6d151fd2eaaf1c_8c145e4742214189b.jpg"/>
Винда по умолчанию создает 4 раздела:

1.  Recovery (300Мб). Очевидно, что он используется для восстановления системы. Оставим как есть.
2.  EFI partition (100Мб). Помечается как system type (не любят в Майкрософте называть вещи своими техническими именами). Собственно сюда и пишутся загрузчики.
3.  MSR (128Мб, [Microsoft Reserved Partition](http://en.wikipedia.org/wiki/Microsoft_Reserved_Partition)). Для меня остается загадкой, зачем он нужен. Данных там никаких нет, просто пустое место, зарезервированное для каких-то непонятных целей в будущем.
4.  Основной раздел. Мы его поделим на 3: 200 гигов под винду, 500 гигов для раздела под данные и остальное пространство пока оставим неразмеченным (отформатируем потом при установке Kubuntu).

Пропустим саму установку Windows, т.к. в ней все стандартно и понятно.
Теперь загрузимся с USB в Kubuntu Live.
Проверим EFI раздел:

```
kubuntu@kubuntu:~$ efibootmgr
BootCurrent: 0003
Timeout: 0 seconds
BootOrder: 0000,0003,0001
Boot0000* Windows Boot Manager
Boot0001* Hard Drive
Boot0003* UEFI: JetFlashTranscend 16GB 
```

Boot0000 — виндовый загрузчик
Boot0001 — дефолтный загрузчик
Boot0003 — флешка с Kubuntu Live
Обратите внимание, что список загрузчиков не привязан к одному физическому диску как в MBR. Он хранится в NVRAM.
Можем также сразу посмотреть, что же в этом разделе, подмонтировав его:

```
kubuntu@kubuntu:~$ sudo mkdir /media/efi
kubuntu@kubuntu:~$ sudo mount /dev/sda2 /media/efi 
```

Там окажутся следующие файлы:

```
EFI
|--Boot
|    |--bootx64.efi # дефолтный загрузчик
|--Microsoft
     |--Boot
         |--bootmgfw.efi # основной виндовый загрузчик
         |--# много других файлов 
```

Убедились, что все хорошо. Теперь продолжаем разбивку диска (через KDE Partition Manager).
<img width="740" height="511" src="../_resources/d2ebd0048d1c4c65953a7df2c48f1038_fd5262fa1585494fb.png"/>
Первые пять разделов остались прежними. Обратите внимание, как Kubuntu определила разделы:

- sda2 определился как FAT32. Это практически верно, т.к. файловая система типа EFI основана на FAT, только с жесткими спецификациями.
- sda3 (MSR) не определился, т.к. файловой системы там так таковой нет.

Нам осталось только отформатировать раздел для Kubuntu в ext4, и выделить раздел под swap.
Несколько слов про swap. [Рекомендуют](https://help.ubuntu.com/community/SwapFaq#How_much_swap_do_I_need.3F) на swap выделять от SQRT(RAM) до 2xRAM. Т.к. у меня 16 Гб RAM, то по минимуму мне надо 4 Гб свопа. Хотя я с трудом могу представить ситуации, при которых он будет использоваться: десктоп в hibernate я не перевожу, и сильно тяжелых программ, которые жрут больше 16 гигов, не использую.
P.S. При форматировании раздела в swap Partition Manager может выдать ошибки, которые связаны с тем, что Kubuntu автоматически монтирует в себя любой swap раздел, однако на результат эти ошибки не влияют.
Итак, финальная разбивка:
<img width="740" height="508" src="../_resources/3fc158f866ba40b98675165f5e045d54_6df61f6c3e5f469ea.png"/>
Теперь самое главное для правильного dual boot'а. При установке Kubuntu важно выбрать, куда установить загрузчик:
<img width="740" height="571" src="../_resources/aba6b7141f2149b2bb5edb76916fe1fd_d3e2ab1bc68540df8.png"/>
Указываем, конечно же на раздел EFI.
После завершения установки Kubuntu, заходим в систему и проверяем, какие файлы появились на efi разделе (монтировать уже не нужно):

```
user@kubuntu:~$ sudo ls /boot/efi/EFI
Boot  Microsoft  ubuntu
user@kubuntu:~$ sudo ls /boot/efi/EFI/ubuntu
grub.cfg  grubx64.efi  MokManager.efi  shimx64.efi 
```

Смотрим, как теперь выглядит список загрузчиков:

```
user@kubuntu:~$ efibootmgr -v
BootCurrent: 0002
Timeout: 0 seconds
BootOrder: 0002,0000,0003,0001
Boot0000* Windows Boot Manager  HD(2,96800,32000,c4f37e07-0441-4967-a1ac-75fb5a36e4f3)File(\EFI\Microsoft\Boot\bootmgfw.efi)
Boot0001* Hard Drive    BIOS(2,0,00)
Boot0002* ubuntu        HD(2,96800,32000,c4f37e07-0441-4967-a1ac-75fb5a36e4f3)File(\EFI\ubuntu\shimx64.efi)
Boot0003* ubuntu        HD(2,96800,32000,c4f37e07-0441-4967-a1ac-75fb5a36e4f3)File(EFI\Ubuntu\grubx64.efi) 
```

Вот как это выглядит при загрузке:
<img width="740" height="142" src="../_resources/8612f84898fe4546bdd96e2d1b2ad3ac_a5be2f11ea784d41b.jpg"/>
А еще эти загрузчики доступны сразу из UEFI (в старом BIOS'е такое было бы невозможно — там был выбор только диска, он просто не знал, что такое загрузчики):
<img width="740" height="555" src="../_resources/871c898db59b419ab04b80a31b6d8957_b8060d06ca7b41be9.jpg"/>
Ну и напоследок: чтобы dual boot правильно работал, в Windows надо обязательно отключить fast boot. Это такая нехорошая фича, которая может привести к потере данных.

**Объяснение:**

Теги:

- [dual boot](https://habr.com/ru/search/?target_type=posts&order=relevance&q=%5Bdual%20boot%5D)
- [установка windows 8](https://habr.com/ru/search/?target_type=posts&order=relevance&q=%5B%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0%20windows%208%5D)
- [установка linux](https://habr.com/ru/search/?target_type=posts&order=relevance&q=%5B%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0%20linux%5D)
- [uefi](https://habr.com/ru/search/?target_type=posts&order=relevance&q=%5Buefi%5D)

Хабы:

- [Настройка Linux](https://habr.com/ru/hub/linux/)
- [Системное администрирование](https://habr.com/ru/hub/sys_admin/)
- [UEFI](https://habr.com/ru/hub/UEFI/)

Всего голосов 26: ↑23 и ↓3 +20

[Комментарии 42](https://habr.com/ru/articles/259283/comments/)

Закрыть

### Редакторский дайджест

Присылаем лучшие статьи раз в месяц

[](https://habr.com/ru/users/Extrapolator/)

9

Карма

0

Рейтинг

[@Extrapolator](https://habr.com/ru/users/Extrapolator/)

Пользователь

## Комментарии 42

<a id="comment_8442955"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/fb8/1f2/b5d/fb81f2b5dd05acb0b3ce8c2dd5bca67a.jpg)](https://habr.com/ru/users/Meklon/ "Meklon")[Meklon](https://habr.com/ru/users/Meklon/) [1 июн 2015 в 19:28](#comment_8442955)

Спасибо. Как раз на Kubuntu. Только до сих пор сиду на привычном MBR. Ключевой вопрос — а нафига, если grub и так сам находит все разделы с ОС?

Комментарий пока не оценивали 0

<a id="comment_8443031"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/130/202/b4e/130202b4ec7c628a2db731d072ee6249.png)](https://habr.com/ru/users/Bringoff/ "Bringoff")[Bringoff](https://habr.com/ru/users/Bringoff/) [1 июн 2015 в 20:35](#comment_8443031)

Не знаю, с чем это связано (возможно, и вправду не в EFI раздел ставил), но после обновления Ubuntu Grub отказывался запускать винду и всякие фиксы ребилды списков не помогали.

Комментарий пока не оценивали 0

<a id="comment_8443059"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/fb8/1f2/b5d/fb81f2b5dd05acb0b3ce8c2dd5bca67a.jpg)](https://habr.com/ru/users/Meklon/ "Meklon")[Meklon](https://habr.com/ru/users/Meklon/) [1 июн 2015 в 20:48](#comment_8443059)

Сложно сказать. У меня сейчас Kubuntu 15.04, MBR, grub2, Windows 7, Windows 10. Все работает.

Комментарий пока не оценивали 0

<a id="comment_8443065"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/6b0/c4c/829/6b0c4c829359b208e9525b6009bd2d59.jpg)](https://habr.com/ru/users/xlin/ "xlin")[xlin](https://habr.com/ru/users/xlin/) [1 июн 2015 в 20:52](#comment_8443065)

Потому что MBR для людей =)

Всего голосов 6: ↑3 и ↓3 0

<a id="comment_8443079"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/fb8/1f2/b5d/fb81f2b5dd05acb0b3ce8c2dd5bca67a.jpg)](https://habr.com/ru/users/Meklon/ "Meklon")[Meklon](https://habr.com/ru/users/Meklon/) [1 июн 2015 в 21:20](#comment_8443079)

Да вот фиг его знает. Я не трогаю, пока понятно и работает. Явных минусов нет)

Всего голосов 3: ↑2 и ↓1 +1

<a id="comment_8443353"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/6ab/f72/794/6abf72794ebaa649ab0ae7af91d1e58a.jpg)](https://habr.com/ru/users/AmdY/ "AmdY")[AmdY](https://habr.com/ru/users/AmdY/) [2 июн 2015 в 00:00](#comment_8443353)

Та же беда была, после обновления кубунты и двух перезагрузок всё само починилось и винда нашлась грабом

Комментарий пока не оценивали 0

<a id="comment_8443069"></a>

[](https://habr.com/ru/users/Extrapolator/ "Extrapolator")[Extrapolator](https://habr.com/ru/users/Extrapolator/) [1 июн 2015 в 21:07](#comment_8443069)

Согласен, с MBR пока еще вполне можно работать. Однако в самом дизайне MBR много недочетов, которые вынуждают использовать разные хаки, такие как запись в нестандартизированную область диска между собственно MBR и первым разделом диска, и использование extended partitions для создания более 4 разделов.
Вообще сейчас тенденция идет к тому, что скоро все производители перейдут на GPT, поэтому я просто решил заранее изучить тему :)

Комментарий пока не оценивали 0

<a id="comment_8443085"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/fb8/1f2/b5d/fb81f2b5dd05acb0b3ce8c2dd5bca67a.jpg)](https://habr.com/ru/users/Meklon/ "Meklon")[Meklon](https://habr.com/ru/users/Meklon/) [1 июн 2015 в 21:21](#comment_8443085)

У меня GPT только на 3 ТБ винчестере — хранилище в домашнем сервере. Из-за ограничений по размеру.

Комментарий пока не оценивали 0

<a id="comment_8443143"></a>

[](https://habr.com/ru/users/ilrandir/ "ilrandir")[ilrandir](https://habr.com/ru/users/ilrandir/) [1 июн 2015 в 21:54](#comment_8443143)

Единственная причина использовать GPT это необходимость одного раздела больше 2 тб размером. Я так понимаю что проблем с восстановлением разделов с GPT диска еще не было? Посмотрим как пойдет.

Всего голосов 1: ↑1 и ↓0 +1

<a id="comment_8443421"></a>

НЛО прилетело и опубликовало эту надпись здесь

<a id="comment_8443903"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/968/6e6/6f8/9686e66f8dbb84a0824b2a8f9e2748ed.jpg)](https://habr.com/ru/users/achekalin/ "achekalin")[achekalin](https://habr.com/ru/users/achekalin/) [2 июн 2015 в 11:42](#comment_8443903)

\> винду на отдельный диск
Ваша правда, только в буках, увы, диск чаще всего мало что один, так еще и небольшой (потому что SSD).
Про «небольшой» — это мое личное, я стараюсь именно твердотельные диски в буках использовать, и классических 128 Гб отлично хватает под мои задачи.
А вот про «один» — это ахиллесова стопа всех небольших девайсов, туда второй диск (кроме как на шнурочке снаружи) и не подключить. Более того, часто биосы ноутбуков особо не отличаются выбором опций загрузки с чего угодно, кроме встроенного диска, ибо — «зачем юзера путать»?
Так что, да, dual boot порой ой как нужен.
P.S. Спасибо автору за напоминание про вредность fast boot. На SSD скорость загрузки несущественно меньше, а вот шансы что-то отгрести — выше, так что не стоит оно того.

Комментарий пока не оценивали 0

<a id="comment_8444841"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/714/3f4/b60/7143f4b60c94e82765e2110dfec44755.jpg)](https://habr.com/ru/users/Scraelos/ "Scraelos")[Scraelos](https://habr.com/ru/users/Scraelos/) [2 июн 2015 в 17:20](#comment_8444841)

если в ноутбуке есть mSATA, то ничто не мешает поставить это [market.yandex.ru/product/10777568?hid&show-uid=610417314332547891](https://market.yandex.ru/product/10777568?hid&show-uid=610417314332547891)

Комментарий пока не оценивали 0

<a id="comment_8444341"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/65a/a9e/b12/65aa9eb12890a4444c32c5267af977ec.png)](https://habr.com/ru/users/CaptainFlint/ "CaptainFlint")[CaptainFlint](https://habr.com/ru/users/CaptainFlint/) [2 июн 2015 в 14:27](#comment_8444341)

> 3\. Нахрена нужен своп когда есть 16 гигов оперативы.

Например, чтобы использовать Suspend to Disk, который записывает данные именно в своп.

Комментарий пока не оценивали 0

<a id="comment_8443185"></a>

НЛО прилетело и опубликовало эту надпись здесь

<a id="comment_8443261"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/de5/270/246/de5270246819eaeef6426e2bdccedce7.jpg)](https://habr.com/ru/users/larrabee/ "larrabee")[larrabee](https://habr.com/ru/users/larrabee/) [1 июн 2015 в 22:59](#comment_8443261)

Ставить можно в любом порядке. Что меня удивило, так это то, что винда при установке найдя EFI раздел с gummiboot (он грузит мой Arch) не стала его форматировать и творить с ним всякие непотребства, а просто записало туда свое EFI приложение. gummiboot умеет сам находить виндовый загрузчик на разделе, поэтому вся инструкция сводится к такой:

1.  Разбить диск в GPT и создать EFI раздел и раздел под Linux
2.  Поставить Linux
3.  Поставить Windows

Всего голосов 3: ↑3 и ↓0 +3

<a id="comment_8443287"></a>

[](https://habr.com/ru/users/Extrapolator/ "Extrapolator")[Extrapolator](https://habr.com/ru/users/Extrapolator/) [1 июн 2015 в 23:21](#comment_8443287)

О, интересный расклад. Я даже не стал пробовать винду после линукса ставить — почему-то посчитал, что она все сломает по умолчанию. Спасибо за информацию, это очень полезно знать.

Комментарий пока не оценивали 0

<a id="comment_8443517"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/a9e/a96/2c9/a9ea962c9779843d7537c37dfc1a601c.jpg)](https://habr.com/ru/users/Botkin/ "Botkin")[Botkin](https://habr.com/ru/users/Botkin/) [2 июн 2015 в 06:37](#comment_8443517)

С 7 версии так делает (как минимум).
Люди почему-то привыкли судить о винде по версиям 12летней давности

Комментарий пока не оценивали 0

<a id="comment_8443931"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/de5/270/246/de5270246819eaeef6426e2bdccedce7.jpg)](https://habr.com/ru/users/larrabee/ "larrabee")[larrabee](https://habr.com/ru/users/larrabee/) [2 июн 2015 в 11:52](#comment_8443931)

Потому что обычно это помогает избежать лишних разочарований.
Недавно пробовал поставить 7ку на ультрабук от Sony в режиме UEFI.
Первая проблема\- загрузчик установочного диска вылетает с ошибкой. Бага известна с релиза Win 7, но почему то не пофикшена ни в SP1, не отдельным фиксом. Решил заменой загрузчика на версию от Windows 8. После этого получил зависания на экране загрузки. Судя по форумам проблема известная, возникает на некотрых ноутах и ПК и не решается (виноват драйвер disk.sys).
И как после этого верить в силу винды, если элементарные баги пятилетней давности висят без нормальных фиксов?

Комментарий пока не оценивали 0

<a id="comment_8444473"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/a9e/a96/2c9/a9ea962c9779843d7537c37dfc1a601c.jpg)](https://habr.com/ru/users/Botkin/ "Botkin")[Botkin](https://habr.com/ru/users/Botkin/) [2 июн 2015 в 15:32](#comment_8444473)

после 7 уже вышло 2 версии ОС

Всего голосов 1: ↑1 и ↓0 +1

<a id="comment_8444557"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/71c/8da/a7b/71c8daa7bec4020ffb44f6c8d19aacee.png)](https://habr.com/ru/users/lvx/ "lvx")[lvx](https://habr.com/ru/users/lvx/) [2 июн 2015 в 16:09](#comment_8444557)

Разве это что-то меняет в том факте, что Windows 7 всё ещё поддерживается мелкомягкими?

Комментарий пока не оценивали 0

<a id="comment_8444921"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/130/202/b4e/130202b4ec7c628a2db731d072ee6249.png)](https://habr.com/ru/users/Bringoff/ "Bringoff")[Bringoff](https://habr.com/ru/users/Bringoff/) [2 июн 2015 в 17:54](#comment_8444921)

Основная поддержка уже закончилась.

Комментарий пока не оценивали 0

<a id="comment_8443417"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/65a/a9e/b12/65aa9eb12890a4444c32c5267af977ec.png)](https://habr.com/ru/users/CaptainFlint/ "CaptainFlint")[CaptainFlint](https://habr.com/ru/users/CaptainFlint/) [2 июн 2015 в 00:57 Комментарий был изменен](#comment_8443417)

> Кроме того, UEFI приятно выглядит и там есть поддержка мышки.

Рекомендую иметь в виду, что это не всегда так. Я уже неоднократно сталкивался с ноутбуками и планшетами, в которых прошивка UEFI (и порой даже с поддержкой Secure Boot), но интерфейс настроек биоса — всё тот же убогий сине-белый набор менюшек в текстовом режиме.

> MSR (128Мб, [Microsoft Reserved Partition](http://en.wikipedia.org/wiki/Microsoft_Reserved_Partition)). Для меня остается загадкой, зачем он нужен.

В общем-то, по ссылке как раз описано, зачем он нужен: если раньше между MBR и первым разделом всегда было как минимум 62 сектора (а когда ушли от выравнивания по цилиндрам — то и почти целый мегабайт), то в GPT эта область занята самой GPT-таблицей. Так что приложения, которые использовали эти секторы под свои нужды, остались без доступного пространства.
В качестве примера можно привести тот же Grub. Если попробовать установить его в режиме Legacy (не-EFI) на GPT-диск, то он откажется это делать, если на диске нет специального раздела с типом BIOS Boot Partition, который точно так же, как и MSR, ни во что не отформатирован, а просто выполняет роль резервного пространства, куда граб смог бы записать своё основное тело.
Остаётся, конечно, вопрос, зачем MS резервирует аж 128 мегабайт, но, возможно, они просто перестраховываются, чтоб не вляпаться в очередные «64 Кб, 128 Гб, 2 Тб должно хватить каждому». Кстати, если диск не слишком большой, то под MSR будет отъедаться 32 Мб. Кое-какие подробности имеются в [MSDN](https://msdn.microsoft.com/en-us/library/windows/hardware/dn640535%28v=vs.85%29.aspx#gpt_faq_what_is_msr).

Всего голосов 2: ↑2 и ↓0 +2

<a id="comment_8443543"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/130/202/b4e/130202b4ec7c628a2db731d072ee6249.png)](https://habr.com/ru/users/Bringoff/ "Bringoff")[Bringoff](https://habr.com/ru/users/Bringoff/) [2 июн 2015 в 07:42](#comment_8443543)

> Рекомендую иметь в виду, что это не всегда так. Я уже неоднократно сталкивался с ноутбуками и планшетами, в которых прошивка UEFI (и порой даже с поддержкой Secure Boot), но интерфейс настроек биоса — всё тот же убогий сине-белый набор менюшек в текстовом режиме.

Имеется такое непотребство. Lenovo g500, с которого пишу — EFI, Secure Boot, а на вид все тот же BIOS. Я когда только купил, даже не сразу понял, а где, собственно, UEFI.

Комментарий пока не оценивали 0

<a id="comment_8464427"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/779/381/3a7/7793813a75b4a09f8e495d91164c61ce.jpg)](https://habr.com/ru/users/CodeRush/ "CodeRush")[CodeRush](https://habr.com/ru/users/CodeRush/) [16 июн 2015 в 21:38](#comment_8464427)

Расскажу вам как разработчик UEFI, что вот этот «убогий сине-белый набор менюшек в текстовом режиме» превосходит по стабильности и отлаженности графический сетап на порядок. И работает даже в text mode, когда с VBIOS или GOP-драйвером что-то стряслось. И serial redirection в нем работает замечательно, а не как попало. И ломаться там почти нечему, ибо 90% кода уже лет 5 входит в состав открытого проекта TianoCore.
UEFI — это не сетап и не менюшки, это в первую очередь интерфейс для драйверов и загрузчиков, свободный груза легаси-костылей времен царя Гороха, способный не загружать неподписанные option ROM'ы и выступать в качестве root-of-trust для ОС.

Комментарий пока не оценивали 0

<a id="comment_8443483"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/9c1/d9c/41c/9c1d9c41c73b18614c5326af18f6396c.jpg)](https://habr.com/ru/users/mefikru/ "mefikru")[mefikru](https://habr.com/ru/users/mefikru/) [2 июн 2015 в 04:12](#comment_8443483)

Есть задача разбить диск в GPT под досом… Диск меньше 2Tb. Именно GPT и именно из под DOS. Ghost зараза не умеет. Я уже и такие извращение как HX DOS Extender пробовал, но DLL для diskpart не хватает. Кто-нибудь может что подсказать?

Комментарий пока не оценивали 0

<a id="comment_8443555"></a>

[](https://habr.com/ru/users/Cobolorum/ "Cobolorum")[Cobolorum](https://habr.com/ru/users/Cobolorum/) [2 июн 2015 в 07:53](#comment_8443555)

У вас под досом какая то многозадачная и реалтайм прикладуха крутится, что нельзя ее выключить на 5 минут?
Может проще загрузиться с live usb и разбить?

Всего голосов 2: ↑2 и ↓0 +2

<a id="comment_8443915"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/9c1/d9c/41c/9c1d9c41c73b18614c5326af18f6396c.jpg)](https://habr.com/ru/users/mefikru/ "mefikru")[mefikru](https://habr.com/ru/users/mefikru/) [2 июн 2015 в 11:47](#comment_8443915)

У меня диск, с кучей софта, для прошивки BIOS, MAC адресов и т.п. Почти весь такой софт только зараза для DOS, загружается досовое меню, из которого выбираешь какую плату шить. Не EFI разделы раньше лил через ghost, а сейчас хочется накатывать UEFI загрузчик виндовый, с GPT. Хочется сделать единым меню. На линух не могу перейти, кучу досовых тулзов. Некоторые тулзы не работают из под win32, т.е. winpe тоже блин не катит.

Комментарий пока не оценивали 0

<a id="comment_8443833"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/1df/b61/1b0/1dfb611b054d3d799c3849c7107dcc34.png)](https://habr.com/ru/users/justabaka/ "justabaka")[justabaka](https://habr.com/ru/users/justabaka/) [2 июн 2015 в 11:13 Комментарий был изменен](#comment_8443833)

Всю работу за автора делает grub-install/update-grub и программы установки ОС, никаких граблей нет, никакого виртуозного соло в консоли не требуется. Скучно и обыденно, максимум тянет на заметку в блокноте эникея :(
Увы, но в итоге всю статью (кроме вводных пассажей о UEFI) можно выразить двумя фразами:
1\. Cтавим сначала Windows, затем Linux, выбрав efi-раздел для установки загрузчика.
2\. В Windows отключаем Fast Boot.

Комментарий пока не оценивали 0

<a id="comment_8444129"></a>

[](https://habr.com/ru/users/en1gma/ "en1gma")[en1gma](https://habr.com/ru/users/en1gma/) [2 июн 2015 в 12:57](#comment_8444129)

когда же человеки привыкнут, что есть uefi, есть bios, (есть u-boot, есть coreboot, есть....) и есть конфигуратор для их настройки.
не любой графический конфигуратор есть uefi *(у меня был доступ к «машинам белой сборки» в середине-конце 90х с графическим конфигуратором для bios, вылитым win3.x)*.
аналогично, не любой текстовый конфигуратор есть bios *(те же uefi от insyde, которые с конца 00х, если не раньше, «ставятся» на многие ноуты)*.
есть и графические конфигураторы, стилизованные под текстовые.
прям болезнь какая-то: джип, ксерокс, симка, кола…

Всего голосов 2: ↑2 и ↓0 +2

<a id="comment_8444205"></a>

НЛО прилетело и опубликовало эту надпись здесь

<a id="comment_8444241"></a>

[](https://habr.com/ru/users/JerleShannara/ "JerleShannara")[JerleShannara](https://habr.com/ru/users/JerleShannara/) [2 июн 2015 в 13:40](#comment_8444241)

Белая сборка 90ых годов «вылитый win3.1» и есть этот самый win 3.1 (а сборка есть компак), и BIOS/UEFI/PopaBigimota Setup в классическом понимании там отсутствует, а то, что им кажется — грузится либо с 1-3 дискет, либо ставится на жесткий диск. И эта дребедень у компаков тянулась с 80ых годов до покупки их HP.
Да и смысла это улучшать, очень мало кому нужно на своей рабочей машине постоянно лезть в настройки биоса (оверклокеры, хотя там один раз разогнал и забыл).

Комментарий пока не оценивали 0

<a id="comment_8444541"></a>

НЛО прилетело и опубликовало эту надпись здесь

<a id="comment_8445291"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/e4d/f97/9a8/e4df979a8b9e081af1488dc34cab05cf.jpg)](https://habr.com/ru/users/Jeditobe/ "Jeditobe")[Jeditobe](https://habr.com/ru/users/Jeditobe/) [2 июн 2015 в 21:22 Комментарий был изменен](#comment_8445291)

**What is a Microsoft Reserved Partition (MSR)?**
The Microsoft Reserved Partition (MSR) reserves space on each disk drive for subsequent use by operating system software. GPT disks do not allow hidden sectors. Software components that formerly used hidden sectors now allocate portions of the MSR for component-specific partitions. For example, converting a basic disk to a dynamic disk causes the MSR on that disk to be reduced in size and a newly created partition holds the dynamic disk database. The MSR has the Partition GUID:
DEFINE\_GUID (PARTITION\_MSFT\_RESERVED\_GUID, 0xE3C9E316L, 0x0B5C, 0x4DB8, 0x81, 0x7D, 0xF9, 0x2D, 0xF0, 0x02, 0x15, 0xAE)
**What disks require an MSR?**
Every GPT disk must contain an MSR. The order of partitions on the disk should be ESP (if any), OEM (if any) and MSR followed by primary data partition(s). It is particularly important that the MSR be created before other primary data partitions.
**Who creates the MSR?**
The MSR must be created when disk-partitioning information is first written to the drive. If the manufacturer partitions the disk, the manufacturer must create the MSR at the same time. If Windows partitions the disk during setup, Windows creates the MSR.
**Why must the MSR be created when the disk is first partitioned?**
After the disk is partitioned, there will be no free space left to create an MSR.
**How big is the MSR?**
When initially created, the size of the MSR depends on the size of the disk drive:
On drives less than 16GB in size, the MSR is 32MB.
On drives greater than or equal two 16GB, the MSR is 128 MB.
As the MSR is divided into other partitions, it becomes smaller.

Комментарий пока не оценивали 0

<a id="comment_8445379"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/fb8/1f2/b5d/fb81f2b5dd05acb0b3ce8c2dd5bca67a.jpg)](https://habr.com/ru/users/Meklon/ "Meklon")[Meklon](https://habr.com/ru/users/Meklon/) [2 июн 2015 в 22:28](#comment_8445379)

> What disks require an MSR?
> Every GPT disk must contain an MSR

Это что за фигня вообще? А если Linux? Почему кусок от Microsoft является обязательным?

Комментарий пока не оценивали 0

<a id="comment_8445569"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/65a/a9e/b12/65aa9eb12890a4444c32c5267af977ec.png)](https://habr.com/ru/users/CaptainFlint/ "CaptainFlint")[CaptainFlint](https://habr.com/ru/users/CaptainFlint/) [3 июн 2015 в 01:33](#comment_8445569)

Потому что это выдержка из документа под названием «Windows and GPT».

Комментарий пока не оценивали 0

<a id="comment_8486113"></a>

[](https://habr.com/ru/users/MrMerak/ "MrMerak")[MrMerak](https://habr.com/ru/users/MrMerak/) [3 июл 2015 в 14:40](#comment_8486113)

" надо обязательно отключить fast boot"
и как это сделать?

Комментарий пока не оценивали 0

<a id="comment_8486171"></a>

[](https://habr.com/ru/users/MrMerak/ "MrMerak")[MrMerak](https://habr.com/ru/users/MrMerak/) [3 июл 2015 в 15:17](#comment_8486171)

и кстати потом можно будет винду переустанавливать?

Комментарий пока не оценивали 0

<a id="comment_8486963"></a>

[](https://habr.com/ru/users/MrMerak/ "MrMerak")[MrMerak](https://habr.com/ru/users/MrMerak/) [4 июл 2015 в 15:46](#comment_8486963)

автор можно Ваш е-мейд? есть пара вопросов по статье, плз

Комментарий пока не оценивали 0

<a id="comment_8487881"></a>

[](https://habr.com/ru/users/Extrapolator/ "Extrapolator")[Extrapolator](https://habr.com/ru/users/Extrapolator/) [6 июл 2015 в 10:32](#comment_8487881)

ответил в личку

Комментарий пока не оценивали 0

<a id="comment_8662221"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/ad1/bbe/3cb/ad1bbe3cb61419fa42c31e6aefb42b01.jpg)](https://habr.com/ru/users/DP_Amethyst/ "DP_Amethyst")[DP_Amethyst](https://habr.com/ru/users/DP_Amethyst/) [22 ноя 2015 в 07:45](#comment_8662221)

Насколько я понимаю, статья описывает установку систем на один физический носитель. А теперь, внимание, вопрос: куда записывать grub-efi, если windows и Ubuntu устанавливаются на разные носители(ноутбук с 2 винтами каждый по 1TB)?

Комментарий пока не оценивали 0

<a id="comment_8662827"></a>

[](https://habr.com/ru/users/Extrapolator/ "Extrapolator")[Extrapolator](https://habr.com/ru/users/Extrapolator/) [23 ноя 2015 в 11:06](#comment_8662827)

С этим не разбирался, но по идее порядок действий будет такой же:
1\. Поставить Винду с ее стандартным загрузчиком на один диск, скорее всего это уже будет efi boot partition
2\. Поставить linux и efi на другой диск
3\. Сконфигурить grub-efi, при необходимости прописав там строчку для загрузки винды.
В случае нескольких загрузочных efi разделов, они должны автоматически подгружаться, поэтому по идее даже дополнительно настраивать ничего не надо будет.

Комментарий пока не оценивали 0

<a id="comment_8663093"></a>

[![](https://habrastorage.org/r/w48/getpro/habr/avatars/65a/a9e/b12/65aa9eb12890a4444c32c5267af977ec.png)](https://habr.com/ru/users/CaptainFlint/ "CaptainFlint")[CaptainFlint](https://habr.com/ru/users/CaptainFlint/) [23 ноя 2015 в 13:15](#comment_8663093)

Если диски стационарные, то EFI-раздел можно оставить только на одном диске (лучше на виндовом, т. к. Windows менее гибкая в этом плане). EFI-грабу всё равно, откуда грузить ядро.
Если планируется диски вынимать или переставлять их в другие компьютеры, то лучше на каждом держать свой EFI-раздел. Но тут ещё надо учитывать, что на другом компьютере в UEFI будет отсутствовать загрузочная запись, указывающая на конкретный EFI-файл граба, так что придётся либо выбирать файл вручную (если прошивка это умеет), либо создавать дубликат в виде умолчального файла /EFI/BOOT/BOOTx64.EFI. Некоторые системы это сами делают, но не знаю, относится ли к ним Убунта.

Комментарий пока не оценивали 0

Только полноправные пользователи могут оставлять комментарии. [Войдите](https://habr.com/kek/v1/auth/habrahabr/?back=/ru/articles/259283/&hl=ru), пожалуйста.

## Публикации

- [![](https://habrastorage.org/r/w48/getpro/habr/avatars/9e0/aa9/119/9e0aa9119b00183b206002b8df7ab964.png)](https://habr.com/ru/users/ProgerXP/ "ProgerXP")[ProgerXP](https://habr.com/ru/users/ProgerXP/) 16 часов назад
    
    ## [О, «Герои»? Дайте две! Как я писал очередной браузерный клон легендарной стратегии, в который уже почти\* можно играть](https://habr.com/ru/companies/soletude/articles/719280/)
    
    Уровень сложности Простой
    
    Время на прочтение 15 мин
    
    Количество просмотров 5.8K
    
    Кейс
    
    Всего голосов 76: ↑76 и ↓0 +76
    
    [Комментарии 22](https://habr.com/ru/companies/soletude/articles/719280/comments/)[+22](https://habr.com/ru/companies/soletude/articles/719280/comments/)
    
- [![](https://habrastorage.org/r/w48/getpro/habr/avatars/578/95d/2e1/57895d2e114fe15534125172bfcbfb54.jpg)](https://habr.com/ru/users/Gorislav/ "Gorislav")[Gorislav](https://habr.com/ru/users/Gorislav/) 16 часов назад
    
    ## [«Процедурное рисование» в ComfyUI](https://habr.com/ru/articles/729848/)
    
    Время на прочтение 7 мин
    
    Количество просмотров 2.3K
    
    Туториал
    
    Всего голосов 52: ↑52 и ↓0 +52
    
    [Комментарии 1](https://habr.com/ru/articles/729848/comments/)[+1](https://habr.com/ru/articles/729848/comments/)
    
- [![](https://habrastorage.org/r/w48/getpro/habr/avatars/b05/b8c/284/b05b8c28457a98724d5a0639af8268fb.jpg)](https://habr.com/ru/users/dmitriyrudnev/ "dmitriyrudnev")[dmitriyrudnev](https://habr.com/ru/users/dmitriyrudnev/) 20 часов назад
    
    ## [«Ямбический» электронный ключ на Black Pill](https://habr.com/ru/companies/ruvds/articles/721616/)
    
    Уровень сложности Средний
    
    Время на прочтение 6 мин
    
    Количество просмотров 2.1K
    
    Обзор
    
    Всего голосов 44: ↑44 и ↓0 +44
    
    [Комментарии 8](https://habr.com/ru/companies/ruvds/articles/721616/comments/)[+8](https://habr.com/ru/companies/ruvds/articles/721616/comments/)
    
- [](https://habr.com/ru/users/itdog/ "itdog")[itdog](https://habr.com/ru/users/itdog/) 20 часов назад
    
    ## [Какого провайдера VPS выбрать для собственного VPN в 2023 году. Платим за всё российской картой](https://habr.com/ru/articles/729750/)
    
    Уровень сложности Простой
    
    Время на прочтение 5 мин
    
    Количество просмотров 13K
    
    Обзор
    
    Всего голосов 36: ↑36 и ↓0 +36
    
    [Комментарии 79](https://habr.com/ru/articles/729750/comments/)[+79](https://habr.com/ru/articles/729750/comments/)
    
- [![](https://habrastorage.org/r/w48/getpro/habr/avatars/d7a/462/aef/d7a462aef5791b8e4e58f931a19c3fd6.jpg)](https://habr.com/ru/users/myoffice_ru/ "myoffice_ru")[myoffice_ru](https://habr.com/ru/users/myoffice_ru/) 16 часов назад
    
    ## [МойОфис выпустил Squadus — единое цифровое рабочее пространство. Рассказываем о новинке](https://habr.com/ru/companies/ncloudtech/articles/729792/)
    
    Время на прочтение 5 мин
    
    Количество просмотров 2.4K
    
    Всего голосов 41: ↑37 и ↓4 +33
    
    [Комментарии 7](https://habr.com/ru/companies/ncloudtech/articles/729792/comments/)[+7](https://habr.com/ru/companies/ncloudtech/articles/729792/comments/)
    

[Каким должен быть идеальный конвейер обработки данных: ищем ответ в новом сезоне](https://effect.habr.com/a/PzokneqDHTYwY7DRO8gabnUv81e-UExTzPL4oisBtrPhxZioHadalNBtZqdOi5HiMrvmrvlIKWL726Q6lIdTF-cfYAui1MpE93qFFpFh1SaO2ooCB3R8sHbeXLetAg)

Спецпроект

## Минуточку внимания

[Разместить](https://tmtm.ru/megapost/)

[![](https://effect.habr.com/a/YVAhaISmI5OMcu_kgqCGkopqhy4RV4HYyuHN4u5-rYre1Y5D-KPhIFbYL5ijYj51vRQWRn-QLhMhZwUQqSZKEYWbrxB3k471vFEHs8x_3a7CHHQuwTdKx1qF8sLZCmMBQcPQQK2vohwKyz9VDT_rFtUiVY1ve76r5_W-PMp8jAW1vzgiazU9CtZyfxZDuI6M1ufYaBNj0o8uONK2Ww)<br>Турбо<br>### Как небольшой компании расцвести на Хабре за полгода](https://effect.habr.com/a/0Q9WSZ0k6XSo96xyiHjqqw-5_9hmzscoK9zspnNZiMeQzYJmIb-M91ZwtCCqWHdk6-79Iv9hTcya0P-2AVuK7-WTDloMapWQVrlLDp0-ASO6LwtVtoGUadgv9-5F)

[![](https://effect.habr.com/a/61KzoL8jDSkzIKRTQ3Pn89cgN2M702GihKW1xTxSGU4nONDhLoaAuvpf1de6XptoCdEDVF4fKyx9XBZzUSM75ux_SQcnc8BFx0dTLk98u5qg3AuiwwexmkLeJ1OlzeUo1x2O0jINdsOhPQMg8WyFLKHTHfwn-NeFdmiuw8aPNSp7bEloSA6Yl1KEeb7xbVEff37Ct8LqJ5cn_3MRBM5pmftJmE_0pZjBLUYrhPfx_g0_Mi55yQ-4bqU)<br>Спецпроект<br>### Сезон Big Data: ныряем в самые глубокие озёра данных](https://effect.habr.com/a/O2esAWXG-NUnxvn-dwb2p8GJMDmIPw-07C_OUh3w9kPmPFJHRMabecSWSOmQmCRJ-mnNmzwlN01wSyp0NG5EzlsmWqEW8aQKC3gh2uzIsCRw7Kvj_CTVq9m2X6t84A)

[![](https://effect.habr.com/a/yK3W_zcYscGJsQ2CTyTRyLwVpGRhbyFOHwAXnBtf291ncrWW14G5ySPsqonV_EeXQ1_O2nBSZTYF5XFAhqcfZADpmQA5lUAhJ9NPgWHyRIOSzRz74AcRL647ii5XsOH6DO7J9CiqRDORCYFONhmPLNHQXEXvYIR-St8b8VynssHwYwWIhuELeBcnhkygEpaixIMpSh4H_8_GayWShI_HX2o)<br>Интересно<br>### Получаем хабрамерч за вредные советы по поиску работы](https://effect.habr.com/a/e66gMgTxtOH97Jc-RSIZWuZOa0HfcYRUAhRBWTR2WftMb2hE_Rfxke8cR-Ay8s6bSdmOtNbVBkkMLbhCP3Y4YGZmZ8YheRLeJL4Zs8P7b08s0zYhxIZxZBh58047DZH1aWFOFhNDZx2r68E)

[Вопросы и ответы](https://qna.habr.com/questions?utm_campaign=questions_postlist&utm_content=questions&utm_medium=habr_block&utm_source=habr_mob)

- [Как обойти ошибку 0xc00000e при замене SSD?](https://qna.habr.com/q/1266536?utm_campaign=questions_postlist&utm_content=question&utm_medium=habr_block&utm_source=habr_mob)
    
    ЖелезоПростой3 ответа
    
- [Если установить другую ОС, останутся ли настройки Ryzen Master'а?](https://qna.habr.com/q/1265496?utm_campaign=questions_postlist&utm_content=question&utm_medium=habr_block&utm_source=habr_mob)
    
    LinuxПростой2 ответа
    
- [Как решить ошибку при установке Ubuntu 18.04?](https://qna.habr.com/q/1264522?utm_campaign=questions_postlist&utm_content=question&utm_medium=habr_block&utm_source=habr_mob)
    
    LinuxСредний3 ответа
    
- [Как подружить несколько ОС-ей в VHD на одном компе?](https://qna.habr.com/q/1259984?utm_campaign=questions_postlist&utm_content=question&utm_medium=habr_block&utm_source=habr_mob)
    
    WindowsСредний2 ответа
    
- [Какие варианты могут быть при рандомной невозможности системой увидеть рут-раздел при загрузке?](https://qna.habr.com/q/1259530?utm_campaign=questions_postlist&utm_content=question&utm_medium=habr_block&utm_source=habr_mob)
    
    LinuxПростой1 ответ
    

[Больше вопросов на Хабр Q&A](https://qna.habr.com/questions?utm_campaign=questions_postlist&utm_content=questions_all&utm_medium=habr_block&utm_source=habr_mob)

## Читают сейчас

- ## [Windows vs. Linux в настоящее время](https://habr.com/ru/articles/729842/)
    
    Количество просмотров 18K
    
    [Комментарии 269](https://habr.com/ru/articles/729842/comments/)[+269](https://habr.com/ru/articles/729842/comments/)
    
- ## [Солнечное затмение 20 апреля 2023 года](https://habr.com/ru/articles/729940/)
    
    Количество просмотров 6.7K
    
    [Комментарии 16](https://habr.com/ru/articles/729940/comments/)[+16](https://habr.com/ru/articles/729940/comments/)
    
- ## [Кто все вот эти на полках: краткое руководство по новым брендам ноутбуков](https://habr.com/ru/companies/sberbank/articles/729910/)
    
    Количество просмотров 8.2K
    
    [Комментарии 9](https://habr.com/ru/companies/sberbank/articles/729910/comments/)[+9](https://habr.com/ru/companies/sberbank/articles/729910/comments/)
    
- ## [Какого провайдера VPS выбрать для собственного VPN в 2023 году. Платим за всё российской картой](https://habr.com/ru/articles/729750/)
    
    Количество просмотров 13K
    
    [Комментарии 79](https://habr.com/ru/articles/729750/comments/)[+79](https://habr.com/ru/articles/729750/comments/)
    
- ## [О, «Герои»? Дайте две! Как я писал очередной браузерный клон легендарной стратегии, в который уже почти\* можно играть](https://habr.com/ru/companies/soletude/articles/719280/)
    
    Количество просмотров 5.8K
    
    [Комментарии 22](https://habr.com/ru/companies/soletude/articles/719280/comments/)[+22](https://habr.com/ru/companies/soletude/articles/719280/comments/)
    
- [Каким должен быть идеальный конвейер обработки данных: ищем ответ в новом сезоне](https://effect.habr.com/a/PzokneqDHTYwY7DRO8gabnUv81e-UExTzPL4oisBtrPhxZioHadalNBtZqdOi5HiMrvmrvlIKWL726Q6lIdTF-cfYAui1MpE93qFFpFh1SaO2ooCB3R8sHbeXLetAg)
    
    Спецпроект
    

## Истории

Неделя тестировщиков

Пистолет, распознающий владельца

Маск снова взялся за ИИ

Зачем инженеру танцевать танго

Активность найма на IT-рынке в марте

Бесплатные вакансии в тестировании

День космонавтики

Топ-7 годноты из блогов компаний

Читай и худей

Полезная подборка о зрении

Тренируйся: физкульт-подборка

Сеньоры — очень странные люди

## Работа

[Системный администратор](https://career.habr.com/vacancies/sistemnij_administrator)

151 вакансия

[DevOps инженер](https://career.habr.com/vacancies/devops)

55 вакансий

[Все вакансии](https://career.habr.com/catalog)

Ваш аккаунт

- [Войти](https://habr.com/kek/v1/auth/habrahabr/?back=/ru/articles/259283/&hl=ru)
- [Регистрация](https://habr.com/kek/v1/auth/habrahabr-register/?back=/ru/articles/259283/&hl=ru)

Разделы

- [Статьи](https://habr.com/ru/)
- [Новости](https://habr.com/ru/news/)
- [Хабы](https://habr.com/ru/hubs/)
- [Компании](https://habr.com/ru/companies/)
- [Авторы](https://habr.com/ru/users/)
- [Песочница](https://habr.com/ru/sandbox/)

Информация

- [Устройство сайта](https://habr.com/ru/docs/help/)
- [Для авторов](https://habr.com/ru/docs/authors/codex/)
- [Для компаний](https://habr.com/ru/docs/companies/corpblogs/)
- [Документы](https://habr.com/ru/docs/docs/transparency/)
- [Соглашение](https://account.habr.com/info/agreement)
- [Конфиденциальность](https://account.habr.com/info/confidential/)

Услуги

- [Корпоративный блог](https://company.habr.com/ru/corporate-blogs/)
- [Медийная реклама](https://company.habr.com/ru/advertising/)
- [Нативные проекты](https://company.habr.com/ru/native-special/)
- [Образовательные программы](https://company.habr.com/ru/education-programs/)
- [Стартапам](https://company.habr.com/ru/hello-startup/)
- [Спецпроекты](https://habr.com/ru/specials/)

[Facebook](https://www.facebook.com/habrahabr.ru) [Twitter](https://twitter.com/habr_com) [VK](https://vk.com/habr) [Telegram](https://telegram.me/habr_com) [Youtube](https://www.youtube.com/channel/UCd_sTwKqVrweTt4oAKY5y4w) [Яндекс Дзен](https://zen.yandex.ru/habr)

[Техническая поддержка](https://habr.com/ru/feedback/) [Вернуться на старую версию](https://habr.com/berserk-mode-nope)

© 2006–2023, [Habr](https://company.habr.com/)