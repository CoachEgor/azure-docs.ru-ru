---
title: Настройка одностраничного приложения — платформа Microsoft Identity | Службы
description: Сведения о создании одностраничного приложения (конфигурация кода приложения)
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/11/2020
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: f1e0bf44515aab18019b19b4f0a6f84183e5aac3
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2020
ms.locfileid: "77160089"
---
# <a name="single-page-application-code-configuration"></a>Одностраничное приложение: конфигурация кода

Узнайте, как настроить код для одностраничного приложения (SPA).

## <a name="msal-libraries-that-support-implicit-flow"></a>Библиотеки MSAL, поддерживающие неявные потоки

Платформа удостоверений Майкрософт предоставляет следующие библиотеки библиотеки проверки подлинности (MSAL) для поддержки неявного потока с помощью рекомендуемых в отрасли методов обеспечения безопасности:  

| Библиотека MSAL | Description |
|--------------|--------------|
| ![MSAL.js](media/sample-v2-code/logo_js.png) <br/> [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)  | Простая библиотека JavaScript для использования в любом веб-приложении на стороне клиента, созданном с помощью платформ JavaScript или SPA, таких как угловой, Vue. js и реагируют. js. |
| ![MSAL угловой](media/sample-v2-code/logo_angular.png) <br/> [MSALный угловой](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | Оболочка основной библиотеки MSAL. js для упрощения использования в одностраничных приложениях, созданных с помощью угловой платформы. Эта библиотека находится на этапе предварительной версии и содержит [Известные проблемы](https://github.com/AzureAD/microsoft-authentication-library-for-js/issues?q=is%3Aopen+is%3Aissue+label%3Aangular) с определенными версиями и браузерами. |

## <a name="application-code-configuration"></a>Конфигурация кода приложения

В библиотеке MSAL сведения о регистрации приложения передаются как конфигурация во время инициализации библиотеки.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
// Configuration object constructed.
const config = {
    auth: {
        clientId: 'your_app_id',
        redirectUri: "your_app_redirect_uri" //defaults to application start page
    }
}

// create UserAgentApplication instance
const userAgentApplication = new UserAgentApplication(config);
```

Дополнительные сведения о настраиваемых параметрах см. в разделе [Инициализация приложения с помощью MSAL. js](msal-js-initializing-client-applications.md).

# <a name="angulartabangular"></a>[Angular](#tab/angular)

```javascript
//In app.module.ts
import { MsalModule } from '@azure/msal-angular';

@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id'
            })]
         })

  export class AppModule { }
```

---

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Вход и выход](scenario-spa-sign-in.md)
