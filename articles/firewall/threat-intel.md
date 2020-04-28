---
title: Фильтрация на основе анализа угроз в брандмауэре Azure
description: Фильтрация на основе Microsoft Threat Intelligence может быть включена в брандмауэре с целю создания оповещений и запрета трафика, поступающего с известных вредоносных IP-адресов и доменов, а также передающегося на них.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/19/2019
ms.author: victorh
ms.openlocfilehash: c291dbe9c1eb37e68174a2353e296a376c7d0896
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2020
ms.locfileid: "74168675"
---
# <a name="azure-firewall-threat-intelligence-based-filtering"></a>Фильтрация на основе интеллектуального анализа угроз в брандмауэре Azure

Фильтрация на основе Microsoft Threat Intelligence может быть включена в брандмауэре с целю создания оповещений и запрета трафика, поступающего с известных вредоносных IP-адресов и доменов, а также передающегося на них. IP-адреса и домены также передаются из канала Microsoft Threat Intelligence. [Intelligent Security Graph](https://www.microsoft.com/en-us/security/operations/intelligence) включает аналитику угроз Майкрософт и используется несколькими службами, включая центр безопасности Azure.

![Аналитика угроз брандмауэра](media/threat-intel/firewall-threat.png)

Если включена фильтрация на основе аналитики угроз, то связанные правила обрабатываются перед любыми правилами NAT, сетевыми правилами или правилами приложения.

Можно выбрать только запись оповещения при срабатывании правила или выбрать режим предупреждения и запрета.

По умолчанию фильтрация на основе аналитики угроз включена в режиме предупреждения. Вы не сможете отключить эту функцию или изменить режим, пока интерфейс портала не станет доступен в вашем регионе.

![Интерфейс портала фильтрации на основе аналитики угроз](media/threat-intel/threat-intel-ui.png)

## <a name="logs"></a>Журналы

В следующем фрагменте журнала показано активированное правило:

```
{
    "category": "AzureFirewallNetworkRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallThreatIntelLog",
    "properties": {
         "msg": "HTTP request from 10.0.0.5:54074 to somemaliciousdomain.com:80. Action: Alert. ThreatIntel: Bot Networks"
    }
}
```

## <a name="testing"></a>Тестирование

- **Исходящее тестирование** — предупреждения исходящего трафика должны быть редкими, так как это означает, что ваша среда была скомпрометирована. Чтобы помочь в проверке работоспособности исходящих оповещений, было создано тестовое полное доменное имя, которое запускает оповещение. Используйте **testmaliciousdomain.eastus.cloudapp.Azure.com** для исходящих тестов.

- **Проверка входящего** трафика. Вы можете видеть предупреждения по входящему трафику, если в брандмауэре настроены правила ДНаТ. Это справедливо, даже если в правиле ДНаТ разрешены только определенные источники, и трафик в противном случае отклоняется. Брандмауэр Azure не предупреждает о всех известных сканерах портов. только для сканеров, которые также известны как вредоносные действия.

## <a name="next-steps"></a>Дальнейшие действия

- См. [примеры log Analytics брандмауэре Azure](log-analytics-samples.md)
- Узнайте, как [развернуть и настроить брандмауэр Azure](tutorial-firewall-deploy-portal.md)
- Обзор [отчета по Microsoft Security Intelligence](https://www.microsoft.com/en-us/security/operations/security-intelligence-report)