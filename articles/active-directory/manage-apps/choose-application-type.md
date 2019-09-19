---
title: Как выбрать нужный тип приложения при добавлении приложения | Документация Майкрософт
description: Сведения о поддерживаемых типах приложений, которые можно интегрировать с Azure AD, и о вариантах их настройки
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/03/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ROBOTS: NOINDEX
ms.openlocfilehash: d5bd2397c345a4f670bde343f751cd69f825ecb9
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/17/2019
ms.locfileid: "71056060"
---
# <a name="choosing-the-application-type-when-adding-an-application-in-azure-active-directory"></a>Выбор типа приложения при добавлении приложения в Azure Active Directory

Сведения о четырех типах приложений, которые можно добавить в Azure Active Directory (Azure AD). При добавлении приложения в Azure Active Directory вам будет предложено выбрать один из четырех типов приложений.

## <a name="what-are-the-types-of-applications"></a>Какие есть типы приложений?

Azure AD поддерживает четыре основных типа приложений, которые можно добавить с помощью команды **Добавить** в разделе **Корпоративные приложения**. Сюда входит следующее.

- **Приложения из коллекции Azure AD** — это предварительно интегрированные приложения для единого входа через Azure AD.

- **Приложения прокси приложения** — приложения, работающее в локальной среде, которым необходимо предоставить безопасный единый вход извне.

- **Специально разработанные приложения** — приложения, которые ваша организация намерена разработать на платформе разработки приложений Azure AD, но, возможно, они еще находятся на стадии обсуждения.

- **Приложения не из коллекции** — собственные приложения. К ним относится любая нужная веб-ссылка или любое приложение с полем имени пользователя и пароля и поддержкой протоколов SAML или OpenID Connect либо системы SCIM, которую нужно интегрировать для единого входа через Azure AD.

## <a name="features-and-capabilities-supported-by-the-application-types"></a>Функции и возможности, поддерживаемые типами приложений

Приложения всех четырех описанных выше типов поддерживают следующие функции в Azure AD:

- **Краткое руководство** . быстро приступить к работе с приложением, следуя [простым шагам развертывания](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started) .

- **Управление общими свойствами** — получение [прямой ссылки](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) на приложение, [Настройка фирменной символики](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) приложения или [отключение приложения](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) для всех пользователей.

- **Управление пользователями и группами.** [Назначьте](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) или [удалите](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) пользователей и группы в приложении и при необходимости назначьте им конкретные роли, к которым они будут иметь доступ.

- **Самостоятельный доступ к приложениям.** Разрешите пользователям запрашивать [самостоятельный доступ к приложениям](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) с помощью панели доступа к приложениям. Для этого добавьте приложение напрямую или [присоедините группу с самостоятельным доступом](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), при необходимости настроив бизнес-утверждение.

- **Журналы входа**. Просмотрите [все операции входа в приложение](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) или приложения.

- **Журналы аудита.** Просмотрите [подробные журналы аудита изменений в приложении](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs) или приложениях.

- **Условный доступ и доступ на основе рисков.** Задайте эффективные [правила доступа на основе условия](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access), применяющиеся при попытке входа пользователей в конкретное приложение.

- **Представление разрешений.** Просмотрите все [разрешения OAuth2](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent), к которым приложение имеет доступ в вашем каталоге, из одного расположения.

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Режимы единого входа и подготовки, поддерживаемые конкретными типами приложений

В приведенной ниже таблице описаны различные режимы единого входа и подготовки, поддерживаемые приложениями всех указанных типов. С ее помощью вы можете определить, какое приложение необходимо добавить для достижения конкретной цели.

  ![Таблица. Различные режимы единого входа и подготовки, поддерживаемые каждым типом приложения](./media/choose-application-type/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a>Как выбрать режим единого входа

Ниже указаны поддерживаемые режимы **единого входа** для приложений Azure AD.

- **Единый вход Azure AD отключен.** Выберите **режим** "Единый вход Azure AD отключен", если вы еще не готовы к интеграции этого приложения с единым входом в Azure AD или просто его тестируете.

- **Вход по ссылке**. Выберите **режим** [Вход по ссылке](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) при наличии приложения, уже подключенного с использованием решения единого входа, или если требуется опубликовать простую ссылку для пользователей на [панели доступа к приложениям](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) или в [средстве запуска приложений Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none).

- **Вход по паролю**. Выберите **режим единого входа** [Вход по паролю](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis), если приложение отображает HTML-поле имени пользователя и пароля, а также если необходимо безопасно сохранить имя пользователя и пароль для их повторного использования в приложении.

- **Вход на основе SAML**. Выберите режим единого входа [Вход на основе SAML](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis), если приложение поддерживает протоколы SAML или OpenID Connect или если вам нужно сопоставить пользователей с конкретными ролями приложений на основе правил, определенных в утверждениях SAML*.

  >[!NOTE]
  >Этот параметр недоступен, если для приложения настроен прокси приложения.

- **Вход на основе заголовков.** Для единого входа в приложение, использующее решение PingAccess с поддержкой аутентификации на основе HTTP-заголовка, выберите режим единого входа [Вход на основе заголовков](application-proxy-configure-single-sign-on-with-ping-access.md).

  >[!NOTE]
  >Этот параметр доступен только, если для приложения настроен прокси приложения и PingAccess.

- **Интегрированная проверка подлинности Windows**. Выберите режим единого входа [Интегрированная проверка подлинности Windows](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) при использовании локального приложения WIA, для которого необходимо выполнить единый вход.

  >[!NOTE]
  >Этот параметр доступен, только если для приложения настроен прокси приложения.

## <a name="single-sign-on-modes-for-custom-developed-applications"></a>Режимы единого входа для специально разработанных приложений

Специально разработанные приложения также поддерживают дополнительные режимы единого входа, не указанные в списке выше, к которым относятся:

- единый вход на основе [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code);

- единый вход на основе [OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code);

- единый вход на основе [WS-Federation 1.2](https://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html);

- единый вход на основе [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference).

Ознакомьтесь с [руководством разработчика по Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide), чтобы узнать, как создавать специально разработанные приложения, которые поддерживают эти режимы единого входа.

## <a name="how-to-set-an-applications-single-sign-on-mode"></a>Настройка режима единого входа для приложения

Чтобы настроить режим единого входа приложения, выполните следующие действия:

1. Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор** или **соадминистратор**.
1. Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.
1. В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.
1. В меню навигации Azure Active Directory слева выберите **Корпоративные приложения**.
1. Щелкните **Все приложения**, чтобы открыть полный список приложений.

   * Если нужное приложение отсутствует в списке, в верхней части списка **Все приложения** воспользуйтесь элементом управления **Фильтр**, указав в нем для параметра **Показать** значение **Все приложения**.

1. Выберите приложение, для которого необходимо настроить единый вход.
1. После загрузки приложения выберите пункт **Единый вход** в меню навигации этого приложения слева.

## <a name="how-to-choose-a-provisioning-mode"></a>Как выбрать режим подготовки

- **Ручная подготовка.** Выберите режим подготовки [Вручную](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes), если у вас есть учетные записи или необходимо управлять учетными записями приложения за пределами Azure AD.

- **Автоматическая подготовка.** Выберите **режим подготовки** [Автоматически](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning), если необходимо включить автоматическую подготовку на основе API или отмену подготовки учетных записей пользователей для этого приложения. 

- **Автоматическая подготовка на основе SCIM.** Выберите этот [режим подготовки](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning), если приложение поддерживает протокол SCIM для обнаружения изменений пользователей и групп, которые добавляются автоматически в любое приложение, интегрированное с Azure AD. 

  >[!NOTE]
  >Этот параметр не является определенным режимом подготовки, но он включен по умолчанию для всех приложений, интегрированных с Azure AD.

## <a name="how-to-set-an-applications-provisioning-mode"></a>Как настроить режим подготовки для приложения

Чтобы настроить для приложения режим **подготовки**, сделайте следующее:

1. Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор** или **соадминистратор**.
1. Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.
1. В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.
1. В меню навигации Azure Active Directory слева выберите **Корпоративные приложения**.
1. Щелкните **Все приложения**, чтобы открыть полный список приложений.

   * Если нужное приложение отсутствует в списке, в верхней части списка **Все приложения** воспользуйтесь элементом управления **Фильтр**, указав в нем для параметра **Показать** значение **Все приложения**.

1. Выберите приложение, для которого необходимо настроить подготовку.
1. После загрузки приложения щелкните **Подготовка** в меню навигации слева.

## <a name="next-steps"></a>Следующие шаги

[Управление приложениями с помощью Azure Active Directory](what-is-application-management.md)
