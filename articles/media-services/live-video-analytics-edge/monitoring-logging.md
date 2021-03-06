---
title: Мониторинг и ведение журналов — Azure
description: В этой статье представлен обзор функции Live Video Analytics по IoT Edge мониторингу и ведению журнала.
ms.topic: reference
ms.date: 04/27/2020
ms.openlocfilehash: 8ae455a4157cd649f610620e486323ac2c0a5744
ms.sourcegitcommit: cc13f3fc9b8d309986409276b48ffb77953f4458
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97401055"
---
# <a name="monitoring-and-logging"></a>Мониторинг и ведение журнала

В этой статье вы узнаете, как получать события из службы "интерактивная аналитика видео" в модуле IoT Edge для удаленного мониторинга. 

Кроме того, вы узнаете, как управлять журналами, создаваемыми модулем.

## <a name="taxonomy-of-events"></a>Классификация событий

Интерактивная аналитика видео на IoT Edge выдает события или данные телеметрии в соответствии со следующей классификацией.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/telemetry-schema/taxonomy.png" alt-text="Классификация событий":::

* Операционные: события, которые создаются как часть действий, выполненных пользователем, или во время выполнения [графа мультимедиа](media-graph-concept.md).
   
   * Том: ожидается низкий (несколько раз в минуту или даже более низкая ставка).
   * Примеры:

      Запись начата (ниже), запись остановлена
      
      ```
      {
        "body": {
          "outputType": "assetName",
          "outputLocation": "sampleAssetFromEVR-LVAEdge-20200512T233309Z"
        },
        "applicationProperties": {
          "topic": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<my-resource-group>/providers/microsoft.media/mediaservices/<ams-account-name>",
          "subject": "/graphInstances/Sample-Graph-2/sinks/assetSink",
          "eventType": "Microsoft.Media.Graph.Operational.RecordingStarted",
          "eventTime": "2020-05-12T23:33:10.392Z",
          "dataVersion": "1.0"
        }
      }
      ```
