---
title: Изменение и повторное развертывание микрослужбы в Azure | Документация Майкрософт
description: Это руководство содержит сведения об изменении и повторном развертывании микрослужбы в решении удаленного мониторинга.
author: dominicbetts
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 04/19/2018
ms.topic: conceptual
ms.openlocfilehash: 1552c54afe2195d58a032e9cc7bfa5aa70c844b1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "61447647"
---
# <a name="customize-and-redeploy-a-microservice"></a>Настройка и повторное развертывание микрослужбы

В этом руководстве показано, как изменить одну из [микрослужб](https://azure.com/microservices) в решении для удаленного мониторинга, создать образ микрослужбы, развернуть образ в концентратор DOCKER и использовать его в решении для удаленного мониторинга. Для предоставления этой концепции в руководстве используется базовый сценарий, в котором вы вызываете API микрослужбы и изменяете сообщение о состоянии с "Alive and Well" (Активно и работоспособно) на "New Edits Made Here!" (Сюда внесены новые изменения!).

Решение для удаленного мониторинга использует микрослужбы, созданные с помощью образов Docker, которые извлечены из центра Docker. 

В этом руководстве описано следующее:

>[!div class="checklist"]
> * Изменение и создание микрослужбы в решении для удаленного мониторинга.
> * Создание образа Docker.
> * Отправка образа Docker в центр Docker.
> * Извлечение нового образа Docker.
> * Визуализация изменений. 

## <a name="prerequisites"></a>Предварительные требования

Чтобы выполнить это руководство, требуется:

>[!div class="checklist"]
> * [Локальное развертывание акселератора решения для удаленного мониторинга](iot-accelerators-remote-monitoring-deploy-local.md)
> * [Учетная запись Docker](https://hub.docker.com/).
> * [Postman](https://www.getpostman.com/) — необходимо, чтобы просматривать ответ API.

## <a name="call-the-api-and-view-response-status"></a>Вызов API и просмотр состояния ответа

На этом этапе вы вызовите стандартный API микрослужбы диспетчера Центра Интернета вещей. API вернет сообщение о состоянии, которое позже можно будет изменить, настроив микрослужбу.

1. Убедитесь, что решение для удаленного мониторинга запущено локально на вашем компьютере.
2. Откройте Postman из расположения, в которое вы его скачали.
3. В Postman введите следующее значение в поле GET (Получить): `http://localhost:8080/iothubmanager/v1/status`.
4. Просмотрите возвращенное значение. Должен отобразиться следующий результат — "Status": "OK:Alive and Well".

    ![Сообщение Alive and Well (Активно и работоспособно) в приложении Postman](./media/iot-accelerators-microservices-example/postman-alive-well.png)

## <a name="change-the-status-and-build-the-image"></a>Изменение состояния и создание образа

Теперь измените сообщение о состоянии микрослужбы диспетчера Центра Интернета вещей на "New Edits Made Here!", а затем повторно создайте образ Docker с этим новым состоянием. Если у вас возникнут здесь проблемы, ознакомьтесь с разделом [об устранении неполадок](#Troubleshoot).

1. Убедитесь, что терминал открыт и перейдите в каталог, в который вы клонировали решение для удаленного мониторинга. 
1. Перейдите в каталог azure-iot-pcs-remote-monitoring-dotnet/services/iothub-manager/Services.
1. Откройте файл StatusService.cs в любом текстовом редакторе или в интегрированной среде разработки. 
1. Найдите следующий код:

    ```csharp
    var result = new StatusServiceModel(true, "Alive and well!");
    ```

    Замените его приведенным ниже кодом и сохраните.

    ```csharp
    var result = new StatusServiceModel(true, "New Edits Made Here!");
    ```

5. Вернитесь в окно терминала, но теперь перейдите в этот каталог: azure-iot-pcs-remote-monitoring-dotnet/services/iothub-manager/scripts/docker.
6. Чтобы создать образ Docker, введите следующее:

    ```sh
    sh build
    ```
    
    или в Windows:
    
    ```cmd
    ./build.cmd
    ```

7. Чтобы убедиться, что образ успешно создан, введите следующее:

    ```cmd/sh
    docker images 
    ```

Должен быть указан этот репозиторий: azureiotpcs/iothub-manager-dotnet.

![Успешно созданный образ Docker](./media/iot-accelerators-microservices-example/successful-docker-image.png)

## <a name="tag-and-push-the-image"></a>Пометка и отправка образов
Прежде чем можно будет отправить новый образ Docker в центр Docker, необходимо пометить образы. Если у вас возникнут здесь проблемы, ознакомьтесь с разделом [об устранении неполадок](#Troubleshoot).

1. Найдите идентификатор созданного образа Docker. Для этого введите следующее:

    ```cmd/sh
    docker images
    ```

2. Чтобы пометить образ как тестируемый, введите следующее:

    ```cmd/sh
    docker tag [Image ID] [docker ID]/iothub-manager-dotnet:testing 
    ```

3. Чтобы отправить помеченный образ в центр Docker, введите следующее:

    ```cmd/sh
    docker push [docker ID]/iothub-manager-dotnet:testing
    ```

4. Откройте веб-браузер, перейдите в [центр Docker](https://hub.docker.com/) и выполните вход.
5. Теперь в центре Docker должен отобразиться только что отправленный образ Docker.
![Образ Docker в центре Docker](./media/iot-accelerators-microservices-example/docker-image-in-docker-hub.png)

## <a name="update-your-remote-monitoring-solution"></a>Обновление решения для удаленного мониторинга
Теперь нужно обновить локальный файл docker-compose.yml, чтобы извлечь новый образ Docker из центра Docker. Если у вас возникнут здесь проблемы, ознакомьтесь с разделом [об устранении неполадок](#Troubleshoot).

1. Вернитесь в окно терминала и перейдите в этот каталог: azure-iot-pcs-remote-monitoring-dotnet/services/scripts/local.
2. Откройте файл docker-compose.yml в любом текстовом редакторе или в интегрированной среде разработки.
3. Найдите следующий код:

    ```yml
    image: azureiotpcs/iothub-manager-dotnet:testing
    ```

    Измените его на следующий код и сохраните.

    ```yml
    image: [docker ID]/iothub-manager-dotnet:testing
    ```

## <a name="view-the-new-response-status"></a>Просмотр нового состояния ответа
В завершении повторно разверните локальный экземпляр решения для удаленного мониторинга и просмотрите новое состояние ответа в Postman.

1. Вернитесь в окно терминала и перейдите в этот каталог: azure-iot-pcs-remote-monitoring-dotnet/scripts/local.
2. Запустите локальный экземпляр решения для удаленного мониторинга. Для этого в терминале введите следующую команду:

    ```cmd/sh
    docker-compose up
    ```

3. Откройте Postman из расположения, в которое вы его скачали.
4. В Postman введите следующий запрос в поле GET (Получить): `http://localhost:8080/iothubmanager/v1/status`. Должен отобразиться следующий результат — "Status": "OK: New Edits Made Here!".

![Сообщение "New Edits Made Here" в приложении Postman](./media/iot-accelerators-microservices-example/new-postman-message.png)

## <a name="troubleshoot"></a><a name="Troubleshoot"></a>Устранение неполадок

Если у вас возникли проблемы, попробуйте удалить образы и контейнеры Docker на локальном компьютере.

1. Чтобы удалить все контейнеры, сначала необходимо остановить все запущенные. Откройте терминал и введите следующее:

    ```cmd/sh
    docker stop $(docker ps -aq)
    docker rm $(docker ps -aq)
    ```
    
2. Чтобы удалить все образы, откройте терминал и введите следующее: 

    ```cmd/sh
    docker rmi $(docker images -q)
    ```

3. Чтобы проверить, остались ли на компьютере контейнеры, введите следующее:

    ```cmd/sh
    docker ps -aq 
    ```

    Если все контейнеры успешно удалены, результатов быть не должно.

4. Чтобы проверить, остались ли на компьютере образы, введите следующее:

    ```cmd/sh
    docker images
    ```

    Если все контейнеры успешно удалены, результатов быть не должно.

## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как выполнять следующие задачи:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Изменение и создание микрослужбы в решении для удаленного мониторинга.
> * Создание образа Docker.
> * Отправка образа Docker в центр Docker.
> * Извлечение нового образа Docker.
> * Визуализация изменений. 

Далее можно попытаться [настроить микрослужбу моделирования устройств в решении для удаленного мониторинга](iot-accelerators-microservices-example.md).

Дополнительные сведения для разработчиков о решении для удаленного мониторинга см. в следующих источниках:

* [Справочник разработчика](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
<!-- Next tutorials in the sequence -->

