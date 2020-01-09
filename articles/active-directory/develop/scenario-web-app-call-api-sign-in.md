---
title: Удаление учетных записей из кэша маркеров при выходе — платформа Microsoft Identity | Службы
description: Узнайте, как удалить учетную запись из кэша маркеров при выходе
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5472b424f7d2b44b62e6e4495afaf7bdfbbc8439
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75423510"
---
# <a name="remove-accounts-from-the-cache-on-global-sign-out"></a>Удаление учетных записей из кэша при глобальном выходе

Вы уже знакомы с добавлением единого входа в веб-приложение. Вы узнаете, что в [веб-приложении, в котором входят пользователи, — добавление входа в систему](scenario-web-app-sign-user-sign-in.md).

В этом случае, если пользователь выйдет из этого приложения или из любого приложения, вы хотите удалить его из кэша маркеров, связанных с этим пользователем маркеров.

## <a name="intercepting-the-callback-after-sign-out---single-sign-out"></a>Перехват обратного вызова после выхода — единый выход

Приложение может перехватить событие после `logout`, например очистить запись кэша маркеров, связанную с учетной записью, с которой выполнен выход. Веб-приложение будет хранить маркеры доступа для пользователя в кэше. Перехват обратного вызова после `logout` позволяет веб-приложению удалить пользователя из кэша маркеров.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Этот механизм проиллюстрирован в методе `AddMsal()` [вебаппсервицеколлектионекстенсионс. CS # L151-L157](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/db7f74fd7e65bab9d21092ac1b98a00803e5ceb2/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L157)

**URL-адрес выхода** , зарегистрированный для приложения, позволяет реализовать единый выход. Конечная точка `logout` платформы Microsoft Identity будет вызывать **URL-адрес выхода** , зарегистрированный в приложении. Этот вызов происходит, если выход был инициирован из веб-приложения или из другого веб-приложения или браузера. Дополнительные сведения см. в разделе [единый выход](v2-protocols-oidc.md#single-sign-out).

```csharp
public static class WebAppServiceCollectionExtensions
{
 public static IServiceCollection AddMsal(this IServiceCollection services, IConfiguration configuration, IEnumerable<string> initialScopes, string configSectionName = "AzureAd")
 {
  // Code omitted here

  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
   // Code omitted here

   // Handling the sign-out: removing the account from MSAL.NET cache
   options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
   {
    // Remove the account from MSAL.NET token cache
    var tokenAcquisition = context.HttpContext.RequestServices.GetRequiredService<ITokenAcquisition>();
    await tokenAcquisition.RemoveAccountAsync(context).ConfigureAwait(false);
   };
  });
  return services;
 }
}
```

Код для Ремовеаккаунтасинк доступен в [Microsoft. Identity. Web/токенаккуиситион. CS # L264-L288](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/db7f74fd7e65bab9d21092ac1b98a00803e5ceb2/Microsoft.Identity.Web/TokenAcquisition.cs#L264-L288).

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

Пример ASP.NET не удаляет учетные записи из кэша при глобальном выходе

# <a name="javatabjava"></a>[Java](#tab/java)

Пример Java не удаляет учетные записи из кэша при глобальном выходе

# <a name="pythontabpython"></a>[Python](#tab/python)

Пример Python не удаляет учетные записи из кэша при глобальном выходе

---

## <a name="next-steps"></a>Дальнейшие действия

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [Получение маркера для веб-приложения](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnetcore)

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [Получение маркера для веб-приложения](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnet)

# <a name="javatabjava"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Получение маркера для веб-приложения](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=java)

# <a name="pythontabpython"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Получение маркера для веб-приложения](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=python)

---
