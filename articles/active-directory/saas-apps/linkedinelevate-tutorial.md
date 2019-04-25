---
title: Руководство по Интеграция Azure Active Directory с LinkedIn Elevate | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и LinkedIn Elevate.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6ca8e537f261b59fb4e069d47d24e21abbdeca46
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60260033"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-elevate"></a>Руководство по Интеграция Azure Active Directory с LinkedIn Elevate

В этом руководстве описано, как интегрировать приложение LinkedIn Elevate с Azure Active Directory (Azure AD).

Интеграция Azure AD с LinkedIn Elevate обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать, у кого есть доступ к приложению LinkedIn Elevate.
- Вы можете включить автоматический вход пользователей в LinkedIn Elevate (единый вход) с использованием учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — через портал управления Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Технические условия

Чтобы настроить интеграцию Azure AD с LinkedIn Elevate, вам потребуется:

- подписка Azure AD;
- подписка LinkedIn Elevate с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не следует использовать рабочую среду при отсутствии необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде.
Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление LinkedIn Elevate из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-linkedin-elevate-from-the-gallery"></a>Добавление LinkedIn Elevate из коллекции
Чтобы настроить интеграцию приложения LinkedIn Elevate с Azure AD, вам нужно добавить это приложение из коллекции в свой список управляемых приложений SaaS.

**Добавление приложения LinkedIn Elevate из коллекции**

1. На **[портале управления Azure](https://portal.azure.com)** в левой области навигации нажмите значок **Azure Active Directory**.

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]

