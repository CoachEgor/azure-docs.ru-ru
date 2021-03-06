---
title: Подключение стандартных данных от VMware для конечной точки "черная" в Azure Sentinel | Документация Майкрософт
description: Узнайте, как подключить стандартные данные VMware для конечной точки "Черная Cloud" к Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2020
ms.author: yelevin
ms.openlocfilehash: e13b251c9bd95a5b52d63d8ea1bbf265c9c46fd8
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96023880"
---
# <a name="connect-your-vmware-carbon-black-cloud-endpoint-standard-to-azure-sentinel-with-azure-function"></a>Подключение стандарта VMware для облачной конечной точки к Azure Sentinel с помощью функции Azure

> [!IMPORTANT]
> Стандартный соединитель данных для конечной точки VMware "черная" в Azure Sentinel в настоящее время находится в общедоступной предварительной версии.
> Эта функция предоставляется без соглашения об уровне обслуживания и не рекомендуется для рабочих нагрузок. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Соединитель стандартных конечных точек "VMware — черный облако" позволяет легко подключать все журналы решений безопасности для [стандартных конечных точек VMware](https://www.carbonblack.com/products/endpoint-standard/) и "Стандартный" с помощью Azure Sentinel, просматривать панели мониторинга, создавать пользовательские оповещения и улучшать исследование. Интеграция между VMware и черным Sentinel Azure использует функции Azure для извлечения данных журнала с помощью REST API.


> [!NOTE]
> Данные будут храниться в географическом расположении рабочей области, на которой вы используете метку Azure.

## <a name="configure-and-connect-vmware-carbon-black-cloud-endpoint-standard"></a>Настройка и подключение облачной конечной точки VMware "черная"

Функции Azure могут интегрировать и запрашивать события и журналы непосредственно в стандарте VMware "Черная облачная конечная точка" и пересылать их в Azure Sentinel.

1. На портале Sentinel Azure щелкните **соединители данных** и выберите **VMware углекислый** соединитель.
2. Выберите **открыть страницу соединителя**.
3. Следуйте инструкциям на странице **VMware углекислый черный цвет** .


## <a name="find-your-data"></a>Поиск данных

После установки успешного подключения данные появляются в Log Analytics под **CarbonBlackAuditLogs_CL**, **CarbonBlackNotifications_CL** и * * * * CarbonBlackEvents_CL * * * * таблицах.

## <a name="validate-connectivity"></a>Проверка подключения
Если журналы начнут появляться в Log Analytics, это может занять до 20 минут. 


## <a name="next-steps"></a>Следующие шаги
В этом документе вы узнали, как подключить стандартную облачную конечную точку VMware для Azure к Sentinel с помощью приложений-функций Azure. Ознакомьтесь с дополнительными сведениями об Azure Sentinel в соответствующих статьях.
- Узнайте, как [отслеживать свои данные и потенциальные угрозы](quickstart-get-visibility.md).
- Узнайте, как приступить к [обнаружению угроз с помощью Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Используйте книги](tutorial-monitor-your-data.md) для мониторинга данных.

