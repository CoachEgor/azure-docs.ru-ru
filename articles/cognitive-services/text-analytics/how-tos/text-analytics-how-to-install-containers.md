---
title: Установка и запуск контейнеров
titleSuffix: Text Analytics -  Azure Cognitive Services
description: В этом пошаговом руководстве показано, как скачивать, устанавливать и выполнять контейнеры для Анализа текста.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 04/16/2019
ms.author: diberry
ms.openlocfilehash: e0e8b9f767376db8028a3ac4a2d8659bab69268b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60829962"
---
# <a name="install-and-run-text-analytics-containers"></a>Установка и запуск контейнеров API анализа текста

Контейнеры API анализа текста предоставляют расширенную обработку необработанного текста на естественных языках и включают в себя три основные функции: анализ тональности, извлечение ключевых фраз и определение языка. Связывание сущностей в настоящее время не поддерживается в контейнере. 

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Технические условия

Для запуска любой из контейнеров анализа текста, необходимо иметь компьютер и контейнер сред размещения.

## <a name="preparation"></a>Подготовка

Прежде чем использовать контейнеры Анализа текста, необходимо выполнить следующие условия:

|Обязательно для заполнения|Назначение|
|--|--|
|Модуль Docker| На [главном компьютере](#the-host-computer) должен быть установлен модуль Docker. Docker предоставляет пакеты, которые настраивают среду Docker в ОС [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) и [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Ознакомьтесь с [общими сведениями о Docker и контейнерах](https://docs.docker.com/engine/docker-overview/).<br><br> Docker нужно настроить таким образом, чтобы контейнеры могли подключать и отправлять данные о выставлении счетов в Azure. <br><br> **В ОС Windows** для Docker нужно также настроить поддержку контейнеров Linux.<br><br>|
|Опыт работы с Docker | Требуется базовое представление о понятиях Docker, включая реестры, репозитории, контейнеры и образы контейнеров, а также знание основных команд `docker`.| 
|`Cognitive Services` Ресурс |Для использования контейнера необходимо следующее:<br><br>Объект [ _Cognitive Services_ ](text-analytics-how-to-access-key.md) ресурсов Azure, чтобы получить связанный ключ выставления счетов и выставления счетов URI конечной точки. Оба значения доступны на страницах Общие сведения о Cognitive Services и ключей на портале Azure и необходимые для запуска контейнера. Необходимо добавить `text/analytics/v2.0` маршрутизации URI конечной точки, как показано в следующем примере BILLING_ENDPOINT_URI.<br><br>**{BILLING_KEY}**: ключ ресурса<br><br>**{BILLING_ENDPOINT_URI}**: пример URI конечной точки: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1`|

### <a name="the-host-computer"></a>Главный компьютер

[!INCLUDE [Host Computer requirements](../../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Требования к контейнеру и рекомендации

В следующей таблице описаны минимальные и рекомендуемые требования к ЦП (минимум 2,6 ГГц или быстрее) и памяти, в ГБ, для выделения для каждого контейнера Анализа текста.

| Контейнер | Минимальная | Рекомендуется | ТРАНЗАКЦИЙ В СЕКУНДУ<br>(Минимум, максимум)|
|-----------|---------|-------------|--|
|Извлечение ключевой фразы | 1 ядро, 2 ГБ памяти | 1 ядро, 4 ГБ памяти |15, 30|
|Распознавание языка | 1 ядро, 2 ГБ памяти | 1 ядро, 4 ГБ памяти |15, 30|
|Анализ тональности | 1 ядро, 2 ГБ памяти | 1 ядро, 4 ГБ памяти |15, 30|

* Частота каждого ядра должна быть минимум 2,6 ГГц.
* Транзакций в Секунду - транзакций в секунду

Ядро и память соответствуют параметрам `--cpus` и `--memory`, которые используются как часть команды `docker run`.

## <a name="get-the-container-image-with-docker-pull"></a>Получение образа контейнера с помощью `docker pull`

Образы контейнеров для Анализа текста доступны в Реестре контейнеров Майкрософт. 

| Контейнер | Репозиторий |
|-----------|------------|
|Извлечение ключевой фразы | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
|Распознавание языка | `mcr.microsoft.com/azure-cognitive-services/language` |
|Анализ тональности | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

Используйте [ `docker pull` ](https://docs.docker.com/engine/reference/commandline/pull/) команду, чтобы скачать образ контейнера из реестра контейнеров Microsoft.

Полное описание доступных тегов для контейнеров Анализа текста см. в следующих контейнерах в центре Docker:

* [Извлечение ключевых фраз](https://go.microsoft.com/fwlink/?linkid=2018757).
* [Распознавание языка](https://go.microsoft.com/fwlink/?linkid=2018759).
* [Анализ тональности](https://go.microsoft.com/fwlink/?linkid=2018654).

Воспользуйтесь командой [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/), чтобы скачать образ контейнера.


### <a name="docker-pull-for-the-key-phrase-extraction-container"></a>docker pull для контейнера извлечения ключевой фразы

```
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```

### <a name="docker-pull-for-the-language-detection-container"></a>docker pull для контейнера определения языка

```
docker pull mcr.microsoft.com/azure-cognitive-services/language:latest
```

### <a name="docker-pull-for-the-sentiment-container"></a>docker pull для контейнера тональности

```
docker pull mcr.microsoft.com/azure-cognitive-services/sentiment:latest
```

[!INCLUDE [Tip for using docker list](../../../../includes/cognitive-services-containers-docker-list-tip.md)]


## <a name="how-to-use-the-container"></a>Использование контейнера

После размещения контейнера на [главном компьютере](#the-host-computer) воспользуйтесь следующей процедурой для работы с ним.

1. [Запустите контейнер](#run-the-container-with-docker-run) с необходимыми настройками выставления счетов. Доступны дополнительные [примеры](../text-analytics-resource-container-config.md#example-docker-run-commands) команды `docker run`. 
1. [Запрос конечной точки прогнозирования контейнера](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Запуск контейнера с помощью команды `docker run`

Воспользуйтесь командой [docker run](https://docs.docker.com/engine/reference/commandline/run/) для запуска любого из трех контейнеров. В команде используются следующие параметры:

| Placeholder | Value |
|-------------|-------|
|{BILLING_KEY} | Этот ключ используется для запуска контейнера, а также доступна на портале Azure `Cognitive Services` страницы "ключи".  |
|{BILLING_ENDPOINT_URI} | Выставления счетов значение URI конечной точки можно найти в Azure `Cognitive Services` странице "Обзор". <br><br>Пример:<br>`Billing=https://westus.api.cognitive.microsoft.com/text/analytics/v2.0`|

Необходимо добавить `text/analytics/v2.0` маршрутизации URI конечной точки, как показано в приведенном выше примере BILLING_ENDPOINT_URI.

В следующем примере команды `docker run` замените имена параметров собственными значениями.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/keyphrase \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Эта команда:

* запускает контейнер ключевой фразы из образа контейнера;
* выделяет одно ядро ЦП и 4 ГБ памяти;
* предоставляет TCP-порт 5000 и выделяет псевдотелетайп для контейнера;
* автоматически удаляет контейнер после завершения его работы. Образ контейнера остается доступным на главном компьютере. 

Доступны дополнительные [примеры](../text-analytics-resource-container-config.md#example-docker-run-commands) команды `docker run`. 

> [!IMPORTANT]
> Для выполнения контейнера необходимо указать параметры `Eula`, `Billing` и `ApiKey`. В противном случае контейнер не запустится.  Дополнительные сведения см. в [разделе о выставлении счетов](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

## <a name="query-the-containers-prediction-endpoint"></a>Запрос конечной точки прогнозирования контейнера

Контейнер предоставляет API запроса конечной точки прогнозирования на основе REST. 

Используйте узел `https://localhost:5000` для API контейнера.

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Остановка контейнера

[!INCLUDE [How to stop the container](../../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Устранение неполадок

Если контейнер запускается с выходным [подключением](../text-analytics-resource-container-config.md#mount-settings) и включенным ведением журнала, контейнер создает файлы журнала, которые удобно использовать для устранения неполадок, возникающих во время запуска или работы контейнера. 

## <a name="billing"></a>Выставление счетов

Отправить контейнеры анализа текста, выставления счетов в Azure, с помощью _Cognitive Services_ ресурсов на учетную запись Azure. 

[!INCLUDE [Container's Billing Settings](../../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Дополнительные сведения об этих параметрах см. в статье [Настройка контейнеров](../text-analytics-resource-container-config.md).

## <a name="summary"></a>Сводка

В этой статье вы узнали основные понятия и рабочий процесс для скачивания, установки и выполнения контейнеров Анализа текста. В разделе "Сводка" сделайте следующее.

* Анализ текста предоставляет три контейнера Linux для Docker, инкапсулируя извлечение ключевых фраз, определение языка и анализ тональности.
* Образы контейнеров скачиваются из Реестра контейнеров (Майкрософт) в Azure.
* Образы контейнеров выполняются в Docker.
* Указав узел URI контейнера, пакет SDK или REST API можно использовать для вызова операций в контейнерах Анализа текста.
* При создании экземпляра контейнера нужно указать данные для выставления счетов.

> [!IMPORTANT]
> Контейнеры Cognitive Services не лицензируются для запуска без подключения к Azure для отслеживания использования. Клиенты должны разрешить контейнерам непрерывную передачу данных для выставления счетов в службу контроля потребления. Контейнеры Cognitive Services не отправляют в корпорацию Майкрософт данные клиента (например, анализируемые изображения или тексты).

## <a name="next-steps"></a>Дальнейшие действия

* Ознакомьтесь со статьей о [конфигурации контейнеров](../text-analytics-resource-container-config.md).
* Чтобы решить проблемы, связанные с функциональностью, ознакомьтесь с [часто задаваемыми вопросами](../text-analytics-resource-faq.md).