1. Нажмите кнопку **Добавить** в верхней части диалогового окна.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **LinkedIn Elevate**. На панели результатов щелкните **LinkedIn Elevate**, чтобы добавить приложение.

    ![Создание тестового пользователя Azure AD](./media/linkedinelevate-tutorial/tutorial-linkedinElevate_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в приложение LinkedIn Elevate с использованием тестового пользователя Britta Simon.

Для работы единого входа службе Azure AD нужно знать, какой пользователь в LinkedIn Elevate соответствует пользователю в Azure AD. Иными словами, нужно установить связь между пользователем Azure AD и соответствующим пользователем в LinkedIn Elevate.

Чтобы установить эту связь, следует назначить **имя пользователя** в Azure AD в качестве значения **имени пользователя** в LinkedIn Elevate.

Чтобы настроить и проверить единый вход Azure AD в LinkedIn Elevate, выполните следующие действия:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя LinkedIn Elevate](#creating-a-linkedin-elevate-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе мы включим на портале Azure единый вход Azure AD и настроим его в приложении LinkedIn Elevate.

**Настройка единого входа Azure AD в LinkedIn Elevate**

1. На портале управления Azure на странице интеграции с приложением **LinkedIn Elevate** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.

    ![Настройка единого входа](./media/linkedinelevate-tutorial/tutorial-linkedin_01.png)

1. В другом окне веб-браузера войдите в свой клиент LinkedIn Elevate с правами администратора.

1. В **Account Center** (Центр учетных записей) в разделе **Settings** (Параметры) щелкните **Global Settings** (Глобальные параметры). Кроме того, выберите **Elevate — Elevate AAD Test** (Проверка Azure AD для Elevate) из раскрывающегося списка.

    ![Настройка единого входа](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_01.png)

1. Щелкните **OR Click Here to load and copy individual fields from the form** (Или щелкните здесь, чтобы загрузить и скопировать отдельные поля из формы) и скопируйте значения **Entity Id** (Идентификатор сущности) и **Assertion Consumer Service (ACS) Url** (URL-адрес службы обработчика утверждений (ACS)).

    ![Настройка единого входа](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_03.png)

1. Если вы хотите настроить единый вход в режиме, **инициированном поставщиком удостоверений**, то на портале Azure в разделе **Домены и URL-адреса приложения LinkedIn Elevate** выполните следующие действия.

    ![Настройка единого входа](./media/linkedinelevate-tutorial/tutorial_linkedin_signon_01.png)

    a. В текстовом поле **Идентификатор** введите **идентификатор сущности**, скопированный с портала LinkedIn. 

    2. В текстовом поле **URL-адрес ответа** введите **URL-адрес службы обработчика утверждений (ACS)**, скопированный с портала LinkedIn.

1. Если вы хотите настроить единый вход в режиме, **инициированном поставщиком услуг**, то установите флажок "Показать дополнительные параметры URL-адресов" в разделе настроек и настройте URL-адрес входа в таком формате:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>` 

    ![Настройка единого входа](./media/linkedinelevate-tutorial/tutorial_linkedin_signon_02.png) 

1. Приложение LinkedIn Elevate ожидает утверждения SAML в определенном формате, а для этого вам нужно добавить настраиваемые сопоставления атрибутов в конфигурацию атрибутов токена SAML. На следующем снимке экрана приведен пример. По умолчанию **идентификатор пользователя** имеет значение **user.userprincipalname**, но для LinkedIn Elevate требуется сопоставить это значение с адресом электронной почты пользователя. Для этого можно использовать атрибут **user.mail** из списка или соответствующее значение атрибута, основанное на конфигурации организации.

    ![Настройка единого входа](./media/linkedinelevate-tutorial/updateusermail.png)

1. В разделе **Атрибуты пользователя** установите флажок **Просмотреть и изменить все другие атрибуты пользователей** и задайте эти атрибуты. Необходимо также добавить утверждение с именем **department**, значение которого должно быть сопоставлено с **user.department**.

    | Имя атрибута | Значение атрибута |
    | --- | --- |
    | department| user.department |

      ![Создание тестового пользователя Azure AD](./media/linkedinelevate-tutorial/userattribute.png)

      a. Щелкните "Добавить атрибут", чтобы открыть страницу сведений об атрибутах. Добавьте атрибут department, как показано ниже.

      ![Создание тестового пользователя Azure AD](./media/linkedinelevate-tutorial/adduserattribute.png)

      2. Нажмите кнопку **ОК**, чтобы сохранить атрибут.

      c. Измените имя атрибута **emailaddress** на **email**.

1. В разделе **Сертификат подписи SAML** щелкните **XML метаданных** и сохраните XML-файл на компьютере.

    ![Настройка единого входа](./media/linkedinelevate-tutorial/tutorial-linkedinElevate_certificate.png) 

1. Выберите команду **Сохранить**.

    ![Настройка единого входа](./media/linkedinelevate-tutorial/tutorial_general_400.png)

1. Перейдите в раздел **LinkedIn Admin Settings** (Параметры администратора LinkedIn). Передайте XML-файл, скачанный с портала Azure. Для этого нажмите кнопку "Upload XML file" (Передать XML-файл).

    ![Настройка единого входа](./media/linkedinelevate-tutorial/tutorial_linkedin_metadata_03.png)

1. Нажмите кнопку **On** (Включить), чтобы включить единый вход. Состояние единого входа изменится с **Not Connected** (Не подключено) на **Connected** (Подключено.)

    ![Настройка единого входа](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале управления Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале управления Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/linkedinelevate-tutorial/create_aaduser_01.png) 

1. Перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**, чтобы отобразить список пользователей.

    ![Создание тестового пользователя Azure AD](./media/linkedinelevate-tutorial/create_aaduser_02.png) 

1. В верхней части диалогового окна щелкните **Добавить**, чтобы открыть диалоговое окно **Пользователь**.

    ![Создание тестового пользователя Azure AD](./media/linkedinelevate-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.

    ![Создание тестового пользователя Azure AD](./media/linkedinelevate-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    2. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.

### <a name="creating-a-linkedin-elevate-test-user"></a>Создание тестового пользователя LinkedIn Elevate

Приложение LinkedIn Elevate поддерживает JIT-подготовку пользователей, поэтому после аутентификации пользователи будут созданы в приложении автоматически. На странице параметров администратора на портале LinkedIn Elevate включите параметр **Automatically assign licenses** (Автоматически назначать лицензии), чтобы активировать JIT-подготовку, одновременно с которой пользователю будет сразу назначена лицензия. LinkedIn Elevate также поддерживает автоматическую подготовку пользователей. Дополнительные сведения о настройке автоматической подготовки пользователей можно найти [здесь](linkedinelevate-provisioning-tutorial.md).

   ![Создание тестового пользователя Azure AD](./media/linkedinelevate-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе мы разрешим пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к LinkedIn Elevate.

![Назначение пользователя][200] 

**Назначение пользователя Britta Simon приложению LinkedIn Elevate**

1. На портале управления Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201]

1. В списке приложений выберите **LinkedIn Elevate**.

    ![Настройка единого входа](./media/linkedinelevate-tutorial/tutorial-linkedinElevate_0001.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент LinkedIn Elevate на панели доступа, вы перейдете на страницу входа Azure. Успешно выполнив вход, вы должны попасть в приложение LinkedIn Elevate.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Руководство по настройке LinkedIn Elevate для автоматической подготовки пользователей](linkedinelevate-provisioning-tutorial.md)
* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Руководство по настройке Google Apps для автоматической подготовки пользователей](linkedinelevate-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/linkedinElevate-tutorial/tutorial_general_01.png
[2]: ./media/linkedinElevate-tutorial/tutorial_general_02.png
[3]: ./media/linkedinElevate-tutorial/tutorial_general_03.png
[4]: ./media/linkedinElevate-tutorial/tutorial_general_04.png

[100]: ./media/linkedinElevate-tutorial/tutorial_general_100.png

[200]: ./media/linkedinElevate-tutorial/tutorial_general_200.png
[201]: ./media/linkedinElevate-tutorial/tutorial_general_201.png
[202]: ./media/linkedinElevate-tutorial/tutorial_general_202.png
[203]: ./media/linkedinElevate-tutorial/tutorial_general_203.png
