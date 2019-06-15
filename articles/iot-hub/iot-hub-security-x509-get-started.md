---
title: Руководство по использованию сертификата X.509 для обеспечения безопасности в Центре Интернета вещей Azure | Документация Майкрософт
description: Приступая к работе с защитой на основе сертификата X.509 в Центре Интернета вещей Azure в смоделированной среде.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/10/2017
ms.openlocfilehash: 0bfb66f54ec09e86b46a41499211e93a0083e8d1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65779925"
---
# <a name="set-up-x509-security-in-your-azure-iot-hub"></a>Настройка сертификата безопасности X.509 в Центре Интернета вещей Azure

Это руководство имитирует действия, необходимые для обеспечения защиты в Центре Интернета вещей за счет *аутентификации на основе сертификата X.509*. В качестве примера мы продемонстрируем, как использовать инструмент OpenSSL с открытым кодом для локального создания сертификатов на компьютере Windows. Мы рекомендуем использовать это руководство только для тестирования. Для рабочей среды следует приобрести сертификаты в *корневом центре сертификации (ЦС)* .

## <a name="prerequisites"></a>Технические условия

Для работы с этим руководством вам потребуется подготовить следующие ресурсы:

* Центр Интернета вещей, созданный в вашей подписке Azure. Дополнительные сведения см. в статье [Создание Центра Интернета вещей с помощью портала Azure](iot-hub-create-through-portal.md).

