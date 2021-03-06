---
title: Ограничения и квоты. Пользовательская служба визуального распознавания
titleSuffix: Azure Cognitive Services
description: В этой статье описываются различные типы лицензионных ключей, а также ограничения и квоты для Пользовательская служба визуального распознавания.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: pafarley
ms.openlocfilehash: b79bf5e8ead16bbdf9c69e8d7faae43fa778ab3e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91316585"
---
# <a name="limits-and-quotas"></a>Ограничения и квоты

Существует два уровня ключей для службы Пользовательское визуальное распознавание. Вы можете зарегистрировать подписку F0 (бесплатная) или S0 (стандартная) на портале Azure. Дополнительные сведения о ценах и транзакциях см. на соответствующей [странице с ценами на Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/).

Число учебных изображений на проект и число тегов на проект для проектов категории S0 со временем будет увеличено.

|Фактор|**F0**|**S0**|
|-----|-----|-----|
|Проекты|2|100|
|Число учебных изображений на проект |5 000|100 000|
|Прогнозы в месяц|10 000 |Неограниченно|
|Теги/проект|50|500|
|Количество итераций |10|10|
|Минимальное число помеченных изображений на тег, классификация (рекомендуется 50) |5|5|
|Минимальное число помеченных изображений на тег, обнаружение объектов (рекомендуется 50)|15|15|
|Срок хранения прогнозных изображений|30 дней|30 дней|
|Число операций [Prediction](https://go.microsoft.com/fwlink/?linkid=865445) с хранилищем (транзакций в секунду)|2|10|
|Число операций [Prediction](https://go.microsoft.com/fwlink/?linkid=865445) без хранилища (транзакций в секунду)|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (число вызовов API в секунду)|2|10|
|[Другие вызовы API](https://go.microsoft.com/fwlink/?linkid=865446) (число транзакций в секунду)|10|10|
|Принятые типы изображений|JPG, PNG, BMP, GIF|JPG, PNG, BMP, GIF|
|Минимальная высота или ширина изображения в пикселях|256 (см. Примечание)|256 (см. Примечание)|
|Максимальная высота или ширина изображения в пикселях|10 240|10 240|
|Максимальный размер изображения (при передаче учебного изображения) |6 МБ|6 МБ|
|Максимальный размер изображения (для прогнозирования)|4 МБ|4 МБ|
|Образ обучения для обнаружения объектов (максимальное число регионов на объект)|300|300|
|Максимальное число тегов на образ классификации|100|100|

> [!NOTE]
> Изображения размером менее 256 пикселей будут приняты, но были бы масштабированы.
> Пропорции изображения не должны быть больше 25
