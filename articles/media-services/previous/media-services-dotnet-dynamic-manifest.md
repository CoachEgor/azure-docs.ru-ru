---
title: Создание фильтров с помощью пакета SDK для .NET служб мультимедиа Azure
description: В этом разделе описывается создание фильтров, с помощью которых клиент может передавать определенные секции потока. Для достижения такой выборочной потоковой передачи службы мультимедиа создают динамические манифесты.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 2f6894ca-fb43-43c0-9151-ddbb2833cafd
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako;cenkdin
ms.openlocfilehash: 05b899658b5c58e15b2f30ab759eb49319979fee
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61465564"
---
# <a name="creating-filters-with-media-services-net-sdk"></a>Создание фильтров с помощью пакета SDK Служб мультимедиа для .NET 
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [REST](media-services-rest-dynamic-manifest.md)
> 
> 

Начиная с версии 2.17, службы мультимедиа позволяют определять фильтры для активов. Эти фильтры представляют собой правила на стороне сервера. Они позволяют пользователям выполнять следующие действия: воспроизводить только часть видео (не все видео) или выбирать только часть представлений аудио и видео, которые может обрабатывать клиентское устройство (не все представления, связанные с файлом). Такая фильтрация активов достигается с помощью **динамических манифестов**, которые создаются по запросу клиента для потоковой передачи видео на основе указанных фильтров.

Подробные сведения о фильтрах и динамическом манифесте см. в статье [Фильтры и динамические манифесты](media-services-dynamic-manifest-overview.md).

В этой статье показано, как использовать пакет SDK .NET для Служб мультимедиа при создании, обновлении и удалении фильтров. 

Учтите, что при обновлении фильтра может понадобиться до двух минут на обновление правил конечной точкой потоковой передачи. Если содержимое было обработано с помощью данного фильтра (и кэшировано на прокси-серверах и в кэшах CDN), обновление этого фильтра может привести к сбоям проигрывателя. Всегда очищайте кэш после обновления фильтра. Если такой вариант невозможен, рассмотрите возможность использования другого фильтра. 

## <a name="types-used-to-create-filters"></a>Типы, используемые для создания фильтров
При создании фильтров используются следующие типы: 

* **IStreamingFilter**.  В основе этого типа лежит следующий интерфейс API REST: [Filter](https://docs.microsoft.com/rest/api/media/operations/filter)
* **IStreamingAssetFilter**. В основе этого типа лежит следующий интерфейс API REST: [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* **PresentationTimeRange**. В основе этого типа лежит следующий интерфейс API REST: [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* **FilterTrackSelectStatement** и **IFilterTrackPropertyCondition**. В основе этих типов лежат следующие интерфейсы API REST: [FilterTrackSelect и FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

## <a name="createupdatereaddelete-global-filters"></a>Создание, обновление, чтение и удаление глобальных фильтров
Следующий код демонстрирует, как использовать .NET для создания, обновления, чтения и удаления фильтров файлов.

```csharp
    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();

    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();

    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);

    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);

    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();

    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();
```

## <a name="createupdatereaddelete-asset-filters"></a>Создание, обновление, чтение и удаление фильтров ресурсов-контейнеров
Следующий код демонстрирует, как использовать .NET для создания, обновления, чтения и удаления фильтров файлов.

```csharp
    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);

    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();


    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());

    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);

    filter.Update();

    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filterUpdated.Delete();

```


## <a name="build-streaming-urls-that-use-filters"></a>Построение URL-адресов потоковой передачи с использованием фильтров
Сведения о публикации и доставке ресурсов см. в статье [Доставка содержимого клиентам](media-services-deliver-content-overview.md).

Следующие примеры показывают, как добавлять фильтры к URL-адресам потоковой передачи.

**MPEG DASH** 

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V4**

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V3**

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Smooth Streaming**

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


## <a name="media-services-learning-paths"></a>Схемы обучения работе со службами мультимедиа
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Отзывы
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>См. также
[Обзор динамических манифестов](media-services-dynamic-manifest-overview.md)

