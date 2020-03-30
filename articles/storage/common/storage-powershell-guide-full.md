---
title: Использование Azure PowerShell со службой хранилища Azure | Документация Майкрософт
description: Узнайте, как использовать командлеты Azure PowerShell для службы хранилища Azure.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/16/2018
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: d2404ee58f5f44fbe5625f267e6d1c504d0bd237
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "75465092"
---
# <a name="using-azure-powershell-with-azure-storage"></a>Использование Azure PowerShell со службой хранилища Azure

PowerShell используется для создания ресурсов Azure и управления ими с помощью командной строки PowerShell или сценариев. Для службы хранилища Azure эти командлеты делятся на две категории — уровня управления и уровня данных. Командлеты уровня управления используются для операций управления учетной записью хранения: создания и удаления учетных записей хранения, установки свойств, смены ключей доступа и т. д. Командлеты уровня данных используются для операций управления данными *в* учетной записи хранения: отправки больших двоичных объектов, создания общих файловых ресурсов и добавления сообщений в очередь.

В этом практическом руководстве описаны стандартные операции с использованием командлетов уровня управления для учетных записей хранения. Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Вывод списка учетных записей хранения.
> * Получение ссылки на имеющуюся учетную запись хранения.
> * Создание учетной записи хранения
> * Настройка свойств учетной записи хранения.
> * Получение и повторное создание ключей доступа.
> * Защита доступа к учетной записи хранения
> * Включение Аналитики Службы хранилища

В этой статье приводятся ссылки на несколько других статей об использовании PowerShell со службой хранилища Azure. В них описывается, как включать Аналитику Службы хранилища и обращаться к ней, как использовать командлеты плоскости данных и как обращаться к независимым облакам Azure для Китая и Германии, а также к облаку для государственных организаций.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Для работы с этим упражнением требуется модуль Az Azure PowerShell версии 0.7 или более поздней. Чтобы узнать версию, выполните команду `Get-Module -ListAvailable Az`. Если вам необходимо выполнить установку или обновление, см. статью [об установке модуля Azure PowerShell](/powershell/azure/install-Az-ps).

Для этого упражнения команды можно вводить в обычное окно PowerShell или в редактор [интегрированной среды сценариев Windows PowerShell (ISE)](/powershell/scripting/components/ise/exploring-the-windows-powershell-ise), а затем протестировать одну или несколько из них во время выполнения этих примеров. Вы можете выделить строки, которые необходимо выполнить, и щелкнуть Run Selected (Запустить выделенные), чтобы выполнить эти команды.

Дополнительные сведения об учетных записях хранения см. в статьях [Введение в хранилище Microsoft Azure](storage-introduction.md) и [Об учетных записях хранения Azure](storage-create-storage-account.md).

## <a name="sign-in-to-azure"></a>Вход в Azure

Войдите в подписку Azure с помощью команды `Connect-AzAccount` и следуйте инструкциям на экране.

```powershell
Connect-AzAccount
```

## <a name="list-the-storage-accounts-in-the-subscription"></a>Вывод списка учетных записей хранения в подписке

Выполните командлет [Get-AzStorageAccount](/powershell/module/az.storage/Get-azStorageAccount), чтобы получить список учетных записей хранения в текущей подписке.

```powershell
Get-AzStorageAccount | Select StorageAccountName, Location
```

## <a name="get-a-reference-to-a-storage-account"></a>Получение ссылки на учетную запись хранения

Затем потребуется получить ссылку на учетную запись хранения. Вы можете создать учетную запись хранения или получить ссылку на имеющуюся. В следующих разделах продемонстрированы оба способа.

### <a name="use-an-existing-storage-account"></a>Использование имеющейся учетной записи хранения

Для извлечения имеющейся учетной записи хранения вам понадобится имя группы ресурсов и учетной записи хранения. Задайте переменные для этих двух полей, а затем используйте командлет [Get-AzStorageAccount](/powershell/module/az.storage/Get-azStorageAccount).

```powershell
$resourceGroup = "myexistingresourcegroup"
$storageAccountName = "myexistingstorageaccount"

$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName
```

