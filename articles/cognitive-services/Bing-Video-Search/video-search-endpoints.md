---
title: Конечные точки поиска видео — Поиск видео Bing
titlesuffix: Azure Cognitive Services
description: Сводная информация о конечной точке API Bing для поиска видео.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: conceptual
ms.date: 12/04/2017
ms.author: rosh
ms.openlocfilehash: c153f577f76944d22f9a1b0fb4b24d332d2a02c8
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220777"
---
# <a name="video-search-endpoints"></a>Конечные точки для поиска видео
**API для поиска видео** включает три конечные точки.  Конечная точка 1 возвращает видео из Интернета на основе запроса. Конечная точка 2 возвращает аналитические сведения о видео на основе параметра `modules` URL-адреса.  Конечная точка 3 возвращает популярные видео.

## <a name="endpoints"></a>Конечные точки
Чтобы получить результаты поиска видео с помощью API Bing, отправьте запрос `GET` на одну из конечных точек, приведенных ниже. Для определения дополнительных спецификаций используйте заголовки и параметры URL-адреса.

**Конечная точка 1** возвращает результаты поиска видео, соответствующие поисковому запросу пользователя, который определен с помощью `?q=""`.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search
```

**Конечная точка 2** возвращает аналитические сведения о видео, например связанные видео. Включите [параметр запроса](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query-parameters) `modules`, который представляет собой список запрашиваемых аналитических сведений с разделителями-запятыми.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/details
```

**Конечная точка 3** возвращает видео, которые набирают популярность. За основу берутся результаты поисковых запросов, выполненных другими пользователями. Видео разделяются на различные категории, например, если в них фигурируют примечательные люди или события.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending
```

Дополнительные сведения о заголовках, параметрах, кодах рынков, объектах ответов, ошибках и т. п. вы найдете в справочнике [по API Bing для поиска видео версии 7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference).
## <a name="response-json"></a>Ответ в формате JSON
Ответ на запрос на поиск видео содержит результаты в виде объектов JSON. Примеры синтаксического анализа результатов см. в этом [руководстве](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app) и [исходном коде](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app-source).

## <a name="next-steps"></a>Дополнительная информация
Интерфейсы API **Bing** поддерживают действия поиска, которые возвращают результаты определенного типа. Все конечные точки поиска возвращают результаты в виде объектов ответа JSON.  Все конечные точки поддерживают запросы, которые возвращают результаты с учетом языка и (или) местоположения по значениям долготы, широты и радиуса поиска.

Полные сведения о параметрах, поддерживаемых каждой конечной точкой, приведены в справочной документации по каждому типу.
Простые примеры запросов к API для поиска видео см. в [кратких руководствах по поиску видео](https://docs.microsoft.com/azure/cognitive-services/bing-video-search).