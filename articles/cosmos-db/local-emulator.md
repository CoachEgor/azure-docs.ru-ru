---
title: Разработка с помощью эмулятора Azure Cosmos в локальной среде
description: С помощью эмулятора Azure Cosmos вы можете бесплатно разрабатывать и тестировать приложения локально. Для этого вам не нужно создавать подписку Azure.
ms.service: cosmos-db
ms.topic: tutorial
author: markjbrown
ms.author: mjbrown
ms.date: 01/31/2020
ms.openlocfilehash: 287933de6403d680c5aa5b6c78df49abe5f2ac56
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/25/2020
ms.locfileid: "77591376"
---
# <a name="use-the-azure-cosmos-emulator-for-local-development-and-testing"></a>Использование эмулятора Azure Cosmos для разработки и тестирования в локальной среде

Эмулятор Azure Cosmos предоставляет локальную среду для разработки, которая эмулирует службу Azure Cosmos DB. С помощью эмулятора Azure Cosmos вы можете локально разрабатывать и тестировать приложения, не создавая подписку Azure и не тратя средства. Если приложение в эмуляторе Azure Cosmos работает надлежащим образом, вы можете перейти на использование учетной записи Azure Cosmos в облаке.

Для разработки эмулятор Azure Cosmos можно использовать с учетными записями [SQL](local-emulator.md#sql-api), [Cassandra](local-emulator.md#cassandra-api), [MongoDB](local-emulator.md#azure-cosmos-dbs-api-for-mongodb), [Gremlin](local-emulator.md#gremlin-api) и [API таблиц](local-emulator.md#table-api). Но сейчас представление обозревателя данных в эмуляторе полностью поддерживает только клиенты для API SQL. 

## <a name="how-the-emulator-works"></a>Как работает эмулятор

Эмулятор Azure Cosmos обеспечивает высокоточную эмуляцию службы Azure Cosmos DB. В эмуляторе реализованы те же функции, что и в службе Azure Cosmos DB, включая возможность создания данных и выполнения запросов к ним, подготовку и масштабирование контейнеров, а также выполнение хранимых процедур и триггеров. С помощью эмулятора Azure Cosmos можно разрабатывать и тестировать приложения. Чтобы развернуть эти приложения в глобальной среде Azure, нужно изменить всего один параметр конфигурации для конечной точки подключения к Azure Cosmos DB.

Хотя эмуляция службы Azure Cosmos DB является точной, реализация эмулятора отличается от службы. Например, эмулятор использует стандартные компоненты операционной системы: локальную файловую систему для сохранения данных и стек протокола HTTPS для подключений. Это значит, что в режиме эмулятора не применимы некоторые возможности инфраструктуры Azure, включая глобальную репликацию, задержку менее 10 миллисекунд для операций чтения и записи или настраиваемые уровни согласованности.

Вы можете переносить данные между эмулятором Azure Cosmos и службой Azure Cosmos DB с помощью [средства миграции данных Azure Cosmos DB](https://github.com/azure/azure-documentdb-datamigrationtool).

Вы можете запустить эмулятор Azure Cosmos для контейнера Docker в Windows. Команда docker pull описана в [Центре Docker](https://hub.docker.com/r/microsoft/azure-cosmosdb-emulator/), а исходный код эмулятора размещен в репозитории [GitHub](https://github.com/Azure/azure-cosmos-db-emulator-docker) для `Dockerfile` вместе с дополнительными сведениями.

## <a name="differences-between-the-emulator-and-the-service"></a>Различия между эмулятором и службой

Эмулятор Azure Cosmos DB предоставляет эмулированную среду, которая выполняется на локальном компьютере разработчика. Поэтому есть некоторые различия между доступными возможностями в эмуляторе и облачной учетной записи Azure Cosmos DB.

* Сейчас обозреватель данных в эмуляторе поддерживает только клиенты для API SQL. Представление обозревателя данных и операции для таких API Azure Cosmos DB, как MongoDB, таблиц, Graph и Cassandra, поддерживаются не полностью.
* Эмулятор Azure Cosmos поддерживает только одну предопределенную учетную запись и известный главный ключ. Повторное создание ключа в эмуляторе Azure Cosmos невозможно, но вы можете изменить ключ по умолчанию с помощью параметра командной строки.
* Эмулятор Azure Cosmos не поддерживает масштабирование и не может работать с большим числом контейнеров.
* Эмулятор Azure Cosmos не поддерживает разные [уровни согласованности Azure Cosmos DB](consistency-levels.md).
* Эмулятор Azure Cosmos не поддерживает [репликацию между несколькими регионами](distribute-data-globally.md).
* Локальная версия эмулятора Azure Cosmos может не всегда отражать свежие изменения в службе Azure Cosmos DB. Поэтому, чтобы точно оценить необходимую пропускную способность для приложения (в единицах запроса), используйте [планировщик ресурсов Azure Cosmos DB](https://www.documentdb.com/capacityplanner).
* Если вы используете эмулятор Azure Cosmos, по умолчанию можно создать до 25 контейнеров фиксированного размера (только пакеты SDK для Azure Cosmos DB) или 5 контейнеров неограниченного размера (эмулятор Azure Cosmos). Дополнительные сведения об изменении этого значения см. в разделе [Изменение количества коллекций](#set-partitioncount).

## <a name="system-requirements"></a>Требования к системе

Эмулятор Azure Cosmos имеет следующие требования к оборудованию и программному обеспечению.

* Требования к программному обеспечению
  * Windows Server 2012 R2, Windows Server 2016 или Windows 10.
  * 64-разрядная операционная система.
* Минимальные требования к оборудованию
  * 2 ГБ ОЗУ
  * 10 ГБ свободного дискового пространства

## <a name="installation"></a>Установка

Эмулятор Azure Cosmos можно скачать и установить из [Центра загрузки Майкрософт](https://aka.ms/cosmosdb-emulator). Вы также можете запустить эмулятор в Docker для Windows. Инструкции по использованию эмулятора в Docker для Windows см. в статье [Использование эмулятора Azure Cosmos DB для разработки и тестирования в локальной среде](#running-on-docker).

> [!NOTE]
> Чтобы установить, настроить и запустить эмулятор Azure Cosmos, нужны права администратора на локальном компьютере. Эмулятор создаст сертификат, добавит его и настроит правила брандмауэра для запуска своих служб. Это означает, что у эмулятора должна быть возможность выполнять такие операции.

## <a name="running-on-windows"></a>Запуск в Windows

Чтобы запустить эмулятор Azure Cosmos, нажмите кнопку "Пуск" или клавишу Windows. Начните вводить текст **эмулятор Azure Cosmos** и выберите эмулятор в списке приложений.

![Нажмите кнопку "Пуск" или клавишу Windows, начните вводить **эмулятор Azure Cosmos** и выберите эмулятор в списке приложений](./media/local-emulator/database-local-emulator-start.png)

При запуске эмулятора в области уведомлений панели задач Windows появится соответствующий значок. ![Уведомление локального эмулятора Azure Cosmos DB на панели задач](./media/local-emulator/database-local-emulator-taskbar.png)

Эмулятор Azure Cosmos по умолчанию выполняется на локальном компьютере (localhost) и прослушивает порт 8081.

Эмулятор Azure Cosmos по умолчанию устанавливается в каталог `C:\Program Files\Azure Cosmos DB Emulator`. Можно также запустить и остановить эмулятор из командной строки. Дополнительные сведения см. в [справочнике по программе командной строки](#command-line).

## <a name="start-data-explorer"></a>Запуск обозревателя данных

При запуске эмулятор Azure Cosmos автоматически откроет в браузере страницу обозревателя данных Azure Cosmos. В адресной строке отобразится адрес `https://localhost:8081/_explorer/index.html`. Чтобы позднее открыть страницу обозревателя данных, вы можете ввести в браузере этот URL-адрес или запустить обозреватель из эмулятора Azure Cosmos, используя значок в области уведомлений Windows, как показано ниже.

![Запуск обозревателя данных из локального эмулятора Azure Cosmos](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a>Проверка наличия обновлений

Обозреватель данных сообщает о наличии нового обновления, доступного для скачивания.

> [!NOTE]
> Данные, созданные в одной версии эмулятора Azure Cosmos DB (папка %LOCALAPPDATA%\CosmosDBEmulator или указанный вами путь к данным), могут оказаться недоступными при использовании другой версии. Если нужно сохранить данные на длительный срок, мы рекомендуем разместить их в учетной записи хранения Azure Cosmos, а не эмуляторе Azure Cosmos.

## <a name="authenticating-requests"></a>Выполнение проверки подлинности запросов

Как и в облачной службе Azure, в эмуляторе Azure Cosmos для каждого запроса должна выполняться аутентификация. Эмулятор Azure Cosmos поддерживает аутентификацию с помощью главного ключа, используя одну предопределенную учетную запись и известный ключ аутентификации. В качестве учетных данных для эмулятора Azure Cosmos можно использовать только эту учетную запись и только этот ключ. К ним относятся:

```bash
Account name: localhost:<port>
Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
```

> [!NOTE]
> Главный ключ, поддерживаемый эмулятором Azure Cosmos, предназначен для использования только с этим эмулятором. Использовать рабочую учетную запись и ключ Azure Cosmos DB с эмулятором Azure Cosmos нельзя.

> [!NOTE]
> Если вы запустили эмулятор с параметром /Key, вместо `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==` следует использовать сгенерированный ключ. Дополнительные сведения о параметре /Key см. в [справочнике по программе командной строки](#command-line).

Как и Azure Cosmos DB, эмулятор Azure Cosmos поддерживает только безопасное подключение по протоколу SSL.

## <a name="running-on-a-local-network"></a>Запуск в локальной сети

Вы можете запустить эмулятор в локальной сети. Чтобы разрешить доступ к сети, укажите параметр `/AllowNetworkAccess` в [командной строке](#command-line-syntax), а также один из дополнительных параметров `/Key=key_string` или `/KeyFile=file_name`. Можно также применить параметр `/GenKeyFile=file_name`, чтобы заранее создать файл со случайным ключом. Его можно затем передать в `/KeyFile=file_name` или `/Key=contents_of_file`.

Чтобы получить доступ к сети в первый раз, нужно завершить работу эмулятора и удалить каталог данных эмулятора (%LOCALAPPDATA%\CosmosDBEmulator).

## <a name="developing-with-the-emulator"></a>Разработка с помощью эмулятора

### <a name="sql-api"></a>API SQL

Когда эмулятор Azure Cosmos будет запущен на локальном компьютере, вы сможете работать с ним с помощью любых поддерживаемых [пакетов SDK для Azure Cosmos DB](sql-api-sdk-dotnet.md) или [REST API Azure Cosmos DB](/rest/api/cosmos-db/). Эмулятор Azure Cosmos также содержит встроенный обозреватель данных, который позволяет создавать коллекции для API SQL или API Cosmos DB для MongoDB, а также просматривать и редактировать элементы без необходимости писать код.

```csharp
// Connect to the Azure Cosmos Emulator running locally
DocumentClient client = new DocumentClient(
   new Uri("https://localhost:8081"), "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

```

### <a name="azure-cosmos-dbs-api-for-mongodb"></a>API Azure Cosmos DB для MongoDB

Когда эмулятор Azure Cosmos будет запущен на локальном компьютере, вы сможете взаимодействовать с ним с помощью [пакета SDK для API Azure Cosmos DB для MongoDB](mongodb-introduction.md). Запустите эмулятор из командной строки с правами администратора, указав параметр /EnableMongoDbEndpoint. Затем подключитесь к учетной записи API MongoDB, используя следующую строку подключения:

```bash
mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true
```

### <a name="table-api"></a>API таблиц

Когда эмулятор Azure Cosmos будет запущен на локальном компьютере, вы сможете взаимодействовать с ним с помощью [пакета SDK для API таблиц Azure Cosmos DB](table-storage-how-to-use-dotnet.md). Запустите эмулятор из командной строки с правами администратора, указав параметр /EnableTableEndpoint. Затем выполните следующий код для подключения к учетной записи API таблиц:

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Table;
using CloudTable = Microsoft.WindowsAzure.Storage.Table.CloudTable;
using CloudTableClient = Microsoft.WindowsAzure.Storage.Table.CloudTableClient;

string connectionString = "DefaultEndpointsProtocol=http;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=http://localhost:8902/;";

CloudStorageAccount account = CloudStorageAccount.Parse(connectionString);
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("testtable");
table.CreateIfNotExists();
table.Execute(TableOperation.Insert(new DynamicTableEntity("partitionKey", "rowKey")));
```

### <a name="cassandra-api"></a>API Cassandra

Запустите эмулятор из командной строки с правами администратора, указав параметр /EnableCassandraEndpoint. В качестве альтернативы можно также задать переменную среды `AZURE_COSMOS_EMULATOR_CASSANDRA_ENDPOINT=true`.

* [Установите Python 2.7](https://www.python.org/downloads/release/python-2716/).

* [Установите Cassandra CLI/CQLSH](https://cassandra.apache.org/download/)

* Выполните следующие команды в обычном окне командной строки:

  ```bash
  set Path=c:\Python27;%Path%
  cd /d C:\sdk\apache-cassandra-3.11.3\bin
  set SSL_VERSION=TLSv1_2
  set SSL_VALIDATE=false
  cqlsh localhost 10350 -u localhost -p C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw== --ssl
  ```

* В оболочке CQLSH выполните следующие команды, чтобы подключиться к конечной точке Cassandra:

  ```bash
  CREATE KEYSPACE MyKeySpace WITH replication = {'class':'MyClass', 'replication_factor': 1};
  DESCRIBE keyspaces;
  USE mykeyspace;
  CREATE table table1(my_id int PRIMARY KEY, my_name text, my_desc text);
  INSERT into table1 (my_id, my_name, my_desc) values( 1, 'name1', 'description 1');
  SELECT * from table1;
  EXIT
  ```

### <a name="gremlin-api"></a>API Gremlin

Запустите эмулятор из командной строки с правами администратора, указав параметр /EnableGremlinEndpoint. В качестве альтернативы можно также задать переменную среды `AZURE_COSMOS_EMULATOR_GREMLIN_ENDPOINT=true`.

* [Установите apache-tinkerpop-gremlin-console-3.3.4.](https://archive.apache.org/dist/tinkerpop/3.3.4)

* В обозревателе данных эмулятора создайте базу данных db1 и коллекцию coll1, указав параметр /name для ключа раздела

* Выполните следующие команды в обычном окне командной строки:

  ```bash
  cd /d C:\sdk\apache-tinkerpop-gremlin-console-3.3.4-bin\apache-tinkerpop-gremlin-console-3.3.4
  
  copy /y conf\remote.yaml conf\remote-localcompute.yaml
  notepad.exe conf\remote-localcompute.yaml
    hosts: [localhost]
    port: 8901
    username: /dbs/db1/colls/coll1
    password: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
    connectionPool: {
    enableSsl: false}
    serializer: { className: org.apache.tinkerpop.gremlin.driver.ser.GraphSONMessageSerializerV1d0,
    config: { serializeResultToString: true  }}

  bin\gremlin.bat
  ```

* В оболочке Gremlin выполните следующие команды, чтобы подключиться к конечной точке Gremlin:

  ```bash
  :remote connect tinkerpop.server conf/remote-localcompute.yaml
  :remote console
  :> g.V()
  :> g.addV('person1').property(id, '1').property('name', 'somename1')
  :> g.addV('person2').property(id, '2').property('name', 'somename2')
  :> g.V()
  ```

## <a name="export-the-ssl-certificate"></a>Экспорт SSL-сертификата

Языки и среда выполнения .NET используют хранилище сертификатов Windows для безопасного подключения к локальному эмулятору Azure Cosmos DB. Другие языки используют собственные методы для управления сертификатами и использования сертификатов. Java поддерживает собственное [хранилище сертификатов](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html), а Python применяет [оболочки сокетов](https://docs.python.org/2/library/ssl.html).

Чтобы получить сертификат, используемый с языками и средами выполнения, которые не интегрируются с хранилищем сертификатов Windows, необходимо экспортировать сертификат с помощью диспетчера сертификатов Windows. Чтобы открыть его, запустите файл certlm.msc или выполните пошаговые инструкции по [экспорту сертификатов в эмуляторе Azure Cosmos](./local-emulator-export-ssl-certificates.md). Когда откроется диспетчер сертификатов, откройте в нем "Личные сертификаты", как показано ниже, и экспортируйте сертификат с понятным именем DocumentDBEmulatorCertificate в формате X.509 с кодировкой BASE-64 (CER-файл).

![Сертификат SSL для эмулятора Azure Cosmos DB](./media/local-emulator/database-local-emulator-ssl_certificate.png)

Чтобы импортировать сертификат X.509 в хранилище сертификатов Java, выполните инструкции из статьи [Добавление сертификата в хранилище сертификатов ЦС Java](https://docs.microsoft.com/azure/java-add-certificate-ca-store). После импорта сертификата в хранилище сертификатов клиенты для SQL и API Azure Cosmos DB для MongoDB смогут подключаться к эмулятору Azure Cosmos.

При подключении к эмулятору с помощью пакетов SDK для Python и Node.js проверка SSL отключена.

## <a id="command-line"></a>Справочник по программе командной строки
С помощью командной строки из папки установки можно запускать и останавливать эмулятор, настраивать параметры и выполнять другие операции.

### <a name="command-line-syntax"></a>Синтаксис для командной строки

    Microsoft.Azure.Cosmos.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/EnableMongoDbEndpoint] [/?]

Чтобы просмотреть список параметров, в командной строке введите `Microsoft.Azure.Cosmos.Emulator.exe /?` .

|**Параметр** | **Описание** | **Command**| **Аргументы**|
|---|---|---|---|
|[Нет аргументов] | Запускает эмулятор Azure Cosmos с параметрами по умолчанию. |Microsoft.Azure.Cosmos.Emulator.exe| |
|[Help] |Отображает список поддерживаемых аргументов командной строки.|Microsoft.Azure.Cosmos.Emulator.exe /? | |
| GetStatus |Получает состояние эмулятора Azure Cosmos. Состояние обозначается кодом выхода: 1 = запуск, 2 = выполнение, 3 = остановка. Код выхода с отрицательным значением указывает, что произошла ошибка. Другие выходные данные не формируются. | Microsoft.Azure.Cosmos.Emulator.exe /GetStatus| |
| Shutdown| Завершает работу эмулятора Azure Cosmos.| Microsoft.Azure.Cosmos.Emulator.exe /Shutdown | |
|DataPath | Указывает путь для сохранения файлов данных. По умолчанию имеет значение %LocalAppdata%\CosmosDBEmulator. | Microsoft.Azure.Cosmos.Emulator.exe /DataPath=\<datapath\> | \<datapath:\> любой доступный путь. |
|Порт | Указывает номер порта, который должен использоваться эмулятором. Значение по умолчанию — 8081. |Microsoft.Azure.Cosmos.Emulator.exe /Port=\<port\> | \<port:\> один номер порта. |
| ComputePort | Указанный номер порта для службы Compute Interop Gateway. Порт пробы для конечной точки HTTP шлюза вычисляется как ComputePort+79. Это означает, что должны быть открыты и свободны оба порта: ComputePort и ComputePort+79. Значение по умолчанию ― 8900. | Microsoft.Azure.Cosmos.Emulator.exe /ComputePort=\<computeport\> | \<computeport\>: один номер порта. |
| EnableMongoDbEndpoint=3.2 | Включает API MongoDB 3.2 | Microsoft.Azure.Cosmos.Emulator.exe /EnableMongoDbEndpoint=3.2 | |
| EnableMongoDbEndpoint=3.6 | Включает API MongoDB 3.6 | Microsoft.Azure.Cosmos.Emulator.exe /EnableMongoDbEndpoint=3.6 | |
| MongoPort | Указывает номер порта для использования с интерфейсом совместимости с MongoDB. Значение по умолчанию — 10255. |Microsoft.Azure.Cosmos.Emulator.exe /MongoPort=\<mongoport\>|\<mongoport:\> один номер порта.|
| EnableCassandraEndpoint | Включает API Cassandra | Microsoft.Azure.Cosmos.Emulator.exe /EnableCassandraEndpoint | |
| CassandraPort | Указывает номер порта для конечной точки Cassandra. Значение по умолчанию — 10350. | Microsoft.Azure.Cosmos.Emulator.exe /CassandraPort=\<cassandraport\> | \<cassandraport\>: один номер порта. |
| EnableGremlinEndpoint | Включает API Gremlin | Microsoft.Azure.Cosmos.Emulator.exe /EnableGremlinEndpoint | |
| GremlinPort | Номер порта для конечной точки Gremlin. Значение по умолчанию — 8901. | Microsoft.Azure.Cosmos.Emulator.exe /GremlinPort=\<port\> | \<port:\> один номер порта. |
|EnableTableEndpoint | Включает API таблиц Azure | Microsoft.Azure.Cosmos.Emulator.exe /EnableTableEndpoint | |
|TablePort | Номер порта для конечной точки таблицы Azure. Значение по умолчанию — 8902. | Microsoft.Azure.Cosmos.Emulator.exe /TablePort=\<port\> | \<port:\> один номер порта.|
| KeyFile | Считывает ключ авторизации из указанного файла. Используйте параметр /GenKeyFile, чтобы сгенерировать файл ключа. | Microsoft.Azure.Cosmos.Emulator.exe /KeyFile=\<file_name\> | \<file_name\>: Путь к файлу. |
| ResetDataPath | Рекурсивно удаляет файлы по указанному пути. Если путь не указан, используется путь по умолчанию %LOCALAPPDATA%\CosmosDbEmulator | Microsoft.Azure.Cosmos.Emulator.exe /ResetDataPath=\<path> | \<path\>: Путь к файлу  |
| StartTraces  |  Начинает сбор журналов трассировки для отладки с помощью LOGMAN. | Microsoft.Azure.Cosmos.Emulator.exe /StartTraces | |
| StopTraces     | Завершает сбор журналов трассировки для отладки с помощью LOGMAN. | Microsoft.Azure.Cosmos.Emulator.exe /StopTraces  | |
| StartWprTraces  |  Начинает сбор журналов трассировки для отладки с помощью средства записи производительности Windows. | Microsoft.Azure.Cosmos.Emulator.exe /StartWprTraces | |
| StopWprTraces     | Останавливает сбор журналов трассировки для отладки с помощью средства записи производительности Windows. | Microsoft.Azure.Cosmos.Emulator.exe /StopWprTraces  | |
|FailOnSslCertificateNameMismatch | По умолчанию эмулятор повторно создает собственный самозаверяющий SSL-сертификат, если альтернативное имя субъекта этого сертификата не содержит одно из следующих значений: доменное имя узла, локальный адрес IPv4, localhost или 127.0.0.1. С таким параметром эмулятор завершится ошибкой при запуске. В этом случае следует применить параметр /GenCert, чтобы создать и установить новый самозаверяющий сертификат SSL. | Microsoft.Azure.Cosmos.Emulator.exe /FailOnSslCertificateNameMismatch  | |
| GenCert | Создает и устанавливает новый самозаверяющий сертификат SSL. Можно добавить необязательный список дополнительны DNS-имен, разделенных запятыми, для доступа к эмулятору по сети. | Microsoft.Azure.Cosmos.Emulator.exe /GenCert=\<dns-names\> |\<dns-names\>: необязательный список дополнительны DNS-имен, разделенных запятыми  |
| DirectPorts |Указывает порты, которые нужно использовать для прямого подключения. По умолчанию это порты 10251, 10252, 10253, 10254. | Microsoft.Azure.Cosmos.Emulator.exe /DirectPorts:\<directports\> | \<directports:\> разделенный запятыми список из 4 портов. |
| Клавиши |Ключ проверки подлинности для эмулятора. Ключ должен иметь формат 64-разрядного вектора в кодировке Base-64. | Microsoft.Azure.Cosmos.Emulator.exe /Key:\<key\> | \<key:\> Ключ должен иметь формат 64-разрядного вектора в кодировке base-64|
| EnableRateLimiting | Указывает, что ограничение частоты запросов включено. |Microsoft.Azure.Cosmos.Emulator.exe /EnableRateLimiting | |
| DisableRateLimiting |Указывает, что ограничение частоты запросов отключено. |Microsoft.Azure.Cosmos.Emulator.exe /DisableRateLimiting | |
| NoUI | Скрывает пользовательский интерфейс эмулятора. | Microsoft.Azure.Cosmos.Emulator.exe /NoUI | |
| NoExplorer | Скрывает обозреватель данных при запуске. |Microsoft.Azure.Cosmos.Emulator.exe /NoExplorer | | 
| PartitionCount | Указывает максимальное количество секционированных контейнеров. Дополнительные сведения см. в разделе [Изменение количества контейнеров](#set-partitioncount). | Microsoft.Azure.Cosmos.Emulator.exe /PartitionCount=\<partitioncount\> | \<partitioncount:\> Максимально допустимое количество односекционных контейнеров. Значение по умолчанию равно 25. Максимально допустимое значение — 250.|
| DefaultPartitionCount| Указывает количество секций по умолчанию для секционированного контейнера. | Microsoft.Azure.Cosmos.Emulator.exe /DefaultPartitionCount=\<defaultpartitioncount\> | \<defaultpartitioncount\> Значение по умолчанию — 25.|
| AllowNetworkAccess | Разрешает доступ к эмулятору по сети. Для включения сетевого доступа нужно также передать один из параметров: /Key=\<строка_ключа\> или /KeyFile=\<имя_файла\>. | Microsoft.Azure.Cosmos.Emulator.exe /AllowNetworkAccess /Key=\<key_string\> или Microsoft.Azure.Cosmos.Emulator.exe /AllowNetworkAccess /KeyFile=\<file_name\>| |
| NoFirewall | Не изменяйте правила брандмауэра при использовании /AllowNetworkAccess. |Microsoft.Azure.Cosmos.Emulator.exe /NoFirewall | |
| GenKeyFile | Создает новый ключ авторизации и сохраняет его в указанном файле. Созданный ключ можно использовать с параметрами/Key или/KeyFile. | Microsoft.Azure.Cosmos.Emulator.exe /GenKeyFile=\<путь к файлу ключа\> | |
| Согласованность | Определяет уровень согласованности по умолчанию для учетной записи. | Microsoft.Azure.Cosmos.Emulator.exe /Consistency=\<consistency\> | \<consistency:\> значение должно быть одного из следующих [уровней согласованности](consistency-levels.md): Session, Strong, Eventual или BoundedStaleness. По умолчанию используется значение Session. |
| ? | Показывает справочные сообщения.| | |

## <a id="set-partitioncount"></a>Изменение количества контейнеров

По умолчанию можно создать до 25 контейнеров фиксированного размера (только пакеты SDK для Azure Cosmos DB) или 5 контейнеров неограниченного размера (эмулятор Azure Cosmos). Изменив значение **PartitionCount**, вы сможете создать до 250 контейнеров фиксированного размера или 50 контейнеров неограниченного размера либо любое их сочетание в пределах указанных ограничений (где один контейнер неограниченного размера приравнивается к 5 контейнерам фиксированного размера). Но мы не рекомендуем настраивать для эмулятора запуск более 200 контейнеров фиксированного размера. Дополнительное количество создаваемых ими операций ввода-вывода может привести к непредсказуемым превышениям времени ожидания при использовании API такой конечной точки.

При попытке создать контейнер, превышающий ограничение на количество секций, эмулятор генерирует исключение ServiceUnavailable со следующим сообщением:

Sorry, we are currently experiencing high demand in this region, and cannot fulfill your request at this time. We work continuously to bring more and more capacity online, and encourage you to try again.
ActivityId: 12345678-1234-1234-1234-123456789abc. (К сожалению, сейчас в этом регионе отмечается высокий спрос, и мы не можем выполнить ваш запрос. Мы постоянно работаем над повышением доступной емкости и рекомендуем вам попробовать еще раз. Вы можете обратиться к нам на адрес askcosmosdb@microsoft.com в любое время и по любому поводу.)

Чтобы изменить количество доступных для эмулятора Azure Cosmos DB контейнеров, сделайте следующее.

1. Удалите все локальные данные эмулятора Azure Cosmos, щелкнув правой кнопкой мыши значок **эмулятора Azure Cosmos DB** в области уведомлений и выбрав **Сброс данных**.
2. Удалите все данные эмулятора в папке `%LOCALAPPDATA%\CosmosDBEmulator`.
3. Выйдите из всех открытых экземпляров, щелкнув правой кнопкой мыши значок **эмулятора Azure Cosmos DB** в области уведомлений и выбрав **Выход**. Выход из всех экземпляров может занять около минуты.
4. Установите последнюю версию [эмулятора Azure Cosmos](https://aka.ms/cosmosdb-emulator).
5. Запустите эмулятор с флагом PartitionCount, задав значение <= 250. Например: `C:\Program Files\Azure Cosmos DB Emulator> Microsoft.Azure.Cosmos.Emulator.exe /PartitionCount=100`.

## <a name="controlling-the-emulator"></a>Управление эмулятором

Эмулятор поставляется вместе с модулем PowerShell, который позволяет запускать, останавливать службу и получать сведения о ее состоянии. Выполните следующий командлет, чтобы использовать модуль PowerShell:

```powershell
Import-Module "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules\Microsoft.Azure.CosmosDB.Emulator"
```

Или поместите каталог `PSModules` в `PSModulesPath` и импортируйте его с помощью следующей команды:

```powershell
$env:PSModulesPath += "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules"
Import-Module Microsoft.Azure.CosmosDB.Emulator
```

Ниже приводится сводка команд для управления эмулятором из PowerShell:

### `Get-CosmosDbEmulatorStatus`

**Синтаксис**

`Get-CosmosDbEmulatorStatus`

**Замечания**

Возвращает одно из следующих значений ServiceControllerStatus: ServiceControllerStatus.StartPending, ServiceControllerStatus.Running или ServiceControllerStatus.Stopped.

### `Start-CosmosDbEmulator`

**Синтаксис**

`Start-CosmosDbEmulator [-DataPath <string>] [-DefaultPartitionCount <uint16>] [-DirectPort <uint16[]>] [-MongoPort <uint16>] [-NoUI] [-NoWait] [-PartitionCount <uint16>] [-Port <uint16>] [<CommonParameters>]`

**Замечания**

Запускает эмулятор. По умолчанию команда ожидает, когда эмулятор будет готов к приему запросов. Используйте параметр -NoWait, если командлет должен вернуть результат сразу после запуска эмулятора.

### `Stop-CosmosDbEmulator`

**Синтаксис**

 `Stop-CosmosDbEmulator [-NoWait]`

**Замечания**

Останавливает работу эмулятора. По умолчанию эта команда ожидает, когда эмулятор окончательно завершит работу. Используйте параметр -NoWait, если командлет должен вернуть результат сразу после перехода эмулятора в автономный режим.

### `Uninstall-CosmosDbEmulator`

**Синтаксис**

`Uninstall-CosmosDbEmulator [-RemoveData]`

**Замечания**

Удаляет эмулятор и при необходимости все содержимое $env:LOCALAPPDATA\CosmosDbEmulator.
Командлет гарантирует остановку работы эмулятора перед удалением.

## <a name="running-on-docker"></a>Запуск в Docker

Эмулятор Azure Cosmos может выполняться в Docker для Windows. Этот эмулятор не работает в Docker для Oracle Linux.

Установив [Docker для Windows](https://www.docker.com/docker-windows), переключитесь на контейнеры Windows. Для этого на панели инструментов щелкните правой кнопкой мыши значок Docker и выберите **Switch to Windows containers** (Переключиться на контейнеры Windows).

Затем извлеките образ эмулятора из Docker Hub, выполнив следующую команду в любой оболочке.

```bash
docker pull mcr.microsoft.com/cosmosdb/windows/azure-cosmos-emulator
```
Чтобы запустить образ, выполните следующие команды:

Из командной строки:
```cmd

md %LOCALAPPDATA%\CosmosDBEmulator\bind-mount

docker run --name azure-cosmosdb-emulator --memory 2GB --mount "type=bind,source=%LOCALAPPDATA%\CosmosDBEmulator\bind-mount,destination=C:\CosmosDB.Emulator\bind-mount" --interactive --tty -p 8081:8081 -p 8900:8900 -p 8901:8901 -p 8902:8902 -p 10250:10250 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254 -p 10255:10255 -p 10256:10256 -p 10350:10350 mcr.microsoft.com/cosmosdb/windows/azure-cosmos-emulator
```

> [!NOTE]
> Если при выполнении команды docker run отображается ошибка конфликта портов (указанный порт уже используется), можно передать пользовательский порт, изменив номера портов. Например, можно изменить -p 8081:8081 на -p 443:8081.

Из PowerShell:
```powershell

md $env:LOCALAPPDATA\CosmosDBEmulator\bind-mount 2>null

docker run --name azure-cosmosdb-emulator --memory 2GB --mount "type=bind,source=$env:LOCALAPPDATA\CosmosDBEmulator\bind-mount,destination=C:\CosmosDB.Emulator\bind-mount" --interactive --tty -p 8081:8081 -p 8900:8900 -p 8901:8901 -p 8902:8902 -p 10250:10250 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254 -p 10255:10255 -p 10256:10256 -p 10350:10350 mcr.microsoft.com/cosmosdb/windows/azure-cosmos-emulator

```

Ответ выглядит примерно так:

```
Starting emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import the SSL certificate from an administrator command prompt on the host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
```

Теперь воспользуйтесь конечной точкой и главным ключом из ответа в клиенте и импортируйте SSL-сертификат на узел. Чтобы импортировать SSL-сертификат, выполните следующие действия из командной строки с правами администратора.

Из командной строки:

```cmd
cd  %LOCALAPPDATA%\CosmosDBEmulator\bind-mount
powershell .\importcert.ps1
```

Из PowerShell:
```powershell
cd $env:LOCALAPPDATA\CosmosDBEmulator\bind-mount
.\importcert.ps1
```

Если закрыть интерактивную оболочку после запуска эмулятора, его контейнер завершит работу.

Чтобы открыть обозреватель данных, перейдите по приведенному ниже URL-адресу в браузере. Конечная точка эмулятора отобразится в сообщении ответа, приведенном выше.

    https://<emulator endpoint provided in response>/_explorer/index.html

Если у вас есть клиентское приложение .NET, работающее в контейнере docker для Linux, и вы используете эмулятор Cosmos Azure на хост-компьютере, для импорта сертификата в контейнер docker в Linux используйте следующий раздел для Linux.

## Запуск на компьютере Mac или Linux<a id="mac"></a>

В настоящее время эмулятор Cosmos можно запустить только на компьютере с Windows. Пользователи компьютеров Mac или Linux могут запускать эмулятор на виртуальной машине Windows, на которой размещен гипервизор, такой как Parallels или VirtualBox. Ниже приведены шаги по реализации.

На виртуальной машине Windows выполните приведенную ниже команду и запишите адрес IPv4.

```cmd
ipconfig.exe
```

В вашем приложении нужно изменить URI, используемый в качестве конечной точки, чтобы использовать адрес IPv4, возвращаемый `ipconfig.exe` вместо `localhost`.

Затем на виртуальной машине Windows запустите эмулятор Cosmos из командной строки, используя следующие параметры.

```cmd
Microsoft.Azure.Cosmos.Emulator.exe /AllowNetworkAccess /Key=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
```

Наконец, сертификат ЦС эмулятора необходимо импортировать в среду Linux или Mac.

### <a name="linux"></a>Linux

При работе в Linux для выполнения проверки .NET полагается на OpenSSL:

1. выполните действие раздела [Как экспортировать SSL-сертификаты эмулятора Azure Cosmos DB](./local-emulator-export-ssl-certificates.md#how-to-export-the-azure-cosmos-db-ssl-certificate) (PFX-файл доступен при выборе экспорта закрытого ключа). 

1. Скопируйте этот PFX-файл в среду Linux.

1. Преобразуйте PFX-файл в CRT-файл

   ```bash
   openssl pkcs12 -in YourPFX.pfx -clcerts -nokeys -out YourCTR.crt
   ```

1. Скопируйте CRT-файл в папку, содержащую пользовательские сертификаты в дистрибутиве Linux. Обычно в дистрибутивах Debian она находится в `/usr/local/share/ca-certificates/`.

   ```bash
   cp YourCTR.crt /usr/local/share/ca-certificates/
   ```

1. Обновите сертификаты ЦС, которые будут обновлять папку `/etc/ssl/certs/`.

   ```bash
   update-ca-certificates
   ```

### <a name="mac-os"></a>MacOS

При работе на компьютере Mac выполните следующие действия.

1. выполните действие раздела [Как экспортировать SSL-сертификаты эмулятора Azure Cosmos DB](./local-emulator-export-ssl-certificates.md#how-to-export-the-azure-cosmos-db-ssl-certificate) (PFX-файл доступен при выборе экспорта закрытого ключа).

1. Скопируйте этот PFX-файл в среду Mac.

1. Откройте приложение *Keychain Access* (Доступ к цепочке ключей) и импортируйте PFX-файл.

1. Откройте список сертификатов и найдите сертификат с именем `localhost`.

1. Откройте контекстное меню для этого конкретного элемента, выберите *Получить элемент* и в разделе *Доверие*, в параметре  > *When using this certificate* (Во время использовании этого сертификата) выберите *Always Trust* (Всегда доверять). 

   ![Откройте контекстное меню для этого конкретного элемента, выберите "Получить элемент" и в разделе "Доверие", в параметре "When using this certificate" (Во время использовании этого сертификата) выберите "Always Trust" (Всегда доверять).](./media/local-emulator/mac-trust-certificate.png)

После выполнения этих действий ваша среда будет доверять сертификату, который используется эмулятором, при подключении к IP-адресу, предоставляемому `/AllowNetworkAccess`.

## <a name="troubleshooting"></a>Устранение неполадок

Ниже перечислены способы устранения неполадок с эмулятором Azure Cosmos.

- Если установлена новая версия эмулятора и возникли ошибки, убедитесь, что вы сбросили данные. Вы можете сбросить данные, щелкнув правой кнопкой мыши значок эмулятора Azure Cosmos в области уведомлений и выбрав "Сбросить данные". Если это не поможет устранить проблемы, вы можете удалить эмулятор и все его старые версии, затем удалить каталог C:\Program files\Azure Cosmos DB Emulator и повторно установить эмулятор. Инструкции см. в разделе [Удаление локального эмулятора](#uninstall).

- В случае аварийного завершения эмулятора Azure Cosmos DB соберите файлы резервной копии из папки %LOCALAPPDATA%\CrashDumps, сожмите их и создайте запрос в службу поддержки на портале [Azure](https://portal.azure.com).

- Если в `Microsoft.Azure.Cosmos.ComputeServiceStartupEntryPoint.exe` возникают сбои, это может означать нарушение состояния счетчиков производительности. Обычно такую проблему удается исправить, выполнив следующую команду из командной строки с правами администратора:

  ```cmd
  lodctr /R
   ```

- Если возникли проблемы с подключением, [соберите файлы трассировки](#trace-files), сожмите их и создайте запрос в службу поддержки на портале [Azure](https://portal.azure.com).

- При получении сообщения **Служба недоступна** эмулятор может не инициализировать сетевой стек. Проверьте, установлен ли защищенный клиент Pulse или сетевой клиент Juniper, так как драйверы их сетевого фильтра могут вызвать проблему. Удаление драйверов сетевого фильтра сторонних производителей обычно устраняет проблему. Также можно запустить эмулятор с параметром /DisableRIO, который настраивает для сетевого взаимодействия эмулятора использование Winsock. 

- Если во время работы эмулятора компьютер переходит в ждущий режим или запускает какие-либо обновления операционной системы, может появиться сообщение **Service is currently unavailable** (Служба сейчас недоступна). Сбросьте данные эмулятора, щелкнув правой кнопкой мыши значок, который отображается в области уведомлений Windows, и выбрав действие **Сбросить данные**.

### <a id="trace-files"></a>Сбор файлов трассировки

Для сбора отладочных трассировок выполните следующие команды в командной строке с правами администратора.

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. `Microsoft.Azure.Cosmos.Emulator.exe /shutdown`. Проверьте область уведомлений, чтобы убедиться, что работа программы завершена, так как на это может потребоваться до минуты. Кроме того, можно просто щелкнуть **Выйти** в пользовательском интерфейсе эмулятора Azure Cosmos.
3. `Microsoft.Azure.Cosmos.Emulator.exe /startwprtraces`
4. `Microsoft.Azure.Cosmos.Emulator.exe`
5. Воспроизведите проблему. Если обозреватель данных не работает, необходимо подождать несколько секунд, чтобы открылся браузер и ошибка возникла снова.
6. `Microsoft.Azure.Cosmos.Emulator.exe /stopwprtraces`
7. Перейдите к `%ProgramFiles%\Azure Cosmos DB Emulator` и найдите файл docdbemulator_000001.etl.
8. Создайте запрос в службу поддержки на [портале Azure](https://portal.azure.com) и добавьте ETL-файл вместе с действиями по воспроизведению.

### <a id="uninstall"></a>Удаление локального эмулятора

1. Выйдите из всех открытых экземпляров локального эмулятора, щелкнув правой кнопкой мыши значок эмулятора Azure Cosmos в области уведомлений и выбрав "Выйти". Выход из всех экземпляров может занять около минуты.
2. Введите в поле поиска Windows **Приложения и возможности** и выберите команду **Apps & features (System settings)** (Приложения и возможности (системные параметры)).
3. В списке приложений перейдите к **эмулятору Azure Cosmos DB**, выберите его, щелкните **Удалить**, подтвердите и щелкните **Удалить** еще раз.
4. После удаления приложения перейдите к папке `%LOCALAPPDATA%\CosmosDBEmulator` удалите ее.

## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы также узнали, как использовать локальный эмулятор для бесплатной разработки в локальной среде. Теперь вы можете перейти к следующему руководству, из которого вы узнаете, как экспортировать SSL-сертификаты эмулятора.

> [!div class="nextstepaction"]
> [Экспорт сертификатов эмулятора Azure Cosmos](local-emulator-export-ssl-certificates.md)
