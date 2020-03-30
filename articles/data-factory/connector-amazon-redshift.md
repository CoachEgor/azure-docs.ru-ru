---
title: Копирование данных из Amazon Redshift
description: Узнайте, как копировать данные из Amazon Redshift в поддерживаемые хранилища данных-приемники с помощью фабрики данных Azure.
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/04/2018
ms.openlocfilehash: 4d729a0117c7c409d1a3e0c3fd440aed96153203
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "79243592"
---
# <a name="copy-data-from-amazon-redshift-using-azure-data-factory"></a>Копирование данных из Amazon Redshift с помощью фабрики данных Azure
> [!div class="op_single_selector" title1="Выберите используемую версию службы "Фабрика данных":"]
> * [Версия 1](v1/data-factory-amazon-redshift-connector.md)
> * [Текущая версия](connector-amazon-redshift.md)


В этой статье описывается, как с помощью действия копирования в фабрике данных Azure копировать данные из Amazon Redshift. Это продолжение [статьи об обзоре действия копирования](copy-activity-overview.md), в которой представлены общие сведения о действии копирования.

## <a name="supported-capabilities"></a>Поддерживаемые возможности

Этот разъем Amazon Redshift поддерживается для следующих действий:

- [Копирование активности](copy-activity-overview.md) с [помощью поддерживаемой матрицы источника/раковины](copy-activity-overview.md)
- [Активность поиска](control-flow-lookup-activity.md)

Данные из Amazon Redshift можно скопировать в любое хранилище данных, поддерживаемое в качестве приемника. Список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования, приведен в таблице [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats).

Этот соединитель Amazon Redshift поддерживает получение данных из Redshift с помощью запроса или встроенной поддержки операции UNLOAD Redshift.

