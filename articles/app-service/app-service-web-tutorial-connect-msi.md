---
title: Безопасное подключение к Базе данных SQL Azure из службы приложений с использованием управляемого удостоверения | Документация Майкрософт
description: Узнайте, как сделать подключение к базе данных более безопасным с использованием управляемого удостоверения, а также как применить это к другим службам Azure.
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: syntaxc4
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 04/17/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 3125db03dc13f70524fd094736f50b563ef712a4
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/11/2018
ms.locfileid: "44379933"
---
# <a name="tutorial-secure-azure-sql-database-connection-from-app-service-using-a-managed-identity"></a>Руководство. Безопасное подключение к Базе данных SQL Azure из службы приложений с использованием управляемого удостоверения

[Служба приложений](app-service-web-overview.md) — это служба веб-размещения с самостоятельной установкой исправлений и высоким уровнем масштабируемости в Azure. Она также предоставляет [управляемое удостоверение](app-service-managed-service-identity.md) для вашего приложения, которое является готовым решением для защиты доступа к [Базе данных SQL Azure](/azure/sql-database/) и другим службам Azure. Управляемые удостоверения в службе приложений делают ваше приложение более безопасным, устраняя из него секреты, такие как учетные данные в строках подключения. В этом руководстве вы добавите управляемое удостоверение в пример веб-приложения ASP.NET, которое вы создали при работе с руководством [по созданию приложения ASP.NET в Azure с подключением к Базе данных SQL](app-service-web-tutorial-dotnet-sqldatabase.md). Когда вы закончите, ваш пример приложения будет безопасно подключаться к базе данных SQL без необходимости использования имен пользователей и паролей.

Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Включение управляемых удостоверений
> * Предоставление управляемому удостоверению доступа к Базе данных SQL
> * Настройка кода приложения для проверки подлинности с помощью базы данных SQL с использованием проверки подлинности Azure Active Directory.
> * Предоставление управляемому удостоверению минимальных привилегий в Базе данных SQL

