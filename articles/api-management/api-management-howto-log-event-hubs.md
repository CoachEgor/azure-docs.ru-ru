---
title: Как регистрировать события в Центрах событий Azure при использовании службы управления API Azure | Документация Майкрософт
description: Узнайте, как регистрировать события в Центрах событий Azure при использовании службы управления API Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 88f6507d-7460-4eb2-bffd-76025b73f8c4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/29/2018
ms.author: apimpm
ms.openlocfilehash: 2f07f6a27e78ee4df8c64a09918758d02c28c6d4
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2020
ms.locfileid: "76898790"
---
# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Как регистрировать события в Центрах событий Azure при использовании службы управления API Azure
Центры событий Azure — это высокомасштабируемая служба приема данных, которая может обрабатывать миллионы событий в секунду, позволяя вам обрабатывать и анализировать огромное количество данных, создаваемых подключенными устройствами и приложениями. Центры событий выступают в качестве "главного входа"для событий конвейера. После того как данные поступили в концентратор событий, они могут быть преобразованы и сохранены с использованием любого поставщика аналитики в режиме реального времени или адаптера пакетной обработки или хранения. Центры событий отделяют создание потока событий от потребления этих событий, чтобы потребители событий могли обращаться к событиям по собственному расписанию.

Эта статья сопровождает видеоролик [Интеграция службы управления API Azure с Центрами событий](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/). В ней рассматривается регистрация событий управления API с помощью Центров событий Azure.

## <a name="create-an-azure-event-hub"></a>Создание концентратора событий Azure

Подробные инструкции о том, как создать концентратор событий и получить строки подключения, необходимые для отправки событий в концентратор и получения событий из него, см. в статье [Создание пространства имен Центров событий и концентратора событий с помощью портала Azure](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).

## <a name="create-an-api-management-logger"></a>Создание средства ведения журнала для управления API
Теперь, когда концентратор событий создан, нужно настроить [средство ведения журнала](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/logger) в службе управления API, которое сможет регистрировать события в концентраторе событий.

Средства ведения журнала службы управления API настраиваются с помощью [REST API службы управления API](https://aka.ms/apimapi). Подробные примеры запросов см. [в разделе Создание средств ведения журнала](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/logger/createorupdate).

## <a name="configure-log-to-eventhubs-policies"></a>Настройка политик регистрации в концентраторах событий

Если средство ведения журналов в управлении API уже настроено, вы можете настроить политики регистрации в концентраторах событий для регистрации нужных событий. Политику регистрации в концентраторах событий можно использовать в разделе входящей или исходящей политики.

1. Перейдите к экземпляру службы управления API Azure.
2. Выберите вкладку API.
3. Выберите API-интерфейс, для которого требуется добавить политику. В этом примере мы добавляем политику **Echo API** в продукт **Unlimited**.
4. Выберите **Все операции**.
5. В верхней части экрана выберите вкладку "Конструктор".
6. В окне обработки входящих или исходящих запросов щелкните треугольник (рядом со значком карандаша).
7. Выберите редактор кода. Дополнительные сведения см. в статье [Настройка или изменение политик службы управления API Azure](set-edit-policies.md).
8. Наведите указатель мыши на раздел политики `inbound` или `outbound`.
9. В окне справа выберите **Дополнительные политики** > **Log to Event Hub** (Регистрация в концентраторе событий). В результате будет вставлен шаблон инструкции политики `log-to-eventhub`.

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```
Замените `logger-id` значением, которое использовалось на предыдущем шаге для `{new logger name}` в URL-адресе, чтобы создать средство ведения журнала.

Вы можете использовать любое выражение, которое возвращает строку в качестве значения для элемента `log-to-eventhub` . В этом примере регистрируется строка, содержащая дату и время, имя службы, идентификатор запроса, IP-адрес запроса и имя операции.

Нажмите кнопку **Сохранить** , чтобы сохранить конфигурацию обновленной политики. Сразу же после сохранения политика становится активной, а события регистрируются в указанном концентраторе событий.

## <a name="next-steps"></a>Дальнейшие действия
* Дополнительная информация о Центрах событий Azure
  * [Приступая к работе с Центрами событий Azure](../event-hubs/event-hubs-c-getstarted-send.md)
  * [Прием сообщений через EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Руководство по программированию Центров событий](../event-hubs/event-hubs-programming-guide.md)
* Дополнительные сведения об интеграции службы управления API и Центров событий
  * [Справочник по сущности "Средство ведения журнала"](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/logger)
  * [Справочник по политике регистрации в концентраторе событий](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [Мониторинг API-интерфейсов с помощью Управления API Azure, Центров событий и Moesif](api-management-log-to-eventhub-sample.md)  
* Узнайте больше об интеграции с [Azure Application Insights](api-management-howto-app-insights.md).

[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png
