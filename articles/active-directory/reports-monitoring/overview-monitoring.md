---
title: Мониторинг в Azure Active Directory | Документы Майкрософт
description: Общие сведения о мониторинге в Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: e2b3d8ce-708a-46e4-b474-123792f35526
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: efa4b625afb641209d3920c8663ed810ee27e1ad
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/01/2020
ms.locfileid: "89228653"
---
# <a name="what-is-azure-active-directory-monitoring"></a>Мониторинг в Azure Active Directory

Теперь функция мониторинга Azure Active Directory (Azure AD) позволяет направлять журналы действий в различные конечные точки. Эти журналы затем можно сохранить для долговременного использования или интегрировать со сторонними средствами для управления сведениями и событиями безопасности (SIEM), чтобы получать аналитические сведения о своей среде.

Сейчас журналы можно направить в следующие конечные точки:

- Учетная запись хранения Azure.
- Концентратор событий Azure для интеграции с экземплярами Splunk и Sumologic.
- Рабочая область Azure Log Analytics, где вы можете анализировать данные, а также создавать информационные панели и оповещения для определенных событий.

**Требуемая роль**: глобальный администратор.

> [!VIDEO https://www.youtube.com/embed/syT-9KNfug8]

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="diagnostic-settings-configuration"></a>Настройка параметров диагностики

Чтобы настроить параметры мониторинга для журналов действий Azure AD, сначала войдите на [портал Azure](https://portal.azure.com), а затем выберите **Azure Active Directory**. Отсюда вы можете получить доступ к странице параметров диагностики двумя способами:

* В разделе **Мониторинг** выберите **Параметры диагностики**.

    ![Параметры диагностики](./media/overview-monitoring/diagnostic-settings.png)
    
* Выберите **Журналы аудита** или **События входа**, а затем — **Экспорт параметров**. 

    ![Параметры экспорта](./media/overview-monitoring/export-settings.png)


## <a name="route-logs-to-storage-account"></a>Передача журналов в учетную запись хранения

Направляя журналы в учетную запись хранения Azure, вы можете хранить их в дольше, чем определено в наших [политиках хранения](reference-reports-data-retention.md) по умолчанию. См. сведения о том, как [направлять данные в учетную запись хранения](quickstart-azure-monitor-route-logs-to-storage-account.md).

## <a name="stream-logs-to-event-hub"></a>Потоковая передача журналов в концентратор событий

Направляя журналы в концентратор событий Azure, вы можете выполнить интеграцию со сторонними средствами, например Sumologic и Splunk. Такая интеграция позволяет объединить данные из журнала действий Azure AD с другими данными, управляемыми средствами SIEM, что позволяет получать более подробные аналитические сведения о среде. См. сведения о том, как [осуществлять потоковую передачу журналов в концентратор событий](tutorial-azure-monitor-stream-logs-to-event-hub.md).

## <a name="send-logs-to-azure-monitor-logs"></a>Отправка данных в журналы Azure Monitor

[Журналы Azure Monitor](../../azure-monitor/log-query/log-query-overview.md) — это решение, которое консолидирует данные мониторинга из различных источников, а также предоставляет язык запросов и аналитическую систему для получения сведений о работе приложений и ресурсов. Отправляя журналы действий Azure AD в Azure Monitor Logs вы можете быстро извлекать и отслеживать собранные данные, а также создавать на их основе оповещения. См. дополнительные сведения об [отправке данных в журналы Azure Monitor](howto-integrate-activity-logs-with-log-analytics.md).

Вы также можете установить готовые представления для журналов действий Azure AD, чтобы отслеживать распространенные сценарии, связанные со входом в систему и событиями аудита. См. сведения об [установке и использовании предоставлений анализа журналов для журналов действий Azure AD](howto-install-use-log-analytics-views.md).

## <a name="next-steps"></a>Дальнейшие действия

* [Журналы действий в Azure Monitor](concept-activity-logs-azure-monitor.md)
* [Потоковая передача журналов в концентратор событий](tutorial-azure-monitor-stream-logs-to-event-hub.md)
* [Send logs to Azure Monitor logs](howto-integrate-activity-logs-with-log-analytics.md) (Отправка данных в журналы Azure Monitor)