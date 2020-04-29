---
title: Настройка журналов диагностики Центров событий Azure | Документация Майкрософт
description: Узнайте, как настроить журналы действий и журналы диагностики для Центров событий в Azure.
keywords: ''
documentationcenter: ''
services: event-hubs
author: ShubhaVijayasarathy
manager: ''
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 68aa62ad34f8db531d439a581ef024862da0f90c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77162316"
---
# <a name="set-up-diagnostic-logs-for-an-azure-event-hub"></a>Настройка журналов диагностики для центра событий Azure

Для Центров событий Azure можно просмотреть журналы двух типов.

* **[Журналы действий.](../azure-monitor/platform/platform-logs-overview.md)** Эти журналы содержат сведения об операциях, выполняемых с заданием. Данные журналы всегда включены.
* **[Журналы диагностики.](../azure-monitor/platform/platform-logs-overview.md)** Вы можете настроить журналы диагностики, чтобы получать подробные сведения обо всем, что происходит с заданием. Журналы диагностики охватывают действия с момента создания задания до его удаления, включая обновления и действия, которые происходят во время выполнения задания.

## <a name="enable-diagnostic-logs"></a>Включение журналов диагностики

По умолчанию журналы диагностики отключены. Чтобы включить журналы диагностики, сделайте следующее:

1.  На [портале Azure](https://portal.azure.com) в разделе **Мониторинг и управление** щелкните **Журналы диагностики**.

    ![Панель навигации к журналам диагностики](./media/event-hubs-diagnostic-logs/image1.png)

2.  Выберите ресурс, который необходимо отслеживать.

3.  Щелкните **Включить диагностику**.

    ![Включение журналов диагностики](./media/event-hubs-diagnostic-logs/image2.png)

4.  Для параметра **Состояние** щелкните **Вкл**.

    ![Изменение состояния журналов диагностики](./media/event-hubs-diagnostic-logs/image3.png)

5.  Задайте нужный целевой объект архива. Например, учетная запись хранения, концентратор событий или журналы Azure Monitor.

6.  Сохраните новые параметры диагностики.

Новые параметры вступят в силу в течение 10 минут. После этого журналы появятся в настроенной цели для архивирования на панели **Журналы диагностики**.

Дополнительные сведения о настройке системы диагностики доступны в [обзоре журналов диагностики Azure](../azure-monitor/platform/platform-logs-overview.md).

## <a name="diagnostic-logs-categories"></a>Категории журналов диагностики

Центры событий записывают журналы диагностики двух категорий.

* **Журналы архивирования.** Журналы, связанные с архивами Центров событий, в частности, с ошибками архива.
* **Операционные журналы.** Сведения о том, что происходит во время операций в Центрах событий, в частности, тип операции, включая создание концентратора событий, используемые ресурсы, а также состояние операции.

## <a name="diagnostic-logs-schema"></a>Схема журналов диагностики

Все журналы хранятся в формате JSON (нотация объектов JavaScript). Каждая запись содержит строковые поля, использующие формат, описанный в следующих разделах.

### <a name="archive-logs-schema"></a>Схема архивных журналов

Строки JSON архивных журналов содержат элементы, перечисленные в следующей таблице.

Имя | Описание
------- | -------
TaskName | Описание задачи, завершившейся сбоем.
ActivityId | Внутренний идентификатор, используемый для отслеживания.
trackingId | Внутренний идентификатор, используемый для отслеживания.
resourceId | Идентификатор ресурса Azure Resource Manager.
eventHub | Полное имя концентратора событий (включает в себя имя пространства имен).
partitionId | Секция концентратора событий, в которую записываются данные.
archiveStep | ArchiveFlushWriter
startTime | Время начала сбоя.
failures | Количество произошедших сбоев.
durationInSeconds | Продолжительность сбоя.
сообщение | Сообщение об ошибке.
категория | ArchiveLogs

Ниже приведен пример строки JSON журнала архивирования.

```json
{
   "TaskName": "EventHubArchiveUserError",
   "ActivityId": "21b89a0b-8095-471a-9db8-d151d74ecf26",
   "trackingId": "21b89a0b-8095-471a-9db8-d151d74ecf26_B7",
   "resourceId": "/SUBSCRIPTIONS/854D368F-1828-428F-8F3C-F2AFFA9B2F7D/RESOURCEGROUPS/DEFAULT-EVENTHUB-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/FBETTATI-OPERA-EVENTHUB",
   "eventHub": "fbettati-opera-eventhub:eventhub:eh123~32766",
   "partitionId": "1",
   "archiveStep": "ArchiveFlushWriter",
   "startTime": "9/22/2016 5:11:21 AM",
   "failures": 3,
   "durationInSeconds": 360,
   "message": "Microsoft.WindowsAzure.Storage.StorageException: The remote server returned an error: (404) Not Found. ---> System.Net.WebException: The remote server returned an error: (404) Not Found.\r\n   at Microsoft.WindowsAzure.Storage.Shared.Protocol.HttpResponseParsers.ProcessExpectedStatusCodeNoException[T](HttpStatusCode expectedStatusCode, HttpStatusCode actualStatusCode, T retVal, StorageCommandBase`1 cmd, Exception ex)\r\n   at Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob.<PutBlockImpl>b__3e(RESTCommand`1 cmd, HttpWebResponse resp, Exception ex, OperationContext ctx)\r\n   at Microsoft.WindowsAzure.Storage.Core.Executor.Executor.EndGetResponse[T](IAsyncResult getResponseResult)\r\n   --- End of inner exception stack trace ---\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.StorageAsyncResult`1.End()\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.AsyncExtensions.<>c__DisplayClass4.<CreateCallbackVoid>b__3(IAsyncResult ar)\r\n--- End of stack trace from previous location where exception was thrown ---\r\n   at System.",
   "category": "ArchiveLogs"
}
```

### <a name="operational-logs-schema"></a>Схема операционных журналов

Строки JSON операционного журнала содержат элементы, перечисленные в приведенной ниже таблице.

Имя | Описание
------- | -------
ActivityId | Внутренний идентификатор, используемый для отслеживания.
EventName | Имя операции.  
resourceId | Идентификатор ресурса Azure Resource Manager.
SubscriptionId | Идентификатор подписки.
EventTimeString | Время операции.
EventProperties | Свойства операции.
Состояние | Состояние операции.
Caller | Объект, вызвавший операцию (портал Azure или клиент управления).
категория | OperationalLogs

Ниже приведен пример строки JSON операционного журнала.

```json
Example:
{
   "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
   "EventName": "Create EventHub",
   "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/SHOEBOXEHNS-CY4001",
   "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
   "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
   "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
   "Status": "Succeeded",
   "Caller": "ServiceBus Client",
   "category": "OperationalLogs"
}
```

## <a name="next-steps"></a>Дальнейшие шаги
- [Что такое Центры событий Azure?](event-hubs-what-is-event-hubs.md)
- [Общие сведения об API Центров событий](event-hubs-api-overview.md)
- Приступая к работе с Центрами событий
    - [.NET Core](get-started-dotnet-standard-send-v2.md)
    - [Java](get-started-java-send-v2.md)
    - [Python](get-started-python-send-v2.md)
    - [JavaScript](get-started-java-send-v2.md)
