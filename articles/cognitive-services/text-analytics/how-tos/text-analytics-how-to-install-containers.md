---
title: Установка и запуск контейнеров — Анализ текста
titleSuffix: Azure Cognitive Services
description: В этом пошаговом руководстве показано, как скачивать, устанавливать и выполнять контейнеры для Анализа текста.
services: cognitive-services
author: aahill
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 2d44df1bb828140e662b06ffbe5fb14f207f68e0
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "80877083"
---
# <a name="install-and-run-text-analytics-containers"></a>Установка и запуск контейнеров API анализа текста

Контейнеры позволяют запускать API анализа текста в собственной среде и прекрасно подходят для конкретных требований к безопасности и управлению данными. Контейнеры Анализ текста обеспечивают расширенную обработку естественного языка по необработанному тексту и включают три основные функции: анализ тональностиности, извлечение ключевых фраз и определение языка. Связывание сущностей в настоящее время не поддерживается в контейнере.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

> [!IMPORTANT]
> Бесплатная учетная запись ограничена 5 000 транзакциями в месяц, а для контейнеров допустимы только <a href="https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics" target="_blank">ценовые <span class="docon docon-navigate-external x-hidden-focus"></span> категории</a> " **бесплатный** " и " **стандартный** ". Дополнительные сведения о тарифах запросов транзакций см. в разделе [ограничения данных](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview#data-limits).

## <a name="prerequisites"></a>Предварительные требования

Чтобы запустить любой из Анализ текста контейнеров, необходимо иметь главный компьютер и среды контейнеров.

## <a name="preparation"></a>Подготовка

Прежде чем использовать контейнеры Анализа текста, необходимо выполнить следующие условия:

|Обязательный|Назначение|
|--|--|
|Модуль Docker| На [главном компьютере](#the-host-computer) должен быть установлен модуль Docker. Docker предоставляет пакеты, которые настраивают среду с Docker для [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) и [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Ознакомьтесь с [общими сведениями о Docker и контейнерах](https://docs.docker.com/engine/docker-overview/).<br><br> Docker нужно настроить таким образом, чтобы контейнеры могли подключать и отправлять данные о выставлении счетов в Azure. <br><br> **В ОС Windows** для Docker нужно также настроить поддержку контейнеров Linux.<br><br>|
|Опыт работы с Docker | Требуется базовое представление о понятиях Docker, включая реестры, репозитории, контейнеры и образы контейнеров, а также знание основных команд `docker`.| 
|Ресурс API анализа текста |Для использования контейнера необходимо следующее:<br><br>Ресурс Azure [анализ текста](../../cognitive-services-apis-create-account.md) для получения соответствующего ключа API и URI конечной точки. Оба значения доступны на страницах "Обзор" и "Ключи" API анализа текста на портале Azure и необходимы для запуска контейнера.<br><br>**{API_KEY}**: один из двух доступных ключей ресурсов на странице " **ключи** "<br><br>**{ENDPOINT_URI}**: конечная точка, указанная на странице **обзора**|

[!INCLUDE [Gathering required parameters](../../containers/includes/container-gathering-required-parameters.md)]

## <a name="the-host-computer"></a>Главный компьютер

[!INCLUDE [Host Computer requirements](../../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Требования к контейнеру и рекомендации

В следующей таблице описаны минимальные и рекомендуемые требования к ЦП (минимум 2,6 ГГц или быстрее) и памяти, в ГБ, для выделения для каждого контейнера Анализа текста.

# <a name="key-phrase-extraction"></a>[извлечение ключевых фраз](#tab/keyphrase)

[!INCLUDE [key-phrase-extraction-container-requirements](../includes/key-phrase-extraction-container-requirements.md)]

# <a name="language-detection"></a>[распознавание языка](#tab/language)

[!INCLUDE [language-detection-container-requirements](../includes/language-detection-container-requirements.md)]

# <a name="sentiment-analysis"></a>[Анализ тональности](#tab/sentiment).

[!INCLUDE [sentiment-analysis-container-requirements](../includes/sentiment-analysis-container-requirements.md)]

***

* Частота каждого ядра должна быть минимум 2,6 ГГц.
* TPS — транзакций в секунду.

Ядро и память соответствуют параметрам `--cpus` и `--memory`, которые используются как часть команды `docker run`.

## <a name="get-the-container-image-with-docker-pull"></a>Получение образа контейнера с помощью `docker pull`

Образы контейнеров для Анализ текста доступны в реестре контейнеров Microsoft.

# <a name="key-phrase-extraction"></a>[извлечение ключевых фраз](#tab/keyphrase)

[!INCLUDE [key-phrase-extraction-container-repository](../includes/key-phrase-extraction-container-repository.md)]

# <a name="language-detection"></a>[распознавание языка](#tab/language)

[!INCLUDE [language-detection-container-repository](../includes/language-detection-container-repository.md)]

# <a name="sentiment-analysis"></a>[Анализ тональности](#tab/sentiment).

[!INCLUDE [sentiment-analysis-container-repository](../includes/sentiment-analysis-container-repository.md)]

***

[!INCLUDE [Tip for using docker list](../../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-text-analytics-containers"></a>Извлечение DOCKER для контейнеров Анализ текста

# <a name="key-phrase-extraction"></a>[извлечение ключевых фраз](#tab/keyphrase)

[!INCLUDE [docker-pull-key-phrase-extraction-container](../includes/docker-pull-key-phrase-extraction-container.md)]

# <a name="language-detection"></a>[распознавание языка](#tab/language)

[!INCLUDE [docker-pull-language-detection-container](../includes/docker-pull-language-detection-container.md)]

# <a name="sentiment-analysis"></a>[Анализ тональности](#tab/sentiment).

[!INCLUDE [docker-pull-sentiment-analysis-container](../includes/docker-pull-sentiment-analysis-container.md)]

***

## <a name="how-to-use-the-container"></a>Использование контейнера

После размещения контейнера на [главном компьютере](#the-host-computer) воспользуйтесь следующей процедурой для работы с ним.

1. [Запустите контейнер](#run-the-container-with-docker-run) с необходимыми настройками выставления счетов. Доступны дополнительные [примеры](../text-analytics-resource-container-config.md#example-docker-run-commands) команды `docker run`.
1. [Запросите конечную точку прогнозирования контейнера](#query-the-containers-prediction-endpoint).

## <a name="run-the-container-with-docker-run"></a>Запуск контейнера с помощью команды `docker run`

Воспользуйтесь командой [docker run](https://docs.docker.com/engine/reference/commandline/run/) для запуска любого из трех контейнеров. Дополнительные сведения о том, как получить значения и `{ENDPOINT_URI}` `{API_KEY}` , см. в разделе [сбор обязательных параметров](#gathering-required-parameters) .

Доступны [примеры](../text-analytics-resource-container-config.md#example-docker-run-commands) `docker run` команд.

# <a name="key-phrase-extraction"></a>[извлечение ключевых фраз](#tab/keyphrase)

[!INCLUDE [docker-run-key-phrase-extraction-container](../includes/docker-run-key-phrase-extraction-container.md)]

# <a name="language-detection"></a>[распознавание языка](#tab/language)

[!INCLUDE [docker-run-language-detection-container](../includes/docker-run-language-detection-container.md)]

# <a name="sentiment-analysis"></a>[Анализ тональности](#tab/sentiment).

[!INCLUDE [docker-run-sentiment-analysis-container](../includes/docker-run-sentiment-analysis-container.md)]

***

> [!IMPORTANT]
> Для запуска контейнера необходимо указать параметры `Eula`, `Billing` и `ApiKey`. В противном случае контейнер не запустится.  Дополнительные сведения см. в [разделе о выставлении счетов](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

## <a name="query-the-containers-prediction-endpoint"></a>Запрос конечной точки прогнозирования контейнера

Контейнер предоставляет API запроса конечной точки прогнозирования на основе REST.

Используйте узел `http://localhost:5000` для API контейнера.

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Остановка контейнера

[!INCLUDE [How to stop the container](../../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Устранение неполадок

Если контейнер запускается с выходным [подключением](../text-analytics-resource-container-config.md#mount-settings) и включенным ведением журнала, контейнер создает файлы журнала, которые удобно использовать для устранения неполадок, возникающих во время запуска или работы контейнера.

[!INCLUDE [Cognitive Services FAQ note](../../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Выставление счетов

Контейнеры API анализа текста отправляют данные для выставления счетов в Azure с помощью ресурса _API анализа текста_ в учетной записи Azure. 

[!INCLUDE [Container's Billing Settings](../../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Дополнительные сведения об этих параметрах см. в статье [Настройка контейнеров](../text-analytics-resource-container-config.md).

<!--blogs/samples/video course -->

[!INCLUDE [Discoverability of more container information](../../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Сводка

В этой статье вы узнали основные понятия и рабочий процесс для скачивания, установки и выполнения контейнеров Анализа текста. В разделе "Сводка" сделайте следующее.

* Анализ текста предоставляет три контейнера Linux для DOCKER, включая различные возможности:
   * *извлечение ключевых фраз*
   * *распознавание языка*
   * *Анализ тональности*.
* Образы контейнеров скачиваются из Реестра контейнеров (Майкрософт) в Azure.
* Образы контейнеров выполняются в Docker.
* Указав узел URI контейнера, пакет SDK или REST API можно использовать для вызова операций в контейнерах Анализа текста.
* При создании экземпляра контейнера нужно указать данные для выставления счетов.

> [!IMPORTANT]
> Контейнеры Cognitive Services не лицензируются для запуска без подключения к Azure для отслеживания использования. Клиенты должны разрешить контейнерам непрерывную передачу данных для выставления счетов в службу контроля потребления. Контейнеры Cognitive Services не отправляют в корпорацию Майкрософт данные клиента (например, анализируемые изображения или тексты).

## <a name="next-steps"></a>Следующие шаги

* Ознакомьтесь со статьей о [конфигурации контейнеров](../text-analytics-resource-container-config.md).
* Чтобы решить проблемы, связанные с функциональностью, ознакомьтесь с [часто задаваемыми вопросами](../text-analytics-resource-faq.md).
