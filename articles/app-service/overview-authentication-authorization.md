---
title: Аутентификация и авторизация
description: Узнайте о встроенной поддержке проверки подлинности и авторизации в службе приложений Azure и функциях Azure, а также о том, как она помогает защитить приложение от несанкционированного доступа.
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.topic: article
ms.date: 07/08/2020
ms.reviewer: mahender
ms.custom: seodec18, fasttrack-edit, has-adal-ref
ms.openlocfilehash: 1b95b1e96dc26fb72338518fc969c69b035d5f68
ms.sourcegitcommit: 5db975ced62cd095be587d99da01949222fc69a3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2020
ms.locfileid: "97095242"
---
# <a name="authentication-and-authorization-in-azure-app-service-and-azure-functions"></a>Проверка подлинности и авторизация в службе приложений Azure и функциях Azure

Служба приложений Azure обеспечивает встроенную поддержку проверки подлинности и авторизации, поэтому для входа пользователей в систему и предоставления доступа к данным необходимо написать минимальный код или обойтись без кода в веб-приложении, RESTfu API и серверной части мобильных приложений, а также в [Функциях Azure](../azure-functions/functions-overview.md). В этой статье описывается, как служба приложений помогает упростить проверку подлинности и авторизацию для вашего приложения.

Безопасные проверка подлинности и авторизация требуют глубокого понимания средств обеспечения безопасности, в том числе федерации, шифрования, управления [токенами JWT](https://wikipedia.org/wiki/JSON_Web_Token), [типов предоставления разрешений](https://oauth.net/2/grant-types/) и т. д. Служба приложений предоставляет эти служебные программы, чтобы вы могли уделять больше времени предоставлению бизнес-преимуществ для своего клиента.

> [!IMPORTANT]
> Для проверки подлинности и авторизации не требуется использовать эту функцию. Вы можете использовать объединенные функции безопасности в вашей веб-среде или написать собственные служебные программы. Однако следует помнить, что [хром 80 вносит коренные изменения в реализацию SameSite для файлов cookie](https://www.chromestatus.com/feature/5088147346030592) (Дата выпуска около марта 2020), а также пользовательскую удаленную проверку подлинности или другие сценарии, использующие межсайтовые публикации файлов cookie, могут прерываться при обновлении клиентских браузеров Chrome. Обходной путь является сложным, так как он должен поддерживать различные поведения SameSite для различных браузеров. 
>
> ASP.NET Core 2,1 и более поздних версий, размещенных в службе приложений, уже исправлены для этого критического изменения и соответствующим образом обрабатывают браузеры Chrome 80 и более ранние версии. Кроме того, одно и то же исправление для ASP.NET Framework 4.7.2 было развернуто в экземплярах службы приложений в течение 2020 января. Дополнительные сведения см. в [статье обновление файла cookie SameSite для службы приложений Azure](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/).
>

> [!NOTE]
> Функция проверки подлинности и авторизации иногда также называется простой проверкой подлинности.

> [!NOTE]
> Включение этой функции приведет к автоматическому перенаправлению **всех** небезопасных HTTP-запросов к приложению по протоколу HTTPS независимо от параметра конфигурации службы приложений для [принудительного применения HTTPS](configure-ssl-bindings.md#enforce-https). При необходимости его можно отключить с помощью `requireHttps` параметра в [файле конфигурации параметров проверки подлинности](app-service-authentication-how-to.md#configuration-file-reference), но необходимо убедиться, что маркеры безопасности не передаются через небезопасные подключения HTTP.

Сведения для собственных мобильных приложений см. в статье [Проверка подлинности и авторизация в мобильных приложениях Azure](/previous-versions/azure/app-service-mobile/app-service-mobile-auth).

## <a name="how-it-works"></a>Принцип работы

### <a name="on-windows"></a>В Windows

Модуль проверки подлинности и авторизации выполняется в той же песочнице, что и код приложения. Если он включен, каждый входящий HTTP-запрос проходит через него перед обработкой кодом вашего приложения.

![Схема архитектуры, показывающая запросы, перехваченные процессом в песочнице сайта, который взаимодействует с поставщиками удостоверений до разрешения трафика на развернутый сайт.](media/app-service-authentication-overview/architecture.png)

Этот модуль выполняет следующие операции для вашего приложения:

- проверку подлинности пользователей с указанным поставщиком;
- проверку, хранение и обновление токенов;
- управление сеансом с проверкой подлинности;
- вставка сведений об удостоверении в заголовки запросов.

Модуль работает отдельно от кода приложения и настраивается с помощью параметров приложения. Для кода приложения не нужны пакеты SDK, определенные языки или изменения. 

### <a name="on-containers"></a>В контейнерах

Модуль проверки подлинности и авторизации выполняется в отдельном контейнере, изолированном от кода приложения. Используя то, что известно как [шаблон посредник](/azure/architecture/patterns/ambassador), он взаимодействует с входящим трафиком для выполнения аналогичных функций в Windows. Поскольку она не выполняется в процессе, прямая интеграция с конкретными языковыми платформами невозможна; Однако соответствующие сведения, необходимые вашему приложению, передаются с помощью заголовков запросов, как описано ниже.

### <a name="userapplication-claims"></a>Утверждения для пользователей и приложений

Для всех языковых платформ служба приложений делает утверждения в входящем токене (от пользователя, прошедшего проверку подлинности или клиентское приложение) доступным для кода путем их вставки в заголовки запроса. Для приложений ASP.NET 4.6 служба приложений заполняет свойство [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) утверждениями пользователя, прошедшими проверку подлинности, поэтому вы можете следовать стандартным шаблонам кода .NET, включая атрибут `[Authorize]`. Аналогичным образом для приложений PHP служба приложений заполняет переменную `_SERVER['REMOTE_USER']`. Для приложений Java утверждения доступны в [Tomcat сервлета](configure-language-java.md#authenticate-users-easy-auth).

Для [функций Azure](../azure-functions/functions-overview.md) `ClaimsPrincipal.Current` не заполняется для кода .NET, но вы по-прежнему можете найти утверждения пользователя в заголовках запроса или получить `ClaimsPrincipal` объект из контекста запроса или даже через параметр привязки. Дополнительные сведения см. [в разделе Работа с удостоверениями клиентов](../azure-functions/functions-bindings-http-webhook-trigger.md#working-with-client-identities) .

Дополнительные сведения см. в разделе [Access user claims](app-service-authentication-how-to.md#access-user-claims) (Доступ к утверждениям пользователя).

> [!NOTE]
> В настоящее время ASP.NET Core не поддерживает заполнение текущего пользователя с помощью функции проверки подлинности и авторизации. Однако для заполнения этого зазора существуют некоторые [сторонние компоненты по промежуточного слоя с открытым кодом](https://github.com/MaximRouiller/MaximeRouiller.Azure.AppService.EasyAuth) .
>

### <a name="token-store"></a>Хранилище токенов

Служба приложений предоставляет встроенное хранилище токенов, которое представляет собой репозиторий токенов, связанных с пользователями ваших веб-приложений, API или собственных мобильных приложений. При включении проверки подлинности с помощью любого поставщика это хранилище токенов немедленно становится доступным для вашего приложения. Если для кода вашего приложения требуется доступ к данным из этих поставщиков от имени пользователя, чтобы: 

- опубликовать запись в ленте Facebook прошедшего проверку подлинности пользователя;
- чтение корпоративных данных пользователя с помощью API Microsoft Graph

Как правило, вам необходимо писать код для сбора, хранения и обновления этих токенов в своем приложении. В хранилище токенов при необходимости вы просто [извлекаете токены](app-service-authentication-how-to.md#retrieve-tokens-in-app-code) и [указываете службе приложений обновить их](app-service-authentication-how-to.md#refresh-identity-provider-tokens), если они становятся недопустимыми. 

Маркеры ID, маркеры доступа и маркеры обновления кэшируются для сеанса с проверкой подлинности и доступны только связанному пользователю.  

Если вам не нужно работать с токенами в приложении, хранилище токенов можно отключить на странице **аутентификации и авторизации** приложения.

### <a name="logging-and-tracing"></a>Ведение журнала и трассировка

Если вы [включите ведение журнала приложений](troubleshoot-diagnostic-logs.md), вы увидите трассировку проверки подлинности и авторизации непосредственно в файлах журналов. Если отображается сообщение об ошибке проверки подлинности, которое не ожидалось, вы можете найти все детали, просмотрев существующие журналы приложений. Если вы включили [трассировку неудачно завершенных запросов](troubleshoot-diagnostic-logs.md), вы можете точно определить, какую роль мог выполнять модуль проверки подлинности и авторизации в неудавшемся запросе. Найдите ссылки на модуль с именем `EasyAuthModule_32/64` в журналах трассировки. 

## <a name="identity-providers"></a>Поставщики удостоверений

Служба приложений использует [федеративную идентификацию](https://en.wikipedia.org/wiki/Federated_identity), при которой сторонний поставщик управляет удостоверениями пользователей и потоком проверки подлинности. По умолчанию доступны пять поставщиков удостоверений. 

| Поставщик | Конечная точка входа |
| - | - |
| [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) | `/.auth/login/aad` |
| [Учетная запись Майкрософт](../active-directory/develop/v2-overview.md) | `/.auth/login/microsoftaccount` |
| [Facebook](https://developers.facebook.com/docs/facebook-login) | `/.auth/login/facebook` |
| [Google](https://developers.google.com/identity/choose-auth) | `/.auth/login/google` |
| [Twitter](https://developer.twitter.com/en/docs/basics/authentication) | `/.auth/login/twitter` |
| Любой поставщик [OpenID Connect Connect](https://openid.net/connect/) (Предварительная версия) | `/.auth/login/<providerName>` |

При включении проверки подлинности и авторизации с помощью одного из этих поставщиков конечная точка входа станет доступной для проверки подлинности пользователя и для проверки токенов проверки подлинности от поставщика. Вы можете легко предоставить своим пользователям любое количество этих вариантов входа.

Существует [устаревший путь расширения][custom-auth] для интеграции с другими поставщиками удостоверений или настраиваемое решение проверки подлинности, но это не рекомендуется. Вместо этого рассмотрите возможность использования поддержки OpenID Connect Connect.

## <a name="authentication-flow"></a>Поток проверки подлинности

Поток проверки подлинности одинаков для всех поставщиков, но будет различным для разных способов входа в систему: с использованием пакета SDK поставщика или без.

- Без использования пакета SDK поставщика. Приложение делегирует федеративный вход в службу приложений. Обычно это относится к приложениям браузера, которые могут предоставить пользователю страницу входа поставщика. Код сервера управляет процессом входа, поэтому он также называется _управляемым сервером потоком_ или _потоком сервера_. Этот вариант применяется к браузерным приложениям. Он также относится к собственным приложениям, в которых вход пользователей в систему выполняется с помощью пакета SDK для клиента мобильных приложений. Пакет SDK открывает веб-представление, в котором пользователи выполняют вход с помощью проверки подлинности службы приложений. 
- С использованием пакета SDK поставщика. Вход пользователей в приложение на странице поставщика выполняется вручную, а затем приложение отправляет маркер проверки подлинности в Службу приложений Azure для проверки. Обычно это относится к внебраузерным приложениям, которые не могут предоставить пользователю страницу входа в систему. Код приложения управляет процессом входа, поэтому его также называют _управляемым клиентом потоком_ или _потоком клиента_. Этот вариант применяется к REST API, [Функциям Azure](../azure-functions/functions-overview.md) и клиентам браузера JavaScript, а также к браузерным приложениям, которым требуется больше гибкости при входе в систему. Это также относится к собственным мобильным приложениям, в которых вход пользователей в систему выполняется с помощью пакета SDK поставщика.

> [!NOTE]
> Вызовы из доверенного приложения браузера в службе приложений на другой REST API в службе приложений или в [функции Azure](../azure-functions/functions-overview.md) могут проходить проверку подлинности с помощью управляемого сервером потока. Дополнительные сведения см. в статье [Настройка проверки подлинности и авторизации в Службе приложений Azure](app-service-authentication-how-to.md).
>

В приведенной ниже таблице показаны шаги выполнения потока проверки подлинности.

| Шаг | Без использования пакета SDK поставщика | С использованием пакета SDK поставщика |
| - | - | - |
| 1. вход пользователя | Перенаправляет клиента к `/.auth/login/<provider>`. | Код клиента выполняет вход пользователя непосредственно через пакет SDK поставщика и получает токен проверки подлинности. Дополнительные сведения см. в документации поставщика. |
| 2. После проверки подлинности | Поставщик перенаправляет клиент к `/.auth/login/<provider>/callback`. | Клиентский код [отправляет токен из поставщика](app-service-authentication-how-to.md#validate-tokens-from-providers) в `/.auth/login/<provider>` для проверки. |
| 3. Установите сеанс с проверкой подлинности | Служба приложений добавляет файлы cookie, прошедшие проверку подлинности, в ответ. | Служба приложений возвращает собственный токен проверки подлинности в код клиента. |
| 4. предоставление содержимого, прошедшего проверку подлинности | Клиент включает файлы cookie, прошедшие проверку подлинности, в последующие запросы (автоматически обрабатываются браузером). | Код клиента предоставляет токен проверки подлинности в заголовке `X-ZUMO-AUTH` (автоматически обрабатывается пакетами SDK для клиента мобильных приложений). |

Для клиентских браузеров служба приложений может автоматически перенаправлять всех не прошедших проверку пользователей к `/.auth/login/<provider>`. Вы также можете отправить пользователям одну или несколько ссылок `/.auth/login/<provider>`, чтобы они вошли в ваше приложение с использованием поставщика по выбору.

<a name="authorization"></a>

## <a name="authorization-behavior"></a>Поведение авторизации

В [портал Azure](https://portal.azure.com)можно настроить авторизацию службы приложений с помощью ряда поведений, если входящий запрос не прошел проверку подлинности.

![Снимок экрана, показывающий раскрывающееся меню "действие, которое необходимо выполнить, если запрос не прошел проверку подлинности"](media/app-service-authentication-overview/authorization-flow.png)

Ниже описаны возможные варианты.

### <a name="allow-anonymous-requests-no-action"></a>Разрешить анонимные запросы (без действий)

Этот параметр откладывает авторизацию трафика, не прошедшего проверку подлинности, в код приложения. Для запросов, прошедших проверку подлинности, служба приложений также передает сведения о проверке подлинности в заголовках HTTP. 

Такой вариант обеспечивает большую гибкость в обработке анонимных запросов. Например, он позволяет [предоставлять пользователям несколько поставщиков входа](app-service-authentication-how-to.md#use-multiple-sign-in-providers). Тем не менее необходимо написать код. 

### <a name="allow-only-authenticated-requests"></a>Разрешить только запросы, прошедшие проверку подлинности

Параметр используется **для \<provider> входа в систему**. Служба приложений перенаправляет все анонимные запросы к `/.auth/login/<provider>` для выбранного вами поставщика. Если анонимный запрос поступает из собственного мобильного приложения, возвращаемый ответ `HTTP 401 Unauthorized`.

В этом случае в клиентском приложении не нужен код для проверки подлинности. Более точная авторизация, например авторизация для конкретной роли, может выполняться путем проверки утверждений пользователя (см. раздел [Access user claims](app-service-authentication-how-to.md#access-user-claims) (Доступ к утверждениям пользователя)).

> [!CAUTION]
> Таким образом, ограниченный доступ применяется ко всем вызовам приложения, что может быть нежелательно для приложений, которым требуется общедоступная Домашняя страница, как во многих одностраничных приложениях.

> [!NOTE]
> По умолчанию любой пользователь в клиенте Azure AD может запросить маркер для вашего приложения из Azure AD. Вы можете [настроить приложение в Azure AD](../active-directory/develop/howto-restrict-your-app-to-a-set-of-users.md) , если вы хотите ограничить доступ к приложению определенным набором пользователей.

## <a name="more-resources"></a>Дополнительные ресурсы

* [Руководство. Проверка подлинности и авторизация пользователей в веб-приложении, которое обращается к службе хранилища Azure и Microsoft Graph](scenario-secure-app-authentication-app-service.md)
* [Руководство по сквозной проверке подлинности и авторизации в службе приложений Azure (Windows)](tutorial-auth-aad.md)  
* [Руководство по сквозной проверке подлинности и авторизации в службе приложений Azure (Linux)](./tutorial-auth-aad.md?pivots=platform-linux%3fpivots%3dplatform-linux)  
* [Customize authentication and authorization in Azure App Service](app-service-authentication-how-to.md) (Настройка проверки подлинности и авторизации в Службе приложений Azure)
* [Интеграция .NET Core с Azure AppService Еасяус (третья сторона)](https://github.com/MaximRouiller/MaximeRouiller.Azure.AppService.EasyAuth)
* [Получение проверки подлинности службы приложений Azure с помощью .NET Core (третья сторона)](https://github.com/kirkone/KK.AspNetCore.EasyAuthAuthentication)

Практические руководства для поставщика:

* [Настройка приложения службы приложений для использования службы входа Azure Active Directory][AAD]
* [Как настроить приложение службы приложений для использования имени для входа Facebook][Facebook]
* [Настройка приложения для использования имени входа Google][Google]
* [Настройка приложения для использования входа по учетной записи Майкрософт][MSA]
* [Настройка приложения для использования имени входа Twitter][Twitter]
* [Настройка приложения для использования поставщика OpenID Connect Connect для входа (Предварительная версия)][OIDC]
* [Настройка приложения для использования входа с помощью Apple (Предварительная версия)][Apple]

[AAD]: configure-authentication-provider-aad.md
[Facebook]: configure-authentication-provider-facebook.md
[Google]: configure-authentication-provider-google.md
[MSA]: configure-authentication-provider-microsoft.md
[Twitter]: configure-authentication-provider-twitter.md
[OIDC]: configure-authentication-provider-openid-connect.md
[Apple]: configure-authentication-provider-apple.md

[custom-auth]: /previous-versions/azure/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk#custom-auth

[ADAL-Android]: /previous-versions/azure/app-service-mobile/app-service-mobile-android-how-to-use-client-library#adal
[ADAL-iOS]: /previous-versions/azure/app-service-mobile/app-service-mobile-ios-how-to-use-client-library#adal
[ADAL-dotnet]: /previous-versions/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library#adal
