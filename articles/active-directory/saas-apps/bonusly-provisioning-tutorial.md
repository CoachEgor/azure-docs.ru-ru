---
title: Руководство по настройке Bonusly для автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт
description: Сведения о настройке Azure Active Directory для автоматической подготовки и отзыва учетных записей пользователей в Bonusly.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 879b0ee9-042a-441b-90a7-8c364d62426a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7694e441a59680a9b9544d3479100c1f779964ff
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "77058951"
---
# <a name="tutorial-configure-bonusly-for-automatic-user-provisioning"></a>Руководство по настройке Bonusly для автоматической подготовки пользователей

В этом руководстве описаны шаги, которые нужно выполнить в Bonusly и Azure Active Directory (Azure AD), чтобы настроить Azure AD для автоматической подготовки и отзыва пользователей и групп в Bonusly.

> [!NOTE]
> В этом руководстве рассматривается соединитель, созданный на базе службы подготовки пользователей Azure AD. Подробные сведения о том, что делает эта служба, как она работает, и часто задаваемые вопросы см. в статье [Автоматическая подготовка пользователей и ее отзыв для приложений SaaS в Azure Active Directory](../app-provisioning/user-provisioning.md).

## <a name="prerequisites"></a>Предварительные условия

Сценарий, описанный в этом руководстве, предполагает, что у вас уже имеется:

