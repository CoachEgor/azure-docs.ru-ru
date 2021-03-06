---
title: Руководство по Интеграция единого входа Azure Active Directory с Roadmunk
description: Сведения о том, как настроить единый вход между Azure Active Directory и Roadmunk.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/28/2020
ms.author: jeedes
ms.openlocfilehash: a7f4682be2f7fbf308aba32768efa932f27b7a87
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "96181719"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-roadmunk"></a>Руководство по Интеграция единого входа Azure Active Directory с Roadmunk

В этом учебнике описано, как интегрировать Roadmunk с Azure Active Directory (Azure AD). Интеграция Roadmunk с Azure AD обеспечивает следующие возможности:

* контроль доступа к Roadmunk с помощью Azure AD;
* автоматический вход пользователей в Roadmunk с учетными записями Azure AD.
* Централизованное управление учетными записями через портал Azure.

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Roadmunk с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

Roadmunk поддерживает единый вход, активируемый *поставщиком услуг* или *поставщиком удостоверений*.

## <a name="add-roadmunk-from-the-gallery"></a>Добавление Roadmunk из коллекции

Чтобы интегрировать Roadmunk с Azure AD, добавьте Roadmunk из коллекции в список управляемых приложений SaaS.

1. Войдите на портал Azure с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области слева выберите **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Roadmunk**.
1. Выберите **Roadmunk** в списке результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-sso-for-roadmunk"></a>Настройка и проверка единого входа Azure AD для Roadmunk

Настройте и проверьте единый вход Azure AD в Roadmunk с помощью тестового пользователя *B.Simon*. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Roadmunk.

Ниже описано, как настроить и проверить работу единого входа Azure AD в Roadmunk.

