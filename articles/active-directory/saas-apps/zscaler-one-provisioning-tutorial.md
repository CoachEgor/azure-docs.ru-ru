---
title: Руководство. Настройка Zscaler для автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт
description: Узнайте, как настроить Azure Active Directory для автоматической инициализации и отмены предоставления учетных записей пользователей, Zscaler их.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 72f6ba2b-73ed-420a-863a-aff672f26fa3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 87413932acee576934ee50b59546371b03ceaf7e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "77064178"
---
# <a name="tutorial-configure-zscaler-one-for-automatic-user-provisioning"></a>Руководство. Настройка Zscaler для автоматической подготовки пользователей

В этом руководстве показано, как выполнить действия в Zscaler One и Azure Active Directory (Azure AD), чтобы настроить Azure AD для автоматической инициализации и отмены наполнения пользователей и групп, Zscaler их.

> [!NOTE]
> В этом руководстве рассматривается соединитель, созданный на основе службы подготовки пользователей Azure AD. Сведения о том, что делает эта служба, как она работает, а также часто задаваемые вопросы см. в статье [Автоматическая подготовка пользователей и их отзыв для приложений SaaS в Azure Active Directory](../active-directory-saas-app-provisioning.md).


## <a name="prerequisites"></a>Предварительные требования

Сценарий, описанный в этом руководстве, предполагает, что у вас уже имеется:

* Клиент Azure AD.
* Zscaler один клиент.
* Учетная запись пользователя в Zscaler одна с разрешениями администратора.

> [!NOTE]
> Интеграция подготовки Azure AD зависит от Zscaler одного SCIM API. Этот API доступен для Zscaler одного разработчика учетных записей с корпоративным пакетом.

## <a name="add-zscaler-one-from-the-azure-marketplace"></a>Добавление Zscaler из Azure Marketplace

Перед настройкой Zscaler для автоматической подготовки пользователей с помощью Azure AD добавьте Zscaler один из Azure Marketplace в список управляемых приложений SaaS.

