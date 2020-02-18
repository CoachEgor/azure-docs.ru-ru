---
title: Настройка входа с использованием учетной записи Azure AD с помощью настраиваемых политик
titleSuffix: Azure AD B2C
description: Настройка входа в службу Azure Active Directory B2C с помощью учетной записи Azure Active Directory с использованием пользовательских политик.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 64934dd5bc591415c0bad6ac3dc6a4a2d98dd005
ms.sourcegitcommit: b95983c3735233d2163ef2a81d19a67376bfaf15
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2020
ms.locfileid: "77136311"
---
# <a name="set-up-sign-in-with-an-azure-active-directory-account-using-custom-policies-in-azure-active-directory-b2c"></a>Настройка входа в Azure Active Directory B2C с помощью учетной записи Azure Active Directory с использованием пользовательских политик

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

В этой статье показано, как включить вход для пользователей из Организации Azure Active Directory (Azure AD) с помощью [настраиваемых политик](custom-policy-overview.md) в Azure Active Directory B2C (Azure AD B2C).

## <a name="prerequisites"></a>предварительные требования

Выполните шаги, описанные в статье [Начало работы с настраиваемыми политиками в Azure Active Directory B2C](custom-policy-get-started.md).

## <a name="register-an-application"></a>Регистрация приложения

Чтобы включить вход для пользователей из определенной организации Azure AD, вам необходимо зарегистрировать приложение в клиенте организации Azure AD.

