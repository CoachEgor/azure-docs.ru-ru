---
title: Кодирование ресурса с использованием кодировщика мультимедиа Standard на портале Azure | Документация Майкрософт
description: В этом руководстве описаны шаги кодирования ресурса с помощью кодировщика мультимедиа Standard на портале Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 107d9e9a-71e9-43e5-b17c-6e00983aceab
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 7c147d99eaabee70316521d1f2bdc41933162ab1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "69542607"
---
# <a name="encode-an-asset-by-using-media-encoder-standard-in-the-azure-portal"></a>Кодирование ресурса с помощью кодировщика мультимедиа Standard на портале Azure

> [!NOTE]
> Для работы с этим учебником требуется учетная запись Azure. Для получения подробной информации [см.](https://azure.microsoft.com/pricing/free-trial/) 
> 
> 

При работе со службами мультимедиа Azure один из самых частых сценариев — доставка потоковой передачи с переменной скоростью клиентам. Службы мультимедиа поддерживают следующие потоковые передачи с переменной скоростью: Apple HTTP Live Streaming (HLS), Microsoft Smooth Streaming и динамическая адаптивная потоковая передача по HTTP (DASH, также называется MPEG-DASH). Чтобы подготовить видео к потоковой передаче с переменной скоростью, необходимо закодировать исходное видео в файлы с поддержкой разных скоростей. Для кодирования видео можно использовать медиа-кодера Standard.  

Службы мультимедиа обеспечивают динамическую упаковку. При использовании динамической упаковки можно доставлять MP4-файлы с несколькими скоростями в форматах HLS, Smooth Streaming и MPEG-DASH без распаковки в этих форматах потоковой передачи. Используя динамическую упаковку, можно хранить и оплачивать файлы в одном формате хранения. Службы мультимедиа создают и обрабатывают соответствующий ответ с учетом запроса клиента.

Чтобы воспользоваться преимуществами динамического шифрования, закодируйте исходный файл в набор MP4-файлов с несколькими скоростями. Этапы кодирования описаны далее в этой статье.

Сведения о масштабировании обработки мультимедиа см. в статье [Изменение типа зарезервированных единиц](media-services-portal-scale-media-processing.md).

## <a name="encode-in-the-azure-portal"></a>Кодировка на портале Azure

Чтобы закодировать содержимое с помощью кодировщика мультимедиа Standard:

1. Выберите учетную запись служб мультимедиа Azure на [портале Azure](https://portal.azure.com/).
2. Выберите**активы** **настроек.** >  Выберите ресурс, который требуется закодировать.
3. Нажмите кнопку **Кодировать**.
4. В области **кодирования ресурса** выберите обработчик **Кодировщик мультимедиа Standard** с предустановкой. Дополнительные сведения о предустановках см. в статьях [Использование стандартной версии кодировщика мультимедиа Azure для автоматического создания схемы скоростей](media-services-autogen-bitrate-ladder-with-mes.md) и [Предустановки задач для Media Encoder Standard](media-services-mes-presets-overview.md). Важно выбрать предустановку, которая больше всего подходит для входного видео. Например, если известно, что входное видео имеет разрешение 1920 &#215; 1080 пикселей, можно использовать предустановку **H264 Multiple Bitrate 1080p**. Если у вас видео с низким разрешением (640 &#215; 360), предустановку **H264 Multiple Bitrate 1080p** использовать не следует.
   
   Чтобы упростить управление ресурсами, вы можете изменить имена выходного ресурса и задания.
   
   ![Кодирование ресурсов](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Выберите **Создать**.

## <a name="media-services-learning-paths"></a>Схемы обучения работе со службами мультимедиа
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Отзывы
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Дальнейшие действия
* [Отслеживайте ход выполнения задания кодирования](media-services-portal-check-job-progress.md) на портале Azure.  

