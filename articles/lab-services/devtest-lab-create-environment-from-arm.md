---
title: Создание сред со множеством виртуальных машин и ресурсов PaaS с помощью шаблонов Azure Resource Manager | Документация Майкрософт
description: Узнайте, как в Azure DevTest Labs создавать среды со множеством виртуальных машин и ресурсов PaaS, используя шаблон Azure Resource Manager.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2018
ms.author: spelluru
ms.openlocfilehash: 96e3a24b0c9f9ab21652ffcd1b29deeb512581e5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60202710"
---
# <a name="create-multi-vm-environments-and-paas-resources-with-azure-resource-manager-templates"></a>Создание сред со множеством виртуальных машин и ресурсов PaaS с помощью шаблонов Azure Resource Manager

[Портал Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040) позволяет с легкостью и [вовремя добавлять виртуальную машину в лабораторию](https://docs.microsoft.com/azure/devtest-lab/devtest-lab-add-vm). Но если среда содержит несколько виртуальных машин, каждую из них необходимо создавать отдельно. Для сценариев, где задействовано многоуровневое веб-приложение или ферма SharePoint, требуется механизм создания множества виртуальных машин одним действием. С помощью шаблонов Azure Resource Manager теперь можно определить инфраструктуру и конфигурацию решения Azure и многократно развернуть множество виртуальных машин в согласованном состоянии. Эта функция обеспечивает следующие преимущества:

- Шаблоны Azure Resource Manager загружаются непосредственно из репозитория системы управления версиями (Git в GitHub или Azure DevOps Services).
- После настройки пользователи могут создать среду, выбрав шаблон Azure Resource Manager на портале Azure, как это делается для других типов [баз виртуальных машин](./devtest-lab-comparing-vm-base-image-types.md).
- На основе шаблона Azure Resource Manager в среде можно подготовить ресурсы Azure PaaS, в дополнение к виртуальным машинам IaaS.
- В лаборатории можно отслеживать стоимость сред, а также отдельных виртуальных машин, созданных другими типами баз.
- Ресурсы PaaS создаются и будут отображаться в отслеживании расходов. Однако автоматическое завершение работы виртуальной машины не применяется к ресурсам PaaS.

Узнайте обо всех [преимуществах использования шаблонов Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#the-benefits-of-using-resource-manager) для развертывания, обновления или удаления всех лабораторных ресурсов за одну операцию.

> [!NOTE]
> Если вы используете шаблон Resource Manager в качестве основы для создания дополнительных лабораторных виртуальных машин, то следует помнить, что существуют некоторые различия при создании нескольких виртуальных машин и одной виртуальной машины. В руководстве по [использованию шаблона Azure Resource Manager для виртуальной машины](devtest-lab-use-resource-manager-template.md) эти различия описываются более подробно.
>

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="devtest-labs-public-environments"></a>Общедоступные среды DevTest Labs
Azure DevTest Labs имеет [общедоступный репозиторий шаблонов Azure Resource Manager](https://github.com/Azure/azure-devtestlab/tree/master/Environments), который можно использовать, чтобы создавать среды без необходимости самостоятельно подключаться к внешнему источнику GitHub. В этом репозитории содержатся часто используемые шаблоны, такие как веб-приложения Azure, кластер Service Fabric и среда фермы SharePoint для разработки. Этот компонент похож на общедоступный репозиторий артефактов, включенный для каждой лаборатории, которую вы создаете. Репозиторий среды позволяет быстро приступить к работе с помощью предварительно созданных шаблонов с минимальным количеством входных параметров, чтобы вы могли быстро приступить к работе с ресурсами PaaS в лаборатории. Дополнительные сведения см. в статье [Настройка и использование общедоступных сред в Azure DevTest Labs](devtest-lab-configure-use-public-environments.md).

## <a name="configure-your-own-template-repositories"></a>Настройка собственных репозиториев шаблонов
При использования подходов "инфраструктура как код" и "конфигурация как код" управлять шаблонами среды рекомендуется в системе управления версиями. Служба Azure DevTest Labs следует этой рекомендации и загружает все шаблоны Azure Resource Manager непосредственно из репозиториев Git в GitHub или Azure DevOps Services. В результате шаблоны Resource Manager можно использовать в ходе всего цикла выпуска — от тестовой до рабочей среды.

Изучите шаблоны, созданные командой DevTest Labs, в [общедоступном репозитории GitHub](https://github.com/Azure/azure-devtestlab/tree/master/Environments). В общедоступном репозитории можно просматривать шаблоны, к которым пользователи предоставили общий доступ и которые можно использовать напрямую или настроить в соответствии со своими потребностями. После создания шаблона сохраните его в этот репозиторий, чтобы поделиться с другими. Также можно настроить собственный репозиторий Git с помощью шаблонов, которые используются для настройки сред в облаке. 

Существует несколько правил, которым необходимо следовать при упорядочивании шаблонов Azure Resource Manager в репозитории:

- Имя файла основного шаблона должно быть `azuredeploy.json`. 

    ![Основные файлы шаблонов Azure Resource Manager](./media/devtest-lab-create-environment-from-arm/master-template.png)

- Чтобы использовать значения параметров, определенные в файле параметров, имя этого файла должно быть `azuredeploy.parameters.json`.
- Для создания значения URI parametersLink можно использовать параметры `_artifactsLocation` и `_artifactsLocationSasToken`, чтобы служба DevTest Labs автоматически управляла вложенными шаблонами. Дополнительные сведения см. в статье [How Azure DevTest Labs makes nested Resource Manager template deployments easier for testing environments](https://blogs.msdn.microsoft.com/devtestlab/2017/05/23/how-azure-devtest-labs-makes-nested-arm-template-deployments-easier-for-testing-environments/) (Как команда Azure DevTest Labs упрощает развертывание вложенных шаблонов Resource Manager в тестовых средах).
- Можно определить метаданные, чтобы указать отображаемое имя и описание шаблона. Эти метаданные должны находиться в файле с именем `metadata.json`. В следующем примере файла метаданных показано, как указать отображаемое имя и описание: 

    ```json
    { 
        "itemDisplayName": "<your template name>", 
        "description": "<description of the template>" 
    }
    ```

Ниже описаны шаги, которые помогут вам добавить в лабораторию репозиторий, используя портал Azure. 

1. Войдите на [портале Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Щелкните **Все службы** и выберите в списке **DevTest Labs**.
1. Из списка лабораторий выберите нужную лабораторию.   
1. В области **Обзор** лаборатории выберите **Конфигурация и политики**.

    ![Configuration and Policies (Конфигурация и политики)](./media/devtest-lab-create-environment-from-arm/configuration-and-policies-menu.png)

1. В списке параметров **Configuration and Policies** (Конфигурация и политики) выберите пункт **Репозитории**. В области **Репозитории** перечислены репозитории, которые добавлены в лабораторию. Репозиторий с именем `Public Repo` автоматически создается для всех лабораторий и подключается к [репозиторию DevTest Labs на сайте GitHub](https://github.com/Azure/azure-devtestlab), который содержит несколько артефактов виртуальной машины.

    ![Общедоступный репозиторий](./media/devtest-lab-create-environment-from-arm/public-repo.png)

1. Щелкните **+Добавить**, чтобы добавить репозиторий шаблонов Azure Resource Manager.
1. Когда откроется вторая область **Репозитории**, введите необходимые сведения, как описано ниже.
    - **Имя**: введите имя репозитория, которое используется в лаборатории.
    - **URL-адрес клона Git**: введите URL-адрес клона HTTPS Git из GitHub или Azure DevOps Services.  
    - **Ветвь**: введите имя ветви для доступа к определениям шаблонов Azure Resource Manager. 
    - **Личный маркер доступа**: используется для безопасного доступа к репозиторию. Чтобы получить маркер в Azure DevOps Services, выберите **&lt;Ваше имя> > Мой профиль > Безопасность > Public access token** (Общедоступный маркер доступа). Для получения маркера в GitHub выберите свой аватар, а затем щелкните **Параметры > Public access token** (Общедоступный маркер доступа). 
    - **Folder paths** (Пути к папке): с помощью одного из двух полей ввода введите путь к папке, который начинается с косой черты (/) и относится к URI клона Git, ведущему к определениях артефактов (первое поле ввода) или определениям шаблонов Azure Resource Manager.   
    
        ![Общедоступный репозиторий](./media/devtest-lab-create-environment-from-arm/repo-values.png)


1. Когда все обязательные поля заполнены и данные проверены, нажмите кнопку **Сохранить**.

В следующем разделе описывается процесс создания сред на основе шаблона Azure Resource Manager.

## <a name="create-an-environment-from-a-resource-manager-template-using-the-azure-portal"></a>Создание среды на основе шаблона Resource Manager с помощью портала Azure

После настройки в лаборатории репозитория шаблонов Azure Resource Manager пользователи лаборатории могут создать среду, выполнив следующие действия на портале Azure:

1. Войдите на [портале Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Щелкните **Все службы** и выберите в списке **DevTest Labs**.
1. Из списка лабораторий выберите нужную лабораторию.   
1. В области лаборатории щелкните **Добавить+**.
1. В области **Выбор базы** в начале списка отображаются базовые образы, которые можно использовать с шаблонами Azure Resource Manager. Выберите нужный шаблон Azure Resource Manager.

    ![Выбор основы](./media/devtest-lab-create-environment-from-arm/choose-a-base.png)
  
1. В области **Добавить** введите значение параметра **Имя среды**. Имя среды будет отображаться для пользователей вашей лаборатории. Остальные поля ввода определяются в шаблоне Azure Resource Manager. Если в шаблоне определены значения по умолчанию или есть файл `azuredeploy.parameter.json`, то значения по умолчанию отобразятся в этих полях ввода. Для параметров типа *защищенной строки* можно использовать секреты, хранящиеся в Azure Key Vault. См. дополнительные сведения о [хранении секретов в Azure Key Vault и их использовании их при создании ресурсов лаборатории](devtest-lab-store-secrets-in-key-vault.md).  

    ![Добавление области](./media/devtest-lab-create-environment-from-arm/add.png)

    > [!NOTE]
    > Существует несколько значений параметров, которые, даже если они указаны, отображаются как пустые значения. Таким образом, если пользователи назначат эти значения параметрам в шаблоне Azure Resource Manager, эти значения не отобразятся в DevTest Labs. Вместо этого будут показаны пустые поля ввода, где пользователи лаборатории должны ввести значение при создании среды.
    > 
    > - GEN-UNIQUE
    > - GEN-UNIQUE-[N]
    > - GEN-SSH-PUB-KEY
    > - GEN-PASSWORD 
 
1. Нажмите кнопку **Добавить**, чтобы создать среду. Сразу начнется процесс подготовки среды. В списке **My virtual machines** (Мои виртуальные машины) будет отображаться состояние процесса. Лаборатория автоматически создает группу ресурсов, чтобы подготовить все ресурсы, определенные в шаблоне Azure Resource Manager.
1. Выберите созданную среду в списке **Мои виртуальные машины**, чтобы открыть область группы ресурсов и просмотреть все ресурсы, подготовленные в этой среде.
    
    ![Список "My virtual machines" (Мои виртуальные машины)](./media/devtest-lab-create-environment-from-arm/all-environment-resources.png)
   
   Вы также можете развернуть среду, чтобы просто просмотреть список виртуальных машин, которые подготовлены к работе в этой среде.
    
    ![Список "My virtual machines" (Мои виртуальные машины)](./media/devtest-lab-create-environment-from-arm/my-vm-list.png)

1. Щелкните любую из сред, чтобы просмотреть доступные действия, такие как применение артефактов, присоединение дисков данных, изменение времени автоматического завершения работы и другие.

    ![Действия среды](./media/devtest-lab-create-environment-from-arm/environment-actions.png)

## <a name="automate-deployment-of-environments"></a>Автоматизация развертывания сред
Azure DevTest Labs предоставляет возможность использования [шаблона Azure Resource Manager управления](../azure-resource-manager/resource-group-authoring-templates.md) создание среды с набором ресурсов в лаборатории. Эти среды может содержать все ресурсы Azure, которые могут быть созданы с помощью шаблонов Resource Manager. Среды DevTest Labs позволяют пользователям легко развертывать сложные инфраструктуры согласованность в пределах лаборатории. В настоящее время добавления среды в лабораторию с помощью портала Azure невозможно при создании его один раз, но в среде разработки или тестирования ситуациях, где несколько операции создания, автоматизированного развертывания позволяет для повышения эффективности.

Выполните следующие действия [настроить собственные репозитории шаблона](#configure-your-own-template-repositories) раздел, прежде чем продолжить: 

1. Создание шаблона Resource Manager, который определяет ресурсы, создаваемой. 
2. Настройка шаблона Resource Manager в Git репозитория. 
3. Подключения репозитория Git в лабораторию. 

### <a name="powershell-script-to-deploy-the-resource-manager-template"></a>Сценарий PowerShell для развертывания шаблона Resource Manager
Сохраните сценарий PowerShell в следующем разделе на жесткий диск (например: deployenv.ps1) и запустите сценарий, после указания значений для SubscriptionId, ResourceGroupName, LabName, имя репозитория, TemplateName (папка) в репозитории Git, EnvironmentName.

```powershell
./deployenv.ps1 -SubscriptionId "000000000-0000-0000-0000-0000000000000" -LabName "mydevtestlab" -ResourceGroupName "mydevtestlabRG994248" -RepositoryName "SP Repository" -TemplateName "My Environment template name" -EnvironmentName "SPResourceGroupEnv"  
```

#### <a name="sample-script"></a>Пример скрипта
Ниже приведен пример сценария для создания среды в лаборатории. Комментарии в скрипте помогут вам лучше понять сценарий. 

```powershell
#Requires -Module Az.Resources

[CmdletBinding()]

param (
# ID of the Azure Subscription where the lab is created.
[string] [Parameter(Mandatory=$true)] $SubscriptionId,

# Name of the lab (existing) in which to create the environment.
[string] [Parameter(Mandatory=$true)] $LabName,

# Name of the connected repository in the lab. 
[string] [Parameter(Mandatory=$true)] $RepositoryName,

# Name of the template (folder name in the Git repository) based on which the environment will be created.
[string] [Parameter(Mandatory=$true)] $TemplateName,

# Name of the environment to be created in the lab.
[string] [Parameter(Mandatory=$true)] $EnvironmentName,

# The parameters to be passed to the template. Each parameter is prefixed with “-param_”. 
# For example, if the template has a parameter named “TestVMName” with a value of “MyVMName”, the string in $Params will have the form: `-param_TestVMName MyVMName`. 
# This convention allows the script to dynamically handle different templates.
[Parameter(ValueFromRemainingArguments=$true)]
    $Params
)

# Save this script as the deployenv.ps1 file
# Run the script after you specify values for SubscriptionId, ResourceGroupName, LabName, RepositoryName, TemplateName (folder) in the Git repo, EnvironmentName
# ./deployenv.ps1 -SubscriptionId "000000000-0000-0000-0000-0000000000000" -LabName "mydevtestlab" -ResourceGroupName "mydevtestlabRG994248" -RepositoryName "SP Repository" -TemplateName "My Environment template name" -EnvironmentName "SPResourceGroupEnv"    

# Comment this statement to completely automate the environment creation.    
# Sign in to Azure. 
Connect-AzAccount

# Select the subscription that has the lab.  
Set-AzContext -SubscriptionId $SubscriptionId | Out-Null

# Get information about the user, specifically the user ID, which is used later in the script.  
$UserId = $((Get-AzADUser -UserPrincipalName (Get-AzContext).Account).Id.Guid)
        
# Get information about the lab such as lab location. 
$lab = Get-AzResource -ResourceType "Microsoft.DevTestLab/labs" -Name $LabName -ResourceGroupName $ResourceGroupName 
if ($lab -eq $null) { throw "Unable to find lab $LabName in subscription $SubscriptionId." } 
    
# Get information about the repository in the lab. 
$repository = Get-AzResource -ResourceGroupName $lab.ResourceGroupName `
    -ResourceType 'Microsoft.DevTestLab/labs/artifactsources' `
    -ResourceName $LabName `
    -ApiVersion 2016-05-15 `
    | Where-Object { $RepositoryName -in ($_.Name, $_.Properties.displayName) } `
    | Select-Object -First 1
if ($repository -eq $null) { throw "Unable to find repository $RepositoryName in lab $LabName." } 

# Get information about the Resource Manager template based on which the environment will be created. 
$template = Get-AzResource -ResourceGroupName $lab.ResourceGroupName `
    -ResourceType "Microsoft.DevTestLab/labs/artifactSources/armTemplates" `
    -ResourceName "$LabName/$($repository.Name)" `
    -ApiVersion 2016-05-15 `
    | Where-Object { $TemplateName -in ($_.Name, $_.Properties.displayName) } `
    | Select-Object -First 1
if ($template -eq $null) { throw "Unable to find template $TemplateName in lab $LabName." } 

# Build the template parameters with parameter name and values.     
$parameters = Get-Member -InputObject $template.Properties.contents.parameters -MemberType NoteProperty | Select-Object -ExpandProperty Name
$templateParameters = @()

# The custom parameters need to be extracted from $Params and formatted as name/value pairs.
$Params | ForEach-Object {
    if ($_ -match '^-param_(.*)' -and $Matches[1] -in $parameters) {
        $name = $Matches[1]                
    } elseif ( $name ) {
        $templateParameters += @{ "name" = "$name"; "value" = "$_" }
        $name = $null #reset name variable
    }
}

# Once name/value pairs are isolated, create an object to hold the necessary template properties
$templateProperties = @{ "deploymentProperties" = @{ "armTemplateId" = "$($template.ResourceId)"; "parameters" = $templateParameters }; } 

# Now, create or deploy the environment in the lab by using the New-AzResource command. 
New-AzResource -Location $Lab.Location `
    -ResourceGroupName $lab.ResourceGroupName `
    -Properties $templateProperties `
    -ResourceType 'Microsoft.DevTestLab/labs/users/environments' `
    -ResourceName "$LabName/$UserId/$EnvironmentName" `
    -ApiVersion '2016-05-15' -Force 
 
Write-Output "Environment $EnvironmentName completed."
```

Можно также использовать Azure CLI для развертывания ресурсов с помощью шаблонов Resource Manager. Дополнительные сведения см. в статье [Развертывание ресурсов с использованием шаблонов Resource Manager и Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli).

> [!NOTE]
> Только пользователь с правами владельца лаборатории может создать виртуальные машины из шаблона Resource Manager с помощью Azure PowerShell. Если необходимо автоматизировать создание виртуальной машины с помощью шаблона Resource Manager и у вас есть только разрешения пользователя, воспользуйтесь [командой CLI](https://docs.microsoft.com/cli/azure/lab/vm#az-lab-vm-create)**az lab vm create**.

### <a name="general-limitations"></a>Общие ограничения 

Учитывайте следующие ограничения при использовании шаблона Resource Manager в DevTest Labs.

- Любой создаваемый вами шаблон Resource Manager не может ссылаться на существующие ресурсы. Он может ссылаться только на новые ресурсы. Кроме того, если у вас есть существующий шаблон Resource Manager, который обычно развертывается за пределами DevTest Labs и содержит ссылки на существующие ресурсы, его нельзя использовать в лаборатории.

   Единственное исключение — вы **можете** ссылаться на существующую виртуальную сеть. 

- Формулы нельзя создавать на основе виртуальных машин лаборатории, которые были созданы из шаблона Resource Manager. 

- Пользовательские образы нельзя создавать на основе виртуальных машин лаборатории, которые были созданы из шаблона Resource Manager. 

- Большинство политик не обрабатываются при развертывании шаблонов Resource Manager.

   Например, может быть установлена политика лаборатории, указывающая, что пользователь может создать только пять виртуальных машин. Тем не менее пользователь может развернуть шаблон Resource Manager, который создает десятки виртуальных машин. К политикам, которые не обрабатываются, относятся следующие:

   - число виртуальных машин на пользователя;
   - число виртуальных машин уровня Premium на пользователя лаборатории;
   - число дисков уровня Premium на пользователя лаборатории.


### <a name="configure-environment-resource-group-access-rights-for-lab-users"></a>Настройка прав доступа к группе ресурсов среды для пользователей лаборатории

Пользователи лаборатории могут развернуть шаблон Resource Manager. Но по умолчанию у них есть только права доступа на чтение. Это означает, что они не могут изменять ресурсы в группе ресурсов среды. Например, они не могут останавливать или запускать свои ресурсы.

Выполните следующие действия, чтобы предоставить пользователям лаборатории права доступа для участников. После этого при развертывании шаблона Resource Manager они смогут редактировать ресурсы в соответствующей среде. 


1. В области лаборатории **Обзор** выберите **Конфигурация и политики**.
1. Выберите **Параметры лаборатории**.
1. В области "Параметры лаборатории" выберите **Участник**, чтобы предоставить пользователям лаборатории разрешения на запись.

    ![Настройка прав доступа пользователей лаборатории](./media/devtest-lab-create-environment-from-arm/configure-access-rights.png)

1. Щелкните **Сохранить**.

## <a name="next-steps"></a>Дальнейшие действия
* К созданной виртуальной машине можно подключиться, щелкнув **Подключиться** в области управления виртуальной машиной.
* Для просмотра и администрирования ресурсов в среде выберите среду в списке **Мои виртуальные машины** в вашей лаборатории. 
* Вы можете ознакомиться с [шаблонами Azure Resource Manager из коллекции шаблонов быстрого запуска Azure](https://github.com/Azure/azure-quickstart-templates).
