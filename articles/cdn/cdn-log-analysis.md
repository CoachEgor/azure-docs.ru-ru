---
title: Анализ вариантов использования Azure CDN | Документация Майкрософт
description: В этой статье описываются разные типы аналитических отчетов, доступные для продуктов Azure CDN.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: allensu
ms.openlocfilehash: c78dff9f5258023d90100c70bd244e8e0d016d6f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81253617"
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Анализ вариантов использования CDN Azure

Когда вы включите CDN для вашего приложения, вам, скорее всего, нужно будет отслеживать использование CDN, проверять работоспособность службы доставки и устранять возможные неполадки. Azure CDN позволяет делать это следующим образом. 

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Базовая аналитика с использованием журналов диагностики Azure

Эта функция доступна на конечных точках CDN всех ценовых категорий. Журналы диагностики Azure позволяют экспортировать основную аналитику в службу хранилища Azure, концентраторы событий или журналы Azure Monitor. Журналы Azure Monitor предлагают решение с настраиваемыми и настраиваемыми графами. См. дополнительные сведения о [журналах диагностики Azure](cdn-azure-diagnostic-logs.md).

## <a name="verizon-core-reports"></a>С помощью базовых отчетов Verizon

Как пользователь с Azure CDN с профилем Verizon **Стандартный** или **Премиум** вы можете просматривать базовые отчеты Verizon на вспомогательном портале Verizon. Verizon основные отчеты доступны через параметр **управления** в портал Azure и предлагает разнообразные графики и представления. См. дополнительные сведения о [базовых отчетах Verizon](cdn-analyze-usage-patterns.md).

## <a name="verizon-custom-reports"></a>Настраиваемые отчеты Verizon

Как пользователь с Azure CDN с профилем Verizon **Стандартный** или **Премиум** вы можете просматривать настраиваемые отчеты Verizon на вспомогательном портале Verizon. Пользовательские отчеты Verizon доступны через параметр **управления** из портал Azure. На странице настраиваемых отчетов Verizon отображается число попаданий или данных, передаваемых для каждой граничной записи CName, принадлежащей профилю Azure CDN. Данные можно группировать по HTTP-коду ответа или состоянию кэша за любой период времени. См. дополнительные сведения о [настраиваемых отчетах Verizon](cdn-verizon-custom-reports.md).

## <a name="azure-cdn-premium-from-verizon-reports"></a>Отчеты Azure CDN уровня "Премиум" от Verizon

С помощью **Azure CDN от Verizon уровня "Премиум"** можно получить доступ к следующим отчетам:
   * [Расширенные HTTP-отчеты.](cdn-advanced-http-reports.md)
   * [Статистика в режиме реального времени.](cdn-real-time-stats.md)
   * [Производительность граничного узла](cdn-edge-performance.md)

