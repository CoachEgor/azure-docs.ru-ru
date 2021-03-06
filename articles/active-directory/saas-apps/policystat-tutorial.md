---
title: Руководство по Интеграция Azure Active Directory с PolicyStat | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении PolicyStat.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: 4d6274f3e1356a8ff8a997e830f0ebee43f50cff
ms.sourcegitcommit: 4064234b1b4be79c411ef677569f29ae73e78731
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/28/2020
ms.locfileid: "92893162"
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a>Руководство по Интеграция Azure Active Directory с PolicyStat

В этом руководстве описано, как интегрировать PolicyStat с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением PolicyStat обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к PolicyStat.
* Вы можете включить автоматический вход пользователей (единый вход) в PolicyStat с помощью учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](../manage-apps/what-is-single-sign-on.md).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с PolicyStat, вам потребуется:

* Подписка Azure AD. (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка PolicyStat с поддержкой единого входа.

> [!NOTE]
> Эту интеграцию также можно использовать в облачной среде Azure AD для государственных организаций США. Это приложение можно найти в коллекции облачных приложений с поддержкой Azure AD для государственных организаций США и настроить таким же образом, как и в общедоступном облаке.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* PolicyStat поддерживает единый вход, инициированный **поставщиком услуг**.

* PolicyStat поддерживает **JIT** -подготовку пользователей.

## <a name="adding-policystat-from-the-gallery"></a>Добавление PolicyStat из коллекции

Чтобы настроить интеграцию PolicyStat с Azure AD, необходимо добавить PolicyStat из коллекции в список управляемых приложений SaaS.

**Чтобы добавить PolicyStat из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **PolicyStat** , выберите **PolicyStat** на панели результатов и нажмите кнопку **Добавить** , чтобы добавить это приложение.

     ![PolicyStat в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в PolicyStat с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в PolicyStat.

Чтобы настроить и проверить единый вход Azure AD в PolicyStat, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в PolicyStat](#configure-policystat-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
4. **[Создание тестового пользователя PolicyStat](#create-policystat-test-user)** требуется для того, чтобы в PolicyStat существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
5. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в PolicyStat, выполните следующие действия:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **PolicyStat** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed** , чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить** , чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Изменение базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа для приложения PolicyStat](common/sp-identifier.png)

    а. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<companyname>.policystat.com`.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://<companyname>.policystat.com/saml2/metadata/`.

    > [!NOTE]
    > Эти значения приведены для примера. Необходимо обновить эти значения действующим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь к [группе поддержки клиентов PolicyStat](http://www.policystat.com/support/). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

4. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать** , чтобы скачать нужный вам **XML метаданных федерации** , и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

5. Приложение PolicyStat ожидает проверочные утверждения SAML в определенном формате, который требует добавить настраиваемые сопоставления атрибутов в вашу конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию. Нажмите кнопку **Изменить** , чтобы открыть диалоговое окно **Атрибуты пользователя**.

    ![Снимок экрана, на котором показано диалоговое окно "Атрибуты пользователя" с выбранным значком "Изменить".](common/edit-attribute.png)

6. В дополнение к описанному выше приложение PolicyStat ожидает несколько дополнительных атрибутов в ответе SAML. В разделе **Утверждения пользователя** диалогового окна **Атрибуты пользователя** выполните следующие действия, чтобы добавить атрибут токена SAML, как показано в приведенной ниже таблице.

    | Имя | Исходный атрибут |
    |------------------- | -------------------- |
    | uid | ExtractMailPrefix([mail]) |

    а. Щелкните **Добавить новое утверждение** , чтобы открыть диалоговое окно **Управление утверждениями пользователя**.
    
    ![Снимок экрана, на котором показан раздел "Утверждения пользователя" с выбранными действиями "Добавить новое утверждение" и "Сохранить"](common/new-save-attribute.png)

    ![Снимок экрана: диалоговое окно "Управление утверждениями пользователя" с выделенными текстовыми полями "Имя", "Преобразование" и "Параметр", а также с выделенной кнопкой "Сохранить".](./media/policystat-tutorial/attribute01.png)

    b. В текстовом поле **Имя** введите имя атрибута, отображаемое для этой строки.

    c. Оставьте пустым поле **Пространство имен**.

    d. В качестве источника выберите **преобразование**.

    д) В списке **Преобразование** выберите значение атрибута, отображаемое для этой строки.
    
    е) В списке **Параметр 1** введите значение атрибута, отображаемое для этой строки.

    ж. Выберите команду **Сохранить**.

7. Скопируйте требуемый URL-адрес из раздела **Настройка PolicyStat**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а. URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-policystat-single-sign-on"></a>Настройка единого входа в PolicyStat

1. В другом окне веб-браузера войдите на сайт PolicyStat своей компании в качестве администратора.

2. Щелкните вкладку **Admin** (Администрирование), а затем щелкните **Single Sign-On Configuration** (Конфигурация единого входа) в левой области навигации.
   
    ![Меню "Администратор"](./media/policystat-tutorial/ic808633.png "Меню «Администратор»")

3. Щелкните **Your IDP Metadata** (Метаданные вашего поставщика удостоверений), а затем в разделе **Your IDP Metadata** (Метаданные вашего поставщика удостоверений) сделайте следующее.
   
    ![Снимок экрана, на котором выбрано действие Your IDP Metadata (Метаданные вашего поставщика удостоверений).](./media/policystat-tutorial/ic808636.png "Конфигурация единого входа")
   
    а. Откройте скачанный файл метаданных, скопируйте его содержимое и вставьте его в текстовое поле **Your Identity Provider Metadata** (Метаданные поставщика удостоверений).

    b. Щелкните **Сохранить изменения**.

4. Щелкните **Configure Attributes** (Настроить атрибуты), а затем в разделе **Configure Attributes** (Настройка атрибутов) сделайте следующее.
   
    а. В текстовом поле **Username Attribute** (Атрибут имени пользователя) введите значение **uid**.

    b. В текстовое поле **First Name Attribute** (Атрибут имени) введите имя утверждения атрибута имени из Azure **`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`** .

    c. В текстовое поле **Last Name Attribute** (Атрибут фамилии) введите имя утверждения атрибута фамилии из Azure **`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`** .

    d. В текстовое поле **Email Attribute** (Атрибут адреса электронной почты) введите имя утверждения атрибута адреса электронной почты из Azure **`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`** .

    д) Щелкните **Сохранить изменения**.

