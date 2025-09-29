Sharing files with Fedora 32 using Samba is cross-platform, convenient, reliable, and performant.
[[Fedora_Linux_Начало]]
## What is ‘Samba’?

[Samba](https://www.samba.org/samba/) is a high-quality implementation of [Server Message Block protocol (SMB)](https://en.wikipedia.org/wiki/Server_Message_Block). Originally developed by Microsoft for connecting windows computers together via local-area-networks, it is now extensively used for internal network communications.

Apple used to maintain it’s own independent file sharing called “[Apple Filing Protocol (**AFP**)](https://en.wikipedia.org/wiki/Apple_Filing_Protocol)“, however in [recent times](https://appleinsider.com/articles/13/06/11/apple-shifts-from-afp-file-sharing-to-smb2-in-os-x-109-mavericks), it also has also switched to SMB.

**In this guide we provide the minimal instructions to enable:**

- Public Folder Sharing (Both Read Only and Read Write)
- User Home Folder Access

Note about this guide: The convention '**~\]$**' for a local user command prompt, and '**~\]#**' for a super user prompt will be used.

## Public Sharing Folder

Having a shared public place where authenticated users on an internal network can access files, or even modify and change files if they are given permission, can be very convenient. This part of the guide walks through the process of setting up a shared folder, ready for sharing with Samba.

Пожалуйста, обратите внимание: В этом руководстве предполагается, что папка общего доступа находится в современной файловой системе Linux; другие файловые системы, такие как NTFS или FAT32, работать не будут. Samba использует списки управления доступом POSIX (ACL).

Для тех, кто желает узнать больше о списках контроля доступа, пожалуйста, ознакомьтесь с документацией: "[Red Hat Enterprise Linux 7: руководство системного администратора: глава 5. Списки контроля доступа](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/ch-access_control_lists)", поскольку это также относится к Fedora 32.

В общем, это проблема только для тех, кто хочет предоставить общий доступ к диску или файловой системе, которые были созданы вне обычного процесса установки Fedora. (например, к внешнему жесткому диску).

*

Samba может совместно использовать пути к файловой системе, которые не поддерживают списки управления доступом POSIX, однако это выходит за рамки данного руководства.*

### Создать папку

Для этого руководства будет использоваться папка ***/ srv / public /*** для общего доступа.

> Каталог */srv/* содержит данные, относящиеся к конкретному сайту, обслуживаемые системой Red Hat Enterprise Linux. Этот каталог предоставляет пользователям информацию о расположении файлов данных для определенной службы, такой как FTP, WWW или CVS. Данные, относящиеся только к определенному пользователю, должны храниться в каталоге */home/*.
> 
> [Red Hat Enterprise Linux 7, руководство по администрированию хранилища: глава 2. Структура и обслуживание файловой системы: 2.1.1.8. Каталог /srv/](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/storage_administration_guide/ch-filesystem#s3-filesystem-srv)

**Создайте папку (выдаст сообщение об ошибке, если папка уже существует).**

~\]# mkdir --подробный /srv / общедоступный 

**Подтвердите существование папки:**

~\]$ ls --directory /srv/ public 

**Ожидаемый результат:**
*/srv / public*

### Настройка контекста безопасности файловой системы

Для получения доступа *на чтение и запись* к общей папке в этом руководстве будет использоваться контекст безопасности *public_content_rw_t* . Те, кто хочет *только для чтения*, могут использовать: *public_content_t*.

> Помечайте файлы и каталоги, созданные с помощью типа *public_content_rw_t*, чтобы предоставить им общий доступ с разрешениями на чтение и запись через vsftpd. Другие службы, такие как Apache HTTP Server, Samba и NFS, также имеют доступ к файлам, помеченным этим типом. Помните, что логические значения для каждой службы должны быть включены, прежде чем они смогут выполнять запись в файлы, помеченные этим типом.
> 
> [Red Hat Enterprise Linux 7, Руководство пользователя и администратора SELinux: глава 16. Протокол передачи файлов: 16.1. Типы: public_content_rw_t](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/selinux_users_and_administrators_guide/chap-managing_confined_services-file_transfer_protocol#sect-Managing_Confined_Services-File_Transfer_Protocol-Types)

Добавьте */srv/public* как *“public_content_rw_t”* в системный реестр настройки контекста безопасности локальной файловой системы:

**Добавить новый контекст безопасности файловой системы:**

~\]# семантика fcontext --добавить --введите public_content_rw_t "/srv/public(/.\*)?"

**

Проверьте новый контекст безопасности файловой системы безопасности:**

~\]# semanage fcontext --locallist --список 

**Ожидаемый результат: (должен включать)**
*/srv / public(/.\*)? все файлы system_u:object_r:public_content_rw_t: s0*

Теперь, когда папка добавлена в реестр контекста безопасности файловой системы локальной системы, команда **restorecon** может быть использована для ‘восстановления’ контекста в папке:

**Восстановите контекст безопасности в папке /srv/public:**

$~\]# restorecon -Rv /srv/public 