> [!TIP]
> Чтобы обеспечить наилучшую производительность при копировании больших объемов данных из Redshift, рекомендуется использовать встроенный механизм Redshift UNLOAD через Amazon S3. Дополнительные сведения см. в разделе [Копирование данных из Amazon Redshift с помощью UNLOAD](#use-unload-to-copy-data-from-amazon-redshift).

## <a name="prerequisites"></a>Предварительные требования

* При копировании данных в локальное хранилище данных с помощью [локальной среды IR](create-self-hosted-integration-runtime.md) предоставьте среде выполнения интеграции доступ к кластеру Amazon Redshift (использовав IP-адрес компьютера). Инструкции см. в статье об [авторизации доступа к кластеру](https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html).
* Если вы копируете данные в хранилище данных Azure, вам нужно знать IP-адреса вычислительных ресурсов и диапазоны SQL, используемые центрами обработки данных Azure. Диапазоны IP-адресов центра обработки данных Azure приведены на [этой странице](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="getting-started"></a>Начало работы

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Следующие разделы содержат сведения о свойствах, которые используются для определения сущностей фабрики данных, относящихся к соединителю Amazon Redshift.

## <a name="linked-service-properties"></a>Свойства связанной службы

Для связанной службы Amazon Redshift поддерживаются следующие свойства:

| Свойство | Описание | Обязательно |
|:--- |:--- |:--- |
| type | Свойство типа должно быть установлено: **AmazonRedshift** | Да |
| server |IP-адрес или имя узла сервера Amazon Redshift. |Да |
| порт |Номер TCP-порта, используемого сервером Amazon Redshift для прослушивания клиентских подключений. |Нет, значение по умолчанию — 5439 |
| База данных |Имя базы данных Amazon Redshift. |Да |
| username |Имя пользователя, имеющего доступ к базе данных. |Да |
| password |Пароль для учетной записи пользователя. Пометьте это поле как SecureString, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). |Да |
| connectVia | [Среда выполнения интеграции](concepts-integration-runtime.md), используемая для подключения к хранилищу данных. Вы можете использовать среду выполнения интеграции Azure или локальную среду IR (если хранилище данных расположено в частной сети). Если не указано другое, по умолчанию используется интегрированная среда выполнения Azure. |нет |

**Примере:**

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "<server name>",
            "database": "<database name>",
            "username": "<username>",
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

## <a name="dataset-properties"></a>Свойства набора данных

Полный список разделов и свойств, доступных [datasets](concepts-datasets-linked-services.md) для определения наборов данных, см. Этот раздел содержит список свойств, поддерживаемых набором данных Amazon Redshift.

Для копирования данных amazon Redshift поддерживаются следующие свойства:

| Свойство | Описание | Обязательно |
|:--- |:--- |:--- |
| type | Свойство типа набора данных должно быть установлено: **AmazonRedshiftTable** | Да |
| схема | Имя схемы. |Нет (если свойство query указано в источнике действия)  |
| table | Имя таблицы. |Нет (если свойство query указано в источнике действия)  |
| tableName | Название стола со схемой. Это свойство поддерживается для обратной совместимости. Использовать `schema` `table` и для новой рабочей нагрузки. | Нет (если свойство query указано в источнике действия) |

**Пример**

```json
{
    "name": "AmazonRedshiftDataset",
    "properties":
    {
        "type": "AmazonRedshiftTable",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Amazon Redshift linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

Если вы `RelationalTable` использовали набор набранных данных, он по-прежнему поддерживается как есть, в то время как вам предлагается использовать новый в будущем.

## <a name="copy-activity-properties"></a>Свойства действия копирования

Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure](concepts-pipelines-activities.md). Этот раздел содержит список свойств, поддерживаемых источником Amazon Redshift.

### <a name="amazon-redshift-as-source"></a>Amazon Redshift в качестве источника

Чтобы скопировать данные из Amazon Redshift, задайте тип источника **AmazonRedshiftSource** в действии копирования. Следующие свойства поддерживаются в разделе **источника активности** копирования:

| Свойство | Описание | Обязательно |
|:--- |:--- |:--- |
| type | Свойство type источника действия копирования должно иметь значение **AmazonRedshiftSource**. | Да |
| query |Используйте пользовательский запрос для чтения данных. Например, select * from MyTable. |Нет (если для набора данных задано свойство tableName) |
| redshiftUnloadSettings | Группа свойств при использовании Amazon Redshift UNLOAD. | нет |
| s3LinkedServiceName | Относится к службе Amazon S3, которую необходимо использовать в качестве промежуточного хранилища, указав имя связанной службы типа AmazonS3. | Да, если используется UNLOAD |
| bucketName | Укажите контейнер S3 для хранения промежуточных данных. Если не указано иное, служба фабрики данных создает его автоматически.  | Да, если используется UNLOAD |

**Пример. Источник Amazon Redshift в действии копирования с использованием UNLOAD**

```json
"source": {
    "type": "AmazonRedshiftSource",
    "query": "<SQL query>",
    "redshiftUnloadSettings": {
        "s3LinkedServiceName": {
            "referenceName": "<Amazon S3 linked service>",
            "type": "LinkedServiceReference"
        },
        "bucketName": "bucketForUnload"
    }
}
```

Дополнительные сведения о том, как использовать UNLOAD для эффективного копирования данных из Amazon Redshift, см. в следующем разделе.

## <a name="use-unload-to-copy-data-from-amazon-redshift"></a>Копирование данных из Amazon Redshift с помощью UNLOAD

[UNLOAD](https://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) — это механизм, предоставляемый Amazon Redshift, позволяющий выгрузить результаты запроса в один или несколько файлов в Amazon Simple Storage Service (Amazon S3). Компания Amazon рекомендует использовать этот способ для копирования большого набора данных из Redshift.

**Пример. Копирование данных из Amazon Redshift в хранилище данных SQL Azure с помощью UNLOAD, промежуточного копирования и PolyBase**

В этом варианте использования действие копирования выгружает данные из Amazon Redshift в Amazon S3, как задано в redshiftUnloadSettings, а затем копирует данные из Amazon S3 в большой двоичный объект Azure, заданный в stagingSettings, и наконец, использует PolyBase для загрузки данных в хранилище данных SQL. Формат промежуточного хранения обрабатывается действием копирования должным образом.

![Рабочий процесс копирования из Redshift в хранилище данных SQL](media/copy-data-from-amazon-redshift/redshift-to-sql-dw-copy-workflow.png)

```json
"activities":[
    {
        "name": "CopyFromAmazonRedshiftToSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "AmazonRedshiftDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "AmazonRedshiftSource",
                "query": "select * from MyTable",
                "redshiftUnloadSettings": {
                    "s3LinkedServiceName": {
                        "referenceName": "AmazonS3LinkedService",
                        "type": "LinkedServiceReference"
                    },
                    "bucketName": "bucketForUnload"
                }
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": "AzureStorageLinkedService",
                "path": "adfstagingcopydata"
            },
            "dataIntegrationUnits": 32
        }
    }
]
```

## <a name="data-type-mapping-for-amazon-redshift"></a>Сопоставление типов данных для Amazon Redshift

При копировании данных из Amazon Redshift для промежуточных типов данных фабрики данных Azure используются следующие сопоставления из типов данных Amazon Redshift. Дополнительные сведения о том, как действие копирования сопоставляет исходную схему и типы данных для приемника, см. в статье [Сопоставление схем в действии копирования](copy-activity-schema-and-type-mapping.md).

| Тип данных Amazon Redshift | Тип промежуточных данных фабрики данных |
|:--- |:--- |
| bigint |Int64 |
| BOOLEAN |Строка |
| CHAR |Строка |
| DATE |Дата и время |
| DECIMAL |Decimal |
| DOUBLE PRECISION |Double |
| INTEGER |Int32 |
| real |Один |
| SMALLINT |Int16 |
| TEXT |Строка |
| timestamp |Дата и время |
| VARCHAR |Строка |

## <a name="lookup-activity-properties"></a>Свойства активности поиска

Чтобы узнать подробности о свойствах, проверьте [активность поиска.](control-flow-lookup-activity.md)

## <a name="next-steps"></a>Дальнейшие действия
В таблице [Поддерживаемые хранилища данных](copy-activity-overview.md#supported-data-stores-and-formats) приведен список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования в фабрике данных Azure.
