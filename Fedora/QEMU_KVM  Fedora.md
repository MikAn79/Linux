---
tags:
  - fedora
  - виртуализация
  - kvm
  - qemu
---
#виртуализация #qemu #kvm
[[QEMU_KVM  Fedora]]
[[Fedora_Linux_Начало]]


---

### Анализ скрипта:
1. **Обновление системы**:
   ```bash
   dnf update -y
   ```
   - Команда `dnf update` работает корректно в DNF 5. Она обновляет все пакеты до последних версий.

2. **Установка пакетов**:
   ```bash
   dnf install -y @virtualization qemu-kvm libvirt virt-install virt-manager bridge-utils
   ```
   - Мета-пакет `@virtualization` включает все необходимые компоненты для работы с KVM/QEMU.
   - Пакеты `qemu-kvm`, `libvirt`, `virt-install`, `virt-manager` и `bridge-utils` поддерживаются в Fedora 42.

3. **Прочие команды**:
   - `systemctl enable libvirtd` и `systemctl start libvirtd` работают корректно, так как они относятся к systemd, а не к DNF.
   - `usermod -aG libvirt "$CURRENT_USER"` добавляет пользователя в группу `libvirt`.

---

### Возможные улучшения для DNF 5:
Хотя скрипт уже совместим с DNF 5, можно добавить следующие улучшения для лучшей совместимости и удобства:

1. **Использование `dnf check`**:
   - После установки пакетов можно выполнить проверку целостности системы:
     ```bash
     dnf check
     ```

2. **Очистка кэша DNF**:
   - Для освобождения места после установки можно очистить кэш:
     ```bash
     dnf clean all
     ```

3. **Логирование**:
   - Добавьте логирование всех операций в файл для отладки:
     ```bash
     exec > >(tee -a /var/log/kvm_install.log) 2>&1
     ```

---
[Гостевые и драйвер virio](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/virtio-win-0.1.285-1/ "гостевые и драйвера virtio")
### Обновленный скрипт с учетом DNF 5:
Вот улучшенная версия скрипта:

```bash
#!/bin/bash

# Логирование всех операций
exec > >(tee -a /var/log/kvm_install.log) 2>&1

# Проверка прав суперпользователя
if [[ $EUID -ne 0 ]]; then
  echo "Этот скрипт должен быть запущен с правами root (sudo)."
  exit 1
fi

# Проверка поддержки аппаратной виртуализации
if egrep -q 'vmx|svm' /proc/cpuinfo; then
  echo "Аппаратная виртуализация поддерживается."
else
  echo "Аппаратная виртуализация НЕ поддерживается. Убедитесь, что ваш процессор поддерживает Intel VT-x или AMD-V."
  exit 1
fi

# Обновление системы
echo "Обновление системы..."
dnf update -y

# Установка необходимых пакетов
echo "Установка QEMU/KVM и связанных пакетов..."
dnf install -y @virtualization qemu-kvm libvirt virt-install virt-manager bridge-utils

# Очистка кэша DNF
echo "Очистка кэша DNF..."
dnf clean all

# Проверка целостности системы
echo "Проверка целостности системы..."
dnf check

# Включение и запуск службы libvirtd
echo "Включение и запуск службы libvirtd..."
systemctl enable libvirtd
systemctl start libvirtd

# Добавление текущего пользователя в группу libvirt
CURRENT_USER=$(logname)
echo "Добавление пользователя '$CURRENT_USER' в группу libvirt..."
usermod -aG libvirt "$CURRENT_USER"

# Проверка статуса libvirtd
echo "Проверка статуса службы libvirtd..."
systemctl status libvirtd --no-pager

# Проверка работы KVM
if [[ -e /dev/kvm ]]; then
  echo "KVM работает корректно."
else
  echo "KVM не обнаружен. Проверьте настройки системы."
  exit 1
fi

echo "Установка QEMU/KVM завершена успешно!"
echo "Для применения изменений перезагрузите систему или выполните команду 'newgrp libvirt'."
```

---

### Изменения и улучшения:
1. **Логирование**:
   - Все действия скрипта записываются в файл `/var/log/kvm_install.log`.
2. **Очистка кэша**:
   - После установки пакетов выполняется очистка кэша DNF.
3. **Проверка целостности**:
   - Используется команда `dnf check` для проверки системы на наличие ошибок.

---

### Заключение:
Скрипт полностью совместим с DNF 5 и Fedora 42. Улучшения, такие как логирование и очистка кэша, делают его более надежным и удобным для использования. Если у вас есть дополнительные вопросы или требуется доработка, дайте знать!