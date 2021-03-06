---
title: Создание классической виртуальной машины Linux с помощью классического Azure CLI | Документация Майкрософт
description: Узнайте, как создать виртуальную машину Linux с помощью классического Azure CLI, используя классическую модель развертывания.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: f8071a2e-ed91-4f77-87d9-519a37e5364f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: cynthn
ms.openlocfilehash: 507d9e12a37d7bf187a3e56b04cb47ac0104773d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982047"
---
# <a name="how-to-create-a-classic-linux-vm-with-the-azure-classic-cli"></a>Как создать классическую виртуальную машину Linux с помощью классического Azure CLI
> [!IMPORTANT] 
> В Azure предлагаются две модели развертывания для создания ресурсов и работы с ними: [модель Resource Manager и классическая модель](../../../resource-manager-deployment-model.md). В этой статье рассматривается использование классической модели развертывания. Для большинства новых развертываний Майкрософт рекомендует использовать модель диспетчера ресурсов. Версия для модели развертывания с помощью Resource Manager доступна [здесь](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

В этом разделе описано создание виртуальной машины Linux с помощью классических интерфейса командной строки Azure и модели развертывания. Мы используем образ Linux из раздела **ОБРАЗЫ** в Azure. Команды классического Azure CLI позволяют настраивать, помимо прочих, следующие параметры:

* Подключение виртуальной машины к виртуальной сети.
* Добавление виртуальной машины к существующей облачной службе.
* Добавление виртуальной машины к существующей учетной записи хранения
* Добавление виртуальной машины в группу доступности или расположение.

> [!IMPORTANT]
> Если к виртуальной машине необходимо подключаться с помощью имени узла по виртуальной сети или настроить подключения между организациями, при создании виртуальной машины обязательно укажите виртуальную сеть. Виртуальную машину можно настроить для подключения к виртуальной сети только при ее создании. Дополнительную информацию о виртуальных сетях см. в разделе [Обзор виртуальных сетей Azure](http://go.microsoft.com/fwlink/p/?LinkID=294063).
> 
> 

## <a name="how-to-create-a-linux-vm-using-the-classic-deployment-model"></a>Создание виртуальной машины Linux с использованием классической модели развертывания
[!INCLUDE [virtual-machines-create-LinuxVM](../../../../includes/virtual-machines-create-linuxvm.md)]

