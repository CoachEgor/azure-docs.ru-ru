---
title: Отправка файлов с устройств в Центр Интернета вещей с помощью Node.js | Документация Майкрософт
description: Сведения об отправке файлов с устройства в облако с помощью пакета SDK для устройств Azure IoT для Node.js. Отправленные файлы хранятся в контейнере больших двоичных объектов службы хранилища Azure.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 06/28/2017
ms.openlocfilehash: db3da5ff2d7e8b6fa493f5338fac93df0d1a7fe2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "77110899"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-nodejs"></a>Загрузка файлов с устройства в облако с помощью IoT Hub (Node.js)

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Этот учебник основан на коде в [сообщении облаке к устройству с](iot-hub-node-node-c2d.md) помощью учебника IoT Hub, чтобы показать, как использовать [возможности загрузки файлов IoT Hub](iot-hub-devguide-file-upload.md) для загрузки файла в [хранилище Azure blob.](../storage/index.yml) В этом руководстве описаны следующие процедуры.

* как безопасно предоставить устройству универсальный код ресурса (URI) большого двоичного объекта Azure для передачи файла;

* как использовать уведомления о передаче файлов Центра Интернета вещей для активации обработки файла в серверной части приложения.

[Телеметрия «Отправить» с устройства на быстрый запуск концентратора IoT](quickstart-send-telemetry-node.md) демонстрирует основную функциональность обмена сообщениями от устройства к облаку IoT Hub. Тем не менее в некоторых случаях не просто сопоставить данные, отправляемые устройством, с относительно небольшими сообщениями, отправляемыми из устройства в облако, которые принимает Центр Интернета вещей. Пример:

* Большие файлы, содержащие изображения.
* Видео
* Данные вибрации с высокой частотой выборки.
* Некоторая форма предварительно обработанных данных.

Эти файлы обычно обрабатываются в виде пакета в облаке с помощью таких инструментов, как [фабрика данных Azure](../data-factory/introduction.md) или стек [Hadoop](../hdinsight/index.yml). При передаче файлов с устройства вы можете рассчитывать на безопасность и надежность Центра Интернета вещей.

По завершении работы с этим руководством вы запустите два консольных приложения Node.js:

* **SimulatedDevice.js** — передает файл в хранилище с помощью универсального кода ресурса (URI) SAS, предоставленного вашим Центром Интернета вещей.

* **ReadFileUploadNotification.js** — получает уведомления о передаче файлов от Центра Интернета вещей.

> [!NOTE]
> Существуют пакеты SDK для устройств Azure IoT, обеспечивающие поддержку многих платформ устройств и языков (включая C, .NET, JavaScript, Python и Java) в Центре Интернета вещей. Для пошаговой инструкции о том, как подключить устройство к концентратору Azure IoT, обратитесь в Центр разработчиков IoT Azure.

## <a name="prerequisites"></a>Предварительные требования

* Версия node.js 10.0.x или позже. В статье [Prepare your development environment](https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md) (Подготовка среды разработки) описывается, как установить Node.js для работы с этим учебником в ОС Windows или Linux.

