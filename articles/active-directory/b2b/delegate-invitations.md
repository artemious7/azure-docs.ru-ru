---
title: Делегирование приглашений для службы совместной работы Azure Active Directory B2B | Документация Майкрософт
description: Свойства пользователя службы совместной работы Azure Active Directory B2B можно настраивать.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 6389a4987c590cd2d0f1dc648f9d003581102265
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984781"
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a>Делегирование приглашений для службы совместной работы Azure Active Directory B2B

Благодаря службе совместной работы Azure Active Directory (Azure AD) B2B больше не нужно быть глобальным администратором, чтобы приглашать пользователей. Вместо этого можно использовать политики и делегировать приглашения пользователям, роли которых дают возможность отправлять приглашения. Вам доступен важный новый способ делегирования приглашений гостевым пользователям — с помощью роли Guest Inviter.

## <a name="guest-inviter-role"></a>Роль Guest Inviter
Можно назначить пользователю роль Guest Inviter для отправки приглашений. Для отправки приглашений не нужно быть участником роли глобального администратора. По умолчанию обычные пользователи могут также вызывать API приглашения, если только глобальный администратор не отключил для них приглашения. Кроме того, пользователь может вызвать API с помощью портала Azure или PowerShell.

Ниже приведен пример, показывающий, как использовать PowerShell, чтобы добавить пользователя в роль Guest Inviter:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a>Управление пользователями, имеющими разрешение на приглашение

![Настройка пользователей с разрешением на приглашение](media/delegate-invitations/control-who-to-invite.png)

Используя службу совместной работы Azure AD B2B, администратор клиентов может устанавливать следующие политики приглашения:

- отключение приглашений;
- приглашать могут только администраторы и пользователи с ролью Guest Inviter;
- приглашать могут администраторы и участники роли Guest Inviter;
- приглашать могут все пользователи, включая гостей.

По умолчанию для клиентов настраивается вариант 4 (приглашать пользователей B2B могут все пользователи, включая гостей).

## <a name="next-steps"></a>Дополнительная информация

Другие статьи о службе совместной работы Azure AD B2B перечислены ниже.

- [Что такое служба совместной работы Azure AD B2B?](what-is-b2b.md)
- [Add B2B collaboration guest users without an invitation](add-user-without-invite.md) (Добавление гостевых пользователей службы совместной работы B2B без приглашения)
- [Добавление пользователя службы совместной работы Azure Active Directory B2B в роль](add-guest-to-role.md)


