---
title: Руководство по Интеграция Azure Active Directory с RightScale | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Rightscale.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 3a8d376d-95fb-4dd7-832a-4fdd4dd7c87c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 416a98c5f9c5a2ec813206ea9ea7f311b23e86cb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61080742"
---
# <a name="tutorial-azure-active-directory-integration-with-rightscale"></a>Руководство по Интеграция Azure Active Directory с Rightscale

В этом руководстве описано, как интегрировать Rightscale с Azure Active Directory (Azure AD).

Интеграция Rightscale с Azure AD обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к Rightscale.
- Вы можете включить автоматический вход пользователей в Rightscale (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Технические условия

Чтобы настроить интеграцию Azure AD с приложением Rightscale, вам потребуется:

- подписка Azure AD;
- подписка Rightscale с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Rightscale из коллекции.
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-rightscale-from-the-gallery"></a>Добавление Rightscale из коллекции
Чтобы настроить интеграцию Rightscale с Azure AD, необходимо добавить Rightscale из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Rightscale из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Приложения][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Приложения][3]

1. В поле поиска введите **Rightscale**.

    ![Создание тестового пользователя Azure AD](./media/rightscale-tutorial/tutorial_rightscale_search.png)

1. На панели результатов выберите **Rightscale** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Создание тестового пользователя Azure AD](./media/rightscale-tutorial/tutorial_rightscale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в Rightscale с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходима информация о том, какой пользователь в Rightscale соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Rightscale.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Rightscale.

Чтобы настроить и проверить единый вход Azure AD в Rightscale, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Rightscale](#creating-a-rightscale-test-user)** требуется для того, чтобы в Rightscale существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Rightscale.

**Чтобы настроить единый вход Azure AD в Rightscale, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **Rightscale** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/rightscale-tutorial/tutorial_rightscale_samlbase.png)

1. Если вы хотите настроить приложение в **режиме, инициируемом IdP**, то в разделе **Домены и URL-адреса приложения Rightscale** не нужно ничего делать, так как это приложение уже предварительно интегрировано в Azure.

    ![Настройка единого входа](./media/rightscale-tutorial/tutorial_rightscale_url.png)

1. В разделе **Домены и URL-адреса приложения Rightscale** выполните приведенные ниже действия, если вы хотите настроить приложение в **режиме, инициируемом поставщиком услуг**.
    
    ![Настройка единого входа](./media/rightscale-tutorial/tutorial_rightscale_url1.png)

    a. Установите флажок **Показать дополнительные параметры URL-адресов**.

    2. В текстовое поле **URL-адрес для входа** введите URL-адрес `https://login.rightscale.com/`.

1. В разделе **Сертификат для подписи токена SAML** щелкните **Certificate (Base64)** (Сертификат (Base64)), а затем сохраните файл сертификата на компьютере.

    ![Настройка единого входа](./media/rightscale-tutorial/tutorial_rightscale_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/rightscale-tutorial/tutorial_general_400.png)

1. В разделе **Конфигурация Rightscale** щелкните **Настроить Rightscale**, чтобы открыть окно **Настройка единого входа**. Скопируйте **идентификатор сущности SAML и URL-адрес службы единого входа SAML** из раздела **Quick Reference** (Краткий справочник).

    ![Настройка единого входа](./media/rightscale-tutorial/tutorial_rightscale_configure.png) 

1. Чтобы настроить для приложения единый вход, нужно войти в клиент приложения RightScale с правами администратора.

    a. В меню в верхней части откройте вкладку **Параметры** и выберите **Единый вход**.
   
    ![Настройка единого входа](./media/rightscale-tutorial/tutorial_rightscale_001.png) 

    2. Нажмите кнопку **Новый** и добавьте свои поставщики удостоверений SAML в области **Your SAML Identity Providers** (Ваши поставщики удостоверений SAML).
   
    ![Настройка единого входа](./media/rightscale-tutorial/tutorial_rightscale_002.png) 
 
    c. В текстовом поле **Отображаемое имя** введите название своей компании.
   
    ![Настройка единого входа](./media/rightscale-tutorial/tutorial_rightscale_003.png)
 
    d. Выберите **Allow RightScale-initiated SSO using a discovery hint** (Разрешить единый вход, инициированный RightScale, с использованием подсказки обнаружения) и в приведенном ниже текстовом поле введите **имя домена**.
   
    ![Настройка единого входа](./media/rightscale-tutorial/tutorial_rightscale_004.png)

    д. В текстовое поле **SAML SSO Endpoint** (Конечная точка единого входа SAML) в Rightscale вставьте значение **SAML Single Sign-On Service URL** (URL-адрес службы единого входа SAML), скопированное на портале Azure.
   
    ![Настройка единого входа](./media/rightscale-tutorial/tutorial_rightscale_006.png)

    Е. В текстовое поле **SAML EntityID** (Идентификатор сущности SAML) в Rightscale вставьте значение **SAML Entity ID** (Идентификатор сущности SAML), скопированное на портале Azure.
   
    ![Настройка единого входа](./media/rightscale-tutorial/tutorial_rightscale_008.png)

    ж. Нажмите кнопку **Browse** (Обзор), чтобы передать сертификат, скачанный с портала Azure.
   
    ![Настройка единого входа](./media/rightscale-tutorial/tutorial_rightscale_009.png)

    h. Выберите команду **Сохранить**.

   > [!TIP]
   > Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).
   > 

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/rightscale-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/rightscale-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/rightscale-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/rightscale-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    2. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-a-rightscale-test-user"></a>Создание тестового пользователя Rightscale

В этом разделе описано, как создать пользователя Britta Simon в приложении RightScale. Обратитесь к [группе поддержки клиентов Rightscale](mailto:support@rightscale.com), чтобы добавить пользователей на платформу Rightscale.

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Rightscale.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в Rightscale, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. Из списка приложений выберите **Rightscale**.

    ![Настройка единого входа](./media/rightscale-tutorial/tutorial_rightscale_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

Цель этого раздела — проверить конфигурацию единого входа Azure AD с помощью панели доступа.  

Щелкнув элемент RightScale на панели доступа, вы автоматически войдете в приложение RightScale.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/rightscale-tutorial/tutorial_general_01.png
[2]: ./media/rightscale-tutorial/tutorial_general_02.png
[3]: ./media/rightscale-tutorial/tutorial_general_03.png
[4]: ./media/rightscale-tutorial/tutorial_general_04.png

[100]: ./media/rightscale-tutorial/tutorial_general_100.png

[200]: ./media/rightscale-tutorial/tutorial_general_200.png
[201]: ./media/rightscale-tutorial/tutorial_general_201.png
[202]: ./media/rightscale-tutorial/tutorial_general_202.png
[203]: ./media/rightscale-tutorial/tutorial_general_203.png

