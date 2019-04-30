---
title: Перенос данных SQL Server в SQL Azure с помощью фабрики данных Azure — командный процесс обработки и анализа данных
description: Настройка конвейера ADF, который объединяет два действия миграции данных, которые ежедневно перемещают данные между локальными базами данных и в облаке.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 59f8b8b253fc914e5723a9c41475ec78bc3f376e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61429354"
---
# <a name="move-data-from-an-on-premises-sql-server-to-sql-azure-with-azure-data-factory"></a>Перенос данных из локального SQL Server в SQL Azure с фабрикой данных Azure

В этой статье демонстрируется, как перенести данные из базы данных локального SQL Server в базу данных SQL Azure через хранилище BLOB-объектов Azure с помощью фабрики данных Azure (ADF).

Таблица, обобщающая различные варианты перемещения данных в Базу данных SQL Azure, содержится в статье [Перемещение данных в базу данных SQL Azure для машинного обучения Azure](move-sql-azure.md).

## <a name="intro"></a>Введение. Что такое ADF и когда ее использовать при переносе данных?
Фабрика данных Azure представляет собой полностью управляемую облачную службу интеграции информации, которая организует и автоматизирует перемещение и преобразование данных. Ключевым принципом модели ADF является конвейер. Конвейер — это логическая группа действий, каждое из которых определяет операции, выполняемые с данными, содержащимися в наборах данных. Информация, необходимая фабрике данных для подключения к внешним ресурсам, определяется связанными службами.

С ADF существующие службы обработки данных могут быть включены в конвейеры данных, обладающие высокой доступностью и управляемые в облаке. Для этих конвейеров данных можно запланировать прием, подготовку, преобразование, анализ и публикацию данных, а сложными данными и обработкой зависимостей управляет ADF. Решения можно быстро построить и развернуть в облаке, подключив большое количество локальных и облачных источников данных.

ADF стоит использовать в следующих случаях:

* когда данные нужно постоянно переносить в гибридном сценарии, при котором осуществляется доступ как к локальным, так и к облачным ресурсам;
* когда в процессе переноса данные необходимо обработать, изменить или добавить к ним операции бизнес-логики.

