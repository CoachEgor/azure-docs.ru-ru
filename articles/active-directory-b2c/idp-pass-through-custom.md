---
title: Передача маркера доступа с помощью пользовательской политики в приложение в Azure Active Directory B2C | Документация Майкрософт
description: Сведения о передаче маркера доступа для поставщиков удостоверений OAuth2.0 в качестве утверждения через пользовательскую политику в приложение в Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: d9420398c012b8da18e3b035cb845db6ce8c942d
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2019
ms.locfileid: "66511065"
---
# <a name="pass-an-access-token-through-a-custom-policy-to-your-application-in-azure-active-directory-b2c"></a>Передача маркера доступа с помощью пользовательской политики в приложение в Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

[Пользовательская политика](active-directory-b2c-get-started-custom.md) в Azure Active Directory (Azure AD) B2C предоставляет пользователям вашего приложения возможность зарегистрироваться или войти в систему с помощью поставщика удостоверений. При этом Azure AD B2C получает [маркер доступа](active-directory-b2c-reference-tokens.md) от поставщика удостоверений. Azure AD B2C использует этот маркер для извлечения сведений о пользователе. Вы добавляете тип утверждения и исходящее утверждение в пользовательскую политику для передачи маркера приложениям, зарегистрированным в Azure AD B2C. 

Azure AD B2C поддерживает передачу маркеров доступа от поставщиков удостоверений по протоколу [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) и стандарту [Подключение OpenID](active-directory-b2c-reference-oidc.md). Для остальных поставщиков удостоверений утверждение возвращается пустым.

## <a name="prerequisites"></a>Технические условия

- Ваша пользовательская политика поддерживает поставщиков удостоверений OAuth 2.0 или Подключение OpenID.

## <a name="add-the-claim-elements"></a>Добавление элементов утверждения 

1. Откройте файл *TrustframeworkExtensions.xml* и добавьте элемент **ClaimType** с идентификатором `identityProviderAccessToken` в элемент **ClaimsSchema**:

    ```XML
    <BuildingBlocks>
      <ClaimsSchema>
        <ClaimType Id="identityProviderAccessToken">
          <DisplayName>Identity Provider Access Token</DisplayName>
          <DataType>string</DataType>
          <AdminHelpText>Stores the access token of the identity provider.</AdminHelpText>
        </ClaimType>
        ...
      </ClaimsSchema>
    </BuildingBlocks>
    ```

2. Добавьте элемент **OutputClaim** в элемент **TechnicalProfile** для каждого поставщика удостоверений OAuth 2.0, для которого нужно использовать маркер доступа. В следующем примере показано добавление элемента в технический профиль Facebook:

    ```XML
    <ClaimsProvider>
      <DisplayName>Facebook</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Facebook-OAUTH">
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="identityProviderAccessToken" PartnerClaimType="{oauth2:access_token}" />
          </OutputClaims>
          ...
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

3. Сохраните файл *TrustframeworkExtensions.xml*.
4. Откройте файл политики проверяющей стороны, например *SignUpOrSignIn.xml*, и добавьте элемент **OutputClaim** в **TechnicalProfile**:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
      <TechnicalProfile Id="PolicyProfile">
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="identityProviderAccessToken" PartnerClaimType="idp_access_token"/>
        </OutputClaims>
        ...
      </TechnicalProfile>
    </RelyingParty>
    ```

5. Сохраните файл политики.

## <a name="test-your-policy"></a>Тестирование политики

При тестировании приложений в Azure AD B2C может потребоваться вернуть маркер Azure AD B2C в `https://jwt.ms` для просмотра в нем утверждений.

### <a name="upload-the-files"></a>Передача файлов

1. Войдите на [портале Azure](https://portal.azure.com/).
2. Убедитесь, что используете каталог, содержащий клиент Azure AD B2C, щелкнув **Фильтр каталога и подписки** в верхнем меню и выбрав каталог, содержащий ваш клиент.
3. Выберите **Все службы** в левом верхнем углу окна портала Azure, а затем найдите и выберите **Azure AD B2C**.
4. Выберите **Инфраструктура процедур идентификации**.
5. На странице "Настраиваемые политики" щелкните **Отправить политику**.
6. Выберите **Перезаписать политику, если она существует**, а затем найдите и выберите файл *TrustFrameworkExtensions.xml*.
7. Щелкните **Отправить**.
8. Повторите шаги с 5 по 7 для файла проверяющей стороны, например *SignUpOrSignIn.xml*.

### <a name="run-the-policy"></a>Запуск политики

1. Откройте измененную политику. Например, *B2C_1A_signup_signin*.
2. В разделе **Приложение** выберите зарегистрированное ранее приложение. Чтобы маркер отображался, как в приведенном ниже примере, **URL-адрес ответа** должен быть следующим: `https://jwt.ms`.
3. Щелкните **Запустить сейчас**.

    Должен отобразиться результат, аналогичный следующему примеру:

    ![Декодированный маркер](./media/idp-pass-through-custom/idp-pass-through-custom-token.png)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в [справочнике по маркерам Azure Active Directory](active-directory-b2c-reference-tokens.md).