**Проверьте правильность применения контекста безопасности:**

~\]$ ls --directory --context /srv / public/

**

Ожидаемый результат:**
*unfined_u:object_r:**public_content_rw_t**: s0 /srv/ public/*

### Права пользователя

#### Создание групп общего доступа

Чтобы разрешить пользователю доступ к общедоступной папке только для *чтения* или для *чтения и записи*, создайте две новые группы, которые регулируют эти привилегии: *public_readonly* и *public_readwrite*.

Учетным записям пользователей можно предоставить доступ к *только для чтения* или *чтения и записи*, добавив свою учетную запись в соответствующую группу (и разрешив вход через Samba, создав smb-пароль). Этот процесс продемонстрирован в разделе: “Тест общедоступного доступа (localhost)”.

**Создайте группы public_readonly и public_readwrite:**

~\]# groupadd public_readonly
~\]# groupadd public_readwrite 

**Проверьте успешное создание групп:**

~\]$ доступ к группе public_readonly public_readwrite 

**Ожидаемый результат: (Примечание: * x: 1 ...:* число, вероятно, будет отличаться в вашей системе)**
*public_readonly: x:1009: public_readwrite: x: 1010:*

#### Установить разрешения

Теперь установите соответствующие разрешения пользователя для общедоступной папки:

**Установите права доступа для пользователей и групп для папки:**

~\]# chmod --подробный 2700 / srv / общедоступный 
~\]# setfacl -m group:public_readonly: r-x /srv / общедоступный 
~\]# установить facl -m по умолчанию: группа:public_readonly: r-x / srv /public 
~\]# setfacl -m group:public_readwrite:rwx /srv /public 
~\]# setfacl -m по умолчанию: group:public_readwrite:rwx /srv /public 

**Убедитесь, что разрешения пользователя применены правильно:**

~\]$ getfacl --absolute-names / srv / public 

**Ожидаемый результат:**
*файл: /srv/ общедоступный владелец: корневая группа: флаги root: -s- user==rwx group==--- group: public_readonly:r-x group:public_readwrite: rwx mask==rwx other==--- default: пользователь==rwx default: группа==--- default: группа: public_readonly: r-x default: группа:public_readwrite: rwx default :маска==rwx по умолчанию: другое==---*

### Установка

~\]# dnf установить samba

### Имя хоста (общесистемное)

Samba будет использовать имя компьютера при обмене файлами; полезно задать имя хоста, чтобы компьютер можно было легко найти в локальной сети.

**Просмотреть текущее имя хоста:**

~\] статус $ hostnamectl

Если вы хотите изменить свое имя хоста на что-то более описательное, используйте команду:

**Измените имя хоста вашей системы (пример):**

~\]# hostnamectl задайте имя хоста "simple-samba-server"

Для более полного обзора команды **hostnamectl**, пожалуйста, прочтите предыдущую статью журнала Fedora: "[Как задать имя хоста в Fedora](https://fedoramagazine.org/set-hostname-fedora/)".

### Брандмауэр

Настройка брандмауэра - сложная и трудоемкая задача. В этом руководстве будут представлены лишь минимальные манипуляции с брандмауэром, позволяющие Samba проходить через него.

Для тех, кому интересно узнать больше о настройке брандмауэров, пожалуйста, ознакомьтесь с документацией: "[Red Hat Enterprise Linux 8: Защита сетей: глава 5. Использование и настройка брандмауэра](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/securing_networks/using-and-configuring-firewalls_securing-networks)", поскольку в целом это применимо и к Fedora 32.

**Разрешить доступ Samba через брандмауэр:**

~\]# firewall-cmd --add-service=samba --постоянный
~\]# firewall-cmd --перезагрузить 

**Убедитесь, что Samba включена в ваш активный брандмауэр:**

~\]$ firewall-cmd --list-services 

**Выходные данные (должны включать):**
*samba*

### Настройка

#### Удалить конфигурацию по умолчанию

Стандартная конфигурация, входящая в комплект поставки Fedora 32, не требуется для этого простого руководства. В частности, она включает поддержку совместного использования принтеров с Samba.

Для этого руководства сделайте резервную копию конфигурации по умолчанию и создайте новый файл конфигурации с нуля.

**Создайте резервную копию существующей конфигурации Samba:**

~\]# cp --verbose --no-clobber /etc/samba/smb.conf /etc/samba/smb.conf.fedora0 

**Очистите файл конфигурации:**

~\]# > /etc/samba/smb.conf

#### Настройка Samba

Пожалуйста, обратите внимание: Этот файл конфигурации не содержит никаких глобальных определений; значения по умолчанию, предоставленные Samba, подходят для целей данного руководства.

**Отредактируйте файл конфигурации Samba с помощью Vim:**

~\]# vim /etc/samba/smb.conf

Add the following to */etc/samba/smb.conf* file:

