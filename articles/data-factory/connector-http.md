---
title: Копирование данных из источника HTTP с помощью фабрики данных Azure | Документация Майкрософт
description: Узнайте, как копировать данные из облака или локального источника HTTP на поддерживаемые приемники хранилища данных с помощью действия копирования в конвейере фабрики данных Azure.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: jingwang
ms.openlocfilehash: 87505081f16008dff7da1f567c1265c695f3f0ab
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60653778"
---
# <a name="copy-data-from-an-http-endpoint-by-using-azure-data-factory"></a>Копирование данных с источника HTTP с помощью фабрики данных Azure

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Версия 1](v1/data-factory-http-connector.md)
> * [Текущая версия](connector-http.md)

В этой статье описывается, как с помощью действия копирования в фабрике данных Azure копировать данные из конечной точки HTTP. Это продолжение статьи о [действии копирования в Фабрике данных Azure](copy-activity-overview.md), в которой представлены общие сведения о действии копирования.

Далее приводятся различия между соединителем HTTP, [соединителем REST](connector-rest.md) и [соединителем веб-таблиц](connector-web-table.md).

- **Соединитель REST** предназначен для поддержки копирования данных из RESTful API. 
- **Соединитель HTTP** применяется для извлечения данных из любой конечной точки HTTP, например для скачивания файла. Пока соединитель REST недоступен, для копирования данных из RESTful API можно использовать соединитель HTTP, который поддерживается, но является менее функциональным по сравнению с соединителем REST.
- **Соединитель веб-таблиц** извлекает содержимое таблицы со страницы HTML.

## <a name="supported-capabilities"></a>Поддерживаемые возможности

Данные из источника HTTP можно скопировать в любое хранилище данных, поддерживаемое в качестве приемника. Список хранилищ данных, поддерживаемых действием копирования в качестве источников и приемников, приведен в разделе [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats).

Можно использовать этот соединитель HTTP для таких действий.

- Извлечение данных из конечной точки HTTP/S с помощью методов **GET** или **POST** HTTP.
- Извлечение данных с использованием одного из следующих типов проверки подлинности: **Anonymous**, **Basic**, **Digest**, **Windows** и **ClientCertificate**.
- Копирование ответа HTTP "как есть" или его анализ с использованием [поддерживаемых форматов файлов и сжатия кодеков](supported-file-formats-and-compression-codecs.md).

> [!TIP]
> Чтобы проверить HTTP-запрос для извлечения данных, прежде чем настраивать соединитель HTTP в Data Factory, узнайте о спецификации API для заголовка и тела требований. Для проверки можно использовать такие средства, как Postman или веб-браузер.

## <a name="get-started"></a>Начало работы

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

В разделах ниже приведены сведения о свойствах, которые используются для определения сущностей Фабрики данных, относящихся к соединителю HTTP.

## <a name="linked-service-properties"></a>Свойства связанной службы

Для связанной службы HTTP поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| тип | Для свойства **type** необходимо задать значение **HttpServer**. | Да |
| url | Базовый URL-адрес веб-сервера. | Да |
| enableServerCertificateValidation | Укажите, следует ли включать проверку SSL-сертификата на сервере при подключении к конечной точке HTTP. Если ваш сервер HTTPS использует самозаверенный сертификат, установите свойство **false**. | Нет<br /> (значение по умолчанию **true**) |
| authenticationType | Указывает тип проверки подлинности. Допустимые значения **Anonymous**, **Basic** , **Digest**, **Windows** и **ClientCertificate**. <br><br> С дополнительными свойствами и примерами JSON этих типов проверки подлинности ознакомьтесь в последующих разделах этой таблицы. | Да |
| connectVia | [Среда выполнения интеграции](concepts-integration-runtime.md), используемая для подключения к хранилищу данных. Вы можете использовать Azure Integration Runtime или локальную среду IR (если хранилище данных расположено в частной сети). Если не указано другое, это свойство по умолчанию использует интегрированную среду выполнения Azure. |Нет |

### <a name="using-basic-digest-or-windows-authentication"></a>Использование типов проверки подлинности Basic, Digest или Windows

Установите свойство **authenticationType** как **Базовый**, **Дайджест** или **Windows**. В дополнение к общим свойствам, описанных в предыдущих разделах, укажите следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| userName | Имя пользователя для доступа к конечной точке HTTP. | Да |
| password | Пароль для пользователя (значение **userName**). Пометьте это поле как **SecureString**, чтобы безопасно хранить его в фабрике данных. Вы можете также [указать секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). | Да |

**Пример**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "Basic",
            "url" : "<HTTP endpoint>",
            "userName": "<user name>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="using-clientcertificate-authentication"></a>Использование типа проверки подлинности ClientCertificate

Чтобы использовать проверку подлинности ClientCertificate, установите свойство **authenticationType** для **ClientCertificate**. В дополнение к общим свойствам, описанных в предыдущих разделах, укажите следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| embeddedCertData | Данные сертификата, закодированные в Base64. | Укажите либо **embeddedCertData** или **certThumbprint**. |
| certThumbprint | Отпечаток сертификата, установленного в хранилище сертификатов компьютера локальной среды выполнения интеграции. Применяется, только если в свойство **connectVia** задан тип среды выполнения интеграции. | Укажите либо **embeddedCertData** или **certThumbprint**. |
| password | Пароль, связанный с сертификатом. Пометьте это поле как **SecureString**, чтобы безопасно хранить его в фабрике данных. Вы можете также [указать секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). | Нет |

