---
title: Конечная точка для веб-поиска
titleSuffix: Azure Cognitive Services
description: Сводные сведения о конечной точке API для поиска в Интернете.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-gedod
ms.openlocfilehash: 3058ca6cf0eb99486dd4c269d43b274fb367f7a9
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2018
ms.locfileid: "46125465"
---
# <a name="web-search-endpoint"></a>Конечная точка для поиска в Интернете
**API для поиска в Интернете** возвращает веб-страницы, новости, изображения, видео и [сущности](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Сущности содержат сводные данные о человеке, месте или теме.
## <a name="endpoint"></a>Конечная точка
Чтобы получить результаты поиска в Интернете с помощью API Bing, отправьте запрос `GET` на следующую конечную точку. Заголовки и параметры URL-адреса определяют дополнительные спецификации.

**Конечная точка** возвращает результаты поиска в Интернете, соответствующие поисковому запросу пользователя, который определен с помощью `?q=""`.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/search
```
Конечная точка: дополнительные сведения о заголовках, параметрах, кодах рынков, объектах ответов, ошибках и т. п. вы найдете в справочнике [по API Bing для поиска в Интернете версии 7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference).

## <a name="response-json"></a>Ответ в формате JSON
Ответ на запрос на поиск в Интернете содержит результаты в виде объектов JSON. Для анализа результатов требуются процедуры обработки элементов каждого типа. Чтобы ознакомиться с примерами, изучите это [руководство](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app) и [исходный код](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app-source).

## <a name="next-steps"></a>Дополнительная информация
Интерфейсы API **Bing** поддерживают действия поиска, которые возвращают результаты определенного типа. Все конечные точки поиска возвращают результаты в виде объектов ответа JSON.  Все конечные точки поддерживают запросы, которые возвращают результаты с учетом языка и (или) местоположения по значениям долготы, широты и радиуса поиска.

Полные сведения о параметрах, поддерживаемых каждой конечной точкой, приведены в справочной документации по каждому типу.
Примеры простых запросов к API поиска в Интернете приведены в [кратких руководствах по поиску в Интернете](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/search-the-web).