\# smb.conf - Samba Configuration File

# The name of the share is in square brackets \[\],
#   this will be shared as //hostname/sharename

# There are a three exceptions:
#   the \[global\] section;
#   the \[homes\] section, that is dynamically set to the username;
#   the \[printers\] section, same as \[homes\], but for printers.

# path: the physical filesystem path (or device)
# comment: a label on the share, seen on the network.
# read only: disable writing, defaults to true.

# For a full list of configuration options,
#   please read the manual: "man smb.conf".

\[global\]

\[public\]
path = /srv/public
comment = Public Folder
read only = No

### Write Permission

By default Samba is not granted permission to modify any file of the system. Modify system’s security configuration to allow Samba to modify any filesystem path that has the security context of *public_content_rw_t*.

For convenience, Fedora has a built-in SELinux Boolean for this purpose called: *smbd_anon_write*, setting this to *true* will enable Samba to write in any filesystem path that has been set to the security context of *public_content_rw_t*.

For those who are wishing Samba only have a read-only access to their public sharing folder, they may choose skip this step and not set this boolean.

There are many more SELinux boolean that are available for Samba. For those who are interested, please read the documentation: "[Red Hat Enterprise Linux 7: SELinux User's and Administrator's Guide: 15.3. Samba Booleans](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/selinux_users_and_administrators_guide/sect-managing_confined_services-samba-booleans)", it also apply to Fedora 32 without any adaptation.

**Set SELinux Boolean allowing Samba to write to filesystem paths set with the security context *public_content_rw_t*:**
~\]# setsebool -P smbd_anon_write=1

**Verify bool has been correctly set:**
$ getsebool smbd_anon_write

**Expected Output:**
*smbd_anon_write --> on*

## Samba Services

The Samba service is divided into two parts that we need to start.

### Samba ‘smb’ Service

The Samba “Server Message Block” (SMB) services is for sharing files and printers over the local network.

Manual: “[smbd – server to provide SMB/CIFS services to clients](https://www.samba.org/samba/docs/current/man-html/smbd.8.html)“

### Enable and Start Services

For those who are interested in learning more about configuring, enabling, disabling, and managing services, please consider studying the documentation: "[Red Hat Enterprise Linux 7: System Administrator's Guide: 10.2. Managing System Services](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/sect-managing_services_with_systemd-services)".

**Enable and start smb and nmb services:**
~\]# systemctl enable smb.service
~\]# systemctl start smb.service

**Verify smb service:**
~\]# systemctl status smb.service

### Test Public Sharing (localhost)

To demonstrate allowing and removing access to the public shared folder, create a new user called *samba_test_user*, this user will be granted permissions first to read the public folder, and then access to read and write the public folder.

