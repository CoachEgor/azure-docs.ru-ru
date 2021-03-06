---
title: Руководство по интеграции Azure Active Directory с Andromeda | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Andromeda.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.openlocfilehash: e8cb939b48f8cfe311ec10c0850cfb234de04fad
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/16/2020
ms.locfileid: "97589786"
---
# <a name="tutorial-azure-active-directory-integration-with-andromeda"></a>Руководство по интеграции Azure Active Directory с Andromeda

Из этого руководства вы узнаете, как интегрировать Andromeda с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением Andromeda обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к Andromeda.
* Вы можете включить автоматический вход пользователей в Andromeda (единый вход) с помощью учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](../manage-apps/what-is-single-sign-on.md).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>предварительные требования

Чтобы настроить интеграцию Azure AD с Andromeda, вам потребуется:

* подписка Azure AD; (если у вас нет среды Azure AD, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/));
* подписка Andromeda с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Andromeda поддерживает вход, инициируемый **поставщиком услуг и поставщиком удостоверений**.
* Andromeda поддерживает **JIT**-подготовку пользователей.

## <a name="adding-andromeda-from-the-gallery"></a>Добавление Andromeda из коллекции.

Чтобы настроить интеграцию Andromeda с Azure AD, необходимо добавить Andromeda из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Andromeda из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Andromeda**. На панели результатов выберите **Andromeda** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Andromeda в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в приложение Andromeda с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Andromeda.

