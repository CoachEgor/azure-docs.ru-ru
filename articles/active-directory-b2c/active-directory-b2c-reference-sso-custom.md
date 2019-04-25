---
title: Управление сеансами единого входа с помощью настраиваемых политик в Azure Active Directory B2C | Документация Майкрософт
description: Узнайте, как для управлять сеансами единого входа с помощью настраиваемых политик в Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: d1d76e3ac995d4ee63e36ac3560d20f473d3ea2d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60316975"
---
# <a name="single-sign-on-session-management-in-azure-active-directory-b2c"></a>Управление сеансами единого входа в Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Управление сеансами единого входа в Azure Active Directory (Azure AD) B2C позволяет администраторам управлять взаимодействием с пользователем после прохождения им аутентификации. Например, администратор может управлять отображением выбранных поставщиков удостоверений или необходимостью повторного ввода данных локальной учетной записи. В этой статье описывается настройка параметров единого входа для Azure AD B2C.

Управление сеансами единого входа состоит из двух частей. Первая относится к непосредственному взаимодействию пользователя с Azure AD B2C, а вторая — к взаимодействию пользователя с внешними сторонами, например Facebook. Azure AD B2C не переопределяет и не обходит сеансы единого входа, которые могут поддерживаться внешними сторонами. Вместо этого маршрут для доступа к внешней стороне, проходящий через Azure AD B2C, "запоминается", устраняя необходимость повторно предлагать пользователю выбрать поставщик удостоверений социальных сетей или поставщик корпоративных удостоверений. Окончательный выбор сеанса единого входа остается за внешней стороной.

Для управления сеансами единого входа используется та же семантика, что и для любого другого технического профиля в настраиваемых политиках. При выполнении шага оркестрации из технического профиля, связанного с шагом, запрашиваются ссылка на `UseTechnicalProfileForSessionManagement`. Затем, при наличии такового, указанный поставщик сеансов единого входа проверяется, чтобы определить, является ли пользователь участником сеанса. Если это так, то поставщик сеансов единого входа используется для повторного заполнения данных сеанса. Аналогичным образом после выполнения шага оркестрации поставщик используется для сохранения сведений в сеансе, если был указан поставщик сеансов единого входа.

В Azure AD B2C можно использовать определенное количество поставщиков сеансов единого входа.

* NoopSSOSessionProvider;
* DefaultSSOSessionProvider;
* ExternalLoginSSOSessionProvider;
* SamlSSOSessionProvider.

Классы управления единым входом указываются с помощью элемента `<UseTechnicalProfileForSessionManagement ReferenceId=“{ID}" />` технического профиля.

## <a name="noopssosessionprovider"></a>NoopSSOSessionProvider

Как можно понять из названия, этот поставщик не выполняет никаких действий. Его можно использовать для подавления реакции службы единого входа на события для конкретного технического профиля.

## <a name="defaultssosessionprovider"></a>DefaultSSOSessionProvider

Этот поставщик можно использовать для хранения утверждений в сеансе. Как правило, этот поставщик указывается в техническом профиле, используемом для управления локальными учетными записями. При использовании DefaultSSOSessionProvider для хранения утверждений в рамках сеанса необходимо убедиться в следующем. Все утверждения, которые будут возвращены в приложение или использоваться в соответствии с предварительными условиями в последующих шагах, должны храниться в сеансе или считываться из из профиля пользователя в каталоге. Это убережет вас от сбоя аутентификации из-за отсутствующих утверждений.

```XML
<TechnicalProfile Id="SM-AAD">
    <DisplayName>Session Mananagement Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.DefaultSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="objectId" />
        <PersistedClaim ClaimTypeReferenceId="newUser" />
        <PersistedClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" />
    </PersistedClaims>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="objectIdFromSession" DefaultValue="true" />
    </OutputClaims>
</TechnicalProfile>
```

Чтобы добавить утверждения в сеанс, используйте элемент `<PersistedClaims>` технического профиля. При использовании этого поставщика для повторного заполнения данных сеанса сохраненные утверждения добавляются в контейнер утверждений. `<OutputClaims>` используется для получения утверждений из сеанса.

## <a name="externalloginssosessionprovider"></a>ExternalLoginSSOSessionProvider

Этот поставщик используется для подавления экрана "Выбор поставщика удостоверений". Обычно он указывается в техническом профиле, настроенном для внешнего поставщика удостоверений, например Facebook. 

```XML
<TechnicalProfile Id="SM-SocialLogin">
    <DisplayName>Session Mananagement Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.ExternalLoginSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

## <a name="samlssosessionprovider"></a>SamlSSOSessionProvider

Этот поставщик используется для управления сеансами SAML в Azure AD B2C между приложениями, а также сеансами внешних поставщиков удостоверений SAML.

```XML
<TechnicalProfile Id="SM-Reflector-SAML">
    <DisplayName>Session Management Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="IncludeSessionIndex">false</Item>
        <Item Key="RegisterServiceProviders">false</Item>
    </Metadata>
</TechnicalProfile>
```

Технический профиль содержит два элемента метаданных.

| Элемент | По умолчанию | Возможные значения | ОПИСАНИЕ
| --- | --- | --- | --- |
| IncludeSessionIndex | Да | True или false | Указывает поставщику, что следует сохранить индекс сеанса. |
| RegisterServiceProviders | Да | True или false | Указывает, что поставщик должен зарегистрировать все поставщики услуг SAML, которыми было выдано утверждение. |

При использовании поставщика для хранения сеанса поставщика удостоверений SAML указанные выше элементы должны иметь значение false. При использовании поставщика для хранения сеанса SAML B2C указанные выше элементы должны иметь значение true или должны быть опущены, так как значением по умолчанию является true. Для завершения выхода из сеанса SAML требуются `SessionIndex` и `NameID`.

