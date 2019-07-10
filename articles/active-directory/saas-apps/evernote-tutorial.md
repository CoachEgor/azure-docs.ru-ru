---
title: Руководство по Интеграция Azure Active Directory с Evernote | Документация Майкрософт
description: Сведения о настройке единого входа между Azure Active Directory и Evernote.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/10/2019
ms.author: jeedes
ms.openlocfilehash: 4185e9f5e1b411f95afacb80499db048769ea27b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67102969"
---
# <a name="tutorial-azure-active-directory-integration-with-evernote"></a>Руководство по Интеграция Azure Active Directory с Evernote

В этом руководстве описано, как интегрировать Evernote с Azure Active Directory (Azure AD).
Интеграция Evernote с Azure AD обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к Evernote.
* Вы можете включить автоматический вход пользователей в Evernote (единый вход) с помощью учетной записи Azure Active Directory.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Evernote, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка Evernote с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Evernote поддерживает единый вход, инициированный **поставщиком услуг** и **поставщиком удостоверений**.

## <a name="adding-evernote-from-the-gallery"></a>Добавление Evernote из коллекции.

Чтобы настроить интеграцию Evernote с Azure AD, необходимо добавить Evernote из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Evernote из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Новое приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Evernote**, выберите **Evernote** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Evernote в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure Active Directory в приложение Evernote с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure Active Directory и соответствующим пользователем в Evernote.

Чтобы настроить и проверить единый вход Azure AD в Evernote, необходимо выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Evernote](#configure-evernote-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Evernote](#create-evernote-test-user)** требуется, чтобы в Evernote существовал пользователь Britta Simon, связанный с соответствующим представлением в Azure Active Directory.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure Active Directory в Evernote, сделайте следующее:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Evernote** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** выполните следующее действие.

    ![Сведения о домене и URL-адресах единого входа приложения Evernote](common/idp-identifier.png)

    В текстовом поле **Идентификатор** введите URL-адрес: `https://www.evernote.com/saml2`

5. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    ![image](common/both-preintegrated-signon.png)

    В текстовом поле **URL-адрес входа** введите URL-адрес: `https://www.evernote.com/Login.action`.

6. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

7. В разделе **Сертификат подписи SAML** нажмите кнопку **Изменить**, чтобы открыть диалоговое окно **Сертификат подписи SAML**.

    ![image](common/edit-certificate.png) 

    ![image](./media/evernote-tutorial/samlassertion.png)

    a. Для параметра **Вариант подписывания** выберите значение **Ответ знака SAML и утверждение**.

    b. Нажмите кнопку **Сохранить**

8. Требуемый URL-адрес вы можете скопировать из раздела **Настройка Evernote**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-evernote-single-sign-on"></a>Настройка единого входа в Evernote

1. В другом окне веб-браузера войдите на сайт компании Evernote в качестве администратора.

2. Перейдите в **консоль администрирования**.

    ![Консоль администрирования](./media/evernote-tutorial/tutorial_evernote_adminconsole.png)

3. Из **консоли администрирования** перейдите в раздел **Безопасность** и выберите **Единый вход**.

    ![Настройка единого входа](./media/evernote-tutorial/tutorial_evernote_sso.png)

4. Задайте следующие значения.

    ![Настройка сертификата](./media/evernote-tutorial/tutorial_evernote_certx.png)
    
    a.  **Включить единый вход**. Единый вход включен по умолчанию (щелкните **Отключить единый вход**, чтобы удалить требование единого входа).

    b. Вставьте значение **URL-адреса входа**, скопированное на портале Azure, в текстовое поле **SAML HTTP Request URL** (URL-адрес HTTP-запроса SAML).

    c. Откройте в Блокноте скачанный из Azure AD сертификат и скопируйте его содержимое, включая строки BEGIN CERTIFICATE и END CERTIFICATE, в текстовое поле **X.509 Certificate** (Сертификат X.509). 

    Щелкните **Сохранить изменения**.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD 

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В разделе свойств пользователя сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    а. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите `brittasimon@yourcompanydomain.extension`. Например BrittaSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход в Azure путем предоставления этому пользователю доступа к Evernote.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Evernote**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Evernote**.

    ![Ссылка на Evernote в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-evernote-test-user"></a>Создание тестового пользователя в Evernote

Чтобы пользователи Azure AD могли выполнять вход в Evernote, они должны быть подготовлены для Evernote.  
В случае с Evernote подготовка выполняется вручную.

**Чтобы подготовить учетные записи пользователей, выполните следующие действия.**

1. Войдите на сайт компании Evernote в качестве администратора.

2. Щелкните **Консоль администрирования**.

    ![Консоль администрирования](./media/evernote-tutorial/tutorial_evernote_adminconsole.png)

3. В **консоли администрирования** выберите **Добавить пользователей**.

    ![Добавление тестового пользователя](./media/evernote-tutorial/create_aaduser_0001.png)

4. **Добавьте адреса электронной почты участников команды** в **соответствующих полях** и нажмите кнопку **Пригласить**.

    ![Добавление тестового пользователя](./media/evernote-tutorial/create_aaduser_0002.png)
    
5. После этого владелец учетной записи Azure Active Directory получает сообщение электронной почты, чтобы принять приглашение.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Evernote на Панели доступа, вы автоматически войдете в приложение Evernote, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

