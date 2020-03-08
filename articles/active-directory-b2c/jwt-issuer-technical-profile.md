---
title: Определение технического профиля для издателя JWT в настраиваемой политике
titleSuffix: Azure AD B2C
description: Определите технический профиль для издателя JSON Web Token (JWT) в пользовательской политике в Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/06/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c23648d70192607b2a5b977dcdd445931e995154
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78671809"
---
# <a name="define-a-technical-profile-for-a-jwt-token-issuer-in-an-azure-active-directory-b2c-custom-policy"></a>Определение технического профиля для издателя токенов JWT в пользовательской политике Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) выдает несколько типов маркеров безопасности при обработке каждого потока проверки подлинности. Технический профиль для издателя токенов JWT выдает токен JWT, который возвращается в приложение проверяющей стороны. Как правило, этот технический профиль является последним шагом оркестрации на пути взаимодействия пользователя.

## <a name="protocol"></a>Протокол

Атрибуту **Name** элемента **Protocol** необходимо присвоить значение `None`. Для элемента **OutputTokenFormat** задайте значение `JWT`.

В следующем примере показан технический профиль для `JwtIssuer`:

```XML
<TechnicalProfile Id="JwtIssuer">
  <DisplayName>JWT Issuer</DisplayName>
  <Protocol Name="None" />
  <OutputTokenFormat>JWT</OutputTokenFormat>
  ...
</TechnicalProfile>
```

## <a name="input-output-and-persist-claims"></a>Входные данные, выходные данные и сохранение утверждений

Элементы **InputClaims**, **OutputClaims** и **PersistClaims** являются пустыми или отсутствуют. Элементы **InutputClaimsTransformations** и **OutputClaimsTransformations** также отсутствуют.

## <a name="metadata"></a>Метаданные

| Атрибут | Обязательно | Описание |
| --------- | -------- | ----------- |
| issuer_refresh_token_user_identity_claim_type | Да | Утверждение, которое следует использовать в качестве удостоверения пользователя в кодах авторизации OAuth2 и токенах обновления. По умолчанию необходимо присвоить значение `objectId`, если не указан другой тип утверждения SubjectNamingInfo. |
| SendTokenResponseBodyWithJsonNumbers | Нет | Всегда имеет значение `true`. Для устаревшего формата, где числовые значения представлены в виде строк вместо чисел JSON, задается значение `false`. Этот атрибут необходим для клиентов, которые приняли зависимость в ранней реализации, в которой возвращены такие свойства, как строки. |
| token_lifetime_secs | Нет | Время существования маркеров доступа. Время существования токена носителя OAuth 2.0, используемого для получения доступа к защищенному ресурсу. Значение по умолчанию составляет 3600 секунд (1 час). Минимальное значение (включительно) — 300 секунд (5 минут). Максимальное значение (включительно) — 86 400 секунд (24 часа). |
| id_token_lifetime_secs | Нет | Время существования токена идентификатора. Значение по умолчанию составляет 3600 секунд (1 час). Минимальное значение (включительно) — 300 секунд (5 минут). Максимальное значение (включительно) — 86 400 секунд (24 часа). |
| refresh_token_lifetime_secs | Нет | Время существования токена обновления. Максимальный период времени, до наступления которого маркер обновления может использоваться для получения нового маркера доступа, если приложению была предоставлена область offline_access. По умолчанию это 1 209 600 секунд (14 дней). Минимальное значение (включительно) — 86 400 секунд (24 часа). Максимальное значение (включительно) — 7 776 000 секунд (90 дней). |
| rolling_refresh_token_lifetime_secs | Нет | Время жизни скользящего окна токена обновления. По истечении этого периода пользователю будет необходимо пройти проверку подлинности повторно вне зависимости от срока действия последнего токена обновления, полученного приложением. Если вы не хотите применять время существования скользящего окна, задайте для параметра allow_infinite_rolling_refresh_token значение `true`. По умолчанию это 7 776 000 секунд (90 дней). Минимальное значение (включительно) — 86 400 секунд (24 часа). Максимальное значение (включительно) — 31 536 000 секунд (365 дней). |
| allow_infinite_rolling_refresh_token | Нет | Если задать значение `true`, время существования скользящего окна токена обновления будет неограниченно. |
| IssuanceClaimPattern | Нет | Управляет утверждением издателя (iss). Одно из значений:<ul><li>Аусоритяндтенантгуид — утверждение ISS включает доменное имя, например `login.microsoftonline` или `tenant-name.b2clogin.com`, и идентификатор клиента HTTPS:\//login.microsoftonline.com/00000000-0000-0000-0000-000000000000/v2.0/.</li><li>AuthorityWithTfp — утверждение iss включает ваше доменное имя, например `login.microsoftonline` или `tenant-name.b2clogin.com`, ваш идентификатор клиента и имя политики проверяющей стороны. HTTPS:\//login.microsoftonline.com/tfp/00000000-0000-0000-0000-000000000000/b2c_1a_tp_sign-up-or-sign-in/v2.0/</li></ul> Значение по умолчанию: Аусоритяндтенантгуид |
| AuthenticationContextReferenceClaimPattern | Нет | Управляет значением утверждения `acr`.<ul><li>None — Azure AD B2C не выдает утверждение acr.</li><li>PolicyId — утверждение `acr` содержит имя политики.</li></ul>Параметры, использующиеся для настройки этого значения, — TFP (политика инфраструктуры доверия) и ACR (ссылка на контекст проверки подлинности). Мы рекомендуем для этого параметра задать значение TFP. Для задания значения должен существовать параметр `<Item>` с `Key="AuthenticationContextReferenceClaimPattern"` со значением `None`. В политику проверяющей стороны добавьте элемент `<OutputClaims>` и `<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />`. Кроме того, ваша политика должна содержать утверждение типа `<ClaimType Id="trustFrameworkPolicy">   <DisplayName>trustFrameworkPolicy</DisplayName>     <DataType>string</DataType> </ClaimType>`. |
|рефрештокенусержаурнэйид| Нет | Идентификатор пути взаимодействия пользователя, который должен быть выполнен во время [обновления маркера доступа](authorization-code-flow.md#4-refresh-the-token) POST к конечной точке `/token`. |

## <a name="cryptographic-keys"></a>Криптографические ключи

Элемент CryptographicKeys содержит следующие атрибуты:

| Атрибут | Обязательно | Описание |
| --------- | -------- | ----------- |
| issuer_secret | Да | Сертификат X509 (набор ключей RSA), используемый для подписывания токена JWT. Это ключ `B2C_1A_TokenSigningKeyContainer`, настроенный при работе со статьей [Azure Active Directory B2C: начало работы с настраиваемыми политиками](custom-policy-get-started.md). |
| issuer_refresh_token_key | Да | Сертификат X509 (набор ключей RSA), используемый для шифрования токена обновления. Вы настроили ключ `B2C_1A_TokenEncryptionKeyContainer` во время работы со статьей [Azure Active Directory B2C: начало работы с настраиваемыми политиками](custom-policy-get-started.md). |














