---
title: Сравнение индексаторов видео и предварительных установок служб мультимедиа Azure v3
description: В этой статье сравниваются возможности индексатора видео и предустановки служб мультимедиа Azure версии 3.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 2e2abd4ffe5a6d0a6336d811599db687da146f1e
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2020
ms.locfileid: "76513190"
---
# <a name="compare-azure-media-services-v3-presets-and-video-indexer"></a>Сравнение предустановок Служб мультимедиа версии 3 и Индексатора видео 

В этой статье сравниваются возможности **API Индексатора видео** и **API Служб мультимедиа (версия 3)** . 

В настоящее время существует перекрытие функций, предоставляемых [API индексатора видео](https://api-portal.videoindexer.ai/) и API- [интерфейсов служб мультимедиа v3](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json). В следующей таблице представлены сведения о текущих различиях и сходстве. 

## <a name="compare"></a>Сравнение

|Функция|API Индексатора видео |Предустановки анализатора видео и звука<br/>в Службах мультимедиа Azure, версия 3|
|---|---|---|
|Аналитические сведения о мультимедиа|[Расширенный](video-indexer-output-json-v2.md) |[Основы](../latest/intelligence-concept.md)|
|Возможности|Ознакомьтесь с полным списком поддерживаемых возможностей в следующей статье: <br/> [Обзор](video-indexer-overview.md)|Возвращает только аналитические сведения о видео|
|Выставление счетов|[Цены на Службы мультимедиа](https://azure.microsoft.com/pricing/details/media-services/#analytics)|[Цены на Службы мультимедиа](https://azure.microsoft.com/pricing/details/media-services/#analytics)|
|Соответствие требованиям|[Iso 27001](https://www.microsoft.com/TrustCenter/Compliance/ISO-IEC-27001), [ISO 27018](https://www.microsoft.com/trustcenter/Compliance/ISO-IEC-27018), [SoC 1, 2, 3](https://www.microsoft.com/TrustCenter/Compliance/SOC), [HIPAA](https://www.microsoft.com/trustcenter/compliance/hipaa), [FedRAMP](https://www.microsoft.com/TrustCenter/Compliance/fedramp), [PCI](https://www.microsoft.com/trustcenter/compliance/pci)и [HITRUST](https://www.microsoft.com/TrustCenter/Compliance/hitrust) Certified. Последние обновления см. в [текущем состоянии сертификатов "индексатор видео](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942)".|Службы мультимедиа соответствует требованиям многих сертификатов. Ознакомьтесь с документом [Azure Compliance Offerings.pdf](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf) (Предложение для соответствия требованиям Azure) и просмотрите сведения о Службах мультимедиа, чтобы узнать, соответствуют ли они требованиям необходимого сертификата.|
|Бесплатное пробное использование|Восточная часть США|Недоступно|
|Доступность по регионам|Восточная часть США 2, Юго-Центральная часть США, Запад 2, Северная Европа, Западная Европа, Юго-Восточная Азия, Восточная Азия и Восточная Австралия.  Последние обновления см. на странице [продукты по регионам](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services) .|Ознакомьтесь со статьей [Доступность продуктов по регионам](https://azure.microsoft.com/global-infrastructure/services/?products=media-services).|

## <a name="next-steps"></a>Дальнейшие действия

[Общие сведения об Индексаторе видео](video-indexer-overview.md)

[Что такое Службы мультимедиа Azure версии 3?](../latest/media-services-overview.md)
