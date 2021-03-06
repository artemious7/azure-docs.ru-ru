---
title: Предоплата за использование ресурсов Azure Cosmos DB для экономии средств | Документация Майкрософт
description: Сведения о том, как воспользоваться резервной мощностью Azure Cosmos DB для сокращения затрат на вычисления.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 1be2d67d8a1ee51c4883ae1f50b80ad3a9691c2d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46981973"
---
# <a name="prepay-for-azure-cosmos-db-resources-with-reserved-capacity"></a>Предоплата ресурсов Azure Cosmos DB с резервной мощностью

Резервная мощность Azure Cosmos DB позволяет экономить деньги за счет предварительной оплаты ресурсов Azure Cosmos DB в течение заданного периода (один или три года). Резервная мощность Azure Cosmos DB позволяет получить скидку на пропускную способность, подготовленную для ресурсов Cosmos DB, например базы данных, контейнеры (таблицы/коллекции/графы). Однолетние и трехлетние контракты на резервную мощность Azure Cosmos DB с предварительной оплатой позволят сэкономить существенную сумму (до 65 %) по сравнению с обычной платой. Резервная мощность предоставляет скидку на выставление счетов и не влияет на состояние среды выполнения ресурсов Cosmos DB.

Резервная мощность Azure Cosmos DB покрывает расходы на пропускную способность, подготовленную для ресурсов, но не распространяется на расходы хранилища и сети. После того как вы купите резервирование, при начислении платы за пропускную способность, соответствующей атрибутам резервирования, не будут применяться ставки оплаты по мере использования. Дополнительные сведения о резервировании см. в статье [Общие сведения об Azure Reserved VM Instances](../billing/billing-save-compute-costs-reservations.md). 