Теперь у вас есть переменная $storageAccount, указывающая на имеющуюся учетную запись хранения.

### <a name="create-a-storage-account"></a>Создание учетной записи хранения

В следующем сценарии показано, как создать учетную запись хранения общего назначения с помощью команды [New-AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount). После создания учетной записи извлеките ее контекст, который можно использовать в последующих командах вместо указания аутентификации при каждом вызове.

```powershell
# Get list of locations and select one.
Get-AzLocation | select Location
$location = "eastus"

# Create a new resource group.
$resourceGroup = "teststoragerg"
New-AzResourceGroup -Name $resourceGroup -Location $location

# Set the name of the storage account and the SKU name.
$storageAccountName = "testpshstorage"
$skuName = "Standard_LRS"

# Create the storage account.
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName $skuName

# Retrieve the context.
$ctx = $storageAccount.Context
```

В этом сценарии используются следующие командлеты PowerShell.

*   [Get-AzLocation](/powershell/module/az.resources/get-azlocation) — для получения списка допустимых расположений. В этом примере в качестве расположения используется `eastus`.

*   [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) — для создания группы ресурсов. Группа ресурсов — это логический контейнер, в котором происходит развертывание ресурсов Azure и управление ими. Наша группа ресурсов называется `teststoragerg`.

*   [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) — для создания учетной записи хранения. В этом примере используется `testpshstorage`.

Имя SKU определяет тип репликации учетной записи хранения (например, локально избыточное хранилище). Для получения дополнительной информации о репликации можно ознакомиться на [канале Azure Storage Replication.](storage-redundancy.md)

> [!IMPORTANT]
> Имя для учетной записи хранения является уникальным в пределах Azure и должно содержать только строчные буквы. Соглашения об именовании и ограничениях см. в статье [Naming and Referencing Containers, Blobs, and Metadata](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) (Именование контейнеров, больших двоичных объектов и метаданных и ссылка на них).
>

Теперь у вас есть учетная запись хранения и ссылка на нее.

## <a name="manage-the-storage-account"></a>Управление учетной записью хранения

В следующем разделе показаны некоторые команды, которые можно использовать для управления новой или имеющейся учетной записью хранения, имея на нее ссылку.

### <a name="storage-account-properties"></a>Свойства учетной записи хранения

Чтобы изменить параметры для учетной записи хранения, используйте командлет [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount). Несмотря на то что невозможно изменить расположение учетной записи хранения или группу ресурсов, в которой она находится, вы можете изменить множество других свойств. Ниже перечислены некоторые свойства, которые можно изменить с помощью PowerShell.

* **Личный домен**, присвоенный учетной записи хранения.

* **Теги**, присвоенные учетной записи хранения. Теги часто используются для классификации ресурсов в целях выставления счетов.

* **SKU** — это параметр репликации для учетной записи хранения (как, например, LRS для локально избыточного хранилища). Например, можно изменить Standard\_LRS на Standard\_GRS или Standard\_RAGRS. Обратите внимание, что вы\_не можете\_изменить стандартные\_ЗРС, Стандартные ГЗРС, Стандартные РАГЗРС или Премиум\_LRS на другие СКУ, или изменить другие СКУ на них.

* **Уровень доступа** для учетных записей хранилища BLOB-объектов. Для уровня доступа задано значение **Горячий** или **Холодный**, что позволяет свести к минимуму затраты, выбрав уровень доступа, соответствующий использованию учетной записи хранения. Дополнительные сведения см. в статье [Хранилище BLOB-объектов Azure: "горячий", "холодный" и архивный (предварительная версия) уровни](../blobs/storage-blob-storage-tiers.md).

* Разрешение только трафика по протоколу HTTPS.

### <a name="manage-the-access-keys"></a>Управление ключами доступа

Учетная запись хранилища Azure предоставляется с двумя ключами учетной записи. Чтобы получить ключи, используйте командлет [Get-AzStorageAccountKey](/powershell/module/az.Storage/Get-azStorageAccountKey). В этом примере извлекается первый ключ. Получите другой ключ, использовав `Value[1]` вместо `Value[0]`.

