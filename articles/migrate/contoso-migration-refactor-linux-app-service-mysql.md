---
title: Рефакторинг приложения службы поддержки Linux для Службы приложений Azure и MySQL Azure в Contoso | Документация Майкрософт
description: Сведения о рефакторинге локального приложения Linux в Contoso путем его перемещения в Службу приложений Azure с использованием GitHub для перехода на веб-уровень и Базу данных SQL Azure.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: raynew
ms.openlocfilehash: 3f85d9d18aa49a378c63fa1f1692c7dc665489be
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/10/2018
ms.locfileid: "44296529"
---
# <a name="contoso-migration-refactor-a-contoso-linux-service-desk-app-to-multiple-regions-with-azure-app-service-traffic-manager-and-azure-mysql"></a>Миграция в компании Contoso: рефакторинг приложения службы поддержки Linux для использования нескольких регионов со Службой приложений Azure, диспетчером трафика и MySQL Azure в Contoso

В этой статье показано, как Contoso выполняет рефакторинг локального двухуровневого приложения для службы поддержки Linux (osTicket) путем переноса в Службу приложений Azure с интеграцией GitHub и MySQL Azure.

Это документ из цикла статей о том, как вымышленная компания Contoso переносит локальные ресурсы в облако Microsoft Azure. Этот цикл содержит общие сведения и сценарии, которые иллюстрируют настройку инфраструктуры миграции и выполнение миграций различных типов. Сценарии становятся все более сложными. Со временем мы добавим другие статьи.

