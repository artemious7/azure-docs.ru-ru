---
title: Сведенья о функциях языка определения рабочих процессов в Azure Logic Apps | Документация Майкрософт
description: Сведения о функциях языка определения рабочих процессов в Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: reference
ms.date: 08/15/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 8a2e06d2e6cf3e470d4e0909e5559ac0411292fd
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/30/2018
ms.locfileid: "43307119"
---
# <a name="functions-reference-for-workflow-definition-language-in-azure-logic-apps"></a>Сведенья о функциях языка определения рабочих процессов в Azure Logic Apps

Некоторые [выражения](../logic-apps/logic-apps-workflow-definition-language.md#expressions) в [Azure Logic Apps](../logic-apps/logic-apps-overview.md) получают значения из действий среды выполнения, которые могут не существовать в начале определения рабочего процесса приложения логики. Чтобы работать с этими значениями или ссылаться на них в выражениях, вы можете использовать *функции*, предоставляемые [языком определений рабочего процесса](../logic-apps/logic-apps-workflow-definition-language.md). Например, вы можете использовать математические функции для вычислений, такие как функция [add()](../logic-apps/workflow-definition-language-functions-reference.md#add), которая возвращает сумму из целых чисел или чисел с плавающей запятой. Вот еще несколько примеров задач, которые можно выполнять с помощью функций:

| Задача | Синтаксис функции | Результат | 
| ---- | --------------- | ------ | 
| Возвращает строку символов в нижнем регистре. | toLower('<*текст*>') <p>Например, toLower('Hello'). | "hello" | 
| Возвращение глобального уникального идентификатора (GUID). | guid() |"c2ecc88d-88c8-4096-912c-d6f2e2b138ce" | 
|||| 

В этой статье описываются функции, которые можно использовать при создании определений приложений логики.
Чтобы найти функции, [основанные на общем назначении](#ordered-by-purpose), продолжайте с помощью следующих таблиц. Или, подробные сведения о каждой функции см. в [алфавитном списке](#alphabetical-list). 

> [!NOTE]
> В синтаксисе для определения параметров знак вопроса (?), который расположен после параметра, означает, что параметр является необязательным. Примеры см. в разделе о [getFutureTime()](#getFutureTime).

## <a name="functions-in-expressions"></a>Функции в выражениях

Чтобы показать, как использовать функцию в выражении, в этом примере показано, как можно получить значение из параметра `customerName` и присвоить его свойству `accountName` с помощью функции [parameters()](#parameters) в выражении:

```json
"accountName": "@parameters('customerName')"
```

Вот некоторые другие общие способы использования функций в выражениях:

| Задача | Синтаксис функции в выражении | 
| ---- | -------------------------------- | 
| Выполнить операцию с элементом, передав этот элемент функции. | "\@<*имя_функции*>(<*элемент*>)" | 
| 1. Получить значение *параметра*, используя вложенную функцию `parameters()`. </br>2. Выполнить операцию с результатом, передав это значение *функции*. | "\@<*имя_функции*>(parameters('<*имя_параметра*>'))" | 
| 1. Получить результат из вложенной внутренней *функции*. </br>2. Передать результат внешней функции (*имя_функции2*). | "\@<*имя_функции2*>(<*имя_функции*>(<*элемент*>))" | 
| 1. Получить результат выполнения *функции*. </br>2. Учитывая, что результат является объектом со свойством *имя_свойства*, получить значение этого свойства. | "\@<*имя_функции*>(<*элемент*>).<*имя_свойства*>" | 
||| 

Например, функция `concat()` может принимать два или более строковых значения в качестве параметров. Эта функция объединяет эти строки в одну. Вы можете передать строковые литералы, например "Sophia" и "Owen", чтобы получить комбинированную строку "SophiaOwen":

```json
"customerName": "@concat('Sophia', 'Owen')"
```

Или можно получить строковые значения параметров. В этом примере используется функция `parameters()` в каждом параметре `concat()` и параметры `firstName` и `lastName`. Затем передайте полученные строки в функцию `concat()`, чтобы получить комбинированную строку, например "SophiaOwen":

```json
"customerName": "@concat(parameters('firstName'), parameters('lastName'))"
```

В любом случае в обоих примерах присвойте результат свойству `customerName`. 

Ниже приведены доступные функции, упорядоченные по общему назначению, или вы можете просмотреть функции в [алфавитном порядке](#alphabetical-list).

<a name="ordered-by-purpose"></a>
<a name="string-functions"></a>

## <a name="string-functions"></a>Строковые функции

Для работы со строками вы можете использовать эти строковые функции, а также некоторые [функции для коллекций](#collection-functions). Строковые функции работают только со строками. 

| Строковая функция | Задача | 
| --------------- | ---- | 
| [concat](../logic-apps/workflow-definition-language-functions-reference.md#concat) | Объединяет две или более строк и возвращает объединенную строку. | 
| [endsWith](../logic-apps/workflow-definition-language-functions-reference.md#endswith) | Проверяет, заканчивается ли строка определенной подстрокой. | 
| [guid](../logic-apps/workflow-definition-language-functions-reference.md#guid) | Создает глобально уникальный идентификатор (GUID) в виде строки. | 
| [indexOf](../logic-apps/workflow-definition-language-functions-reference.md#indexof) | Возвращает начальную позицию подстроки. | 
| [lastIndexOf](../logic-apps/workflow-definition-language-functions-reference.md#lastindexof) | Возвращает начальную позицию последнего вхождения подстроки. | 
| [replace](../logic-apps/workflow-definition-language-functions-reference.md#replace) | Заменяет подстроку указанной строкой и возвращает обновленную строку. | 
| [split](../logic-apps/workflow-definition-language-functions-reference.md#split) | Возвращает массив, который содержит все символы из строки, отделенные разделителем. | 
| [startsWith](../logic-apps/workflow-definition-language-functions-reference.md#startswith) | Проверяет, начинается ли строка с определенной подстроки. | 
| [substring](../logic-apps/workflow-definition-language-functions-reference.md#substring) | Возвращает символы из строки, начиная с указанной позиции. | 
| [toLower](../logic-apps/workflow-definition-language-functions-reference.md#toLower) | Возвращает строку символов в нижнем регистре. | 
| [toUpper](../logic-apps/workflow-definition-language-functions-reference.md#toUpper) | Возвращает строку символов в верхнем регистре. | 
| [trim](../logic-apps/workflow-definition-language-functions-reference.md#trim) | Удаляет все начальные и конечные пробелы и возвращает обновленную строку. | 
||| 

<a name="collection-functions"></a>

## <a name="collection-functions"></a>Функции для коллекций

Для работы с коллекциями, обычно массивами, строками, а иногда и словарями, вы можете использовать следующие функции. 

| Функция для коллекций | Задача | 
| ------------------- | ---- | 
| [contains](../logic-apps/workflow-definition-language-functions-reference.md#contains) | Проверяет наличие определенного элемента в коллекции. |
| [empty](../logic-apps/workflow-definition-language-functions-reference.md#empty) | Проверяет, является ли коллекция пустой. | 
| [first](../logic-apps/workflow-definition-language-functions-reference.md#first) | Возвращает первый элемент из коллекции. | 
| [intersection](../logic-apps/workflow-definition-language-functions-reference.md#intersection) | Возвращает коллекцию, которая содержит *только* общие элементы в указанных коллекциях. | 
| [join](../logic-apps/workflow-definition-language-functions-reference.md#join) | Возвращает строку, содержащую *все* элементы из массива, в которой каждый символ отделен разделителем. | 
| [last](../logic-apps/workflow-definition-language-functions-reference.md#last) | Возвращает последний элемент из коллекции. | 
| [длина](../logic-apps/workflow-definition-language-functions-reference.md#length) | Возвращает число элементов в строке или массиве. | 
| [skip](../logic-apps/workflow-definition-language-functions-reference.md#skip) | Удаляет элементы из начала коллекции и возвращает *все другие элементы*. | 
| [take](../logic-apps/workflow-definition-language-functions-reference.md#take) | Возвращает элементы, расположенные в начале коллекции. | 
| [union](../logic-apps/workflow-definition-language-functions-reference.md#union) | Возвращает коллекцию, которая содержит *все* элементы из указанных коллекций. | 
||| 

<a name="comparison-functions"></a>

## <a name="logical-comparison-functions"></a>Функции логического сравнения

Чтобы работать с условиями, сравнивать значения и результаты выражений или оценивать различные типы логики, можно использовать следующие функции логического сравнения. Подробные сведения о каждой функции см. в [алфавитном списке](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

| Функция логического сравнения | Задача | 
| --------------------------- | ---- | 
| [and](../logic-apps/workflow-definition-language-functions-reference.md#and) (и); | Проверяет, истинны ли все выражения. | 
| [equals](../logic-apps/workflow-definition-language-functions-reference.md#equals) | Проверяет, эквивалентны ли оба значения. | 
| [greater](../logic-apps/workflow-definition-language-functions-reference.md#greater) | Проверяет, является ли первое значение большим, чем второе. | 
| [greaterOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#greaterOrEquals) | Проверяет, является ли первое значение большим, чем второе, или равным ему. | 
| [if](../logic-apps/workflow-definition-language-functions-reference.md#if) (если); | Проверьте, какое значение имеет выражение: true или false. Возвращает указанное значение на основе результата. | 
| [less](../logic-apps/workflow-definition-language-functions-reference.md#less) | Проверяет, является ли первое значение меньшим, чем второе. | 
| [lessOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#lessOrEquals) | Проверяет, является ли первое значение меньшим, чем второе, или равным ему. | 
| [not](../logic-apps/workflow-definition-language-functions-reference.md#not) (не); | Проверяет, имеет ли выражение значение false. | 
| [or](../logic-apps/workflow-definition-language-functions-reference.md#or) (или). | Проверяет, является ли хотя бы одно выражение истинным. |
||| 

<a name="conversion-functions"></a>

## <a name="conversion-functions"></a>Функции преобразования

Чтобы изменить тип или формат значения, можно использовать приведенные ниже функции преобразования. Например, вы можете изменить значение с логического на целочисленное. Чтобы узнать, как приложения логики обрабатывают типы содержимого во время преобразования, см. статью [Обработка типов содержимого в приложениях логики](../logic-apps/logic-apps-content-type.md). Подробные сведения о каждой функции см. в [алфавитном списке](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

| Функция преобразования | Задача | 
| ------------------- | ---- | 
| [array](../logic-apps/workflow-definition-language-functions-reference.md#array). | Возвращает массив из одного экземпляра указанных входных данных. Для использования нескольких входных данных см. раздел [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray). | 
| [base64](../logic-apps/workflow-definition-language-functions-reference.md#base64) | Возвращает версию строки с кодировкой base64 для заданной строки. | 
| [base64ToBinary](../logic-apps/workflow-definition-language-functions-reference.md#base64ToBinary) | Возвращает двоичную версию строки с кодировкой base64. | 
| [base64ToString](../logic-apps/workflow-definition-language-functions-reference.md#base64ToString) | Возвращает строковую версию строки с кодировкой base64. | 
| [binary](../logic-apps/workflow-definition-language-functions-reference.md#binary) | Возвращает двоичную версию входного значения. | 
| [bool](../logic-apps/workflow-definition-language-functions-reference.md#bool); | Возвращает логическую версию входного значения. | 
| [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray) | Возвращает массив из нескольких экземпляров входных данных. | 
| [dataUri](../logic-apps/workflow-definition-language-functions-reference.md#dataUri) | Возвращает URI данных входного значения. | 
| [dataUriToBinary](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToBinary) | Возвращает двоичную версию строки URI данных. | 
| [dataUriToString](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToString) | Возвращает строковую версию URI данных. | 
| [decodeBase64](../logic-apps/workflow-definition-language-functions-reference.md#decodeBase64) | Возвращает строковую версию строки с кодировкой base64. | 
| [decodeDataUri](../logic-apps/workflow-definition-language-functions-reference.md#decodeDataUri) | Возвращает двоичную версию строки URI данных. | 
| [decodeUriComponent](../logic-apps/workflow-definition-language-functions-reference.md#decodeUriComponent) | Возвращает строку, которая заменяет escape-символы декодированными версиями. | 
| [encodeUriComponent](../logic-apps/workflow-definition-language-functions-reference.md#encodeUriComponent) | Возвращает строку, которая заменяет символы, опасные для URL-адреса, escape-символами. | 
| [float](../logic-apps/workflow-definition-language-functions-reference.md#float) | Возвращает значение с плавающей запятой в качестве входного значения. | 
| [int](../logic-apps/workflow-definition-language-functions-reference.md#int) | Возвращает целочисленную версию строки. | 
| [json](../logic-apps/workflow-definition-language-functions-reference.md#json) | Возвращает значение типа JSON либо объект для строки или XML. | 
| [string](../logic-apps/workflow-definition-language-functions-reference.md#string) | Возвращает строковую версию входного значения. | 
| [uriComponent](../logic-apps/workflow-definition-language-functions-reference.md#uriComponent) | Возвращает кодированную версию URI для входного значения, заменив символы, опасные для URL-адреса, на escape-символы. | 
| [uriComponentToBinary](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToBinary) | Возвращает двоичную версию строки с закодированным URI. | 
| [uriComponentToString](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToString) | Возвращает строковую версию строки с закодированным URI. | 
| [xml](../logic-apps/workflow-definition-language-functions-reference.md#xml) | Возвращает XML-версию строки. | 
||| 

<a name="math-functions"></a>

## <a name="math-functions"></a>Математические функции

Для работы с целыми числами и числами с плавающей запятой можно использовать следующие математические функции. Подробные сведения о каждой функции см. в [алфавитном списке](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

| Математическая функция | Задача | 
| ------------- | ---- | 
| [добавление](../logic-apps/workflow-definition-language-functions-reference.md#add) | Возвращает результат сложения двух чисел. | 
| [div](../logic-apps/workflow-definition-language-functions-reference.md#div) | Возвращает результат деления двух чисел. | 
| [max](../logic-apps/workflow-definition-language-functions-reference.md#max) | Возвращает наибольшее значение из набора чисел или массива. | 
| [min](../logic-apps/workflow-definition-language-functions-reference.md#min) | Возвращает наименьшее значение из набора чисел или массива. | 
| [mod (модуль)](../logic-apps/workflow-definition-language-functions-reference.md#mod) | Возвращает остаток результата деления двух чисел. | 
| [mul](../logic-apps/workflow-definition-language-functions-reference.md#mul) | Возвращает результат умножения двух чисел. | 
| [rand](../logic-apps/workflow-definition-language-functions-reference.md#rand) | Возвращает случайное целое число из указанного диапазона. | 
| [range](../logic-apps/workflow-definition-language-functions-reference.md#range) | Возвращает массив целых чисел, который начинается с заданного целого числа. | 
| [sub](../logic-apps/workflow-definition-language-functions-reference.md#sub) | Вычитает второе число из первого числа и возвращает результат. | 
||| 

<a name="date-time-functions"></a>

## <a name="date-and-time-functions"></a>Функции даты и времени

Чтобы работать с датами и временем, вы можете использовать эти функции даты и времени.
Подробные сведения о каждой функции см. в [алфавитном списке](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

| Функция даты и времени | Задача | 
| --------------------- | ---- | 
| [addDays](../logic-apps/workflow-definition-language-functions-reference.md#addDays) | Добавляет количество дней к метке времени. | 
| [addHours](../logic-apps/workflow-definition-language-functions-reference.md#addHours) | Добавляет количество часов к метке времени. | 
| [addMinutes](../logic-apps/workflow-definition-language-functions-reference.md#addMinutes) | Добавляет количество минут к метке времени. | 
| [addSeconds](../logic-apps/workflow-definition-language-functions-reference.md#addSeconds) | Добавляет количество секунд к метке времени. |  
| [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime) | Добавляет количество единиц времени к метке времени. См. раздел [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime). | 
| [convertFromUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertFromUtc) | Преобразовывает метку времени формата UTC в целевой часовой пояс. | 
| [convertTimeZone](../logic-apps/workflow-definition-language-functions-reference.md#convertTimeZone) | Преобразовывает метку времени из исходного часового пояса в целевой. | 
| [convertToUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertToUtc) | Преобразует метку времени с исходным часовым поясом в формат UTC. | 
| [dayOfMonth](../logic-apps/workflow-definition-language-functions-reference.md#dayOfMonth) | Возвращает компонент дня месяца из метки времени. | 
| [dayOfWeek](../logic-apps/workflow-definition-language-functions-reference.md#dayOfWeek) | Возвращает компонент дня недели из метки времени. | 
| [dayOfYear](../logic-apps/workflow-definition-language-functions-reference.md#dayOfYear) | Возвращает компонент дня года из метки времени. | 
| [formatDateTime](../logic-apps/workflow-definition-language-functions-reference.md#formatDateTime) | Возвращает дату из метки времени. | 
| [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime) | Возвращает текущую метку времени, а также указанные единицы времени. См. раздел [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime). | 
| [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime) | Возвращает текущую метку времени, вычитая указанные единицы времени. См. раздел [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime). | 
| [startOfDay](../logic-apps/workflow-definition-language-functions-reference.md#startOfDay) | Возвращает начало дня для метки времени. | 
| [startOfHour](../logic-apps/workflow-definition-language-functions-reference.md#startOfHour) | Возвращает начало часа для метки времени. | 
| [startOfMonth](../logic-apps/workflow-definition-language-functions-reference.md#startOfMonth) | Возвращает начало месяца для метки времени. | 
| [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime) | Вычитает количество единиц времени из метки времени. См. раздел [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime). | 
| [ticks](../logic-apps/workflow-definition-language-functions-reference.md#ticks) | Возвращает значение свойства `ticks` для указанной метки времени. | 
| [utcNow](../logic-apps/workflow-definition-language-functions-reference.md#utcNow) | Возвращает текущую метку времени в виде строки. | 
||| 

<a name="workflow-functions"></a>

## <a name="workflow-functions"></a>Функции рабочего процесса

Эти функции рабочего процесса могут помочь:

* получить сведения об экземпляре рабочего процесса во время выполнения; 
* работать с входными данными, используемыми при создании экземпляра приложения логики;
* ссылаться на выходные данные из триггеров и действий.

Например, вы можете ссылаться на выходные данные одного действия и использовать их в действии, которое выполняется позже. Подробные сведения о каждой функции см. в [алфавитном списке](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

| Функция рабочего процесса | Задача | 
| ----------------- | ---- | 
| [action](../logic-apps/workflow-definition-language-functions-reference.md#action) | Возвращает выходные данные текущего действия во время выполнения или значения из других пар "имя — значение" JSON. См. раздел об [actions](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody) | Возвращает результат действия `body` во время выполнения. См. раздел [body](../logic-apps/workflow-definition-language-functions-reference.md#body). | 
| [actionOutputs](../logic-apps/workflow-definition-language-functions-reference.md#actionOutputs) | Возвращает результат действия во время выполнения. См. раздел [actions](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [actions](../logic-apps/workflow-definition-language-functions-reference.md#actions) | Возвращает выходные данные действия во время выполнения или значения из других пар "имя — значение" JSON. См. раздел [action](../logic-apps/workflow-definition-language-functions-reference.md#action).  | 
| [body](#body) | Возвращает результат действия `body` во время выполнения. См. раздел [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody). | 
| [formDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#formDataMultiValues) | Создает массив значений, которые соответствуют имени ключа в выходных данных *form-data* или *form-encoded* действия. | 
| [formDataValue](../logic-apps/workflow-definition-language-functions-reference.md#formDataValue) | Возвращает значение, которое соответствует имени ключа в выходных данных *form-data* или *form-encoded* действия. | 
| [Item](../logic-apps/workflow-definition-language-functions-reference.md#item) | При использовании внутри повторяющегося действия в массиве возвращает текущий элемент массива во время текущей итерации действия. | 
| [items](../logic-apps/workflow-definition-language-functions-reference.md#items) | При использовании внутри цикла for-each или do-until возвращает текущий элемент из указанного цикла.| 
| [listCallbackUrl](../logic-apps/workflow-definition-language-functions-reference.md#listCallbackUrl) | Возвращает URL-адрес обратного вызова, который вызывает триггер или действие. | 
| [multipartBody](../logic-apps/workflow-definition-language-functions-reference.md#multipartBody) | Возвращает текст указанной части выходных данных действия, которые состоят из нескольких частей. | 
| [parameters](../logic-apps/workflow-definition-language-functions-reference.md#parameters) | Возвращает значение параметра, описанного в определении вашего приложения логики. | 
| [trigger](../logic-apps/workflow-definition-language-functions-reference.md#trigger) | Возвращает выходные данные триггера во время выполнения или значения из других пар "имя — значение" JSON. См. разделы [triggerOutputs](#triggerOutputs) и [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody). | 
| [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody) | Возвращает выходные данные `body` триггера во время выполнения. См. раздел [trigger](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [triggerFormDataValue](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataValue) | Возвращает одно значение, соответствующее имени ключа, из выходных данных *form-data* или *form-encoded* триггера. | 
| [triggerMultipartBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerMultipartBody) | Возвращает текст указанной части выходных данных триггера, которые состоят из нескольких частей. | 
| [triggerFormDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataMultiValues) | Создает массив значений, которые соответствуют имени ключа в выходных данных *form-data* или *form-encoded* триггера. | 
| [triggerOutputs](../logic-apps/workflow-definition-language-functions-reference.md#triggerOutputs) | Возвращает выходные данные триггера во время выполнения или значения из других пар "имя — значение" JSON. См. раздел [trigger](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [variables](../logic-apps/workflow-definition-language-functions-reference.md#variables) | Возвращает значение для указанной переменной. | 
| [workflow](../logic-apps/workflow-definition-language-functions-reference.md#workflow) | Возвращает все сведения о самом рабочем процессе во время выполнения. | 
||| 

<a name="uri-parsing-functions"></a>

## <a name="uri-parsing-functions"></a>Функции анализа URI

Чтобы работать с идентификаторами URI и получать различные значения их свойств, вы можете использовать эти функции анализа URI. Подробные сведения о каждой функции см. в [алфавитном списке](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

| Функция анализа URI | Задача | 
| -------------------- | ---- | 
| [uriHost](../logic-apps/workflow-definition-language-functions-reference.md#uriHost) | Возвращает значение `host` для URI. | 
| [uriPath](../logic-apps/workflow-definition-language-functions-reference.md#uriPath) | Возвращает значение `path` для URI. | 
| [uriPathAndQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriPathAndQuery) | Возвращает значения `path` и `query` для URI. | 
| [uriPort](../logic-apps/workflow-definition-language-functions-reference.md#uriPort) | Возвращает значение `port` для URI. | 
| [uriQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriQuery) | Возвращает значение `query` для URI. | 
| [uriScheme](../logic-apps/workflow-definition-language-functions-reference.md#uriScheme) | Возвращает значение `scheme` для URI. | 
||| 

<a name="manipulation-functions"></a>

## <a name="manipulation-functions-json--xml"></a>Функции обработки. JSON и язык XML

Для работы с объектами JSON и узлами XML вы можете использовать следующие функции обработки. Подробные сведения о каждой функции см. в [алфавитном списке](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

| Функция обработки | Задача | 
| --------------------- | ---- | 
| [addProperty](../logic-apps/workflow-definition-language-functions-reference.md#addProperty) | Добавляет свойство и его значения или пару "имя — значение" в объект JSON и возвращает обновленный объект. | 
| [coalesce](../logic-apps/workflow-definition-language-functions-reference.md#coalesce) | Возвращает первое ненулевое значение из одного или нескольких параметров. | 
| [removeProperty](../logic-apps/workflow-definition-language-functions-reference.md#removeProperty) | Удаляет свойство из объекта JSON и возвращает обновленный объект. | 
| [setProperty](../logic-apps/workflow-definition-language-functions-reference.md#setProperty) | Задает значение свойства объекта JSON и возвращает обновленный объект. | 
| [xpath](../logic-apps/workflow-definition-language-functions-reference.md#xpath) | Проверяет XML на наличие узлов или значений, которые соответствуют выражению XPath, и возвращает соответствующие узлы или значения. | 
||| 

<a name="alphabetical-list"></a>
<a name="action"></a>

### <a name="action"></a>action

Возвращает *текущие* выходные данные действия во время выполнения или значения из других пар "имя — значение" JSON, которые вы можете назначить выражению. По умолчанию эта функция ссылается на весь объект действия, но вы можете дополнительно указать свойство, значение которого требуется. См. раздел об [actions()](../logic-apps/workflow-definition-language-functions-reference.md#actions).

Вы можете использовать функцию `action()` только со следующими объектами: 

* Свойство `unsubscribe` для действия веб-перехватчика, чтобы получить результат из исходного запроса `subscribe`.
* Свойство `trackedProperties` для действия.
* Условие цикла `do-until` для действия.

```
action()
action().outputs.body.<property> 
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*property*> | Нет  | Строка | Имя свойства объекта действия, значение которого требуется: **name**, **startTime**, **endTime**, **inputs**, **outputs**, **status**, **code**, **trackingId** и **clientTrackingId**. На портале Azure вы можете найти эти свойства, просмотрев подробности в конкретном журнале выполнения. Дополнительные сведения см. в статье с [действиями запуска рабочего процесса в REST API](https://docs.microsoft.com/rest/api/logic/workflowrunactions/get). | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | -----| ----------- | 
| <*выходные_данные_действия*> | Строка | Выходные данные текущего действия или свойства | 
|||| 

<a name="actionBody"></a>

### <a name="actionbody"></a>actionBody

Возвращает результат действия `body` во время выполнения. Сокращение для `actions('<actionName>').outputs.body`. См. разделы о [body()](#body) и [actions()](#actions).

```
actionBody('<actionName>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Yes | Строка | Имя действия для требуемого результата `body` | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | -----| ----------- | 
| <*выходные_данные_body_действия*> | Строка | Выходные данные `body` указанного действия | 
|||| 

*Пример*

В этом примере извлекаются выходные данные `body` действия Twitter `Get user`: 

```
actionBody('Get_user')
```

Возвращается такой результат:

```json
"body": {
  "FullName": "Contoso Corporation",
  "Location": "Generic Town, USA",
  "Id": 283541717,
  "UserName": "ContosoInc",
  "FollowersCount": 172,
  "Description": "Leading the way in transforming the digital workplace.",
  "StatusesCount": 93,
  "FriendsCount": 126,
  "FavouritesCount": 46,
  "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
}
```

<a name="actionOutputs"></a>

### <a name="actionoutputs"></a>actionOutputs

Возвращает результат действия во время выполнения. Сокращение для `actions('<actionName>').outputs`. См. раздел об [actions()](#actions).

```
actionOutputs('<actionName>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Yes | Строка | Имя действия для требуемого результата. | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | -----| ----------- | 
| <*выходные данные*> | Строка | Выходные данные указанного действия | 
|||| 

*Пример*

В этом примере извлекаются выходные данные действия Twitter `Get user`: 

```
actionOutputs('Get_user')
```

Возвращается такой результат:

```json
{ 
  "statusCode": 200,
  "headers": {
    "Pragma": "no-cache",
    "Vary": "Accept-Encoding",
    "x-ms-request-id": "a916ec8f52211265d98159adde2efe0b",
    "X-Content-Type-Options": "nosniff",
    "Timing-Allow-Origin": "*",
    "Cache-Control": "no-cache",
    "Date": "Mon, 09 Apr 2018 18:47:12 GMT",
    "Set-Cookie": "ARRAffinity=b9400932367ab5e3b6802e3d6158afffb12fcde8666715f5a5fbd4142d0f0b7d;Path=/;HttpOnly;Domain=twitter-wus.azconn-wus.p.azurewebsites.net",
    "X-AspNet-Version": "4.0.30319",
    "X-Powered-By": "ASP.NET",
    "Content-Type": "application/json; charset=utf-8",
    "Expires": "-1",
    "Content-Length": "339"
  },
  "body": {
    "FullName": "Contoso Corporation",
    "Location": "Generic Town, USA",
    "Id": 283541717,
    "UserName": "ContosoInc",
    "FollowersCount": 172,
    "Description": "Leading the way in transforming the digital workplace.",
    "StatusesCount": 93,
    "FriendsCount": 126,
    "FavouritesCount": 46,
    "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
  }
}
```

<a name="actions"></a>

### <a name="actions"></a>actions

Возвращает выходные данные действия во время выполнения или значения из других пар "имя — значение" JSON, которые вы можете назначить выражению. По умолчанию эта функция ссылается на весь объект действия, но вы можете дополнительно указать свойство, значение которого требуется. Сокращенные версии см. в разделах об [actionBody()](#actionBody), [actionOutputs()](#actionOutputs) и [body()](#body). Для текущего действия см. раздел об [()](#action).

> [!NOTE] 
> Раньше вы могли использовать функцию `actions()` или элемент `conditions` при указании того, что действие выполнялось на основе результата другого действия. Однако, чтобы явно объявить зависимости между действиями, вы должны использовать свойство `runAfter` зависимого действия. Дополнительные сведения о свойстве `runAfter` см. в статье [Схема языка определения рабочих процессов в Azure Logic Apps](../logic-apps/logic-apps-workflow-definition-language.md).

```
actions('<actionName>')
actions('<actionName>').outputs.body.<property> 
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Yes | Строка | Имя объекта действия, выходные данные которого требуется получить.  | 
| <*property*> | Нет  | Строка | Имя свойства объекта действия, значение которого требуется: **name**, **startTime**, **endTime**, **inputs**, **outputs**, **status**, **code**, **trackingId** и **clientTrackingId**. На портале Azure вы можете найти эти свойства, просмотрев подробности в конкретном журнале выполнения. Дополнительные сведения см. в статье с [действиями запуска рабочего процесса в REST API](https://docs.microsoft.com/rest/api/logic/workflowrunactions/get). | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | -----| ----------- | 
| <*выходные_данные_действия*> | Строка | Выходные данные указанного действия или свойства. | 
|||| 

*Пример*

В этом примере извлекаются значение свойства `status` из действия Twitter `Get user` во время выполнения: 

```
actions('Get_user').outputs.body.status 
```

Возвращается такой результат: `"Succeeded"`

<a name="add"></a>

### <a name="add"></a>добавление

Возвращает результат сложения двух чисел.

```
add(<summand_1>, <summand_2>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*summand_1*>, <*summand_2*> | Yes | Целое число, число с плавающей запятой или смешанного типа | Числа для добавления | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | -----| ----------- | 
| <*итоговая_сумма*> | Целое число и число с плавающей запятой | Возвращает результат сложения указанных чисел | 
|||| 

*Пример*

В этом примере добавляются указанные числа:

```
add(1, 1.5)
```

Возвращается такой результат: `2.5`

<a name="addDays"></a>

### <a name="adddays"></a>addDays

Добавляет количество дней к метке времени.

```
addDays('<timestamp>', <days>, '<format>'?)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*timestamp*> | Yes | Строка | Строка, содержащая метку времени | 
| <*days*> | Yes | Целое число  | Положительное или отрицательное число дней для добавления | 
| <*format*> | Нет  | Строка | Либо [один описатель формата](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), либо [пользовательский шаблон формата](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Формат по умолчанию для временной метки — ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (ГГГГ-ММ-ДДTЧЧ:ММ:СС:fffffffK), который соответствует [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) и сохраняет информацию о часовом поясе. |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*обновленная_метка_времени*> | Строка | Метка времени и указанное число дней  | 
|||| 

*Пример 1*

В этом примере к указанной метке времени добавляются 10 дней:

```
addDays('2018-03-15T13:00:00Z', 10)
```

Возвращается такой результат: `"2018-03-25T00:00:0000000Z"`

*Пример 2*

В этом примере от указанной метки времени отнимается пять дней:

```
addDays('2018-03-15T00:00:00Z', -5)
```

Возвращается такой результат: `"2018-03-10T00:00:0000000Z"`

<a name="addHours"></a>

### <a name="addhours"></a>addhours

Добавляет количество часов к метке времени.

```
addHours('<timestamp>', <hours>, '<format>'?)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*timestamp*> | Yes | Строка | Строка, содержащая метку времени | 
| <*hours*> | Yes | Целое число  | Положительное или отрицательное число часов для добавления | 
| <*format*> | Нет  | Строка | Либо [один описатель формата](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), либо [пользовательский шаблон формата](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Формат по умолчанию для временной метки — ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (ГГГГ-ММ-ДДTЧЧ:ММ:СС:fffffffK), который соответствует [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) и сохраняет информацию о часовом поясе. |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*обновленная_метка_времени*> | Строка | Метка времени и указанное число часов  | 
|||| 

*Пример 1*

В этом примере к указанной метке времени добавляется 10 часов:

```
addHours('2018-03-15T00:00:00Z', 10)
```

Возвращается такой результат: `"2018-03-15T10:00:0000000Z"`

*Пример 2*

В этом примере от указанной метки времени отнимается пять часов:

```
addHours('2018-03-15T15:00:00Z', -5)
```

Возвращается такой результат: `"2018-03-15T10:00:0000000Z"`

<a name="addMinutes"></a>

### <a name="addminutes"></a>addminutes

Добавляет количество минут к метке времени.

```
addMinutes('<timestamp>', <minutes>, '<format>'?)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*timestamp*> | Yes | Строка | Строка, содержащая метку времени | 
| <*minutes*> | Yes | Целое число  | Положительное или отрицательное число минут для добавления | 
| <*format*> | Нет  | Строка | Либо [один описатель формата](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), либо [пользовательский шаблон формата](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Формат по умолчанию для временной метки — ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (ГГГГ-ММ-ДДTЧЧ:ММ:СС:fffffffK), который соответствует [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) и сохраняет информацию о часовом поясе. |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*обновленная_метка_времени*> | Строка | Метка времени и указанное число минут | 
|||| 

*Пример 1*

В этом примере к указанной метке времени добавляется 10 минут:

```
addMinutes('2018-03-15T00:10:00Z', 10)
```

Возвращается такой результат: `"2018-03-15T00:20:00.0000000Z"`

*Пример 2*

В этом примере от указанной метки времени отнимается пять минут:

```
addMinutes('2018-03-15T00:20:00Z', -5)
```

Возвращается такой результат: `"2018-03-15T00:15:00.0000000Z"`

<a name="addProperty"></a>

### <a name="addproperty"></a>addProperty

Добавляет свойство и его значения или пару "имя — значение" в объект JSON и возвращает обновленный объект. Если объект уже существует во время выполнения, функция выдает ошибку.

```
addProperty(<object>, '<property>', <value>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*object*> | Yes | Объект. | Объект JSON, в который вы хотите добавить свойство | 
| <*property*> | Yes | Строка | Имя добавляемого свойства | 
| <*value*> | Yes | Любой | Значение для свойства |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*обновленный_объект*> | Объект. | Обновленный объект JSON с указанным свойством | 
|||| 

*Пример*

В этом примере свойство `accountNumber` добавляется в объект `customerProfile`, который преобразуется в JSON с помощью функции [JSON()](#json). Функция присваивает значение, которое формируется функцией [guid()](#guid), и возвращает обновленный объект:

```
addProperty(json('customerProfile'), 'accountNumber', guid())
```

<a name="addSeconds"></a>

### <a name="addseconds"></a>addseconds

Добавляет количество секунд к метке времени.

```
addSeconds('<timestamp>', <seconds>, '<format>'?)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*timestamp*> | Yes | Строка | Строка, содержащая метку времени | 
| <*seconds*> | Yes | Целое число  | Положительное или отрицательное число секунд для добавления | 
| <*format*> | Нет  | Строка | Либо [один описатель формата](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), либо [пользовательский шаблон формата](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Формат по умолчанию для временной метки — ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (ГГГГ-ММ-ДДTЧЧ:ММ:СС:fffffffK), который соответствует [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) и сохраняет информацию о часовом поясе. |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*обновленная_метка_времени*> | Строка | Метка времени и указанное число секунд  | 
|||| 

*Пример 1*

В этом примере к указанной метке времени добавляется 10 секунд:

```
addSeconds('2018-03-15T00:00:00Z', 10)
```

Возвращается такой результат: `"2018-03-15T00:00:10.0000000Z"`

*Пример 2*

В этом примере от указанной метки времени отнимается пять секунд:

```
addSeconds('2018-03-15T00:00:30Z', -5)
```

Возвращается такой результат: `"2018-03-15T00:00:25.0000000Z"`

<a name="addToTime"></a>

### <a name="addtotime"></a>addToTime

Добавляет количество единиц времени к метке времени. См. раздел [getFutureTime()](#getFutureTime).

```
addToTime('<timestamp>', <interval>, '<timeUnit>', '<format>'?)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*timestamp*> | Yes | Строка | Строка, содержащая метку времени | 
| <*interval*> | Yes | Целое число  | Число единиц времени для добавления | 
| <*timeUnit*> | Yes | Строка | Единицы времени для использования с *интервалом*: "секунда", "минута", "час", "день", "неделя", "месяц", "год" | 
| <*format*> | Нет  | Строка | Либо [один описатель формата](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), либо [пользовательский шаблон формата](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Формат по умолчанию для временной метки — ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (ГГГГ-ММ-ДДTЧЧ:ММ:СС:fffffffK), который соответствует [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) и сохраняет информацию о часовом поясе. |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*обновленная_метка_времени*> | Строка | Метка времени и указанное число единиц времени  | 
|||| 

*Пример 1*

В этом примере к указанной метке времени добавляется 1 день:

```
addToTime('2018-01-01T00:00:00Z', 1, 'Day') 
```

Возвращается такой результат: `"2018-01-02T00:00:00:0000000Z"`

*Пример 2*

В этом примере к указанной метке времени добавляется 1 день:

```
addToTime('2018-01-01T00:00:00Z', 1, 'Day', 'D')
```

И возвращается результат с использованием необязательного формата "D": `"Tuesday, January 2, 2018"`

<a name="and"></a>

### <a name="and"></a>и

Проверяет, истинны ли все выражения. Возвращает значение true, если все выражения имеют значения, и false, если хотя бы одно выражение имеет значение false.

```
and(<expression1>, <expression2>, ...)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*expression1*>, <*expression2*>, ... | Yes | Логическое | Выражения, которые следует проверить | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | -----| ----------- | 
| Значение true или false | Логическое | Возвращает значение true, если все выражения имеют значение true. Возвращает значение false, если хотя бы одно выражение имеет значение false. | 
|||| 

*Пример 1*

В этих примерах проверяется, все ли указанные логические значения верны:

```
and(true, true)
and(false, true)
and(false, false)
```

И возвращаются следующие результаты:

* В первом примере оба выражения условия выполнены, поэтому возвращается `true`. 
* Во втором примере одно из выражений имеет значение false, поэтому возвращается `false`.
* В третьем примере оба выражения имеют значения false, поэтому возвращается `false`.

*Пример 2*

В этих примерах проверяется, все ли указанные выражения верны:

```
and(equals(1, 1), equals(2, 2))
and(equals(1, 1), equals(1, 2))
and(equals(1, 2), equals(1, 3))
```

И возвращаются следующие результаты:

* В первом примере оба выражения условия выполнены, поэтому возвращается `true`. 
* Во втором примере одно из выражений имеет значение false, поэтому возвращается `false`.
* В третьем примере оба выражения имеют значения false, поэтому возвращается `false`.

<a name="array"></a>

### <a name="array"></a>array

Возвращает массив из одного экземпляра указанных входных данных. Для использования нескольких входных данных см. раздел [createArray()](#createArray). 

```
array('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | Строка для создания массива | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| [<*значение*>] | Массив, | Массив, содержащий один экземпляр указанных входных данных | 
|||| 

*Пример*

В этом примере создается массив из строки "hello":

```
array('hello')
```

Возвращается такой результат: `["hello"]`

<a name="base64"></a>

### <a name="base64"></a>base64

Возвращает версию строки с кодировкой base64 для заданной строки.

```
base64('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | Строка входных данных | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*строка_base64*> | Строка | Версия строки входных данных с кодировкой base64 | 
|||| 

*Пример*

В этом примере строка "hello" преобразуется в строку с кодировкой base64:

```
base64('hello')
```

Возвращается такой результат: `"aGVsbG8="`

<a name="base64ToBinary"></a>

### <a name="base64tobinary"></a>base64ToBinary

Возвращает двоичную версию строки с кодировкой base64.

```
base64ToBinary('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | Преобразуемая строка с кодировкой base64 | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*двоичная версия для строки base64*> | Строка | Двоичная версия строки с кодировкой base64 | 
|||| 

*Пример*

В этом примере строка "aGVsbG8=" в кодировке base64 преобразуется в двоичную строку:

```
base64ToBinary('aGVsbG8=')
```

Возвращается такой результат: 

`"0110000101000111010101100111001101100010010001110011100000111101"`

<a name="base64ToString"></a>

### <a name="base64tostring"></a>base64ToString

Возвращает декодированную версию строки с кодировкой base64. Используйте эту функцию вместо [decodeBase64()](#decodeBase64). Хотя обе функции работают одинаково, `base64ToString()` является предпочтительной.

```
base64ToString('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | Декодируемая строка с кодировкой base64. | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*декодированная строка base64*> | Строка | Строковая версия строки с кодировкой base64 | 
|||| 

*Пример*

В этом примере строка "aGVsbG8=" в кодировке base64 преобразуется в простую строку:

```
base64ToString('aGVsbG8=')
```

Возвращается такой результат: `"hello"`

<a name="binary"></a>

### <a name="binary"></a>binary; 

Возвращает двоичную версию строки.

```
binary('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | Преобразуемая строка | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*двоичная форма входного значения*> | Строка | Двоичная версия указанной строки | 
|||| 

*Пример*

В этом примере строка "hello" преобразуется в двоичную строку:

```
binary('hello')
```

Возвращается такой результат: 

`"0110100001100101011011000110110001101111"`

<a name="body"></a>

### <a name="body"></a>текст

Возвращает результат действия `body` во время выполнения. Сокращение для `actions('<actionName>').outputs.body`. См. разделы [actionBody()](#actionBody) и [actions()](#actions).

```
body('<actionName>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Yes | Строка | Имя действия для требуемого результата `body` | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | -----| ----------- | 
| <*выходные_данные_body_действия*> | Строка | Выходные данные `body` указанного действия | 
|||| 

*Пример*

В этом примере извлекаются выходные данные `body` из действия Twitter `Get user`: 

```
body('Get_user')
```

Возвращается такой результат: 

```json
"body": {
    "FullName": "Contoso Corporation",
    "Location": "Generic Town, USA",
    "Id": 283541717,
    "UserName": "ContosoInc",
    "FollowersCount": 172,
    "Description": "Leading the way in transforming the digital workplace.",
    "StatusesCount": 93,
    "FriendsCount": 126,
    "FavouritesCount": 46,
    "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
}
```

<a name="bool"></a>

### <a name="bool"></a>bool

Возвращает логическую версию значения.

```
bool(<value>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Любой | Значение, которое необходимо преобразовать | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| Значение true или false | Логическое | Логическая версия указанного значения | 
|||| 

*Пример*

В этих примерах указанные значения преобразуются в логические: 

```
bool(1)
bool(0)
```

И возвращаются следующие результаты: 

* Первый пример: `true`. 
* Второй пример: `false`.

<a name="coalesce"></a>

### <a name="coalesce"></a>coalesce

Возвращает первое ненулевое значение из одного или нескольких параметров. Пустые строки, пустые массивы и пустые объекты не имеют значение null.

```
coalesce(<object_1>, <object_2>, ...)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*object_1*>, <*object_2*>, ... | Yes | Любой, могут быть разные типы | Один или несколько элементов для проверки на наличие значения NULL | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*первый не нулевой элемент*> | Любой | Первый элемент или значение, которое не равно NULL. Если все параметры равны NULL, эта функция возвращает значение NULL. | 
|||| 

*Пример*

Эти примеры возвращают первое значение, не равное NULL, из указанных значений или равное NULL, когда все значения равны NULL:

```
coalesce(null, true, false)
coalesce(null, 'hello', 'world')
coalesce(null, null, null)
```

И возвращаются следующие результаты: 

* Первый пример: `true`. 
* Второй пример: `"hello"`.
* Третий пример: `null`.

<a name="concat"></a>

### <a name="concat"></a>concat

Объединяет две или более строк и возвращает объединенную строку. 

```
concat('<text1>', '<text2>', ...)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*text1*>, <*text2*>, ... | Yes | Строка | По крайней мере две строки для объединения | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*текст1текст2...*> | Строка | Строка, созданная из объединенных входных строк | 
|||| 

*Пример*

В этом примере объединяются строки "Hello" и "World":

```
concat('Hello', 'World')
```

Возвращается такой результат: `"HelloWorld"`

<a name="contains"></a>

### <a name="contains"></a>contains

Проверяет наличие определенного элемента в коллекции. Возвращает true, когда элемент найден, и false, когда не найден. Эта функция учитывает регистр.

```
contains('<collection>', '<value>')
contains([<collection>], '<value>')
```

В частности, эта функция работает с такими типами коллекций: 

* *строка*, чтобы найти *подстроку*.;
* *массив*, чтобы найти *значение*;
* *словарь*, чтобы найти *ключ*.

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*collection*> | Yes | Строка, массив или словарь | Коллекция для проверки | 
| <*value*> | Yes | Строка, массив или словарь соответственно | Искомый элемент | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| Значение true или false | Логическое | Возвращает true, если элемент найден. В противном случае возвращает значение false. |
|||| 

*Пример 1*

В этом примере строка "hello world" проверяется на наличие подстроки "world" и возвращается значение true:

```
contains('hello world', 'world')
```

*Пример 2*

В этом примере строка "hello world" проверяется на наличие подстроки "universe" и возвращается значение false:

```
contains('hello world', 'universe')
```

<a name="convertFromUtc"></a>

### <a name="convertfromutc"></a>convertFromUtc

Преобразовывает метку времени формата UTC в целевой часовой пояс.

```
convertFromUtc('<timestamp>', '<destinationTimeZone>', '<format>'?)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*timestamp*> | Yes | Строка | Строка, содержащая метку времени | 
| <*destinationTimeZone*> | Yes | Строка | Имя целевого часового пояса. Дополнительные сведения см. в разделе [идентификаторов часовых поясов](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*format*> | Нет  | Строка | Либо [один описатель формата](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), либо [пользовательский шаблон формата](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Формат по умолчанию для временной метки — ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (ГГГГ-ММ-ДДTЧЧ:ММ:СС:fffffffK), который соответствует [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) и сохраняет информацию о часовом поясе. |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*преобразованная метка времени*> | Строка | Метка времени, преобразованная в целевой часовой пояс | 
|||| 

*Пример 1*

В этом примере метка времени преобразуется в указанный часовой пояс: 

```
convertFromUtc('2018-01-01T08:00:00.0000000Z', 'Pacific Standard Time')
```

Возвращается такой результат: `"2018-01-01T00:00:00.0000000"`

*Пример 2*

В этом примере метка времени преобразуется в указанный часовой пояс и формат:

```
convertFromUtc('2018-01-01T08:00:00.0000000Z', 'Pacific Standard Time', 'D')
```

Возвращается такой результат: `"Monday, January 1, 2018"`

<a name="convertTimeZone"></a>

### <a name="converttimezone"></a>convertTimeZone

Преобразовывает метку времени из исходного часового пояса в целевой.

```
convertTimeZone('<timestamp>', '<sourceTimeZone>', '<destinationTimeZone>', '<format>'?)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*timestamp*> | Yes | Строка | Строка, содержащая метку времени | 
| <*sourceTimeZone*> | Yes | Строка | Имя исходного часового пояса. Дополнительные сведения см. в разделе [идентификаторов часовых поясов](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*destinationTimeZone*> | Yes | Строка | Имя целевого часового пояса. Дополнительные сведения см. в разделе [идентификаторов часовых поясов](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*format*> | Нет  | Строка | Либо [один описатель формата](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), либо [пользовательский шаблон формата](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Формат по умолчанию для временной метки — ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (ГГГГ-ММ-ДДTЧЧ:ММ:СС:fffffffK), который соответствует [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) и сохраняет информацию о часовом поясе. |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*преобразованная метка времени*> | Строка | Метка времени, преобразованная в целевой часовой пояс | 
|||| 

*Пример 1*

В этом примере исходный часовой пояс преобразуется в целевой: 

```
convertTimeZone('2018-01-01T08:00:00.0000000Z', 'UTC', 'Pacific Standard Time')
```

Возвращается такой результат: `"2018-01-01T00:00:00.0000000"`

*Пример 2*

В этом примере часовой пояс преобразуется в указанный часовой пояс и формат:

```
convertTimeZone('2018-01-01T80:00:00.0000000Z', 'UTC', 'Pacific Standard Time', 'D')
```

Возвращается такой результат: `"Monday, January 1, 2018"`

<a name="convertToUtc"></a>

### <a name="converttoutc"></a>convertToUtc

Преобразует метку времени с исходным часовым поясом в формат UTC.

```
convertToUtc('<timestamp>', '<sourceTimeZone>', '<format>'?)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*timestamp*> | Yes | Строка | Строка, содержащая метку времени | 
| <*sourceTimeZone*> | Yes | Строка | Имя исходного часового пояса. Дополнительные сведения см. в разделе [идентификаторов часовых поясов](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*format*> | Нет  | Строка | Либо [один описатель формата](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), либо [пользовательский шаблон формата](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Формат по умолчанию для временной метки — ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (ГГГГ-ММ-ДДTЧЧ:ММ:СС:fffffffK), который соответствует [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) и сохраняет информацию о часовом поясе. |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*преобразованная метка времени*> | Строка | Метка времени, которая преобразуются в формат UTC | 
|||| 

*Пример 1*

В этом примере метка времени преобразуется в формат UTC: 

```
convertToUtc('01/01/2018 00:00:00', 'Pacific Standard Time')
```

Возвращается такой результат: `"2018-01-01T08:00:00.0000000Z"`

*Пример 2*

В этом примере метка времени преобразуется в формат UTC:

```
convertToUtc('01/01/2018 00:00:00', 'Pacific Standard Time', 'D')
```

Возвращается такой результат: `"Monday, January 1, 2018"`

<a name="createArray"></a>

### <a name="createarray"></a>createArray

Возвращает массив из нескольких экземпляров входных данных. Массивы из одного экземпляра входных данных см. в разделе [array()](#array).

```
createArray('<object1>', '<object2>', ...)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*object1*>, <*object2*>, ... | Yes | Любой, но не смешанный | По крайней мере два элемента для создания массива | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| [<*объект1*>, <*объект2*>, ...] | Массив, | Массив, созданный из всех входных элементов | 
|||| 

*Пример*

В этом примере создается массив из этих входных элементов:

```
createArray('h', 'e', 'l', 'l', 'o')
```

Возвращается такой результат: `["h", "e", "l", "l", "o"]`

<a name="dataUri"></a>

### <a name="datauri"></a>dataUri

Возвращает URI данных для строки. 

```
dataUri('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | Преобразуемая строка | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*URI данных*> | Строка | URI данных для входной строки | 
|||| 

*Пример*

В этом примере создается URI данных для строки "hello":

```
dataUri('hello') 
```

Возвращается такой результат: `"data:text/plain;charset=utf-8;base64,aGVsbG8="`

<a name="dataUriToBinary"></a>

### <a name="datauritobinary"></a>dataUriToBinary

Возвращает двоичную версию URI данных. Используйте эту функцию вместо [decodeDataUri()](#decodeDataUri). Хотя обе функции работают одинаково, `decodeDataUri()` является предпочтительной.

```
dataUriToBinary('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | URI данных для преобразования | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*двоичная версия URI данных*> | Строка | Двоичная версия URI данных | 
|||| 

*Пример*

В этом примере создается двоичная версия этого URI данных:

```
dataUriToBinary('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

Возвращается такой результат: 

`"01100100011000010111010001100001001110100111010001100101011110000111010000101111011100000
1101100011000010110100101101110001110110110001101101000011000010111001001110011011001010111
0100001111010111010101110100011001100010110100111000001110110110001001100001011100110110010
10011011000110100001011000110000101000111010101100111001101100010010001110011100000111101"`

<a name="dataUriToString"></a>

### <a name="datauritostring"></a>dataUriToString

Возвращает строковую версию URI данных.

```
dataUriToString('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | URI данных для преобразования | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*строка URI данных*> | Строка | Строковая версия URI данных | 
|||| 

*Пример*

В этом примере создается строка этого URI данных:

```
dataUriToString('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

Возвращается такой результат: `"hello"`

<a name="dayOfMonth"></a>

### <a name="dayofmonth"></a>dayOfMonth

Возвращает день месяца из метки времени. 

```
dayOfMonth('<timestamp>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*timestamp*> | Yes | Строка | Строка, содержащая метку времени | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*день месяца*> | Целое число  | День месяца из указанной метки времени | 
|||| 

*Пример*

В этом примере возвращается число дня месяца из этой метки времени:

```
dayOfMonth('2018-03-15T13:27:36Z')
```

Возвращается такой результат: `15`

<a name="dayOfWeek"></a>

### <a name="dayofweek"></a>dayOfWeek

Возвращает день недели из метки времени.  

```
dayOfWeek('<timestamp>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*timestamp*> | Yes | Строка | Строка, содержащая метку времени | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*день недели*> | Целое число  | День недели из указанной метки времени, где воскресенье имеет значение 0, понедельник — 1 и т. д. | 
|||| 

*Пример*

В этом примере возвращается номер дня недели из этой метки времени:

```
dayOfWeek('2018-03-15T13:27:36Z')
```

Возвращается такой результат: `3`

<a name="dayOfYear"></a>

### <a name="dayofyear"></a>dayOfYear

Возвращает день года из метки времени. 

```
dayOfYear('<timestamp>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*timestamp*> | Yes | Строка | Строка, содержащая метку времени | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*день года*> | Целое число  | День года из указанной метки времени | 
|||| 

*Пример*

В этом примере возвращается число дня года из этой метки времени:

```
dayOfYear('2018-03-15T13:27:36Z')
```

Возвращается такой результат: `74`

<a name="decodeBase64"></a>

### <a name="decodebase64"></a>decodeBase64

Возвращает декодированную версию строки с кодировкой base64. Используйте [base64ToString()](#base64ToString) вместо `decodeBase64()`. Хотя обе функции работают одинаково, `base64ToString()` является предпочтительной.

```
decodeBase64('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | Декодируемая строка с кодировкой base64. | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*декодированная строка base64*> | Строка | Строковая версия строки с кодировкой base64 | 
|||| 

*Пример*

В этом примере создается строка для строки с кодировкой base64:

```
decodeBase64('aGVsbG8=')
```

Возвращается такой результат: `"hello"`

<a name="decodeDataUri"></a>

### <a name="decodedatauri"></a>decodeDataUri

Возвращает двоичную версию URI данных. Используйте [dataUriToBinary()](#dataUriToBinary) вместо `decodeDataUri()`. Хотя обе функции работают одинаково, `dataUriToBinary()` является предпочтительной.

```
decodeDataUri('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | Декодируемая строка URI данных | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*двоичная версия URI данных*> | Строка | Двоичная версия строки URI данных | 
|||| 

*Пример*

В этом примере возвращается двоичная версия этого URI данных:

```
decodeDataUri('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

Возвращается такой результат: 

`"01100100011000010111010001100001001110100111010001100101011110000111010000101111011100000
1101100011000010110100101101110001110110110001101101000011000010111001001110011011001010111
0100001111010111010101110100011001100010110100111000001110110110001001100001011100110110010
10011011000110100001011000110000101000111010101100111001101100010010001110011100000111101"`

<a name="decodeUriComponent"></a>

### <a name="decodeuricomponent"></a>decodeUriComponent

Возвращает строку, которая заменяет escape-символы декодированными версиями. 

```
decodeUriComponent('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | Строка с декодируемыми escape-символами | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*декодированный URI*> | Строка | Обновленная строка с декодированными escape-символами | 
|||| 

*Пример*

В этом примере escape-символы в этой строке заменяются декодированными версиями:

```
decodeUriComponent('http%3A%2F%2Fcontoso.com')
```

Возвращается такой результат: `"https://contoso.com"`

<a name="div"></a>

### <a name="div"></a>div

Возвращает целочисленный результат деления двух чисел. Чтобы получить остаток, см. раздел [mod()](#mod).

```
div(<dividend>, <divisor>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*dividend*> | Yes | Целое число и число с плавающей запятой | Число, которое нужно поделить на *делитель* | 
| <*divisor*> | Yes | Целое число и число с плавающей запятой | Число, на которое делится *делимое* (не может быть равно 0). | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*результат деления*> | Целое число  | Целое число в результате деления первого числа на второе | 
|||| 

*Пример*

В обоих примерах первое число делится на второе:

```
div(10, 5)
div(11, 5)
```

Возвращается результат: `2`

<a name="encodeUriComponent"></a>

### <a name="encodeuricomponent"></a>encodeUriComponent

Возвращает кодированную версию URI для строки, заменив символы, опасные для URL-адреса, на escape-символы. Используйте [uriComponent()](#uriComponent) вместо `encodeUriComponent()`. Хотя обе функции работают одинаково, `uriComponent()` является предпочтительной.

```
encodeUriComponent('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | Строка для преобразования в формат закодированного URI | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*закодированный URI*> | Строка | Строка с закодированным URI, содержащая escape-символы | 
|||| 

*Пример*

В этом примере создается версия с закодированным URI для этой строки:

```
encodeUriComponent('https://contoso.com')
```

Возвращается такой результат: `"http%3A%2F%2Fcontoso.com"`

<a name="empty"></a>

### <a name="empty"></a>empty

Проверяет, является ли коллекция пустой. Возвращает значение true, если коллекция пуста, или false, если нет.

```
empty('<collection>')
empty([<collection>])
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*collection*> | Yes | Строка, массив или объект | Коллекция для проверки | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| Значение true или false | Логическое | Возвращает значение true, если коллекция пуста. В противном случае возвращает значение false. | 
|||| 

*Пример* 

В этих примерах проверяется, являются ли указанные коллекции пустыми:

```
empty('')
empty('abc')
```

И возвращаются следующие результаты: 

* В первом примере передается пустая строка, поэтому функция возвращает `true`. 
* Во втором примере передается строка "abc", поэтому функция возвращает `false`. 

<a name="endswith"></a>

### <a name="endswith"></a>endsWith

Проверяет, заканчивается ли строка определенной подстрокой. Возвращает true, если подстрока найдена, или возвращает false, если нет. Эта функция не учитывает регистр.

```
endsWith('<text>', '<searchText>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*text*> | Yes | Строка | Проверяемая строка | 
| <*searchText*> | Yes | Строка | Конечная подстрока для поиска | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| Значение true или false  | Логическое | Возвращает значение true, если конечная подстрока обнаружена. В противном случае возвращает значение false. | 
|||| 

*Пример 1* 

В этом примере проверяется, заканчивается ли строка "hello world" строкой "world":

```
endsWith('hello world', 'world')
```

Возвращается такой результат: `true`

*Пример 2*

В этом примере проверяется, заканчивается ли строка "hello world" строкой "universe":

```
endsWith('hello world', 'universe')
```

Возвращается такой результат: `false`

<a name="equals"></a>

### <a name="equals"></a>equals (равно)

Проверяет, эквивалентны ли оба значения, выражения или объекта. Возвращается значение true, если они эквивалентны, или false, если нет.

```
equals('<object1>', '<object2>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*object1*>, <*object2*> | Yes | Различные | Значения, выражения или объекты для сравнения | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| Значение true или false | Логическое | Возвращает значение true, если оба значения эквивалентны. В противном случае возвращает значение false. | 
|||| 

*Пример*

В этих примерах проверяется, являются ли указанные входные данные эквивалентными. 

```
equals(true, 1)
equals('abc', 'abcd')
```

И возвращаются следующие результаты: 

* В первом примере оба значения эквивалентны, поэтому функция возвращает `true`.
* Во втором примере оба значения не эквивалентны, поэтому функция возвращает `false`.

<a name="first"></a>

### <a name="first"></a>first

Возвращает первый элемент из строки или массива.

```
first('<collection>')
first([<collection>])
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*collection*> | Yes | Строка или массив | Коллекция, где нужно найти первый элемент. |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*первый элемент коллекции*> | Любой | Первый элемент в коллекции | 
|||| 

*Пример*

В этих примерах выполняется поиск первого элемента в этих коллекциях:

```
first('hello')
first([0, 1, 2])
```

И возвращаются следующие результаты: 

* Первый пример: `"h"`.
* Второй пример: `0`.

<a name="float"></a>

### <a name="float"></a>float;

Преобразует строковую версию числа с плавающей запятой в фактическое число с плавающей запятой. Эта функция используется только при передаче пользовательских параметров в приложение, такое как приложение логики.

```
float('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | Строка с допустимым числом с плавающей запятой, которую нужно преобразовать |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*значение с плавающей запятой*> | Float | Число с плавающей запятой для указанной строки | 
|||| 

*Пример*

В этом примере создается строковая версия для этого числа с плавающей запятой:

```
float('10.333')
```

Возвращается такой результат: `10.333`

<a name="formatDateTime"></a>

### <a name="formatdatetime"></a>formatDateTime

Возвращает метку времени в указанном формате.

```
formatDateTime('<timestamp>', '<format>'?)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*timestamp*> | Yes | Строка | Строка, содержащая метку времени | 
| <*format*> | Нет  | Строка | Либо [один описатель формата](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), либо [пользовательский шаблон формата](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Формат по умолчанию для временной метки — ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (ГГГГ-ММ-ДДTЧЧ:ММ:СС:fffffffK), который соответствует [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) и сохраняет информацию о часовом поясе. |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*метка времени в другом формате*> | Строка | Обновленная метка времени в указанном формате | 
|||| 

*Пример*

В этом примере метка времени преобразуется в указанный формат:

```
formatDateTime('03/15/2018 12:00:00', 'yyyy-MM-ddTHH:mm:ss')
```

Возвращается такой результат: `"2018-03-15T12:00:00"`

<a name="formDataMultiValues"></a>

### <a name="formdatamultivalues"></a>formDataMultiValues

Возвращает массив значений, которые соответствуют имени ключа в выходных данных *form-data* или *form-encoded* действия. 

```
formDataMultiValues('<actionName>', '<key>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Yes | Строка | Действие, выходные данные которого содержат требуемое значение ключа | 
| <*key*> | Yes | Строка | Имя ключа, значение которого нужно получить | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| [<*массив со значениями ключа*>] | Массив, | Массив со всеми значениями, которые соответствуют заданному ключу | 
|||| 

*Пример* 

В этом примере создается массив из значения ключа "Subject" в выходных данных form-data или form-encoded указанного действия:  

```
formDataMultiValues('Send_an_email', 'Subject')
```

И возвращается текст темы в массиве, например: `["Hello world"]`.

<a name="formDataValue"></a>

### <a name="formdatavalue"></a>formDataValue

Возвращает значение, которое соответствует имени ключа в выходных данных *form-data* или *form-encoded* действия. Если функция обнаруживает более одного совпадения, она выдает ошибку.

```
formDataValue('<actionName>', '<key>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Yes | Строка | Действие, выходные данные которого содержат требуемое значение ключа | 
| <*key*> | Yes | Строка | Имя ключа, значение которого нужно получить |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*значение ключа*> | Строка | Значение в указанном ключе  | 
|||| 

*Пример* 

В этом примере создается строка из значения ключа "Subject" в выходных данных form-data или form-encoded указанного действия.  

```
formDataValue('Send_an_email', 'Subject')
```

И возвращается текст темы в виде строки, например: `"Hello world"`

<a name="getFutureTime"></a>

### <a name="getfuturetime"></a>getFutureTime

Возвращает текущую метку времени, а также указанные единицы времени.

```
getFutureTime(<interval>, <timeUnit>, <format>?)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*interval*> | Yes | Целое число  | Число единиц времени для вычитания | 
| <*timeUnit*> | Yes | Строка | Единицы времени для использования с *интервалом*: "секунда", "минута", "час", "день", "неделя", "месяц", "год" | 
| <*format*> | Нет  | Строка | Либо [один описатель формата](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), либо [пользовательский шаблон формата](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Формат по умолчанию для временной метки — ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (ГГГГ-ММ-ДДTЧЧ:ММ:СС:fffffffK), который соответствует [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) и сохраняет информацию о часовом поясе. | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*обновленная_метка_времени*> | Строка | Текущая метка времени и указанное число единиц времени | 
|||| 

*Пример 1*

Допустим, текущая метка времени такая: "2018-03-01T00:00:00.0000000Z". В этом примере к этой метке времени добавляется пять дней:

```
getFutureTime(5, 'Day')
```

Возвращается такой результат: `"2018-03-06T00:00:00.0000000Z"`

*Пример 2*

Допустим, текущая метка времени такая: "2018-03-01T00:00:00.0000000Z". В этом примере добавляется пять дней и результат преобразуется в формат "D":

```
getFutureTime(5, 'Day', 'D')
```

Возвращается такой результат: `"Tuesday, March 6, 2018"`

<a name="getPastTime"></a>

### <a name="getpasttime"></a>getPastTime

Возвращает текущую метку времени, вычитая указанные единицы времени.

```
getPastTime(<interval>, <timeUnit>, <format>?)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*interval*> | Yes | Целое число  | Число единиц времени для вычитания | 
| <*timeUnit*> | Yes | Строка | Единицы времени для использования с *интервалом*: "секунда", "минута", "час", "день", "неделя", "месяц", "год" | 
| <*format*> | Нет  | Строка | Либо [один описатель формата](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), либо [пользовательский шаблон формата](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Формат по умолчанию для временной метки — ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (ГГГГ-ММ-ДДTЧЧ:ММ:СС:fffffffK), который соответствует [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) и сохраняет информацию о часовом поясе. | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*обновленная_метка_времени*> | Строка | Текущая метка времени за вычетом указанного числа единиц времени | 
|||| 

*Пример 1*

Допустим, текущая метка времени такая: "2018-02-01T00:00:00.0000000Z". В этом примере от указанной метки времени отнимается пять дней.

```
getPastTime(5, 'Day')
```

Возвращается такой результат: `"2018-01-27T00:00:00.0000000Z"`

*Пример 2*

Допустим, текущая метка времени такая: "2018-02-01T00:00:00.0000000Z". В этом примере отнимается пять дней и результат преобразуется в формат "D":

```
getPastTime(5, 'Day', 'D')
```

Возвращается такой результат: `"Saturday, January 27, 2018"`

<a name="greater"></a>

### <a name="greater"></a>greater

Проверяет, является ли первое значение большим, чем второе. Возвращает значение true, если первое значение больше, или значение false, если меньше.

```
greater(<value>, <compareTo>)
greater('<value>', '<compareTo>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Целое число, число с плавающей запятой или строка | Проверяет, является ли первое значение большим, чем второе | 
| <*compareTo*> | Yes | Целое число, число с плавающей запятой или строка соответственно | Значение сравнения | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| Значение true или false | Логическое | Возвращает значение true, если первое значение большим, чем второе. Возвращает значение false, если первое значение меньше или равно второму. | 
|||| 

*Пример*

В этих примерах проверяется, является ли первое значение большим, чем второе.

```
greater(10, 5)
greater('apple', 'banana')
```

И возвращаются следующие результаты: 

* Первый пример: `true`.
* Второй пример: `false`.

<a name="greaterOrEquals"></a>

### <a name="greaterorequals"></a>greaterOrEquals

Проверяет, является ли первое значение большим, чем второе, или равным ему.
Возвращает true, если первое значение больше или равно, или значение false, если первое значение меньше.

```
greaterOrEquals(<value>, <compareTo>)
greaterOrEquals('<value>', '<compareTo>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Целое число, число с плавающей запятой или строка | Проверяет, является ли первое значение большим, чем второе, или равным ему | 
| <*compareTo*> | Yes | Целое число, число с плавающей запятой или строка соответственно | Значение сравнения | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| Значение true или false | Логическое | Возвращает значение true, если первое значение больше или равно второму. Возвращает значение false, если первое значение меньше, чем второе. | 
|||| 

*Пример*

В этих примерах проверяется, является ли первое значение большим чем второе, или равным ему.

```
greaterOrEquals(5, 5)
greaterOrEquals('apple', 'banana')
```

И возвращаются следующие результаты: 

* Первый пример: `true`.
* Второй пример: `false`.

<a name="guid"></a>

### <a name="guid"></a>GUID

Создает глобально уникальный идентификатор (GUID) в виде строки, например, "c2ecc88d-88c8-4096-912c-d6f2e2b138ce": 

```
guid()
```

Кроме того, вы можете указать другой формат для GUID, отличный от формата по умолчанию ("D"), который состоит из 32 цифр, разделенных дефисом.

```
guid('<format>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*format*> | Нет  | Строка | Один [указатель формата](https://msdn.microsoft.com/library/97af8hh4) для возвращенного идентификатора GUID. По умолчанию имеет формат "D", но можно использовать "N", "D", "B", "P" или "X". | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*значение GUID*> | Строка | Случайно сгенерированный GUID | 
|||| 

*Пример* 

В этом примере создается тот же идентификатор GUID, но в виде 32 цифр, разделенных дефисом и заключенных в круглые скобки: 

```
guid('P')
```

Возвращается такой результат: `"(c2ecc88d-88c8-4096-912c-d6f2e2b138ce)"`

<a name="if"></a>

### <a name="if"></a>if

Проверьте, какое значение имеет выражение: true или false. Возвращает указанное значение на основе результата.

```
if(<expression>, <valueIfTrue>, <valueIfFalse>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*expression*> | Yes | Логическое | Выражение для вычисления | 
| <*valueIfTrue*> | Yes | Любой | Возвращаемое значение, если выражение истинно | 
| <*valueIfFalse*> | Yes | Любой | Возвращаемое значение, если выражение ложно | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*указанное возвращаемое значение*> | Любой | Возвращает указанное значение в зависимости от результата выражения: true или false. | 
|||| 

*Пример* 

В этом примере возвращается `"yes"`, потому что указанное выражение возвращает значение true. В противном случае в примере возвращается `"no"`:

```
if(equals(1, 1), 'yes', 'no')
```

<a name="indexof"></a>

### <a name="indexof"></a>indexOf

Возвращает начальную позицию или значение индекса для подстроки. Эта функция не учитывает регистр, и индексы начинаются с числа 0. 

```
indexOf('<text>', '<searchText>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*text*> | Yes | Строка | Строки, которая содержит подстроку для поиска | 
| <*searchText*> | Yes | Строка | Подстрока для поиска | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*значение индекса*>| Целое число  | Начальное положение или значение индекса указанной подстроки. <p>Если строка не найдена, возвращается -1. | 
|||| 

*Пример* 

Этот пример находит начальное значение индекса для подстроки "world" в строке "hello world":

```
indexOf('hello world', 'world')
```

Возвращается такой результат: `6`

<a name="int"></a>

### <a name="int"></a>int

Возвращает целочисленную версию строки.

```
int('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | Преобразуемая строка | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*целочисленный результат*> | Целое число  | Целочисленная версия указанной строки | 
|||| 

*Пример* 

В этом примере создается целочисленная версия строки "10":

```
int('10')
```

Возвращается такой результат: `10`

<a name="item"></a>

### <a name="item"></a>item

При использовании внутри повторяющегося действия в массиве возвращает текущий элемент массива во время текущей итерации действия. Вы также можете получить значения свойств этого элемента. 

```
item()
```

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*текущий элемент массива*> | Любой | Текущий элемент в массиве для текущей итерации действия | 
|||| 

*Пример* 

В этом примере извлекается элемент `body` из текущего сообщения для действия "Send_an_email" внутри текущей итерации цикла for-each:

```
item().body
```

<a name="items"></a>

### <a name="items"></a>Items

Возвращает текущий элемент из каждого цикла в цикле for-each. Используйте эту функцию внутри цикла for-each.

```
items('<loopName>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*loopName*> | Yes | Строка | Имя каждого цикла for-each | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*item*> | Любой | Элемент из текущего цикла в указанном цикле for-each | 
|||| 

*Пример* 

В этом примере извлекается текущий элемент из указанного цикла for-each:

```
items('myForEachLoopName')
```

<a name="json"></a>

### <a name="json"></a>json

Возвращает значение типа JSON либо объект для строки или XML.

```
json('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка или XML | Строка или XML для преобразования | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*результат JSON*> | Собственный тип или объект JSON | Значение собственного типа или объекта JSON для заданной строки или XML. Если строка имеет значение NULL, функция возвращает пустой объект. | 
|||| 

*Пример 1* 

В этом примере эта строка преобразуется в значение JSON:

```
json('[1, 2, 3]')
```

Возвращается такой результат: `[1, 2, 3]`

*Пример 2*

В этом примере эта строка преобразуется в JSON: 

```
json('{"fullName": "Sophia Owen"}')
```

Возвращается такой результат:

```
{
  "fullName": "Sophia Owen"
}
```

*Пример 3*

В этом примере этот XML преобразуется в JSON: 

```
json(xml('<?xml version="1.0"?> <root> <person id='1'> <name>Sophia Owen</name> <occupation>Engineer</occupation> </person> </root>'))
```

Возвращается такой результат:

```json
{ 
   "?xml": { "@version": "1.0" }, 
   "root": { 
      "person": [ { 
         "@id": "1", 
         "name": "Sophia Owen", 
         "occupation": "Engineer" 
       } ] 
   } 
}
```

<a name="intersection"></a>

### <a name="intersection"></a>пересечению

Возвращает коллекцию, которая содержит *только* общие элементы в указанных коллекциях. Чтобы появиться в результатах, элемент должен находиться во всех коллекциях, переданных этой функции. Если один или несколько элементов имеют одинаковое имя, в результатах появляется последний элемент с таким именем.

```
intersection([<collection1>], [<collection2>], ...)
intersection('<collection1>', '<collection2>', ...)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*collection1*>, <*collection2*>, ... | Yes | Массив или объект, но не оба типа | Коллекции, из которых нужно получить *только* общие элементы | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*общий элементы*> | Массив или объект соответственно | Коллекция, которая содержит только общие элементы в указанных коллекциях | 
|||| 

*Пример* 

В этом примере находятся общие элементы в этих массивах:  

```
intersection([1, 2, 3], [101, 2, 1, 10], [6, 8, 1, 2])
```

И возвращается массив *только* с этими элементами: `[1, 2]`

<a name="join"></a>

### <a name="join"></a>join

Возвращает строку, содержащую все элементы из массива, в которой каждый символ отделен *разделителем*.

```
join([<collection>], '<delimiter>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*collection*> | Yes | Массив, | Массив, который содержит элементы для объединения |  
| <*delimiter*> | Yes | Строка | Разделитель, содержащийся между каждым символом в результирующей строке | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*симв.1*><*разделитель*><*симв.2*><*разделитель*>... | Строка | Полученная строка, созданная из всех элементов в указанном массиве |
|||| 

*Пример* 

В этом примере создается строка из всех элементов этого массива с указанным символом в качестве разделителя:

```
join([a, b, c], '.')
```

Возвращается такой результат: `"a.b.c"`

<a name="last"></a>

### <a name="last"></a>last

Возвращает последний элемент из коллекции.

```
last('<collection>')
last([<collection>])
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*collection*> | Yes | Строка или массив | Коллекция, в которой нужно найти последний элемент | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*последний элемент коллекции*> | Строка или массив соответственно | Последний элемент в коллекции | 
|||| 

*Пример* 

Эти примеру ищут последний элемент в этих коллекциях:

```
last('abcd')
last([0, 1, 2, 3])
```

И возвращаются следующие результаты: 

* Первый пример: `"d"`.
* Второй пример: `3`.

<a name="lastindexof"></a>

### <a name="lastindexof"></a>lastIndexOf

Возвращает начальную позицию или значение индекса последнего вхождения подстроки. Эта функция не учитывает регистр, и индексы начинаются с числа 0.

```
lastIndexOf('<text>', '<searchText>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*text*> | Yes | Строка | Строки, которая содержит подстроку для поиска | 
| <*searchText*> | Yes | Строка | Подстрока для поиска | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*конечное значение индекса*> | Целое число  | Начальная позиция или значение индекса последнего вхождения указанной подстроки. <p>Если строка не найдена, возвращается -1. | 
|||| 

*Пример* 

Этот пример находит начальное значение индекса для последнего вхождения подстроки "world" в строке "hello world":

```
lastIndexOf('hello world', 'world')
```

Возвращается такой результат: `6`

<a name="length"></a>

### <a name="length"></a>длина

Возвращает число элементов в коллекции.

```
length('<collection>')
length([<collection>])
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*collection*> | Yes | Строка или массив | Коллекция с элементами для подсчета | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*длина или число*> | Целое число  | Число элементов в данной коллекции | 
|||| 

*Пример*

В этих примерах подсчитывается количество элементов в этих коллекциях: 

```
length('abcd')
length([0, 1, 2, 3])
```

Возвращается результат: `4`

<a name="less"></a>

### <a name="less"></a>less

Проверяет, является ли первое значение меньшим, чем второе.
Возвращает true, если первое значение меньше, или значение false, если первое значение больше.

```
less(<value>, <compareTo>)
less('<value>', '<compareTo>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Целое число, число с плавающей запятой или строка | Первое значение, для которого выполняется проверка того, является ли оно меньшим, чем второе. | 
| <*compareTo*> | Yes | Целое число, число с плавающей запятой или строка соответственно | Элемент для сравнения | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| Значение true или false | Логическое | Возвращает значение true, если первое значение меньше, чем второе. Возвращает значение false, если первое значение больше или равно второму. | 
|||| 

*Пример*

В этих примерах проверяется, является ли первое значение меньшим, чем второе.

```
less(5, 10)
less('banana', 'apple')
```

И возвращаются следующие результаты: 

* Первый пример: `true`.
* Второй пример: `false`.

<a name="lessOrEquals"></a>

### <a name="lessorequals"></a>lessOrEquals

Проверяет, является ли первое значение меньшим, чем второе, или равным ему.
Возвращает true, если первое значение меньше или равно, или значение false, если первое значение больше.

```
lessOrEquals(<value>, <compareTo>)
lessOrEquals('<value>', '<compareTo>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Целое число, число с плавающей запятой или строка | Проверяет, является ли первое значение меньшим, чем второе, или равным ему | 
| <*compareTo*> | Yes | Целое число, число с плавающей запятой или строка соответственно | Элемент для сравнения | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| Значение true или false  | Логическое | Возвращает значение true, если первое значение меньше или равно второму. Возвращает значение false, если первое значение больше, чем второе. |  
|||| 

*Пример*

В этих примерах проверяется, является ли первое значение меньшим или равным второму.

```
lessOrEquals(10, 10)
lessOrEquals('apply', 'apple')
```

И возвращаются следующие результаты: 

* Первый пример: `true`.
* Второй пример: `false`.

<a name="listCallbackUrl"></a>

### <a name="listcallbackurl"></a>listCallbackUrl

Возвращает URL-адрес обратного вызова, который вызывает триггер или действие. Эта функция работает только с триггерами и действиями для типов соединителей **HttpWebhook** и **ApiConnectionWebhook**, но не типов **Manual**, **Recurrence**, **HTTP** и **APIConnection**. 

```
listCallbackUrl()
```

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*URL обратного вызова*> | Строка | URL-адрес обратного вызова для действия или триггера |  
|||| 

*Пример*

В этом примере показан пример URL-адреса обратного вызова, который может вернуть эта функция:

`"https://prod-01.westus.logic.azure.com:443/workflows/<*workflow-ID*>/triggers/manual/run?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=<*signature-ID*>"`

<a name="max"></a>

### <a name="max"></a>max

Возвращает наибольшее значение из списка или массива, охватывающих последние и начальные значения. 

```
max(<number1>, <number2>, ...)
max([<number1>, <number2>, ...])
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*number1*>, <*number2*>, ... | Yes | Целое число, число с плавающей запятой или оба типа | Набор чисел, из которого требуется получить наибольшее значение | 
| [<*number1*>, <*number2*>, ...] | Yes | Массив — целое число, число с плавающей запятой или оба типа | Массив чисел, из которого требуется получить наибольшее значение | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*макс. значение*> | Целое число и число с плавающей запятой | Максимальное значение в указанном массиве или набор чисел | 
|||| 

*Пример* 

В этих примерах извлекается наибольшее значение из набора чисел и массива:

```
max(1, 2, 3)
max([1, 2, 3])
```

Возвращается результат: `3`

<a name="min"></a>

### <a name="min"></a>Min

Возвращает наименьшее значение из набора чисел или массива.

```
min(<number1>, <number2>, ...)
min([<number1>, <number2>, ...])
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*number1*>, <*number2*>, ... | Yes | Целое число, число с плавающей запятой или оба типа | Набор чисел, из которого требуется получить наименьшее значение | 
| [<*number1*>, <*number2*>, ...] | Yes | Массив — целое число, число с плавающей запятой или оба типа | Массив чисел, из которого требуется получить наименьшее значение | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*мин. значение*> | Целое число и число с плавающей запятой | Наименьшее значение в указанном наборе чисел или массиве | 
|||| 

*Пример* 

В этих примерах извлекается наименьшее значение из набора чисел и массива:

```
min(1, 2, 3)
min([1, 2, 3])
```

Возвращается результат: `1`

<a name="mod"></a>

### <a name="mod"></a>mod (модуль)

Возвращает остаток результата деления двух чисел. Чтобы получить целочисленный результат, см. раздел [div()](#div).

```
mod(<dividend>, <divisor>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*dividend*> | Yes | Целое число и число с плавающей запятой | Число, которое нужно поделить на *делитель* | 
| <*divisor*> | Yes | Целое число и число с плавающей запятой | Число, на которое делится *делимое* (не может быть равно 0). | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*остаток*> | Целое число и число с плавающей запятой | Остаток результата деления первого числа на второе | 
|||| 

*Пример* 

В этом примере первое число делится на второе:

```
mod(3, 2)
```

Возвращается результат: `1`

<a name="mul"></a>

### <a name="mul"></a>mul

Возвращает результат умножения двух чисел.

```
mul(<multiplicand1>, <multiplicand2>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*multiplicand1*> | Yes | Целое число и число с плавающей запятой | Число для умножения на *multiplicand2* | 
| <*multiplicand2*> | Yes | Целое число и число с плавающей запятой | Число, на которое умножается *multiplicand1* | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*результат*> | Целое число и число с плавающей запятой | Произведение от умножения первого числа на второе | 
|||| 

*Пример* 

В этом примере первое число умножается на второе:

```
mul(1, 2)
mul(1.5, 2)
```

И возвращаются следующие результаты:

* Первый пример: `2`.
* Второй пример: `3`.

<a name="multipartBody"></a>

### <a name="multipartbody"></a>multipartBody

Возвращает текст указанной части выходных данных действия, которые состоят из нескольких частей.

```
multipartBody('<actionName>', <index>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Yes | Строка | Имя действия, которое содержит выходные данные, состоящие из нескольких частей | 
| <*index*> | Yes | Целое число  | Значение индекса необходимой части | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*текст*> | Строка | Текст указанной части | 
|||| 

<a name="not"></a>

### <a name="not"></a>not

Проверяет, имеет ли выражение значение false. Возвращает значение true, если выражение ложно, или значение false, если значение истинно.

```
not(<expression>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*expression*> | Yes | Логическое | Выражение для вычисления | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| Значение true или false | Логическое | Возвращает значение true, если выражение ложно. Возвращает значение false, если выражение истинно. |  
|||| 

*Пример 1*

В этих примерах проверяется, все ли указанные выражения ложны: 

```
not(false)
not(true)
```

И возвращаются следующие результаты:

* В первом примере выражение имеет значение false, поэтому функция возвращает `true`.
* Во втором примере выражение имеет значение true, поэтому функция возвращает `false`.

*Пример 2*

В этих примерах проверяется, все ли указанные выражения ложны: 

```
not(equals(1, 2))
not(equals(1, 1))
```

И возвращаются следующие результаты:

* В первом примере выражение имеет значение false, поэтому функция возвращает `true`.
* Во втором примере выражение имеет значение true, поэтому функция возвращает `false`.

<a name="or"></a>

### <a name="or"></a>или

Проверяет, является ли хотя бы одно выражение истинным. Возвращает значение true, если хотя бы одно выражение истинно, или значение false, когда все являются ложными.

```
or(<expression1>, <expression2>, ...)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*expression1*>, <*expression2*>, ... | Yes | Логическое | Выражения, которые следует проверить | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| Значение true или false | Логическое | Возвращает значение true, если хотя бы одно выражение истинно. Возвращает значение false, если все выражения ложны. |  
|||| 

*Пример 1*

В этих примерах проверяется, является ли хотя бы одно выражение истинным:

```
or(true, false)
or(false, false)
```

И возвращаются следующие результаты:

* В первом примере по крайней мере одно выражение имеет значение true, поэтому функция возвращает `true`.
* Во втором примере оба выражения имеют значения false, поэтому функция возвращает `false`.

*Пример 2*

В этих примерах проверяется, является ли хотя бы одно выражение истинным:

```
or(equals(1, 1), equals(1, 2))
or(equals(1, 2), equals(1, 3))
```

И возвращаются следующие результаты:

* В первом примере по крайней мере одно выражение имеет значение true, поэтому функция возвращает `true`.
* Во втором примере оба выражения имеют значения false, поэтому функция возвращает `false`.

<a name="parameters"></a>

### <a name="parameters"></a>parameters

Возвращает значение параметра, описанного в определении вашего приложения логики. 

```
parameters('<parameterName>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*parameterName*> | Yes | Строка | Имя параметра, значение которого требуются получить | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*значение параметра*> | Любой | Значение указанного параметра | 
|||| 

*Пример* 

Предположим, что это значение JSON:

```json
{
  "fullName": "Sophia Owen"
}
```

Этот пример получает значение указанного параметра:

```
parameters('fullName')
```

Возвращается такой результат: `"Sophia Owen"`

<a name="rand"></a>

### <a name="rand"></a>rand

Возвращает случайное целое число из указанного диапазона с включительным значением только в начале.

```
rand(<minValue>, <maxValue>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*minValue*> | Yes | Целое число  | Наименьшее целое число в диапазоне | 
| <*maxValue*> | Yes | Целое число  | Целое число, следующее за наибольшим целым числом в диапазоне, которое функция может вернуть | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*случайный результат*> | Целое число  | Случайное целое число, возвращаемое из указанного диапазона |  
|||| 

*Пример*

Этот пример получает случайное целое число из указанного диапазона, исключая максимальное значение: 

```
rand(1, 5)
```

И возвращает в виде результата одно из этих чисел: `1`, `2`, `3` или `4`. 

<a name="range"></a>

### <a name="range"></a>range

Возвращает массив целых чисел, который начинается с заданного целого числа.

```
range(<startIndex>, <count>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*startIndex*> | Yes | Целое число  | Целочисленное значение, являющееся первым элементом массива | 
| <*count*> | Yes | Целое число  | Количество целых чисел в массиве | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| [<*результат диапазона*>] | Массив, | Массив с целыми числами, который начинается с указанного индекса |  
|||| 

*Пример*

В этом примере создается целочисленный массив, который начинается с указанного индекса и имеет указанное число целых чисел:

```
range(1, 4)
```

Возвращается такой результат: `[1, 2, 3, 4]`

<a name="replace"></a>

### <a name="replace"></a>replace

Заменяет подстроку указанной строкой и возвращает полученную строку. Эта функция учитывает регистр.

```
replace('<text>', '<oldText>', '<newText>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*text*> | Yes | Строка | Строка, которая содержит заменяемую подстроку | 
| <*oldText*> | Yes | Строка | Заменяемая подстрока | 
| <*newText*> | Yes | Строка | Строка для замены | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*обновленный текст*> | Строка | Обновленная строка после замены подстроки <p>Если подстрока не найдена, возвращает исходную строку. | 
|||| 

*Пример* 

Этот пример выполняет поиск подстроки "old" в строке "the old string" и заменяет "old" на "new": 

```
replace('the old string', 'old', 'new')
```

Возвращается такой результат: `"the new string"`

<a name="removeProperty"></a>

### <a name="removeproperty"></a>removeProperty

Удаляет свойство из объекта и возвращает обновленный объект.

```
removeProperty(<object>, '<property>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*object*> | Yes | Объект. | Объект JSON, из которого вы хотите удалить свойство | 
| <*property*> | Yes | Строка | Имя удаляемого свойства | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*обновленный_объект*> | Объект. | Обновленный объект JSON без указанного свойства | 
|||| 

*Пример*

В этом примере свойство `"accountLocation"` удаляется из объекта `"customerProfile"`, который преобразуется в JSON с помощью функции [JSON ()](#json), и возвращается обновленный объект:

```
removeProperty(json('customerProfile'), 'accountLocation')
```

<a name="setProperty"></a>

### <a name="setproperty"></a>setProperty

Задает значение свойства объекта и возвращает обновленный объект. Чтобы добавить новое свойство, можно использовать эту функцию или функцию [addProperty()](#addProperty).

```
setProperty(<object>, '<property>', <value>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*object*> | Yes | Объект. | Объект JSON, свойство которого вы хотите установить. | 
| <*property*> | Yes | Строка | Имя существующего или нового задаваемого свойства | 
| <*value*> | Yes | Любой | Значение, задаваемое для указанного свойства |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*обновленный_объект*> | Объект. | Обновленный объект JSON, свойство которого задается | 
|||| 

*Пример*

В этом примере устанавливается свойство `"accountNumber"` в объекте `"customerProfile"`, который преобразуется в JSON с помощью функции [JSON()](#json). Функция присваивает значение, которое формируется функцией [guid()](#guid), и возвращает обновленный объект JSON:

```
setProperty(json('customerProfile'), 'accountNumber', guid())
```

<a name="skip"></a>

### <a name="skip"></a>skip

Удаляет элементы из начала коллекции и возвращает *все другие элементы*.

```
skip([<collection>], <count>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*collection*> | Yes | Массив, | Коллекция, элементы которой требуется удалить | 
| <*count*> | Yes | Целое число  | Положительное целое число для количества элементов в начале коллекции, которые требуется удалить | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| [<*обновленная коллекция*>] | Массив, | Обновленная коллекция после удаления указанных элементов. | 
|||| 

*Пример*

В этом примере удаляется один элемент, число 0, в начале указанного массива: 

```
skip([0, 1, 2, 3], 1)
```

И возвращается исходный массив и оставшиеся элементы: `[1,2,3]`

<a name="split"></a>

### <a name="split"></a>split

Возвращает массив, который содержит все символы из строки, отделенные *разделителем*.

```
split('<text>', '<separator>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*text*> | Yes | Строка | Строка, содержащая символы, которые требуется разделить |  
| <*separator*> | Yes | Строка | Разделитель между символами полученного массива | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| [<*симв.1*><*разделитель*><*симв.2*><*разделитель*>...] | Массив, | Полученный массив, состоящий из всех элементов в указанной строке. |
|||| 

*Пример* 

В этом примере создается массив из указанной строки с разделителем-запятой между каждым символом:

```
split('abc', ',')
```

Возвращается такой результат: `[a, b, c]`

<a name="startOfDay"></a>

### <a name="startofday"></a>startOfDay

Возвращает начало дня для метки времени. 

```
startOfDay('<timestamp>', '<format>'?)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*timestamp*> | Yes | Строка | Строка, содержащая метку времени | 
| <*format*> | Нет  | Строка | Либо [один описатель формата](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), либо [пользовательский шаблон формата](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Формат по умолчанию для временной метки — ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (ГГГГ-ММ-ДДTЧЧ:ММ:СС:fffffffK), который соответствует [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) и сохраняет информацию о часовом поясе. |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*обновленная_метка_времени*> | Строка | Указанная метка времени, которая начинается с нулевого часа дня | 
|||| 

*Пример* 

Этот пример находит начало дня для этой метки времени:

```
startOfDay('2018-03-15T13:30:30Z')
```

Возвращается такой результат: `"2018-03-15T00:00:00.0000000Z"`

<a name="startOfHour"></a>

### <a name="startofhour"></a>startOfHour

Возвращает начало часа для метки времени. 

```
startOfHour('<timestamp>', '<format>'?)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*timestamp*> | Yes | Строка | Строка, содержащая метку времени | 
| <*format*> | Нет  | Строка | Либо [один описатель формата](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), либо [пользовательский шаблон формата](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Формат по умолчанию для временной метки — ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (ГГГГ-ММ-ДДTЧЧ:ММ:СС:fffffffK), который соответствует [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) и сохраняет информацию о часовом поясе. |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*обновленная_метка_времени*> | Строка | Указанная метка времени, которая начинается с нулевой минуты часа | 
|||| 

*Пример* 

Этот пример находит начало часа для этой метки времени:

```
startOfHour('2018-03-15T13:30:30Z')
```

Возвращается такой результат: `"2018-03-15T13:00:00.0000000Z"`

<a name="startOfMonth"></a>

### <a name="startofmonth"></a>startOfMonth

Возвращает начало месяца для метки времени. 

```
startOfMonth('<timestamp>', '<format>'?)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*timestamp*> | Yes | Строка | Строка, содержащая метку времени | 
| <*format*> | Нет  | Строка | Либо [один описатель формата](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), либо [пользовательский шаблон формата](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Формат по умолчанию для временной метки — ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (ГГГГ-ММ-ДДTЧЧ:ММ:СС:fffffffK), который соответствует [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) и сохраняет информацию о часовом поясе. |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*обновленная_метка_времени*> | Строка | Указанная метка времени, которая начинается с нулевого часа первого дня месяца | 
|||| 

*Пример* 

Этот пример находит начало месяца для этой метки времени:

```
startOfMonth('2018-03-15T13:30:30Z')
```

Возвращается такой результат: `"2018-03-01T00:00:00.0000000Z"`

<a name="startswith"></a>

### <a name="startswith"></a>startsWith

Проверяет, начинается ли строка с определенной подстроки. Возвращает true, если подстрока найдена, или возвращает false, если нет. Эта функция не учитывает регистр.

```
startsWith('<text>', '<searchText>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*text*> | Yes | Строка | Проверяемая строка | 
| <*searchText*> | Yes | Строка | Начальная строка для поиска | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| Значение true или false  | Логическое | Возвращает значение true при обнаружении начальной подстроки. В противном случае возвращает значение false. | 
|||| 

*Пример 1* 

В этом примере проверяется, начинается ли строка "hello world" подстрокой "hello":

```
startsWith('hello world', 'hello')
```

Возвращается такой результат: `true`

*Пример 2*

В этом примере проверяется, начинается ли строка "hello world" подстрокой "greetings":

```
startsWith('hello world', 'greetings')
```

Возвращается такой результат: `false`

<a name="string"></a>

### <a name="string"></a>строка

Возвращает строковую версию значения.

```
string(<value>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Любой | Значение, которое необходимо преобразовать | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*строковое значение*> | Строка | Строковая версия указанного значения | 
|||| 

*Пример 1* 

В этом примере создается строковая версия для этого числа:

```
string(10)
```

Возвращается такой результат: `"10"`

*Пример 2*

В этом примере создается строка для указанного объекта JSON и используется символ обратной косой черты (\\) в качестве escape-символа для двойных кавычек (").

```
string( { "name": "Sophie Owen" } )
```

Возвращается такой результат: `"{ \\"name\\": \\"Sophie Owen\\" }"`

<a name="sub"></a>

### <a name="sub"></a>sub

Вычитает второе число из первого числа и возвращает результат.

```
sub(<minuend>, <subtrahend>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*minuend*> | Yes | Целое число и число с плавающей запятой | Число, из которого вычитается *вычитаемое* | 
| <*subtrahend*> | Yes | Целое число и число с плавающей запятой | Число, которое вычитается из *уменьшаемого* | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*результат*> | Целое число и число с плавающей запятой | Вычитается второе число из первого числа и возвращается результат | 
|||| 

*Пример* 

В этом примере из первого числа вычитается второе число:

```
sub(10.3, .3)
```

Возвращается такой результат: `10`

<a name="substring"></a>

### <a name="substring"></a>substring

Возвращает символы из строки, начиная с указанной позиции или индекса. Значения индекса начинаются с числа 0. 

```
substring('<text>', <startIndex>, <length>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*text*> | Yes | Строка | Строка, символы которой требуется получить | 
| <*startIndex*> | Yes | Целое число  | Положительное число начальной позиции или значения индекса | 
| <*length*> | Yes | Целое число  | Положительное число символов для подстроки | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*результат подстроки*> | Строка | Подстрока с указанным количеством символов, начиная с указанной позиции индекса в исходной строке | 
|||| 

*Пример* 

В этом примере создается пятисимвольная подстрока из указанной строки, начиная со значения индекса 6:

```
substring('hello world', 6, 5)
```

Возвращается такой результат: `"world"`

<a name="subtractFromTime"></a>

### <a name="subtractfromtime"></a>subtractFromTime

Вычитает количество единиц времени из метки времени. См. раздел [getPastTime](#getPastTime).

```
subtractFromTime('<timestamp>', <interval>, '<timeUnit>', '<format>'?)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*timestamp*> | Yes | Строка | Строка, содержащая метку времени | 
| <*interval*> | Yes | Целое число  | Число единиц времени для вычитания | 
| <*timeUnit*> | Yes | Строка | Единицы времени для использования с *интервалом*: "секунда", "минута", "час", "день", "неделя", "месяц", "год" | 
| <*format*> | Нет  | Строка | Либо [один описатель формата](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), либо [пользовательский шаблон формата](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Формат по умолчанию для временной метки — ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (ГГГГ-ММ-ДДTЧЧ:ММ:СС:fffffffK), который соответствует [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) и сохраняет информацию о часовом поясе. | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*обновленная_метка_времени*> | Строка | Метка времени, от которой отнято указанное число единиц времени | 
|||| 

*Пример 1*

В этом примере из этой метки времени вычитается один день:

```
subtractFromTime('2018-01-02T00:00:00Z', 1, 'Day') 
```

Возвращается такой результат: `"2018-01-01T00:00:00:0000000Z"`

*Пример 2*

В этом примере из этой метки времени вычитается один день:

```
subtractFromTime('2018-01-02T00:00:00Z', 1, 'Day', 'D') 
```

И возвращается результат с использованием необязательного формата "D": `"Monday, January, 1, 2018"`

<a name="take"></a>

### <a name="take"></a>take

Возвращает элементы, расположенные в начале коллекции. 

```
take('<collection>', <count>)
take([<collection>], <count>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*collection*> | Yes | Строка или массив | Коллекция, элементы которой требуется получить | 
| <*count*> | Yes | Целое число  | Положительное целое число для количества элементов в начале коллекции, которые требуется получить | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*подмножество*> или [<*подмножество*>] | Строка или массив соответственно | Строка или массив, который содержит заданное число элементов, взятых из первой части исходной коллекции | 
|||| 

*Пример*

Эти примеры получают указанное количество элементов из первой части этих коллекций:

```
take('abcde`, 3)
take([0, 1, 2, 3, 4], 3)
```

И возвращаются следующие результаты:

* Первый пример: `"abc"`.
* Второй пример: `[0, 1, 2]`.

<a name="ticks"></a>

### <a name="ticks"></a>ticks

Возвращает значение свойства `ticks` для указанной метки времени. *Интервал* составляет 100 наносекунд.

```
ticks('<timestamp>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*timestamp*> | Yes | Строка | Строка для метки времени | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*число тактов*> | Целое число  | Число тактов с момента в указанной метке времени | 
|||| 

<a name="toLower"></a>

### <a name="tolower"></a>toLower

Возвращает строку символов в нижнем регистре. Если символ в строке не имеет версии в нижнем регистре, он добавляется в возвращаемую строку без изменений.

```
toLower('<text>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*text*> | Yes | Строка | Строка, возвращаемая в нижнем регистре | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*текст в нижнем регистре*> | Строка | Исходная строка в нижнем регистре | 
|||| 

*Пример* 

В этом примере эта строка преобразуется в нижний регистр: 

```
toLower('Hello World')
```

Возвращается такой результат: `"hello world"`

<a name="toUpper"></a>

### <a name="toupper"></a>toUpper

Возвращает строку символов в верхнем регистре. Если символ в строке не имеет версии в верхнем регистре, он добавляется в возвращаемую строку без изменений.

```
toUpper('<text>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*text*> | Yes | Строка | Строка, возвращаемая в верхнем регистре | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*текст в верхнем регистре*> | Строка | Исходная строка в верхнем регистре | 
|||| 

*Пример* 

В этом примере эта строка преобразуется в верхний регистр:

```
toUpper('Hello World')
```

Возвращается такой результат: `"HELLO WORLD"`

<a name="trigger"></a>

### <a name="trigger"></a>trigger

Возвращает выходные данные триггера во время выполнения или значение из других пар "имя — значение" JSON, которые вы можете назначить выражению. 

* Внутри входных данных триггера функция возвращает выходные данные предыдущего выполнения. 

* Внутри условий триггера функция возвращает выходные данные текущего выполнения. 

По умолчанию эта функция ссылается на весь объект триггера, но вы можете дополнительно указать свойство, значение которого требуется. Сокращенные версии этой функции см. в разделах [triggerOutputs()](#triggerOutputs) и [triggerBody()](#triggerBody). 

```
trigger()
```

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*выходные данные триггера*> | Строка | Выходные данные из триггера во время выполнения | 
|||| 

<a name="triggerBody"></a>

### <a name="triggerbody"></a>triggerBody

Возвращает выходные данные `body` триггера во время выполнения. Сокращение для `trigger().outputs.body`. См. раздел [trigger()](#trigger). 

```
triggerBody()
```

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*выходные данные body триггера*> | Строка | Выходные данные `body` из триггера | 
|||| 

<a name="triggerFormDataMultiValues"></a>

### <a name="triggerformdatamultivalues"></a>triggerFormDataMultiValues

Возвращает массив значений, которые соответствуют имени ключа в выходных данных *form-data* или *form-encoded* триггера. 

```
triggerFormDataMultiValues('<key>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*key*> | Yes | Строка | Имя ключа, значение которого нужно получить | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| [<*массив со значениями ключа*>] | Массив, | Массив со всеми значениями, которые соответствуют заданному ключу | 
|||| 

*Пример* 

В этом примере создается массив из значения ключа "feedUrl" в выходных данных form-data или form-encoded триггера RSS. 

```
triggerFormDataMultiValues('feedUrl')
```

Возвращается этот массив в качестве примера: `["http://feeds.reuters.com/reuters/topNews"]`

<a name="triggerFormDataValue"></a>

### <a name="triggerformdatavalue"></a>triggerFormDataValue

Возвращает строку с одним значением, которое соответствует имени ключа в выходных данных *form-data* или *form-encoded* триггера. Если функция обнаруживает более одного совпадения, она выдает ошибку.

```
triggerFormDataValue('<key>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*key*> | Yes | Строка | Имя ключа, значение которого нужно получить |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*значение ключа*> | Строка | Значение в указанном ключе | 
|||| 

*Пример* 

В этом примере создается строка из значения ключа "feedUrl" в выходных данных form-data или form-encoded триггера RSS.

```
triggerFormDataValue('feedUrl')
```

Возвращается эта строка в качестве примера: `"http://feeds.reuters.com/reuters/topNews"` 

<a name="triggerMultipartBody"></a>

### <a name="triggermultipartbody"></a>triggerMultipartBody

Возвращает текст указанной части выходных данных триггера, которые состоят из нескольких частей. 

```
triggerMultipartBody(<index>)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*index*> | Yes | Целое число  | Значение индекса необходимой части |
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*текст*> | Строка | Текст указанной части выходных данных триггера, которые состоят из нескольких частей | 
|||| 

<a name="triggerOutputs"></a>

### <a name="triggeroutputs"></a>triggerOutputs

Возвращает выходные данные триггера во время выполнения или значения из других пар "имя — значение" JSON. Сокращение для `trigger().outputs`. См. раздел [trigger()](#trigger). 

```
triggerOutputs()
```

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*выходные данные триггера*> | Строка | Выходные данные из триггера во время выполнения  | 
|||| 

<a name="trim"></a>

### <a name="trim"></a>trim

Удаляет все начальные и конечные пробелы и возвращает обновленную строку.

```
trim('<text>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*text*> | Yes | Строка | Строка, которая содержит начальные и конечные пробелы для удаления | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*обновленный текст*> | Строка | Обновленная версия исходной строки без начальных и конечных пробелов | 
|||| 

*Пример* 

В этом примере удаляются начальные и конечные пробелы из строки " Hello World  ":  

```
trim(' Hello World  ')
```

Возвращается такой результат: `"Hello World"`

<a name="union"></a>

### <a name="union"></a>union

Возвращает коллекцию, которая содержит *все* элементы из указанных коллекций. Чтобы появиться в результатах, элемент должен содержаться в любой коллекции, переданной этой функции. Если один или несколько элементов имеют одинаковое имя, в результатах появляется последний элемент с таким именем. 

```
union('<collection1>', '<collection2>', ...)
union([<collection1>], [<collection2>], ...)
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*collection1*>, <*collection2*>, ...  | Yes | Массив или объект, но не оба типа | Коллекции, из которых вы хотите получить *все* элементы | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*обновленная коллекция*> | Массив или объект соответственно | Коллекция, которая содержит все элементы из указанных коллекций, без повторений | 
|||| 

*Пример* 

В этом примере извлекаются *все* элементы из коллекций: 

```
union([1, 2, 3], [1, 2, 10, 101])
```

Возвращается такой результат: `[1, 2, 3, 10, 101]`

<a name="uriComponent"></a>

### <a name="uricomponent"></a>uriComponent

Возвращает кодированную версию URI для строки, заменив символы, опасные для URL-адреса, на escape-символы. Используйте эту функцию вместо [encodeUriComponent()](#encodeUriComponent). Хотя обе функции работают одинаково, `uriComponent()` является предпочтительной.

```
uriComponent('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | Строка для преобразования в формат закодированного URI | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*закодированный URI*> | Строка | Строка с закодированным URI, содержащая escape-символы | 
|||| 

*Пример*

В этом примере создается версия с закодированным URI для этой строки:

```
uriComponent('https://contoso.com')
```

Возвращается такой результат: `"http%3A%2F%2Fcontoso.com"`

<a name="uriComponentToBinary"></a>

### <a name="uricomponenttobinary"></a>uriComponentToBinary

Возвращает двоичную версию компонента URI.

```
uriComponentToBinary('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | Преобразуемая строка с закодированным URI | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*двоичная версия закодированного URI*> | Строка | Двоичная версия строки с закодированным URI. Двоичное содержимое в формате Base64, представленное с помощью `$content`. | 
|||| 

*Пример*

В этом примере создается двоичная версия для этой строки с закодированным URI: 

```
uriComponentToBinary('http%3A%2F%2Fcontoso.com')
```

Возвращается такой результат: 

`"001000100110100001110100011101000111000000100101001100
11010000010010010100110010010001100010010100110010010001
10011000110110111101101110011101000110111101110011011011
110010111001100011011011110110110100100010"`

<a name="uriComponentToString"></a>

### <a name="uricomponenttostring"></a>uriComponentToString

Возвращает декодированную версию строки с закодированным URI.

```
uriComponentToString('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | Декодируемая строка с закодированным URI | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*декодированный URI*> | Строка | Декодированная версия строки с закодированным URI | 
|||| 

*Пример*

В этом примере создается декодированная версия строки с закодированным URI: 

```
uriComponentToString('http%3A%2F%2Fcontoso.com')
```

Возвращается такой результат: `"https://contoso.com"` 

<a name="uriHost"></a>

### <a name="urihost"></a>uriHost

Возвращает значение `host` для URI.

```
uriHost('<uri>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*uri*> | Yes | Строка | URI, значение `host` которого требуется | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*значение узла*> | Строка | Значение `host` для указанного URI. | 
|||| 

*Пример*

В этом примере выполняется поиск значения `host` для этого URI: 

```
uriHost('https://www.localhost.com:8080')
```

Возвращается такой результат: `"www.localhost.com"`

<a name="uriPath"></a>

### <a name="uripath"></a>uriPath

Возвращает значение `path` для URI. 

```
uriPath('<uri>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*uri*> | Yes | Строка | URI, значение `path` которого требуется | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*значение пути*> | Строка | Значение `path` для указанного URI. Если `path` не имеет значения, вернется символ "/". | 
|||| 

*Пример*

В этом примере выполняется поиск значения `path` для этого URI: 

```
uriPath('http://www.contoso.com/catalog/shownew.htm?date=today')
```

Возвращается такой результат: `"/catalog/shownew.htm"`

<a name="uriPathAndQuery"></a>

### <a name="uripathandquery"></a>uriPathAndQuery

Возвращает значения `path` и `query` для URI.

```
uriPathAndQuery('<uri>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*uri*> | Yes | Строка | URI, значения `path` и `query` которого требуется извлечь | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*значение запроса пути*> | Строка | Значения `path` и `query` для указанного URI. Если `path` не указывает значение, вернется символ "/". | 
|||| 

*Пример*

В этом примере выполняется поиск значения `path` и `query` для этого URI:

```
uriPathAndQuery('http://www.contoso.com/catalog/shownew.htm?date=today')
```

Возвращается такой результат: `"/catalog/shownew.htm?date=today"`

<a name="uriPort"></a>

### <a name="uriport"></a>uriPort

Возвращает значение `port` для URI.

```
uriPort('<uri>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*uri*> | Yes | Строка | URI, значение `port` которого требуется | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*значение порта*> | Целое число  | Значение `port` для указанного URI. Если `port` не указывает значение, возвращается порт по умолчанию для протокола. | 
|||| 

*Пример*

В этом примере возвращается значение `port` для этого URI:

```
uriPort('http://www.localhost:8080')
```

Возвращается такой результат: `8080`

<a name="uriQuery"></a>

### <a name="uriquery"></a>uriQuery

Возвращает значение `query` для URI.

```
uriQuery('<uri>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*uri*> | Yes | Строка | URI, значение `query` которого требуется | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*значение запроса*> | Строка | Значение `query` для указанного URI. | 
|||| 

*Пример*

В этом примере возвращается значение `query` для этого URI: 

```
uriQuery('http://www.contoso.com/catalog/shownew.htm?date=today')
```

Возвращается такой результат: `"?date=today"`

<a name="uriScheme"></a>

### <a name="urischeme"></a>uriScheme

Возвращает значение `scheme` для URI.

```
uriScheme('<uri>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*uri*> | Yes | Строка | URI, значение `scheme` которого требуется | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*значение схемы*> | Строка | Значение `scheme` для указанного URI. | 
|||| 

*Пример*

В этом примере возвращается значение `scheme` для этого URI:

```
uriScheme('http://www.contoso.com/catalog/shownew.htm?date=today')
```

Возвращается такой результат: `"http"`

<a name="utcNow"></a>

### <a name="utcnow"></a>utcnow

Возвращает текущую метку времени. 

```
utcNow('<format>')
```

Кроме того, можно указать другой формат с помощью параметра <*format*>. 


| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*format*> | Нет  | Строка | Либо [один описатель формата](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), либо [пользовательский шаблон формата](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Формат по умолчанию для временной метки — ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (ГГГГ-ММ-ДДTЧЧ:ММ:СС:fffffffK), который соответствует [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) и сохраняет информацию о часовом поясе. | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*текущая метка времени*> | Строка | Текущая дата и время | 
|||| 

*Пример 1*

Предположим, сейчас 13:00:00 15 апреля 2018 года. В этом примере возвращается текущая метка времени: 

```
utcNow()
```

Возвращается такой результат: `"2018-04-15T13:00:00.0000000Z"`

*Пример 2*

Предположим, сейчас 13:00:00 15 апреля 2018 года. В этом примере возвращается текущая метка времени с использованием необязательного формата "D":

```
utcNow('D')
```

Возвращается такой результат: `"Sunday, April 15, 2018"`

<a name="variables"></a>

### <a name="variables"></a>variables

Возвращает значение для указанной переменной. 

```
variables('<variableName>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*variableName*> | Yes | Строка | Имя переменной, значение которой нужно получить | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*значение переменной*> | Любой | Значение указанной переменной | 
|||| 

*Пример*

Предположим, что текущее значение для переменной "numItems" — 20. В этом примере извлекается целочисленное значение этой переменной:

```
variables('numItems')
```

Возвращается такой результат: `20`

<a name="workflow"></a>

### <a name="workflow"></a>рабочий процесс

Возвращает все сведения о самом рабочем процессе во время выполнения. 

```
workflow().<property>
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*property*> | Нет  | Строка | Имя свойства рабочего процесса, значение которого требуется <p>Объект рабочего процесса имеет такие свойства: **name**, **type**, **id**, **location** и **run**. Значение свойства **run** также является объектом, имеющим следующие свойства: **name**, **type** и **id**. | 
||||| 

*Пример*

В этом примере возвращается имя текущего выполнения рабочего процесса:

```
workflow().run.name
```

<a name="xml"></a>

### <a name="xml"></a>xml

Возвращает версию XML строки, которая содержит объект JSON. 

```
xml('<value>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*value*> | Yes | Строка | Строка с объектом JSON для преобразования <p>Объект JSON должен содержать только одно корневое свойство. <br>Используйте обратную косую черту (\\) как escape-символ для двойных кавычек ("). | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*версия XML*> | Объект. | Закодированный XML для заданной строки или объекта JSON | 
|||| 

*Пример 1*

В этом примере создается версия XML для этой строки, которая содержит объект JSON: 

`xml( '{ \"name\": \"Sophia Owen\" }' )`

Возвращается следующий XML: 

```xml
<name>Sophia Owen</name>
```

*Пример 2*

Предположим, что у вас есть этот объект JSON:

```json
{ 
  "person": { 
    "name": "Sophia Owen", 
    "city": "Seattle" 
  } 
}
```

В этом примере создается XML для строки, содержащей этот объект JSON:

`xml( '{ \"person\": { \"name\": \"Sophia Owen\", \"city\": \"Seattle\" } }' )`

Возвращается следующий XML: 

```xml
<person>
  <name>Sophia Owen</name>
  <city>Seattle</city>
<person>
```

<a name="xpath"></a>

### <a name="xpath"></a>xpath

Проверяет XML на наличие узлов или значений, которые соответствуют выражению XPath, и возвращает соответствующие узлы или значения. Выражение XPath (или просто "XPath") помогает перемещаться по структуре документа XML, чтобы вы могли выбирать узлы или вычислять значения в содержимом XML.

```
xpath('<xml>', '<xpath>')
```

| Параметр | Обязательно | type | ОПИСАНИЕ | 
| --------- | -------- | ---- | ----------- | 
| <*xml*> | Yes | Любой | Строка XML для поиска узлов и значений, которые соответствуют значению выражения XPath | 
| <*xpath*> | Yes | Любой | Выражение XPath, используемое для поиска соответствующих узлов или значений XML | 
||||| 

| Возвращаемое значение | type | ОПИСАНИЕ | 
| ------------ | ---- | ----------- | 
| <*узел XML*> | XML | Узел XML, где только один узел соответствует указанному выражению XPath | 
| <*value*> | Любой | Значение из узла XML, где только одно значение соответствует указанному выражению XPath | 
| [<*узел XML1*>, <*узел XML2*>, ...] </br>-или- </br>[<*значение1*>, <*значение2*>, ...] | Массив, | Массив с узлами XML или значениями, которые соответствуют указанному выражению XPath | 
|||| 

*Пример 1*

В этом примере выполняется поиск узлов, которые соответствуют узлу `<name></name>` ​​в указанных аргументах, и возвращается массив с этими значениями узлов: 

`xpath(xml(parameters('items')), '/produce/item/name')`

Ниже приведены аргументы.

* Строка "items", которая содержит этот XML-код:

  `"<?xml version="1.0"?> <produce> <item> <name>Gala</name> <type>apple</type> <count>20</count> </item> <item> <name>Honeycrisp</name> <type>apple</type> <count>10</count> </item> </produce>"`

  В этом примере функция [parameters()](#parameters) используется, чтобы получить строку XML из аргумента "items", а функция [xml()](#xml) — чтобы преобразовать строку в формат XML. 

* Это выражение XPath, который передается в виде строки:

  `"/produce/item/name"`

Ниже приведен массив результатов с узлами, которые соответствуют `<name></name`:

`[ <name>Gala</name>, <name>Honeycrisp</name> ]`

*Пример 2*

Следуя примеру 1, этот пример находит узлы, которые соответствуют узлу `<count></count>`, и добавляет эти значения с помощью функции `sum()`:

`xpath(xml(parameters('items')), 'sum(/produce/item/count)')`

Возвращается такой результат: `30`

*Пример 3*

В этом примере оба выражения находят узлы, которые соответствуют узлу `<location></location>` в указанных аргументах, которые содержат XML с пространством имен. Выражения используют обратную косую черту (\\) как escape-символ для двойных кавычек (").

* *Выражение 1*

  `xpath(xml(body('Http')), '/*[name()=\"file\"]/*[name()=\"location\"]')`

* *Выражение 2* 

  `xpath(xml(body('Http')), '/*[local-name=()=\"file\"] and namespace-uri()=\"http://contoso.com\"/*[local-name()]=\"location\" and namespace-uri()=\"\"]')`

Ниже приведены аргументы.

* Это код XML, который включает пространство имен документа XML `xmlns="http://contoso.com"`: 

  ```xml
  <?xml version="1.0"?> <file xmlns="http://contoso.com"> <location>Paris</location> </file>
  ```

* Можно использовать любое выражение XPath:

  * `/*[name()=\"file\"]/*[name()=\"location\"]`

  * `/*[local-name=()=\"file\"] and namespace-uri()=\"http://contoso.com\"/*[local-name()]=\"location\" and namespace-uri()=\"\"]`

Ниже приведен узел, соответствующий узлу `<location></location`:

```xml
<location xmlns="https://contoso.com">Paris</location>
```

*Пример 4*

Следуя примеру 3, этот пример находит значение в узле `<location></location>`: 

`xpath(xml(body('Http')), 'string(/*[name()=\"file\"]/*[name()=\"location\"])')`

Возвращается такой результат: `"Paris"`

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о [языке определения рабочего процесса](../logic-apps/logic-apps-workflow-definition-language.md).
