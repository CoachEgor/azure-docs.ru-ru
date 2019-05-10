---
title: Руководство по Интеграция Azure Active Directory с Atlassian Cloud | Документация Майкрософт
description: Сведения о настройке единого входа Azure Active Directory в приложении Atlassian Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/03/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 66b8b9076c0a4b1fddda4ab0bcfe9f104d7dcf8a
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65191119"
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Руководство по Интеграция Azure Active Directory с Atlassian Cloud

В этом руководстве описано, как интегрировать приложение Atlassian Cloud с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением Atlassian Cloud обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к Atlassian Cloud.
* Вы можете включить автоматический вход для пользователей в Atlassian Cloud (единый вход) с использованием учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с приложением Atlassian Cloud, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/));
* Подписка Atlassian Cloud с поддержкой единого входа.
* Чтобы включить язык разметки заявлений системы безопасности (SAML) для продуктов компании Atlassian Cloud, потребуется настроить Atlassian Access. Дополнительные сведения об [Atlassian Access]( https://www.atlassian.com/enterprise/cloud/identity-manager).

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Atlassian Cloud поддерживает единый вход, инициированный **поставщиком услуг и поставщиком удостоверений**.

## <a name="adding-atlassian-cloud-from-the-gallery"></a>Добавление Atlassian Cloud из коллекции

Чтобы настроить интеграцию Atlassian Cloud с Azure AD, необходимо добавить Atlassian Cloud из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Atlassian Cloud из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Atlassian Cloud**, выберите **Atlassian Cloud** на панели результатов и щелкните **Добавить**, чтобы добавить это приложение.

     ![Atlassian Cloud в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Atlassian Cloud с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Atlassian Cloud.

Чтобы настроить и проверить единый вход Azure AD в Atlassian Cloud, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Atlassian Cloud](#configure-atlassian-cloud-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Atlassian Cloud](#create-atlassian-cloud-test-user)** требуется, чтобы в Atlassian Cloud существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Atlassian Cloud, сделайте следующее:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Atlassian Cloud** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа для [название приложения]](common/idp-relay.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://auth.atlassian.com/saml/<unique ID>`.

    б) В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`.

    c. Щелкните **Задать дополнительные URL-адреса**.

    d. В текстовом поле **Состояние ретранслятора** введите URL-адрес в формате `https://<instancename>.atlassian.net`.

    > [!NOTE]
    > Приведенные выше значения используются только для примера. Замените их фактическими значениями идентификатора и URL-адреса ответа. Вы получите эти значения на экране **настройки SAML для Atlassian Cloud**, который описан в руководстве по **настройке единого входа в Atlassian Cloud**.

5. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа для [название приложения]](common/both-signonurl.png)

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://<instancename>.atlassian.net`.

    > [!NOTE]
    > Приведенное выше значение "URL-адрес для входа" используется только для примера. Для входа укажите фактический URL-адрес. Для получения этого значения обратитесь к [группе поддержки клиентов Atlassian Cloud](https://support.atlassian.com/).

6. Приложение Atlassian Cloud ожидает проверочные утверждения SAML в определенном формате, который требует добавить сопоставления настраиваемых атрибутов в вашу конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию, когда **nameidentifier** сопоставляется с **user.userprincipalname**. Приложение Atlassian Cloud ожидает сопоставления **nameidentifier** с **user.mail**, поэтому необходимо изменить сопоставление атрибутов, щелкнув значок **Edit** (Изменить).

    ![image](common/edit-attribute.png)

7. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

8. Скопируйте требуемый URL-адрес из раздела **Настройка Atlassian Cloud**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-atlassian-cloud-single-sign-on"></a>Настройка единого входа в Atlassian Cloud

1. Для автоматизации настройки в Atlassian Cloud необходимо установить **Расширение браузера "Безопасный вход в мои приложения"**, щелкнув **Установить расширение**.

    ![Расширение "Мои приложения"](common/install-myappssecure-extension.png)

2. После добавления расширения в браузере щелкните **Настроить Atlassian Cloud**, чтобы перейти к приложению Atlassian Cloud. После этого укажите учетные данные администратора для входа в Atlassian Cloud. Расширение браузера автоматически настроит приложение и автоматизирует шаги 3–7.

    ![Параметры настройки](common/setup-sso.png)

3. Если необходимо вручную настроить Atlassian Cloud, откройте новое окно веб-браузера, зайдите на сайт компании Atlassian Cloud в роли администратора и выполните следующие шаги.

4. Перед настройкой единого входа необходимо проверить домен. Дополнительные сведения см. в документе по [проверке домена Atlassian](https://confluence.atlassian.com/cloud/domain-verification-873871234.html).

5. В левой области выберите **SAML single sign-on** (Единый вход SAML). Подпишитесь на Atlassian Identity Manager.

    ![Настройка единого входа](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

6. В окне**Add SAML configuration** (Добавление конфигурации SAML) выполните следующее.

    ![Настройка единого входа](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. В поле **Identity provider Entity ID** (Идентификатор сущности поставщика удостоверений) вставьте **идентификатор Azure AD**, скопированный на портале Azure.

    b. В поле **Identity provider SSO URL** (URL-адрес единого входа поставщика удостоверений) вставьте **URL-адрес входа**, скопированный на портале Azure.

    c. Откройте сертификат, скачанный с портала Azure в TXT-файле, скопируйте значения (без строк *Begin Certificate* и *End Certificate*) и вставьте их в поле **Public X509 certificate** (Общий сертификат X509).

    d. Щелкните **Сохранить конфигурацию**.

7. Чтобы убедиться, что вы настроили правильные URL-адреса, обновите параметры Azure AD, выполнив следующее.

    ![Настройка единого входа](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. В окне SAML скопируйте **идентификатор удостоверения поставщика услуг** и вставьте его в поле **Идентификатор** на портале Azure в разделе **Базовая конфигурация SAML** для Atlassian Cloud.

    b. В окне SAML скопируйте **URL-адрес службы обработчика утверждений поставщика службы** и вставьте его в поле **URL-адрес ответа** на портале Azure в разделе **Базовая конфигурация SAML** для Atlassian Cloud. URL-адрес для входа является URL-адресом клиента приложения Atlassian Cloud.

    > [!NOTE]
    > Если вы уже давно являетесь клиентом, после обновления значений **идентификатора удостоверений поставщика услуг** и **URL-адреса службы обработчика утверждений** на портале Azure выберите **Yes, update configuration** (Да, обновить конфигурацию). Если вы — новый клиент, этот шаг можно пропустить.

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

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure путем предоставления доступа к Atlassian Cloud.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Atlassian Cloud**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений введите и выберите **Atlassian Cloud**.

    ![Ссылка на Atlassian Cloud в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-atlassian-cloud-test-user"></a>Создание тестового пользователя в Atlassian Cloud

Чтобы настроить вход в Atlassian Cloud для пользователей Azure AD, подготовьте учетные записи пользователей вручную в Atlassian Cloud, выполнив следующее.

1. В области **администрирования** выберите **Users** (Пользователи).

    ![Ссылка на пункт Users (Пользователи) в Atlassian Cloud](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png)

2. Чтобы создать пользователя в Atlassian Cloud, выберите **Invite user** (Пригласить пользователя).

    ![Создание пользователя Atlassian Cloud](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png)

3. В поле **Email address** (Адрес электронной почты) введите адрес, а затем назначьте доступ к приложению.

    ![Создание пользователя Atlassian Cloud](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)

4. Чтобы отправить приглашение для пользователя по электронной почте, выберите **Invite users** (Пригласить пользователей). Приглашение по электронной почте отправляется пользователю, и после его принятия пользователь получит доступ к системе.

> [!NOTE]
> Вы также можете создать несколько пользователей, нажав кнопку **Bulk Create** (Массовое создание) в разделе **Users** (Пользователи).

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент Atlassian Cloud на панели доступа, вы автоматически войдете в приложение Atlassian Cloud, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
   
