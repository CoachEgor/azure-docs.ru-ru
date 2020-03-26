---
title: Руководство по Интеграция Azure Active Directory с SAML SSO for Confluence by resolution GmbH | Документация Майкрософт
description: Узнайте, как настроить единый вход для Azure Active Directory и SAML SSO for Confluence by resolution GmbH.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 6b47d483-d3a3-442d-b123-171e3f0f7486
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9676a71940569b26d6b0b6bfef767108ae57d953
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "73161219"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-confluence-by-resolution-gmbh"></a>Руководство по Интеграция Azure Active Directory с SAML SSO for Confluence by resolution GmbH

В этом руководстве описано, как интегрировать SAML SSO for Confluence by resolution GmbH с Azure Active Directory (Azure AD).
Интеграция SAML SSO for Confluence by resolution GmbH с Azure AD предоставляет следующие преимущества.

* Вы можете управлять доступом пользователей Azure AD к SAML SSO for Confluence by resolution GmbH.
* Вы можете включить автоматический вход пользователей в SAML SSO for Confluence by resolution GmbH (единый вход) с помощью учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Для настройки интеграции Azure AD с SAML SSO for Confluence by resolution GmbH требуется:

* Подписка Azure AD. (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* Подписка SAML SSO for Confluence by resolution GmbH с поддержкой единого входа

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* SAML SSO for Confluence by resolution GmbH поддерживает единый вход инициированного **пакета обновления** и **выдающей точки распространения**

## <a name="adding-saml-sso-for-confluence-by-resolution-gmbh-from-the-gallery"></a>Добавление SAML SSO for Confluence by resolution GmbH из коллекции

Чтобы настроить интеграцию SAML SSO for Confluence by resolution GmbH в Azure AD, необходимо добавить SAML SSO for Confluence by resolution GmbH из коллекции в список управляемых приложений SaaS.

**Чтобы добавить SAML SSO for Confluence by resolution GmbH из коллекции, выполните следующее.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. Чтобы добавить приложение, в поле поиска введите **SAML SSO for Confluence by resolution GmbH**, затем на панели результатов выберите **SAML SSO for Confluence by resolution GmbH** и нажмите кнопку **Добавить**.

     ![Список результатов SAML SSO for Confluence by resolution GmbH](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в SAML SSO for Confluence by resolution GmbH с использованием тестового пользователя **Britta Simon**.
Чтобы единый вход работал, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в SAML SSO for Confluence by resolution GmbH.

Чтобы настроить и проверить единый вход Azure AD в SAML SSO for Confluence by resolution GmbH, требуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в SAML SSO for Confluence by resolution GmbH](#configure-saml-sso-for-confluence-by-resolution-gmbh-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя SAML SSO for Confluence by resolution GmbH](#create-saml-sso-for-confluence-by-resolution-gmbh-test-user)** требуется, чтобы создать в SAML SSO for Confluence by resolution GmbH пользователя Britta Simon, связанного с соответствующим пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в SAML SSO for Confluence by resolution GmbH, выполните следующее.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **SAML SSO for Confluence by resolution GmbH** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Изменение базовой конфигурации SAML](common/edit-urls.png)

4. Если вы хотите настроить приложение в режиме, инициируемом **выдающейся точкой распространения**, в разделе **Базовая конфигурация SAML** выполните следующие действия.

    ![Информация о едином входе доменов и URL-адресов приложения SAML SSO for Confluence by resolution GmbH](common/idp-intiated.png)

    а. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://<server-base-url>/plugins/servlet/samlsso`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<server-base-url>/plugins/servlet/samlsso`.

    c. Чтобы настроить приложение для работы в режиме, инициируемом пакетом обновления, щелкните **Задать дополнительные URL-адреса** и выполните следующее действие.

    ![Информация о едином входе доменов и URL-адресов приложения SAML SSO for Confluence by resolution GmbH](common/metadata-upload-additional-signon.png)

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://<server-base-url>/plugins/servlet/samlsso`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените их фактическими значениями идентификатора, URL-адреса ответа и URL-адреса входа. Обратитесь к [группе поддержки SAML SSO for Confluence by resolution GmbH](https://www.resolution.de/go/support) для получения этих значений. Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

4. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать нужный вам **XML метаданных федерации**, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

### <a name="configure-saml-sso-for-confluence-by-resolution-gmbh-single-sign-on"></a>Настройка единого входа для SAML SSO for Confluence by resolution GmbH

1. В другом окне браузера войдите на **портал администрирования SAML SSO for Confluence by resolution GmbH** с правами администратора.

2. Наведите указатель мыши на шестеренку и щелкните **Add-ons** (Надстройки).
    
    ![Настройка единого входа](./media/samlssoconfluence-tutorial/addon1.png)

3. Вы перейдете на страницу доступа с правами администратора. Введите пароль и нажмите кнопку **Confirm** (Подтвердить).

    ![Настройка единого входа](./media/samlssoconfluence-tutorial/addon2.png)

4. В разделе **ATLASSIAN MARKETPLACE** щелкните **Find new add-ons** (Найти новые надстройки). 

    ![Настройка единого входа](./media/samlssoconfluence-tutorial/addon.png)

5. Найдите подключаемый модуль **SAML Single Sign On (SSO) for Confluence** и нажмите кнопку **Install** (Установить), чтобы установить новый подключаемый модуль SAML.

    ![Настройка единого входа](./media/samlssoconfluence-tutorial/addon7.png)

6. Начнется установка подключаемого модуля. Щелкните **Закрыть**.

    ![Настройка единого входа](./media/samlssoconfluence-tutorial/addon8.png)

    ![Настройка единого входа](./media/samlssoconfluence-tutorial/addon9.png)

7.  Нажмите кнопку **Управление**.

    ![Настройка единого входа](./media/samlssoconfluence-tutorial/addon10.png)
    
8. Щелкните **Configure** (Настройка), чтобы настроить новый подключаемый модуль.

    ![Настройка единого входа](./media/samlssoconfluence-tutorial/addon11.png)

9. Этот новый подключаемый модуль можно также найти на вкладке **USERS & SECURITY** (Пользователи и безопасность).

    ![Настройка единого входа](./media/samlssoconfluence-tutorial/addon3.png)
    
10. На странице **SAML SingleSignOn Plugin Configuration** (Конфигурация подключаемого модуля единого входа SAML) нажмите кнопку **Add new IdP** (Добавить новый поставщик удостоверений), чтобы настроить параметры поставщика удостоверений.

    ![Настройка единого входа](./media/samlssoconfluence-tutorial/addon4.png)

11. На странице **Choose your SAML Identity Provider** (Выбор поставщика удостоверений SAML) выполните следующие действия:

    ![Настройка единого входа](./media/samlssoconfluence-tutorial/addon5a.png)
 
    а. Для типа поставщика удостоверений выберите значение **Azure AD**.
    
    b. Добавьте **имя** поставщика удостоверений (например, Azure AD).
    
    c. Добавьте **описание** поставщика удостоверений (например, Azure AD).
    
    d. Щелкните **Далее**.
    
12. На странице **Identity provider configuration** (Настройка поставщика удостоверений) нажмите кнопку **Next** (Далее).

    ![Настройка единого входа](./media/samlssoconfluence-tutorial/addon5b.png)

13. На странице **Import SAML IdP Metadata** (Импорт метаданных поставщика удостоверений SAML) выполните следующие действия:

    ![Настройка единого входа](./media/samlssoconfluence-tutorial/addon5c.png)

    а. Нажмите кнопку **Load File** (Загрузить файл) и выберите XML-файл метаданных, который вы скачали на шаге 5.

    b. Нажмите кнопку **Import** (Импортировать).
    
    c. Дождитесь завершения импорта.
    
    d. Нажмите кнопку **Next** (Далее).
    
14. На странице **User ID attribute and transformation** (Атрибут и преобразование идентификатора пользователя) нажмите кнопку **Next** (Далее).

    ![Настройка единого входа](./media/samlssoconfluence-tutorial/addon5d.png)
    
15. На странице **User creation and update** (Создание и изменение пользователя) нажмите кнопку **Save & Next** (Сохранить и продолжить), чтобы сохранить параметры.   
    
    ![Настройка единого входа](./media/samlssoconfluence-tutorial/addon6a.png)
    
16. На странице **Test your settings** (Проверка параметров) нажмите кнопку **Skip test & configure manually** (Пропустить проверку и настроить вручную), чтобы не выполнять проверку на этом этапе. Проверка будет выполнена на одном из следующих этапов, и для этого потребуется выполнить некоторые настройки на портале Azure. 
    
    ![Настройка единого входа](./media/samlssoconfluence-tutorial/addon6b.png)
    
17. В открывшемся окне с сообщением **Skipping the test means...** (Если вы пропустите проверку...) нажмите кнопку **OK**.
    
    ![Настройка единого входа](./media/samlssoconfluence-tutorial/addon6c.png)

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

В этом разделе описано, как позволить пользователю Britta Simon использовать единый вход Azure путем предоставления доступа к SAML SSO for Confluence by resolution GmbH.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **SAML SSO for Confluence by resolution GmbH**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. Из списка приложений введите и выберите **SAML SSO for Confluence by resolution GmbH**.

    ![Список приложений SAML SSO for Confluence by resolution GmbH](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-saml-sso-for-confluence-by-resolution-gmbh-test-user"></a>Создание тестового пользователя SAML SSO for Confluence by resolution GmbH

Чтобы пользователи Azure AD могли входить в SAML SSO for Confluence by resolution GmbH, их необходимо подготовить в SAML SSO for Confluence by resolution GmbH.  
Подготовка в SAML SSO for Confluence by resolution GmbH выполняется вручную.

**Чтобы подготовить учетную запись пользователя, сделайте следующее:**

1. Войдите на свой корпоративный сайт SAML SSO for Confluence by resolution GmbH с правами администратора.

2. Наведите указатель мыши на шестеренку и щелкните **User management** (Управление пользователями).

    ![Добавление сотрудника](./media/samlssoconfluence-tutorial/user1.png) 

3. В разделе "Users" (Пользователи) выберите вкладку **Add users** (Добавление пользователей). На диалоговой странице **Add a User** (Добавление пользователя) выполните следующее.

    ![Добавление сотрудника](./media/samlssoconfluence-tutorial/user2.png) 

    а. В текстовом поле **Username** (Имя пользователя) введите электронный адрес пользователя, например Britta Simon.

    b. В текстовом поле **Full Name** (Полное имя) введите полное имя пользователя, например Britta Simon.

    c. В текстовом поле **Email** (Электронная почта) введите адрес электронной почты пользователя, например Brittasimon@contoso.com.

    d. В текстовом поле **Password** (Пароль) введите пароль пользователя Britta Simon.

    д) Щелкните **Confirm Password** (Подтвердить пароль) и повторно введите пароль.
    
    е) Нажмите кнопку **Добавить**.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "SAML SSO for Confluence by resolution GmbH" на панели доступа, вы должны автоматически войти в SAML SSO for Confluence by resolution GmbH, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

