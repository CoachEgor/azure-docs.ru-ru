---
title: Дополнительные сведения о MSAL | Службы
titleSuffix: Microsoft identity platform
description: Библиотека аутентификации Майкрософт (MSAL) позволяет разработчикам приложений получать токены для вызова защищенных веб-API. Это могут быть веб-API Microsoft Graph, другие API Майкрософт, сторонние веб-API или ваши собственные веб-API. MSAL поддерживает несколько архитектур и платформ приложений.
services: active-directory
author: TylerMSFT
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 5e0ad98b24b0e917d079d72ecc7e1930d30cc04a
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2020
ms.locfileid: "76699569"
---
# <a name="overview-of-microsoft-authentication-library-msal"></a>Обзор библиотеки аутентификации Майкрософт (MSAL)
Библиотека аутентификации Майкрософт (MSAL) позволяет разработчикам получать [маркеры](developer-glossary.md#security-token) из конечной точки платформы удостоверений Майкрософт, чтобы получить доступ к защищенным веб-API. Это могут быть веб-API Microsoft Graph, другие API Майкрософт, сторонние веб-API или ваши собственные веб-API. MSAL доступна для .NET, JavaScript, Android и iOS. Она поддерживает множество различных архитектур и платформ приложений.

MSAL предоставляет множество способов для получения маркеров с использованием согласованного API для нескольких платформ. Использование MSAL обеспечивает следующие преимущества:

* Не требуется напрямую использовать библиотеки OAuth или код для соответствующего протокола в приложении.
* Получение маркеров от имени пользователя или приложения (когда применимо к платформе).
* Поддерживает кэш маркеров и обновляет маркеры, срок действия которых скоро истечет. Нет необходимости вручную регулировать истечение срока действия маркера.
* Помогает вам определить аудиторию, которая будет выполнять вход в ваше приложение (ваша организация, несколько организаций, рабочие, школьные и личные учетные записи Майкрософт, удостоверения социальных сетей с помощью Azure AD B2C, пользователи в независимых и национальных облаках).
* Помогает настроить приложение из файлов конфигурации.
* Поможет вам устранять неполадки, выявляя действенные исключения, ведя журналы и телеметрию.

## <a name="application-types-and-scenarios"></a>Типы приложений и сценариев
С помощью MSAL вы можете получить маркер из нескольких типов приложений: веб-приложений, веб-API, одностраничных приложений (JavaScript), мобильных и собственных приложений, управляющих программ и серверных приложений.

MSAL можно использовать во многих сценариях приложений, включая следующие:

* [одностраничные приложения (JavaScript);](scenario-spa-overview.md)
* [веб-приложения, в которые могут входить пользователи;](scenario-web-app-sign-user-overview.md)
* [веб-приложения, в которые могут входить пользователи и которые могут вызывать веб-API от имени пользователя;](scenario-web-app-call-api-overview.md)
* [защита веб-API, чтобы доступ к нему могли получить только прошедшие аутентификацию пользователи;](scenario-protected-web-api-overview.md)
* [веб-API, вызывающий другой подчиненный веб-API от имени выполнившего вход пользователя;](scenario-web-api-call-api-overview.md)
* [классическое приложение, вызывающее веб-API от имени выполнившего вход пользователя;](scenario-desktop-overview.md)
* [мобильное приложение, вызывающее веб-API от имени пользователя, вошедшего в систему в интерактивном режиме;](scenario-mobile-overview.md)
* [классическая управляющая программа / управляющая программа службы, вызывающая веб-API от своего имени.](scenario-daemon-overview.md)

## <a name="languages-and-frameworks"></a>Языки и платформы

| Библиотека | Поддерживаемые платформы и среды|
| --- | --- |
| [MSAL для .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)| Платформа .NET Framework, .NET Core, Xamarin Android, Xamarin iOS, универсальная платформа Windows|
| [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)| Платформы JavaScript/TypeScript, такие как AngularJS, Ember.js или Durandal.js|
| [MSAL для Android](https://github.com/AzureAD/microsoft-authentication-library-for-android)|Android|
| [MSAL для iOS и macOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|iOS и macOS|
| [MSAL Java (Предварительная версия)](https://github.com/AzureAD/microsoft-authentication-library-for-java)|Java|
| [MSAL Python (Предварительная версия)](https://github.com/AzureAD/microsoft-authentication-library-for-python)|Python|

## <a name="differences-between-adal-and-msal"></a>Различия между ADAL и MSAL

Библиотека проверки подлинности Active Directory (ADAL) интегрируется с конечной точкой Azure AD для разработчиков (версия 1.0), а MSAL интегрируется с конечной точкой платформы удостоверений Майкрософт (версия 2.0). Конечная точка версии 1.0 поддерживает рабочие учетные записи, но не личные учетные записи. Конечная точка Azure AD версии 2.0 — это объединение личных учетных записей Майкрософт и рабочих учетных записей Майкрософт в единую систему аутентификации. Кроме того, с помощью MSAL можно выполнить аутентификацию для Azure AD B2C.

Дополнительные сведения см. в статье о [миграции в MSAL.NET из ADAL.NET](msal-net-migration.md) и [миграции в MSAL.js из ADAL.js](msal-compare-msal-js-and-adal-js.md).
