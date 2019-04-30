---
title: Использование службы "Сетка событий Azure" с событиями в схеме CloudEvents
description: В этой статье описана установка схемы CloudEvents для событий в службе "Сетка событий Azure".
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 11/07/2018
ms.author: babanisa
ms.openlocfilehash: 0195ce82396a7b05335242a38a2881e1b2d1afb3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61436612"
---
# <a name="use-cloudevents-schema-with-event-grid"></a>Использование схемы CloudEvents со службой "Сетка событий"

В дополнение к [схеме событий по умолчанию](event-schema.md), служба "Сетка событий Azure" поддерживает события в [схеме CloudEvents JSON](https://github.com/cloudevents/spec/blob/master/json-format.md). [CloudEvents](https://cloudevents.io/) — [открытая спецификация](https://github.com/cloudevents/spec/blob/master/spec.md) для описания данных о событиях.

CloudEvents упрощает взаимодействие, предоставляя общую схему событий для публикации и использования событий на основе облака. Эта схема предоставляет обычные средства, стандартные способы маршрутизации и обработки событий, а также универсальные методы десериализации внешней схемы событий. Общая схема позволяет легко интегрировать работу на разных платформах.

Создание CloudEvents — это [совместная работа](https://github.com/cloudevents/spec/blob/master/community/contributors.md) нескольких компаний, включая корпорацию Майкрософт и компанию [Cloud Native Computing Foundation](https://www.cncf.io/). В настоящее время доступна версия 0.1.

В этой статье описывается использование схемы CloudEvents со службой "Сетка событий".

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

## <a name="install-preview-feature"></a>Установка предварительной версии функции

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="cloudevent-schema"></a>Схема CloudEvent

Ниже приведен пример события хранилища BLOB-объектов Azure в формате CloudEvents.

``` JSON
{
    "cloudEventsVersion" : "0.1",
    "eventType" : "Microsoft.Storage.BlobCreated",
    "eventTypeVersion" : "",
    "source" : "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-account}#blobServices/default/containers/{storage-container}/blobs/{new-file}",
    "eventID" : "173d9985-401e-0075-2497-de268c06ff25",
    "eventTime" : "2018-04-28T02:18:47.1281675Z",
    "data" : {
      "api": "PutBlockList",
      "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
      "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
      "eTag": "0x8D4BCC2E4835CD0",
      "contentType": "application/octet-stream",
      "contentLength": 524288,
      "blobType": "BlockBlob",
      "url": "https://oc2d2817345i60006.blob.core.windows.net/oc2d2817345i200097container/oc2d2817345i20002296blob",
      "sequencer": "00000000000004420000000000028963",
      "storageDiagnostics": {
        "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
      }
    }
}
```

Для CloudEvents v0.1 доступны следующие свойства.

| CloudEvents        | type     | Пример значения JSON             | Описание                                                        | Сопоставление службы "Сетка событий"
|--------------------|----------|--------------------------------|--------------------------------------------------------------------|-------------------------
| eventType          | String   | "com.example.someevent"          | Тип вхождений                                   | eventType
| eventTypeVersion   | String   | "1.0"                            | Версия eventType (необязательно)                            | dataVersion
| cloudEventsVersion | String   | "0.1"                            | Версия спецификации CloudEvents, которую использует событие        | *сквозное*
| источник             | URI      | "/mycontext"                     | Описание источника событий                                       | topic#subject
| eventID            | String   | "1234-1234-1234"                 | Идентификатор события                                                    | идентификатор
| eventTime          | Timestamp| "2018-04-05T17:31:00Z"           | Метка времени, когда произошло событие (необязательно)                    | eventTime
| schemaURL          | URI      | "https:\//myschema.com"           | Ссылка на схему, к которой привязан атрибут данных (необязательно) | *не используется*
| contentType        | String   | "application/json"               | Описание формата кодирования данных (необязательно)                       | *не используется*
| extensions         | Карта      | { "extA": "vA", "extB", "vB" }  | Любые дополнительные метаданные (необязательно)                                 | *не используется*
| данные               | Object   | { "objA": "vA", "objB", "vB" }  | Полезные данные события (необязательно)                                       | данные

Дополнительные сведения см. в разделе [Context Attributes](https://github.com/cloudevents/spec/blob/master/spec.md#context-attributes) (Атрибуты контекста).

Значения заголовков для событий, доставляемых в схеме CloudEvents и схеме "Сетка событий", совпадают, за исключением элемента `content-type`. Для схемы CloudEvents значение заголовка — `"content-type":"application/cloudevents+json; charset=utf-8"`. Для схемы "Сетка событий" значение заголовка — `"content-type":"application/json; charset=utf-8"`.

## <a name="configure-event-grid-for-cloudevents"></a>Настройка сетки событий для CloudEvents

Службу "Сетка событий" можно использовать для ввода и вывода событий в схеме CloudEvents. Кроме того, CloudEvents можно использовать для системных событий, таких как события хранилища BLOB-объектов и Центра Интернета вещей, а также настраиваемые события. CloudEvents также может преобразовать эти события (в другой формат и обратно) при передаче по сети.


| Схема ввода       | Схема вывода
|--------------------|---------------------
| Формат CloudEvents | Формат CloudEvents
| Формат сетки событий  | Формат CloudEvents
| Формат CloudEvents | Формат сетки событий
| Формат сетки событий  | Формат сетки событий

Для всех схем событий служба "Сетка событий" требует проверки при публикации в разделе сетки событий и создании подписки на события. Дополнительные сведения см. в разделе [Сетка событий: безопасность и проверка подлинности](security-authentication.md).

### <a name="input-schema"></a>Схема ввода

Схема ввода для пользовательского раздела указывается при создании этого раздела.

Для интерфейса командной строки Azure:

```azurecli-interactive
# If you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  --name <topic_name> \
  -l westcentralus \
  -g gridResourceGroup \
  --input-schema cloudeventv01schema
```

Для PowerShell используйте команду:

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

New-AzureRmEventGridTopic `
  -ResourceGroupName gridResourceGroup `
  -Location westcentralus `
  -Name <topic_name> `
  -InputSchema CloudEventV01Schema
```

Текущая версия CloudEvents не поддерживает пакетную обработку событий. При публикации в разделе с помощью схемы CloudEvent необходимо публиковать каждое событие отдельно.

### <a name="output-schema"></a>Схема вывода

Схема вывода указывается при создании подписки на событие.

Для интерфейса командной строки Azure:

```azurecli-interactive
topicID=$(az eventgrid topic show --name <topic-name> -g gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --name <event_subscription_name> \
  --source-resource-id $topicID \
  --endpoint <endpoint_URL> \
  --event-delivery-schema cloudeventv01schema
```

Для PowerShell используйте команду:
```azurepowershell-interactive
$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name <topic-name>).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -DeliverySchema CloudEventV01Schema
```

Текущая версия CloudEvents не поддерживает пакетную обработку событий. Подписка на события, настроенная для схемы CloudEvent, получает каждое событие отдельно. Сейчас, если событие доставляется в схему CloudEvents, триггер службы "Сетка событий" нельзя использовать в приложении "Функции Azure". Используйте триггер HTTP. Примеры реализации триггера HTTP, который получает события в схеме CloudEvents, см. в разделе [Использование триггера HTTP в качестве триггера службы "Сетка событий"](../azure-functions/functions-bindings-event-grid.md#use-an-http-trigger-as-an-event-grid-trigger).

## <a name="next-steps"></a>Дальнейшие действия

* Ознакомьтесь со сведениями о [мониторинге доставки сообщений в службе "Сетка событий"](monitor-event-delivery.md).
* Мы рекомендуем вам протестировать CloudEvents и [внести свой вклад](https://github.com/cloudevents/spec/blob/master/CONTRIBUTING.md) в усовершенствование схемы, оставляя комментарии.
* Дополнительные сведения о создании подписки на Сетку событий Azure см. в статье [Схема подписки для службы "Сетка событий"](subscription-creation-schema.md).
