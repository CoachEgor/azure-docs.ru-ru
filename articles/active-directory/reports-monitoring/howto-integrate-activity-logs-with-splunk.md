---
title: Интеграция Splunk с помощью Azure Monitor | Документация Майкрософт
description: Узнайте, как интегрировать журналы Azure Active Directory с SumoLogic с помощью Azure Monitor
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 03/10/2020
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: a17f0a0c9c6a6c8bd466114e98b2b8c6ee5e9f8c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "85608166"
---
# <a name="how-to-integrate-azure-active-directory-logs-with-splunk-using-azure-monitor"></a>Инструкции. Интеграция журналов Azure Active Directory с Splunk с помощью Azure Monitor

В этой статье вы узнаете, как интегрировать журналы Azure Active Directory (Azure AD) со Splunk с помощью Azure Monitor. Сначала вы направите журналы в концентратор событий Azure, а затем интегрируете этот концентратор событий со Splunk.

## <a name="prerequisites"></a>Предварительные условия

Для использования этой функции необходимо иметь следующее.

- Концентратор событий Azure, содержащий журналы действий Azure AD. Узнайте, как [настроить потоковую передачу журналов действий в концентратор событий](quickstart-azure-monitor-stream-logs-to-event-hub.md). 

-  [Microsoft Azure добавить в для Splunk](https://splunkbase.splunk.com/app/3757/). 

## <a name="integrate-azure-active-directory-logs"></a>Интеграция журналов Azure Active Directory 

1. Откройте свой экземпляр Splunk и выберите **Data Summary** (Сводка данных).

    ![Кнопка "Data Summary" (Сводка данных)](./media/howto-integrate-activity-logs-with-splunk/DataSummary.png)

2. Выберите вкладку **Sourcetypes** (Типы источников), а затем — **amal: aadal:audit**.

    ![Вкладка "Sourcetypes" (Типы источников) в диалоговом окне сводки данных](./media/howto-integrate-activity-logs-with-splunk/sourcetypeaadal.png)

    Отобразятся журналы действий Azure AD, как показано на рисунке ниже.

    ![Журналы действий](./media/howto-integrate-activity-logs-with-splunk/activitylogs.png)

> [!NOTE]
> Если вам не удается установить надстройку в экземпляре Splunk (например, если вы используете прокси-сервер или работаете со Splunk Cloud), можно передать эти события в сборщик событий HTTP Splunk с помощью этой [функции Azure](https://github.com/Microsoft/AzureFunctionforSplunkVS), активируемой при поступлении новых сообщений в концентратор событий. 
>

## <a name="next-steps"></a>Дальнейшие шаги

* [Анализ схемы журналов аудита в Azure Monitor](reference-azure-monitor-audit-log-schema.md)
* [Анализ схемы журналов входа в Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md)
* [Часто задаваемые вопросы и известные проблемы](concept-activity-logs-azure-monitor.md#frequently-asked-questions)