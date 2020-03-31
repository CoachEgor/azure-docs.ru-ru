---
title: "Мощность оболочки: Миграция сервера S'L в базу данных S'L"
titleSuffix: Azure Database Migration Service
description: Научитесь переходить с внутреннего сервера S'L на базу данных Azure S'L, используя Azure PowerShell с миграционной службой базы данных Azure.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 02/20/2020
ms.openlocfilehash: f63f79402b457017257f1762c6ddc7e04c0ee1af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "77650696"
---
# <a name="migrate-sql-server-on-premises-to-azure-sql-database-using-azure-powershell"></a>Перенос данных из локального экземпляра SQL Server в Базу данных SQL Azure с помощью Azure PowerShell

В этой статье выполняется перенос базы данных **Adventureworks2012**, восстановленной на локальном экземпляре SQL Server 2016 (или более поздней версии), в базу данных SQL Azure с помощью Microsoft Azure PowerShell. Вы можете выполнить перенос баз данных из локального экземпляра SQL Server в базу данных SQL Azure с помощью модуля `Az.DataMigration` в Microsoft Azure PowerShell.

Вы узнаете, как выполнять следующие задачи:
> [!div class="checklist"]
>
> * Создайте группу ресурсов.
> * создание экземпляра Azure Database Migration Service;
> * создание проекта миграции в экземпляре Azure Database Migration Service;
> * выполнение миграции.

## <a name="prerequisites"></a>Предварительные требования

Для выполнения этих действий вам потребуется следующее:

