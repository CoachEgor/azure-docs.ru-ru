---
title: Архивация журналов диагностики Azure
description: Узнайте, как выполнить архивацию журналов диагностики Azure для долгосрочного хранения в учетной записи хранения.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: 82aaa573c55748daf62b620cdd82561bae6af492
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60345997"
---
# <a name="archive-azure-diagnostic-logs"></a>Архивация журналов диагностики Azure

В этой статье описано, как настроить архивацию [журналов диагностики Azure](../../azure-monitor/platform/diagnostic-logs-overview.md) в учетной записи хранения с помощью портала Azure, командлетов PowerShell, интерфейса командной строки или REST API. Архивацию целесообразно применять, если вам нужно хранить журналы диагностики с использованием необязательной политики хранения для аудита, статического анализа или резервного копирования. Учетная запись хранения не обязательно должна находиться в той самой подписке, в которой находится ресурс, выдающий журналы, если у пользователя, настраивающего параметр, имеется соответствующий доступ RBAC к обеим подпискам.

> [!WARNING]
> Формат данных журнала в учетной записи хранения будет изменен на JSON Lines с 1 ноября 2018 г. [См. дополнительные сведения, включая информацию об обновлении инструментария для включения поддержки нового формата.](./../../azure-monitor/platform/diagnostic-logs-append-blobs.md) 
>
> 

## <a name="prerequisites"></a>Технические условия

Перед началом работы необходимо [создать учетную запись хранения](../../storage/common/storage-quickstart-create-account.md) для архивации журналов диагностики. Настоятельно рекомендуется не использовать имеющуюся учетную запись хранения, в которой содержатся другие данные (не данные мониторинга), чтобы лучше контролировать доступ к данным мониторинга. Но если в учетную запись хранения вы архивируете журнал действий и метрики диагностики, целесообразно использовать эту учетную запись и для хранения журналов диагностики. Так все данные мониторинга будут храниться в одном расположении.

> [!NOTE]
>  Сейчас невозможно архивировать данные в учетную запись хранения, которая находится в защищенной виртуальной сети.

## <a name="diagnostic-settings"></a>Параметры диагностики

Чтобы архивировать журналы диагностики с помощью любого из описанных ниже методов, укажите **параметр диагностики** для конкретного ресурса. Параметр диагностики для ресурса определяет категории журналов и данные метрик, отправляемых в место назначения (учетной записи хранения, пространство имен концентраторов событий или рабочую область Log Analytics). Он также определяет политику хранения событий (продолжительность хранения в днях) для событий каждой категории журналов и данных метрик, содержащихся в учетной записи хранения. Если для политики хранения задано значение 0, события для такой категории журналов хранятся неограниченное время, то есть они сохраняются навсегда. Для политики хранения можно задать любое значение от 1 до 2 147 483 647. [Подробные сведения о параметрах диагностики см. здесь](../../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings). Политики хранения применяются по дням, поэтому в конце дня (по времени в формате UTC) журналы, срок которых теперь превышает период хранения, будут удалены. Например, если настроена политика хранения в течение одного дня, то в начале текущего дня журналы за вчерашний день будет удалены. Удаление начинается в полночь по времени UTC. Обратите внимание, что удаление журналов из учетной записи хранения может занять до 24 часов. 

> [!NOTE]
> Отправка многомерных метрик с помощью параметров диагностики сейчас не поддерживается. Метрики с измерениями экспортируются как преобразованные в плоскую структуру одномерные метрики, агрегированные по значениям измерений.
>
> *Пример*. Метрику "Входящие сообщения" в концентраторе событий можно изучить и вывести в виде диаграммы на уровне очереди. Но при экспорте с помощью параметров диагностики метрика будет представлена в виде всех входящих сообщений для всех очередей концентратора событий.
>
>

## <a name="archive-diagnostic-logs-using-the-portal"></a>Архивация журналов диагностики с помощью портала

1. На портале перейдите к Azure Monitor и щелкните **Параметры диагностики**.

    ![Раздел мониторинга Azure Monitor](media/archive-diagnostic-logs/diagnostic-settings-blade.png)

2. При необходимости отфильтруйте список по группе или типу ресурса, а затем щелкните ресурс, для которого необходимо задать параметр диагностики.

3. Если параметров для выбранного ресурса не существует, вам будет предложено создать параметр. Щелкните Turn on diagnostics (Включить диагностику).

   ![Добавление параметра диагностики — имеющиеся параметры отсутствуют](media/archive-diagnostic-logs/diagnostic-settings-none.png)

   Если в ресурсе имеются параметры, для него отобразится список настроенных параметров. Нажмите Add diagnostic setting (Добавить параметр диагностики).

   ![Добавление параметра диагностики — имеющиеся параметры](media/archive-diagnostic-logs/diagnostic-settings-multiple.png)

