---
title: Настройка браузеров & представлений (MSAL iOS/macOS) | Службы
titleSuffix: Microsoft identity platform
description: Узнайте, как настроить интерфейс MSAL iOS/macOS в браузере для входа пользователей.
services: active-directory
author: tylermsft
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/28/2019
ms.author: twhitney
ms.reviewer: oldalton
ms.custom: aaddev
ms.openlocfilehash: fad3a90bd11104b4d770ddc1c527cba7d299d150
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2020
ms.locfileid: "76697631"
---
# <a name="how-to-customize-browsers-and-webviews-for-iosmacos"></a>Как настроить браузеры и представления для iOS/macOS

Для интерактивной проверки подлинности требуется веб-браузер. В iOS Библиотека проверки подлинности Майкрософт (MSAL) по умолчанию использует системный веб-браузер (который может отображаться на вашем приложении) для выполнения интерактивной проверки подлинности для входа пользователей. Использование системного браузера дает преимущества совместного использования состояния единого входа (SSO) с другими приложениями и веб-приложениями.

Вы можете изменить интерфейс, настроив конфигурацию на другие параметры отображения веб-содержимого, например:

Только для iOS:

- [асвебаусентикатионсессион](https://developer.apple.com/documentation/authenticationservices/aswebauthenticationsession?language=objc)
- [сфаусентикатионсессион](https://developer.apple.com/documentation/safariservices/sfauthenticationsession?language=objc) 
- [сфсафаривиевконтроллер](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?language=objc)

Для iOS и macOS:

- [Вквебвиев](https://developer.apple.com/documentation/webkit/wkwebview?language=objc).

MSAL для macOS поддерживает только `WKWebView`.

## <a name="system-browsers"></a>Браузеры системы

Для iOS, `ASWebAuthenticationSession`, `SFAuthenticationSession`и `SFSafariViewController` считаются системными браузерами. В общем случае системные браузеры совместно используют файлы cookie и другие данные веб-сайта с помощью приложения браузера Safari.

По умолчанию MSAL динамически обнаружит версию iOS и выберет рекомендуемый системный браузер, доступный в этой версии. В iOS 12 + он будет `ASWebAuthenticationSession`. 

| Версия | браузер |
|:-------------:|:-------------:|
| iOS 12 + | асвебаусентикатионсессион |
| iOS 11 | сфаусентикатионсессион |
| iOS 10 | сфсафаривиевконтроллер |

Разработчики также могут выбрать другой обозреватель системы для приложений MSAL:

- `SFAuthenticationSession` — версия `ASWebAuthenticationSession`для iOS 11.
- `SFSafariViewController` является более общим целям и предоставляет интерфейс для обзора веб-страниц и может также использоваться для входа в систему. В iOS 9 и 10 файлы cookie и другие веб-сайты предоставляются с помощью Safari, но не в iOS 11 и более поздних версиях.

## <a name="in-app-browser"></a>Браузер в приложении

[Вквебвиев](https://developer.apple.com/documentation/webkit/wkwebview) — это браузер в приложении, который отображает веб-содержимое. Он не предоставляет доступ к файлам cookie или данным веб-сайта другим экземплярам **вквебвиев** или браузеру Safari. Вквебвиев — это кросс-платформенный браузер, доступный как для iOS, так и для macOS.

## <a name="cookie-sharing-and-single-sign-on-sso-implications"></a>Влияние совместного использования файлов cookie и единого входа (SSO)

Используемый браузер влияет на единый вход из-за того, как они совместно используют файлы cookie. В следующих таблицах представлены возможности единого входа для каждого браузера.

| Технология    | Тип браузера  | доступность iOS | доступность macOS | Совместное использование файлов cookie и других данных  | Доступность MSAL | Единый вход |
|:-------------:|:-------------:|:-------------:|:-------------:|:-------------:|:-------------:|-------------:|
| [асвебаусентикатионсессион](https://developer.apple.com/documentation/authenticationservices/aswebauthenticationsession) | Система | iOS12 и выше | macOS 10,15 и выше | Да | Только iOS | экземпляры «w/Safari»
| [сфаусентикатионсессион](https://developer.apple.com/documentation/safariservices/sfauthenticationsession) | Система | iOS11 и выше | Н/Д | Да | Только iOS |  экземпляры «w/Safari»
| [сфсафаривиевконтроллер](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) | Система | iOS11 и выше | Н/Д | Нет | Только iOS | Нет * *
| **сфсафаривиевконтроллер** | Система | iOS10 | Н/Д | Да | Только iOS |  экземпляры «w/Safari»
| **вквебвиев**  | В приложении | iOS8 и выше | macOS 10,10 и выше | Нет | iOS и macOS | Нет * *

\* * Чтобы единый вход работал, маркеры должны совместно использоваться приложениями. Для этого требуется кэш маркеров или приложение брокера, например Microsoft Authenticator для iOS.

## <a name="change-the-default-browser-for-the-request"></a>Изменение браузера по умолчанию для запроса

Вы можете использовать браузер в приложении или конкретный системный браузер в зависимости от требований к UX, изменив следующее свойство в `MSALWebviewParameters`:

```objc
@property (nonatomic) MSALWebviewType webviewType;
```

## <a name="change-per-interactive-request"></a>Изменение на интерактивный запрос

Каждый запрос может быть настроен на переопределение браузера по умолчанию путем изменения свойства `MSALInteractiveTokenParameters.webviewParameters.webviewType` перед передачей его в `acquireTokenWithParameters:completionBlock:` API.

Кроме того, MSAL поддерживает передачу пользовательских `WKWebView` путем установки свойства `MSALInteractiveTokenParameters.webviewParameters.customWebView`.

Пример.

Objective-C
```objc
UIViewController *myParentController = ...;
WKWebView *myCustomWebView = ...;
MSALWebviewParameters *webViewParameters = [[MSALWebviewParameters alloc] initWithParentViewController:myParentController];
webViewParameters.webviewType = MSALWebviewTypeWKWebView;
webViewParameters.customWebview = myCustomWebView;
MSALInteractiveTokenParameters *interactiveParameters = [[MSALInteractiveTokenParameters alloc] initWithScopes:@[@"myscope"] webviewParameters:webViewParameters];
    
[app acquireTokenWithParameters:interactiveParameters completionBlock:completionBlock];
```
Swift
```swift
let myParentController: UIViewController = ...
let myCustomWebView: WKWebView = ...
let webViewParameters = MSALWebviewParameters(parentViewController: myParentController)
webViewParameters.webviewType = MSALWebviewType.wkWebView
webViewParameters.customWebview = myCustomWebView
let interactiveParameters = MSALInteractiveTokenParameters(scopes: ["myscope"], webviewParameters: webViewParameters)

app.acquireToken(with: interactiveParameters, completionBlock: completionBlock)
```

При использовании настраиваемого WebView уведомления используются для указания состояния отображаемого веб-содержимого, например:

```objc
/*! Fired at the start of a resource load in the webview. The URL of the load, if available, will be in the @"url" key in the userInfo dictionary */
extern NSString *MSALWebAuthDidStartLoadNotification;

/*! Fired when a resource finishes loading in the webview. */
extern NSString *MSALWebAuthDidFinishLoadNotification;

/*! Fired when web authentication fails due to reasons originating from the network. Look at the @"error" key in the userInfo dictionary for more details.*/
extern NSString *MSALWebAuthDidFailNotification;

/*! Fired when authentication finishes */
extern NSString *MSALWebAuthDidCompleteNotification;

/*! Fired before ADAL invokes the broker app */
extern NSString *MSALWebAuthWillSwitchToBrokerApp;
```

### <a name="options"></a>Параметры

Все поддерживаемые типы веб-браузеров MSAL объявляются в [перечислении мсалвебвиевтипе](https://github.com/AzureAD/microsoft-authentication-library-for-objc/blob/master/MSAL/src/public/MSALDefinitions.h#L47)

```objc
typedef NS_ENUM(NSInteger, MSALWebviewType)
{
#if TARGET_OS_IPHONE
    // For iOS 11 and up, uses AuthenticationSession (ASWebAuthenticationSession
    // or SFAuthenticationSession).
    // For older versions, with AuthenticationSession not being available, uses
    // SafariViewController.
    MSALWebviewTypeDefault,
    
    // Use SFAuthenticationSession/ASWebAuthenticationSession
    MSALWebviewTypeAuthenticationSession,
    
    // Use SFSafariViewController for all versions.
    MSALWebviewTypeSafariViewController,
    
#endif
    // Use WKWebView
    MSALWebviewTypeWKWebView,
};
```

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в [Потоки проверки подлинности и сценарии приложений](authentication-flows-app-scenarios.md)
