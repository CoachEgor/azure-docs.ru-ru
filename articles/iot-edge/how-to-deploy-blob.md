---
title: Развертывание хранилища BLOB-объектов в модуле на устройстве. Azure IoT Edge
description: Разверните модуль хранилища BLOB-объектов Azure на вашем устройстве IoT Edge для хранения данных на границе.
author: kgremban
ms.author: kgremban
ms.date: 12/13/2019
ms.topic: conceptual
ms.service: iot-edge
ms.reviewer: arduppal
ms.openlocfilehash: 8c2df4854f4cdb93c08e22f7dcdc23b1b69b13d6
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2020
ms.locfileid: "76548787"
---
# <a name="deploy-the-azure-blob-storage-on-iot-edge-module-to-your-device"></a>Развертывание хранилища BLOB-объектов Azure в модуле IoT Edge на устройстве

Существует несколько способов развертывания модулей на IoT Edge устройстве, и все они работают для хранилища BLOB-объектов Azure в модулях IoT Edge. Два простейших метода — это портал Azure или шаблоны Visual Studio Code.

## <a name="prerequisites"></a>Технические условия

- [Центр Интернета вещей](../iot-hub/iot-hub-create-through-portal.md) в подписке Azure.
- [Устройство IoT Edge](how-to-register-device.md) с установленной средой выполнения IoT Edge.
- [Visual Studio Code](https://code.visualstudio.com/) и [средства Azure IOT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) при развертывании из Visual Studio Code.

## <a name="deploy-from-the-azure-portal"></a>Развертывание из портал Azure

В портал Azure описывается создание манифеста развертывания и принудительная отправка развертывания на устройство IoT Edge.

### <a name="select-your-device"></a>Выбор устройства

1. Войдите на [портал Azure](https://portal.azure.com) и перейдите к своему Центру Интернета вещей.
1. В меню выберите **IoT Edge**.
1. Щелкните идентификатор целевого устройства в списке устройств.
1. Щелкните **Set Modules** (Настроить модули).

### <a name="configure-a-deployment-manifest"></a>Настройка манифеста развертывания

Манифест развертывания — это документ JSON, в котором определены развертываемые модули, способ передачи данных между этими модулями и требуемые свойства для двойников модулей. В портал Azure есть мастер, который поможет вам создать манифест развертывания. Он состоит из трех шагов, упорядоченных по вкладкам: **модули**, **маршруты**и **Проверка + создать**.

#### <a name="add-modules"></a>Добавление модулей

1. В разделе **IOT Edge модули** страницы щелкните раскрывающийся список **Добавить** и выберите пункт **IOT Edge** , чтобы открыть страницу **Добавление модуля IOT Edge** .

2. На вкладке **Параметры модуля** укажите имя модуля, а затем укажите URI образа контейнера:

   Примеры.
  
   - **Имя модуля IOT Edge**: `azureblobstorageoniotedge`
   - **URI изображения**: `mcr.microsoft.com/azure-blob-storage:latest`

   ![Параметры двойника модуля](./media/how-to-deploy-blob/addmodule-tab1.png)

   Не выбирайте **Добавить** , пока вы не зауказали значения на вкладках **Параметры модуля**, **Параметры создания контейнера**и **Параметры модуля двойника** , как описано в этой процедуре.

   > [!IMPORTANT]
   > Azure IoT Edge учитывает регистр при вызове модулей, а пакет SDK хранилища также по умолчанию имеет нижний регистр. Хотя имя модуля в [Azure Marketplace](how-to-deploy-modules-portal.md#deploy-modules-from-azure-marketplace) — **азуреблобсторажеониотедже**, изменение имени на строчное помогает гарантировать, что подключения к хранилищу BLOB-объектов Azure в модуле IOT EDGE не прерываются.

3. Откройте вкладку **Параметры создания контейнера** .

   ![Параметры двойника модуля](./media/how-to-deploy-blob/addmodule-tab3.png)

   Скопируйте и вставьте следующий код JSON в поле, чтобы предоставить сведения об учетной записи хранения и подключение к хранилищу на устройстве.
  
   ```json
   {
     "Env":[
       "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
       "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
     ],
     "HostConfig":{
       "Binds":[
           "<storage mount>"
       ],
       "PortBindings":{
         "11002/tcp":[{"HostPort":"11002"}]
       }
     }
   }
   ```

4. Обновите JSON, скопированный в **Параметры создания контейнера** , со следующими сведениями:

   - Замените `<your storage account name>` запоминаемым именем. Длина имени учетной записи должна составлять от 3 до 24 символов, а строчные буквы и цифры. Нельзя использовать пробелы.

   - Замените `<your storage account key>` на 64-байтовый ключ Base64. Вы можете создать ключ с помощью таких средств, как [GeneratePlus](https://generate.plus/en/base64). Вы будете использовать эти учетные данные для доступа к хранилищу BLOB-объектов из других модулей.

   - Замените `<storage mount>` в соответствии с операционной системой контейнера. Укажите имя [тома](https://docs.docker.com/storage/volumes/) или абсолютный путь к существующему каталогу на устройстве IOT EDGE, где модуль BLOB-объектов будет хранить свои данные. Подключение к хранилищу сопоставляет расположение на вашем устройстве, которое вы задаете в заданном расположении в модуле.

     - Для контейнеров Linux используется формат *\<пути к хранилищу или > тома:/блобрут*. Например.
         - использование [подключения тома](https://docs.docker.com/storage/volumes/): **My-Volume:/блобрут**
         - Используйте [Подключение BIND](https://docs.docker.com/storage/bind-mounts/): **/СРВ/контаинердата:/блобрут**. Обязательно выполните действия по [предоставлению доступа к каталогу пользователю контейнера](how-to-store-data-blob.md#granting-directory-access-to-container-user-on-linux) .
     - Для контейнеров Windows используется формат *\<путь к хранилищу или > томов: C:/блобрут*. Например.
         - Используйте [Подключение тома](https://docs.docker.com/storage/volumes/): **My-Volume: C:/блобрут**.
         - Используйте [BIND Mount](https://docs.docker.com/storage/bind-mounts/): **c:/контаинердата: c:/блобрут**.
         - Вместо использования локального диска можно назначить сетевое расположение SMB. Дополнительные сведения см. в разделе [использование общего ресурса SMB в качестве локального хранилища](how-to-store-data-blob.md#using-smb-share-as-your-local-storage) .

     > [!IMPORTANT]
     > Не изменяйте вторую половину значения подключения к хранилищу, которое указывает на определенное место в модуле. Подключение к хранилищу всегда должно заканчиваться на **:/блобрут** для контейнеров Linux и **: C:/Блобрут** для контейнеров Windows.

5. На вкладке **двойника Settings (параметры модуля** ) Скопируйте следующий код JSON и вставьте его в поле.

   ![Параметры двойника модуля](./media/how-to-deploy-blob/addmodule-tab4.png)

   Настройте каждое свойство с соответствующим значением, как указано заполнителями. Если вы используете симулятор IoT Edge, присвойте значения соответствующим переменным среды для этих свойств, как описано в [девицетоклаудуплоадпропертиес](how-to-store-data-blob.md#devicetoclouduploadproperties) и [девицеаутоделетепропертиес](how-to-store-data-blob.md#deviceautodeleteproperties).

   ```json
   {
     "deviceAutoDeleteProperties": {
       "deleteOn": <true, false>,
       "deleteAfterMinutes": <timeToLiveInMinutes>,
       "retainWhileUploading": <true,false>
     },
     "deviceToCloudUploadProperties": {
       "uploadOn": <true, false>,
       "uploadOrder": "<NewestFirst, OldestFirst>",
       "cloudStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>; EndpointSuffix=<your end point suffix>",
       "storageContainersForUpload": {
         "<source container name1>": {
           "target": "<target container name1>"
         }
       },
       "deleteAfterUpload": <true,false>
     }
   }
   ```

   Сведения о настройке Девицетоклаудуплоадпропертиес и Девицеаутоделетепропертиес после развертывания модуля см. [в разделе изменение модуля двойника](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin). Дополнительные сведения о требуемых свойствах см. в разделе [Определение или обновление требуемых свойств](module-composition.md#define-or-update-desired-properties).

6. Выберите **Добавить**.

7. Нажмите кнопку **Далее: Маршруты** , чтобы перейти к разделу маршруты.

#### <a name="specify-routes"></a>Настройка маршрутов

Сохранить маршруты по умолчанию и нажать кнопку **Далее: Проверка + создать** , чтобы перейти к разделу "Проверка".

#### <a name="review-deployment"></a>Проверка развертывания

В разделе обзорной информации вы увидите манифест развертывания в формате JSON, который был создан на основе параметров, выбранных вами в двух предыдущих разделах. Также есть два объявленных модуля, которые не были добавлены: **$edgeAgent** и **$edgeHub**. Эти два модуля составляют [среду выполнения IoT Edge](iot-edge-runtime.md) и являются обязательными для каждого развертывания.

Просмотрите сведения о развертывании и нажмите кнопку **создать**.

### <a name="verify-your-deployment"></a>Проверка развертывания

После создания развертывания вы вернетесь на страницу **IOT Edge** центра Интернета вещей.

1. Выберите устройство IoT Edge, на которое отправили развертывание, чтобы открыть сведения о нем.
1. В сведениях об устройстве убедитесь, что для модуля службы хранилища BLOB-объектов настроены параметры **Указано в развертывании** и **Данные, полученные от устройства**.

Запуск модуля на устройстве может занять несколько секунд. Затем информация о нем передается в Центр Интернета вещей. Обновите страницу, чтобы увидеть обновленное состояние.

## <a name="deploy-from-visual-studio-code"></a>Развертывание из Visual Studio Code

Azure IoT Edge предоставляет шаблоны в Visual Studio Code для разработки решений для границы. Выполните следующие действия, чтобы создать новое IoT Edge решение с модулем хранилища BLOB-объектов и настроить манифест развертывания.

1. Выберите элемент **Вид** > **Палитра команд**.

1. В палитре команд введите и выполните команду **Azure IoT Edge: New IoT Edge solution**.

   ![Запуск команды создания решения IoT Edge](./media/how-to-develop-csharp-module/new-solution.png)

   Следуйте инструкциям на экране в палитре команд для создания решения.

   | Поле | Значение |
   | ----- | ----- |
   | Выбор папки | Выберите расположение на компьютере разработки, чтобы Visual Studio Code создать файлы решения. |
   | Введите название решения. | Введите описательное имя решения или примите имя по умолчанию **EdgeSolution**. |
   | Выбор шаблона модуля | Выберите **Existing Module (Enter full image URL)** (Имеющийся модуль (введите полный URL-адрес образа)). |
   | Указание имени модуля | Введите строчное имя для модуля, например **азуреблобсторажеониотедже**.<br/><br/>В модуле IoT Edge очень важно использовать знаки нижнего регистра для имени хранилища BLOB-объектов Azure. IoT Edge учитывает регистр при ссылке на модули, а название пакета SDK службы хранилища по умолчанию преобразовывается в нижний регистр. |
   | Указание образа Docker для модуля | Укажите универсальный код ресурса (URI) образа: **mcr.microsoft.com/azure-blob-storage:latest**. |

   Visual Studio Code принимает предоставленные сведения, создает решение IoT Edge, а затем загружает его в новом окне. Шаблон решения создает шаблон манифеста развертывания, который содержит образ модуля хранилища BLOB-объектов, но вам нужно настроить параметры создания модуля.

1. Откройте *deployment.template.json* в рабочей области нового решения и найдите раздел **modules**. Внесите следующие изменения конфигурации.

   1. Удалите модуль **симулатедтемпературесенсор** , так как он не требуется для этого развертывания.

   1. Скопируйте и вставьте следующий код в поле `createOptions`:

      ```json
      "Env":[
       "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
       "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
      ],
      "HostConfig":{
        "Binds": ["<storage mount>"],
        "PortBindings":{
          "11002/tcp": [{"HostPort":"11002"}]
        }
      }
      ```

      ![Креатеоптионс модуля обновления — Visual Studio Code](./media/how-to-deploy-blob/create-options.png)

1. Замените `<your storage account name>` запоминаемым именем. Длина имени учетной записи должна составлять от 3 до 24 символов, а строчные буквы и цифры. Нельзя использовать пробелы.

1. Замените `<your storage account key>` на 64-байтовый ключ Base64. Вы можете создать ключ с помощью таких средств, как [GeneratePlus](https://generate.plus/en/base64). Вы будете использовать эти учетные данные для доступа к хранилищу BLOB-объектов из других модулей.

1. Замените `<storage mount>` в соответствии с операционной системой контейнера. Укажите имя [тома](https://docs.docker.com/storage/volumes/) или абсолютный путь к каталогу на устройстве IoT Edge, где модуль BLOB-объектов должен хранить данные. Подключение к хранилищу сопоставляет расположение на вашем устройстве, которое вы задаете в заданном расположении в модуле.  

     - Для контейнеров Linux используется формат *\<пути к хранилищу или > тома:/блобрут*. Например.
         - использование [подключения тома](https://docs.docker.com/storage/volumes/): **My-Volume:/блобрут**
         - Используйте [Подключение BIND](https://docs.docker.com/storage/bind-mounts/): **/СРВ/контаинердата:/блобрут**. Обязательно выполните действия по [предоставлению доступа к каталогу пользователю контейнера](how-to-store-data-blob.md#granting-directory-access-to-container-user-on-linux) .
     - Для контейнеров Windows используется формат *\<путь к хранилищу или > томов: C:/блобрут*. Например.
         - Используйте [Подключение тома](https://docs.docker.com/storage/volumes/): **My-Volume: C:/блобрут**.
         - Используйте [BIND Mount](https://docs.docker.com/storage/bind-mounts/): **c:/контаинердата: c:/блобрут**.
         - Вместо использования локального диска можно назначить сетевое расположение SMB. Дополнительные сведения см. в разделе [использование общего ресурса SMB в качестве локального хранилища](how-to-store-data-blob.md#using-smb-share-as-your-local-storage) .

     > [!IMPORTANT]
     > Не изменяйте вторую половину значения подключения к хранилищу, которое указывает на определенное место в модуле. Подключение к хранилищу всегда должно заканчиваться на **:/блобрут** для контейнеров Linux и **: C:/Блобрут** для контейнеров Windows.

1. Настройте [девицетоклаудуплоадпропертиес](how-to-store-data-blob.md#devicetoclouduploadproperties) и [девицеаутоделетепропертиес](how-to-store-data-blob.md#deviceautodeleteproperties) для модуля, добавив следующий код JSON в файл *Deployment. Template. JSON* . Настройте каждое свойство с соответствующим значением и сохраните файл. Если вы используете симулятор IoT Edge, задайте значения для соответствующих переменных среды для этих свойств, которые можно найти в разделе пояснения в [девицетоклаудуплоадпропертиес](how-to-store-data-blob.md#devicetoclouduploadproperties) и [девицеаутоделетепропертиес](how-to-store-data-blob.md#deviceautodeleteproperties) .

   ```json
   "<your azureblobstorageoniotedge module name>":{
     "properties.desired": {
       "deviceAutoDeleteProperties": {
         "deleteOn": <true, false>,
         "deleteAfterMinutes": <timeToLiveInMinutes>,
         "retainWhileUploading": <true, false>
       },
       "deviceToCloudUploadProperties": {
         "uploadOn": <true, false>,
         "uploadOrder": "<NewestFirst, OldestFirst>",
         "cloudStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>",
         "storageContainersForUpload": {
           "<source container name1>": {
             "target": "<target container name1>"
           }
         },
         "deleteAfterUpload": <true, false>
       }
     }
   }
   ```

   ![Настройка требуемых свойств для азуреблобсторажеониотедже-Visual Studio Code](./media/how-to-deploy-blob/devicetocloud-deviceautodelete.png)

   Сведения о настройке Девицетоклаудуплоадпропертиес и Девицеаутоделетепропертиес после развертывания модуля см. [в разделе изменение модуля двойника](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin). Дополнительные сведения о параметрах создания контейнера, перезапуске политики и требуемом состоянии см. в разделе [EdgeAgent требуемые свойства](module-edgeagent-edgehub.md#edgeagent-desired-properties).

1. Сохраните файл *deployment.template.json*.

1. Щелкните правой кнопкой мыши **deployment.template.json** и выберите **Создать манифест развертывания IoT Edge**.

1. Visual Studio Code принимает сведения, указанные в файле *Deployment. Template. JSON* , и использует его для создания нового файла манифеста развертывания. Манифест развертывания создается в новой папке **config** в рабочей области решения. Получив этот файл, выполните действия, описанные в разделе [Развертывание модулей Azure IoT Edge из Visual Studio Code](how-to-deploy-modules-vscode.md) или [Развертывание модулей IoT Edge Azure с помощью Azure CLI 2.0](how-to-deploy-modules-cli.md).

## <a name="deploy-multiple-module-instances"></a>Развертывание нескольких экземпляров модулей

Если вы хотите развернуть несколько экземпляров хранилища BLOB-объектов Azure в модуле IoT Edge, необходимо указать другой путь к хранилищу и изменить значение `HostPort`, к которому привязан модуль. Модули службы хранилища BLOB-объектов всегда предоставляют порт 11002 в контейнере, но вы можете объявить, к какому порту он привязан в узле.

Измените **Параметры создания контейнера** (в портал Azure) или поле **креатеоптионс** (в файле *Deployment. Template. JSON* в Visual Studio Code), чтобы изменить значение `HostPort`:

```json
"PortBindings":{
  "11002/tcp": [{"HostPort":"<port number>"}]
}
```

При подключении к дополнительным модулям хранилища BLOB-объектов измените конечную точку, чтобы она указывала на обновленный порт узла.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о [хранилище BLOB-объектов Azure см. на IOT Edge](how-to-store-data-blob.md)

Дополнительные сведения о работе манифестов развертывания и об их создании см. в руководстве по [использованию, настройке и повторном использовании модулей Azure IoT Edge](module-composition.md).
