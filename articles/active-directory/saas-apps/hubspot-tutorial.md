---
title: Руководство по Интеграция Azure Active Directory с HubSpot | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и HubSpot.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.openlocfilehash: 51e27526b10bcdd74d6e2bffb8bf620d7b022aac
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88551478"
---
# <a name="tutorial-azure-active-directory-integration-with-hubspot"></a>Руководство по Интеграция Azure Active Directory с HubSpot

Цель этого руководства — показать, как интегрировать Azure Active Directory (Azure AD) с приложением HubSpot.

Интеграция HubSpot с Azure AD обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к HubSpot.
* Пользователи могут автоматически входить в HubSpot с помощью учетных записей Azure AD (единый вход).
* Вы можете управлять учетными записями централизованно, через портал Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье о [едином входе в приложения в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с HubSpot, вам потребуется:

* Подписка Azure AD. Если у вас еще нет подписки Azure AD, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.
* подписка HubSpot с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде и интеграцию HubSpot с Azure AD.

HubSpot поддерживает следующие функции:

* **единый вход, инициированный поставщиком службы**;
* **единый вход, инициированный поставщиком удостоверений**;

## <a name="add-hubspot-in-the-azure-portal"></a>Добавление HubSpot на портале Azure

Чтобы интегрировать HubSpot с Azure AD, необходимо добавить HubSpot в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com).

1. В меню слева выберите **Azure Active Directory**.

    ![Пункт Azure Active Directory](common/select-azuread.png)

1. Щелкните **Корпоративные приложения** > **Все приложения**.

    ![Область "Корпоративные приложения"](common/enterprise-applications.png)

1. Чтобы добавить приложение, выберите **Новое приложение**.

    ![Элемент "Новое приложение"](common/add-new-app.png)

