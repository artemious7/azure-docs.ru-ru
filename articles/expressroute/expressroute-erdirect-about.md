---
title: Сведения об Azure ExpressRoute Direct | Документация Майкрософт
description: Эта страница содержит обзор ExpressRoute Direct (предварительная версия)
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: c33bec76fe17336221c873778c2993d75fec81e8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46962238"
---
# <a name="about-expressroute-direct-preview"></a>Сведения об ExpressRoute Direct (предварительная версия)

ExpressRoute Direct дает пользователям возможность подключения непосредственно к глобальной сети корпорации Майкрософт в стратегически распределенных по всему миру расположениях пиринга. ExpressRoute обеспечивает двойное подключение 100 Гбит/с, которое при масштабировании поддерживает подключение "активный — активный". 

Основные неограниченные функции, предоставляемые ExpressRoute Direct, включают:

* возможность приема больших объемов данных в службах, таких как служба хранилища и Cosmos DB; 
* физическая изоляция для отраслей, которые регулируются и требуют специализированной и изолированной связи, например: банковское дело, правительство и розничная торговля; 
* четкий контроль распределения схем на основе бизнес-единиц.

> [!IMPORTANT]
> Продукт ExpressRoute Direct в настоящее время доступен в предварительной версии.
>
> Общедоступная предварительная версия предоставляется без соглашения об уровне обслуживания и не должна использоваться для производственных рабочих нагрузок. Некоторые функции могут не поддерживаться, иметь ограничения и быть доступными не во всех расположениях Azure. См. [дополнительные условия использования для предварительных версий Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="enroll-in-the-preview"></a>Регистрация для использования предварительной версии

Прежде чем использовать ExpressRoute Direct, необходимо зарегистрировать свою подписку для использования предварительной версии. Для регистрации отправьте электронное сообщение с идентификатором подписки по адресу <ExpressRouteDirect@microsoft.com>. ExpressRoute Direct — это функция корпоративного уровня. Укажите дополнительные сведения:

* сценарии, которые необходимо выполнить с помощью **ExpressRoute Direct**;
* настройки местоположения — полный список всех расположений см. в разделе [Партнеры и одноранговые расположения ExpressRoute](expressroute-locations-providers.md);
* временную шкалу для реализации;
* все вопросы относительно служб.

## <a name="expressroute-using-a-service-provider-and-expressroute-direct"></a>ExpressRoute с использованием поставщика служб и ExpressRoute Direct

| **ExpressRoute с использованием поставщика служб** | **ExpressRoute Direct** | 
| --- | --- | 
| Использование поставщика служб для быстрого развертывания и подключения к существующей инфраструктуре. | Требуется инфраструктура на 100 Гбит/с и полное управление всеми слоями.
| Интеграция с сотнями поставщиков, включая Ethernet и MPLS. | Прямая или выделенная емкость для регулируемых отраслей промышленности и массовое проникновение данных. | 
| Каналы SKU от 50 Мбит/с до 10 Гбит/с | Каналы SKU от 1Гбит/с до 100 Гбит/с
| Оптимизировано для одного клиента | Оптимизировано для одного клиента,поставщиков облачных служб, нескольких бизнес-единиц

## <a name="expressroute-direct-circuits"></a>Каналы ExpressRoute Direct

Microsoft Azure ExpressRoute позволяет переносить локальные сети в облако Microsoft по частному подключению, которое обеспечивается поставщиком услуг подключения. ExpressRoute позволяет устанавливать подключения к облачным службам Майкрософт, таким как Microsoft Azure, Office 365 и Dynamics 365.  

Каждое место пиринга имеет доступ к глобальной сети Майкрософт и может иметь доступ к любому региону в геополитической зоне по умолчанию и ко всем глобальным регионам с каналом класса "Премиум".  

Функциональные возможности в большинстве случаев соответствуют каналам, в которых используется поставщик службы ExpressRoute. Чтобы поддерживать дополнительную степень детализации и новые возможности, предлагаемые ExpressRoute Direct, для каналов ExpressRoute Direct существуют определенные ключевые возможности.

## <a name="circuit-skus"></a>Каналы SKU

ExpressRoute Direct поддерживает сценарии приема данных в хранилище Azure и другие сервисы больших данных. Каналы ExpressRoute в ExpressRoute Direct теперь также поддерживают каналы SKU **40 Гбит/с** и **100 Гбит/с**. 

## <a name="vlan-tagging"></a>Добавление тегов в виртуальной локальной сети

ExpressRoute Direct поддерживает добавление тегов в виртуальной локальной сети как QinQ так и Dot1Q.

* **Добавление тегов виртуальной локальной сети QinQ** позволяет изолировать домены маршрутизации на каждом канале ExpressRoute. Azure динамически выделяет S-тег при создании канала, который не может быть изменен. Каждый пиринг на канале (закрытый и Майкрософт) будет использовать уникальный C-тег в качестве виртуальной локальной сети. C-тег не обязательно должен быть уникальным во всех каналах портов ExpressRoute Direct. 

* **Добавление тегов в виртуальной локальной сети Dot1Q** позволяет использовать единственную виртуальную локальную сеть с тегами для каждой пары ExpressRoute Direct. C-тег, используемый для пиринга, должен быть уникальным для всех каналов и пирингов в паре ExpressRoute Direct.

## <a name="workflow"></a>Рабочий процесс

![рабочий процесс](./media/expressroute-erdirect-about/workflow1.png)

## <a name="sla"></a>Соглашение об уровне обслуживания

ExpressRoute Direct предоставляет одно и то же соглашение об обслуживании корпоративного уровня с резервными подключениями "активный — активный" в глобальной сети Майкрософт. Инфраструктура ExpressRoute является резервной, а подключение к глобальной сети Майкрософт — резервной и разноплановой, соответствующей требованиям пользователей. В предварительной версии не будет соглашения об уровне обслуживания, его можно использовать только для непроизводственных рабочих нагрузок.

## <a name="next-steps"></a>Дополнительная информация

[Настройка ExpressRoute Direct](expressroute-howto-erdirect.md)