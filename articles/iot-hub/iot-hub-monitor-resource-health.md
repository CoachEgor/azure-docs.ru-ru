---
title: Отслеживание работоспособности Центра Интернета вещей Azure | Документация Майкрософт
description: Использование Azure Monitor и службы "Работоспособность ресурсов Azure" для наблюдения за Центром Интернета вещей и быстрой диагностики неполадок
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/21/2020
ms.author: robinsh
ms.custom:
- amqp
- 'Role: Cloud Development'
- 'Role: Technical Support'
ms.openlocfilehash: 140bbc698db773f851cf03d80e8e51be9c2a21c3
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/28/2020
ms.locfileid: "87327554"
---
# <a name="monitor-the-health-of-azure-iot-hub-and-diagnose-problems-quickly"></a>Мониторинг работоспособности Центра Интернета вещей Azure и быстрая диагностика неполадок

Компании, внедряющие Центр Интернета вещей Azure, ожидают надежной работы ресурсов. Чтобы помочь вам внимательно отслеживать действия, Центр Интернета вещей полностью интегрирован с [Azure Monitor](../azure-monitor/index.yml) и службой [Работоспособность ресурсов Azure](../service-health/resource-health-overview.md). Эти две службы работают для предоставления данных, необходимых для обеспечения работоспособности решений Интернета вещей в рабочем состоянии.

