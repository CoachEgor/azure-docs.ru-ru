---
title: Руководство. Интеграция Azure Active Directory с The Cloud Security Fabric | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в The Cloud Security Fabric.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 549e8810-1b3b-4351-bf4b-f07de98980d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a556b38ca4947b71555ba7b023607b392900bdaf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60429604"
---
# <a name="tutorial-azure-active-directory-integration-with-the-cloud-security-fabric"></a>Руководство. Интеграция Azure Active Directory с The Cloud Security Fabric

В этом руководстве описано, как интегрировать The Cloud Security Fabric с Azure Active Directory (Azure AD).

Интеграция Azure AD с The Cloud Security Fabric обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к The Cloud Security Fabric.
- Вы можете включить автоматический вход пользователей в The Cloud Security Fabric (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Технические условия

Чтобы настроить интеграцию Azure AD с The Cloud Security Fabric, вам потребуется:

- подписка Azure AD;
- подписка The Cloud Security Fabric с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление The Cloud Security Fabric из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-the-cloud-security-fabric-from-the-gallery"></a>Добавление The Cloud Security Fabric из коллекции
Чтобы настроить интеграцию The Cloud Security Fabric с Azure AD, вам нужно добавить The Cloud Security Fabric из коллекции в список управляемых приложений SaaS.

**Чтобы добавить The Cloud Security Fabric из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"][3]

1. В поле поиска введите **The Cloud Security Fabric**, выберите **The Cloud Security Fabric** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![The Cloud Security Fabric в списке результатов](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в The Cloud Security Fabric с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в The Cloud Security Fabric соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в The Cloud Security Fabric.

Чтобы настроить и проверить единый вход Azure AD в The Cloud Security Fabric, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя The Cloud Security Fabric](#create-a-the-cloud-security-fabric-test-user)** требуется для того, чтобы в The Cloud Security Fabric существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
1. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении The Cloud Security Fabric.

**Чтобы настроить единый вход Azure AD в The Cloud Security Fabric, выполните следующие действия:**

1. На портале Azure на странице интеграции с приложением **The Cloud Security Fabric** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.

    ![Диалоговое окно "Единый вход"](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_samlbase.png)

1. В разделе **Домены и URL-адреса приложения The Cloud Security Fabric** выполните следующие действия:

    ![Сведения о домене и URL-адресах единого входа для приложения The Cloud Security Fabric](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес.

    | |
    |--|
    | `https://platform.cloudlock.com` |
    | `https://app.cloudlock.com` |

    2. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате:
    
    | |
    |--|
    | `https://platform.cloudlock.com/gate/saml/sso/<subdomain>` |
    | `https://app.cloudlock.com/gate/saml/sso/<subdomain>` |

    > [!NOTE]
    > Значение идентификатора приведено для примера и не является реальным. Вместо него нужно указать фактический идентификатор. Чтобы получить это значение, обратитесь в [службу поддержки The Cloud Security Fabric](mailto:support@cloudlock.com). 

1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Ссылка для скачивания сертификата](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_certificate.png)

1. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/ciscocloudlock-tutorial/tutorial_general_400.png)

1. Чтобы настроить единый вход на стороне **The Cloud Security Fabric**, отправьте скачанный **XML-файл метаданных** в [службу поддержки The Cloud Security Fabric](mailto:support@cloudlock.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка Azure Active Directory](./media/ciscocloudlock-tutorial/create_aaduser_01.png)

1. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/ciscocloudlock-tutorial/create_aaduser_02.png)

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/ciscocloudlock-tutorial/create_aaduser_03.png)

1. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/ciscocloudlock-tutorial/create_aaduser_04.png)

    a. В поле **Имя** введите **BrittaSimon**.

    Б. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    d. Нажмите кнопку **Создать**.

### <a name="create-a-the-cloud-security-fabric-test-user"></a>Создание тестового пользователя The Cloud Security Fabric

В этом разделе описано, как создать пользователя Britta Simon в приложении The Cloud Security Fabric. Чтобы добавить пользователей на платформе The Cloud Security Fabric, обратитесь в  [службу поддержки The Cloud Security Fabric](mailto:support@cloudlock.com) . Перед использованием единого входа необходимо создать и активировать пользователей. 

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к The Cloud Security Fabric.

![Назначение роли пользователя][200]

**Чтобы назначить пользователя Britta Simon в The Cloud Security Fabric, выполните указанные ниже действия:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201]

1. В списке приложений выберите **The Cloud Security Fabric**.

    ![Ссылка на The Cloud Security Fabric в списке приложений](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_app.png)  

1. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент The Cloud Security Fabric на панели доступа, вы автоматически войдете в приложение The Cloud Security Fabric.
Дополнительные сведения о панели доступа см. в статье с [общими сведениями о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/ciscocloudlock-tutorial/tutorial_general_01.png
[2]: ./media/ciscocloudlock-tutorial/tutorial_general_02.png
[3]: ./media/ciscocloudlock-tutorial/tutorial_general_03.png
[4]: ./media/ciscocloudlock-tutorial/tutorial_general_04.png

[100]: ./media/ciscocloudlock-tutorial/tutorial_general_100.png

[200]: ./media/ciscocloudlock-tutorial/tutorial_general_200.png
[201]: ./media/ciscocloudlock-tutorial/tutorial_general_201.png
[202]: ./media/ciscocloudlock-tutorial/tutorial_general_202.png
[203]: ./media/ciscocloudlock-tutorial/tutorial_general_203.png
