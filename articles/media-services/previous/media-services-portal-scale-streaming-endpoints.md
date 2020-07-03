---
title: Масштабирование конечных точек потоковой передачи с помощью портала Azure | Документация Майкрософт
description: В этом учебнике пошагово описано, как масштабировать конечные точки потоковой передачи с помощью портала Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 1008b3a3-2fa1-4146-85bd-2cf43cd1e00e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 40820170bae275f090c5f898387698fc562e59ae
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80985547"
---
# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Масштабирование конечных точек потоковой передачи с помощью портала Azure
## <a name="overview"></a>Обзор

> [!NOTE]
> Для работы с этим учебником требуется учетная запись Azure. Дополнительные сведения см. в статье [Бесплатная пробная версия Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

Конечные точки уровня **Премиум** подходят для выполнения более сложных задач благодаря выделенной пропускной способности и возможности масштабирования. Клиенты, имеющие конечную точку потоковой передачи уровня **Премиум**, по умолчанию получают одну единицу потоковой передачи. Конечную точку потоковой передачи можно масштабировать, добавляя единицы потоковой передачи. Каждая единица потоковой передачи предоставляет дополнительную пропускную способность для приложения. Дополнительные сведения о типах конечных точек потоковой передачи и конфигурации сети CDN см. в статье [Управление конечными точками потоковой передачи с помощью портала Azure](media-services-streaming-endpoints-overview.md).
 
В этом разделе показано, как масштабировать конечную точку потоковой передачи.

Дополнительные сведения о ценах на службы мультимедиа см. [здесь](https://azure.microsoft.com/pricing/details/media-services/).

## <a name="scale-streaming-endpoints"></a>Масштабирование конечных точек потоковой передачи

Чтобы изменить число единиц потоковой передачи, сделайте следующее.

1. Выберите учетную запись служб мультимедиа Azure на [портале Azure](https://portal.azure.com/).
2. В окне **Параметры** щелкните**Конечные точки потоковой передачи**.
3. Щелкните конечную точку потоковой передачи, которую требуется масштабировать. 

    > [!NOTE] 
    > Масштабировать можно только конечные точки потоковой передачи уровня **Премиум**.

4. Переместите ползунок, чтобы указать нужное число единиц потоковой передачи.

    ![конечная точка потоковой передачи](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

## <a name="next-steps"></a>Дальнейшие шаги
Просмотрите схемы обучения работе со службами мультимедиа.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Предоставление отзыва
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

