---
title: Руководство по Интеграция Azure Active Directory с HackerOne | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в HackerOne.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 229d1efb-b6a5-4df8-9839-5d551487db4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6faf023d36ea40e3694161ee0cfc8aac7c5652fa
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2019
ms.locfileid: "65898390"
---
# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a>Руководство по Интеграция Azure Active Directory с HackerOne

В данном руководстве описано, как интегрировать Azure Active Directory (Azure AD) с приложением HackerOne.
Интеграция HackerOn с Azure AD дает приведенные далее преимущества:

* С помощью Azure AD вы можете контролировать доступ к HackerOn.
* Вы можете включить автоматический вход пользователей в HackerOne (единый вход) с помощью учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с HackerOn, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка на HackerOne с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* HackerOne поддерживает единый вход, инициированный **поставщиком услуг**.
* HackerOne поддерживает **JIT**-подготовку пользователей.

## <a name="adding-hackerone-from-the-gallery"></a>Добавление HackerOne из коллекции

Чтобы настроить интеграцию HackerOne с Azure AD, необходимо добавить HackerOne из коллекции в список управляемых приложений SaaS.

**Чтобы добавить HackerOn из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **HackerOne**, выберите **HackerOne** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![HackerOne в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в HackerOne с использованием тестового пользователя **Britta Simon**.
Чтобы обеспечить работу единого входа, нужно установить связь между пользователем Azure AD и соответствующим пользователем в HackerOne.

Чтобы настроить и проверить единый вход Azure AD в HackerOne, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в HackerOne](#configure-hackerone-single-sign-on)** требуется для того, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя HackerOne](#create-hackerone-test-user)** требуется для того, чтобы в HackerOne существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в HackerOne, выполните следующие действия:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **HackerOne** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа приложения HackerOne](common/sp-identifier.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://hackerone.com/<company name>/authentication`.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес следующим образом: `https://hackerone.com/users/saml/metadata`

    > [!NOTE]
    > Значение URL-адреса входа приведено для примера. Вместо него необходимо указать фактический URL-адрес входа. Чтобы получить его, обратитесь в [службу поддержки клиентов HackerOne](mailto:support@hackerone.com). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

6. В разделе **Set up HackerOne** (Настройка HackerOne) скопируйте требуемый URL-адрес.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    б) Идентификатор Azure AD.

    в) URL-адрес выхода.

### <a name="configure-hackerone-single-sign-on"></a>Настройка единого входа для HackerOne

1. Войдите в клиент HackerOne как администратор.

2. В верхнем меню щелкните пункт **Параметры**.

    ![Настройка единого входа](./media/hackerone-tutorial/tutorial_hackerone_001.png)

3. Перейдите к разделу **Authentication** (Аутентификация) и щелкните **Add SAML settings** (Добавить параметры SAML).

    ![Настройка единого входа](./media/hackerone-tutorial/tutorial_hackerone_003.png)

4. В диалоговом окне **SAML Settings** (Параметры SAML) выполните следующие действия.

    ![Настройка единого входа](./media/hackerone-tutorial/tutorial_hackerone_004.png)

    a. Введите имя зарегистрированного домена в поле **Email Domain** (Домен электронной почты).

    b. В текстовом поле **Single Sign On URL** (URL-адрес единого входа) вставьте значение **URL-адреса входа**, скопированное на портале Azure.

    c. Откройте в Блокноте **файл сертификата**, скачанный на портале Azure, скопируйте его содержимое в буфер обмена, а затем вставьте его в текстовое поле **X509 Certificate** (Сертификат X509).

    d. Выберите команду **Сохранить**.

5. В диалоговом окне "Authentication Settings" (Параметры аутентификации) выполните следующие действия.

    ![Настройка единого входа](./media/hackerone-tutorial/tutorial_hackerone_005.png)

    a. Щелкните **Run test**(Выполнить проверку).

    b. Если поле **Status** (Состояние) имеет значение **Last test status: created** (Состояние при последней проверке: создан), обратитесь в [службу поддержки HackerOne](mailto:support@hackerone.com), чтобы запросить проверку своей конфигурации.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В разделе свойств пользователя сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    а. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите **brittasimon\@домен_вашей_компании.доменная_зона**.  
    Например BrittaSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure путем предоставления доступа к HackerOne.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения** и **HackerOne**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **HackerOne**.

    ![Ссылка на HackerOne в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-hackerone-test-user"></a>Создание тестового пользователя HackerOne

В этом разделе объясняется, как создать в HackerOne пользователя Britta Simon. Приложение HackerOne поддерживает JIT-подготовку пользователей, которая включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в HackerOne, он создается после проверки подлинности.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку HackerOne на Панели доступа, вы автоматически войдете в приложение HackerOne, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)