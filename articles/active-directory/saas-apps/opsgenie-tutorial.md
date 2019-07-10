---
title: Руководство по Интеграция Azure Active Directory с OpsGenie | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и OpsGenie.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 41b59b22-a61d-4fe6-ab0d-6c3991d1375f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 446ac54d84f7b2b3bf3aaf6eaf5536f0dfb804fe
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67095758"
---
# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a>Руководство по Интеграция Azure Active Directory с OpsGenie

В этом руководстве описано, как интегрировать OpsGenie с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением OpsGenie обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к OpsGenie.
* Вы можете включить автоматический вход пользователей в OpsGenie (единый вход) с помощью их учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с OpsGenie, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка OpsGenie с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* OpsGenie поддерживает единый вход, инициированный **поставщиком услуг**.

## <a name="adding-opsgenie-from-the-gallery"></a>Добавление OpsGenie из коллекции

Чтобы настроить интеграцию OpsGenie с Azure AD, необходимо добавить OpsGenie из коллекции в список управляемых приложений SaaS.

**Чтобы добавить OpsGenie из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **OpsGenie**, выберите **OpsGenie** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![OpsGenie в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описаны настройка и проверка единого входа Azure AD в приложение OpsGenie с использованием тестового пользователя **B. Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в OpsGenie.

Чтобы настроить и проверить единый вход Azure AD в OpsGenie, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в OpsGenie](#configure-opsgenie-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя B. Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить B. Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя OpsGenie](#create-opsgenie-test-user)** требуется для того, чтобы в OpsGenie существовал пользователь B. Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в OpsGenie, выполните приведенные ниже действия.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **OpsGenie** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа приложения OpsGenie](common/sp-signonurl.png)

    В текстовом поле **URL-адрес входа** введите URL-адрес: `https://app.opsgenie.com/auth/login`.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** нажмите кнопку "Копировать", чтобы копировать **URL-адрес метаданных федерации приложений** и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/copy-metadataurl.png)

6. Требуемый URL-адрес можно скопировать из раздела **Настройка OpsGenie**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-opsgenie-single-sign-on"></a>Настройка единого входа в OpsGenie

1. Откройте другое окно браузера и войдите в OpsGenie с правами администратора.

2. Щелкните **Параметры** и откройте вкладку **Единый вход**.
   
    ![Единый вход в OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_06.png)

3. Чтобы включить единый вход, установите флажок **Включено**.
   
    ![Параметры OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_07.png) 

4. В разделе **Поставщик** откройте вкладку **Azure Active Directory**.
   
    ![Параметры OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_08.png) 

5. На странице диалогового окна Azure Active Directory выполните следующие действия:
   
    ![Параметры OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_09.png)
    
    a. В текстовое поле **SAML 2.0 Endpoint** (Конечная точка SAML 2.0) вставьте значение **URL-адреса входа**, скопированное на портале Azure.
    
    b. В текстовое поле **URL-адрес метаданных** вставьте значение **URL-адреса метаданных федерации приложений**, скопированное на портале Azure.
    
    c. Нажмите кнопку **Сохранить изменения**.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD 

Цель этого раздела — создать на портале Azure тестового пользователя с именем B. Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В разделе свойств пользователя сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    a. В поле **Имя** введите **B. Simon**.
  
    b. В поле **Имя пользователя** введите **bsimon@yourcompanydomain.extension** .  
    Например BSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к OpsGenie.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **OpsGenie**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. Из списка приложений выберите **OpsGenie**.

    ![Ссылка на OpsGenie в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** выберите **B. Simon** из списка пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-opsgenie-test-user"></a>Создание тестового пользователя OpsGenie

Цель этого раздела — создать пользователя с именем Britta Simon в OpsGenie. 

1. В окне веб-браузера войдите в клиент OpsGenie с правами администратора.

2. Перейдите к списку пользователей, щелкнув **Пользователь** на левой панели.
   
    ![Параметры OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_10.png) 

3. Нажмите кнопку **Add User**(Добавить пользователя).

4. На странице **Добавление пользователя** выполните следующие действия.
   
    ![Параметры OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_11.png)
   
    a. В текстовом поле **Email** (Адрес электронной почты) введите адрес электронной почты пользователя B. Simon в Azure Active Directory.
   
    b. В текстовом поле **Full Name** (Полное имя) введите **B. Simon**.
   
    c. Выберите команду **Сохранить**. 

>[!NOTE]
>B. Пользователь Simon получит электронное сообщение с инструкциями по настройке профиля.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку OpsGenie на Панели доступа, вы автоматически войдете в приложение OpsGenie, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

