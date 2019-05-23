---
title: включение файла
description: включение файла
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 3f72f6a5097221c904faff633b5a4ee5a6e023c1
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/23/2019
ms.locfileid: "66121601"
---
## <a name="use-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Получение маркера для API Microsoft Graph с помощью MSAL

В этом разделе вы получите маркер для API Microsoft Graph с помощью MSAL.

1. В файле *MainWindow.xaml.cs* добавьте ссылку для MSAL в класс:

    ```csharp
    using Microsoft.Identity.Client;
    ```

2. Замените код класса `MainWindow` следующим:

    ```csharp
    public partial class MainWindow : Window
    {
        //Set the API Endpoint to Graph 'me' endpoint
        string graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

        //Set the scope for API call to user.read
        string[] scopes = new string[] { "user.read" };


        public MainWindow()
        {
            InitializeComponent();
        }

      /// <summary>
        /// Call AcquireToken - to acquire a token requiring user to sign-in
        /// </summary>
        private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
        {
            AuthenticationResult authResult = null;
            var app = App.PublicClientApp;
            ResultText.Text = string.Empty;
            TokenInfoText.Text = string.Empty;

            var accounts = await app.GetAccountsAsync();
            var firstAccount = accounts.FirstOrDefault();

            try
            {
                authResult = await app.AcquireTokenSilent(scopes, firstAccount)
                    .ExecuteAsync();
            }
            catch (MsalUiRequiredException ex)
            {
                // A MsalUiRequiredException happened on AcquireTokenSilent.
                // This indicates you need to call AcquireTokenInteractive to acquire a token
                System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

                try
                {
                    authResult = await app.AcquireTokenInteractive(scopes)
                        .WithAccount(accounts.FirstOrDefault())
                        .WithPrompt(Prompt.SelectAccount)
                        .ExecuteAsync();
                }
                catch (MsalException msalex)
                {
                    ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
                }
            }
            catch (Exception ex)
            {
                ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
                return;
            }

            if (authResult != null)
            {
                ResultText.Text = await GetHttpContentWithToken(graphAPIEndpoint, authResult.AccessToken);
                DisplayBasicTokenInfo(authResult);
                this.SignOutButton.Visibility = Visibility.Visible;
            }
        }
    ```

<!--start-collapse-->
### <a name="more-information"></a>Дополнительные сведения

#### <a name="get-a-user-token-interactively"></a>Интерактивное получение маркера пользователя

При вызове метода `AcquireTokenInteractive` появится окно, в котором пользователю предлагается выполнить вход. Когда пользователь впервые запрашивает доступ к защищенному ресурсу, приложение обычно требует выполнить интерактивный вход. Пользователям также может потребоваться выполнить вход при сбое автоматической операции получения маркера (например, по истечении срока действия пароля пользователя).

#### <a name="get-a-user-token-silently"></a>Автоматическое получение маркера пользователя

Метод `AcquireTokenSilent` обрабатывает получение и обновление маркера без участия пользователя. После первого выполнения метода `AcquireTokenInteractive` обычно используется метод `AcquireTokenSilent`, чтобы получить маркеры для доступа к защищенным ресурсам для последующих вызовов, так как вызовы для запроса или обновления маркеров выполняются автоматически.

Иногда метод `AcquireTokenSilent` завершается ошибкой. Причина может заключаться в том, что пользователь вышел из системы или изменил пароль на другом устройстве. Когда MSAL обнаруживает, что эту проблему можно решить, запросив интерактивное действие, возникает исключение `MsalUiRequiredException`. Приложение может обработать это исключение двумя способами:

* Может немедленно вызвать `AcquireTokenInteractive`. Этот вызов приводит к появлению запроса на вход пользователя. Этот шаблон обычно используется в интерактивных приложениях, где пользователю недоступно автономное содержимое. Этот шаблон используется в примере, созданном в ходе настройки с помощью этих инструкций. Вы увидите его в действии при первом запуске примера. 

* Так как приложение еще не использовалось, `PublicClientApp.Users.FirstOrDefault()` будет содержать значение NULL и будет выдано исключение `MsalUiRequiredException`. 

* Код в примере обработает исключение, вызвав `AcquireTokenInteractive`, в результате чего пользователю будет предложено войти.