3. Задайте имя параметра и установите флажок **Export to Storage Account** (Экспорт в учетную запись хранения), а затем выберите учетную запись хранения. При необходимости с помощью ползунков **Хранение (дни)** укажите продолжительность хранения этих журналов в днях. Нулевое значение означает, что журналы будут храниться неограниченно долго.

   ![Добавление параметра диагностики — имеющиеся параметры](media/archive-diagnostic-logs/diagnostic-settings-configure.png)

4. Выберите команду **Сохранить**.

Через несколько секунд появится новый параметр в списке параметров для данного ресурса, и сразу же после создания данных о событии журналы диагностики заархивируются в необходимую учетную запись хранения.

## <a name="archive-diagnostic-logs-via-azure-powershell"></a>Архивация журналов диагностики с помощью Azure PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

```
Set-AzDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Свойство | Обязательно для заполнения | ОПИСАНИЕ |
| --- | --- | --- |
| resourceId |Yes |Идентификатор ресурса, для которого будет указан параметр диагностики. |
| StorageAccountId |Нет  |Идентификатор учетной записи хранения, в которую будут сохранены журналы диагностики. |
| Categories |Нет  |Разделенный запятыми список категорий журналов, которые будут включены. |
| Enabled |Yes |Логическое значение, определяющее включение и отключение диагностики на этом ресурсе. |
| RetentionEnabled |Нет  |Логическое значение, определяющее включение политики хранения на этом ресурсе. |
| RetentionInDays |Нет  |Количество дней, в течение которых будут храниться события: от 1 до 2 147 483 647. Нулевое значение означает, что журналы хранятся неограниченно долго. |

## <a name="archive-diagnostic-logs-via-the-azure-cli"></a>Архивация журналов диагностики с помощью Azure CLI

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <name or ID of storage account> \
    --resource <target resource object ID> \
    --resource-group <storage account resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true,
        "retentionPolicy": {
            "days": <# days to retain>,
            "enabled": true
        }
    }]'
```

Вы можете расширить набор категорий в журнале диагностики, добавив словари в массив JSON, переданный в качестве параметра `--logs`.

Аргумент `--resource-group` является обязательным, только если `--storage-account` — не идентификатор объекта. Полную документация по архивации журналов диагностики в хранилище см. в [справочнике по командам CLI](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create).

## <a name="archive-diagnostic-logs-via-the-rest-api"></a>Архивация журналов диагностики с помощью REST API

Подробные сведения о настройке параметра диагностики с помощью REST API Azure Monitor см. [здесь](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings).

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a>Схема журналов диагностики в учетной записи хранилища

Когда вы настроите архивацию, в учетной записи хранения будет создан контейнер хранилища, как только в одной из включенных категорий журналов возникнет событие журнала. Для больших двоичных объектов в контейнере используется то же соглашение об именовании, что и для журналов действий и журналов диагностики, как показано ниже:

```
insights-logs-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/RESOURCEGROUPS/{resource group name}/PROVIDERS/{resource provider name}/{resource type}/{resource name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Например, большой двоичный объект может иметь такое имя:

```
insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Каждый большой двоичный объект PT1H.json содержит большой двоичный объект JSON с событиями, произошедшими в течение часа, указанного в URL-адресе этого объекта (например, h=12). В течение заданного часа события добавляются в файл PT1H.json по мере возникновения. Значение минуты (m=00) всегда равно 00, так как события журнала диагностики разбиваются на отдельные большие двоичные объекты каждый час.

В файле PT1H.json каждое событие сохраняется в массиве records в следующем формате:

``` JSON
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| Имя элемента | ОПИСАНИЕ |
| --- | --- |
| time |Метка времени, когда служба Azure создала событие при обработке соответствующего этому событию запроса. |
| resourceId |Идентификатор ресурса для затронутого ресурса. |
| operationName |Имя операции. |
| category |Категория журналов для события. |
| properties |Набор пар `<Key, Value>` (например, Dictionary) c подробным описанием события. |

> [!NOTE]
> Свойства и их использование зависят от ресурса.

## <a name="next-steps"></a>Дальнейшие действия

* [Скачивание больших двоичных объектов для анализа](../../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Потоковая передача журналов диагностики Azure в пространство имен Центров событий](../../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md)
* [Руководство. Архивация журналов Azure Active Directory в учетной записи хранения Azure (предварительная версия)](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md)
* [Сбор и использование данных журнала из ресурсов Azure](../../azure-monitor/platform/diagnostic-logs-overview.md)

