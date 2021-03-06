---
title: Поддерживаемые источники данных — QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker автоматически извлекает пары "вопрос — ответ" из слабоструктурированного содержимого, такого как страницы вопросов и ответов, руководства по продукции, рекомендации, документы поддержки и политики, которые хранятся в виде веб-страниц, PDF-файлов или файлов документации MS Word. Также содержимое можно добавлять в базу знаний из структурированных файлов с содержимым вопросов и ответов (QnA).
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/25/2018
ms.author: tulasim
ms.openlocfilehash: 982bcbb9060a3f29000de2a0487b61dc58e24f6e
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/08/2018
ms.locfileid: "48855465"
---
# <a name="data-sources-for-qna-maker-content"></a>Источники данных для содержимого QnA Maker

QnA Maker автоматически извлекает пары "вопрос — ответ" из слабоструктурированного содержимого, такого как страницы вопросов и ответов, руководства по продукции, рекомендации, документы поддержки и политики, которые хранятся в виде веб-страниц, PDF-файлов или файлов документации MS Word. Также содержимое можно добавлять в базу знаний из структурированных файлов с содержимым вопросов и ответов (QnA). 

В приведенной ниже таблице представлены типы содержимого и форматы файлов, поддерживаемые QnA Maker.

|Тип источника|Тип содержимого| Примеры|
|--|--|--|
|URL-адрес|Часто задаваемые вопросы (неструктурированные, с разделами или темами на домашней страницей)|[простые вопросы и ответы](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs), [часто задаваемые вопросы со ссылками](https://www.microsoft.com/software-download/faq), [часто задаваемые вопросы с темами на домашней странице](https://support.microsoft.com/products/windows?os=windows-10)|
|PDF/DOC|Часто задаваемые вопросы, руководство по продуктам, брошюры, документы, объявления политики, руководства поддержки, структурированные вопросы и ответы (QnA) и т. д.|[Структурированные вопросы и ответы.doc](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Bot%20Service%20Sample%20FAQ.docx), [Пример руководства по продукту.pdf](https://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf), [Пример слабоструктурированного файла.doc](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx), [Пример технического документа.pdf](https://azure.microsoft.com/mediahandler/files/resourcefiles/azure-stack-wortmann-bring-the-power-of-the-public-cloud-into-your-data-center/Azure_Stack_Wortmann_Bring_the_Power_of_the_Public_Cloud_into_Your_Data_Center.pdf)|
|Excel|Файл структурированных вопросов и ответов (включая поддержку RTF, HTML)|[Пример часто задаваемых вопросов QnA.xls](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/QnA%20Maker%20Sample%20FAQ.xlsx)|
|TXT/TSV|Файл структурированных вопросов и ответов (QnA)|[Пример беседы.tsv](https://raw.githubusercontent.com/Microsoft/BotBuilder-PersonalityChat/master/CSharp/Datasets/Queries_Responses_Friendly_QnAMaker.tsv)|

## <a name="faq-urls"></a>URL-адреса вопросов и ответов

QnA Maker может поддерживать веб-страницы вопросов и ответов в трех разных формах: простые страницы вопросов и ответов, страницы вопросов и ответов со ссылками, страницы вопросов и ответов с темами на домашней странице.

### <a name="plain-faq-pages"></a>Простые страницы вопросов и ответов

Это наиболее распространенный тип страниц с вопросами и ответами, в котором ответы содержатся на той же странице, сразу после каждого вопроса. 

Ниже показан пример простой страницы вопросов и ответов.

![Простая страница вопросов и ответов](../media/qnamaker-concepts-datasources/plain-faq.png) 

 
### <a name="faq-pages-with-links"></a>Страницы вопросов и ответов со ссылками 

В этом типе страниц с вопросами и ответами все вопросы собраны вместе и содержат ссылки на ответы, расположенные либо в разных разделах на той же странице, либо на разных страницах.

Ниже показан пример страницы вопросов и ответов со ссылками на разделы, которые расположенные на той же странице.

 ![Страница вопросов и ответов со ссылками на разделы](../media/qnamaker-concepts-datasources/sectionlink-faq.png) 


### <a name="faq-pages-with-a-topics-homepage"></a>Страницы вопросов и ответов с темами на домашней странице

У этого типа вопросов и ответов есть домашняя страница с темами, где каждая тема — ссылка на соответствующие вопросы и ответы на другой странице. Здесь QnA Maker сканирует все связанные страницы, чтобы извлечь соответствующие вопросы и ответы.

Ниже приведен пример страницы вопросов и ответов, в которой темы домашней страницы имеют ссылки на разделы вопросов и ответов с разных страниц. 

 ![Страница вопросов и ответов с прямыми ссылками](../media/qnamaker-concepts-datasources/topics-faq.png) 


## <a name="pdf-doc-files"></a>PDF- или DOC-файлы

QnA Maker может обрабатывать слабоструктурированное содержимое в PDF- или DOC-файле и преобразовывать его в вопросы и ответы. Хороший файл, который можно успешно извлечь, — это тот, где содержимое организовано в некоторой структурированной форме и представлено в четко определенных разделах. Разделы могут быть дополнительно разбиты на подразделы или подтемы. Извлечение лучше всего работает с документами, которые имеют четкую структуру с иерархической структурой заголовков.

QnA Maker идентифицирует разделы, подразделы и отношения в файле на основе визуальных признаков, таких как размер шрифта, начертание шрифта, нумерация, цвета и т.д. Слабоструктурированные PDF- или DOC-файлы могут быть руководствами, часто задаваемыми вопросами, рекомендациями, политиками, брошюрами, объявлениями и многими другими типами файлов. Ниже приведены несколько примеров типов этих файлов.

### <a name="product-manuals"></a>Руководства по продукции

Руководством обычно называются справочные материалы, входящие в комплект поставки продукта. Они помогают пользователям устанавливать, использовать, обслуживать продукт и устранять возможные неполадки. Когда QnA Maker обрабатывает руководство, он извлекает заголовки и подзаголовки в качестве вопросов, а последующее содержимое — в качестве ответов. Пример вы можете найти [здесь](https://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

Ниже приведен пример руководства со страницей индексов и иерархическим содержимым

 ![Пример руководства по продукту](../media/qnamaker-concepts-datasources/product-manual.png) 

> [!NOTE]
> Извлечение лучше всего работает с руководствами с отдельным содержанием и (или) страницей индексов и хорошей иерархической структурой заголовков.

### <a name="brochures-guidelines-papers-and-other-files"></a>Брошюры, рекомендации, документы и другие файлы

Многие другие типы документов также могут быть обработаны для создания пар вопросов и ответов при условии, что они имеют четкую структуру и макет. К ним относятся: брошюры, рекомендации, отчеты, технические документы, научные статьи, политики, книги и т. д. Пример вы можете найти [здесь](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx).

Ниже приведен пример слабоструктурированного документа без индекса:

 ![Слабоструктурированный документ хранилища BLOB-объектов](../media/qnamaker-concepts-datasources/semi-structured-doc.png) 

### <a name="structured-qna-document"></a>Документ структурированных вопросов и ответов

Формат структурированных "вопросов — ответов" в DOC-файлах представлен в чередующемся формате — по одному вопросу в строке, за которым следует ответ в следующей строке, как показано ниже. 

```
Question1

Answer1

Question2

Answer2
```

Ниже приведен пример структурированного документа Word вопросов и ответов.

 ![Документ структурированных вопросов и ответов](../media/qnamaker-concepts-datasources/structured-qna-doc.png) 

## <a name="structured-txt-tsv-and-xls-files"></a>Структурированные *TXT*, *TSV* и *XLS* файлы

Вопросы и ответы в форме структурированных файлов в формате *TXT*, *TSV* или *XLS* также могут быть загружены в QnA Maker для создания или расширения базы знаний.  Они могут быть либо обычным текстом, либо иметь содержимое в формате RTF или HTML. 

| Вопрос  | Ответ  | Метаданные                |
|-----------|---------|-------------------------|
| Question1 | Answer1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 |      `Key:Value`           |

Все остальные столбцы в исходном файле игнорируются.

Ниже приведен пример структурированных вопросов и ответов файла формата *XLS* с содержимым HTML

 ![Структурированные вопросы и ответы в Excel](../media/qnamaker-concepts-datasources/structured-qna-xls.png)

## <a name="structured-data-format-through-import"></a>Загрузка данных в структурированном формате через импорт

При импорте базы знаний содержимое существующей базы знаний полностью заменяется. Для импорта нужно предоставить структурированный TSV-файл с информацией об источнике данных. Эти сведения помогают QnA Maker сгруппировать пары "вопрос — ответ" и сопоставить их с конкретным источником данных.

| Вопрос  | Ответ  | Источник| Метаданные                |
|-----------|---------|----|---------------------|
| Question1 | Answer1 | Url1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 | Редактирование|    `Key:Value`       |

## <a name="editorially-add-to-knowledge-base"></a>Добавление в базу знаний в режиме редактирования

Если у вас нет готового содержимого для заполнения базы знаний, вы можете добавить вопросы и ответы в режиме редактирования прямо в базу знаний QnA Maker. Узнайте, как [обновить базу знаний](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Настройка службы QnA Maker](../How-To/set-up-qnamaker-service-azure.md)

## <a name="see-also"></a>См. также 

[Общие сведения о QnA Maker](../Overview/overview.md)
