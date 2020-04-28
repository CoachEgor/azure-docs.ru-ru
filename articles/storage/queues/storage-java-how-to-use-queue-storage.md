---
title: Использование хранилища очередей из Java в службе хранилища Azure
description: Узнайте, как использовать хранилище очередей для создания и удаления очередей, а также для вставки, получения и удаления сообщений с помощью клиентской библиотеки службы хранилища Azure для Java.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 12/08/2016
ms.service: storage
ms.subservice: queues
ms.topic: conceptual
ms.reviewer: cbrooks
ms.openlocfilehash: 7658b8541e7a79a5e547a6649b35681446e34b0b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80067131"
---
# <a name="how-to-use-queue-storage-from-java"></a>Использование хранилища очередей из Java

[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

В этом руководстве показано, как реализовать типичные сценарии с использованием службы хранения очередей Azure. Примеры написаны на Java и используют [пакет SDK службы хранилища Azure для Java][Azure Storage SDK for Java]. В число рассматриваемых сценариев входят **Вставка**, **Просмотр**, **Получение**и **Удаление** сообщений очереди, а также **Создание** и **Удаление** очередей. Дополнительные сведения об очередях см. в разделе [Дальнейшие действия](#next-steps).

> [!NOTE]
> Пакет SDK доступен разработчикам, использующим хранилище Azure на устройствах Android. Дополнительные сведения см. в разделе [Microsoft Azure Storage SDK for Android][Azure Storage SDK for Android] (Пакет SDK хранилища Azure для Android).

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Создание приложения Java

В этом разделе вы будете использовать функции хранилища, которые можно запускать в приложении Java локально или в коде, работающем в веб-приложении в Azure.

Для этого необходимо установить пакет SDK для Java (JDK) и создать учетную запись хранения Azure в подписке Azure. После этого необходимо убедиться, что ваша система разработки соответствует минимальным требованиям и зависимостям, перечисленным в репозитории [пакета SDK службы хранилища Azure для Java][Azure Storage SDK for Java] на сайте GitHub. Если ваша система отвечает указанным требованиям можно приступить к выполнению инструкций по загрузке библиотек хранилища Azure для Java из репозитория и их установке на своей системе. После выполнения этих задач вы сможете создать приложение Java, которое будет использовать примеры, приведенные в этой статье.

## <a name="configure-your-application-to-access-queue-storage"></a>Настройка приложения для доступа к хранилищу очередей

Если нужно использовать API-интерфейсы Azure для доступа к очередям, добавьте следующие инструкции импорта в верхнюю часть файла Java:

```java
// Include the following imports to use queue APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.queue.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a>Настройка строки подключения к хранилищу Azure

Клиент хранилища Azure использует строку подключения с целью хранения конечных точек и учетных данных для доступа к службам управления данными. При работе в клиентском приложении необходимо указать для хранилища строку подключения в следующем формате, используя имя своей учетной записи хранения и первичный ключ доступа для учетной записи хранения, указанные на [портале Azure](https://portal.azure.com) значениями *AccountName* и *AccountKey*. В этом примере показано, как объявить статическое поле для размещения строки подключения:

```java
// Define the connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

Если приложение выполняется в роли на платформе Microsoft Azure, эта строка может храниться в файле конфигурации службы *ServiceConfiguration.cscfg*, для доступа к которой можно использовать вызов метода **RoleEnvironment.getConfigurationSettings** . Ниже приведен пример получения строки подключения из элемента **Setting** с именем *StorageConnectionString* в файле конфигурации службы:

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

В приведенных ниже примерах предполагается, что вы использовали одно из этих двух определений для получения строки подключения к хранилищу.

## <a name="how-to-create-a-queue"></a>Практическое руководство. Создание очереди
Объект **CloudQueueClient** позволяет получать ссылочные объекты для очередей. Следующий код создает объект **CloudQueueClient**. (Примечание. Есть и другие способы создания объектов **CloudStorageAccount**. Дополнительные сведения см. в разделе **CloudStorageAccount** в [справочнике по пакету SDK для клиента службы хранилища Azure].)

Используйте объект **CloudQueueClient** для получения ссылки на очередь, которую необходимо использовать. Очередь можно создать, если она не существует.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

   // Create the queue client.
   CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

   // Retrieve a reference to a queue.
   CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Create the queue if it doesn't already exist.
   queue.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-add-a-message-to-a-queue"></a>Практическое руководство. Добавление сообщения в очередь
Чтобы вставить сообщение в существующую очередь, сначала создайте новый объект **CloudQueueMessage**. Затем вызовите метод **AddMessage** . **CloudQueueMessage** можно создать либо из строки (в формате UTF-8), либо в массиве байтов. Ниже приведен код, создающий очередь (если она не существует) и вставляет сообщение "Hello, World".

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.createIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.addMessage(message);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-peek-at-the-next-message"></a>Практическое руководство. Просмотр следующего сообщения
Вы можете просмотреть сообщение в начале очереди, не удаляя его из очереди, вызвав метод **peekMessage**.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Peek at the next message.
    CloudQueueMessage peekedMessage = queue.peekMessage();

    // Output the message value.
    if (peekedMessage != null)
    {
      System.out.println(peekedMessage.getMessageContentAsString());
   }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Практическое руководство. Изменение содержимого сообщения в очереди
Вы можете изменить содержимое сообщения непосредственно в очереди. Если сообщение представляет собой рабочую задачу, можно использовать эту функцию для обновления состояния рабочей задачи. Следующий код добавляет новое содержимое в очередь сообщений и продлевает время ожидания видимости еще на 60 секунд. Увеличение времени ожидания видимости сохраняет состояние работы, связанное с сообщением, и предоставляет клиенту еще одну минуту, чтобы продолжить работу над сообщением. Этот метод можно использовать для отслеживания многошаговых рабочих процессов по сообщениям в очереди без необходимости начинать с самого начала в случае сбоя шага обработки в связи с ошибкой аппаратного или программного обеспечения. Обычно также сохраняется счетчик повторов. Если количество повторов сообщения превысит *n* раз, его нужно удалить. Это обеспечивает защиту от сообщений, которые инициируют ошибку приложения при каждой попытке обработки.

Приведенный ниже пример кода выполняет поиск в очереди сообщения, находит первое сообщение, которое содержит "Привет, мир", затем изменяет контент сообщения и выполняет выход.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // The maximum number of messages that can be retrieved is 32.
    final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

    // Loop through the messages in the queue.
    for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
    {
        // Check for a specific string.
        if (message.getMessageContentAsString().equals("Hello, World"))
        {
            // Modify the content of the first matching message.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 30 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 30, updateFields, null, null);
            break;
        }
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

В отличие от этого, следующий пример кода просто обновляет первое видимое сообщение в очереди.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve the first visible message in the queue.
    CloudQueueMessage message = queue.retrieveMessage();

    if (message != null)
    {
        // Modify the message content.
        message.setMessageContent("Updated contents.");
        // Set it to be visible in 60 seconds.
        EnumSet<MessageUpdateFields> updateFields =
            EnumSet.of(MessageUpdateFields.CONTENT,
            MessageUpdateFields.VISIBILITY);
        // Update the message.
        queue.updateMessage(message, 60, updateFields, null, null);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-get-the-queue-length"></a>Практическое руководство. Получение длины очереди
Вы можете узнать приблизительное количество сообщений в очереди. Метод **downloadAttributes** запрашивает у службы очередей несколько текущих значений, включая количество сообщений в очереди. Счетчик указывает число лишь приблизительно, так как сообщения могут добавляться или удаляться после ответа службы очередей на ваш запрос. Метод **getApproximateMessageCount** возвращает последнее значение, полученное при вызове **downloadAttributes** без обращения к службе очередей.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Download the approximate message count from the server.
    queue.downloadAttributes();

    // Retrieve the newly cached approximate message count.
    long cachedMessageCount = queue.getApproximateMessageCount();

    // Display the queue length.
    System.out.println(String.format("Queue length: %d", cachedMessageCount));
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-dequeue-the-next-message"></a>Практическое руководство. Удаление следующего сообщения из очереди
Код удаляет сообщение из очереди в два этапа. При вызове метода **retrieveMessage**вы получаете следующее сообщение в очереди. Сообщение, возвращаемое методом **retrieveMessage** , становится невидимым для другого кода, считывающего сообщения из этой очереди. По умолчанию это сообщение остается невидимым в течение 30 секунд. Чтобы завершить удаление сообщения из очереди, необходимо также вызвать **deleteMessage**. Этот двухэтапный процесс удаления сообщения позволяет удостовериться, что если коду не удастся обработать сообщение из-за сбоя оборудования или программного обеспечения, другой экземпляр кода сможет получить то же сообщение и повторить попытку. Код вызывает **deleteMessage** сразу после обработки сообщения.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve the first visible message in the queue.
    CloudQueueMessage retrievedMessage = queue.retrieveMessage();

    if (retrievedMessage != null)
    {
        // Process the message in less than 30 seconds, and then delete the message.
        queue.deleteMessage(retrievedMessage);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="additional-options-for-dequeuing-messages"></a>Дополнительные варианты удаления сообщений из очереди
Способ извлечения сообщения из очереди можно настроить двумя способами. Во-первых, можно получить пакет сообщений (до 32 сообщений). Во-вторых, можно задать более длительное или короткое время ожидания видимости, чтобы предоставить коду больше или меньше времени на полную обработку каждого сообщения.

В следующем примере кода метод **retrieveMessages** используется для получения 20 сообщений в одном вызове. Затем он обрабатывает каждое сообщение с помощью цикла **for** . Пример также задает время ожидания невидимости в размере пяти минут (300 секунд) для каждого сообщения. Пять минут начинается для всех сообщений одновременно, поэтому при прошествии пяти минут с момента вызова **retreivemessages**все сообщения, которые не были удалены, снова станут видимыми.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
        // Do processing for all messages in less than 5 minutes,
        // deleting each message after processing.
        queue.deleteMessage(message);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-list-the-queues"></a>Как перечислять очереди
Чтобы получить список текущей очереди, вызовите метод **CloudQueueClient.listQueues()**, который возвращает коллекцию объектов **CloudQueue**.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient =
        storageAccount.createCloudQueueClient();

    // Loop through the collection of queues.
    for (CloudQueue queue : queueClient.listQueues())
    {
        // Output each queue name.
        System.out.println(queue.getName());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-a-queue"></a>Практическое руководство. Удаление очереди
Для удаления очереди и всех сообщений в ней вызовите метод **deleteIfExists** в объекте **CloudQueue**.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Delete the queue if it exists.
    queue.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a>Дальнейшие действия
Вы изучили основные сведения о хранилище очередей. Дополнительные сведения о более сложных задачах по использованию хранилища можно найти по следующим ссылкам.

* [Пакет SDK службы хранилища Azure для Java][Azure Storage SDK for Java]
* [справочнике по пакету SDK для клиента службы хранилища Azure][Azure Storage Client SDK Reference]
* [REST API служб хранилища Azure][Azure Storage Services REST API]
* [Блог рабочей группы службы хранилища Azure][Azure Storage Team Blog]

[Azure SDK for Java]: https://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[справочнике по пакету SDK для клиента службы хранилища Azure]: https://javadoc.io/doc/com.microsoft.azure/azure-core/0.8.0/index.html
[Azure Storage Services REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: https://blogs.msdn.com/b/windowsazurestorage/
