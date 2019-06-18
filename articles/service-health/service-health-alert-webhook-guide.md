---
title: Настройка уведомлений о работоспособности службы Azure для существующих систем управления проблемами, с помощью веб-перехватчика
description: Передача персонализированных уведомлений о событиях работоспособности служб в существующую систему управления проблемами.
author: stephbaron
ms.author: stbaron
ms.topic: conceptual
ms.service: service-health
ms.workload: Supportability
ms.date: 3/27/2018
ms.openlocfilehash: 3e9a564310d750e63c9cf81d19f698e75712d09a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67067177"
---
# <a name="configure-health-notifications-for-existing-problem-management-systems-using-a-webhook"></a>Настройка уведомлений о работоспособности для существующих систем управления проблемами с помощью веб-перехватчика

В этой статье показано, как настроить оповещения о работоспособности служб для отправки данных через веб-перехватчики в существующую систему уведомлений.

Сейчас можно настроить оповещения о работоспособности службы таким образом, чтобы в случае, если вас затронет инцидент со службой Azure, вы получили уведомление в сообщении SMS или по электронной почте.
Однако у вас уже может быть установлена внешняя система уведомлений, которую вы хотите использовать.
В этом документе показаны наиболее важные части полезных данных веб-перехватчика, а также способы создания пользовательских оповещений для получения уведомлений в случае, если вас затронут проблемы со службами.

Если вы хотите использовать предварительно настроенную интеграцию, см. следующие инструкции:
* [Настройка оповещений с помощью ServiceNow](service-health-alert-webhook-servicenow.md).
* [Настройка оповещений с помощью PagerDuty](service-health-alert-webhook-pagerduty.md).
* [Настройка оповещений с помощью OpsGenie](service-health-alert-webhook-opsgenie.md).

### <a name="watch-an-introductory-video"></a>Просмотрите видео с вводной информацией

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUV]

## <a name="configuring-a-custom-notification-using-the-service-health-webhook-payload"></a>Настройка пользовательского уведомления с помощью полезных данных веб-перехватчика работоспособности служб
Если вы хотите настроить собственную интеграцию пользовательского веб-перехватчика, необходимо проанализировать полезные данные JSON, которые отправляются во время уведомлений о работоспособности служб.

Просмотрите [пример](../azure-monitor/platform/activity-log-alerts-webhook.md) того, как выглядят полезные данные веб-перехватчика `ServiceHealth`.

Оповещение о работоспособности службы можно распознать по `context.eventSource == "ServiceHealth"`. Наиболее значимые свойства для приема:
 * `data.context.activityLog.status`
 * `data.context.activityLog.level`
 * `data.context.activityLog.subscriptionId`
 * `data.context.activityLog.properties.title`
 * `data.context.activityLog.properties.impactStartTime`
 * `data.context.activityLog.properties.communication`
 * `data.context.activityLog.properties.impactedServices`
 * `data.context.activityLog.properties.trackingId`

## <a name="creating-a-direct-link-to-the-service-health-dashboard-for-an-incident"></a>Создание прямой ссылки на панель мониторинга "Работоспособность служб" для инцидента
Можно добавить прямую ссылку на панель мониторинга "Работоспособность служб" на рабочем столе или мобильном устройстве, создав специализированный URL-адрес. Используйте `trackingId`, а также первую и последние три цифры вашего `subscriptionId`, чтобы создать такой адрес:
```
https://app.azure.com/h/<trackingId>/<first and last three digits of subscriptionId>
```

Например если `subscriptionId` = `bba14129-e895-429b-8809-278e836ecdb3`, а `trackingId` = `0DET-URB`, то URL-адресом панели мониторинга "Работоспособность служб" будет:

```
https://app.azure.com/h/0DET-URB/bbadb3
```

## <a name="using-the-level-to-detect-the-severity-of-the-issue"></a>Использование уровня для выявления серьезности проблемы
Свойство `level` в полезных данных может иметь любое из значений — `Informational`, `Warning`, `Error` или `Critical` — в порядке от самой низкой серьезности к самой высокой.

## <a name="parsing-the-impacted-services-to-understand-the-full-scope-of-the-incident"></a>Анализ затронутых служб для определения полного масштаба инцидента
Оповещения о работоспособности служб могут информировать вас о проблемах, затрагивающих несколько регионов и служб. Чтобы получить полные сведения, необходимо проанализировать значение `impactedServices`.
Внутреннее содержимое представляет собой [экранированную строку JSON](https://json.org/). После разэкранирования она будет содержать еще один объект JSON, который можно проанализировать обычным образам.

```json
{"data.context.activityLog.properties.impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"Australia East\"},{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"Alerts & Metrics\"},{\"ImpactedRegions\":[{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"App Service\"}]"}
```

превращается в

```json
[
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia East"
         },
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"Alerts & Metrics"
   },
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"App Service"
   }
]
```

Это указывает на наличие проблем со службой Alerts & Metrics (Оповещения и метрики) в Восточной и Юго-Восточной Австралии, а также проблем со службой App Service (Служба приложений) в Юго-Восточной Австралии.


## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Проверка интеграции с веб-перехватчиком с помощью запроса HTTP POST
1. Создайте полезные данные о работоспособности служб, которые хотите отправить. Пример полезных данных для веб-перехватчика службы работоспособности служб см. в статье [Веб-перехватчики для оповещений журнала действий Azure](../azure-monitor/platform/activity-log-alerts-webhook.md).

2. Создайте запрос HTTP POST следующим образом:

    ```
    POST        https://your.webhook.endpoint

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
3. Вы должны получить ответ `2XX - Successful`.

4. Откройте [PagerDuty](https://www.pagerduty.com/) и убедитесь, что интеграция настроена успешно.

## <a name="next-steps"></a>Дальнейшие действия
- Просмотрите схему веб-перехватчика оповещений журнала действий в статье [Объекты webhook для оповещений журнала действий Azure](../azure-monitor/platform/activity-log-alerts-webhook.md). 
- Дополнительные сведения об уведомлениях о работоспособности службы см. в [этой статье](../azure-monitor/platform/service-notifications.md).
- Дополнительные сведения о группах действий см. в статье [Создание групп действий и управление ими на портале Azure](../azure-monitor/platform/action-groups.md).