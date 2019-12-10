---
title: REST API обмен утверждениями в качестве проверки
titleSuffix: Azure AD B2C
description: Пошаговое руководство по созданию Azure AD B2C пути взаимодействия пользователя, взаимодействующего со службами RESTFUL.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/21/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 8730870bfae9f704ee43594497f79942b70a6181
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2019
ms.locfileid: "74949378"
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-on-user-input"></a>Пошаговое руководство. Интеграция обмена утверждениями REST API в путях взаимодействия пользователей Azure AD B2C как проверка входных данных

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Инфраструктура процедур идентификации, лежащая в основе Azure Active Directory B2C (Azure AD B2C), позволяет разработчикам служб удостоверений интегрировать взаимодействие с REST API в пути взаимодействия пользователя.

В конце этого пошагового руководства вы сможете создать путь взаимодействия пользователя Azure AD B2C, который взаимодействует со службами RESTful.

Инфраструктура процедур идентификации отправляет и получает данные в утверждениях. Взаимодействие с API:

- можно реализовать как обмен утверждениями REST API или профиль проверки, выполняемой на этапе оркестрации;
- обычно проверяет входные данные пользователя. Если полученное значение отклоняется, то пользователь может повторно ввести допустимое значение с возможностью возвращения сообщения об ошибке.

Взаимодействие можно также реализовать на этапе оркестрации. Дополнительные сведения см. в статье [Пошаговое руководство. Интеграция обмена утверждениями REST API в пути взаимодействия пользователя Azure AD B2C как этап оркестрации](active-directory-b2c-rest-api-step-custom.md).

В качестве примера профиля проверки мы будем использовать путь взаимодействия пользователя для изменения профиля в файле начального пакета ProfileEdit.xml.

Мы можем проверить, не входит ли имя, указанное пользователем в политике изменения профиля, в список исключений.

## <a name="prerequisites"></a>Технические условия

- Клиент Azure AD B2C, необходимый для регистрации или входа с использованием локальной учетной записи, как описано в статье [Azure Active Directory B2C. Приступая к работе с настраиваемыми политиками](active-directory-b2c-get-started-custom.md).
- Конечная точка REST API, с которой устанавливается взаимодействие. Для этого руководства мы настроили демонстрационный сайт [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/), используя службу REST API.

## <a name="step-1-prepare-the-rest-api-function"></a>Шаг 1. Подготовка функции REST API

