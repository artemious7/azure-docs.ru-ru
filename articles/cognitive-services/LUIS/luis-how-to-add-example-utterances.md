---
title: Добавление примеров высказываний в приложения LUIS
titleSuffix: Azure Cognitive Services
description: Сведения о добавлении фрагментов речи в приложения службы "Распознавание речи" (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 387668263a6bab6e12a21adf04aebfbbf108a006
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47036496"
---
# <a name="add-example-utterances-and-label-with-entities"></a>Добавление примеров высказываний и меток с сущностями

Примеры высказываний являются примерами текста пользовательских вопросов или команд. Для обучения службы "Распознавание речи"(LUIS) необходимо добавить [примеры фрагментов речи](luis-concept-utterance.md) в [намерение](luis-concept-intent.md).

Как правило, сначала следует добавить пример высказывания в намерение, а затем создать сущности и высказывания на странице намерений. Если вам нужно сначала создать сущности, см. [этот](luis-how-to-add-entities.md) раздел.

## <a name="add-an-utterance"></a>Добавление высказывания
На странице намерений введите соответствующий пример высказывания, который ожидается от пользователей (например, `book 2 adult business tickets to Paris tomorrow on Air France`), в текстовом поле под именем намерения и нажмите клавишу ВВОД. 
 
>[!NOTE]
>LUIS преобразует все высказывания в нижний регистр.

![Снимок экрана страницы сведений о намерении с выделенным высказыванием](./media/luis-how-to-add-example-utterances/add-new-utterance-to-intent.png) 

Высказывания добавляются в список высказываний для текущего намерения. 

## <a name="ignoring-words-and-punctuation"></a>Игнорирование слов и пунктуации
Если вам нужно игнорировать некоторые слова или пунктуацию в примере высказывания, используйте [шаблон](luis-concept-patterns.md#pattern-syntax) с синтаксисом _игнорирования_. 

## <a name="add-simple-entity-label"></a>Добавление простой метки сущности
В следующей процедуре создайте и отметьте настраиваемые сущности для следующих высказываний на странице намерений:

```
book me 2 adult business tickets to Paris tomorrow on Air France
```

1. Выберите Air France в высказывании, чтобы пометить его как простую сущность.

    > [!NOTE]
    > Выбрав слова, которые следует пометить, как сущности, сделайте следующее:
    > * Если это одно слово — просто выберите его. 
    > * Если это словосочетание — выделите всю фразу от начала до конца.

2. В окне раскрывающего списка сущностей можно выбрать имеющийся объект или добавить новую сущность. Чтобы добавить новую сущность, введите ее имя в текстовое поле и выберите **Create new entity** (Создать сущность). 
 
    ![Снимок экрана страницы сведений о намерениях с выделенным пунктом простой маркировки сущностей](./media/luis-how-to-add-example-utterances/create-airline-simple-entity.png)

3. Во всплывающем диалоговом окне **What type of entity do you want to create?** (Какой тип сущности необходимо создать?) проверьте имя сущности, выберите простой тип сущности и щелкните **Done** (Готово).

    ![Образ диалогового окна подтверждения](./media/luis-how-to-add-example-utterances/create-simple-airline-entity.png)

    Дополнительные сведения об извлечении простой сущности из ответа на запрос JSON конечной точки см. в [этом разделе](luis-concept-data-extraction.md#simple-entity-data). Дополнительные сведения об использовании простой сущности см. в соответствующем [кратком руководстве](luis-quickstart-primary-and-secondary-data.md).


## <a name="add-list-entity-and-label"></a>Добавление сущности списка и метки
Сущности списка представляют собой фиксированный, закрытый набор (точные текстовые совпадения) связанных слов в системе. 

Для сущности списка напитков можно установить два нормализованных значения: вода и содовая. Каждое нормализованное название имеет синонимы. Синонимы к слову "вода": H20, газировка, простая вода. Синонимы к слову "содовая": соковая вода, кола, имбирная вода. При создании сущности не нужно знать все значения. Вы можете добавить больше значений после просмотра высказываний реального пользователя с синонимами.

|Нормализованное название|синонимы;|
|--|--|
|Вода|H20, газировка, простая вода|
|Содовая|Соковая вода, кола, имбирная вода|

При создании сущности списка со страницы намерений выполняется два действия, которые могут быть неявными. Во-первых, создается список путем добавления первого элемента списка. Во-вторых, первый элемент списка называется словом или фразой, выбранными из высказывания. Хотя вы можете изменить это позже со страницы сущности, может быть быстрее выбрать высказывание со словом, которым нужно назвать элемент списка.

Например, если нужно создать список типов напитков и вы выбрали из высказывания слово `h2o`, для создания сущности в списке будет один элемент с именем h20. Если требуется универсальное имя, следует выбрать высказывание, использующее более общее имя. 

1. В высказывании выберите слово, которое является первым в списке, а затем введите имя списка в текстовое поле и выберите **Create new entity** (Создать сущность).   

    ![Снимок экрана страницы сведений о намерениях с выделенным пунктом Create new entity (Создать сущность)](./media/luis-how-to-add-example-utterances/create-drink-list-entity.png)

2. В диалогом окне **What type of entity do you want to create?** (Какой тип сущности необходимо создать?) добавьте синонимы данного элемента списка. Для элемента "вода" в списке напитков добавьте `h20`, `perrier` и `waters`, а затем выберите **Done** (Готово). Обратите внимание, что словосочетание "минеральные воды" добавляется, так как синонимы списка сопоставляются на уровне токена. В английском языке и его региональных параметрах этот уровень находится на уровне слова, поэтому словосочетание "минеральные воды" не будут соответствовать слову "вода", если только оно не указано в списке. 

    ![Снимок экрана диалогового окна What type of entity do you want to create? (Какой тип сущности необходимо создать?)](./media/luis-how-to-add-example-utterances/drink-list-ddl.png)

    Этот список напитков имеет только один тип напитков (вода). Вы можете добавить дополнительные типы напитков, отметив другие высказывания или изменив сущность на панели навигации **Сущности** слева. [Редактирование](luis-how-to-add-entities.md#add-list-entities) сущностей предоставляет варианты ввода дополнительных элементов с соответствующими синонимами или [импорта](luis-how-to-add-entities.md#import-list-entity-values) списка. 

    Дополнительные сведения об извлечении сущностей списков из ответа на запрос JSON конечной точки см. в [этом разделе](luis-concept-data-extraction.md#list-entity-data). Дополнительные сведения об использовании сущности списка см. в соответствующем [кратком руководстве](luis-quickstart-intent-and-list-entity.md).

## <a name="add-synonyms-to-the-list-entity"></a>Добавление синонимов в список сущностей 
Добавьте синоним в список сущностей, выбрав слово или фразу в высказывании. При наличии сущности списка напитков и необходимости добавить `agua` как синоним для слова "вода", следуйте таким указаниям:

В высказывании выберите для слова "вода" синоним, например `aqua`, выберите имя сущности списка в раскрывающемся списке, например **Напитки**, щелкните **Set as synonym** (Установить как синоним), а затем выберите элемент списка, для которого имя является синонимом (например, **вода**).

![Снимок экрана страницы сведений о намерениях с выделенным пунктом создания синонима](./media/luis-how-to-add-example-utterances/set-agua-as-synonym.png)

## <a name="create-new-item-for-list-entity"></a>Создание элемента для сущности списка
Создайте элемент для имеющейся сущности списка, выбрав слово или фразу в высказывании. При наличии списка напитков и необходимости добавить `tea` как новый элемент, следуйте таким указаниям:

В высказывании выберите слово для нового элемента списка (например `tea`), выберите имя сущности списка в раскрывающемся списке (например, **Напиток**), а затем выберите **Create a new synonym** (Создать синоним). 

![Снимок экрана добавления нового элемента списка](./media/luis-how-to-add-example-utterances/list-entity-create-new-item.png)

Теперь слово выделено синим цветом. Если навести указатель мыши на это слово, отобразится тег с именем элемента списка (например, "чай").

![Снимок экрана с тэгом нового элемента списка](./media/luis-how-to-add-example-utterances/list-entity-item-name-tag.png)

## <a name="wrap-entities-in-composite-label"></a>Создание программы-оболочки в составной метке
Составные сущности создаются в разделе **Сущности**. Невозможно создать составную сущность на странице намерений. После создания составной сущности можно создавать программу-оболочку сущностей в высказывании на странице намерений. 

Если высказывание — `book 2 tickets from Seattle to Cairo`, составное высказывание может возвращать сведения о сущности количества билетов (2), исходное расположение (Сиэтл) и целевое расположение (Каир) в одной родительской сущности. 

Выполните следующие [действия](luis-how-to-add-entities.md#add-prebuilt-entity), чтобы добавить **номер** готовой сущности. После создания сущности `2` в высказывании выделяется синим цветом, что свидетельствует о том, что это сущность с меткой. Предварительно созданные сущности помечаются LUIS. Невозможно добавить или удалить метку предварительно созданной сущности из одного высказывания. Вы можете только добавлять или удалять все предварительно созданные метки, добавив или удалив предварительно созданную сущность из приложения.

Выполните следующие [действия](#add-hierarchical-entity-and-label) для создания иерархической сущности **Location**. Отметьте исходное и целевое расположения в примере высказывания. 

Прежде чем переносить сущности в составную сущность, убедитесь, что все дочерние объекты выделены синим цветом (это означает, что они помечены в высказывании).

1. Чтобы создать программу-оболочку для отдельных сущностей в составной сущности, выберите первую сущность с метками в высказывании составной сущности. В примере высказывания `book 2 tickets from Seattle to Cairo` первая сущность имеет значение 2. Отображается раскрывающийся список с вариантами для этого раздела.

    ![Снимок экрана с выбранными номерами и раскрывающимся списком параметров](./media/luis-how-to-add-example-utterances/wrap-1.png)

2. Выберите **Wrap composite entity** (Создать программу-оболочку составной сущности) из раскрывающегося списка. 

    ![Снимок экрана раскрывающегося списка параметров для создания программы-оболочки составной сущности с выделенным параметром переноса в составную сущность](./media/luis-how-to-add-example-utterances/wrap-2.png)

3. Выберите последнее слово составной сущности. В высказывании в этом примере выберите Location::Destination (представляющий Каир). Теперь зеленая линия находится под словами, включая слова не являющиеся сущностями высказывания, которые являются составными.

    ![Снимок экрана страницы намерений BookFlight с выделенным числом](./media/luis-how-to-add-example-utterances/wrap-composite.png)

4. Выберите название составной сущности из раскрывающегося списка. В этом примере это **TicketOrder**.

    ![Снимок экрана переноса слов с составной сущностью с именем составной сущности, выделенной в раскрывающемся списке](./media/luis-how-to-add-example-utterances/wrap-4.png)

    Если правильно создать программу-оболочку сущности, зеленая линия находится под всей этой фразой.

    ![Снимок экрана высказывания с выделенной составной сущностью](./media/luis-how-to-add-example-utterances/wrap-5.png)

    Дополнительные сведения об извлечении составной сущности из ответа на запрос JSON конечной точки см. в [этом разделе](luis-concept-data-extraction.md#composite-entity-data). Дополнительные сведения об использовании составной сущности см. в соответствующем [кратком руководстве](luis-tutorial-composite-entity.md).

## <a name="add-hierarchical-entity-and-label"></a>Добавление иерархической сущности и метки
Иерархическая сущность — это категория контекстно-зависимых обученных и концептуально связанных сущностей. В следующем примере сущность содержит исходное и конечное расположения. 

В высказывании `Book 2 tickets from Seattle to Cairo` "Сиэтл" является исходным, а "Каир" — конечным расположением. Каждое расположение контекстуально отличается и получается на основе порядка и выбора слов в высказывании.

1. На странице намерений в высказывании выберите "Сиэтл", а затем введите имя сущности Location и щелкните **Create new entity** (Создать сущность).

    ![Снимок экрана диалогового окна Create Hierarchical Entity Labeling (Создание меток иерархических сущностей)](./media/luis-how-to-add-example-utterances/create-hier-ent-1.png)

2. Во всплывающем диалоговом окне выберите Hierarchical (Иерархический) для параметра **Тип сущности**, добавьте `Origin` и `Destination` как дочерние элементы, а затем выберите **Done** (Готово).

    ![Снимок экрана страницы сведений о намерениях с выделенной сущностью ToLocation](./media/luis-how-to-add-example-utterances/create-location-hierarchical-entity.png)

3. Слово в высказывании было помечено иерархической родительской сущностью. Необходимо назначить слово дочерней сущности. Вернитесь к высказыванию на странице намерений. Выделите слово, а затем из раскрывающегося списка выберите имя сущности, которую вы создали, и перейдите в меню справа, чтобы выбрать необходимую дочернюю сущность.

    ![Снимок экрана страницы сведений о намерениях с выделенной сущностью ToLocation](./media/luis-how-to-add-example-utterances/label-tolocation.png)

    >[!CAUTION]
    >Имена дочерних сущностей должны быть уникальными для всех сущностей в одном приложении. Две разные иерархические сущности не могут содержать дочерние элементы с тем же именем. 

    Дополнительные сведения об извлечении иерархической сущности из ответа на запрос JSON конечной точки см. в [этом разделе](luis-concept-data-extraction.md#hierarchical-entity-data). Дополнительные сведения об использовании иерархической сущности см. в соответствующем [кратком руководстве](luis-quickstart-intent-and-hier-entity.md).


## <a name="remove-entity-labels-from-utterances"></a>Удаление меток сущности из высказываний
Вы можете удалить прошедшие машинное обучение метки сущностей из высказывания на странице намерений. Если сущность не прошла машинное обучение, ее нельзя удалить из высказывания. Если необходимо удалить сущность, которая не прошла машинное обучение, необходимо удалить ее из всего приложения. 

Чтобы удалить из высказывания метку сущности, прошедшей машинное обучение, выберите необходимую сущность в высказывании. Затем выберите **Remove Label** (Удалить метку) в появившемся поле раскрывающегося списка сущностей.

![Снимок экрана страницы сведений о намерении с выделенным пунктом удаления метки](./media/luis-how-to-add-example-utterances/remove-label.png) 

## <a name="add-prebuilt-entity-label"></a>Добавление предварительно созданной сущности
При добавлении предварительно созданных сущностей в приложение LUIS не нужно отмечать высказывания этими сущностями. Дополнительные сведения о предварительно созданных сущностях и их добавлении см. в [этом разделе](luis-how-to-add-entities.md#add-prebuilt-entity).

## <a name="add-regular-expression-entity-label"></a>Добавление метки сущности регулярного высказывания
При добавлении сущностей регулярного высказывания в приложение LUIS не нужно отмечать высказывания этими сущностями. Дополнительные сведения о сущностях регулярного высказывания и их добавлении см. в [этом разделе](luis-how-to-add-entities.md#add-regular-expression-entities).

## <a name="create-a-pattern-from-an-utterance"></a>Создание шаблона на основе высказывания
Дополнительные сведения см. в разделе [Добавление шаблона на основе существующего фрагмента речи на странице сущности или намерения](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="add-patternany-entity-label"></a>Добавление метки сущности pattern.any
При добавлении сущностей pattern.any в приложение LUIS невозможно отметить высказывания этими сущностями. Они действительны только в шаблонах. Дополнительные сведения о сущностях pattern.any и их добавлении см. в [этом разделе](luis-how-to-add-entities.md#add-patternany-entities).

<!--
Fix this - moved to luis-how-to-add-intents.md - how ?

## Search in utterances
## Prediction discrepancy errors
## Filter by intent prediction discrepancy errors
## Filter by entity type
## Switch to token view
## Delete utterances
## Edit an utterance
## Reassign utterances

-->
## <a name="train-your-app-after-changing-model-with-utterances"></a>Обучение приложения после изменения модели с высказываниями
После добавления, изменения и удаления высказываний выполните [обучение](luis-how-to-train.md) и [публикацию](luis-how-to-publish-app.md) приложения, чтобы применить изменения к запросам конечной точки. 

## <a name="next-steps"></a>Дополнительная информация

Отметив высказывания в намерениях, можно создать [составную сущность](luis-how-to-add-entities.md).
