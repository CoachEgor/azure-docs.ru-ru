---
title: Миграция из индексаторов версии 1 и v2 в индексатор видео служб мультимедиа Azure | Документация Майкрософт
description: В этом разделе описано, как выполнить миграцию из Azure Media Indexer v1 и v2 в индексатор видео служб мультимедиа Azure.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2019
ms.author: juliako
ms.openlocfilehash: 2268c074480f99ca23117ca2ffd2c87c1dbb10a2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "76513241"
---
# <a name="migrate-from-media-indexer-and-media-indexer-2-to-video-indexer"></a>Миграция из индексатора мультимедиа и индексатора мультимедиа 2 в индексатор видео

Процессор [Azure Media indexer](media-services-index-content.md) media и [Azure Media indexer 2 Preview](media-services-process-content-with-indexer2.md) обработчики мультимедиа выводятся из эксплуатации. Сведения о датах выбытия см. в разделе об [устаревших компонентах](legacy-components.md) . [Индексатор видео служб мультимедиа Azure](https://docs.microsoft.com/azure/media-services/video-indexer/) заменяет эти устаревшие обработчики мультимедиа.

Индексатор видео служб мультимедиа Azure создан на основе Аналитика мультимедиа Azure, Когнитивный поиск Azure Cognitive Services (например, API распознавания лиц, Microsoft Translator, API компьютерного зрения и службы Пользовательское распознавание речи). Индексатор видео позволяет извлекать аналитические сведения из видеоматериалов с помощью видео- и аудиомоделей. Чтобы узнать, в каких сценариях можно использовать индексатор видео, какие функции он предлагает и как приступить к работе, см. статью [видео и звуковые модели индексатора видео](../video-indexer/video-indexer-overview.md). 

Вы можете извлекать аналитические данные из видео-и звуковых файлов с помощью [предварительных установок анализатора служб мультимедиа Azure v3](../latest/analyzing-video-audio-files-concept.md) или напрямую с помощью [API-интерфейсов индексатора видео](https://api-portal.videoindexer.ai/). В настоящее время существует перекрытие функций, предоставляемых API индексатора видео и API-интерфейсов служб мультимедиа v3.

> [!NOTE]
> Чтобы определить, в каких случаях вам лучше использовать Индексатор видео, а в каких — предустановки анализатора Служб мультимедиа, ознакомьтесь со статьей, в которой приводится [сравнение](../video-indexer/compare-video-indexer-with-media-services-presets.md) этих инструментов. 

В этой статье рассматриваются действия по миграции из Azure Media Indexer и Azure Media Indexer 2 в индексатор видео служб мультимедиа Azure.  

## <a name="migration-options"></a>Варианты переноса 

|Если требуется  |а затем — |
|---|---|
|решение, которое предоставляет возможность подзаписи речи в текст для любого формата файлов мультимедиа в форматах файлов скрытых субтитров: ВТТ, SRT или TTML<br/>а также дополнительные сведения о звуках, такие как ключевые слова, темы для событий, акустические события, диаризатиони докладчика, извлечение и перевод сущностей.| Обновите приложения для использования возможностей индексатора видео Azure с помощью индексатора видео версии 2 REST API или предустановки анализатора звука для служб мультимедиа Azure v3.|
|возможности преобразования речи в текст| Используйте API распознавания речи Cognitive Services напрямую.|  

## <a name="getting-started-with-video-indexer"></a>Приступая к работе с индексатором видео

В следующем разделе приведены ссылки на соответствующие вопросы: [как начать работу с индексатором видео?](https://docs.microsoft.com/azure/media-services/video-indexer/video-indexer-overview#how-can-i-get-started-with-video-indexer) 

## <a name="getting-started-with-media-services-v3-apis"></a>Начало работы с API-интерфейсами служб мультимедиа v3

API служб мультимедиа Azure v3 позволяет извлекать аналитические данные из видео и звуковых файлов с помощью [предустановок анализатора служб мультимедиа Azure v3](../latest/analyzing-video-audio-files-concept.md). 

**AudioAnalyzerPreset** позволяет извлекать множество звуковых аналитических сведений из аудио- или видеофайла. Выходные данные включают файл ВТТ или TTML для записи звука и JSON-файл (со всеми дополнительными сведениями о Audio Insights). В данные аудио Insights входят ключевые слова, индексирование докладчика и анализ тональности речи. Аудиоанализерпресет также поддерживает определение языка для конкретных языков. Подробные сведения см. в разделе [преобразования](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#audioanalyzerpreset).

### <a name="get-started"></a>Начало работы

Чтобы приступить к работе, изучите следующую статью:

* [Руководство](../latest/analyze-videos-tutorial-with-api.md)
* Примеры Аудиоанализерпресет: [пакет SDK для Java](https://github.com/Azure-Samples/media-services-v3-java/tree/master/AudioAnalytics/AudioAnalyzer) или [пакет SDK для .NET](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/master/AudioAnalytics/AudioAnalyzer)
* Примеры Видеоанализерпресет: [пакет SDK для Java](https://github.com/Azure-Samples/media-services-v3-java/tree/master/VideoAnalytics/VideoAnalyzer) или [пакет SDK для .NET](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/master/VideoAnalytics/VideoAnalyzer)

## <a name="getting-started-with-cognitive-services-speech-services"></a>Начало работы с Cognitive Services голосовыми службами

[Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/) предоставляет службу преобразования речи в текст, расшифровывает звуковые потоки в текст в режиме реального времени, которые могут использоваться приложениями, инструментами или устройствами. Вы можете использовать преобразование речи в текст, чтобы [настроить собственную акустическую модель, модель языка или произношение модели](../../cognitive-services/speech-service/how-to-custom-speech-train-model.md). Дополнительные сведения см. в разделе [Cognitive Services преобразование речи в текст](../../cognitive-services/speech-service/speech-to-text.md). 

> [!NOTE] 
> Служба преобразования речи в текст не принимает форматы видеофайлов и принимает только [определенные звуковые форматы](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-speech-to-text#audio-formats). 

Дополнительные сведения о службе преобразования текста в речь и о том, как приступить к работе, см. в разделе [что такое преобразование речи в текст?](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-to-text)

## <a name="known-differences-from-deprecated-services"></a>Известные отличия от устаревших служб 

Вы обнаружите, что индексатор видео, службы мультимедиа Azure v3 Аудиоанализерпресет и службы Cognitive Services Speech Services более надежны и получают более качественные выходные данные, чем выпущенные Azure Media Indexer 1 и Azure Media Indexer 2 процессора.  

Ниже перечислены некоторые известные отличия. 

* Cognitive Services службы речи не поддерживают извлечение ключевых слов. Однако индексатор видео и службы мультимедиа v3 Аудиоанализерпресет оба предлагают более надежный набор ключевых слов в формате JSON. 

## <a name="need-help"></a>Требуется помощь?

Вы можете отправить запрос в службу поддержки, перейдя к [новому запросу в службу поддержки](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) .

## <a name="next-steps"></a>Дальнейшие шаги

* [Устаревшие компоненты](legacy-components.md)
* [Страница цен](https://azure.microsoft.com/pricing/details/media-services/#encoding)


