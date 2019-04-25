---
title: Руководство по Интеграция Azure Active Directory с itslearning | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении itslearning.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 60587ba3-1396-4b8a-9ac1-e22a98e5e0ac
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a12fb0123e96c592a8af9fe04f6ff17f8fea62a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60270170"
---
# <a name="tutorial-azure-active-directory-integration-with-itslearning"></a>Руководство по Интеграция Azure Active Directory с itslearning

В этом руководстве описано, как интегрировать Azure Active Directory (Azure AD) с приложением itslearning.

Интеграция приложения itslearning с Azure AD обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к itslearning.
- Вы можете включить автоматический вход пользователей в itslearning (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Технические условия

Чтобы настроить интеграцию Azure AD с itslearning, вам потребуется:

- подписка Azure AD;
- подписка с поддержкой единого входа itslearning.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление itslearning из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-itslearning-from-the-gallery"></a>Добавление itslearning из коллекции
Чтобы настроить интеграцию itslearning с Azure AD, необходимо добавить itslearning из коллекции в список управляемых приложений SaaS.

**Чтобы добавить itslearning из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **itslearning**.

    ![Создание тестового пользователя Azure AD](./media/itslearning-tutorial/tutorial_itslearning_search.png)

1. На панели результатов выберите **Iitslearning** и нажмите кнопку **Добавить**, чтобы добавить приложение.

    ![Создание тестового пользователя Azure AD](./media/itslearning-tutorial/tutorial_itslearning_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в itslearning с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в itslearning соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователям Azure AD и соответствующим пользователем в itslearning.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в itslearning.

Чтобы настроить и проверить единый вход Azure AD в itslearning, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя itslearning](#creating-an-itslearning-test-user)** требуется для создания в itslearning пользователя Britta Simon, связанного с представлением этого же пользователя в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В данном разделе описано, как включить единый вход Azure AD на портале управления Azure и настроить его в приложении itslearning.

**Чтобы настроить единый вход Azure AD в itslearning, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **itslearning** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/itslearning-tutorial/tutorial_itslearning_samlbase.png)

1. В разделе **Домены и URL-адреса приложения itslearning** выполните следующие действия:

    ![Настройка единого входа](./media/itslearning-tutorial/tutorial_itslearning_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес.
    
    | |
    |--| 
    | `https://www.itslearning.com/index.aspx`|
    | `https://us1.itslearning.com/index.aspx`|

    2. В текстовом поле **Идентификатор** введите URL-адрес в формате: `urn:mace:saml2v2.no:services:com.itslearning`

1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Настройка единого входа](./media/itslearning-tutorial/tutorial_itslearning_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/itslearning-tutorial/tutorial_general_400.png)

1. Чтобы настроить единый вход на стороне **itslearning**, отправьте скачанный **XML-файл метаданных** в [службу поддержки itslearning](mailto:support@itslearning.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/itslearning-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/itslearning-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/itslearning-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/itslearning-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    2. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-an-itslearning-test-user"></a>Создание тестового пользователя itslearning

В этом разделе описано, как создать пользователя Britta Simon в приложении itslearning. Чтобы добавить пользователей на платформу itslearning, обратитесь к [группе поддержки клиентов itslearning](mailto:support@itslearning.com).  Перед использованием единого входа необходимо создать и активировать пользователей.

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure путем предоставления доступа к itslearning.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon приложению itslearning, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **itslearning**.

    ![Настройка единого входа](./media/itslearning-tutorial/tutorial_itslearning_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Когда вы нажмете плитку itslearning на панели доступа, должна появиться страница входа в приложение itslearning. Нажмите кнопку **Вход с помощью Microsoft Azure ACS1** для успешного входа в приложение.

  ![Вход](./media/itslearning-tutorial/login.png)

См. дополнительные сведения о [панели доступа](../user-help/active-directory-saas-access-panel-introduction.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/itslearning-tutorial/tutorial_general_01.png
[2]: ./media/itslearning-tutorial/tutorial_general_02.png
[3]: ./media/itslearning-tutorial/tutorial_general_03.png
[4]: ./media/itslearning-tutorial/tutorial_general_04.png

[100]: ./media/itslearning-tutorial/tutorial_general_100.png

[200]: ./media/itslearning-tutorial/tutorial_general_200.png
[201]: ./media/itslearning-tutorial/tutorial_general_201.png
[202]: ./media/itslearning-tutorial/tutorial_general_202.png
[203]: ./media/itslearning-tutorial/tutorial_general_203.png

