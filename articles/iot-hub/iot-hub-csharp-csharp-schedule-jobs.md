---
title: Планирование заданий c помощью Центра Интернета вещей Azure (.NET и .NET) | Документация Майкрософт
description: Планирование заданий с помощью Центра Интернета вещей Azure для вызова прямого метода на нескольких устройствах. Используйте пакеты SDK для устройств Azure IoT для .NET, чтобы реализовать приложения имитации устройства и приложение службы, на которых будет выполнено задание.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: robinsh
ms.custom: mqtt
ms.openlocfilehash: 4c71a108d1967027465d127db50737119af3e2c1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81733372"
---
# <a name="schedule-and-broadcast-jobs-net"></a>Планирование и трансляция заданий (.NET)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Центр Интернета вещей Azure позволяет планировать и отслеживать задания по обновлению для миллионов устройств. Что можно сделать с помощью заданий?

* Обновление требуемых свойств

* Обновление тегов

* Вызов прямых методов

Задание выступает в роли оболочки для одного из этих действий и отслеживает его выполнение для определенного набора устройств в соответствии с запросом на двойнике устройства. Например, внутреннее приложение может использовать задание для вызова прямого метода перезапуска на 10 000 устройств. Вы можете определить набор устройств с помощью запроса на двойнике устройства и запланировать момент времени в будущем для выполнения задания. Задание отслеживает ход выполнения по мере того, как каждое из устройств получает вызов и выполняет прямой метод перезагрузки.

Дополнительные сведения об этих возможностях см. в указанных ниже статьях.

* Двойники устройств и свойства: [Приступая к работе с двойниками устройств (предварительная версия)](iot-hub-csharp-csharp-twin-getstarted.md) и [Руководство. Настройка устройств с помощью требуемых свойств (предварительная версия)](tutorial-device-twins.md).

* Прямые методы: [Общие сведения о прямых методах и информация о вызове этих методов из Центра Интернета вещей](iot-hub-devguide-direct-methods.md) и [Использование прямых методов на устройстве Интернета вещей (Node.js)](quickstart-control-device-dotnet.md).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

В этом учебнике демонстрируется выполнение следующих действий:

* Создайте приложение устройства, которое реализует прямой метод с именем **LockDoor**, который может быть вызван внутренним приложением.

* Создание внутреннего приложения, которое создает задание для вызова прямого метода **LockDoor** на нескольких устройствах. Еще одно задание отправляет обновления требуемых свойств на несколько устройств.

