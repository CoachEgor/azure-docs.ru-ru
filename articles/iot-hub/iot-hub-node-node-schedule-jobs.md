---
title: Планирование заданий с помощью Центра Интернета вещей Azure (Node) | Документация Майкрософт
description: Планирование заданий с помощью Центра Интернета вещей Azure для вызова прямого метода на нескольких устройствах. Используйте пакеты SDK для Центра Интернета вещей Azure для Node.js, чтобы реализовать приложения имитации устройства и приложение службы, на которых будет выполнено задание.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 10/06/2017
ms.openlocfilehash: 7dc3c19db7dc71a593c3fa9bd05db37968257d31
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68668017"
---
# <a name="schedule-and-broadcast-jobs-nodejs"></a>Планирование и вещание заданий (Node. js)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Центр Интернета вещей Azure — это полностью управляемая служба, которая позволяет внутреннему приложению создавать и отслеживать задания, осуществляющие планирование и обновление миллионов устройств.  Задания можно использовать для следующих действий:

* Обновление требуемых свойств
* Обновление тегов
* Вызов прямых методов

По сути, задание включает одно из этих действий, отслеживая ход его выполнения на наборе устройств (определяется запросом двойника устройства).  Например, с помощью задания внутреннее приложение может вызывать метод перезагрузки на 10 000 устройств, определенных запросом двойника устройства и запланированных в будущем. Затем это приложение может отследить ход выполнения задания по мере получения и выполнения метода Reboot на каждом из этих устройств.

Дополнительные сведения о каждой из этих возможностей см. в следующих статьях:

* Двойник устройства и свойства: [Начало работы с двойниковами устройств](iot-hub-node-node-twin-getstarted.md) и [учебником: Использование свойств двойникаа устройства](tutorial-device-twins.md)

