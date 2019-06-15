---
title: Управление сеансами. Средство моделирования угроз Microsoft Azure | Документация Майкрософт
description: Устранение угроз, обнаруженных с помощью средства моделирования угроз
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.openlocfilehash: e8f3cf3889b3f79e930630ff0e768a0c4875eec6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60611693"
---
# <a name="security-frame-session-management"></a>Механизм безопасности. Управление сеансом
| Продукт или служба | Статья |
| --------------- | ------- |
| **Azure AD**    | <ul><li>[Реализуйте надлежащее событие выхода с помощью методов ADAL при использовании Azure AD](#logout-adal)</li></ul> |
| Устройства Интернета вещей | <ul><li>[Используйте токены SaS с ограниченным временем существования](#finite-tokens)</li></ul> |
| **Azure DocumentDB** | <ul><li>[Используйте токены ресурсов с минимальным временем существования](#resource-tokens)</li></ul> |
| **ADFS** | <ul><li>[Реализуйте надлежащее событие выхода с помощью методов WsFederation при использовании ADFS](#wsfederation-logout)</li></ul> |
| **Сервер удостоверений** | <ul><li>[Реализуйте надлежащее событие выхода при использовании сервера удостоверений](#proper-logout)</li></ul> |
| **Веб-приложение** | <ul><li>[Используйте защищенные файлы cookie в приложениях, использующих протокол HTTPS](#https-secure-cookies)</li><li>[Укажите атрибут httpOnly в определении файла cookie всех приложений на основе протокола HTTP](#cookie-definition)</li><li>[Уменьшите риск атак с подделкой межсайтовых запросов на веб-страницах ASP.NET](#csrf-asp)</li><li>[Настройте период бездействия сеанса](#inactivity-lifetime)</li><li>[Реализуйте правильный выход из приложения](#proper-app-logout)</li></ul> |
| **Веб-интерфейс API** | <ul><li>[Уменьшите риск атак с подделкой межсайтовых запросов в веб-интерфейсах API ASP.NET](#csrf-api)</li></ul> |

## <a id="logout-adal"></a>Реализуйте надлежащее событие выхода с помощью методов ADAL при использовании Azure AD

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Azure AD | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | Универсальный |
| **Атрибуты**              | Н/Д  |
| **Справочные материалы**              | Н/Д  |
| **Действия** | Если приложение использует маркер доступа, выданный Azure AD, необходимо вызывать обработчик событий выхода. |

### <a name="example"></a>Пример
```csharp
HttpContext.GetOwinContext().Authentication.SignOut(OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType)
```

### <a name="example"></a>Пример
Кроме того, также необходимо уничтожить сеанс пользователя путем вызова метода Session.Abandon(). Ниже приведен метод безопасной реализации события выхода пользователя.
```csharp
    [HttpPost]
        [ValidateAntiForgeryToken]
        public void LogOff()
        {
            string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            AuthenticationContext authContext = new AuthenticationContext(Authority + TenantId, new NaiveSessionCache(userObjectID));
            authContext.TokenCache.Clear();
            Session.Clear();
            Session.Abandon();
            Response.SetCookie(new HttpCookie("ASP.NET_SessionId", string.Empty));
            HttpContext.GetOwinContext().Authentication.SignOut(
                OpenIdConnectAuthenticationDefaults.AuthenticationType,
                CookieAuthenticationDefaults.AuthenticationType);
        } 
```

## <a id="finite-tokens"></a>Используйте токены SaS с ограниченным временем существования

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Устройства Интернета вещей | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | Универсальный |
| **Атрибуты**              | Н/Д  |
| **Справочные материалы**              | Н/Д  |
| **Действия** | Токены SaS, созданные для проверки подлинности Центра Интернета вещей Azure, должны иметь ограниченный срок действия. Задайте минимальный срок действия, чтобы ограничить срок повторного воспроизведения токенов SaS в случае их компрометирования.|

## <a id="resource-tokens"></a>Используйте токены ресурсов с минимальным временем существования

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Azure DocumentDB | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | Универсальный |
| **Атрибуты**              | Н/Д  |
| **Справочные материалы**              | Н/Д  |
| **Действия** | Задайте минимальное время существования токенов ресурсов. Допустимый интервал времени по умолчанию для маркеров ресурсов составляет 1 час.|

## <a id="wsfederation-logout"></a>Реализуйте надлежащее событие выхода с помощью методов WsFederation при использовании ADFS

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | ADFS | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | Универсальный |
| **Атрибуты**              | Н/Д  |
| **Справочные материалы**              | Н/Д  |
| **Действия** | Если приложение использует токен службы токенов безопасности, выданный ADFS, чтобы выйти из системы пользователя, обработчик событий выхода должен вызвать метод WSFederationAuthenticationModule.FederatedSignOut(). Кроме того, необходимо уничтожить текущий сеанс, а также сбросить и обнулить значение токена сеанса.|

### <a name="example"></a>Пример
```csharp
        [HttpPost, ValidateAntiForgeryToken]
        [Authorization]
        public ActionResult SignOut(string redirectUrl)
        {
            if (!this.User.Identity.IsAuthenticated)
            {
                return this.View("LogOff", null);
            }

            // Removes the user profile.
            this.Session.Clear();
            this.Session.Abandon();
            HttpContext.Current.Response.Cookies.Add(new System.Web.HttpCookie("ASP.NET_SessionId", string.Empty)
                {
                    Expires = DateTime.Now.AddDays(-1D),
                    Secure = true,
                    HttpOnly = true
                });

            // Signs out at the specified security token service (STS) by using the WS-Federation protocol.
            Uri signOutUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Issuer);
            Uri replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm);
            if (!string.IsNullOrEmpty(redirectUrl))
            {
                replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm + redirectUrl);
            }
           //     Signs out of the current session and raises the appropriate events.
            var authModule = FederatedAuthentication.WSFederationAuthenticationModule;
            authModule.SignOut(false);
        //     Signs out at the specified security token service (STS) by using the WS-Federation
        //     protocol.            
            WSFederationAuthenticationModule.FederatedSignOut(signOutUrl, replyUrl);
            return new RedirectResult(redirectUrl);
        }
```

## <a id="proper-logout"></a>Реализуйте надлежащее событие выхода при использовании сервера удостоверений

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Сервер удостоверений | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | Универсальный |
| **Атрибуты**              | Н/Д  |
| **Справочные материалы**              | [Сведения о федеративном выходе на платформе IdentityServer3](https://identityserver.github.io/Documentation/docsv2/advanced/federated-signout.html) |
| **Действия** | IdentityServer поддерживает возможность создавать федерацию с внешними поставщиками удостоверений. В зависимости от используемого протокола вы можете настроить отправку уведомлений о выходе пользователя из системы поставщика удостоверений. Это уведомление позволит клиентам IdentityServer также выйти из системы пользователя. Сведения о реализации этого подхода см. в документации в разделе справочных сведений.|

## <a id="https-secure-cookies"></a>Используйте защищенные файлы cookie в приложениях, использующих протокол HTTPS

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Веб-приложение | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | Универсальный |
| **Атрибуты**              | EnvironmentType: OnPrem |
| **Справочные материалы**              | [Элемент httpCookies (схема параметров ASP.NET)](https://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [Свойство HttpCookie.Secure](https://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx) |
| **Действия** | Как правило, файлы cookie доступны только в домене, для которого они были заданы. К сожалению, определение "домен" не включает в себя протокол, и поэтому файлы cookie, созданные по протоколу HTTPS, доступны по протоколу HTTP. Атрибут secure указывает браузеру, что файлы cookie доступны только по протоколу HTTPS. Убедитесь, что во всех этих файлах, заданных по протоколу HTTPS, используется атрибут **secure**. Это требование можно выполнить в файле web.config, задав атрибуту requireSSL значение true. Мы рекомендуем использовать этот подход, так как он позволяет применить атрибут **secure** для всех текущих и будущих файлов cookie, не внося дополнительные изменения в код.|

### <a name="example"></a>Пример
```csharp
<configuration>
  <system.web>
    <httpCookies requireSSL="true"/>
  </system.web>
</configuration>
```
Этот параметр применяется, даже если доступ к приложению осуществляется по протоколу HTTP. Если для доступа к приложению используется протокол HTTP, то этот параметр приводит к сбою приложения, так как из-за атрибута secure браузер не сможет возвращать файлы cookie в приложение.

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Веб-приложение | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | Веб-формы, MVC 5 |
| **Атрибуты**              | EnvironmentType: OnPrem |
| **Справочные материалы**              | Н/Д  |
| **Действия** | Если веб-приложение выступает проверяющей стороной, а в качестве сервера ADFS используется IdP, атрибут secure в токене FedAuth можно настроить, задав в разделе `system.identityModel.services` файла web.config для параметра requireSSL значение true.|

### <a name="example"></a>Пример
```csharp
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:20:0" />
    ....  
    </federationConfiguration>
  </system.identityModel.services>
```

## <a id="cookie-definition"></a>Укажите атрибут httpOnly в определении файла cookie всех приложений на основе протокола HTTP

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Веб-приложение | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | Универсальный |
| **Атрибуты**              | Н/Д  |
| **Справочные материалы**              | [Сведения об атрибуте secure в файлах cookie](https://en.wikipedia.org/wiki/HTTP_cookie#Secure_cookie) |
| **Действия** | Чтобы устранить угрозу раскрытия информации при атаках с использованием межсайтового сценария, в файлы cookie можно добавить новый атрибут httpOnly, поддерживаемый во всех основных браузерах. Атрибут делает файл cookie недоступным для сценария. С помощью атрибута httpOnly веб-приложение снижает возможность кражи конфиденциальной информации, содержащейся в файле cookie, через сценарий и ее отправки злоумышленнику. |

### <a name="example"></a>Пример
Во всех приложениях на основе HTTP, использующих файлы cookie, необходимо определить атрибут httpOnly в определении файла cookie. Это можно сделать, внедрив в файл web.config следующую конфигурацию:
```XML
<system.web>
.
.
   <httpCookies requireSSL="false" httpOnlyCookies="true"/>
.
.
</system.web>
```

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Веб-приложение | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | Веб-формы |
| **Атрибуты**              | Н/Д  |
| **Справочные материалы**              | [Свойство FormsAuthentication.RequireSSL](https://msdn.microsoft.com/library/system.web.security.formsauthentication.requiressl.aspx) |
| **Действия** | Значение свойства RequireSSL задается в файле конфигурации web.config для приложения ASP.NET с помощью атрибута requireSSL. Этот атрибут также позволяет указать, нужно ли использовать протокол SSL для возвращения билета проверки подлинности (файла cookie) на сервер.|

### <a name="example"></a>Пример 
Следующий пример кода задает атрибут requireSSL в файле web.config.
```XML
<authentication mode="Forms">
  <forms loginUrl="member_login.aspx" cookieless="UseCookies" requireSSL="true"/>
</authentication>
```

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Веб-приложение | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | MVC5 |
| **Атрибуты**              | EnvironmentType: OnPrem |
| **Справочные материалы**              | [Запись блога о конфигурации Windows Identity Foundation (WIF). Часть 2](https://blogs.msdn.microsoft.com/alikl/2011/02/01/windows-identity-foundation-wif-configuration-part-ii-cookiehandler-chunkedcookiehandler-customcookiehandler/) |
| **Действия** | Чтобы задать атрибут httpOnly для файлов cookie FedAuth, атрибут hideFromCsript должен иметь значение true. |

### <a name="example"></a>Пример
Ниже приведена правильная конфигурация.
```XML
<federatedAuthentication>
<cookieHandler mode="Custom"
                       hideFromScript="true"
                       name="FedAuth"
                       path="/"
                       requireSsl="true"
                       persistentSessionLifetime="25">
</cookieHandler>
</federatedAuthentication>
```

## <a id="csrf-asp"></a>Уменьшите риск атак с подделкой межсайтовых запросов на веб-страницах ASP.NET

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Веб-приложение | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | Универсальный |
| **Атрибуты**              | Н/Д  |
| **Справочные материалы**              | Н/Д  |
| **Действия** | Подделка межсайтовых запросов (CSRF или XSRF) — это тип атак, при которых злоумышленник может выполнять действия в контексте безопасности разных пользовательских активных сеансов на веб-сайте. Целью является изменение или удаление содержимого, если аутентификация полученных на целевом веб-сайте запросов выполняется исключительно с помощью файлов cookie сеанса. Злоумышленник может воспользоваться этой уязвимостью и внедрить в код сайта, на который выполнил вход пользователь, команду, которая перенаправляет пользователя на другой URL-адрес. Это можно сделать несколькими способами, например, разместив другой веб-сайт, который скачивает ресурсы с уязвимого сервера, или заставив пользователя щелкнуть ссылку. Чтобы предотвратить эту атаку, сервер должен отправлять дополнительный токен клиенту, добавлять его во все последующие запросы клиента, а затем проверять наличие этого токена, относящегося к текущему сеансу, во всех будущих запросах, например с помощью метода AntiForgeryToken или ViewState ASP.NET. |

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Веб-приложение | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | MVC 5, MVC 6 |
| **Атрибуты**              | Н/Д  |
| **Справочные материалы**              | [Сведения о предотвращении подделки межсайтовых запросов в ASP.NET MVC и на веб-страницах ASP.NET](https://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) |
| **Действия** | Формы Anti-CSRF и ASP.NET MVC. Используйте в представлениях вспомогательный метод `AntiForgeryToken`. Вставьте `Html.AntiForgeryToken()` в форму.|

### <a name="example"></a>Пример
```csharp
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
```

### <a name="example"></a>Пример
```csharp
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a>Пример
В то же время Html.AntiForgeryToken() предоставляет посетителю файл cookie __RequestVerificationToken с таким же значением, как и случайное скрытое значение, показанное выше. Затем добавьте в целевой метод действия фильтр [ValidateAntiForgeryToken], чтобы проверить входящую форму POST. Пример:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Фильтр авторизации, проверяющий, что:
* Входящий запрос имеет файл cookie с именем __RequestVerificationToken.
* Входящий запрос имеет запись `Request.Form` с именем __RequestVerificationToken.
* Эти значения файла cookie и `Request.Form` совпадают. При условии, что все параметры заданы правильно, запрос выполняется в обычном режиме. В противном случае проверка подлинности завершается ошибкой и отображается следующее сообщение: "Обязательный маркер против подделки не был предоставлен или был недопустимым". 

### <a name="example"></a>Пример
Anti-CSRF и AJAX: Маркера формы может стать проблемой для запросов AJAX, так как запрос AJAX может отправлять данные JSON, не данные HTML-формы. В этом случае можно отправлять токены в настраиваемый заголовок HTTP. Следующий код создает токены с использованием синтаксиса Razor, а затем добавляет их в запрос AJAX. 
```csharp
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }

    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a>Пример
При обработке запроса извлеките токены из его заголовка. Затем вызовите метод AntiForgery.Validate для проверки этих токенов. Если токены недопустимые, метод AntiForgery.Validate вызывает исключение.
```csharp
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Веб-приложение | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | Веб-формы |
| **Атрибуты**              | Н/Д  |
| **Справочные материалы**              | [Сведения о предотвращении сетевых атак с помощью встроенных функций ASP.NET](https://msdn.microsoft.com/library/ms972969.aspx#securitybarriers_topic2) |
| **Действия** | Подделку межсайтовых запросов в приложениях на основе веб-форм можно предотвратить, добавив параметр ViewStateUserKey в случайную строку, уникальную для каждого пользователя, например в строку с идентификатором пользователя, а еще лучше с идентификатором сеанса. По разным техническим и социальным причинам идентификатор сеанса подходит гораздо лучше, так как его нельзя предсказать, время его ожидания истекает и он уникальный для каждого пользователя.|

### <a name="example"></a>Пример
Ниже приведен код, который необходимо добавить на всех страницах.
```csharp
void Page_Init (object sender, EventArgs e) {
   ViewStateUserKey = Session.SessionID;
   :
}
```

## <a id="inactivity-lifetime"></a>Настройте период бездействия сеанса

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Веб-приложение | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | Универсальный |
| **Атрибуты**              | Н/Д  |
| **Справочные материалы**              | [Свойство HttpSessionState.Timeout](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx) |
| **Действия** | Время ожидания сеанса — это событие, возникающее, когда пользователь не выполняет никаких действий на веб-сайте период времени (заданного сервером). Это событие на стороне сервера изменяет состояние сеанса пользователя на "Недопустимый" (например, "больше не используется") и дает указание веб-серверу уничтожить его (удалить все данные сеанса). Пример кода ниже задает для атрибута времени ожидания сеанса значение 15 минут в файле web.config.|

### <a name="example"></a>Пример
```XML 
<configuration>
  <system.web>
    <sessionState mode="InProc" cookieless="true" timeout="15" />
  </system.web>
</configuration>
```

## <a id="threat-detection"></a>Включение обнаружения угроз в Azure SQL

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Веб-приложение | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | Веб-формы |
| **Атрибуты**              | Н/Д  |
| **Справочные материалы**              | [Элемент Forms для проверки подлинности (схема параметров ASP.NET)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx) |
| **Действия** | Задайте для параметра времени ожидания билета проверки подлинности (файла cookie) значение, не превышающее 15 минут.|

### <a name="example"></a>Пример
```XML
<forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms>
```

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Веб-приложение | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | Веб-формы, MVC 5 |
| **Атрибуты**              | EnvironmentType: OnPrem |
| **Справочные материалы**              | [asdeqa](https://skf.azurewebsites.net/Mitigations/Details/wefr) |
| **Действия** | Когда веб-приложение является проверяющей стороной, а ADFS выступает службой токенов безопасности, время существования файлов cookie проверки подлинности (токенов FedAuth) задается с использованием следующей конфигурации в файле web.config:|

### <a name="example"></a>Пример
```XML
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:15:0" />
      <!-- Set requireHttps=true; -->
      <wsFederation passiveRedirectEnabled="true" issuer="http://localhost:39529/" realm="https://localhost:44302/" reply="https://localhost:44302/" requireHttps="true"/>
      <!--
      Use the code below to enable encryption-decryption of claims received from ADFS. Thumbprint value varies based on the certificate being used.
      <serviceCertificate>
        <certificateReference findValue="4FBBBA33A1D11A9022A5BF3492FF83320007686A" storeLocation="LocalMachine" storeName="My" x509FindType="FindByThumbprint" />
      </serviceCertificate>
      -->
    </federationConfiguration>
  </system.identityModel.services>
```

### <a name="example"></a>Пример
Время существования токена утверждений, выданного ADFS, также должно составлять 15 минут. Чтобы задать это значение, выполните на сервере ADFS следующий командлет PowerShell:
```csharp
Set-ADFSRelyingPartyTrust -TargetName “<RelyingPartyWebApp>” -ClaimsProviderName @(“Active Directory”) -TokenLifetime 15 -AlwaysRequireAuthentication $true
```

## <a id="proper-app-logout"></a>Реализуйте правильный выход из приложения

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Веб-приложение | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | Универсальный |
| **Атрибуты**              | Н/Д  |
| **Справочные материалы**              | Н/Д  |
| **Действия** | После нажатия кнопки выхода приложение должно правильно завершить этот процесс. То есть оно должно уничтожить сеанс пользователя, а также сбросить и обнулить значение файла cookie сеанса и файла cookie проверки подлинности. Кроме того, когда к идентификатору пользователя привязаны несколько сеансов, они должны завершаться все вместе на стороне сервера во время ожидания или выхода. Наконец, убедитесь, что функция выхода доступна на каждой странице. |

## <a id="csrf-api"></a>Уменьшите риск атак с подделкой межсайтовых запросов в веб-интерфейсах API ASP.NET

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Веб-API | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | Универсальный |
| **Атрибуты**              | Н/Д  |
| **Справочные материалы**              | Н/Д  |
| **Действия** | Подделка межсайтовых запросов (CSRF или XSRF) — это тип атак, при которых злоумышленник может выполнять действия в контексте безопасности разных пользовательских активных сеансов на веб-сайте. Целью является изменение или удаление содержимого, если аутентификация полученных на целевом веб-сайте запросов выполняется исключительно с помощью файлов cookie сеанса. Злоумышленник может воспользоваться этой уязвимостью и внедрить в код сайта, на который выполнил вход пользователь, команду, которая перенаправляет пользователя на другой URL-адрес. Это можно сделать несколькими способами, например, разместив другой веб-сайт, который скачивает ресурсы с уязвимого сервера, или заставив пользователя щелкнуть ссылку. Чтобы предотвратить эту атаку, сервер должен отправлять дополнительный токен клиенту, добавлять его во все последующие запросы клиента, а затем проверять наличие этого токена, относящегося к текущему сеансу, во всех будущих запросах, например с помощью метода AntiForgeryToken или ViewState ASP.NET. |

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Веб-API | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | MVC 5, MVC 6 |
| **Атрибуты**              | Н/Д  |
| **Справочные материалы**              | [Статья о предотвращении риска атак с подделкой межсайтовых запросов в веб-интерфейсах API ASP.NET](https://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) |
| **Действия** | Anti-CSRF и AJAX: Маркера формы может стать проблемой для запросов AJAX, так как запрос AJAX может отправлять данные JSON, не данные HTML-формы. В этом случае можно отправлять токены в настраиваемый заголовок HTTP. Следующий код создает токены с использованием синтаксиса Razor, а затем добавляет их в запрос AJAX. |

### <a name="example"></a>Пример
```Javascript
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }
    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a>Пример
При обработке запроса извлеките токены из его заголовка. Затем вызовите метод AntiForgery.Validate для проверки этих токенов. Если токены недопустимые, метод AntiForgery.Validate вызывает исключение.
```csharp
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

### <a name="example"></a>Пример
Формы Anti-CSRF и ASP.NET MVC. Используйте в представлениях вспомогательный метод AntiForgeryToken. Вставьте Html.AntiForgeryToken() в форму.
```csharp
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
}
```

### <a name="example"></a>Пример
Выходные данные приведенного выше примера будут примерно следующие:
```csharp
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a>Пример
В то же время Html.AntiForgeryToken() предоставляет посетителю файл cookie __RequestVerificationToken с таким же значением, как и случайное скрытое значение, показанное выше. Затем добавьте в целевой метод действия фильтр [ValidateAntiForgeryToken], чтобы проверить входящую форму POST. Пример:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Фильтр авторизации, проверяющий, что:
* Входящий запрос имеет файл cookie с именем __RequestVerificationToken.
* Входящий запрос имеет запись `Request.Form` с именем __RequestVerificationToken.
* Эти значения файла cookie и `Request.Form` совпадают. При условии, что все параметры заданы правильно, запрос выполняется в обычном режиме. В противном случае проверка подлинности завершается ошибкой и отображается следующее сообщение: "Обязательный маркер против подделки не был предоставлен или был недопустимым".

| Название                   | Сведения      |
| ----------------------- | ------------ |
| **Компонент**               | Веб-API | 
| **Этап SDL**               | Создание |  
| **Применимые технологии** | MVC 5, MVC 6 |
| **Атрибуты**              | Поставщик удостоверений — ADFS или Azure AD |
| **Справочные материалы**              | [Статья об обеспечении безопасности веб-интерфейсов API с помощью отдельной учетной записи и локального имени входа в веб-API ASP.NET 2.2](https://www.asp.net/web-api/overview/security/individual-accounts-in-web-api) |
| **Действия** | Если для защиты веб-интерфейса API используется OAuth 2.0, в заголовке запроса авторизации должен находиться токен носителя. Доступ к веб-приложению предоставляется, только если этот токен в запросе действительный. В отличие от проверки подлинности на основе файлов cookie браузеры не добавляют токены носителя к запросам. Клиент, отправивший запрос, должен явным образом прикрепить токен носителя в заголовке запроса. Таким образом, в веб-интерфейсах API ASP.NET, защищенных с помощью OAuth 2.0, токены носителя используются для защиты от подделки межсайтовых запросов. Обратите внимание, что если сегмент MVC приложения использует проверку подлинности на основе форм (то есть использует файлы cookie), в веб-приложении MVC необходимо использовать токены защиты от подделки. |

### <a name="example"></a>Пример
Веб-API должен использовать только токены носителя, а не файлы cookie. Это можно сделать с помощью следующей настройки в методе `WebApiConfig.Register`.

```csharp
config.SuppressDefaultHostAuthentication();
config.Filters.Add(new HostAuthenticationFilter(OAuthDefaults.AuthenticationType));
```

Метод SuppressDefaultHostAuthentication сообщает веб-API, что необходимо игнорировать любые проверки подлинности, которые происходят прежде, чем запрос достигнет конвейера веб-API, либо с помощью службы IIS, либо с помощью ПО промежуточного слоя OWIN. Таким образом можно ограничить веб-API для проверки подлинности только с помощью токенов носителя.
