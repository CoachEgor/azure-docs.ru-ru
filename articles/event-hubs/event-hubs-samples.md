---
title: Примеры кодов для Центров событий Azure | Документация Майкрософт
description: В этой статье приведен список примеров кодов для Центров событий Azure, доступных на сайте GitHub.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.custom: seodec18
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 1c1198733fb56303d328ee97152442d25dbe945a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60343483"
---
# <a name="git-repositories-with-samples-for-azure-event-hubs"></a>Репозитории Git с примерами кодов для Центров событий Azure 
Примеры концентраторов событий можно найти на [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples). В примерах продемонстрированы основные функции [Центров событий Azure](/azure/event-hubs/). В этой статье приведены категории примеров с описаниями и ссылками.

## <a name="net-samples"></a>Примеры для .NET

| Имя примера | Описание | 
| ----------- | ----------- | 
| [SampleSender](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) | В этом примере показано, как написать консольное приложение для .NET Core, которое отправляет набор событий в концентратор событий. |
| [SampleEHReceiver](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) | В этом примере показано, как создать консольное приложение .NET Core для получения набора событий из концентратора событий с помощью библиотеки узла обработчика событий.  | 

## <a name="java-samples"></a>Примеры для Java

| Имя примера | Описание | 
| ----------- | ----------- | 
| [SendBatch](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SendBatch)  | В этом примере показано, как принимать пакеты событий в концентратор событий. | 
| [SimpleSend](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend) | В этом примере показано, как принимать события в концентратор событий. |
| [AdvanceSendOptions](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/AdvancedSendOptions) | Этот пример иллюстрирует различные параметры, доступные для приема событий Центрами событий. |
| [ReceiveByDateTime](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveByDateTime) | В этом примере показано, как получать события из раздела концентратора событий с использованием определенного смещения даты и времени. |
| [ReceiveUsingOffset](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveUsingOffset) | В этом примере показано, как получать события из раздела концентратора событий с использованием определенного смещения даты. |  
| [ReceiveUsingSequenceNumber](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveUsingSequenceNumber) | В этом примере показано, как получать события из разделов концентратора событий, используя порядковый номер. |   
| [EventProcessorSample](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/EventProcessorSample) |В этом примере показано, как получать события из концентратора событий, используя узел обработчика событий, который обеспечивает автоматический выбор разделов и переключение между несколькими параллельными приемниками. | 
| [AutoScaleOnIngress](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Benchmarks/AutoScaleOnIngress) | В этом примере показано, как концентратор событий может автоматически масштабироваться при высоких нагрузках. Пример отправляет события со скоростью, которая превышает установленную скорость концентратора событий, в результате чего концентратор событий будет увеличивать масштаб. | 
| [IngressBenchmark](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Benchmarks/IngressBenchmark) | Этот пример позволяет измерять скорость приема. | 

## <a name="python-samples"></a>Примеры для Python
Можно просмотреть примеры Python для концентраторов событий Azure в репозитории GitHub [azure-event-hubs-python](https://github.com/Azure/azure-event-hubs-python/tree/master/examples).

## <a name="nodejs-samples"></a>Примеры для Node.js
Примеры Node.js можно найти для концентраторов событий Azure в [azure sdk для js](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples) репозитория GitHub.

## <a name="go-samples"></a>Примеры Go
Можно просмотреть примеры Go для концентраторов событий Azure в репозитории GitHub [azure-event-hubs-go](https://github.com/Azure/azure-event-hubs-go/tree/master/_examples).

## <a name="azure-cli-samples"></a>Примеры Azure CLI
Можно просмотреть примеры Azure CLI для концентраторов событий Azure в репозитории GitHub [azure-event-hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples/Management/CLI).

## <a name="azure-powershell-samples"></a>Примеры сценариев Azure PowerShell
Можно просмотреть примеры Azure PowerShell для концентраторов событий Azure в репозитории GitHub [azure-event-hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples/Management/PowerShell).
 
## <a name="apache-kafka-samples"></a>Примеры для Apache Kafka
Примеры использования функции "Центры событий для Apache Kafka" можно найти в репозитории GitHub [azure-event-hubs-for-kafka](https://github.com/Azure/azure-event-hubs-for-kafka).

## <a name="next-steps"></a>Дальнейшие действия
Чтобы узнать больше о Центрах событий, обратитесь к следующим статьям:

- [Общие сведения о Центрах событий](event-hubs-what-is-event-hubs.md)
- [Особенности Центров событий](event-hubs-features.md)
- [Часто задаваемые вопросы о Центрах событий](event-hubs-faq.md)