* Прямые методы: [Руководство разработчика для центра Интернета вещей. прямые методы](iot-hub-devguide-direct-methods.md) и [учебник. прямые методы](quickstart-control-device-node.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

В этом учебнике описаны следующие процедуры.

* Создание приложения Node.js имитации устройства с прямым методом, который позволяет выполнить действие **lockDoor** путем вызова из серверной части решения.

* Создание консольного приложения Node.js, которое с помощью задания вызывает в приложении имитации устройства прямой метод **lockDoor** и обновляет требуемые свойства с помощью задания устройства.

По завершении работы с этим руководством у вас будет два приложения Node.js:

* **simDevice.js**, которое подключается к Центру Интернета вещей с удостоверением устройства и получает прямой метод **lockDoor**.

* **scheduleJobService.js**, которое вызывает прямой метод в приложении имитации устройства и обновляет требуемые свойства двойника устройства с помощью задания.

Для работы с этим учебником требуется:

* Node. js версии 10.0. x или более поздней версии [Подготовка среды разработки](https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md) описывает установку Node. js для этого руководства в Windows или Linux.

* Активная учетная запись Azure. Если ее нет, можно создать [бесплатную учетную запись](https://azure.microsoft.com/pricing/free-trial/) всего за несколько минут.

## <a name="create-an-iot-hub"></a>Создание Центра Интернета вещей

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Создание приложения виртуального устройства

В этом разделе вы создадите консольное приложение Node.js, отвечающее на прямой метод, вызванный из облака, который активирует имитированный метод **lockDoor**.

1. Создайте пустую папку с именем **simDevice**.  В папке **simDevice** создайте файл package.json, используя следующую команду в командной строке.  Примите значения по умолчанию:

   ```
   npm init
   ```

2. В командной строке в папке **simDevice** выполните следующую команду, чтобы установить пакет SDK для устройств **azure-iot-device** и пакет **azure-iot-device-mqtt**.
   
   ```
   npm install azure-iot-device azure-iot-device-mqtt --save
   ```

3. В текстовом редакторе создайте файл **simDevice.js** в папке **simDevice**.

4. Добавьте следующие инструкции require в начало файла **simDevice.js**:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Добавьте переменную **connectionString**, чтобы создать с ее помощью экземпляр **клиента**.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Добавьте следующую функцию для обработки метода **lockDoor**.
   
    ```
    var onLockDoor = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```

7. Добавьте следующий код для регистрации обработчика для метода **lockDoor**.

   ```
   client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub. Register handler for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
   });
   ```

8. Сохраните и закройте файл **simDevice.js**.

> [!NOTE]
> Для простоты в этом руководстве не реализуются политики повтора. В рабочем коде следует реализовать политики повторных попыток (например, с экспоненциальной задержкой), как указано в статье [Обработка временных сбоев](/azure/architecture/best-practices/transient-faults).
>

## <a name="get-the-iot-hub-connection-string"></a>Получение строки подключения для центра Интернета вещей

[!INCLUDE [iot-hub-howto-schedule-jobs-shared-access-policy-text](../../includes/iot-hub-howto-schedule-jobs-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-registryrw-connection-string](../../includes/iot-hub-include-find-registryrw-connection-string.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Планирование заданий для вызова прямого метода и обновления свойств двойника устройства

В этом разделе создается консольное приложение Node.js, которое инициирует удаленное действие **lockDoor** на устройстве с помощью прямого метода и обновляет свойства двойника устройства.

1. Создайте пустую папку с именем **scheduleJobService**.  В папке **scheduleJobService** создайте файл package.json, используя следующую команду в командной строке.  Примите значения по умолчанию:

    ```
    npm init
    ```

2. В командной строке в папке **scheduleJobService** выполните следующую команду, чтобы установить пакет SDK для устройств **azure-iothub** и пакет **azure-iot-device-mqtt**.
   
    ```
    npm install azure-iothub uuid --save
    ```

3. В текстовом редакторе создайте файл **scheduleJobService.js** в папке **scheduleJobService**.

4. Добавьте следующие инструкции require в начале файла **dmpatterns_gscheduleJobServiceetstarted_service.js**:
   
    ```
    'use strict';
   
    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```

5. Добавьте следующие объявления переменных и замените значения заполнителей:
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var queryCondition = "deviceId IN ['myDeviceId']";
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  300;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```

6. Добавьте следующую функцию, которая используется, чтобы отслеживать выполнение задания:
   
    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```

7. Добавьте следующий код, чтобы запланировать задание, которое вызывает метод устройства:
   
    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        responseTimeoutInSeconds: 15 // Timeout after 15 seconds if device is unable to process method
    };
   
    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                queryCondition,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```

8. Добавьте следующий код, чтобы запланировать задание, которое обновляет двойник устройства:
   
    ```
    var twinPatch = {
       etag: '*',
       properties: {
           desired: {
               building: '43',
               floor: 3
           }
       }
    };
   
    var twinJobId = uuid.v4();
   
    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                queryCondition,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```

9. Сохраните и закройте файл **scheduleJobService.js**.

## <a name="run-the-applications"></a>Запуск приложений

Теперь все готово к запуску приложений.

1. В командной строке в папке **simDevice** выполните следующую команду, чтобы начать прослушивание прямого метода перезагрузки:
   
    ```
    node simDevice.js
    ```

2. В командной строке в папке **scheduleJobService** выполните следующую команду, чтобы активировать задачи для блокировки дверей и обновления двойника.
   
    ```
    node scheduleJobService.js
    ```

3. В консоли отобразится ответ устройства на прямой метод.

## <a name="next-steps"></a>Следующие шаги

В этом учебнике описано использование задания для планирования прямого метода на устройстве и обновления свойств двойника устройства.

Чтобы продолжить знакомство с центром Интернета вещей и шаблонами управления устройствами, такими как удаленный через обновление встроенного [по Air, см. раздел учебник. Как обновить](tutorial-firmware-update.md)встроенное по.

Чтобы продолжить знакомство с центром Интернета вещей, см. статью Приступая к [работе с Azure IOT Edge](../iot-edge/tutorial-simulate-device-linux.md).
