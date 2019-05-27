---
title: Журналы диагностики Azure | Документация Майкрософт
description: Клиент может включить анализ журналов для Azure CDN.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2018
ms.author: magattus
ms.openlocfilehash: a5fab3e2bf9908fa35cf5f5485df3116b7718d8c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "66125909"
---
# <a name="azure-diagnostic-logs"></a>Журналы диагностики Azure

Журналы диагностики Azure позволяют просматривать данные основной аналитики и сохранять их в одно или несколько из следующих целевых расположений:

 - Учетная запись службы хранилища Azure
 - Центры событий Azure
 - [Рабочая область Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
Эта функция доступна на конечных точках CDN всех ценовых категорий. 

Журналы диагностики Azure позволяют экспортировать основные метрики использования из конечной точки CDN в различные источники и настраивать их для дальнейшего использования. Например, вам доступны следующие типы экспорта данных:

- экспорт данных в хранилище BLOB-объектов, экспорт в CSV-файл и создание диаграмм в Excel;
- экспорт данных в Центры событий и их сопоставление с данными из других служб Azure;
- Экспорт данных в журналы Azure Monitor и просматривать данные в рабочей области Log Analytics

На схеме ниже показано типичное представление данных основной аналитики CDN.

![Портал — журналы диагностики](./media/cdn-diagnostics-log/01_OMS-workspace.png)

*Рисунок 1. Представление основной аналитики CDN*

Дополнительные сведения см. в статье [о журналах диагностики](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="enable-logging-with-the-azure-portal"></a>Включение ведения журнала с помощью портала Azure

Выполните инструкции ниже, чтобы включить ведение журнала для основной аналитики CDN:

Войдите на [портале Azure](https://portal.azure.com). Если для рабочего процесса не включена CDN, [создайте профиль и конечную точку Azure CDN](cdn-create-new-endpoint.md) перед продолжением.

1. На портале Azure перейдите к **профилю CDN**.

2. На портале Azure найдите профиль CDN или выберите на панели мониторинга. Затем выберите конечную точку CDN, для которой нужно включить журналы диагностики.

    ![Портал — журналы диагностики](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. В разделе "Мониторинг" выберите **Журналы диагностики**.

   Появится страница **Журналы диагностики**.

    ![Портал — журналы диагностики](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a>Включение ведения журнала с помощью службы хранилища Azure

Чтобы использовать учетную запись хранения для хранения журналов, выполните следующие действия.
    
1. В поле **Имя** введите имя для параметров журнала диагностики.
 
2. Выберите **Архивировать в учетной записи хранения**, а затем выберите **CoreAnalytics**. 

2. Для параметра **Хранение (в днях)** выберите число дней хранения. Нулевое значение означает, что журналы будут храниться неограниченно долго. 

    ![Портал — журналы диагностики](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png) 

3. Выберите **Учетная запись хранения**.

    Появится страница **Выбрать учетную запись хранения**.

4. Выберите учетную запись хранения в раскрывающемся списке, а затем нажмите кнопку **ОК**.

    ![Портал — журналы диагностики](./media/cdn-diagnostics-log/cdn-select-storage-account.png)

5. После внесения параметров журнала диагностики выберите **Сохранить**.

### <a name="logging-with-azure-monitor"></a>Ведение журнала с помощью Azure Monitor

Чтобы использовать Azure Monitor для хранения журналов, выполните следующие действия.

1. На странице **Журналы диагностики** установите флажок **Отправить в Log Analytics**. 

    ![Портал — журналы диагностики](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. Выберите **Настройка** настроить ведение журнала Azure Monitor. 

   Откроется страница **Рабочие области Log Analytics**.

    >[!NOTE] 
    >Рабочие области OMS теперь называются рабочими областями Log Analytics.

    ![Портал — журналы диагностики](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. Щелкните **Создать рабочую область**.

    Откроется страница **Рабочая область Log Analytics**.

    >[!NOTE] 
    >Рабочие области OMS теперь называются рабочими областями Log Analytics.

    ![Портал — журналы диагностики](./media/cdn-diagnostics-log/07_Create-new.png)

4. Введите имя **рабочей области Log Analytics**. Имя рабочей области Log Analytics должно быть уникальным и может содержать только буквы, цифры и дефисы. Пробелы и знаки подчеркивания не допускаются. 

5. В качестве **подписки** выберите имеющуюся подписку из раскрывающегося списка. 

6. **Группа ресурсов** — создайте группу ресурсов Azure или выберите существующую.

7. **Расположение** — выберите расположение из списка.

8. Выберите **Закрепить на панели мониторинга**, если вы хотите сохранить конфигурацию журнала на панели мониторинга. 

9. Нажмите кнопку **ОК**, чтобы завершить настройку.

10. После создания рабочей области вы вернетесь на страницу **журналов диагностики**. Проверьте и подтвердите имя рабочей области Log Analytics.

    ![Портал — журналы диагностики](./media/cdn-diagnostics-log/09_Return-to-logging.png)

11. Выберите **CoreAnalytics**, а затем нажмите кнопку **Сохранить**.

12. Для просмотра новой рабочей области Log Analytics выберите **Основная аналитика** со страницы конечной точки CDN.

    ![Портал — журналы диагностики](./media/cdn-diagnostics-log/cdn-core-analytics-page.png) 

    Теперь рабочая область Log Analytics готова для записи данных в журнал. Чтобы воспользоваться этими данными, необходимо использовать [Azure Monitor регистрирует решение](#consuming-diagnostics-logs-from-a-log-analytics-workspace), описываемое далее в этой статье.

Дополнительные сведения о задержках данных журналов вы найдете [здесь](#log-data-delays).

## <a name="enable-logging-with-powershell"></a>Включение ведения журнала с помощью PowerShell

Используйте командлеты Azure PowerShell из примера ниже, чтобы включить журналы диагностики.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="enabling-diagnostic-logs-in-a-storage-account"></a>Включение журналов диагностики в учетной записи хранения

1. Войдите в систему и выберите подписку:

    Connect-AzAccount 

    Select-AzureSubscription -SubscriptionId 

2. Чтобы включить журналы диагностики в учетной записи хранения, введите следующую команду:

    ```powershell
    Set-AzDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
    ```

3. Чтобы включить журналы диагностики в рабочей области Log Analytics, введите следующую команду:

    ```powershell
    Set-AzDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
    ```

## <a name="consuming-diagnostics-logs-from-azure-storage"></a>Использование журналов диагностики из службы хранилища Azure
В этом разделе описана схема данных основной аналитики CDN и структура их хранения в учетной записи хранения Azure, а также приведены примеры кода для скачивания журналов в формате CSV.

### <a name="using-microsoft-azure-storage-explorer"></a>Использование обозревателя хранилищ Microsoft Azure
Прежде чем обращаться к данным основной аналитики из учетной записи хранения Azure, сначала необходимо установить средство для доступа к содержимому в учетной записи хранения. На рынке доступно несколько таких средств, но мы рекомендуем использовать обозреватель хранилищ Microsoft Azure. Вы можете скачать это средство [на странице обозревателя хранилищ Azure](https://storageexplorer.com/). После скачивания и установки программы настройте в ней использование той же учетной записи хранения Azure, которая была настроена в качестве назначения для журналов диагностики CDN.

1.  Откройте **обозреватель хранилищ Microsoft Azure**.
2.  Найдите учетную запись хранения.
3.  Разверните узел **Контейнеры больших двоичных объектов** в учетной записи хранения.
4.  Выберите контейнер с именем *insights-logs-coreanalytics*.
5.  Результаты отобразятся на панели справа, начиная с первого уровня, который обозначен как *resourceId=*. Продолжайте выбирать уровни, пока не найдете файл *PT1H.json*. Описание пути см. в следующих данных о [формате пути большого двоичного объекта](cdn-azure-diagnostic-logs.md#blob-path-format).
6.  Каждый файл *PT1H.json* в большом двоичном объекте представляет журналы аналитики за один час для конкретной конечной точки CDN или соответствующего личного домена.
7.  Схема содержимого этого JSON-файла описана в разделе "Схемы журналов основной аналитики".


#### <a name="blob-path-format"></a>Формат пути BLOB-объекта

Журналы основной аналитики создаются каждый час, собираются в один BLOB-объект и хранятся в нем в формате полезных данных JSON. Поскольку средство обозревателя хранилищ воспринимает символ "/" как разделитель каталогов и отслеживает иерархию, путь к BLOB-объекту Azure отображается как иерархическая структура, сформированная из его имени. Имя BLOB-объекта соответствует описанному ниже соглашению об именовании.   

```resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json```

**Описание полей**

|Value|Описание|
|-------|---------|
|Идентификатор подписки    |Идентификатор подписки Azure в формате GUID.|
|Имя группы ресурсов |Имя группы ресурсов, к которой относятся ресурсы CDN.|
|Имя профиля |Имя профиля CDN.|
|Имя конечной точки |Имя конечной точки CDN.|
|Год|  4-значное обозначение года, например 2017|
|Месяц| 2-значное обозначение номера месяца 01 — январь, 12 — декабрь.|
|День|   2-значное обозначение дня месяца|
|PT1H.json| Фактический JSON-файл, в котором хранятся данные аналитики.|

### <a name="exporting-the-core-analytics-data-to-a-csv-file"></a>Экспорт данных основной аналитики в CSV-файл

Чтобы упростить вам работу, мы предлагаем пример кода для средства работы с основной аналитикой. Это средство выполняет скачивание JSON-файлов с преобразованием в неструктурированные файлы с разделителями-запятыми, на основе которых можно создавать диаграммы или вычислять статистику.

Вот как использовать это средство:

1.  Перейдите по ссылке GitHub: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv)
2.  Скачайте код.
3.  Следуйте инструкциям по компиляции и настройке.
4.  Запустите средство.
5.  Полученный CSV-файл позволяет просмотреть данные аналитики в простой одноуровневой структуре.

## <a name="consuming-diagnostics-logs-from-a-log-analytics-workspace"></a>Использование журналов диагностики в рабочей области Log Analytics
Azure Monitor — это служба Azure, которая отслеживает облачные и локальные среды, чтобы поддерживать уровень их доступности и производительности. Она собирает данные, формируемые ресурсами в облачных и локальных средах, а также другими средствами мониторинга, и на их основе предоставляет аналитические сведения для нескольких источников. 

Чтобы использовать Azure Monitor, [включить ведение журнала](#enable-logging-with-azure-storage) в рабочую область Azure Log Analytics, как описано ранее в этой статье.

### <a name="using-the-log-analytics-workspace"></a>Использование рабочей области Log Analytics

 На следующей схеме показана архитектура входов и выходов репозитория:

![Рабочая область Log Analytics](./media/cdn-diagnostics-log/12_Repo-overview.png)

*Рисунок 3. Репозиторий OMS Log Analytics*.

Решения по управлению позволяют отобразить данные различными способами. Решения по управлению можно получить из [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

Можно установить решения для мониторинга из Azure marketplace, выбрав **получить сейчас** ссылку в нижней части каждого решения.

### <a name="add-an-azure-monitor-cdn-monitoring-solution"></a>Добавьте решение для мониторинга Azure Monitor CDN

Выполните следующие действия, чтобы добавить Azure Monitor, решение для мониторинга.

1.   Войдите на портал Azure, используя подписку Azure, и перейдите на панель мониторинга.
    ![Панель мониторинга Azure](./media/cdn-diagnostics-log/13_Azure-dashboard.png)

2. На странице **Создать** выберите **Marketplace**, а затем — **Мониторинг и управление**.

    ![Marketplace](./media/cdn-diagnostics-log/14_Marketplace.png)

3. На странице **Мониторинг и управление** щелкните **Просмотреть все**.

    ![Просмотреть все](./media/cdn-diagnostics-log/15_See-all.png)

4. Введите CDN в поле поиска.

    ![Просмотреть все](./media/cdn-diagnostics-log/16_Search-for.png)

5. Выберите **Azure CDN Core Analytics**. 

    ![Просмотреть все](./media/cdn-diagnostics-log/17_Core-analytics.png)

6. Когда вы нажмете кнопку **Создать**, вам будет предложено создать рабочую область Log Analytics или использовать имеющуюся. 

    ![Просмотреть все](./media/cdn-diagnostics-log/18_Adding-solution.png)

7. Выберите созданную ранее рабочую область. Затем необходимо добавить учетную запись автоматизации.

    ![Просмотреть все](./media/cdn-diagnostics-log/19_Add-automation.png)

8. На следующем снимке экрана показана форма автоматизации, которую необходимо заполнить. 

    ![Просмотреть все](./media/cdn-diagnostics-log/20_Automation.png)

9. Создав учетную запись автоматизации, вы можете добавить решение. Нажмите кнопку **Создать**.

    ![Просмотреть все](./media/cdn-diagnostics-log/21_Ready.png)

10. Решение добавлено в рабочую область. Вернитесь к панели мониторинга на портале Azure.

    ![Просмотреть все](./media/cdn-diagnostics-log/22_Dashboard.png)

    Выберите созданную рабочую область Log Analytics, чтобы перейти к ней. 

11. Выберите плитку **Портал OMS**, чтобы увидеть новое решение.

    ![Просмотреть все](./media/cdn-diagnostics-log/23_workspace.png)

12. Портал должен выглядеть примерно так, как на снимке экрана ниже:

    ![Просмотреть все](./media/cdn-diagnostics-log/24_OMS-solution.png)

    Выберите одну из плиток, чтобы увидеть несколько представлений данных.

    ![Просмотреть все](./media/cdn-diagnostics-log/25_Interior-view.png)

    Представления можно прокрутить влево или вправо, чтобы увидеть дополнительные плитки, показывающие отдельные представления данных. 

    Выберите одну из плиток для просмотра дополнительных сведений о данных.

     ![Просмотреть все](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a>Предложения и ценовые категории

Предложения и ценовые категории для решений по управлению представлены [здесь](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions).

### <a name="customizing-views"></a>Настройка представлений

Представление о данных можно настроить с помощью **конструктора представлений**. Чтобы начать разработку, перейдите к рабочей области Log Analytics и выберите плитку **Конструктор представлений**.

![Конструктор представлений](./media/cdn-diagnostics-log/27_Designer.png)

Перетаскивайте типы диаграмм и вносите сведения о данных для анализа.

![Конструктор представлений](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a>Задержка данных журнала

В следующей таблице представлены задержки данных журналов для профилей **Azure CDN уровня "Стандартный" от Майкрософт**, **Azure CDN уровня "Стандартный" от Akamai** и **Azure CDN уровня "Стандартный" или "Премиум" от Verizon**.

Задержка данных журнала Майкрософт | Задержка данных журнала Verizon | Задержка данных журнала Akamai
--- | --- | ---
Задержка на 1 час. | Задержка составляет 1 час, а от момента распространения конечной точки до начала отображения данных может пройти до 2 часов. | Задержка составляет 24 часа, а данные, созданные более 24 часов назад, начнут отображаться с задержкой до 2 часов. Если данные созданы недавно, задержка отображения журналов может составить до 25 часов.

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a>Типы журналов диагностики для основной аналитики CDN

В настоящее время все предлагаемые журналы основной аналитики корпорации Майкрософт содержат метрики для статистики HTTP-ответов и исходящих данных, зафиксированные в точках присутствия и (или) границах CDN.

### <a name="core-analytics-metrics-details"></a>Сведения о метриках основной аналитики
В следующей таблице представлен список метрик, доступных в журналах основной аналитики для профилей **Azure CDN уровня "Стандартный" от Майкрософт**, **Azure CDN уровня "Стандартный" от Akamai** и **Azure CDN уровня "Стандартный" или "Премиум" от Verizon**. Не все метрики доступны у всех поставщиков, хотя различия минимальны. В этой таблице также есть сведения о доступности метрик у определенных поставщиков. Метрики доступны только для конечных точек CDN, на которых зарегистрирован трафик.


|Метрика                     | Описание | Microsoft | Verizon | Akamai |
|---------------------------|-------------|-----------|---------|--------|
| RequestCountTotal         | Общее количество запросов за этот период. | Да | Да |Да |
| RequestCountHttpStatus2xx | Количество запросов, на которые возвращены ответы с кодами HTTP 2xx (200, 202 и т. д.). | Да | Да |Да |
| RequestCountHttpStatus3xx | Количество запросов, на которые возвращены ответы с кодами HTTP 3xx (300, 302 и т. д.). | Да | Да |Да |
| RequestCountHttpStatus4xx | Количество запросов, на которые возвращены ответы с кодами HTTP 4xx (400, 404 и т. д.). | Да | Да |Да |
| RequestCountHttpStatus5xx | Количество запросов, на которые возвращены ответы с кодами HTTP 5xx (500, 504 и т. д.). | Да | Да |Да |
| RequestCountHttpStatusOthers | Количество всех остальных кодов HTTP (кроме 2xx–5xx). | Да | Да |Да |
| RequestCountHttpStatus200 | Количество всех запросов, на которые возвращался ответ с кодом HTTP 200. | Да | Нет  |Да |
| RequestCountHttpStatus206 | Количество всех запросов, на которые возвращался ответ с кодом HTTP 206. | Да | Нет  |Да |
| RequestCountHttpStatus302 | Количество всех запросов, на которые возвращался ответ с кодом HTTP 302. | Да | Нет  |Да |
| RequestCountHttpStatus304 | Количество всех запросов, на которые возвращался ответ с кодом HTTP 304. | Да | Нет  |Да |
| RequestCountHttpStatus404 | Количество всех запросов, на которые возвращался ответ с кодом HTTP 404. | Да | Нет  |Да |
| RequestCountCacheHit | Количество всех запросов, в результате которых произошло попадание в кэш. Такие ресурсы возвращаются клиенту непосредственно из точки присутствия. | Да | Да | Нет  |
| RequestCountCacheMiss | Количество всех запросов, в результате которых произошел промах кэша. Это означает, что ресурс не найден в ближайшей к клиенту точке присутствия и поэтому был получен из источника. | Да | Да | Нет |
| RequestCountCacheNoCache | Количество всех запросов к ресурсу, кэширование которых запрещено из-за конфигурации пользователя на границе. | Да | Да | Нет |
| RequestCountCacheUncacheable | Количество всех запросов к ресурсам, кэширование которых запрещено заголовками Cache-Control и Expires, указывающими, что ресурс не должен кэшироваться в точке присутствия или с помощью клиента HTTP. | Да | Да | Нет |
| RequestCountCacheOthers | Количество всех запросов с состоянием кэша, не указанным выше. | Нет | Да | Нет  |
| EgressTotal | Объем передачи исходящих данных в ГБ. | Да |Да |Да |
| EgressHttpStatus2xx | Объем передачи исходящих данных* для ответов с кодами состояния HTTP 2xx (ГБ). | Да | Да | Нет  |
| EgressHttpStatus3xx | Объем передачи исходящих данных для ответов с кодами состояния HTTP 3xx (ГБ). | Да | Да | Нет  |
| EgressHttpStatus4xx | Объем передачи исходящих данных для ответов с кодами состояния HTTP 4xx (ГБ). | Да | Да | Нет  |
| EgressHttpStatus5xx | Объем передачи исходящих данных для ответов с кодами состояния HTTP 5xx (ГБ). | Да | Да | Нет |
| EgressHttpStatusOthers | Объем передачи исходящих данных для ответов с другими кодами состояния HTTP (ГБ). | Да | Да | Нет  |
| EgressCacheHit | Объем передачи исходящих данных для ответов, которые были доставлены непосредственно из кэша CDN на точки присутствия и границы CDN. | Да | Да | Нет |
| EgressCacheMiss. | Объем передачи исходящих данных для ответов, не найденных на ближайшем POP-сервере и полученных с сервера-источника. | Да | Да | Нет |
| EgressCacheNoCache | Объем передачи исходящих данных для ресурсов, кэширование которых запрещено из-за конфигурации пользователя на границе. | Да | Да | Нет |
| EgressCacheUncacheable | Объем передачи исходящих данных для ресурсов, кэширование которых запрещено заголовками Cache-Control и (или) Expires. Эти заголовки указывают, что ресурс не должен кэшироваться в точке присутствия или в HTTP-клиенте. | Да | Да | Нет |
| EgressCacheOthers | Объем передачи исходящих данных для других сценариев с использованием кэша. | Нет | Да | Нет |

*Передача исходящих данных — это трафик, который доставляется с POP-серверов CDN клиенту.


### <a name="schema-of-the-core-analytics-logs"></a>Схема журналов основной аналитики 

Все журналы хранятся в формате JSON, и каждая запись содержит строковые поля, как показано на схеме ниже.

```json
    "records": [
        {
            "time": "2017-04-27T01:00:00",
            "resourceId": "<ARM Resource Id of the CDN Endpoint>",
            "operationName": "Microsoft.Cdn/profiles/endpoints/contentDelivery",
            "category": "CoreAnalytics",
            "properties": {
                "DomainName": "<Name of the domain for which the statistics is reported>",
                "RequestCountTotal": integer value,
                "RequestCountHttpStatus2xx": integer value,
                "RequestCountHttpStatus3xx": integer value,
                "RequestCountHttpStatus4xx": integer value,
                "RequestCountHttpStatus5xx": integer value,
                "RequestCountHttpStatusOthers": integer value,
                "RequestCountHttpStatus200": integer value,
                "RequestCountHttpStatus206": integer value,
                "RequestCountHttpStatus302": integer value,
                "RequestCountHttpStatus304": integer value,
                "RequestCountHttpStatus404": integer value,
                "RequestCountCacheHit": integer value,
                "RequestCountCacheMiss": integer value,
                "RequestCountCacheNoCache": integer value,
                "RequestCountCacheUncacheable": integer value,
                "RequestCountCacheOthers": integer value,
                "EgressTotal": double value,
                "EgressHttpStatus2xx": double value,
                "EgressHttpStatus3xx": double value,
                "EgressHttpStatus4xx": double value,
                "EgressHttpStatus5xx": double value,
                "EgressHttpStatusOthers": double value,
                "EgressCacheHit": double value,
                "EgressCacheMiss": double value,
                "EgressCacheNoCache": double value,
                "EgressCacheUncacheable": double value,
                "EgressCacheOthers": double value,
            }
        }

    ]
}
```

Здесь *time* представляет начало периода, для которого выводятся статистические данные. Если метрика не поддерживается поставщиком CDN, вместо значения типа double или integer ей присваивается значение NULL. Это значение указывает на отсутствие метрики, то есть по смыслу отличается от значения 0. Каждому домену, настроенному на конечной точке, соответствует один набор этих метрик.

Пример набора свойств:

```json
{
     "DomainName": "manlingakamaitest2.azureedge.net",
     "RequestCountTotal": 480,
     "RequestCountHttpStatus2xx": 480,
     "RequestCountHttpStatus3xx": 0,
     "RequestCountHttpStatus4xx": 0,
     "RequestCountHttpStatus5xx": 0,
     "RequestCountHttpStatusOthers": 0,
     "RequestCountHttpStatus200": 480,
     "RequestCountHttpStatus206": 0,
     "RequestCountHttpStatus302": 0,
     "RequestCountHttpStatus304": 0,
     "RequestCountHttpStatus404": 0,
     "RequestCountCacheHit": null,
     "RequestCountCacheMiss": null,
     "RequestCountCacheNoCache": null,
     "RequestCountCacheUncacheable": null,
     "RequestCountCacheOthers": null,
     "EgressTotal": 0.09,
     "EgressHttpStatus2xx": null,
     "EgressHttpStatus3xx": null,
     "EgressHttpStatus4xx": null,
     "EgressHttpStatus5xx": null,
     "EgressHttpStatusOthers": null,
     "EgressCacheHit": null,
     "EgressCacheMiss": null,
     "EgressCacheNoCache": null,
     "EgressCacheUncacheable": null,
     "EgressCacheOthers": null
}

```

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Журналы диагностики Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Анализ вариантов использования CDN Azure](https://docs.microsoft.com/azure/cdn/cdn-analyze-usage-patterns)
* [Журналы Azure Monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)
* [Log Analytics REST API Reference](https://docs.microsoft.com/rest/api/loganalytics) (Справочник по REST API Log Analytics)







