---
title: Руководство по Интеграция Azure Active Directory с Kantega SSO for FishEye/Crucible | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Kantega SSO for FishEye/Crucible.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 9fe951fd-1530-4d33-a1a4-390385b99ce9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e8205fe300cb88dc9cced5f5a75e692700f9f34b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60265048"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-fisheyecrucible"></a>Руководство по Интеграция Azure Active Directory с Kantega SSO for FishEye/Crucible

В этом руководстве описано, как интегрировать Kantega SSO for FishEye/Crucible с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением Kantega SSO for FishEye/Crucible обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к Kantega SSO for FishEye/Crucible.
- Вы можете включить автоматический вход пользователей в Kantega SSO for FishEye/Crucible (единый вход) с использованием учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Технические условия

Чтобы настроить интеграцию Azure AD с Kantega SSO for FishEye/Crucible, вам потребуется:

- подписка Azure AD;
- подписка Kantega SSO for FishEye/Crucible с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Kantega SSO for FishEye/Crucible из коллекции.
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-kantega-sso-for-fisheyecrucible-from-the-gallery"></a>Добавление Kantega SSO for FishEye/Crucible из коллекции
Чтобы настроить интеграцию Kantega SSO for FishEye/Crucible с Azure AD, необходимо добавить Kantega SSO for FishEye/Crucible из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Kantega SSO for FishEye/Crucible из коллекции, сделайте следующее.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **Kantega SSO for FishEye/Crucible**.

    ![Создание тестового пользователя Azure AD](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_search.png)

1. На панели результатов выберите **Kantega SSO for FishEye/Crucible** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Создание тестового пользователя Azure AD](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в Kantega SSO for FishEye/Crucible с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходима информация о том, какой пользователь в Kantega SSO for FishEye/Crucible соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Kantega SSO for FishEye/Crucible.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Kantega SSO for FishEye/Crucible.

Чтобы настроить и проверить единый вход Azure AD в Kantega SSO for FishEye/Crucible, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Kantega SSO for FishEye/Crucible](#creating-a-kantega-sso-for-fisheyecrucible-test-user)** требуется для того, чтобы в Kantega SSO for FishEye/Crucible существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В данном разделе описано, как включить единый вход в Azure AD на портале Azure и настроить его в приложении Kantega SSO for FishEye/Crucible.

**Чтобы настроить единый вход Azure AD в Kantega SSO for FishEye/Crucible, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **Kantega SSO for FishEye/Crucible** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_samlbase.png)

1. Чтобы использовать режим, инициируемый **IdP**, в разделе **Домены и URL-адреса приложения Kantega SSO for FishEye/Crucible** выполните следующие действия.

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_url1.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    2. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`.

1. Чтобы использовать режим, инициируемый **поставщиком услуг**, установите флажок **Показать дополнительные параметры URL-адресов**.

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_url2.png)

    В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`
     
    > [!NOTE] 
    > Эти значения приведены для примера. Замените их фактическими значениями идентификатора, URL-адреса ответа и URL-адреса входа. Эти значения предоставляются во время настройки подключаемого модуля FishEye/Crucible, которая описывается далее в этом руководстве.

1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_400.png)
    
1. В другом окне веб-браузера войдите на свой локальный сервер FishEye или Crucible в качестве администратора.

1. Наведите указатель мыши на шестеренку и щелкните **Add-ons** (Надстройки).

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/addon1.png)

1. В разделе "System Settings" (Параметры системы) щелкните **Find new add-ons** (Найти новые надстройки). 

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/add-on2.png)

1. Найдите подключаемый модуль **Kantega SSO for Crucible (SAML & Kerberos)** и нажмите кнопку **Install** (Установить), чтобы установить новый подключаемый модуль SAML.

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/addon2.png)

1. Начнется установка подключаемого модуля. 

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/addon33.png)

1. Установка завершится. Нажмите кнопку **Закрыть**

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/addon34.png)

1.  Нажмите кнопку **Управление**.

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/addon35.png)

1. Щелкните **Configure** (Настройка), чтобы настроить новый подключаемый модуль. 

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/addon3.png)

1. В разделе **SAML** сделайте следующее. Выберите **Azure Active Directory (Azure AD)** из раскрывающегося списка **Добавление поставщика удостоверений**.

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/addon4.png)

1. Выберите уровень подписки **Базовый**.

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/addon5.png)

