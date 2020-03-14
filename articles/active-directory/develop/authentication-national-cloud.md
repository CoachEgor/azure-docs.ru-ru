---
title: Аутентификация Azure AD & национальными облаками | Службы
titleSuffix: Microsoft identity platform
description: Узнайте о конечных точках для регистрации и проверки подлинности приложений в национальных облаках.
services: active-directory
author: negoe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/28/2019
ms.author: negoe
ms.reviewer: negoe,celested
ms.custom: aaddev
ms.openlocfilehash: 20a053369149dc29d6485c49bb091a75bb9fb591
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79263001"
---
# <a name="national-clouds"></a>Местные облака

Национальные облака — это физически изолированные экземпляры Azure. Эти регионы Azure призваны обеспечить соблюдение требований по размещению и независимости данных, а также законодательных требований в пределах географических границ.

Включая глобальное облако, Azure Active Directory (Azure AD) развертывается в следующих национальных облаках:  

- Azure для государственных организаций
- Azure для Германии
- Azure China 21Vianet

Национальные облака являются уникальными и представляют собой отдельную среду из глобальной среды Azure. Важно помнить о ключевых различиях при разработке приложения для этих сред. Отличия включают регистрацию приложений, получение маркеров и настройку конечных точек.

## <a name="app-registration-endpoints"></a>Конечные точки регистрации приложений

Для каждого из национальных облаков существует отдельный портал Azure. Для интеграции приложений с платформой Microsoft Identity в национальном облаке необходимо зарегистрировать приложение отдельно в каждом портал Azure, характерном для конкретной среды.

В следующей таблице перечислены базовые URL-адреса для конечных точек Azure AD, используемых для регистрации приложения для каждого национального облака.

| Национальное облако | Конечная точка портала Azure AD |
|----------------|--------------------------|
| Azure AD для US Gov организаций | `https://portal.azure.us` |
| Azure AD для Германии | `https://portal.microsoftazure.de` |
| Azure AD для Китая через 21Vianet | `https://portal.azure.cn` |
| Azure AD (глобальная служба) |`https://portal.azure.com` |

## <a name="azure-ad-authentication-endpoints"></a>Конечные точки Azure AD для проверки подлинности

Все национальные облака проводят проверку подлинности пользователей в каждой среде отдельно и имеют отдельные конечные точки для проверки подлинности.

В следующей таблице перечислены базовые URL-адреса для конечных точек Azure AD, используемых для получения маркеров для каждого национального облака.

| Национальное облако | Конечная точка проверки подлинности Azure AD |
|----------------|-------------------------|
| Azure AD для US Gov организаций | `https://login.microsoftonline.us` |
| Azure AD для Германии| `https://login.microsoftonline.de` |
| Azure AD для Китая через 21Vianet | `https://login.chinacloudapi.cn` |
| Azure AD (глобальная служба)| `https://login.microsoftonline.com` |

Вы можете сформировать запросы к конечным точкам авторизации или маркера Azure AD, используя соответствующий базовый URL-адрес для конкретного региона. Например, для Azure для Германии:

  - стандартная конечная точка авторизации — `https://login.microsoftonline.de/common/oauth2/authorize`;
  - стандартная конечная точка токена — `https://login.microsoftonline.de/common/oauth2/token`.

Для приложений с одним клиентом замените "Common" в предыдущих URL-адресах на идентификатор клиента или имя. Например, `https://login.microsoftonline.de/contoso.com`.

## <a name="microsoft-graph-api"></a>API Microsoft Graph

Чтобы узнать, как вызывать интерфейсы API Microsoft Graph в национальной облачной среде, перейдите в раздел [Microsoft Graph в облачных развертываниях в облаке](https://developer.microsoft.com/graph/docs/concepts/deployments).

> [!IMPORTANT]
> Некоторые службы и функции, которые находятся в определенных регионах глобальной службы, могут быть недоступны во всех национальных облаках. Чтобы узнать, какие службы доступны, перейдите к разделу [продукты, доступные по регионам](https://azure.microsoft.com/global-infrastructure/services/?products=all&regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia,china-non-regional,china-east,china-east-2,china-north,china-north-2,germany-non-regional,germany-central,germany-northeast).

Чтобы узнать, как создать приложение с помощью платформы Microsoft Identity, следуйте указаниям в [руководстве по библиотеке проверки подлинности Майкрософт (MSAL)](msal-national-cloud.md). В частности, это приложение будет входить в систему пользователя и получить маркер доступа для вызова API Microsoft Graph.

## <a name="next-steps"></a>Дальнейшие действия

См. также:

- [Azure для государственных организаций](https://docs.microsoft.com/azure/azure-government/)
- [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure для Германии](https://docs.microsoft.com/azure/germany/)
- [Основы проверки подлинности Azure AD](authentication-scenarios.md)
