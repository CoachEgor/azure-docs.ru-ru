---
title: Всегда оставаться в системе Azure Active Directory B2C
description: Настройка функции "Оставаться в системе" в Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 052e1bc4e9a14b34e21b0bfeb4193fbd0b2b1a22
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76848906"
---
# <a name="enable-keep-me-signed-in-kmsi-in-azure-active-directory-b2c"></a>Включение функции "Оставаться в системе" в Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Вы можете включить функцию "оставаться в системе" (функции "оставаться) для пользователей вашего веб-приложения и собственных приложений с локальными учетными записями в каталоге Azure Active Directory B2C (Azure AD B2C). Эта функция предоставляет пользователям доступ к приложению без запроса на повторное ввод имени пользователя и пароля. Доступ отменяется, когда пользователь выходит из системы.

Пользователям не следует включать этот параметр на общедоступных компьютерах.

![Пример страницы входа, на которой отображается флажок "оставаться в системе"](./media/custom-policy-keep-me-signed-in/kmsi.PNG)

## <a name="prerequisites"></a>Технические условия

Клиент Azure AD B2C, настроенный для разрешения входа в локальную учетную запись. ФУНКЦИИ "оставаться не поддерживается для учетных записей внешних поставщиков удостоверений.

Если у вас нет клиента, вы можете создать его с помощью процедуры, описанной в статье [Руководство по созданию клиента Azure Active Directory B2C](tutorial-create-tenant.md).

## <a name="add-a-content-definition-element"></a>Добавление элемента определения содержимого

В элементе **BuildingBlocks** файла расширения добавьте элемент **ContentDefinitions**.

