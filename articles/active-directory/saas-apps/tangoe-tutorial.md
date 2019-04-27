---
title: Руководство по Интеграция Azure Active Directory с Tangoe Command Premium Mobile | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении Tangoe Command Premium Mobile.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 2b0b544c-9c2c-49cd-862b-ec2ee9330126
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8c9f410fa890df7aac3c3bf4d89468b92e69ba38
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60869389"
---
# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a>Руководство по Интеграция Azure Active Directory с Tangoe Command Premium Mobile

В этом учебнике описано, как интегрировать Tangoe Command Premium Mobile с Azure Active Directory (Azure AD).

Интеграция Tangoe Command Premium Mobile с Azure AD дает приведенные далее преимущества:

- С помощью Azure AD вы можете контролировать доступ к Tangoe Command Premium Mobile.
- Вы можете включить автоматический вход пользователей в Tangoe Command Premium Mobile (единый вход) с использованием учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Технические условия

Чтобы настроить интеграцию Azure AD с Tangoe Command Premium Mobile, вам потребуется следующее:

- подписка Azure AD;
- подписка Tangoe Command Premium Mobile с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Tangoe Command Premium Mobile из коллекции.
1. Настройка и проверка единого входа в Azure AD.

## <a name="add-tangoe-command-premium-mobile-from-the-gallery"></a>Добавление Tangoe Command Premium Mobile из коллекции
Чтобы настроить интеграцию Tangoe Command Premium Mobile с Azure AD, необходимо добавить Tangoe Command Premium Mobile из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Tangoe Command Premium Mobile из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **Tangoe Command Premium Mobile**, на панели результатов выберите **Tangoe Command Premium Mobile** и нажмите кнопку **Добавить**, чтобы добавить выбранное приложение.

    ![Добавление Tangoe Command Premium Mobile из коллекции](./media/tangoe-tutorial/tutorial_tangoe_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD
В этом разделе описана настройка и проверка единого входа Azure AD в Tangoe Command Premium Mobile с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в Tangoe Command Premium Mobile соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Tangoe Command Premium Mobile.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Tangoe Command Premium Mobile.

Чтобы настроить и проверить единый вход Azure AD в Tangoe Command Premium Mobile, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Tangoe Command Premium Mobile](#create-a-tangoe-command-premium-mobile-test-user)** требуется для того, чтобы в Tangoe Command Premium Mobile существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
1. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе мы включим на портале Azure единый вход Azure AD и настроим его в приложении Tangoe Command Premium Mobile.

**Чтобы настроить единый вход Azure AD в Tangoe Command Premium Mobile, выполните следующие действия:**

1. На портале Azure на странице интеграции с приложением **Tangoe Command Premium Mobile** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Параметр "Вход на основе SAML"](./media/tangoe-tutorial/tutorial_tangoe_samlbase.png)

1. В разделе **Домены и URL-адреса приложения Tangoe Command Premium Mobile** сделайте следующее:

    ![Домены и URL-адреса приложения Tangoe Command Premium Mobile](./media/tangoe-tutorial/tutorial_tangoe_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=<tenant issuer>&TARGET=<target page url>`

    2. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://sso.tangoe.com/sp/ACS.saml2`.

    > [!NOTE] 
    > Эти значения приведены для примера. Замените их фактическими значениями URL-адреса ответа и URL-адреса входа. Для получения этих значений обратитесь в [службу поддержки клиентов Tangoe Command Premium Mobile](https://www.tangoe.com/contact-us/). 

1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Раздел "Сертификат подписи SAML"](./media/tangoe-tutorial/tutorial_tangoe_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить"](./media/tangoe-tutorial/tutorial_general_400.png)
    
1. В разделе **Настройка Tangoe Command Premium Mobile** щелкните **Настроить Tangoe Command Premium Mobile**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода, идентификатор сущности SAML и URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Раздел настройки Tangoe Command Premium Mobile](./media/tangoe-tutorial/tutorial_tangoe_configure.png) 

1. Чтобы настроить единый вход для своего приложения, обратитесь в [службу поддержки клиентов Tangoe Command Premium Mobile](https://www.tangoe.com/contact-us/) и предоставьте следующие сведения:

   - скачанный файл метаданных;
   - **идентификатор сущности SAML**;
   - **URL-адрес службы единого входа SAML**;
   - **URL-адрес выхода**.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/tangoe-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Выберите "Пользователи и группы" > "Все пользователи".](./media/tangoe-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Добавление пользователя](./media/tangoe-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Диалоговое окно "Пользователь"](./media/tangoe-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    2. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="create-a-tangoe-command-premium-mobile-test-user"></a>Создание тестового пользователя Tangoe Command Premium Mobile

В этом разделе мы создадим пользователя Britta Simon в приложении Tangoe Command Premium Mobile. 

Перед выполнением единого входа все пользователи должны быть подготовлены в приложении Tangoe Command Premium Mobile. Для этого обратитесь в [службу поддержки клиентов Tangoe Command Premium Mobile](https://www.tangoe.com/contact-us/). 

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе мы разрешим пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Tangoe Command Premium Mobile.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon приложению Tangoe Command Premium Mobile, выполните следующие действия:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **Tangoe Command Premium Mobile**.

    ![Tangoe Command Premium Mobile в списке приложений](./media/tangoe-tutorial/tutorial_tangoe_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент Tangoe Command Premium Mobile, вы автоматически войдете в приложение Tangoe Command Premium Mobile. Дополнительные сведения о панели доступа см. в статье с [общими сведениями о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/tangoe-tutorial/tutorial_general_01.png
[2]: ./media/tangoe-tutorial/tutorial_general_02.png
[3]: ./media/tangoe-tutorial/tutorial_general_03.png
[4]: ./media/tangoe-tutorial/tutorial_general_04.png

[100]: ./media/tangoe-tutorial/tutorial_general_100.png

[200]: ./media/tangoe-tutorial/tutorial_general_200.png
[201]: ./media/tangoe-tutorial/tutorial_general_201.png
[202]: ./media/tangoe-tutorial/tutorial_general_202.png
[203]: ./media/tangoe-tutorial/tutorial_general_203.png