* клиент Azure AD;
* [клиент Bonusly](https://bonus.ly/pricing);
* учетная запись пользователя в Bonusly с разрешениями администратора.

> [!NOTE]
> Интеграция подготовки Azure AD зависит от [Rest API Bonusly](https://konghq.com/solutions/gateway/), доступного для разработчиков Bonusly.

## <a name="adding-bonusly-from-the-gallery"></a>Добавление Bonusly из коллекции

Перед настройкой Bonusly для автоматической подготовки пользователей в Azure AD необходимо добавить Bonusly из коллекции приложений Azure AD в список управляемых приложений SaaS.

**Чтобы добавить Bonusly из коллекции приложений Azure AD, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Bonusly**, выберите **Bonusly** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Bonusly в списке результатов](common/search-new-app.png)

## <a name="assigning-users-to-bonusly"></a>Назначение пользователей в Bonusly

В Azure Active Directory для определения того, какие пользователи должны получать доступ к выбранным приложениям, используется концепция, называемая "назначение". В контексте автоматической подготовки синхронизированы будут только те пользователи и группы, которые были "назначены" приложению в Azure AD. 

Перед настройкой и включением автоматической подготовки пользователей нужно решить, какие пользователи или группы в Azure AD должны иметь доступ к Bonusly. Когда данный вопрос будет решен, этих пользователей или группы можно будет назначить приложению Bonusly, следуя инструкциям:

* [Назначение корпоративному приложению пользователя или группы](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-bonusly"></a>Важные рекомендации по назначению пользователей в Bonusly

* Рекомендуется назначить одного пользователя Azure AD в Bonusly для тестирования конфигурации автоматической подготовки пользователей. Дополнительные пользователи и/или группы можно назначить позднее.

* При назначении пользователя в Bonusly в диалоговом окне назначения необходимо выбрать действительную роль для конкретного приложения (если доступно). Пользователи с ролью **Доступ по умолчанию** исключаются из подготовки.

## <a name="configuring-automatic-user-provisioning-to-bonusly"></a>Настройка автоматической подготовки пользователей в Bonusly

В этом разделе описывается, как настроить службу подготовки Azure AD для создания, обновления и отключения пользователей и групп в Bonusly на основе их назначений в Azure AD.

> [!TIP]
> Для Bonusly также можно включить единый вход на основе SAML. Для этого выполните инструкции, приведенные в статье [Руководство. Интеграция Azure Active Directory с Bonusly](bonus-tutorial.md). Единый вход можно настроить независимо от автоматической подготовки пользователей, хотя эти две возможности дополняют друг друга.

### <a name="to-configure-automatic-user-provisioning-for-bonusly-in-azure-ad"></a>Чтобы настроить автоматическую подготовку пользователей в Bonusly, сделайте следующее.

1. Войдите в [портал Azure](https://portal.azure.com) и выберите **корпоративные приложения**, выберите **все приложения**, а затем выберите **премия**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Bonusly**.

    ![Ссылка на Bonusly в списке "Приложения"](common/all-applications.png)

3. Выберите вкладку **Подготовка**.

    ![Подготовка Bonusly](./media/bonusly-provisioning-tutorial/ProvisioningTab.png)

4. Для параметра **Режим подготовки к работе** выберите значение **Automatic** (Автоматически).

    ![Подготовка Bonusly](./media/bonusly-provisioning-tutorial/ProvisioningCredentials.png)

5. В разделе **Учетные данные администратора** укажите **секретный токен** учетной записи Bonusly, как описано в шаге 6.

    ![Подготовка Bonusly](./media/bonusly-provisioning-tutorial/secrettoken.png)

6. **Секретный токен** учетной записи Bonusly можно найти в разделе **Admin > Company > Integrations** (Администрирование > Организация > Интеграции). В разделе **If you want to code** (Если вы хотите написать код ) выберите **API> Create New API Access Token** (Создать новый токен доступа к API), чтобы создать новый секретный токен.

    ![Подготовка Bonusly](./media/bonusly-provisioning-tutorial/BonuslyIntegrations.png)

    ![Подготовка Bonusly](./media/bonusly-provisioning-tutorial/BonsulyRestApi.png)

    ![Подготовка Bonusly](./media/bonusly-provisioning-tutorial/CreateToken.png)

7. На следующем экране введите имя маркера доступа в предоставленном текстовом поле, а затем нажмите кнопку **Create Api Key** (Создать ключ API). Новый маркер доступа будет отображаться во всплывающем окне в течение нескольких секунд.

    ![Подготовка Bonusly](./media/bonusly-provisioning-tutorial/Token01.png)

    ![Подготовка Bonusly](./media/bonusly-provisioning-tutorial/Token02.png)

8. После заполнения полей, указанных на шаге 5, щелкните **Проверить подключение**, чтобы убедиться, что Azure AD может подключиться к Bonusly. Если установить подключение не удалось, убедитесь, что у учетной записи Bonusly есть разрешения администратора, и повторите попытку.

    ![Подготовка Bonusly](./media/bonusly-provisioning-tutorial/TestConnection.png)

9. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки, а также установите флажок **Send an email notification when a failure occurs** (Отправить уведомление по электронной почте при сбое).

    ![Подготовка Bonusly](./media/bonusly-provisioning-tutorial/EmailNotification.png)

10. Нажмите кнопку **Сохранить**.

11. В разделе **Сопоставления** выберите **Synchronize Azure Active Directory Users to Bonusly** (Синхронизировать пользователей Azure Active Directory с Bonusly).

    ![Подготовка Bonusly](./media/bonusly-provisioning-tutorial/UserMappings.png)

12. В разделе **Сопоставление атрибутов** просмотрите пользовательские атрибуты, которые синхронизированы из Azure AD в Bonusly. Атрибуты, выбранные как свойства **сопоставления**, используются для сопоставления учетных записей пользователей в Bonusly для операций обновления. Нажмите кнопку **Сохранить**, чтобы зафиксировать все изменения.

    ![Подготовка Bonusly](./media/bonusly-provisioning-tutorial/UserAttributeMapping.png)

13. Чтобы настроить фильтры области, ознакомьтесь со следующими инструкциями, предоставленными в [руководстве по фильтрам области](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

14. Чтобы включить службу подготовки Azure AD для Bonusly, в разделе **Параметры** измените значение параметра **Состояние подготовки** на **Включено**.

    ![Подготовка Bonusly](./media/bonusly-provisioning-tutorial/ProvisioningStatus.png)

15. Определите пользователей или группы для подготовки в Bonusly, выбрав нужные значения в поле **Область** в разделе **Параметры**.

    ![Подготовка Bonusly](./media/bonusly-provisioning-tutorial/ScopeSync.png)

16. Когда будете готовы выполнить подготовку, нажмите кнопку **Сохранить**.

    ![Подготовка Bonusly](./media/bonusly-provisioning-tutorial/SaveProvisioning.png)

После этого начнется начальная синхронизация пользователей и (или) групп, определенных в поле **Область** раздела **Параметры**. Начальная синхронизация занимает больше времени, чем последующие операции синхронизации. Если служба запущена, они выполняются примерно каждые 40 минут. В разделе **Сведения о синхронизации** можно отслеживать ход выполнения и переходить по ссылкам для просмотра отчетов о подготовке, в которых зафиксированы все действия, выполняемые службой подготовки Azure AD с приложением Bonusly.

Дополнительные сведения о чтении журналов подготовки Azure AD см. в руководстве по [отчетам об автоматической подготовке учетных записей](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Управление подготовкой учетных записей пользователей для корпоративных приложений](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Дальнейшие действия

* [Сведения о просмотре журналов и получении отчетов о действиях по подготовке](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/bonusly-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/bonusly-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/bonusly-provisioning-tutorial/tutorial_general_03.png
