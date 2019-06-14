---
title: Автоматическое включение параметров диагностики с помощью шаблона Resource Manager
description: Узнайте, как использовать шаблон Resource Manager для создания параметров диагностики, которые позволят передавать журналы диагностики в Центры событий или сохранять их в учетной записи хранения.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 3/26/2018
ms.author: johnkem
ms.subservice: ''
ms.openlocfilehash: 7edce5175a1dda66abf3316cb8f0eb33e9f64ef7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60236037"
---
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Автоматическое включение параметров диагностики при создании ресурса из шаблона Resource Manager
В этой статье мы покажем, как применить [шаблон Azure Resource Manager](../../azure-resource-manager/resource-group-authoring-templates.md) для настройки параметров диагностики при создании ресурса. Это позволяет автоматически запускать потоковую передачу журналов диагностики и метрик в концентраторы событий, архивировать их в учетную запись хранения или их отправкой в рабочую область Log Analytics при создании ресурса.

> [!WARNING]
> Формат данных журнала в учетной записи хранения будет изменен на JSON Lines с 1 ноября 2018 г. [См. дополнительные сведения, включая информацию об обновлении инструментария для включения поддержки нового формата.](./../../azure-monitor/platform/diagnostic-logs-append-blobs.md) 
>
> 

Для разных типов ресурсов журналы диагностики включаются с помощью шаблона Resource Manager по-разному.

* **Невычислительные** ресурсы (например, группы безопасности сети, Logic Apps или служба автоматизации) используют [параметры диагностики, описанные в этой статье](../../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings).
* **Вычислительные** ресурсы (на основе WAD/LAD) используют [файл конфигурации WAD/LAD, описанный в этой статье](/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines).

В этой статьей объясняется, как настроить диагностику с помощью каждого из этих методов.

Основные этапы:

1. Создайте шаблон в виде файла JSON, который описывает, как создать ресурс и включить диагностику.
2. [Разверните шаблон с помощью любого метода развертывания](../../azure-resource-manager/resource-group-template-deploy.md).

Ниже приведены примеры файла JSON шаблона для создания вычислительных и невычислительных ресурсов.

## <a name="non-compute-resource-template"></a>Шаблон для невычислительных ресурсов
Для невычислительных ресурсов выполните два действия.

1. Добавьте параметры в большой двоичный объект параметров для имени учетной записи хранения, идентификатора правила авторизации концентратора событий и/или идентификатор рабочей области Log Analytics (позволит архивировать журналы диагностики в учетную запись хранения, потоковую передачу журналов в концентраторы событий, и (или) отправлять журналы в Azure Monitor).
   
    ```json
    "settingName": {
      "type": "string",
      "metadata": {
        "description": "Name for the diagnostic setting resource. Eg. 'archiveToStorage' or 'forSecurityTeam'."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "eventHubAuthorizationRuleId": {
      "type": "string",
      "metadata": {
        "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
      }
    },
    "eventHubName": {
      "type": "string",
      "metadata": {
        "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Azure Resource ID of the Log Analytics workspace for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. Добавьте ресурс с типом `[resource namespace]/providers/diagnosticSettings`в массив ресурсов того ресурса, для которого нужно включить журналы диагностики.
   
    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "[concat('Microsoft.Insights/', parameters('settingName'))]",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2017-05-01-preview",
        "properties": {
          "name": "[parameters('settingName')]",
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
          "eventHubName": "[parameters('eventHubName')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ],
          "metrics": [
            {
              "category": "AllMetrics",
              "enabled": true,
              "retentionPolicy": {
                "enabled": false,
                "days": 0
              }
            }
          ]
        }
      }
    ]
    ```

Свойства большого двоичного объекта для параметров диагностики соответствуют [формату, который описан в этой статье](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings/createorupdate). Добавление свойства `metrics` позволит также передавать метрики ресурса в эти же выходные данные, если [этот ресурс поддерживает метрики Azure Monitor](../../azure-monitor/platform/metrics-supported.md).

Ниже приведен полный пример, в котором создается приложение логики, а также включается потоковая передача в Центры событий и хранение в учетной записи хранения.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "logicAppName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Logic App that will be created."
      }
    },
    "testUri": {
      "type": "string",
      "defaultValue": "https://azure.microsoft.com/status/feed/"
    },
    "settingName": {
      "type": "string",
      "metadata": {
        "description": "Name of the setting. Name for the diagnostic setting resource. Eg. 'archiveToStorage' or 'forSecurityTeam'."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "eventHubAuthorizationRuleId": {
      "type": "string",
      "metadata": {
        "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
      }
    },
    "eventHubName": {
      "type": "string",
      "metadata": {
        "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Logic/workflows",
      "name": "[parameters('logicAppName')]",
      "apiVersion": "2016-06-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "definition": {
          "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "[concat('Microsoft.Insights/', parameters('settingName'))]",
          "dependsOn": [
            "[resourceId('Microsoft.Logic/workflows', parameters('logicAppName'))]"
          ],
          "apiVersion": "2017-05-01-preview",
          "properties": {
            "name": "[parameters('settingName')]",
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
            "eventHubName": "[parameters('eventHubName')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "WorkflowRuntime",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ],
            "metrics": [
              {
                "timeGrain": "PT1M",
                "enabled": true,
                "retentionPolicy": {
                  "enabled": false,
                  "days": 0
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>Шаблон для вычислительных ресурсов
Чтобы включить диагностику для вычислительных ресурсов, например виртуальной машины или Service Fabric, выполните эти действия.

1. Добавьте расширение системы диагностики Azure в определение ресурсов для виртуальной машины.
2. Укажите в качестве параметра учетную запись хранения и (или) концентратор событий.
3. Добавьте содержимое XML-файла WADCfg в свойство XMLCfg, правильно экранируя все XML-символы.

> [!WARNING]
> Этот этап может оказаться непростой задачей. [В этой статье](../../virtual-machines/extensions/diagnostics-template.md#diagnostics-configuration-variables) приводится пример разбиения схемы конфигурации диагностики на переменные, которые правильно экранируются и форматируются.
> 
> 

Весь процесса описан [в этом документе](../../virtual-machines/extensions/diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)с примерами.

## <a name="next-steps"></a>Дальнейшие действия
* [Дополнительные сведения о журналах диагностики Azure](../../azure-monitor/platform/diagnostic-logs-overview.md)
* [Потоковая передача журналов диагностики Azure в Центры событий](../../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md)


