---
title: Руководство по Интеграция Azure Active Directory с Shmoop For Schools | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Shmoop For Schools.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: jeedes
ms.openlocfilehash: fdd9a0e72b9fee374de810ce6e18bb0f5c2fbe25
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/18/2020
ms.locfileid: "88548622"
---
# <a name="tutorial-integrate-shmoop-for-schools-with-azure-active-directory"></a>Руководство по Интеграции Azure Active Directory с Shmoop For Schools

В этом руководстве вы узнаете, как интегрировать Shmoop For Schools с Azure Active Directory (Azure AD). Интеграция Azure Active Directory с Shmoop For Schools обеспечивает следующие возможности.

* С помощью Azure AD можно контролировать доступ к Shmoop For Schools.
* Можно разрешить автоматический вход пользователей в Shmoop For Schools с помощью учетной записи Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Shmoop For Schools с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Shmoop For Schools поддерживает единый вход, инициируемый **поставщиком услуг**.
* Shmoop For Schools поддерживает **JIT**-подготовку пользователей.

## <a name="adding-shmoop-for-schools-from-the-gallery"></a>Добавление Shmoop For Schools из коллекции

Чтобы настроить интеграцию Shmoop For Schools с Azure AD, необходимо добавить Shmoop For Schools из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Shmoop For Schools**.
1. Выберите **Shmoop For Schools** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-single-sign-on-for-shmoop-for-schools"></a>Настройка и проверка единого входа Azure AD для Shmoop for Schools

Настройте и проверьте единый вход Azure AD для Shmoop For Schools с помощью тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Shmoop For Schools.

Для настройки и проверки единого входа Azure AD для Shmoop For Schools, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    * **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    * **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
2. **[Настройка единого входа в Shmoop For Schools](#configure-shmoop-for-schools-sso)** необходима для настройки параметров единого входа на стороне приложения.
    * **[Создание тестового пользователя Shmoop For Schools](#create-shmoop-for-schools-test-user)** требуется для того, чтобы в Shmoop For Schools существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
3. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Shmoop For Schools** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    а. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://schools.shmoop.com/public-api/saml2/start/<uniqueid>`.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://schools.shmoop.com/<uniqueid>`.

    > [!NOTE]
    > Эти значения приведены для примера. Необходимо обновить эти значения действующим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов Shmoop For Schools](mailto:support@shmoop.com). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. Приложение Shmoop For Schools ожидает утверждения SAML в определенном формате, что требует добавления настраиваемых пользовательских сопоставлений атрибутов в конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию.

    ![Изображение](common/default-attributes.png)

    > [!NOTE]
    > Shmoop For Schools поддерживает две роли для пользователей: **Учитель** и **Ученик**. Настройте эти роли в Azure AD, чтобы пользователям можно было назначить соответствующие роли. Чтобы понять, как настроить роли в Azure AD, см. [здесь](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management).

1. В дополнение к описанному выше приложение Shmoop For Schools ожидает несколько дополнительных атрибутов в ответе SAML, как показано ниже. Эти атрибуты также заранее заполнены, но вы можете изменить их в соответствии со своими требованиями.

    | Имя |  Исходный атрибут|
    | --------- | --------------- |
    | роль      | user.assignedroles |

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** нажмите кнопку "Копировать", чтобы копировать **URL-адрес метаданных федерации приложений** и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B.Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.
1. В верхней части экрана выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
    1. В поле **Имя** введите `B.Simon`.  
    1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `B.Simon@contoso.com`.
    1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
    1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход в Azure для пользователя B.Simon, предоставив этому пользователю доступ к Shmoop For Schools.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Shmoop For Schools**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-shmoop-for-schools-sso"></a>Настройка единого входа для Shmoop For Schools

Чтобы настроить единый вход на стороне **Shmoop For Schools**, отправьте [группе поддержки Shmoop For Schools](mailto:support@shmoop.com)**URL-адрес метаданных федерации приложений**. Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-shmoop-for-schools-test-user"></a>Создание тестового пользователя Shmoop For Schools

В этом разделе описано, как создать пользователя B.Simon в приложении Shmoop For Schools. Shmoop For Schools поддерживает JIT-подготовку. Эта функция включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в Shmoop For Schools, он создается после выполнения аутентификации.

> [!NOTE]
> Чтобы создать учетную запись пользователя вручную, обратитесь в [службу поддержки Shmoop For Schools](mailto:support@shmoop.com).

## <a name="test-sso"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "Shmoop For Schools" на Панели доступа, вы автоматически войдете в приложение Shmoop For Schools, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Испробуйте Shmoop for Schools с Azure AD](https://aad.portal.azure.com/)