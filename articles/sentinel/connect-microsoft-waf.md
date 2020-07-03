---
title: Подключение данных брандмауэра веб-приложения к Azure Sentinel
description: Узнайте, как подключить данные брандмауэра веб-приложения Майкрософт к Azure Sentinel.
author: yelevin
manager: rkarlin
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: a5cef16694fa2cfae036152d22cfa4473956fc72
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77588184"
---
# <a name="connect-data-from-microsoft-web-application-firewall"></a>Подключение данных из брандмауэра веб-приложения Майкрософт



Вы можете выполнять потоковую передачу журналов из шлюза приложений Azure через брандмауэр веб-приложения Microsoft (WAF). Эта WAF защищает приложения от распространенных веб-уязвимостей, таких как внедрение кода SQL и межсайтовые сценарии, и позволяет настраивать правила для сокращения числа ложных срабатываний. Выполните эти инструкции, чтобы выполнить потоковую передачу журналов брандмауэра веб-приложения Майкрософт в Azure Sentinel.


## <a name="prerequisites"></a>Предварительные условия

- Существующий ресурс шлюза приложений

## <a name="connect-to-microsoft-web-application-firewall"></a>Подключение к брандмауэру веб-приложения Майкрософт

Если у вас уже есть брандмауэр веб-приложения Майкрософт, убедитесь, что у вас есть имеющийся ресурс шлюза.
После развертывания и получения данных брандмауэром веб-приложения Майкрософт данные предупреждений можно легко передать в Azure Sentinel.
    
1. На портале Sentinel Azure выберите **соединители данных**.
1. На странице соединители данных выберите плитку **WAF** .
1. Перейдите к [ресурсу шлюза приложений](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAllResourcesBlade/resourceType/Microsoft.Network%2FapplicationGateways)и выберите свой WAF.
    1. Выберите **Параметры диагностики**.
    1. Выберите **+ Добавить параметр диагностики** в таблице.
    1. На странице **параметры диагностики** введите **имя** и выберите **отправить в log Analytics**.
    1. В разделе **log Analytics Рабочая область** выберите рабочую область "Sentinel Azure".
    1. Выберите типы журналов, которые необходимо проанализировать. Рекомендуется: ApplicationGatewayAccessLog и ApplicationGatewayFirewallLog.
1. Чтобы использовать соответствующую схему в Log Analytics для оповещений брандмауэра веб-приложения Майкрософт, выполните поиск по запросу **AzureDiagnostics**.

## <a name="next-steps"></a>Дальнейшие шаги
В этом документе вы узнали, как подключить брандмауэр веб-приложения Майкрософт к Azure Sentinel. Ознакомьтесь с дополнительными сведениями об Azure Sentinel в соответствующих статьях.
- Узнайте, как [отслеживать свои данные и потенциальные угрозы](quickstart-get-visibility.md).
- Узнайте, как приступить к [обнаружению угроз с помощью Azure Sentinel](tutorial-detect-threats-built-in.md).