* У вас есть [Visual Studio 2017 или Visual Studio 2019](https://www.visualstudio.com/vs/) установлена на вашем компьютере.

## <a name="get-x509-ca-certificates"></a>Получение сертификатов ЦС X.509

Для обеспечения безопасности на основе сертификатов X.509 в Центре Интернета вещей вам понадобится [цепочка сертификатов X.509](https://en.wikipedia.org/wiki/X.509#Certificate_chains_and_cross-certification), которая включает корневой сертификат, а также любые промежуточные сертификаты вплоть до конечного.

Для получения сертификатов можно воспользоваться одним из следующих способов.

* Приобретите сертификат X.509 в *корневом центре сертификации (ЦС)* . Рекомендуется использовать этот способ для рабочей среды.

* Создайте собственные сертификаты X.509 с помощью стороннего инструмента, например [OpenSSL](https://www.openssl.org/). Это способ хорошо подходит для разработки и тестирования. Сведения о создании тестовых сертификатов ЦС с помощью PowerShell или Bash см. в статье [Managing test CA certificates for samples and tutorials](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md). В остальной части этого руководства используются тестовые сертификаты ЦС, созданные по инструкциям в статье [Managing test CA certificates for samples and tutorials](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md).

* Создать [промежуточный сертификат ЦС X.509](iot-hub-x509ca-overview.md#sign-devices-into-the-certificate-chain-of-trust) подписи с использованием существующего сертификата корневого ЦС и отправьте его в центр Интернета вещей. Если промежуточный сертификат отправлен и проверить, как описано ниже, его можно использовать вместо сертификата корневого ЦС, приведенные ниже. Средства, такие как OpenSSL ([openssl req](https://www.openssl.org/docs/manmaster/man1/openssl-req.html) и [ЦС openssl](https://www.openssl.org/docs/manmaster/man1/openssl-ca.html)) можно использовать для создания и подписания промежуточный сертификат ЦС.


## <a name="register-x509-ca-certificates-to-your-iot-hub"></a>Регистрация сертификата ЦС X.509 в Центре Интернета вещей

Ниже описано, как добавить новый центр сертификации в Центр Интернета вещей с помощью портала.

1. На портале Azure перейдите к Центру Интернета вещей и откройте меню **Параметры** > **Сертификаты**.

2. Выберите **Добавить**, чтобы добавить новый сертификат.

3. Введите понятное отображаемое имя для сертификата. Выберите файл корневого сертификата с именем *RootCA.cer*, созданный в предыдущем разделе на вашем компьютере. Щелкните **Отправить**.

4. После получения уведомления об успешной отправке сертификата выберите **Сохранить**.

    ![Передача сертификата](./media/iot-hub-security-x509-get-started/add-new-cert.png)  

   Вы увидите свой сертификат в списке **обозревателя сертификатов**. Обратите внимание, что этот сертификат имеет **состояние** *Не проверено*.

5. Щелкните сертификат, добавленный на предыдущем шаге.

6. В колонке **Сведения о сертификате** выберите **Generate Verification Code** (Создать код проверки).

7. Будет создан **код проверки** для проверки владельца сертификата. Скопируйте код в буфер обмена.

   ![Проверка сертификата](./media/iot-hub-security-x509-get-started/verify-cert.png)  

8. Теперь необходимо подписать *код проверки* с помощью закрытого ключа, связанного с вашим сертификатом ЦС X.509, который создает подпись. Выполнить этот процесс подписания можно с помощью инструментов, таких как OpenSSL. Это процесс называется [проверкой принадлежности](https://tools.ietf.org/html/rfc5280#section-3.1). В шаге 3 руководства [Managing test CA certificates for samples and tutorials](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) описывается создание кода проверки.

9. Отправьте итоговую подпись из шага 8 выше в Центр Интернета вещей на портале. В колонке **Сведения о сертификате** на портале Azure перейдите к **файлу PEM или CER сертификата проверки**, а затем выберите подпись, например *VerifyCert4.cer*, созданную с помощью примера команды PowerShell, а также значка _проводника_.

10. После успешной передачи сертификата выберите **Проверить**. В колонке **Сертификаты** **состояние** вашего сертификата изменится на **_Проверено_** . Если оно не обновляется автоматически, выберите **Обновить**.

    ![Проверка отправки сертификата](./media/iot-hub-security-x509-get-started/upload-cert-verification.png)  

## <a name="create-an-x509-device-for-your-iot-hub"></a>Создание устройства X.509 для Центра Интернета вещей

1. На портале Azure перейдите к странице Центра Интернета вещей **Обозреватели > 	Устройства Интернета вещей**.

2. Щелкните **+ Добавить**, чтобы добавить новое устройство.

3. Присвойте понятное отображаемое имя **идентификатору устройства** и в качестве **типа аутентификации** выберите **_X.509 CA Signed_** (Подписан ЦС X.509). Выберите команду **Сохранить**.

   ![Создание устройства X.509 на портале](./media/iot-hub-security-x509-get-started/create-x509-device.png)

## <a name="authenticate-your-x509-device-with-the-x509-certificates"></a>Аутентификация устройства X.509 с помощью сертификатов X.509

Чтобы аутентифицировать устройство X.509, сначала необходимо подписать устройство с помощью сертификата ЦС. Подписывание конечных устройств обычно выполняется на заводе, где соответствующим образом включены производственные инструменты. Так как устройство переходит от одного производителя к другому, действие подписывания каждого производителя записывается в качестве промежуточного сертификата в цепочке. Конечным результатом является цепочка сертификатов от сертификатов ЦС до конечных сертификатов устройства. В шаге 4 руководства [Managing test CA certificates for samples and tutorials](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) описывается создание сертификата устройства.

Далее мы рассмотрим, как создать приложение C#, чтобы смоделировать устройство X.509, зарегистрированное для вашего Центра Интернета вещей. Мы отправим значения температуры и влажности с виртуального устройства в ваш центр. Обратите внимание, что в этом руководстве мы создадим только приложение устройства. Создание приложения службы Центра Интернета вещей, которое будет отправлять ответы на события, отправленные этим виртуальным устройством, останется в качестве упражнения для читателей. В примере приложения C# предполагается, что вы выполнили процедуру, описанную в статье [Managing test CA certificates for samples and tutorials](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md).

1. В Visual Studio создайте проект классического приложения Windows на языке Visual C# с помощью шаблона проекта консольного приложения. Назовите проект **SimulateX509Device**.

   ![Создание проекта устройства X.509 в Visual Studio](./media/iot-hub-security-x509-get-started/create-device-project.png)

2. В обозревателе решений щелкните правой кнопкой мыши проект **SimulateX509Device** и выберите **Управление пакетами NuGet...** . В окне "Диспетчер пакетов NuGet" выберите **Обзор** и найдите **microsoft.azure.devices.client**. Выберите **Установить**, чтобы установить пакет **Microsoft.Azure.Devices.Client**, и примите условия использования. В результате выполняется скачивание и установка пакета NuGet SDK для устройств Azure IoT и его зависимостей, а также добавляется соответствующая ссылка.

   ![Добавление пакета SDK NuGet устройства в Visual Studio](./media/iot-hub-security-x509-get-started/device-sdk-nuget.png)

3. Добавьте следующие строки кода в начало файла *Program.cs*.

    ```CSharp
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using System.Security.Cryptography.X509Certificates;
    ```

4. Добавьте следующие строки кода внутри класса **Program**.

    ```CSharp
        private static int MESSAGE_COUNT = 5;
        private const int TEMPERATURE_THRESHOLD = 30;
        private static String deviceId = "<your-device-id>";
        private static float temperature;
        private static float humidity;
        private static Random rnd = new Random();
    ```

     Используйте понятное имя устройства, используемое на предыдущем шаге, вместо заполнителя _<идентификатор_вашего_устройства>_ .

5. Добавьте следующую функцию, чтобы создать случайные числа для значений температуры и влажности и отправить их в центр.

    ```CSharp
    static async Task SendEvent(DeviceClient deviceClient)
    {
        string dataBuffer;
        Console.WriteLine("Device sending {0} messages to IoTHub...\n", MESSAGE_COUNT);

        for (int count = 0; count < MESSAGE_COUNT; count++)
        {
            temperature = rnd.Next(20, 35);
            humidity = rnd.Next(60, 80);
            dataBuffer = string.Format("{{\"deviceId\":\"{0}\",\"messageId\":{1},\"temperature\":{2},\"humidity\":{3}}}", deviceId, count, temperature, humidity);
            Message eventMessage = new Message(Encoding.UTF8.GetBytes(dataBuffer));
            eventMessage.Properties.Add("temperatureAlert", (temperature > TEMPERATURE_THRESHOLD) ? "true" : "false");
            Console.WriteLine("\t{0}> Sending message: {1}, Data: [{2}]", DateTime.Now.ToLocalTime(), count, dataBuffer);

            await deviceClient.SendEventAsync(eventMessage);
        }
    }
    ```

6. Наконец, добавьте следующую функцию **Main**, заменив заполнители _device-id_, _your-iot-hub-name_ и _absolute-path-to-your-device-pfx-file_ согласно требованиям программы настройки.

    ```CSharp
    try
    {
        var cert = new X509Certificate2(@"<absolute-path-to-your-device-pfx-file>", "1234");
        var auth = new DeviceAuthenticationWithX509Certificate("<device-id>", cert);
        var deviceClient = DeviceClient.Create("<your-iot-hub-name>.azure-devices.net", auth, TransportType.Amqp_Tcp_Only);

        if (deviceClient == null)
        {
            Console.WriteLine("Failed to create DeviceClient!");
        }
        else
        {
            Console.WriteLine("Successfully created DeviceClient!");
            SendEvent(deviceClient).Wait();
        }

        Console.WriteLine("Exiting...\n");
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```

   Этот код подключается к Центру Интернета вещей, создавая строку подключения для устройства X.509. После успешного подключения он отправляет события температуры и влажности в центр и ожидает его ответа. 
7. Так как это приложение получает доступ к *PFX*-файлу, может потребоваться запустить режим *администратора*. Создайте решение Visual Studio. Откройте новое командное окно в качестве **администратора** и перейдите к папке, содержащей это решение. Перейдите в каталог *bin/Debug* в папке решения. Запустите приложение **SimulateX509Device.exe** из командного окна _администратора_. Устройство должно быть успешно подключено к центру и отправлять события. 

   ![Запуск приложения устройства](./media/iot-hub-security-x509-get-started/device-app-success.png)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о безопасности решения IoT см.:

* [Рекомендации по обеспечению безопасности Интернета вещей](../iot-fundamentals/iot-security-best-practices.md)

* [Архитектура безопасности Интернета вещей](../iot-fundamentals/iot-security-architecture.md)

* [Защита развертывания Интернета вещей](../iot-fundamentals/iot-security-deployment.md)

Для дальнейшего изучения возможностей Центра Интернета вещей см. следующие статьи:

* [Краткое руководство. Развертывание первого модуля IoT Edge на устройстве под управлением 64-разрядной ОС Linux](../iot-edge/tutorial-simulate-device-linux.md)