Чтобы настроить и проверить единый вход Azure AD в Andromeda, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Andromeda](#configure-andromeda-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Andromeda](#create-andromeda-test-user)** нужно для того, чтобы в Andromeda существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Andromeda, сделайте следующее.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Andromeda** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** выполните следующие действия.

    ![Снимок экрана, на котором показан раздел "Базовая конфигурация SAML", где можно ввести идентификатор, URL-адрес ответа и нажать кнопку "Сохранить"](common/idp-intiated.png)

    а. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://<tenantURL>.ngcxpress.com/`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<tenantURL>.ngcxpress.com/SAMLConsumer.aspx`.

5. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    ![Снимок экрана, на котором показан параметр "Задать дополнительные URL-адреса" и поле для ввода URL-адреса входа](common/metadata-upload-additional-signon.png)

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://<tenantURL>.ngcxpress.com/SAMLLogon.aspx`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените их на фактические значения идентификатора, URL-адреса ответа и URL-адреса входа, которые описываются далее в этом руководстве.

6. Приложение Andromeda ожидает утверждения SAML в определенном формате. Настройте следующие утверждения для этого приложения. Управлять значениями этих атрибутов можно в разделе **Атрибуты пользователя** на странице интеграции приложения. На странице **Настройка единого входа с помощью SAML** нажмите кнопку **Изменить**, чтобы открыть диалоговое окно **Атрибуты пользователя**.

    ![Снимок экрана: раздел "Атрибуты пользователя" с такими атрибутами, как "givenname: user.givenname" и "emailaddress: user.mail".](common/edit-attribute.png)

    > [!Important]
    > Очистите определения пространств имен при настройке.

7. В разделе **Утверждения пользователя** диалогового окна **Атрибуты пользователя** измените утверждения, щелкнув значок **Изменить**, или добавьте утверждение, нажав кнопку **Добавить новое утверждение**, чтобы настроить атрибут токена SAML, как показано на рисунке выше, и выполните следующие действия. 

    | Имя | Исходный атрибут|
    | ------ | -----------|
    | роль | Определенная роль для приложения |
    | type | Тип приложения |
    | company | CompanyName |

    > [!NOTE]
    > Значения, указанные выше, приведены в качестве примера. Эти значения представлены исключительно для демонстрации. Используйте роли своей организации.

    1. Щелкните **Добавить новое утверждение**, чтобы открыть диалоговое окно **Управление утверждениями пользователя**.

        ![Снимок экрана: раздел "Утверждения пользователя" с кнопками "Добавить новое утверждение" и "Сохранить".](common/new-save-attribute.png)

        ![Снимок экрана: раздел "Управление утверждениями пользователя", где можно ввести описанные на этом шаге значения.](common/new-attribute-details.png)

    1. В текстовом поле **Имя** введите имя атрибута, отображаемое для этой строки.

    1. Оставьте пустым поле **Пространство имен**.

    1. В качестве источника выберите **Атрибут**.

    1. В списке **Атрибут источника** введите значение атрибута, отображаемое для этой строки.

    1. Нажмите кнопку **ОК**.

    1. Выберите команду **Сохранить**.

8. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

9. Требуемые URL-адреса можно скопировать из раздела **Настройка Andromeda**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    1. URL-адрес входа.

    1. Идентификатор Azure AD

    1. URL-адрес выхода.

### <a name="configure-andromeda-single-sign-on"></a>Настройка единого входа в Andromeda

1. Войдите на сайт компании Andromeda от имени администратора.

2. В верхней части строки меню щелкните **Admin** (Администратор) и выберите **Administration** (Администрирование).

    ![Администратор Andromeda](./media/andromedascm-tutorial/tutorial_andromedascm_admin.png)

3. Слева в разделе **Interfaces** (Интерфейсы) щелкните **SAML Configuration** (Настройка SAML).

    ![SAML Andromeda](./media/andromedascm-tutorial/tutorial_andromedascm_saml.png)

4. На странице раздела **SAML Configuration** (Настройка SAML) сделайте следующее:

    ![Конфигурация Andromeda](./media/andromedascm-tutorial/tutorial_andromedascm_config.png)

    1. Установите флажок **Enable SSO with SAML** (Включить единый вход с помощью SAML).

    1. В разделе **Andromeda Information** (Сведения о приложении Andromeda) скопируйте значение **SP Identity** (Идентификатор удостоверения поставщика услуг) и вставьте его в текстовое поле **Идентификатор** раздела **Базовая конфигурация SAML**.

    1. Скопируйте значение **Consumer URL** (URL-адрес объекта-получателя) и вставьте его в текстовое поле **URL-адрес ответа** раздела **Базовая конфигурация SAML**.

    1. Скопируйте значение **Logon URL** (URL-адрес входа) и вставьте его в текстовое поле **URL-адрес входа** раздела **Базовая конфигурация SAML**.

    1. В разделе **SAML Identity Provider** (Поставщик удостоверений SAML) введите имя поставщика удостоверений.

    1. В текстовое поле **Single Sign On End Point** (Конечная точка единого входа) вставьте значение **URL-адрес входа**, скопированное на портале Azure.

    1. Откройте в Блокноте **сертификат в кодировке Base64**, загруженный с портала Azure, и вставьте его в текстовое поле **Сертификат X 509**.

    1. Сопоставьте следующие атрибуты с соответствующим значением для выполнения единого входа через Azure AD. Для входа требуется атрибут **User ID** (Идентификатор пользователя). Для подготовки необходимо указать атрибуты: **Email** (Электронный адрес), **Company** (Компания), **UserType** (Тип пользователя) и **Role** (Роль). В этом разделе определяются сопоставления атрибутов (имя и значения), которые соответствуют указанным на портале Azure.

        ![Сопоставление атрибутов в Andromeda](./media/andromedascm-tutorial/tutorial_andromedascm_attbmap.png)

    1. Выберите команду **Сохранить**.

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

Из этого раздела вы узнаете, как предоставить пользователю Britta Simon доступ к Andromeda, чтобы он мог использовать единый вход Azure.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Andromeda**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Andromeda**.

    ![Ссылка на Andromeda в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-andromeda-test-user"></a>Создание тестового пользователя Andromeda

В этом разделе вы создадите в Andromeda пользователя Britta Simon. Приложение Andromeda поддерживает JIT-подготовку пользователей, которая включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в Andromeda, он создается после выполнения аутентификации. Чтобы создать учетную запись пользователя вручную, обратитесь в [службу поддержки Andromeda](https://www.ngcsoftware.com/support/).

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "Andromeda" на Панели доступа, вы автоматически войдете в приложение Andromeda, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](./tutorial-list.md)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Что представляет собой условный доступ в Azure Active Directory?](../conditional-access/overview.md)