---
title: Примеры преобразования логических утверждений для схемы инфраструктуры процедур идентификации Azure Active Directory B2C | Документация Майкрософт
description: Примеры преобразования логических утверждений для схемы инфраструктуры процедур идентификации Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: da4fc4704ee72210e180ef95fe6a821c8d116fa2
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/17/2019
ms.locfileid: "71064575"
---
# <a name="boolean-claims-transformations"></a>Преобразования логических утверждений

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

В этой статье приведены примеры использования логических преобразований утверждений схемы инфраструктуры процедур идентификации в Azure Active Directory B2C (Azure AD B2C). Дополнительные сведения см. в статье о [преобразовании утверждений](claimstransformations.md).

## <a name="andclaims"></a>Утверждение AndClaims

Выполняет операцию AND для двух логических элементов InputClaim и задает элемент OutputClaim с результатом операции.

| Элемент  | TransformationClaimType  | Тип данных  | Примечания |
|-------| ------------------------ | ---------- | ----- |
| InputClaim | inputClaim1 | boolean | Первый оцениваемый элемент ClaimType. |
| InputClaim | inputClaim2  | boolean | Второй оцениваемый элемент ClaimType. |
|outputClaim | outputClaim | boolean | Элементы ClaimType, создаваемые после вызова этого преобразования утверждений (true или false). |

В следующем преобразовании утверждений показано, как выполнять операцию AND для двух логических элементов ClaimType: `isEmailNotExist` и `isSocialAccount`. Для исходящего утверждения `presentEmailSelfAsserted` присваивается значение `true`, если значения обоих входящих утверждений — `true`. На шаге оркестрации можно использовать необходимое условие для предварительной настройки страницы самоподтверждения, только если электронная почта учетной записи социальной сети пуста.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="AndClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="isEmailNotExist" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="isSocialAccount" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="presentEmailSelfAsserted" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Пример

- Входящие утверждения:
    - **inputClaim1**: true.
    - **inputClaim2**: false.
- Исходящие утверждения:
    - **outputClaim**: false.


## <a name="assertbooleanclaimisequaltovalue"></a>AssertBooleanClaimIsEqualToValue

Проверяет, равны ли логические значения двух утверждений, и создает исключение, если это не так.

| Элемент | TransformationClaimType  | Тип данных  | Примечания |
| ---- | ------------------------ | ---------- | ----- |
| InputClaim | InputClaim | boolean | Элемент ClaimType, который необходимо подтвердить. |
| InputParameter |valueToCompareTo | boolean | Значение для сравнения (true или false). |

Преобразование утверждений **AssertBooleanClaimIsEqualToValue** всегда выполняется с помощью [технического профиля проверки](validation-technical-profile.md), вызываемого через [самоподтвержденный технический профиль](self-asserted-technical-profile.md). Метаданные самоподтвержденного технического профиля **UserMessageIfClaimsTransformationBooleanValueIsNotEqual** управляют сообщением, поступающим пользователю из технического профиля.

![Выполнение AssertStringClaimsAreEqual](./media/boolean-transformations/assert-execution.png)

В следующем преобразовании утверждений показано, как проверить значение логического элемента ClaimType на основе значения `true`. Если значение ClaimType `accountEnabled` — false, выдается сообщение об ошибке.

```XML
<ClaimsTransformation Id="AssertAccountEnabledIsTrue" TransformationMethod="AssertBooleanClaimIsEqualToValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="accountEnabled" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="valueToCompareTo" DataType="boolean" Value="true" />
  </InputParameters>
</ClaimsTransformation>
```


Технический профиль проверки `login-NonInteractive` вызывает преобразование утверждений `AssertAccountEnabledIsTrue`.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertAccountEnabledIsTrue" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

Самоподтвержденный технический профиль вызывает технический профиль проверки **login-NonInteractive**.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <Metadata>
    <Item Key="UserMessageIfClaimsTransformationBooleanValueIsNotEqual">Custom error message if account is disabled.</Item>
  </Metadata>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="login-NonInteractive" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

### <a name="example"></a>Пример

- Входящие утверждения:
    - **inputClaim**: false.
    - **valueToCompareTo**: true.
- Результат: возникла ошибка.

## <a name="notclaims"></a>NotClaims

Выполняет операцию Not для логического элемента inputClaim и задает элемент outputClaim с результатом операции.

| Элемент | TransformationClaimType | Тип данных | Примечания |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | InputClaim | boolean | Обрабатываемое утверждение. |
| outputClaim | outputClaim | boolean | Элементы ClaimType, создаваемые после вызова данного ClaimsTransformation (true или false). |

Это преобразование утверждений используется для выполнения логического отрицания утверждения.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="NotClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userExists" TransformationClaimType="inputClaim" />
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="userExists" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Пример

- Входящие утверждения:
    - **inputClaim**: false.
- Исходящие утверждения:
    - **outputClaim**: true.

## <a name="orclaims"></a>OrClaims

Вычисляет значение Or для двух логических элементов InputClaim и задает элемент outputClaim с результатом операции.

| Элемент | TransformationClaimType | Тип данных | Примечания |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim1 | boolean | Первый оцениваемый элемент ClaimType. |
| InputClaim | inputClaim2 | boolean | Второй оцениваемый элемент ClaimType. |
| outputClaim | outputClaim | boolean | Элементы ClaimType, создаваемые после вызова ClaimsTransformation (true или false). |

В следующем преобразовании утверждений показано, как выполнять операцию `Or` для двух логических элементов ClaimType. На шаге оркестрации можно использовать необходимое условие для предварительной настройки страницы самоподтверждения, если значение одного из утверждений — `true`.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="OrClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="isLastTOSAcceptedNotExists" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="isLastTOSAcceptedGreaterThanNow" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="presentTOSSelfAsserted" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
</ClaimsTransformation>
```

### <a name="example"></a>Пример

- Входящие утверждения:
    - **inputClaim1**: true.
    - **inputClaim2**: false.
- Исходящие утверждения:
    - **outputClaim**: true.

