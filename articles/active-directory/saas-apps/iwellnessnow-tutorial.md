---
title: Руководство. Интеграция Azure Active Directory с iWellnessNow | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и iWellnessNow.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 24ffc841-7a77-481c-9cc4-6f8bda58fe66
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 02b831df98db5b9d63873a0da93e603cd7cbf308
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60269095"
---
# <a name="tutorial-azure-active-directory-integration-with-iwellnessnow"></a>Руководство. Интеграция Azure Active Directory с iWellnessNow

В этом руководстве описано, как интегрировать iWellnessNow с Azure Active Directory (Azure AD).

Интеграция iWellnessNow с Azure AD обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к iWellnessNow.
- Вы можете включить автоматический вход пользователей в iWellnessNow (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Технические условия

Чтобы настроить интеграцию Azure AD с iWellnessNow, вам потребуется:

- подписка Azure AD;
- подписка iWellnessNow с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление iWellnessNow из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-iwellnessnow-from-the-gallery"></a>Добавление iWellnessNow из коллекции
Чтобы настроить интеграцию iWellnessNow с Azure AD, необходимо добавить iWellnessNow из коллекции в список управляемых приложений SaaS.

**Чтобы добавить iWellnessNow из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"][3]

1. В поле поиска введите **iWellnessNow**, выберите **iWellnessNow** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![iWellnessNow в списке результатов](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в iWellnessNow с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в iWellnessNow соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в iWellnessNow.

Чтобы настроить и проверить единый вход Azure AD в iWellnessNow, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя iWellnessNow](#create-an-iwellnessnow-test-user)** требуется для того, чтобы в iWellnessNow существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
1. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении iWellnessNow.

**Чтобы настроить единый вход Azure AD в iWellnessNow, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **iWellnessNow** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Диалоговое окно "Единый вход"](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_samlbase.png)

1. Если у вас есть **файл метаданных поставщика службы** и вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, то в разделе **Домены и URL-адреса приложения iWellnessNow** сделайте следующее:

    ![Передача сведений о домене и URL-адресах для единого входа для приложения iWellnessNow](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_upload.png)

    a. Щелкните **Отправить файл метаданных**.

    ![Передача конфигурации домена и URL-адресов для единого входа в приложение iWellnessNow](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_uploadconfig.png)

    2\. Щелкните **значок папки**, выберите файл метаданных и нажмите кнопку **Отправить**.
    
    c. После успешной передачи **файла метаданных поставщика службы** значения **идентификатора** и **URL-адреса ответа** автоматически добавятся в раздел **Домены и URL-адреса приложения iWellnessNow**, как показано ниже:

    ![Сведения о домене и URL-адресах для единого входа для приложения iWellnessNow](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url3.png)

1. Если у вас нет **файл метаданных поставщика службы** и хотите настроить приложение в **IDP** инициировал режим, выполните следующие действия:

    ![Сведения о домене и URL-адресах для единого входа для приложения iWellnessNow](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `http://<CustomerName>.iwellnessnow.com`

    2\. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://<CustomerName>.iwellnessnow.com/ssologin`.

1. Установите флажок **Показать дополнительные параметры URL-адресов**, и выполните следующее действие, если хотите настроить приложение для работы в режиме, инициируемом **поставщиком услуг**:

    ![Сведения о домене и URL-адресах для единого входа для приложения iWellnessNow](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url1.png)

    В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<CustomerName>.iwellnessnow.com/`
     
    > [!NOTE] 
    > Эти значения приведены для примера. Замените их фактическими значениями идентификатора, URL-адреса ответа и URL-адреса входа. Чтобы получить эти значения, обратитесь к [группе поддержки клиентов iWellnessNow](mailto:info@iwellnessnow.com).

1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Ссылка для скачивания сертификата](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/iwellnessnow-tutorial/tutorial_general_400.png)
    
1. Чтобы настроить единый вход на стороне **iWellnessNow**, отправьте [группе поддержки iWellnessNow](mailto:info@iwellnessnow.com) скачанный **XML-файл метаданных**. Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка Azure Active Directory](./media/iwellnessnow-tutorial/create_aaduser_01.png)

1. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/iwellnessnow-tutorial/create_aaduser_02.png)

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/iwellnessnow-tutorial/create_aaduser_03.png)

1. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/iwellnessnow-tutorial/create_aaduser_04.png)

    a. В поле **Имя** введите **BrittaSimon**.

    Б. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="create-an-iwellnessnow-test-user"></a>Создание тестового пользователя iWellnessNow

В этом разделе описано, как создать пользователя Britta Simon в iWellnessNow. Обратитесь в службу  [технической поддержки iWellnessNow](mailto:info@iwellnessnow.com), чтобы добавить пользователей на платформу iWellnessNow. Перед использованием единого входа необходимо создать и активировать пользователей.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к iWellnessNow.

![Назначение роли пользователя][200] 

**Чтобы назначить пользователя Britta Simon в iWellnessNow, сделайте следующее.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. Из списка приложений выберите **iWellnessNow**.

    ![Ссылка на iWellnessNow в списке "Приложения"](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_app.png)  

1. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку "iWellnessNow" на панели доступа, вы автоматически войдете в приложение iWellnessNow.
Дополнительные сведения о панели доступа см. в статье с [общими сведениями о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/iwellnessnow-tutorial/tutorial_general_01.png
[2]: ./media/iwellnessnow-tutorial/tutorial_general_02.png
[3]: ./media/iwellnessnow-tutorial/tutorial_general_03.png
[4]: ./media/iwellnessnow-tutorial/tutorial_general_04.png

[100]: ./media/iwellnessnow-tutorial/tutorial_general_100.png

[200]: ./media/iwellnessnow-tutorial/tutorial_general_200.png
[201]: ./media/iwellnessnow-tutorial/tutorial_general_201.png
[202]: ./media/iwellnessnow-tutorial/tutorial_general_202.png
[203]: ./media/iwellnessnow-tutorial/tutorial_general_203.png