По завершении работы с этим руководством у вас будет два консольных приложения .NET (C#):

* **SimulateDeviceMethods**. Это приложение подключается к центру Интернета вещей и реализует прямой метод **LockDoor** .

* **ScheduleJob**. Это приложение использует задания для вызова прямого метода **LockDoor** и обновления требуемых свойств двойникаа устройства на нескольких устройствах.

## <a name="prerequisites"></a>Предварительные условия

* приведенному.

* Активная учетная запись Azure. Если у вас нет учетной записи, можно создать [бесплатную учетную запись](https://azure.microsoft.com/pricing/free-trial/) всего за несколько минут.

* Убедитесь, что в брандмауэре открыт порт 8883. В примере для устройства в этой статье используется протокол MQTT, который обменивается данными через порт 8883. В некоторых корпоративных и академических сетях этот порт может быть заблокирован. Дополнительные сведения и способы устранения этой проблемы см. в разделе о [подключении к Центру Интернета вещей по протоколу MQTT](iot-hub-mqtt-support.md#connecting-to-iot-hub).

## <a name="create-an-iot-hub"></a>Создание Центра Интернета вещей

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Регистрация нового устройства в центре Интернета вещей

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-a-simulated-device-app"></a>Создание приложения виртуального устройства

В этом разделе вы создадите консольное приложение .NET, которое отвечает на вызов прямого метода, выполняемый серверной частью решения.

1. В Visual Studio выберите **создать новый проект**, а затем выберите шаблон проекта **консольное приложение (.NET Framework)** . Нажмите кнопку **Далее**, чтобы продолжить.

1. В окне **Настройка нового проекта**назовите проект *SimulateDeviceMethods*, а затем выберите **создать**.

    ![Настройка проекта SimulateDeviceMethods](./media/iot-hub-csharp-csharp-schedule-jobs/configure-device-app.png)

1. В обозреватель решений щелкните правой кнопкой мыши проект **SimulateDeviceMethods** , а затем выберите пункт **Управление пакетами NuGet**.

1. В **диспетчере пакетов NuGet**выберите **Обзор** и найдите и выберите **Microsoft. Azure. Devices. Client**. Выберите пункт **Установить**.

    ![Клиентское приложение в окне "Диспетчер пакетов NuGet"](./media/iot-hub-csharp-csharp-schedule-jobs/device-app-nuget.png)

    На этом шаге выполняется скачивание, установка и Добавление ссылки на пакет [SDK для устройств Azure IOT](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) и его зависимости.

1. Добавьте следующие `using` инструкции в начало файла **Program.CS** :

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;
    ```

1. Добавьте следующие поля в класс **Program** . Замените значение заполнителя строкой подключения устройства, записанной в предыдущем разделе.

    ```csharp
    static string DeviceConnectionString = "<yourDeviceConnectionString>";
    static DeviceClient Client = null;
    ```

1. Добавьте следующий код, чтобы реализовать прямой метод на устройстве:

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

1. Добавьте следующий метод для реализации прослушивателя двойникова устройства на устройстве:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, 
      object userContext)
    {
        Console.WriteLine("Desired property change:");
        Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
    }
    ```

1. И наконец, добавьте этот код в метод **Main**, чтобы открыть подключение к Центру Интернета вещей и инициализировать прослушиватель метода:

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

1. Сохраните результаты работы и создайте решение.

> [!NOTE]
> Для простоты в этом руководстве не реализованы политики повтора. В рабочем коде следует реализовать политики повтора (например, повтор подключения), как это было предложено в [обработке временных сбоев](/azure/architecture/best-practices/transient-faults).
>

## <a name="get-the-iot-hub-connection-string"></a>Получение строки подключения для центра Интернета вещей

[!INCLUDE [iot-hub-howto-schedule-jobs-shared-access-policy-text](../../includes/iot-hub-howto-schedule-jobs-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-registryrw-connection-string](../../includes/iot-hub-include-find-registryrw-connection-string.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a>Планирование заданий для вызова прямого метода и обновления свойств двойника устройства

В этом разделе вы создадите консольное приложение .NET (на C#), которое использует задания для вызова прямого метода **LockDoor** и отправки требуемых обновлений свойств на несколько устройств.

1. В Visual Studio выберите **Файл** > **Создать** > **Проект**. В окне **Создание нового проекта**выберите **консольное приложение (.NET Framework)**, а затем нажмите кнопку **Далее**.

1. В окне **Настройка нового проекта**назовите проект *ScheduleJob*. В качестве **решения**выберите **Добавить в решение**и нажмите кнопку **создать**.

    ![Имя и Настройка проекта ScheduleJob](./media/iot-hub-csharp-csharp-schedule-jobs/config-schedule-job-app.png)

1. В обозреватель решений щелкните правой кнопкой мыши проект **ScheduleJob** , а затем выберите пункт **Управление пакетами NuGet**.

1. В **диспетчере пакетов NuGet**нажмите кнопку **Обзор**, найдите и выберите **Microsoft. Azure. Devices**, а затем нажмите кнопку **установить**.

   В результате выполняется скачивание и установка пакета NuGet [SDK для служб Интернета вещей Azure](https://www.nuget.org/packages/Microsoft.Azure.Devices/) и его зависимостей, а также добавляется соответствующая ссылка.

1. Добавьте следующие `using` инструкции в начало файла **Program.CS** :

    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

1. Добавьте следующую инструкцию `using`, если она отсутствует в инструкциях по умолчанию.

    ```csharp
    using System.Threading;
    using System.Threading.Tasks;
    ```

1. Добавьте следующие поля в класс **Program** . Замените заполнители строкой подключения центра Интернета вещей, скопированным ранее в [поле получение строки подключения центра Интернета вещей](#get-the-iot-hub-connection-string) и имени устройства.

    ```csharp
    static JobClient jobClient;
    static string connString = "<yourIotHubConnectionString>";
    static string deviceId = "<yourDeviceId>";
    ```

1. Добавьте в класс **Program** следующий метод:

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

1. Добавьте в класс **Program** следующий метод:

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

1. Добавьте другой метод в класс **Program**.

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
    > Дополнительные сведения о синтаксисе запросов см. в статье [Язык запросов Центра Интернета вещей для двойников устройств, заданий и маршрутизации сообщений](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-query-language).
    >

1. Наконец, добавьте следующие строки в метод **Main** :

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

1. Сохраните результаты работы и создайте решение.

## <a name="run-the-apps"></a>Запуск приложений

Теперь все готово к запуску приложений.

1. В обозреватель решений Visual Studio щелкните решение правой кнопкой мыши и выберите команду **задать запускаемые проекты**.

1. Выберите пункт **Общие свойства** > **запускаемый проект**, а затем выберите **Несколько запускаемых проектов**.

1. Убедитесь, что `SimulateDeviceMethods` находится в верхней части списка, за которым следует `ScheduleJob`. Задайте оба действия для **запуска** и нажмите кнопку **ОК**.

1. Запустите проекты, щелкнув **Запуск**, или перейдите к меню **Отладка** и щелкните **Начать отладку**.

   Отобразятся выходные данные с устройства и серверных приложений.

    ![Выполнение приложений для планирования заданий](./media/iot-hub-csharp-csharp-schedule-jobs/schedule-jobs-console-results.png)

## <a name="next-steps"></a>Дальнейшие шаги

В этом учебнике описано использование задания для планирования прямого метода на устройстве и обновления свойств двойника устройства.

* Чтобы продолжить знакомство с Центром Интернета вещей и шаблонами управления устройствами, такими как удаленное обновление встроенного ПО, ознакомьтесь с [этим руководством](tutorial-firmware-update.md).

* Сведения о развертывании решений на базе искусственного интеллекта на пограничных устройствах с помощью Azure IoT Edge см. в руководстве [Развертывание первого модуля IoT Edge на устройстве под управлением 64-разрядной ОС Linux](../iot-edge/tutorial-simulate-device-linux.md).
