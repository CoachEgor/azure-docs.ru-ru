---
title: Руководство по интеграции единого входа Azure Active Directory с VeraSMART | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и VeraSMART.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/07/2020
ms.author: jeedes
ms.openlocfilehash: 9412cdcb820f451df444dc2f30a4f538bc287b01
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/24/2020
ms.locfileid: "92517707"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-verasmart"></a>Руководство по интеграции единого входа Azure Active Directory с VeraSMART

В этом учебнике описано, как интегрировать VeraSMART с Azure Active Directory (Azure AD). Интеграция VeraSMART с Azure AD обеспечивает следующие возможности.

* Контроль доступа к VeraSMART с помощью Azure AD.
* Вы можете включить автоматический вход пользователей в VeraSMART с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка VeraSMART с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* VeraSMART поддерживает единый вход, инициированный **пакетом обновления и IDP** .
* VeraSMART поддерживает **JIT** -подготовку пользователей.
* После настройки VeraSMART вы можете применить функцию управления сеансом, которая в реальном времени защищает конфиденциальные данные вашей организации от кражи и несанкционированного доступа. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-verasmart-from-the-gallery"></a>Добавление VeraSMART из коллекции

Чтобы настроить интеграцию приложения VeraSMART с Azure AD, вам нужно добавить VeraSMART из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory** .
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения** .
1. Чтобы добавить новое приложение, выберите **Новое приложение** .
1. В разделе **Добавление из коллекции** в поле поиска введите **VeraSMART** .
1. Выберите **VeraSMART** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-sso-for-verasmart"></a>Настройка и проверка единого входа Azure AD для VeraSMART

Настройте и проверьте единый вход Azure AD в VeraSMART с помощью тестового пользователя **B. Simon** . Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в VeraSMART.

Чтобы настроить и проверить единый вход Azure AD в VeraSMART, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в VeraSMART](#configure-verasmart-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя VeraSMART](#create-verasmart-test-user)** требуется для того, чтобы в приложении VeraSMART существовал пользователь B. Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **VeraSMART** найдите раздел **Управление** и выберите **Единый вход** .
1. На странице **Выбрать метод единого входа** выберите **SAML** .
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений** , в разделе **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://<SUBDOMAIN>.calero.com/<DOMAIN_NAME>/VeraSMART`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<SUBDOMAIN>.calero.com/<DOMAIN_NAME>/VeraSMART/Saml2/Acs`.

1. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг** , щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://<SUBDOMAIN>.calero.com/<DOMAIN_NAME>/VeraSMART/SSO`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените их фактическими значениями идентификатора, URL-адреса ответа и URL-адреса входа. Чтобы получить их, обратитесь в [службу поддержки клиентов VeraSMART](mailto:support@calero.com). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** нажмите кнопку "Копировать", чтобы скопировать **URL-адрес метаданных федерации приложений** и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B.Simon.

1. На портале Azure в области слева выберите **Azure Active Directory** , **Пользователи** , а затем — **Все пользователи** .
1. В верхней части экрана выберите **Новый пользователь** .
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `B.Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `B.Simon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль** .
   1. Нажмите кнопку **Создать** .

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход в Azure для пользователя B. Simon, предоставив этому пользователю доступ к VeraSMART.

1. На портале Azure выберите **Корпоративные приложения** , а затем — **Все приложения** .
1. В списке приложений выберите **VeraSMART** .
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы** .

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя** , а в диалоговом окне **Добавление назначения**  выберите **Пользователи и группы** .

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать** .
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать** , расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить** .

## <a name="configure-verasmart-sso"></a>Настройка единого входа в VeraSMART

1. Войдите в учетную запись VeraSMART от имени администратора.

1. Последовательно выберите **Administration** -> **Security** -> **Authentication Configuration** (Администрирование > Безопасность > Конфигурация проверки подлинности).

    ![Снимок экрана, на котором показаны выбранные элементы VeraSMART, Administration > Security > Authentication Configuration (Администрирование > Безопасность > Конфигурация проверки подлинности).](./media/verasmart-tutorial/configuration.png)

1. На следующей странице выполните приведенные ниже шаги.

    ![Конфигурация](./media/verasmart-tutorial/upload-metadata.png)

    а. Из раскрывающегося списка выберите **SAML2** в качестве **метода единого входа** .

    b. В текстовом поле **Metadata location** (Расположение метаданных) введите URL-адрес файла метаданных.

    c. Нажмите кнопку **Process IDP metadata** (Обработать метаданные поставщика удостоверений).

    > [!NOTE]
    > Кроме того, можно также щелкнуть параметр **Choose File** (Выбрать файл), чтобы передать файл **метаданных** .

    d. Выберите значение в раскрывающемся списке **Entity ID** (Идентификатор сущности).

    д) Щелкните **Save** (Сохранить).

### <a name="create-verasmart-test-user"></a>Создание тестового пользователя VeraSMART

В этом разделе описано, как в приложении VeraSMART создать пользователя с именем B. Simon. Приложение VeraSMART поддерживает JIT-подготовку пользователей, которая включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в VeraSMART, он создается после проверки подлинности.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку VeraSMART на панели доступа, вы автоматически войдете в приложение VeraSMART, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](./tutorial-list.md)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Что представляет собой условный доступ в Azure Active Directory?](../conditional-access/overview.md)

- [Попробуйте использовать VeraSMART с Azure AD](https://aad.portal.azure.com/)

- [Что такое управление сеансами в Microsoft Cloud App Security?](/cloud-app-security/proxy-intro-aad)