---
title: Определяемые пользователем функции в шаблонах
description: Описывает, как определять и использовать определяемые пользователем функции в шаблоне Azure Resource Manager.
ms.topic: conceptual
ms.date: 09/05/2019
ms.openlocfilehash: 58b9ba7b162736329cf775e2be5a47bfcae0a4ca
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/16/2020
ms.locfileid: "76122480"
---
# <a name="user-defined-functions-in-azure-resource-manager-template"></a>Определяемые пользователем функции в шаблоне Azure Resource Manager

В шаблоне можно создать свои собственные функции. Эти функции доступны для использования в шаблоне. Определяемые пользователем функции отделены от [стандартных функций шаблонов](template-functions.md) , которые автоматически доступны в шаблоне. Создавайте собственные функции при наличии сложных выражений, которые многократно используются в шаблоне.

В этой статье описывается добавление определяемых пользователем функций в шаблон Azure Resource Manager.

## <a name="define-the-function"></a>Определение функции

Для функции требуется значение пространства имен, чтобы избежать конфликтов именования с функциями шаблона. В следующем примере показана функция, которая возвращает имя учетной записи хранения:

```json
"functions": [
  {
    "namespace": "contoso",
    "members": {
      "uniqueName": {
        "parameters": [
          {
            "name": "namePrefix",
            "type": "string"
          }
        ],
        "output": {
          "type": "string",
          "value": "[concat(toLower(parameters('namePrefix')), uniqueString(resourceGroup().id))]"
        }
      }
    }
  }
],
```

## <a name="use-the-function"></a>Использование функции

В следующем примере показано, как вызвать функцию.

```json
"resources": [
  {
    "name": "[contoso.uniqueName(parameters('storageNamePrefix'))]",
    "apiVersion": "2016-01-01",
    "type": "Microsoft.Storage/storageAccounts",
    "location": "South Central US",
    "tags": {},
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
  }
]
```

## <a name="limitations"></a>Ограничения

При определении пользовательской функции есть несколько ограничений:

* Функция не может обращаться к переменным.
* Функция может использовать только параметры, определенные в самой функции. При использовании функции [Parameters](template-functions-deployment.md#parameters) в определяемой пользователем функции вы можете использовать только параметры этой функции.
* Функция не может вызывать другие функции, определяемые пользователем.
* Функция не может использовать функцию [Reference](template-functions-resource.md#reference) или любую из функций [списка](template-functions-resource.md#list) .
* Для параметров этой функции нельзя задавать значения по умолчанию.


## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения о доступных свойствах определяемых пользователем функций см. в разделе Общие сведения о [структуре и синтаксисе шаблонов Azure Resource Manager](template-syntax.md).
* Список доступных функций шаблонов см. в разделе [функции шаблонов Azure Resource Manager](template-functions.md).
