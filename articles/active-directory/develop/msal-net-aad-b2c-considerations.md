---
title: Azure AD B2C (MSAL.NET) | Службы
titleSuffix: Microsoft identity platform
description: Ознакомьтесь с конкретными соображениями при использовании Azure AD B2C с библиотекой проверки подлинности Майкрософт для .NET (MSAL.NET).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/29/2019
ms.author: jeferrie
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 697b4bc8e3a25085ac6f7d600ea2227dd30a6624
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2020
ms.locfileid: "77084005"
---
# <a name="use-msalnet-to-sign-in-users-with-social-identities"></a>Использование MSAL.NET для входа пользователей с удостоверениями социальных сетей

MSAL.NET можно использовать для входа пользователей с удостоверениями социальных сетей с помощью [Azure Active Directory B2C (Azure AD B2C)](https://aka.ms/aadb2c). Azure AD B2C построены на основе понятия политик. В MSAL.NET указание политики преобразуется в предоставление центра.

- При создании экземпляра общедоступного клиентского приложения необходимо указать политику в центре сертификации.
- Если необходимо применить политику, необходимо вызвать переопределение `AcquireTokenInteractive`, содержащее параметр `authority`.

Эта страница предназначена для MSAL 3. x. Если вы заинтересованы в MSAL 2. x, см. раздел [Azure AD B2C особенности в MSAL 2. x](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-Specifics-MSAL-2.x).

## <a name="authority-for-a-azure-ad-b2c-tenant-and-policy"></a>Центр для клиента Azure AD B2C и политики

Используемый центр `https://{azureADB2CHostname}/tfp/{tenant}/{policyName}`, где:

- `azureADB2CHostname` — имя клиента Azure AD B2C и узла (например `{your-tenant-name}.b2clogin.com`),
- `tenant` — это полное имя клиента Azure AD B2C (например, `{your-tenant-name}.onmicrosoft.com`) или GUID для клиента. 
- `policyName` имя применяемой политики или пользовательского процесса (например, "b2c_1_susi" для регистрации или входа).

Дополнительные сведения о Azure AD B2Cных центрах см. в этой [документации](/azure/active-directory-b2c/b2clogin).

## <a name="instantiating-the-application"></a>Создание экземпляра приложения

При создании приложения необходимо предоставить центр.

```csharp
// Azure AD B2C Coordinates
public static string Tenant = "fabrikamb2c.onmicrosoft.com";
public static string AzureADB2CHostname = "fabrikamb2c.b2clogin.com";
public static string ClientID = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
public static string PolicySignUpSignIn = "b2c_1_susi";
public static string PolicyEditProfile = "b2c_1_edit_profile";
public static string PolicyResetPassword = "b2c_1_reset";

public static string AuthorityBase = $"https://{AzureADB2CHostname}/tfp/{Tenant}/";
public static string Authority = $"{AuthorityBase}{PolicySignUpSignIn}";
public static string AuthorityEditProfile = $"{AuthorityBase}{PolicyEditProfile}";
public static string AuthorityPasswordReset = $"{AuthorityBase}{PolicyResetPassword}";

application = PublicClientApplicationBuilder.Create(ClientID)
               .WithB2CAuthority(Authority)
               .Build();
```

## <a name="acquire-a-token-to-apply-a-policy"></a>Получение маркера для применения политики

Получение маркера для защищенного Azure AD B2C API в общедоступном клиентском приложении требует использования переопределений с центром сертификации:

```csharp
IEnumerable<IAccount> accounts = await application.GetAccountsAsync();
AuthenticationResult ar = await application .AcquireTokenInteractive(scopes)
                                            .WithAccount(GetAccountByPolicy(accounts, policy))
                                            .WithParentActivityOrWindow(ParentActivityOrWindow)
                                            .ExecuteAsync();
```

вставьте

- `policy` быть одной из предыдущих строк (например, `PolicySignUpSignIn`).
- `ParentActivityOrWindow` требуется для Android (действие) и необязательно для других платформ, поддерживающих родительский пользовательский интерфейс, например Windows в Windows и UIViewController в iOS. Дополнительные сведения см. в [диалоговом окне пользовательского интерфейса](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Acquiring-tokens-interactively#withparentactivityorwindow).
- `GetAccountByPolicy(IEnumerable<IAccount>, string)` — это метод, который находит учетную запись для данной политики. Пример:

  ```csharp
  private IAccount GetAccountByPolicy(IEnumerable<IAccount> accounts, string policy)
  {
   foreach (var account in accounts)
   {
    string userIdentifier = account.HomeAccountId.ObjectId.Split('.')[0];
    if (userIdentifier.EndsWith(policy.ToLower()))
     return account;
   }
   return null;
  }
  ```

Применение политики или потока пользователей (например, предоставление конечным пользователям возможности редактировать свой профиль или сброс пароля) в настоящее время выполняется путем вызова `AcquireTokenInteractive`. В случае с этими двумя политиками вы не используете возвращенный результат маркера или проверки подлинности.

## <a name="special-case-of-editprofile-and-resetpassword-policies"></a>Особый случай политик Едитпрофиле и Ресетпассворд

Если вы хотите предоставить пользователям возможности для входа с помощью социальных удостоверений и изменения профиля, примените политику Azure AD B2C изменить профиль. Это можно сделать, вызвав `AcquireTokenInteractive` с конкретными полномочиями для этой политики, и запросите значение `Prompt.NoPrompt`, чтобы диалоговое окно выбора учетной записи не отображалось (так как пользователь уже выполнил вход и имеет активный сеанс cookie).

```csharp
private async void EditProfileButton_Click(object sender, RoutedEventArgs e)
{
 IEnumerable<IAccount> accounts = await app.GetAccountsAsync();
 try
 {
  var authResult = await app.AcquireToken(scopes:App.ApiScopes)
                               .WithAccount(GetUserByPolicy(accounts, App.PolicyEditProfile)),
                               .WithPrompt(Prompt.NoPrompt),
                               .WithB2CAuthority(App.AuthorityEditProfile)
                               .ExecuteAsync();
  DisplayBasicTokenInfo(authResult);
 }
 catch
 {
  . . .
 }
}
```
## <a name="resource-owner-password-credentials-ropc-with-azure-ad-b2c"></a>Учетные данные владельца ресурса (РОПК) с Azure AD B2C
Дополнительные сведения о потоке РОПК см. в этой [документации](v2-oauth-ropc.md).

Этот поток **не рекомендуется** , так как приложение, запрашивающее у пользователя пароль, не является безопасным. Дополнительные сведения об этой проблеме см. в [этой статье](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/). 

Используя имя пользователя и пароль, вы предоставляете ряд вещей:
- Основные принципы современного удостоверения: пароль передается, а затем воспроизводится. Так как мы предоставляем эту концепцию общего секрета, который можно перехватить. Это несовместимо с паролем.
- Пользователи, которым требуется выполнить MFA, не смогут войти в систему (так как взаимодействие отсутствует).
- Пользователи не смогут выполнять единый вход.

### <a name="configure-the-ropc-flow-in-azure-ad-b2c"></a>Настройка потока РОПК в Azure AD B2C
В клиенте Azure AD B2C создайте новый поток пользователя и выберите **Вход с помощью ропк**. Это позволит включить политику РОПК для клиента. Дополнительные сведения см. [в разделе Настройка потока учетных данных для пароля владельца ресурса](/azure/active-directory-b2c/configure-ropc) .

`IPublicClientApplication` содержит метод:
```csharp
AcquireTokenByUsernamePassword(
            IEnumerable<string> scopes,
            string username,
            SecureString password)
```

Этот метод принимает в качестве параметров:
- Области, для *которых* запрашивается маркер доступа.
- *Имя пользователя*.
- *Пароль* SecureString для пользователя.

Не забудьте использовать центр, который содержит политику РОПК.

### <a name="limitations-of-the-ropc-flow"></a>Ограничения потока РОПК
 - Поток РОПК **работает только для локальных учетных записей** (где вы регистрируетесь в Azure AD B2C с помощью электронной почты или имени пользователя). Этот поток не работает, если вы выполняете Федерацию с любыми поставщиками удостоверений, поддерживаемыми Azure AD B2C (Facebook, Google и т. д.).

## <a name="google-auth-and-embedded-webview"></a>Google auth и Embedded WebView

Если вы являетесь Azure AD B2C разработчиком, использующим Google в качестве поставщика удостоверений, мы переносимся к использованию системного браузера, так как Google не разрешает [проверку подлинности из внедренных](https://developers.googleblog.com/2016/08/modernizing-oauth-interactions-in-native-apps.html)веб-представлений. В настоящее время `login.microsoftonline.com` является доверенным центром Google. Использование этого центра будет работать с внедренными WebView. Однако использование `b2clogin.com` не является доверенным центром Google, поэтому пользователи не смогут пройти проверку подлинности.

Мы будем предоставлять обновление этой [проблемы](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/688) в случае изменения.

## <a name="caching-with-azure-ad-b2c-in-msalnet"></a>Кэширование с помощью Azure AD B2C в MSAL.Net 

### <a name="known-issue-with-azure-ad-b2c"></a>Известная ошибка Azure AD B2C

MSAL.Net поддерживает [кэш маркеров](/dotnet/api/microsoft.identity.client.tokencache?view=azure-dotnet). Ключ кэширования маркера основан на утверждениях, возвращенных поставщиком удостоверений. В настоящее время MSAL.Net требуется два утверждения для создания ключа кэша маркера:  
- `tid`, который является ИДЕНТИФИКАТОРом клиента Azure AD, и 
- `preferred_username` 

Оба эти утверждения отсутствуют во многих Azure AD B2C сценариях. 

Влияние клиента на то, что при попытке отобразить поле username вы получаете сообщение «отсутствует в ответе маркера» в качестве значения? Если да, это связано с тем, что Azure AD B2C не возвращает значение в IdToken для preferred_username из-за ограничений с учетными записями социальных сетей и внешними поставщиками удостоверений (поставщиков удостоверений). Azure AD возвращает значение для preferred_username, так как оно знает, кто является пользователем, но для Azure AD B2C, так как пользователь может войти в систему с помощью локальной учетной записи, Facebook, Google, GitHub и т. д. Azure AD B2C для preferred_username не имеет единообразного значения. Чтобы разблокировать MSAL от развертывания с помощью ADAL, мы решили использовать "отсутствует в ответе маркера" в нашем конце при работе с учетными записями Azure AD B2C, когда IdToken возвращает Nothing для preferred_username. MSAL должен возвращать значение для preferred_username, чтобы обеспечить совместимость кэша между библиотеками.

### <a name="workarounds"></a>Обходные решения

#### <a name="mitigation-for-the-missing-tenant-id"></a>Устранение рисков для отсутствующего идентификатора клиента

Предлагаемый обходной путь заключается в использовании [кэширования политикой](#acquire-a-token-to-apply-a-policy) .

Кроме того, можно использовать утверждение `tid`, если вы используете [пользовательские политики B2C](https://aka.ms/ief), так как оно предоставляет возможность возврата дополнительных утверждений приложению. Дополнительные сведения о [преобразовании утверждений](/azure/active-directory-b2c/claims-transformation-technical-profile)

#### <a name="mitigation-for-missing-from-the-token-response"></a>Устранение рисков "отсутствует в ответе маркера"
Один из вариантов — использовать утверждение Name в качестве предпочтительного имени пользователя. Этот процесс упоминается в этом [документе B2C doc](../../active-directory-b2c/user-flow-overview.md) -> "в столбце" Заявка на возврат "выберите утверждения, которые должны возвращаться в маркерах авторизации, отправляемых обратно в приложение после успешного редактирования профиля. Например, выберите отображаемое имя, почтовый индекс.

## <a name="next-steps"></a>Дальнейшие действия 

Дополнительные сведения о получении маркеров в интерактивном режиме с помощью MSAL.NET для Azure AD B2C приложений приведены в следующем примере.

| Образец | Платформа | Description|
|------ | -------- | -----------|
|[Active-Directory-B2C-Xamarin-Native](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) | Xamarin iOS, Xamarin Android, UWP | Простое приложение Xamarin Forms, демонстрирующие использование MSAL.NET для проверки подлинности пользователей с помощью Azure AD B2C, а также доступа к веб-API с результирующими маркерами.|