Вы можете приобрести резервную мощность Azure Cosmos DB на [портале Azure](https://portal.azure.com). Чтобы приобрести резервную мощность:

* Необходимо быть участником роли "Владелец" по крайней мере для одной подписки с соглашением Enterprise или подписки с оплатой по мере использования.  
* Для подписок с Соглашением Enterprise покупки резервирования Azure следует включить на [портале EA](https://ea.azure.com/).  
* В рамках программы для поставщиков облачных решений (CSP) только агенты администрирования или агенты продаж могут приобрести резервную мощность Azure Cosmos DB.

## <a name="determine-the-required-throughput-before-purchase"></a>Определение требуемой пропускной способности перед приобретением

Размер резервирования должен основываться на общем объеме пропускной способности, используемой имеющимися или будущими ресурсами Azure Cosmos DB (например, базы данных или контейнеры: коллекции, таблицы, графы). Требуемую пропускную способность можно определить одним из следующих способов:

* Перейдите на [портал Azure](https://portal.azure.com), найдите учетную запись Azure Cosmos DB, откройте колонку "Метрики" и получите подробные сведения о средней пропускной способности в секунду за 3–6 месяцев на вкладке **Пропускная способность**. Предоставьте этот размер в качестве единиц резервной мощности при покупке.

Кроме этого, если вы являетесь клиентом с Соглашением Enterprise (EA), то для получения информации о пропускной способности Azure Cosmos DB можно скачать файл потребления и использовать значение **Тип службы** в разделе **Дополнительная информация** этого файла.

Вы также можете суммировать среднюю пропускную способность для всех рабочих нагрузок в учетных записях Azure Cosmos DB, которая планируется на следующие один или три года, и использовать это значение для резервирования.

## <a name="buy-azure-cosmos-db-reserved-capacity"></a>Приобретение резервной мощности Azure Cosmos DB

1. Войдите на [портале Azure](https://portal.azure.com).  

2. Выберите **Все службы** > **Резервирования** > **Добавить**.  

3. На панели **Выберите тип продукта** выберите **Azure Cosmos DB**, а затем щелкните **Выбрать**, чтобы приобрести новое резервирование.  

4. Заполните обязательные поля, как описано в следующей таблице:

   ![Заполнение формы резервной мощности](./media/cosmos-db-reserved-capacity/fill_reserved_capacity_form.png) 

   |Поле  |ОПИСАНИЕ  |
   |---------|---------|
   |ИМЯ   |    Название резервирования. Это поле автоматически заполняется значением `CosmosDB_Reservation_<timeStamp>`. Вы можете указать другое название во время создания или изменить название после создания резервирования.      |
   |Подписка  |   Подписка, используемая для оплаты резервной мощности Azure Cosmos DB. Метод оплаты для выбранной подписки используется для взимания первоначальных затрат. Необходимо наличие одного из следующих типов подписки: <br/><br/>  [Соглашение Enterprise](https://azure.microsoft.com/pricing/enterprise-agreement/) (номер предложения: MS-AZR-0017P). Для подписки с Соглашением Enterprise плата вычитается из баланса денежных обязательств или относится к избыточным расходам. <br/><br/> [Оплата по мере использования](https://azure.microsoft.com/offers/ms-azr-0003p/) (номер предложения: MS-AZR-0003P). Для подписки с оплатой по мере использования плата взимается с помощью метода оплаты (кредитной карты или счета), указанного для подписки.    |
   |Область   |   Область действия резервирования определяет, на сколько подписок распространяется скидка на использование резервной мощности и как эта мощность используется в отдельных подписках. Резервирование предусматривает область действия одной или любой подписки. Если выбрать:   <br/><br/>  **Одна подписка**. Скидка на резервирование применяется к экземплярам Azure Cosmos DB в выбранной подписке. <br/><br/>  **Любая подписка**. Скидка на резервирование применяется к экземплярам Azure Cosmos DB, работающим в любых подписках в текущем контексте выставления счетов. Контекст выставления счетов определяется в зависимости от способа регистрации в Azure. Для клиентов с соглашением Enterprise общая область действует в рамках регистрации и включает в себя все подписки (за исключением подписок для разработки и тестирования), заданные при регистрации. Для клиентов с подпиской с оплатой по мере использования общая область включает в себя все подписки с оплатой по мере использования, созданные администратором учетной записи.  <br/><br/> После приобретения резервной мощности можно изменить область резервирования.  |
   |Тип резервной мощности   |  Тип резервной мощности представляет собой подготовленную пропускную способность, выраженную в единицах запроса.|
   |Единицы резервной мощности  |      Объем пропускной способности, который необходимо зарезервировать. Вы можете вычислить это значение путем определения пропускной способности, необходимой для всех ресурсов Cosmos DB (например, баз данных или контейнеров) в регионе, а затем умножив полученное значение на количество регионов, которые нужно связать с базой данных Cosmos DB.  <br/> Например, если у вас есть пять регионов с 1 млн ЕЗ/с в каждом, выберите 5 миллионов ЕЗ/с, чтобы приобрести емкость резервирования.    |
   |Термин  |   Один или три года.   |

5. Просмотрите скидку и стоимость резервирования в разделе **Стоимость**. Цена за резервирование применяется к ресурсам Azure Cosmos DB с подготовленной пропускной способностью во всех регионах.  

6. Щелкните **Приобрести**. После успешной покупки отобразится окно, указанное на следующем снимке экрана. 

   ![Заполнение формы резервной мощности](./media/cosmos-db-reserved-capacity/reserved_capacity_successful.png) 

После приобретения резервирования оно должно немедленно примениться ко всем имеющимся ресурсам Azure Cosmos DB, которые соответствуют условиям резервирования. Если у вас нет ресурсов Azure Cosmos DB, резервирование будет применяться при развертывании нового экземпляра Cosmos DB, который соответствует условиям резервирования. В обоих случаях период резервирования начинается сразу после успешной операции приобретения. 

По истечении срока действия резервирования экземпляры Azure Cosmos DB будут продолжать работу, и плата за них будет взиматься по обычным тарифам оплаты по мере использования.

## <a name="next-steps"></a>Дополнительная информация

Скидка на резервирование автоматически применяется к ресурсам Azure Cosmos DB, которые соответствуют области и атрибутам резервирования. Вы можете обновить область резервирования с помощью портала Azure, PowerShell, CLI или API.

*  Чтобы узнать, как скидки на резервную мощность применяются к Azure Cosmos DB, ознакомьтесь со статьей [Общие сведения о применении скидки на резервирование в Azure Cosmos DB](../billing/billing-understand-cosmosdb-reservation-charges.md)

* Дополнительные сведения о резервировании в Azure см. в следующих статьях:

   * [Основные сведения о резервировании в Azure](../billing/billing-save-compute-costs-reservations.md)  
   * [Управление резервированиями для ресурсов Azure](../billing/billing-manage-reserved-vm-instance.md)  
   * [Общие сведения об использовании зарезервированных экземпляров Azure с Соглашением о регистрации Enterprise](../billing/billing-understand-reserved-instance-usage-ea.md)  
   * [Общие сведения об использовании резервирования Azure для подписки с оплатой по мере использования](../billing/billing-understand-reserved-instance-usage.md)
   * [Продажа Microsoft Azure Reserved VM Instances](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Требуется помощь? Обратитесь в службу поддержки.

Если у вас есть дополнительные вопросы, [обратитесь в службу поддержки](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), которая поможет быстро устранить проблему.

