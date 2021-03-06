---
title: Поддержка языков — API визуального поиска Bing
titleSuffix: Azure Cognitive Services
description: Список естественных языков, стран и регионов, поддерживаемых API визуального поиска Bing. В API визуального поиска Bing реализована поддержка более трех десятков стран и регионов, во многих из которых используется несколько языков.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 09/25/2018
ms.author: scottwhi
ms.openlocfilehash: 1dd434bbf71e482f5d0a52b3c84e46382079b43a
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886405"
---
# <a name="language-and-region-support-for-the-bing-visual-search-api"></a>Поддержка языков и регионов в API визуального поиска Bing

API Bing Visual Search поддерживает более трех десятков стран и регионов, причем во многих из них используется несколько языков. В каждом запросе должна быть указана страна или регион и выбранный язык пользователя. Знание рынка пользователя помогает Bing возвращать соответствующие результаты. Если не указать страну или регион и язык, Bing сделает все возможное для определения страны или региона и языка пользователя. Так как результаты могут содержать ссылки на Bing, знание страны или региона и языка позволит отобразить предпочтительную локализованную версию Bing, если пользователь откроет соответствующие ссылки.

Чтобы указать страну или регион и язык, задайте в качестве параметра запроса `mkt` (рынок) код из приведенной ниже таблицы **Рынки**. Код рынка задает как страну или регион, так и язык. Если пользователь предпочитает отображение текста на другом языке, задайте в качестве параметра запроса `setLang` соответствующий код языка.

Кроме того, можно указать страну или регион с помощью параметра запроса `cc`. Указывая страну или регион, необходимо также задать один или несколько кодов языков с помощью HTTP-заголовка `Accept-Language`. Поддерживаемые языки различаются в зависимости от страны или региона. Они указаны для каждой страны или региона в таблице "Рынки".



> [!NOTE]
> Применяются следующие ограничения рынков:
>
> - Аннотации для распознавания изображений доступны только на английском языке.
> - Рецепты, покупки и страницы с аналитическими сведениями доступны только на рынке en-US.


## <a name="countries"></a>Страны

|Страна или регион|Код|
|-------|----|
|Аргентина|AR|
|Австралия|AU|
|Австрия|AT|
|Бельгия|BE|
|Бразилия|BR|
|Канада|CA|
|Чили|CL|
|Дания|DK|
|Финляндия|FI|
|Франция|СВ|
|Германия|DE|
|Гонконг, САР|HK|
|Индия|IN|
|Индонезия|ИД|
|Италия|IT|
|Япония|JP|
|Корея|KR|
|Малайзия|MY|
|Мексика|MX|
|Нидерланды|NL|
|Новая Зеландия|NZ|
|Норвегия|НЕТ|
|Китай|CN|
|Польша|PL|
|Португалия|PT|
|Филиппины|PH|
|Россия|RU|
|Саудовская Аравия|SA|
|ЮАР|ZA|
|Испания|ES|
|Швеция|SE|
|Швейцария|CH|
|Тайвань|TW|
|Турция|TR|
|Великобритания|GB|
|США|США|


## <a name="markets"></a>Рынки

|Страна или регион|Язык|Код рынка|
|-------|--------|-----------|
|Аргентина|Испанский|es-AR|
|Австралия|Английский|en-AU|
|Австрия|Немецкий|de-AT|
|Бельгия|Нидерландский|nl-BE|
|Бельгия|Французский|fr-BE|
|Бразилия|Португальский|pt-BR|
|Канада|Английский|en-CA|
|Канада|Французский|fr-CA|
|Чили|Испанский|es-CL|
|Дания|Датский|da-DK|
|Финляндия|Финский|fi-FI|
|Франция|Французский|fr-FR|
|Германия|Немецкий|de-DE|
|Гонконг, САР|Китайский, традиционное письмо|zh-HK|
|Индия|Английский|en-IN|
|Индонезия|Английский|en-ID|
|Италия|Итальянский|it-IT|
|Япония|Японский|ja-JP|
|Корея|Корейский|ko-KR|
|Малайзия|Английский|en-MY|
|Мексика|Испанский|es-MX|
|Нидерланды|Нидерландский|nl-NL|
|Новая Зеландия|Английский|en-NZ|
|Китай|Китайский|zh-CN|
|Польша|Польский|pl-PL|
|Португалия|Португальский|pt-PT|
|Филиппины|Английский|en-PH|
|Россия|Русский|ru-RU|
|Саудовская Аравия|Арабский|ar-SA|
|ЮАР|Английский|en-ZA|
|Испания|Испанский|es-ES|
|Швеция|Шведский|sv-SE|
|Швейцария|Французский|fr-CH|
|Швейцария|Немецкий|de-CH|
|Тайвань|Китайский, традиционное письмо|zh-TW|
|Турция|Турецкий|tr-TR|
|Великобритания|Английский|en-GB|
|США|Английский|en-US|
|США|Испанский|es-US|