* [SQL Server 2016 или более поздней версии](https://www.microsoft.com/sql-server/sql-server-downloads) (любой выпуск).
* При установке SQL Server Express включите протокол TCP/IP, который отключен по умолчанию. Для этого выполните инструкции в разделе [Использование диспетчера конфигурации SQL Server](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure) статьи "Включение или отключение сетевого протокола сервера".
* [Настройте брандмауэр Windows для доступа к ядру СУБД](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
* Экземпляр Базы данных SQL Azure. Экземпляр базы данных SQL Azure можно создать, ознакомившись со сведениями в статье [Создание базы данных SQL Azure на портале Azure](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
* [Помощник по миграции данных](https://www.microsoft.com/download/details.aspx?id=53595) 3.3 или более поздней версии.
* Создание виртуальной сети Microsoft Azure с помощью модели развертывания диспетчера ресурсов Azure, которая обеспечивает миграционную службу базы данных Azure с подключением к исходным серверам с помощью [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) или [VPN.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)
* Для завершения оценки вашей предварительной базы данных и миграции схем с помощью помощника по миграции данных, описанного в статье [Выполнение оценки миграции сервера S'L](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem)
* Загрузить и установить модуль Az.DataMigration из галереи PowerShell с помощью [Install-Module PowerShell cmdlet;](https://docs.microsoft.com/powershell/module/powershellget/Install-Module?view=powershell-5.1) не забудьте открыть окно команды Powershell, используя запуск в качестве администратора.
* Убедитесь, что учетные данные, используемые для подключения к исходному экземпляру SQL Server, имеют разрешение [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql).
* Убедитесь, что учетные данные, используемые для подключения к целевому экземпляру Базы данных SQL Azure, имеют разрешение на управление базами данных в целевой базе данных SQL Azure.
* Подписка Azure. Если у вас его нет, создайте [бесплатную](https://azure.microsoft.com/free/) учетную запись, прежде чем начать.

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Вход в подписку Microsoft Azure

Чтобы войти в подписку Azure с помощью PowerShell, следуйте инструкциям в статье [Вход с помощью Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Группа ресурсов Azure является логическим контейнером, в котором происходит развертывание ресурсов Azure и управление ими. Прежде чем создать виртуальную машину, создайте группу ресурсов.

Создайте группу ресурсов, используя команду [New-AzResourceGroup.](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup)

В следующем примере создается группа ресурсов с именем *myResourceGroup* в регионе *EastUS*.

```powershell
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

## <a name="create-an-instance-of-azure-database-migration-service"></a>Создание экземпляра Azure Database Migration Service

Вы можете создать экземпляр Azure Database Migration Service, выполнив командлет `New-AzDataMigrationService`. Для этого командлета нужно передать следующие обязательные параметры:

* *Название группы ресурсов Azure*. Вы можете использовать команду [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) для создания группы ресурсов Azure, как показано ранее, и предоставления ее имени в качестве параметра.
* *Имя службы*. Строка, соответствующая требуемому уникальному имени службы для Azure Database Migration Service. 
* *Расположение*. Указывает расположение службы. Укажите расположение центра обработки данных Azure, например западная часть США или Юго-Восточная Азия.
* *Sku*. Этот параметр соответствует имени SKU DMS. Поддерживаемое сейчас имя Sku — *GeneralPurpose_4vCores*.
* *VirtualSubnetId.* Для создания подсети можно использовать cmdlet [New-AzVirtualNetworkSubnetConfig.](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig) 

В указанном ниже примере создается служба с именем *MyDMS* в группе ресурсов *MyDMSResourceGroup*, которая расположена в регионе *Восточная часть США*, с использованием виртуальной сети с именем *MyVNET* и подсети с именем *MySubnet*.

```powershell
 $vNet = Get-AzVirtualNetwork -ResourceGroupName MyDMSResourceGroup -Name MyVNET

$vSubNet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vNet -Name MySubnet

$service = New-AzDms -ResourceGroupName myResourceGroup `
  -ServiceName MyDMS `
  -Location EastUS `
  -Sku Basic_2vCores `  
  -VirtualSubnetId $vSubNet.Id`
```

## <a name="create-a-migration-project"></a>Создание проекта миграции

Создав экземпляр Azure Database Migration Service, создайте проект миграции. Для проекта Azure Database Migration Service необходимо иметь сведения о подключении для исходных и целевых экземпляров, а также список баз данных, которые требуется перенести как часть проекта.

### <a name="create-a-database-connection-info-object-for-the-source-and-target-connections"></a>Создание объекта сведений о подключении к базе данных для исходных и целевых подключений

Объект сведений о подключении к базе данных можно создать, выполнив командлет `New-AzDmsConnInfo`. Для этого командлета требуются следующие параметры:

* *ServerType*. Тип запрошенного подключения к базе данных, например SQL, Oracle или MySQL. Используйте SQL для SQL Server и SQL Azure.
* *DataSource*. Имя или IP-адрес экземпляра SQL Server или базы данных SQL Azure.
* *AuthType*. Тип проверки подлинности для подключения. Это может быть SqlAuthentication или WindowsAuthentication.
* Параметр *TrustServerCertificate* задает значение, указывающее, будет ли шифроваться канал при проходе по цепочке сертификатов для проверки доверия. Значение может быть true или false.

В приведенном ниже примере создается объект сведений о подключении для исходного SQL Server MySourceSQLServer с использованием проверки подлинности SQL:

```powershell
$sourceConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource MySourceSQLServer `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$true
```

В следующем примере показано создание сведений о подключении для сервера Базы данных SQL Azure с именем SQLAzureTarget с использованием проверки подлинности SQL.

```powershell
$targetConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource "sqlazuretarget.database.windows.net" `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$false
```

### <a name="provide-databases-for-the-migration-project"></a>Указание баз данных для проекта миграции

Создайте список объектов `AzDataMigrationDatabaseInfo`, в котором указаны базы данных, переносимые в рамках проекта миграции баз данных Azure. Его можно предоставить в виде параметра для создания проекта. Cmdlet `New-AzDataMigrationDatabaseInfo` может быть использован для создания AzDataMigrationDatabaseInfo. 

В следующем примере создается проект `AzDataMigrationDatabaseInfo` для базы данных **AdventureWorks2016**, который добавляется в список, чтобы его можно было предоставить в виде параметра для создания проекта.

```powershell
$dbInfo1 = New-AzDataMigrationDatabaseInfo -SourceDatabaseName AdventureWorks2016
$dbList = @($dbInfo1)
```

### <a name="create-a-project-object"></a>Создание объекта проекта

Наконец, можно создать проект миграции баз данных Azure с именем *MyDMSProject*, который расположен в *восточной части США*, выполнив команду `New-AzDataMigrationProject` и добавив раннее созданные исходные и целевые подключения и список баз данных для миграции.

```powershell
$project = New-AzDataMigrationProject -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName MyDMSProject `
  -Location EastUS `
  -SourceType SQL `
  -TargetType SQLDB `
  -SourceConnection $sourceConnInfo `
  -TargetConnection $targetConnInfo `
  -DatabaseInfo $dbList
```

## <a name="create-and-start-a-migration-task"></a>Создание и запуск задачи миграции

Наконец, создайте и запустите задачу миграции баз данных Azure. Помимо информации, предоставленной с проектом, созданным в качестве предварительного условия, для задачи миграции баз данных Azure необходимо иметь учетные данные для исходного и целевого подключения, а также список таблиц базы данных, которые необходимо перенести.

### <a name="create-credential-parameters-for-source-and-target"></a>Создание параметров учетных данных для исходного и целевого подключения

Учетные данные безопасности подключения можно создать в качестве объекта [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0).

В следующем примере показано создание объектов *PSCredential* для исходного и целевого подключения. Пароли представлены в качестве строковых переменных *$sourcePassword* и *$targetPassword*.

```powershell
$secpasswd = ConvertTo-SecureString -String $sourcePassword -AsPlainText -Force
$sourceCred = New-Object System.Management.Automation.PSCredential ($sourceUserName, $secpasswd)
$secpasswd = ConvertTo-SecureString -String $targetPassword -AsPlainText -Force
$targetCred = New-Object System.Management.Automation.PSCredential ($targetUserName, $secpasswd)
```

### <a name="create-a-table-map-and-select-source-and-target-parameters-for-migration"></a>Создание сопоставления таблицы и выбор исходных и целевых параметров для миграции

Еще один параметр, необходимый для миграции, — это сопоставление исходных и целевых таблиц для миграции. Создайте словарь таблиц, который обеспечивает сопоставление между исходными и целевыми таблицами для миграции. В следующем примере показано сопоставление между исходными и целевыми таблицами схемы управления персоналом для базы данных AdventureWorks 2016.

```powershell
$tableMap = New-Object 'system.collections.generic.dictionary[string,string]'
$tableMap.Add("HumanResources.Department", "HumanResources.Department")
$tableMap.Add("HumanResources.Employee","HumanResources.Employee")
$tableMap.Add("HumanResources.EmployeeDepartmentHistory","HumanResources.EmployeeDepartmentHistory")
$tableMap.Add("HumanResources.EmployeePayHistory","HumanResources.EmployeePayHistory")
$tableMap.Add("HumanResources.JobCandidate","HumanResources.JobCandidate")
$tableMap.Add("HumanResources.Shift","HumanResources.Shift")
```

Теперь необходимо выбрать исходную и целевую базы данных и обеспечить сопоставление мигрируемых таблиц в качестве параметра, выполнив командлет `New-AzDmsSelectedDB`, как показано в следующем примере:

```powershell
$selectedDbs = New-AzDmsSelectedDB -MigrateSqlServerSqlDb -Name AdventureWorks2016 `
  -TargetDatabaseName AdventureWorks2016 `
  -TableMap $tableMap
```

### <a name="create-the-migration-task-and-start-it"></a>Создать задачу миграции и запустить ее

Выполните командлет `New-AzDataMigrationTask`, чтобы создать и запустить задачу миграции. Для этого командлета требуются следующие параметры:

* *TaskType.* Для типа миграции "из SQL Server в Базу данных SQL Azure" в создаваемой задаче миграции ожидается тип *MigrateSqlServerSqlDb*. 
* *Название группы ресурсов*. Имя группы ресурсов Azure, в которой будет создана задача.
* *ServiceName*. Экземпляр Azure Database Migration Service, в котором будет создана задача.
* *ПроектИмя*. Имя проекта Azure Database Migration Service, в котором будет создана задача. 
* *TaskName*. Имя задачи, которая будет создана. 
* *SourceConnection*. Объект AzDmsConnInfo, представляющий исходное соединение сервера S'L.
* *TargetConnection.* Объект AzDmsConnInfo, представляющий целевое подключение базы данных Azure S'L.
* *SourceCred.* Объект [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) для подключения к исходному серверу.
* *TargetCred.* Объект [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) для подключения к целевому серверу.
* *SelectedDatabase.* AzDataMigrationSelectedDB объект, представляющий исходное и целевое картографическое базирование.
* *СхемаВалисация*. (необязательный параметр коммутатора) После миграции выполняется сравнение информации о схеме между источником и целью.
* *DataIntegrityПроверка*. (необязательный параметр коммутатора) После миграции выполняется проверка целостности данных на основе чексумов между источником и целью.
* *ЗапросАнализА .* (необязательный параметр коммутатора) После миграции выполняется быстрый и интеллектуальный анализ запросов путем извлечения запросов из исходной базы данных и выполнения их в целевой.

В следующем примере создается и запускается задача миграции с именем myDMSTask:

```powershell
$migTask = New-AzDataMigrationTask -TaskType MigrateSqlServerSqlDb `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs `
```

Следующий пример создает и запускает ту же задачу миграции, что и выше, но также выполняет все три проверки:

```powershell
$migTask = New-AzDataMigrationTask -TaskType MigrateSqlServerSqlDb `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs `
  -SchemaValidation `
  -DataIntegrityValidation `
  -QueryAnalysisValidation `
```

## <a name="monitor-the-migration"></a>Мониторинг миграции

Вы можете отслеживать выполняемую задачу миграции, запросив свойство состояния задачи, как показано в следующем примере:

```powershell
if (($mytask.ProjectTask.Properties.State -eq "Running") -or ($mytask.ProjectTask.Properties.State -eq "Queued"))
{
  write-host "migration task running"
}
```

## <a name="deleting-the-dms-instance"></a>Удаление экземпляра DMS

После завершения миграции можно удалить экземпляр Azure Database Migration Service.

```powershell
Remove-AzDms -ResourceGroupName myResourceGroup -ServiceName MyDMS
```

## <a name="next-step"></a>Следующий шаг

* Просмотрите [руководство по миграции базы данных Майкрософт](https://datamigration.microsoft.com/).
