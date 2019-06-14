---
title: Копирование данных из Salesforce и в Salesforce с помощью фабрики данных Azure | Документация Майкрософт
description: Узнайте, как копировать данные из Salesforce в поддерживаемые приемники данных или из поддерживаемых источников данных в Salesforce с помощью действия копирования в конвейере фабрики данных.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/19/2019
ms.author: jingwang
ms.openlocfilehash: 6056df9aa9079887bfb06ca20ad564eb52baff38
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60546578"
---
# <a name="copy-data-from-and-to-salesforce-by-using-azure-data-factory"></a>Копирование данных в Salesforce и обратно с помощью фабрики данных Azure
> [!div class="op_single_selector" title1="Выберите версию службы фабрики данных, которую вы используете:"]
> * [Версия 1](v1/data-factory-salesforce-connector.md)
> * [Текущая версия](connector-salesforce.md)

В этой статье описывается, как с помощью действия копирования в фабрике данных Azure копировать данные в Salesforce или из Salesforce. Это продолжение [статьи с обзором действия копирования](copy-activity-overview.md), в которой представлены общие сведения о действии копирования.

## <a name="supported-capabilities"></a>Поддерживаемые возможности

Данные из Salesforce можно скопировать в любое хранилище данных, поддерживаемое в качестве приемника. В свою очередь, данные из любого хранилища данных, поддерживаемого в качестве источника, можно скопировать в Salesforce. Список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования, приведен в таблице [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats).

В частности, этот соединитель Salesforce поддерживает следующее.

- Выпуски Salesforce Developer, Professional, Enterprise и Unlimited.
- Копирование данных в рабочую среду, песочницу или личный домен Salesforce, а также из них.

Соединитель Salesforce построена на основе API REST или массового Salesforce, с помощью [v45](https://developer.salesforce.com/docs/atlas.en-us.218.0.api_rest.meta/api_rest/dome_versions.htm) для копирования данных из и [v40](https://developer.salesforce.com/docs/atlas.en-us.208.0.api_asynch.meta/api_asynch/asynch_api_intro.htm) для копирования данных.

## <a name="prerequisites"></a>Технические условия

В Salesforce требуется включить разрешение API. Дополнительные сведения о включении доступа к API в Salesforce с помощью набора разрешений см. [здесь](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/).

## <a name="salesforce-request-limits"></a>Ограничения запросов Salesforce

Для Salesforce установлены ограничения на общее число запросов API и одновременных запросов API. Обратите внимание на следующие моменты.

- Если количество одновременных запросов превышает ограничение, выполняется регулирование и возникают случайные ошибки.
- В случае превышения ограничения на общее число запросов учетная запись Salesforce блокируется на 24 часа.

Кроме того, в обоих случаях вы можете получить сообщение об ошибке "REQUEST_LIMIT_EXCEEDED". Дополнительные сведения см. в разделе API request limits (Ограничения запросов API) руководства об [ограничениях для разработчика Salesforce](https://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf).

## <a name="get-started"></a>Начало работы

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Следующие разделы содержат сведения о свойствах, которые используются для определения сущностей фабрики данных, относящихся к соединителю Salesforce.

## <a name="linked-service-properties"></a>Свойства связанной службы

Для связанной службы Salesforce поддерживаются следующие свойства.

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type |Для свойства type нужно задать значение **Salesforce**. |Yes |
| environmentUrl | Укажите URL-адрес экземпляра Salesforce. <br> Значение по умолчанию — `"https://login.salesforce.com"`. <br> Чтобы скопировать данные из песочницы, укажите `"https://test.salesforce.com"`. <br> Чтобы скопировать данные из пользовательского домена, укажите URL-адрес, например `"https://[domain].my.salesforce.com"`. |Нет |
| username |Укажите имя пользователя для учетной записи пользователя. |Yes |
| password |Укажите пароль для учетной записи пользователя.<br/><br/>Пометьте это поле как SecureString, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). |Yes |
| securityToken |Укажите маркер безопасности для учетной записи пользователя. Инструкции по получению и сбросу маркера безопасности см. в [этом разделе](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm). Общие сведения о маркере безопасности см. в статье [Security and the API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm) (Безопасность и API).<br/><br/>Пометьте это поле как SecureString, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). |Yes |
| connectVia | [Среда выполнения интеграции](concepts-integration-runtime.md), используемая для подключения к хранилищу данных. Если не указано другое, по умолчанию используется интегрированная среда выполнения Azure. | "Нет" для источника, "Да" для приемника, если связанная с источником служба не имеет среды выполнения интеграции |

>[!IMPORTANT]
>При копировании данных в Salesforce среду выполнения интеграции Azure по умолчанию нельзя использовать для копирования. Другими словами, если связанная служба источника не имеет указанной среды выполнения интеграции, явным образом [создайте IR Azure](create-azure-integration-runtime.md#create-azure-ir) в расположении вблизи экземпляра Salesforce. Как показано в следующем примере, ассоциируйте связанную службу Salesforce.

**Пример. Хранение учетных данных в Фабрике данных**

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            },
            "securityToken": {
                "type": "SecureString",
                "value": "<security token>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Пример. Хранение учетных данных в Key Vault**

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of password in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            },
            "securityToken": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of security token in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
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

