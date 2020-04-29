---
title: PowerShell для конечных точек и правил виртуальной сети для одной и для баз данных в составе пула
description: Предоставляет скрипты PowerShell для создания конечных точек виртуальных служб и управления ими для базы данных SQL Azure и Azure синапсе.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: PowerShell
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: genemi, vanto
ms.date: 03/12/2019
tags: azure-synapse
ms.openlocfilehash: 1e8ec394eab1df0aebe394b8acebc74c7ed49e9c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80124704"
---
# <a name="powershell--create-a-virtual-service-endpoint-and-vnet-rule-for-sql"></a>PowerShell: создание конечной точки службы и правила виртуальной сети для SQL

*Правила виртуальной сети* — это одна из функций безопасности брандмауэра, которая определяет, принимает ли сервер базы данных для отдельных баз данных и эластичных пулов в [базе данных SQL](sql-database-technical-overview.md) Azure или для баз данных в [Azure синапсе](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) связь, отправляемую из определенных подсетей в виртуальных сетях.

> [!IMPORTANT]
> Эта статья относится к Azure SQL Server и к базе данных SQL и хранилищу данных в Azure синапсе, которые создаются на сервере Azure SQL. Для простоты база данных SQL используется при обращении к базе данных SQL и Azure синапсе. Эта статья *не* применяется к развертыванию **управляемого экземпляра** в Базе данных SQL Azure, так как с ним не связана конечная точка службы.

В этой статье предлагается и описывается сценарий PowerShell, который выполняет следующие действия:

1. Создает *конечную точку службы виртуальной сети* Microsoft Azure в подсети.
2. Добавляет конечную точку в брандмауэр сервера Базы данных SQL Azure, чтобы создать *правило виртуальной сети*.

Причины для создания правила приведены в разделе [Использование конечных точек службы и правил виртуальной сети для базы данных SQL Azure][sql-db-vnet-service-endpoint-rule-overview-735r].