Если вы используете свойство **certThumbprint** для проверки подлинности, а сертификат установлен в личном хранилище локального компьютера, предоставьте разрешения на чтение для локальной среды выполнения интеграции.

1. Откройте консоль управления (MMC). Добавьте оснастку **Сертификаты**, которая связана с **локальным компьютером**.
2. Разверните **Сертификаты** > **Личные**, и затем выберите **Сертификаты**.
3. Щелкните правой кнопкой мыши сертификат в личном хранилище, а затем выберите **Все задачи** > **Управление закрытыми ключами**.
3. На вкладке **Безопасность** добавьте учетную запись пользователя, с помощью которой запущена служба узла локальной среды выполнения интеграции (DIAHostService) с доступом на чтение к сертификату.

**Пример 1. Использование certThumbprint**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "certThumbprint": "<thumbprint of certificate>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Пример 2. Использование embeddedCertData**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "embeddedCertData": "<Base64-encoded cert data>",
            "password": {
                "type": "SecureString",
                "value": "password of cert"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Свойства набора данных

Этот раздел содержит список свойств, поддерживаемых набором данных HTTP. 

Полный список разделов и свойств, используемых для определения наборов данных, приведен в статье [Наборы данных и связанные службы в фабрике данных Azure](concepts-datasets-linked-services.md). 

Чтобы скопировать данные из HTTP, установите свойство **типа** набора данных **HttpFile**. Поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| тип | Свойство **type** для набора данных должно иметь значение **HttpFile**. | Да |
| relativeUrl | Относительный URL-адрес ресурса, который содержит данные. Если свойство не задано, используется только URL-адрес, указанный в определении связанной службы. | Нет |
| requestMethod | Метод HTTP. Допустимые значения: **GET** (по умолчанию) и **POST**. | Нет |
| additionalHeaders | Дополнительные заголовки HTTP-запроса. | Нет |
| requestBody | Текст HTTP-запроса. | Нет |
| свойства | Если требуется получить данные из конечной точки HTTP "как есть", — без анализа и копирования в хранилище файлов — можно пропустить раздел **форматирования** в определениях входного и выходного наборов данных.<br/><br/>Поддерживаемые форматы для выполнения анализа содержимого ответа HTTP в процессе выполнения операции копирования: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat** и **ParquetFormat**. Свойству **type** в разделе **format** необходимо присвоить одно из этих значений. Дополнительные сведения см. в разделах о [формате JSON](supported-file-formats-and-compression-codecs.md#json-format), [текстовом формате](supported-file-formats-and-compression-codecs.md#text-format), [формате Avro](supported-file-formats-and-compression-codecs.md#avro-format), [формате Orc](supported-file-formats-and-compression-codecs.md#orc-format) и [ формате Parquet](supported-file-formats-and-compression-codecs.md#parquet-format). |Нет |
| compression | Укажите тип и уровень сжатия данных. Дополнительные сведения см. в разделе [Поддержка сжатия](supported-file-formats-and-compression-codecs.md#compression-support).<br/><br/>Поддерживаемые типы: **GZip**, **Deflate**, **BZip2** и **ZipDeflate**.<br/>Поддерживаемые уровни:  **Оптимальный** и **Самый быстрый**. |Нет |

> [!NOTE]
> Поддерживаемый размер полезных данных запроса HTTP — около 500 КБ. Если размер полезных данных, которые вы хотите передать в конечную веб-точку, превышает размер 500 КБ, разбейте полезные данные на небольшие блоки.

**Пример 1. Использование метода Get (по умолчанию)**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        }
    }
}
```

**Пример 2. Использование метода Post**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "requestMethod": "Post",
            "requestBody": "<body for POST HTTP request>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Свойства действия копирования

Этот раздел содержит список свойств, поддерживаемых источником HTTP.

Полный список разделов и свойств, доступных для определения действий, см. в статье, посвященной [конвейерам и действиям в Фабрике данных Azure](concepts-pipelines-activities.md). 

### <a name="http-as-source"></a>HTTP в качестве источника

Чтобы копировать данные из HTTP, установите **тип источника** **HttpSource** в действии копирования. В разделе **source** действия копирования поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| тип | Свойство **type** источника действия копирования должно иметь значение **HttpSource**. | Да |
| httpRequestTimeout | Время ожидания (значение **Временной диапазон**) ответа для HTTP-запроса. Это значение является интервалом времени для получения ответа, а не считывания данных ответа. По умолчанию используется значение **00:01:40**.  | Нет |

**Пример**

```json
"activities":[
    {
        "name": "CopyFromHTTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<HTTP input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "HttpSource",
                "httpRequestTimeout": "00:01:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```


## <a name="next-steps"></a>Дальнейшие действия

В разделе [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats) приведен список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования в Фабрике данных Azure.
