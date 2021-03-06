---
title: Как настроить политики условного доступа Azure Active Directory для попыток доступа из ненадежных сетей | Документация Майкрософт
description: Узнайте, как настроить политики условного доступа Azure Active Directory(Azure AD) для попыток доступа из ненадежных сетей.
services: active-directory
keywords: условный доступ к приложениям, условный доступ посредством Azure Active Directory, безопасный доступ к ресурсам организации, политики условного доступа
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.component: conditional-access
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/23/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 5ddde65b2a68e71d86af6ce3dcd2847736cf5823
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627191"
---
# <a name="how-to-configure-conditional-access-policies-for-access-attempts-from-untrusted-networks"></a>Как настроить политики условного доступа для попыток доступа из ненадежных сетей   

В эпоху мобильных и облачных технологий Azure Active Directory (Azure AD) обеспечивает единый вход для устройств, приложений и служб из любого расположения. В результате пользователи могут получать доступ к облачным приложениям не только из сети вашей организации, но и из любого ненадежного расположения в Интернете. С помощью [условного доступа Azure Active Directory (Azure AD)](../active-directory-conditional-access-azure-portal.md) можно контролировать доступ авторизованных пользователей к облачным приложениям. Одним из общих требований в этом контексте является контроль попыток доступа, инициированных из ненадежных сетей. В этой статье предоставляются сведения, необходимые для настройки политики условного доступа, которая обрабатывает это требование. 

## <a name="prerequisites"></a>Предварительные требования

В данной статье предполагается, что вы знакомы с: 

- Основными понятиями условного доступа Azure AD. 
- Настройкой политики условного доступа на портале Azure.

См.:

- Общие сведения об условном доступе см. в статье [Условный доступ в Azure Active Directory](../active-directory-conditional-access-azure-portal.md) 

- Если вы хотите получить опыт настройки политик условного доступа, прочтите статью [Краткое руководство. Требование многофакторной идентификации (MFA) для конкретных приложений с помощью условного доступа Azure Active Directory](app-based-mfa.md). 


## <a name="scenario-description"></a>Описание сценария

Чтобы управлять балансом между безопасностью и производительностью, может быть достаточно требовать проверки подлинности пользователя с использованием пароля. Однако при попытке доступа из расположения ненадежной сети повышается риск входа незаконными пользователями. Чтобы устранить эту проблему, можно заблокировать попытки доступа из ненадежных сетей. В качестве альтернативы также можете задать требование многофакторной проверки подлинности (MFA), чтобы получить дополнительную уверенность в том, что попытка была сделана законным владельцем учетной записи. 

Благодаря условному доступу Azure AD можно выполнить это требование с помощью единой политики, которая предоставляет доступ: 

- к выбранным облачным приложениям;

- к выбранным пользователям и группам;  

- по требованию многофакторной проверки подлинности; 

- когда попытка доступа выполняется из: 

    - ненадежного расположения.


## <a name="considerations"></a>Рекомендации

Сложность этого сценария состоит в том, чтобы определить *попытку доступа из ненадежного расположения* и записать это как условие условного доступа. В политике условного доступа можно настроить условие [расположения](location-condition.md) для сценариев, связанных с сетевыми расположениями. Условие расположения позволяет выбрать именованные расположения, которые представляют собой логическое группирование диапазонов IP-адресов, стран и регионов.  

Как правило, вашей организации принадлежат один или несколько диапазонов адресов, например 199.30.16.0–199.30.16.24.
Чтобы настроить именованное расположение, сделайте следующее:

- Укажите этот диапазон (199.30.16.0/24). 

- Назначьте описательное имя, например **Корпоративная сеть**. 


Вместо того, чтобы пытаться определить ненадежные расположения, вы можете сделать следующее:

- Включить 

    ![Условный доступ](./media/untrusted-networks/02.png)

- Исключить все надежные расположения. 

    ![Условный доступ](./media/untrusted-networks/01.png)



## <a name="implementation"></a>Реализация

С помощью подхода, описанного в этой статье, теперь можно настроить политику условного доступа для ненадежных расположений. Всегда следует проверять политику прежде чем развернуть ее в рабочей среде, чтобы убедиться в том, что она работает правильно. В идеале по возможности первоначальные тесты нужно выполнять на тестовом клиенте. Дополнительные сведения см. в разделе [Как развертывать новую политику?](best-practices.md#how-should-you-deploy-a-new-policy). 



## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения см. в статье [Условный доступ в Azure Active Directory](../active-directory-conditional-access-azure-portal.md)