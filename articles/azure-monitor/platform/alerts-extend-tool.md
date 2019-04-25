---
title: Расширение оповещений из Log Analytics в облако Azure для государственных организаций
description: В этой статье описываются средства и API, с помощью которых можно расширить оповещения из Log Analytics в оповещения Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 9d734f74c4e12b369e46c15dcb9d01a8185dddd6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60431176"
---
# <a name="extend-alerts-from-log-analytics-into-azure-alerts"></a>Расширение оповещений из Log Analytics в оповещения Azure
На портале OMS функция оповещения заменяется на "Оповещения Azure" в облаке Azure для государственных организаций. В рамках этого процесса оповещения, которые были изначально настроены в Log Analytics, будут расширены в Azure. Если вы не хотите ждать, когда они будут перенесены в Azure автоматически, то процесс инициализации можно запустить:

- Вручную, с портала Operations Management Suite. 
- Программным образом, с помощью API AlertsVersion.  

> [!NOTE]
> Начиная с 1 мая 2019 г., корпорация Майкрософт будет автоматически планомерно расширять оповещения, созданные в экземплярах портала OMS Azure для государственных организаций, до функции "Оповещения Azure". Если у вас возникли проблемы с созданием [групп действий](../../azure-monitor/platform/action-groups.md), используйте [эти шаги по устранению неполадок](alerts-extend-tool.md#troubleshooting), чтобы автоматически создать группы действий. До 15 марта 2019 г. на портале OMS Azure для государственных организаций можно использовать приведенные ниже действия.

## <a name="option-1-initiate-from-the-operations-management-suite-portal"></a>Вариант 1. Запуск с портала Operations Management Suite
Ниже описано, как отображать оповещения для рабочей области на портале Operations Management Suite для облака Azure для государственных организаций.  

1. На портале Azure щелкните **Все службы**. В списке ресурсов введите **Log Analytics**. Как только вы начнете вводить символы, список отфильтруется соответствующим образом. Выберите **Log Analytics**.
2. В области подписок Log Analytics выберите рабочую область, а затем выберите плитку **Портал OMS**.
![Снимок экрана с выделенной плиткой портала OMS области подписки Log Analytics](media/alerts-extend-tool/azure-portal-01.png) 
3. После того как вас перенаправили на портал Operations Management Suite, необходимо выбрать значок **Параметры**.
![Снимок экрана с выделенным значком "Параметры" на портале Operations Management Suite](media/alerts-extend-tool/oms-portal-settings-option.png) 
4. На странице **Параметры** выберите **Оповещения**.  
5. Выберите **Extend into Azure** (Расширить в Azure).
![Снимок экрана страницы параметров оповещений портала Operations Management Suite с выделенным расширением в Azure](media/alerts-extend-tool/ExtendInto.png)
6. В области **Оповещения** появится мастер, который состоит из 3 шагов. Ознакомьтесь с общими сведениями и нажмите кнопку **Далее**.
![Снимок экрана первого шага мастера](media/alerts-extend-tool/ExtendStep1.png)  
7. На втором шаге можно просмотреть сводку предлагаемых изменений, перечислив соответствующие [Группы действий](../../azure-monitor/platform/action-groups.md) для оповещений. Если подобные действия будут выполняться для нескольких оповещений, мастер предложит связать одну группу действий со всеми остальными.  Соглашение об именовании выглядит следующим образом: *WorkspaceName_AG_#Number*. Чтобы продолжить, нажмите кнопку **Далее**.
![Снимок экрана второго шага мастера](media/alerts-extend-tool/ExtendStep2.png)  
8. На последнем шаге мастера нажмите кнопку **Готово** и при появлении запроса подтвердите его. (Необязательно.) Чтобы получать уведомления о завершении процесса и об успешном перемещении всех оповещений в оповещения Azure, необходимо предоставить адрес электронной почты.
![Снимок экрана третьего шага мастера](media/alerts-extend-tool/ExtendStep3.png)

После завершения работы мастера параметр расширения оповещений в Azure будет удален со страницы **Параметры оповещений**. Оповещения перемещаются в Azure в фоновом режиме, на что может потребоваться некоторое время. Во время проведения этой операции делать изменения оповещений на портале Operations Management Suite запрещено. Текущее состояние операции можно увидеть на баннере в верхней части портала. Если ранее вы предоставили адрес электронной почты, то при завершении процесса вы получите уведомление.  


Даже после того, как оповещения были успешно перемещены в Azure, они все еще будут отображаться на портале Operations Management Suite.
![Снимок экрана страницы "Параметры оповещений" портала Operations Management Suite](media/alerts-extend-tool/PostExtendList.png)


## <a name="option-2-use-the-alertsversion-api"></a>Вариант 2. Использование API AlertsVersion
Для расширения оповещений из Log Analytics в оповещения Azure из любого клиента, с которого можно выполнить вызов REST API, можно использовать API AlertsVersion Log Analytics. Использовав [ARMClient](https://github.com/projectkudu/ARMClient) — инструмент командной строки с открытым исходным кодом, можно получить доступ к API из PowerShell. Вывод результатов можно выполнить в формате JSON.  

Перед использованием API необходимо создать запрос GET. С помощью этого запроса можно оценить и возвратить сводку предложенных изменений, не предпринимая фактической попытки расширения в Azure с помощью запроса POST. Список результатов оповещений и предложенный список [Группы действий](../../azure-monitor/platform/action-groups.md) находятся в формате JSON. Если подобные действия будут выполняться для нескольких оповещений, служба предложит связать одну группу действий со всеми остальными. Соглашение об именовании выглядит следующим образом: *WorkspaceName_AG_#Number*.

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Если запрос GET выполнен успешно, то в качестве данных формата JSON будет возвращен код состояния HTTP со значением 200 вместе со списком оповещений и предложенных групп действий. Далее представлен пример ответа на запрос:

```json
{
    "version": 1,
    "migrationSummary": {
        "alertsCount": 2,
        "actionGroupsCount": 2,
        "alerts": [
            {
                "alertName": "DemoAlert_1",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_1"
            },
            {
                "alertName": "DemoAlert_2",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_2"
            }
        ],
        "actionGroups": [
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                "actions": {
                    "emailIds": [
                        "JohnDoe@mail.com"
                    ],
                    "webhookActions": [
                        {
                            "name": "Webhook_1",
                            "serviceUri": "https://test.com"
                        }
                    ],
                    "itsmAction": {}
                }
            },
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                 "actions": {
                    "emailIds": [
                        "test1@mail.com",
                          "test2@mail.com"
                    ],
                    "webhookActions": [],
                    "itsmAction": {
                        "connectionId": "<Guid>",
                        "templateInfo":"{\"PayloadRevision\":0,\"WorkItemType\":\"Incident\",\"UseTemplate\":false,\"WorkItemData\":\"{\\\"contact_type\\\":\\\"email\\\",\\\"impact\\\":\\\"3\\\",\\\"urgency\\\":\\\"2\\\",\\\"category\\\":\\\"request\\\",\\\"subcategory\\\":\\\"password\\\"}\",\"CreateOneWIPerCI\":false}"
                    }
                }
            }
        ]
    }
}

```
Если в указанной рабочей области не установлено никаких определенных правил, в формате JSON будут представлены следующие данные:

```json
{
    "version": 1,
    "Message": "No Alerts found in the workspace for migration."
}
```

Если все правила оповещений в указанной рабочей области уже были расширены в Azure, на запрос GET будет получен следующий ответ:

```json
{
    "version": 2
}
```

Чтобы начать миграцию оповещений в Azure, инициируйте ответ POST. Ответ POST подтверждает выбранные намерения, также как и принятие условия, чтобы оповещения могли быть расширены с Log Analytics в оповещения Azure. Основываясь на результатах, полученных ранее во время выполнения ответа GET, действие выполняется по расписанию и оповещения обрабатываются так, как было сказано. При необходимости можно предоставить список адресов электронной почты, на которые Log Analytics отправит отчет об успешном завершении запланированного фонового процесса миграции оповещений. Можно использовать следующий пример запроса.

```
$emailJSON = “{‘Recipients’: [‘a@b.com’, ‘b@a.com’]}”
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $emailJSON
```

> [!NOTE]
> Результат миграции оповещений в оповещения Azure может изменяться в зависимости от сводной информации, предоставленной ответом GET. Оповещения, которые находятся в Log Analytics, временно недоступны для изменения на портале Operations Management Suite во время планирования. Тем не менее оповещение можно создать. 

При успешном выполнении запроса POST он возвращает состояние HTTP 200 OK вместе со следующим ответом:

```json
{
    "version": 2
}
```

Он указывает, что оповещение было успешно расширено в оповещения Azure. Эта версия свойства используется, только чтобы проверить, было ли оповещение расширено в Azure и является ли оно связанным с [API поиска по службе Log Analytics](../../azure-monitor/platform/api-alerts.md). При успешном расширении оповещений в Azure отчет будет отправлен на все адреса электронной почты, которые входят в запрос POST. Если все оповещения в указанной рабочей области уже запланированы для перемещения, ответом на запрос POST будет сообщение о том, что попытка перемещения запрещена (код состояния 403). Чтобы просмотреть сообщения об ошибке или понять, на каком этапе процесса возникли проблемы, можно отправить запрос GET. Если в качестве ответа будет получено сообщение об ошибке, оно будет возвращено вместе со сводными сведениями.

```json
{
    "version": 1,
    "message": "OMS was unable to extend your alerts into Azure, Error: The subscription is not registered to use the namespace 'microsoft.insights'. OMS will schedule extending your alerts, once remediation steps illustrated in the troubleshooting guide are done.",
    "recipients": [
       "john.doe@email.com",
       "jane.doe@email.com"
     ],
    "migrationSummary": {
        "alertsCount": 2,
        "actionGroupsCount": 2,
        "alerts": [
            {
                "alertName": "DemoAlert_1",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_1"
            },
            {
                "alertName": "DemoAlert_2",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_2"
            }
        ],
        "actionGroups": [
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                "actions": {
                    "emailIds": [
                        "JohnDoe@mail.com"
                    ],
                    "webhookActions": [
                        {
                            "name": "Webhook_1",
                            "serviceUri": "https://test.com"
                        }
                    ],
                    "itsmAction": {}
                }
            },
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                 "actions": {
                    "emailIds": [
                        "test1@mail.com",
                          "test2@mail.com"
                    ],
                    "webhookActions": [],
                    "itsmAction": {
                        "connectionId": "<Guid>",
                        "templateInfo":"{\"PayloadRevision\":0,\"WorkItemType\":\"Incident\",\"UseTemplate\":false,\"WorkItemData\":\"{\\\"contact_type\\\":\\\"email\\\",\\\"impact\\\":\\\"3\\\",\\\"urgency\\\":\\\"2\\\",\\\"category\\\":\\\"request\\\",\\\"subcategory\\\":\\\"password\\\"}\",\"CreateOneWIPerCI\":false}"
                    }
                }
            }
        ]
    }
}              

```

## <a name="troubleshooting"></a>Устранение неполадок 
Во время процесса расширения оповещений могут возникнуть проблемы, из-за которых система не сможет создать необходимые [группы действий](../../azure-monitor/platform/action-groups.md). В таких случаях на баннере раздела **Оповещение** портала Operations Management Suite либо в вызове GET, который выполняется для всех API, будет отображено сообщение об ошибке.

> [!IMPORTANT]
> Если до 15 марта 2019 г. пользователи портала OMS на базе облака Azure для государственных организаций не предпримут следующие действия по исправлению, оповещения продолжат выполняться в Azure, но любое их действие или уведомление работать не будет. Для получения уведомлений об оповещениях необходимо вручную изменить соответствующие правила генерации оповещений в Azure и добавить [группы действий](../../azure-monitor/platform/action-groups.md).

Здесь перечислены действия по исправлению для каждой возможной ошибки.
- **Ошибка. Блокировка области присутствует на уровне группы подписки или ресурсов для операций записи**.   ![Снимок экрана страницы параметров оповещений портала Operations Management Suite с выделенным сообщением об ошибке блокировки области](media/alerts-extend-tool/ErrorScopeLock.png)

    Когда функция "Блокировка области" включена, она ограничивает любое изменение в подписке или группе ресурсов, которое содержится в рабочей области Log Analytics (Operations Management Suite). Системе не удалось расширить оповещения в Azure или создать необходимые группы.
    
    Чтобы устранить эту проблему, из подписки или группы ресурсов, которые содержат рабочую область, необходимо удалить блокировку *ReadOnly*. Это можно сделать с помощью портала Azure, PowerShell, Azure CLI или API. Дополнительные сведения см. в статье [Блокировка ресурсов для предотвращения непредвиденных изменений](../../azure-resource-manager/resource-group-lock-resources.md). 
    
    После устранения проблемы с помощью действий, приведенных в статье, Operations Management Suite расширяет оповещения в Azure на следующий день в рамках запланированного запуска. Выполнять дальнейшие действия или инициировать что-либо не требуется.

- **Ошибка. Политика присутствует на уровне подписки и группы ресурсов**.   ![Снимок экрана страницы параметров оповещений портала Operations Management Suite с выделенным сообщением об ошибке политики](media/alerts-extend-tool/ErrorPolicy.png)

    Когда применяется [политика Azure](../../governance/policy/overview.md), она ограничивает любые ресурсы, созданные в подписке или группе ресурсов, которые содержатся в рабочей области Log Analytics (Operations Management Suite). Системе не удалось расширить оповещения в Azure или создать необходимые группы.
    
    Для устранения отредактируйте политику, которая вызвала ошибку *[RequestDisallowedByPolicy](../../azure-resource-manager/resource-manager-policy-requestdisallowedbypolicy-error.md)*, препятствующую созданию новых ресурсов в вашей подписке или группе ресурсов, в которых содержится рабочая область. Это можно сделать с помощью портала Azure, PowerShell, Azure CLI или API. Чтобы найти соответствующую политику, из-за которой происходит ошибка, можно использовать аудит действий. Дополнительные сведения см. в статье [Просмотр журналов действий для аудита действий с ресурсами](../../azure-resource-manager/resource-group-audit.md). 
    
    После устранения проблемы с помощью действий, приведенных в статье, Operations Management Suite расширяет оповещения в Azure на следующий день в рамках запланированного запуска. Выполнять дальнейшие действия или инициировать что-либо не требуется.


## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения см. в статье [Функция "Унифицированные оповещения" в Azure Monitor](../../azure-monitor/platform/alerts-overview.md).
* Дополнительные сведения см. в статье [Оповещения журнала в Azure Monitor. Оповещения Azure (предварительная версия)](alerts-unified-log.md).