1. В поле поиска введите **HubSpot**. В результатах поиска выберите **HubSpot**, а затем щелкните **Добавить**.

    ![HubSpot в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в HubSpot с использованием тестового пользователя **Britta Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в HubSpot.

Чтобы настроить и проверить единый вход Azure AD в HubSpot, потребуется выполнить действия в следующих стандартных блоках.

| Задача | Описание |
| --- | --- |
| **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** | Предоставление пользователям возможности использовать эту функцию. |
| **[Настройка единого входа в HubSpot](#configure-hubspot-single-sign-on)** . | Настройка параметров единого входа в приложении. |
| **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** | Проверка работы единого входа Azure AD на примере пользователя Britta Simon. |
| **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** | Предоставление пользователю Britta Simon возможности использовать единый вход Azure AD. |
| **[Создание тестового пользователя HubSpot](#create-a-hubspot-test-user)** . | Требуется, чтобы в HubSpot существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD. |
| **[Проверка единого входа](#test-single-sign-on)** | необходима, чтобы убедиться, что конфигурация работает правильно. |

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описывается настройка единого входа Azure AD в HubSpot на портале Azure.

1. На [портале Azure](https://portal.azure.com/) в области интеграции с приложением **HubSpot** выберите **Единый вход**.

    ![Параметр для настройки единого входа](common/select-sso.png)

1. На панели **Выбрать метод единого входа** выберите режим **SAML** или **SAML/WS-FED**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

1. На панели **Настройка единого входа с помощью SAML** щелкните **Изменить** (значок карандаша), чтобы открыть панель **Базовая конфигурация SAML**.

    ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. Чтобы настроить *режим, инициируемый поставщиком удостоверений*, в области **Базовая конфигурация SAML** выполните приведенные ниже действия.

    1. В поле **Идентификатор** введите URL-адрес в формате https:\//api.hubspot.com/login-api/v1/saml/login?portalId=\<CUSTOMER ID\>.

    1. В поле **URL-адрес ответа** введите URL-адрес в формате https:\//api.hubspot.com/login-api/v1/saml/acs?portalId=\<CUSTOMER ID\>.

    ![Сведения о домене и URL-адресах единого входа для HubSpot](common/idp-intiated.png)

    > [!NOTE]
    > Можно также посмотреть шаблоны форматирования URL-адресов в области **Базовая конфигурация SAML** на портале Azure.

1. Чтобы настроить приложение в режиме, *инициируемом поставщиком служб*, выполните следующие действия.

    1. Щелкните **Задать дополнительные URL-адреса**.

    1. В поле **URL-адрес для входа** введите **https:\//app.hubspot.com/login**.

    ![Параметр "Задать дополнительные URL-адреса"](common/metadata-upload-additional-signon.png)

1. В области **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** нажмите кнопку **Скачать**, расположенную рядом с элементом **Сертификат (Base64)** . Выберите вариант для скачивания в соответствии с вашими требованиями. Сохраните сертификат на своем компьютере.

    ![Параметр для скачивания сертификата в кодировке Base64](common/certificatebase64.png)

1. Требуемые URL-адреса можно скопировать из раздела **Настройка HubSpot**.

    * URL-адрес входа.
    * Идентификатор Azure AD
    * URL-адрес выхода.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

### <a name="configure-hubspot-single-sign-on"></a>Настройка единого входа в HubSpot

1. Откройте новую вкладку в браузере и войдите в учетную запись администратора HubSpot.

1. Выберите значок **Settings** (Параметры) в правом верхнем углу страницы.

    ![Значок "Settings" (Параметры) в HubSpot](./media/hubspot-tutorial/config1.png)

1. Щелкните **Account Defaults** (Параметры учетной записи по умолчанию).

    ![Параметр "Account Defaults" (Параметры учетной записи по умолчанию) в HubSpot](./media/hubspot-tutorial/config2.png)

1. Прокрутите вниз до раздела **Security** (Безопасность) и щелкните **Set up** (Настроить).

    ![Параметр "Set up" (Настроить) в HubSpot](./media/hubspot-tutorial/config3.png)

1. В разделе **Set up single sign-on** (Настройка единого входа) сделайте следующее.

    1. В поле **Audience URl (Service Provider Entity ID)** (URl аудитории (идентификатор сущности поставщика услуг)) щелкните **Copy** (Копировать), чтобы скопировать значение. На портале Azure в области **Базовая конфигурация SAML** вставьте это значение в поле **Идентификатор**.

    1. В поле **Sign on URl, ACS, Recipient, or Redirect** (URl для входа, ACS, получатель или адрес перенаправления), щелкните **Copy** (Копировать), чтобы скопировать значение. На портале Azure в области **Базовая конфигурация SAML** вставьте это значение в поле **URL-адрес ответа**.

    1. В HubSpot в поле **Identity Provider Identifier or Issuer URL** (Идентификатор поставщика удостоверений или URL-адрес издателя) вставьте значение **Идентификатор Azure AD**, скопированное на портале Azure.

    1. В HubSpot в поле **Identity Provider Single Sign-On URL** (URL-адрес единого входа поставщика удостоверений) вставьте значение **URL-адрес входа**, скопированное на портале Azure.

    1. В Блокноте Windows откройте скачанный файл сертификата формата Base64. Выделите и скопируйте содержимое файла. Затем в HubSpot вставьте его в поле **X.509 Certificate** (Сертификат X.509).

    1. Нажмите кнопку **Проверить**.

        ![Раздел "Set up single sign-on" (Настройка единого входа) в HubSpot](./media/hubspot-tutorial/config4.png)

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure выберите **Azure Active Directory** > **Пользователи** > **Все пользователи**.

    ![Пункты меню "Пользователи" и "Все пользователи"](common/users.png)

1. Выберите **Новый пользователь**.

    ![Элемент "Новый пользователь"](common/new-user.png)

1. В области **Пользователь** выполните приведенные ниже действия.

    1. В поле **Имя** введите **BrittaSimon**.
  
    1. В поле **Имя пользователя** введите **brittasimon\@\<your-company-domain>.\<extension\>** . Например, **brittasimon\@contoso.com**.

    1. Установите флажок **Показать пароль**. Запишите значение, которое отображается в поле **Пароль**.

    1. Нажмите кнопку **создания**.

    ![Панель "Пользователь"](common/user-properties.png)

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе вы предоставите пользователю Britta Simon доступ к HubSpot, что позволит этому пользователю использовать единый вход Azure.

1. На портале Azure выберите **Корпоративные приложения** > **Все приложения** > **HubSpot**.

    ![Область "Корпоративные приложения"](common/enterprise-applications.png)

1. В списке приложений выберите **HubSpot**.

    ![HubSpot в списке приложений](common/all-applications.png)

1. В меню выберите **Пользователи и группы**.

    ![Пункт меню "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**. Затем в области **Добавление назначения** щелкните **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

1. На панели **Пользователи и группы** в списке пользователей выберите **Britta Simon**. Щелкните **Выбрать**.

1. Если вы планируете получать значение роли в утверждении SAML, выберите на панели **Выбор роли** подходящую роль для пользователя из предложенного списка. Щелкните **Выбрать**.

1. В области **Добавление назначения** выберите **Назначить**.

### <a name="create-a-hubspot-test-user"></a>Создание тестового пользователя HubSpot

Чтобы пользователь Azure AD мог войти в HubSpot, его нужно подготовить к работе в HubSpot. В HubSpot подготовка к работе выполняется вручную.

Чтобы подготовить к работе учетную запись пользователя в HubSpot, выполните следующие действия

1. Войдите на свой корпоративный веб-сайт HubSpot с правами администратора.

1. Выберите значок **Settings** (Параметры) в правом верхнем углу страницы.

    ![Значок "Settings" (Параметры) в HubSpot](./media/hubspot-tutorial/config1.png)

1. Выберите **Users & Teams** (Пользователи и группы).

    ![Параметр "Users & Teams" (Пользователи и команды) в HubSpot](./media/hubspot-tutorial/user1.png)

1. Щелкните **Create user** (Создать пользователя).

    ![Параметр "Create user" (Создать пользователя) в HubSpot](./media/hubspot-tutorial/user2.png)

1. В поле **Add email addess(es)** (Добавьте адреса электронной почты) введите адрес электронной почты пользователя в формате brittasimon\@contoso.com, а затем щелкните **Next** (Далее).

    ![Поле "Add email address(es)" (Добавьте адреса электронной почты) в разделе "Create users" (Создание пользователей) в HubSpot](./media/hubspot-tutorial/user3.png)

1. В разделе **Create users** (Создание пользователей) по очереди откройте все вкладки. На каждой вкладке задайте соответствующие параметры и разрешения для пользователя. Затем нажмите кнопку **Далее**.

    ![Вкладки в разделе "Create user" (Создание пользователя) в HubSpot](./media/hubspot-tutorial/user4.png)

1. Щелкните **Send** (Отправить), чтобы отправить приглашение пользователю.

    ![Параметр "Send" (Отправить) в HubSpot](./media/hubspot-tutorial/user5.png)

    > [!NOTE]
    > После того, как пользователь примет приглашение, его учетная запись будет активирована.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе объясняется, как проверить конфигурацию единого входа Azure AD с помощью портала "Мои приложения".

После настройки единого входа вы сможете автоматически войти в приложение HubSpot, выбрав **HubSpot** на портале "Мои приложения". Дополнительные сведения о портале "Мои приложения" см. в статье [Access and use apps on the My Apps portal](../user-help/my-apps-portal-end-user-access.md) (Доступ к приложениям и их использование на портале "Мои приложения").

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в статьях ниже:

- [Список руководств по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