* Активная учетная запись Azure. Если ее нет, можно создать [бесплатную учетную запись](https://azure.microsoft.com/pricing/free-trial/) всего за несколько минут.

* Убедитесь, что в брандмауэре открыт порт 8883. Образец устройства в этой статье использует протокол МЗТТ, который общается по порту 8883. В некоторых корпоративных и академических сетях этот порт может быть заблокирован. Дополнительные сведения и способы устранения этой проблемы см. в разделе о [подключении к Центру Интернета вещей по протоколу MQTT](iot-hub-mqtt-support.md#connecting-to-iot-hub).

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Передача файла из приложения устройства

В этом разделе вы создадите приложение для устройства, чтобы отправлять файлы в Центр Интернета вещей.

1. Создайте пустую папку с именем ```simulateddevice```.  В папке ```simulateddevice``` создайте файл package.json, указав приведенную ниже команду в командной строке.  Примите значения по умолчанию:

    ```cmd/sh
    npm init
    ```

2. В командной строке в папке ```simulateddevice``` выполните следующую команду, чтобы установить пакет SDK для устройств **azure-iot-device** и пакет **azure-iot-device-mqtt**.

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

3. В текстовом редакторе создайте файл **SimulatedDevice.js** в папке ```simulateddevice```.

4. Добавьте следующие инструкции ```require``` в начало файла **SimulatedDevice.js** :

    ```javascript
    'use strict';

    var fs = require('fs');
    var mqtt = require('azure-iot-device-mqtt').Mqtt;
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    ```

5. Добавьте переменную `deviceconnectionstring`, чтобы создать с ее помощью экземпляр **клиента**.  Замените `{deviceconnectionstring}` именем устройства, созданного в разделе о *создании центра Интернета вещей*.

    ```javascript
    var connectionString = '{deviceconnectionstring}';
    var filename = 'myimage.png';
    ```

    > [!NOTE]
    > Для простоты строка подключения добавляется в код, хотя мы не рекомендуем так делать. В зависимости от сценария использования и архитектуры могут потребоваться более безопасные способы хранения этого секрета.

6. Чтобы подключить клиент, добавьте следующий код:

    ```javascript
    var client = clientFromConnectionString(connectionString);
    console.log('Client connected');
    ```

7. Создайте обратный вызов и отправьте файл с помощью функции **uploadToBlob**.

    ```javascript
    fs.stat(filename, function (err, stats) {
        const rr = fs.createReadStream(filename);

        client.uploadToBlob(filename, rr, stats.size, function (err) {
            if (err) {
                console.error('Error uploading file: ' + err.toString());
            } else {
                console.log('File uploaded');
            }
        });
    });
    ```

8. Сохраните и закройте файл **SimulatedDevice.js** .

9. Скопируйте файл образа в папку `simulateddevice` и переименуйте его на `myimage.png`.

## <a name="get-the-iot-hub-connection-string"></a>Получите строку соединения концентратора IoT

В этой статье вы создаете бэкэнд-сервис для получения сообщений уведомлений о загрузке файлов из созданного в центре IoT [концентратора Отправки с устройства на концентратор IoT.](quickstart-send-telemetry-node.md) Для получения сообщений уведомлений о загрузке файлов службе требуется разрешение **на подключение службы.** По умолчанию каждый концентратор IoT создается с помощью службы общего доступа, названной **службой,** которая предоставляет это разрешение.

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="receive-a-file-upload-notification"></a>Получение уведомления о передачи файла

В этом разделе вам предстоит создать консольное приложение Node.js, которое получает уведомления об отправке файлов из Центра Интернета вещей.

Чтобы выполнить инструкции в этом разделе, вы можете использовать строку подключения **iothubowner** из Центра Интернета вещей. Строку подключения можно найти на [портале Azure](https://portal.azure.com/) в колонке **Политика общего доступа**.

1. Создайте пустую папку с именем ```fileuploadnotification```.  В папке ```fileuploadnotification``` создайте файл package.json, указав приведенную ниже команду в командной строке.  Примите значения по умолчанию:

    ```cmd/sh
    npm init
    ```

2. В командной строке в папке ```fileuploadnotification``` выполните следующую команду, чтобы установить пакет SDK **azure-iothub**:

    ```cmd/sh
    npm install azure-iothub --save
    ```

3. С помощью текстового редактора создайте файл **FileUploadNotification.js** в папке `fileuploadnotification`.

4. Добавьте следующие инструкции `require` в начало файла **FileUploadNotification.js**:

    ```javascript
    'use strict';

    var Client = require('azure-iothub').Client;
    ```

5. Добавьте переменную `iothubconnectionstring`, чтобы создать с ее помощью экземпляр **клиента**.  Замените `{iothubconnectionstring}` значение заполнителя строкой подключения концентратора IoT, которую вы скопировали ранее в [строке подключения концентратора IoT:](#get-the-iot-hub-connection-string)

    ```javascript
    var connectionString = '{iothubconnectionstring}';
    ```

    > [!NOTE]
    > Для простоты строка подключения добавляется в код, хотя мы не рекомендуем так делать. В зависимости от сценария использования и архитектуры могут потребоваться более безопасные способы хранения этого секрета.

6. Чтобы подключить клиент, добавьте следующий код:

    ```javascript
    var serviceClient = Client.fromConnectionString(connectionString);
    ```

7. Откройте клиент и воспользуйтесь функцией **getFileNotificationReceiver**, чтобы получить обновления состояния.

    ```javascript
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFileNotificationReceiver(function receiveFileUploadNotification(err, receiver){
          if (err) {
            console.error('error getting the file notification receiver: ' + err.toString());
          } else {
            receiver.on('message', function (msg) {
              console.log('File upload from device:')
              console.log(msg.getData().toString('utf-8'));
            });
          }
        });
      }
    });
    ```

8. Сохраните и закройте файл **FileUploadNotification.js**.

## <a name="run-the-applications"></a>Запуск приложений

Теперь все готово к запуску приложений.

В командной строке в папке `fileuploadnotification` выполните следующую команду:

```cmd/sh
node FileUploadNotification.js
```

В командной строке в папке `simulateddevice` выполните следующую команду:

```cmd/sh
node SimulatedDevice.js
```

На следующем скриншоте показан выход из приложения **SimulatedDevice:**

![Выходные данные приложения simulated-device](./media/iot-hub-node-node-file-upload/simulated-device.png)

На следующем снимке экрана показаны выходные данные приложения **FileUploadNotification**:

![Выходные данные приложения read-file-upload-notification](./media/iot-hub-node-node-file-upload/read-file-upload-notification.png)

На портале можно просмотреть отправленный файл в контейнере хранилища, который вы настроили.

![Отправленный файл](./media/iot-hub-node-node-file-upload/uploaded-file.png)

## <a name="next-steps"></a>Дальнейшие действия

В этом руководство показано, как использовать возможности передачи файлов Центра Интернета вещей, чтобы упростить передачу файлов из устройств. Изучение функций и сценариев Центра Интернета вещей можно продолжить в следующих руководствах:

* [Создание Центра Интернета вещей с помощью шаблона Azure Resource Manager (PowerShell)](iot-hub-rm-template-powershell.md)

* [Пакет SDK для устройств Azure IoT для C](iot-hub-device-sdk-c-intro.md)

* [IoT Hub SDKs](iot-hub-devguide-sdks.md) (Пакеты SDK для Центра Интернета вещей)
