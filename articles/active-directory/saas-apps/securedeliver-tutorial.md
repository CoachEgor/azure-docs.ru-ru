---
title: Руководство по Интеграция Azure Active Directory с SECURE DELIVER | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в SECURE DELIVER.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: fccd5668-fe6f-4e6d-a9ce-ba4f321c33d1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e08c8c4c576f9dd0ee6896b7adf53279afc9f53c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60340342"
---
# <a name="tutorial-azure-active-directory-integration-with-secure-deliver"></a>Руководство по Интеграция Azure Active Directory с SECURE DELIVER

В этом руководстве описано, как интегрировать SECURE DELIVER с Azure Active Directory (Azure AD).

Интеграция SECURE DELIVER с Azure AD имеет следующие преимущества.

- С помощью Azure AD вы можете управлять доступом к SECURE DELIVER.
- Вы можете включить автоматический вход пользователей в SECURE DELIVER (единый вход) с использованием учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Технические условия

Чтобы настроить интеграцию Azure AD с SECURE DELIVER, вам потребуется следующее.

- подписка Azure AD;
- подписка SECURE DELIVER с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление SECURE DELIVER из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-secure-deliver-from-the-gallery"></a>Добавление SECURE DELIVER из коллекции
Чтобы настроить интеграцию SECURE DELIVER с Azure AD, необходимо добавить SECURE DELIVER из коллекции в список управляемых приложений SaaS.

**Чтобы добавить SECURE DELIVER из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **SECURE DELIVER**.

    ![Создание тестового пользователя Azure AD](./media/securedeliver-tutorial/tutorial_securedeliver_search.png)

1. На панели результатов выберите **SECURE DELIVER** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Создание тестового пользователя Azure AD](./media/securedeliver-tutorial/tutorial_securedeliver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в SECURE DELIVER с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходима информация о том, какой пользователь в SECURE DELIVER соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в SECURE DELIVER.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в SECURE DELIVER.

Чтобы настроить и проверить единый вход Azure AD в SECURE DELIVER, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя SECURE DELIVER](#creating-a-secure-deliver-test-user)** требуется для того, чтобы в SECURE DELIVER существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении SECURE DELIVER.

**Чтобы настроить единый вход Azure AD в SECURE DELIVER, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **SECURE DELIVER** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/securedeliver-tutorial/tutorial_securedeliver_samlbase.png)

1. В разделе **Домены и URL-адреса приложения SECURE DELIVER** выполните следующие действия.

    ![Настройка единого входа](./media/securedeliver-tutorial/tutorial_securedeliver_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<companyname>.i-securedeliver.jp/sd/<tenantname>/jsf/login/sso`

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<companyname>.i-securedeliver.jp/sd/<tenantname>/postResponse`

    > [!NOTE] 
    > Эти значения приведены для примера. Замените эти значения фактическим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь к [группе поддержки клиентов SECURE DELIVER](mailto:iw-sd-support@fujifilm.com). 
 
1. В разделе **Сертификат для подписи токена SAML** щелкните **Certificate (Base64)** (Сертификат (Base64)), а затем сохраните файл сертификата на компьютере.

    ![Настройка единого входа](./media/securedeliver-tutorial/tutorial_securedeliver_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/securedeliver-tutorial/tutorial_general_400.png)

1. В разделе **Настройка SECURE DELIVER** щелкните **Настроить SECURE DELIVER**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода, идентификатор сущности SAML и URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Настройка единого входа](./media/securedeliver-tutorial/tutorial_securedeliver_configure.png) 

1. Чтобы настроить единый вход на стороне **SECURE DELIVER**, нужно отправить скачанный **сертификат в кодировке Base64**, **URL-адрес выхода, идентификатор сущности SAML и URL-адрес службы единого входа SAML** [группе поддержки SECURE DELIVER](mailto:iw-sd-support@fujifilm.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/securedeliver-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/securedeliver-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/securedeliver-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/securedeliver-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    2. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-a-secure-deliver-test-user"></a>Создание тестового пользователя в SECURE DELIVER

Цель этого раздела — создать в приложении SECURE DELIVER пользователя с именем Britta Simon. Обратитесь к [группе поддержки SECURE DELIVER](mailto:iw-sd-support@fujifilm.com), чтобы добавить пользователей в учетную запись SECURE DELIVER.

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к SECURE DELIVER.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon приложению SECURE DELIVER, сделайте следующее.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **SECURE DELIVER**.

    ![Настройка единого входа](./media/securedeliver-tutorial/tutorial_securedeliver_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

Цель этого раздела — проверить конфигурацию единого входа Azure AD с помощью панели доступа.  

Щелкнув плитку SECURE DELIVER на панели доступа, вы автоматически войдете в приложение SECURE DELIVER.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/securedeliver-tutorial/tutorial_general_01.png
[2]: ./media/securedeliver-tutorial/tutorial_general_02.png
[3]: ./media/securedeliver-tutorial/tutorial_general_03.png
[4]: ./media/securedeliver-tutorial/tutorial_general_04.png

[100]: ./media/securedeliver-tutorial/tutorial_general_100.png

[200]: ./media/securedeliver-tutorial/tutorial_general_200.png
[201]: ./media/securedeliver-tutorial/tutorial_general_201.png
[202]: ./media/securedeliver-tutorial/tutorial_general_202.png
[203]: ./media/securedeliver-tutorial/tutorial_general_203.png

