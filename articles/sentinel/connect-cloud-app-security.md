---
title: Подключение Cloud App Security данных к Azure Sentinel | Документация Майкрософт
description: Узнайте, как подключить данные Cloud App Security к Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/23/2019
ms.author: yelevin
ms.openlocfilehash: 348576fbbdd1037f9e2e792218b96bbbecf36668
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/25/2020
ms.locfileid: "77588371"
---
# <a name="connect-data-from-microsoft-cloud-app-security"></a>Подключение данных из Microsoft Cloud App Security 



Вы можете выполнять потоковую передачу журналов из [Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) в метку Azure одним щелчком мыши. Это подключение позволяет передавать оповещения из Cloud App Security в метку Azure. 

## <a name="prerequisites"></a>Предварительные требования

- Пользователь с разрешениями глобального администратора или администратора безопасности
- Чтобы выполнить потоковую передачу журналов Cloud Discovery в Azure Sentinel, [включите метку Azure в качестве SIEM в Microsoft Cloud App Security](https://aka.ms/AzureSentinelMCAS).

> [!IMPORTANT]
> Прием журналов Cloud Discovery в настоящее время находится в общедоступной предварительной версии.
> Эта функция предоставляется без соглашения об уровне обслуживания и не рекомендуется для рабочих нагрузок.
> Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
## <a name="connect-to-cloud-app-security"></a>Подключение к Cloud App Security

Если у вас уже есть Cloud App Security, убедитесь, что он [включен в сети](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security).
Если Cloud App Security развернут и принимает данные, данные предупреждений можно легко передать в Azure Sentinel.


1. В поле Sentinel Azure выберите **соединители данных**, щелкните плитку **Cloud App Security** и выберите **открыть страницу соединителя**.

1. Выберите журналы для потоковой передачи в Azure Sentinel. Вы можете выбрать **оповещения** и **Cloud Discovery журналы** (Предварительная версия). 

1. Нажмите кнопку **Соединить**.

1. Чтобы использовать соответствующую схему в Log Analytics для Cloud App Security предупреждений, выполните поиск по запросу **секуритялерт**.




## <a name="next-steps"></a>Следующие шаги
В этом документе вы узнали, как подключить Microsoft Cloud App Security к Azure Sentinel. Ознакомьтесь с дополнительными сведениями об Azure Sentinel в соответствующих статьях.
- Узнайте, как [отслеживать свои данные и потенциальные угрозы](quickstart-get-visibility.md).
- Узнайте, как приступить к [обнаружению угроз с помощью Azure Sentinel](tutorial-detect-threats.md).
