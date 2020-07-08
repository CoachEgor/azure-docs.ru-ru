---
title: Переменные в шаблонах
description: Описывает определение переменных в шаблоне Azure Resource Manager.
ms.topic: conceptual
ms.date: 09/05/2019
ms.openlocfilehash: cf135959d30702ea58b7a1d4fdd82625a39245d2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "75483821"
---
# <a name="variables-in-azure-resource-manager-template"></a>Переменные в шаблоне Azure Resource Manager

В этой статье описывается определение и использование переменных в шаблоне Azure Resource Manager. Для упрощения шаблона используются переменные. Вместо повторения сложных выражений во всем шаблоне определяется переменная, которая содержит сложное выражение. Затем вы ссылаетесь на эту переменную по мере необходимости во всем шаблоне.

Диспетчер ресурсов разрешает переменные перед началом операций развертывания. Когда в шаблоне используется переменная, диспетчер ресурсов заменяет ее разрешенным значением.

## <a name="define-variable"></a>Определение переменной

В приведенном ниже примере показано определение переменной. В нем создается строковое значение имени учетной записи хранения. Он использует несколько функций шаблона для получения значения параметра и объединяет его в уникальную строку.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

Нельзя использовать функцию [Reference](template-functions-resource.md#reference) или любую из функций [List](template-functions-resource.md#list) в разделе Variables. Эти функции получают состояние среды выполнения ресурса и не могут быть выполнены до развертывания при разрешении переменных.

## <a name="use-variable"></a>Использование переменной

В шаблоне ссылка на значение параметра осуществляется с помощью функции [Variables](template-functions-deployment.md#variables) . В следующем примере показано, как использовать переменную для свойства ресурса.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageName')]",
    ...
  }
]
```

## <a name="configuration-variables"></a>Переменные конфигурации

Можно определить переменные, которые содержат связанные значения для настройки среды. Переменная определяется как объект со значениями. В следующем примере показан объект, содержащий значения для двух сред — **Test** и **произ**.

```json
"variables": {
  "environmentSettings": {
    "test": {
      "instanceSize": "Small",
      "instanceCount": 1
    },
    "prod": {
      "instanceSize": "Large",
      "instanceCount": 4
    }
  }
},
```

В параметрах следует создать значение, указывающее, какие значения конфигурации будут использоваться.

```json
"parameters": {
  "environmentName": {
    "type": "string",
    "allowedValues": [
      "test",
      "prod"
    ]
  }
},
```

Чтобы получить параметры для указанной среды, используйте вместе переменную и параметр.

```json
"[variables('environmentSettings')[parameters('environmentName')].instanceSize]"
```

## <a name="example-templates"></a>Образцы шаблонов

В следующих примерах демонстрируются сценарии использования переменных.

|Шаблон  |Описание  |
|---------|---------|
| [variable definitions](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variables.json) | Демонстрирует различные типы переменных. Шаблон не развертывает ресурсы. При его помощи только создаются и возвращаются значения переменных. |
| [configuration variable](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variablesconfigurations.json) | Демонстрирует использование переменной, определяющей значения конфигурации. Шаблон не развертывает ресурсы. При его помощи только создаются и возвращаются значения переменных. |
| [network security rules](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) и [parameter file](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json) | Отвечает за создание массива в правильном формате для назначения правил безопасности в группе безопасности сети. |

## <a name="next-steps"></a>Дальнейшие шаги

* Дополнительные сведения о доступных свойствах переменных см. в разделе Общие сведения о [структуре и синтаксисе шаблонов Azure Resource Manager](template-syntax.md).
* Рекомендации по созданию переменных см. в разделе рекомендации [— переменные](template-best-practices.md#variables).
