---
title: Развертывание студии (классическое) рабочее пространство с менеджером ресурсов Azure
titleSuffix: ML Studio (classic) - Azure
description: Как развернуть рабочее пространство для студии машинного обучения Azure (классический) с помощью шаблона Azure Resource Manager
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 02/05/2018
ms.openlocfilehash: 34333d4fe6e9b34a0c8b56cca8123f4ed93a917a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218121"
---
# <a name="deploy-azure-machine-learning-studio-classic-workspace-using-azure-resource-manager"></a>Развертывание студии машинного обучения Azure (классическое) рабочее пространство с помощью менеджера ресурсов Azure

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Шаблон развертывания Azure Resource Manager позволяет сэкономить время, предоставляя масштабируемый способ развертывания взаимосвязанных компонентов с возможностью проверки и механизмом повтора. Например, для настройки рабочей студии Azure Machine Learning Studio (классические) рабочие места необходимо сначала настроить учетную запись хранения Azure, а затем развернуть рабочее пространство. Представьте себе выполнение этого задания вручную для сотен рабочих областей. Проще йога — использовать шаблон Azure Resource Manager для развертывания рабочего пространства студии (классического) и всех его зависимостей. В этой статье представлено пошаговое выполнение этого процесса. Подробный обзор Azure Resource Manager см. в статье [Общие сведения об Azure Resource Manager](../../azure-resource-manager/management/overview.md).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Пошаговое создание рабочей области машинного обучения
Мы создадим группу ресурсов Azure, а затем разместим новую учетную запись хранения Azure и новую рабочую студию Azure Machine Learning Studio с помощью шаблона «Менеджер ресурсов». После завершения развертывания мы выведем важные сведения о созданных рабочих областях (первичный ключ, идентификатор и URL-адрес рабочей области).

### <a name="create-an-azure-resource-manager-template"></a>Создание шаблона Azure Resource Manager

Рабочей области машинного обучения требуется учетная запись хранения Azure для хранения связанного с ней набора данных.
В следующем шаблоне для создания имени учетной записи хранения и рабочей области используется имя группы ресурсов.  Имя учетной записи хранения также используется в качестве свойства при создании рабочей области.

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
Сохраните этот шаблон как файл mlworkspace.json в папке C:\temp\.

### <a name="deploy-the-resource-group-based-on-the-template"></a>Развертывание группы ресурсов на основе шаблона

* Откройте PowerShell.
* Установите модули для Azure Resource Manager и управления службами Azure.

```powershell
# Install the Azure Resource Manager modules from the PowerShell Gallery (press "A")
Install-Module Az -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press "A")
Install-Module Azure -Scope CurrentUser
```

   При этом скачиваются и устанавливаются модули, необходимые для выполнения оставшихся действий. Их достаточно выполнить один раз в среде выполнения команд PowerShell.

* Проверка подлинности в Azure

```powershell
# Authenticate (enter your credentials in the pop-up window)
Connect-AzAccount
```
Этот шаг необходимо выполнять для каждого сеанса. После выполнения проверки подлинности должны отображаться сведения о подписке.

![Учетная запись Azure](./media/deploy-with-resource-manager-template/azuresubscription.png)

Получив доступ к Azure, мы можем создать группу ресурсов.

* Создание группы ресурсов

```powershell
$rg = New-AzResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Убедитесь, что группа ресурсов должным образом подготовлена к работе. **Положение должно** быть "Успешно".
Шаблон использует имя группы ресурсов для создания имени учетной записи хранения. Имя учетной записи хранения должно содержать от 3 до 24 знаков и состоять только из цифр и букв нижнего регистра.

![Группа ресурсов](./media/deploy-with-resource-manager-template/resourcegroupprovisioning.png)

* Разверните новую рабочую область машинного обучения с помощью развертывания группы ресурсов.

```powershell
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

После завершения развертывания очень просто получить доступ к свойствам развернутой рабочей области. Например, можно получить доступ к сведениям о маркере первичного ключа.

```powershell
# Access Azure Machine Learning Studio Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Еще один способ получения токенов существующего рабочего пространства — использование команды Invoke-AzResourceAction. Например, можно отобразить список основных и дополнительных маркеров всех рабочих областей.

```powershell
# List the primary and secondary tokens of all workspaces
Get-AzResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |ForEach-Object { Invoke-AzResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}
```
После того, как рабочее пространство подготовлено, вы также можете автоматизировать многие задачи Azure Machine Learning Studio (классические) с помощью [модуля PowerShell для студии машинного обучения Azure (классический).](https://aka.ms/amlps)

## <a name="next-steps"></a>Дальнейшие действия

* Узнайте больше о [создании шаблонов Azure Resource Manager](../../azure-resource-manager/templates/template-syntax.md).
* Просмотрите [репозиторий шаблонов быстрого запуска Azure](https://github.com/Azure/azure-quickstart-templates).
* Просмотрите видео об [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39).
* Подробные сведения см. в статье [Microsoft.MachineLearning resource types](https://docs.microsoft.com/azure/templates/microsoft.machinelearning/allversions) (Типы ресурсов Microsoft.MachineLearning)

<!--Link references-->
