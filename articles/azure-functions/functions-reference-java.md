---
title: Справочник разработчика Java для функций Azure
description: Информация о разработке функций на языке Java.
ms.topic: conceptual
ms.date: 09/14/2018
ms.openlocfilehash: 19a290fe7717d7838e8fcd1d1f5cddb3f54eb812
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82145334"
---
# <a name="azure-functions-java-developer-guide"></a>Руководство разработчика Java по Функциям Azure

Среда выполнения функций Azure поддерживает [Java SE 8 LTS (Zulu 8.31.0.2-JRE 8.0.181-win_x64)](https://repos.azul.com/azure-only/zulu/packages/zulu-8/8u181/). В этом руководства содержатся сведения об тонкостях написания функций Azure с помощью Java.

Как и в случае с другими языками, приложение-функция может иметь одну или несколько функций. Функция Java — это `public` метод, дополненный заметкой `@FunctionName`. Этот метод определяет запись для функции Java и должен быть уникальным в определенном пакете. Один приложение-функция, написанный на языке Java, может иметь несколько классов с несколькими открытыми `@FunctionName`методами, заметками которых является.

В этой статье предполагается, что вы уже прочли [руководство для разработчиков по Функциям Azure](functions-reference.md). Кроме того, вы должны заполнить краткое руководство по функциям, чтобы создать свою первую функцию с помощью [Visual Studio Code](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-java) или [Maven](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-java).

## <a name="programming-model"></a>Модель программирования 

Понятия [триггеров и привязок](functions-triggers-bindings.md) играют решающую роль в Функциях Azure. Триггеры запускают выполнение вашего кода. Привязки предоставляют возможность передавать данные, а также возвращать их из функции без необходимости написания кода доступа к ним.

## <a name="create-java-functions"></a>Создание функций Java

Чтобы упростить создание функций Java, существуют средства на основе Maven и архетипов, использующие стандартные шаблоны Java для создания проектов с определенным триггером функции.    

### <a name="maven-based-tooling"></a>Инструментарий на основе Maven

В следующих средах разработки есть Инструментарий функций Azure, позволяющий создавать проекты функций Java: 

+ [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions)
+ [Eclipse](functions-create-maven-eclipse.md)
+ [IntelliJ](functions-create-maven-intellij.md)

В статье выше показано, как создать первые функции с помощью вашей интегрированной среды разработки. 

### <a name="project-scaffolding"></a>Формирование шаблонов проектов

Если вы предпочитаете выполнять разработку из командной строки с помощью терминала, самый простой способ формирования шаблонов проектов функций на основе Java `Apache Maven` — использовать архетипов. Функция Java Maven архетипа for Azure опубликована в следующем списке _groupId_:_ArtifactId_: [com. Microsoft. Azure: Azure-functions-архетипа](https://search.maven.org/artifact/com.microsoft.azure/azure-functions-archetype/). 

Следующая команда создает новый проект функции Java с помощью этого архетипа:

```
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

Чтобы приступить к работе с этой архетипа, ознакомьтесь с [кратким](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-java)руководством по Java. 

## <a name="create-kotlin-functions-preview"></a>Создание функций Котлин (Предварительная версия)

Существует также Maven архетипа для создания функций Котлин. Этот архетипа, который сейчас находится в предварительной версии, опубликован в следующем каталоге _groupId_:_ArtifactId_: [com. Microsoft. Azure: Azure-functions-Котлин-архетипа](https://search.maven.org/artifact/com.microsoft.azure/azure-functions-kotlin-archetype/). 

Следующая команда создает новый проект функции Java с помощью этого архетипа:

```
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-kotlin-archetype
```

Чтобы приступить к работе с этой архетипа, ознакомьтесь с [кратким](functions-create-first-kotlin-maven.md)руководством по Котлин.

## <a name="folder-structure"></a>Структура папок

Ниже приведена структура папок проекта Java для функций Azure.

```
FunctionsProject
 | - src
 | | - main
 | | | - java
 | | | | - FunctionApp
 | | | | | - MyFirstFunction.java
 | | | | | - MySecondFunction.java
 | - target
 | | - azure-functions
 | | | - FunctionApp
 | | | | - FunctionApp.jar
 | | | | - host.json
 | | | | - MyFirstFunction
 | | | | | - function.json
 | | | | - MySecondFunction
 | | | | | - function.json
 | | | | - bin
 | | | | - lib
 | - pom.xml
```

_* Проект Котлин выглядит очень похожим, так как он все еще Maven_

Для настройки приложения-функции можно использовать общий файл [Host. JSON](functions-host-json.md) . У каждой функции есть собственный файл кода (.java) и файл конфигурации привязки (function.json).

Вы можете добавить несколько функций в проект. Не добавляйте функции в отдельные JAR-файлы. В `FunctionApp` целевом каталоге это то, что развертывается в приложении-функции в Azure.

## <a name="triggers-and-annotations"></a>Триггеры и заметки

 Функции вызываются триггером, например HTTP-запросом, таймером или обновлением данных. Функция должна обработать этот триггер, а также любые другие входные данные, чтобы получить один или несколько выходов.

Используйте заметки Java, включенные в пакет [com.microsoft.azure.functions.annotation.*](/java/api/com.microsoft.azure.functions.annotation), чтобы привязать входные и выходные данные к своим методам. Дополнительные сведения см. в [справочных документах по Java](/java/api/com.microsoft.azure.functions.annotation).

> [!IMPORTANT] 
> Учетную запись хранения Azure необходимо настроить в [локальном. Settings. JSON](/azure/azure-functions/functions-run-local#local-settings-file) для запуска хранилища BLOB-объектов Azure, хранилища очередей Azure или для локальных триггеров хранилища таблиц Azure.

Пример:

```java
public class Function {
    public String echo(@HttpTrigger(name = "req", 
      methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
        String req, ExecutionContext context) {
        return String.format(req);
    }
}
```

Ниже приведена созданная `function.json` соответствующая [функция Azure-functions-maven-plugin](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-maven-plugin):

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.Function.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "post" ]
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}

```

## <a name="jdk-runtime-availability-and-support"></a>Обеспечения доступности и предоставления поддержки времени выполнения пакета JDK 

Для локальной разработки приложений с функциями Java Скачайте и используйте [Azul Zulu Enterprise для Azure](https://assets.azul.com/files/Zulu-for-Azure-FAQ.pdf) Java 8 JDK от [Azul Systems](https://www.azul.com/downloads/azure-only/zulu/). Функции Azure используют среду выполнения Azul Java 8 JDK, если вы развертываете приложения-функции в облако.

[Поддержка Azure](https://azure.microsoft.com/support/) для проблем, связанных с JDK и приложениями-функциями, доступна в [полном плане поддержки](https://azure.microsoft.com/support/plans/).

## <a name="customize-jvm"></a>Настройка ВИРТУАЛЬНОЙ машины Java

Функции позволяют настроить виртуальную машину Java (ВИРТУАЛЬНОЙ машины Java), используемую для выполнения функций Java. По умолчанию используются [следующие параметры виртуальной машины Java](https://github.com/Azure/azure-functions-java-worker/blob/master/worker.config.json#L7) :

* `-XX:+TieredCompilation`
* `-XX:TieredStopAtLevel=1`
* `-noverify` 
* `-Djava.net.preferIPv4Stack=true`
* `-jar`

Дополнительные аргументы можно указать в параметре приложения с именем `JAVA_OPTS`. Вы можете добавить параметры приложения в приложение функции, развернутое в Azure, в портал Azure или Azure CLI.

> [!IMPORTANT]  
> В плане потребления необходимо также добавить параметр WEBSITE_USE_PLACEHOLDER со значением 0, чтобы настройка работала. Этот параметр увеличивает время холодного запуска для функций Java.

### <a name="azure-portal"></a>Портал Azure

В [портал Azure](https://portal.azure.com)используйте [вкладку Параметры приложения](functions-how-to-use-azure-function-app-settings.md#settings) , чтобы добавить `JAVA_OPTS` параметр.

### <a name="azure-cli"></a>Azure CLI

Для установки `JAVA_OPTS`можно использовать команду [AZ functionapp config appSettings Set](/cli/azure/functionapp/config/appsettings) , как показано в следующем примере:

#### <a name="consumption-plan"></a>[План потребления](#tab/consumption)
```azurecli-interactive
az functionapp config appsettings set \
--settings "JAVA_OPTS=-Djava.awt.headless=true" \
"WEBSITE_USE_PLACEHOLDER=0" \
--name <APP_NAME> --resource-group <RESOURCE_GROUP>
```
#### <a name="dedicated-plan--premium-plan"></a>[План выделенного плана/Premium](#tab/dedicated+premium)
```azurecli-interactive
az functionapp config appsettings set \
--settings "JAVA_OPTS=-Djava.awt.headless=true" \
--name <APP_NAME> --resource-group <RESOURCE_GROUP>
```
---

Этот пример включает режим без монитора. Замените `<APP_NAME>` именем приложения функции и `<RESOURCE_GROUP>` группой ресурсов. 

## <a name="third-party-libraries"></a>Сторонние библиотеки 

Служба "Функции Azure" поддерживает использование сторонних библиотек. По умолчанию все зависимости, указанные в файле `pom.xml` проекта, автоматически объединяются во время [`mvn package`](https://github.com/Microsoft/azure-maven-plugins/blob/master/azure-functions-maven-plugin/README.md#azure-functionspackage) этой цели. Зависимости библиотек, не указанных как зависимости в файле `pom.xml`, поместите в каталог `lib` в корневой папке функции. Зависимости, помещенные `lib` в каталог, добавляются в загрузчик системных классов во время выполнения.

`com.microsoft.azure.functions:azure-functions-java-library` Зависимость предоставляется для подкаталогов классов по умолчанию и не требует включения в `lib` каталог. Кроме того, [Azure-functions-Java-Worker](https://github.com/Azure/azure-functions-java-worker) добавляет зависимости, перечисленные [здесь](https://github.com/Azure/azure-functions-java-worker/wiki/Azure-Java-Functions-Worker-Dependencies) , в подкаталог классов.

## <a name="data-type-support"></a>Поддержка типов данных

Для привязки к входным или выходным привязкам можно использовать обычные старые `azure-functions-java-library`объекты Java (POJO), типы, определенные в, или примитивные типы данных, такие как String и Integer.

### <a name="pojos"></a>POJO

Для преобразования входных данных в POJO [Azure-functions-Java-Worker](https://github.com/Azure/azure-functions-java-worker) использует библиотеку [gson](https://github.com/google/gson) . Типам POJO, используемые как входные данные для функций, необходим такой же модификатор доступа `public`.

### <a name="binary-data"></a>Двоичные данные

Привязка входных двоичных данных `byte[]`или выходов к `dataType` с помощью задания поля в файле Function `binary`. JSON для:

```java
   @FunctionName("BlobTrigger")
    @StorageAccount("AzureWebJobsStorage")
     public void blobTrigger(
        @BlobTrigger(name = "content", path = "myblob/{fileName}", dataType = "binary") byte[] content,
        @BindingName("fileName") String fileName,
        final ExecutionContext context
    ) {
        context.getLogger().info("Java Blob trigger function processed a blob.\n Name: " + fileName + "\n Size: " + content.length + " Bytes");
    }
```

Если предполагается наличие значений NULL, `Optional<T>`используйте.

## <a name="bindings"></a>Привязки

Входные и выходные привязки реализуют декларативный способ подключения к данным из кода. У функции может быть несколько входных и выходных привязок.

### <a name="input-binding-example"></a>Пример входной привязки

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class Function {
    @FunctionName("echo")
    public static String echo(
        @HttpTrigger(name = "req", methods = { "put" }, authLevel = AuthorizationLevel.ANONYMOUS, route = "items/{id}") String inputReq,
        @TableInput(name = "item", tableName = "items", partitionKey = "Example", rowKey = "{id}", connection = "AzureWebJobsStorage") TestInputData inputData
        @TableOutput(name = "myOutputTable", tableName = "Person", connection = "AzureWebJobsStorage") OutputBinding<Person> testOutputData,
    ) {
        testOutputData.setValue(new Person(httpbody + "Partition", httpbody + "Row", httpbody + "Name"));
        return "Hello, " + inputReq + " and " + inputData.getKey() + ".";
    }

    public static class TestInputData {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
    public static class Person {
        public String PartitionKey;
        public String RowKey;
        public String Name;

        public Person(String p, String r, String n) {
            this.PartitionKey = p;
            this.RowKey = r;
            this.Name = n;
        }
    }
}
```

Эта функция вызывается с HTTP-запросом. 
- Полезные данные HTTP-запроса передаются в `String` качестве `inputReq`аргумента.
- Одна запись извлекается из хранилища таблиц и передается как `TestInputData` аргумент `inputData`.

Чтобы получить пакет входных данных, можно выполнить привязку `String[]`к `POJO[]`, `List<String>`, или `List<POJO>`.

```java
@FunctionName("ProcessIotMessages")
    public void processIotMessages(
        @EventHubTrigger(name = "message", eventHubName = "%AzureWebJobsEventHubPath%", connection = "AzureWebJobsEventHubSender", cardinality = Cardinality.MANY) List<TestEventData> messages,
        final ExecutionContext context)
    {
        context.getLogger().info("Java Event Hub trigger received messages. Batch size: " + messages.size());
    }
    
    public class TestEventData {
    public String id;
}

```

Эта функция запускается всякий раз, когда в настроенном концентраторе событий появляются новые данные. Так как `cardinality` параметр имеет значение `MANY`, функция получает пакет сообщений из концентратора событий. `EventData`из концентратора событий преобразуется `TestEventData` в для выполнения функции.

### <a name="output-binding-example"></a>Пример выходной привязки

Выходную привязку можно привязать к возвращаемому значению `$return`с помощью. 

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class Function {
    @FunctionName("copy")
    @StorageAccount("AzureWebJobsStorage")
    @BlobOutput(name = "$return", path = "samples-output-java/{name}")
    public static String copy(@BlobTrigger(name = "blob", path = "samples-input-java/{name}") String content) {
        return content;
    }
}
```

При наличии нескольких выходных привязок используйте возвращаемое значение только для одной из них.

Чтобы отправить несколько выходных значений, используйте тип `OutputBinding<T>`, который определен в пакете `azure-functions-java-library`. 

```java
@FunctionName("QueueOutputPOJOList")
    public HttpResponseMessage QueueOutputPOJOList(@HttpTrigger(name = "req", methods = { HttpMethod.GET,
            HttpMethod.POST }, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            @QueueOutput(name = "itemsOut", queueName = "test-output-java-pojo", connection = "AzureWebJobsStorage") OutputBinding<List<TestData>> itemsOut, 
            final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");
       
        String query = request.getQueryParameters().get("queueMessageId");
        String queueMessageId = request.getBody().orElse(query);
        itemsOut.setValue(new ArrayList<TestData>());
        if (queueMessageId != null) {
            TestData testData1 = new TestData();
            testData1.id = "msg1"+queueMessageId;
            TestData testData2 = new TestData();
            testData2.id = "msg2"+queueMessageId;

            itemsOut.getValue().add(testData1);
            itemsOut.getValue().add(testData2);

            return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + queueMessageId).build();
        } else {
            return request.createResponseBuilder(HttpStatus.INTERNAL_SERVER_ERROR)
                    .body("Did not find expected items in CosmosDB input list").build();
        }
    }

     public static class TestData {
        public String id;
    }
```

Эта функция вызывается для HttpRequest. Он записывает несколько значений в хранилище очередей.

## <a name="httprequestmessage-and-httpresponsemessage"></a>HttpRequestMessage и HttpResponseMessage

 Они определены в `azure-functions-java-library`. Они являются вспомогательными типами для работы с HttpTrigger функциями.

| Специализированный тип      |       Назначение        | Типичное использование                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    Триггер HTTP     | Возвращает метод, заголовки или запросы |
| `HttpResponseMessage` | Привязка к выходным данным HTTP | Возвращает состояние, отличное от 200   |

## <a name="metadata"></a>Метаданные

Несколько триггеров отправляют [метаданные триггеров](/azure/azure-functions/functions-triggers-bindings) вместе с входными данными. Для привязки к метаданным триггера можно использовать аннотацию `@BindingName` .


```Java
package com.example;

import java.util.Optional;
import com.microsoft.azure.functions.annotation.*;


public class Function {
    @FunctionName("metadata")
    public static String metadata(
        @HttpTrigger(name = "req", methods = { "get", "post" }, authLevel = AuthorizationLevel.ANONYMOUS) Optional<String> body,
        @BindingName("name") String queryValue
    ) {
        return body.orElse(queryValue);
    }
}
```
В предыдущем примере `queryValue` привязка привязана к параметру `name` строки запроса в URL-адресе запроса HTTP. `http://{example.host}/api/metadata?name=test` Вот еще один пример, показывающий, как `Id` выполнить привязку к метаданным триггера очереди.

```java
 @FunctionName("QueueTriggerMetadata")
    public void QueueTriggerMetadata(
        @QueueTrigger(name = "message", queueName = "test-input-java-metadata", connection = "AzureWebJobsStorage") String message,@BindingName("Id") String metadataId,
        @QueueOutput(name = "output", queueName = "test-output-java-metadata", connection = "AzureWebJobsStorage") OutputBinding<TestData> output,
        final ExecutionContext context
    ) {
        context.getLogger().info("Java Queue trigger function processed a message: " + message + " with metadaId:" + metadataId );
        TestData testData = new TestData();
        testData.id = metadataId;
        output.setValue(testData);
    }
```

> [!NOTE]
> Имя, указанное в заметке, должно соответствовать свойству метаданных.

## <a name="execution-context"></a>Контекст выполнения

`ExecutionContext`, определенный в `azure-functions-java-library`, содержит вспомогательные методы для взаимодействия со средой выполнения функций.

### <a name="logger"></a>Средство ведения журнала

Используйте `getLogger`, определенную `ExecutionContext`в, для записи журналов из кода функции.

Пример:

```java

import com.microsoft.azure.functions.*;
import com.microsoft.azure.functions.annotation.*;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        if (req.isEmpty()) {
            context.getLogger().warning("Empty request body received by function " + context.getFunctionName() + " with invocation " + context.getInvocationId());
        }
        return String.format(req);
    }
}
```

## <a name="view-logs-and-trace"></a>Просмотр журналов и трассировки

Вы можете использовать Azure CLI для потокового ведения журнала Java stdout и stderr, а также для других событий ведения журнала приложений. 

Вот как можно настроить приложение функции для записи журнала приложений с помощью Azure CLI:

```azurecli-interactive
az webapp log config --name functionname --resource-group myResourceGroup --application-logging true
```

Чтобы передать выходные данные журнала для приложения-функции с помощью Azure CLI, откройте новую командную строку, bash или сеанс терминала и введите следующую команду:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```
Команда [AZ webapp log хвоста](/cli/azure/webapp/log) имеет параметры для фильтрации выходных данных с помощью `--provider` параметра. 

Чтобы загрузить файлы журнала в виде одного ZIP-файла с помощью Azure CLI, откройте новую командную строку, bash или сеанс терминала и введите следующую команду:

```azurecli-interactive
az webapp log download --resource-group resourcegroupname --name functionappname
```

Перед выполнением этой команды необходимо включить ведение журнала файловой системы в портал Azure или Azure CLI.

## <a name="environment-variables"></a>Переменные среды

В Функциях [параметры приложения](functions-app-settings.md), такие как строки подключения службы, доступны в виде переменных среды во время выполнения. Доступ к этим параметрам можно получить с `System.getenv("AzureWebJobsStorage")`помощью,.

В следующем примере возвращается [параметр приложения](functions-how-to-use-azure-function-app-settings.md#settings)с ключом с именем `myAppSetting`:

```java

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        context.getLogger().info("My app setting value: "+ System.getenv("myAppSetting"));
        return String.format(req);
    }
}

```

> [!NOTE]
> Значение AppSetting FUNCTIONS_EXTENSION_VERSION должно быть ~ 2 или ~ 3 для оптимизированного процесса холодного запуска.

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные сведения о разработке приложений Java с помощью функций Azure см. в следующих ресурсах:

* [Рекомендации по функциям Azure](functions-best-practices.md)
* [Справочник разработчика по функциям Azure](functions-reference.md)
* [Azure Functions triggers and bindings (Триггеры и привязки в Функциях Azure)](functions-triggers-bindings.md)
* Локальная разработка и отладка с помощью [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md)и [Eclipse](functions-create-maven-eclipse.md)
* [Remote Debug Java Azure Functions with Visual Studio Code (Удаленная отладка функций Azure на языке Java с помощью Visual Studio Code)](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
* [Подключаемый модуль Maven для функций Azure](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-functions-maven-plugin/README.md) 
* Оптимизируйте создание функций с помощью `azure-functions:add` цели и подготовьте промежуточный каталог к [развертыванию ZIP-файлов](deployment-zip-push.md).
