---
title: Управление регионами в Azure Stack | Документация Майкрософт
description: Общие сведения об управлении регионами в Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: sethm
ms.reviewer: efemmano
ms.openlocfilehash: 401b81ceb7ab71528a4ad11bc7d8944b4d732933
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "49078872"
---
# <a name="region-management-in-azure-stack"></a>Управление регионами в Azure Stack

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

Azure Stack использует концепцию регионов, которые представляют собой логические единицы, содержащие аппаратные ресурсы инфраструктуры Azure Stack. В разделе управления регионами вы найдете все ресурсы, необходимые для успешной эксплуатации инфраструктуры Azure Stack.

Один регион включает одно интегрированное развертывание системы (также именуется *облаком Azure Stack*). Каждый Пакет средств разработки Azure Stack содержит один регион с именем **local**. Если вы развернете вторую интегрированную систему Azure Stack или настроите второй экземпляр пакета SDK на другом оборудовании, это новое облако Azure будет представлять собой другой регион.

## <a name="information-available-through-the-region-management-tile"></a>Сведения, доступные на плитке управления регионами

Azure Stack предоставляет ряд возможностей для управления регионами, которые можно вызвать при помощи плитки **Управление регионами**. На административном портале для оператора Azure Stack эта плитка размещается на панели мониторинга по умолчанию. С ее помощью вы можете контролировать и обновлять регион Azure Stack и его компоненты. Состав компонентов зависит от региона.

 ![Плитка управления регионами](media/azure-stack-manage-region/image1.png)

 Если щелкнуть любой регион на плитке управления регионами, отобразятся следующие сведения:

  ![Описание панелей в колонке управления регионами](media/azure-stack-manage-region/image2.png)

1. **Меню ресурсов.** Здесь представлено несколько областей управления инфраструктурой. Вы можете просматривать ресурсы пользователей (например, учетные записи хранения и виртуальные сети) и управлять ими.

2. **Оповещения.** Откроется список оповещений для всей системы и подробные сведения о каждом из них.

3. **Обновления.** Здесь вы можете просмотреть текущую версию инфраструктуры Azure Stack, доступные обновления и журнал обновлений. Кроме того, здесь можно обновить интегрированную систему.

4. **Поставщики ресурсов.** Поставщики ресурсов позволяют управлять функциями пользователя, предоставляющимися необходимыми для работы Azure Stack компонентами. Каждый поставщик ресурсов предоставляет собственные административные функции. Сюда могут входить оповещения, метрики и другие функции, характерные для конкретного поставщика ресурсов.

5. **Инфраструктурные роли.** Инфраструктурные роли — это обязательные компоненты для выполнения Azure Stack. Здесь перечислены только те инфраструктурные роли, от которых поступают оповещения. Щелкнув любую роль, вы можете просмотреть оповещения для нее и все экземпляры роли, в которых она выполняется.

## <a name="next-steps"></a>Дополнительная информация

- [Monitor health and alerts in Azure Stack](azure-stack-monitor-health.md) (Мониторинг работоспособности и оповещений в Azure Stack).
- [Manage updates in Azure Stack overview](azure-stack-updates.md) (Обзор управления обновлениями в Azure Stack).
