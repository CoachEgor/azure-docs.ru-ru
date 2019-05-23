---
title: включение файла
description: включение файла
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/15/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: c779147e464a592d45da8a9a2d8e812320dc23e8
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/23/2019
ms.locfileid: "66162745"
---
<!-- This is the note explaining about the avro and json formats when routing to blob storage. -->
> [!NOTE]
> Данные можно записывать в хранилище BLOB-объектов либо в формате [Apache Avro](https://avro.apache.org/), который выбран по умолчанию, либо в формате JSON (предварительная версия). 
>    
> Возможность кодирования в формате JSON присутствует в предварительной версии для всех регионов, для которых доступен Центр Интернета вещей, за исключением восточной части США, западной части США и Западной Европы. Формат кодирования можно указать только во время настройки конечной точки хранилища BLOB-объектов. Для уже настроенной конечной точки формат изменить нельзя. Если используется кодирование JSON, в свойствах системы обмена сообщениями нужно установить для параметра contentType значение JSON, а для параметра contentEncoding — значение UTF-8. 
>
> Более подробные сведения об использовании конечной точки хранилища BLOB-объектов, см. в [руководстве по маршрутизации в хранилище BLOB-объектов](../articles/iot-hub/iot-hub-devguide-messages-d2c.md#azure-blob-storage).
>