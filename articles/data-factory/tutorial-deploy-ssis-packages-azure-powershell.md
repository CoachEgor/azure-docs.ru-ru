---
title: Подготовка среды выполнения интеграции Azure SSIS с помощью PowerShell | Документация Майкрософт
description: Узнайте, как с помощью PowerShell подготовить среду выполнения интеграции Azure SSIS в фабрике данных Azure, чтобы запускать и развертывать пакеты служб SSIS в Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: tutorial
ms.date: 10/28/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: cba8af369beac935834da8d2073510e0f997648b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "66123458"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory-with-powershell"></a>Подготовка среды выполнения интеграции Azure SSIS в фабрике данных Azure с помощью PowerShell
В этом руководстве представлены шаги по подготовке среды выполнения интеграции (IR) Azure SSIS в фабрике данных. Затем можно использовать SQL Server Data Tools (SSDT) ​​или SQL Server Management Studio (SSMS) для запуска и развертывания пакетов служб SQL Server Integration Services (SSIS) в этой среде выполнения в Azure. Вот какие шаги выполняются в этом руководстве:

> [!NOTE]
> В этой статье используется Azure PowerShell для подготовки служб среды выполнения интеграции Integration Services Azure. Дополнительные сведения о работе с пользовательским интерфейсом фабрики данных для подготовки среды выполнения интеграции Integration Services Azure см. в статье [Подготовка Integration Runtime Azure–SSIS в Фабрике данных Azure](tutorial-create-azure-ssis-runtime-portal.md). 