Чтобы добавить Zscaler из Marketplace, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com) в области навигации слева выберите **Azure Active Directory**.

    ![Значок Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна выберите **Новое приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Zscaler 1** и выберите **Zscaler One** на панели результатов. Нажмите **Добавить**, чтобы добавить приложение.

    ![Zscaler One в списке результатов](common/search-new-app.png)

## <a name="assign-users-to-zscaler-one"></a>Назначить пользователей Zscaler один

В Azure Active Directory для определения того, какие пользователи должны получать доступ к выбранным приложениям, используется концепция, называемая *назначением*. В контексте автоматической подготовки синхронизируются только те пользователи и группы, которые были назначены приложению в Azure AD.

Перед настройкой и включением автоматической подготовки пользователей решите, какие пользователи или группы в Azure AD должны иметь доступ к Zscaler. Чтобы назначить этих пользователей или группы, Zscaler их, следуйте инструкциям в статье [Назначение пользователя или группы корпоративному приложению](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-zscaler-one"></a>Важные советы по назначению пользователей Zscaler

* Рекомендуется назначить одного пользователя Azure AD, чтобы Zscaler его для проверки конфигурации автоматической подготовки пользователей. Дополнительных пользователей или группы можно будет назначить позже.

* При назначении пользователя Zscaler выберите любую допустимую роль конкретного приложения, если она доступна, в диалоговом окне Назначение. Пользователи с ролью **Доступ по умолчанию** исключаются из подготовки.

## <a name="configure-automatic-user-provisioning-to-zscaler-one"></a>Настройка автоматической подготовки пользователей для Zscaler

В этом разделе описывается процедура настройки службы подготовки Azure AD. Используйте его для создания, обновления и отключения пользователей или групп в Zscaler на основе назначения пользователей или групп в Azure AD.

> [!TIP]
> Вы также можете включить единый вход на основе SAML для Zscaler. Следуйте инструкциям в руководстве по [единому единого входа Zscaler](zscaler-One-tutorial.md). Единый вход можно настроить независимо от автоматической подготовки пользователей, хотя эти две функции дополняют друг друга.

### <a name="configure-automatic-user-provisioning-for-zscaler-one-in-azure-ad"></a>Настройка автоматической подготовки пользователей для Zscaler 1 в Azure AD

1. Войдите на [портал Azure](https://portal.azure.com). Выберите **корпоративные приложения**  >  **все приложения**  >  ,**Zscaler одно**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. Из списка приложений выберите **Zscaler One**.

    ![Ссылка на Zscaler в списке "приложения"](common/all-applications.png)

3. Выберите вкладку **Подготовка**.

    ![Zscaler одну подготовку](./media/zscaler-one-provisioning-tutorial/provisioning-tab.png)

4. Для параметра **Режим подготовки к работе** выберите значение **Automatic** (Автоматически).

    ![Zscaler один режим подготовки](./media/zscaler-one-provisioning-tutorial/provisioning-credentials.png)

5. В разделе **учетные данные администратора** введите **URL-адрес клиента** и **токен секрета** с параметрами для вашей учетной записи Zscaler, как описано в шаге 6.

6. Чтобы получить URL-адрес клиента и маркер секрета, перейдите в раздел **Администрирование**  >  **Параметры проверки подлинности** в Zscaler один пользовательский интерфейс портала. В разделе **Authentication Type** (Тип проверки подлинности) выберите **SAML**.

    ![Zscaler один параметр проверки подлинности](./media/zscaler-one-provisioning-tutorial/secret-token-1.png)

    а. Выберите **настроить SAML** , чтобы открыть диалоговое окно **Настройка параметров SAML** .

    ![Zscaler. одна настройка SAML](./media/zscaler-one-provisioning-tutorial/secret-token-2.png)

    b. Выберите **включить подготовку на основе scim** , чтобы получить параметры в **базовом URL-адресе** и **токене носителя**. Затем сохраните параметры. Скопируйте параметр **базового URL-адреса** в **URL-адрес клиента** в портал Azure. Скопируйте параметр **токена носителя** в **маркер секрета** в портал Azure.

7. После заполнения полей, показанных на шаге 5, выберите **проверить подключение** , чтобы убедиться, что Azure AD может подключиться к Zscaler. В случае сбоя подключения убедитесь, что у вашей учетной записи Zscaler есть права администратора, и повторите попытку.

    ![Zscaler одно тестовое подключение](./media/zscaler-one-provisioning-tutorial/test-connection.png)

8. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки. Установите флажок **Отправить уведомление по электронной почте при сбое**.

    ![Zscaler одно уведомление по электронной почте](./media/zscaler-one-provisioning-tutorial/notification.png)

9. Нажмите кнопку **Сохранить**.

10. В разделе **сопоставления** выберите **синхронизировать Azure Active Directory пользователей, чтобы Zscaler их**.

    ![Zscaler синхронизацию одного пользователя](./media/zscaler-one-provisioning-tutorial/user-mappings.png)

11. Изучите пользовательские атрибуты, которые синхронизированы из Azure AD, чтобы Zscaler их в разделе **сопоставления атрибутов** . Атрибуты, выбранные как свойства **Matching** , используются для сопоставления учетных записей пользователей в Zscaler одной для операций обновления. Нажмите **Сохранить**, чтобы сохранить все изменения.

    ![Zscaler один соответствующий атрибут пользователя](./media/zscaler-one-provisioning-tutorial/user-attribute-mappings.png)

12. В разделе **сопоставления** выберите **синхронизировать Azure Active Directory группы, чтобы Zscaler их**.

    ![Zscaler одну группу синхронизации](./media/zscaler-one-provisioning-tutorial/group-mappings.png)

13. Проверьте атрибуты группы, которые синхронизированы из Azure AD, чтобы Zscaler одну из них в разделе **сопоставления атрибутов** . Атрибуты, выбранные как свойства **Matching** , используются для сопоставления групп в Zscaler одной для операций обновления. Нажмите **Сохранить**, чтобы сохранить все изменения.

    ![Zscaler один соответствующий атрибут группы](./media/zscaler-one-provisioning-tutorial/group-attribute-mappings.png)

14. Чтобы настроить фильтры области, ознакомьтесь с инструкциями в статье [Подготовка приложений на основе атрибутов с использованием фильтров области](./../active-directory-saas-scoping-filters.md).

15. Чтобы включить службу подготовки Azure AD для Zscaler, в разделе **Параметры** измените значение параметра **состояние подготовки** на **включено**.

    ![Zscaler одно состояние подготовки](./media/zscaler-one-provisioning-tutorial/provisioning-status.png)

16. Определите пользователей или группы, которые вы хотите подготавливать для Zscaler. В разделе **Параметры** выберите нужные значения в разделе **Область**.

    ![Zscaler одну область](./media/zscaler-one-provisioning-tutorial/scoping.png)

17. Когда будете готовы выполнить подготовку, нажмите кнопку **Сохранить**.

    ![Zscaler одно сохранение](./media/zscaler-one-provisioning-tutorial/save-provisioning.png)

После этого начнется начальная синхронизация пользователей или групп, определенных в поле **Область** раздела **Параметры**. Начальная синхронизация занимает больше времени, чем последующие операции синхронизации. Они происходят примерно каждые 40 минут, пока выполняется служба подготовки Azure AD. 

В разделе **Сведения о синхронизации** можно отслеживать ход выполнения и переходить по ссылкам для просмотра отчета о подготовке. В отчете описаны все действия, выполняемые службой подготовки Azure AD на Zscaler.

Сведения о журналах подготовки Azure AD см. в руководстве [по отчетам об автоматической подготовке учетных записей пользователей](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Managing user account provisioning for enterprise apps in the Azure portal](../app-provisioning/configure-automatic-user-provisioning-portal.md) (Управление подготовкой учетных записей пользователей для корпоративных приложений на портале Azure)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Дальнейшие действия

* [Сведения о просмотре журналов и получении отчетов о действиях по подготовке](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-03.png
