---
title: Начало работы с очередями служебной шины Azure | Документация Майкрософт
description: В этом руководстве вы создадите консольные приложения .NET Core для отправки сообщений в очередь служебной шины и получения сообщений из нее.
ms.topic: conceptual
ms.tgt_pltfrm: dotnet
ms.date: 09/01/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 8c862fda5ee2bb5b7418fda1b4e45cd06742cf95
ms.sourcegitcommit: 6a770fc07237f02bea8cc463f3d8cc5c246d7c65
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95819319"
---
# <a name="get-started-with-service-bus-queues"></a>Начало работы с очередями служебной шины
В этом руководстве вы создадите консольные приложения .NET Core для отправки сообщений в очередь служебной шины и получения сообщений из нее.

> [!WARNING]
> В этом кратком руководстве используется старый пакет Microsoft. Azure. ServiceBus. Краткое руководство, в котором используется последний пакет Azure. Messaging. ServiceBus, см. в разделе [Отправка и получение событий с помощью пакета Azure. Messaging. servicebus](service-bus-dotnet-get-started-with-queues.md). 

## <a name="prerequisites"></a>Предварительные требования

- [Visual Studio 2019](https://www.visualstudio.com/vs).
- [Пакет SDK для .NET Core](https://www.microsoft.com/net/download/windows) версии 2.0 или более новой.
- Подписка Azure. Для работы с этим учебником требуется учетная запись Azure. Вы можете активировать [преимущества подписчика MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) или зарегистрироваться для получения [бесплатной учетной записи](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
- Если у вас нет подходящей очереди служебной шины, [создайте ее с помощью портала Azure](service-bus-quickstart-portal.md).

  - Ознакомьтесь с общими сведениями об очередях Служебной шины.
  - Создайте пространство имен служебной шины.
  - Получите строку подключения.
  - Создается очередь служебной шины.

## <a name="send-messages"></a>Отправка сообщений

Для отправки сообщений в очередь создайте консольное приложение C# с помощью Visual Studio.

### <a name="create-a-console-application"></a>Создание консольного приложение

Запустите Visual Studio и создайте проект **консольного приложения (.NET Core)** для C#. В этом примере имя приложения *коресендерапп*.

### <a name="add-the-service-bus-nuget-package"></a>Получение пакета NuGet для служебной шины

1. Щелкните созданный проект правой кнопкой мыши и выберите **Управление пакетами NuGet**.
1. Нажмите кнопку **Обзор**. Найдите и выберите **[Microsoft. Azure. servicebus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus/)**.
1. Нажмите кнопку **установить** , чтобы завершить установку, а затем закройте диспетчер пакетов NuGet.

    ![Установка пакета NuGet][nuget-pkg]

### <a name="write-code-to-send-messages-to-the-queue"></a>Написание кода для отправки сообщений в очередь

1. В *Program.CS* добавьте следующие инструкции в `using` начале определения пространства имен перед объявлением класса:

    ```csharp
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

1. В `Program` классе объявите следующие переменные:

    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string QueueName = "<your_queue_name>";
    static IQueueClient queueClient;
    ```

    Введите строку подключения для пространства имен в качестве `ServiceBusConnectionString` переменной. Введите имя очереди.

1. Замените `Main()` метод следующим **асинхронным** `Main` методом. Он вызывает `SendMessagesAsync()` метод, который будет добавлен на следующем шаге для отправки сообщений в очередь. 

    ```csharp
    public static async Task Main(string[] args)
    {    
        const int numberOfMessages = 10;
        queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after sending all the messages.");
        Console.WriteLine("======================================================");

        // Send messages.
        await SendMessagesAsync(numberOfMessages);

        Console.ReadKey();

        await queueClient.CloseAsync();
    }
    ```
1. Сразу после `MainAsync()` метода добавьте следующий `SendMessagesAsync()` метод, который выполняет отправку числа сообщений, указанных в `numberOfMessagesToSend` (в данный момент установлено равным 10):

    ```csharp
    static async Task SendMessagesAsync(int numberOfMessagesToSend)
    {
        try
        {
            for (var i = 0; i < numberOfMessagesToSend; i++)
            {
                // Create a new message to send to the queue.
                string messageBody = $"Message {i}";
                var message = new Message(Encoding.UTF8.GetBytes(messageBody));

                // Write the body of the message to the console.
                Console.WriteLine($"Sending message: {messageBody}");

                // Send the message to the queue.
                await queueClient.SendAsync(message);
            }
        }
        catch (Exception exception)
        {
            Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
        }
    }
    ```

Вот как должен выглядеть файл *Program.CS* .

```csharp
namespace CoreSenderApp
{
    using System;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;

    class Program
    {
        // Connection String for the namespace can be obtained from the Azure portal under the 
        // 'Shared Access policies' section.
        const string ServiceBusConnectionString = "<your_connection_string>";
        const string QueueName = "<your_queue_name>";
        static IQueueClient queueClient;

        public static async Task Main(string[] args)
        {    
            const int numberOfMessages = 10;
            queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    
            Console.WriteLine("======================================================");
            Console.WriteLine("Press ENTER key to exit after sending all the messages.");
            Console.WriteLine("======================================================");
    
            // Send messages.
            await SendMessagesAsync(numberOfMessages);
    
            Console.ReadKey();
    
            await queueClient.CloseAsync();
        }

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
    }
}
```

Запустите программу и проверьте портал Azure.

Выберите имя очереди в окне **Обзор** пространства имен, чтобы отобразить список **основных** компонентов очереди.

![Получено сообщений с количеством и размером][queue-message]

Значение **счетчика активных сообщений** для очереди теперь равно **10**. Каждый раз, когда вы запускаете это приложение отправителя без получения сообщений, это значение увеличивается на 10.

Текущий размер очереди увеличивает **Текущее** значение в **Essentials**  каждый раз, когда приложение добавляет сообщения в очередь.

В следующем разделе описано, как получить эти сообщения.

## <a name="receive-messages"></a>Получение сообщений

Чтобы получить отправленные сообщения, создайте другое приложение **консольного приложения (.NET Core)** . Установите пакет NuGet **Microsoft. Azure. servicebus** , как и для приложения отправителя.

### <a name="write-code-to-receive-messages-from-the-queue"></a>Написание кода для получения сообщений из очереди

1. В *Program.CS* добавьте следующие инструкции в `using` начале определения пространства имен перед объявлением класса:

    ```csharp
    using System;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

1. В `Program` классе объявите следующие переменные:

    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string QueueName = "<your_queue_name>";
    static IQueueClient queueClient;
    ```

    Введите строку подключения для пространства имен в качестве `ServiceBusConnectionString` переменной. Введите имя очереди.

1. Замените метод `Main()` следующим кодом:

    ```csharp
    static void Main(string[] args)
    {
        MainAsync().GetAwaiter().GetResult();
    }

    static async Task MainAsync()
    {
        queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
        Console.WriteLine("======================================================");

        // Register QueueClient's MessageHandler and receive messages in a loop
        RegisterOnMessageHandlerAndReceiveMessages();

        Console.ReadKey();

        await queueClient.CloseAsync();
    }
    ```

1. Сразу после `MainAsync()` метода добавьте следующий метод, который регистрирует обработчик сообщений и получает сообщения, отправленные приложением отправителя:

    ```csharp
    static void RegisterOnMessageHandlerAndReceiveMessages()
    {
        // Configure the message handler options in terms of exception handling, number of concurrent messages to deliver, etc.
        var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
        {
            // Maximum number of concurrent calls to the callback ProcessMessagesAsync(), set to 1 for simplicity.
            // Set it according to how many messages the application wants to process in parallel.
            MaxConcurrentCalls = 1,

            // Indicates whether the message pump should automatically complete the messages after returning from user callback.
            // False below indicates the complete operation is handled by the user callback as in ProcessMessagesAsync().
            AutoComplete = false
        };

        // Register the function that processes messages.
        queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    }
    ```

1. Сразу после предыдущего метода добавьте следующий метод `ProcessMessagesAsync()` для обработки полученных сообщений:

    ```csharp
    static async Task ProcessMessagesAsync(Message message, CancellationToken token)
    {
        // Process the message.
        Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

        // Complete the message so that it is not received again.
        // This can be done only if the queue Client is created in ReceiveMode.PeekLock mode (which is the default).
        await queueClient.CompleteAsync(message.SystemProperties.LockToken);

        // Note: Use the cancellationToken passed as necessary to determine if the queueClient has already been closed.
        // If queueClient has already been closed, you can choose to not call CompleteAsync() or AbandonAsync() etc.
        // to avoid unnecessary exceptions.
    }
    ```

1. Наконец, добавьте следующий метод для обработки исключений, которые могут возникнуть:

    ```csharp
    // Use this handler to examine the exceptions received on the message pump.
    static Task ExceptionReceivedHandler(ExceptionReceivedEventArgs exceptionReceivedEventArgs)
    {
        Console.WriteLine($"Message handler encountered an exception {exceptionReceivedEventArgs.Exception}.");
        var context = exceptionReceivedEventArgs.ExceptionReceivedContext;
        Console.WriteLine("Exception context for troubleshooting:");
        Console.WriteLine($"- Endpoint: {context.Endpoint}");
        Console.WriteLine($"- Entity Path: {context.EntityPath}");
        Console.WriteLine($"- Executing Action: {context.Action}");
        return Task.CompletedTask;
    }
    ```

Вот как должен выглядеть файл *Program.CS* :

```csharp
namespace CoreReceiverApp
{
    using System;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;

    class Program
    {
        // Connection String for the namespace can be obtained from the Azure portal under the 
        // 'Shared Access policies' section.
        const string ServiceBusConnectionString = "<your_connection_string>";
        const string QueueName = "<your_queue_name>";
        static IQueueClient queueClient;

        static void Main(string[] args)
        {
            MainAsync().GetAwaiter().GetResult();
        }

        static async Task MainAsync()
        {
            queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

            Console.WriteLine("======================================================");
            Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
            Console.WriteLine("======================================================");

            // Register QueueClient's MessageHandler and receive messages in a loop
            RegisterOnMessageHandlerAndReceiveMessages();
 
            Console.ReadKey();

            await queueClient.CloseAsync();
        }

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

        static async Task ProcessMessagesAsync(Message message, CancellationToken token)
        {
            // Process the message
            Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

            // Complete the message so that it is not received again.
            // This can be done only if the queueClient is created in ReceiveMode.PeekLock mode (which is default).
            await queueClient.CompleteAsync(message.SystemProperties.LockToken);

            // Note: Use the cancellationToken passed as necessary to determine if the queueClient has already been closed.
            // If queueClient has already been Closed, you may chose to not call CompleteAsync() or AbandonAsync() etc. calls 
            // to avoid unnecessary exceptions.
        }

        static Task ExceptionReceivedHandler(ExceptionReceivedEventArgs exceptionReceivedEventArgs)
        {
            Console.WriteLine($"Message handler encountered an exception {exceptionReceivedEventArgs.Exception}.");
            var context = exceptionReceivedEventArgs.ExceptionReceivedContext;
            Console.WriteLine("Exception context for troubleshooting:");
            Console.WriteLine($"- Endpoint: {context.Endpoint}");
            Console.WriteLine($"- Entity Path: {context.EntityPath}");
            Console.WriteLine($"- Executing Action: {context.Action}");
            return Task.CompletedTask;
        }
    }
}
```

Запустите программу и перейдите на портал еще раз. **Число активных сообщений** и **текущие** значения теперь равны **0**.

![Очередь после получения сообщений][queue-message-receive]

Поздравляем! Теперь вы создали очередь, отправили в нее набор сообщений и получили эти сообщения из одной очереди.

> [!NOTE]
> Вы можете управлять ресурсами служебной шины с помощью [обозревателя служебной шины](https://github.com/paolosalvatori/ServiceBusExplorer/). Обозреватель служебной шины позволяет пользователям легко подключаться к пространству имен служебной шины и администрировать сущности обмена сообщениями. Это средство предоставляет расширенные возможности, такие как функции импорта и экспорта, а также возможность тестирования разделов, очередей, подписок, служб ретранслятора, центров уведомлений и концентраторов событий.

## <a name="next-steps"></a>Дальнейшие действия

Ознакомьтесь с [примерами в репозитории GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples), демонстрирующими расширенные возможности обмена сообщениями служебной шины.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues-legacy/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues-legacy/messages-sent-to-essentials.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues-legacy/queue-message-receive-in-essentials.png

