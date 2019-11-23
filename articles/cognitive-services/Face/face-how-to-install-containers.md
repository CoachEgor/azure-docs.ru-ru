---
title: Install and run containers - FACE API
titleSuffix: Azure Cognitive Services
description: This article shows you how to download, install, and run containers for Face in this walkthrough tutorial.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: 574f6bead9cac384c72d2d0cd35353eb571a9490
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/22/2019
ms.locfileid: "74327044"
---
# <a name="install-and-run-face-containers-preview"></a>Install and run Face containers (Preview)

Azure Cognitive Services Face provides a standardized Linux container for Docker that detects human faces in images. It also identifies attributes, which include face landmarks such as noses and eyes, gender, age, and other machine-predicted facial features. In addition to detection, Face can check if two faces in the same image or different images are the same by using a confidence score. Face also can compare faces against a database to see if a similar-looking or identical face already exists. It also can organize similar faces into groups by using shared visual traits.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Технические условия

You must meet the following prerequisites before you use the Face API containers.

|Обязательно для заполнения|Цель|
|--|--|
|Модуль Docker| The Docker Engine must be installed on a [host computer](#the-host-computer). Docker предоставляет пакеты, которые настраивают среду Docker в ОС [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) и [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Ознакомьтесь с [общими сведениями о Docker и контейнерах](https://docs.docker.com/engine/docker-overview/).<br><br> Docker нужно настроить таким образом, чтобы контейнеры могли подключать и отправлять данные о выставлении счетов в Azure. <br><br> On Windows, Docker also must be configured to support Linux containers.<br><br>|
|Опыт работы с Docker | You need a basic understanding of Docker concepts, such as registries, repositories, containers, and container images. You also need knowledge of basic `docker` commands.| 
|Face resource |To use the container, you must have:<br><br>An Azure **Face** resource and the associated API key and the endpoint URI. Both values are available on the **Overview** and **Keys** pages for the resource. They're required to start the container.<br><br>**{API_KEY}** : One of the two available resource keys on the **Keys** page<br><br>**{ENDPOINT_URI}** : The endpoint as provided on the **Overview** page

[!INCLUDE [Gathering required container parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="request-access-to-the-private-container-registry"></a>Запрос доступа к частному реестру контейнеров

[!INCLUDE [Request access to private container registry](../../../includes/cognitive-services-containers-request-access.md)]

### <a name="the-host-computer"></a>Главный компьютер

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Требования к контейнеру и рекомендации

В следующей таблице описаны минимальное и рекомендуемое число ядра ЦП и памяти для каждого контейнера API распознавания лиц.

| Контейнер | Минимальная | Рекомендуется | Число транзакций в секунду<br>(Minimum, maximum)|
|-----------|---------|-------------|--|
|Распознавание лиц | 1 core, 2-GB memory | 1 core, 4-GB memory |10, 20|

* Each core must be at least 2.6 GHz or faster.
* Transactions per second (TPS).

Ядро и память соответствуют параметрам `--cpus` и `--memory`, которые используются как часть команды `docker run`.

## <a name="get-the-container-image-with-docker-pull"></a>Get the container image with docker pull

Container images for the Face API are available. 

| Контейнер | Репозиторий |
|-----------|------------|
| Распознавание лиц | `containerpreview.azurecr.io/microsoft/cognitive-services-face:latest` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-face-container"></a>docker pull для контейнера распознавания лиц

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-face:latest
```

## <a name="use-the-container"></a>Use the container

После размещения контейнера на [главном компьютере](#the-host-computer) выполните приведенные ниже инструкции для работы с ним.

1. [Run the container](#run-the-container-with-docker-run) with the required billing settings. Доступны дополнительные [примеры](./face-resource-container-config.md#example-docker-run-commands) команды `docker run`. 
1. [Запрос конечной точки прогнозирования контейнера](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Run the container with docker run

Воспользуйтесь командой [docker run](https://docs.docker.com/engine/reference/commandline/run/) для запуска контейнера. Refer to [gathering required parameters](#gathering-required-parameters) for details on how to get the `{ENDPOINT_URI}` and `{API_KEY}` values.

[Examples](face-resource-container-config.md#example-docker-run-commands) of the `docker run` command are available.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-face \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Эта команда:

* Runs a face container from the container image.
* Allocates one CPU core and 4 GB of memory.
* Exposes TCP port 5000 and allocates a pseudo TTY for the container.
* автоматически удаляет контейнер после завершения его работы. Образ контейнера остается доступным на главном компьютере. 

Доступны дополнительные [примеры](./face-resource-container-config.md#example-docker-run-commands) команды `docker run`. 

> [!IMPORTANT]
> The `Eula`, `Billing`, and `ApiKey` options must be specified to run the container or the container won't start. Дополнительные сведения см. в [разделе о выставлении счетов](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]


## <a name="query-the-containers-prediction-endpoint"></a>Запрос конечной точки прогнозирования контейнера

Контейнер предоставляет API запроса конечной точки прогнозирования на основе REST. 

Используйте узел `http://localhost:5000` для API контейнера.


<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Остановка контейнера

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Устранение неисправностей

If you run the container with an output [mount](./face-resource-container-config.md#mount-settings) and logging is enabled, the container generates log files that are helpful to troubleshoot issues that happen while you start or run the container.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Выставление счетов

The Face API containers send billing information to Azure by using a Face API resource on your Azure account. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Дополнительные сведения об этих параметрах см. в разделе [Настройка контейнеров](./face-resource-container-config.md).

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Резюме

In this article, you learned concepts and workflow for how to download, install, and run Face API containers. В разделе "Сводка" сделайте следующее.

* Container images are downloaded from the Azure Container Registry.
* Образы контейнеров выполняются в Docker.
* You can use either the REST API or the SDK to call operations in Face API containers by specifying the host URI of the container.
* You must specify billing information when you instantiate a container.

> [!IMPORTANT]
> Cognitive Services containers aren't licensed to run without being connected to Azure for metering. Customers must enable the containers to communicate billing information with the metering service at all times. Контейнеры Cognitive Services не отправляют в корпорацию Майкрософт данные клиента, например анализируемые изображения или тексты.

## <a name="next-steps"></a>Дальнейшие действия

* For configuration settings, see [Configure containers](face-resource-container-config.md).
* To learn more about how to detect and identify faces, see [Face overview](Overview.md).
* For information about the methods supported by the container, see the [Face API](//westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).
* To use more Cognitive Services containers, see [Cognitive Services containers](../cognitive-services-container-support.md).
