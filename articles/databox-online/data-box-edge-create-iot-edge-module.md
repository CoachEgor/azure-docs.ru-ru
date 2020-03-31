---
title: Модуль Azure IoT Edge для Azure Data Box Edge на языке C# | Документация Майкрософт
description: Сведения о разработке на языке C# модуля IoT Edge, который можно развернуть в Data Box Edge.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 08/06/2019
ms.author: alkohli
ms.openlocfilehash: 3aa1190fb713c2fbdedcb1ce84a65d4263693827
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "78942555"
---
# <a name="develop-a-c-iot-edge-module-to-move-files-on-data-box-edge"></a>Разработка модуля IoT Edge для перемещения файлов на Data Box Edge

В этой статье описано, как создать модуль IoT Edge, предназначенный для развертывания на устройство Data Box Edge. Azure Data Box Edge — это решение хранилища, с помощью которого можно обрабатывать данные и отправлять их по сети в Azure.

Модули Azure IoT Edge можно использовать вместе с Data Box Edge, чтобы преобразовывать данные при передаче в Azure. Используемый в этой статье модуль реализует логику копирования файла из общего локального ресурса в общий облачный ресурс на устройстве Data Box Edge.

Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Создание реестра контейнеров для хранения модулей (в виде образов Docker) и управление ими.
> * Создание модуля IoT Edge, предназначенного для развертывания на устройстве Data Box Edge.


## <a name="about-the-iot-edge-module"></a>Общая информация о модулях IoT Edge

На устройстве Data Box Edge можно развертывать и выполнять модули IoT Edge. Модули Edge — это по сути контейнеры Docker, предназначенные для выполнения конкретной задачи, например для приема сообщений от устройства, преобразования сообщений или отправки сообщений в Центр Интернета вещей. В этой статье описано, как создать модуль, который копирует файлы из общего локального ресурса в общий облачный ресурс на устройстве Data Box Edge.

1. Файлы сохраняются в общий локальный ресурс на устройстве Data Box Edge.
2. Генератор файловых событий создает файловое событие для каждого файла, сохраненного в общий локальный ресурс. События файла также генерируются при изменении файла. Эти файловые события отправляются в концентратор IoT Edge, который расположен в среде выполнения IoT Edge.
3. Пользовательский модуль IoT Edge обрабатывает файловое событие и создает объект файлового события с относительным путем к соответствующему файлу. Модуль вычисляет абсолютный путь, используя этот относительный путь, и копирует файл из общего локального ресурса в общий облачный ресурс. Затем модуль удаляет файл из общего локального ресурса.

![Как работает модуль Azure IoT Edge на устройстве Data Box Edge](./media/data-box-edge-create-iot-edge-module/how-module-works-1.png)

Как только файл попадает в общий облачный ресурс, он автоматически загружается в учетную запись хранения Azure.

## <a name="prerequisites"></a>Предварительные требования

Перед началом работы убедитесь, что у вас есть следующие ресурсы:

- Работающее устройство Data Box Edge.

    - Связанный с этим устройством ресурс Центра Интернета вещей.
    - Настроенная на устройстве роль вычислений Edge.
    Для получения дополнительной информации перейдите на [настройку вычислений](data-box-edge-deploy-configure-compute.md#configure-compute) для вашего края коробки данных.

- Ресурсы для разработки:

    - [Визуальный код студии](https://code.visualstudio.com/).
    - [Си для визуального кода студии (питание от OmniSharp) расширение](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).
    - [Расширение Azure IoT Edge для Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).
    - [Пакет SDK для .NET Core 2.1](https://www.microsoft.com/net/download).
    - [Докер CE](https://store.docker.com/editions/community/docker-ce-desktop-windows). Вам может потребоваться учетная запись для скачивания и установки программного обеспечения.

## <a name="create-a-container-registry"></a>Создание реестра контейнеров

Реестр контейнеров Azure — это частный реестр Docker в Azure, где можно хранить частные образы контейнеров Docker и управлять ими. Две самые популярные облачные службы реестров Docker — Реестр контейнеров Azure и Центр Docker. В этой статье используется Реестр контейнеров.

1. Войти на портал Azure по адресу [https://portal.azure.com](https://portal.azure.com).
2. Выберите **Создать ресурс > Контейнеры > Реестр контейнеров**. Нажмите кнопку **Создать**.
3. Сделайте следующее:

   1. Создайте уникальное в пределах Azure **имя реестра**, которое содержит от 5 до 50 буквенно-цифровых символов.
   2. Выберите **подписку**.
   3. Выберите существующую **группу ресурсов** или создайте новую.
   4. Выберите **местоположение**. Мы рекомендуем выбрать то же расположение, которое связано с ресурсом Data Box Edge.
   5. **Включите** параметр **Пользователь-администратор**.
   6. Для номера SKU выберите вариант **Базовый**.

      ![Создание реестра контейнеров](./media/data-box-edge-create-iot-edge-module/create-container-registry-1.png)
 
4. Выберите **Создать**.
5. После создания реестра контейнеров просмотрите его и выберите **Ключи доступа**.

    ![Получение ключей доступа](./media/data-box-edge-create-iot-edge-module/get-access-keys-1.png)
 
6. Скопируйте значения **Сервер входа**, **Имя пользователя** и **Пароль**. Эти значения вам потребуются позже при публикации образа Docker в реестре и добавлении учетных данных реестра в среду выполнения Azure IoT Edge.


## <a name="create-an-iot-edge-module-project"></a>Создание проекта модуля IoT Edge

На следующих шагах создается проект модуля IoT Edge на основе пакета SDK для .NET Core 2.1. В этом проекте используется Visual Studio Code с расширением Azure IoT Edge.

### <a name="create-a-new-solution"></a>Создание решения

Создайте шаблон решения C#, который можно настроить с помощью собственного кода.

1. В Visual Studio Code выберите **Представление > Палитра команд**, чтобы открыть палитру команд VS Code.
2. В палитре команд введите и выполните команду **Azure: Sign in** (Azure: Вход) и следуйте инструкциям, чтобы войти в свою учетную запись Azure. Если вход был выполнен, то этот шаг можно пропустить.
3. В палитре команд введите и запустите команду **Azure IoT Edge: Новое решение IoT Edge.** В палитре команд укажите следующие сведения для создания решения:

    1. Выберите папку, где требуется создать решение.
    2. Введите имя своего решения или примите имя по умолчанию **EdgeSolution**.
    
        ![Создание решения 1](./media/data-box-edge-create-iot-edge-module/create-new-solution-1.png)

    3. Выберите **модуль C'** в качестве шаблона модуля.
    4. Замените имя модуля, предложенное по умолчанию, любым именем на ваш выбор. В нашем примере это **FileCopyModule**.
    
        ![Создание решения 2](./media/data-box-edge-create-iot-edge-module/create-new-solution-2.png)

    5. В качестве репозитория образов первого модуля укажите Реестр контейнеров Azure, который вы создали в предыдущем разделе. Замените **localhost:5000** скопированным значением имени входа на сервер.

        Строка должна выглядеть так: `<Login server name>/<Module name>`. В нашем примере это строка `mycontreg2.azurecr.io/filecopymodule`.

        ![Создание решения 3](./media/data-box-edge-create-iot-edge-module/create-new-solution-3.png)

4. Выберите **Файл > Открыть папку**.

    ![Создание решения 4](./media/data-box-edge-create-iot-edge-module/create-new-solution-4.png)

5. Найдите и выберите папку **EdgeSolution**, которую вы создали ранее. Окно VS Code загружает рабочую область решения IoT Edge, в которой размещены пять элементов верхнего уровня. В этой статье мы не будем изменять папку **.vscode**, а также файлы **.gitignore**, **.env** и **deployment.template.json**.
    
    Сейчас из этих компонентов нас интересует только папка modules. Эта папка содержит код C# для модуля и файлы Docker для сборки модуля в образ контейнера.

    ![Создание решения 5](./media/data-box-edge-create-iot-edge-module/create-new-solution-5.png)

### <a name="update-the-module-with-custom-code"></a>Обновление модуля с помощью пользовательского кода

1. В VS Code Explorer, открытые **модули > FileCopyModule > Program.cs**.
2. В верхней части **пространства имен CSharpModule** добавьте следующие операторы using для типов, которые будут использоваться позже. **Microsoft.Azure.Devices.Client.Transport.Mqtt** — это протокол отправки сообщений в центр IoT Edge.

    ```
    namespace FileCopyModule
    {
        using Microsoft.Azure.Devices.Client.Transport.Mqtt;
        using Newtonsoft.Json;
    ```
3. Добавьте в класс Program переменные **InputFolderPath** и **OutputFolderPath**.

    ```
    class Program
        {
            static int counter;
            private const string InputFolderPath = "/home/input";
            private const string OutputFolderPath = "/home/output";
    ```

4. Сразу после предыдущего шага добавьте класс **FileEvent** для определения тела сообщений.

    ```
    /// <summary>
    /// The FileEvent class defines the body of incoming messages. 
    /// </summary>
    private class FileEvent
    {
        public string ChangeType { get; set; }

        public string ShareRelativeFilePath { get; set; }

        public string ShareName { get; set; }
    }
    ```

5. В **методе Init**код создает и настраивает объект **ModuleClient.** Этот объект позволяет модулю подключаться к локальной среде выполнения Azure IoT Edge по протоколу MQTT для отправки и получения сообщений. Строка подключения, используемая в методе Init, предоставляется модулю средой выполнения IoT Edge. Этот код регистрирует обратный вызов FileCopy для получения сообщений от центра IoT Edge через конечную точку **input1**. Замените **метод Init** следующим кодом.

    ```
    /// <summary>
    /// Initializes the ModuleClient and sets up the callback to receive
    /// messages containing file event information
    /// </summary>
    static async Task Init()
    {
        MqttTransportSettings mqttSetting = new MqttTransportSettings(TransportType.Mqtt_Tcp_Only);
        ITransportSettings[] settings = { mqttSetting };

        // Open a connection to the IoT Edge runtime
        ModuleClient ioTHubModuleClient = await ModuleClient.CreateFromEnvironmentAsync(settings);
        await ioTHubModuleClient.OpenAsync();
        Console.WriteLine("IoT Hub module client initialized.");

        // Register callback to be called when a message is received by the module
        await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FileCopy, ioTHubModuleClient);
    }
    ```

6. Удалите код для **метода PipeMessage** и на его месте, вставьте код для **FileCopy**.

    ```
        /// <summary>
        /// This method is called whenever the module is sent a message from the IoT Edge Hub.
        /// This method deserializes the file event, extracts the corresponding relative file path, and creates the absolute input file path using the relative file path and the InputFolderPath.
        /// This method also forms the absolute output file path using the relative file path and the OutputFolderPath. It then copies the input file to output file and deletes the input file after the copy is complete.
        /// </summary>
        static async Task<MessageResponse> FileCopy(Message message, object userContext)
        {
            int counterValue = Interlocked.Increment(ref counter);

            try
            {
                byte[] messageBytes = message.GetBytes();
                string messageString = Encoding.UTF8.GetString(messageBytes);
                Console.WriteLine($"Received message: {counterValue}, Body: [{messageString}]");

                if (!string.IsNullOrEmpty(messageString))
                {
                    var fileEvent = JsonConvert.DeserializeObject<FileEvent>(messageString);

                    string relativeFileName = fileEvent.ShareRelativeFilePath.Replace("\\", "/");
                    string inputFilePath = InputFolderPath + relativeFileName;
                    string outputFilePath = OutputFolderPath + relativeFileName;

                    if (File.Exists(inputFilePath))                
                    {
                        Console.WriteLine($"Moving input file: {inputFilePath} to output file: {outputFilePath}");
                        var outputDir = Path.GetDirectoryName(outputFilePath);
                        if (!Directory.Exists(outputDir))
                        {
                            Directory.CreateDirectory(outputDir);
                        }

                        File.Copy(inputFilePath, outputFilePath, true);
                        Console.WriteLine($"Copied input file: {inputFilePath} to output file: {outputFilePath}");
                        File.Delete(inputFilePath);
                        Console.WriteLine($"Deleted input file: {inputFilePath}");
                    } 
                    else
                    {
                        Console.WriteLine($"Skipping this event as input file doesn't exist: {inputFilePath}");   
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Caught exception: {0}", ex.Message);
                Console.WriteLine(ex.StackTrace);
            }

            Console.WriteLine($"Processed event.");
            return MessageResponse.Completed;
        }
    ```

7. Сохраните этот файл.
8. Вы также можете [скачать существующий образец кода](https://azure.microsoft.com/resources/samples/data-box-edge-csharp-modules/?cdn=disable) для этого проекта. Затем можно проверить сохраненный файл **в program.cs** файлв в этом примере.

## <a name="build-your-iot-edge-solution"></a>Сборка решения IoT Edge

В предыдущем разделе описано, как создать решение IoT Edge и добавить в FileCopyModule код для копирования файлов из общего локального ресурса в общий облачный ресурс. Теперь необходимо создать решение в качестве образа контейнера и передать его в реестр контейнеров.

1. В VSCode, перейдите на терминал > новый терминал, чтобы открыть новый визуальный код студии интегрированного терминала.
2. Войдя в Docker, введя следующую команду в интегрированном терминале.

    `docker login <ACR login server> -u <ACR username>`

    Укажите имя пользователя и сервер входа, которые вы скопировали из Реестра контейнеров Azure.

    ![Сборка и отправка решения IoT Edge](./media/data-box-edge-create-iot-edge-module/build-iot-edge-solution-1.png)

2. В ответ на запрос введите пароль. Сервер входа, имя пользователя и пароль можно также получить в разделе **Ключи доступа** Реестра контейнеров на портале Azure.
 
3. Предоставив учетные данные, вы сможете отправить образ модуля в Реестр контейнеров Azure. В обозревателе VS Code щелкните правой кнопкой мыши файл **module.json** и выберите действие **Build and Push IoT Edge solution** (Собрать и отправить решение IoT Edge).

    ![Сборка и отправка решения IoT Edge](./media/data-box-edge-create-iot-edge-module/build-iot-edge-solution-2.png)
 
    Получив команду на сборку решения, Visual Studio Code выполнит в интегрированном окне терминала две команды: docker build и docker push. Они создают код, упаковывают его в контейнер CSharpModule.dll и передают в реестр контейнера, который был указан при инициализации решения.

    Вам будет предложено выбрать платформу модуля. Выберите *amd64*, которая соответствует Linux.

    ![Выбор платформы](./media/data-box-edge-create-iot-edge-module/select-platform.png)

    > [!IMPORTANT] 
    > Поддерживаются только модули Linux.

    Возможно, вы увидите следующее предупреждение, которое можно игнорировать:

    *Program.cs (77,44): предупреждение CS1998: Этот метод async не хватает "ждать" операторов и будет работать синхронно. Рассмотрите возможность использования оператора 'await' для ожидания неблокирующих вызовов API или 'ожидайте Task.Run(...)', чтобы сделать работу, связанную с процессором, на фоновом потоке.*

4. Полный адрес образа контейнера с тегом можно увидеть в окне интегрированного терминала VS Code. Адрес образа создается на основе информации, сохраненной в файле module.json в формате `<repository>:<version>-<platform>`. Для этой статьи адрес должен выглядеть так: `mycontreg2.azurecr.io/filecopymodule:0.0.1-amd64`.

## <a name="next-steps"></a>Дальнейшие действия

Чтобы развернуть и запустить этот модуль на [Add a module](data-box-edge-deploy-configure-compute.md#add-a-module)Data Box Edge, см.
