---
page-title: Managing the GRUB Menu in Fedora Distributions | Baeldung on Linux
url: https://www.baeldung.com/linux/grub-menu-management
date: 2025-02-21 13:26:53
tags:
  - kernel
  - grub
---
#kernel 
> sudo grub2-set-default 1

---

## 1\. Введение[](https://www.baeldung.com/linux/grub-menu-management#introduction)

Универсальный загрузчик ([GRUB](https://www.baeldung.com/linux/popular-bootloaders)) — это распространённый инструмент для выполнения процесса загрузки в большинстве дистрибутивов Linux. Несмотря на схожесть в использовании, методы управления им могут различаться в зависимости от дистрибутива Linux.

В этом руководстве мы рассмотрим настройку GRUB в Fedora Linux и других дистрибутивах на базе RHEL.

Возьмём в качестве примера дистрибутив Fedora, который входит в семейство Red Hat Linux. Начиная с 29-й и 30-й версий, в нём были реализованы определённые улучшения GRUB.

Сначала мы рассмотрим, как хранятся записи в меню GRUB. Обычно определения записей хранятся в файле */boot/grub2/grub.cfg* в блоках *menuentry*. Однако **в Fedora 30 используется спецификация [BootLoaderSpec](https://www.freedesktop.org/wiki/Specifications/BootLoaderSpec/) (BLS), которая требует хранить каждое определение записи в отдельном файле**. Эти файлы можно найти в папке */boot/loader/entries*. После новой установки он содержит конфигурацию GRUB нашей системы, а также аварийную конфигурацию:

```
$ sudo ls /boot/loader/entries
aa2fe2e4c83744f98664b52388fba883-0-rescue.conf
aa2fe2e4c83744f98664b52388fba883-6.2.9-300.fc38.x86_64.conf
```

Кроме того, **в Fedora 29 появилась функция автоматического скрытия меню, которая скрывает меню GRUB, когда доступно только одно ядро**. Более старые версии ядра, сохранённые, например, для аварийного восстановления, здесь не учитываются. Конечно, если мы правильно настроили [время ожидания](https://www.baeldung.com/linux/grub-menu-remove-timeout) меню GRUB, мы всё равно можем открыть меню, нажав клавишу F8.

Мы можем отключить это поведение с помощью команды [*grub2-editenv*](https://www.mankier.com/1/grub2-editenv):

```
$ sudo grub2-editenv - unset menu_auto_hide
```

Следовательно, меню GRUB отображается сразу.

## 3\. Файл конфигурации GRUB[](https://www.baeldung.com/linux/grub-menu-management#the-grub-configuration-file)

[Параметры GRUB](https://www.gnu.org/software/grub/manual/grub/grub.html) хранятся в файле */etc/default/grub*. Это подходящее место для внесения любых пользовательских изменений. Давайте проверим его содержимое:

```
$ cat /etc/default/grub
GRUB_TIMEOUT="5"
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT="saved"
GRUB_DISABLE_SUBMENU="true"
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX=""
GRUB_DISABLE_RECOVERY="true"
GRUB_ENABLE_BLSCFG="true"
```

**Обратите внимание, что для параметра *GRUB\_ENABLE\_BLSCFG* установлено значение *true*, что означает, что конфигурация соответствует спецификации BLS.**

## 4\. Команда *grub2-mkconfig*[](https://www.baeldung.com/linux/grub-menu-management#the-grub2-mkconfig-command)

Чтобы изменить конфигурацию GRUB, сначала **отредактируйте файл */etc/default/grub*. Затем с помощью команды [*grub2-mkconfig*](https://www.mankier.com/8/grub2-mkconfig) создайте новый файл */boot/grub2/grub.cfg*, используя содержимое файла */etc/default/grub***.

По умолчанию меню GRUB отображается в течение четырёх секунд. Если этот период кажется нам слишком коротким, мы можем увеличить его в конфигурации GRUB. В качестве примера установим время ожидания 10 секунд. Для этого нам нужно изменить значение параметра *GRUB\_TIMEOUT*:

```
$ sudo nano /etc/default/grub
# ...
GRUB_TIMEOUT="10"
```

Далее давайте обновим файл *grub.cfg*:

```
$ sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

Параметр *\-o* предоставляет имя выходного файла.

## 5\. Установка ядра по умолчанию[](https://www.baeldung.com/linux/grub-menu-management#setting-the-default-kernel)

Давайте взглянем на запись *GRUB\_DEFAULT=”saved”*. Это значение говорит о том, что по умолчанию используется последнее установленное ядро. Кроме того, мы можем изменить ядро, указанное как *“saved”*, с помощью команды *grub2-set-default*. Нам нужно ввести индекс, начиная с нуля, который для второй записи равен 1:

```
$ sudo grub2-set-default 1
```

Мы можем изменить это значение по умолчанию, указав нужное имя записи с помощью ключа *GRUB\_DEFAULT* в */etc/default/grub*:

```
GRUB_DEFAULT="Fedora Linux (6.2.9-300.fc38.x86_64) 38 (Workstation Edition)"
```

Чтобы найти имя записи в рамках BLS, давайте [*проверим*](https://www.baeldung.com/linux/common-text-search) файлы конфигурации в папке */boot/loader/entries* на наличие ключа *title*:

```
$ sudo sh -c 'grep title /boot/loader/entries/* | cut -d " " -f2-'
Fedora Linux (0-rescue-aa2fe2e4c83744f98664b52388fba883) 38 (Workstation Edition)
Fedora Linux (6.2.9-300.fc38.x86_64) 38 (Workstation Edition)
```

Чтобы изменения вступили в силу, давайте создадим новый файл конфигурации GRUB:

```
$ sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

The [*grubby*](https://linux.die.net/man/8/grubby) command comes with the Fedora distribution, so we don’t need to install it. **With *grubby*, we can modify, add, and remove GRUB menu entries.** The command accepts a bunch of options to perform various tasks. *grubby* makes persistent changes to the GRUB configuration.

Let’s list all GRUB entries together with their parameters by employing the *–info=ALL* option:

```
$ sudo grubby --info=ALL
index=0
kernel="/boot/vmlinuz-6.2.9-300.fc38.x86_64"
args="ro rootflags=subvol=root rhgb quiet"
root="UUID=f9a12274-d915-4f46-860e-08b3b8f0ea35"
initrd="/boot/initramfs-6.2.9-300.fc38.x86_64.img"
title="Fedora Linux (6.2.9-300.fc38.x86_64) 38 (Workstation Edition)"
id="aa2fe2e4c83744f98664b52388fba883-6.2.9-300.fc38.x86_64"
index=1
kernel="/boot/vmlinuz-0-rescue-aa2fe2e4c83744f98664b52388fba883"
args="ro rootflags=subvol=root rhgb quiet"
root="UUID=f9a12274-d915-4f46-860e-08b3b8f0ea35"
initrd="/boot/initramfs-0-rescue-aa2fe2e4c83744f98664b52388fba883.img"
title="Fedora Linux (0-rescue-aa2fe2e4c83744f98664b52388fba883) 38 (Workstation Edition)"
id="aa2fe2e4c83744f98664b52388fba883-0-rescue"
```

First, note that we’re provided with the kernel’s index. Next, we see our system’s kernel entry. *vmlinuz-6.2.9-300.fc38.x86\_64*, and the rescue entry with the *vmlinuz-0-rescue-aa2fe2e4c83744f98664b52388fba883* kernel. Both kernels are in the same */boot* directory.

Next, the *id* key provides the name of the entry’s configuration file in the */boot/loader/entries* folder.

Now let’s check which entry is the default one with the *–default-index* option:

```
$ sudo grubby --default-index
0
```

Next, let’s find the corresponding kernel with the *–default-kernel* switch:

```
$ sudo grubby --default-kernel
/boot/vmlinuz-6.2.9-300.fc38.x86_64
```

Finally, we can obtain the default kernel’s title with *–default-title*:

```
$ sudo grubby --default-title
Fedora Linux (6.2.9-300.fc38.x86_64) 38 (Workstation Edition)
```

## 7\. More Actions With *grubby*[](https://www.baeldung.com/linux/grub-menu-management#more-actions-with-grubby)

The *grubby* actions are fired by passing the appropriate options to the command. If we need to specify the entry we want to print, modify, or delete, we can use the kernel name or entry index. In addition, the keywords *DEFAULT* and *ALL* refer to the default kernel and all kernels, respectively.

### 7.1. Adding and Removing Kernel Arguments[](https://www.baeldung.com/linux/grub-menu-management#1-adding-and-removing-kernel-arguments)

**With the *–args* and *–remove-arg* options, we can respectively add and remove kernel arguments.** Subsequently, we need the *–update-kernel* switch to point to the target kernel. As an example, let’s allow the boot messages on the screen. So, we need to remove the *rhgb* *quiet* combination to hide the splash screen:

```
$ sudo grubby --remove-args="rhgb quiet" --update-kernel /boot/vmlinuz-6.2.9-300.fc38.x86_64
```

Afterward, let’s review the changes:

```
$ sudo grubby --info vmlinuz-6.2.9-300.fc38.x86_64
index=0
kernel="/boot/vmlinuz-6.2.9-300.fc38.x86_64"
args="ro rootflags=subvol=root"
# ...
```

As this kernel is the default one, we can achieve the same effect using *DEFAULT*:

```
$ sudo grubby --remove-args="rhgb quiet" --update-kernel DEFAULT
```

Finally, let’s use the entry’s index as well:

```
$ sudo grubby --remove-args="rhgb quiet" --update-kernel 0
```

### 7.2. Bulk Changes[](https://www.baeldung.com/linux/grub-menu-management#2-bulk-changes)

**We can modify the arguments of all available kernels with *–arg* and *–remove-arg* options combined with *–update-kernel=ALL*.** So, let’s disable Fedora’s splash screen for all kernels:

```
$ sudo grubby --remove-args="rhgb quiet" --update-kernel=ALL
```

Now, let’s check all kernels:

```
$ sudo grubby --info=ALL
index=0
kernel="/boot/vmlinuz-6.2.9-300.fc38.x86_64"
args="ro rootflags=subvol=root"
# ...
index=1
kernel="/boot/vmlinuz-0-rescue-aa2fe2e4c83744f98664b52388fba883"
args="ro rootflags=subvol=root"
# ...
```

Finally, let’s restore the splash screen with *–args*:

```
$ sudo grubby --args="rhgb quiet" --update-kernel=ALL
```

### 7.3. Changing the Default Kernel[](https://www.baeldung.com/linux/grub-menu-management#3-changing-the-default-kernel)

**To set the default kernel, we can use the *–set-default* switch.** As a reference, we can use the kernel name or index as well. So, let’s set the rescue kernel as the default one:

```
$ sudo grubby --set-default=/boot/vmlinuz-0-rescue-aa2fe2e4c83744f98664b52388fba883
The default is /boot/loader/entries/aa2fe2e4c83744f98664b52388fba883-0-rescue.conf with index 1 and kernel /boot/vmlinuz-0-rescue-aa2fe2e4c83744f98664b52388fba883
```

Next, let’s make the entry of index 0 the default one with *–set-default-index*:

```
$ sudo grubby --set-default-index=0
The default is /boot/loader/entries/aa2fe2e4c83744f98664b52388fba883-6.2.9-300.fc38.x86_64.conf with index 0 and kernel /boot/vmlinuz-6.2.9-300.fc38.x86_64
```

### 7.4. Booting Into Text Mode[](https://www.baeldung.com/linux/grub-menu-management#4-booting-into-text-mode)

**A typical case when we’re working with GRUB is enabling [text mode](https://www.baeldung.com/linux/boot-linux-command-line-mode).** Thus, we need to add *3* to the argument list. Let’s do that for the rescue kernel:

```
$ sudo grubby --args="3" --update-kernel /boot/vmlinuz-0-rescue-aa2fe2e4c83744f98664b52388fba883
```

As usual, let’s check the result:

```
$ sudo grubby --info /boot/vmlinuz-0-rescue-aa2fe2e4c83744f98664b52388fba883
index=1
kernel="/boot/vmlinuz-0-rescue-aa2fe2e4c83744f98664b52388fba883"
args="ro rootflags=subvol=root rhgb quiet 3"
# ...
```

In this example, we have the splash screen, but we end up in the text console.

### 7.5. Adding New Entries[](https://www.baeldung.com/linux/grub-menu-management#5-adding-new-entries)

With the *grubby* command, we can add or remove entries in the GRUB menu. So, let’s add the text mode entry permanently with the *–add-kernel* option:

```
$ sudo sh -c 'grubby --add-kernel $(grubby --default-kernel) --copy-default --args="3" --title "The text-mode kernel"'
An entry for kernel 6.2.9-300.fc38.x86_64 already exists, adding /boot/loader/entries/aa2fe2e4c83744f98664b52388fba883-6.2.9-300.fc38.x86_64.0~custom.conf
```

First, let’s take a close look at the command construction. The argument to *–add-kernel* is the path to the kernel, to which we want to add a new entry. We obtained it with the grubby *–default-kernel* invocation. Of course, we can do it explicitly with *–add-kernel vmlinuz-6.2.9-300.fc38.x86\_64*.

Next comes the *–copy-defaults* option, which copies as many settings as possible from the current default kernel. Then, we’re adding the *3* text mode argument to the new kernel. Finally, we set the title for the new entry.

Additionally, we could make the new entry the default one by means of the *–make-default* switch.

Then, let’s examine the results of this command by listing all kernels:

```
$ sudo grubby --info=ALL
index=0
kernel="/boot/vmlinuz-6.2.9-300.fc38.x86_64"
args="ro rootflags=subvol=root rhgb quiet 3"
root="UUID=f9a12274-d915-4f46-860e-08b3b8f0ea35"
initrd="/boot/initramfs-6.2.9-300.fc38.x86_64.img"
title="The text-mode kernel"
id="aa2fe2e4c83744f98664b52388fba883-6.2.9-300.fc38.x86_64.0~custom"
index=1
kernel="/boot/vmlinuz-6.2.9-300.fc38.x86_64"
args="ro rootflags=subvol=root rhgb quiet"
root="UUID=f9a12274-d915-4f46-860e-08b3b8f0ea35"
initrd="/boot/initramfs-6.2.9-300.fc38.x86_64.img"
title="Fedora Linux (6.2.9-300.fc38.x86_64) 38 (Workstation Edition)"
id="aa2fe2e4c83744f98664b52388fba883-6.2.9-300.fc38.x86_64"
# ...
```

Let’s observe that the new kernel has been added at the first position of the list. We’re going to check if it has automatically become the default one. Let’s recall that, so far, the default one was the kernel with the index 0:

```
$ grubby --default-index
1
```

So, the default kernel’s index has been updated accordingly and points to the correct entry.

### 7.6. Removing Kernel Entries[](https://www.baeldung.com/linux/grub-menu-management#6-removing-kernel-entries)

By passing the index to the *–remove-kernel* option, we can remove the corresponding entry. So, let’s get rid of the text mode entry of index 0:

```
$ sudo grubby --remove-kernel=0
```

Then, let’s verify that the entries have been reindexed:

```
$ sudo grubby --info=ALL
index=0
kernel="/boot/vmlinuz-6.2.9-300.fc38.x86_64"
args="ro rootflags=subvol=root rhgb quiet"
# ...
index=1
kernel="/boot/vmlinuz-0-rescue-aa2fe2e4c83744f98664b52388fba883"
# ...
```

Therefore, if we’re about to remove more entries, we should always check new indices.

**The *–remove-kernel* option accepts the kernel name as an argument, too. However, we should be extremely cautious when using this approach because all entries that use the same kernel would be removed.**

## 8\. Conclusion[](https://www.baeldung.com/linux/grub-menu-management#conclusion)

In this article, we reviewed Fedora’s and other RHEL-based distributions’ way of maintaining the GRUB menu. First, we talked about the specifics of GRUB configuration in RHEL-based Linux. Next, we learned the *grub2-mkconfig* command, which applies custom changes to the GRUB configuration.

Next, we focused on the *grubby* command, designed to fully manage the GRUB menu. We used it to modify kernel arguments, set the default kernel, and create custom configurations.