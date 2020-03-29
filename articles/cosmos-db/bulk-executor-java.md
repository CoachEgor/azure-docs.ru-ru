---
title: Используйте библиотеку java-исполнителя сыпучих исполнителей в Azure Cosmos DB для выполнения операций по массовому импорту и обновлению
description: Массовый импорт и обновление документов Azure Cosmos DB с использованием библиотеки Java-исполнителя
author: tknandu
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: ramkris
ms.reviewer: sngun
ms.openlocfilehash: bf2a2385b3129ddf24ede7f6d851701186b0e33c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "75445710"
---
# <a name="use-bulk-executor-java-library-to-perform-bulk-operations-on-azure-cosmos-db-data"></a>Выполнение массовых операций с данными Azure Cosmos DB с помощью библиотеки Java массового исполнителя

В этом руководстве приведены инструкции по использованию библиотеки BulkExecutor Java в Azure Cosmos DB для импорта и обновления документов Azure Cosmos DB. Подробнее о библиотеке массового исполнителя и о том, как с ее помощью эффективно использовать пропускную способность и хранилище, можно узнать в статье с [общими сведениями о библиотеке массового исполнителя](bulk-executor-overview.md). В этом учебнике вы создаете Java-приложение, которое генерирует случайные документы, и они навалом импортируются в контейнер Azure Cosmos. После импорта вы массово обновите некоторые свойства документа. 

В настоящее время основная библиотека исполнителей поддерживается только учетными записями Azure Cosmos DB S'L API и Gremlin API. В этой статье описывается, как использовать библиотеку Java-исполнителя с учетными записями API S'L. Дополнительные сведения об использовании библиотеки массового исполнителя .NET с помощью API Gremlin см. в разделе [Использование библиотеки BulkExecutor .NET для выполнения массовых операций с графами в API Gremlin в Azure Cosmos DB](bulk-executor-graph-dotnet.md).

## <a name="prerequisites"></a>Предварительные требования

* Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), прежде чем начинать работу.  

