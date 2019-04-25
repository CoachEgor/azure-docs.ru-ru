---
title: Архивация журнала действий Azure
description: Архивация журнала действий Azure для его долгосрочного хранения в учетной записи хранения
author: nkiest
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: nikiest
ms.subservice: logs
ms.openlocfilehash: b6009471048232b52020e4bef6272ed8cb1bd35b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60345855"
---
# <a name="archive-the-azure-activity-log"></a>Архивация журнала действий Azure
В этой статье описано, как настроить архивацию [**журнала действий Azure**](../../azure-monitor/platform/activity-logs-overview.md) в учетной записи хранения с помощью портала Azure, командлетов PowerShell или кроссплатформенного интерфейса командной строки. Архивацию целесообразно применять, если вам нужно хранить данные журнала действий дольше 90 дней (с полным контролем над политикой хранения) для аудита, статического анализа или резервного копирования. Если вам требуется хранить события в течение не более 90 дней, не нужно настраивать архивацию в учетную запись хранения, так как события журнала действий можно хранить в течение этого периода на платформе Azure.

## <a name="prerequisites"></a>Предварительные требования
Прежде чем начать, необходимо [создать учетную запись хранения](../../storage/common/storage-quickstart-create-account.md) для архивации данных журнала действий. Настоятельно рекомендуется не использовать имеющуюся учетную запись хранения, в которой содержатся другие данные (не данные мониторинга), чтобы лучше контролировать доступ к данным мониторинга. Но если вы также архивируете в учетную запись хранения журналы диагностики и метрики, целесообразно использовать эту учетную запись и для хранения данных журнала действий. Так все данные мониторинга будут храниться в одном расположении. Учетная запись хранения не обязательно должна находиться в той самой подписке, в которой создаются журналы, если у пользователя, настраивающего параметр, имеется соответствующий доступ RBAC к обеим подпискам.