Полный список разделов и свойств, доступных для определения наборов данных, см. в статье о [наборах данных](concepts-datasets-linked-services.md). Этот раздел содержит список свойств, поддерживаемых набором данных Salesforce.

Чтобы скопировать данные в Salesforce, а также из Salesforce, для свойства type набора данных установите значение **SalesforceObject**. Поддерживаются следующие свойства.

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Для свойства type нужно задать значение **SalesforceObject**.  | Yes |
| objectApiName | Имя объекта Salesforce, из которого извлекаются данные. | "Нет" для источника, "Да" для приемника |

> [!IMPORTANT]
> Для любых настраиваемых объектов **имя API** должно содержать приставку "__c".

![Фабрика данных — подключение к Salesforce: имя API](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

**Пример.**

```json
{
    "name": "SalesforceDataset",
    "properties": {
        "type": "SalesforceObject",
        "linkedServiceName": {
            "referenceName": "<Salesforce linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "objectApiName": "MyTable__c"
        }
    }
}
```

>[!NOTE]
>Для обратной совместимости: если для копирования данных из Salesforce используется прежний тип набора данных "RelationalTable", он будет работать, пока не появится предложение переключиться на новый тип "SalesforceObject".

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойство type для набора данных должно иметь значение **RelationalTable**. | Yes |
| tableName | Имя таблицы в Salesforce | Нет (если свойство query указано в источнике действия) |

## <a name="copy-activity-properties"></a>Свойства действия копирования

Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure](concepts-pipelines-activities.md). Этот раздел содержит список свойств, поддерживаемых источником и приемником Salesforce.

### <a name="salesforce-as-a-source-type"></a>Salesforce в качестве источника данных

