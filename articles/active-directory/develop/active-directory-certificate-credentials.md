---
title: Учетные данные сертификата в Azure AD | Документация Майкрософт
description: В этой статье рассматривается регистрация и использование учетных данных сертификата для аутентификации приложения.
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 88f0c64a-25f7-4974-aca2-2acadc9acbd8
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: ryanwi
ms.reviewer: nacanuma, jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 34a499ac12bcf6f4aee671eb209b709390ca6139
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545123"
---
# <a name="certificate-credentials-for-application-authentication"></a>Учетные данные сертификата для аутентификации приложения

Azure Active Directory (Azure AD) позволяет приложению использовать для аутентификации свои учетные данные, например, в потоке предоставления учетных данных клиента OAuth 2.0 ([версия 1.0](v1-oauth2-client-creds-grant-flow.md), [версия 2.0](v2-oauth2-client-creds-grant-flow.md)) и потоке On-Behalf-Of ([версия 1.0](v1-oauth2-on-behalf-of-flow.md), [версия 2.0](v2-oauth2-on-behalf-of-flow.md)).

Одной из форм учетных данных, которые приложение может использовать для аутентификации, является утверждение JSON Web Token (JWT), подписанное с помощью сертификата приложения.

## <a name="assertion-format"></a>Формат утверждения
Чтобы вычислить утверждение, можно использовать одну из множества библиотек [JSON Web Token](https://jwt.ms/) на удобном для вас языке. Маркер содержит следующие сведения:

### <a name="header"></a>Верхний колонтитул

| Параметр |  Комментарий |
| --- | --- |
| `alg` | Должен иметь значение **RS256** |
| `typ` | Должен иметь значение **JWT** |
| `x5t` | Это должен быть отпечаток SHA-1 сертификата X.509 |

### <a name="claims-payload"></a>Утверждения (полезные данные)

| Параметр |  Примечания |
| --- | --- |
| `aud` | Audience: Должно быть **https://login.microsoftonline.com/*tenant_Id*/oauth2/token** |
| `exp` | Срок действия: дата, когда истекает срок действия маркера. Время представлено как количество секунд с 1 января 1970 года (1970-01-01T0:0:0Z) в формате UTC до истечения срока действия маркера.|
| `iss` | Издатель: параметр должен иметь значение client_id (идентификатор приложения службы клиента) |
| `jti` | GUID: идентификатор JWT |
| `nbf` | Не ранее: дата, до которой маркер не может использоваться. Время представлено как количество секунд с 1 января 1970 года (1970-01-01T0:0:0Z) в формате UTC до времени выдачи маркера. |
| `sub` | Субъект: параметр должен иметь значение client_id (идентификатор приложения службы клиента, как и `iss`) |

### <a name="signature"></a>Подпись

Подпись формируется через применение сертификата, как описано в [документе RFC7519 о спецификации JSON Web Token](https://tools.ietf.org/html/rfc7519).

## <a name="example-of-a-decoded-jwt-assertion"></a>Пример декодированного утверждения JWT

```
{
  "alg": "RS256",
  "typ": "JWT",
  "x5t": "gx8tGysyjcRqKjFPnd7RFwvwZI0"
}
.
{
  "aud": "https: //login.microsoftonline.com/contoso.onmicrosoft.com/oauth2/token",
  "exp": 1484593341,
  "iss": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05",
  "jti": "22b3bb26-e046-42df-9c96-65dbd72c1c81",
  "nbf": 1484592741,  
  "sub": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05"
}
.
"Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"

```

## <a name="example-of-an-encoded-jwt-assertion"></a>Пример закодированного утверждения JWT

Ниже приводится строка, которая является примером закодированного утверждения. Если внимательно ее изучить, то можно заметить, что она состоит из трех разделов, разделенных точками (.).
* В первом разделе закодирован заголовок.
* Во втором разделе закодированы полезные данные.
* Последний раздел является подписью, вычисленной с помощью сертификатов из содержимого первых двух разделов.

```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

## <a name="register-your-certificate-with-azure-ad"></a>Регистрация сертификат в Azure AD

Учетные данные сертификата можно связать с клиентским приложением в Azure AD через портал Azure с помощью любого из следующих методов:

### <a name="uploading-the-certificate-file"></a>Передача файла сертификата

При регистрации приложения Azure для клиентского приложения сделайте следующее.
1. Выберите **Параметры > ключи**, а затем выберите **Отправить открытый ключ**. 
2. Выберите файл сертификата, который требуется передать.
3. Щелкните **Сохранить**. 
   
   После сохранения выполняется передача сертификата и отображаются значения отпечатка сертификата, даты начала и истечения срока действия. 

### <a name="updating-the-application-manifest"></a>Обновление манифеста приложения

Получив сертификат, необходимо вычислить следующие значения:

- `$base64Thumbprint` — хэш сертификата в кодировке base64;
- `$base64Value` — необработанные данные сертификата в кодировке base64.

Также необходимо предоставить идентификатор GUID для определения ключа в манифесте приложения (`$keyId`).

При регистрации приложения Azure для клиентского приложения сделайте следующее.
1. Откройте манифест приложения.
2. Замените свойство *keyCredentials* данными нового сертификата, используя приведенную ниже схему.

   ```
   "keyCredentials": [
       {
           "customKeyIdentifier": "$base64Thumbprint",
           "keyId": "$keyid",
           "type": "AsymmetricX509Cert",
           "usage": "Verify",
           "value":  "$base64Value"
       }
   ]
   ```
3. Сохраните изменения в манифесте приложения и передайте его в Azure AD. 

   Свойство `keyCredentials` является многозначным, поэтому для расширенного управления ключами можно передать несколько сертификатов.
   
## <a name="code-sample"></a>Пример кода

В примере кода в статье [Authenticating to Azure AD in daemon apps with certificates](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) (Аутентификация приложений управляющей программы в Azure AD с помощью сертификатов) показано, как приложение использует собственные учетные данные для аутентификации. В не также показано, как [создать самозаверяющий сертификат](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential#create-a-self-signed-certificate) с помощью команды PowerShell `New-SelfSignedCertificate`. Можно также воспользоваться преимуществами [сценариев создания приложений](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/AppCreationScripts/AppCreationScripts.md) для создания сертификатов, вычисления отпечатков и т. д.