**Статья** | **Дополнительные сведения** | **Состояние**
--- | --- | ---
[Статья 1. Общие сведения](contoso-migration-overview.md) | Обзор серии статей, стратегии миграции компании Contoso и используемых в этой серии примеров приложений. | Доступна
[Статья 2. Развертывание инфраструктуры миграции в Contoso](contoso-migration-infrastructure.md) | Компания Contoso готовит локальную инфраструктуру и инфраструктуру Azure для миграции. Для всех статей миграции в серии используется одна и та же инфраструктура. | Доступна
[Статья 3. Оценка готовности локальных ресурсов к переносу в Azure](contoso-migration-assessment.md)  | Компания Contoso выполняет оценку локального приложения SmartHotel360 в VMware. Для оценки виртуальных машин приложения Contoso использует службу "Миграция Azure". Для оценки базы данных SQL Server приложения используется Помощник по миграции данных. | Доступна
[Статья 4. Повторное размещение приложения на виртуальной машине Azure и в Управляемом экземпляре Базы данных SQL](contoso-migration-rehost-vm-sql-managed-instance.md) | Специалисты компании Contoso переносят локальное приложение SmartHotel360 в Azure по методу lift-and-shift. Специалисты компании Contoso переносят виртуальную машину внешнего интерфейса приложения с помощью [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Они переносят базу данных приложения в Управляемый экземпляр Базы данных SQL Azure с помощью [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Доступна   
[Статья 5. Повторное размещение приложения на виртуальных машинах Azure](contoso-migration-rehost-vm.md) | Специалисты компании Contoso переносят виртуальные машины приложения SmartHotel360 на виртуальные машины Azure с помощью службы Site Recovery. | Доступна
[Статья 6. Повторное размещение приложения на виртуальных машинах Azure и в группе доступности SQL Server AlwaysOn](contoso-migration-rehost-vm-sql-ag.md) | Компания Contoso переносит приложение SmartHotel360. Для переноса виртуальных машин приложения компания Contoso использует Site Recovery. Она использует службу Azure Database Migration Service для миграции базы данных приложения в кластер SQL Server, который защищен группой доступности AlwaysOn. | Доступна 
[Статья 7. Повторное размещение приложения Linux на виртуальных машинах Azure](contoso-migration-rehost-linux-vm.md) | Специалисты компании Contoso выполняют миграцию приложения osTicket для Linux по методу lift-and-shift на виртуальные машины Azure с помощью Azure Site Recovery. | Доступна
[Статья 8. Повторное размещение приложения Linux на виртуальных машинах Azure и в Azure MySQL](contoso-migration-rehost-linux-vm-mysql.md) | Специалисты компании Contoso переносят приложение osTicket для Linux на виртуальные машины Azure с помощью Azure Site Recovery, а также переносят базу данных приложения в экземпляр Azure MySQL Server, используя MySQL Workbench. | Доступна
[Статья 9. Рефакторинг приложения в веб-приложениях Azure и базе данных SQL Azure](contoso-migration-refactor-web-app-sql.md) | Специалисты компании Contoso переносят приложение SmartHotel360 в веб-приложение Azure, а базу данных приложения — в экземпляр SQL Server Azure с помощью Помощника по миграции баз данных. | Доступна
Статья 10. Рефакторинг приложения Linux в веб-приложениях Azure и Azure MySQL | Специалисты компании Contoso переносят свое приложение osTicket для Linux в веб-приложение Azure в нескольких регионах Azure с помощью диспетчера трафика Azure, интегрированного с GitHub для непрерывной поставки. Компания Contoso переносит базу данных приложения в экземпляр Базы данных Azure для MySQL. | Эта статья
[Статья 11. Рефакторинг Team Foundation Server в Azure DevOps Services](contoso-migration-tfs-vsts.md) | Специалисты компании Contoso переносят локальное развертывание Team Foundation Server в Azure DevOps Services в Azure. | Доступна
[Статья 12. Перепроектирование приложения для использования контейнеров Azure и Базы данных SQL Azure](contoso-migration-rearchitect-container-sql.md) | Специалисты компании Contoso переносят приложение SmartHotel в Azure. Затем уровень веб-приложений преобразуется в контейнер Windows, работающий в Azure Service Fabric, и базу данных в службе "База данных SQL Azure". | Доступна
[Статья 13. Повторное создание приложения в Azure](contoso-migration-rebuild.md) | Специалисты компании Contoso выполняют повторную сборку приложения SmartHotel360, используя ряд возможностей и служб Azure, включая Службу приложений Azure, Службу Azure Kubernetes (AKS), Функции Azure, Cognitive Services и Azure Cosmos DB. | Доступна

В этой статье описывается, как Contoso переносит в Azure свое двухуровневое приложение службы поддержки osTicket на базе комплекса Linux, Apache, MySQL, PHP (LAMP). Если вы хотите использовать это приложение с открытым исходным кодом, вы можете загрузить его с [GitHub](https://github.com/osTicket/osTicket).


## <a name="business-drivers"></a>Бизнес-факторы

Совместными усилиями ИТ-руководство и деловые партнеры определили следующие цели своей деятельности:

- **Адаптация к расширению бизнеса.** Компания Contoso растет и переходит на новые рынки. Требуются дополнительные агенты обслуживания клиентов. 
- **Масштаб.** Решение должно создаваться таким образом, чтобы компания Contoso имела возможность добавлять дополнительные агенты обслуживания пользователей по мере расширения бизнеса.
- **Повышение устойчивости.** Предыдущие проблемы с системой повлияли только на внутренних пользователей. В новой бизнес-модели эти проблемы будут влиять на внешних пользователей, и компании Contoso потребуется, чтобы приложение работало постоянно.

## <a name="migration-goals"></a>Цели миграции

Для определения наилучшего способа миграции команда Contoso по работе в облаке определила ее цели.

- Приложения должны масштабироваться за пределы текущих локальных ресурсов и производительности.  Компания Contoso переносит приложение, чтобы воспользоваться преимуществом масштабирования по требованию в Azure.
- Им необходимо переместить базу кода приложения в конвейер непрерывной поставки.  Так как изменения приложения передаются в GitHub, компании Contoso необходимо, чтобы эти изменения развертывались без задействования персонала.
- Приложение должно быть устойчивым к возможным расширению и отработке отказа. Компании Contoso необходимо развернуть приложение в двух регионах Azure и настроить его таким образом, чтобы оно масштабировалось автоматически.
- Contoso необходимо свести к минимуму задачи администрирования баз данных после переноса приложения в облако.

## <a name="solution-design"></a>Архитектура решения
Определив свои цели и требования, специалисты Contoso начинают разработку и анализ решения развертывания и идентифицируют процесс миграции, включая службы Azure, которые будут использоваться при этой миграции.


## <a name="current-architecture"></a>Текущая архитектура

- У приложения есть два уровня, которые размещаются на двух виртуальных машинах (OSTICKETWEB и OSTICKETMYSQL).
- Виртуальные машины расположены на узле VMware ESXi **contosohost1.contoso.com** (версии 6.5).
- Средой VMware управляет сервер vCenter Server 6.5 (**vcenter.contoso.com**), который запущен на виртуальной машине.
- Contoso использует локальный центр обработки данных (contoso-datacenter) с локальным контроллером домена (**contosodc1**).

![Текущая архитектура](./media/contoso-migration-refactor-linux-app-service-mysql/current-architecture.png) 


## <a name="proposed-architecture"></a>Предлагаемая архитектура

Ниже приведена предлагаемая архитектура.

- Приложение веб-уровня на OSTICKETWEB будет перенесено путем создания Службы приложений Azure в двух регионах Azure. Служба приложений Azure для Linux будет работать с использованием контейнера Docker PHP 7.0.
- Код приложения будет перемещен на портал GitHub, а веб-приложение Azure — настроено для непрерывной поставки с помощью GitHub.
- Серверы приложения Azure будут развернуты как в основном (восточная часть США 2), так и в дополнительном регионе (центральная часть США).
- Диспетчер трафика будет настроен для двух веб-приложений Azure в обоих регионах.
- Диспетчер трафика будет настроен в режиме приоритета для принудительного перенаправления трафика через регион "Восточная часть США 2".
- Если сервер приложений Azure в восточной части США 2 переходит в автономный режим, пользователи могут использовать версию приложения после отработки отказа в регионе "Центральная часть США".
- База данных приложения будет перенесена в службу PaaS MySQL для Azure путем использования средств MySQL Workbench. Будет создана резервная копия локальной базы данных. Затем будет выполнено ее восстановление в MySQL для Azure.
- База данных будет размещена в основном регионе "Восточная часть США 2" в подсети (PROD-DB-EUS2) производственной сети (VNET-PROD-EUS2).
- Так как миграция выполняется для рабочей нагрузки в рабочей среде, ресурсы Azure для приложения будут находиться в рабочей группе ресурсов **ContosoRG**.
- Ресурс диспетчера трафика будет развертываться в группе ресурсов инфраструктуры компании Contoso **ContosoInfraRG**.
- После завершения миграции локальные виртуальные машины в центре обработки данных Contoso будут выведены из эксплуатации.


![Архитектура сценария](./media/contoso-migration-refactor-linux-app-service-mysql/proposed-architecture.png) 


## <a name="migration-process"></a>Процесс миграции

Порядок миграции в Contoso будет следующим:

1. В качестве первого шага администраторы Contoso настраивают инфраструктуру Azure, включая подготовку Службы приложений Azure, настройку диспетчера трафика и подготовку экземпляра MySQL для Azure.
2. После подготовки Azure компания переносит базу данных с помощью MySQL Workbench. 
3. После запуска базы данных в Azure компания настраивает частный репозиторий GitHub для Службы приложений Azure с непрерывной поставкой и загружает его с помощью приложения osTicket.
4. Затем с портала Azure они загружают приложение из GitHub в контейнер Docker под управлением Службы приложений Azure. 
5. Компания настроила параметры DNS и автоматическое масштабирование приложения.

![Процесс миграции](./media/contoso-migration-refactor-linux-app-service-mysql/migration-process.png) 


### <a name="azure-services"></a>Службы Azure

**Служба** | **Описание** | **Стоимость**
--- | --- | ---
[службе приложений Azure](https://azure.microsoft.com/services/app-service/) | Служба работает и масштабирует приложения, используя Azure PaaS для веб-сайтов.  | Ценовая политика основывается на размере необходимых экземпляров и компонентов. [Узнайте больше](https://azure.microsoft.com/pricing/details/app-service/windows/).
[Диспетчер трафика](https://azure.microsoft.com/services/traffic-manager/) | Подсистема балансировки нагрузки, использующая DNS для перенаправления пользователей в Azure или на внешние веб-сайты и службы. | Ценообразование основано на количестве получаемых запросов DNS и отслеживаемых конечных точек. | [Узнайте больше](https://azure.microsoft.com/pricing/details/traffic-manager/).
[База данных Azure для MySQL](https://docs.microsoft.com/azure/mysql/) | База данных расположена на ядре сервера MySQL с открытым исходным кодом. Она предоставляет разработанную сообществом, полностью управляемую и готовую к использованию на предприятии базу данных MySQL как услугу для разработки и развертывания приложений. | Ценообразование основано на вычислении, хранении и резервном копировании. [Узнайте больше](https://azure.microsoft.com/pricing/details/mysql/).

 
## <a name="prerequisites"></a>Предварительные требования

Ниже показано, что необходимо сделать специалистам компании Contoso, чтобы реализовать этот сценарий.

**Требования** | **Дополнительные сведения**
--- | ---
**Подписка Azure.** | Специалисты Contoso создали подписки ранее в этой серии статей. Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись Azure](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Если вы создаете бесплатную учетную запись, вы являетесь администратором своей подписки и можете выполнять любые действия.<br/><br/> Если вы используете существующую подписку, в которой не являетесь администратором, администратор должен назначить вам права владельца или участника. 
**Инфраструктура Azure** | Contoso настраивает свою инфраструктуру Azure, как описано в статье [Развертывание инфраструктуры Azure для миграции в Contoso](contoso-migration-infrastructure.md).



## <a name="scenario-steps"></a>Шаги выполнения сценария

Миграция в Contoso будет осуществляться следующим образом:

> [!div class="checklist"]
> * **Шаг 1. Подготовка Службы приложений Azure**. Администраторы компании Contoso подготовят веб-приложения в основном и дополнительном регионах.
> * **Шаг 2. Настройка диспетчера трафика**. Компания настраивает диспетчер трафика перед веб-приложениями с целью маршрутизации и балансировки нагрузки трафика.
> * **Шаг 3. Подготовка MySQL**. Специалисты компании подготавливают в Azure экземпляр базы данных MySQL для Azure.
> * **Шаг 4. Миграция базы данных**. Компания переносит базу данных с помощью MySQL Workbench. 
> * **Шаг 5. Настройка GitHub**. Специалисты компании настраивают локальный репозиторий GitHub для веб-сайтов и кода приложения.
> * **Шаг 6. Развертывание веб-приложений**. Они развертывают веб-приложения из GitHub.




## <a name="step-1-provision-azure-app-services"></a>Шаг 1. Подготовка Службы приложений Azure

Администраторы Contoso подготавливают два веб-приложения (по одному в каждом регионе) с помощью Службы приложений Azure.

1. Компания создает ресурс веб-приложения в основном регионе "Восточная часть США 2" (**osticket-eus2**) из Azure Marketplace.
2. Компания добавляет ресурс в рабочую группу ресурсов **ContosoRG**.

    ![Приложение Azure](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app1.png) 

3. Специалисты компании создают план службы приложений в основном регионе (**APP-SVP-EUS2**), применяя стандартный размер.

     ![Приложение Azure](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app2.png) 
    
4. Специалисты компании выбирают операционную систему Linux со стеком времени выполнения PHP 7.0, который представляет собой контейнер Docker.

    ![Приложение Azure](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app3.png) 

5. Специалисты компании создают второе веб-приложение (**osticket cus**) и план службы приложений для региона "Центральная часть США".

    ![Приложение Azure](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app4.png) 


**Нужна дополнительная помощь?**

- Узнайте больше о [веб-приложениях Службы приложений Azure](https://docs.microsoft.com/azure/app-service/app-service-web-overview).
- Узнайте больше о [Службе приложений Azure на платформе Linux](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro).


## <a name="step-2-set-up-traffic-manager"></a>Шаг 2. Настройка диспетчера трафика

Администраторы Contoso настраивают диспетчер трафика для направления входящих веб-запросов в веб-приложения веб-уровня osTicket.

1. Они создают ресурс диспетчера трафика (**osticket.trafficmanager.net**) в Azure Marketplace. Они применяют приоритетную маршрутизацию, чтобы регион "Восточная часть США 2" был основным сайтом. Специалисты компании размещают ресурс в группе ресурсов инфраструктуры (**ContosoInfraRG**). Обратите внимание, что диспетчер трафика является глобальным и не привязан к определенному месту.

    ![Диспетчер трафика](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager1.png) 

2. Теперь специалисты компании настраивают диспетчер трафика с конечными точками. Специалисты компании добавляют веб-приложение в регионе "Восточная часть США 2" как основной сайт (**osticket eus2**), а приложение в регионе "Центральная часть США" — как дополнительный (**osticket cus**).

    ![Диспетчер трафика](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager2.png) 

3. После добавления конечных точек они могут контролировать их.

    ![Диспетчер трафика](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager3.png)

**Нужна дополнительная помощь?**

- Подробнее о [диспетчере трафика](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview).
- Узнайте больше о [перенаправлении трафика к приоритетной конечной точке](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-priority-routing-method).
 
## <a name="step-3-provision-azure-database-for-mysql"></a>Шаг 3. Подготовка Базы данных Azure для MySQL

Администраторы компании Contoso подготавливают экземпляр базы данных MySQL в основном регионе "Восточная часть США 2".

1. Специалисты компании создают Базу данных Azure для ресурса MySQL на портале Azure. 

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-1.png)

2. Они добавляют имя **contosoosticket** для базы данных Azure. Саму базу данных они добавляют в рабочую группу ресурсов **ContosoRG** и указывают для нее учетные данные.
3. В локальной среде используется база данных MySQL 5.7, поэтому для обеспечения совместимости выбрана именно эта версия. Для размера они выбирают значение по умолчанию, соответствующее требованиям компании для базы данных.

     ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-2.png)

4. В разделе **Варианты резервирования для архивации** специалисты выбирают **Геоизбыточное**. Этот вариант позволяет восстановить базу данных в дополнительном регионе "Центральная часть США" в случае сбоя. Данный параметр можно настроить только при подготовке базы данных.

    ![Избыточность](./media/contoso-migration-refactor-linux-app-service-mysql/db-redundancy.png)

4. Специалисты настраивают безопасность подключения. В разделе **Безопасность подключения** для базы данных специалисты компании настраивают правила брандмауэра для предоставления базе данных доступа к службам Azure.
5. Они добавляют IP-адрес клиента локальной рабочей станции в начальный и конечный IP-адреса. Это позволяет веб-приложениям получать доступ к базе данных MySQL, а также к ее клиенту, который выполняет миграцию.

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-3.png)



## <a name="step-4-migrate-the-database"></a>Шаг 4. Миграция базы данных

Администраторы компании Contoso выполняют миграцию базы данных с помощью функции резервного копирования и восстановления в инструментах MySQL. Специалисты установят MySQL Workbench, создадут резервную копию базы данных из OSTICKETMYSQL, а затем восстановят ее в Базе данных Azure для сервера MySQL.

### <a name="install-mysql-workbench"></a>Установка MySQL Workbench

1. Специалисты компании проверяют [предварительные условия и загружают MySQL Workbench](https://dev.mysql.com/downloads/workbench/?utm_source=tuicool).
2. Специалисты компании устанавливают MySQL Workbench для Windows в соответствии с [инструкциями по установке](https://dev.mysql.com/doc/workbench/en/wb-installing.html). Компьютер, на котором они устанавливаются, должен быть доступен для виртуальной машины OSTICKETMYSQL и Azure через Интернет.
3. Они создают в среде MySQL Workbench подключение MySQL к OSTICKETMYSQL. 

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench1.png)

4. Они экспортируют базу данных в виде **osticket** в автономный файл.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench2.png)

5. После локальной архивации базы данных они создают подключения к Базе данных Azure для экземпляра MySQL.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench3.png)

6. Теперь специалисты компании могут импортировать (восстановить) базу данных в экземпляре Azure MySQL из автономного файла. Для экземпляра создается новая схема (osticket).

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench4.png)

7. После восстановления данных их можно получить, сделав запрос с помощью Workbench, и просмотреть на портале Azure.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench5.png)

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench6.png)