> [!NOTE]
> Аутентификация Azure Active Directory _отличается_ от [ интегрированной аутентификации Windows](/previous-versions/windows/it-pro/windows-server-2003/cc758557(v=ws.10)) в локальном экземпляре Active Directory (AD DS). AD DS и Azure Active Directory используют разные протоколы аутентификации. См. дополнительные сведения о [различии между Windows Server AD DS и Azure AD](../active-directory/fundamentals/understand-azure-identity-solutions.md#the-difference-between-windows-server-ad-ds-and-azure-ad).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Предварительные требования

Эта статья является продолжением статьи [Руководство. Создание приложения ASP.NET в Azure с подключением к базе данных SQL](app-service-web-tutorial-dotnet-sqldatabase.md). Если вы еще этого не сделали, сначала ознакомьтесь с этим руководством. Кроме того, вы можете адаптировать шаги для своего собственного приложения ASP.NET с базой данных SQL.

<!-- ![app running in App Service](./media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png) -->

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="enable-managed-identities"></a>Включение управляемых удостоверений

Чтобы включить управляемое удостоверение для приложения Azure, используйте команду [az webapp identity assign](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) в Cloud Shell. В следующей команде замените *\<app name>*.

```azurecli-interactive
az webapp identity assign --resource-group myResourceGroup --name <app name>
```

Ниже приведен пример выходных данных после создания удостоверения в Azure Active Directory:

```json
{
  "additionalProperties": {},
  "principalId": "21dfa71c-9e6f-4d17-9e90-1d28801c9735",
  "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47",
  "type": "SystemAssigned"
}
```

Значение `principalId` потребуется вам на следующем шаге. Если вы хотите увидеть подробности нового удостоверения в Azure Active Directory, запустите следующую необязательную команду со значением `principalId`:

```azurecli-interactive
az ad sp show --id <principalid>
```

## <a name="grant-database-access-to-identity"></a>Предоставление доступа к базе данных

Теперь вы предоставите удостоверению службы приложения доступ к базе данных, используя команду [`az sql server ad-admin create`](/cli/azure/sql/server/ad-admin?view=azure-cli-latest#az-sql-server-ad-admin_create) в Cloud Shell. В следующей команде замените *\<server_name>* и <principalid_from_last_step>. Введите имя администратора вместо *\<admin_user>*.

```azurecli-interactive
az sql server ad-admin create --resource-group myResourceGroup --server-name <server_name> --display-name <admin_user> --object-id <principalid_from_last_step>
```

Теперь управляемое удостоверение имеет доступ к вашему серверу Базы данных SQL Azure.

## <a name="modify-connection-string"></a>Изменение строки подключения

Измените подключение, которое вы установили ранее для своего приложения, используя команду [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) в Cloud Shell. В следующей команде замените *\<app name>* на имя вашего приложения, а также *\<server_name>* и *\<db_name>* именами вашей базы данных SQL.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection='Server=tcp:<server_name>.database.windows.net,1433;Database=<db_name>;' --connection-string-type SQLAzure
```

## <a name="modify-aspnet-code"></a>Изменение кода ASP.NET

В проекте **DotNetAppSqlDb** в Visual Studio откройте _packages.config_ и добавьте следующую строку в список пакетов.

```xml
<package id="Microsoft.Azure.Services.AppAuthentication" version="1.1.0-preview" targetFramework="net461" />
```

Откройте _Models\MyDatabaseContext.cs_ и добавьте следующие инструкции `using` в начало файла:

```csharp
using System.Data.SqlClient;
using Microsoft.Azure.Services.AppAuthentication;
using System.Web.Configuration;
```

Добавьте в класс `MyDatabaseContext` следующий конструктор:

```csharp
public MyDatabaseContext(SqlConnection conn) : base(conn, true)
{
    conn.ConnectionString = WebConfigurationManager.ConnectionStrings["MyDbConnection"].ConnectionString;
    // DataSource != LocalDB means app is running in Azure with the SQLDB connection string you configured
    if(conn.DataSource != "(localdb)\\MSSQLLocalDB")
        conn.AccessToken = (new AzureServiceTokenProvider()).GetAccessTokenAsync("https://database.windows.net/").Result;

    Database.SetInitializer<MyDatabaseContext>(null);
}
```

Этот конструктор настраивает собственный объект SqlConnection для использования маркера доступа базы данных Azure SQL из службы приложений. С помощью маркера доступа приложение службы приложений проходит проверку подлинности в Базе данных SQL Azure с использованием управляемого удостоверения. Дополнительные сведения см. в разделе [Получение маркеров для ресурсов Azure](app-service-managed-service-identity.md#obtaining-tokens-for-azure-resources). Оператор `if` позволяет продолжить тестирование приложения локально с помощью LocalDB.

> [!NOTE]
> `SqlConnection.AccessToken` в настоящее время поддерживается только в .NET Framework 4.6 и выше, но не в [.NET Core](https://www.microsoft.com/net/learn/get-started/windows).
>

Чтобы использовать этот новый конструктор, откройте `Controllers\TodosController.cs` и найдите строку `private MyDatabaseContext db = new MyDatabaseContext();`. В имеющемся коде используется контроллер по умолчанию `MyDatabaseContext` для создания базы данных с использованием стандартной строки подключения, в которой есть имя пользователя и пароль в виде обычного текста до [того, как вы его измените](#modify-connection-string).

Замените всю эту строку следующим кодом.

```csharp
private MyDatabaseContext db = new MyDatabaseContext(new SqlConnection());
```

### <a name="publish-your-changes"></a>Публикация изменений

Теперь требуется опубликовать изменения в Azure.

В **обозревателе решений** щелкните правой кнопкой мыши проект **DotNetAppSqlDb** и выберите **Опубликовать**.

![Публикация в обозревателе решений](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

На странице публикации щелкните **Опубликовать**. Когда на новой веб-странице отображается список дел, ваше приложение подключается к базе данных с управляемым удостоверением.

![Веб-приложение Azure после включения Code First Migrations](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Теперь вы должны иметь возможность редактировать список дел по-прежнему.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="grant-minimal-privileges-to-identity"></a>Предоставление удостоверению минимальных привилегий

На предыдущих шагах вы могли заметить, что управляемое удостоверение подключается к SQL Server с правами администратора Azure AD. Чтобы предоставить управляемому удостоверению минимальные привилегии, вам необходимо войти на сервер Базы данных SQL Azure в качестве администратора Azure AD и добавить группу Azure Active Directory, которая содержит это управляемое удостоверение. 

### <a name="add-managed-identity-to-an-azure-active-directory-group"></a>Добавление управляемого удостоверения в группу Azure Active Directory

В Cloud Shell добавьте управляемое удостоверение, назначенное приложению, в новую группу Azure Active Directory с именем _myAzureSQLDBAccessGroup_ , как демонстрируется в следующем сценарии:

```azurecli-interactive
groupid=$(az ad group create --display-name myAzureSQLDBAccessGroup --mail-nickname myAzureSQLDBAccessGroup --query objectId --output tsv)
msiobjectid=$(az webapp identity show --resource-group <group_name> --name <app_name> --query principalId --output tsv)
az ad group member add --group $groupid --member-id $msiobjectid
az ad group member list -g $groupid
```

Если вы хотите увидеть полные выходные данные JSON для каждой команды, удалите параметры `--query objectId --output tsv`.

### <a name="reconfigure-azure-ad-administrator"></a>Перенастройка администратора Azure AD

Ранее вы назначили для управляемого удостоверения роль администратора Azure AD в Базе данных SQL. Вы не можете использовать это удостоверение для интерактивного входа (для добавления пользователей базы данных), поэтому вам нужно использовать настоящего пользователя Azure AD. Чтобы сделать это, выполните шаги, описанные в разделе [Подготовка администратора Azure Active Directory для сервера базы данных SQL Azure](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server). 

### <a name="grant-permissions-to-azure-active-directory-group"></a>Предоставление разрешений группе Azure Active Directory

В Cloud Shell войдите в базу данных SQL с помощью команды SQLCMD. Замените _\<servername>_ на имя сервера базы данных SQL, а _\<AADusername>_ и _\<AADpassword>_ учетными данными пользователя Azure AD.

```azurecli-interactive
sqlcmd -S <server_name>.database.windows.net -U <AADuser_name> -P "<AADpassword>" -G -l 30
```

В командной строке SQL запустите следующие команды, которые добавляют группу Azure Active Directory, созданную ранее в качестве пользователя.

```sql
CREATE USER [myAzureSQLDBAccessGroup] FROM EXTERNAL PROVIDER;
GO
```

Введите `EXIT`, чтобы вернуться в командную строку Cloud Shell. Затем запустите снова SQLCMD, но укажите имя базы данных вместо _\<dbname>_.

```azurecli-interactive
sqlcmd -S <server_name>.database.windows.net -d <db_name> -U <AADuser_name> -P "<AADpassword>" -G -l 30
```

В командной строке SQL нужной базы данных выполните следующие команды, чтобы предоставить разрешения на чтение и запись группе Azure Active Directory.

```sql
ALTER ROLE db_datareader ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_datawriter ADD MEMBER [myAzureSQLDBAccessGroup];
GO
```

## <a name="next-steps"></a>Дополнительная информация

Вы научились выполнять следующие задачи:

> [!div class="checklist"]
> * Включение управляемых удостоверений
> * Предоставление управляемому удостоверению доступа к Базе данных SQL
> * Настройка кода приложения для проверки подлинности с помощью базы данных SQL с использованием проверки подлинности Azure Active Directory.
> * Предоставление управляемому удостоверению минимальных привилегий в Базе данных SQL

Перейдите к следующему руководству, чтобы научиться сопоставлять пользовательские DNS-имена с веб-приложением.

> [!div class="nextstepaction"]
> [Сопоставление существующего настраиваемого DNS-имени с веб-приложениями Azure](app-service-web-tutorial-custom-domain.md)
