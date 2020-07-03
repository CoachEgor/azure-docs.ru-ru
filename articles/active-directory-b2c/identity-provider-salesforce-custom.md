---
title: Настройка входа с помощью поставщика SAML SalesForce с использованием настраиваемых политик
titleSuffix: Azure AD B2C
description: Настройте вход с помощью поставщика SAML Salesforce, используя пользовательские политики Azure в Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/27/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 183fe1604cc363a9121d5eef3737751c54e9bdf1
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82229722"
---
# <a name="set-up-sign-in-with-a-salesforce-saml-provider-by-using-custom-policies-in-azure-active-directory-b2c"></a>Настройка входа с помощью поставщика SAML Salesforce, используя пользовательские политики Azure в Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

В этой статье показано, как включить вход для пользователей из Организации SalesForce с помощью [пользовательских политик](custom-policy-overview.md) в Azure Active Directory B2C (Azure AD B2C). Чтобы включить вход, добавьте [технический профиль поставщика удостоверений SAML](saml-identity-provider-technical-profile.md) в настраиваемую политику.

## <a name="prerequisites"></a>Предварительные условия

- Выполните шаги, описанные в статье [Начало работы с настраиваемыми политиками в Azure Active Directory B2C](custom-policy-get-started.md).
- Если это еще не сделано, зарегистрируйтесь для получения [бесплатной учетной записи Developer Edition](https://developer.salesforce.com/signup). В этой статье предполагается использование [Salesforce Lightning Experience](https://developer.salesforce.com/page/Lightning_Experience_FAQ).
- [Настройте мой домен](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) для своей организации Salesforce.

### <a name="set-up-salesforce-as-an-identity-provider"></a>Настройка Salesforce в качестве поставщика удостоверений

1. [Войдите в Salesforce](https://login.salesforce.com/).
2. В меню слева в разделе **Параметры** разверните узел **Удостоверение** и выберите **Поставщик удостоверений**.
3. Выберите **Enable Identity Provider** (Включить поставщик удостоверений).
4. В разделе **Select the certificate** (Выберите сертификат) выберите сертификат, который необходимо использовать в Salesforce при взаимодействии с Azure AD B2C. Вы можете использовать сертификат по умолчанию.
5. Нажмите кнопку **Сохранить**.

### <a name="create-a-connected-app-in-salesforce"></a>Создание подключенного приложения в Salesforce

1. На странице **поставщик удостоверений** выберите **поставщики услуг, которые теперь созданы с помощью подключенных приложений. Щелкните здесь.**
2. В разделе **Основные сведения** введите нужные значения для подключенного приложения.
3. В разделе **Параметры веб-приложения** установите флажок **Включить SAML**.
4. В поле **Идентификатор сущности** введите следующий URL-адрес. Замените значение `your-tenant` на имя клиента Azure AD B2C.

      ```
      https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase
      ```

6. В поле **ACS URL** (URL-адрес ACS) введите приведенный ниже URL-адрес. Замените значение `your-tenant` на имя клиента Azure AD B2C.

      ```
      https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
      ```
7. Прокрутите до нижней части списка и нажмите кнопку **Сохранить**.

### <a name="get-the-metadata-url"></a>Получить URL-адреса метаданных

1. На странице общих сведений о подключенном приложении щелкните **Управление**.
2. Скопируйте значение **конечной точки обнаружения метаданных**, а затем сохраните его. Оно будет использоваться далее в этой статье.

### <a name="set-up-salesforce-users-to-federate"></a>Настройка пользователей Salesforce для федерации

1. На странице **Управление** подключенного приложения щелкните **Управление профилями**.
2. Выберите профили (или группы пользователей) для федерации с Azure AD B2C. Используя права системного администратора, установите флажок **Системный администратор** для федерации с помощью учетной записи Salesforce.

## <a name="generate-a-signing-certificate"></a>Создание сертификата для подписи

Запросы, отправляемые в Salesforce, должны быть подписаны Azure AD B2C. Чтобы создать сертификат подписи, откройте Azure PowerShell и выполните следующие команды.

> [!NOTE]
> Обновите имя клиента и пароль в первых двух строках.

```powershell
$tenantName = "<YOUR TENANT NAME>.onmicrosoft.com"
$pwdText = "<YOUR PASSWORD HERE>"

$Cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "SamlIdp.$tenantName" -Subject "B2C SAML Signing Cert" -HashAlgorithm SHA256 -KeySpec Signature -KeyLength 2048

$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText

Export-PfxCertificate -Cert $Cert -FilePath .\B2CSigningCert.pfx -Password $pwd
```

## <a name="create-a-policy-key"></a>Создание ключа политики

Вам необходимо сохранить сертификат, который вы создали в клиенте Azure AD B2C.

1. Войдите на [портал Azure](https://portal.azure.com/).
2. Убедитесь, что используете каталог с клиентом Azure AD B2C, выбрав фильтр **Каталог и подписка** в меню вверху и каталог с вашим клиентом.
3. Выберите **Все службы** в левом верхнем углу окна портала Azure, а затем найдите и выберите **Azure AD B2C**.
4. На странице "Обзор" выберите **Identity Experience Framework**.
5. Выберите **Ключи политики**, а затем щелкните **Добавить**.
6. Для пункта **Параметры** выберите `Upload`.
7. Введите **имя** политики. Например, SAMLSigningCert. Префикс `B2C_1A_` будет автоматически добавлен к имени ключа.
8. Найдите созданный сертификат B2CSigningCert.pfx и выберите его.
9. Введите **пароль** для сертификата.
3. Нажмите кнопку **Создать**.

## <a name="add-a-claims-provider"></a>Добавление поставщика утверждений

Если необходимо разрешить пользователям входить в систему с помощью учетной записи Salesforce, нужно определить учетную запись в качестве поставщика утверждений, с которым Azure AD B2C может взаимодействовать через конечную точку. Конечная точка предоставляет набор утверждений, используемых Azure AD B2C, чтобы проверить, была ли выполнена проверка подлинности определенного пользователя.

Чтобы определить учетную запись Salesforce в качестве поставщика утверждений, добавьте ее в элемент **ClaimsProviders** в файле расширения политики. Дополнительные сведения см. [в разделе Определение технического профиля поставщика удостоверений SAML](saml-identity-provider-technical-profile.md).

1. Откройте файл *TrustFrameworkExtensions.xml*.
1. Найдите элемент **ClaimsProviders**. Если он не существует, добавьте его в корневой элемент.
1. Добавьте новый элемент **ClaimsProvider** следующим образом.

    ```XML
    <ClaimsProvider>
      <Domain>salesforce</Domain>
      <DisplayName>Salesforce</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="salesforce">
          <DisplayName>Salesforce</DisplayName>
          <Description>Login with your Salesforce account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="WantsSignedAssertions">false</Item>
            <Item Key="PartnerEntity">https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp.xml</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="userId"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="username"/>
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="SAMLIdp" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Saml-idp"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Измените значение **PartnerEntity** на скопированный ранее URL-адрес метаданных Salesforce.
1. Измените значение обоих экземпляров **StorageReferenceId** на имя ключа сертификата для подписи. Например, B2C_1A_SAMLSigningCert.
1. Откройте `<ClaimsProviders>` раздел и добавьте следующий фрагмент XML-кода. Если ваша политика уже содержит `SM-Saml-idp` технический профиль, перейдите к следующему шагу. Дополнительные сведения см. [в разделе Управление сеансами единого входа](custom-policy-reference-sso.md).

    ```XML
    <ClaimsProvider>
      <DisplayName>Session Management</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="SM-Saml-idp">
          <DisplayName>Session Management Provider</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="IncludeSessionIndex">false</Item>
            <Item Key="RegisterServiceProviders">false</Item>
          </Metadata>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```
1. Сохраните файл.

### <a name="upload-the-extension-file-for-verification"></a>Отправка файла расширения для проверки

К этому моменту политика настроена, так что Azure AD B2C знает, как взаимодействовать с учетной записью Salesforce. Попробуйте отправить файл расширения политики, чтобы убедиться, что все в порядке.

1. На странице **Пользовательские политики** в клиенте Azure AD B2C выберите **Отправить политику**.
2. Включите функцию **Перезаписать политику, если она уже существует**, а затем найдите и выберите файл *TrustFrameworkExtensions.xml*.
3. Щелкните **Отправить**.

## <a name="register-the-claims-provider"></a>Регистрация поставщика утверждений

На этом этапе поставщик удостоверений уже настроен, но еще недоступен ни на одном экране регистрации или входа. Чтобы сделать его доступным, необходимо создать дубликат существующего шаблона пути взаимодействия пользователя, а затем изменить его таким образом, чтобы он также содержал поставщик удостоверений Salesforce.

1. Откройте файл *TrustFrameworkBase.xml* из начального пакета.
2. Найдите и скопируйте все содержимое элемента **UserJourney**, в котором присутствует запись `Id="SignUpOrSignIn"`.
3. Откройте файл *TrustFrameworkExtensions.xml* и найдите элемент **UserJourneys**. Если элемент не существует, добавьте его.
4. Вставьте все скопированное содержимое элемента **UserJourney** в качестве дочернего элемента в элемент **UserJourneys**.
5. Переименуйте идентификатор пути взаимодействия пользователя. Например, `SignUpSignInSalesforce`.

### <a name="display-the-button"></a>Отображение кнопки

Элемент **ClaimsProviderSelection** является аналогом кнопки поставщика удостоверений на экране регистрации или входа. Если вы добавите для учетной записи LinkedIn элемент **ClaimsProviderSelection**, при переходе пользователя на страницу отобразится новая кнопка.

1. Найдите элемент **OrchestrationStep**, содержащий `Order="1"` в только что созданном пути взаимодействия пользователя.
2. Добавьте следующий элемент в тэг **ClaimsProviderSelects**. Установите для параметра **TargetClaimsExchangeId** соответствующее значение, например `SalesforceExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="SalesforceExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Связывание кнопки с действием

Теперь, когда у вас есть кнопка, вам необходимо связать ее с действием. В этом случае действие — это возможность взаимодействия Azure AD B2C с учетной записью Salesforce для получения токена.

1. Найдите элемент **OrchestrationStep**, содержащий `Order="2"` в пути пользователя.
2. Добавьте следующий элемент **ClaimsExchange** , убедившись, что используется то же значение **идентификатора** , которое использовалось для **таржетклаимсексчанжеид**:

    ```XML
    <ClaimsExchange Id="SalesforceExchange" TechnicalProfileReferenceId="salesforce" />
    ```

    Измените значение **течникалпрофилереференцеид** на идентификатор созданного ранее **идентификатора** технического профиля. Например, `LinkedIn-OAUTH`.

3. Сохраните файл *TrustFrameworkExtensions.xml* и повторно отправьте его для проверки.

## <a name="create-an-azure-ad-b2c-application"></a>Создание приложения Azure AD B2C

Связь с Azure AD B2C происходит через приложение, регистрируемое в клиенте B2C. В этом разделе перечислены необязательные действия, которые можно выполнить, чтобы создать тестовое приложение, если вы его еще не создали.

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

## <a name="update-and-test-the-relying-party-file"></a>Обновление и тестирование файла проверяющей стороны

Обновите файл проверяющей стороны, который активирует только что созданный путь взаимодействия пользователя:

1. Создайте копию *SignUpOrSignIn.xml* в рабочем каталоге и переименуйте ее. Например, переименуйте ее в *SignUpSignInSalesforce.xml*.
2. Откройте новый файл и обновите значение атрибута **PolicyId** для **TrustFrameworkPolicy**, указав уникальное значение. Например, `SignUpSignInSalesforce`.
3. Обновите значение **PublicPolicyUri**, указав URI для политики. Например: `http://contoso.com/B2C_1A_signup_signin_salesforce`.
4. Обновите значение атрибута **ReferenceId** для элемента **DefaultUserJourney**, чтобы он соответствовал идентификатору созданного вами нового пути взаимодействия пользователя (SignUpSignInSalesforce).
5. Сохраните изменения, отправьте файл, а затем выберите в списке новую политику.
6. Убедитесь, что созданное приложение Azure AD B2C выбрано в поле **Выберите приложение**, а затем протестируйте его, щелкнув **Запустить сейчас**.
