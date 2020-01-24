---
title: MSAL для iOS & macOS разницы | Службы
titleSuffix: Microsoft identity platform
description: Описание различий использования библиотеки проверки подлинности Майкрософт (MSAL) между iOS и macOS.
services: active-directory
author: TylerMSFT
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/28/2019
ms.author: twhitney
ms.reviewer: oldalton
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: a17739151c56fec92a9697e1e04c39cb44da5088
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2020
ms.locfileid: "76696237"
---
# <a name="microsoft-authentication-library-for-ios-and-macos-differences"></a>Отличия библиотеки проверки подлинности Майкрософт для iOS и macOS

В этой статье объясняются различия в функциональных возможностях библиотеки проверки подлинности Майкрософт (MSAL) для iOS и macOS.

> [!NOTE]
> На компьютере Mac MSAL поддерживает только приложения macOS.

## <a name="general-differences"></a>Общие различия

MSAL для macOS — это подмножество функций, доступных для iOS.

MSAL для macOS не поддерживает:

- различные типы браузеров, такие как `ASWebAuthenticationSession`, `SFAuthenticationSession`, `SFSafariViewController`.
- Проверка подлинности через посредника через Microsoft Authenticator приложение не поддерживается для macOS.

Совместное использование цепочки ключей между приложениями одного издателя более ограничено в macOS 10,14 и более ранних версиях. Используйте [списки управления доступом](https://developer.apple.com/documentation/security/keychain_services/access_control_lists?language=objc) , чтобы указать пути к приложениям, которые должны совместно использовать цепочку ключей. Пользователь может видеть дополнительные запросы цепочки ключей.

В macOS 10.15 + поведение MSAL одинаково между iOS и macOS. MSAL использует [группы доступа к цепочке ключей](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps?language=objc) для совместного использования цепочки ключей. 

### <a name="conditional-access-authentication-differences"></a>Различия в проверке подлинности условного доступа

Для сценариев условного доступа при использовании MSAL для iOS будет меньше запросов пользователя. Это связано с тем, что в iOS используется приложение брокера (Microsoft Authenticator), которое устраняет необходимость запрашивать пользователя в некоторых случаях.

### <a name="project-setup-differences"></a>Различия в настройке проекта

**macOS**

- При настройке проекта на macOS убедитесь, что приложение подписано с помощью действительного сертификата разработки или рабочей среды. MSAL по-прежнему работает в режиме без знака, но он ведет себя иначе с точки зрения сохраняемости кэша. Приложение должно запускаться только без подписи в целях отладки. Если вы распространяете приложение без подписи, оно будет выполнять следующие действия:
1. В 10,14 и более ранних версиях MSAL будет предлагать пользователю ввести пароль цепочки ключей каждый раз при перезапуске приложения.
2. В 10.15 + MSAL предложит пользователю ввести учетные данные для каждого получения маркера. 

- приложениям macOS не требуется реализовывать вызов AppDelegate.

**iOS**

- Чтобы настроить проект для поддержки потока брокера проверки подлинности, необходимо выполнить дополнительные действия. Эти действия вызываются в этом руководстве.
- проекты iOS должны регистрировать пользовательские схемы в файле info. plist. Это не требуется для macOS.
