---
title: Краткое руководство. Защита веб-API с помощью Azure Active Directory
description: Узнайте, как создать веб-интерфейс REST API на основе Node.js, который интегрируется с Azure AD для аутентификации.
services: active-directory
documentationcenter: nodejs
author: rwike77
manager: CelesteDG
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: ryanwi
ms.custom: seo-javascript-september2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7307afb2ce9e10326c42f96733d3f80979c4a4b3
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/17/2019
ms.locfileid: "71056539"
---
# <a name="quickstart-secure-a-web-api-with-azure-active-directory"></a>Краткое руководство. Защита веб-API с помощью Azure Active Directory

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

В этом руководстве показано, как защитить конечную точку API [Restify](http://restify.com/) с [Passport](http://passportjs.org/) с помощью модуля [passport azure ad](https://github.com/AzureAD/passport-azure-ad) для реализации обмена данными с Azure Active Directory (AAD).

В рамках этого краткого руководства рассматриваются вопросы защиты конечных точек API. Здесь не рассматриваются вопросы входа в систему и сохранения маркеров проверки подлинности. За это отвечает приложение клиента. Дополнительные сведения о реализации клиента см. в статье [Вход в веб-приложение Node.js и выход из него с помощью Azure AD](quickstart-v1-openid-connect-code.md).

Полный образец кода, используемого в этой статье, можно найти в [GitHub](https://github.com/Azure-Samples/active-directory-node-webapi-basic).

## <a name="prerequisites"></a>Предварительные требования

Чтобы начать работу, выполните следующие предварительные требования.

### <a name="create-the-sample-project"></a>Создание примера проекта

Для работы этого серверного приложения требуется несколько зависимостей пакета для поддержки Restify и Passport, а также данные учетной записи, передаваемые в AAD.

Чтобы начать, добавьте следующий код в файл с именем `package.json`:

```Shell
{
  "name": "active-directory-webapi-nodejs",
  "version": "0.0.1",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "passport": "0.4.0",
    "passport-azure-ad": "4.0.0",
    "restify": "7.7.0"
  }
}
```

После создания `package.json` запустите `npm install` в командной строке, чтобы установить зависимости пакета.

#### <a name="configure-the-project-to-use-active-directory"></a>Настройка проекта для использования Active Directory

Чтобы приступить к настройке приложения, необходимо получить несколько значений, связанных с учетной записью, с помощью Azure CLI. Чтобы приступить к работе с CLI, проще всего использовать Azure Cloud Shell.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

В Cloud Shell введите следующую команду:

```azurecli-interactive
az ad app create --display-name node-aad-demo --homepage http://localhost --identifier-uris http://node-aad-demo
```

Ниже приведены [аргументы](/cli/azure/ad/app?view=azure-cli-latest#az-ad-app-create) для команды `create`.

| Аргумент  | ОПИСАНИЕ |
|---------|---------|
|`display-name` | Понятное имя регистрации. |
|`homepage` | URL-адрес, по которому пользователи могут войти и использовать приложение. |
|`identifier-uris` | Разделенные пробелами уникальные универсальные коды ресурсов (URI), которые может использовать Azure AD для этого приложения. |

Чтобы подключиться к Azure Active Directory, необходимы следующие сведения:

| ИМЯ  | ОПИСАНИЕ | Имя переменной в файле конфигурации. |
| ------------- | ------------- | ------------- |
| Имя клиента.  | [Имя клиента](quickstart-create-new-tenant.md), которое нужно использовать для проверки подлинности. | `tenantName`  |
| Идентификатор клиента  | Идентификатор клиента — это термин OAuth, используемый для _идентификатора приложения_ AAD. |  `clientID`  |

Скопируйте из ответа регистрации в Azure Cloud Shell значение `appId` и создайте файл с именем `config.js`. Затем добавьте следующий код и замените значения маркерами в квадратных скобках:

```JavaScript
const tenantName    = //<YOUR_TENANT_NAME>;
const clientID      = //<YOUR_APP_ID_FROM_CLOUD_SHELL>;
const serverPort    = 3000;

module.exports.serverPort = serverPort;

module.exports.credentials = {
  identityMetadata: `https://login.microsoftonline.com/${tenantName}.onmicrosoft.com/.well-known/openid-configuration`, 
  clientID: clientID
};
```

Дополнительные сведения об отдельных параметрах конфигурации см. в документации модуля [passport azure ad](https://github.com/AzureAD/passport-azure-ad#5-usage).

### <a name="implement-the-server"></a>Реализация сервера

Модуль [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad#5-usage) имеет две стратегии проверки подлинности: [OIDC](https://github.com/AzureAD/passport-azure-ad#51-oidcstrategy) и [Носитель](https://github.com/AzureAD/passport-azure-ad#52-bearerstrategy). Сервер, реализованный в этой статье, использует стратегию на основе носителя для защиты конечной точки API.

### <a name="step-1-import-dependencies"></a>Шаг 1. Импорт зависимостей

Создайте файл с именем `app.js` и вставьте в него следующий текст:

```JavaScript
const
      restify = require('restify')
    , restifyPlugins = require ('restify').plugins
    , passport = require('passport')
    , BearerStrategy = require('passport-azure-ad').BearerStrategy
    , config = require('./config')
    , authenticatedUserTokens = []
    , serverPort = process.env.PORT || config.serverPort
;
```

В этом разделе кода:

- `restify` и подключаемые модули указываются для настойки сервера Restify.
- Модули `passport` и `passport-azure-ad` отвечают за обмен данными с AAD.
- Переменная `config` инициализируется с использованием значений файла `config.js`, созданного на предыдущем шаге.
- Для `authenticatedUserTokens` создается массив, в котором сохраняются маркеры пользователей, когда они передаются в защищенные конечные точки.
- `serverPort` определяется на основе порта среды обработки или из файла конфигурации.

### <a name="step-2-instantiate-an-authentication-strategy"></a>Шаг 2. Создание экземпляра стратегии проверки подлинности

При защите конечной точки нужно предоставить стратегию для определения того, исходит ли текущий запрос от пользователя, прошедшего проверку подлинности. Переменная `authenticatonStrategy` является экземпляром класса `BearerStrategy` `passport-azure-ad`. Добавьте приведенный ниже код после инструкций `require`.

```JavaScript
const authenticationStrategy = new BearerStrategy(config.credentials, (token, done) => {
    let currentUser = null;

    let userToken = authenticatedUserTokens.find((user) => {
        currentUser = user;
        user.sub === token.sub;
    });

    if(!userToken) {
        authenticatedUserTokens.push(token);
    }

    return done(null, currentUser, token);
});
```

Эта реализация использует автоматическую регистрацию, добавляя маркеры проверки подлинности в массив `authenticatedUserTokens`, если они еще не созданы.

После создания экземпляра стратегии необходимо передать его в Passport через метод `use`. Добавьте следующий код в `app.js`, чтобы использовать стратегию в службе Passport.

```JavaScript
passport.use(authenticationStrategy);
```

### <a name="step-3-server-configuration"></a>Шаг 3. Конфигурация сервера

Определив стратегию проверки подлинности, можно настроить сервер Restify с основными параметрами и настроить использование Passport для обеспечения безопасности.

```JavaScript
const server = restify.createServer({ name: 'Azure Active Directory with Node.js Demo' });
server.use(restifyPlugins.authorizationParser());
server.use(passport.initialize());
server.use(passport.session());
```
Этот сервер инициализирован и настроен для анализа заголовков авторизации и настройки использования Passport.

### <a name="step-4-define-routes"></a>Шаг 4. Определение маршрутов

Теперь вы можете определить маршруты и решить, какие из них необходимо защитить с помощью AAD. Этот проект включает два маршрута, где корневой уровень является открытым, а для маршрута `/api` установлено требование проверки подлинности.

В `app.js` добавьте следующий код для маршрута корневого уровня:

```JavaScript
server.get('/', (req, res, next) => {
    res.send(200, 'Try: curl -isS -X GET http://127.0.0.1:3000/api');
    next();
});
```

Корневой маршрут разрешает все запросы и возвращает сообщение, содержащее команду для проверки маршрута `/api`. В отличие от этого маршрут `/api` заблокирован с помощью [`passport.authenticate`](http://passportjs.org/docs/authenticate). Добавьте следующий код после корневого маршрута.

```JavaScript
server.get('/api', passport.authenticate('oauth-bearer', { session: false }), (req, res, next) => {
    res.json({ message: 'response from API endpoint' });
    return next();
});
```

Эта конфигурация разрешает только авторизованные запросы, которые могут получить доступ к `/api` по маркеру носителя. С помощью параметра `session: false` можно отключить сеансы и требовать, чтобы маркер передавался с каждым запросом API.

Наконец, чтобы настроить сервер для прослушивания заданного порта, нужно вызвать метод `listen`.

```JavaScript
server.listen(serverPort);
```

## <a name="run-the-sample"></a>Запуск примера

Теперь, когда сервер реализован, его можно запустить, открыв командную строку и введя следующее:

```shell
npm start
```

При запущенном сервере можно отправить запрос на сервер для проверки результатов. Для демонстрации ответа из корневого маршрута откройте оболочку Bash и введите следующий код:

```shell
curl -isS -X GET http://127.0.0.1:3000/
```

Если ваш сервер настроен правильно, ответ будет выглядеть так:

```shell
HTTP/1.1 200 OK
Server: Azure Active Directory with Node.js Demo
Content-Type: application/json
Content-Length: 49
Date: Tue, 10 Oct 2017 18:35:13 GMT
Connection: keep-alive

Try: curl -isS -X GET http://127.0.0.1:3000/api
```

Затем можно проверить маршрут, который требует проверки подлинности, введя в оболочке Bash следующую команду:

```shell
curl -isS -X GET http://127.0.0.1:3000/api
```

Если сервер настроен правильно, то он должен отвечать с состоянием `Unauthorized`.

```shell
HTTP/1.1 401 Unauthorized
Server: Azure Active Directory with Node.js Demo
WWW-Authenticate: token is not found
Date: Tue, 10 Oct 2017 16:22:03 GMT
Connection: keep-alive
Content-Length: 12

Unauthorized
```

После создания безопасного API можно реализовать клиент, который может передавать маркеры проверки подлинности в API.

## <a name="next-steps"></a>Дополнительная информация

* Вам нужно реализовать аналог клиента для подключения к серверу, который обрабатывает операции входа, выхода и управления маркерами. См. примеры клиентских приложений [iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios) и [Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android).
* См. дополнительные сведения о [входе в веб-приложение Node.js и выходе из него с помощью Azure AD](quickstart-v1-openid-connect-code.md).
