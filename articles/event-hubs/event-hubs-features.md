---
title: Обзор функций Центров событий Azure | Документация Майкрософт
description: Обзор и сведения о Центрах событий Azure
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2018
ms.author: shvija
ms.openlocfilehash: c4a9a3189f3de101528871e4dba95bf7a76b9846
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746920"
---
# <a name="event-hubs-features-overview"></a>Обзор функций Центров событий

Центры событий Azure — это масштабируемая служба обработки событий, которая принимает и обрабатывает большие объемы событий и данных с низкой задержкой и высокой надежностью. Подробный обзор этой службы см. в статье [Что такое Центры событий?](event-hubs-what-is-event-hubs.md).

Эта статья продолжает тему [Центров событий](event-hubs-what-is-event-hubs.md) и содержит техническую информацию и сведения о реализации компонентов и функций этой службы.

## <a name="namespace"></a>Пространство имен
Пространство имен концентраторов событий предоставляет уникальный контейнер, ограничивающий область действия. Вы можете обращаться к этому контейнеру по [полному доменному имени](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) и создавать в нем концентраторы событий или разделы Kafka. 

## <a name="event-publishers"></a>Издатели событий

Любая сущность, которая отправляет данные в концентратор событий, является создателем или *издателем событий*. Издатели событий могут публиковать события с помощью HTTPS или AMQP 1.0. Издатели событий используют маркер подписанного URL-адреса (SAS), чтобы идентифицировать себя в концентраторе событий. Также они могут использовать уникальное удостоверение или общий подписанный URL-адрес.

### <a name="publishing-an-event"></a>Публикация события

