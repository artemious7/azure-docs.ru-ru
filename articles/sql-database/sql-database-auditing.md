---
title: Приступая к работе с аудитом баз данных SQL Azure | Документация Майкрософт
description: Используйте аудит базы данных Azure SQL для отслеживания событий базы данных в журнале аудита.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: ronitr
ms.author: ronitr
ms.reviewer: vanto
manager: craigg
ms.date: 02/10/2018
ms.openlocfilehash: 99a22c7bd535b8ca020e08004ebbf6423a734a0b
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816996"
---
# <a name="get-started-with-sql-database-auditing"></a>Приступая к работе с аудитом базы данных SQL
Аудит базы данных SQL Azure позволяет отслеживать события базы данных и записывать их в журнал аудита в учетной записи хранения Azure. Аудит также дает следующие возможности:

* Аудит может помочь вам соблюсти требования нормативов, проанализировать работу с базой данных и получить представление о расхождениях и аномалиях, которые могут указывать на бизнес-проблемы или предполагаемые нарушения безопасности.

* Средства аудита способствуют соблюдению стандартов соответствия, но не гарантируют их выполнение. Дополнительную информацию о программах Azure, поддерживающих проверку соблюдения стандартов, см. в [Центре управления безопасностью Azure](https://azure.microsoft.com/support/trust-center/compliance/).


## <a id="subheading-1"></a>Обзор аудита баз данных SQL Azure
Используйте аудит базы данных SQL, чтобы выполнять следующие действия:


* **Сохранить** журнал аудита выбранных событий. Вы можете указать, какие категории действий базы данных должны проходить аудит.
* **Создавать отчеты** по действиям, производимым с базой данных. Вы можете использовать предварительно настроенные отчеты и панель мониторинга для быстрого начала работы с отчетностью по действиям и событиям.
* **Анализировать** отчеты. Вы можете искать подозрительные события, необычную деятельность и тенденции.

Аудит можно настроить для различных типов категорий событий (см. раздел [Настройка аудита базы данных](#subheading-2)).

> [!IMPORTANT]
> Журналы аудита записываются в **добавочные большие двоичные объекты** в хранилище BLOB-объектов Azure в подписке Azure.
>
> * Поддержка добавочных больших двоичных объектов в **хранилище класса Premium** сейчас **не предоставляется**.
> * **Хранилище в виртуальной сети** в настоящее время **не поддерживается**.

## <a id="subheading-8"></a>Определение политики аудита уровня сервера и базы данных

Политику аудита можно определить для конкретной базы данных или как политику сервера по умолчанию.

* Политика сервера применяется ко всем существующим и новым базам данных на сервере.

* Если *включен аудит больших двоичных объектов для сервера*, он *обязательно применяется к базе данных*. Аудит базы данных будет выполнен независимо от его параметров.

* Включение аудита больших двоичных объектов для базы данных (кроме его включения для сервера), *не* приводит к переопределению или изменению настроек аудита больших двоичных объектов для сервера. Оба аудита будут выполняться параллельно. Таким образом, аудит базы данных выполняется дважды: один раз в соответствии с политикой сервера и еще раз в соответствии с политикой базы данных.

   > [!NOTE]
   > Не устанавливайте политику аудита больших двоичных объектов одновременно для сервера и базы данных, за исключением следующих случаев.
    > * Если для определенной базы данных нужно использовать другую *учетную запись хранения* или другой *срок хранения*.
    > * Нужно провести аудит типов событий или категорий для конкретной базы данных, которая отличается от остальных баз данных на сервере. Например, может потребоваться выполнить аудит вставки данных в таблицу только для конкретной базы данных.
   >
   > Во всех остальных случаях мы рекомендуем включать аудит больших двоичных объектов только на уровне сервера, а на уровне баз данных отключить все параметры аудита для всех баз данных.


## <a id="subheading-2"></a>Настройка аудита базы данных
В следующем разделе описывается настройка аудита на портале Azure.

1. Перейдите на [портал Azure](https://portal.azure.com).
2. Перейдите к параметру **Аудит** в разделе "Безопасность" на панели базы данных или сервера SQL.

    <a id="auditing-screenshot"></a> ![Область навигации][1]

3. Если необходимо настроить политику аудита сервера, выберите ссылку **Просмотреть параметры сервера** на странице аудита базы данных. Вы можете затем просмотреть или изменить параметры аудита сервера. Политики аудита сервера применяются ко всем существующим и создаваемым базам данных на этом сервере.

    ![Область навигации][2]

4. Если вам нужно включить аудит на уровне базы данных, установите для параметра **Аудит** значение **Вкл.**

    Если включен аудит сервера, настроенный аудит для базы данных будет существовать параллельно с ним.

    ![Область навигации][3]

5. **Новое.** Теперь есть несколько вариантов настройки расположения для записи журналов аудита. Вы можете записывать журналы в учетную запись хранения Azure, в рабочую область OMS для использования службой Log Analytics или в концентратор событий для потребления с использованием концентратора событий. Вы также можете настроить любое сочетание этих вариантов, и журналы аудита будут записываться в каждый из них.

    ![варианты хранилищ](./media/sql-database-auditing-get-started/auditing-select-destination.png)

6. Чтобы настроить запись журналов аудита в учетную запись хранения, выберите **Хранилище** и щелкните **Сведения о хранилище**. Выберите учетную запись хранения Azure, в которой будут храниться журналы, и укажите срок хранения. Старые журналы будут удалены. Нажмите кнопку **ОК**.

    ![запись хранения Azure](./media/sql-database-auditing-get-started/auditing_select_storage.png)

7. Чтобы настроить запись журналов аудита в рабочую область OMS, выберите **Log Analytics (предварительная версия)** и щелкните **Сведения о Log Analytics**. Выберите или создайте рабочую область OMS, в которую будут записываться журналы, а затем нажмите кнопку **ОК**.

    ![OMS](./media/sql-database-auditing-get-started/auditing_select_oms.png)

8. Чтобы настроить запись журналов аудита в концентратор событий, выберите **Концентратор событий (предварительная версия)** и щелкните **Сведения о концентраторе событий**. Выберите концентратор событий, в который необходимо записывать журналы, а затем нажмите кнопку **ОК**. Убедитесь, что концентратор событий находится в том же регионе, что база данных и сервер.

    ![концентратор событий;](./media/sql-database-auditing-get-started/auditing_select_event_hub.png)

9. Выберите команду **Сохранить**.
10. Настроить события аудита можно с помощью [командлетов PowerShell](#subheading-7) или [REST API](#subheading-9).
11. После настройки параметров аудита можно включить новую функцию обнаружения угроз и настроить адреса электронной почты для получения предупреждений системы безопасности. Использование функции обнаружения угроз позволяет настроить упреждающие оповещения об аномальной активности в базах данных, которая может указывать на потенциальные угрозы безопасности. Дополнительные сведения см. в статье [Обнаружение угроз для базы данных SQL](sql-database-threat-detection-get-started.md). 

## <a id="subheading-3"></a>Анализ журналов и отчетов аудита
Если журналы аудита записываются в Log Analytics:
- Используйте [портал Azure](https://portal.azure.com).  Откройте соответствующую базу данных. В верхней области страницы базы данных **Аудит** щелкните **Ознакомиться с журналами аудита**.

    ![просмотр журналов аудита](./media/sql-database-auditing-get-started/7_auditing_get_started_blob_view_audit_logs.png)

- Затем щелкните **Открыть в OMS** в верхней части страницы **Записи аудита**. Откроется представление журналов в Log Analytics, где можно настроить диапазон времени и поисковой запрос.

    ![Открыть в OMS](./media/sql-database-auditing-get-started/auditing_open_in_oms.png)

- Кроме того, к журналам аудита также можно перейти в колонке Log Analytics. Откройте рабочую область Log Analytics и в разделе **General** (Общие сведения) щелкните **Журналы**. Вы можете начать с простого запроса, например *выполните поиск SQLSecurityAuditEvents*, чтобы просмотреть журналы аудита.
    На этой странице можно также использовать [Log Analytics в Operations Management Suite (OMS)](../log-analytics/log-analytics-log-search.md), чтобы выполнить расширенный поиск данных журнала аудита. Log Analytics предоставляет аналитические данные по работе систем в режиме реального времени, используя встроенный поиск и настраиваемые панели мониторинга для быстрого анализа миллионов записей по всем рабочим нагрузкам и серверам. Дополнительную полезную информацию о языке поиска и командах Log Analytics OMS см. в статье [Анализ данных Log Analytics в Azure Monitor](../log-analytics/log-analytics-log-search.md).

Если журналы аудита записываются в концентратор событий:
- Чтобы работать с данными журналов аудита из концентратора событий, необходимо настроить потоковую передачу для получения событий и их записи в целевой объект. Дополнительные сведения см. в [документации по Центрам событий Azure](https://docs.microsoft.com/azure/event-hubs/).

Если журналы аудита записываются в учетную запись хранения Azure, их можно просматривать несколькими способами:
- Журналы аудита объединяются в учетной записи, выбранной на этапе настройки. Журналы аудита можно просматривать с помощью таких инструментов, как [обозреватель хранилищ Azure](http://storageexplorer.com/). В службе хранилища Azure журналы аудита сохраняются в виде коллекции файлов больших двоичных объектов в контейнере **sqldbauditlogs**. Дополнительные сведения об иерархии папки для хранения, соглашении об именовании и формате журнала см. в [документации по формату журнала аудита больших двоичных объектов](https://go.microsoft.com/fwlink/?linkid=829599).

- Используйте [портал Azure](https://portal.azure.com).  Откройте соответствующую базу данных. В верхней области страницы базы данных **Аудит** щелкните **Ознакомиться с журналами аудита**.

    ![Область навигации][7]

    Откроется колонка **Записи аудита**, в которой можно просматривать журналы.

    - Для просмотра записей за определенную дату щелкните **Фильтр** в верхней области страницы **Записи аудита**.
    - Можно переключаться между записями аудита, созданными *политикой аудита сервера* и *политикой аудита базы данных*, изменяя значение параметра **Источник аудита**.
    - Вы можете просматривать только записи аудита, связанные с внедрением кода SQL. Для этого установите флажок **Show only audit records for SQL injections** (Показывать только записи аудита для внедрения кода SQL).

       ![Область навигации][8]

- Используйте системную функцию **sys.fn_get_audit_file** (T-SQL), чтобы вернуть данные журнала аудита в табличном формате. Дополнительные сведения об использовании этой функции см. в статье [sys.fn_get_audit_file (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).


- Используйте **объединение файлов аудита** в SQL Server Management Studio (начиная с SSMS 17):
    1. В меню SSMS выберите **Файл** > **Открыть** > **Объединение файлов аудита**.

        ![Область навигации][9]
    2. Откроется диалоговое окно **Добавление файлов аудита**. Выберите один из способов **добавления**. Вы можете использовать объединение файлов аудита из локального диска или импортировать их из службы хранилища Azure. Необходимо предоставить сведения о службе хранилища Azure и ключ учетной записи.

    3. После добавления всех файлов для слияния нажмите кнопку **OK** для завершения операции слияния.

    4. Объединенный файл откроется в SSMS, где вы сможете его просмотреть и проанализировать, а также экспортировать в XEL или CSV-файл или в таблицу.

- Используйте Power BI. Вы можете просматривать и анализировать данные журнала аудита в Power BI. Чтобы получить дополнительные сведения и загрузить шаблон, перейдите к [статье об анализе данных журнала аудита в Power BI](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).
- Скачайте файлы журналов из контейнера больших двоичных объектов хранилища Azure из портала или с помощью [обозревателя хранилища Azure](http://storageexplorer.com/).
    * После загрузки файла журнала дважды щелкните по нему, чтобы открыть, просмотреть и проанализировать файл в среде SSMS.
    * Вы также можете загрузить одновременно несколько файлов через обозреватель хранилища Azure. Чтобы сделать это, щелкните правой кнопкой мыши конкретную вложенную папку и выберите **Сохранить как**, чтобы сохранить ее в локальной папке.

* Дополнительные методы
   * После загрузки нескольких файлов или вложенной папки, содержащей файлы журнала, можно объединить их локально, как описано выше в инструкциях по объединению файлов аудита в SSMS.
   * Чтобы просмотреть журналы аудита больших двоичных объектов программным способом:

     * Используйте библиотеку C# [для считывания расширенных событий](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/).
     * Используйте [запросы к файлам расширенных событий](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) с помощью PowerShell.

## <a id="subheading-5"></a>Рекомендации для рабочей среды
<!--The description in this section refers to preceding screen captures.-->

### <a id="subheading-6">Аудит геореплицированных баз данных</a>
При использовании геореплицированных баз данных, если включить аудит для базы данных-источника, к базе данных-получателю будут применяться идентичные политики аудита. Кроме того, можно настроить аудит базы данных-получателя, включив аудит для **сервера-получателя**, независимо от базы данных-источника.

* Аудит на уровне сервера (**рекомендуется**). Включите аудит на **сервере-источнике** и **сервере-получателе** — все базы данных-источники и базы данных-получатели пройдут независимый аудит на основе своей соответствующей политики на уровне сервера.

* Аудит на уровне базы данных. Аудит на уровне базы данных для баз данных-получателей можно настроить только с помощью параметров аудита в базе данных-источнике.
   * Его необходимо включить для самой *базы данных-источника*, а не для сервера.
   * После включения аудита в базе данных-источнике он станет доступным в базе данных-получателе.

    >[!IMPORTANT]
    >При использовании аудита на уровне базы данных настройки хранилища для базы данных-получателя будут совпадать с настройками для базы данных-источника, что приведет к появлению трафика между регионами. Рекомендуем включать аудит только на уровне сервера, а на уровне баз данных отключить все параметры аудита для всех баз данных.
<br>

### <a id="subheading-6">Повторное создание ключа к хранилищу данных</a>
Обычно в рабочей среде приходится периодически обновлять ключи хранилища. Если журналы аудита записываются в службу хранилища Azure, при обновлении ключей необходимо повторно сохранить политику аудита. Процесс таков:

1. Откройте раздел **Сведения о хранилище**. В поле **Ключ доступа к хранилищу** выберите **Вторичный**, а затем нажмите кнопку **OK**. Затем щелкните **Сохранить** в верхней области страницы настройки аудита.

    ![Область навигации][5]
2. Перейдите на страницу конфигурации хранилища и повторно создайте первичный ключ доступа.

    ![Область навигации][6]
3. Вернитесь на страницу настройки аудита, измените значение ключа доступа к хранилищу со вторичного на первичный, затем нажмите кнопку **ОК**. Затем щелкните **Сохранить** в верхней области страницы настройки аудита.
4. Вернитесь на страницу настройки хранилища и повторно создайте ключ доступа получателя (для подготовки к следующему циклу обновления ключей).

## <a name="additional-information"></a>Дополнительная информация

* Дополнительные сведения о формате журнала, иерархии папки для хранения, соглашении об именовании см. в [документации по формату журнала аудита больших двоичных объектов](https://go.microsoft.com/fwlink/?linkid=829599).

    > [!IMPORTANT]
    > Azure SQL Database Audit хранит 4000 символов данных для символьных полей в записи аудита. Если возвращаемые после выполнения проверяемого действия значения **statement** или **data_sensitivity_information** содержат более 4000 символов, все данные сверх этого числа **будут усечены и не будут проверены**.

* Журналы аудита записываются в **добавочные большие двоичные объекты** в хранилище BLOB-объектов Azure в подписке Azure.
    * Поддержка добавочных больших двоичных объектов в **хранилище класса Premium** сейчас **не предоставляется**.
    * **Хранилище в виртуальной сети** в настоящее время **не поддерживается**.

* Политика аудита по умолчанию включает в себя все действия и приведенные ниже группы действий, которые будут осуществлять аудит всех запросов и хранимых процедур, выполняемых с базой данных, а также успешных и неудачных попыток входа:

    BATCH_COMPLETED_GROUP;<br>
    SUCCESSFUL_DATABASE_AUTHENTICATION_GROUP;<br>
    FAILED_DATABASE_AUTHENTICATION_GROUP.

    Можно настроить аудит для различных типов действий и групп действий с помощью PowerShell, как описано в разделе [Управление аудитом базы данных SQL с помощью Azure PowerShell](#subheading-7).

## <a id="subheading-7"></a>Управление аудитом базы данных SQL с помощью Azure PowerShell

**Командлеты PowerShell**

* [Создание или обновление политики аудита больших двоичных объектов базы данных (Set-AzureRMSqlDatabaseAuditing)][105]
* [Создание или обновление политики аудита больших двоичных объектов сервера (Set-AzureRMSqlServerAuditing)][106]
* [Получение политики аудита баз данных (Get-AzureRMSqlDatabaseAuditing)][101]
* [Получение политики аудита больших двоичных объектов сервера (Get-AzureRMSqlDatabaseAuditing)][102]

Пример сценария см. в статье [Настройка аудита и обнаружения угроз для базы данных SQL с помощью PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a id="subheading-9"></a>Управление аудитом базы данных SQL с помощью REST API

**REST API — аудит больших двоичных объектов**:

* [Создание или обновление политики аудита BLOB-объектов базы данных](https://docs.microsoft.com/rest/api/sql/database%20auditing%20settings/createorupdate)
* [Создание или обновление политики аудита BLOB-объектов сервера](https://docs.microsoft.com/rest/api/sql/server%20auditing%20settings/createorupdate)
* [Получение политики аудита BLOB-объектов базы данных](https://docs.microsoft.com/rest/api/sql/database%20auditing%20settings/get)
* [Получение политики аудита BLOB-объектов сервера](https://docs.microsoft.com/rest/api/sql/server%20auditing%20settings/get)

Расширенная политика с поддержкой предложения WHERE для дополнительной фильтрации:
* [Создание или обновление *расширенной* политики аудита BLOB-объектов базы данных](https://docs.microsoft.com/rest/api/sql/database%20extended%20auditing%20settings/createorupdate)
* [Создание или обновление *расширенной* политики аудита BLOB-объектов сервера](https://docs.microsoft.com/rest/api/sql/server%20extended%20auditing%20settings/createorupdate)
* [Получение *расширенной* политики аудита BLOB-объектов базы данных](https://docs.microsoft.com/rest/api/sql/database%20extended%20auditing%20settings/get)
* [Получение *расширенной* политики аудита BLOB-объектов сервера](https://docs.microsoft.com/rest/api/sql/server%20extended%20auditing%20settings/get)

<!--Anchors-->
[Azure SQL Database Auditing overview]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Practices for usage in production]: #subheading-5
[Storage Key Regeneration]: #subheading-6
[Manage SQL database auditing using Azure PowerShell]: #subheading-7
[Blob/Table differences in Server auditing policy inheritance]: (#subheading-8)
[Manage SQL database auditing using REST API]: #subheading-9

<!--Image references-->
[1]: ./media/sql-database-auditing-get-started/1_auditing_get_started_settings.png
[2]: ./media/sql-database-auditing-get-started/2_auditing_get_started_server_inherit.png
[3]: ./media/sql-database-auditing-get-started/3_auditing_get_started_turn_on.png
[4]: ./media/sql-database-auditing-get-started/4_auditing_get_started_storage_details.png
[5]: ./media/sql-database-auditing-get-started/5_auditing_get_started_storage_key_regeneration.png
[6]: ./media/sql-database-auditing-get-started/6_auditing_get_started_regenerate_key.png
[7]: ./media/sql-database-auditing-get-started/7_auditing_get_started_blob_view_audit_logs.png
[8]: ./media/sql-database-auditing-get-started/8_auditing_get_started_blob_audit_records.png
[9]: ./media/sql-database-auditing-get-started/9_auditing_get_started_ssms_1.png
[10]: ./media/sql-database-auditing-get-started/10_auditing_get_started_ssms_2.png

[101]: /powershell/module/azurerm.sql/get-azurermsqldatabaseauditing
[102]: /powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditing
[103]: /powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing
[104]: /powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing
[105]: /powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditing
[106]: /powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditing
