---
title: Создание клиента Azure для приложения с несколькими клиентами
description: Руководство для независимых поставщиков программного обеспечения при интеграции с Azure Active Directory
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: kenwith
ms.reviewer: jeeds
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a0b63c130d7d1e72bd3320e40213ae3cb1069a6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "84763250"
---
# <a name="create-an-azure-tenant-for-a-multi-tenant-application"></a>Создание клиента Azure для приложения с несколькими клиентами  

Чтобы предоставить доступ к многоклиентским приложениям, необходимо создать клиент Azure Active Directory для регистрации приложения и включения федерации удостоверений клиента. См. раздел [Выбор правильного протокола Федерации для приложения с несколькими клиентами](isv-choose-multi-tenant-federation.md). Этот клиент позволит протестировать приложение и Федерацию в среде, похожей на клиентские среды Azure AD.

## <a name="costs-of-hosting-a-multi-tenant-application"></a>Затраты на размещение приложения с несколькими клиентами

Azure Active Directory доступен в нескольких выпусках. [См. подробные сведения о сравнении функций](https://azure.microsoft.com/pricing/details/active-directory/).

Вы можете создать подписку Azure и Azure Active Directory бесплатно и использовать основные функции.

## <a name="create-your-tenant"></a>Создание клиента

1. Создайте клиент. См. раздел [Настройка среды разработки](../develop/quickstart-create-new-tenant.md).

2. Включите и проверьте доступ к приложению для единого входа.

   а. **Для приложений OIDC или Oath** [зарегистрируйте приложение](../develop/quickstart-register-app.md) в качестве приложения с несколькими клиентами. Выберите учетные записи в любом организационном каталоге и личные учетные записи Майкрософт в списке Поддерживаемые типы учетных записей.

   b. **Для приложений на основе SAML и WS-подач**вы [настраиваете приложения единого входа на основе SAML](configure-single-sign-on-non-gallery-applications.md) с помощью универсального шаблона SAML в Azure AD.

При необходимости можно также [преобразовать приложение с одним клиентом в несколько клиентов](../develop/howto-convert-app-to-be-multi-tenant.md) .

## <a name="next-steps"></a>Следующие шаги

[Интеграция единого входа в приложение](isv-sso-content.md)
