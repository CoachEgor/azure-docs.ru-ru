---
title: Функции шаблонов — логические
description: Описываются функции, используемые в шаблоне Azure Resource Manager для определения логических значений.
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: df8433d167a166fe94d965f81e42cd0b3e8f0e54
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2019
ms.locfileid: "74150692"
---
# <a name="logical-functions-for-azure-resource-manager-templates"></a>Логические функции для шаблонов Azure Resource Manager

Resource Manager предоставляет ряд функций для выполнения сравнений в шаблонах.

* [and](#and) (и);
* [bool](#bool);
* [if](#if) (если);
* [not](#not) (не);
* [or](#or) (или).

## <a name="and"></a>и

`and(arg1, arg2, ...)`

Проверяет, соответствуют ли истине все значения параметров.

### <a name="parameters"></a>parameters

| Параметр | обязательные | введите | ОПИСАНИЕ |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |логическое значение |Первое значение, которое необходимо проверить на соответствие истине. |
| arg2 |Yes |логическое значение |Второе значение, которое необходимо проверить на соответствие истине. |
| дополнительные аргументы |Нет |логическое значение |Дополнительные аргументы для проверки соответствия истине. |

### <a name="return-value"></a>Возвращаемое значение

Возвращает результат **True**, если все значения соответствуют истине. В противном случае — **False**.

### <a name="examples"></a>Примеры

В следующем [примере шаблона](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) показано, как использовать логические функции.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Выходные данные из предыдущего примера:

| имя | введите | Значение |
| ---- | ---- | ----- |
| andExampleOutput | Bool | Ложь |
| orExampleOutput | Bool | True, |
| notExampleOutput | Bool | Ложь |

## <a name="bool"></a>bool

`bool(arg1)`

Преобразует параметр в логическое значение.

### <a name="parameters"></a>parameters

| Параметр | обязательные | введите | ОПИСАНИЕ |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |строка или целое число |Значение, которое необходимо преобразовать в логическое. |

### <a name="return-value"></a>Возвращаемое значение
Логическое выражение преобразованного значения.

### <a name="examples"></a>Примеры

В следующем [примере шаблона](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/bool.json) показано, как использовать функцию bool со строкой или целым числом.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "trueString": {
            "value": "[bool('true')]",
            "type" : "bool"
        },
        "falseString": {
            "value": "[bool('false')]",
            "type" : "bool"
        },
        "trueInt": {
            "value": "[bool(1)]",
            "type" : "bool"
        },
        "falseInt": {
            "value": "[bool(0)]",
            "type" : "bool"
        }
    }
}
```

Выходные данные из предыдущего примера со значениями по умолчанию:

| имя | введите | Значение |
| ---- | ---- | ----- |
| trueString | Bool | True, |
| falseString | Bool | Ложь |
| trueInt | Bool | True, |
| falseInt | Bool | Ложь |

## <a name="if"></a>if

`if(condition, trueValue, falseValue)`

Возвращает значение в зависимости от того, выполняется ли условие.

### <a name="parameters"></a>parameters

| Параметр | обязательные | введите | ОПИСАНИЕ |
|:--- |:--- |:--- |:--- |
| condition |Yes |логическое значение |Значение для проверки истинности или ложности. |
| trueValue |Yes | строка, целое число, объект или массив |Возвращаемое значение, если условие выполняется. |
| falseValue |Yes | строка, целое число, объект или массив |Возвращаемое значение, если условие не выполняется. |

### <a name="return-value"></a>Возвращаемое значение

Возвращает второй параметр, если первый параметр имеет значение **True** (Истина). В противном случае возвращает третий параметр.

### <a name="remarks"></a>Примечания

Если условие **истинно**, вычисляется только значение true. Если условие имеет значение **false**, вычисляется только значение false. С помощью функции **If** можно включить выражения, которые являются только условно допустимыми. Например, можно сослаться на ресурс, который существует по одному условию, но не под другим условием. Пример условной оценки выражений показан в следующем разделе.

### <a name="examples"></a>Примеры

В следующем [примере шаблона](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/if.json) показано, как использовать функцию `if`:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "yesOutput": {
            "type": "string",
            "value": "[if(equals('a', 'a'), 'yes', 'no')]"
        },
        "noOutput": {
            "type": "string",
            "value": "[if(equals('a', 'b'), 'yes', 'no')]"
        },
        "objectOutput": {
            "type": "object",
            "value": "[if(equals('a', 'a'), json('{\"test\": \"value1\"}'), json('null'))]"
        }
    }
}
```

Выходные данные из предыдущего примера:

| имя | введите | Значение |
| ---- | ---- | ----- |
| yesOutput | Строка, | Да |
| noOutput | Строка, | Нет |
| objectOutput | Объект. | { "test": "value1" } |

В следующем [примере шаблона](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/conditionWithReference.json) показано, как использовать эту функцию с выражениями, которые являются только условно допустимыми.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "logAnalytics": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "condition": "[not(empty(parameters('logAnalytics')))]",
            "name": "[concat(parameters('vmName'),'/omsOnboarding')]",
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "location": "[parameters('location')]",
            "apiVersion": "2017-03-30",
            "properties": {
                "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                "type": "MicrosoftMonitoringAgent",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "workspaceId": "[if(not(empty(parameters('logAnalytics'))), reference(parameters('logAnalytics'), '2015-11-01-preview').customerId, json('null'))]"
                },
                "protectedSettings": {
                    "workspaceKey": "[if(not(empty(parameters('logAnalytics'))), listKeys(parameters('logAnalytics'), '2015-11-01-preview').primarySharedKey, json('null'))]"
                }
            }
        }
    ],
    "outputs": {
        "mgmtStatus": {
            "type": "string",
            "value": "[if(not(empty(parameters('logAnalytics'))), 'Enabled monitoring for VM!', 'Nothing to enable')]"
        }
    }
}
```

## <a name="not"></a>not

`not(arg1)`

Преобразует логическое значение в противоположное ему значение.

### <a name="parameters"></a>parameters

| Параметр | обязательные | введите | ОПИСАНИЕ |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |логическое значение |Значение, которое необходимо преобразовать. |

### <a name="return-value"></a>Возвращаемое значение

Возвращает значение **True** (Истина), если параметр имеет значение **False** (Ложь). Возвращает значение **False** (Ложь), если параметр имеет значение **True** (Истина).

### <a name="examples"></a>Примеры

В следующем [примере шаблона](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) показано, как использовать логические функции.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Выходные данные из предыдущего примера:

| имя | введите | Значение |
| ---- | ---- | ----- |
| andExampleOutput | Bool | Ложь |
| orExampleOutput | Bool | True, |
| notExampleOutput | Bool | Ложь |

В следующем [примере шаблона](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/not-equals.json) используется **not** и [equals](resource-group-template-functions-comparison.md#equals).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "checkNotEquals": {
            "type": "bool",
            "value": "[not(equals(1, 2))]"
        }
    }
```

