---
title: Обновления схемы от 1 августа 2015 г. (предварительная версия). Azure Logic Apps | Документация Майкрософт
description: Обновленная схема от 1 августа 2015 г. (предварительная версия) для определений приложений логики в Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.assetid: 0d03a4d4-e8a8-4c81-aed5-bfd2a28c7f0c
ms.topic: article
ms.date: 05/31/2016
ms.openlocfilehash: 92f522c72f69218e55b1ee4cfff74511a30288b0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60553765"
---
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a>Обновления схемы для Azure Logic Apps от 1 августа 2015 г. (ознакомительная версия)

Эта схема и версия API для Azure Logic Apps включает основные улучшения, которые повышают надежность и простоту использования приложений логики:

* Тип действия **APIApp** теперь называется [**APIConnection**](#api-connections).
* Действие **Repeat** теперь называется [**Foreach**](#foreach).
* [**Прослушиватель HTTP** (приложение API)](#http-listener) больше не требуется.
* При вызове дочерних рабочих процессов используется [новая схема](#child-workflows).

<a name="api-connections"></a>

## <a name="move-to-api-connections"></a>Переход к API-подключениям

Самое значительное изменение в этом обновлении: для использования API больше не нужно развертывать приложения API в подписке Azure. Ниже приведены способы использования API.

* Управляемые API
* Пользовательские веб-API

Обработка этих способов немного отличается из-за разных моделей управления и размещения. Одно из преимуществ этой модели заключается в том, что вы больше не ограничены ресурсами, развернутыми в вашей группе ресурсов Azure. 

### <a name="managed-apis"></a>Управляемые API

Корпорация Майкрософт управляет некоторыми интерфейсами API от вашего имени, например Office 365, Salesforce, Twitter и FTP. Некоторые из управляемых интерфейсов API, например переводчик Bing Translate, можно использовать "как есть", а для других требуется настройка, также называемая *подключением*.

Например, при использовании Office 365 необходимо создать подключение, которое содержит токен входа в Office 365. Этот токен безопасно хранится и обновляется, поэтому ваше приложение логики всегда может вызвать интерфейс API Office 365. Для подключения к FTP-серверу или SQL Server необходимо создать подключение, которое содержит строку подключения. 

В определении эти действия называются `APIConnection`. Вот пример подключения, которое вызывает Office 365 для отправки сообщения электронной почты:

``` json
{
   "actions": {
      "Send_an_email": {
         "type": "ApiConnection",
         "inputs": {
            "host": {
               "api": {
                  "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
               },
               "connection": {
                  "name": "@parameters('$connections')['shared_office365']['connectionId']"
               }
            },
            "method": "POST",
            "body": {
               "Subject": "Reminder",
               "Body": "Don't forget!",
               "To": "me@contoso.com"
            },
            "path": "/Mail"
         }
      }
   }
}
```

Объект `host` — это часть входных данных, которая является уникальной для подключений API и включает следующие компоненты: `api` и `connection`. В объекте `api` указывается URL-адрес среды выполнения, в которой размещается управляемый API-интерфейс. Все доступные управляемые интерфейсы API можно просмотреть путем вызова следующего метода:

```text
GET https://management.azure.com/subscriptions/<Azure-subscription-ID>/providers/Microsoft.Web/locations/<location>/managedApis?api-version=2015-08-01-preview
```

При использовании интерфейса API в нем могут быть определены или не определены *параметры подключения*. Таким образом, если в API не определены эти параметры, подключение не требуется. Если в API определены эти параметры, необходимо создать подключение с указанным именем.  
Затем необходимо сослаться на это имя в объекте `connection` внутри объекта `host`. Чтобы создать подключение в группе ресурсов, вызовите следующий метод:

```text
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Web/connections/<name>?api-version=2015-08-01-preview
```

Текст должен быть следующим:

``` json
{
   "properties": {
      "api": {
         "id": "/subscriptions/<Azure-subscription-ID>/providers/Microsoft.Web/managedApis/azureblob"
      },
      "parameterValues": {
         "accountName": "<Azure-storage-account-name-with-different-parameters-for-each-API>"
      }
   },
   "location": "<logic-app-location>"
}
```

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a>Развертывание управляемых интерфейсов API в шаблоне Azure Resource Manager

Если интерактивный вход не требуется, в шаблоне Azure Resource Manager можно создать полное приложение.
Если вход требуется, то по-прежнему можно применить шаблон Resource Manager, но для авторизации подключений все равно придется воспользоваться порталом Azure. 

``` json
"resources": [ {
   "apiVersion": "2015-08-01-preview",
   "name": "azureblob",
   "type": "Microsoft.Web/connections",
   "location": "[resourceGroup().location]",
   "properties": {
      "api": {
         "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
      },
      "parameterValues": {
         "accountName": "[parameters('storageAccountName')]",
         "accessKey": "[parameters('storageAccountKey')]"
      }
    },
},
{
   "type": "Microsoft.Logic/workflows",
   "apiVersion": "2015-08-01-preview",
   "name": "[parameters('logicAppName')]",
   "location": "[resourceGroup().location]",
   "dependsOn": ["[resourceId('Microsoft.Web/connections', 'azureblob')]"],
   "properties": {
      "sku": {
         "name": "[parameters('sku')]",
         "plan": {
            "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('svcPlanName'))]"
         }
      },
      "parameters": {
         "$connections": {
             "value": {
                  "azureblob": {
                     "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                     "connectionName": "azureblob",
                     "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                  }
             }
         }
      },
      "definition": {
         "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
            "type": "Object",
            "$connections": {
               "defaultValue": {},
 
            }
         },
         "triggers": {
            "Recurrence": {
               "type": "Recurrence",
               "recurrence": {
                  "frequency": "Day",
                  "interval": 1
               }
            }
         },
         "actions": {
            "Create_file": {
               "type": "ApiConnection",
               "inputs": {
                  "host": {
                     "api": {
                        "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                     },
                     "connection": {
                       "name": "@parameters('$connections')['azureblob']['connectionId']"
                     }
                  },
                  "method": "POST",
                  "queries": {
                     "folderPath": "[concat('/', parameters('containerName'))]",
                     "name": "helloworld.txt"
                  },
                  "body": "@decodeDataUri('data:, Hello+world!')",
                  "path": "/datasets/default/files"
               },
               "conditions": []
            }
         },
         "outputs": {}
      }
   }
} ]
```

В этом примере можно увидеть, что подключения — это просто ресурсы, находящиеся в группе ресурсов. Они ссылаются на управляемые интерфейсы API, доступные в подписке.

### <a name="your-custom-web-apis"></a>Пользовательские веб-API

Если вы используете собственные интерфейсы API (не управляемые корпорацией Майкрософт), используйте встроенное действие **HTTP** для их вызова. В идеале для API необходимо предоставить конечную точку Swagger. Эта конечная точка помогает Конструктору Logic Apps отображать входные и выходные данные API. Без конечной точки Swagger конструктор может отображать входные и выходные данные только как непрозрачные объекты JSON.

Вот пример, в котором показано новое свойство `metadata.apiDefinitionUrl` :

``` json
"actions": {
   "mycustomAPI": {
      "type": "Http",
      "metadata": {
         "apiDefinitionUrl": "https://mysite.azurewebsites.net/api/apidef/"  
      },
      "inputs": {
         "uri": "https://mysite.azurewebsites.net/api/getsomedata",
         "method": "GET"
      }
   }
}
```

Если веб-API размещается в службе приложений Azure, он автоматически отображается в списке действий, доступных в конструкторе. В противном случае необходимо вставить URL-адрес напрямую. Конечная точка Swagger должна быть непроверенной, чтобы ее можно было использовать в конструкторе приложений логики. Хотя вы можете защитить сам API с помощью любого метода, поддерживаемого в Swagger.

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a>Вызов развернутых приложений API с версией 2015-08-01-preview

Если вы уже развернули приложение API, его можно вызвать с помощью действия **HTTP**.
Например, если вы используете Dropbox для просмотра списка файлов, то в определении схемы версии **2014-12-01-preview** может содержаться что-то подобное этому:

``` json
"definition": {
   "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
         "defaultValue": "eyJ0eX...wCn90",
         "type": "String",
         "metadata": {
            "token": {
               "name": "/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
            }
         }
      }
    },
    "actions": {
       "dropboxconnector": {
          "type": "ApiApp",
          "inputs": {
             "apiVersion": "2015-01-14",
             "host": {
                "id": "/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                "gateway": "https://avdemo.azurewebsites.net"
             },
             "operation": "ListFiles",
             "parameters": {
                "FolderPath": "/myfolder"
             },
             "authentication": {
                "type": "Raw",
                "scheme": "Zumo",
                "parameter": "@parameters('/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
             }
          }
       }
    }
}
```

Теперь можно создать аналогичное действие HTTP и оставить раздел `parameters` с определением приложения логики без изменений, например:

``` json
"actions": {
   "dropboxconnector": {
      "type": "Http",
      "metadata": {
         "apiDefinitionUrl": "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
      },
      "inputs": {
         "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
         "method": "POST",
         "body": {
            "FolderPath": "/myfolder"
         },
         "authentication": {
            "type": "Raw",
            "scheme": "Zumo",
            "parameter": "@parameters('/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
         }
      }
   }
}
```

Вот описаний каждого из этих свойств:

| Свойство действия | Описание |
| --- | --- |
| `type` | `Http` вместо `APIapp` |
| `metadata.apiDefinitionUrl` | Чтобы использовать это действие в конструкторе приложений логики, включите конечную точку метаданных, которая состоит из следующих компонентов: `{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | Состоит из следующих компонентов: `{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Всегда `POST` |
| `inputs.body` | Совпадает с параметрами приложения API. |
| `inputs.authentication` | Совпадает с проверкой подлинности приложения API. |

Этот подход должен работать для всех действий приложений API. Обратите внимание, эти предыдущие приложения API больше не поддерживаются. Поэтому вы должны перейти на один из других вариантов: управляемый интерфейс API или размещение настраиваемого веб-API.

<a name="foreach"></a>

## <a name="renamed-repeat-to-foreach"></a>Переименование действия Repeat на Foreach

За время использования предыдущей версии схемы мы получили множество отзывов клиентов о том, что название действия **Repeat** приводит к путанице и не отражает того факта, что действие **Repeat** на самом деле представляет собой цикл for-each. Поэтому мы переименовали `repeat` в `foreach`. Ранее это действие нужно было написать так, как показано в следующем примере:

``` json
"actions": {
   "pingBing": {
      "type": "Http",
      "repeat": "@range(0,2)",
      "inputs": {
         "method": "GET",
         "uri": "https://www.bing.com/search?q=@{repeatItem()}"
      }
   }
}
```

Теперь следует использовать эту версию:

``` json
"actions": {
   "pingBing": {
      "type": "Http",
      "foreach": "@range(0,2)",
      "inputs": {
         "method": "GET",
         "uri": "https://www.bing.com/search?q=@{item()}"
      }
   }
}
```

Кроме того, функция `repeatItem()`, которая ссылалась на элемент, обрабатываемый в текущей итерации цикла for, теперь переименована в `item()`. 

### <a name="reference-outputs-from-foreach"></a>Эталонные выходные данные из Foreach

Для упрощения выходные данные действия `foreach` больше не упаковываются в объект с именем `repeatItems`. Эти изменения сопряжены с удалением функций `repeatItem()`, `repeatBody()` и `repeatOutputs()`.

Таким образом, с помощью предыдущего примера `repeat` можно было получить следующие выходные данные:

``` json
"repeatItems": [ {
   "name": "pingBing",
   "inputs": {
      "uri": "https://www.bing.com/search?q=0",
      "method": "GET"
   },
   "outputs": {
      "headers": { },
      "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"https://schemas.live.com/Web/\">...</html>"
   },
   "status": "Succeeded"
} ]
```

Теперь вы получите следующие выходные данные:

``` json
[ {
   "name": "pingBing",
      "inputs": {
         "uri": "https://www.bing.com/search?q=0",
         "method": "GET"
      },
      "outputs": {
         "headers": { },
         "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"https://schemas.live.com/Web/\">...</html>"
      },
      "status": "Succeeded"
} ]
```

Раньше, чтобы получить `body` из действия, ссылаясь на эти выходные данные, необходимо было сделать следующее:

``` json
"actions": {
   "secondAction": {
      "type": "Http",
      "repeat": "@outputs('pingBing').repeatItems",
      "inputs": {
         "method": "POST",
         "uri": "https://www.example.com",
         "body": "@repeatItem().outputs.body"
      }
   }
}
```

Теперь можно использовать следующую версию:

``` json
"actions": {
   "secondAction": {
      "type": "Http",
      "foreach": "@outputs('pingBing')",
      "inputs": {
         "method": "POST",
         "uri": "https://www.example.com",
         "body": "@item().outputs.body"
      }
   }
}
```

<a name="http-listener"></a>

## <a name="native-http-listener"></a>Встроенный прослушиватель HTTP

Функции прослушивателя HTTP теперь являются встроенными, поэтому больше не нужно развертывать приложение API прослушивателя HTTP. Дополнительные сведения см. в разделе [Настройка конечной точки HTTP для приложения логики](../logic-apps/logic-apps-http-endpoint.md). 

После внесения этих изменений в Logic Apps функция `@accessKeys()` была удалена и заменена функцией `@listCallbackURL()`, которая получает конечную точку при необходимости. Кроме того, теперь в приложении логики необходимо определить хотя бы один триггер. Если вы хотите запустить (`/run`) рабочий процесс, вам потребуется один из этих типов триггеров: `Manual`, `ApiConnectionWebhook` или `HttpWebhook`.

<a name="child-workflows"></a>

## <a name="call-child-workflows"></a>Вызов дочерних рабочих процессов

Ранее для вызова дочерних рабочих процессов нужно было перейти к рабочему процессу, получить токен доступа и вставить его в определение приложения логики, которое должно вызвать этот дочерний рабочий процесс. В этой схеме ядро Logic Apps автоматически создает SAS в среде выполнения для дочернего рабочего процесса. Поэтому вам не нужно вставлять секреты в определение. Вот пример: 

``` json
"myNestedWorkflow": {
   "type": "Workflow",
   "inputs": {
      "host": {
         "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/myWorkflow001",
         "triggerName": "myEndpointTrigger"
      },
      "queries": {
         "extrafield": "specialValue"
      },
      "headers": {
         "x-ms-date": "@utcnow()",
         "Content-type": "application/json"
      },
      "body": {
         "contentFieldOne": "value100",
         "anotherField": 10.001
      }
   },
   "conditions": []
}
```

Кроме того, дочерние рабочие процессы получают полный доступ ко входящим запросам. Таким образом, вы можете передавать параметры в разделе `queries` и в объекте `headers`. Можно также полностью определить весь раздел `body`.

Теперь у дочерних рабочих процессов имеются эти необходимые изменения. Раньше дочерний рабочий процесс можно было вызвать напрямую. Теперь, чтобы родительский рабочий процесс мог выполнить вызов, необходимо определить конечную точку триггера в рабочем процессе. В общих чертах это означает, что вам необходимо добавить триггер типа `Manual`, а затем использовать его в определении родительского рабочего процесса. Свойство `host` содержит элемент `triggerName`, так как всегда следует указывать вызываемый триггер.

## <a name="other-changes"></a>Другие изменения

### <a name="new-queries-property"></a>Новое свойство queries

Все типы действий теперь поддерживают новые входные данные, называемые `queries`. Эти входные данные могут быть структурированным объектом, который избавит вас от необходимости собирать строку вручную.

### <a name="renamed-parse-function-to-json"></a>Переименованные функции parse() на json

Теперь функция `parse()` переименована в функцию `json()` для будущих типов содержимого.

## <a name="enterprise-integration-apis"></a>Интерфейсы API для Корпоративной интеграции

Эта схема еще не поддерживает управляемые версии интерфейсов API для Корпоративной интеграции, например AS2. Тем не менее вы можете использовать существующие развернутые интерфейсы API BizTalk, используя действие HTTP. Дополнительные сведения см. в разделе об использовании уже развернутых приложений API в [схеме интеграции](https://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). 
