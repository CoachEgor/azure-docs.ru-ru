---
title: ClaimsProviders в Azure Active Directory B2C | Документация Майкрософт
description: Сведения об указании элемента ClaimsProviders настраиваемой политики в Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 5a20f40c893c36823906d5cecadd9be21b8a4fd2
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76836020"
---
# <a name="claimsproviders"></a>ClaimsProviders

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Поставщик утверждений может иметь несколько [технических профилей](technicalprofiles.md). Каждый поставщик утверждений должен иметь один или несколько технических профилей, которые определяют конечные точки и протоколы для связи с этим поставщиком утверждений. Поставщик утверждений может иметь несколько технических профилей. Например, может быть определено несколько технических профилей, так как рассматриваемый поставщик утверждений поддерживает несколько протоколов, различные конечные точки с разными возможностями или выпускает различные утверждения на разных уровнях безопасности. Это может быть приемлемым для выдачи конфиденциальных утверждений в одном пути взаимодействия пользователя, а в другом — нет.

```XML
<ClaimsProviders>
  <ClaimsProvider>
    <Domain>Domain name</Domain>
    <DisplayName>Display name</DisplayName>
    <TechnicalProfiles>
      </TechnicalProfile>
        ...
      </TechnicalProfile>
        ...
    </TechnicalProfiles>
  </ClaimsProvider>
  ...
</ClaimsProviders>
```

Элемент **ClaimsProviders** содержит следующий элемент.

| Элемент | Вхождения | Description |
| ------- | ----------- | ----------- |
| ClaimsProvider | 1:n | Аккредитованный поставщик утверждений, который может использоваться в различных путях взаимодействия пользователя. |

## <a name="claimsprovider"></a>ClaimsProvider

Элемент **ClaimsProvider** содержит следующие элементы.

| Элемент | Вхождения | Description |
| ------- | ---------- | ----------- |
| Домен | 0:1 | Строка, содержащая имя домена для поставщика утверждений. Например, если в техническом профиле указан поставщик удостоверений Facebook, доменное имя будет иметь значение Facebook.com. Если не переопределен технический профиль, это доменное имя используется для всех технических профилей, определенных в поставщике утверждений. Имя домена также можно использовать в **domain_hint**. Дополнительные сведения см. в разделе **Перенаправление операции входа в поставщик социальных сетей** статьи [Настройка прямого входа в систему с помощью Azure Active Directory B2C](direct-signin.md). |
| DisplayName | 1:1 | Строка с именем технического профиля, отображаемым для пользователей. |
| [Технические профили](technicalprofiles.md) | 0:1 | Набор технических профилей поддерживается поставщиком утверждений. |

**Поставщика утверждений** организует связь технических профилей с поставщиком утверждений. В следующем примере показано поставщика утверждений Azure Active Directory с техническими профилями Azure Active Directory.

```XML
<ClaimsProvider>
  <DisplayName>Azure Active Directory</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="AAD-Common">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId-NoError">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
      ...
    </TechnicalProfile>
      ...
    <TechnicalProfile Id="AAD-UserWritePasswordUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWritePhoneNumberUsingObjectId">
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

В следующем примере показано поставщика утверждений Facebook с техническим профилем **Facebook OAUTH**.

```XML
<ClaimsProvider>
  <Domain>facebook.com</Domain>
  <DisplayName>Facebook</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Facebook-OAUTH">
      <DisplayName>Facebook</DisplayName>
      <Protocol Name="OAuth2" />
        ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```
