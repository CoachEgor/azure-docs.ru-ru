---
title: Элемент пользовательского интерфейса StorageAccountSelector для портала Azure | Документация Майкрософт
description: Сведения об элементе пользовательского интерфейса Microsoft.Storage.StorageAccountSelector для портала Azure.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: tomfitz
ms.openlocfilehash: c6d4ef50645902aecd57ceb9fc48b7d99bf22d53
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "62104872"
---
# <a name="microsoftstoragestorageaccountselector-ui-element"></a>Элемент пользовательского интерфейса Microsoft.Storage.StorageAccountSelector
Элемент управления для выбора новой или имеющейся учетной записи хранения.

## <a name="ui-sample"></a>Пример элемента пользовательского интерфейса

Элемент управления, который отображает значение по умолчанию.

![Элемент пользовательского интерфейса Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft.storage.storageaccountselector.png)

Элемент управления, который позволяет выбрать существующую учетную запись хранения или создать новую.

![Microsoft.Storage.StorageAccountSelector: создание учетной записи хранения](./media/managed-application-elements/microsoft.storage.storageaccountselector-new.png)

## <a name="schema"></a>Схема

```json
{
  "name": "element1",
  "type": "Microsoft.Storage.StorageAccountSelector",
  "label": "Storage account",
  "toolTip": "",
  "defaultValue": {
    "name": "storageaccount01",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "options": {
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Примечания
- Если необходимо, параметр `defaultValue.name` автоматически проверяется на уникальность. Если имя учетной записи хранения не является уникальным, пользователь должен указать другое имя или выбрать существующую учетную запись хранения.
- Значением по умолчанию для параметра `defaultValue.type` является **Premium_LRS**.
- Любой тип, не указанный в `constraints.allowedTypes`, скрыт, а любой тип, не указанный в `constraints.excludedTypes`, отображается. Параметры `constraints.allowedTypes` и `constraints.excludedTypes` являются необязательными. При этом их нельзя использовать одновременно.
- Если для параметра `options.hideExisting` задано значение **true**, пользователь не может выбрать имеющуюся учетную запись хранения. Значение по умолчанию — **false**.

## <a name="sample-output"></a>Пример выходных данных

```json
{
  "name": "storageaccount01",
  "resourceGroup": "rg01",
  "type": "Premium_LRS",
  "newOrExisting": "new"
}
```

## <a name="next-steps"></a>Дальнейшие действия
* Общие сведения о создании определений пользовательского интерфейса см. в статье [Начало работы с CreateUiDefinition](create-uidefinition-overview.md).
* Дополнительные сведения об общих свойствах элементов пользовательского интерфейса см. в статье [Элементы CreateUiDefinition](create-uidefinition-elements.md).