* Вы можете [попробовать Azure Cosmos DB бесплатно](https://azure.microsoft.com/try/cosmosdb/) без подписки Azure, бесплатно и без обязательств. Или вы можете применить [эмулятор Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/local-emulator) с конечной точкой `https://localhost:8081`. Первичный ключ предоставляется в разделе [Выполнение проверки подлинности запросов](local-emulator.md#authenticating-requests).  

* [Комплект разработки Java (JDK) 1.7](https://aka.ms/azure-jdks)  
  - В Ubuntu выполните команду `apt-get install default-jdk`, чтобы установить JDK.  

  - Обязательно настройте переменную среды JAVA_HOME так, чтобы она указывала на папку, в которой установлен пакет JDK.

* [Скачать](https://maven.apache.org/download.cgi) и [установить](https://maven.apache.org/install.html) двоичный архив [Maven](https://maven.apache.org/)  
  
  - В Ubuntu выполните команду `apt-get install maven`, чтобы установить Maven.

* Создайте учетную запись API Azure Cosmos DB S'L, используя шаги, описанные в разделе [учетной записи создания](create-sql-api-java.md#create-a-database-account) статьи быстрого запуска Java.

## <a name="clone-the-sample-application"></a>Клонирование примера приложения

Теперь перейдем к работе с кодом, скачав пример приложения Java из GitHub. Это приложение выполняют массовые операции с данными Azure Cosmos DB. Чтобы клонировать приложения, откройте командную строку, перейдите в каталог, в который вы хотите его скопировать, и выполните следующую команду:

```
 git clone https://github.com/Azure/azure-cosmosdb-bulkexecutor-java-getting-started.git 
```

Клонированный репозиторий содержит два примера bulkimport и bulkupdate, указанных относительно папки \azure-cosmosdb-bulkexecutor-java-getting-started\samples\bulkexecutor-sample\src\main\java\com\microsoft\azure\cosmosdb\bulkexecutor. Приложение bulkimport создает случайные документы и импортирует их в Azure Cosmos DB. Приложение bulkupdate обновляет некоторые документы в Azure Cosmos DB. В следующих разделах мы рассмотрим код в каждом из этих примеров приложений. 

## <a name="bulk-import-data-to-azure-cosmos-db"></a>Массовый импорт данных в Azure Cosmos DB

1. Строки подключения Azure Cosmos DB считываются в качестве аргументов и присваиваются переменным, определенным в файле CmdLineConfiguration.java.  

2. Далее инициализируется объект DocumentClient с помощью следующих операторов:  

   ```java
   ConnectionPolicy connectionPolicy = new ConnectionPolicy();
   connectionPolicy.setMaxPoolSize(1000);
   DocumentClient client = new DocumentClient(
      HOST,
      MASTER_KEY, 
      connectionPolicy,
      ConsistencyLevel.Session)
   ```

3. Объект DocumentBulkExecutor инициализируется с высокими значениями повторения для запросов времени ожидания и регулируемых запросов. Далее для них устанавливается значение 0, чтобы передать управление перегрузкой DocumentBulkExecutor на время его существования.  

   ```java
   // Set client's retry options high for initialization
   client.getConnectionPolicy().getRetryOptions().setMaxRetryWaitTimeInSeconds(30);
   client.getConnectionPolicy().getRetryOptions().setMaxRetryAttemptsOnThrottledRequests(9);

   // Builder pattern
   Builder bulkExecutorBuilder = DocumentBulkExecutor.builder().from(
     client,
     DATABASE_NAME,
     COLLECTION_NAME,
     collection.getPartitionKey(),
     offerThroughput) // throughput you want to allocate for bulk import out of the container's total throughput

   // Instantiate DocumentBulkExecutor
   DocumentBulkExecutor bulkExecutor = bulkExecutorBuilder.build()

   // Set retries to 0 to pass complete control to bulk executor
   client.getConnectionPolicy().getRetryOptions().setMaxRetryWaitTimeInSeconds(0);
   client.getConnectionPolicy().getRetryOptions().setMaxRetryAttemptsOnThrottledRequests(0);
   ```

4. Вызовите импортAll API, который генерирует случайные документы для массового импорта в контейнер Azure Cosmos. Вы можете настроить конфигурации командной строки в файле CmdLineConfiguration.java.

   ```java
   BulkImportResponse bulkImportResponse = bulkExecutor.importAll(documents, false, true, null);
   ```
   API массового импорта принимает коллекцию сериализованных документов JSON и имеет указанный ниже синтаксис. Дополнительные сведения см. в [документации по API](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor).

   ```java
   public BulkImportResponse importAll(
        Collection<String> documents,
        boolean isUpsert,
        boolean disableAutomaticIdGeneration,
        Integer maxConcurrencyPerPartitionRange) throws DocumentClientException;   
   ```

   Метод importAll принимает следующие параметры:
 
   |**Параметр**  |**Описание**  |
   |---------|---------|
   |isUpsert    |   Параметр, активирующий операцию upsert в документах. Если документ с заданным идентификатором уже существует, он обновляется.  |
   |disableAutomaticIdGeneration     |   Параметр, отключающий автоматическое создание идентификатора. По умолчанию установлено значение true.   |
   |maxConcurrencyPerPartitionRange    |  Максимальная степень параллелизма для одного диапазона ключей разделов. Значение по умолчанию — 20.  |

   **Определение объекта ответа массового импорта**. Результат вызова API массового импорта содержит следующие методы get:

   |**Параметр**  |**Описание**  |
   |---------|---------|
   |int getNumberOfDocumentsImported()  |   Общее число документов, которые были успешно импортированы, среди предоставленных для вызова API массового импорта.      |
   |double getTotalRequestUnitsConsumed()   |  Общее число единиц запросов (ЕЗ), потребляемых вызовом API массового импорта.       |
   |Duration getTotalTimeTaken()   |    Общее время, затрачиваемое на выполнение вызова API массового импорта.     |
   |Список\<Исключений> getErrors() |  Получает список ошибок, если не удается вставить некоторые документы за пределами пакета, предоставленного вызову API массового импорта.       |
   |Список\<объектов> getBadInputDocuments()  |    Список документов с неверным форматом, которые не были успешно импортированы при вызове API массового импорта. Пользователь должен исправить возвращенные документы и повторить импорт. Документы с неверным форматом включают в себя документы, значение идентификаторов которых не является строкой (NULL или любой другой тип данных считается недействительным).     |

5. После того, как приложение массового импорта будет готово, создайте программу командной строки из источника, выполнив команду mvn clean package. Эта команда создает JAR-файл в целевой папке:  

   ```java
   mvn clean package
   ```

6. После создания целевых зависимостей вы можете вызвать приложения массового импорта, выполнив следующую команду:  

   ```java
   java -Xmx12G -jar bulkexecutor-sample-1.0-SNAPSHOT-jar-with-dependencies.jar -serviceEndpoint *<Fill in your Azure Cosmos DB’s endpoint>*  -masterKey *<Fill in your Azure Cosmos DB’s master key>* -databaseId bulkImportDb -collectionId bulkImportColl -operation import -shouldCreateCollection -collectionThroughput 1000000 -partitionKey /profileid -maxConnectionPoolSize 6000 -numberOfDocumentsForEachCheckpoint 1000000 -numberOfCheckpoints 10
   ```

   Средство массового импорта создает базу данных и коллекцию с именем базы данных, именем коллекции и значениями пропускной способности, указанными в файле App.config. 

## <a name="bulk-update-data-in-azure-cosmos-db"></a>Массовое обновление данных в Azure Cosmos DB

С помощью API BulkUpdateAsync можно обновить имеющиеся документы. В этом примере вы присвоите для поля Name новое значение и удалите поле Description из существующих документов. Полный набор поддерживаемых операции обновления полей см. в [документации по API](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor). 

1. Определите элементы для обновления вместе с соответствующими операциями обновления полей. В этом примере используйте SetUpdateOperation для обновления поля Name и UnsetUpdateOperation, чтобы удалить поле Description из всех документов. Вы также можете выполнять другие операции, такие как увеличение значения в поле документа по определенному значению, передача определенных значений в поле массива или удаление определенного значения из него. Дополнительные сведения о различных методах, предоставляемых API массового обновления, см. в [документации по API](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor).  

   ```java
   SetUpdateOperation<String> nameUpdate = new SetUpdateOperation<>("Name","UpdatedDocValue");
   UnsetUpdateOperation descriptionUpdate = new UnsetUpdateOperation("description");

   ArrayList<UpdateOperationBase> updateOperations = new ArrayList<>();
   updateOperations.add(nameUpdate);
   updateOperations.add(descriptionUpdate);

   List<UpdateItem> updateItems = new ArrayList<>(cfg.getNumberOfDocumentsForEachCheckpoint());
   IntStream.range(0, cfg.getNumberOfDocumentsForEachCheckpoint()).mapToObj(j -> {                      
    return new UpdateItem(Long.toString(prefix + j), Long.toString(prefix + j), updateOperations);
    }).collect(Collectors.toCollection(() -> updateItems));
   ```

2. Вызовите updateAll API, который генерирует случайные документы, которые затем навалом импортируются в контейнер Azure Cosmos. Вы можете настроить конфигурации командной строки, которые будут передаваться в файл CmdLineConfiguration.java.

   ```java
   BulkUpdateResponse bulkUpdateResponse = bulkExecutor.updateAll(updateItems, null)
   ```

   API массового обновления принимает коллекцию обновляемых элементов. Каждый обновленный элемент указывает список операций обновлений поля, которые будут выполнены в документе, определенном значением идентификатора и ключа раздела. Дополнительные сведения см. в [документации по API](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor).

   ```java
   public BulkUpdateResponse updateAll(
        Collection<UpdateItem> updateItems,
        Integer maxConcurrencyPerPartitionRange) throws DocumentClientException;
   ```

   Метод updateAll принимает следующие параметры:

   |**Параметр** |**Описание** |
   |---------|---------|
   |maxConcurrencyPerPartitionRange   |  Максимальная степень параллелизма для одного диапазона ключей разделов. Значение по умолчанию — 20.  |
 
   **Определение объекта ответа массового импорта**. Результат вызова API массового импорта содержит следующие методы get:

   |**Параметр** |**Описание**  |
   |---------|---------|
   |int getNumberOfDocumentsUpdated()  |   Общее число документов, которые были успешно обновлены из документов, предоставленных для вызова API массового обновления.      |
   |double getTotalRequestUnitsConsumed() |  Общее число единиц запросов (ЕЗ), потребляемых вызовом API массового обновления.       |
   |Duration getTotalTimeTaken()  |   Общее время, затрачиваемое на выполнение вызова API массового обновления.      |
   |Список\<Исключений> getErrors()   |    Получает список ошибок, если не удается вставить некоторые документы за пределами пакета, предоставленного вызову API массового обновления.      |

3. После того, как приложение массового обновления будет готово, создайте программу командной строки из источника, выполнив команду mvn clean package. Эта команда создает JAR-файл в целевой папке:  

   ```
   mvn clean package
   ```

4. После создания целевых зависимостей вы можете вызвать приложения массового обновления, выполнив следующую команду:

   ```
   java -Xmx12G -jar bulkexecutor-sample-1.0-SNAPSHOT-jar-with-dependencies.jar -serviceEndpoint **<Fill in your Azure Cosmos DB’s endpoint>* -masterKey **<Fill in your Azure Cosmos DB’s master key>* -databaseId bulkUpdateDb -collectionId bulkUpdateColl -operation update -collectionThroughput 1000000 -partitionKey /profileid -maxConnectionPoolSize 6000 -numberOfDocumentsForEachCheckpoint 1000000 -numberOfCheckpoints 10
   ```

## <a name="performance-tips"></a>Советы по улучшению производительности 

Учтите следующие моменты для повышения производительности при использовании библиотеки массового исполнителя:

* Для достижения оптимальной производительности запустите приложение на виртуальной машине Azure в том же регионе, где находится регион записи вашей учетной записи Cosmos DB.  
* Чтобы получить более высокую пропускную способность, сделайте следующее:  

   * Установите для размера кучи виртуальной машины Java достаточно высокое значение, чтобы избежать проблем с памятью при обработке большого количества документов. Рекомендуемый размер кучи: максимум (3 ГБ, 3 * sizeof (все документы, переданные в API массового импорта в одном пакете)).  
   * Имеется время предварительной обработки, из-за которого вы получаете более высокую пропускную способность при выполнении массовых операций с большим количеством документов. Таким образом, если вы хотите импортировать 10 000 000 документов, лучше всего 10 раз выполнить массовый импорт 10 пакетов документов, размер которых соответствует 1 000 000 документов, чем 100 раз выполнить массовый импорт 100 пакетов документов, размер которых соответствует 100 000 документов.  

* Рекомендуется мгновенно использовать один объект DocumentBulkExcutor для всего приложения в рамках одной виртуальной машины, которая соответствует определенному контейнеру Azure Cosmos.  

* Это полезно, так как однократное выполнение API массовой операции потребляет большую часть ресурсов ЦП и операций ввода-вывода сети клиентского компьютера. Это происходит при порождении нескольких задач изнутри. Избегайте появления в процессе приложения нескольких параллельных задач, каждая из которых выполняет вызовы API массовой операции. Если один вызов API массовой операции, который выполняется на одной виртуальной машине, не может использовать всю пропускную способность вашего контейнера (если пропускная способность контейнера > 1 млн ЕЗ/с), предпочтительнее создавать отдельные виртуальные машины для одновременного выполнения вызовов API.

    
## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения о пакете Maven и заметки о выпуске библиотеки Java массового исполнителя см. в статье [со сведениями о пакете SDK массового исполнителя](sql-api-sdk-bulk-executor-java.md).


