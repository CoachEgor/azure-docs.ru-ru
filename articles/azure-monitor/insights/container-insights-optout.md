---
title: Как остановить мониторинг кластера Службы Azure Kubernetes | Документация Майкрософт
description: В этой статье описывается, как остановить мониторинг кластера Azure AKS с помощью Azure Monitor для контейнеров.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/13/2018
ms.author: magoedte
ms.openlocfilehash: 0e4268cb3a8d6ac62da12f689560338eee7e6935
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65071817"
---
# <a name="how-to-stop-monitoring-your-azure-kubernetes-service-aks-with-azure-monitor-for-containers"></a>Как остановить мониторинг кластера Службы Azure Kubernetes (AKS) с помощью Azure Monitor для контейнеров

Если после включения мониторинга вашего кластера AKS вы решите, что больше не хотите его отслеживать, вы можете остановить мониторинг. В этой статье показано, как это сделать с помощью Azure CLI или предоставленных шаблонов Azure Resource Manager.  


## <a name="azure-cli"></a>Инфраструктура CLI Azure
Используйте команду [​az aks disable-addons](https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-disable-addons), чтобы отключить Azure Monitor для контейнеров. Команда удаляет агент из узлов кластера, он не удаляет решение или данные, собранные и сохраненные в ресурсе Azure Monitor.  

```azurecli
az aks disable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG
```

Сведения о повторном включении мониторинга кластера см. в разделе [Включение мониторинга с помощью Azure CLI](container-insights-enable-new-cluster.md#enable-using-azure-cli).

## <a name="azure-resource-manager-template"></a>Шаблон диспетчера ресурсов Azure
Предоставлены два шаблона Azure Resource Manager, которые позволяют последовательно и многократно удалять ресурсы решения в вашей группе ресурсов. Один шаблон JSON задает конфигурацию прекращения мониторинга, а другой содержит значения параметров, которые можно настроить, чтобы указать идентификатор ресурса кластера AKS и группу ресурсов, в которой он развернут. 

Если вы не знакомы с концепцией развертывания ресурсов с помощью шаблона, ознакомьтесь со статьями:
* [Развертывание ресурсов с использованием шаблонов Resource Manager и Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)
* [Развертывание ресурсов с использованием шаблонов Resource Manager и Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md)

>[!NOTE]
>Развертывание шаблона должно проходить в той же группе ресурсов, что и у кластера. Если вы опустите какие-либо другие свойства или надстройки при использовании этого шаблона, это может привести к удалению их из кластера. Например, *enableRBAC*.  
>

Если вы решили использовать Azure CLI, необходимо сначала установить интерфейс командной строки и использовать его локально. Необходимо запустить Azure CLI версии 2.0.27 или более поздней. Для определения версии выполните `az --version`. Если вам необходимо установить или обновить Azure CLI, ознакомьтесь со статьей [Установка Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

### <a name="create-template"></a>Создание шаблона

1. Скопируйте и вставьте в него следующий синтаксис JSON:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "aksResourceId": {
           "type": "string",
           "metadata": {
             "description": "AKS Cluster Resource ID"
           }
       },
      "aksResourceLocation": {
        "type": "string",
        "metadata": {
           "description": "Location of the AKS resource e.g. \"East US\""
         }
       }
    },
    "resources": [
      {
        "name": "[split(parameters('aksResourceId'),'/')[8]]",
        "type": "Microsoft.ContainerService/managedClusters",
        "location": "[parameters('aksResourceLocation')]",
        "apiVersion": "2018-03-31",
        "properties": {
          "mode": "Incremental",
          "id": "[parameters('aksResourceId')]",
          "addonProfiles": {
            "omsagent": {
              "enabled": false,
              "config": null
            }
           }
         }
       }
      ]
    }
    ```

2. Сохраните этот файл как **OptOutTemplate.json** в локальной папке.
3. Вставьте в него следующий синтаксис JSON:

    ```json
    {
     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "aksResourceId": {
         "value": "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
      },
      "aksResourceLocation": {
        "value": "<aksClusterRegion>"
        }
      }
    }
    ```

4. Замените значения **aksResourceId** и **aksResourceLocation** значениями кластера AKS, которые можно найти на странице **свойств** выбранного кластера.

    ![Страница свойств контейнера](media/container-insights-optout/container-properties-page.png)

    На странице **Свойства** также скопируйте **идентификатор ресурса рабочей области**. Это значение потребуется, если позже вы решите удалить рабочую область Log Analytics, что выходит за рамки этого процесса. 

5. Сохраните этот файл как **OptOutParam.json** в локальной папке.
6. Теперь вы можете развернуть этот шаблон. 

### <a name="remove-the-solution-using-azure-cli"></a>Удаление решения с помощью Azure CLI
Выполните следующую команду с помощью Azure CLI в Linux, чтобы удалить решение и очистить конфигурацию в кластере AKS.

```azurecli
az login   
az account set --subscription "Subscription Name" 
az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
```

Изменение конфигурации может занять несколько минут. По завершении выполнения вы увидите сообщение, подобное приведенному ниже, с возвращенным результатом:

```azurecli
ProvisioningState       : Succeeded
```

### <a name="remove-the-solution-using-powershell"></a>Удаление решения с помощью PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Выполните следующие команды PowerShell в папке с шаблоном, чтобы удалить решение и очистить конфигурацию в кластере AKS.    

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName <yourSubscriptionName>
New-AzResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
```

Изменение конфигурации может занять несколько минут. По завершении выполнения вы увидите сообщение, подобное приведенному ниже, с возвращенным результатом:

```powershell
ProvisioningState       : Succeeded
```

Если рабочая область была создана только для мониторинга кластера и больше не требуется, ее необходимо удалить вручную. Если вам неизвестно, как удалить рабочую область, прочитайте раздел [Удаление рабочей области Azure Log Analytics на портале Azure](../../log-analytics/log-analytics-manage-del-workspace.md). Не забывайте об **идентификаторе ресурса рабочей области**, скопированном ранее на шаге 4, он вам еще понадобится. 