1. [Настройте единый вход в Azure AD](#configure-azure-ad-sso), чтобы пользователи могли использовать эту функцию.
    1. [Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user) позволяет проверить работу единого входа Azure AD с помощью пользователя B.Simon.
    1. [Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user) необходимо, чтобы пользователь B. Simon мог использовать единый вход Azure AD.
1. [Настройка единого входа в Roadmunk](#configure-roadmunk-sso) необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. [Создайте тестового пользователя Roadmunk](#create-roadmunk-test-user), чтобы связать B.Simon в Roadmunk с представлением этого же пользователя в Azure AD.
1. [Проверьте единый вход](#test-sso), чтобы убедиться в правильности настройки.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure:

1. На портале Azure на странице интеграции с приложением **Roadmunk** найдите раздел **Управление**, а затем выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок пера, чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить эти параметры.

   ![Снимок экрана: значок редактирования для базовой конфигурации SAML.](common/edit-urls.png)

1. Если вы хотите настроить приложение в режиме, инициируемом поставщиком удостоверений, и у вас есть файл метаданных поставщика услуг, выполните в разделе **Базовая конфигурация SAML** следующие действия:

    а. Щелкните **Отправить файл метаданных**.

    ![Снимок экрана: ссылка для отправки файла метаданных.](common/upload-metadata.png)

    б. Щелкните значок папки, чтобы выбрать файл метаданных, скачанный на шаге 4 процедуры "Настройка единого входа в Roadmunk". Затем нажмите кнопку **Отправить**.

    ![Снимок экрана: выбор файла метаданных.](common/browse-upload-metadata.png)

    После успешной отправки файла метаданных автоматически заполняются значения параметров **Идентификатор** и **URL-адрес ответа** в области **Базовая конфигурация SAML**.

    ![Снимок экрана: раздел базовой конфигурации SAML. Здесь выделены поля "Идентификатор" и "URL-адрес ответа".](common/idp-intiated.png)

    > [!Note]
    > Если значения **Идентификатор** и **URL-адрес ответа** не заполнятся автоматически, введите их вручную.

1. Если вы хотите настроить приложение для работы в режиме, инициируемом поставщиком услуг, выберите **Задать дополнительные URL-адреса**. В поле **URL-адрес входа** введите `https://login.roadmunk.com`.

    ![Снимок экрана: место для указания URL-адреса входа для режима, инициируемого поставщиком услуг.](common/metadata-upload-additional-signon.png)

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите поле **XML метаданных федерации**. Затем щелкните **Скачать**, чтобы получить сертификат и сохранить его на локальном компьютере.

    ![Снимок экрана: ссылка для скачивания сертификата подписи SAML.](common/metadataxml.png)

1. Скопируйте один или несколько нужных URL-адресов в разделе **Настройка Roadmunk**.

    ![Снимок экрана: место для копирования URL-адресов конфигурации.](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

С помощью инструкций из этого раздела вы создадите на портале Azure тестового пользователя. Этому пользователю вы присвоите имя *B.Simon*.

1. Слева на портале Azure выберите **Azure Active Directory** > **Пользователи** > **Все пользователи**.
1. В верхней части окна выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `B.Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, введите `B.Simon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   1. Нажмите кнопку **создания**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе вы разрешите пользователю B.Simon использовать единый вход Azure, предоставив доступ к Roadmunk.

1. На портале Azure выберите **Корпоративные приложения** > **Все приложения**.
1. В списке приложений выберите **Roadmunk**.
1. На странице обзора приложения найдите раздел **Управление** и выберите вкладку **Пользователи и группы**.
1. Выберите **Добавить пользователя**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.
1. В диалоговом окне **Пользователи и группы** в списке **Пользователи** выберите **B.Simon**. В нижней части этого диалогового окна щелкните **Выбрать**.
1. Если пользователям нужно назначить роль, выберите ее в раскрывающемся меню **Выбор роли**. Если для этого приложения не настроена ни одна роль, будет выбрана роль **Доступ по умолчанию**.
1. В диалоговом окне **Добавление назначения** выберите **Назначить**.

## <a name="configure-roadmunk-sso"></a>Настройка единого входа Roadmunk

1. Войдите на веб-сайт Roadmunk с правами администратора.

1. В нижней части страницы щелкните значок пользователя, а затем **Account Settings** (Параметры учетной записи).

    ![Снимок экрана: место для выбора параметров учетной записи пользователя.](./media/roadmunk-tutorial/account.png)

1. Теперь откройте **Company** > **Authentication Settings** (Компания — Параметры аутентификации).

1. На странице **Authentication Settings** (Параметры аутентификации) выполните следующие действия:

    ![Снимок экрана: страница Authentication Settings (Параметры аутентификации).](./media/roadmunk-tutorial/saml-sso.png)

    а. Включите **SAML Single Sign On (SSO)** (Единый вход SAML).

    б. В разделе **Step 1** (Шаг 1) отправьте XML-файл метаданных или укажите URL-адрес метаданных.

    c. В разделе **Step 2** (Шаг 2) скачайте файл метаданных Roadmunk и сохраните его на компьютере.

    d. Если вы хотите использовать только единый вход, в разделе **Step 3** (Шаг 3) выберите **Enforce SAML Sign-In Only** (Принудительное применение только входа SAML).

    д) Щелкните **Сохранить**.


### <a name="create-roadmunk-test-user"></a>Создание тестового пользователя Roadmunk

1. Войдите на веб-сайт Roadmunk с правами администратора.

1. В нижней части страницы щелкните значок пользователя, а затем **Account Settings** (Параметры учетной записи).

    ![Снимок экрана: как открыть параметры учетной записи для тестового пользователя.](./media/roadmunk-tutorial/account.png)

1. Откройте вкладку **Users** (Пользователи), а затем выберите **Invite User** (Пригласить пользователя).

    ![Снимок экрана: вкладка Users (Пользователи). Выделена кнопка Invite User (Пригласить пользователя). В открытом окне выделены поля Email (Адрес электронной почты) и Role (Роль).](./media/roadmunk-tutorial/create-user.png)

1. В появившейся форме введите необходимые сведения, а затем выберите **Invite** (Пригласить).


## <a name="test-sso"></a>Проверка единого входа 

В этом разделе вы с помощью панели доступа выполните проверку конфигурации единого входа Azure AD.

Щелкнув плитку **Roadmunk** на портале "Мои приложения", вы автоматически войдете на сайт Roadmunk, для которого настроили единый вход. Дополнительные сведения см. в статье о [входе на портал "Мои приложения" и запуске приложений на нем](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Дальнейшие действия

После настройки Roadmunk вы сможете включить управление сеансом. Этот механизм защищает от утечки данных и несанкционированного доступа к конфиденциальным данным вашей организации в режиме реального времени. Управление сеансом является расширением функции условного доступа. 

[Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security.](/cloud-app-security/proxy-deployment-any-app)