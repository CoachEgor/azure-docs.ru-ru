---
title: Создание очереди Служебной шины с помощью портала Azure
description: Из этого краткого руководства вы узнаете, как создать очередь Служебной шины Azure с помощью портала Azure. Затем вы выполните отправку и получение сообщений из очереди, используя образец клиентского приложения.
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: 0c88db652efe54a497af094160fb426be7c43d16
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2020
ms.locfileid: "79215612"
---
# <a name="quickstart-use-azure-portal-to-create-a-service-bus-queue"></a>Краткое руководство. Создание очереди служебной шины с помощью портала Azure
В этом кратком руководстве объясняется, как отправлять и получать сообщения через очередь служебной шины, создав с помощью [портала Azure][Azure portal] пространство имен для обмена сообщениями и очередь в этом пространстве имен, а также как получить учетные данные для авторизации в этом пространстве имен. Далее в этой процедуре демонстрируется отправка и получение сообщений через созданную очередь с применением [библиотеки .NET Standard](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus).

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством вам потребуются:

- Подписка Azure. Если у вас еще нет подписки Azure, вы можете создать [бесплатная учетная запись][], прежде чем начинать работу.
- [Visual Studio 2017 с обновлением 3 (версия 15.3, 26730.01)](https://www.visualstudio.com/vs) или более новая версия. Используйте Visual Studio для сборки примера, который отправляет сообщения в очередь и получает сообщения из очереди. Этот пример необходим для тестирования очереди, созданной вами с помощью PowerShell. 
- [Пакет SDK для .NET Core](https://www.microsoft.com/net/download/windows) версии 2.0 или более новой.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="send-and-receive-messages"></a>Отправка и получение сообщений

> [!NOTE]
> В этом разделе для отправки и получения сообщений используется пример .NET. Примеры отправки и получения сообщений с использованием других языков программирования см. в [этой статье](service-bus-samples.md). 
> 
> Пошаговые инструкции по отправке и приему сообщений с использованием различных языков программирования см. в следующих кратких руководствах:
> - [.NET](service-bus-dotnet-get-started-with-queues.md)
> - [Java](service-bus-java-how-to-use-queues.md)
> - [Пример Node. js с использованием пакета Служебной шины Azure](service-bus-nodejs-how-to-use-queues-new-package.md)
> - [Пример Node. js с использованием пакета azure-sb](service-bus-nodejs-how-to-use-queues.md)
> - [PHP](service-bus-php-how-to-use-queues.md)
> - [Python](service-bus-python-how-to-use-queues.md)
> - [Ruby](service-bus-ruby-how-to-use-queues.md)

Подготовив пространство имен и очередь и получив необходимые учетные данные, вы можете приступать к отправке и получению сообщений. Этот код можно изучить в [папке с примером на GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/TopicFilters).

Чтобы выполнить этот код, сделайте следующее:

1. Выполните следующую команду, которая клонирует [репозиторий GitHub для служебной шины](https://github.com/Azure/azure-service-bus/):

   ```
   git clone https://github.com/Azure/azure-service-bus.git
   ```
3. Перейдите к папке `azure-service-bus\samples\DotNet\GettingStarted\BasicSendReceiveQuickStart\BasicSendReceiveQuickStart` с примерами.
4. Скопируйте строку подключения и имя очереди, которые вы получили в разделе "Получение учетных данных управления".
5.  В командной строке введите следующую команду:

    ```
    dotnet build
    ```
6.  Перейдите в папку `bin\Debug\netcoreapp2.0`.
7.  Введите приведенную ниже команду, чтобы запустить программу. Не забудьте ввести вместо `myConnectionString` значение, которое вы получили ранее, а вместо `myQueueName` — имя созданной очереди:

    ```shell
    dotnet BasicSendReceiveQuickStart.dll -ConnectionString "myConnectionString" -QueueName "myQueueName"
    ``` 
8. Проследите, как в очередь отправляются 10 сообщений, а затем поступают из нее:

   ![Выходные данные программы](./media/service-bus-quickstart-portal/dotnet.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

С помощью портала можно удалить группу ресурсов, пространство имен и очередь.

## <a name="understand-the-sample-code"></a>Разбор примера кода

Этот раздел содержит дополнительные сведения о работе этого примера кода. 

### <a name="get-connection-string-and-queue"></a>Получение строки подключения и очереди

Строка подключения и имя очереди передаются в метод `Main()` в качестве аргументов командной строки. `Main()` объявляет две строковые переменные для хранения этих значений:

```csharp
static void Main(string[] args)
{
    string ServiceBusConnectionString = "";
    string QueueName = "";

    for (int i = 0; i < args.Length; i++)
    {
        var p = new Program();
        if (args[i] == "-ConnectionString")
        {
            Console.WriteLine($"ConnectionString: {args[i+1]}");
            ServiceBusConnectionString = args[i + 1]; 
        }
        else if(args[i] == "-QueueName")
        {
            Console.WriteLine($"QueueName: {args[i+1]}");
            QueueName = args[i + 1];
        }                
    }

    if (ServiceBusConnectionString != "" && QueueName != "")
        MainAsync(ServiceBusConnectionString, QueueName).GetAwaiter().GetResult();
    else
    {
        Console.WriteLine("Specify -Connectionstring and -QueueName to execute the example.");
        Console.ReadKey();
    }                            
}
```
 
Затем метод `Main()` запускает асинхронный цикл обработки сообщений `MainAsync()`.

### <a name="message-loop"></a>Цикл обработки сообщений

Метод MainAsync() создает клиент очереди, используя аргументы командной строки, затем вызывает обработчик получения сообщений с именем `RegisterOnMessageHandlerAndReceiveMessages()` и отправляет набор сообщений.

```csharp
static async Task MainAsync(string ServiceBusConnectionString, string QueueName)
{
    const int numberOfMessages = 10;
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

    Console.WriteLine("======================================================");
    Console.WriteLine("Press any key to exit after receiving all the messages.");
    Console.WriteLine("======================================================");

    // Register QueueClient's MessageHandler and receive messages in a loop
    RegisterOnMessageHandlerAndReceiveMessages();

    // Send Messages
    await SendMessagesAsync(numberOfMessages);

    Console.ReadKey();

    await queueClient.CloseAsync();
}
```

Метод `RegisterOnMessageHandlerAndReceiveMessages()` просто настраивает несколько параметров для обработчика событий и вызывает метод `RegisterMessageHandler()` клиента, который начинает прием сообщений:

```csharp
static void RegisterOnMessageHandlerAndReceiveMessages()
{
    // Configure the MessageHandler Options in terms of exception handling, number of concurrent messages to deliver etc.
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        // Maximum number of Concurrent calls to the callback `ProcessMessagesAsync`, set to 1 for simplicity.
        // Set it according to how many messages the application wants to process in parallel.
        MaxConcurrentCalls = 1,

        // Indicates whether MessagePump should automatically complete the messages after returning from User Callback.
        // False below indicates the Complete will be handled by the User Callback as in `ProcessMessagesAsync` below.
        AutoComplete = false
    };

    // Register the function that will process messages
    queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
} 
```

### <a name="send-messages"></a>Отправка сообщений

Операции создания и отправки сообщений выполняются в методе `SendMessagesAsync()`:

```csharp
static async Task SendMessagesAsync(int numberOfMessagesToSend)
{
    try
    {
        for (var i = 0; i < numberOfMessagesToSend; i++)
        {
            // Create a new message to send to the queue
            string messageBody = $"Message {i}";
            var message = new Message(Encoding.UTF8.GetBytes(messageBody));

            // Write the body of the message to the console
            Console.WriteLine($"Sending message: {messageBody}");

            // Send the message to the queue
            await queueClient.SendAsync(message);
        }
    }
    catch (Exception exception)
    {
        Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
    }
}
```

### <a name="process-messages"></a>Обработка сообщений

Метод `ProcessMessagesAsync()` обнаруживает новые сообщения, обрабатывает их и завершает их прием:

```csharp
static async Task ProcessMessagesAsync(Message message, CancellationToken token)
{
    // Process the message
    Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

    // Complete the message so that it is not received again.
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
}
```
> [!NOTE]
> Вы можете управлять ресурсами служебной шины с помощью [обозревателя служебной шины](https://github.com/paolosalvatori/ServiceBusExplorer/). Обозреватель служебной шины позволяет без труда подключаться к пространству имен служебной шины и управлять сущностями обмена сообщениями. Средство предоставляет дополнительные возможности, например функции импорта и экспорта или возможность проверять разделы, очереди, подписки, службы ретрансляции, центры уведомлений и концентраторы событий. 

## <a name="next-steps"></a>Дальнейшие действия

С помощью этой статьи вы создали пространство имен служебной шины и другие ресурсы, необходимые для отправки и получения сообщений через очередь. См. подробнее о **создании кода для отправки и получения сообщений**. 

> [!div class="nextstepaction"]
> [Отправка и получение сообщений](service-bus-dotnet-get-started-with-queues.md)


[бесплатная учетная запись]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
[Azure portal]: https://portal.azure.com/

[service-bus-flow]: ./media/service-bus-quickstart-portal/service-bus-flow.png
