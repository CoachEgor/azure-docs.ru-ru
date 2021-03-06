---
title: Краткое руководство. Настройка единого входа на основе SAML для приложения в арендаторе Azure Active Directory (Azure AD)
description: В этом кратком руководстве описывается процесс настройки единого входа на основе SAML для приложения в арендаторе Azure Active Directory (Azure AD).
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 07/01/2020
ms.author: kenwith
ms.openlocfilehash: 235c5c19c9a6e587106b3fd41691a3f775caa84f
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "94656621"
---
# <a name="quickstart-set-up-saml-based-single-sign-on-sso-for-an-application-in-your-azure-active-directory-azure-ad-tenant"></a>Краткое руководство. Настройка единого входа на основе SAML для приложения в арендаторе Azure Active Directory (Azure AD)

Чтобы использовать упрощенный вход пользователей, настройте единый вход для приложения, которое вы добавили в арендатор Azure Active Directory (Azure AD). Когда вы настроите единый вход пользователи смогут входить в приложение, используя свои учетные данные Azure AD. Возможности единого входа включены в бесплатную версию Azure AD.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить единый вход для приложения, которое вы добавили в арендатор Azure AD, вам потребуется следующее:

- Учетная запись Azure с активной подпиской. [Создайте учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) бесплатно.
- Одна из следующих ролей: глобальный администратор, администратор облачных приложений, администратор приложений или владелец субъекта-службы.
- Приложение, которое поддерживает единый вход и было предварительно настроено и добавлено в коллекцию Azure AD. Большинство приложений могут использовать Azure AD для единого входа. Приложения в коллекции Azure AD уже предварительно настроены. Если приложение не указано в списке или является специально разработанным приложением, вы все равно сможете использовать его с Azure AD. Для этого просмотрите руководства и другую документацию в оглавлении. В этом кратком руководстве основное внимание уделяется приложениям, которые были предварительно настроены для единого входа и добавлены в коллекцию Azure AD разработчиками приложений.
- Необязательное действие: Знакомство с инструкциями по [просмотру приложений](view-applications-portal.md).
- Необязательное действие: Знакомство с инструкциями по [добавлению приложения](add-application-portal.md).
- Необязательное действие: Знакомство с инструкциями по [настройке приложения](add-application-portal-configure.md).
- Необязательное действие: [Назначение пользователей приложению](add-application-portal-assign-users.md).


>[!IMPORTANT]
>Не используйте рабочую среду для проверки инструкций, приведенных в этом кратком руководстве.

## <a name="enable-single-sign-on-for-an-app"></a>Включение единого входа для приложения

После того как приложение будет добавлено в арендатор Azure AD, отобразится страница обзора. Если вы настраиваете уже добавленное приложение, ознакомьтесь с первым кратким руководством. Оно поможет просмотреть приложения, добавленные в арендатор. 

Чтобы настроить единый вход для приложения, сделайте следующее:

1. На портале Azure AD выберите элемент **Корпоративные приложения**. Найдите и выберите приложение, для которого нужно настроить единый вход.
1. В разделе **Управление** выберите **Единый вход**, чтобы открыть область **Единый вход** для изменения.

    > [!IMPORTANT]
    > Если для единого входа приложение использует стандарт OpenID Connect (OIDC), в области навигации не будет отображаться параметр единого входа. Сведения о настройке единого входа на основе OIDC см. в соответствующем кратком руководстве.

    :::image type="content" source="media/add-application-portal-setup-sso/configure-sso.png" alt-text="Снимок экрана со страницей настройки единого входа на портале Azure AD.":::

1. Выберите **SAML**, чтобы открыть страницу настройки единого входа. В этом примере мы настраиваем для единого входа приложение GitHub. После настройки приложения GitHub пользователи смогут входить в GitHub с помощью своих учетных данных из арендатора Azure AD.

    :::image type="content" source="media/add-application-portal-setup-sso/github-sso.png" alt-text="Снимок экрана со страницей настройки единого входа в GitHub.":::

1. Процесс настройки приложения для использования Azure AD для единого входа на основе SAML зависит от приложения. Доступна ссылка на руководство для GitHub. Инструкции для других приложений см. в статье [Руководства по интеграции приложений SaaS с Azure Active Directory](/azure/active-directory/saas-apps/).
1. Следуйте указаниям по настройке единого входа для приложения. Многие приложения имеют определенные требования к подписке для включения единого входа. Например, приложение GitHub требует наличия подписки Enterprise.
    > [!TIP]
    > Дополнительные сведения о параметрах конфигурации SAML см. в разделе [Настройка единого входа на основе SAML](configure-saml-single-sign-on.md).

    :::image type="content" source="media/add-application-portal-setup-sso/github-pricing.png" alt-text="Снимок экрана с параметром единого входа в подписке Enterprise на странице цен на GitHub.":::

> [!TIP]
> Вы можете автоматизировать управление приложениями с помощью API Graph — см. [соответствующую статью](/graph/application-saml-sso-configure-api).


## <a name="clean-up-resources"></a>Очистка ресурсов

Завершив работу с этой серией кратких руководств, удалите приложение, чтобы очистить тестовый арендатор. Сведения о том, как удалить приложение, см. в последнем кратком руководстве этой серии, [Удаление приложения](delete-application-portal.md).

## <a name="next-steps"></a>Дальнейшие действия

Чтобы узнать, как удалить приложение, перейдите к следующей статье.
> [!div class="nextstepaction"]
> [Удаление приложения](delete-application-portal.md)