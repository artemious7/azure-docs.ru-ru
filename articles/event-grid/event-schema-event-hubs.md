---
title: Схема событий концентраторов в службе "Сетка событий Azure"
description: Описание свойств для событий концентраторов, используемых со службой "Сетка событий Azure"
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: reference
ms.date: 08/17/2018
ms.author: tomfitz
ms.openlocfilehash: e301f3895126ed52b8d4c1f046f69dfcedb3563c
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2018
ms.locfileid: "42146827"
---
# <a name="azure-event-grid-event-schema-for-event-hubs"></a>Схема событий службы "Сетка событий Azure" для концентраторов событий

В этой статье описаны свойства и схема для событий концентраторов. Общие сведения о схемах событий см. в статье [Схема событий службы "Сетка событий Azure"](event-schema.md).

Список примеров сценариев и руководства см. в статье [Источники событий в службе "Сетка событий Azure"](event-sources.md#event-hubs).

### <a name="available-event-types"></a>Доступные типы событий

Тип события **Microsoft.EventHub.CaptureFileCreated** возникает в службе "Центры событий" при создании файла записи.

## <a name="example-event"></a>Пример события

Этот пример события демонстрирует схему события в Центрах событий, возникающего, когда файл сохраняется при помощи функции Сбора: 

```json
[
    {
        "topic": "/subscriptions/<guid>/resourcegroups/rgDataMigrationSample/providers/Microsoft.EventHub/namespaces/tfdatamigratens",
        "subject": "eventhubs/hubdatamigration",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-08-31T19:12:46.0498024Z",
        "id": "14e87d03-6fbf-4bb2-9a21-92bd1281f247",
        "data": {
            "fileUrl": "https://tf0831datamigrate.blob.core.windows.net/windturbinecapture/tfdatamigratens/hubdatamigration/1/2017/08/31/19/11/45.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 249168,
            "eventCount": 1500,
            "firstSequenceNumber": 2400,
            "lastSequenceNumber": 3899,
            "firstEnqueueTime": "2017-08-31T19:12:14.674Z",
            "lastEnqueueTime": "2017-08-31T19:12:44.309Z"
        },
        "dataVersion": "",
        "metadataVersion": "1"
    }
]
```

## <a name="event-properties"></a>Свойства события

Событие содержит следующие высокоуровневые данные:

| Свойство | type | ОПИСАНИЕ |
| -------- | ---- | ----------- |
| Раздел | строка | Полный путь к ресурсу для источника событий. Это поле защищено от записи. Это значение предоставляет служба "Сетка событий". |
| subject | строка | Определенный издателем путь к субъекту событий. |
| eventType | строка | Один из зарегистрированных типов событий для этого источника событий. |
| eventTime | строка | Время создания события с учетом времени поставщика в формате UTC. |
| id | строка | Уникальный идентификатор события. |
| data | object | Данные события концентратора. |
| dataVersion | строка | Версия схемы для объекта данных. Версию схемы определяет издатель. |
| metadataVersion | строка | Версия схемы для метаданных события. Служба "Сетка событий" определяет схему свойств верхнего уровня. Это значение предоставляет служба "Сетка событий". |

Объект данных имеет следующие свойства:

| Свойство | type | ОПИСАНИЕ |
| -------- | ---- | ----------- |
| fileUrl | строка | Путь к файлу записи. |
| fileType | строка | Тип файла записи. |
| partitionId | строка | Идентификатор сегмента. |
| sizeInBytes | целое число | Размер файла. |
| eventCount | целое число | Число событий в файле. |
| firstSequenceNumber | целое число | Наименьший порядковый номер в очереди. |
| lastSequenceNumber | целое число | Последний порядковый номер в очереди. |
| firstEnqueueTime | строка | Первые данные времени в очереди. |
| lastEnqueueTime | строка | Последние данные времени в очереди. |

## <a name="next-steps"></a>Дополнительная информация

* См. общие сведения о [службе "Сетка событий Azure"](overview.md).
* Дополнительные сведения о создании подписки на Сетку событий Azure см. в статье [Схема подписки для службы "Сетка событий"](subscription-creation-schema.md).
* Сведения об обработке событий в Центрах событий см. в статье [Потоковая передача больших данных в хранилище данных](event-grid-event-hubs-integration.md).