> [!NOTE]
> В рамках этой статьи настройка функций REST API не рассматривается. [Функции Azure](https://docs.microsoft.com/azure/azure-functions/functions-reference) предоставляют отличный набор инструментов для создания служб RESTful в облаке.

Мы создали функцию Azure, которая получает утверждение, ожидаемое как `playerTag`. Функция проверяет, существует ли это утверждение. Полный код функции Azure доступен на сайте [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).

```csharp
if (requestContentAsJObject.playerTag == null)
{
  return request.CreateResponse(HttpStatusCode.BadRequest);
}

var playerTag = ((string) requestContentAsJObject.playerTag).ToLower();

if (playerTag == "mcvinny" || playerTag == "msgates123" || playerTag == "revcottonmarcus")
{
  return request.CreateResponse<ResponseContent>(
    HttpStatusCode.Conflict,
    new ResponseContent
    {
      version = "1.0.0",
      status = (int) HttpStatusCode.Conflict,
      userMessage = $"The player tag '{requestContentAsJObject.playerTag}' is already used."
    },
    new JsonMediaTypeFormatter(),
    "application/json");
}

return request.CreateResponse(HttpStatusCode.OK);
```

Инфраструктура процедур идентификации ожидает, что функция Azure вернет утверждение `userMessage`. Это утверждение будет представлено пользователю в виде строки, если при проверке произойдет сбой, например при возвращении кода состояния 409 (конфликт) в примере выше.

## <a name="step-2-configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworkextensionsxml-file"></a>Шаг 2. Настройка обмена утверждениями RESTful API как технического профиля в файле TrustFrameworkExtensions.xml

Технический профиль представляет собой полную конфигурацию обмена, заданную с помощью службы RESTful. Откройте файл TrustFrameworkExtensions.xml и добавьте следующий фрагмент XML-кода в элемент `<ClaimsProviders>`.

> [!NOTE]
> В следующем XML-коде поставщик RESTful `Version=1.0.0.0` описан как протокол. Рассматривайте его как функцию, которая будет взаимодействовать с внешней службой. <!-- TODO: A full definition of the schema can be found...link to RESTful Provider schema definition>-->

```xml
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-CheckPlayerTagWebHook">
            <DisplayName>Check Player Tag Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/CheckPlayerTagWebHook?code=L/05YRSpojU0nECzM4Tp3LjBiA2ZGh3kTwwp1OVV7m0SelnvlRVLCg==</Item>
                <Item Key="SendClaimsIn">Body</Item>
                <!-- Set AuthenticationType to Basic or ClientCertificate in production environments -->
                <Item Key="AuthenticationType">None</Item>
                <!-- REMOVE the following line in production environments -->
                <Item Key="AllowInsecureAuthInProduction">true</Item>
            </Metadata>
            <InputClaims>
                <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="playerTag" />
            </InputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
        <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
            <ValidationTechnicalProfiles>
                <ValidationTechnicalProfile ReferenceId="AzureFunctions-CheckPlayerTagWebHook" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

Элемент `InputClaims` определяет утверждения, которые отправляются из инфраструктуры процедур идентификации в службу REST. В этом примере содержимое утверждения `givenName` отправляется в службу REST как `playerTag`. В этом примере инфраструктура процедур идентификации не ожидает возврата утверждений. Вместо этого она ожидает ответ от службы REST и действует на основе полученного кода состояния.

В комментариях выше `AuthenticationType` и `AllowInsecureAuthInProduction` указать изменения, которые следует внести при переходе в рабочую среду. Чтобы узнать, как защитить интерфейсы API RESTFUL для рабочей среды, см. статью Защита API RESTful [с помощью базовой проверки подлинности](active-directory-b2c-custom-rest-api-netfw-secure-basic.md) и [защищенных API-интерфейсов RESTful с проверкой подлинности сертификата](active-directory-b2c-custom-rest-api-netfw-secure-cert.md).

## <a name="step-3-include-the-restful-service-claims-exchange-in-self-asserted-technical-profile-where-you-want-to-validate-the-user-input"></a>Шаг 3. Включение обмена утверждениями службы REST в самоподтвержденном техническом профиле, в котором необходимо реализовать проверку входных данных пользователя

Чаще всего этап проверки используется при взаимодействии с пользователем. Все взаимодействия, где пользователь должен предоставить входные данные, выполняются с использованием *самоподтвержденных технических профилей*. В этом примере мы добавим этап проверки в технический профиль Self-Asserted-ProfileUpdate. Это технический профиль, используемый файлом политики проверяющей стороны `Profile Edit`.

Чтобы добавить обмен утверждениями в самоподтвержденный технический профиль, выполните следующие действия:

1. Откройте файл TrustFrameworkBase.xml и найдите `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.
2. Проверьте конфигурацию этого технического профиля. Просмотрите, как определен обмен утверждениями, запрашивающими данные пользователя (входящие утверждения), и утверждениями, возвращаемыми самоподтвержденным поставщиком (исходящие утверждения).
3. Найдите `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`. Обратите внимание, что этот профиль вызывается как пятый этап оркестрации `<UserJourney Id="ProfileEdit">`.

## <a name="step-4-upload-and-test-the-profile-edit-rp-policy-file"></a>Шаг 4. Отправка и тестирование файла политики изменения профиля проверяющей стороны

1. Отправьте новую версию файла TrustFrameworkExtensions.xml.
2. Проверьте файл политики изменения профиля проверяющей стороны с помощью кнопки **Запустить**.
3. Выполните проверку, указав одно из имеющихся имен (например, mcvinny) в поле **Заданное имя**. Если все настроено правильно, то должно отобразиться сообщение, указывающее, что playerTag уже используется.

## <a name="next-steps"></a>Дальнейшие действия

[Azure Active Directory B2C. Создание и использование настраиваемых атрибутов в пользовательской политике изменения профиля](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)

[Пошаговое руководство. Интеграция обмена утверждениями REST API в пути взаимодействия пользователя Azure AD B2C как этап оркестрации](active-directory-b2c-rest-api-step-custom.md)

[Справочник: технический профиль RESTFUL](restful-technical-profile.md)

Дополнительные сведения о защите API см. в следующих статьях:

* [Azure Active Directory B2C: Secure your RESTful services using HTTP basic authentication](active-directory-b2c-custom-rest-api-netfw-secure-basic.md) (Azure Active Directory B2C: защита служб RESTful с использованием обычной проверки подлинности HTTP)
* [Azure Active Directory B2C. Защита служб RESTful с помощью сертификатов клиента](active-directory-b2c-custom-rest-api-netfw-secure-cert.md)