Чтобы копировать данные из Salesforce, установите тип источника **SalesforceSource** в действии копирования. В разделе **source** действия копирования поддерживаются следующие свойства.

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойство type источника действия копирования должно иметь значение **SalesforceSource**. | Yes |
| query |Используйте пользовательский запрос для чтения данных. Вы можете использовать запрос, написанный на [объектно-ориентированном языке запросов Salesforce (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm), или запрос SQL-92. Дополнительные советы см. в разделе [Советы по запросам](#query-tips). Если запрос не указан, будут извлечены все данные объекта Salesforce, заданного в наборе данных в свойстве objectApiName. | Нет (если в наборе данных задано свойство objectApiName) |
| readBehavior | Указывает, следует ли запрашивать существующие записи или все записи, включая удаленные. Если значение не задано, по умолчанию используется первое значение. <br>Допустимые значения: **query** (по умолчанию), **queryAll**.  | Нет |

> [!IMPORTANT]
> Для любых настраиваемых объектов **имя API** должно содержать приставку "__c".

![Фабрика данных — подключение к Salesforce: список имен API](media/copy-data-from-salesforce/data-factory-salesforce-api-name-2.png)

**Пример.**

```json
"activities":[
    {
        "name": "CopyFromSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Salesforce input dataset name>",
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
                "type": "SalesforceSource",
                "query": "SELECT Col_Currency__c, Col_Date__c, Col_Email__c FROM AllDataType__c"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

>[!NOTE]
>Для обратной совместимости: если для копирования данных из Salesforce используется прежний тип набора данных "RelationalSource", он будет работать, пока не появится предложение переключиться на новый тип "SalesforceSource".

### <a name="salesforce-as-a-sink-type"></a>Salesforce в качестве типа приемника

Чтобы скопировать данные в Salesforce, установите тип приемника **SalesforceSink** в действии копирования. В разделе **sink** действия копирования поддерживаются следующие свойства.

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойство type приемника действия копирования должно иметь значение **SalesforceSink**. | Yes |
| writeBehavior | Поведение операции при записи.<br/>Допустимые значения: **Insert** (Вставка) и **Upsert** (Вставка-обновление). | Нет (по умолчанию используется Insert) |
| externalIdFieldName | Имя поля для внешнего идентификатора при операции upsert. Это поле должно быть определено в объекте Salesforce как External Id Field (Поле внешнего идентификатора). Оно не может иметь значения NULL в соответствующих входных данных. | "Да" для операции Upsert (Вставка-обновление) |
| writeBatchSize | Количество строк данных, записываемых в Salesforce одним пакетом. | Нет (значение по умолчанию — 5,000) |
| ignoreNullValues | Указывает, следует ли игнорировать значения NULL из входных данных во время операции записи.<br/>Допустимые значения: **true** и **false**.<br>- **True**. При выполнении операции upsert или обновления (update) оставьте данные в целевом объекте без изменений. При выполнении операции вставки (insert) вставьте определенное значение по умолчанию.<br/>- **False**. При выполнении операции upsert или обновления (update) обновите данные в целевом объекте до значения NULL. При выполнении операции вставки (insert) вставьте значение NULL. | Нет (по умолчанию используется значение false) |

**Пример. Приемник Salesforce в действии копирования**

```json
"activities":[
    {
        "name": "CopyToSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Salesforce output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SalesforceSink",
                "writeBehavior": "Upsert",
                "externalIdFieldName": "CustomerId__c",
                "writeBatchSize": 10000,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="query-tips"></a>Советы по запросам

### <a name="retrieve-data-from-a-salesforce-report"></a>Получение данных из отчета Salesforce

Вы можете извлекать данные из отчетов Salesforce, указывая запросы в формате `{call "<report name>"}`. Например, `"query": "{call \"TestReport\"}"`.

### <a name="retrieve-deleted-records-from-the-salesforce-recycle-bin"></a>Получение удаленных записей из корзины Salesforce

Чтобы запросить обратимо удаленные записи из корзины Salesforce, укажите `readBehavior` как `queryAll`. 

### <a name="difference-between-soql-and-sql-query-syntax"></a>Различия между синтаксисом запросов SOQL и SQL

При копировании данных из Salesforce можно использовать SOQL или SQL-запрос. Обратите внимание, что эти два запроса имеют различную поддержку синтаксиса и функциональности, их не следует смешивать. Рекомендуется использовать SOQL-запрос, который поддерживается корпорацией Salesforce. Главные различия показаны в следующей таблице.

| Синтаксис | Режим SOQL | Режим SQL |
|:--- |:--- |:--- |
| Выбор столбцов | Требуется для перечисления полей, копируемых в запросе, например `SELECT field1, filed2 FROM objectname` | `SELECT *` поддерживается в дополнении к выделенному фрагменту столбца. |
| Кавычки | Имена полей или объектов не заключаются в кавычки. | Имена полей или объектов заключаются в кавычки, например `SELECT "id" FROM "Account"` |
| Формат даты и времени |  Подробнее см. [здесь](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql_select_dateformats.htm), а примеры — в следующем разделе. | Подробнее см. [здесь](https://docs.microsoft.com/sql/odbc/reference/develop-app/date-time-and-timestamp-literals?view=sql-server-2017), а примеры — в следующем разделе. |
| Логические значения | Представленные в виде `False` и `True`, например `SELECT … WHERE IsDeleted=True`. | Представленные в значении 0 или 1, например `SELECT … WHERE IsDeleted=1`. |
| Переименование столбцов | Не поддерживается. | Поддерживается, например, `SELECT a AS b FROM …`. |
| Связь | Поддерживается, например, `Account_vod__r.nvs_Country__c`. | Не поддерживается. |

### <a name="retrieve-data-by-using-a-where-clause-on-the-datetime-column"></a>Извлечение данных с использованием предложения where для столбца даты и времени

При указании запроса SOQL или SQL обратите внимание на различие в формате даты и времени. Например:

* **Пример SOQL**: `SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= @{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-ddTHH:mm:ssZ')} AND LastModifiedDate < @{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-ddTHH:mm:ssZ')}`
* **Пример SQL**: `SELECT * FROM Account WHERE LastModifiedDate >= {ts'@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}'} AND LastModifiedDate < {ts'@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'}`

### <a name="error-of-malformedquerytruncated"></a>Ошибка MALFORMED_QUERY: усеченное

Если произошла ошибка, из «MALFORMED_QUERY: Усечены», обычно это из-за в данных есть столбец типа JunctionIdList и Salesforce имеет ограничения, о поддержке таких данных с большим числом строк. Чтобы устранить проблему, попробуйте исключить столбец JunctionIdList или ограничить количество строк для копирования (можно разделить на несколько запусков действия копирования).

## <a name="data-type-mapping-for-salesforce"></a>Сопоставление типов данных для Salesforce

При копировании данных из Salesforce для промежуточных типов данных фабрики данных используются следующие сопоставления из типов данных Salesforce. Дополнительные сведения о том, как действие копирования сопоставляет исходную схему и типы данных для приемника, см. в статье [Сопоставление схем в действии копирования](copy-activity-schema-and-type-mapping.md).

| Тип данных Salesforce | Тип промежуточных данных фабрики данных |
|:--- |:--- |
| Auto Number |String |
| Флажок |Boolean |
| Валюта |Decimal |
| Дата |Datetime |
| Дата и время |Datetime |
| Email |String |
| Идентификатор |String |
| Связь для подстановки |String |
| Список множественного выбора |String |
| Number |Decimal |
| Процент |Decimal |
| Номер телефона |String |
| Список выбора |String |
| Text |String |
| Текстовое поле |String |
| Текстовое поле (длинное) |String |
| Текстовое поле (расширенное) |String |
| Текст (зашифрованный) |String |
| URL-адрес |String |

## <a name="next-steps"></a>Дальнейшие действия
В таблице [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats) приведен список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования в фабрике данных.
