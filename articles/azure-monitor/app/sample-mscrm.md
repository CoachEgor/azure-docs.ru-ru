---
title: Microsoft Dynamics CRM и Azure Application Insights | Документация Майкрософт
description: Получение данных телеметрии из Microsoft Dynamics CRM Online с помощью Application Insights. Пошаговое руководство по настройке, получению данных, визуализации и экспорту.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 04c66338-687e-49e5-9975-be935f98f156
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/16/2018
ms.reviewer: mazhar
ms.author: mbullwin
ms.openlocfilehash: 6119f1116d255f7cd2a2bfc20e86eeca9e5dfe82
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60523489"
---
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Пошаговое руководство. Включение телеметрии для Microsoft Dynamics CRM Online с помощью Application Insights
В этой статье показано, как получить данные телеметрии из службы [Microsoft Dynamics CRM Online](https://www.dynamics.com/) с помощью [Azure Application Insights](https://azure.microsoft.com/services/application-insights/). Мы рассмотрим весь процесс добавления сценария Application Insights в приложение, сбор данных и их визуализацию.

> [!NOTE]
> [Получите пример решения.](https://dynamicsandappinsights.codeplex.com/)
> 
> 

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Добавление Application Insights в новый или существующий экземпляр CRM Online
Чтобы отслеживать работу приложения, добавьте в него пакет SDK для Application Insights. Пакет SDK отправляет данные телеметрии на [портал Application Insights](https://portal.azure.com), где вы можете использовать наши эффективные инструменты анализа и диагностики, а также экспортировать данные в хранилище.

### <a name="create-an-application-insights-resource-in-azure"></a>Создание ресурса Application Insights в Azure
1. Получите [учетную запись Microsoft Azure](https://azure.com/pricing). 
2. Войдите на [портал Azure](https://portal.azure.com) и добавьте новый ресурс Application Insights. Здесь будут обрабатываться и отображаться ваши данные.

    ![Щелкните значок «+» и последовательно выберите «Службы для разработчиков», Application Insights.](./media/sample-mscrm/01.png)

    Выберите приложение ASP.NET в качестве типа приложения.
3. Следуйте инструкциям для [получения скрипта SDK JavaScript для приложения](../../azure-monitor/app/javascript.md#set-up-application-insights-for-your-web-page), скопируйте фрагмент кода JavaScript и не забудьте заменить ключ инструментирования на правильное значение для вашего ресурса Application Insights.

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Создание веб-ресурса JavaScript в Microsoft Dynamics CRM
1. Откройте экземпляр CRM Online и войдите с правами администратора.
2. Последовательно откройте элементы "Параметры Microsoft Dynamics CRM", "Настройка", "Настройка системы".

    ![Параметры Microsoft Dynamics CRM](./media/sample-mscrm/00001.png)

    ![Параметры > Настройки](./media/sample-mscrm/00002.png)

1. Создайте ресурс JavaScript.

    ![Диалоговое окно создания веб-ресурса](./media/sample-mscrm/07.png)

    Присвойте ему имя, выберите тип **Script (JScript)** и откройте текстовый редактор.

    ![Откройте текстовый редактор](./media/sample-mscrm/00004.png)
2. Скопируйте код из пакета SDK JavaScript для Application Insights, в котором вы перед этим настроили ключ инструментирования. При копировании необходимо пропускать теги скриптов. См. снимок экрана ниже:

    ![Задайте ключ инструментирования](./media/sample-mscrm/000005.png)

    Код содержит ключ инструментирования, который идентифицирует ресурс Application Insights.
3. Щелкните «Сохранить», а затем «Опубликовать».

    ![Сохранение и публикация](./media/sample-mscrm/00006.png)

### <a name="instrument-forms"></a>Формы инструментов
1. В Microsoft CRM Online откройте форму «Учетная запись».

    ![Форма учетной записи](./media/sample-mscrm/00007.png)
2. Откройте свойства формы.

    ![Свойства формы](./media/sample-mscrm/00008.png)
3. Добавьте созданный веб-ресурс JavaScript.

    ![Меню "Добавить"](./media/sample-mscrm/13.png)

4. Сохраните и опубликуйте настройки формы.

## <a name="metrics-captured"></a>Собранные показатели телеметрии
Теперь сбор данных телеметрии для формы настроен. При каждом использовании данные будут отправляться в ресурс Application Insights.

Ниже приведены примеры отображаемых данных.

#### <a name="application-health"></a>Работоспособность приложения
![Время загрузки образца страницы](./media/sample-mscrm/15.png)

![Пример диаграммы просмотров страницы](./media/sample-mscrm/16.png)

Исключения браузера:

![Диаграмма "Исключения браузера"](./media/sample-mscrm/17.png)

Щелкните диаграмму, чтобы получить более подробную информацию.

![Список исключений](./media/sample-mscrm/18.png)

#### <a name="usage"></a>Использование
![Представления числа пользователей, сеансов и просмотров страниц](./media/sample-mscrm/19.png)

![Диаграммы сеансов](./media/sample-mscrm/20.png)

![Версии браузеров](./media/sample-mscrm/21.png)

#### <a name="browsers"></a>Браузеры
![Разбивка времени загрузки страницы](./media/sample-mscrm/22.png)

![Количество сеансов по версиям браузера](./media/sample-mscrm/23.png)

#### <a name="geolocation"></a>Географическое положение
![Число сеансов по странам](./media/sample-mscrm/24.png)

![Число сеансов и пользователей по странам](./media/sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Запрос на просмотр внутренней страницы
![Сводка по просмотрам страницы](./media/sample-mscrm/26.png)

![Поиск событий просмотра страницы](./media/sample-mscrm/27.png)

![Похожие просмотры страниц](./media/sample-mscrm/28.png)

![Свойства просмотра страниц](./media/sample-mscrm/29.png)

![Страницы по сеансам](./media/sample-mscrm/30.png)

## <a name="sample-code"></a>Пример кода
[Получите пример кода](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI
Если [экспортировать данные в Microsoft Power BI](../../azure-monitor/app/export-power-bi.md ), то можно выполнить еще более подробный анализ данных.

## <a name="sample-microsoft-dynamics-crm-solution"></a>Образец решения Microsoft Dynamics CRM
[Ниже приведен простой пример решения, реализованного в Microsoft Dynamics CRM](https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Подробнее
* [Что такое Azure Application Insights?](../../azure-monitor/app/app-insights-overview.md)
* [Application Insights для веб-страниц](../../azure-monitor/app/javascript.md)
* [Дополнительные примеры и пошаговые руководства](../../azure-monitor/app/app-insights-overview.md)
