---
title: Руководство по Интеграция Azure Active Directory с LockPath Keylight | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в LockPath Keylight.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 234a32f1-9f56-4650-9e31-7b38ad734b1a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4bc5121f6604fae9a28b52db1bfb308d7cdb968d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60263545"
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a>Руководство по Интеграция Azure Active Directory с LockPath Keylight

В этом руководстве описано, как интегрировать LockPath Keylight с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением LockPath Keylight обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к LockPath Keylight.
- Вы можете включить автоматический вход пользователей в LockPath Keylight (единый вход) с использованием учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Технические условия

Чтобы настроить интеграцию Azure AD с LockPath Keylight, вам потребуется:

- подписка Azure AD;
- подписка LockPath Keylight с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление LockPath Keylight из коллекции.
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-lockpath-keylight-from-the-gallery"></a>Добавление LockPath Keylight из коллекции
Чтобы настроить интеграцию LockPath Keylight с Azure AD, необходимо добавить LockPath Keylight из коллекции в список управляемых приложений SaaS.

**Чтобы добавить LockPath Keylight из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **LockPath Keylight**.

    ![Создание тестового пользователя Azure AD](./media/keylight-tutorial/tutorial_keylight_search.png)

1. На панели результатов выберите **LockPath Keylight** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Создание тестового пользователя Azure AD](./media/keylight-tutorial/tutorial_keylight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в LockPath Keylight с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходима информация о том, какой пользователь в LockPath Keylight соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в LockPath Keylight.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в LockPath Keylight.

Чтобы настроить и проверить единый вход Azure AD в LockPath Keylight, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя LockPath Keylight](#creating-a-lockpath-keylight-test-user)** требуется для того, чтобы в LockPath Keylight существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В данном разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении LockPath Keylight.

**Чтобы настроить единый вход Azure AD в LockPath Keylight, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **LockPath Keylight** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/keylight-tutorial/tutorial_keylight_samlbase.png)

1. В разделе **Домены и URL-адреса приложения LockPath Keylight** выполните следующие действия.

    ![Настройка единого входа](./media/keylight-tutorial/tutorial_keylight_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<company name>.keylightgrc.com/`

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<company name>.keylightgrc.com`

    c. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://<company name>.keylightgrc.com/Login.aspx`.
    
    > [!NOTE] 
    > Эти значения приведены для примера. Замените их фактическими значениями идентификатора, URL-адреса ответа и URL-адреса входа. Чтобы получить эти значения, обратитесь к [группе поддержки клиентов LockPath Keylight](https://www.lockpath.com/contact/). 

1. В разделе **Сертификат подписи SAML** щелкните **Сертификат (необработанный)**, а затем сохраните файл сертификата на компьютер.

    ![Настройка единого входа](./media/keylight-tutorial/tutorial_keylight_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/keylight-tutorial/tutorial_general_400.png)
    
1. В разделе **Конфигурация LockPath Keylight** щелкните **Настроить LockPath Keylight**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода и URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Настройка единого входа](./media/keylight-tutorial/tutorial_keylight_configure.png) 

1. Чтобы включить единый вход в LockPath Keylight, выполните следующие действия.
   
    a. Войдите в учетную запись LockPath Keylight в качестве администратора.
    
    2. В меню вверху щелкните **Person** (Пользователь) и выберите **Keylight Setup** (Настройка Keylight).
   
    ![Настройка единого входа](./media/keylight-tutorial/401.png) 

    c. В представлении в виде дерева слева щелкните **SAML**.
   
    ![Настройка единого входа](./media/keylight-tutorial/402.png) 

    d. В диалоговом окне **SAML Settings** (Параметры SAML) нажмите кнопку **Edit** (Изменить).
   
    ![Настройка единого входа](./media/keylight-tutorial/404.png) 

1. На странице диалогового окна **Изменение параметров SAML** выполните следующие действия:
   
    ![Настройка единого входа](./media/keylight-tutorial/405.png) 
   
    a. Задайте для параметра **SAML authentication** (Аутентификация SAML) значение **Active** (Активно).

    2. Вставьте значение **SAML Single Sign-On Service URL** (URL-адрес службы единого входа SAML), скопированное на портале Azure, в текстовое поле **Identity Provider Login URL** (URL-адрес входа поставщика удостоверений).

    c. Вставьте значение **URL-адрес службы единого выхода**, скопированное на портале Azure, в текстовое поле **Identity Provider Logout URL** (URL-адрес выхода поставщика удостоверений).

    d. Щелкните **Choose File** (Выбрать файл), чтобы выбрать скачанный сертификат LockPath Keylight, а затем нажмите кнопку **Open** (Открыть), чтобы передать этот сертификат.

    д. В поле **SAML User Id location** (Расположение идентификатора пользователя SAML) выберите значение **NameIdentifier element of the subject statement** (Элемент NameIdentifier оператора Subject).
    
    Е. Введите значение **Keylight Service Provider** (Поставщик услуг Keylight), используя следующий формат: **https://&lt;название_компании&gt;.keylightgrc.com**.
    
    ж. Задайте для параметра **Auto-provision users** (Автоматическая подготовка пользователей) значение **Active** (Активно).

    h. Задайте для параметра **Auto-provision account type** (Тип учетной записи для автоматической подготовки) значение **Full User** (С полным доступом).

    i. Задайте для параметра **Auto-provision security role** (Роль безопасности для автоматической подготовки) значение **Standard User with SAML** (Права обычного пользователя с SAML).
    
    j. Задайте для параметра **Auto-provision security config** (Конфигурация безопасности для автоматической подготовки) значение **Standard User Configuration** (Конфигурация обычного пользователя).
     
    k. В текстовое поле **Email Attribute** (Атрибут электронной почты) введите `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    l. В текстовое поле **First name attribute** (Атрибут имени) введите значение `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
    
    m. В текстовое поле **Last name attribute** (Атрибут фамилии) введите значение `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.
    
    n. Выберите команду **Сохранить**.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/keylight-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/keylight-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/keylight-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/keylight-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    2. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-a-lockpath-keylight-test-user"></a>Создание тестового пользователя LockPath Keylight

В этом разделе описано, как создать пользователя Britta Simon в приложении LockPath Keylight. LockPath Keylight поддерживает JIT-подготовку. Эта функция включена по умолчанию.

В этом разделе никакие действия с вашей стороны не требуются. Если пользователь не существует, то он создается при доступе к LockPath Keylight. 

>[!NOTE]
>Если вам нужно вручную создать пользователя, необходимо обратиться к [группе поддержки клиентов LockPath Keylight](https://www.lockpath.com/contact/). 

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к LockPath Keylight.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в LockPath Keylight, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. Из списка приложений выберите **LockPath Keylight**.

    ![Настройка единого входа](./media/keylight-tutorial/tutorial_keylight_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "LockPath Keylight" на панели доступа, вы автоматически войдете в приложение LockPath Keylight. 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/keylight-tutorial/tutorial_general_01.png
[2]: ./media/keylight-tutorial/tutorial_general_02.png
[3]: ./media/keylight-tutorial/tutorial_general_03.png
[4]: ./media/keylight-tutorial/tutorial_general_04.png

[100]: ./media/keylight-tutorial/tutorial_general_100.png

[200]: ./media/keylight-tutorial/tutorial_general_200.png
[201]: ./media/keylight-tutorial/tutorial_general_201.png
[202]: ./media/keylight-tutorial/tutorial_general_202.png
[203]: ./media/keylight-tutorial/tutorial_general_203.png

