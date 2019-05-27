---
title: Планирование заданий c помощью Центра Интернета вещей Azure (.NET и .NET) | Документация Майкрософт
description: Планирование заданий с помощью Центра Интернета вещей Azure для вызова прямого метода на нескольких устройствах. Используйте пакеты SDK для устройств Azure IoT для .NET, чтобы реализовать приложения имитации устройства и приложение службы, на которых будет выполнено задание.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/06/2018
ms.author: robinsh
ms.openlocfilehash: f21f1eed6babee52f30c6eccc79f88dc7bee5d58
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/17/2019
ms.locfileid: "65864482"
---
# <a name="schedule-and-broadcast-jobs-netnet"></a>Планирование и трансляция заданий (.NET и .NET)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Центр Интернета вещей Azure позволяет планировать и отслеживать задания по обновлению для миллионов устройств. Что можно сделать с помощью заданий?

* Обновление требуемых свойств
* Обновление тегов
* Вызов прямых методов

Задание выступает в роли оболочки для одного из этих действий и отслеживает его выполнение для определенного набора устройств в соответствии с запросом на двойнике устройства. Например, внутреннее приложение может использовать задание для вызова прямого метода перезапуска на 10 000 устройств. Вы можете определить набор устройств с помощью запроса на двойнике устройства и запланировать момент времени в будущем для выполнения задания. Задание отслеживает ход выполнения по мере того, как каждое из устройств получает вызов и выполняет прямой метод перезагрузки.

Дополнительные сведения об этих возможностях см. в указанных ниже статьях.

* Двойник устройства и свойства: [Начало работы с двойниками устройств](iot-hub-csharp-csharp-twin-getstarted.md) и [руководства: Практическое использование свойств двойников устройств](tutorial-device-twins.md)

