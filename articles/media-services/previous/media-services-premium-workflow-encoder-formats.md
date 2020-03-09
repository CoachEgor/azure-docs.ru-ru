---
title: Форматы и кодеки рабочего процесса Media Encoder Premium | Документация Майкрософт
description: В этом разделе содержится обзор форматов и кодеков расширенного рабочего процесса кодировщика мультимедиа.
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
ms.date: 03/19/2019
ms.author: juliako
ms.reviewer: anilmur
ms.openlocfilehash: 87cd7c63939331190530a46071a6b4c40480562f
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2020
ms.locfileid: "78392961"
---
# <a name="media-encoder-premium-workflow-formats-and-codecs"></a>Форматы и кодеки рабочего процесса Premium Media Encoder

> [!NOTE]
> Обработчик мультимедиа расширенного рабочего процесса кодировщика мультимедиа, рассматриваемый в этом разделе, недоступен в Китае. 

Этот документ содержит список форматов входных и выходных файлов, а также список кодеков, которые поддерживает общедоступная предварительная версия **расширенного рабочего процесса кодировщика мультимедиа** .

[Входные форматы и кодеки Media Encoder Premium Workflow](#input_formats)

Выходные форматы и кодеки Media Encoder Premium Workflow

**Расширенный рабочий процесс кодировщика мультимедиа** поддерживает скрытые субтитры, описанные в [этом](#closed_captioning) разделе. 

## <a id="input_formats"></a>Входные форматы и кодеки расширенного рабочего процесса кодировщика мультимедиа

В следующем разделе приводится список кодеков и форматов файлов, которые этот кодировщик мультимедиа поддерживает на входе.

### <a name="input-containerfile-formats"></a>Контейнер ввода/ форматы файлов

* Adobe® Flash® F4V
* MXF/SMPTE 377M
* GXF
* Транспортные потоки MPEG-2
* Программные потоки MPEG-2
* MPEG-4/MP4
* Windows Media/ASF
* AVI (без сжатия 8 бит/10 бит)

### <a name="input-video-codecs"></a>Входные видеокодеки

* AVC 8-разрядный/10-разрядный, до 4:2:2, включая AVCIntra
* Avid DNxHD (в MXF)
* DVCPro/DVCProHD (в MXF)
* Профиль HEVC/H.265, Main и Main 10
* JPEG2000
* MPEG-2 (до 422 Profile и High Level, включая такие варианты, как XDCAM, XDCAM HD, XDCAM IMX, CableLabs® и D10)
* MPEG-1
* Windows Media Video/VC-1

### <a name="input-audio-codecs"></a>Входные аудиокодеки

* AES (SMPTE 331M и 302M, AES3-2003)
* Dolby® E
* Dolby® Digital (AC3)
* AAC (AAC-LC, AAC-HE и AAC-HEv2; до 5.1)
* MPEG Layer 2
* MP3 (MPEG-1 Audio Layer 3)
* Windows Media Audio
* WAV/PCM

## <a id="output_format"></a>Выходные форматы и кодеки расширенного рабочего процесса кодировщика мультимедиа

В следующем разделе приводится список кодеков и форматов файлов, которые этот кодировщик мультимедиа поддерживает на выходе.

### <a name="output-containerfile-formats"></a>Контейнер вывода/ форматы файлов

* Adobe® Flash® F4V
* MXF (OP1a, XDCAM и AS02)
* DPP (включая AS11)
* GXF
* MPEG-4/MP4
* Windows Media/ASF
* AVI (без сжатия 8 бит/10 бит)
* Формат файлов Smooth Streaming (PIFF 1.3)
* MPEG-TS 

### <a name="output-video-codecs"></a>Выходные видеокодеки

* AVC (H.264; 8-разрядный; до High Profile, Level 5.2; 4K Ultra HD; AVC Intra)
* Avid DNxHD (в MXF)
* DVCPro/DVCProHD (в MXF)
* MPEG-2 (до 422 Profile и High Level, включая такие варианты, как XDCAM, XDCAM HD, XDCAM IMX, CableLabs® и D10)
* MPEG-1
* Windows Media Video/VC-1
* Создание эскизов JPEG
* HEVC (H.265; 8 и 10 бит, профиль Main и Main 10)


### <a name="output-audio-codecs"></a>Выходные аудиокодеки

* AES (SMPTE 331M и 302M, AES3-2003)
* Dolby® Digital (AC3)
* Dolby® Digital Plus (E-AC3) до 7.1
* AAC (AAC-LC, AAC-HE и AAC-HEv2; до 5.1)
* MPEG Layer 2
* MP3 (MPEG-1 Audio Layer 3)
* Windows Media Audio

>[!NOTE]
>При кодировании в формат Dolby® Digital (AC3) выходные данные могут записываться только в MP4-файл ISO.

## <a id="closed_captioning"></a>Поддержка скрытых титров

При приеме **расширенный рабочий процесс кодировщика мультимедиа** поддерживает:

1. файлы SCC
2. файлы SMPTE-TT
3. CEA-608/CEA-708 – переносятся как данные пользователя (сообщения SEI простейших потоков H.264, ATSC/53, SCTE20) или переносятся как вспомогательные данные в файлах MXF/GXF
4. Файлы субтитров STL

На выходе доступны следующие параметры:

1. Преобразование CEA-608 в CEA-708
2. Пропуск CEA-608/CEA-708 (встраиваются в сообщения SEI простейших потоков H.264 или переносятся как дополнительные данный в файлах MXF)
3. SCC
4. SMPTE Timed Text (из источника CEA-608 через SMPTE RP2052, включая создание файла DFXP)
5. Файл подзаголовка SRT
6. Потоки подзаголовка DVB

> [!NOTE]
> Для доставки через потоковую передачу в службах мультимедиа Azure поддерживаются не все указанные выше форматы.

## <a name="known-issues"></a>Известные проблемы

Если входящее видео не содержит скрытых субтитров, выходящий ресурс по-прежнему будет содержать пустой файл TTML. 

## <a name="need-help"></a>Требуется помощь?

Вы можете открыть запрос в службу поддержки, перейдя к разделу [нового запроса на техническую поддержку](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)
## <a name="media-services-learning-paths"></a>Схемы обучения работе со службами мультимедиа

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Отзывы

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