1. Войдите на [портал Azure](https://portal.azure.com).
1. Убедитесь, что вы используете каталог, содержащий клиент Azure AD Организации (например, contoso.com). Выберите **Фильтр каталог и подписка** в верхнем меню, а затем выберите каталог, содержащий ваш клиент Azure AD.
1. Выберите **Все службы** в левом верхнем углу окна портала Azure, а затем найдите и выберите **Регистрация приложений**.
1. Выберите **Новая регистрация**.
1. Введите значение **Name** (Имя) для приложения. Например, `Azure AD B2C App`.
1. Примите выбранные по умолчанию **учетные записи в этом каталоге организации только** для этого приложения.
1. В качестве **URI перенаправления**оставьте значение **Web**и введите следующий URL-адрес в строчных буквах, где `your-B2C-tenant-name` заменяется именем клиента Azure AD B2C.

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Например, `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.

1. Выберите **Зарегистрировать**. Запишите значение параметра **Идентификатор приложения (клиент)** . Оно вам потребуется в дальнейшем.
1. Выберите **сертификаты & секреты**, а затем выберите **новый секрет клиента**.
1. Введите **Описание** секрета, выберите срок действия, а затем нажмите кнопку **Добавить**. Запишите **значение** секрета для использования на следующем шаге.

## <a name="configuring-optional-claims"></a>Настройка необязательных утверждений

Если вы хотите получить `family_name` и `given_name` утверждения из Azure AD, можно настроить дополнительные утверждения для приложения в пользовательском интерфейсе портал Azure или манифесте приложения. Дополнительные сведения см. в статье [предоставление дополнительных утверждений для приложения Azure AD](../active-directory/develop/active-directory-optional-claims.md).

1. Войдите на [портал Azure](https://portal.azure.com). Найдите и выберите **Azure Active Directory**.
1. В разделе **Управление** выберите **Регистрация приложений**.
1. В списке выберите приложение, для которого нужно настроить необязательные утверждения.
1. В разделе **Управление** выберите **Конфигурация токена (Предварительная версия)** .
1. Выберите **добавить необязательное утверждение**.
1. Выберите тип токена, который требуется настроить.
1. Выберите необязательные утверждения для добавления.
1. Нажмите кнопку **Добавить**.

## <a name="create-a-policy-key"></a>Создание ключа политики

Вам необходимо сохранить ключ приложения, который вы создали в клиенте Azure AD B2C.

1. Убедитесь, что вы используете каталог, содержащий клиент Azure AD B2C. Выберите **Фильтр каталог и подписка** в верхнем меню, а затем выберите каталог, содержащий клиент Azure AD B2C.
1. Выберите **Все службы** в левом верхнем углу окна портала Azure, а затем найдите и выберите **Azure AD B2C**.
1. В разделе **политики**выберите **инфраструктура процедур идентификации**.
1. Выберите **ключи политики** и нажмите кнопку **Добавить**.
1. Для пункта **Параметры** выберите `Manual`.
1. Введите **имя** ключа политики. Например, `ContosoAppSecret`.  Префикс `B2C_1A_` автоматически добавляется к имени ключа при его создании, поэтому ссылка в XML-файле в следующем разделе будет *B2C_1A_ContosoAppSecret*.
1. В качестве **секрета**введите секрет клиента, записанный ранее.
1. Для параметра **Использование ключа** выберите `Signature`.
1. Нажмите кнопку **Создать**.

## <a name="add-a-claims-provider"></a>Добавление поставщика утверждений

Если необходимо разрешить пользователям входить в систему с помощью Azure Active Directory, нужно определить Azure Active Directory в качестве поставщика утверждений, с которым Azure AD B2C может взаимодействовать через конечную точку. Конечная точка предоставляет набор утверждений, используемых Azure AD B2C, чтобы проверить, была ли выполнена проверка подлинности определенного пользователя.

Вы можете определить Azure AD в качестве поставщика утверждений, добавив Azure AD к элементу **ClaimsProvider** в файле расширения политики.

1. Откройте файл *TrustFrameworkExtensions.xml*.
2. Найдите элемент **ClaimsProviders**. Если он не существует, добавьте его в корневой элемент.
3. Добавьте новый элемент **ClaimsProvider** следующим образом.
    ```xml
    <ClaimsProvider>
      <Domain>Contoso</Domain>
      <DisplayName>Login using Contoso</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="OIDC-Contoso">
          <DisplayName>Contoso Employee</DisplayName>
          <Description>Login with your Contoso account</Description>
          <Protocol Name="OpenIdConnect"/>
          <Metadata>
            <Item Key="METADATA">https://login.microsoftonline.com/tenant-name.onmicrosoft.com/v2.0/.well-known/openid-configuration</Item>
            <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
            <Item Key="response_types">code</Item>
            <Item Key="scope">openid profile</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">false</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_ContosoAppSecret"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="oid"/>
            <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" AlwaysUseDefaultValue="true" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="iss" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. В элементе **ClaimsProvider** укажите для **Domain** уникальное значение, позволяющее отличить этот поставщик удостоверений от других. Например, `Contoso`. Не помещайте `.com` в конец этого параметра домена.
5. В элементе **ClaimsProvider** обновите значение **DisplayName**, указав понятное имя поставщика утверждений. В настоящее время это значение не используется.

### <a name="update-the-technical-profile"></a>Обновление технического профиля

Чтобы получить токен из конечной точки Azure AD, вам необходимо определить протоколы, используемые Azure AD B2C для взаимодействия с Azure AD. Этот выполняется в элементе **TechnicalProfile** в **ClaimsProvider**.

1. Обновите идентификатор элемента **TechnicalProfile**. Этот идентификатор используется для ссылки на этот технический профиль из других частей политики, например `OIDC-Contoso`.
1. Обновите значение **DisplayName**. Это значение будет отображаться на кнопке входа на экране входа в систему.
1. Обновите значение **Description**.
1. Azure AD использует протокол OpenID Connect, поэтому для параметра **Protocol** должно быть задано значение `OpenIdConnect`.
1. Задайте для параметра **METADATA** значение `https://login.microsoftonline.com/tenant-name.onmicrosoft.com/v2.0/.well-known/openid-configuration`, где `tenant-name` — это имя клиента Azure AD. Например `https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration`.
1. Задайте для параметра **client_id** значение идентификатора приложения из регистрации приложения.
1. В разделе **криптографиккэйс**измените значение **идентификатором storagereferenceid** на имя созданного ранее ключа политики. Например, `B2C_1A_ContosoAppSecret`.

### <a name="upload-the-extension-file-for-verification"></a>Отправка файла расширения для проверки

К этому моменту политика настроена, так что Azure AD B2C знает, как взаимодействовать с каталогом Azure AD. Попробуйте отправить файл расширения политики, чтобы убедиться, что все в порядке.

1. На странице **Пользовательские политики** в клиенте Azure AD B2C выберите **Отправить политику**.
1. Включите функцию **Перезаписать политику, если она уже существует**, а затем найдите и выберите файл *TrustFrameworkExtensions.xml*.
1. Щелкните **Отправить**.

## <a name="register-the-claims-provider"></a>Регистрация поставщика утверждений

На этом этапе поставщик удостоверений настроен, но еще не доступен ни на одной из страниц регистрации и входа. Чтобы сделать его доступным, создайте дубликат существующего шаблона, а затем измените его, чтобы у него также был поставщик удостоверений Azure AD:

1. Откройте файл *TrustFrameworkBase.xml* из начального пакета.
1. Найдите и скопируйте все содержимое элемента **UserJourney**, в котором присутствует запись `Id="SignUpOrSignIn"`.
1. Откройте файл *TrustFrameworkExtensions.xml* и найдите элемент **UserJourneys**. Если элемент не существует, добавьте его.
1. Вставьте все скопированное содержимое элемента **UserJourney** в качестве дочернего элемента в элемент **UserJourneys**.
1. Переименуйте идентификатор пути взаимодействия пользователя. Например, `SignUpSignInContoso`.

### <a name="display-the-button"></a>Отображение кнопки

Элемент **клаимспровидерселектион** аналогичен кнопке поставщика удостоверений на странице регистрации или входа. Если вы добавите для учетной записи Azure AD элемент **ClaimsProviderSelection**, при переходе пользователя на страницу отобразится новая кнопка.

1. Найдите элемент **орчестратионстеп** , который включает `Order="1"` в пути взаимодействия пользователя, созданного в *TrustFrameworkExtensions. XML*.
1. Добавьте следующий элемент под элементом **ClaimsProviderSelections**. Установите для параметра **TargetClaimsExchangeId** соответствующее значение, например `ContosoExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Связывание кнопки с действием

Теперь, когда у вас есть кнопка, вам необходимо связать ее с действием. В этом случае действие — это возможность взаимодействия Azure AD B2C с Azure AD для получения токена. Свяжите кнопку с действием, связав технический профиль для поставщика утверждений Azure AD.

1. Найдите элемент **OrchestrationStep**, содержащий `Order="2"` в пути пользователя.
1. Добавьте следующий элемент **ClaimsExchange**, убедившись, что для **Id** можно использовать то же значение, которое было использовано для **TargetClaimsExchangeId**:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="OIDC-Contoso" />
    ```

    Обновите значение **TechnicalProfileReferenceId**, присвоив ему значение **Id** ранее созданного технического профиля. Например, `OIDC-Contoso`.

1. Сохраните файл *TrustFrameworkExtensions.xml* и повторно отправьте его для проверки.

## <a name="create-an-azure-ad-b2c-application"></a>Создание приложения Azure AD B2C

Связь с Azure AD B2C происходит через приложение, регистрируемое в клиенте B2C. В этом разделе перечислены необязательные действия, которые можно выполнить, чтобы создать тестовое приложение, если вы его еще не создали.

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

## <a name="update-and-test-the-relying-party-file"></a>Обновление и тестирование файла проверяющей стороны

Обновите файл проверяющей стороны, который активирует созданный путь взаимодействия пользователя.

1. Создайте копию *SignUpOrSignIn.xml* в рабочем каталоге и переименуйте ее. Например, переименуйте ее в *SignUpSignInContoso.xml*.
1. Откройте новый файл и обновите значение атрибута **PolicyId** для **TrustFrameworkPolicy**, указав уникальное значение. Например, `SignUpSignInContoso`.
1. Обновите значение **PublicPolicyUri**, указав URI для политики. Например, `http://contoso.com/B2C_1A_signup_signin_contoso`.
1. Обновите значение атрибута **ReferenceId** в **дефаултусержаурнэй** , чтобы оно совпадало с идентификатором пути взаимодействия пользователя, созданного ранее. Например, *сигнупсигнинконтосо*.
1. Сохраните изменения и отправьте файл.
1. В разделе **пользовательские политики**выберите новую политику в списке.
1. В раскрывающемся списке **Выбор приложения** выберите созданное ранее приложение Azure AD B2C. Например, *testapp1*.
1. Скопируйте **конечную точку "запустить сейчас** " и откройте ее в частном окне браузера, например в режиме режиме инкогнито в Google Chrome или в окне InPrivate в Microsoft погранично. Открытие в частном окне браузера позволяет протестировать полное путешествие пользователя, не используя кэшированные учетные данные Azure AD.
1. Выберите кнопку входа в Azure AD, например *сотрудник Contoso*, а затем введите учетные данные пользователя в клиенте организации Azure AD. Вам будет предложено авторизовать приложение, а затем ввести сведения для профиля.

Если вход выполнен успешно, браузер перенаправляется в `https://jwt.ms`, который отображает содержимое маркера, возвращенного Azure AD B2C.

## <a name="next-steps"></a>Дальнейшие действия

При работе с пользовательскими политиками иногда может потребоваться дополнительная информация при устранении неполадок политики во время ее разработки.

Чтобы помочь в диагностике проблем, можно временно перевести политику в режим разработчика и собрать журналы с помощью Azure Application Insights. Узнайте, как в [Azure Active Directory B2C: сбор журналов](troubleshoot-with-application-insights.md).
