---
title: Использование нескольких источников Azure Cosmos DB в работе с базами данных NoSQL с открытым кодом
description: ''
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: ea3a16cf6b5aa4e46ad401e59aacb4d936a7429a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963906"
---
# <a name="using-azure-cosmos-db-multi-master-with-open-source-nosql-databases"></a>Использование нескольких источников Azure Cosmos DB в работе с базами данных NoSQL с открытым кодом

Поддержка нескольких источников Azure Cosmos DB — это собственная серверная реализация, которая доступна для всех предложений NoSQL с открытым кодом, поддерживаемых Cosmos DB. Она доступна для всех пакетов SDK, поддерживаемых Cosmos DB.

Azure Cosmos DB — это первая в мире служба, предлагающая поддержку нескольких источников для этих баз данных NoSQL с открытым кодом.

|Модель  |Поддержка  |
|---------|---------|
|MongoDB  | Шаблон "активный-активный"  |
|График  | Шаблон "активный-активный" |
|Cassandra  | Шаблон "активный-активный"   |

## <a name="use-mongodb-clients-with-multi-master"></a>Использование клиентов MongoDB с несколькими источниками

Функция нескольких источников включается для учетных записей API MongoDB таким же образом, как она включается для других интерфейсов API Azure Cosmos DB. После включения функции нескольких источников для учетных записей API MongoDB каждый экземпляр клиентского приложения может выбрать предпочтительный регион для операций чтения и записи. С точки зрения драйвера MongoDB предпочтительный регион представляется клиенту как основной регион набора реплик. Таким образом каждый регион распределенной базы данных может выступать в качестве основного региона набора реплик. Функция нескольких источников Azure Cosmos DB позволяет значительно сократить задержки при записи для глобально распределенных приложений MongoDB. 

### <a name="set-the-primary-region"></a>Настройка основного региона

Каждый экземпляр клиента MongoDB может выбрать основной регион, добавив `@<preferred_primary_region_name>` в поле "application name", принимаемое драйвером клиента MongoDB. Большинство драйверов принимают его в строке подключения, например:

`mongodb://fabrikam:KEY@fabrikam.documents.azure.com:10255/?ssl=true&replicaSet=globaldb&appname=@West US`

После подключения может возникнуть одна из следующих ситуаций.

* Если имя предпочтительного региона доступно для региона записи, то Azure Cosmos DB выбирает его в качестве основного региона.

* Если не имя предпочтительного региона не указано, то в качестве основного региона Azure Cosmos DB выбирает регион по умолчанию.

* Если имя предпочтительного региона указано, но он не может использоваться как регион записи для учетной записи базы данных, то в качестве основного региона Azure Cosmos DB выберет ближайший регион записи, который доступен.

Некоторые драйверы, такие как драйвер NodeJS, сначала отправляют операции записи на узел, указанный в начальной строке подключения. Чтобы при использовании таких драйверов операции записи направлялись в предпочтительный регион, помимо имени приложения необходимо изменить DNS-имя в строке подключения, чтобы указать имя этого региона. Обязательно указывайте имя региона без пробелов. Например: 

`mongodb://fabrikam:KEY@fabrikam-westus.documents.azure.com:10255/?ssl=true&replicaSet=globaldb&appname=@West US`

### <a name="conflict-resolution-mode"></a>Режим разрешения конфликтов

Режим разрешения конфликтов для учетных записей API MongoDB для Azure Cosmos DB всегда действует по принципу "побеждает последняя запись". При этом используется метка времени регионального сервера, который принял операцию записи.

## <a name="next-steps"></a>Дополнительная информация

В этой статье вы узнали о поддержке нескольких источников для учетных записей API MongoDB для Azure Cosmos DB. Далее рекомендуем ознакомиться со следующими ресурсами:

* [Включение поддержки нескольких источников для учетных записей Azure Cosmos DB](enable-multi-master.md)

* [Понимание принципов разрешения конфликта в Azure Cosmos DB](multi-master-conflict-resolution.md)
