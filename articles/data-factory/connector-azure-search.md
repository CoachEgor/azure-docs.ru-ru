---
title: Копирование данных в индекс службы "Поиск" с использованием фабрики данных | Документация Майкрософт
description: Узнайте, как отправлять или копировать данные в индекс службы "Поиск Azure" с помощью действия копирования в конвейере фабрики данных Azure.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: jingwang
ms.openlocfilehash: edf475ac11168c33a6b11ccda3482ac44579e8d8
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2019
ms.locfileid: "68726220"
---
# <a name="copy-data-to-an-azure-search-index-using-azure-data-factory"></a>Копирование данных в индекс службы "Поиск Azure" с использованием фабрики данных Azure

> [!div class="op_single_selector" title1="Выберите используемую версию службы "Фабрика данных":"]
> * [Версия 1](v1/data-factory-azure-search-connector.md)
> * [Текущая версия](connector-azure-search.md)

В этой статье описывается, как с помощью действия копирования в фабрике данных Azure копировать данные в индекс службы "Поиск Azure". Это продолжение [статьи об обзоре действия копирования](copy-activity-overview.md), в которой представлены общие сведения о действии копирования.

## <a name="supported-capabilities"></a>Поддерживаемые возможности

Данные можно копировать из любых поддерживаемых исходных хранилищ данных в индекс службы "Поиск Azure". Список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования, приведен в таблице [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats).

## <a name="getting-started"></a>Приступая к работе

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Следующие разделы содержат сведения о свойствах, которые используются для определения сущностей фабрики данных, относящихся к соединителю службы "Поиск Azure".

## <a name="linked-service-properties"></a>Свойства связанной службы

Для связанной службы "Поиск Azure" поддерживаются следующие свойства:

| Свойство | Описание | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойству type необходимо задать значение **AzureSearch** | Да |
| url | URL-адрес службы Поиска Azure. | Да |
| ключ | Ключ администратора службы Поиска Azure. Пометьте это поле как SecureString, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). | Да |
| connectVia | [Среда выполнения интеграции](concepts-integration-runtime.md), используемая для подключения к хранилищу данных. Вы можете использовать среду выполнения интеграции Azure или локальную среду IR (если хранилище данных расположено в частной сети). Если не указано другое, по умолчанию используется интегрированная среда выполнения Azure. |Нет |

> [!IMPORTANT]
> При копировании данных из облачного хранилища данных в индекс службы поиска Azure в связанной службе поиска Azure необходимо ссылаться на Azure Integration Runtime с явно заданным регионом в Коннактвиа. Задайте регион, в котором находится служба "Поиск Azure". Дополнительные сведения о среде IR Azure см. [здесь](concepts-integration-runtime.md#azure-integration-runtime).

**Пример.**

```json
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": {
                "type": "SecureString",
                "value": "<AdminKey>"
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

Полный список разделов и свойств, доступных для определения наборов данных, см. в статье о наборах данных. Этот раздел содержит список свойств, поддерживаемых набором данных Поиска Azure.

Чтобы скопировать данные в службу поиска Azure, поддерживаются следующие свойства:

| Свойство | Описание | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Для свойства type набора данных необходимо задать следующее значение: **AzureSearchIndex** | Да |
| indexName | Имя индекса Поиска Azure. Фабрика данных не создает индекс. Индекс должен существовать в Поиске Azure. | Да |

**Пример.**

```json
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "typeProperties" : {
            "indexName": "products"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Azure Search linked service name>",
            "type": "LinkedServiceReference"
        }
   }
}
```

## <a name="copy-activity-properties"></a>Свойства действия копирования

Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure](concepts-pipelines-activities.md). Этот раздел содержит список свойств, поддерживаемых источником службы "Поиск Azure".

### <a name="azure-search-as-sink"></a>Служба "Поиск Azure" в качестве приемника

Чтобы скопировать данные в службу "Поиск Azure", в действии копирования задайте тип источника **AzureSearchIndexSink**. В разделе **sink** действия копирования поддерживаются следующие свойства:

| Свойство | Описание | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойству type источника действия копирования необходимо задать значение **AzureSearchIndexSink** | Да |
| writeBehavior | Указывает действие (объединение или замена), выполняемое, если документ уже существует в индексе. Ознакомьтесь с разделом [Свойство WriteBehavior](#writebehavior-property).<br/><br/>Допустимые значения: **Merge** (объединение), задано по умолчанию и **Upload** (отправка). | Нет |
| writeBatchSize | Передает данные в индекс Поиска Azure, когда размер буфера достигает значения writeBatchSize. Ознакомьтесь с разделом [Свойство WriteBatchSize](#writebatchsize-property).<br/><br/>Допустимые значения: целые числа от 1 до 1000; значение по умолчанию — 1000. | Нет |

### <a name="writebehavior-property"></a>Свойство WriteBehavior

При записи данных AzureSearchSink выполняет операцию upsert. Другими словами, если при создании документа ключ документа уже существует в индексе Поиска Azure, Поиск Azure обновляет существующий документ, а не создает исключения из-за конфликта.

AzureSearchSink проявляет два типа поведения upsert (с использованием пакета SDK AzureSearch):

- **Объединение**. Все столбцы в новом документе объединяются со столбцами в существующем. Для столбцов с значением null в новом документе значение столбцов в существующем документе сохраняется.
- **Upload** (отправка). Новый документ заменяет существующий. Для столбцов, не указанных в новом документе, задается значение null независимо от того, есть ли в существующем документе столбцы с значением, отличным от null.

Поведение по умолчанию — **объединение**.

### <a name="writebatchsize-property"></a>Свойство WriteBatchSize

Служба Поиска Azure поддерживает запись документов в виде пакета. Пакет может содержать от 1 до 1000 действий. Действие обрабатывает один документ для выполнения операции передачи или объединения.

**Пример.**

```json
"activities":[
    {
        "name": "CopyToAzureSearch",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Search output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureSearchIndexSink",
                "writeBehavior": "Merge"
            }
        }
    }
]
```

### <a name="data-type-support"></a>Поддержка типов данных

В следующей таблице представлены сведения о поддержке типов данных Поиска Azure.

| Тип данных Поиска Azure | Поддерживается в приемнике Поиска Azure |
| ---------------------- | ------------------------------ |
| String | Y |
| Int32 | Y |
| Int64 | Y |
| Double | Y |
| Boolean | Y |
| DataTimeOffset | Y |
| String Array | в |
| GeographyPoint | в |

## <a name="next-steps"></a>Следующие шаги
В таблице [Поддерживаемые хранилища данных](copy-activity-overview.md##supported-data-stores-and-formats) приведен список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования в фабрике данных Azure.
