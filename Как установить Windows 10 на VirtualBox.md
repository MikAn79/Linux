[Главная](https://losst.pro/) >\> [Инструкции](https://losst.pro/instruktsii) >\> Как установить Windows 10 на VirtualBox

# Как установить Windows 10 на VirtualBox

Опубликовано: 18 января, 2018 от [admin](https://losst.pro/author/admin "Просмотр всех записей admin") , 10 комменариев, время чтения: 6 минут, 31 секунд

Windows на виртуальной машине может понадобиться для решения совершенно различных задач, например, для тестирования программного обеспечения, анонимности и так далее. Но для пользователей Linux есть еще одна причина - множество Windows программ не поддерживаются в эмуляторе Wine и нам нужно иметь полноценную Windows чтобы использовать необходимые программы. В таком случае VirtualBox - это идеальное решение.

В этой статье мы рассмотрим как установить Windows 10 на VirtualBox, разберем полную настройку виртуальной машины, а также настройку установленной системы для того, чтобы добиться максимальной производительности и избежать проблем.

Содержание статьи:

- [Что нам понадобится?](#%D0%A7%D1%82%D0%BE_%D0%BD%D0%B0%D0%BC_%D0%BF%D0%BE%D0%BD%D0%B0%D0%B4%D0%BE%D0%B1%D0%B8%D1%82%D1%81%D1%8F "Что нам понадобится?")
- [Подготовка виртуальной машины](#%D0%9F%D0%BE%D0%B4%D0%B3%D0%BE%D1%82%D0%BE%D0%B2%D0%BA%D0%B0_%D0%B2%D0%B8%D1%80%D1%82%D1%83%D0%B0%D0%BB%D1%8C%D0%BD%D0%BE%D0%B9_%D0%BC%D0%B0%D1%88%D0%B8%D0%BD%D1%8B "Подготовка виртуальной машины")
- [Установка Windows 10 в VirtualBox](#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0_Windows_10_%D0%B2_VirtualBox "Установка Windows 10 в VirtualBox")
    - [Шаг 1\. Запуск машины](#%D0%A8%D0%B0%D0%B3_1_%D0%97%D0%B0%D0%BF%D1%83%D1%81%D0%BA_%D0%BC%D0%B0%D1%88%D0%B8%D0%BD%D1%8B "Шаг 1. Запуск машины")
    - [Шаг 2\. Загрузка](#%D0%A8%D0%B0%D0%B3_2_%D0%97%D0%B0%D0%B3%D1%80%D1%83%D0%B7%D0%BA%D0%B0 "Шаг 2. Загрузка")
    - [Шаг 3\. Язык системы](#%D0%A8%D0%B0%D0%B3_3_%D0%AF%D0%B7%D1%8B%D0%BA_%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D1%8B "Шаг 3. Язык системы")
    - [Шаг 4\. Подготовка](#%D0%A8%D0%B0%D0%B3_4_%D0%9F%D0%BE%D0%B4%D0%B3%D0%BE%D1%82%D0%BE%D0%B2%D0%BA%D0%B0 "Шаг 4. Подготовка")
    - [Шаг 5\. Лицензионный ключ](#%D0%A8%D0%B0%D0%B3_5_%D0%9B%D0%B8%D1%86%D0%B5%D0%BD%D0%B7%D0%B8%D0%BE%D0%BD%D0%BD%D1%8B%D0%B9_%D0%BA%D0%BB%D1%8E%D1%87 "Шаг 5. Лицензионный ключ")
    - [Шаг 6\. Лицензия](#%D0%A8%D0%B0%D0%B3_6_%D0%9B%D0%B8%D1%86%D0%B5%D0%BD%D0%B7%D0%B8%D1%8F "Шаг 6. Лицензия")
    - [Шаг 7\. Способ установки](#%D0%A8%D0%B0%D0%B3_7_%D0%A1%D0%BF%D0%BE%D1%81%D0%BE%D0%B1_%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B8 "Шаг 7. Способ установки")
    - [Шаг 8\. Создание раздела диска](#%D0%A8%D0%B0%D0%B3_8_%D0%A1%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5_%D1%80%D0%B0%D0%B7%D0%B4%D0%B5%D0%BB%D0%B0_%D0%B4%D0%B8%D1%81%D0%BA%D0%B0 "Шаг 8. Создание раздела диска")
    - [Шаг 9\. Установка Windows 10](#%D0%A8%D0%B0%D0%B3_9_%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0_Windows_10 "Шаг 9. Установка Windows 10")
    - [Шаг 10\. Параметры по умолчанию](#%D0%A8%D0%B0%D0%B3_10_%D0%9F%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D1%8B_%D0%BF%D0%BE_%D1%83%D0%BC%D0%BE%D0%BB%D1%87%D0%B0%D0%BD%D0%B8%D1%8E "Шаг 10. Параметры по умолчанию")
    - [Шаг 11\. Настройка сети](#%D0%A8%D0%B0%D0%B3_11_%D0%9D%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0_%D1%81%D0%B5%D1%82%D0%B8 "Шаг 11. Настройка сети")
    - [Шаг 12\. Учетная запись](#%D0%A8%D0%B0%D0%B3_12_%D0%A3%D1%87%D0%B5%D1%82%D0%BD%D0%B0%D1%8F_%D0%B7%D0%B0%D0%BF%D0%B8%D1%81%D1%8C "Шаг 12. Учетная запись")
    - [Шаг 13\. Настройка](#%D0%A8%D0%B0%D0%B3_13_%D0%9D%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0 "Шаг 13. Настройка")
    - [Шаг 14\. Готово](#%D0%A8%D0%B0%D0%B3_14_%D0%93%D0%BE%D1%82%D0%BE%D0%B2%D0%BE "Шаг 14. Готово")
- [Настройка Windows в VirtualBox](#%D0%9D%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0_Windows_%D0%B2_VirtualBox "Настройка Windows в VirtualBox")
- [Оптимизация Windows для VirtualBox](#%D0%9E%D0%BF%D1%82%D0%B8%D0%BC%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F_Windows_%D0%B4%D0%BB%D1%8F_VirtualBox "Оптимизация Windows для VirtualBox")
- [Выводы](#%D0%92%D1%8B%D0%B2%D0%BE%D0%B4%D1%8B "Выводы")

## Что нам понадобится?

Вот основные вещи, которые нам понадобятся для работы:

- Установочный образ Windows 10;
- Самая свежая версия VirtualBox - чем новее версия тем больше шансов, что там нет ошибок и все работает хорошо;
- Компьютер с поддержкой аппаратной виртуализации AMD-VT или Intel-X - вы можете запустить Windows и без виртуализации, но тогда она будет работать очень медленно;
- Оперативная память \- 6 Гб, для Windows 10 нужно выделить минимум 3 гигабайта, еще 3 останется системе, при меньшем объеме система может тормозить;
- 30 Гб свободного места на диске - необходимо для жесткого диска виртуальной машины.

Я предполагаю, что VirtualBox у вас уже установлена и готова к работе.

## Подготовка виртуальной машины

Сначала вам нужно создать саму виртуальную машину. Для этого нажмите кнопку "Создать":

![[Snimok-ekrana-ot-2017-12-26-10-2_06ba25beeeb54869a.png]](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-10-26-27.png)

В открывшемся окне введите имя будущей машины, выберите объем ОЗУ \- 3 Гб и поставьте переключатель в положение **"Создавать новый виртуальный диск"**:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-10-2_f4eb099b93454055b.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-10-27-00.png)

Параметры диска можно оставить по умолчанию, объем \- 32 гигабайта:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-10-2_f7f14ed8077640d4b.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-10-28-02.png)

Машина создана, но она еще не готова. Дальше откройте для нее контекстное меню и выберите **"Настроить"**. В открывшемся окне перейдите на вкладку **"Дисплей". **Отметьте галочки включить 3D и 2D ускорение, а затем сделайте объем видеопамяти равным 256.

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-10-3_1452c4cd57d047db9.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-10-31-47.png)

Теперь наша машина готова, нажмите **"Ok"**.

## Установка Windows 10 в VirtualBox

Дальше я буду пошагово разбирать все, что вам необходимо чтобы установка Windows на VirtualBox прошла успешно.

### Шаг 1\. Запуск машины

Запустите виртуальную машину и выберите образ Windows 10 или вставьте диск в привод, а затем выберите ваш привод:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-10-3_2a195241ca8144079.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-10-35-15.png)

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-10-3_83b9984891b943958.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-10-35-09.png)

### Шаг 2\. Загрузка

Дождитесь окончания загрузки.

### Шаг 3\. Язык системы

Выберите язык и раскладку клавиатуры:

![[Snimok-ekrana-ot-2017-12-26-11-0_8e9f0ae2a19c4dc38.png]](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-00-27.png)

### Шаг 4\. Подготовка

Нажмите кнопку **"Установить"**:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-0_bf60d87372cf40c4a.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-01-33.png)

### Шаг 5\. Лицензионный ключ

Введите любой лицензионный ключ, подходящий для вашей версии системы:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-0_730e6b8b1bff4ff5b.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-04-20.png)

### Шаг 6\. Лицензия

Примите условия лицензионного соглашения:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-0_752aaba14c594eda9.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-04-30.png)

### Шаг 7\. Способ установки

Это способ, которым будет выполняться установка. На самом деле у нас только один вариант \- **"Выборочная, только установка Windows"**:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-0_3af90d02126c41fa8.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-05-20.png)

### Шаг 8\. Создание раздела диска

В следующем окне нажмите **"Создать"**:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-0_1f048a878bf54a768.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-07-17.png)

Затем выберите все доступное пространство и нажмите **"Принять"**:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-0_2bb5101612b443a9a.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-07-21.png)

С созданием раздела для файлов восстановления соглашайтесь, пусть будет.

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-0_884285b65504486fa.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-07-27.png)

Затем, нажмите **"Далее"** для начала процесса установки.

### Шаг 9\. Установка Windows 10

Дождитесь пока завершиться установка Windows  10 на VirtualBox файлов и их распаковки на жесткий диск:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-0_1388ea53b5d94f248.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-08-14.png)

### [<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-1_9c537937fc4843758.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-17-22.png)

### Шаг 10\. Параметры по умолчанию

Система предложит вам использовать параметры по умолчанию, лучше согласиться, чтобы не вникать во все подробности, потом можно будет все изменить:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-3_e8181cf9100d49d48.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-33-22.png)

### Шаг 11\. Настройка сети

На этом шаге выберите, что компьютер принадлежит вам:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-4_e9bd57c806554711b.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-46-33.png)

### Шаг 12\. Учетная запись

От учетной записи Microsoft отказываемся, она нам не нужна. Выберите **"пропустить этот шаг"**:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-4_31a3c8d4a0844d999.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-46-54.png)

Затем введите имя пользователя, пароль и подсказку для локального пользователя:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-4_5c270d12203d46d7b.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-47-13.png)

### Шаг 13\. Настройка

Дождитесь окончания настройки системы:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-5_194a049d242b4ed28.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-50-05.png)

### [<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-5_128065bc05974ba99.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-50-14.png)

### Шаг 14\. Готово

Windows установлена и перед вами открылся рабочий стол, но это еще не все. Нам осталось сделать несколько действий, чтобы получить максимальную производительность и удобство использования от системы.

## Настройка Windows в VirtualBox

Первое, что вам нужно будет сделать \- это установить дополнения гостевой ОС. Этот пакет программ дает возможность интерактивно менять разрешение экрана, передавать файлы между гостевой и основной системой использовать общий буфер обмена и многое другое. Для установки откройте меню "Устройства" и выберите **"Подключить образ дополнений гостевой ОС"**.

Затем откройте **"Этот компьютер"** и выполните двойной клик по подключенному образу:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-5_457ce9436bc24d1fb.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-57-15.png)

Далее запустите приложение для вашей системы:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-5_42f8b1af7fc24b7db.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-58-10.png)

Затем вам предстоит пройти несколько шагов мастера установки:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-5_48fa0eeb49734dfca.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-59-07.png) [<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-5_443a7c57c00448dd8.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-59-10.png)[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-5_e1d7e579640f4c688.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-59-18.png)[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-11-5_c2fc743cd8584b339.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-11-59-46.png)[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-12-0_dc15ba502b7747a1b.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-12-00-10.png)

После завершения установки систему нужно перезагрузить.

## Оптимизация Windows для VirtualBox

В Windows есть множество служб и процессов, которые выполняются в фоне и нагружают систему, при этом они не всегда нужны, особенно на виртуальной машине, чтобы улучшить производительность Windows можно все это отключить. Специально для этого компания WMVare выпустила инструмент, который вы можете скачать [по ссылке](https://labs.vmware.com/flings/vmware-os-optimization-tool).

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-12-0_7fce7e55a4e54a86a.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-12-04-11.png)

Вы уже можете включить общий буфер обмена через меню **"Устройства"** -\> **"Буфер обмена"** -\> **"Двунаправленный"**, так что проблем с копированием ссылки у вас возникнуть не должно.

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-12-0_ee38b6a7fa3b4bf5b.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-12-06-52.png)