ADF позволяет выполнять планирование и отслеживание заданий с помощью простых сценариев JSON, управляющих перемещением данных на периодической основе. ADF также обладает другими возможностями, такими как поддержка сложных операций. Дополнительные сведения об ADF см. в документации по [фабрике данных Azure](https://azure.microsoft.com/services/data-factory/).

## <a name="scenario"></a>Сценарий
Мы настраиваем конвейер ADF, который объединяет два действия переноса данных. Вместе они ежедневно перемещают данные между локальными базами данных SQL и базой данных SQL Azure в облаке. Эти два действия таковы:

* копирование данных из локальных баз данных SQL Server в учетную запись хранения больших двоичных объектов Azure;
* копирование данных из учетной записи хранилища больших двоичных объектов Azure в базу данных SQL Azure.

> [!NOTE]
> Показанные здесь шаги были взяты из более подробного учебника, предоставленного группой разработчиков ADF: [Копирование данных из локальной базы данных SQL Server в хранилище BLOB-объектов Azure](https://docs.microsoft.com/azure/data-factory/tutorial-hybrid-copy-portal/). Ссылки на соответствующие разделы этой статьи предоставляются по мере необходимости.
>
>

## <a name="prereqs"></a>Предварительные требования
Для выполнения действий, описанных в этом учебнике, вам необходимо следующее.

* **Подписка Azure**. Если у вас нет подписки, вы можете зарегистрироваться для получения [бесплатной пробной версии](https://azure.microsoft.com/pricing/free-trial/).
* **Azure storage account**. Учетная запись хранения Azure используется в этом учебнике для хранения данных. Если у вас ее нет, см. раздел [Создание учетной записи хранения](../../storage/common/storage-quickstart-create-account.md). После создания учетной записи хранения необходимо получить ключ, используемый для доступа к хранилищу. Ознакомьтесь с разделом [Управление ключами доступа к хранилищу](../../storage/common/storage-account-manage.md#access-keys).
* Доступ к **базе данных SQL Azure**. Если необходимо настроить базу данных SQL Azure, раздел [Приступая к работе с базой данных SQL Microsoft Azure](../../sql-database/sql-database-get-started.md) предоставляет сведения о том, как подготовить новый экземпляр базы данных SQL Azure.
* Установленная и настроенная локальная среда **Azure PowerShell**. Инструкции см. в статье [Приступая к работе с командлетами Azure PowerShell](/powershell/azure/overview).

> [!NOTE]
> В этой процедуре используется [портал Azure](https://portal.azure.com/).
>
>

## <a name="upload-data"></a>Отправка данных в локальный SQL Server
Для демонстрации процесса переноса данных мы используем [набор данных о такси Нью-Йорка](https://chriswhong.com/open-data/foil_nyc_taxi/) . Набор данных о такси Нью-Йорка доступен, как отмечено в этой статье, в хранилище BLOB-объектов Azure [здесь](https://www.andresmh.com/nyctaxitrips/). Данные содержатся в двух файлах: trip_data.csv, содержащем сведения о поездках, и trip_far.csv, содержащем сведения о тарифах для каждой поездки. Пример и описание этих файлов приведены в [описании набора данных «Поездки такси Нью-Йорка»](sql-walkthrough.md#dataset).

Вы можете либо адаптировать описанные здесь процедуры к собственному набору данных, либо выполнить описанные действия с набором данных о такси Нью-Йорка. Для передачи набора данных о поездках в такси Нью-Йорка в локальную базу данных SQL Server выполните процедуру, описанную в разделе [Выполните массовый импорт данных в базу данных SQL Server](sql-walkthrough.md#dbload). Эти инструкции предназначены для SQL Server на виртуальной машине Azure. Процедура передачи для локального SQL Server идентична.

## <a name="create-adf"></a> Создание фабрики данных Azure
Инструкции по созданию фабрики данных Azure и группы ресурсов на [портале Azure](https://portal.azure.com/) представлены [здесь](../../data-factory/tutorial-hybrid-copy-portal.md#create-a-data-factory). Задайте имя *adfdsp* для нового экземпляра ADF и имя *adfdsprg* для созданной группы ресурсов.

## <a name="install-and-configure-azure-data-factory-integration-runtime"></a>Установка и настройка фабрики данных Azure Integration Runtime
Integration Runtime — это управляемая клиентом инфраструктура среды выполнения интеграции, которую фабрика данных Azure использует для обеспечения интеграции данных в разных сетевых средах. Эта среда выполнения раньше называлась "Шлюз управления данными".

Чтобы настроить [следуйте инструкциям по созданию конвейера](https://docs.microsoft.com/azure/data-factory/tutorial-hybrid-copy-portal#create-a-pipeline)

## <a name="adflinkedservices"></a>Создание связанных служб для подключения к ресурсам данных
Информация, необходимая фабрике данных для подключения к внешним ресурсам, определяется связанными службами. В этом сценарии есть три ресурса, для которых требуются связанные службы.

1. Локальный сервер SQL Server
2. Хранилище BLOB-объектов Azure
3. База данных SQL Azure

Пошаговая инструкция по созданию связанных служб приведена в [этом разделе](../../data-factory/tutorial-hybrid-copy-portal.md#create-a-pipeline).


## <a name="adf-tables"></a>Определение и создание таблиц для определения доступа к наборам данных
Для создания таблиц, которые определяют структуру, расположение и доступность наборов данных, используются следующие процедуры на основе сценариев. Для определения таблиц используются файлы JSON. Дополнительные сведения о структуре этих файлов см. в статье [Наборы данных в фабрике данных Azure](../../data-factory/concepts-datasets-linked-services.md).

> [!NOTE]
> Перед выполнением командлета [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) необходимо проверить, правильно ли выбрана подписка для его выполнения, выполнив командлет `Add-AzureAccount`. Документацию по этим командлетам см. в статье [Add-AzureAccount](/powershell/module/servicemanagement/azure/add-azureaccount?view=azuresmps-3.7.0).
>
>

В определениях на основе JSON в таблицах используются следующие имена:

* **имя таблицы** в локальном SQL Server — *nyctaxi_data*;
* the **имя контейнера** в учетной записи хранилища больших двоичных объектов Azure — *containername*

Для этого конвейера ADF необходимо определить три таблицы:

1. [Локальная таблица SQL](#adf-table-onprem-sql).
2. [Таблица больших двоичных объектов](#adf-table-blob-store)
3. [таблица SQL Azure.](#adf-table-azure-sql)

> [!NOTE]
> В этих процедурах для определения и создания действий ADF используется Azure PowerShell. Однако эти задачи также можно выполнить на портале Azure. Дополнительные сведения см. в разделе [Создание наборов данных](../../data-factory/tutorial-hybrid-copy-portal.md#create-a-pipeline).
>
>

### <a name="adf-table-onprem-sql"></a>Локальная таблица SQL
Определение таблицы для локального SQL Server указывается в следующем файле JSON:

```json
{
    "name": "OnPremSQLTable",
    "properties":
    {
        "location":
        {
            "type": "OnPremisesSqlServerTableLocation",
            "tableName": "nyctaxi_data",
            "linkedServiceName": "adfonpremsql"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1,
            "waitOnExternal":
            {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Сюда не были включены имена столбцов. Для выбора имен столбцов с помощью подзапроса включите их сюда (дополнительные сведения приведены в [документации по ADF](../../data-factory/copy-activity-overview.md) ).

Скопируйте определение таблицы JSON в файл с именем *onpremtabledef.json* и сохраните его в известном месте (например, *C:\temp\onpremtabledef.json*). Создайте таблицу в ADF с помощью следующего командлета Azure PowerShell:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


### <a name="adf-table-blob-store"></a>таблица больших двоичных объектов;
Определение таблицы выходных больших двоичных объектов задается следующим образом (здесь данные, полученные из локальных расположений, сопоставляются с большим двоичным объектом Azure):

```json
{
    "name": "OutputBlobTable",
    "properties":
    {
        "location":
        {
            "type": "AzureBlobLocation",
            "folderPath": "containername",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": "\t"
            },
            "linkedServiceName": "adfds"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

Скопируйте определение таблицы JSON в файл с именем *bloboutputtabledef.json* и сохраните его в известном месте (например, *C:\temp\bloboutputtabledef.json*). Создайте таблицу в ADF с помощью следующего командлета Azure PowerShell:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json

### <a name="adf-table-azure-sql"></a>таблица SQL Azure.
Определение таблицы для выходных данных SQL Azure задается следующим образом (эта схема сопоставляет данные, полученные из большого двоичного объекта).

```json
{
    "name": "OutputSQLAzureTable",
    "properties":
    {
        "structure":
        [
            { "name": "column1", "type": "String"},
            { "name": "column2", "type": "String"}
        ],
        "location":
        {
            "type": "AzureSqlTableLocation",
            "tableName": "your_db_name",
            "linkedServiceName": "adfdssqlazure_linked_servicename"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

Скопируйте определение таблицы JSON в файл с именем *AzureSqlTable.json* и сохраните его в известном месте (например, *C:\temp\AzureSqlTable.json*). Создайте таблицу в ADF с помощью следующего командлета Azure PowerShell:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json


## <a name="adf-pipeline"></a>Определение и создание конвейера
Укажите действия, которые принадлежат конвейеру, и создайте конвейер с помощью следующих процедур на основе сценариев. Для определения свойств конвейера используется файл JSON.

* Сценарий предполагает, что **имя конвейера** — *AMLDSProcessPipeline*.
* Также обратите внимание, что мы задаем периодичность выполнения конвейера ежедневно и используем время выполнения задания по умолчанию (00:00 по Гринвичу).

> [!NOTE]
> В следующих процедурах для определения и создания конвейера ADF используется Azure PowerShell. Однако эту задачу также можно выполнить на портале Azure. Дополнительные сведения см. в разделе [Создание конвейера](../../data-factory/tutorial-hybrid-copy-portal.md#create-a-pipeline).
>
>

Используя приведенные выше определения таблиц, определение конвейера ADF указывается следующим образом:

```json
{
    "name": "AMLDSProcessPipeline",
    "properties":
    {
        "description" : "This pipeline has one Copy activity that copies data from an on-premises SQL to Azure blob",
        "activities":
        [
            {
                "name": "CopyFromSQLtoBlob",
                "description": "Copy data from on-premises SQL server to blob",
                "type": "CopyActivity",
                "inputs": [ {"name": "OnPremSQLTable"} ],
                "outputs": [ {"name": "OutputBlobTable"} ],
                "transformation":
                {
                    "source":
                    {
                        "type": "SqlSource",
                        "sqlReaderQuery": "select * from nyctaxi_data"
                    },
                    "sink":
                    {
                        "type": "BlobSink"
                    }
                },
                "Policy":
                {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "style": "StartOfInterval",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            },
            {
                "name": "CopyFromBlobtoSQLAzure",
                "description": "Push data to Sql Azure",
                "type": "CopyActivity",
                "inputs": [ {"name": "OutputBlobTable"} ],
                "outputs": [ {"name": "OutputSQLAzureTable"} ],
                "transformation":
                {
                    "source":
                    {
                        "type": "BlobSource"
                    },
                    "sink":
                    {
                        "type": "SqlSink",
                        "WriteBatchTimeout": "00:5:00",
                    }
                },
                "Policy":
                {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "style": "StartOfInterval",
                    "retry": 2,
                    "timeout": "02:00:00"
                }
            }
        ]
    }
}
```

Скопируйте определение JSON конвейера в файл с именем *pipelinedef.json* и сохраните его в известном месте (например, *C:\temp\pipelinedef.json*). Создайте конвейер в ADF с помощью следующего командлета Azure PowerShell:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json


## <a name="adf-pipeline-start"></a>Запуск конвейера
Конвейер можно запустить с помощью следующей команды:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

Значения параметров *startdate* и *enddate* необходимо заменить фактическими датами, определяющими временной интервал запуска конвейера.

После запуска конвейера вы сможете увидеть, как в контейнере большого двоичного объекта начнут появляться данные, по одному файлу в день.

Обратите внимание, что мы не используем возможность поэтапного поступления данных в конвейер, предоставляемую ADF. Дополнительные сведения об этой и других возможностях ADF см. в [документации по ADF](https://azure.microsoft.com/services/data-factory/).