1. В элементе **ContentDefinitions** добавьте элемент **ContentDefinition** с идентификатором `api.signuporsigninwithkmsi`.
2. В элементе **ContentDefinition** добавьте элементы **LoadUri**, **RecoveryUri** и **DataUri**. Значение `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` элемента **DataUri** — это машиночитаемый идентификатор, который используется для отображения флажка "Оставаться в системе" на страницах входа. Это значение не следует изменять.

    ```XML
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsigninwithkmsi">
          <LoadUri>~/tenant/default/unified.cshtml</LoadUri>
          <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0</DataUri>
          <Metadata>
            <Item Key="DisplayName">Signin and Signup</Item>
          </Metadata>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

## <a name="add-a-sign-in-claims-provider-for-a-local-account"></a>Добавление поставщика утверждений входа для локальной учетной записи

Определить поставщик утверждений для входа с локальной учетной записью можно с помощью элемента **ClaimsProvider** в файле расширения политики:

1. Откройте файл *TrustFrameworkExtensions.xml* из рабочего каталога.
2. Найдите элемент **ClaimsProviders**. Если он не существует, добавьте его в корневой элемент. [Начальный пакет](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) включает в себя поставщик утверждений для входа с использованием локальной учетной записи.
3. Добавьте элемент **ClaimsProvider** со значениями **DisplayName** и **TechnicalProfile**, как показано в следующем примере:

    ```XML
    <ClaimsProviders>
      <ClaimsProvider>
        <DisplayName>Local Account SignIn</DisplayName>
        <TechnicalProfiles>
          <TechnicalProfile Id="login-NonInteractive">
            <Metadata>
              <Item Key="client_id">ProxyIdentityExperienceFrameworkAppId</Item>
              <Item Key="IdTokenAudience">IdentityExperienceFrameworkAppId</Item>
            </Metadata>
            <InputClaims>
              <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
              <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
            </InputClaims>
          </TechnicalProfile>
        </TechnicalProfiles>
      </ClaimsProvider>
    </ClaimsProviders>
    ```

### <a name="add-the-application-identifiers-to-your-custom-policy"></a>Добавление идентификаторов приложений в настраиваемую политику

Добавьте идентификаторы приложений для файла *TrustFrameworkExtensions.xml*.

1. В файле *TrustFrameworkExtensions.xml* найдите элемент **TechnicalProfile** с идентификатором `login-NonInteractive` и элемент **TechnicalProfile** с идентификатором `login-NonInteractive-PasswordChange` и замените все значения `IdentityExperienceFrameworkAppId` на идентификатор приложения Identity Experience Framework, как описано в [руководстве по началу работы](custom-policy-get-started.md).

    ```XML
    <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
    ```

2. Замените все значения `ProxyIdentityExperienceFrameworkAppId` идентификатором прокси приложения Identity Experience Framework, как описано в [руководстве по началу работы](custom-policy-get-started.md).
3. Сохраните файл расширений.

## <a name="create-a-kmsi-enabled-user-journey"></a>Создание функции "Оставаться в системе" во включенном пути взаимодействия пользователя

Добавьте поставщик утверждений входа с локальной учетной записью для пути взаимодействия пользователя.

1. Откройте базовый файл политики. Например, *TrustFrameworkBase.xml*.
2. Найдите элемент **UserJourneys** и скопируйте все содержимое элемента **UserJourney**, который использует идентификатор `SignUpOrSignIn`.
3. Откройте файл расширения, например *TrustFrameworkExtensions.xml*, и найдите элемент **UserJourneys**. Если элемент не существует, добавьте его.
4. Вставьте все содержимое элемента **UserJourney**, который вы скопировали в качестве дочернего элемента объекта **UserJourneys**.
5. Измените значение идентификатора для нового пути взаимодействия пользователя. Например, `SignUpOrSignInWithKmsi`.
6. Наконец, на первом шаге оркестрации измените значение **ContentDefinitionReferenceId** на `api.signuporsigninwithkmsi`. Это необходимо, чтобы включить флажок в пути взаимодействия пользователя.
7. Сохраните и отправьте этот файл расширения, а также убедитесь, что все проверки завершены успешно.

    ```XML
    <UserJourneys>
      <UserJourney Id="SignUpOrSignInWithKmsi">
        <OrchestrationSteps>
          <OrchestrationStep Order="1" Type="CombinedSignInAndSignUp" ContentDefinitionReferenceId="api.signuporsigninwithkmsi">
            <ClaimsProviderSelections>
              <ClaimsProviderSelection ValidationClaimsExchangeId="LocalAccountSigninEmailExchange" />
            </ClaimsProviderSelections>
            <ClaimsExchanges>
              <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
            </ClaimsExchanges>
          </OrchestrationStep>
          <OrchestrationStep Order="2" Type="ClaimsExchange">
            <Preconditions>
              <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
                <Value>objectId</Value>
                <Action>SkipThisOrchestrationStep</Action>
              </Precondition>
            </Preconditions>
            <ClaimsExchanges>
              <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccountSignUpWithLogonEmail" />
            </ClaimsExchanges>
          </OrchestrationStep>
          <!-- This step reads any user attributes that we may not have received when in the token. -->
          <OrchestrationStep Order="3" Type="ClaimsExchange">
            <ClaimsExchanges>
              <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
            </ClaimsExchanges>
          </OrchestrationStep>
          <OrchestrationStep Order="4" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
        </OrchestrationSteps>
        <ClientDefinition ReferenceId="DefaultWeb" />
      </UserJourney>
    </UserJourneys>
    ```

## <a name="create-a-relying-party-file"></a>Создание файла проверяющей стороны

Обновите файл проверяющей стороны, который активирует созданный путь взаимодействия пользователя.

1. Создайте копию файла *SignUpOrSignIn.xml* в рабочем каталоге, а затем переименуйте его. Например, *SignUpOrSignInWithKmsi.xml*.
2. Откройте новый файл и обновите атрибут **PolicyId** для **TrustFrameworkPolicy**, указав уникальное значение. Это имя вашей политики. Например, `SignUpOrSignInWithKmsi`.
3. Измените атрибут **ReferenceId** для элемента **DefaultUserJourney**, чтобы он соответствовал идентификатору созданного вами нового пути взаимодействия пользователя. Например, `SignUpOrSignInWithKmsi`.

    Сервер управления ключами настраивается с помощью элемента **UserJourneyBehaviors** с **SingleSignOn**, **SessionExpiryType** и **SessionExpiryInSeconds** в качестве первых дочерних элементов. Атрибут **KeepAliveInDays** определяет, как долго пользователь будет оставаться в системе. В следующем примере сеанс функции "Оставаться в системе" автоматически заканчивается через `7` дней, независимо от того, как часто пользователь выполняет автоматическую проверку подлинности. Если установить для параметра **KeepAliveInDays** значение `0`, это приведет к отключению функции "Оставаться в системе". По умолчанию это значение равно `0`. Если для **SessionExpiryType** задано значение `Rolling`, то сеанс функции "Оставаться в системе" продлевается на `7` дней каждый раз, когда пользователь выполняет автоматическую проверку подлинности.  Если выбран параметр `Rolling`, следует установить минимальное число дней.

    Значение **SessionExpiryInSeconds** представляет время окончания срока действия сеанса единого входа. Этот параметр используется внутри Azure AD B2C, чтобы проверить, истек ли срок действия сеанса функции "Оставаться в системе". Параметр **KeepAliveInDays** определяет значение максимального срока действия файла cookie сеанса единого входа в веб-браузере. В отличие от параметра **SessionExpiryInSeconds** параметр **KeepAliveInDays** используется, чтобы браузер не очищал файл cookie при завершении работы. Пользователь может автоматически войти только в том случае, если существует действительный файл cookie сеанса единого входа, который управляется параметром **KeepAliveInDays**, и срок его действия не истек, что управляется параметром **SessionExpiryInSeconds**.

    Если на странице регистрации и входа в систему пользователь не включает параметр **Оставаться в системе**, время сеанса истекает после достижения времени, заданного параметром **SessionExpiryInSeconds**, или закрытии браузера. Если пользователь включает параметр **Оставаться в системе**, значение **KeepAliveInDays** переопределяет значение **SessionExpiryInSeconds** и определяет время окончания действия сеанса. Даже если пользователи закрывают браузер и открывают его снова, они могут по-прежнему автоматически входить, пока не превышено значение **KeepAliveInDays**. Рекомендуется установить значение **SessionExpiryInSeconds** на короткий период (1200 секунд), в то время как значение **KeepAliveInDays** установлено на относительно длительный период (7 дней), как показано в следующем примере.

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="SignUpOrSignInWithKmsi" />
      <UserJourneyBehaviors>
        <SingleSignOn Scope="Tenant" KeepAliveInDays="7" />
        <SessionExpiryType>Absolute</SessionExpiryType>
        <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
      </UserJourneyBehaviors>
      <TechnicalProfile Id="PolicyProfile">
        <DisplayName>PolicyProfile</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="displayName" />
          <OutputClaim ClaimTypeReferenceId="givenName" />
          <OutputClaim ClaimTypeReferenceId="surname" />
          <OutputClaim ClaimTypeReferenceId="email" />
          <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        </OutputClaims>
        <SubjectNamingInfo ClaimType="sub" />
      </TechnicalProfile>
    </RelyingParty>
    ```

4. Сохраните изменения и отправьте файл.
5. Чтобы протестировать отправленную настраиваемую политику, на портале Azure перейдите к странице политики и нажмите кнопку **Выполнить**.

Пример политики можно найти [здесь](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in).
