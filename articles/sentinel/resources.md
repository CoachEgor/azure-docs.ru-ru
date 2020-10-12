---
title: Полезные ресурсы при работе с Azure Sentinel | Документация Майкрософт
description: В этом документе представлен список полезных ресурсов при работе с Sentinel Azure.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 9b4c8e38-c986-4223-aa24-a71b01cb15ae
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/02/2019
ms.author: yelevin
ms.openlocfilehash: 54c10d7941e053bec928f8f43310218d89124f9a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89461737"
---
# <a name="useful-resources-for-working-with-azure-sentinel"></a>Полезные ресурсы для работы с Azure Sentinel



В этой статье перечислены ресурсы, которые могут помочь получить дополнительные сведения о работе с Sentinel.

Соединители Azure Logic Apps: <https://docs.microsoft.com/connectors/>


## <a name="auditing-and-reporting"></a>Аудит и создание отчетов
Журналы аудита для Azure Sentinel хранятся в [журналах действий Azure](../azure-monitor/platform/platform-logs-overview.md).

Можно проводить аудит следующих поддерживаемых операций.

|Имя операции|    Тип ресурса|
|----|----|
|Создать или обновить книгу  |Microsoft. Insights/книги|
|Удалить книгу    |Microsoft. Insights/книги|
|Задание рабочего процесса   |Microsoft.Logic/workflows|
|Удалить рабочий процесс    |Microsoft.Logic/workflows|
|Создать сохраненный поиск    |Microsoft. OperationalInsights/workspaces/Саведсеарчес|
|Удалить сохраненный поиск    |Microsoft. OperationalInsights/workspaces/Саведсеарчес|
|Обновление правил генерации оповещений |Microsoft. Секуритинсигхтс/alertRules|
|Удаление правил генерации оповещений |Microsoft. Секуритинсигхтс/alertRules|
|Обновление действий ответа для правила генерации оповещений |Microsoft. Секуритинсигхтс/alertRules/действия|
|Удаление действий ответа на правило генерации оповещений |Microsoft. Секуритинсигхтс/alertRules/действия|
|Обновить закладки   |Microsoft. Секуритинсигхтс/закладки|
|Удалить закладки   |Microsoft. Секуритинсигхтс/закладки|
|Обновление вариантов   |Microsoft. Секуритинсигхтс/cases|
|Исследование вариантов обновления  |Microsoft. Секуритинсигхтс/cases/расследования|
|Создание комментариев к вариантам   |Microsoft. Секуритинсигхтс/cases/Comments|
|Обновление соединителей данных |Microsoft. Секуритинсигхтс/Connects|
|Удаление соединителей данных |Microsoft. Секуритинсигхтс/Connects|
|Обновить параметры    |Microsoft. Секуритинсигхтс/параметры|

### <a name="view-audit-and-reporting-data-in-azure-sentinel"></a>Просмотр данных аудита и отчетов в Azure Sentinel

Вы можете просмотреть эти данные, перепотокая их из журнала действий Azure в Azure Sentinel, где вы сможете выполнять исследование и анализ.

1. Подключите источник данных [действий Azure](connect-azure-activity.md) . После этого события аудита передаются в виде потока в новую таблицу на экране **журналов** с именем AzureActivity.
2. Затем запросите данные с помощью ККЛ, как в любой другой таблице.



## <a name="vendor-documentation"></a>Документация поставщика

| **поставщик**  | **Использование инцидента в Azure Sentinel** | **Ссылка**|
|----|----|----|
| GitHub| Используется для доступа к странице сообщества| <https://github.com/Azure/Azure-Sentinel> |
| палоалто| Настройка CEF| <https://www.paloaltonetworks.com/documentation/misc/cef.html>|
| PluralSight | Курс по языку запросов Kusto| [https://www.pluralsight.com/courses/kusto-query-language-kql-from-scratch](https://www.pluralsight.com/courses/kusto-query-language-kql-from-scratch)|

## <a name="blogs-and-forums"></a>Блоги и форумы

Опубликуйте свои вопросы в [течкоммунити пространстве](https://techcommunity.microsoft.com/t5/Azure-Sentinel/bd-p/AzureSentinel) для Azure Sentinel.

Просмотрите записи в блоге с метками Azure из [течкоммунити](https://techcommunity.microsoft.com/t5/Azure-Sentinel/bg-p/AzureSentinelBlog) и [Microsoft Azure](https://azure.microsoft.com/blog/tag/azure-sentinel/).


## <a name="next-steps"></a>Дальнейшие шаги
В этом документе вы получили список ресурсов, которые полезны при работе с Azure Sentinel. Дополнительные сведения о безопасности и соответствии требованиям Azure см. в [блоге Microsoft Azure безопасность и соответствие требованиям](https://docs.microsoft.com/archive/blogs/azuresecurity/).
