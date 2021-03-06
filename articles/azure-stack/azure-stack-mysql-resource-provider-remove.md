---
title: Удаление поставщика ресурсов MySQL в Azure Stack | Документация Майкрософт
description: Сведения об удалении поставщика ресурсов MySQL из развертывания Azure Stack.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: dcd1c40717cb35fe4daa9ab9e2c66f334ffff5fe
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2018
ms.locfileid: "49361504"
---
# <a name="remove-the-mysql-resource-provider"></a>Удаление поставщика ресурсов MySQL

Прежде чем удалить поставщик ресурсов MySQL, необходимо удалить все его зависимости. Кроме того, потребуется скопировать пакет развертывания, который был использован для установки поставщика ресурсов.

## <a name="dependency-cleanup"></a>Очистка зависимостей

Есть несколько задач очистки, которые нужно запустить перед выполнением скрипта DeployMySqlProvider.ps1 для удаления поставщика ресурсов.

Некоторые из задач очистки выполняют сами клиенты:

* Удаление всех своих баз данных из поставщика ресурсов. (При удалении баз данных клиента данные не удаляются).
* Отмена регистрации в пространстве имен поставщика.

Администраторы выполняют следующие задачи очистки:

* Удаление серверов размещения из адаптера MySQL.
* Удаление всех планов, которые ссылаются на этот адаптер MySQL.
* Удаление всех квот, которые связаны с этим адаптером MySQL.

## <a name="to-remove-the-mysql-resource-provider"></a>Удаление поставщика ресурсов MySQL

1. Убедитесь, что удалены все существующие зависимости поставщика ресурсов MySQL.

   >[!NOTE]
   >Удаление поставщика ресурсов MySQL продолжится, даже если в этот момент его используют зависимые ресурсы.
  
2. Получите копию двоичного файла поставщика ресурсов MySQL и запустите файл для самостоятельного извлечения содержимого во временный каталог.
3. Получите копию двоичного файла поставщика ресурсов SQL и запустите файл для самостоятельного извлечения содержимого во временный каталог.
4. Откройте новую консоль PowerShell с повышенными привилегиями и перейдите в каталог, в который вы извлекли двоичные файлы поставщика ресурсов MySQL.
5. Запустите скрипт DeployMySqlProvider.ps1 со следующими параметрами:
    - **Uninstall**. Удаляет поставщик ресурсов и все связанные с ним ресурсы.
    - **PrivilegedEndpoint**. IP-адрес или DNS-имя привилегированной конечной точки.
    - **AzureEnvironment**. Среда Azure, используемая для развертывания Azure Stack. Требуется только для развертываний Azure AD.
    - **CloudAdminCredential**. Учетные данные администратора облака, необходимые для доступа к привилегированной конечной точке.
    - **DirectoryTenantID**
    - **AzCredential**. Учетные данные администратора службы Azure Stack. Используйте те же учетные данные, которые вы указали при развертывании Azure Stack.

## <a name="next-steps"></a>Дополнительная информация

[Предложение служб приложений как PaaS](azure-stack-app-service-overview.md)
