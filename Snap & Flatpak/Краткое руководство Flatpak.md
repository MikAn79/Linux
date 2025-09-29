# Краткое руководство Flatpak

**Flatpak** — система для создания, распространения и запуска изолированных настольных приложений в Linux. Приложения устанавливаются независимо от хост-системы и изолированы во время выполнения.

---

## Оглавление
- [Установка Flatpak](#установка-flatpak)
- [Работа с репозиториями](#работа-с-репозиториями)
- [Поиск и установка приложений](#поиск-и-установка-приложений)
- [Управление приложениями](#управление-приложениями)
- [Разрешения приложений](#разрешения-приложений)
- [Графический интерфейс](#графический-интерфейс)
- [Известные проблемы](#известные-проблемы)
- [Полезные ссылки](#полезные-ссылки)

---

## Установка Flatpak

```bash
# apt-get install flatpak
```

Для работы от непривилегированного пользователя добавьте его в группу **fuse**:

```bash
# gpasswd -a USER fuse
```
`USER` — имя вашего пользователя.

---

## Работа с репозиториями

### Добавление репозитория

```bash
$ flatpak remote-add name_repository url
```
- `name_repository` — название репозитория
- `url` — адрес репозитория

Пример:

```bash
$ flatpak remote-add flathub https://flathub.org/repo/flathub.flatpakrepo
```

> **Заметка:** При подключении репозитория от пользователя из группы **wheel** будет запрошен пароль **root**.

После подключения обновите данные:

```bash
$ flatpak update
```

### Удаление репозитория

```bash
$ flatpak remote-delete name_repository
```

### Список подключенных репозиториев

```bash
$ flatpak remotes
```

---

## Поиск и установка приложений

### Поиск пакетов

Перед поиском обновите данные:

```bash
$ flatpak update
$ flatpak search name_package
```
`name_package` — название пакета.

### Список пакетов в репозитории

```bash
$ flatpak remote-ls name_repository
```

### Установка приложения

```bash
$ flatpak install name_repository name_package
```
Пример:

```bash
$ flatpak install flathub firefox
```

> Если пакет содержит несколько версий, появится меню выбора.

Файлы размещаются по адресу: `~/.local/share/flatpak`

---

## Управление приложениями

### Список установленных приложений

```bash
$ flatpak list
```

### Запуск приложения

```bash
$ flatpak run name_package
```

### Обновление приложения

```bash
$ flatpak update name_package
```

### Удаление приложения

```bash
$ flatpak uninstall name_package
```

### Удаление неиспользуемых пакетов

```bash
$ flatpak uninstall --unused
```

---

## Разрешения приложений

Flatpak использует песочницу для ограничения доступа приложений.

### Просмотр разрешений

1. Узнайте ID приложения:
    ```bash
    $ flatpak list | grep name_package
    ```
2. Посмотрите разрешения:
    ```bash
    $ flatpak info --show-permissions application_id
    ```

[Список параметров разрешений](https://docs.flatpak.org/en/latest/sandbox-permissions-reference.html)

### Изменение разрешений

```bash
$ flatpak override permission_option application_id
```
Пример:

```bash
$ flatpak override --device=dri org.mozilla.firefox
```

### Сброс разрешений

```bash
$ flatpak override --reset application_id
```

---

## Графический интерфейс

Для управления Flatpak используйте “Центр программ” **Discover**.

- В настройках: **Discover → Добавить репозиторий flathub**
- Можно использовать [web-интерфейс Flathub](https://flathub.org/apps)

---

## Известные проблемы

- Для установки приложений от непривилегированного пользователя добавьте его в группу **fuse**:
    ```bash
    # gpasswd -a USER fuse
    ```
- После установки через терминал, чтобы приложения появились в меню, перелогиньтесь.

### Частые ошибки

- **error remote “flathub” not found**  
  Нет доступного репозитория — добавьте его:
    ```bash
    $ flatpak remote-add flathub https://flathub.org/repo/flathub.flatpakrepo
    ```

- **error: Nothing matches io.brackets.Brackets.flatpakref in remote flathub**  
  Неправильное название файла ярлыка.  
  Уберите из имени `.flatpakref`.

- **Could not unmount revokefs-fuse filesystem / Failed to execute child process fusermount (Permission denied)**  
  Нет прав на монтирование файловой системы:
    ```bash
    # control fusermount wheelonly
    ```

---

## Полезные ссылки

- [Документация Flatpak](https://docs.flatpak.org/en/latest/index.html)