## <a name="log-profile"></a>Профиль журнала
Чтобы настроить архивацию журнала действий с помощью одного из описанных здесь методов, необходимо настроить **профиль журнала** для подписки. Профиль журнала определяет тип событий, которые хранятся или передаются потоком, и выходных данных — учетная запись хранения и/или концентратор событий. Он также определяет политику хранения событий (число дней), содержащихся в учетной записи хранения. Если для политики хранения задано значение 0, события хранятся неограниченное время. В противном случае это может быть присвоено любое значение от 1 до 365. Политики хранения применяются по дням, поэтому в конце дня (по времени в формате UTC) журналы, срок которых теперь превышает период хранения, будут удалены. Например, если настроена политика хранения в течение одного дня, то в начале текущего дня журналы за вчерашний день будет удалены. Удаление начинается в полночь по времени UTC. Обратите внимание, что удаление журналов из учетной записи хранения может занять до 24 часов. Дополнительные сведения о профилях журнала см. [здесь](../../azure-monitor/platform/activity-logs-overview.md#export-the-activity-log-with-a-log-profile). 

## <a name="archive-the-activity-log-using-the-portal"></a>Архивация журнала действий с помощью портала
1. На портале щелкните ссылку **Журнал действий** на панели навигации слева. Если вы не видите эту ссылку, сначала щелкните ссылку **Все службы**.
   
    ![Переход к колонке журнала действий](media/archive-activity-log/activity-logs-portal-navigate-v2.png)
2. В верхней части колонки щелкните **Экспорт в концентратор событий**.
   
    ![Нажатие кнопки "Экспорт"](media/archive-activity-log/activity-logs-portal-export-v2.png)
3. В открывшейся колонке установите флажок **Export to a storage account** (Экспортировать в учетную запись хранения) и выберите учетную запись хранения.
   
    ![Настройка учетной записи хранения](media/archive-activity-log/act-log-portal-export-blade.png)
4. С помощью ползунка или текстового поля задайте число дней, в течение которых необходимо хранить события журнала действий в учетной записи хранения (от 0 до 365 дней). Если требуется, чтобы данные хранились в учетной записи хранения бесконечно, задайте значение 0. Если необходимо указать число больше 365, воспользуйтесь PowerShell или интерфейсом командной строки, как описано ниже.
5. Выберите команду **Сохранить**.

## <a name="archive-the-activity-log-via-powershell"></a>Архивация журнала действий с помощью PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

   ```powershell
   # Settings needed for the new log profile
   $logProfileName = "default"
   $locations = (Get-AzLocation).Location
   $locations += "global"
   $subscriptionId = "<your Azure subscription Id>"
   $resourceGroupName = "<resource group name your storage account belongs to>"
   $storageAccountName = "<your storage account name>"

   # Build the storage account Id from the settings above
   $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

   Add-AzLogProfile -Name $logProfileName -Location $locations -StorageAccountId $storageAccountId
   ```

| Свойство | Обязательно для заполнения | ОПИСАНИЕ |
| --- | --- | --- |
| StorageAccountId |Yes |Идентификатор ресурса для учетной записи хранения, в которую будут сохранены журналы действий. |
| Расположения |Yes |Разделенный запятыми список регионов, для которых будут собираться события журнала действий. Вы можете просмотреть список всех регионов для своей подписки с помощью `(Get-AzLocation).Location`. |
| RetentionInDays |Нет  |Число дней, какие события должны сохраняться, от 1 до 365. Нулевое значение означает, что журналы хранятся неограниченно долго, то есть всегда. |
| Categories |Нет  |Разделенный запятыми список категорий событий, которые будут собираться. Возможные значения: Write, Delete или Action.  Если значение не указано, предполагается наличие всех возможных значений. |

## <a name="archive-the-activity-log-via-cli"></a>Архивация журнала действий с помощью интерфейса командной строки

   ```azurecli-interactive
   az monitor log-profiles create --name "default" --location null --locations "global" "eastus" "westus" --categories "Delete" "Write" "Action"  --enabled false --days 0 --storage-account-id "/subscriptions/<YOUR SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>"
   ```

| Свойство | Обязательно для заполнения | ОПИСАНИЕ |
| --- | --- | --- |
| name |Yes |Имя профиля журнала. |
| storage-account-id |Yes |Идентификатор ресурса для учетной записи хранения, в которую будут сохранены журналы действий. |
| Расположения |Yes |Разделенный пробелами список регионов, для которых будут собираться события журнала действий. Вы можете просмотреть список всех регионов для своей подписки с помощью `az account list-locations --query [].name`. |
| days |Yes |Число дней, какие события должны сохраняться, от 1 до 365. Нулевое значение означает, что журналы будут храниться неограниченно долго, то есть всегда.  Если значение равно нулю, для включенного параметра нужно установить значение true. |
|Включено | Yes |Значение True или False.  Позволяет включить или отключить политику хранения.  Если установлено значение True, параметр дней должен иметь значение больше 0.
| Категории |Yes |Разделенный пробелами список категорий событий, которые будут собираться. Возможные значения: Write, Delete или Action. |

## <a name="storage-schema-of-the-activity-log"></a>Схема хранения журнала действий
После настройки архивации в учетной записи хранения будет создан контейнер хранилища, как только возникнет событие журнала. Для больших двоичных объектов в контейнере используется то же соглашение об именовании, что и для журналов действий и журналов диагностики, как показано ниже:

```
insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/{subscription ID}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Например, большой двоичный объект может иметь такое имя:

```
insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Каждый большой двоичный объект PT1H.json содержит большой двоичный объект JSON с событиями, произошедшими в течение часа, указанного в URL-адресе этого объекта (например, h=12). В течение заданного часа события добавляются в файл PT1H.json по мере возникновения. Значение минуты (m=00) всегда равно 00, так как события журнала действий разбиваются на отдельные большие двоичные объекты каждый час.

В файле PT1H.json каждое событие сохраняется в массиве records в следующем формате:

``` JSON
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
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
| category |Категория действия, например "Запись", "Чтение", "Действие" |
| resultType |Тип результата, например "Успешно", "Ошибка", "Запуск" |
| resultSignature |Зависит от типа ресурса. |
| durationMs |Время выполнения операции в миллисекундах |
| callerIpAddress |IP-адрес пользователя, который выполнил операцию, утверждение имени субъекта-службы или имени участника-пользователя в зависимости от доступности. |
| correlationId |Обычно GUID в строковом формате. События, которые совместно используют идентификатор correlationId, принадлежат к одному общему действию. |
| identity |Большой двоичный объект JSON, описывающий авторизацию и утверждения. |
| authorization |BLOB-объект со свойствами RBAC события. Обычно включает следующие свойства: action, role и scope. |
| level |Уровень события. Одно из следующих значений: Critical (Критическое), Error (Ошибка), Warning (Предупреждение), Informational (Информационное) или Verbose (Подробные сведения) |
| location |Регион, к которому принадлежит расположение (или глобальное местоположение). |
| properties |Набор пар `<Key, Value>` (например, Dictionary) c подробным описанием события. |

> [!NOTE]
> Свойства и их использование зависят от ресурса.
> 
> 

## <a name="next-steps"></a>Дальнейшие действия
* [Скачивание больших двоичных объектов для анализа](../../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Потоковая передача журнала действий в Центры событий](../../azure-monitor/platform/activity-logs-stream-event-hubs.md)
* [Дополнительные сведения о журнале действий](../../azure-monitor/platform/activity-logs-overview.md)


