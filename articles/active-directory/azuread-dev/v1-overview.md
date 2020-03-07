---
title: Обзор Azure Active Directory для разработчиков (версия 1.0)
description: В этой статье представлены общие сведения о входе в рабочие и учебные учетные записи Майкрософт с использованием платформы и конечной точки Azure Active Directory версии 1.0.
services: active-directory
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.subservice: azuread-dev
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/24/2018
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 638c265fda3c8b331415d54047180b3c0ee2174a
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2020
ms.locfileid: "78377923"
---
# <a name="azure-active-directory-for-developers-v10-overview"></a>Обзор Azure Active Directory для разработчиков (версия 1.0)

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Azure Active Directory (Azure AD) — это облачная служба идентификации, которая позволяет разработчиками создавать приложения с поддержкой безопасного входа пользователей с рабочей или учебной учетной записью Майкрософт. Azure AD могут использовать разработчики, которые создают приложения с одним клиентом и бизнес-приложения, а также разрабатывают мультитенантные приложения. Помимо применения базовых возможностей входа, Azure AD также позволяет приложениям вызывать такие API Майкрософт, как [Microsoft Graph](https://docs.microsoft.com/graph/overview) и пользовательские API, которые созданы на платформе Azure AD. В этом документе показано, как с помощью стандартных отраслевых протоколов, таких как OAuth 2.0 и OpenID Connect, добавить поддержку Azure AD в ваше приложение.

> [!NOTE]
> Большая часть содержимого на этой странице описывает платформу и конечную точку версии 1.0, которая поддерживает только рабочие или учебные учетные записи Майкрософт. Сведения о том, как войти в личную учетную запись или учетную запись пользователя Майкрософт, см. в описании [платформы и конечной точки версии 2.0](../develop/v2-overview.md). Конечная точка версии 2.0 предоставляет разработчикам унифицированные возможности для создания приложений с поддержкой входа с использованием всех удостоверений Майкрософт.

| | |
| --- | --- |
|[Основные сведения об аутентификации](v1-authentication-scenarios.md) | Общие сведения об аутентификации с помощью Azure AD. |
|[Типы приложений](app-types.md) | Обзор сценариев аутентификации, поддерживаемых Azure AD. |
| | |

## <a name="get-started"></a>Начало работы

Из руководств по использованию версии 1.0 вы узнаете, как создать приложение на предпочитаемой платформе с использованием пакета SDK для Библиотеки аутентификации Azure AD (ADAL). Сведения о начале работы с версией 1.0 см. в **кратких руководствах** и **руководствах** по [платформе удостоверений Майкрософт (Azure Active Directory для разработчиков)](index.yml).

## <a name="how-to-guides"></a>Практические руководства

Сведения и примеры наиболее распространенных задач в Azure AD см. в **практических руководствах по версии 1.0**.

## <a name="reference-topics"></a>Справочные материалы

В этих статьях содержатся подробные сведения об API-интерфейсах, сообщениях протокола и терминах, используемых в Azure AD.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Библиотеки проверки подлинности Azure Active Directory](active-directory-authentication-libraries.md)   | Общие сведения о библиотеках и пакетах SDK, предоставляемых Azure AD. |
| [Примеры кода](sample-v1-code.md)                                  | Список всех примеров кода Azure AD. |
| [Глоссарий](../develop/developer-glossary.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)                                      | Термины и определения слов, используемых в этой документации. |
|  |  |

## <a name="videos"></a>Видео

Дополнительные сведения о переходе на новую платформу Microsoft Identity см. в разделе [Azure Active Directory видео о платформе разработчиков](videos.md) .

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