После загрузки распакуйте архив и запустите полученную программу:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-12-0_00c52d9eb7104b239.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-12-07-07.png)

В окне программы вы видите все доступные оптимизации, большинство из них по умолчанию включены, так что вам достаточно нажать кнопку "Optimize" в нижнем левом углу, чтобы запустить оптимизацию:

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-12-0_b5034111242d41198.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-12-08-02.png)

[<img width="780" height="439" src="../_resources/Snimok-ekrana-ot-2017-12-26-12-1_8e5b414cb61d46c6b.png" class="jop-noMdConv">](https://losst.pro/wp-content/uploads/2017/12/Snimok-ekrana-ot-2017-12-26-12-10-22.png)

После завершения оптимизации ваша Windows 10 в VirtualBox будет работать намного быстрее и не будет занимать так много оперативной памяти. Если этого недостаточно, то вы можете включить дополнительные оптимизации. Если же вы боитесь что утилита что-то сломает, то сделайте снимок состояния системы, перед тем, как переходить к оптимизации.

×

## Выводы

В этой статье мы рассмотрели как установить Windows 10 на VirtualBox и оптимизировать систему для лучшей работы. Для новичков в Linux такая система может стать незаменимой, поскольку здесь вы сможете запускать программы, которые не поддерживаются в Linux, например, последние версии офиса или другие специфические инструменты. Если у вас остались вопросы, спрашивайте в комментариях!

### Похожие записи

[![](../_resources/Snimok-ekrana-ot-2017-09-13-22-1_619e237ce15040d4a.png)Как сделать общую папку в VirtualBox](https://losst.pro/kak-sdelat-obshhuyu-papku-v-virtualbox "Как сделать общую папку в VirtualBox") [![](../_resources/kali-linux-favorites-sensorstech_e5e8834ba84f4a028.png)Как установить Kali Linux на VirtualBox](https://losst.pro/kak-ustanovit-kali-linux-na-virtualbox "Как установить Kali Linux на VirtualBox") [![](../_resources/Screenshot_20180216-181422-150x1_2ec25597266a4ce2b.png)Как пользоваться wine 3.0 Android](https://losst.pro/kak-polzovatsya-wine-3-0-android "Как пользоваться wine 3.0 Android") [![](../_resources/Screenshot-from-2018-03-19-18-19_38c504eb746942b2b.png)Как запустить Kali Linux с флешки](https://losst.pro/kak-zapustit-kali-linux-s-fleshki "Как запустить Kali Linux с флешки")

## Оцените статью

![Звёзд: 1](../_resources/rating_on_20257a63563444fb850705c54ef2ed02.gif "Звёзд: 1")![Звёзд: 2](../_resources/rating_on_20257a63563444fb850705c54ef2ed02.gif "Звёзд: 2")![Звёзд: 3](../_resources/rating_on_20257a63563444fb850705c54ef2ed02.gif "Звёзд: 3")![Звёзд: 4](../_resources/rating_on_20257a63563444fb850705c54ef2ed02.gif "Звёзд: 4")![Звёзд: 5](../_resources/rating_off_afdd5ba472b3498a907b8febd14bccec.gif "Звёзд: 5") (**7** оценок, среднее: **4,14** из 5)

[![Creative Commons License](../_resources/88x31_fcd755fa360d49a1ba746de8b332bf52.png)](https://losst.pro/content-policy)
Статья распространяется под лицензией Creative Commons ShareAlike 4.0 при копировании материала ссылка на источник обязательна .

Рубрики [Инструкции](https://losst.pro/instruktsii) Метки [virtualbox](https://losst.pro/tag/virtualbox), [windows](https://losst.pro/tag/windows)

## Об авторе

<img width="120" height="120" src="../_resources/admin-avatar_cd40ec30f5854cf5bbdacb1e7015055f.png" class="jop-noMdConv">

[admin](https://losst.pro/author/admin)

Основатель и администратор сайта losst.ru, увлекаюсь открытым программным обеспечением и операционной системой Linux. В качестве основной ОС сейчас использую Ubuntu. Кроме Linux, интересуюсь всем, что связано с информационными технологиями и современной наукой.

### 10 комментариев к “Как установить Windows 10 на VirtualBox”

1.  ![](../_resources/08df6840ec3dece226d5257d51893280_c40bded9e9b649219.jpg)
    
    Виктор
    
    [19 января, 2018 в 9:25 дп](https://losst.pro/kak-ustanovit-windows-10-na-virtualbox#comment-11860)
    
    Если есть купленная лицензия на windows, то лучше уж наверное наоборот, линукс в виндовс.
    
    А чтобы потестировать виндовс 10 (и не только десятку), качайте здесь https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/. официальные установленные образы под VBox. Легальная лицензия на 90 дней - хватит на любыу опыты. Можно продлять 2 раза на +90 дней.
    
    Просто импортируется виртуальную машину, и сразу работает, ничего устаналивать не надо. Версии там либо проф, либо корпоративные
    
    [Ответить](#comment-11860)
    
    - ![](../_resources/d111530203ae7dc7124b587eef387d24_266ba82d242c49ee8.jpg)
        
        [Интересующийся](http://gentoo.org)
        
        [25 января, 2018 в 1:45 пп](https://losst.pro/kak-ustanovit-windows-10-na-virtualbox#comment-12033)
        
        "Если есть купленная лицензия на windows, то лучше уж наверное наоборот, линукс в виндовс."
        Токмо ежели денюжку сэкономить, не более. В остальном же всё не так. В *никсах виртуализация устроена на порядок лучше, чем в винде, работает намного эффективней. Например, есть возможность напрямую пробрасывать железки к гостевой машинке, ту же видеокарту для чтобы игорей (правда, придётся брать в руки бубен, ибо не всё там так просто). Имеется также выбор способов виртуализации, выбирай не хочу. Даже сам факт того, что мелкомягкие выбрали для своей платформы виртуализации Azure за основу Линукс, говорит о многом.
        Кстати, за ссылку премного благодарен. Очень актуальна она для мну сейчас: нужно потестить работу самбы в локалке с виндоузами, а под рукой только линуксы.
        
        [Ответить](#comment-12033)
        
2.  ![](../_resources/b4d69cd8810238a1065fd108db67f4c6_1536a735d2644508a.jpg)
    
    Snooz
    
    [19 января, 2018 в 12:32 пп](https://losst.pro/kak-ustanovit-windows-10-na-virtualbox#comment-11863)
    
    32 разрядная версия окошек живее будет работать на виртуальной машине?
    
    [Ответить](#comment-11863)
    
3.  ![](../_resources/7e6982cad97a18c4cfad6010e8465094_d18196acdf6f47799.jpg)
    
    Юрий
    
    [6 февраля, 2018 в 2:23 пп](https://losst.pro/kak-ustanovit-windows-10-na-virtualbox#comment-12639)
    
    Скажите ,а можно установленную Win10 на ноуте скопировать на VBox, установленную на нем же?
    
    [Ответить](#comment-12639)
    
    - ![](../_resources/b4f90dc03c358001ca1f97b449a88a4f_7168f7e184544a5c9.jpg)
        
        DC25
        
        [6 мая, 2018 в 5:47 пп](https://losst.pro/kak-ustanovit-windows-10-na-virtualbox#comment-16697)
        
        А зачем?
        
        [Ответить](#comment-16697)
        
    - ![](../_resources/ffd7285322f74b08584c044ba2a8bba6_57fd074aa9ab42648.jpg)
        
        Айтош
        
        [17 января, 2019 в 2:42 пп](https://losst.pro/kak-ustanovit-windows-10-na-virtualbox#comment-25328)
        
        Если кто в курсе, ответьте плиз, такой же вопрос у меня. Ключ оем вроде как
        
        [Ответить](#comment-25328)
        
4.  ![](../_resources/ecae3c02da2f13c2414b11bea24b5fe1_583c34f583464f35a.jpg)
    
    Серега
    
    [9 июня, 2018 в 6:32 дп](https://losst.pro/kak-ustanovit-windows-10-na-virtualbox#comment-19036)
    
    круто!
    
    [Ответить](#comment-19036)
    
5.  ![](../_resources/16c0c72d75f9f8e4ac3e7a1e64d63aaa_c131cf2b7a964a7a9.jpg)
    
    Сергей
    
    [10 декабря, 2018 в 11:49 пп](https://losst.pro/kak-ustanovit-windows-10-na-virtualbox#comment-24564)
    
    Добрый день!
    Спасибо за статью!
    Вопрос:
    Если указываю количество ядер для win10 больше 8, то она загружаться отказывается.
    Что посоветуете предпринять?
    
    [Ответить](#comment-24564)
    
6.  ![](../_resources/fe83ad211ed13a2a1a3c58c6af42c010_ad75e66030d54c5e8.jpg)
    
    Андрей
    
    [22 февраля, 2019 в 7:38 пп](https://losst.pro/kak-ustanovit-windows-10-na-virtualbox#comment-26008)
    
    Здравствуйте!
    Можно ли, и если можно, то как, ровно один раз выполнить описанные действия, при этом чтобы гостевая система была доступна абсолютно всем пользователям компьютера? (Linux Manjaro)
    
    [Ответить](#comment-26008)
    
    - <img width="50" height="50" src="../_resources/admin-avatar_cd40ec30f5854cf5bbdacb1e7015055f.png" class="jop-noMdConv">
        
        [admin](https://losst.pro)
        
        [23 февраля, 2019 в 8:11 дп](https://losst.pro/kak-ustanovit-windows-10-na-virtualbox#comment-26034)
        
        Можно. Если создать общую папку для всех пользователей, положить туда файлы виртуальной машины, один раз выполнить установку, а потом добавить виртуальную эту машину всем пользователям.
        
        [Ответить](#comment-26034)
        

### Оставьте комментарий

Вы вошли как mikan. [Изменить свой профиль](https://losst.pro/wp-admin/profile.php). [Выйти?](https://losst.pro/wp-login.php?action=logout&redirect_to=https%3A%2F%2Flosst.pro%2Fkak-ustanovit-windows-10-na-virtualbox&_wpnonce=5451bf6097) Обязательные поля помечены *

Комментарий

[Русский](https://losst.pro/kak-ustanovit-windows-10-na-virtualbox "Русский")

Поиск

## Поиск по командам

[](#)[<img width="340" height="118" src="../_resources/map-2_f24efceda5774225b2faf7ef27316aab.png" class="jop-noMdConv">](https://losst.pro/nachnite-izuchat-linux-pryamo-sejchas)

[](#)[<img width="340" height="118" src="../_resources/vim-3_3015a19dc8884bb7a912f4b123c1c78e.png" class="jop-noMdConv">](https://losst.pro/kak-polzovatsya-tekstovym-redaktorom-vim)

- [<img width="75" height="75" src="../_resources/Snimok-ekrana-ot-2017-09-24-17-0_c1c8cb22c7c34e209.png" class="jop-noMdConv">](https://losst.pro/komanda-chmod-linux "Команда chmod Linux")
    
    [Команда chmod Linux](https://losst.pro/komanda-chmod-linux "Команда chmod Linux")
    
    2017-10-10
    
- [<img width="75" height="75" src="../_resources/find-150x150_c4381b91a43745539516a2ecbb7b5cbb.png" class="jop-noMdConv">](https://losst.pro/komanda-find-v-linux "Команда find в Linux")
    
    [Команда find в Linux](https://losst.pro/komanda-find-v-linux "Команда find в Linux")
    
    2016-01-08
    
- [<img width="75" height="75" src="../_resources/perm1-150x150_21cc6dbc27074d22838843c61c0e5b1f.jpeg" class="jop-noMdConv">](https://losst.pro/prava-dostupa-k-fajlam-v-linux "Права доступа к файлам в Linux")
    
    [Права доступа к файлам в Linux](https://losst.pro/prava-dostupa-k-fajlam-v-linux "Права доступа к файлам в Linux")
    
    2016-10-06
    
- [<img width="75" height="75" src="../_resources/copy-keys-mean-duplicate-copying_5605bc172e9b4f66a.jpg" class="jop-noMdConv">](https://losst.pro/kopirovanie-fajlov-v-linux "Копирование файлов в Linux")
    
    [Копирование файлов в Linux](https://losst.pro/kopirovanie-fajlov-v-linux "Копирование файлов в Linux")
    
    2015-11-01
    
- [<img width="75" height="75" src="../_resources/british-cat-with-a-monocle-31869_9649527c697f40cb9.jpg" class="jop-noMdConv">](https://losst.pro/gerp-poisk-vnutri-fajlov-v-linux "Поиск текста в файлах Linux")
    
    [Поиск текста в файлах Linux](https://losst.pro/gerp-poisk-vnutri-fajlov-v-linux "Поиск текста в файлах Linux")
    
    2015-08-22
    

## Рассылка

Я прочитал(а) и принимаю [политику конфиденциальности](https://losst.pro/politika-konfidentsialnosti)

## Вконтакте

## Мета

- [Управление сайтом](https://losst.pro/wp-admin/)
- [Выйти](https://losst.pro/wp-login.php?action=logout&_wpnonce=5451bf6097)
- [Лента записей](https://losst.pro/feed)
- [Лента комментариев](https://losst.pro/comments/feed)

## Следите за нами в социальных сетях

[![](../_resources/socialtwitter_303b7e0d3a814b55afeb8ba2766845c8.png)](https://twitter.com/Losst_official) [](#)[![](../_resources/socialfb_8ace2085caba4ddd8ad3041ec3528810.png)](https://www.facebook.com/losstofficial)[![](../_resources/socialrss_7acfdd6b004249ad88706869c0d8b7de.png)](https://losst.pro/feed)[![](../_resources/socialtubmlr_07b327fb234d4a0ca4effc9a488284bc.png)](https://losstofficial.tumblr.com/)[![](../_resources/socialpath_60cf844df9ae45bd99c474bdbbeb03c7.png)](https://vk.com/losst_official)[![](../_resources/socialtelegram_a0c811d9ac36470d97557e817258bd2c.png)](https://t.me/losstofficial)[![](../_resources/socialyoutube_2ef5b9fdd45640f9a8031a330ed1ccdf.png)](https://www.youtube.com/channel/UCFhsZ0CeGEXOSpIOUORwbUA)

©Losst 2023 CC-BY-SA [Политика конфиденциальности](https://losst.pro/politika-konfidentsialnosti)

03AD1IbLABw-IFxzmJsSFFfxzR\_klQQpdqUcuDXtLjP7bV1szNVN7kd2i6XK74KyVn3t0x29HMhqrz1SEE\_qHNhbQlpBzCQu-fqq\_gdqnme\_iQdUKayO3un0PhjNdnV5VmAEY5J3cL9NMHh9k-KVFngOfK9kznNR-uwEbyB-HHCUkAQ7WPALvGEa8ayr6mLqz8WKayDGS-uksN1\_tPITcQILaL2mOECqQpN-MIQN9msc15Su\_Cr0YH11f3WnhSvwpEhF9un7WdsEbjEUfJBiTr-pCaXeHc14lffNyDA\_Dir4gLuEcjpCIMmRuZkt3fKXpTLJSf\_jkTIdHHU999WqPmhDI\_qsU-78TaDH7lyugm3XU5J9pkpyPXjEgH6KDOBjWTBEsUwFPQzNrZfzknbSrJAeVwE4iDvBaqk\_Uczb2cOlrbJx9ssVd2XiuHVwpmiweBVw93NFh5q2qrcId4i06PZwVXzP0Zs9Lt64XOSEyMhkiIC0CXqjIVGsTbrZ4zgDhOWKqC6XD7WHd8