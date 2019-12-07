---
title: Использование существующих проигрывателей для воспроизведения содержимого в Azure | Документация Майкрософт
description: В этой статье перечислены существующие проигрыватели, которые можно использовать для воспроизведения содержимого.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 7e9fcf89-0fb6-4fa4-96cb-666320684d69
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: fdcaa7d8eec0065ad9acd76df9918ed7bccc081b
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2019
ms.locfileid: "74895857"
---
# <a name="playing-your-content-with-existing-players"></a>Воспроизведение содержимого с помощью существующих проигрывателей
Службы мультимедиа Azure поддерживают многие популярные форматы потоковой передачи, например Smooth Streaming, HTTP Live Streaming и MPEG-Dash. В этой статье описываются имеющиеся проигрыватели, которые можно использовать для тестирования потоков.

### <a name="the-azure-portal-media-services-content-player"></a>Проигрыватель содержимого служб мультимедиа портала Azure
Портал **Azure** предлагает проигрыватель содержимого, с помощью которого можно проверить видео.

Выберите нужное видео (убедитесь, что оно [опубликовано](media-services-portal-publish.md)) и нажмите кнопку **Воспроизвести** в нижней части портала.

Важные особенности

* **ПРОИГРЫВАТЕЛЬ КОНТЕНТА СЛУЖБ МУЛЬТИМЕДИА** выполняет воспроизведение из конечной точки потоковой передачи по умолчанию. Если требуется воспроизвести из конечной точке потоковой передачи не по умолчанию, используйте другой проигрыватель. Например, [Проигрыватель мультимедиа Azure](https://aka.ms/azuremediaplayer).

![AMSPlayer][AMSPlayer]

### <a name="azure-media-player"></a>Проигрыватель мультимедиа Azure

Используйте [Проигрыватель мультимедиа Azure](https://aka.ms/azuremediaplayer) для воспроизведения содержимого (незашифрованного или защищенного) в любом из следующих форматов:

* Smooth Streaming
* MPEG DASH
* HLS
* Последовательный MP4

### <a name="flash-player"></a>Flash Player

#### <a name="aes-encrypted-with-token"></a>Шифрование AES с маркером

[https://aestoken.azurewebsites.net](https://aestoken.azurewebsites.net)

#### <a name="playready-with-token"></a>PlayReady с маркером

[https://sltoken.azurewebsites.net](https://sltoken.azurewebsites.net)

### <a name="dash-players"></a>Проигрыватели DASH

[https://dashplayer.azurewebsites.net](https://dashplayer.azurewebsites.net)

[https://dashif.org](https://dashif.org)

### <a name="other"></a>Прочее
Для проверки URL-адресов HLS также можно использовать:

* **Safari** на устройстве iOS или
* **3ivx HLS Player** в Windows.

## <a name="developing-video-players"></a>Разработка видеопроигрывателей
Сведения о том, как разрабатывать собственные проигрыватели, см. в статье [Разработка приложений видеопроигрывателя](media-services-develop-video-players.md).

## <a name="media-services-learning-paths"></a>Схемы обучения работе со службами мультимедиа
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Отправить отзыв
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
