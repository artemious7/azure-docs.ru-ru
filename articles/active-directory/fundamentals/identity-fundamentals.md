---
title: Каковы основные принципы управления удостоверениями и доступом в Azure? Azure Active Directory | Документы Майкрософт
description: Узнайте о возможностях дополнительной защиты и дополнительных инструментах, которые доступны в Premium-выпусках Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.author: lizross
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/13/2018
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: f7baa29c77ae4af9813bfc755a39cc07288a3ad2
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/17/2018
ms.locfileid: "45734681"
---
# <a name="what-are-the-fundamentals-of-azure-identity-and-access-management"></a>Каковы основные принципы управления удостоверениями и доступом в Azure?
Azure AD Premium — это облачное решение по управлению удостоверениями и доступом с расширенными возможностями защиты. Эти расширенные возможности помогают обеспечить защищенное удостоверение для всех приложений, защиту удостоверений (расширенную с помощью [Microsoft Intelligent Security Graph](https://www.microsoft.com/security/intelligence)) и [управление привилегированными пользователями (PIM)](../privileged-identity-management/pim-configure.md). Azure AD помогает защитить удостоверения пользователей в режиме реального времени, помогая создавать учитывающие риски адаптивные политики доступа к данным вашей организации.

Ниже представлено короткое видео с краткими сведениями о защите удостоверений и управлении ими в Azure AD.
>[!VIDEO https://www.youtube.com/embed/9LGIJ2-FKIM]

Azure AD также предоставляет набор средств, которые помогут вам защищать, автоматизировать и управлять средой, включая сброс паролей, управление пользователями и группами и запросов на приложения. Azure AD также позволяет управлять пользовательскими устройствами, доступом и контролировать приложения SaaS (программное обеспечение как услуга).

Дополнительные сведения о стоимости Premium-выпусков Azure Active Directory и связанных инструментов см. в разделе [Цены на Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="connect-on-premises-active-directory-with-azure-ad-and-office-365"></a>Подключение локальной службы Active Directory к Azure AD и Office 365
Расширьте вашу локальную реализацию Active Directory в облако с помощью интеграции локальных каталогов с Azure AD, используя [гибридные удостоверения](https://aka.ms/aadframework). [Azure AD Connect](../connect/active-directory-aadconnect.md) обеспечивает возможность такой интеграции, предоставляя пользователям единое удостоверение и единый вход (SSO) для доступа как к локальным ресурсам, так и к облачным службам, таким как Office 365.

Azure AD Connect заменяет собой старые версии средств для интеграции удостоверений, такие как DirSync и Azure AD Sync, предоставляя возможности для синхронизации удостоверений между локальными ресурсами и Azure AD. Синхронизация Azure AD Connect обеспечивается с помощью следующих компонентов:

- **Синхронизация.** Отвечает за создание пользователей, групп и других объектов. Она также отвечает за согласованность сведений о пользователях в локальной среде и в Azure AD. Включение обратной записи паролей также помогает синхронизировать локальные каталоги, когда пользователи изменяют пароли в Azure AD.

- аутентификация; При настройке решения для гибридной идентификации Azure AD важно выбрать правильный метод аутентификации. Для вашей организации вы можете выбрать облачную аутентификацию (синхронизация хэша паролей или сквозная аутентификация) или федеративную аутентификацию (AD FS). Дополнительные сведения о доступных вариантах см. в разделе [Выбор правильного метода аутентификации для гибридного решения для идентификации Azure Active Directory](https://aka.ms/auth-options).

- **Мониторинг работоспособности.** Компонент Azure AD Connect Health предоставляет мониторинг и центральное расположение на портале Azure для просмотра связанных действий. Дополнительные сведения см. в статье [Мониторинг локальной инфраструктуры идентификации и служб синхронизации в облаке](../connect-health/active-directory-aadconnect-health.md).

## <a name="increase-productivity-and-reduce-helpdesk-costs-with-self-service-and-single-sign-on-experiences"></a>Повышение производительности и снижение затрат на службу поддержки за счет самообслуживания и единого входа
Пользователи экономят свое время, когда они имеют одно имя пользователя и пароль, а также получают согласованную работу на всех устройствах. Кроме того, они экономят время, выполняя задачи самообслуживания, такие как [сброс забытого пароля](../user-help/active-directory-passwords-update-your-own-password.md) или запрос на доступ к приложению, без ожидания помощи от службы поддержки.

Совершенствуя возможности единого входа и согласованной работы, Azure AD [расширяет локальную службу Active Directory](../connect/active-directory-aadconnect.md) в облако. Теперь пользователи могут использовать основную рабочую учетную запись на присоединенных к домену устройствах, на корпоративных ресурсах и в любых веб-приложениях и приложениях SaaS, которые могут понадобиться для работы. 

Кроме того, доступ к приложению может быть автоматически подготовлен (или отменен) на основе членства в группах и статуса сотрудника, помогая управлять доступом к приложениям из коллекции или собственным локальным приложениям, разработанным и опубликованным через [Azure AD Application Proxy](../manage-apps/application-proxy.md).

## <a name="manage-and-control-access-to-your-organizational-resources"></a>Управление и контроль доступа к ресурсам организации
Решения по управлению идентификацией и доступом корпорации Майкрософт помогают вам защитить доступ к приложениям и ресурсам в центре обработки данных вашей организации и в облаке. Такое управление доступом предоставляет дополнительные уровни проверки, такие как [многофакторная идентификация](../authentication/concept-mfa-howitworks.md) и [политики условного доступа](../conditional-access/overview.md). Наблюдение за подозрительной активностью с использованием расширенных отчетов, аудита и предупреждений может также помочь снизить потенциальные риски безопасности.

Использование политик условного доступа в Azure AD Premium позволяют вам создавать правила доступа на основе политик для любых приложений, подключенных к Azure AD, таких как приложения SaaS, облачные и локальные пользовательские приложения или веб-приложения. Azure AD оценивает ваши правила в режиме реального времени и применяет их каждый раз, когда пользователь пытается получить доступ к приложению. Политики защиты удостоверений Azure позволяют автоматически принимать меры (с помощью блокировки доступа, принудительного применения многофакторной проверки подлинности или сброса паролей пользователей) при обнаружении подозрительной активности.

## <a name="azure-active-directory-privileged-identity-management"></a>Управление привилегированными удостоверениями в Azure Active Directory
Возможность [управления привилегированными пользователями (PIM)](../privileged-identity-management/pim-getting-started.md), включенная в Azure Active Directory выпуск Premium 2, позволяет обнаруживать, ограничивать и отслеживать учетные записи администратора и доступ этих учетных записей к ресурсам в Azure Active Directory и других веб-службах корпорации Майкрософт. PIM также помогает управлять административным доступом по запросу в течение точно определенного времени. То есть администраторы могут запрашивать временное повышение привилегий, получаемое с помощью многофакторной проверки подлинности, на определенный предварительно заданный период времени, по истечении которого учетные записи будут возвращены к обычному состоянию пользователя.

## <a name="next-steps"></a>Дополнительная информация
Дополнительные сведения об архитектуре Azure AD см. в разделе [Что такое архитектура Azure AD?](active-directory-architecture.md).