The same process demonstrated here can be used to grant access to your public shared folder to other users of your computer.

The *samba_test_user* will be created as a locked user account, disallowing normal login to the computer.

**Create 'samba_test_user**'**, and lock the account.**
~\]# useradd samba_test_user
~\]# passwd --lock samba_test_user

**Set a Samba Password for this Test User (such as 'test')**:
~\]# smbpasswd -a samba_test_user

#### Test Read Only access to the Public Share:

**Add samba_test_user to the public_readonly group:**
~\]# gpasswd --add samba_test_user public_readonly

**Login to the local Samba Service (public folder)**:
~\]$ smbclient --user=samba_test_user //localhost/public

**First, the *ls* command should succeed,
Second, the *mkdir* command should not work,
and finally, *exit*:**
smb: \\> ls
smb: \\> mkdir error
smb: \\> exit

**Remove samba_test_user from the public_readonly group:**
gpasswd --delete samba_test_user public_readonly

#### Test Read and Write access to the Public Share:

**Add samba_test_user to the public_readwrite group:**
~\]# gpasswd --add samba_test_user public_readwrite

**Login to the local Samba Service (public folder):**
~\]$ smbclient --user=samba_test_user //localhost/public

**First, the *ls* command should succeed,
Second, the *mkdir* command should work,
Third, the rmdir command should work,
and finally, *exit*:**
smb: \\> ls
smb: \\> mkdir success
smb: \\> rmdir success
smb: \\> exit

**Remove samba_test_user from the public_readwrite group:**
~\]# gpasswd --delete samba_test_user public_readwrite

After testing is completed, for security, disable the **samba_test_user**‘s ability to login in via samba.

**Disable samba_test_user login via samba:**
~\]# smbpasswd -d samba_test_user

## Home Folder Sharing

In this last section of the guide; Samba will be configured to share a user home folder.

For example: If the user bob has been registered with *smbpasswd*, bob’s home directory */home/bob*, would become the share *//server-name/bob*.

This share will only be available for bob, and no other users.

This is a very convenient way of accessing your own local files; however naturally it carries at a security risk.

### Setup Home Folder Sharing

#### Give Samba Permission for Public Folder Sharing

**Set SELinux Boolean allowing Samba to read and write to home folders:**
~\]# setsebool -P samba_enable_home_dirs=1

**Verify bool has been correctly set:**
$ getsebool samba_enable_home_dirs

**Expected Output:**
*samba_enable_home_dirs --> on*

#### Add Home Sharing to the Samba Configuration

**Append the following to the systems smb.conf file:**

\# The home folder dynamically links to the user home.

# If 'bob' user uses Samba:
# The homes section is used as the template for a new virtual share:

# \[homes\]
# ...   (various options)

# A virtual section for 'bob' is made:
# Share is modified: \[homes\] -> \[bob\]
# Path is added: path = /home/bob
# Any option within the \[homes\] section is appended.

# \[bob\]
#       path = /home/bob
# ...   (copy of various options)


# here is our share,
# same as is included in the Fedora default configuration.

\[homes\]
        comment = Home Directories
        valid users = %S, %D%w%S
        browseable = No
        read only = No
        inherit acls = Yes

#### Reload Samba Configuration

**Tell Samba to reload it's configuration:**
~\]# smbcontrol all reload-config

### Test Home Directory Sharing

**Switch to samba_test_user and create a folder in it's home directory:**
~\]# su samba_test_user
samba_test_user:~\]$ cd ~
samba_test_user:~\]$ mkdir --verbose test_folder
samba_test_user:~\]$ exit

**Enable samba_test_user to login via Samba:**
~\]# smbpasswd -e samba_test_user

**Login to the local Samba Service (samba_test_user home folder):**
$ smbclient --user=samba_test_user //localhost/samba_test_user

**Test (all commands should complete without error):**
smb: \\> ls
smb: \\> ls test_folder
smb: \\> rmdir test_folder
smb: \\> mkdir home_success
smb: \\> rmdir home_success
smb: \\> exit

**Disable samba_test_user** **from login in via Samba**:
~\]# smbpasswd -d samba_test_user