> [!TIP]
> Если все, что вам требуется, это оценить или добавить *имя типа* конечной точки службы виртуальной сети для базы данных SQL в подсеть, то вы можете сразу перейти к более [прямолинейному сценарию PowerShell](#a-verify-subnet-is-endpoint-ps-100).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!IMPORTANT]
> Модуль PowerShell Azure Resource Manager по-прежнему поддерживается базой данных SQL Azure, но вся будущая разработка предназначена для модуля AZ. SQL. Эти командлеты см. в разделе [AzureRM. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Аргументы для команд в модуле AZ и в модулях AzureRm существенно идентичны.

## <a name="major-cmdlets"></a>Основные командлеты

В этой статье рассматривается командлет **New-азсклсервервиртуалнетворкруле** , который добавляет конечную точку подсети в список управления доступом (ACL) сервера базы данных SQL Azure, тем самым создавая правило.

В следующем списке показана последовательность других *основных* командлетов, которые необходимо выполнить для подготовки к вызову командлета **New-азсклсервервиртуалнетворкруле**. В этой статье эти вызовы выполняются в [сценарии 3 "Правило виртуальной сети"](#a-script-30):

1. [New-азвиртуалнетворксубнетконфиг](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig): создает объект подсети.
2. [New-азвиртуалнетворк](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork): создает виртуальную сеть, предоставляя ей подсеть.
3. [Set-азвиртуалнетворксубнетконфиг](https://docs.microsoft.com/powershell/module/az.network/Set-azVirtualNetworkSubnetConfig): назначает конечную точку виртуальной службы вашей подсети.
4. [Set-азвиртуалнетворк](https://docs.microsoft.com/powershell/module/az.network/Set-azVirtualNetwork): сохраняет обновления, внесенные в виртуальную сеть.
5. [New-азсклсервервиртуалнетворкруле](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlservervirtualnetworkrule). После того как подсеть является конечной, добавляет подсеть в качестве правила виртуальной сети в список управления доступом сервера базы данных SQL Azure.
   - В версии 5.1.1 модуля Azure RM PowerShell и последующих версиях командлет предоставляет параметр **-IgnoreMissingVnetServiceEndpoint**.

## <a name="prerequisites-for-running-powershell"></a>Предварительные требования для запуска PowerShell

- Вы уже можете войти в Azure, например, воспользовавшись [порталом Azure][http-azure-portal-link-ref-477t].
- Вы уже можете выполнять сценарии PowerShell.

> [!NOTE]
> Убедитесь, что конечные точки службы включены для виртуальной сети и подсети, которые требуется добавить на сервер. В противном случае вам не удастся создать правило брандмауэра виртуальной сети.

## <a name="one-script-divided-into-four-chunks"></a>Один сценарий состоит четырех блоков

Наш демонстрационный сценарий PowerShell состоит из последовательности сценариев меньшего размера. Такое разделение облегчает обучение и обеспечивает гибкость. Эти сценарии должны запускаться в указанной последовательности. Если сейчас у вас нет времени для выполнения этих сценариев, вы можете ознакомиться с тестовыми выходными данными сценария 4.

<a name="a-script-10" />

### <a name="script-1-variables"></a>Сценарий 1: переменные

Первый сценарий PowerShell присваивает переменным значения. Последующие сценарии зависят от этих переменных.

> [!IMPORTANT]
> При необходимости, прежде чем выполнить этот сценарий, можно изменить значения. Например, если у вас уже есть группа ресурсов, можно изменить присваиваемое значение имени группы ресурсов.
>
> Имя вашей подписки необходимо указать в сценарии.

### <a name="powershell-script-1-source-code"></a>Исходный код сценария 1 PowerShell

```powershell
######### Script 1 ########################################
##   LOG into to your Azure account.                     ##
##   (Needed only one time per powershell.exe session.)  ##
###########################################################

$yesno = Read-Host 'Do you need to log into Azure (only one time per powershell.exe session)?  [yes/no]';
if ('yes' -eq $yesno) { Connect-AzAccount; }

###########################################################
##  Assignments to variables used by the later scripts.  ##
###########################################################

# You can edit these values, if necessary.
$SubscriptionName = 'yourSubscriptionName';
Select-AzSubscription -SubscriptionName $SubscriptionName;

$ResourceGroupName = 'RG-YourNameHere';
$Region            = 'westcentralus';

$VNetName            = 'myVNet';
$SubnetName          = 'mySubnet';
$VNetAddressPrefix   = '10.1.0.0/16';
$SubnetAddressPrefix = '10.1.1.0/24';
$VNetRuleName        = 'myFirstVNetRule-ForAcl';

$SqlDbServerName         = 'mysqldbserver-forvnet';
$SqlDbAdminLoginName     = 'ServerAdmin';
$SqlDbAdminLoginPassword = 'ChangeYourAdminPassword1';

$ServiceEndpointTypeName_SqlDb = 'Microsoft.Sql';  # Official type name.

Write-Host 'Completed script 1, the "Variables".';
```

<a name="a-script-20" />

### <a name="script-2-prerequisites"></a>Сценарий 2: предварительные требования

Этот сценарий подготавливает среду к следующему сценарию, который выполняет действие с конечной точкой. Этот сценарий создает перечисленные ниже элементы, но только если они еще не существует. Сценарий 2 можно пропустить, если вы уверены, что эти элементы уже существуют:

- Группа ресурсов Azure
- сервер Базы данных SQL Azure;

### <a name="powershell-script-2-source-code"></a>Исходный код сценария 2 PowerShell

```powershell
######### Script 2 ########################################
##   Ensure your Resource Group already exists.          ##
###########################################################

Write-Host "Check whether your Resource Group already exists.";

$gottenResourceGroup = $null;

$gottenResourceGroup = Get-AzResourceGroup `
  -Name        $ResourceGroupName `
  -ErrorAction SilentlyContinue;

if ($null -eq $gottenResourceGroup)
{
    Write-Host "Creating your missing Resource Group - $ResourceGroupName.";

    $gottenResourceGroup = New-AzResourceGroup `
      -Name $ResourceGroupName `
      -Location $Region;

    $gottenResourceGroup;
}
else { Write-Host "Good, your Resource Group already exists - $ResourceGroupName."; }

$gottenResourceGroup = $null;

###########################################################
## Ensure your Azure SQL Database server already exists. ##
###########################################################

Write-Host "Check whether your Azure SQL Database server already exists.";

$sqlDbServer = $null;

$sqlDbServer = Get-AzSqlServer `
  -ResourceGroupName $ResourceGroupName `
  -ServerName        $SqlDbServerName `
  -ErrorAction       SilentlyContinue;

if ($null -eq $sqlDbServer) {
    Write-Host "Creating the missing Azure SQL Database server - $SqlDbServerName.";
    Write-Host "Gather the credentials necessary to next create an Azure SQL Database server.";

    $sqlAdministratorCredentials = New-Object `
      -TypeName     System.Management.Automation.PSCredential `
      -ArgumentList `
        $SqlDbAdminLoginName, `
        $(ConvertTo-SecureString `
            -String      $SqlDbAdminLoginPassword `
            -AsPlainText `
            -Force `
         );

    if ($null -eq $sqlAdministratorCredentials) {
        Write-Host "ERROR, unable to create SQL administrator credentials.  Now ending.";
        return;
    }

    Write-Host "Create your Azure SQL Database server.";

    $sqlDbServer = New-AzSqlServer `
      -ResourceGroupName $ResourceGroupName `
      -ServerName        $SqlDbServerName `
      -Location          $Region `
      -SqlAdministratorCredentials $sqlAdministratorCredentials;

    $sqlDbServer;
}
else {
    Write-Host "Good, your Azure SQL Database server already exists - $SqlDbServerName."; 
}

$sqlAdministratorCredentials = $null;
$sqlDbServer                 = $null;

Write-Host 'Completed script 2, the "Prerequisites".';
```

<a name="a-script-30" />

## <a name="script-3-create-an-endpoint-and-a-rule"></a>Сценарий 3: создание конечной точки и правила

Этот сценарий создает виртуальную сеть с подсетью. Затем он присваивает тип конечной точки **Microsoft.Sql** подсети. Наконец, этот сценарий добавляет подсеть в список управления доступом (ACL) вашего сервера Базы данных SQL, тем самым создавая правило.

### <a name="powershell-script-3-source-code"></a>Исходный код сценария 3 PowerShell

```powershell
######### Script 3 ########################################
##   Create your virtual network, and give it a subnet.  ##
###########################################################

Write-Host "Define a subnet '$SubnetName', to be given soon to a virtual network.";

$subnet = New-AzVirtualNetworkSubnetConfig `
  -Name            $SubnetName `
  -AddressPrefix   $SubnetAddressPrefix `
  -ServiceEndpoint $ServiceEndpointTypeName_SqlDb;

Write-Host "Create a virtual network '$VNetName'." `
  "  Give the subnet to the virtual network that we created.";

$vnet = New-AzVirtualNetwork `
  -Name              $VNetName `
  -AddressPrefix     $VNetAddressPrefix `
  -Subnet            $subnet `
  -ResourceGroupName $ResourceGroupName `
  -Location          $Region;

###########################################################
##   Create a Virtual Service endpoint on the subnet.    ##
###########################################################

Write-Host "Assign a Virtual Service endpoint 'Microsoft.Sql' to the subnet.";

$vnet = Set-AzVirtualNetworkSubnetConfig `
  -Name            $SubnetName `
  -AddressPrefix   $SubnetAddressPrefix `
  -VirtualNetwork  $vnet `
  -ServiceEndpoint $ServiceEndpointTypeName_SqlDb;

Write-Host "Persist the updates made to the virtual network > subnet.";

$vnet = Set-AzVirtualNetwork `
  -VirtualNetwork $vnet;

$vnet.Subnets[0].ServiceEndpoints;  # Display the first endpoint.

###########################################################
##   Add the Virtual Service endpoint Id as a rule,      ##
##   into SQL Database ACLs.                             ##
###########################################################

Write-Host "Get the subnet object.";

$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Name              $VNetName;

$subnet = Get-AzVirtualNetworkSubnetConfig `
  -Name           $SubnetName `
  -VirtualNetwork $vnet;

Write-Host "Add the subnet .Id as a rule, into the ACLs for your Azure SQL Database server.";

$vnetRuleObject1 = New-AzSqlServerVirtualNetworkRule `
  -ResourceGroupName      $ResourceGroupName `
  -ServerName             $SqlDbServerName `
  -VirtualNetworkRuleName $VNetRuleName `
  -VirtualNetworkSubnetId $subnet.Id;

$vnetRuleObject1;

Write-Host "Verify that the rule is in the SQL DB ACL.";

$vnetRuleObject2 = Get-AzSqlServerVirtualNetworkRule `
  -ResourceGroupName      $ResourceGroupName `
  -ServerName             $SqlDbServerName `
  -VirtualNetworkRuleName $VNetRuleName;

$vnetRuleObject2;

Write-Host 'Completed script 3, the "Virtual-Network-Rule".';
```

<a name="a-script-40" />

## <a name="script-4-clean-up"></a>Сценарий 4: очистка

Этот сценарий удаляет ресурсы, созданные предыдущим сценарии в целях демонстрации. Однако этот сценарий запрашивает подтверждение, прежде чем удалить следующие элементы:

- сервер Базы данных SQL Azure;
- группа ресурсов Azure.

Сценарий 4 можно запустить в любой момент после завершения сценария 1.

### <a name="powershell-script-4-source-code"></a>Исходный код сценария 4 PowerShell

```powershell
######### Script 4 ########################################
##   Clean-up phase A:  Unconditional deletes.           ##
##                                                       ##
##   1. The test rule is deleted from SQL DB ACL.        ##
##   2. The test endpoint is deleted from the subnet.    ##
##   3. The test virtual network is deleted.             ##
###########################################################

Write-Host "Delete the rule from the SQL DB ACL.";

Remove-AzSqlServerVirtualNetworkRule `
  -ResourceGroupName      $ResourceGroupName `
  -ServerName             $SqlDbServerName `
  -VirtualNetworkRuleName $VNetRuleName `
  -ErrorAction            SilentlyContinue;

Write-Host "Delete the endpoint from the subnet.";

$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Name              $VNetName;

Remove-AzVirtualNetworkSubnetConfig `
  -Name $SubnetName `
  -VirtualNetwork $vnet;

Write-Host "Delete the virtual network (thus also deletes the subnet).";

Remove-AzVirtualNetwork `
  -Name              $VNetName `
  -ResourceGroupName $ResourceGroupName `
  -ErrorAction       SilentlyContinue;

###########################################################
##   Clean-up phase B:  Conditional deletes.             ##
##                                                       ##
##   These might have already existed, so user might     ##
##   want to keep.                                       ##
##                                                       ##
##   1. Azure SQL Database server                        ##
##   2. Azure resource group                             ##
###########################################################

$yesno = Read-Host 'CAUTION !: Do you want to DELETE your Azure SQL Database server AND your Resource Group?  [yes/no]';
if ('yes' -eq $yesno) {
    Write-Host "Remove the Azure SQL DB server.";

    Remove-AzSqlServer `
      -ServerName        $SqlDbServerName `
      -ResourceGroupName $ResourceGroupName `
      -ErrorAction       SilentlyContinue;

    Write-Host "Remove the Azure Resource Group.";

    Remove-AzResourceGroup `
      -Name        $ResourceGroupName `
      -ErrorAction SilentlyContinue;
}
else {
    Write-Host "Skipped over the DELETE of SQL Database and resource group.";
}

Write-Host 'Completed script 4, the "Clean-Up".';
```

<a name="a-actual-output" />

<a name="a-verify-subnet-is-endpoint-ps-100" />

## <a name="verify-your-subnet-is-an-endpoint"></a>Проверка того, является ли подсеть конечной точкой

Возможно, у вас есть подсеть, которой уже было назначено имя типа **Microsoft.Sql**, то есть она уже является конечной точкой службы виртуальной сети. Можно использовать [портал Azure][http-azure-portal-link-ref-477t], чтобы создать правило виртуальной сети для конечной точки.

Возможно, вы не уверены, присвоено ли подсети имя типа **Microsoft.Sql**. Можно выполнить приведенный ниже сценарий PowerShell, чтобы выполнить следующие действия.

1. Выясните, присвоено ли подсети имя типа **Microsoft.Sql**.
2. При необходимости назначьте имя типа, если оно отсутствует.
    - Сценарий запросит *подтверждение*, прежде чем применить отсутствующее имя типа.

### <a name="phases-of-the-script"></a>Этапы сценария

Ниже приведены этапы сценария PowerShell.

1. Вход в учетную запись Azure, требуется только один раз за сеанс PowerShell.  Присвоение значений переменных.
2. Поиск виртуальной сети, а затем подсети.
3. Подсеть помечена именем типа конечной точки службы **Microsoft.Sql**?
4. Добавление имени типа конечной точки службы виртуальной сети **Microsoft.Sql** в подсеть.

> [!IMPORTANT]
> Прежде чем запустить этот сценарий, необходимо изменить значения, присвоенные $-переменным в верхней части сценария.

### <a name="direct-powershell-source-code"></a>Исходный код прямолинейного сценария PowerShell

Этот сценарий PowerShell не обновляет ничего, пока вы не подтвердите это. Этот сценарий может добавить имя типа **Microsoft.Sql** в подсеть. Однако он пытается сделать это, только если у подсети отсутствует имя типа.

```powershell
### 1. LOG into to your Azure account, needed only once per PS session.  Assign variables.
$yesno = Read-Host 'Do you need to log into Azure (only one time per powershell.exe session)?  [yes/no]';
if ('yes' -eq $yesno) { Connect-AzAccount; }

# Assignments to variables used by the later scripts.
# You can EDIT these values, if necessary.

$SubscriptionName  = 'yourSubscriptionName';
Select-AzSubscription -SubscriptionName "$SubscriptionName";

$ResourceGroupName   = 'yourRGName';
$VNetName            = 'yourVNetName';
$SubnetName          = 'yourSubnetName';
$SubnetAddressPrefix = 'Obtain this value from the Azure portal.'; # Looks roughly like: '10.0.0.0/24'

$ServiceEndpointTypeName_SqlDb = 'Microsoft.Sql';  # Do NOT edit. Is official value.

### 2. Search for your virtual network, and then for your subnet.
# Search for the virtual network.
$vnet = $null;
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Name              $VNetName;

if ($vnet -eq $null) {
    Write-Host "Caution: No virtual network found by the name '$VNetName'.";
    Return;
}

$subnet = $null;
for ($nn=0; $nn -lt $vnet.Subnets.Count; $nn++) {
    $subnet = $vnet.Subnets[$nn];
    if ($subnet.Name -eq $SubnetName)
    { break; }
    $subnet = $null;
}

if ($subnet -eq $null) {
    Write-Host "Caution: No subnet found by the name '$SubnetName'";
    Return;
}

### 3. Is your subnet tagged as 'Microsoft.Sql' endpoint server type?
$endpointMsSql = $null;
for ($nn=0; $nn -lt $subnet.ServiceEndpoints.Count; $nn++) {
    $endpointMsSql = $subnet.ServiceEndpoints[$nn];
    if ($endpointMsSql.Service -eq $ServiceEndpointTypeName_SqlDb) {
        $endpointMsSql;
        break;
    }
    $endpointMsSql = $null;
}

if ($endpointMsSql -ne $null) {
    Write-Host "Good: Subnet found, and is already tagged as an endpoint of type '$ServiceEndpointTypeName_SqlDb'.";
    Return;
}
else {
    Write-Host "Caution: Subnet found, but not yet tagged as an endpoint of type '$ServiceEndpointTypeName_SqlDb'.";

    # Ask the user for confirmation.
    $yesno = Read-Host 'Do you want the PS script to apply the endpoint type name to your subnet?  [yes/no]';
    if ('no' -eq $yesno) { Return; }
}

### 4. Add a Virtual Service endpoint of type name 'Microsoft.Sql', on your subnet.
$vnet = Set-AzVirtualNetworkSubnetConfig `
  -Name            $SubnetName `
  -AddressPrefix   $SubnetAddressPrefix `
  -VirtualNetwork  $vnet `
  -ServiceEndpoint $ServiceEndpointTypeName_SqlDb;

# Persist the subnet update.
$vnet = Set-AzVirtualNetwork `
  -VirtualNetwork $vnet;

for ($nn=0; $nn -lt $vnet.Subnets.Count; $nn++) {
    $vnet.Subnets[0].ServiceEndpoints; }  # Display.
```

<!-- Link references: -->
[sql-db-vnet-service-endpoint-rule-overview-735r]: sql-database-vnet-service-endpoint-rule-overview.md
[http-azure-portal-link-ref-477t]: https://portal.azure.com/