Azure Monitor является единым источником мониторинга и ведения журнала для всех служб Azure. Вы можете отправить журналы диагностики, которые Azure Monitor создавать, для Azure Monitor журналов, концентраторов событий или службы хранилища Azure для пользовательской обработки. Параметры метрик и диагностик Azure Monitor позволяют отслеживать производительность ресурсов в режиме реального времени. Прочтите эту статью, чтобы узнать, как [пользоваться Azure Monitor](#use-azure-monitor) с помощью Центра Интернета вещей. 

> [!IMPORTANT]
> События, переданные службой центра IoT с использованием журналов диагностики Azure Monitor, не гарантируют надежность или упорядоченность. Некоторые события могут быть потеряны или доставлены не по порядку. Журналы диагностики также не предназначены для работы в режиме реального времени, и может потребоваться несколько минут для регистрации событий в месте назначения по вашему выбору.

Служба Работоспособность ресурсов Azure поможет выполнить диагностику и получить необходимую поддержку, если неполадки Azure влияют на ваши ресурсы. Панель мониторинга предоставляет текущее и предыдущее состояние работоспособности для каждого из центров Интернета вещей. Перейдите к разделу в нижней части этой статьи, чтобы узнать, как [использовать работоспособность ресурсов Azure](#use-azure-resource-health) с центром Интернета вещей. 

Также Центр Интернета вещей предоставляет собственные метрики, которые можно использовать для оценки состояния ресурсов Интернета вещей. Дополнительные сведения см. в статье [Общие сведения о метриках Центра Интернета вещей](iot-hub-metrics.md).

## <a name="use-azure-monitor"></a>Использование Azure Monitor

Azure Monitor предоставляет сведения диагностики для ресурсов Azure, что позволяет отслеживать операции в Центре Интернета вещей.

Дополнительные сведения о конкретных метриках и событиях, за которыми следит Azure Monitor, см. в статье [Метрики, поддерживаемые Azure Monitor](../azure-monitor/platform/metrics-supported.md) и [Поддерживаемые службы, схемы и категории для журналов диагностики Azure](../azure-monitor/platform/diagnostic-logs-schema.md).

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="understand-the-logs"></a>Общие сведения о журналах

Azure Monitor отслеживает различные операции, выполняемые в Центре Интернета вещей. Каждая категория имеет схему, определяющую, как выполняются отчеты о событиях в этой категории.

#### <a name="connections"></a>Соединения

Категория соединений отслеживает устройство, которое соединяет и отключает события из центра IoT, а также ошибки. Эта категория полезна для отслеживания несанкционированных попыток подключения и получения оповещений при потере подключения к устройствам.

> [!NOTE]
> Чтобы убедиться в состоянии надежного подключения устройств, проверьте [пульс устройства](iot-hub-devguide-identity-registry.md#device-heartbeat).

```json
{
   "records":
   [
        {
            "time": " UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "deviceConnect",
            "category": "Connections",
            "level": "Information",
            "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\",\"maskedIpAddress\":\"<maskedIpAddress>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="cloud-to-device-commands"></a>Отправка команд из облака на устройство

Категория отправки команд из облака на устройство отслеживает ошибки, которые возникают в Центре Интернета вещей и связаны с конвейером сообщений из облака на устройство. В эту категорию входят ошибки, возникающие в следующих ситуациях:

* отправка сообщений из облака на устройство (например, ошибка неавторизированного отправителя);
* получение сообщений из облака на устройство (например, ошибки превышения количества доставляемых сообщений);
* получение ответов на сообщение из облака на устройство (например, ошибки истечения срока действия для отзыва).

Эта категория не перехватывает ошибки, если сообщение из облака на устройстве было успешно доставлено, но неправильно обработано на устройстве.

```json
{
    "records":
    [
        {
            "time": " UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "messageExpired",
            "category": "C2DCommands",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "properties": "{\"deviceId\":\"<deviceId>\",\"messageId\":\"<messageId>\",\"messageSizeInBytes\":\"<messageSize>\",\"protocol\":\"Amqp\",\"deliveryAcknowledgement\":\"<None, NegativeOnly, PositiveOnly, Full>\",\"deliveryCount\":\"0\",\"expiryTime\":\"<timestamp>\",\"timeInSystem\":\"<timeInSystem>\",\"ttl\":<ttl>, \"EventProcessedUtcTime\":\"<UTC timestamp>\",\"EventEnqueuedUtcTime\":\"<UTC timestamp>\", \"maskedIpAddress\": \"<maskedIpAddress>\", \"statusCode\": \"4XX\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="device-identity-operations"></a>Операции с удостоверениями устройства

Категория операций с удостоверениями устройств отслеживает ошибки, возникающие, когда вы пытаетесь создать, обновить или удалить запись реестра удостоверений центра IoT. Отслеживание этой категории целесообразно для сценариев подготовки.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "get",
            "category": "DeviceIdentityOperations",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "properties": "{\"maskedIpAddress\":\"<maskedIpAddress>\",\"deviceId\":\"<deviceId>\", \"statusCode\":\"4XX\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="routes"></a>Маршруты

Категория [Маршрутизация сообщений](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c) отслеживает ошибки, происходящие во время оценки маршрута сообщения и работоспособности конечной точки, как показано в центре Интернета вещей. В эту категорию входят такие события, как:

* правило возвращает значение "не определено";
* Центр Интернета вещей помечает конечную точку как неработоспособную;
* любые ошибки, полученные от конечной точки.

Эта категория не содержит конкретных ошибок, связанных с самими сообщениями (например, ошибки регулирования устройств), которые попадают в категорию "телеметрия устройств".

```json
{
    "records":
    [
        {
            "time":"2019-12-12T03:25:14Z",
            "resourceId":"/SUBSCRIPTIONS/91R34780-3DEC-123A-BE2A-213B5500DFF0/RESOURCEGROUPS/ANON-TEST/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/ANONHUB1",
            "operationName":"endpointUnhealthy",
            "category":"Routes",
            "level":"Error",
            "resultType":"403004",
            "resultDescription":"DeviceMaximumQueueDepthExceeded",
            "properties":"{\"deviceId\":null,\"endpointName\":\"anon-sb-1\",\"messageId\":null,\"details\":\"DeviceMaximumQueueDepthExceeded\",\"routeName\":null,\"statusCode\":\"403\"}",
            "location":"westus"
        }
    ]
}
```

Ниже приведены дополнительные сведения о журналах диагностики для маршрутизации.

* [Список кодов ошибок журнала диагностики маршрутизации](troubleshoot-message-routing.md#diagnostics-error-codes)
* [Список журналов диагностики маршрутизации Оператионнамес](troubleshoot-message-routing.md#diagnostics-operation-names)

#### <a name="device-telemetry"></a>Телеметрия устройства

Категория телеметрии устройств отслеживает ошибки, которые возникают в центре IoT и связаны с конвейером телеметрии. В эту категорию входят ошибки, возникающие при отправке событий телеметрии (например: регулирование) и при получении событий телеметрии (например: неавторизованный модуль чтения). Эта категория не может перехватывать ошибки, которые вызваны кодом, выполняемым на самом устройстве.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "ingress",
            "category": "DeviceTelemetry",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "properties": "{\"deviceId\":\"<deviceId>\",\"batching\":\"0\",\"messageSizeInBytes\":\"<messageSizeInBytes>\",\"EventProcessedUtcTime\":\"<UTC timestamp>\",\"EventEnqueuedUtcTime\":\"<UTC timestamp>\",\"partitionId\":\"1\"}", 
            "location": "Resource location"
        }
    ]
}
```

#### <a name="file-upload-operations"></a>Операции отправки файлов

Категория передачи файлов позволяет отслеживать ошибки, которые возникают в центре IoT и связаны с функцией передачи файлов. В эту категорию входят:

* ошибки, связанные с универсальным кодом ресурса (URI) SAS (например, если срок его действия истекает до того, как устройство уведомит центр о завершении передачи);

* сбои передач, о которых сообщает устройство;

* ошибки, связанные с отсутствием файла в хранилище при создании уведомления для Центра Интернета вещей.

Обратите внимание, что эта категория не может перехватывать ошибки, которые возникают, когда устройство непосредственно передает файл в хранилище.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "ingress",
            "category": "FileUploadOperations",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "durationMs": "1",
            "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\",\"blobUri\":\"http//bloburi.com\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="cloud-to-device-twin-operations"></a>Операции переноса из облака на двойник устройства

Категория операций переноса из облака на двойник устройства отслеживает события, инициируемые службой, на двойнике устройства. Эти операции могут включать получение двойника, обновление или замещение тегов и необходимых свойств.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "read",
            "category": "C2DTwinOperations",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"deviceId\":\"<deviceId>\",\"sdkVersion\":\"<sdkVersion>\",\"messageSize\":\"<messageSize>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="device-to-cloud-twin-operations"></a>Операции переноса с двойника устройства в облако

Категория операций переноса с двойника устройства в облако отслеживает события, инициируемые устройством на двойниках устройства. Эти операции могут включать получение двойника, обновление сообщаемых свойств и подписку на необходимые свойства.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "update",
            "category": "D2CTwinOperations",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authenticationType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="twin-queries"></a>запросы к двойникам;

Категория запросов к двойникам сообщает о запросах к двойникам устройств, инициируемых в облаке.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "query",
            "category": "TwinQueries",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"query\":\"<twin query>\",\"sdkVersion\":\"<sdkVersion>\",\"messageSize\":\"<messageSize>\",\"pageSize\":\"<pageSize>\", \"continuation\":\"<true, false>\", \"resultSize\":\"<resultSize>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="jobs-operations"></a>Операции заданий

Категория операций заданий сообщает о запросах задания по обновлению двойников устройства или вызову прямого метода на нескольких устройствах. Эти запросы инициируются в облаке.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "jobCompleted",
            "category": "JobsOperations",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"jobId\":\"<jobId>\", \"sdkVersion\": \"<sdkVersion>\",\"messageSize\": <messageSize>,\"filter\":\"DeviceId IN ['1414ded9-b445-414d-89b9-e48e8c6285d5']\",\"startTimeUtc\":\"Wednesday, September 13, 2017\",\"duration\":\"0\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="direct-methods"></a>Прямые методы

Категория прямых методов отслеживает взаимодействия типа "запрос — ответ", отправленные на отдельные устройства. Эти запросы инициируются в облаке.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "send",
            "category": "DirectMethods",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"deviceId\":<messageSize>, \"RequestSize\": 1, \"ResponseSize\": 1, \"sdkVersion\": \"2017-07-11\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="distributed-tracing-preview"></a>Распределенная трассировка (предварительная версия)

Категория распределенной трассировки позволяет отследить идентификаторы корреляции для сообщений, содержащих заголовок контекста трассировки. Чтобы полностью включить эти журналы, код на стороне клиента должен быть обновлен путем [анализа и диагностики приложений IOT в полной мере с помощью распределенной трассировки центра Интернета вещей (Предварительная версия)](iot-hub-distributed-tracing.md).

Обратите внимание, что `correlationId` соответствует предложению [контекста трассировки W3C](https://github.com/w3c/trace-context) , где оно содержит, `trace-id` а также `span-id` .

##### <a name="iot-hub-d2c-device-to-cloud-logs"></a>Журналы D2C (с устройства в облако) в Центре Интернета вещей

В Центре Интернета вещей в этот журнал записывается информация при получении сообщения, содержащего допустимые свойства трассировки.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "DiagnosticIoTHubD2C",
            "category": "DistributedTracing",
            "correlationId": "00-8cd869a412459a25f5b4f31311223344-0144d2590aacd909-01",
            "level": "Information",
            "resultType": "Success",
            "resultDescription":"Receive message success",
            "durationMs": "",
            "properties": "{\"messageSize\": 1, \"deviceId\":\"<deviceId>\", \"callerLocalTimeUtc\": : \"2017-02-22T03:27:28.633Z\", \"calleeLocalTimeUtc\": \"2017-02-22T03:27:28.687Z\"}",
            "location": "Resource location"
        }
    ]
}
```

В этом случае `durationMs` не подсчитывается, так как часы Центра Интернета вещей могут быть не синхронизированы с часами устройства, из-за чего расчет длительности может давать ошибочные результаты. Рекомендуется создавать логику с использованием метки времени в разделе `properties`, чтобы записывать пики задержки соединения устройства с облаком.

| Свойство | Тип | Описание |
|--------------------|-----------------------------------------------|------------------------------------------------------------------------------------------------|
| **messageSize** | Тип Integer | Размер сообщения, поступающего с устройства в облако, в байтах |
| **deviceId** | Строка 7-битовых букв и цифр ASCII | Удостоверение устройства |
| **callerLocalTimeUtc** | Метка времени в формате UTC | Время создания сообщения, измеренное локальными часами устройства |
| **calleeLocalTimeUtc** | Метка времени в формате UTC | Время поступления сообщения в шлюз Центра Интернета вещей, измеренное часами на стороне службы Центра Интернета вещей |

##### <a name="iot-hub-ingress-logs"></a>Журналы входящего трафика Центра Интернета вещей

В Центре Интернета вещей в этом журнале фиксируется информация, когда сообщение, содержащее допустимые свойства трассировки записывается во внутренний или встроенный концентратор событий.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "DiagnosticIoTHubIngress",
            "category": "DistributedTracing",
            "correlationId": "00-8cd869a412459a25f5b4f31311223344-349810a9bbd28730-01",
            "level": "Information",
            "resultType": "Success",
            "resultDescription":"Ingress message success",
            "durationMs": "10",
            "properties": "{\"isRoutingEnabled\": \"true\", \"parentSpanId\":\"0144d2590aacd909\"}",
            "location": "Resource location"
        }
    ]
}
```

В `properties` разделе этот журнал содержит дополнительные сведения о входящих сообщениях.

| Свойство | Тип | Описание |
|--------------------|-----------------------------------------------|------------------------------------------------------------------------------------------------|
| **isRoutingEnabled** | Тип String | Значение true или false показывает, включена ли маршрутизация сообщений в Центре Интернета вещей |
| **parentSpanId** | Тип String | [Span-id](https://w3c.github.io/trace-context/#parent-id) родительского сообщения, которое в этом случае является трассировкой сообщения, передаваемого из устройства в облако (D2C) |

##### <a name="iot-hub-egress-logs"></a>Журналы исходящего трафика Центра Интернета вещей

В Центре Интернета вещей в этом журнале фиксируется информация, когда включена [маршрутизация](iot-hub-devguide-messages-d2c.md) и сообщение записывается для [конечной точки](iot-hub-devguide-endpoints.md). Если маршрутизация не включена, в Центре Интернета вещей не фиксируется информация в этом журнале.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "DiagnosticIoTHubEgress",
            "category": "DistributedTracing",
            "correlationId": "00-8cd869a412459a25f5b4f31311223344-98ac3578922acd26-01",
            "level": "Information",
            "resultType": "Success",
            "resultDescription":"Egress message success",
            "durationMs": "10",
            "properties": "{\"endpointType\": \"EventHub\", \"endpointName\": \"myEventHub\", \"parentSpanId\":\"349810a9bbd28730\"}",
            "location": "Resource location"
        }
    ]
}
```

В `properties` разделе этот журнал содержит дополнительные сведения о входящих сообщениях.

| Свойство | Тип | Описание |
|--------------------|-----------------------------------------------|------------------------------------------------------------------------------------------------|
| **endpointName** | Тип String | Имя конечной точки маршрутизации |
| **endpointType** | Тип String | Тип конечной точки маршрутизации |
| **parentSpanId** | Тип String | [Span-id](https://w3c.github.io/trace-context/#parent-id) родительского сообщения, которое в этом случае является трассировкой сообщения входящего трафика Центра Интернета вещей |

#### <a name="configurations"></a>Конфигурации

Журналы конфигурации центра Интернета вещей отслеживают события и ошибки для набора функций автоматического управления устройствами.

```json
{
    "records":
    [
         {
             "time": "2019-09-24T17:21:52Z",
             "resourceId": "Resource Id",
             "operationName": "ReadManyConfigurations",
             "category": "Configurations",
             "resultType": "",
             "resultDescription": "",
             "level": "Information",
             "durationMs": "17",
             "properties": "{\"configurationId\":\"\",\"sdkVersion\":\"2018-06-30\",\"messageSize\":\"0\",\"statusCode\":null}",
             "location": "southcentralus"
         }
    ]
}
```

### <a name="device-streams-preview"></a>Потоки устройств (Предварительная версия)

Категория "потоки устройств" отслеживает взаимодействия "запрос-ответ", отправляемые на отдельные устройства.

```json
{
    "records":
    [
         {
             "time": "2019-09-19T11:12:04Z",
             "resourceId": "Resource Id",
             "operationName": "invoke",
             "category": "DeviceStreams",
             "resultType": "",
             "resultDescription": "",    
             "level": "Information",
             "durationMs": "74",
             "properties": "{\"deviceId\":\"myDevice\",\"moduleId\":\"myModule\",\"sdkVersion\":\"2019-05-01-preview\",\"requestSize\":\"3\",\"responseSize\":\"5\",\"statusCode\":null,\"requestName\":\"myRequest\",\"direction\":\"c2d\"}",
             "location": "Central US"
         }
    ]
}
```

### <a name="read-logs-from-azure-event-hubs"></a>Чтение журналов из Центров событий Azure

После настройки ведения журнала событий в параметрах диагностики можно создавать приложения, которые считывают журналы, что позволяет выполнять действия на основе информации, содержащейся в них. В этом примере кода журналы извлекаются из концентратора событий:

```csharp
class Program
{ 
    static string connectionString = "{your AMS eventhub endpoint connection string}";
    static string monitoringEndpointName = "{your AMS event hub endpoint name}";
    static EventHubClient eventHubClient;
    //This is the Diagnostic Settings schema
    class AzureMonitorDiagnosticLog
    {
        string time { get; set; }
        string resourceId { get; set; }
        string operationName { get; set; }
        string category { get; set; }
        string level { get; set; }
        string resultType { get; set; }
        string resultDescription { get; set; }
        string durationMs { get; set; }
        string callerIpAddress { get; set; }
        string correlationId { get; set; }
        string identity { get; set; }
        string location { get; set; }
        Dictionary<string, string> properties { get; set; }
    };

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName);
        var d2cPartitions = eventHubClient.GetRuntimeInformationAsync().PartitionIds;
        CancellationTokenSource cts = new CancellationTokenSource();
        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
            tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }
        Console.ReadLine();
        Console.WriteLine("Exiting...");
        cts.Cancel();
        Task.WaitAll(tasks.ToArray());
    }

    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested)
            {
                await eventHubReceiver.CloseAsync();
                break;
            }
            EventData eventData = await eventHubReceiver.ReceiveAsync(new TimeSpan(0,0,10));
            if (eventData != null)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
                var deserializer = new JavaScriptSerializer();
                //deserialize json data to azure monitor object
                AzureMonitorDiagnosticLog message = new JavaScriptSerializer().Deserialize<AzureMonitorDiagnosticLog>(result);
            }
        }
    }
}
```

## <a name="use-azure-resource-health"></a>Использование службы "Работоспособность ресурса Azure"

Используйте службу "Работоспособность ресурсов Azure", чтобы отследить, запущен ли ваш Центр Интернета вещей и влияет ли региональный сбой на его работоспособность. Чтобы узнать конкретные сведения о состоянии работоспособности Центра Интернета вещей Azure, ознакомьтесь с разделом [Использование Azure Monitor](#use-azure-monitor).

Центр Интернета вещей Azure указывает состояние работоспособности на региональном уровне. Если региональный сбой влияет на Центр Интернета вещей, отображается состояние работоспособности **Неизвестно**. Дополнительные сведения см. в статье [Типы ресурсов и проверки работоспособности в службе работоспособности ресурсов Azure](../service-health/resource-health-checks-resource-types.md).

Чтобы проверить работоспособность Центра Интернета вещей, сделайте следующее:

1. Войдите на [портал Azure](https://portal.azure.com).

2. Перейдите к службе работоспособность ресурсов **работоспособности служб**  >  **Resource health**.

3. В раскрывающихся списках выберите подписку, а затем в качестве типа ресурса выберите **центр Интернета вещей** .

Дополнительные сведения о том, как интерпретировать данные о работоспособности, см. в статье [Обзор работоспособности ресурсов Azure](../service-health/resource-health-overview.md).

## <a name="next-steps"></a>Дальнейшие действия

* [Общие сведения о метриках Центра Интернета вещей](iot-hub-metrics.md)
* [Удаленный мониторинг и отправка уведомлений в Центре Интернета вещей с помощью службы Azure Logic Apps, обеспечивающей подключение между Центром Интернета вещей и почтовым ящиком](iot-hub-monitoring-notifications-with-azure-logic-apps.md)
