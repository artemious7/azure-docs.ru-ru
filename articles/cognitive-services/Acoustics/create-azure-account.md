---
title: Настройка учетных записей Azure для акустики — Cognitive Services
description: Следуйте этому руководству по настройке учетных записей хранения и учетной записи пакетной службы Azure, необходимых для работы с акустикой.
services: cognitive-services
author: ashtat
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: d5e78df2cb17e8275aef3694dda90a705ef4bdaa
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2018
ms.locfileid: "40181376"
---
# <a name="create-an-azure-batch-account"></a>Создание учетной записи пакетной службы Azure
Следуйте этому руководству по настройке учетных записей хранения и учетной записи пакетной службы Azure, необходимых для работы с акустикой. Сведения о подключаемом модуле Unity, который разработан как часть Project Acoustics, см. в [этой статье](what-is-acoustics.md). Сведения о том, как внедрить акустику в проект Unity, см. в разделе по [началу работы](getting-started.md).  

## <a name="get-an-azure-subscription"></a>Оформление подписки Azure
До настройки учетных записей хранения и учетной записи пакетной службы необходимо получить [подписку Azure](https://azure.microsoft.com/free/). Если регистрируетесь впервые, Azure предоставляет несколько бесплатных ресурсов, которые ограничены по времени, и кредит в сумме 200 долл. США.

## <a name="create-azure-batch-and-storage-accounts"></a>Создание учетных записей хранения и учетной записи пакетной службы Azure
Далее следуйте [этим инструкциям](https://docs.microsoft.com/azure/batch/batch-account-create-portal), чтобы настроить учетную запись пакетной службы Azure и связанные учетные записи хранения Azure.

Выберите параметры по умолчанию для учетных записей хранения и учетной записи пакетной службы:
  
  ![Новая учетная запись пакетной службы](media/NewBatchAccountCreate.png)

  ![Новая учетная запись хранения](media/BatchStorageAccountCreate.png)

Развертывание учетных записей в Azure занимает несколько минут. В правом верхнем углу портала появится уведомление о завершении.
  
  ![Учетные записи развернуты](media/BatchAccountsDeployNotification.png)

Теперь учетные записи должны отображаться на панели мониторинга.
  
  ![Панель мониторинга портала](media/AzurePortalDashboard.png)

## <a name="set-up-acoustics-bake-ui-with-azure-credentials"></a>Настройка пользовательского интерфейса акустической обработки с использованием учетных данных Azure
Щелкните ссылку на учетную запись пакетной службы на панели мониторинга, а затем на открывшейся странице щелкните ссылку **Ключи** для доступа к своим учетным данным.
  
  ![Ссылка на ключи пакетной службы](media/BatchAccessKeys.png)

  ![Учетные данные для учетной записи пакетной службы](media/BatchKeysInfo.png)

Щелкните ссылку **Учетная запись хранения** на странице, чтобы получить доступ к учетным данным учетной записи хранения Azure.
  
  ![Учетные данные учетной записи хранения](media/StorageKeysInfo.png)

Введите эти учетные данные на вкладке создания, как описано в [пошаговом руководстве по пользовательскому интерфейсу сборки](bake-ui-walkthrough.md).

## <a name="node-types-and-region-support"></a>Типы узлов и поддержка регионов
Для Project Acoustics требуются узлы оптимизированных для вычислений виртуальных машин серии H и F, которые могут поддерживаться не во всех регионах Azure. Просмотрите [эту таблицу](https://azure.microsoft.com/global-infrastructure/services), чтобы выбрать правильное расположение для учетной записи пакетной службы. В данный момент виртуальные машины серии H поддерживаются в восточной части США, центрально-северной части США, центрально-южной части США, западной части США, западной части США 2, Северной Европе, Западной Европе и Западной Японии.

## <a name="upgrading-your-quota"></a>Обновление квоты
Учетные записи пакетной службы Azure подготавливаются с ограничением в 20 вычислительных ядер. Вы можете увеличить это ограничение, чтобы ускорить время создания сцены, так как акустическую рабочую нагрузку можно выполнять параллельно на нескольких узлах в пределах количества точек размещения проб в сцене. Вы можете запросить увеличение квоты, щелкнув ссылку **Квота** на странице портала для пакетной службы Azure, а затем щелкните **Запросить увеличение квоты**:

![Увеличение квоты Azure](media/azurequotas.png)

## <a name="next-steps"></a>Дополнительная информация
* Приступите к работе с [интеграцией акустики в проект Unity](getting-started.md).
* Изучите [пример сцены](sample-walkthrough.md).

