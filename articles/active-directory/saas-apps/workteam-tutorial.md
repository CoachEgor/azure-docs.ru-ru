---
title: Руководство по Интеграция Azure Active Directory с Workteam | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Workteam.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 41df17a1-ba69-414f-8ec3-11079b030df6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: d628b12171cd67455ea308a2ca2b29e67855cccc
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2019
ms.locfileid: "68825612"
---
# <a name="tutorial-azure-active-directory-integration-with-workteam"></a>Руководство по Интеграция Azure Active Directory с Workteam

В этом руководстве описано, как интегрировать Workteam с Azure Active Directory (Azure AD).
Интеграция Workteam с Azure AD обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к Workteam.
* Вы можете включить автоматический вход пользователей в Workteam (единый вход) с помощью учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Workteam, вам потребуется следующее:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/));
* подписка Workteam с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Workteam поддерживает единый вход, инициированный **пакетом обновления** и **IDP**.

## <a name="adding-workteam-from-the-gallery"></a>добавление Workteam из коллекции;

Чтобы настроить интеграцию Workteam с Azure AD, необходимо добавить Workteam из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Workteam из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Workteam**, выберите **Workteam** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Workteam в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Workteam с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Workteam.

Чтобы настроить и проверить единый вход Azure AD в Workteam, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Workteam](#configure-workteam-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Workteam](#create-workteam-test-user)** требуется для того, чтобы в Workteam существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Workteam, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Workteam** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. Если нужно настроить приложение в режиме, инициируемом **поставщиком удостоверений**, то в разделе **Базовая конфигурация SAML** пользователю для этого не нужно ничего делать, так как это приложение уже предварительно интегрировано в Azure.

    ![Сведения о домене и URL-адресах единого входа для приложения Workteam](common/preintegrated.png)

5. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Workteam](common/metadata-upload-additional-signon.png)

    В текстовом поле **URL-адрес входа** введите URL-адрес: `https://app.workte.am`.

6. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

7. Скопируйте требуемые URL-адреса из раздела **Настройка Workteam**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-workteam-single-sign-on"></a>Настройка единого входа Workteam

1. В другом окне веб-браузера войдите в Workteam в качестве администратора безопасности.

2. В правом верхнем углу щелкните **логотип профиля** и выберите пункт **Organization settings** (Параметры организации). 

    ![Параметры Workteam](./media/workteam-tutorial/tutorial_workteam_settings.png)

3. В разделе **Authentication** (Проверка подлинности) щелкните **значок параметров**.

     ![Workteam и Azure](./media/workteam-tutorial/tutorial_workteam_azure.png)

4. На странице **Параметры SAML** выполните следующие действия.

     ![Workteam и Saml](./media/workteam-tutorial/tutorial_workteam_saml.png)

    a. Укажите для параметра **SAML IdP** (Поставщик удостоверений SAML) значение **AD Azure**.

    b. В текстовое поле **SAML Single Sign-On Service URL** (URL-адрес службы единого входа SAML) вставьте значение **URL-адрес входа**, скопированное на портале Azure.

    c. В текстовое поле **SAML Entity ID** (Идентификатор сущности SAML) вставьте значение **идентификатора Azure AD**, скопированное на портале Azure.

    d. В Блокноте откройте скачанный с портала Azure **сертификат в кодировке base-64**, скопируйте его содержимое, а затем вставьте его в поле **SAML Signing Certificate (Base64)** (Сертификат для подписи SAML (Base64)).

    д. Последовательно выберите **ОК**.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD 

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В разделе свойств пользователя сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    а. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите **brittasimon@yourcompanydomain.extension** .  
    Например BrittaSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив доступ к Workteam.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем **Workteam**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Workteam**.

    ![Ссылка на Workteam в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-workteam-test-user"></a>Создание тестового пользователя Workteam

Чтобы пользователи Azure AD могли выполнять вход в Workteam, их следует подготовить для работы в Workteam. В Workteam подготовка выполняется вручную.

**Чтобы подготовить учетную запись пользователя, сделайте следующее:**

1. Войдите в Workteam с правами администратора безопасности.

2. В средней верхней части страницы **Organization settings** (Параметры организации) щелкните вкладку **Users** (Пользователи) и нажмите кнопку **New User** (Новый пользователь).

    ![Пользователь Workteam](./media/workteam-tutorial/tutorial_workteam_user.png)

3. На странице **New employee** (Новый сотрудник) выполните следующие действия:

    ![Новый пользователь Workteam](./media/workteam-tutorial/tutorial_workteam_newuser.png)

    a. В текстовое поле **Name** (Имя) введите имя пользователя, например **Brittasimon**.

    b. В текстовом поле **Email** (Адрес электронной почты) введите адрес электронной почты пользователя, например **Brittasimon\@contoso.com**.

    c. Последовательно выберите **ОК**.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Workteam на Панели доступа, вы автоматически войдете в приложение Workteam, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

