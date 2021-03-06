---
title: Ограничения в отношении ресурсов и объектов в службах Azure Analysis Services | Документы Майкрософт
description: Описание ограничений в отношении ресурсов и объектов в службах Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 09/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 4c2cebe2225e475ccd40460e7b10a6ba3ed428d5
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2018
ms.locfileid: "44723864"
---
# <a name="analysis-services-resource-and-object-limits"></a>Ограничения в отношении ресурсов и объектов в службах Analysis Services

В этой статье описываются ограничения в отношении ресурсов и объектов модели.

## <a name="tier-limits"></a>Ограничения по уровням

### <a name="developer-tier"></a>Уровень "Разработка"

Мы рекомендуем использовать этот уровень для сценариев оценки, разработки и тестирования. Один план содержит функции уровня "Стандартный", но имеет ограничения по вычислительной мощности, QPU и объему памяти. Масштабирование реплик запросов *недоступно* на этом уровне. Также на этом уровне не предусмотрено соглашение об уровне обслуживания.

|План  |QPU  |Память (ГБ)  |
|---------|---------|---------|
|D1    |    20     |    3     |


### <a name="basic-tier"></a>Уровень Basic

Этот уровень рекомендуется для рабочих решений с небольшими табличными моделями, ограниченным параллелизмом пользователей и невысокими требованиями к обновлению данных. Масштабирование реплик запросов *недоступно* на этом уровне. Также на этом уровне *не поддерживается* использование перспектив, нескольких секций и функций табличной модели DirectQuery.  

|План  |QPU  |Память (ГБ)  |
|---------|---------|---------|
|B1    |    40     |    10     |
|B2    |    80     |    20     |

### <a name="standard-tier"></a>Уровень Standard

Этот уровень лучше всего подходит для критически важных рабочих приложений с быстрорастущими моделями данных, для которых необходим эластичный параллелизм пользователей. На этом уровне поддерживаются все функции табличного моделирования и возможность быстрого обновления данных, которая позволяет обновлять модели данных практически в реальном времени.

|План  |QPU  |Память (ГБ)  |
|---------|---------|---------|
|S1    |    40     |    10     |
|S2    |    100     |    25     |
|S3    |    200     |    50     |
|S4    |    400     |    100     |
|S8*    |    320     |    200     |
|S9*    |    640    |    400     |

\* Доступно не во всех регионах.  

## <a name="object-limits"></a>Ограничения на объекты

Это теоретические ограничения. При меньших значениях производительность будет снижена.

|Объект.|Максимальные размеры и количество|  
|------------|----------------------------|  
|Базы данных в экземпляре|16 000|  
|Совокупное число таблиц и столбцов в базе данных|16 000|  
|Строки в таблице|Без ограничений<br /><br /> **Предупреждение**. Ни один столбец в таблице не может содержать более 1 999 999 997 отдельных значений.|  
|Иерархии в таблице|15 999|  
|Уровни в иерархии|15 999|  
|Отношения|8000|  
|Ключевые столбцы во всех таблицах|15 999|  
|Меры в таблице|2^31–1 = 2 147 483 647|  
|Ячейки, возвращаемые запросом|2^31–1 = 2 147 483 647|  
|Размер записи исходного запроса|64 КБ|  
|Длина имени объекта|512 символов|  


