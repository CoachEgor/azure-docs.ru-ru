---
title: Журналы диагностики служебной шины Azure | Документация Майкрософт
description: Узнайте, как настроить журналы диагностики для служебной шины в Azure.
keywords: ''
documentationcenter: .net
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 7d4cb8e55c5d1561c09cf85122550a66e3671f17
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60714138"
---
# <a name="service-bus-diagnostic-logs"></a>Журналы диагностики служебной шины

Для служебной шины Azure можно просмотреть журналы двух типов.
* **[Журналы действий](../azure-monitor/platform/activity-logs-overview.md)** . Эти журналы содержат сведения об операциях, выполненных с заданием. Данные журналы всегда включены.
* **[Журналы диагностики](../azure-monitor/platform/diagnostic-logs-overview.md)** . Вы можете настроить журналы диагностики, чтобы получать более подробные сведения обо всем, что происходит с заданием. Журналы диагностики охватывают действия с момента создания задания до его удаления, включая обновления и действия, которые происходят во время выполнения задания.

## <a name="turn-on-diagnostic-logs"></a>Включение журналов диагностики

По умолчанию журналы диагностики отключены. Чтобы включить ведение журналов диагностики, выполните следующее.

1.  На [портале Azure](https://portal.azure.com) в разделе **Мониторинг и управление** щелкните **Журналы диагностики**.

    ![Перемещение к колонке журналов диагностики](./media/service-bus-diagnostic-logs/image1.png)

2. Выберите ресурс, который необходимо отслеживать.  

3.  Щелкните **Включить диагностику**.

    ![Включение журналов диагностики](./media/service-bus-diagnostic-logs/image2.png)

4.  Для параметра **Состояние** щелкните **Вкл**.

    ![Изменение состояния журналов диагностики](./media/service-bus-diagnostic-logs/image3.png)

5.  Задайте нужный; цель для архивирования например журналов учетной записи хранения, концентратор событий или Azure Monitor.

6.  Сохраните новые параметры диагностики.

Новые параметры вступят в силу в течение 10 минут. После этого журналы появятся в настроенной цели для архивирования на вкладке **Журналы диагностики**.

Дополнительные сведения о настройке системы диагностики доступны в [обзоре журналов диагностики Azure](../azure-monitor/platform/diagnostic-logs-overview.md).

## <a name="diagnostic-logs-schema"></a>Схема журналов диагностики

Все журналы хранятся в формате JSON (нотация объектов JavaScript). Каждая запись содержит строковые поля, использующие формат, описанный в следующем разделе.

## <a name="operational-logs-schema"></a>Схема операционных журналов

В журналах в категории **OperationalLogs** регистрируется все, что происходит во время работы служебной шины. В частности, в эти журналы записывается тип операции, включая создание очереди, используемые ресурсы и состояние операции.

Строки JSON операционного журнала содержат элементы, перечисленные в приведенной ниже таблице.

ИМЯ | ОПИСАНИЕ
------- | -------
ActivityId | Внутренний идентификатор, используемый для отслеживания
EventName | Имя операции           
resourceId | Идентификатор ресурса Azure Resource Manager
SubscriptionId | Идентификатор подписки
EventTimeString | Время операции
EventProperties | Свойства операции
Status | Состояние операции
Caller | Объект, вызвавший операцию (портал Azure или клиент управления)
category | OperationalLogs

Ниже приведен пример строки JSON операционного журнала.

```json
{
  "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
  "EventName": "Create Queue",
  "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/SHOEBOXEHNS-CY4001",
  "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
  "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
  "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
  "Status": "Succeeded",
  "Caller": "ServiceBus Client",
  "category": "OperationalLogs"
}
```

## <a name="next-steps"></a>Дальнейшие действия

Чтобы узнать больше о служебной шине, перейдите по следующим ссылкам:

* [Основные сведения о служебной шине](service-bus-messaging-overview.md)
* [Приступая к работе со служебной шиной](service-bus-dotnet-get-started-with-queues.md)