* Диагностика: события, помогающие диагностировать проблемы и/или проблемы с производительностью.

   * Том: может быть высокой (несколько раз в минуту).
   * Примеры:
   
      Сведения о RTSP [SDP](https://en.wikipedia.org/wiki/Session_Description_Protocol) (ниже) или пропуски в входящем видеоканале.

      ```
      {
        "body": {
          "sdp": "SDP:\nv=0\r\no=- 1589326384077235 1 IN IP4 XXX.XX.XX.XXX\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/lots_015.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.04.12\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-73.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/lots_015.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=640028;sprop-parameter-sets=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\r\na=control:track1\r\n"
        },
        "applicationProperties": {
          "topic": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<my-resource-group>/providers/microsoft.media/mediaservices/<ams-account-name>",
          "subject": "/graphInstances/Sample-Graph-2/sources/rtspSource",
          "eventType": "Microsoft.Media.Graph.Diagnostics.MediaSessionEstablished",
          "eventTime": "2020-05-12T23:33:04.077Z",
          "dataVersion": "1.0"
        }
      }
      ```
* Аналитика — события, создаваемые в ходе анализа видео.

   * Том: может быть высокой (несколько раз в минуту или чаще).
   * Примеры:
      
      Обнаружено перемещение (ниже), результат вывода.

   ```      
   {
     "body": {
       "timestamp": 143039375044290,
       "inferences": [
         {
           "type": "motion",
           "motion": {
             "box": {
               "l": 0.48954,
               "t": 0.140741,
               "w": 0.075,
               "h": 0.058824
             }
           }
         }
       ]
     },
     "applicationProperties": {
       "topic": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<my-resource-group>/providers/microsoft.media/mediaservices/<ams-account-name>",
       "subject": "/graphInstances/Sample-Graph-2/processors/md",
       "eventType": "Microsoft.Media.Graph.Analytics.Inference",
       "eventTime": "2020-05-12T23:33:09.381Z",
       "dataVersion": "1.0"
     }
   }
   ```

События, порожденные модулем, отправляются в [центр IOT Edge](../../iot-edge/iot-edge-runtime.md#iot-edge-hub), откуда их можно направлять в другие назначения. 

### <a name="timestamps-in-analytic-events"></a>Метки времени в событиях аналитики

Как указано выше, события, созданные как часть анализа видео, имеют связанную с ними отметку времени. Если вы [записали в реальном времени видео](video-recording-concept.md) в качестве части топологии графа, эта метка времени поможет вам узнать, где в записанном видео произошло определенное событие. Ниже приведены рекомендации по сопоставлению метки времени в событии аналитики с временной шкалой видео, записанном в [ресурс службы мультимедиа Azure](terminology.md#asset).

Сначала извлеките `eventTime` значение. Используйте это значение в [фильтре диапазона времени](playback-recordings-how-to.md#time-range-filters) , чтобы получить подходящую часть записи. Например, может потребоваться получить видео, которое начнется через 30 секунд до `eventTime` и заканчивается 30 секунд. В приведенном выше примере, где `eventTime` — 2020-05-12T23:33:09.381 z, запрос манифеста HLS для окна +/-30 будет выглядеть следующим образом:

```
https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=m3u8-aapl,startTime=2020-05-12T23:32:39Z,endTime=2020-05-12T23:33:39Z).m3u8
```

Приведенный выше URL-адрес вернет так называемый [главный список воспроизведения](https://developer.apple.com/documentation/http_live_streaming/example_playlists_for_http_live_streaming), содержащий URL-адреса для списков воспроизведения мультимедиа. Список воспроизведения мультимедиа будет содержать записи следующего вида:

```
...
#EXTINF:3.103011,no-desc
Fragments(video=143039375031270,format=m3u8-aapl)
...
```
В приведенной выше записи сообщает о наличии фрагмента видео, который начинается с отметки времени `143039375031270` . `timestamp`Значение в событии аналитики использует ту же шкалу, что и список воспроизведения мультимедиа, и может использоваться для указания соответствующего фрагмента видео и поиска в нужном кадре.

Дополнительные сведения см. в одном из многих [статей](https://www.bing.com/search?q=frame+accurate+seeking+in+HLS) по точному поиску в виде кадров в HLS.

## <a name="controlling-events"></a>Управление событиями

Вы можете использовать следующие свойства модуля двойника, как описано в статье [ДВОЙНИКА JSON Schema](module-twin-configuration-schema.md), чтобы управлять операционными и диагностическими событиями, опубликованными в реальном видео Analytics в модуле IOT Edge.

`diagnosticsEventsOutputName` — включить и предоставить (любое) значение для этого свойства, чтобы получать диагностические события из модуля. Удалите его или оставьте пустым, чтобы модуль не подводил публикацию диагностических событий.
   
`operationalEventsOutputName` — включить и предоставить (любое) значение для этого свойства, чтобы получить операционные события из модуля. Удалите его или оставьте пустым, чтобы модуль не подводил публикацию операционных событий.
   
События аналитики создаются такими узлами, как процессор обнаружения движения или обработчиком расширений HTTP, и приемником центра Интернета вещей для отправки их в центр IoT Edge. 

Вы можете управлять [маршрутизацией всех указанных выше событий](../../iot-edge/module-composition.md#declare-routes) с помощью требуемого свойства $edgeHub модуля двойника (в манифесте развертывания):

```
 "$edgeHub": {
   "properties.desired": {
     "schemaVersion": "1.0",
     "routes": {
       "moduleToHub": "FROM /messages/modules/lvaEdge/outputs/* INTO $upstream"
     },
     "storeAndForwardConfiguration": {
       "timeToLiveSecs": 7200
     }
   }
 }
```

В приведенном выше примере Лваедже — это имя для аналитики в реальном времени в модуле IoT Edge, а правило маршрутизации соответствует схеме, определенной в [Declare Routes](../../iot-edge/module-composition.md#declare-routes).

> [!NOTE]
> Чтобы обеспечить доступность событий аналитики в центре IoT Edge, необходимо быть узлом приемника центра Интернета вещей, расположенным на любом узле процессора для обнаружения движения и (или) любым узлом обработчика HTTP-расширений.

## <a name="event-schema"></a>Схема событий

События происходят на пограничном устройстве, и их можно использовать на границе или в облаке. События, создаваемые функцией Live Video Analytics на IoT Edge, соответствуют [шаблону обмена сообщениями с потоковой передачей](../../iot-hub/iot-hub-devguide-messages-construct.md) , установленному центром Интернета вещей Azure, со свойствами системы, свойствами приложения и телом.

### <a name="summary"></a>Сводка

Каждое событие, наблюдаемое через центр Интернета вещей, будет иметь набор общих свойств, как описано ниже.

|Свойство.   |Тип свойства| Тип данных   |Описание|
|---|---|---|---|
|message-id |система |guid|  Уникальный идентификатор события.|
|Раздел| аппликатионпроперти |строка|    Azure Resource Manager путь к учетной записи служб мультимедиа.|
|subject|   аппликатионпроперти |строка|    Вложенный путь к сущности, в которой создается событие.|
|eventTime| аппликатионпроперти|    строка| Время создания события.|
|eventType| аппликатионпроперти |строка|    Идентификатор типа события (см. ниже).|
|текст|текст  |объект|    Данные определенного события.|
|dataVersion    |аппликатионпроперти|   строка  |{Major}. Дополнительный|

### <a name="properties"></a>Свойства

#### <a name="message-id"></a>message-id

Глобальный уникальный идентификатор события (GUID)

#### <a name="topic"></a>Раздел

Представляет учетную запись службы мультимедиа Azure, связанную с графом.

`/subscriptions/{subId}/resourceGroups/{rgName}/providers/Microsoft.Media/mediaServices/{accountName}`

#### <a name="subject"></a>subject

Сущность, в которой создается событие:

`/graphInstances/{graphInstanceName}`<br/>
`/graphInstances/{graphInstanceName}/sources/{sourceName}`<br/>
`/graphInstances/{graphInstanceName}/processors/{processorName}`<br/>
`/graphInstances/{graphInstanceName}/sinks/{sinkName}`

Свойство Subject позволяет сопоставлять универсальные события с соответствующим им модулем. Например, в случае недопустимого имени пользователя или пароля RTSP созданное событие будет находиться `Microsoft.Media.Graph.Diagnostics.ProtocolError` на `/graphInstances/myGraph/sources/myRtspSource` узле.

#### <a name="event-types"></a>Типы событий

Типы событий присваиваются пространству имен в соответствии со следующей схемой:

`Microsoft.Media.Graph.{EventClass}.{EventType}`

#### <a name="event-classes"></a>Классы событий

|Имя класса|Описание|
|---|---|
|Analytics  |События, создаваемые как часть анализа содержимого.|
|Диагностика    |События, помогающие в диагностике проблем и производительности.|
|Операционный    |События, создаваемые как часть операции с ресурсами.|

Типы событий относятся к каждому классу событий.

Примеры:

* Microsoft. Media. Graph. Analytics. вывод
* Microsoft. Media. Graph. Diagnostics. AuthorizationError
* Microsoft. Media. Graph. Рабочее. Графинстанцестартед

### <a name="event-time"></a>Время события

Время события описывается в строке ISO8601 и в момент возникновения события.

### <a name="azure-monitor-collection-using-telegraf"></a>Azure Monitor коллекции с помощью Telegraf

Эти метрики будут сообщать о модуле Live Video Analytics в IoT Edge модуль:  

|Имя метрики|Тип|Метка|Описание|
|-----------|----|-----|-----------|
|lva_active_graph_instances|Датчик|iothub, edge_device, module_name, graph_topology|Общее число активных графиков на топологию.|
|lva_received_bytes_total|Счетчик|iothub, edge_device, module_name, graph_topology, graph_instance, graph_node|Общее число байтов, полученных узлом. Поддерживается только для источников RTSP|
|lva_data_dropped_total|Счетчик|iothub, edge_device, module_name, graph_topology, graph_instance, graph_node, data_kind|Счетчик любых удаленных данных (событий, носителей и т. д.)|

> [!NOTE]
> [Конечная точка Prometheus](https://prometheus.io/docs/practices/naming/) предоставляется по порту **9600** контейнера. Если вы направите свое имя в Live Video Analytics на IoT Edge модуле "Лваедже", он сможет получить доступ к метрикам, отправив запрос GET в http://lvaEdge:9600/metrics .   

Выполните следующие действия, чтобы включить сбор метрик из интерактивной аналитики видео в модуле IoT Edge:

1. Создайте папку на компьютере разработки и перейдите к этой папке.

1. В этой папке создайте `telegraf.toml` файл со следующим содержимым.
    ```
    [agent]
        interval = "30s"
        omit_hostname = true

    [[inputs.prometheus]]
      metric_version = 2
      urls = ["http://edgeHub:9600/metrics", "http://edgeAgent:9600/metrics", "http://{LVA_EDGE_MODULE_NAME}:9600/metrics"]

    [[outputs.azure_monitor]]
      namespace_prefix = ""
      region = "westus"
      resource_id = "/subscriptions/{SUBSCRIPTON_ID}/resourceGroups/{RESOURCE_GROUP}/providers/Microsoft.Devices/IotHubs/{IOT_HUB_NAME}"
    ```
    > [!IMPORTANT]
    > Убедитесь, что переменные (помеченные `{ }` ) в файле содержимого заменены.

1. В этой папке создайте объект `.dockerfile` со следующим содержимым.
    ```
        FROM telegraf:1.15.3-alpine
        COPY telegraf.toml /etc/telegraf/telegraf.conf
    ```

1. Теперь с помощью команды DOCKER CLI **Создайте файл DOCKER** и опубликуйте образ в реестре контейнеров Azure.
    1. Узнайте, как [отправлять и запрашивать образы DOCKER в реестре контейнеров Azure](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-docker-cli).  Дополнительные сведения о реестре контейнеров Azure (запись контроля доступа) можно найти [здесь](https://docs.microsoft.com/azure/container-registry/).


1. После завершения отправки в запись контроля доступа добавьте в файл манифеста развертывания следующий узел:
    ```
    "telegraf": 
    {
      "settings": 
        {
            "image": "{ACR_LINK_TO_YOUR_TELEGRAF_IMAGE}"
        },
      "type": "docker",
      "version": "1.0",
      "status": "running",
      "restartPolicy": "always",
      "env": 
        {
            "AZURE_TENANT_ID": { "value": "{YOUR_TENANT_ID}" },
            "AZURE_CLIENT_ID": { "value": "{YOUR CLIENT_ID}" },
            "AZURE_CLIENT_SECRET": { "value": "{YOUR_CLIENT_SECRET}" }
        }
    ``` 
    > [!IMPORTANT]
    > Убедитесь, что переменные (помеченные `{ }` ) в файле содержимого заменены.


1. **Authentication**
    1. [Проверка подлинности](https://github.com/influxdata/telegraf/blob/master/plugins/outputs/azure_monitor/README.md#azure-authentication)Azure Monitor может выполняться субъектом-службой.
        1. Подключаемый модуль Azure Monitor Telegraf предоставляет [несколько методов проверки подлинности](https://github.com/influxdata/telegraf/blob/master/plugins/outputs/azure_monitor/README.md#azure-authentication). Для использования проверки подлинности субъекта-службы необходимо задать следующие переменные среды.  
            • AZURE_TENANT_ID: указывает клиента, для которого проверяется подлинность.  
            • AZURE_CLIENT_ID: Указывает используемый идентификатор клиента приложения.  
            • AZURE_CLIENT_SECRET: указывает секрет приложения для использования.  
    >[!TIP]
    > Субъекту-службе можно предоставить роль "**Издатель метрик мониторинга**".

1. После развертывания модулей в Azure Monitor в одном пространстве имен будут отображаться метрики с именами метрик, совпадающими с теми, которые были созданы Prometheus. 
    1. В этом случае в портал Azure перейдите в центр Интернета вещей и щелкните ссылку "**метрики**" в левой области навигации. Вы должны увидеть метрики там.
## <a name="logging"></a>Logging

Как и в случае с другими IoT Edgeными модулями, можно также [изучить журналы контейнеров](../../iot-edge/troubleshoot.md#check-container-logs-for-issues) на пограничном устройстве. Сведения, которые записываются в журналы, можно контролировать с помощью [следующих свойств модуля двойника](module-twin-configuration-schema.md) :

* LogLevel

   * Допустимые значения: verbose, Information, Warning, Error, None.
   * Значение по умолчанию — Information — в журналах будут содержаться ошибки, предупреждения и сведения. Ошибка.
   * Если задать для параметра значение "предупреждение", в журналах будут содержаться сообщения об ошибках и предупреждения
   * Если задать значение "ошибка", журналы будут содержать только сообщения об ошибках.
   * Если задать для этого параметра значение нет, журналы создаваться не будут (это не рекомендуется).
   * Параметр verbose следует использовать только в том случае, если требуется предоставить общий доступ к журналам с помощью службы поддержки Azure для диагностики проблемы.
* логкатегориес

   * Разделенный запятыми список из одного или нескольких следующих элементов: Application, Events, Медиапипелине.
   * По умолчанию: Application, Events.
   * Приложение — это высокоуровневая информация из модуля, например сообщения о запуске модуля, ошибки среды и вызовы прямых методов.
   * Events — это все события, описанные ранее в этой статье.
   * Медиапипелине — это журналы низкого уровня, которые могут дать представление об устранении неполадок, например о проблемах подключения с помощью камеры с поддержкой RTSP.
   
### <a name="generating-debug-logs"></a>Создание журналов отладки

В некоторых случаях может потребоваться создать более подробные журналы, чем описано выше, чтобы помочь службе поддержки Azure устранить проблему. Это можно сделать двумя шагами.

Сначала необходимо [связать хранилище модулей с хранилищем устройств](../../iot-edge/how-to-access-host-storage-from-module.md#link-module-storage-to-device-storage) через креатеоптионс. Если изучить [шаблон манифеста развертывания](https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp/blob/master/src/edge/deployment.template.json) с помощью быстрого запуска, вы увидите следующее:

```
"createOptions": {
   …
   "Binds": [
     "/var/local/mediaservices/:/var/lib/azuremediaservices/"
   ]
 }
```

Выше позволяет модулю пограничных записей записывать журналы в путь к хранилищу (устройство) "/Вар/локал/медиасервицес/". Если в модуль добавляется следующее требуемое свойство:

`"debugLogsDirectory": "/var/lib/azuremediaservices/debuglogs/",`

Затем модуль будет записывать журналы отладки в двоичном формате в путь к хранилищу (устройство)/Вар/локал/медиасервицес/дебуглогс/, который можно поделиться с поддержкой Azure.

## <a name="faq"></a>ВОПРОСЫ И ОТВЕТЫ

[Вопросы и ответы](faq.md#monitoring-and-metrics)

## <a name="next-steps"></a>Дальнейшие действия

[Непрерывная запись видео](continuous-video-recording-tutorial.md)
