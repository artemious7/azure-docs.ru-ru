---
title: включение файла
description: включение файла
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 05/17/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: d9c8a0e6a3bd6d79a11ee0d0dab0500a209e5571
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38943923"
---
Чтобы открыть порт или создать конечную точку для виртуальной машины в Azure, создайте сетевой фильтр для подсети или сетевого интерфейса виртуальной машины. Эти фильтры, контролирующие входящий и исходящий трафик, добавляются в группу безопасности сети и присоединяются к ресурсу, который будет получать трафик.

Давайте используем распространенный пример веб-трафика через порт 80. Настройте виртуальную машину для обслуживания веб-запросов через стандартный TCP-порт 80 (не забудьте запустить соответствующие службы и открыть соответствующие правила брандмауэра операционной системы на виртуальной машине). Затем выполните следующие действия.

1. Создайте группу безопасности сети.
2. Создайте правило входящих подключений, разрешающее трафик с:
   * диапазоном портов назначения "80";
   * диапазоном портов источника "*" (разрешает любой порт источника);
   * значением приоритета меньше 65 500 (чтобы это правило имело больший приоритет, чем универсальное запрещающее правило по умолчанию для входящего подключения).
3. Свяжите сетевую группу безопасности с сетевым интерфейсом виртуальной машины или подсетью.

С помощью правил и групп безопасности сети можно создавать сложные сетевые конфигурации для защиты своей среды. В нашем примере используется не более двух правил, которые разрешают HTTP-трафик или удаленное управление. Дополнительные сведения см. в разделе [Дополнительные сведения](#more-information-on-network-security-groups) ниже или в статье [Группа безопасности сети](../articles/virtual-network/security-overview.md).