5. В разделе **Setup** (Настройка) установите флажок **Enable Single Sign-on Integration** (Включить интеграцию единого входа).
   
    ![Конфигурация единого входа](./media/policystat-tutorial/ic808634.png "Конфигурация единого входа")


### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить своей учетной записи использовать единый вход Azure, предоставив доступ к PolicyStat.

1. На портале Azure выберите **Корпоративные приложения** , **Все приложения** , а затем — **PolicyStat**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. Из списка приложений выберите **PolicyStat**.

    ![Ссылка на PolicyStat в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя** , а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** выберите свою учетную запись в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать** , расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-policystat-test-user"></a>Создание тестового пользователя в PolicyStat

В этом разделе вы создадите в PolicyStat пользователя Britta Simon. Приложение PolicyStat поддерживает JIT-подготовку пользователей, которая включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в PolicyStat, он создается после проверки подлинности.

>[!NOTE]
>Вы можете использовать любые другие инструменты создания учетных записей пользователя PolicyStat или API, предоставляемые PolicyStat для подготовки учетных записей пользователя Azure Active Directory.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку PolicyStat на Панели доступа, вы автоматически войдете в приложение PolicyStat, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](./tutorial-list.md)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Что представляет собой условный доступ в Azure Active Directory?](../conditional-access/overview.md)