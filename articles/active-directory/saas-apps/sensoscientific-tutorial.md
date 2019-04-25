---
title: Руководство по Интеграция Azure Active Directory с беспроводной системой мониторинга температуры SensoScientific | Документация Майкрософт
description: Сведения о настройке единого входа между Azure Active Directory и беспроводной системой мониторинга температуры SensoScientific.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: ee9a924d-ccde-45b0-ab40-877f82f5dfa2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 33a625e82f41bee1b8e3980192076d24a7471953
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60340721"
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a>Руководство по Интеграция Azure Active Directory с беспроводной системой мониторинга температуры SensoScientific

Это руководство содержит сведения об интеграции беспроводной системы мониторинга температуры SensoScientific с Azure Active Directory (Azure AD).

Интеграция беспроводной системы мониторинга температуры SensoScientific с Azure AD обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к беспроводной системе мониторинга температуры SensoScientific.
- Вы можете включить автоматический вход пользователей в беспроводную систему мониторинга температуры SensoScientific (единый вход) с использованием учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Технические условия

Чтобы настроить интеграцию Azure AD с беспроводной системой мониторинга температуры SensoScientific, вам потребуется:

- подписка Azure AD;
- подписка на беспроводную систему мониторинга температуры SensoScientific с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление беспроводной системы мониторинга температуры SensoScientific из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-the-gallery"></a>Добавление беспроводной системы мониторинга температуры SensoScientific из коллекции
Чтобы настроить интеграцию беспроводной системы мониторинга температуры SensoScientific с Azure AD, необходимо добавить эту систему из коллекции в список управляемых приложений SaaS.

**Чтобы добавить беспроводную систему мониторинга температуры SensoScientific из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **беспроводная система мониторинга температуры SensoScientific**.

    ![Создание тестового пользователя Azure AD](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_search.png)

1. На панели результатов выберите **беспроводную систему мониторинга температуры SensoScientific** и нажмите кнопку **Добавить**, чтобы добавить приложение.

    ![Создание тестового пользователя Azure AD](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описывается настройка и проверка единого входа Azure AD в беспроводную систему мониторинга температуры SensoScientific с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, в Azure AD необходимо указать, какой пользователь в беспроводной системе мониторинга температуры SensoScientific соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в беспроводной системе мониторинга температуры SensoScientific.

Чтобы установить эту связь, следует назначить **имя пользователя** в Azure AD в качестве значения **имени пользователя** в беспроводной системе мониторинга температуры SensoScientific.

Чтобы настроить и проверить единый вход Azure AD в беспроводной системе мониторинга температуры SensoScientific, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя беспроводной системы мониторинга температуры SensoScientific](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)** требуется для создания пользователя Britta Simon в беспроводной системе мониторинга температуры SensoScientific, связанного с соответствующим представлением пользователя в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении беспроводной системы мониторинга температуры SensoScientific.

**Чтобы настроить единый вход Azure AD в беспроводной системе мониторинга температуры SensoScientific, сделайте следующее:**

1. На портале Azure на странице интеграции с приложением **беспроводной системы мониторинга температуры SensoScientific** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_samlbase.png)

1. В разделе **Домены и URL-адреса приложения беспроводной системы мониторинга температуры SensoScientific** не нужно выполнять никаких действий, так как приложение уже предварительно интегрировано с Azure.

    ![Настройка единого входа](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_url.png)

1. В разделе **Сертификат подписи SAML** щелкните **Сертификат (Base64)**, а затем сохраните файл сертификата на компьютере.

    ![Настройка единого входа](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/sensoscientific-tutorial/tutorial_general_400.png)

1. В разделе **беспроводной системы мониторинга температуры SensoScientific** щелкните **Настроить беспроводную систему мониторинга температуры SensoScientific**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода, идентификатор сущности SAML** и **URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Настройка единого входа](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_configure.png) 

1. Войдите в приложение беспроводной системы мониторинга температуры SensoScientific с правами администратора.

1. В верхнем меню навигации щелкните **Configuration** (Конфигурация) и выберите **Configure** (Настройка) в разделе **Single Sign On** (Единый вход), чтобы открыть параметры единого входа.

    ![Настройка единого входа](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png) 

1. В форме **Single Sign On Settings** (Параметры единого входа) сделайте следующее:
 
    a. В качестве **имени издателя** выберите Azure AD.
    
    2. Вставьте **идентификатор сущности SAML**, скопированный на портале Azure, в текстовое поле Issuer URL (URL-адрес издателя).
    
    c. Вставьте **URL-адрес службы единого входа SAML**, скопированный на портале Azure, в текстовое поле Single Sign-On Service URL (URL-адрес службы единого входа).

    d. Вставьте **URL-адрес выхода**, скопированный на портале Azure, в текстовое поле Single Sign-Out Service URL (URL-адрес службы единого входа).

    д. Перейдите к сертификату, скачанному на портале Azure, и загрузите его сюда.
    
    Е. Выберите команду **Сохранить**.
  
> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory](https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/sensoscientific-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/sensoscientific-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/sensoscientific-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/sensoscientific-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    2. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user"></a>Создание текстового пользователя беспроводной системы мониторинга температуры SensoScientific

Чтобы пользователи Azure AD могли выполнять вход в беспроводную систему мониторинга температуры SensoScientific, они должны быть подготовлены для этой системы. Обратитесь в  [службу поддержки беспроводной системы мониторинга температуры SensoScientific](https://www.sensoscientific.com/contact-us/) , чтобы добавить пользователей на платформу беспроводной системы мониторинга температуры SensoScientific. Перед использованием единого входа необходимо создать и активировать пользователей. 

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как предоставить пользователю Britta Simon доступ к беспроводной системе мониторинга температуры SensoScientific, чтобы он мог использовать единый вход Azure.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в приложении беспроводной системы мониторинга температуры SensoScientific, сделайте следующее:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **беспроводную систему мониторинга температуры SensoScientific**.

    ![Настройка единого входа](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа. Щелкните плитку беспроводной системы мониторинга температуры SensoScientific на панели доступа, чтобы автоматически выполнить вход в приложение беспроводной системы мониторинга температуры SensoScientific. Дополнительные сведения о панели доступа см. в статье [Что такое портал MyApps?](../user-help/active-directory-saas-access-panel-introduction.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sensoscientific-tutorial/tutorial_general_01.png
[2]: ./media/sensoscientific-tutorial/tutorial_general_02.png
[3]: ./media/sensoscientific-tutorial/tutorial_general_03.png
[4]: ./media/sensoscientific-tutorial/tutorial_general_04.png

[100]: ./media/sensoscientific-tutorial/tutorial_general_100.png

[200]: ./media/sensoscientific-tutorial/tutorial_general_200.png
[201]: ./media/sensoscientific-tutorial/tutorial_general_201.png
[202]: ./media/sensoscientific-tutorial/tutorial_general_202.png
[203]: ./media/sensoscientific-tutorial/tutorial_general_203.png

