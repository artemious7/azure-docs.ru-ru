---
title: Настройка перевода в API перевода текстов
titlesuffix: Azure Cognitive Services
description: Используйте Microsoft Translator Hub для создания собственной системы машинного перевода с нужными вам терминами и стилем.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 05/10/2018
ms.author: v-jansko
ms.openlocfilehash: d71158bc74ffe15f133cc637371ddc840ef9df9e
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2018
ms.locfileid: "46124343"
---
# <a name="customize-your-text-translations"></a>Настройка переводов текста

Предварительная версия Microsoft Custom Translator — это компонент службы Microsoft Translator, с помощью которого пользователи могут настраивать расширенные возможности нейронного машинного перевода Microsoft Translator при переводе текста с помощью API перевода текстов (только в версии 3). 

Этот компонент также можно использовать для настройки перевода речи с использованием [предварительной версии службы "Речь" Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/speech-service/).

## <a name="custom-translator"></a>Custom Translator

Custom Translator позволяет создавать нейронные системы перевода, которые понимают особую терминологию вашего бизнеса и отрасли. Настраиваемые системы перевода могут интегрироваться с существующими приложениями, рабочими процессами и веб-сайтами. 

### <a name="how-does-it-work"></a>Как это работает?

Используйте уже переведенные документы (листовки, веб-страницы, документы и т. д.) для создания системы перевода, которая учитывает терминологию и стиль вашей отрасли лучше, чем универсальная система перевода. Пользователи могут отправлять документы TMX, XLIFF, TXT, DOCX и XLSX.  

Эта система также принимает данные, которые параллельны на уровне документов, но еще не согласованы на уровне предложений. Если пользователи имеют доступ к разным версиям одного содержимого на нескольких языках, представленным в отдельных документах, Custom Translator сможет автоматически сопоставить предложения между этими документами.  Система также может использовать данные на одном языке (на любом из языков пары или на обеих сразу) в дополнение к параллельным обучающим данным для улучшения переводов. 

Доступ к этой пользовательской системе можно получить с помощью обычных вызовов API перевода текстов от Майкрософт с использованием параметра категории.

При наличии нужного объема и правильного типа данных для обучения, совершенно нормальным можно считать улучшение качества перевода на 5–10 позиций или даже увеличения показателя BLEU благодаря Custom Translator.

Дополнительные сведения о различных уровнях настройки на основе доступных данных можно найти в [руководстве пользователя по Custom Translator](http://aka.ms/CustomTranslatorDocs).


## <a name="microsoft-translator-hub"></a>Microsoft Translator Hub

Устаревший центр Microsoft Translator Hub можно использовать для статистического машинного перевода. [Подробнее](https://www.microsoft.com/en-us/translator/hub.aspx) 

## <a name="custom-translator-versus-hub"></a>Сравнение Custom Translator и центра

|   | **Microsoft Translator Hub** | **Custom Translator**|
|:-----|:----:|:----:|
|Состояние компонента настройки   | Общедоступная версия  | Предварительный просмотр |
| Версия API перевода текстов  | Только версия 2   | Только версия 3 |
| Настройка SMT | Yes   | Нет  | 
| Настройка NMT | Нет     | Yes |
| Настройка новых единых служб распознавания речи | Нет     | Yes | 
| [Без трассировки](http://www.aka.ms/notrace) | Yes   | Yes | 

## <a name="collaborative-translations-framework"></a>Платформа совместной работы над переводами

> [!NOTE]
> По состоянию на 1 февраля 2018 г. AddTranslation() и AddTranslationArray() больше не доступны для использования в API перевода текстов версии 2.0. Эти методы возвращают ошибку и ничего не записывают. API перевода текстов версии 3.0 не поддерживает эти методы.

>Аналогичные функциональные возможности доступны в API для Translator Hub. Дополнительные сведения см. на странице [https://hub.microsofttranslator.com/swagger](https://hub.microsofttranslator.com/swagger). 

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Настройка системы с учетом особенностей языка с помощью Custom Translator](http://aka.ms/CustomTranslatorDocs)