8. Наконец, специалистам необходимо обновить сведения о базе данных в веб-приложениях. В экземпляре MySQL специалисты открывают **Строки подключения**. 

     ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench7.png)

9. В списке строк они находят и щелкают параметры веб-приложения, чтобы скопировать их.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench8.png)

10. Специалисты компании открывают Блокнот, вставляют строку в новый файл и изменяют его в соответствии с базой данных osticket, экземпляром MySQL и параметрами учетных данных.

     ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench9.png)

11. Специалисты компании могут проверить имя сервера и имя для входа в разделе **Обзор** в экземпляре MySQL на портале Azure.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench10.png)


## <a name="step-5-set-up-github"></a>Шаг 5. Настройка GitHub

Администраторы компании Contoso создают частный репозиторий GitHub и настраивают подключение к базе данных osTicket в MySQL для Azure. После этого они загружают веб-приложение Azure в приложение.  

1.  Специалисты используют общедоступный репозиторий GitHub программного обеспечения osTicket и создают ветвь для учетной записи Contoso GitHub.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github1.png)

2. После этого они открывают папку **include** и находят файл **ost-config.php**.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github2.png)


3. Специалисты компании изменяют этот файл, когда он открывается в браузере.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github3.png)

4. В текстовом редакторе специалисты обновляют сведения о базе данных, в частности **DBHOST** и **DBUSER**. 

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github4.png)

