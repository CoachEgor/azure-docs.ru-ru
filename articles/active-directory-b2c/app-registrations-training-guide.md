---
title: Новые возможности Регистрация приложений в Azure AD B2C
description: Общие сведения о новом интерфейсе регистрации приложений в Azure AD B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 05/25/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 7c9e3344ed819f607feeab88ac43a9dc286a61e0
ms.sourcegitcommit: 6172a6ae13d7062a0a5e00ff411fd363b5c38597
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2020
ms.locfileid: "97109102"
---
# <a name="the-new-app-registrations-experience-for-azure-active-directory-b2c"></a>Новый интерфейс Регистрация приложений для Azure Active Directory B2C

Новый интерфейс **[Регистрация приложений](https://aka.ms/b2cappregistrations)** для Azure Active Directory B2C (Azure AD B2C) теперь является общедоступным. Если вы знакомы с **приложениями** для регистрации приложений для Azure AD B2C, которые здесь называются «устаревшими», это руководство поможет вам приступить к работе с новым интерфейсом.

## <a name="overview"></a>Обзор
Ранее вам пришлось управлять Azure AD B2C приложениями, ориентированными на потребителей, отдельно от остальных приложений, используя прежние возможности. Это означало различные возможности создания приложений в разных местах в Azure.

Новый интерфейс показывает все Azure AD B2C регистрации приложений и регистрации приложений Azure AD в одном месте и обеспечивает единообразный способ управления ими. Чтобы создать клиентское приложение для управления приложением с Microsoft Graph разрешениями для управления ресурсами, необходимо только изучить один из способов.

Вы можете перейти к новому интерфейсу, перейдя к **Регистрация приложений** в Azure AD B2Cном клиенте из **Azure AD B2C** или **Azure Active Directory** служб в портал Azure.

Azure AD B2C Регистрация приложений возможности основаны на общем [интерфейсе регистрации приложений](https://developer.microsoft.com/identity/blogs/new-app-registrations-experience-is-now-generally-available/) для любого клиента Azure AD, но адаптированы для клиентов Azure AD B2C.

## <a name="whats-not-changing"></a>Что не меняется?
- Приложения и соответствующие конфигурации можно найти как есть в новом интерфейсе. Вам не нужно повторно регистрировать приложения, и пользователям ваших приложений не потребуется повторно входить в систему.

> [!NOTE]
> Чтобы просмотреть все ранее созданные приложения, перейдите в колонку **Регистрация приложений** и перейдите на вкладку **все приложения** . В нем будут отображаться приложения, созданные в прежних версиях, новые возможности и созданные в службе Azure AD.

## <a name="key-new-features"></a>Основные новые функции

-   В **едином списке приложений** отображаются все приложения, которые проходят проверку подлинности с помощью Azure AD B2C и Azure AD в одном удобном месте. Кроме того, вы можете воспользоваться преимуществами функций, уже доступных для приложений Azure AD, включая **созданные на** дату, **Сертификаты & состояние секретов** , панель поиска и многое другое.

-   **Объединенная регистрация приложений** позволяет быстро зарегистрировать приложение, будь то клиентское приложение или приложение для доступа к Microsoft Graph.

- Панель **конечные точки** позволяет быстро найти соответствующие конечные точки для вашего сценария, включая конфигурацию OpenID Connect Connect, метаданные SAML, Microsoft Graph API и [конечные точки потока пользователей OAuth 2,0](tokens-overview.md#endpoints).

- **Разрешения API** и **предоставляют API** для более обширной области, разрешений и управления согласия. Теперь для приложения можно назначить разрешения Microsoft Graph и Azure AD Graph.

-   **Владельцы** и **Манифест** теперь доступны для приложений, которые проходят проверку подлинности с помощью Azure AD B2C. Вы можете добавить владельцев для регистраций и непосредственно изменить свойства приложения [с помощью редактора манифестов](../active-directory/develop/reference-app-manifest.md).


## <a name="new-supported-account-types"></a>Новые поддерживаемые типы учетных записей

В новом интерфейсе вы выбрали тип учетной записи поддержки из следующих вариантов:
- Accounts in this organizational directory only (Учетные записи только в этом каталоге организации)
- Учетные записи в любом каталоге организации (любой каталог Azure AD — клиент)
- Учетные записи в любом поставщике удостоверений или в каталоге организации (для проверки подлинности пользователей с помощью потоков пользователей);

Чтобы понять различные типы учетных записей, выберите пункт **помогите мне выбрать** в интерфейсе создания.

В прежних версиях приложения были всегда созданы как приложения, работающие с клиентами. Для этих приложений тип учетной записи устанавливается в **учетные записи в любом поставщике удостоверений или в каталоге организации (для проверки подлинности пользователей с помощью потоков пользователей)**.
> [!NOTE]
> Этот параметр необходим для запуска Azure AD B2C пользовательских потоков для проверки подлинности пользователей этого приложения. Узнайте, [как зарегистрировать приложение для использования с потоками пользователей.](tutorial-register-applications.md)

Этот параметр также можно использовать для использования Azure AD B2C в качестве поставщика службы SAML. [Подробнее.](identity-provider-adfs.md)

## <a name="applications-for-devops-scenarios"></a>Приложения для сценариев DevOps
Можно использовать другие типы учетных записей для создания приложения для управления сценариями DevOps, например с помощью Microsoft Graph для передачи политик инфраструктуры процедур идентификации или подготовки пользователей. Узнайте [, как зарегистрировать приложение Microsoft Graph для управления ресурсами Azure AD B2C](microsoft-graph-get-started.md).

Вы можете увидеть не все разрешения Microsoft Graph, так как многие из этих разрешений не применяются к пользователям потребителя Azure B2C. [Узнайте больше об управлении пользователями с помощью Microsoft Graph](manage-user-accounts-graph-api.md).

## <a name="admin-consent-and-offline_accessopenid-scopes"></a>Согласие администратора и области offline_access + OpenID Connect
<!-- Azure AD B2C doesn't support user consent. That is, when a user signs into an application, the user doesn't see a screen requesting consent for the application permissions. All permissions have to be granted through admin consent.  -->

Область **OpenID Connect** необходима, чтобы Azure AD B2C можно было выполнять вход пользователей в приложение. Область **offline_accessа** необходима для выработки маркеров обновления для пользователя. Эти области были добавлены ранее и по умолчанию предоставил согласие администратора. Теперь можно легко добавить разрешения для этих областей в процессе создания, убедившись в том, что выбран параметр **предоставить согласие администратора в OpenID Connect и offline_access разрешения** . Кроме того, разрешения Microsoft Graph можно добавить с помощью согласия администратора в параметрах **разрешений API** для существующего приложения.

Дополнительные сведения о [разрешениях и согласии](../active-directory/develop/v2-permissions-and-consent.md).

## <a name="platformsauthentication-reply-urlsredirect-uris"></a>Платформы и аутентификация: URL-адреса ответа и URI перенаправления
В прежних версиях управление различными типами платформ было выполнено **в качестве URL** -адресов ответов для Web Apps/API и URI перенаправления для собственных клиентов. "Собственные клиенты" также называются "общедоступными клиентами" и включают приложения для устройств iOS, macOS, Android и других типов мобильных и классических приложений.

В новом интерфейсе URL-адреса ответов и URI перенаправления называются URI перенаправления, которые можно найти в разделе **Проверка подлинности** приложения. Регистрация приложений не ограничены веб-приложениями или собственными приложениями. Вы можете использовать одну регистрацию приложения для всех этих типов платформ, зарегистрировав соответствующие URI перенаправления.

URI перенаправления должны быть связаны с типом приложения — Web или public (мобильные и классические устройства). [Дополнительные сведения о URI перенаправления](../active-directory/develop/quickstart-register-app.md#add-a-redirect-uri)

<!-- Whether an application should be treated as a public client is inferred at run-time from the Redirect URI platform type, if possible. The **Treat application as a public client** setting should be set to **Yes** for flows that might not use a redirect URI, such as ROPC flows. -->

Платформы **iOS/macOS** и **Android** являются типом общедоступного клиента. Они предоставляют простой способ настройки приложений iOS/macOS или Android с соответствующими URI перенаправления для использования с MSAL. Дополнительные сведения о [параметрах конфигурации приложений](../active-directory/develop/msal-client-applications.md).


## <a name="application-certificates--secrets"></a>Сертификаты приложений & секреты

В новом интерфейсе вместо **ключей** для управления сертификатами и секретами используется колонка **& секреты** . Сертификаты & секреты позволяют приложениям опознать себя в службе проверки подлинности при получении маркеров в расположении с веб-адресом (с использованием схемы HTTPS). Для сценариев использования учетных данных клиента при проверке подлинности в Azure AD рекомендуется использовать сертификат вместо секрета клиента. Сертификаты нельзя использовать для проверки подлинности в Azure AD B2C.


## <a name="features-not-applicable-in-azure-ad-b2c-tenants"></a>Функции, неприменимые в клиентах Azure AD B2C
Следующие возможности регистрации приложений Azure AD не применимы к клиентам Azure AD B2C и недоступны.
- **Роли и администраторы** . в настоящее время недоступно для Azure AD B2C.
- **Фирменная символика** — Настройка пользовательского интерфейса и UX настраивается в интерфейсе **фирменной символики компании** или в рамках потока пользователя. Научитесь [настраивать пользовательский интерфейс в Azure Active Directory B2C](customize-ui-with-html.md).
- **Проверка домена издателя** . ваше приложение зарегистрировано в *onmicrosoft.com*, которое не является проверенным доменом. Кроме того, домен издателя в основном используется для предоставления согласия пользователя, который не применяется к Azure AD B2C приложениям для проверки подлинности пользователей. Дополнительные [сведения о домене издателя](../active-directory/develop/howto-configure-publisher-domain.md).
- **Конфигурация токенов** — маркер настраивается как часть пользовательского потока, а не для приложения.
- В настоящее время опыт работы с **краткими руководствами** недоступен для клиентов Azure AD B2C.
<!-- - The **Integration assistant** blade is currently not available for Azure AD B2C tenants. -->

## <a name="limitations"></a>Ограничения
Новый интерфейс имеет следующие ограничения.
- В настоящее время Azure AD B2C не различает возможность выдавать маркеры доступа или ИДЕНТИФИКАТОРы для неявных потоков; Оба типа токенов доступны для неявного потока предоставления разрешений, если в колонке **Проверка подлинности** выбран параметр **токены идентификаторов** .
<!-- - Azure AD B2C doesn't currently support the single-page application "SPA" app type.  -->
- Изменение значения для поддерживаемых учетных записей не поддерживается в пользовательском интерфейсе. Вам потребуется использовать манифест приложения, если вы не переключаетесь между одним клиентом Azure AD и несколькими клиентами.

## <a name="next-steps"></a>Дальнейшие действия

Чтобы приступить к работе с новым интерфейсом регистрации приложения, выполните следующие действия.
* Узнайте, [как зарегистрировать веб-приложение](tutorial-register-applications.md).
* Узнайте [, как зарегистрировать веб-API](add-web-api-application.md).
* Узнайте [, как зарегистрировать собственное клиентское приложение](add-native-application.md).
* Узнайте [, как зарегистрировать приложение Microsoft Graph для управления ресурсами Azure AD B2C](microsoft-graph-get-started.md).
* Узнайте, [как использовать Azure AD B2C в качестве поставщика службы SAML.](identity-provider-adfs.md)
* Сведения о [типах приложений](application-types.md).