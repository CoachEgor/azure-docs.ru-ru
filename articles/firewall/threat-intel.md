---
title: Фильтрация на основе анализа угроз Azure брандмауэра
description: Дополнительные сведения о фильтрации аналитики угроз брандмауэр Azure
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 3/11/2019
ms.author: victorh
ms.openlocfilehash: 4ef9089c94d9e806cc519c4f8243cdcb7e73953a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60194053"
---
# <a name="azure-firewall-threat-intelligence-based-filtering---public-preview"></a>Фильтрация на основе аналитики угроз Azure брандмауэра - общедоступной предварительной версии

Фильтрация на основе Microsoft Threat Intelligence может быть включена в брандмауэре с целю создания оповещений и запрета трафика, поступающего с известных вредоносных IP-адресов и доменов, а также передающегося на них. IP-адреса и домены также передаются из канала Microsoft Threat Intelligence. [Intelligent Security Graph](https://www.microsoft.com/en-us/security/operations/intelligence) лежащих в основе анализа угроз Майкрософт и используется несколько служб, включая Центр безопасности Azure.

![Анализ угроз брандмауэра](media/threat-intel/firewall-threat.png)

> [!IMPORTANT]
> Фильтрация на основе аналитики угроз в настоящее время находится в общедоступной предварительной версии и предоставляется с соглашением об уровне обслуживания предварительной версии. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены.  См. [дополнительные условия использования для предварительных версий Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Если включена фильтрация угроз на основе аналитики, связанные правила обрабатываются перед любой из правил NAT, правила сети и правил приложения. На этапе предварительной версии будут включены только самый высокий достоверности записи.

Вы можете просто войти оповещение при запуске правила, или выберите оповещение и запретить режим.

По умолчанию фильтрация угроз на основе аналитики включается в режиме оповещения. Нельзя отключить эту функцию или изменить режим, пока интерфейс портала станет доступной в вашем регионе.

![Анализ угроз на основе фильтрации интерфейс портала](media/threat-intel/threat-intel-ui.png)

## <a name="logs"></a>Журналы

В следующем фрагменте журнала показаны активированные правила:

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

- **Исходящие тестирования** — оповещения об исходящем трафике должно быть редко, так как это означает, что среды был скомпрометирован. Чтобы помочь работаете исходящих оповещения теста, теста создания полное доменное имя, которое инициирует оповещение. Используйте **testmaliciousdomain.eastus.cloudapp.azure.com** для исходящего трафика тестов.

- **Входящие тестирования** -можно ожидать увидеть предупреждения на входящий трафик, если DNAT правила настраиваются в брандмауэре. Это верно, даже в том случае, если в правиле DNAT допускаются только определенных источников и трафик запрещается. в противном случае. Брандмауэр Azure не выводит предупреждения для всех сканеров порт; только на сканерами, которые известны также принять участие в вредоносных действий.

## <a name="next-steps"></a>Дальнейшие действия

- См. в разделе [примеры анализа журнала брандмауэра Azure](log-analytics-samples.md)
- Узнайте, как [развернуть и настроить брандмауэр подключения к Azure](tutorial-firewall-deploy-portal.md)
- Просмотрите [отчет службы безопасности Майкрософт](https://www.microsoft.com/en-us/security/operations/security-intelligence-report)