* Прямые методы: [Руководство разработчика для центра Интернета вещей — прямые методы](iot-hub-devguide-direct-methods.md) и [руководства: Использование прямых методов](quickstart-control-device-dotnet.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

В этом учебнике описаны следующие процедуры.

* Создание приложения для устройств, которое реализует прямой метод **LockDoor**, вызываемый внутренним приложением.

* Создание внутреннего приложения, которое создает задание для вызова прямого метода **LockDoor** на нескольких устройствах. Еще одно задание отправляет обновления требуемых свойств на несколько устройств.

По завершении работы с этим руководством у вас будет два консольных приложения .NET (C#):

**SimulateDeviceMethods**, которое подключается к Центру Интернета вещей и реализует прямой метод **LockDoor**.

**ScheduleJob** использует задания для вызова прямого метода **LockDoor** и для обновления на нескольких устройствах свойств, установленных на двойнике устройства.

Для работы с этим учебником требуется:

* приведенному.
* Активная учетная запись Azure. Если ее нет, можно создать [бесплатную учетную запись](https://azure.microsoft.com/pricing/free-trial/) всего за несколько минут.

## <a name="create-an-iot-hub"></a>Создание Центра Интернета вещей

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Получение строки подключения для центра Интернета вещей

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Регистрация нового устройства в центре Интернета вещей

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-a-simulated-device-app"></a>Создание приложения виртуального устройства

В этом разделе вы создадите консольное приложение .NET, которое отвечает на вызов прямого метода, выполняемый серверной частью решения.

1. В Visual Studio добавьте в текущее решение проект классического приложения Windows на языке Visual C# с помощью шаблона проекта **консольного приложения** . Присвойте проекту имя **SimulatedDeviceMethods**.
   
    ![Новое классическое приложение устройства Windows на языке Visual C#](./media/iot-hub-csharp-csharp-schedule-jobs/create-device-app.png)
    
2. В обозревателе решений щелкните правой кнопкой мыши проект **SimulateDeviceMethods** и выберите пункт **Управление пакетами NuGet...**.

3. В окне **Диспетчер пакетов NuGet** выберите **Обзор** и найдите **Microsoft.Azure.Devices.Client**. Выберите **Установить**, чтобы установить пакет **Microsoft.Azure.Devices.Client**, и примите условия использования. В результате выполняется скачивание и установка пакета NuGet [SDK для устройств Azure IoT](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) и его зависимостей, а также добавляется соответствующая ссылка.
   
    ![Клиентское приложение в окне "Диспетчер пакетов NuGet"](./media/iot-hub-csharp-csharp-schedule-jobs/device-app-nuget.png)

4. Добавьте следующие инструкции `using` в начало файла **Program.cs** :
   
    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;
    ```

5. Добавьте следующие поля в класс **Program** . Замените значение заполнителя строкой подключения устройства, записанной в предыдущем разделе.

    ```csharp
    static string DeviceConnectionString = "<yourDeviceConnectionString>";
    static DeviceClient Client = null;
    ```

6. Добавьте следующий код, чтобы реализовать прямой метод на устройстве:

    ```csharp
    static Task<MethodResponse> LockDoor(MethodRequest methodRequest, object userContext)
    {
        Console.WriteLine();
        Console.WriteLine("Locking Door!");
        Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);
            
        string result = "'Door was locked.'";
        return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
    }
    ```

7. Добавьте следующий код, чтобы реализовать прослушиватель двойников устройства на устройстве:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, 
      object userContext)
    {
        Console.WriteLine("Desired property change:");
        Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
    }
    ```

8. И наконец, добавьте этот код в метод **Main**, чтобы открыть подключение к Центру Интернета вещей и инициализировать прослушиватель метода:
   
    ```csharp
    try
    {
        Console.WriteLine("Connecting to hub");
        Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, 
          TransportType.Mqtt);

        Client.SetMethodHandlerAsync("LockDoor", LockDoor, null);
        Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null);

        Console.WriteLine("Waiting for direct method call and device twin update\n Press enter to exit.");
        Console.ReadLine();

        Console.WriteLine("Exiting...");

        Client.SetMethodHandlerAsync("LockDoor", null, null);
        Client.CloseAsync().Wait();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```
        
9. Сохраните результаты работы и создайте решение.         

> [!NOTE]
> Для простоты в этом руководстве не реализуются политики повтора. В рабочем коде следует реализовать политики повтора (например, повторную попытку подключения), как указано в статье [Обработка временных сбоев](/azure/architecture/best-practices/transient-faults).
> 

## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a>Планирование заданий для вызова прямого метода и обновления свойств двойника устройства

В этом разделе вы создадите консольное приложение .NET на языке C#, которое использует задания для вызова прямого метода **LockDoor** и для обновления свойств на нескольких устройствах.

1. В Visual Studio добавьте в текущее решение проект классического приложения Windows на языке Visual C# с помощью шаблона проекта **консольного приложения** . Назовите проект **ScheduleJob**.

    ![Новый проект классического приложения Windows на языке Visual C#](./media/iot-hub-csharp-csharp-schedule-jobs/createnetapp.png)

2. В обозревателе решений щелкните правой кнопкой мыши проект **ScheduleJob** и выберите **Управление пакетами NuGet…**.

3. В окне **Диспетчер пакетов NuGet** нажмите кнопку **Обзор**, найдите **microsoft.azure.devices**, щелкните **Установить**, чтобы установить пакет **Microsoft.Azure.Devices**, и примите условия использования. В результате выполняется скачивание и установка пакета NuGet [SDK для служб Azure IoT](https://www.nuget.org/packages/Microsoft.Azure.Devices/) и его зависимостей, а также добавляется соответствующая ссылка.

    ![Окно "Диспетчер пакетов NuGet"](./media/iot-hub-csharp-csharp-schedule-jobs/servicesdknuget.png)

4. Добавьте следующие инструкции `using` в начало файла **Program.cs** :
    
    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

5. Добавьте следующую инструкцию `using`, если она отсутствует в инструкциях по умолчанию.

    ```csharp
    using System.Threading;
    using System.Threading.Tasks;
    ```

6. Добавьте следующие поля в класс **Program** . Замените заполнители строкой подключения Центра Интернета вещей, созданной в предыдущем разделе, и именем устройства.

    ```csharp
    static JobClient jobClient;
    static string connString = "<yourIotHubConnectionString>";
    static string deviceId = "<yourDeviceId>";
    ```

7. Добавьте следующий метод в класс **Program** .

    ```csharp
    public static async Task MonitorJob(string jobId)
    {
        JobResponse result;
        do
        {
            result = await jobClient.GetJobAsync(jobId);
            Console.WriteLine("Job Status : " + result.Status.ToString());
            Thread.Sleep(2000);
        } while ((result.Status != JobStatus.Completed) && 
          (result.Status != JobStatus.Failed));
    }
    ```

8. Добавьте следующий метод в класс **Program** .

    ```csharp
    public static async Task StartMethodJob(string jobId)
    {
        CloudToDeviceMethod directMethod = 
          new CloudToDeviceMethod("LockDoor", TimeSpan.FromSeconds(5), 
          TimeSpan.FromSeconds(5));
       
        JobResponse result = await jobClient.ScheduleDeviceMethodAsync(jobId,
            $"DeviceId IN ['{deviceId}']",
            directMethod,
            DateTime.UtcNow,
            (long)TimeSpan.FromMinutes(2).TotalSeconds);

        Console.WriteLine("Started Method Job");
    }
    ```

9. Добавьте другой метод в класс **Program**.

    ```csharp
    public static async Task StartTwinUpdateJob(string jobId)
    {
        Twin twin = new Twin(deviceId);
        twin.Tags = new TwinCollection();
        twin.Tags["Building"] = "43";
        twin.Tags["Floor"] = "3";
        twin.ETag = "*";

        twin.Properties.Desired["LocationUpdate"] = DateTime.UtcNow;

        JobResponse createJobResponse = jobClient.ScheduleTwinUpdateAsync(
            jobId,
            $"DeviceId IN ['{deviceId}']", 
            twin, 
            DateTime.UtcNow, 
            (long)TimeSpan.FromMinutes(2).TotalSeconds).Result;

        Console.WriteLine("Started Twin Update Job");
    }
    ```

    > [!NOTE]
    > Дополнительные сведения о синтаксисе запросов см. в статье [Язык запросов Центра Интернета вещей для двойников устройств и двойников модулей, заданий и маршрутизации сообщений](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-query-language).
    > 

10. Наконец, добавьте следующие строки в метод **Main** :

    ```csharp
    Console.WriteLine("Press ENTER to start running jobs.");
    Console.ReadLine();

    jobClient = JobClient.CreateFromConnectionString(connString);

    string methodJobId = Guid.NewGuid().ToString();

    StartMethodJob(methodJobId);
    MonitorJob(methodJobId).Wait();
    Console.WriteLine("Press ENTER to run the next job.");
    Console.ReadLine();

    string twinUpdateJobId = Guid.NewGuid().ToString();

    StartTwinUpdateJob(twinUpdateJobId);
    MonitorJob(twinUpdateJobId).Wait();
    Console.WriteLine("Press ENTER to exit.");
    Console.ReadLine();
    ```

11. Сохраните результаты работы и создайте решение. 

## <a name="run-the-apps"></a>Запуск приложений

Теперь все готово к запуску приложений.

1. В обозревателе решений Visual Studio щелкните решение правой кнопкой мыши и выберите пункт **Сборка**. **Несколько запускаемых проектов**. Убедитесь, что `SimulateDeviceMethods` находится в верхней части списка, за которым следует `ScheduleJob`. Установите значение **Запуск** для действий и щелкните **ОК**.

2. Запустите проекты, щелкнув **Запуск**, или перейдите к меню **Отладка** и щелкните **Начать отладку**.

3. Отобразятся выходные данные с устройства и серверных приложений.

    ![Выполнение приложений для планирования заданий](./media/iot-hub-csharp-csharp-schedule-jobs/schedulejobs.png)

## <a name="next-steps"></a>Дальнейшие действия

В этом учебнике описано использование задания для планирования прямого метода на устройстве и обновления свойств двойника устройства.

Чтобы продолжить знакомство с центром Интернета вещей и шаблонами управления устройствами, такими как удаленное обновление встроенного по воздуху по, прочитайте [руководства: Как обновить встроенное по](tutorial-firmware-update.md).

Сведения о развертывании решений на базе искусственного интеллекта на пограничных устройствах с помощью Azure IoT Edge см. в руководстве [Развертывание первого модуля IoT Edge на устройстве под управлением 64-разрядной ОС Linux](../iot-edge/tutorial-simulate-device-linux.md).