---
title: включение файла
description: включение файла
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: b656001c8a7d1bed21c208bc643018c5f751e09c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38733829"
---
Теперь вы можете использовать обозреватель данных на портале Azure для создания базы данных графов. 

1. Щелкните **Обозреватель данных** > **Новый граф**.

    Справа отобразится область **Добавление графа** (вам может потребоваться прокрутить вправо, чтобы увидеть ее).

    ![Страница добавления графа в обозревателе данных на портале Azure](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer-graph.png)

2. На странице **Добавление графа** введите параметры для нового графа.

    Параметр|Рекомендуемое значение|ОПИСАНИЕ
    ---|---|---
    Идентификатор базы данных|sample-database|Введите имя новой базы данных, например *sample-database*. Имя базы данных может иметь длину от 1 до 255 символов и не может содержать `/ \ # ?` или пробел.
    Идентификатор графа|sample-graph|Введите имя новой коллекции, например *sample-graph*. Для имен графов предусмотрены те же требования к символам, что и для идентификаторов баз данных.
    Емкость хранилища|Фиксированный (10 ГБ)|Оставьте значение по умолчанию — **Fixed (10 GB)** (Фиксированный (10 ГБ)). Это значение представляет емкость хранилища базы данных.
    Throughput|400 ЕЗ|Укажите для пропускной способности 400 единиц запросов в секунду. Чтобы сократить задержку, позже вы можете увеличить масштаб пропускной способности.

3. После заполнения формы нажмите кнопку **ОК**.
