---
title: Application Insights и C#
titleSuffix: Azure Cognitive Services
description: Создание бота, интегрированного с приложением LUIS и Application Insights, с помощью C#.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/30/2019
ms.author: diberry
ms.openlocfilehash: 56ceb48be9d5cc9d1cdceed7505e2e3e918a7286
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399658"
---
# <a name="add-luis-results-to-application-insights-with-a-bot-in-c"></a>Добавление результатов LUIS в Application Insights с помощью бота на C#

В этом учебнике приведена процедура добавления данных ответа LUIS в хранилище данных телеметрии [Application Insights](https://azure.microsoft.com/services/application-insights/). Получив эти данные, можно выполнить запрос Kusto языка или Power BI для анализа, статистической обработки и создавать отчеты о намерения и сущности utterance в режиме реального времени. Этот анализ помогает определить, следует ли добавлять или изменять намерения и сущности приложения LUIS.

Бот создается с помощью Bot Framework 3.x и бота веб-приложения Azure. Объект [Bot Framework 4.x с руководством LUIS](luis-csharp-tutorial-bf-v4.md) также доступна.

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * добавление Application Insights в бот веб-приложения;
> * запись и отправка результатов запроса LUIS в Application Insights;
> * отправка запроса в Application Insights для получения основных намерений, оценки и фрагментов речи.

## <a name="prerequisites"></a>Технические условия

* Бот веб-приложения LUIS из **[предыдущего руководства](luis-csharp-tutorial-build-bot-framework-sample.md)** с включенным компонентом Application Insights.
* [Visual Studio 2017](https://www.visualstudio.com/downloads/), локально установленная на компьютере.

> [!Tip]
> Если у вас еще нет подписки, вы можете зарегистрироваться для получения [бесплатной учетной записи](https://azure.microsoft.com/free/).

Весь код из этого руководства можно найти в [репозитории Azure-Samples на GitHub](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/csharp). Все строки, связанные с этим руководством, закомментированы с помощью `//LUIS Tutorial:`.

## <a name="review-luis-web-app-bot"></a>Проверка бота веб-приложения LUIS

Предполагается, что у вас есть код, который выглядит следующим образом, или вы выполнили процедуру в [другом учебнике](luis-csharp-tutorial-build-bot-framework-sample.md):

   [!code-csharp[Web app bot with LUIS](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs "Web app bot with LUIS")]

## <a name="application-insights-in-web-app-bot"></a>Application Insights в боте веб-приложения

В настоящее время служба Application Insights, добавляемая при создании службы бота веб-приложения, собирает общие данные телеметрии состояния для бота. Она не собирает сведения об ответах LUIS. Для анализа и улучшения службы LUIS требуются сведения об ответах LUIS.  

Чтобы бот веб-приложения мог записывать ответы LUIS, служба **[Application Insights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/)** должна быть установлена и настроена для проекта.

## <a name="download-web-app-bot"></a>Загрузка бота веб-приложения

Используйте [Visual Studio 2017](https://www.visualstudio.com/downloads/), чтобы добавить и настроить Application Insights для бота веб-приложения. Чтобы использовать бот веб-приложения в Visual Studio, скачайте код бота веб-приложения.

1. На портале Azure для бота веб-приложения выберите **Сборка**.

    ![Выберите "Сборка" на портале](./media/luis-tutorial-bot-csharp-appinsights/download-build-menu.png)

2. Выберите **Download zip file** (Скачать ZIP-файл) и дождитесь подготовки файла.

    ![Скачать ZIP-файл](./media/luis-tutorial-bot-csharp-appinsights/download-link.png)

3. Выберите **Download zip file** (Скачать ZIP-файл) во всплывающем окне. Запомните расположение на компьютере, оно понадобится в следующем разделе.

    ![Всплывающее окно загрузки ZIP-файла](./media/luis-tutorial-bot-csharp-appinsights/download-popup.png)

## <a name="open-solution-in-visual-studio-2017"></a>Открытие решения в Visual Studio 2017

1. Извлеките файл в папку.

2. Откройте Visual Studio 2017, а затем откройте файл решения, `Microsoft.Bot.Sample.LuisBot.sln`. При появлении предупреждения системы безопасности выберите "ОК".

    ![Открытие решения в Visual Studio 2017](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-security-warning.png)

3. Visual Studio необходимо добавить зависимости в решение. В **обозревателе решений** щелкните правой кнопкой мыши **Ссылки** и выберите **Управление пакетами NuGet**.

    ![Управление пакетами NuGet](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-manage-nuget-packages.png)

4. Диспетчер пакетов NuGet выведет список установленных пакетов. Выберите **Восстановить** в желтой строке. Дождитесь завершения процесса восстановления.

    ![восстановление пакетов NuGet;](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-restore-packages.png)

## <a name="add-application-insights-to-the-project"></a>Добавление Application Insights в проект

Установите и настройте Application Insights в Visual Studio.

1. В Visual Studio 2017 в верхнем меню выберите **Проект**, а затем **Добавить телеметрию Application Insights...** .

2. В окне **Конфигурация Application Insights** выберите **Начать бесплатно**.

    ![Начало настройки Application Insights](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-configure-app-insights.png)

3. Зарегистрируйте приложение в Application Insights. Может понадобиться ввести учетные данные портала Azure.

4. Visual Studio добавляет Application Insights в проект, отображая соответствующее состояние.

    ![Состояние Application Insights](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-adding-application-insights-to-project.png)

    По окончании процесса в окне **Конфигурация Application Insights** отображается состояние хода выполнения.

    ![Состояние хода выполнения Application Insights](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-configured-application-insights-to-project.png)

    **Включить сбор данных трассировки** отображается красным цветом, то есть эта функция не включена. Для этого учебника эта функция не используется.

## <a name="build-and-resolve-errors"></a>Сборка и устранение ошибок

1. Выполните сборку решения, выбрав меню **Сборка**, а затем пункт **Перестроить решение**. Дождитесь завершения сборки.

2. Если происходит сбой сборки с ошибками `CS0104`, необходимо исправить их. В папке `Controllers` в `MessagesController.cs file` исправьте неоднозначное использование типа `Activity`, задав префикс Connector для типа Activity. Чтобы сделать это, измените имя `Activity` в строках 22 и 36 с `Activity` на `Connector.Activity`. Выполните повторную сборку решения. Ошибок сборки быть не должно.

    Полный исходный файл этого кода:

    [!code-csharp[MessagesController.cs file](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/MessagesController.cs "MessagesController.cs file")]

## <a name="publish-project-to-azure"></a>Публикация проекта в Azure

Пакет **Application Insights** теперь находится в проекте и правильно настроен для ваших учетных данных на портале Azure. Изменения для проекта должны быть опубликованы обратно в Azure.

1. Щелкните правой кнопкой мыши имя проекта в **обозревателе решений** и выберите пункт **Опубликовать**.

    ![Публикация проекта на портале](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish.png)

2. В окне **публикации** выберите **Создать профиль**.

    ![В процессе публикации будет создан новый профиль.](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish-1.png)

3. Выберите **Импорт профиля** и нажмите кнопку **ОК**.

    ![В процессе публикации профиль будет импортирован.](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish-2.png)

4. В окне **Импортировать файл параметров публикации** перейдите в папку проекта, перейдите в папку `PostDeployScripts`, выберите файл, который заканчивается на `.PublishSettings`, и выберите `Open`. Теперь вы настроили публикацию для этого проекта.

5. Опубликуйте локальный исходный код в службе Azure Bot, нажав кнопку **Опубликовать**. В окне **вывода** показывается состояние. Остальная часть учебника выполняется на портале Azure. Закройте Visual Studio 2017.

## <a name="open-three-browser-tabs"></a>Откройте три вкладки браузера

На портале Azure найдите бот веб-приложения и откройте его. В следующей процедуре используется три разных представления бота веб-приложения. Поэтому будет проще открыть три отдельные вкладки в браузере:
  
>  * тестирование в веб-чате;
>  * Сборка/открытие редактора кода в Интернете -> Редактор службы приложений;
>  * Редактор службы приложений/открытие консоли Kudu -> консоль диагностики.

## <a name="modify-basicluisdialogcs-code"></a>Изменение кода BasicLuisDialog.cs

1. На вкладке браузера **Редактор службы приложений** откройте файл `BasicLuisDialog.cs`.

2. Добавьте следующую зависимость NuGet в существующие строки `using`:

   [!code-csharp[Add using statement](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=11-12 "Add using statement")]

3. Добавьте функцию `LogToApplicationInsights`:

   [!code-csharp[Add the LogToApplicationInsights function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=61-92 "Add the LogToApplicationInsights function")]

    Ключ инструментирования Application Insights уже находится в параметре бота веб-приложения с именем `BotDevInsightsKey`.

    Последняя строка функции добавляет данные в Application Insights. Имя трассировки — `LUIS`, уникальное имя, получаемое отдельно от всех остальных данных телеметрии, собираемых этим ботом веб-приложения. Все свойства также начинаются с префикса `LUIS_`, чтобы можно было увидеть, какие данные добавляются в рамках этого учебника по сравнению с данными, предоставляемыми ботом веб-приложения.

4. Вызовите функцию `LogToApplicationInsights` в верхней части функции `ShowLuisResult`:

   [!code-csharp[Use the LogToApplicationInsights function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=114-115 "Use the LogToApplicationInsights function")]

## <a name="build-web-app-bot"></a>Сборка бота веб-приложения

1. Выполните сборку бота веб-приложения одном из двух способов. Первый способ: щелкните правой кнопкой мыши `build.cmd` в **редакторе службы приложений**, а затем выберите **Запуск из консоли**. На консоли отображаются выходные данные, а затем `Finished successfully.`.

2. Если сборка не выполнена успешно, необходимо открыть консоль, перейти к сценарию и запустить его, выполнив следующие действия. В **редакторе службы приложений** на верхней синей панели выберите имя бота веб-приложения, а затем в раскрывающемся списке выберите **Открыть консоль Kudu**.

    ![Открытие консоли Kudu](./media/luis-tutorial-bot-csharp-appinsights/open-kudu-console.png)

3. В окне консоли введите следующую команду:

    ```console
    cd site\wwwroot && build.cmd
    ```

    Дождитесь завершения сборки с сообщением `Finished successfully.`.

## <a name="test-the-web-app-bot"></a>Тестирование бота веб-приложения

1. Для проверки бота веб-приложения откройте функцию **тестирования в веб-чате** на портале.

2. Введите фразу `Coffee bar on please`.  

    ![Тестирование бота веб-приложения в чате](./media/luis-tutorial-bot-csharp-appinsights/test-in-web-chat.png)

3. Вы должны увидеть тот же ответ чат-бота. Изменение передает данные в Application Insights, а не в ответы бота. Введите несколько дополнительных фрагментов речи, то есть немного больше данных в Application Insights:

|Высказывания|
|--|
|Доставить пиццу|
|Выключить свет во всех помещениях|
|Включить свет в коридоре|


## <a name="view-luis-entries-in-application-insights"></a>Просмотр записей LUIS в Application Insights

Откройте Application Insights, чтобы просмотреть записи LUIS.

1. На портале выберите **Все ресурсы**, выполните фильтрацию по имени бота веб-приложения. Щелкните ресурс с типом **Application Insights**. Значок для Application Insights — лампочка.

    ![Поиск Application insights на портале Azure](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights.png)

2. Когда ресурс откроется, щелкните значок **поиска** (лупы) на крайней правой панели. Откроется новая панель справа. В зависимости от объема обнаруженных данных телеметрии вывод панели может занять некоторое время. Выполните поиск `LUIS`. Список будет уточнен для включения только тех результатов запросов LUIS, которые были добавлены в этом учебнике.

    ![Поиск трассировок](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-trace.png)

3. Выберите верхнюю запись. Откроется новое окно с более подробными данными, включая пользовательские данные для запроса LUIS в правом углу. Данные содержат основное намерение и его оценку.

    ![Просмотр элемента трассировки](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-trace-item.png)

    Когда вы закончите, нажмите **X** в верхнем правом углу для возврата к списку элементов зависимостей.

> [!Tip]
> Если вы хотите сохранить список зависимостей и вернуться к нему позже, щелкните **...Дополнительно** и выберите **Сохранить избранное**.

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Запрос Application Insights для получения намерения, оценки и фрагмента речи

Application Insights дает возможность запросить данные с помощью [Kusto](https://docs.microsoft.com/azure/application-insights/app-insights-analytics#query-data-in-analytics) языка, а также экспорта его [Power BI](https://powerbi.microsoft.com).

1. Щелкните **Аналитика** в верхней части списка зависимостей над полем фильтра.

    ![Кнопка аналитики](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-analytics-button.png)

2. Откроется новое окно с окном запроса вверху и окном таблицы данных внизу. Если вы использовали базы данных, то такая организация будет вам знакома. Запрос содержит все элементы за последние 24 часа, начиная с имени `LUIS`. В столбце **CustomDimensions** содержатся результаты запросов LUIS в виде пар "имя-значение".

    ![Отчет аналитики по умолчанию](./media/luis-tutorial-bot-csharp-appinsights/analytics-query-1.png)

3. Чтобы получить основное намерение, оценку и фрагмент речи, добавьте следующий код над последней строкой в окне запроса:

    ```kusto
    | extend topIntent = tostring(customDimensions.LUIS_topScoringIntent)
    | extend score = todouble(customDimensions.LUIS_topScoringIntentScore)
    | extend utterance = tostring(customDimensions.LUIS_query)
    ```

4. Выполните запрос. Прокрутите таблицу данных вправо. Вы увидите новые столбцы для основного намерения, оценки и фрагмента речи. Щелкните столбец topIntent для сортировки.

    ![Пользовательский отчет аналитики](./media/luis-tutorial-bot-csharp-appinsights/analytics-query-2.png)

Дополнительные сведения о [язык запросов Kusto](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) или [Экспорт данных в Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi).

## <a name="learn-more-about-bot-framework"></a>Дополнительные сведения о платформе Bot Framework

Дополнительные сведения о [платформе Bot Framework](https://dev.botframework.com/).

## <a name="next-steps"></a>Дальнейшие действия

Другие данные, которые может потребоваться добавить в данные Application Insights, включают идентификатор приложения, идентификатор версии, время последнего изменения модели, дату последнего обучения, дату последней публикации. Эти значения можно получить из URL-адреса конечной точки (идентификатор приложения и идентификатор версии) или из вызова [API разработки](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3d), задания в параметрах бота веб-приложения и получения из этих параметров.  

При использовании одной подписки конечной точки для нескольких приложений LUIS вам также потребуется включить идентификатор подписки и свойство, сообщающее о том, что это общий ключ.

> [!div class="nextstepaction"]
> [Подробнее о примерах фрагментов речи](luis-how-to-add-example-utterances.md)
