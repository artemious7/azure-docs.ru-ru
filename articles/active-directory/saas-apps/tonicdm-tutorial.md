---
title: Руководство по интеграции Azure Active Directory с TonicDM | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и TonicDM.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 19ea9a07-9ecf-43dc-91ba-593ce3c00b01
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: jeedes
ms.openlocfilehash: b582af22707b7bff187ed93fb48ba96d15634ab3
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39433330"
---
# <a name="tutorial-azure-active-directory-integration-with-tonicdm"></a>Руководство по интеграции Azure Active Directory с TonicDM

В этом руководстве описано, как интегрировать TonicDM с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением TonicDM обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к TonicDM.
- Вы можете включить автоматический вход пользователей в TonicDM (единый вход) с использованием их учетных записей Azure AD.
- Вы можете управлять учетными записями централизованно — на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с TonicDM, вам потребуется следующее:

- подписка Azure AD;
- подписка TonicDM с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление TonicDM из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-tonicdm-from-the-gallery"></a>Добавление TonicDM из коллекции
Чтобы настроить интеграцию TonicDM с Azure AD, необходимо добавить TonicDM из коллекции в список управляемых приложений SaaS.

**Чтобы добавить TonicDM из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Новое приложение"][3]

1. В поле поиска введите **TonicDM**, выберите **TonicDM** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![TonicDM в списке результатов](./media/tonicdm-tutorial/tutorial_tonicdm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в TonicDM с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в TonicDM соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в TonicDM.

Чтобы настроить и проверить единый вход Azure AD в TonicDM, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя приложения TonicDM](#create-a-tonicdm-test-user)** требуется для того, чтобы в TonicDM существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD.
1. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить такой вход в приложении TonicDM.

**Чтобы настроить единый вход Azure AD в TonicDM, выполните следующие действия:**

1. На портале Azure на странице интеграции с приложением **TonicDM** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Диалоговое окно "Единый вход"](./media/tonicdm-tutorial/tutorial_tonicdm_samlbase.png)

1. В разделе **Домены и URL-адреса приложения TonicDM** выполните следующие действия:

    ![Сведения о домене и URL-адресах единого входа для приложения TonicDM](./media/tonicdm-tutorial/tutorial_tonicdm_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в формате `https://tonicdm.com/`.

    b. В текстовом поле **Идентификатор** введите URL-адрес: `https://tonicdm.com/saml/metadata`

1. В разделе **Сертификат для подписи токена SAML** щелкните **Certificate (Base64)** (Сертификат (Base64)), а затем сохраните файл сертификата на компьютере.

    ![Ссылка для скачивания сертификата](./media/tonicdm-tutorial/tutorial_tonicdm_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/tonicdm-tutorial/tutorial_general_400.png)

1. В разделе **Конфигурация TonicDM** щелкните **Настроить TonicDM**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода, идентификатор сущности SAML и URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Конфигурация TonicDM](./media/tonicdm-tutorial/tutorial_tonicdm_configure.png) 

1. Чтобы настроить единый вход на стороне **TonicDM**, нужно отправить скачанный **сертификат в формате Base64, URL-адрес выхода, идентификатор сущности SAML и URL-адрес службы единого входа SAML** [службе поддержки TonicDM](mailto:support@tonicdm.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка "Azure Active Directory"](./media/tonicdm-tutorial/create_aaduser_01.png)

1. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/tonicdm-tutorial/create_aaduser_02.png)

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/tonicdm-tutorial/create_aaduser_03.png)

1. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/tonicdm-tutorial/create_aaduser_04.png)

    a. В поле **Имя** введите **BrittaSimon**.

    Б. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="create-a-tonicdm-test-user"></a>Создание тестового пользователя TonicDM

В этом разделе описано, как создать пользователя Britta Simon в приложении TonicDM. Обратитесь в [службу поддержки TonicDM](mailto:support@tonicdm.com), чтобы добавить пользователей на платформу TonicDM. Перед использованием единого входа необходимо создать и активировать пользователей.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к TonicDM.

![Назначение роли пользователя][200] 

**Чтобы назначить пользователя Britta Simon в TonicDM, сделайте следующее:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **TonicDM**.

    ![Ссылка на TonicDM в списке "Приложения"](./media/tonicdm-tutorial/tutorial_tonicdm_app.png)  

1. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент TonicDM на панели доступа, вы автоматически войдете в приложение TonicDM.
Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/tonicdm-tutorial/tutorial_general_01.png
[2]: ./media/tonicdm-tutorial/tutorial_general_02.png
[3]: ./media/tonicdm-tutorial/tutorial_general_03.png
[4]: ./media/tonicdm-tutorial/tutorial_general_04.png

[100]: ./media/tonicdm-tutorial/tutorial_general_100.png

[200]: ./media/tonicdm-tutorial/tutorial_general_200.png
[201]: ./media/tonicdm-tutorial/tutorial_general_201.png
[202]: ./media/tonicdm-tutorial/tutorial_general_202.png
[203]: ./media/tonicdm-tutorial/tutorial_general_203.png

