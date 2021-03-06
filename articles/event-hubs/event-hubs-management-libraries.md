---
title: Библиотеки управления Центров событий Azure | Документация Майкрософт
description: Управление пространствами имен и сущностями Центров событий из .NET
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.date: 08/13/2018
ms.author: shvija
ms.openlocfilehash: 79cddcac4d469753bc39107e6db2d8ce901111d1
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746422"
---
# <a name="event-hubs-management-libraries"></a>Библиотеки управления Центров событий

Библиотеки управления Центров событий Azure можно использовать, чтобы динамически подготавливать пространства имен и сущности Центров событий. Это дает возможность реализовывать сложные развертывания и сценарии обмена сообщениями, позволяя программно определять, какие сущности следует подготовить. В настоящее время эти библиотеки доступны для .NET.

## <a name="supported-functionality"></a>Поддерживаемые функции

* Создание, обновление, удаление пространства имен.
* Создание, обновление, удаление концентраторов событий в службе "Центры событий".
* Создание, обновление, удаление группы потребителей.

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе с библиотеками управления Центров событий, нужно пройти аутентификацию в Azure Active Directory (AAD). AAD требует аутентификации в качестве субъекта-службы, предоставляющего доступ к вашим ресурсам Azure. Сведения о создании субъекта-службы см. в одной из приведенных ниже статей:  

* [Создание приложения Active Directory и субъекта-службы с доступом к ресурсам с помощью портала](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [Использование Azure PowerShell для создания субъекта-службы и доступа к ресурсам](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Использование интерфейса командной строки Azure для создания субъекта-службы и доступа к ресурсам](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

В этих руководствах вы получите `AppId` (идентификатор клиента), `TenantId` и `ClientSecret` (ключ аутентификации), которые используются библиотеками управления для аутентификации. Необходимо иметь разрешения роли **Владелец** для группы ресурсов, которую вы хотите использовать.

## <a name="programming-pattern"></a>Шаблон программирования

Шаблон обработки любого ресурса Центров событий придерживается общего протокола.

1. Получение токена из AAD с использованием библиотеки `Microsoft.IdentityModel.Clients.ActiveDirectory`.
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. Создание объекта `EventHubManagementClient`.
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. Присвоение параметрам `CreateOrUpdate` указанных значений.
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. Выполнение вызова.
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a>Дополнительная информация
* [Пример управления для .NET](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [Справочник по Microsoft.Azure.Management.EventHub](/dotnet/api/Microsoft.Azure.Management.EventHub) 
