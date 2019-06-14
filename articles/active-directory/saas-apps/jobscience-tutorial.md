---
title: Руководство по Интеграция Azure Active Directory с Jobscience | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении Jobscience.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 77282dcc-bbe2-4728-953d-adb4ab6a713b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8199f106c234e216a0982dc9e51413ccf30ae93a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60268695"
---
# <a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Руководство по Интеграция Azure Active Directory с Jobscience

В этом руководстве описано, как интегрировать Jobscience с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением Jobscience обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к Jobscience.
- Вы можете включить автоматический вход пользователей в Jobscience (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Технические условия

Чтобы настроить интеграцию Azure AD с Jobscience, вам потребуется:

- подписка Azure AD;
- подписка Jobscience с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по этой ссылке: [пробная версия](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Jobscience из коллекции.
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-jobscience-from-the-gallery"></a>Добавление Jobscience из коллекции
Чтобы настроить интеграцию Jobscience с Azure AD, необходимо добавить Jobscience из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Jobscience из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **Jobscience**.

    ![Создание тестового пользователя Azure AD](./media/jobscience-tutorial/tutorial_jobscience_search.png)

1. На панели результатов выберите **Jobscience** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Создание тестового пользователя Azure AD](./media/jobscience-tutorial/tutorial_jobscience_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в Jobscience с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходима информация о том, какой пользователь в Jobscience соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Jobscience.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Jobscience.

Чтобы настроить и проверить единый вход Azure AD в Jobscience, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Jobscience](#creating-a-jobscience-test-user)** требуется для того, чтобы в Jobscience существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Jobscience.

**Чтобы настроить единый вход Azure AD в Jobscience, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **Jobscience** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/jobscience-tutorial/tutorial_jobscience_samlbase.png)

1. В разделе **Домены и URL-адреса приложения Jobscience** выполните следующие действия.

    ![Настройка единого входа](./media/jobscience-tutorial/tutorial_jobscience_url.png)

    В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `http://<company name>.my.salesforce.com`.
    
    > [!NOTE] 
    > Это значение приведено для справки. Вместо него необходимо указать фактический URL-адрес входа. Данное значение можно получить у [группы поддержки клиентов Jobscience](https://www.jobscience.com/support) или из профиля единого входа, который вы создадите. Это описано далее в этом руководстве. 
 
1. В разделе **Сертификат для подписи токена SAML** щелкните **Certificate (Base64)** (Сертификат (Base64)), а затем сохраните файл сертификата на компьютере.

    ![Настройка единого входа](./media/jobscience-tutorial/tutorial_jobscience_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/jobscience-tutorial/tutorial_general_400.png)

1. В разделе **Конфигурация Jobscience** щелкните **Настроить Jobscience**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода, идентификатор сущности SAML и URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Настройка единого входа](./media/jobscience-tutorial/tutorial_jobscience_configure.png) 

1. Выполните вход на веб-сайт компании Jobscience в качестве администратора.

1. Перейдите в раздел **Настройка**.
   
   ![Настройка](./media/jobscience-tutorial/IC784358.png "Настройка")

1. В области навигации слева в разделе **Administer** (Администрирование) щелкните **Domain Management** (Управление доменами), чтобы развернуть соответствующий раздел, а затем щелкните **My Domain** (Мой домен), чтобы открыть страницу **My Domain** (Мой домен). 
   
   ![Мой домен](./media/jobscience-tutorial/ic767825.png "Мой домен")

1. Чтобы проверить правильность настройки домена, убедитесь, что он находится на этапе **Step 4 Deployed to Users** (Шаг 4. Развернут для пользователей), и просмотрите раздел **My Domain Settings** (Параметры моего домена).

    ![Домен, развернутый для пользователя](./media/jobscience-tutorial/ic784377.png "Domain Deployed to User")

1. На корпоративном сайте Jobscience щелкните **Security Controls** (Средства управления безопасностью), а затем выберите **Single Sign-On Settings** (Параметры единого входа).
    
    ![Security Controls](./media/jobscience-tutorial/ic784364.png "Security Controls") (Средства управления безопасностью)

1. В разделе **Параметры единого входа** сделайте следующее:
    
    ![Параметры единого входа](./media/jobscience-tutorial/ic781026.png "Параметры единого входа")
    
    a. Установите флажок **SAML включен**.

    2\. Нажмите кнопку **Создать**.

1. В диалоговом окне **Изменение параметров единого входа SAML** выполните следующие действия.
    
    ![Параметры единого входа SAML](./media/jobscience-tutorial/ic784365.png "Параметры единого входа SAML")
    
    a. В текстовом поле **Имя** введите имя конфигурации.

    2\. В текстовое поле **Issuer** (Издатель) вставьте значение **SAML Entity ID** (Идентификатор сущности SAML), скопированное на портале Azure.

    c. В текстовое поле **Entity id** (Идентификатор сущности) введите `https://salesforce-jobscience.com`.

    d. Чтобы отправить сертификат Azure AD, нажмите кнопку **Обзор** .

    д. В поле **SAML Identity Type** (Тип удостоверения SAML) выберите значение **Assertion contains the Federation ID from the User object** (Проверочное утверждение содержит идентификатор федерации из объекта User).

    Е. В поле **SAML Identity Location** (Расположение удостоверения SAML) выберите значение **Identity is in the NameIdentfier element of the Subject statement** (Удостоверение находится в элементе NameIdentifier оператора Subject).

    ж. В текстовое поле **Identity Provider Login URL** (URL-адрес входа IdP) вставьте значение **SAML Single Sign-On Service URL** (URL-адрес службы единого входа SAML), скопированное на портале Azure.

    h. В текстовое поле **Identity Provider Logout URL** (URL-адрес выхода IdP) вставьте значение **URL-адрес выхода**, скопированное на портале Azure.

    i. Выберите команду **Сохранить**.

1. В области навигации слева в разделе **Administer** (Администрирование) щелкните **Domain Management** (Управление доменами), чтобы развернуть соответствующий раздел, а затем щелкните **My Domain** (Мой домен), чтобы открыть страницу **My Domain** (Мой домен). 
    
    ![Мой домен](./media/jobscience-tutorial/ic767825.png "Мой домен")

1. На странице **My Domain** (Мой домен) в разделе **Login Page Branding** (Фирменная символика страницы входа) нажмите кнопку **Edit** (Изменить).
    
    ![Фирменная символика страницы входа](./media/jobscience-tutorial/ic767826.png "Фирменная символика страницы входа")

1. На странице **Login Page Branding** (Фирменная символика страницы входа) в разделе **Authentication Service** (Служба аутентификации) отображается имя **SAML SSO Settings** (Параметры единого входа SAML). Выберите его, а затем нажмите кнопку **Сохранить**.
    
    ![Фирменная символика страницы входа](./media/jobscience-tutorial/ic784366.png "Фирменная символика страницы входа")

1. Чтобы получить URL-адрес единого входа, инициированный поставщиком услуг, в разделе меню **Security Controls** (Средства управления безопасностью) щелкните **Single Sign On settings** (Параметры единого входа).

    ![Security Controls](./media/jobscience-tutorial/ic784368.png "Security Controls") (Средства управления безопасностью)
    
    Выберите профиль единого входа, созданный на предыдущем этапе. На этой странице отображается URL-адрес единого входа для вашей организации (например, [https://companyname.my.salesforce.com?so=companyid](https://companyname.my.salesforce.com?so=companyid)).    

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/jobscience-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/jobscience-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/jobscience-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/jobscience-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    2\. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-a-jobscience-test-user"></a>Создание тестового пользователя Jobscience

Чтобы пользователи Azure AD могли выполнять вход в Jobscience, они должны быть подготовлены в Jobscience. В случае с Jobscience подготовка выполняется вручную.

>[!NOTE]
>Вы можете использовать любые другие инструменты создания учетных записей пользователя Jobscience или API, предоставляемые Jobscience для подготовки учетных записей пользователя Azure Active Directory.
>  
        
**Чтобы настроить подготовку учетных записей пользователей, выполните следующие действия.**

1. Выполните вход на веб-сайт **Jobscience** компании в качестве администратора.

1. Перейдите в раздел "Настройка".
   
   ![Настройка](./media/jobscience-tutorial/ic784358.png "Настройка")
1. Выберите **Manage Users \> Users** (Управление пользователями > Пользователи).
   
   ![Пользователи](./media/jobscience-tutorial/ic784369.png "Пользователи")
1. Щелкните **Новый пользователь**.
   
   ![Все пользователи](./media/jobscience-tutorial/ic784370.png "Все пользователи")
1. В диалоговом окне **Изменить пользователя** выполните следующие действия.
   
   ![Изменение пользователя](./media/jobscience-tutorial/ic784371.png "Изменение пользователя")
   
   a. В текстовое поле **First Name** (Имя) введите имя пользователя, например Britta.
   
   2\. В текстовое поле **Last Name** (Фамилия) введите фамилию пользователя, например Simon.
   
   c. В текстовое поле **Alias** (Псевдоним) введите имя пользователя, например brittas.

   d. В текстовом поле **Email** (Электронная почта) введите адрес электронной почты пользователя, например Brittasimon@contoso.com.

   д. В текстовом поле **User Name** (Имя пользователя) укажите имя пользователя, например Brittasimon@contoso.com.

   Е. В текстовом поле **Nick Name** (Псевдоним) укажите псевдоним пользователя, например Simon.

   ж. Выберите команду **Сохранить**.

    
> [!NOTE]
> Владелец учетной записи Azure Active Directory получит по электронной почте сообщение со ссылкой для активации учетной записи.

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Jobscience.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в Jobscience, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. Из списка приложений выберите **Jobscience**.

    ![Настройка единого входа](./media/jobscience-tutorial/tutorial_jobscience_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "Jobscience" на панели доступа, вы автоматически войдете в приложение Jobscience.
См. дополнительные сведения о [панели доступа](../user-help/active-directory-saas-access-panel-introduction.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/jobscience-tutorial/tutorial_general_01.png
[2]: ./media/jobscience-tutorial/tutorial_general_02.png
[3]: ./media/jobscience-tutorial/tutorial_general_03.png
[4]: ./media/jobscience-tutorial/tutorial_general_04.png

[100]: ./media/jobscience-tutorial/tutorial_general_100.png

[200]: ./media/jobscience-tutorial/tutorial_general_200.png
[201]: ./media/jobscience-tutorial/tutorial_general_201.png
[202]: ./media/jobscience-tutorial/tutorial_general_202.png
[203]: ./media/jobscience-tutorial/tutorial_general_203.png