```powershell
$storageAccountKey = `
    (Get-AzStorageAccountKey `
    -ResourceGroupName $resourceGroup `
    -Name $storageAccountName).Value[0]
```

Чтобы повторно создать ключ, используйте [New-AzStorageAccountKey](/powershell/module/az.Storage/New-azStorageAccountKey).

```powershell
New-AzStorageAccountKey -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -KeyName key1
```

Чтобы повторно создать другой ключ, используйте в качестве имени ключа значение `key2`, а не `key1`.

Повторно создайте один из ключей, а затем извлеките его снова, чтобы увидеть новое значение.

> [!NOTE]
> Необходимо тщательно спланировать повторное создание ключа для рабочей учетной записи хранения. Повторное создание одного или обоих ключей приведет к тому, что с помощью повторно созданного ключа невозможно будет получить доступ к любому приложению. См. сведения о том, как [управлять ключами доступа к учетной записи хранения](storage-account-keys-manage.md).


### <a name="delete-a-storage-account"></a>Удаление учетной записи хранения

Чтобы удалить учетную запись хранения, используйте командлет [Remove-AzStorageAccount](/powershell/module/az.storage/Remove-azStorageAccount).

```powershell
Remove-AzStorageAccount -ResourceGroup $resourceGroup -AccountName $storageAccountName
```

> [!IMPORTANT]
> При удалении учетной записи хранения также удаляются все ресурсы, которые хранятся в ней. Если учетная запись удалена случайно, немедленно обратитесь в службу поддержки и создайте запрос на восстановление учетной записи хранения. Восстановление данных не гарантируется, но в некоторых случаях это возможно. Не создавайте учетную запись хранения с тем же именем, что и старая, до тех пор, пока не будет решена проблема по запросу в службе поддержки.
>

### <a name="protect-your-storage-account-using-vnets-and-firewalls"></a>Защита учетной записи хранения с помощью виртуальных сетей и брандмауэров

По умолчанию все учетные записи хранения можно получить с помощью сети с подключением к Интернету. Однако можно настроить сетевые правила, чтобы разрешить приложениям из определенных виртуальных сетей получать доступ к учетной записи хранения. Для получения дополнительной информации [см.](storage-network-security.md)

В данной статье показано, как управлять этими параметрами, используя следующие командлеты PowerShell:
* [Add-AzStorageAccountNetworkRule](/powershell/module/az.Storage/Add-azStorageAccountNetworkRule)
* [Update-AzStorageAccountNetworkRuleSet.](/powershell/module/az.storage/update-azstorageaccountnetworkruleset)
* [Remove-AzStorageAccountNetworkRule](https://docs.microsoft.com/powershell/module/az.storage/remove-azstorageaccountnetworkrule)

## <a name="use-storage-analytics"></a>Использование Аналитики Службы хранилища

[Решение "Аналитика Службы хранилища Azure"](storage-analytics.md) состоит из [метрик](/rest/api/storageservices/about-storage-analytics-metrics) и [ведения журнала](/rest/api/storageservices/about-storage-analytics-logging) аналитики службы хранилища.

**Метрики аналитики службы хранилища** используются при сборе метрик для учетных записей хранения Azure, которые можно применять для отслеживания работоспособности учетной записи хранения. Метрики можно включить для больших двоичных объектов, файлов, таблиц и очередей.

**Ведение журналов аналитики службы хранилища** производится на стороне сервера и позволяет записывать сведения об успешных и неудачных запросах в вашей учетной записи хранения. Эти журналы позволяют просмотреть подробные сведения об операциях чтения, записи и удаления для таблиц, очередей и BLOB-объектов, а также причины неудачных запросов. Ведение журнала службы недоступно для службы файлов Azure.

Вы можете настроить мониторинг с помощью [портала Azure](https://portal.azure.com), PowerShell или программно, использовав клиентскую библиотеку хранилища.

> [!NOTE]
> Минутную аналитику можно включить с помощью PowerShell. Эта возможность недоступна на портале.
>

* Чтобы узнать, как включить и просматривать данные [Storage analytics metrics](storage-analytics-metrics.md)метрик хранения с помощью PowerShell, см.

* Сведения о том, как включить и получить журнал хранилища данных с помощью PowerShell, см. в статьях [How to enable Storage Logging using PowerShell](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data) (Включение ведения журнала хранилища с помощью PowerShell) и [Finding your Storage Logging log data](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data) (Поиск данных журнала хранилища).

* Подробнее об использовании метрик хранилища и ведения журнала для устранения неполадок хранилища см. в статье [Мониторинг, диагностика и устранение неполадок службы хранилища Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="manage-the-data-in-the-storage-account"></a>Управление данными в учетной записи хранения

Вы узнали о том, как управлять учетной записью хранения с помощью PowerShell. Изучите следующие статьи, в которых описано, как получить доступ к объектам данных в учетной записи хранения.

* [Выполнение операций в хранилище BLOB-объектов Azure с помощью Azure PowerShell](../blobs/storage-how-to-use-blobs-powershell.md)
* [Как использовать PowerShell для управления службой файлов Azure](../files/storage-how-to-use-files-powershell.md)
* [Выполнение операций хранилища очередей Azure с помощью Azure PowerShell](../queues/storage-powershell-how-to-use-queues.md)
* [Выполнение операций в хранилище таблиц Azure с помощью PowerShell](../../storage/tables/table-storage-how-to-use-powershell.md)

API таблицы Azure Cosmos DB предоставляет расширенные функции для хранения таблиц, такие как комплексные возможности глобального распределения, низкие задержки операций чтения и записи, автоматическое вторичное индексирование и выделенная пропускная способность.

* Дополнительные сведения см. в статье [Знакомство со службой Azure Cosmos DB. API таблицы](../../cosmos-db/table-introduction.md).

## <a name="independent-cloud-deployments-of-azure"></a>Независимые облачные развертывания Azure

Большинство людей используют общедоступное облако Azure для глобального развертывания Azure. Но есть и независимые развертывания Microsoft Azure для обеспечения автономности и других целей. Такие независимые развертывания называются средами. Эти доступные среды включают:

* [Azure Government Cloud (Облако Azure для государственных организаций)](https://azure.microsoft.com/features/gov/)
* [Облако Azure China 21Vianet эксплуатируется 21Vianet в Китае](http://www.windowsazure.cn/)
* [Облако Azure для Германии](../../germany/germany-welcome.md).

Дополнительные сведения о доступе к эти облакам и связанным службам хранилища с помощью PowerShell см. в руководстве по [управлению службой хранилища в независимых облаках Azure с помощью PowerShell](storage-powershell-independent-clouds.md).

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы создали группу ресурсов и учетную запись хранения для этого примера, удалите их, удалив группу ресурсов. При этом будут также удалены все ресурсы, содержащиеся в группе. В этом случае происходит удаление созданной учетной записи хранения и самой группы ресурсов.

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```
## <a name="next-steps"></a>Дальнейшие действия

В этом практическом руководстве описаны стандартные операции с использованием командлетов уровня управления для учетных записей хранения. Вы ознакомились с выполнением следующих задач:

> [!div class="checklist"]
> * Вывод списка учетных записей хранения.
> * Получение ссылки на имеющуюся учетную запись хранения.
> * Создание учетной записи хранения
> * Настройка свойств учетной записи хранения.
> * Получение и повторное создание ключей доступа.
> * Защита доступа к учетной записи хранения
> * Включение Аналитики Службы хранилища

В этой статье также приводятся ссылки на несколько других статей. В них описывается, как управлять объектами данных, как включать Аналитику Службы хранилища и как обращаться к независимым облакам Azure для Китая и Германии, а также к облаку для государственных организаций. Ниже приведены дополнительные связанные статьи и ресурсы:

* [AzureRM.Storage](/powershell/module/az.storage/)
* [Azure.Storage](/powershell/module/azure.storage/)
* [Справочник по Windows PowerShell](/powershell/scripting/overview)