5. Затем они применяют изменения.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github5.png)

6. Для каждого веб-приложения (**osticket-eus2** и **osticket-cus**) специалисты изменяют **параметры приложения** на портале Azure.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github6.png)

7. Специалисты вводят строку подключения с именем **osticket** и копируют строку из блокнота в **область значения**. Они выбирают **MySQL** в раскрывающемся списке рядом со строкой и сохраняют параметры.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github7.png)

## <a name="step-6-configure-the-web-apps"></a>Шаг 6. Настройка веб-приложений

В качестве заключительного этапа процесса миграции администраторы компании Contoso настраивают веб-приложения и веб-сайты osTicket.



1. В основном веб-приложении (**osticket-eus2**) они открывают раздел **Вариант развертывания** и устанавливают источник **GitHub**.

    ![Настройка приложения](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app1.png)

2. Специалисты компании выбирают варианты развертывания.

    ![Настройка приложения](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app2.png)

3. После настройки параметров конфигурация имеет состояние ожидания на портале Azure.

    ![Настройка приложения](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app3.png)

4. После обновления конфигурации веб-приложение osTicket загружается из GitHub в контейнер Docker под управлением Службы приложений Azure, а сайт отображается как активный.

    ![Настройка приложения](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app4.png)

5. Затем специалисты повторяют описанные выше действия для дополнительного веб-приложения (**osticket-cus**).
6. После настройки сайт становится доступным через профиль диспетчера трафика. DNS-имя — новое расположение приложения osTicket. [Узнайте больше](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain#map-a-cname-record).

    ![Настройка приложения](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app5.png)
    
7. Компании Contoso необходимо DNS-имя, которое легко запомнить. Специалисты компании создают запись псевдонима (CNAME) **osticket.contoso.com**, указывающую на имя диспетчера трафика в DNS контроллеров домена.

    ![Настройка приложения](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app6.png)

8. Они настраивают оба веб-приложения **osticket-eus2** и **osticket-cus**, чтобы разрешить пользовательские имена узлов.

    ![Настройка приложения](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app7.png)

### <a name="set-up-autoscaling"></a>Настройка автомасштабирования

Наконец, специалисты настраивают автомасштабирование приложения. Это гарантирует, что количество экземпляров приложения будет соответствовать потребностям бизнеса. 

1. В службе приложений **APP-SRV-EUS2** специалисты открывают раздел **единицы масштабирования**.
2. Они настраивают новый параметр автомасштабирования с одним правилом, увеличивающим количество экземпляров на один, когда процент использования ЦП для текущего экземпляра превышает 70 % в течение 10 минут.

    ![Autoscale](./media/contoso-migration-refactor-linux-app-service-mysql/autoscale1.png)

3. Они настраивают тот же параметр в **APP-SRV-CUS**, чтобы убедиться, что применяется такое же поведение, если приложение выполняет отработку отказа в дополнительный регион. Единственная разница в том, что они ограничивают количество экземпляров до 1, так как это необходимо только для отработки отказа.

   ![Autoscale](./media/contoso-migration-refactor-linux-app-service-mysql/autoscale2.png)

##  <a name="clean-up-after-migration"></a>Очистка после миграции

После завершения миграции приложение osTicket рефакторизовано для работы в веб-приложении Azure с непрерывной поставкой, реализуемой с помощью частного репозитория GitHub. Приложение работает в двух регионах для повышения устойчивости. База данных osTicket работает в базе данных Azure для MySQL после перехода на платформу PaaS.

Для очистки Contoso потребуется предпринять следующее: 
- Удалить виртуальную машину VMware из перечня в vCenter.
- Удалить локальные виртуальные машины из локальных заданий резервного копирования.
- Обновить внутренние документы и указать в них новые расположения и IP-адреса. 
- Проверить все ресурсы, взаимодействующие с локальными виртуальными машинами, и обновить все соответствующие параметры или документы согласно новой конфигурации.
- Перенастроить мониторинг, чтобы указать на URL-адрес osticket-trafficmanager.net для отслеживания работы приложения.

## <a name="review-the-deployment"></a>Проверка развертывания

Теперь, когда приложение работает, компании Contoso необходимо полностью ввести его в эксплуатацию и защитить свою новую инфраструктуру.

### <a name="security"></a>Безопасность

Специалисты по безопасности компании Contoso проверили приложение, чтобы выявить любые проблемы безопасности. Они определили, что обмен данными между приложением osTicket и экземпляром базы данных MySQL не настроен для использования протокола SSL. Поэтому им необходимо будет сделать это, чтобы предотвратить взлом трафика, базы данных. [Узнайте больше](https://docs.microsoft.com/azure/mysql/howto-configure-ssl).

### <a name="backups"></a>Резервные копии

- Веб-приложения osTicket не содержат данные о состоянии, и поэтому для них не нужно создавать резервные копии.
- Ее специалистам нет необходимости настраивать резервное копирование для базы данных. В службе "База данных Azure для MySQL" для сервера автоматически создаются резервные копии. Они решили использовать для базы данных геоизбыточное хранилище, поэтому она отказоустойчива и готова к внедрению в рабочую среду. Резервные копии можно использовать для восстановления сервера до точки во времени. [Узнайте больше](https://docs.microsoft.com/azure/mysql/concepts-backup).


### <a name="licensing-and-cost-optimization"></a>Лицензирование и оптимизация затрат

- Проблемы с лицензированием развертывания PaaS отсутствуют.
- Компания будет использовать Управление затратами Azure по лицензии Cloudyn (дочернее подразделение корпорации Майкрософт). Это решение по управлению затратами для нескольких облаков позволяет использовать Azure и другие облачные ресурсы, а также управлять ими.  Дополнительные сведения об управлении расходами см. [на этой странице](https://docs.microsoft.com/azure/cost-management/overview).