* Также приложение может визуально уведомить пользователей, что требуется интерактивный вход, чтобы они могли выбрать подходящее время для входа. Либо приложение может попытаться повторно выполнить метод `AcquireTokenSilent` позже. Этот шаблон часто применяется, когда пользователи могут использовать другие функциональные возможности приложения без прерывания работы — например, если в приложении доступно автономное содержимое. В этом случае пользователи могут решать, что им нужно сделать после входа — получить доступ к защищенному ресурсу или обновить устаревшие данные. Кроме того, приложение может попытаться повторно выполнить `AcquireTokenSilent` при восстановлении сети после временной недоступности.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Вызов API Microsoft Graph с помощью полученного маркера

Добавьте в `MainWindow.xaml.cs` следующий новый метод. Этот метод используется для выполнения запроса `GET` к API Graph с помощью заголовка авторизации "Authorize":

```csharp
/// <summary>
/// Perform an HTTP GET request to a URL using an HTTP Authorization header
/// </summary>
/// <param name="url">The URL</param>
/// <param name="token">The token</param>
/// <returns>String containing the results of the GET operation</returns>
public async Task<string> GetHttpContentWithToken(string url, string token)
{
    var httpClient = new System.Net.Http.HttpClient();
    System.Net.Http.HttpResponseMessage response;
    try
    {
        var request = new System.Net.Http.HttpRequestMessage(System.Net.Http.HttpMethod.Get, url);
        //Add the token in Authorization header
        request.Headers.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
        response = await httpClient.SendAsync(request);
        var content = await response.Content.ReadAsStringAsync();
        return content;
    }
    catch (Exception ex)
    {
        return ex.ToString();
    }
}
```

<!--start-collapse-->
### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Дополнительные сведения о вызове REST через защищенный API

В этом примере приложения используется метод `GetHttpContentWithToken` для выполнения HTTP-запроса `GET` к защищенному ресурсу, требующему маркер, а затем возвращает содержимое вызывающему объекту. Этот метод добавляет полученный маркер в заголовок авторизации HTTP. В этом примере ресурс — это конечная точка *me* API Microsoft Graph, которая отображает сведения о профиле пользователя.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-a-user"></a>Добавление метода для выхода пользователя

Добавьте следующий метод в файл `MainWindow.xaml.cs` для выхода пользователя:

```csharp
/// <summary>
/// Sign out the current user
/// </summary>
private async void SignOutButton_Click(object sender, RoutedEventArgs e)
{
    var accounts = await App.PublicClientApp.GetAccountsAsync(); 

    if (accounts.Any())
    {
        try
        {
            await App.PublicClientApp.RemoveAsync(accounts.FirstOrDefault());
            this.ResultText.Text = "User has signed-out";
            this.CallGraphButton.Visibility = Visibility.Visible;
            this.SignOutButton.Visibility = Visibility.Collapsed;
        }
        catch (MsalException ex)
        {
            ResultText.Text = $"Error signing-out user: {ex.Message}";
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information-about-user-sign-out"></a>Дополнительные сведения о выходе пользователя

Метод `SignOutButton_Click` удаляет пользователей из кэша пользователей MSAL. Фактически это приводит к удалению данных текущего пользователя из MSAL, поэтому в дальнейшем успешно выполняться будут только интерактивные запросы на получение маркеров.

Хотя приложение в этом примере поддерживает одного пользователя, MSAL поддерживает сценарии, в которых несколько пользователей могут войти в систему одновременно. Пример — приложение электронной почты, в котором у пользователя есть несколько учетных записей.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Отображение основных сведений о маркере

Для отображения основных сведений о маркере добавьте следующий метод в файл *MainWindow.xaml.cs*:

```csharp
/// <summary>
/// Display basic information contained in the token
/// </summary>
private void DisplayBasicTokenInfo(AuthenticationResult authResult)
{
    TokenInfoText.Text = "";
    if (authResult != null)
    {
        TokenInfoText.Text += $"Username: {authResult.Account.Username}" + Environment.NewLine;
        TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Дополнительные сведения

После входа пользователя MSAL получает идентификатор маркера в дополнение к маркеру доступа, который используется для вызова Microsoft Graph API. Этот маркер содержит небольшое подмножество сведений, имеющие отношение к пользователям. Метод `DisplayBasicTokenInfo` отображает основные сведения, содержащиеся в маркере. Например, он показывает отображаемое имя и идентификатор пользователя, а также дату истечения срока действия маркера и строку, представляющую сам маркер доступа. Нажав кнопку *вызова API Microsoft Graph* несколько раз, вы увидите, что для последующих запросов повторно используется тот же маркер. Вы также можете увидеть, что библиотека MSAL продлила срок действия.
<!--end-collapse-->