> [!div class="checklist"]
> * Создали фабрику данных.
> * Создание среды выполнения интеграции Azure SSIS.
> * Запуск среды выполнения интеграции Azure SSIS.
> * Развертывание пакетов служб SSIS.
> * Проверка всего скрипта.

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Подписка Azure**. Если у вас еще нет подписки Azure, создайте [бесплатную](https://azure.microsoft.com/free/) учетную запись Azure, прежде чем начинать работу. См. дополнительные сведения о [среде выполнения интеграции Azure SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime). 
- **Сервер базы данных SQL Azure**. Если у вас еще нет сервера базы данных, создайте его на портале Azure перед началом работы. На этом сервере размещается база данных каталога служб SSIS (SSISDB). Мы рекомендуем создать сервер базы данных в одном регионе Azure со средой интеграции. Эта конфигурация позволяет среде выполнения интеграции записывать журналы выполнения в SSISDB, не пересекая регионы Azure. 
    - В зависимости от выбранного сервера базы данных SSISDB может быть создана от вашего имени как отдельная база данных, а также как часть эластичного пула или Управляемого экземпляра. Доступ к ней можно получить через общедоступную сеть или после присоединения к виртуальной сети. Инструкции по выбору типа сервера базы данных для размещения SSISDB см. в разделе [Сравнение логического сервера базы данных SQL и управляемого экземпляра базы данных SQL](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Если База данных SQL Azure используется с конечными точками служб для виртуальной сети или Управляемого экземпляра с целью размещения SSISDB или требования доступа к локальным данным, необходимо присоединить к виртуальной сети вашу среду выполнения интеграции Azure SSIS. Сведения об этом см. в статье [Создание среды выполнения интеграции Azure SSIS в службе "Фабрика данных Azure"](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 
    - Убедитесь, что для сервера базы данных **включена** настройка **Разрешить доступ к службам Azure**. Этот параметр не применяется, когда База данных SQL Azure используется с конечными точками служб для виртуальной сети или Управляемого экземпляра с целью размещения базы данных SSISDB. Дополнительные сведения см. в разделе [Создание правила брандмауэра на уровне сервера с помощью портала Azure](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). Сведения о включении этого параметра с помощью PowerShell см. в статье о [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule). 
    - Добавьте IP-адрес клиентского компьютера или диапазон IP-адресов, который включает IP-адрес клиентского компьютера, в список IP-адресов клиента в параметрах брандмауэра для сервера базы данных. Дополнительные сведения см. в разделе [Правила брандмауэра уровня сервера Базы данных SQL Azure и уровня базы данных SQL Azure](../sql-database/sql-database-firewall-configure.md). 
    - К серверу базы данных можно подключиться с использованием аутентификации SQL и учетных данных администратора сервера или аутентификации Azure Active Directory (AAD) и управляемого удостоверения Фабрики данных Azure.  Для последнего варианта управляемое удостоверение ADF нужно добавить в группу AAD, которая обладает разрешениями на доступ к серверу базы данных. См.статью [Создание среды выполнения интеграции Azure SSIS в службе "Фабрика данных Azure"](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 
    - Убедитесь, что на сервере Базы данных SQL Azure нет каталога SSIS (базы данных SSISDB). При подготовке среды Azure SSIS IR не поддерживается использование существующего каталога SSIS. 
- **Azure PowerShell**. Следуйте инструкциям по [установке и настройке Azure PowerShell](/powershell/azure/install-Az-ps). Вы используете PowerShell для запуска скрипта, чтобы подготовить среду выполнения интеграции Azure SSIS, запускающую пакеты SSIS в облаке. 

> [!NOTE]
> - Список регионов Azure, в которых сейчас доступны Фабрика данных и Azure-SSIS Integration Runtime, см. на странице [Доступность продуктов по регионам](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all). 

## <a name="launch-windows-powershell-ise"></a>Запуск интегрированной среды сценариев Windows PowerShell
Откройте **интегрированную среду сценариев Windows PowerShell** с правами администратора. 

## <a name="create-variables"></a>Создание переменных
Скопируйте и вставьте следующий скрипт. Укажите значения для переменных. Список поддерживаемых **ценовых категорий** базы данных SQL Azure см. в статье [Ограничения ресурсов базы данных SQL Azure](../sql-database/sql-database-resource-limits.md).

```powershell
# Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[Azure subscription name]"
$ResourceGroupName = "[Azure resource group name]"
# Data factory name. Must be globally unique
$DataFactoryName = "[Data factory name]"
$DataFactoryLocation = "EastUS"

# Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[Specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[Specify a description for your Azure-SSIS IR]"
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported, but for other nodes, 1-8 are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf    
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc.
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>)]"
```

## <a name="validate-the-connection-to-database"></a>Проверьте подключение к базе данных
Добавьте приведенный ниже скрипт для проверки сервера Базы данных SQL Azure`<servername>.database.windows.net`. 

```powershell
$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}
```

Чтобы создать базу данных Azure SQL в рамках скрипта, см. следующий пример: 

Задайте значения для переменных, которые еще не были определены. Например:  SSISDBServerName, FirewallIPAddress. 

```powershell
New-AzSqlServer -ResourceGroupName $ResourceGroupName `
    -ServerName $SSISDBServerName `
    -Location $DataFactoryLocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $SSISDBServerAdminUserName, $(ConvertTo-SecureString -String $SSISDBServerAdminPassword -AsPlainText -Force))    

New-AzSqlServerFirewallRule -ResourceGroupName $ResourceGroupName `
    -ServerName $SSISDBServerName `
    -FirewallRuleName "ClientIPAddress_$today" -StartIpAddress $FirewallIPAddress -EndIpAddress $FirewallIPAddress

New-AzSqlServerFirewallRule -ResourceGroupName $ResourceGroupName -ServerName $SSISDBServerName -AllowAllAzureIPs
```

## <a name="log-in-and-select-subscription"></a>Вход и выбор подписки
Добавьте следующий код в скрипт, чтобы войти и выбрать подписку Azure: 

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName
```

## <a name="create-a-resource-group"></a>Создание группы ресурсов
Создайте [группу ресурсов Azure ](../azure-resource-manager/resource-group-overview.md) с помощью команды [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Группа ресурсов — это логический контейнер, в котором ресурсы Azure развертываются и администрируются как группа. В следующем примере создается группа ресурсов с именем `myResourceGroup` в расположении `westeurope`.

Если группа ресурсов уже существует, не копируйте этот код в скрипт. 

```powershell
New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

## <a name="create-a-data-factory"></a>Создание фабрики данных
Чтобы создать фабрику данных, выполните следующие команды:

```powershell
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

## <a name="create-an-integration-runtime"></a>Создание среды выполнения интеграции
Выполните следующую команду для создания среды выполнения интеграции Azure SSIS, запускающей пакеты служб SSIS в Azure: 

```powershell
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
  
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogAdminCredential $serverCreds `
                                           -CatalogPricingTier $SSISDBPricingTier

if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}
```

## <a name="start-integration-runtime"></a>Запуск среды выполнения интеграции
Выполните следующую команду для запуска среды выполнения интеграции SSIS Azure: 

```powershell
write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```
Выполнение этой команды занимает от **20 до 30 минут**. 

## <a name="deploy-ssis-packages"></a>Развертывание пакетов служб SSIS.
Теперь используйте SQL Server Data Tools (SSDT) или SQL Server Management Studio (SSMS) для развертывания пакетов служб SSIS в Azure. Подключитесь к серверу Azure SQL, на котором размещен каталог служб SSIS (SSISDB). Имя сервера Базы данных SQL Azure указывается в следующем формате: `<servername>.database.windows.net`. 

См. документацию SSIS в следующих статьях: 

- [Deploy, run, and monitor an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) (Развертывание, запуск и отслеживание пакета SSIS в Azure)   
- [Connect to the SSISDB Catalog database on Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) (Подключение к базе данных каталога SSISDB в Azure)
- [Schedule the execution of an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) (Планирование выполнения пакета SSIS в Azure)
- [Connect to on-premises data sources with Windows Authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Подключение к локальным источникам данных с помощью аутентификации Windows) 

## <a name="full-script"></a>Полный сценарий
Скрипт PowerShell в этом разделе настраивает экземпляр среды выполнения интеграции Azure SSIS, которая запускает пакеты SSIS, в облаке. После успешного выполнения этого скрипта можно развернуть и выполнить пакеты служб SSIS в облаке Microsoft Azure с базой данных SSISDB, размещенной в базе данных SQL Azure.

1. Откройте интегрированную среду сценариев (ISE) Windows PowerShell.
2. В ISE введите в окне командной строки следующую команду:    
    ```powershell
    Set-ExecutionPolicy Unrestricted -Scope CurrentUser
    ```
3. Скопируйте скрипт PowerShell в этом разделе и вставьте его в ISE.
4. Укажите соответствующие значения для всех параметров в начале скрипта.
5. Выполните скрипт. Команда `Start-AzDataFactoryV2IntegrationRuntime` ближе к концу скрипта выполняется около **20–30 минут**.

> [!NOTE]
> - Скрипт подключается к серверу Базы данных SQL Azure для подготовки базы данных каталога служб SSIS (SSISDB).
> 
> - При подготовке экземпляра среды выполнения интеграции Azure Integration Services также устанавливается пакет функций Azure для служб SSIS и распространяемый компонент Access. Эти компоненты обеспечивают подключение к файлам Excel и Access и другим источникам данных Azure, кроме тех, которые поддерживаются встроенными компонентами. Кроме того, вы можете установить дополнительные компоненты. Дополнительные сведения см. в статье о [пользовательской установке для среды выполнения интеграции Azure SSIS](how-to-configure-azure-ssis-ir-custom-setup.md).

Список поддерживаемых **ценовых категорий** базы данных SQL Azure см. в статье [Ограничения ресурсов базы данных SQL Azure](../sql-database/sql-database-resource-limits.md). 

Список регионов Azure, в которых сейчас доступны Фабрика данных и Azure-SSIS Integration Runtime, см. на странице [Доступность продуктов по регионам](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all). 

```powershell
# Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[Azure subscription name]"
$ResourceGroupName = "[Azure resource group name]"
# Data factory name. Must be globally unique
$DataFactoryName = "[Data factory name]"
$DataFactoryLocation = "EastUS"

# Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[Specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[Specify a description for your Azure-SSIS IR]"
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported, but for other nodes, 1-8 are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf    
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc.
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>)]"

$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}

Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName

Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
    
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
    
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogAdminCredential $serverCreds `
                                           -CatalogPricingTier $SSISDBPricingTier

if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}

write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="join-azure-ssis-ir-to-a-virtual-network"></a>Присоединение среды выполнения интеграции Azure SSIS к виртуальной сети
Если вы используете Базу данных SQL Azure с конечными точками служб для виртуальной сети или Управляемым экземпляром, который присоединяется к виртуальной сети с целью размещения SSISDB, необходимо присоединить среду выполнения интеграции Azure SSIS к той же виртуальной сети. В службе "Фабрика данных Azure" среду выполнения интеграции Azure SSIS можно присоединить к виртуальной среде. Дополнительные сведения см. в статье [Присоединение среды выполнения интеграции Azure SSIS к виртуальной сети](join-azure-ssis-integration-runtime-virtual-network.md).

Полный сценарий создания среды выполнения интеграции Azure SSIS, которая присоединяется к виртуальной сети, см. в статье [Создание среды выполнения интеграции Azure SSIS в фабрике данных Azure](create-azure-ssis-integration-runtime.md).

## <a name="monitor-and-manage-azure-ssis-ir"></a>Мониторинг среды выполнения интеграции IR Azure SSIS и управление ею
Сведения о мониторинге и администрировании среды выполнения интеграции см. по ссылкам ниже. 

- [Среда выполнения интеграции Azure SSIS](monitor-integration-runtime.md#azure-ssis-integration-runtime)
- [Управление средой выполнения интеграции Azure SSIS](manage-azure-ssis-integration-runtime.md)

## <a name="next-steps"></a>Дополнительная информация
Из этого руководства вы узнали, как выполнить следующие задачи: 

> [!div class="checklist"]
> * Создали фабрику данных.
> * Создание среды выполнения интеграции Azure SSIS.
> * Запуск среды выполнения интеграции Azure SSIS.
> * Развертывание пакетов служб SSIS.
> * Проверка всего скрипта.

Дополнительные сведения о настройке Azure-SSIS Integration Runtime см. в следующей статье:

> [!div class="nextstepaction"]
>[Настройка Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)