Вы можете опубликовать событие через протокол AMQP 1.0 или HTTPS. Центры событий предоставляют [клиентские библиотеки и классы](event-hubs-dotnet-framework-api-overview.md) для публикации событий в концентраторе событий от клиентов .NET. Для других сред выполнения и платформ можно использовать любой клиент AMQP 1.0, например [Apache Qpid](http://qpid.apache.org/). Можно публиковать события по отдельности или в пакетном режиме. Для одной публикации (экземпляра данных события) установлено ограничение в 256 КБ, независимо от того, одиночное это событие или пакет. Публикация событий, размер которых превышает это пороговое значение, приводит к ошибке. Мы советуем не передавать издателям сведения о секциях в концентраторе событий, а указывать только *ключ секции* (см. следующий раздел) или их удостоверение через маркер SAS.

Решение об использовании AMQP или HTTPS обусловлено сценарием использования. AMQP требует установки постоянного двунаправленного сокета в дополнение к безопасности на уровне транспорта (TLS) или протоколу SSL/TLS. Использование AMQP связано с повышенными сетевыми затратами при инициализации сеанса, а использование HTTPS — с дополнительной нагрузкой SSL при каждом запросе. AMQP отличается более высокой производительностью при частых публикациях.

![Центры событий;](./media/event-hubs-features/partition_keys.png)

Центры событий обеспечивают доставку всех событий, использующих одинаковое значение ключа секции, в соответствующем порядке и в одну секцию. Если ключи секций используются с применением политик издателя, удостоверение издателя и значение ключа секции должны совпадать. В противном случае возникает ошибка.

### <a name="publisher-policy"></a>Политика издателя

Центры событий обеспечивают точный контроль над издателями событий через *политики издателя*. Политики издателя — это функции среды выполнения, которые упрощают обработку большого количества независимых издателей событий. Благодаря политикам издателя каждый издатель использует свой собственный уникальный идентификатор при публикации событий в концентраторе событий с помощью следующего механизма:

```http
//[my namespace].servicebus.windows.net/[event hub name]/publishers/[my publisher name]
```

Не требуется создавать имена издателей заранее, но они должны соответствовать маркеру SAS, который используется при публикации события, для обеспечения идентификации независимого издателя. При использовании политик издателя значение **PartitionKey** присваивается имени издателя. Для правильной работы эти значения должны совпадать.

## <a name="capture"></a>Регистрация

[Функция "Сбор" в Центрах событий](event-hubs-capture-overview.md) позволяет автоматически собирать данные потоковой передачи в Центры событий и сохранять их в выбранной учетной записи хранения BLOB-объектов или учетной записи службы Azure Data Lake. Эту функцию можно включить на портале Azure и указать минимальный размер записываемых данных и интервал времени для записи. С помощью функции "Сбор" в Центрах событий можно указать собственную учетную запись хранилища BLOB-объектов Azure и контейнер или учетную запись службы Azure Data Lake, используемую для хранения собранных данных. Зафиксированные данные записываются в формате Apache Avro.

## <a name="partitions"></a>Разделы

Центры событий обеспечивают потоковую передачу сообщений так, чтобы каждый секционированный потребитель считывал только определенное подмножество (секцию) потока сообщений. Этот шаблон обеспечивает горизонтальное масштабирование для обработки событий и предоставляет другие функции, ориентированные на поток, которые недоступны для очередей и разделов.

Секция — это упорядоченная последовательность событий, хранящаяся в концентраторе событий. По мере поступления новых событий они добавляются в конец этой последовательности. Секцию можно рассматривать как «журнал фиксации».

![Центры событий;](./media/event-hubs-features/partition.png)

Центры событий сохраняют данные в течение указанного времени хранения, которое определяется для всех секций в концентраторе событий. События удаляются по истечении срока действия; невозможно явно удалить их. Так как секции независимы и содержат собственные последовательности данных, они часто расширяются с разной скоростью.

![Центры событий;](./media/event-hubs-features/multiple_partitions.png)

Количество секций определяется во время создания. Рабочий диапазон — от 2 до 32. Так как число секций неизменно, вам следует продумать масштаб заранее. Секции — это способ организации данных, который соотносится со степенью параллелизма подчиненных элементов, требуемой для работы потребляющих приложений. Поэтому количество секций в концентраторе событий непосредственно связано с предполагаемым числом параллельных модулей чтения. Но вы можете увеличить число секций, превысив максимальное ограничение (32 секции). Для этого обратитесь к группе разработчиков Центров событий.

Хотя секции можно идентифицировать и отправлять напрямую, это не рекомендуется делать. Вместо этого можно использовать конструкции более высокого уровня, описанные в разделах [Издатель событий](#event-publishers) и [Емкость](#capacity). 

Секции заполнены последовательностями данных событий, которые содержат текст события, контейнер определяемых пользователем свойств, а также метаданные события, включая сведения о смещении в секции и ее номер в последовательности потока.

Дополнительные сведения о секциях и компромиссе между доступностью и надежностью см. в статье [Руководство по программированию Центров событий](event-hubs-programming-guide.md#partition-key) и [Доступность и согласованность в Центрах событий](event-hubs-availability-and-consistency.md).

### <a name="partition-key"></a>Ключ секции

Вы можете использовать [ключ секции](event-hubs-programming-guide.md#partition-key), чтобы сопоставлять входные данные событий с определенными секциями для организации данных. Ключ секции — это указываемое отправителем значение, передаваемое в концентратор событий. Оно обрабатывается с помощью статической функции хэширования, в результате чего создается назначение секции. Если при публикации события не указать ключ секции, назначение создается с помощью циклического перебора.

Издателю событий известен только ключ секции, но не сама секция, в которой публикуются события. Благодаря разделению ключа и секции отправителю не нужно располагать избыточными сведениями о последующей обработке и хранении событий. Уникальное удостоверение устройства или пользователя является хорошим ключом секции, но другие атрибуты, например географическое положение, можно также использовать для группировки связанных событий в одну секцию.

## <a name="sas-tokens"></a>Маркеры SAS

Центры событий используют *подписанные URL-адреса*, доступные на уровне пространства имен и концентратора событий. Маркер SAS создается на основе ключа SAS и хэша SHA URL-адреса, закодированного в определенном формате. С помощью имени ключа (политики) и маркера Центры событий могут повторно создать хэш, чтобы аутентифицировать отправителя. Как правило, маркеры SAS для издателей событий создаются только с правом **отправки** конкретному концентратору событий. Этот механизм URL-адреса маркера SAS является основой для идентификации издателя, представленной в политике издателя. Дополнительные сведения о работе с SAS см. в статье [Проверка подлинности подписи при общем доступе с помощью служебной шины](../service-bus-messaging/service-bus-sas.md).

## <a name="event-consumers"></a>Получатели событий

Любая сущность, считывающая данные из концентратора событий, является *потребителем событий*. Все потребители Центров событий подключаются через сеанс AMQP 1.0, в рамках которого события доставляются, как только становятся доступными. Клиенту не требуется проводить опрос доступности данных.

### <a name="consumer-groups"></a>Группы получателей

Механизм публикации и подписки Центров событий реализован в виде *групп потребителей*. Группа потребителей — это представление всего концентратора событий (состояние, позиция или смещение). Группы потребителей обеспечивают каждому из нескольких потребляющих приложений отдельное представление потока событий, а также возможность считывания потока независимо друг от друга в своем темпе и с собственными смещениями.

В архитектуре обработки потока каждое потребляющее приложение соответствует группе потребителей. Если вы хотите записать данные событий в долговременное хранилище, то приложение, записывающее данные в хранилище, является группой потребителей. Сложную обработку событий затем может выполнить еще одна (отдельная) группа потребителей. К секции можно обращаться только через группу потребителей. В концентраторе событий всегда есть группа потребителей по умолчанию, и можно создать до 20 групп потребителей для концентратора событий стандартного уровня.

В секции для каждой группы потребителей может существовать не более пяти одновременных считывателей. Однако **рекомендуется, чтобы в секции для каждой группы потребителей поддерживался только один активный получатель**. Внутри одного раздела каждый читатель получает все сообщения. Если в одном разделе находится несколько читателей, процесс обработки сообщений будет дублироваться. Это нужно обработать в своем коде, что не может быть тривиальной задачей. Тем не менее для некоторых сценариев такой поход допустим.


Ниже приведены примеры соглашения URI группы потребителей.

```http
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #1]
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #2]
```

На следующем рисунке показана архитектура обработки потока Центров событий.

![Центры событий;](./media/event-hubs-features/event_hubs_architecture.png)

### <a name="stream-offsets"></a>Смещение потока

*Смещение* — это положение события внутри секции. Смещение можно представить как клиентский курсор. Смещение представляет собой байт-нумерацию события. Благодаря этому потребитель события (модуль чтения) может указать точку в потоке событий, с которой требуется начать чтение событий. Можно указать смещение как отметку времени или как значение смещения. Потребители ответственны за хранение своих собственных значений смещения вне службы "Центры событий". В секции каждое событие включает смещение.

![Центры событий;](./media/event-hubs-features/partition_offset.png)

### <a name="checkpointing"></a>Контрольные точки

*Создание контрольных точек* — это процесс, с помощью которого модули чтения помечают или фиксируют свое положение в последовательности событий секции. Создание контрольных точек является ответственностью потребителя и выполняется для каждой секции в пределах группы потребителей. Это означает, что для каждой группы потребителей модуль чтения каждой секции должен хранить свое текущее положение в потоке событий и может сообщать службе, когда он считает поток данных завершенным.

Если модуль чтения отключается от секции, при повторном подключении он приступает к чтению данных с контрольной точки, которая ранее была отправлена последним модулем чтения этой секции в этой группе потребителей. При подключении модуль чтения передает это смещение в концентратор событий, чтобы указать место, с которого следует начинать чтение. Таким образом, можно использовать контрольные точки как для маркировки событий как "завершенных" подчиненными приложениями, так и для обеспечения устойчивости в случае отработки отказа между модулями чтения, работающими на разных компьютерах. Вы можете вернуться к предыдущим данным, указав более низкое значение смещения по отношению к этому процессу создания контрольных точек. В рамках такого подхода при создании контрольных точек вы обеспечиваете отказоустойчивость и воспроизведение потока событий.

### <a name="common-consumer-tasks"></a>Стандартные задачи потребителя

Все потребители Центров событий подключаются через двунаправленный канал связи с поддержкой состояния и сеанса AMQP 1.0. Каждая секция связана с сеансом связи AMQP 1.0, что упрощает транспортировку событий, разделенных по секциям.

#### <a name="connect-to-a-partition"></a>Подключение к секции

При подключении к секциям часто используется механизм аренды для координации подключения к конкретным секциям для чтения. Таким образом, в каждой секции в группе потребителей может быть только один активный модуль чтения. Создание контрольных точек, аренда и администрирование модулей чтения упрощается за счет использования класса [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) для клиентов .NET. Узел обработчика событий — интеллектуальный агент потребителя.

#### <a name="read-events"></a>Чтение событий

После открытия сеанса и связи AMQP 1.0 для определенной секции служба Центров событий доставляет события в клиент AMQP 1.0. Этот механизм доставки обеспечивает более высокую пропускную способность и меньшую задержку, чем механизмы извлечения по запросу, такие как HTTP GET. Когда события отправляются клиенту, каждый экземпляр данных событий содержит важные метаданные, такие как смещение и порядковый номер, которые используются для упрощения создания контрольных точек в последовательности событий.

Данные событий
* Offset
* Порядковый номер
* Текст
* Свойства пользователя
* Свойства системы

Пользователь управляет смещением самостоятельно.

## <a name="capacity"></a>Capacity

Центры событий имеют высокомасштабируемую параллельную архитектуру. Но есть несколько моментов, которые следует учитывать при выборе размера и масштаба.

### <a name="throughput-units"></a>Единицы пропускной способности

Пропускная способность Центров событий выражается в *единицах пропускной способности*. Единицы пропускной способности — это предварительно приобретенные единицы емкости. Одна единица пропускной способности включает следующую емкость:

* Входящий трафик: до 1 МБ в секунду либо 1000 событий в секунду (в зависимости от того, что произойдет раньше).
* Исходящий трафик: до 2 МБ в секунду или 4096 событий в секунду.

Превышение входящей емкости, выраженной в приобретенных единицах пропускной способности, подлежит регулированию с вызовом исключения [ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception). Хотя исходящий трафик не связан с возвратом исключения регулирования, он все равно ограничен емкостью, выраженной в приобретенных пропускных единицах. Если возвращаются исключения скорости публикации или ожидается повышенный объем исходящих данных, проверьте количество единиц пропускной способности, приобретенных для пространства имен. Вы можете управлять единицами пропускной способности из колонки **Масштабирование** пространств имен на [портале Azure](https://portal.azure.com). Это также можно реализовать программным способом с помощью [интерфейсов API Центров событий](event-hubs-api-overview.md).

Единицы пропускной способности тарифицируются почасово и приобретаются заранее. После приобретения единицы пропускной способности тарифицируются минимум за один час. Для пространства имен Центров событий можно приобрести до 20 единиц пропускной способности (этот объем можно распределить между всеми концентраторами событий в пространстве имен).

Дополнительные единицы пропускной способности можно приобрести блоками от 20 до 100 единиц. Для этого обратитесь в службу поддержки Azure. Кроме того, вы можете приобрести блоки по 100 единиц пропускной способности.

Мы рекомендуем сбалансировать количество единиц пропускной способности и секций, чтобы добиться оптимального масштабирования. Одна секция имеет максимальный масштаб в размере одной единицы пропускной способности. Количество единиц пропускной способности должно быть меньше или равно количеству секций в концентраторе событий.

Подробные сведения о ценах см. на странице [цен на Центры событий](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о Центрах событий см. по следующим ссылкам:

* Начните работу с [руководства по Центрам событий][Event Hubs tutorial]
* [Руководство по программированию Центров событий](event-hubs-programming-guide.md)
* [Доступность и согласованность в Центрах событий](event-hubs-availability-and-consistency.md)
* [Часто задаваемые вопросы о Центрах событий](event-hubs-faq.md)
* [Примеры Центров событий][]

[Event Hubs tutorial]: event-hubs-dotnet-standard-getstarted-send.md
[Примеры Центров событий]: https://github.com/Azure/azure-event-hubs/tree/master/samples
