## Отключение журналирования EXT4 в Linux

По умолчанию журналирование должно быть включено для всех разделов диска.

Чтобы просмотреть статус раздела, откройте *Терминал* и выполните в нём команду:

**sudo dumpe2fs /dev/sda1 | grep has_journal**

Где **sda1** – это название проверяемого раздела.

Напоминаем, что вывести список имеющихся в *Linux* разделов можно командой:

**ls -l /dev/ | grep sd**

В результате команда вернет строку-описание **Filesystem Features**, если в нем будет элемент **has_journal**, значит журналирование включено.

[![Filesystem Features](https://www.white-windows.ru/wp-content/uploads/2022/11/1.png)](https://www.white-windows.ru/wp-content/uploads/2022/11/1.png)

Чтобы его полностью отключить, выполните команду:

**sudo tune2fs -O ^has_journal /dev/sda1**

Как вариант, можно отключить только запись основных данных, разрешив системе заносить в журнал только метаданные, что также повысит производительность диска.

Для этого выполните команду:

**sudo tune2fs -o journal\_data\_writeback /dev/sda1**

Чтобы восстановить функционал по умолчанию, выполните команду:

**sudo tune2fs -o journal\_data\_ordered /dev/sda1**

[![Terminal](https://www.white-windows.ru/wp-content/uploads/2022/11/2.png)](https://www.white-windows.ru/wp-content/uploads/2022/11/2.png)