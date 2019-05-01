---
title: Элемент BuildingBlocks в Azure Active Directory B2C | Документация Майкрософт
description: Сведения об указании элемента BuildingBlocks настраиваемой политики в Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 1a7a5463d24ba15b7bd2b514b8c7bce3799f3191
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64688901"
---
# <a name="buildingblocks"></a>BuildingBlocks

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Элемент **BuildingBlocks** добавляется внутри элемента [TrustFrameworkPolicy](trustframeworkpolicy.md).

```XML
<TrustFrameworkPolicy
  xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="https://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="mytenant.onmicrosoft.com"
  PolicyId="B2C_1A_TrustFrameworkBase"
  PublicPolicyUri="http://mytenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase">

  <BuildingBlocks>
    <ClaimsSchema>
      ...
    </ClaimsSchema>
    <Predicates>
    ...
    </Predicates>
    <PredicateValidations>
    ...
    </PredicateValidations>
    <ClaimsTransformations>
      ...
    </ClaimsTransformations>
    <ContentDefinitions>
      ...
    </ContentDefinitions>
    <Localization>
      ...
    </Localization>
 </BuildingBlocks>
```

Элемент **BuildingBlocks** содержит следующие элементы, которые необходимо указывать в заданном порядке:

- Элемент [ClaimsSchema](claimsschema.md) определяет типы утверждений, на которые можно ссылаться в рамках политики. Схема утверждений — это область, в которой объявляются типы утверждений. Тип утверждения аналогичен переменной во многих языках программирования. Вы можете использовать тип утверждения для сбора данных от пользователя приложения, получения утверждений от поставщиков удостоверений социальных сетей, отправки и получения данных из настраиваемого интерфейса REST API или хранения внутренних данных, используемых для настраиваемой политики. 

- [Элементы Predicates и PredicateValidationReference](predicates.md) позволяют выполнять процесс проверки, чтобы обеспечить ввод в утверждение только правильно сформированных данных.
 
- Элемент [ClaimsTransformations](claimstransformations.md) содержит список преобразований утверждений, которые могут применяться в политике.  Преобразование утверждений преобразует одно утверждение в другое. В преобразовании утверждений необходимо задать метод преобразования, например: 
    - Изменение регистра строкового утверждения. Например, изменение регистра строки с нижнего на верхний.
    - Если в результате сравнения двух утверждений возвращается значение true, утверждения совпадают. В противном случае возвращается значение false.
    - Создание строкового утверждения на основе предоставленного параметра в политике.
    - Создание случайной строки с помощью генератора случайных чисел.
    - Форматирование утверждения в соответствии с указанным форматом строки. Это преобразование использует метод C# `String.Format`.

- Элемент [ContentDefinitions](contentdefinitions.md) содержит URL-адреса шаблонов HTML5 для использования в пути взаимодействия пользователя. В настраиваемой политике определение содержимого определяет универсальный код ресурса (URI) страницы HTML5, используемой для данного шага пути взаимодействия пользователя. Например, страницы для входа или регистрации, сброса пароля или страниц ошибок. Можно изменить внешний вид путем переопределения LoadUri для файла HTML5. Вы также можете создать новые определения содержимого в соответствии с потребностями. Этот элемент может содержать ссылку на локализованные ресурсы с использованием идентификатора локализации.

- Элемент [Localization](localization.md) предусматривает поддержку нескольких языков. Поддержка локализации в политиках позволяет настроить список поддерживаемых языков в политике и выбрать язык по умолчанию. Также поддерживаются языковые строки и коллекции.


