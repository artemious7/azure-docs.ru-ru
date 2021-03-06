---
title: Создание и передача VHD Red Hat Enterprise Linux для использования в Azure Stack | Документация Майкрософт
description: Узнайте, как создать и передать виртуальный жесткий диск (VHD) Azure, содержащий операционную систему RedHat Linux.
services: azure-stack
documentationcenter: ''
author: JeffGoldner
manager: BradleyB
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: jeffgo
ms.openlocfilehash: d24902b894a632e9fe8c57f2fb2b652b44ab128c
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2018
ms.locfileid: "41946510"
---
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure-stack"></a>Подготовка виртуальной машины на основе Red Hat для Azure Stack

В этой статье вы узнаете, как подготовить виртуальную машину Red Hat Enterprise Linux (RHEL) для использования в Azure Stack. В статье рассматривается RHEL 7.1 и более поздние версии. Низкоуровневые оболочки для подготовки, о которых идет речь в этой статье, — это Hyper-V, Kernel-based Virtual Machine (KVM) и VMware.

Сведения о поддержке Red Hat Enterprise Linux приведены в статье о [часто задаваемых вопросах о Red Hat и Azure Stack](https://access.redhat.com/articles/3413531).

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Подготовка виртуальной машины под управлением Red Hat в диспетчере Hyper-V

В этом разделе предполагается, что вы уже получили ISO-файл с веб-сайта Red Hat и установили образ RHEL на виртуальный жесткий диск. Дополнительные сведения о том, как использовать диспетчер Hyper-V для установки образа операционной системы, см. в статье об [установке Hyper-V и настройке виртуальной машины](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="rhel-installation-notes"></a>Замечания по установке RHEL

* Формат VHDX не поддерживается в Azure Stack. В Azure поддерживается только формат фиксированного виртуального жесткого диска. Вы можете преобразовать диск в формат VHD с помощью диспетчера Hyper-V или командлета convert-vhd. Если вы используете VirtualBox, при создании диска выберите **фиксированный размер** вместо динамически выделяемого (по умолчанию).
* В Azure Stack поддерживаются только виртуальные машины поколения 1. Виртуальную машину первого поколения можно преобразовать из формата VHDX в формат VHD, а также переключить с динамически расширяемого диска на диск фиксированного размера. Вы не можете изменить поколение виртуальной машины. Дополнительные сведения см. в статье [Необходимо создать виртуальную машину поколения 1 или 2 в Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).
* Максимально допустимый размер виртуального жесткого диска составляет 1023 ГБ.
* При установке операционной системы Linux рекомендуется использовать стандартные разделы, а не диспетчер логических томов (LVM), который часто используется по умолчанию при установке. Это позволит избежать конфликта имен LVM при клонировании виртуальных машин, особенно если нужно подключить диск с OC к другой идентичной виртуальной машине в целях устранения неполадок.
* Требуется поддержка ядра для подключения файловых систем UDF. При первой загрузке UDF-носитель, подключенный к гостевой машине, передает конфигурацию подготовки в виртуальную машину Linux. Агент Azure Linux должен подключить файловую систему UDF для считывания конфигурации и подготовки виртуальной машины.
* Не настраивайте раздел swap на диске операционной системы. Вы можете настроить агент Linux для создания файла подкачки на временном диске ресурсов. Дополнительные сведения описаны ниже.
* Размер виртуальной памяти всех виртуальных жестких дисков в Azure должен быть округлен до 1 МБ. Перед конвертацией диска RAW в формат VHD убедитесь, что размер диска RAW в несколько раз превышает 1 МБ. Дополнительные сведения можно найти в инструкциях ниже.
* В Azure Stack не поддерживается cloud-init. На виртуальной машине должна быть настроена поддерживаемая версия агента Windows Azure Linux (WALA).

### <a name="prepare-a-rhel-7-virtual-machine-from-hyper-v-manager"></a>Подготовка виртуальной машины RHEL 7 в диспетчере Hyper-V

1. В диспетчере Hyper-V выберите виртуальную машину.

1. Щелкните **Подключение** , чтобы открыть окно консоли для виртуальной машины.

1. Создайте или измените файл `/etc/sysconfig/network`, добавив следующий текст:

    ```sh
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. Создайте или измените файл `/etc/sysconfig/network-scripts/ifcfg-eth0`, добавив следующий текст в соответствии с потребностями:

    ```sh
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

1. Убедитесь, что сетевая служба запускается во время загрузки, выполнив следующую команду:

    ```bash
    sudo systemctl enable network
    ```

1. Зарегистрируйте подписку Red Hat, чтобы активировать установку пакетов из репозитория RHEL, запустив следующую команду:

    ```bash
    sudo subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. Измените строку загрузки ядра в конфигурации grub, чтобы включить дополнительные параметры ядра для Azure. Для этого откройте файл `/etc/default/grub` в текстовом редакторе и измените параметр `GRUB_CMDLINE_LINUX`. Например: 

    ```sh
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

   Эта команда гарантирует отправку всех сообщений консоли на первый последовательный порт, что может помочь службе поддержки Azure при отладке. Также будут отключены новые соглашения об именовании RHEL 7 для сетевых карт.

   Графическая и "тихая" загрузка бесполезны в облачной среде, в которой нам нужно, чтобы все журналы отправлялись на последовательный порт. При желании можно оставить параметр `crashkernel`. Обратите внимание, что этот параметр сокращает объем доступной памяти на виртуальной машине на 128 MБ и более. Это может оказаться проблемой для виртуальных машин небольшого размера. Мы рекомендуем удалить следующие параметры:

    ```sh
    rhgb quiet crashkernel=auto
    ```

1. После внесения изменений в файл `/etc/default/grub`выполните следующую команду, чтобы повторно создать конфигурацию grub:

    ```bash
    sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

1. Убедитесь, что SSH-сервер установлен и настроен для включения во время загрузки (что обычно задано по умолчанию). Измените файл `/etc/ssh/sshd_config` , добавив в него следующую строку:

    ```sh
    ClientAliveInterval 180
    ```

1. Пакет WALinuxAgent `WALinuxAgent-<version>` был отправлен в репозиторий дополнений Red Hat. Включите репозиторий дополнений, выполнив следующую команду:

    ```bash
    subscription-manager repos --enable=rhel-7-server-extras-rpms
    ```

1. Установите агент Linux для Azure, выполнив следующую команду:

    ```bash
    sudo yum install WALinuxAgent
    sudo systemctl enable waagent.service
    ```

1. Не создавайте пространство подкачки на диске ОС.

    Агент Linux для Azure может автоматически настраивать размер области подкачки с использованием локального диска ресурсов, подключенного к виртуальной машине после ее подготовки в Azure. Локальный диск ресурсов является временным и должен быть очищен при отмене подготовки виртуальной машины. После установки агента Linux для Azure (см. предыдущий шаг) измените соответствующим образом следующие параметры в файле `/etc/waagent.conf`:

    ```sh
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    #NOTE: set this to whatever you need it to be.
    ```

1. Чтобы отменить регистрацию подписки, выполните следующую команду:

    ```bash
    sudo subscription-manager unregister
    ```

1. Если вы используете систему, развернутую с помощью центра сертификации предприятия, виртуальная машина RHEL не будет доверять корневому сертификату Azure Stack. Необходимо поместить его в доверенное корневое хранилище. См. сведения о [добавлении доверенных корневых сертификатов на сервер](https://manuals.gfi.com/en/kerio/connect/content/server-configuration/ssl-certificates/adding-trusted-root-certificates-to-the-server-1605.html).

1. Выполните следующие команды, чтобы отменить подготовку виртуальной машины и подготовить ее в Azure:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

1. В диспетчере Hyper-V выберите **Действие** > **Завершение работы**.

1. Преобразуйте виртуальный жесткий диск в VHD фиксированного размера с помощью либо функции "Изменить диск" диспетчера Hyper-V, либо команды Convert-VHD PowerShell. Виртуальный жесткий диск Linux готов к передаче в Azure.

## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Подготовка виртуальной машины под управлением Red Hat в KVM

1. Скачайте образ KVM RHEL 7 с веб-сайта Red Hat. В примере используется RHEL 7.

1. Задайте пароль пользователя root.

    Создайте зашифрованный пароль и скопируйте результат выполнения команды:

    ```bash
    openssl passwd -1 changeme
    ```

   Установите корневой пароль с помощью Guestfish:

    ```sh
    guestfish --rw -a <image-name>
    > <fs> run
    > <fs> list-filesystems
    > <fs> mount /dev/sda1 /
    > <fs> vi /etc/shadow
    > <fs> exit
    ```

   Во втором поле привилегированного пользователя замените "!!" зашифрованным паролем.

1. Создайте виртуальную машину в KVM из образа qcow2. Укажите тип диска **qcow2** и задайте для модели устройства интерфейса виртуальной сети значение **virtio**. Затем запустите виртуальную машину и войдите в систему как привилегированный пользователь.

1. Создайте или измените файл `/etc/sysconfig/network`, добавив следующий текст:

    ```sh
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. Создайте или измените файл `/etc/sysconfig/network-scripts/ifcfg-eth0`, добавив следующий текст:

    ```sh
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

1. Убедитесь, что сетевая служба запускается во время загрузки, выполнив следующую команду:

    ```bash
    sudo systemctl enable network
    ```

1. Зарегистрируйте подписку Red Hat, чтобы активировать установку пакетов из репозитория RHEL, запустив следующую команду:

    ```bash
    subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. Измените строку загрузки ядра в конфигурации grub, чтобы включить дополнительные параметры ядра для Azure. Для этого откройте файл `/etc/default/grub` в текстовом редакторе и измените параметр `GRUB_CMDLINE_LINUX`. Например: 

    ```sh
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

   Эта команда также гарантирует отправку всех сообщений консоли на первый последовательный порт, что может помочь технической поддержке Azure в плане отладки. Также будут отключены новые соглашения об именовании RHEL 7 для сетевых карт.

   Графическая и "тихая" загрузка бесполезны в облачной среде, где все журналы отправляются на последовательный порт. При желании можно оставить параметр `crashkernel`. Этот параметр сокращает объем доступной памяти на виртуальной машине на 128 MБ и более. Это может оказаться проблемой для виртуальных машин небольшого размера. Мы рекомендуем удалить следующие параметры:

    ```sh
    rhgb quiet crashkernel=auto
    ```

1. После внесения изменений в файл `/etc/default/grub`выполните следующую команду, чтобы повторно создать конфигурацию grub:

    ```bash
    grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

1. Добавьте модули Hyper-V в initramfs.

    Измените файл `/etc/dracut.conf` , добавив в него следующее содержимое:

    ```sh
    add_drivers+="hv_vmbus hv_netvsc hv_storvsc"
    ```

    Повторно создайте initramfs:

    ```bash
    dracut -f -v
    ```

1. Удаление cloud-init:

    ```bash
    yum remove cloud-init
    ```

1. Убедитесь, что SSH-сервер установлен и настроен для включения во время загрузки:

    ```bash
    systemctl enable sshd
    ```

    Измените файл /etc/ssh/sshd_config, включив в него следующие строки:

    ```sh
    PasswordAuthentication yes
    ClientAliveInterval 180
    ```

1. Пакет WALinuxAgent `WALinuxAgent-<version>` был отправлен в репозиторий дополнений Red Hat. Включите репозиторий дополнений, выполнив следующую команду:

    ```bash
    subscription-manager repos --enable=rhel-7-server-extras-rpms
    ```

1. Установите агент Linux для Azure, выполнив следующую команду:

    ```bash
    yum install WALinuxAgent
    ```

    Включите службу waagent:

    ```bash
    systemctl enable waagent.service
    ```

1. Не создавайте пространство подкачки на диске ОС.

    Агент Linux для Azure может автоматически настраивать размер области подкачки с использованием локального диска ресурсов, подключенного к виртуальной машине после ее подготовки в Azure. Локальный диск ресурсов является временным и должен быть очищен при отмене подготовки виртуальной машины. После установки агента Linux для Azure (см. предыдущий шаг) измените соответствующим образом следующие параметры в файле `/etc/waagent.conf`:

    ```sh
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    #NOTE: set this to whatever you need it to be.
    ```

1. Отмените регистрацию подписки (при необходимости), выполнив следующую команду:

    ```bash
    subscription-manager unregister
    ```

1. Если вы используете систему, развернутую с помощью центра сертификации предприятия, виртуальная машина RHEL не будет доверять корневому сертификату Azure Stack. Необходимо поместить его в доверенное корневое хранилище. См. сведения о [добавлении доверенных корневых сертификатов на сервер](https://manuals.gfi.com/en/kerio/connect/content/server-configuration/ssl-certificates/adding-trusted-root-certificates-to-the-server-1605.html).

1. Выполните следующие команды, чтобы отменить подготовку виртуальной машины и подготовить ее в Azure:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

1. Остановите работу виртуальной машины в KVM.

1. Конвертируйте образ qcow2 в формат VHD.

    > [!NOTE]
    > В команде qemu-img версии 2.2.1 и более поздних версиях есть ошибка, которая приводит к созданию неверного формата VHD. Эта проблема устранена в QEMU версии 2.6. Рекомендуется использовать qemu-img версии 2.2.0 или более ранних версий. Либо выполните обновление до версии 2.6 или выше. Справочные материалы: https://bugs.launchpad.net/qemu/+bug/1490611.

    Для начала конвертируйте образ в формат RAW:

    ```bash
    qemu-img convert -f qcow2 -O raw rhel-7.4.qcow2 rhel-7.4.raw
    ```

    Убедитесь, что размер образа в формате RAW соответствует 1 МБ. Если это не так, округлите размер до 1 МБ:

    ```bash
    MB=$((1024*1024))
    size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
    gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
    rounded_size=$((($size/$MB + 1)*$MB))
    qemu-img resize rhel-7.4.raw $rounded_size
    ```

    Конвертируйте диск в формате RAW в виртуальный жесткий диск фиксированного размера:

    ```bash
    qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

    Или с помощью qemu версии **2.6 или выше** добавьте параметр `force_size`:

    ```bash
    qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Подготовка виртуальной машины под управлением Red Hat в VMware

В этом разделе предполагается, что вы уже установили виртуальную машину RHEL в VMWare. Дополнительные сведения об установке операционной системы на виртуальной машине VMware см. [здесь](http://partnerweb.vmware.com/GOSIG/home.html).

* При установке операционной системы Linux мы рекомендуем использовать стандартные разделы, а не LVM (который зачастую используется по умолчанию при установке). Это позволит избежать конфликта имен LVM при клонировании виртуальных машин, особенно если диск с OC может быть подключен к другой идентичной виртуальной машине в целях устранения неполадок. При желании на дисках с данными можно использовать LVM или RAID.
* Не настраивайте раздел swap на диске операционной системы. Вы можете настроить агент Linux для создания файла подкачки на временном диске с ресурсами. Дополнительные сведения приведены ниже.
* При создании виртуального жесткого диска выберите параметр **Store virtual disk as a single file**(Сохранять виртуальный диск как один файл).

### <a name="prepare-a-rhel-7-virtual-machine-from-vmware"></a>Подготовка виртуальной машины RHEL 7 в VMware

1. Создайте или измените файл `/etc/sysconfig/network`, добавив следующий текст:

    ```sh
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. Создайте или измените файл `/etc/sysconfig/network-scripts/ifcfg-eth0`, добавив следующий текст:

    ```sh
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

1. Убедитесь, что сетевая служба запускается во время загрузки, выполнив следующую команду:

    ```bash
    sudo chkconfig network on
    ```

1. Зарегистрируйте подписку Red Hat, чтобы активировать установку пакетов из репозитория RHEL, запустив следующую команду:

    ```bash
    sudo subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. Измените строку загрузки ядра в конфигурации grub, чтобы включить дополнительные параметры ядра для Azure. Для этого откройте файл `/etc/default/grub` в текстовом редакторе и измените параметр `GRUB_CMDLINE_LINUX`. Например: 

    ```sh
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

    Это также гарантирует отправку всех сообщений консоли на первый последовательный порт, что может помочь технической поддержке Azure в плане отладки. Также будут отключены новые соглашения об именовании RHEL 7 для сетевых карт. В дополнение к вышесказанному мы рекомендуем удалить следующие параметры:

    ```sh
    rhgb quiet crashkernel=auto
    ```

    Графическая и "тихая" загрузка бесполезны в облачной среде, в которой нам нужно, чтобы все журналы отправлялись на последовательный порт. При желании можно оставить параметр `crashkernel`. Обратите внимание, что этот параметр сокращает объем доступной памяти на виртуальной машине на 128 MБ и более. Это может оказаться проблемой для виртуальных машин небольшого размера.

1. После внесения изменений в файл `/etc/default/grub`выполните следующую команду, чтобы повторно создать конфигурацию grub:

    ```bash
    sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

1. Добавьте модули Hyper-V в initramfs.

    Измените файл `/etc/dracut.conf`, добавив в него следующее содержимое:

    ```sh
    add_drivers+="hv_vmbus hv_netvsc hv_storvsc"
    ```

    Повторно создайте initramfs:

    ```bash
    dracut -f -v
    ```

1. Убедитесь, что SSH-сервер установлен и настроен для включения во время загрузки. Обычно этот параметр задан по умолчанию. Измените файл `/etc/ssh/sshd_config` , добавив в него следующую строку:

    ```sh
    ClientAliveInterval 180
    ```

1. Пакет WALinuxAgent `WALinuxAgent-<version>` был отправлен в репозиторий дополнений Red Hat. Включите репозиторий дополнений, выполнив следующую команду:

    ```bash
    subscription-manager repos --enable=rhel-7-server-extras-rpms
    ```

1. Установите агент Linux для Azure, выполнив следующую команду:

    ```bash
    sudo yum install WALinuxAgent
    sudo systemctl enable waagent.service
    ```

1. Не создавайте пространство подкачки на диске ОС.

    Агент Linux для Azure может автоматически настраивать размер области подкачки с использованием локального диска ресурсов, подключенного к виртуальной машине после ее подготовки в Azure. Следует отметить, что локальный диск ресурсов является временным и должен быть очищен при отмене подготовки виртуальной машины. После установки агента Linux для Azure (см. предыдущий шаг) измените соответствующим образом следующие параметры в файле `/etc/waagent.conf`:

    ```sh
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    NOTE: set this to whatever you need it to be.
    ```

1. Чтобы отменить регистрацию подписки, выполните следующую команду:

    ```bash
    sudo subscription-manager unregister
    ```

1. Если вы используете систему, развернутую с помощью центра сертификации предприятия, виртуальная машина RHEL не будет доверять корневому сертификату Azure Stack. Необходимо поместить его в доверенное корневое хранилище. См. сведения о [добавлении доверенных корневых сертификатов на сервер](https://manuals.gfi.com/en/kerio/connect/content/server-configuration/ssl-certificates/adding-trusted-root-certificates-to-the-server-1605.html).

1. Выполните следующие команды, чтобы отменить подготовку виртуальной машины и подготовить ее в Azure:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

1. Завершите работу виртуальной машины и конвертируйте файл VMDK в формат VHD.

    > [!NOTE]
    > В команде qemu-img версии 2.2.1 и более поздних версиях есть ошибка, которая приводит к созданию неверного формата VHD. Эта проблема устранена в QEMU версии 2.6. Рекомендуется использовать qemu-img версии 2.2.0 или более ранних версий. Либо выполните обновление до версии 2.6 или выше. Справочные материалы: <https://bugs.launchpad.net/qemu/+bug/1490611>.

    Для начала конвертируйте образ в формат RAW:

    ```bash
    qemu-img convert -f qcow2 -O raw rhel-7.4.qcow2 rhel-7.4.raw
    ```

    Убедитесь, что размер образа в формате RAW соответствует 1 МБ. Если это не так, округлите размер до 1 МБ:

    ```bash
    MB=$((1024*1024))
    size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
    gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
    rounded_size=$((($size/$MB + 1)*$MB))
    qemu-img resize rhel-7.4.raw $rounded_size
    ```

    Конвертируйте диск в формате RAW в виртуальный жесткий диск фиксированного размера:

    ```bash
    qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

    Или с помощью qemu версии **2.6 или выше** добавьте параметр `force_size`:

    ```bash
    qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Подготовка виртуальной машины под управлением Red Hat из ISO-образа с помощью автоматического использования файла kickstart

1. Создайте файл kickstart, который будет включать содержимое ниже, и сохраните его. Дополнительные сведения об установке kickstart см. [здесь](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).

    ```sh
    Kickstart for provisioning a RHEL 7 Azure VM

    System authorization information
    auth --enableshadow --passalgo=sha512

    Use graphical install
    text

    Do not run the Setup Agent on first boot
    firstboot --disable

    Keyboard layouts
    keyboard --vckeymap=us --xlayouts='us'

    System language
    lang en_US.UTF-8

    Network information
    network  --bootproto=dhcp

    Root password
    rootpw --plaintext "to_be_disabled"

    System services
    services --enabled="sshd,waagent,NetworkManager"

    System timezone
    timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

    Partition clearing information
    clearpart --all --initlabel

    Clear the MBR
    zerombr

    Disk partitioning information
    part /boot --fstype="xfs" --size=500
    part / --fstyp="xfs" --size=1 --grow --asprimary

    System bootloader configuration
    bootloader --location=mbr

    Firewall configuration
    firewall --disabled

    Enable SELinux
    selinux --enforcing

    Don't configure X
    skipx

    Power down the machine after install
    poweroff

    %packages
    @base
    @console-internet
    chrony
    sudo
    parted
    -dracut-config-rescue

    %end

    %post --log=/var/log/anaconda/post-install.log

    #!/bin/bash

    Register Red Hat Subscription
    subscription-manager register --username=XXX --password=XXX --auto-attach --force

    Install latest repo update
    yum update -y

    Enable extras repo
    subscription-manager repos --enable=rhel-7-server-extras-rpms

    Install WALinuxAgent
    yum install -y WALinuxAgent

    Unregister Red Hat subscription
    subscription-manager unregister

    Enable waaagent at boot-up
    systemctl enable waagent

    Disable the root account
    usermod root -p '!!'

    Configure swap in WALinuxAgent
    sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
    sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

    Set the cmdline
    sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

    Enable SSH keepalive
    sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

    Build the grub cfg
    grub2-mkconfig -o /boot/grub2/grub.cfg

    Configure network
    cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    EOF

    Deprovision and prepare for Azure
    waagent -force -deprovision

    %end
    ```

1. Поместите файл kickstart в место, доступное для системы установки.

1. Создайте новую виртуальную машину в диспетчере Hyper-V. На странице **Подключить виртуальный жесткий диск** выберите параметр **Подключить виртуальный жесткий диск позднее** и выполните инструкции в мастере создания виртуальной машины.

1. Откройте параметры виртуальной машины:

    a. Подключите новый виртуальный жесткий диск к виртуальной машине. Выберите параметры **VHD Format** (Формат VHD) и **Фиксированный размер**.

    b. Подключите установочный ISO-образ к DVD-дисководу.

    c. В BIOS выберите загрузку с компакт-диска.

1. Запустите виртуальную машину. Когда отобразится руководство по установке, нажмите клавишу **TAB** , чтобы настроить параметры загрузки.

1. Введите `inst.ks=<the location of the kickstart file>` в конце параметров загрузки и нажмите клавишу **ВВОД**.

1. Дождитесь завершения процесса установки. По окончании процесса виртуальная машина завершит работу автоматически. Виртуальный жесткий диск Linux готов к передаче в Azure.

## <a name="known-issues"></a>Известные проблемы

### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>Не удалось включить драйвер Hyper-V в начальный электронный диск при использовании низкоуровневой оболочки Hyper V

В некоторых случаях установщики Linux могут не включать драйверы для Hyper-V в начальный электронный диск (initrd или initramfs), если только не обнаружат, что он работает в среде Hyper-V.

Если для подготовки образа Linux используется другая система виртуализации (например, Virtualbox, Xen и пр.), вам может потребоваться выполнить повторную сборку initrd, чтобы убедиться, что на начальном электронном диске доступны по крайней мере модули ядра hv_vmbus и hv_storvsc. Это известная проблема по крайней мере в системах на основе предшествующего дистрибутива Red Hat.

Чтобы устранить эту проблему, необходимо добавить модули Hyper-V в initramfs и выполнить повторную сборку:

Измените файл `/etc/dracut.conf`, добавив следующее содержимое:

    ```sh
    add_drivers+="hv_vmbus hv_netvsc hv_storvsc"
    ```

Повторно создайте initramfs:

    ```bash
    dracut -f -v
    ```

Дополнительные сведения см. в разделе о [повторном создании initramfs](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Дополнительная информация

Теперь виртуальный жесткий диск Red Hat Enterprise Linux можно использовать для создания виртуальных машин Azure Stack. Если вы впервые отправляете VHD-файл в Azure Stack, ознакомьтесь с разделом [Добавление элементов Marketplace с помощью средства публикации](azure-stack-marketplace-publisher.md).

Чтобы получить дополнительные сведения о низкоуровневых оболочках, сертифицированных для запуска Red Hat Enterprise Linux, посетите [веб-сайт Red Hat](https://access.redhat.com/certified-hypervisors).