Выходные данные из предыдущего примера:

| имя | введите | Значение |
| ---- | ---- | ----- |
| checkNotEquals | Bool | True, |

## <a name="or"></a>или

`or(arg1, arg2, ...)`

Проверяет, соответствует ли истине какое-либо значение параметров.

### <a name="parameters"></a>parameters

| Параметр | обязательные | введите | ОПИСАНИЕ |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |логическое значение |Первое значение, которое необходимо проверить на соответствие истине. |
| arg2 |Yes |логическое значение |Второе значение, которое необходимо проверить на соответствие истине. |
| дополнительные аргументы |Нет |логическое значение |Дополнительные аргументы для проверки соответствия истине. |

### <a name="return-value"></a>Возвращаемое значение

Возвращает результат **True**, если какое-либо значение соответствует истине. В противном случае — **False**.

### <a name="examples"></a>Примеры

В следующем [примере шаблона](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) показано, как использовать логические функции.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Выходные данные из предыдущего примера:

| имя | введите | Значение |
| ---- | ---- | ----- |
| andExampleOutput | Bool | Ложь |
| orExampleOutput | Bool | True, |
| notExampleOutput | Bool | Ложь |

## <a name="next-steps"></a>Дополнительная информация

* Описание разделов в шаблоне Azure Resource Manager см. в статье [Создание шаблонов Azure Resource Manager](resource-group-authoring-templates.md).
* Инструкции по объединению нескольких шаблонов см. в статье [Использование связанных шаблонов в Azure Resource Manager](resource-group-linked-templates.md).
* Указания по выполнению заданного количества циклов итерации при создании типа ресурса см. в статье [Создание нескольких экземпляров ресурсов в Azure Resource Manager](resource-group-create-multiple.md).
* Указания по развертыванию созданного шаблона см. в статье, посвященной [развертыванию приложения с помощью шаблона Azure Resource Manager](resource-group-template-deploy.md).

