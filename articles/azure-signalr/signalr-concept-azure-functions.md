---
title: Создание приложений реального времени с помощью функций Azure и служба Azure SignalR
description: Сведения об использовании служба Azure SignalR в безсерверных приложениях.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: c88e2b012086f61aad3143c8fe6b0bd25ec333e5
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275092"
---
# <a name="build-real-time-apps-with-azure-functions-and-azure-signalr-service"></a>Создание приложений реального времени с помощью функций Azure и служба Azure SignalR

Так как службы Azure SignalR и "Функции Azure" являются полностью управляемыми высокомасштабируемыми службами, которые позволяют сосредоточиться на создании приложений вместо управления инфраструктурой, часто используют обе службы, чтобы обеспечить обмен данными в реальном времени в [беcсерверной](https://azure.microsoft.com/solutions/serverless/) среде.

> [!NOTE]
> Узнайте, как совместно использовать SignalR и функций Azure интерактивного учебника [включить автоматическое обновление веб-приложения с помощью функций Azure и служба SignalR](https://docs.microsoft.com/learn/modules/automatic-update-of-a-webapp-using-azure-functions-and-signalr).

## <a name="integrate-real-time-communications-with-azure-services"></a>Интеграция служб Azure в режиме реального времени

Функции Azure позволяют писать код на [нескольких языках](../azure-functions/supported-languages.md), включая JavaScript, C# и Java, который запускается каждый раз, когда происходят события в облаке. Примеры этих событий.

* Запросы HTTP и веб-перехватчика.
* Периодические таймеры.
* События из служб Azure, такие как:
    - Сетка событий Azure
    - Центры событий;
    - Служебная шина Azure
    - канал изменений Cosmos DB;
    - хранилище: большие двоичные объекты и очереди;
    - соединители Logic Apps, например Salesforce и SQL Server.

Используя службу "Функции Azure" для интеграции этих событий с помощью службы Azure SignalR, вы можете уведомлять тысячи клиентов о каждом событии.

Некоторые распространенные сценарии беcсерверной передачи сообщений в режиме реального времени, которые можно реализовать с помощью служб "Функции Azure" и SignalR, включают следующее.

* Визуализация телеметрии устройства IoT на панели управления в реальном времени или на карте
* Обновление данных в приложении при обновлении документов в Cosmos DB
* Отправка уведомлений в приложении при создании новых заказов в Salesforce

## <a name="signalr-service-bindings-for-azure-functions"></a>Привязки службы SignalR для службы "Функции Azure"

Привязки службы SignalR для службы "Функции Azure" позволяют приложению "Функции Azure" публиковать сообщения клиентов, подключенных к службе SignalR. В ближайшее время клиенты смогут подключаться к службе с помощью пакета SDK клиента SignalR, доступного на языках .NET, JavaScript и Java, а вскоре и с большим количеством языков.

### <a name="an-example-scenario"></a>Пример сценария

Пример использования привязок службы SignalR — это использование службы "Функции Azure" для интеграции с Azure Cosmos DB и службой SignalR для отправки сообщений в режиме реального времени, когда появляются новые события в канале изменения Cosmos DB.

![Cosmos DB, служба "Функции Azure", служба SignalR](media/signalr-concept-azure-functions/signalr-cosmosdb-functions.png)

1. Изменения, сделанные в коллекции Cosmos DB
2. Событие изменения распространяется на канал изменений Cosmos DB
3. Функции Azure активированы событием изменения с помощью триггера Cosmos DB
4. Выходная привязка службы SignalR публикует сообщение службе SignalR
5. Служба SignalR публикует сообщение всем подключенным клиентам

### <a name="authentication-and-users"></a>Проверка подлинности и пользователи

Служба SignalR позволяет рассылать широковещательные сообщения для всех клиентов или только для подмножества клиентов, например тех, которые принадлежат одному пользователю. Привязки службы SignalR для службы "Функции Azure" можно объединить с Проверкой подлинности службы приложений для проверки подлинности пользователей с помощью поставщиков, например Azure Active Directory, Facebook и Twitter. Затем вы можете отправлять сообщения непосредственно этим пользователям, прошедшим проверку подлинности.

## <a name="next-steps"></a>Дальнейшие действия

В этой статье вы получили общие сведения об использовании службы "Функции Azure" со службой SignalR для реализации широкого спектра бессерверных сценариев обмена сообщениями в реальном времени.

Дополнительные сведения о том, как совместное использование функций Azure и служба SignalR посетить следующие ресурсы:

* [Функции разработки и конфигурации с помощью SignalR службы Azure](signalr-concept-serverless-development-config.md)
* [Включить автоматическое обновление веб-приложения с помощью функций Azure и служба SignalR](https://docs.microsoft.com/learn/modules/automatic-update-of-a-webapp-using-azure-functions-and-signalr)

Дополнительные сведения см. в следующих кратких руководствах.

* [Quickstart: Create a chat room with Azure Functions and SignalR Service using C#](signalr-quickstart-azure-functions-csharp.md) (Создание чат-комнаты с помощью служб "Функции Azure" и SignalR с использованием C#)
* [Quickstart: Create a chat room with Azure Functions and SignalR Service using JavaScript](signalr-quickstart-azure-functions-javascript.md) (Создание чат-комнаты с помощью служб "Функции Azure" и SignalR с использованием JavaScript)