1. В разделе **Свойства приложения** выполните следующие действия.

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/addon6.png)

    a. Скопируйте значение **App ID URI** (URI кода приложения) и используйте его как **идентификатор, URL-адрес ответа и URL-адрес входа** в разделе **Домены и URL-адреса приложения Kantega SSO for FishEye/Crucible** на портале Azure.

    2. Нажмите кнопку **Далее**.

1. В разделе **Metadata import** (Импорт метаданных) выполните следующие действия.

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/addon7.png)

    a. Щелкните **Metadata file on my computer** (Файл метаданных на моем компьютере) и передайте файл метаданных, который вы скачали с портала Azure.

    2. Нажмите кнопку **Далее**.

1. В разделе **Name and SSO location** (Имя и расположение единого входа) выполните следующее.

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/addon8.png)

    a. В текстовом поле **Provider Name** (Имя поставщика) введите имя поставщика (например, Azure AD).

    2. Нажмите кнопку **Далее**.

1. Проверьте сертификат для подписи и нажмите кнопку **Next** (Далее).   

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/addon9.png)

1. В разделе **FishEye user accounts** (Учетные записи пользователей FishEye) выполните следующие действия.

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/addon10.png)

    a. Щелкните переключатель **Create users in FishEye's internal Directory if needed** (При необходимости создать пользователей во внутреннем каталоге FishEye) и введите соответствующее имя группы пользователей (это может быть несколько групп, разделенных запятой).

    2. Нажмите кнопку **Далее**.

1. Нажмите кнопку **Готово**

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/addon11.png)

1. В разделе **Known domains for Azure AD** (Известные домены для Azure AD) выполните следующие действия.  

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/addon12.png)

    a. Щелкните **Known domains** (Известные домены) на левой панели страницы.

    2. Введите имя домена в текстовое поле **Known domains** (Известные домены).

    c. Выберите команду **Сохранить**.  

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/kantegassoforfisheyecrucible-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/kantegassoforfisheyecrucible-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/kantegassoforfisheyecrucible-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/kantegassoforfisheyecrucible-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    2. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-a-kantega-sso-for-fisheyecrucible-test-user"></a>Создание тестового пользователя Kantega SSO for FishEye/Crucible

Чтобы пользователи Azure AD могли выполнять вход в FishEye/Crucible, они должны быть подготовлены в Freshservice. В Kantega SSO for FishEye/Crucible подготовка выполняется вручную.

**Чтобы подготовить учетную запись пользователя, сделайте следующее:**

1. Войдите на локальный сервер Crucible в качестве администратора.

1. Наведите указатель мыши на шестеренку и щелкните **Users** (Пользователи).

    ![Добавление сотрудника](./media/kantegassoforfisheyecrucible-tutorial/user1.png) 

1. В разделе **Users** (Пользователи) щелкните **Add user** (Добавить пользователя).

    ![Добавление сотрудника](./media/kantegassoforfisheyecrucible-tutorial/user2.png)

1. На странице диалогового окна **Add New User** (Добавление нового пользователя) выполните следующие действия.

    ![Добавление сотрудника](./media/kantegassoforfisheyecrucible-tutorial/user3.png) 

    a. В текстовом поле **Username** (Имя пользователя) введите электронный адрес пользователя, например Brittasimon@contoso.com.
    
    2. В текстовом поле **Display Name** (Отображаемое имя) введите отображаемое имя пользователя, например Britta Simon.
    
    c. В текстовом поле **Email address** (Адрес электронной почты) введите адрес электронной почты пользователя, например Brittasimon@contoso.com.

    d. В текстовом поле **Password** (Пароль) введите пароль пользователя.  

    д. В текстовом поле **Confirm Password** (Подтверждение пароля) введите пароль еще раз.

    Е. Щелкните **Добавить**.   

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Kantega SSO for FishEye/Crucible.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в Kantega SSO for FishEye/Crucible, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. Из списка приложений выберите **Kantega SSO for FishEye/Crucible**.

    ![Настройка единого входа](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "Kantega SSO for FishEye/Crucible" на панели доступа, вы автоматически войдете в приложение Kantega SSO for FishEye/Crucible.
Дополнительные сведения о панели доступа см. в статье с [общими сведениями о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_01.png
[2]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_02.png
[3]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_03.png
[4]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_04.png

[100]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_100.png

[200]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_200.png
[201]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_201.png
[202]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_202.png
[203]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_203.png

