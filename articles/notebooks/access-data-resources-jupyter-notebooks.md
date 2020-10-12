---
title: Доступ к данным в записных книжках Jupyter в предварительной версии Azure Notebook
description: Узнайте, как получить доступ к файлам, интерфейсам API для мобильных устройств, базам данных и различным ресурсам службы хранилища Azure из записной книжки Jupyter.
ms.topic: how-to
ms.date: 12/04/2018
ms.custom: devx-track-python
ms.openlocfilehash: b2254e6d966ca3281cd9c8b0771cb77fb6dede33
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87846621"
---
# <a name="access-cloud-data-in-a-notebook"></a>Доступ к облачным данным в записной книжке

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Выполнение различных операций в записной книжке Jupyter требует использования данных. Данные, действительно, являются основополагающим элементом записных книжек.

Конечно, вы можете [импортировать файлы данных в проект](work-with-project-data-files.md), даже используя такие команды, как `curl`, из записной книжки для загрузки файла напрямую. Однако, вероятно, вам нужно будет работать с более обширными данными, доступными из нефайловых источников, таких как API REST, реляционные базы данных и облачное хранилище, например таблицы Azure.

В данной статье кратко описаны эти различные варианты. Так как доступ к данным лучше всего описывать в действии, см. исполняемый код на [сайте GitHub](https://github.com/Microsoft/AzureNotebooks/blob/master/Samples/Access%20your%20data%20in%20Azure%20Notebooks.ipynb).

## <a name="rest-apis"></a>Интерфейсы REST API

В целом, к огромному количеству данных, доступных в Интернете, можно получить доступ не через файлы, а через REST API. К счастью, вы можете использовать код для отправки запросов и получения данных JSON, так как ячейка записной книжки может содержать любой код. Затем вы можете преобразовать этот JSON в любой необходимый формат, например кадр данных Pandas.

Чтобы получить доступ к данным с помощью REST API, используйте код в ячейках записной книжки, применяемый в любом другом приложении. Общая структура с использованием библиотеки запросов выглядит следующим образом:

```python
import pandas
import requests

# New York City taxi data for 2014
data_url = 'https://data.cityofnewyork.us/resource/gkne-dk5s.json'

# General data request; include other API keys and credentials as needed in the data argument
response = requests.get(data_url, data={"limit": "20"})

if response.status_code == 200:
    dataframe_rest2 = pandas.DataFrame.from_records(response.json())
    print(dataframe_rest2)
```

## <a name="azure-sql-database-and-sql-managed-instance"></a>База данных SQL Azure и Управляемый экземпляр SQL

Вы можете получить доступ к базам данных SQL или SQL Управляемый экземпляр с помощью библиотек pyodbc или pymssql.

[Использование Python для запроса к базе данных SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python) предоставляет инструкции по созданию базы данных в базе данных SQL, содержащей данные AdventureWorks, и показывает, как выполнять запросы к этим данным. Тот же код использован в примере записной книжки в этой статье.

## <a name="azure-storage"></a>Хранилище Azure

Служба хранилища Azure предоставляет несколько различных типов нереляционных хранилищ, в зависимости от типа имеющихся данных и способа доступа к ним:

- Хранилище таблиц. Предоставляет недорогое хранилище большого объема для табличных данных, таких как собранные журналы датчиков, журналы диагностики и т. д.
- Хранилище BLOB-объектов. Предоставляет файловое хранилище для любых типов данных.

В этом примере записной книжки демонстрируется работа с таблицами и большими двоичными объектами, в том числе использование подписанного URL-адреса для предоставления доступа на чтение к большим двоичным объектам.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

Azure Cosmos DB предоставляет полностью индексируемое хранилище NoSQL для документов JSON. В следующих статьях представлены различные способы работы с Cosmos DB с помощью Python:

- [Azure Cosmos DB — Создание приложения API SQL с помощью Python и портала Azure](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-python)
- [Создание приложения Flask с использованием API Azure Cosmos DB для MongoDB](https://docs.microsoft.com/azure/cosmos-db/create-mongodb-flask)
- [Azure Cosmos DB — Создание графовой базы данных с помощью Python и портала Azure](https://docs.microsoft.com/azure/cosmos-db/create-graph-python)
- [Создание приложения Cassandra с помощью Python и Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/create-cassandra-python)
- [Краткое руководство. Создание приложения API таблиц с помощью Python и Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/create-table-python)

При работе с Cosmos DB вы можете использовать библиотеку [azure-cosmosdb-table](https://pypi.org/project/azure-cosmosdb-table/).

## <a name="other-azure-databases"></a>Другие базы данных Azure

Azure предоставляет ряд других доступных для использования типов баз данных. Приведенные ниже статьи содержат рекомендации по получению доступа к этим базам данных с помощью Python:

- [База данных Azure для PostgreSQL: подключение и запрос данных с помощью Python](https://docs.microsoft.com/azure/postgresql/connect-python)
- [Краткое руководство. Использование кэша Redis для Azure с Python](https://docs.microsoft.com/azure/redis-cache/cache-python-get-started)
- [База данных Azure для MySQL: подключение и запрос данных с помощью Python](https://docs.microsoft.com/azure/mysql/connect-python)
- [Фабрика данных Azure](https://azure.microsoft.com/services/data-factory/).
  - [Code-free copy wizard for Azure Data Factory](https://azure.microsoft.com/updates/code-free-copy-wizard-for-azure-data-factory/) (Мастер копирования для службы "Фабрика данных Azure" без кода)

## <a name="next-steps"></a>Дальнейшие шаги

- [Руководство. Работа с файлами данных проекта](work-with-project-data-files.md)
