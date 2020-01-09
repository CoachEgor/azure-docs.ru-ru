---
title: Перемещение внешних Load Balancer Azure в другой регион Azure с помощью Azure PowerShell
description: Используйте шаблон Azure Resource Manager, чтобы переместить внешние Load Balancer Azure из одного региона Azure в другой с помощью Azure PowerShell.
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/17/2019
ms.author: allensu
ms.openlocfilehash: a24eb4608e7630d5b613751fa2120361eccd7672
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/03/2020
ms.locfileid: "75644823"
---
# <a name="move-azure-external-load-balancer-to-another-region-using-azure-powershell"></a>Перемещение внешних Load Balancer Azure в другой регион с помощью Azure PowerShell

Существуют различные сценарии, в которых необходимо переместить имеющуюся внешнюю подсистему балансировки нагрузки из одного региона в другой. Например, может потребоваться создать внешнюю подсистему балансировки нагрузки с той же конфигурацией для тестирования. При планировании аварийного восстановления может также потребоваться переместить внешнюю подсистему балансировки нагрузки в другой регион.

Внешние подсистемы балансировки нагрузки Azure невозможно переместить из одного региона в другой. Однако можно использовать шаблон Azure Resource Manager для экспорта существующей конфигурации и общедоступного IP-адреса внешней подсистемы балансировки нагрузки.  Затем можно разместить ресурс в другом регионе, экспортировав подсистему балансировки нагрузки и общедоступный IP-адрес в шаблон, изменив параметры в соответствии с регионом назначения, а затем развернув шаблоны в новом регионе.  Дополнительные сведения о диспетчер ресурсов и шаблонах см. [в статье экспорт групп ресурсов в шаблоны](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates) .


## <a name="prerequisites"></a>Технические условия

- Убедитесь, что внешняя подсистема балансировки нагрузки Azure находится в регионе Azure, из которого вы хотите переместиться.

- Внешние подсистемы балансировки нагрузки Azure нельзя перемещать между регионами.  Необходимо связать новую подсистему балансировки нагрузки с ресурсами в целевом регионе.

- Чтобы экспортировать конфигурацию внешней подсистемы балансировки нагрузки и развернуть шаблон для создания внешней подсистемы балансировки нагрузки в другом регионе, вам потребуется роль "участник сети" или выше.
   
- Определите структуру сети в исходном регионе и все ресурсы, которые вы сейчас используете. Этот макет включает, но не ограничен подсистемами балансировки нагрузки, группами безопасности сети, общедоступными IP-адресами и виртуальными сетями.

- Убедитесь, что подписка Azure позволяет создавать внешние подсистемы балансировки нагрузки в используемом целевом регионе. Свяжитесь со службой поддержки, чтобы включить необходимые квоты.

- Убедитесь, что у вашей подписки достаточно ресурсов для поддержки добавления подсистем балансировки нагрузки для этого процесса.  См. статью [Подписка Azure, границы, квоты и ограничения службы](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits).


## <a name="prepare-and-move"></a>Подготовка и перемещение
Ниже показано, как подготовить внешнюю подсистему балансировки нагрузки для перемещения с помощью шаблона диспетчер ресурсов и переместить конфигурацию внешней подсистемы балансировки нагрузки в целевую область с помощью Azure PowerShell.  В рамках этого процесса должна быть включена общедоступная IP-конфигурация внешней подсистемы балансировки нагрузки, которая должна быть выполнена перед перемещением внешней подсистемы балансировки нагрузки.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-public-ip-template-and-deploy-from-azure-powershell"></a>Экспорт шаблона общедоступного IP-адреса и развертывание из Azure PowerShell

1. Войдите в подписку Azure с помощью команды [Connect-азаккаунт](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) и следуйте инструкциям на экране.
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```
2. Получите идентификатор ресурса общедоступного IP-адреса, который необходимо переместить в целевой регион, и поместите его в переменную с помощью команды [Get-азпублиЦипаддресс](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=azps-2.6.0):

    ```azurepowershell-interactive
    $sourcePubIPID = (Get-AzPublicIPaddress -Name <source-public-ip-name> -ResourceGroupName <source-resource-group-name>).Id

    ```
3. Экспортируйте общедоступный исходный IP-адрес в JSON-файл в каталог, в котором выполняется команда [Export — азресаурцеграуп](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0):
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceVNETID -IncludeParameterDefaultValue
   ```

4. Скачанный файл будет называться именем группы ресурсов, из которой был экспортирован ресурс.  Выберите файл, который был экспортирован из команды с именем **\<Resource-Group-name >. JSON** , и откройте ее в любом редакторе:
   
   ```azurepowershell
   notepad.exe <source-resource-group-name>.json
   ```

5. Чтобы изменить параметр имени общедоступного IP-адреса, измените значение свойства **DefaultValue** исходного общедоступного IP-адреса на имя целевого общедоступного IP-адреса, а затем убедитесь, что оно находится в кавычках:
    
    ```json
        {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publicIPAddresses_myVM1pubIP_name": {
        "defaultValue": "<target-publicip-name>",
        "type": "String"
        }
    }

    ```

6. Чтобы изменить целевой регион, в который будет перемещен общедоступный IP-адрес, измените свойство **Location** в разделе ресурсы:

    ```json
            "resources": [
            {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2019-06-01",
            "name": "[parameters('publicIPAddresses_myPubIP_name')]",
            "location": "<target-region>",
            "sku": {
                "name": "Basic",
                "tier": "Regional"
            },
            "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "7549a8f1-80c2-481a-a073-018f5b0b69be",
                "ipAddress": "52.177.6.204",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Dynamic",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
               }
               }
             ]             
    ```
  
7. Чтобы получить коды расположения регионов, можно использовать командлет Azure PowerShell [Get-азлокатион](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) , выполнив следующую команду:

    ```azurepowershell-interactive

    Get-AzLocation | format-table
    
    ```
8. Кроме того, можно изменить другие параметры в шаблоне, если вы выбираете и являются необязательными в зависимости от ваших требований:

    * **SKU** . Вы можете изменить номер SKU общедоступного IP-адреса в конфигурации с Standard на Basic или Basic на Standard, изменив свойство **SKU** > **Name** в файле **\<Resource-Group-name >. JSON** :

         ```json
            "resources": [
                   {
                    "type": "Microsoft.Network/publicIPAddresses",
                    "apiVersion": "2019-06-01",
                    "name": "[parameters('publicIPAddresses_myPubIP_name')]",
                    "location": "<target-region>",
                    "sku": {
                        "name": "Basic",
                        "tier": "Regional"
                    },
         ```

         Дополнительные сведения о различиях между общедоступными IP-адресами уровня "базовый" и "Стандартный" см. [в статье Создание, изменение или удаление общедоступного адреса](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address).

    * **Метод выделения общедоступного IP-адреса** и **время ожидания простоя** . Вы можете изменить оба этих параметра в шаблоне, изменив свойство **публиЦипаллокатионмесод** с **dynamic** на **static** или **static** на **dynamic**. Время ожидания простоя можно изменить, изменив свойство **идлетимеаутинминутес** на желаемое значение.  Значение по умолчанию — **4**:

         ```json
         "resources": [
                {
                "type": "Microsoft.Network/publicIPAddresses",
                "apiVersion": "2019-06-01",
                "name": "[parameters('publicIPAddresses_myPubIP_name')]",
                "location": "<target-region>",
                  "sku": {
                  "name": "Basic",
                  "tier": "Regional"
                 },
                "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "7549a8f1-80c2-481a-a073-018f5b0b69be",
                "ipAddress": "52.177.6.204",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Dynamic",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
                   }
                }            
         ```

        Дополнительные сведения о методах выделения и значениях времени ожидания простоя см. в разделе [Создание, изменение или удаление общедоступного IP-адреса](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address).


9. Сохраните файл **\<Resource-Group-name >. JSON** .

10. Создайте группу ресурсов в целевом регионе для развертывания целевого общедоступного IP-адреса с помощью [New-азресаурцеграуп](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0).
    
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <target-resource-group-name> -location <target-region>
    ```
11. Разверните измененный файл **\<Resource-Group-name >. JSON** в группе ресурсов, созданной на предыдущем шаге, с помощью команды [New-азресаурцеграупдеплоймент](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-2.6.0):

    ```azurepowershell-interactive

    New-AzResourceGroupDeployment -ResourceGroupName <target-resource-group-name> -TemplateFile <source-resource-group-name>.json
    
    ```

12. Чтобы проверить, созданы ли ресурсы в целевом регионе, используйте [Get-азресаурцеграуп](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup?view=azps-2.6.0) и [Get-азпублиЦипаддресс](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=azps-2.6.0):
    
    ```azurepowershell-interactive

    Get-AzResourceGroup -Name <target-resource-group-name>

    ```

    ```azurepowershell-interactive

    Get-AzPublicIPAddress -Name <target-publicip-name> -ResourceGroupName <target-resource-group-name>

    ```

### <a name="export-the-external-load-balancer-template-and-deploy-from-azure-powershell"></a>Экспорт шаблона внешней подсистемы балансировки нагрузки и развертывание из Azure PowerShell

1. Войдите в подписку Azure с помощью команды [Connect-азаккаунт](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) и следуйте инструкциям на экране.
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```

2. Получите идентификатор ресурса внешней подсистемы балансировки нагрузки, которую необходимо переместить в целевой регион, и поместите ее в переменную с помощью команды [Get-азлоадбаланцер](https://docs.microsoft.com/powershell/module/az.network/get-azloadbalancer?view=azps-2.6.0):

    ```azurepowershell-interactive
    $sourceExtLBID = (Get-AzLoadBalancer -Name <source-external-lb-name> -ResourceGroupName <source-resource-group-name>).Id

    ```
3. Экспортируйте конфигурацию внешнего подсистемы балансировки нагрузки исходного кода в JSON-файл в каталог, в котором выполняется команда [Export — азресаурцеграуп](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0):
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceExtLBID -IncludeParameterDefaultValue
   ```
4. Скачанный файл будет называться именем группы ресурсов, из которой был экспортирован ресурс.  Выберите файл, который был экспортирован из команды с именем **\<Resource-Group-name >. JSON** , и откройте ее в любом редакторе:
   
   ```azurepowershell
   notepad.exe <source-resource-group-name>.json
   ```

5. Чтобы изменить параметр имени внешней подсистемы балансировки нагрузки, измените значение свойства **DefaultValue** в поле имя внешней подсистемы балансировки нагрузки на имя целевой внешней подсистемы балансировки нагрузки и убедитесь, что имя находится в кавычках:

    ```json
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        "loadBalancers_myLoadbalancer_ext_name": {
        "defaultValue": "<target-external-lb-name>",
        "type": "String"
            },
        "publicIPAddresses_myPubIP_in_externalid": {
        "defaultValue": "<target-publicIP-resource-ID>",
        "type": "String"
            },

    ```

6.  Чтобы изменить значение целевого общедоступного IP-адреса, который был перемещен выше, сначала необходимо получить идентификатор ресурса, а затем скопировать и вставить его в файл **\<Resource-Group-name >. JSON** .  Чтобы получить идентификатор, используйте команду [Get-азпублиЦипаддресс](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=azps-2.6.0):

    ```azurepowershell-interactive
    $targetPubIPID = (Get-AzPublicIPaddress -Name <target-public-ip-name> -ResourceGroupName <target-resource-group-name>).Id
    ```
    Введите переменную и нажмите клавишу ВВОД, чтобы отобразить идентификатор ресурса.  Выделите путь к ИДЕНТИФИКАТОРу и скопируйте его в буфер обмена:

    ```powershell
    PS C:\> $targetPubIPID
    /subscriptions/7668d659-17fc-4ffd-85ba-9de61fe977e8/resourceGroups/myResourceGroupLB-Move/providers/Microsoft.Network/publicIPAddresses/myPubIP-in-move
    ```

7.  В файле **\<Resource-Group-name >. JSON** вставьте **идентификатор ресурса** из переменной вместо **DefaultValue** во втором параметре внешнего идентификатора общедоступного IP-адреса. Убедитесь, что путь заключен в кавычки:

    ```json
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
            "loadBalancers_myLoadbalancer_ext_name": {
            "defaultValue": "<target-external-lb-name>",
            "type": "String"
            },
            "publicIPAddresses_myPubIP_in_externalid": {
            "defaultValue": "<target-publicIP-resource-ID>",
            "type": "String"
            },

    ```

8.  Если вы настроили исходящие правила NAT и исходящего трафика для балансировщика нагрузки, в этом файле будет указана третья запись для внешнего идентификатора исходящего общедоступного IP-адреса.  Повторите описанные выше действия в **целевом регионе** , чтобы получить идентификатор для исходящего общедоступного iP-адреса, и вставьте эту запись в файл **\<Resource-group-name >. JSON** :

    ```json
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
                "loadBalancers_myLoadbalancer_ext_name": {
                "defaultValue": "<target-external-lb-name>",
                "type": "String"
            },
                "publicIPAddresses_myPubIP_in_externalid": {
                "defaultValue": "<target-publicIP-resource-ID>",
                "type": "String"
            },
                "publicIPAddresses_myPubIP_out_externalid": {
                "defaultValue": "<target-publicIP-outbound-resource-ID>",
                "type": "String"
            }
        },
    ```

10. Чтобы изменить целевой регион, в который будет перемещена конфигурация внешней подсистемы балансировки нагрузки, измените свойство **Location** в разделе **ресурсы** в файле **\<resource-Group-name >. JSON** :

    ```json
        "resources": [
            {
                "type": "Microsoft.Network/loadBalancers",
                "apiVersion": "2019-06-01",
                "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
                "location": "<target-external-lb-region>",
                "sku": {
                    "name": "Standard",
                    "tier": "Regional"
                },
    ```

11. Чтобы получить коды расположения регионов, можно использовать командлет Azure PowerShell [Get-азлокатион](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) , выполнив следующую команду:

    ```azurepowershell-interactive

    Get-AzLocation | format-table
    
    ```
12. Кроме того, можно изменить другие параметры в шаблоне, если вы выбираете и являются необязательными в зависимости от ваших требований:
    
    * **SKU** . Вы можете изменить номер SKU внешней подсистемы балансировки нагрузки с Standard на Basic или Basic на Standard, изменив свойство **SKU** > **Name** в файле **\<Resource-Group-name >. JSON** :

        ```json
        "resources": [
        {
            "type": "Microsoft.Network/loadBalancers",
            "apiVersion": "2019-06-01",
            "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
            "location": "<target-external-lb-region>",
            "sku": {
                "name": "Standard",
                "tier": "Regional"
            },
        ```
      Дополнительные сведения о различиях между подсистемами балансировки нагрузки уровня "базовый" и "Стандартный" см. в статье [обзор Load Balancer (цен. Категория "Стандартный") Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) .

    * **Правила балансировки нагрузки** . в конфигурацию можно добавить или удалить правила балансировки нагрузки, добавив или удалив записи в\<разделе **лоадбаланЦингрулес** файла **resource-Group-name >. JSON** :

        ```json
        "loadBalancingRules": [
                    {
                        "name": "myInboundRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "frontendIPConfiguration": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                            },
                            "frontendPort": 80,
                            "backendPort": 80,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 4,
                            "protocol": "Tcp",
                            "enableTcpReset": false,
                            "loadDistribution": "Default",
                            "disableOutboundSnat": true,
                            "backendAddressPool": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/backendAddressPools/myBEPoolInbound')]"
                            },
                            "probe": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/probes/myHTTPProbe')]"
                            }
                        }
                    }
                ]
        ```
       Дополнительные сведения о правилах балансировки нагрузки см [. в разделе что такое Azure Load Balancer?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)

    * **Пробы** . Вы можете добавить или удалить пробу для балансировщика нагрузки в конфигурации, добавив или удалив записи в разделе " **зонды** " файла **\<resource-Group-name >. JSON** :

        ```json
        "probes": [
                    {
                        "name": "myHTTPProbe",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "protocol": "Http",
                            "port": 80,
                            "requestPath": "/",
                            "intervalInSeconds": 15,
                            "numberOfProbes": 2
                        }
                    }
                ],
        ```
       Дополнительные сведения о Azure Load Balancer зондах работоспособности см. в разделе [проверки работоспособности Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview) .

    * **Правила NAT для входящего трафика** . Вы можете добавить или удалить правила NAT для входящего трафика для подсистемы балансировки нагрузки, добавив или удалив записи в\<разделе **inboundNatRules** файла **resource-Group-name >. JSON** :

        ```json
        "inboundNatRules": [
                    {
                        "name": "myInboundNATRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "frontendIPConfiguration": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                            },
                            "frontendPort": 4422,
                            "backendPort": 3389,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 4,
                            "protocol": "Tcp",
                            "enableTcpReset": false
                        }
                    }
                ]
        ```
        Для завершения добавления или удаления правила NAT для входящего трафика правило должно присутствовать или удалено в качестве свойства **типа** в конце файла **\<Resource-group-name >. JSON** :

        ```json
        {
            "type": "Microsoft.Network/loadBalancers/inboundNatRules",
            "apiVersion": "2019-06-01",
            "name": "[concat(parameters('loadBalancers_myLoadBalancer_name'), '/myInboundNATRule')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name'))]"
            ],
            "properties": {
                "provisioningState": "Succeeded",
                "frontendIPConfiguration": {
                    "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                },
                "frontendPort": 4422,
                "backendPort": 3389,
                "enableFloatingIP": false,
                "idleTimeoutInMinutes": 4,
                "protocol": "Tcp",
                "enableTcpReset": false
            }
        }
        ```
        Дополнительные сведения о правилах NAT для входящего трафика см. в разделе [что такое Azure Load Balancer?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)

    * **Правила исходящего трафика** . Вы можете добавить или удалить правила исходящего трафика в конфигурации, изменив свойство **аутбаундрулес** в файле **\<resource-Group-name >. JSON** :

        ```json
        "outboundRules": [
                    {
                        "name": "myOutboundRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "allocatedOutboundPorts": 10000,
                            "protocol": "All",
                            "enableTcpReset": false,
                            "idleTimeoutInMinutes": 15,
                            "backendAddressPool": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/backendAddressPools/myBEPoolOutbound')]"
                            },
                            "frontendIPConfigurations": [
                                {
                                    "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPoutbound')]"
                                }
                            ]
                        }
                    }
                ]
        ```

         Дополнительные сведения о правилах исходящих подключений см. в разделе [Load Balancer правила исходящего трафика](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview) .

13. Сохраните файл **\<Resource-Group-name >. JSON** .
    
10. Создайте или группу ресурсов в целевом регионе для развертывания целевой внешней подсистемы балансировки нагрузки с помощью [New-азресаурцеграуп](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0). Существующую группу ресурсов, приведенную выше, можно также использовать повторно в рамках этого процесса:
    
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <target-resource-group-name> -location <target-region>
    ```
11. Разверните измененный файл **\<Resource-Group-name >. JSON** в группе ресурсов, созданной на предыдущем шаге, с помощью команды [New-азресаурцеграупдеплоймент](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-2.6.0):

    ```azurepowershell-interactive

    New-AzResourceGroupDeployment -ResourceGroupName <target-resource-group-name> -TemplateFile <source-resource-group-name>.json
    
    ```

12. Чтобы проверить, созданы ли ресурсы в целевом регионе, используйте [Get-азресаурцеграуп](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup?view=azps-2.6.0) и [Get-азлоадбаланцер](https://docs.microsoft.com/powershell/module/az.network/get-azloadbalancer?view=azps-2.6.0):
    
    ```azurepowershell-interactive

    Get-AzResourceGroup -Name <target-resource-group-name>

    ```

    ```azurepowershell-interactive

    Get-AzLoadBalancer -Name <target-publicip-name> -ResourceGroupName <target-resource-group-name>

    ```

## <a name="discard"></a>Игнорировать 

Если после развертывания вы хотите начать или отклонить общедоступный IP-адрес и подсистему балансировки нагрузки в целевом объекте, удалите группу ресурсов, созданную в целевом объекте, и перемещенный общедоступный IP-адрес и подсистема балансировки нагрузки будут удалены.  Чтобы удалить группу ресурсов, используйте команду [Remove-азресаурцеграуп](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0):

```azurepowershell-interactive

Remove-AzResourceGroup -Name <resource-group-name>

```

## <a name="clean-up"></a>Очистка

Чтобы зафиксировать изменения и завершить перемещение NSG, удалите исходный NSG или группу ресурсов, используйте [Remove-азресаурцеграуп](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0) или Remove- [АзпублиЦипаддресс](https://docs.microsoft.com/powershell/module/az.network/remove-azpublicipaddress?view=azps-2.6.0) и [Remove-азлоадбаланцер](https://docs.microsoft.com/powershell/module/az.network/remove-azloadbalancer?view=azps-2.6.0) .

```azurepowershell-interactive

Remove-AzResourceGroup -Name <resource-group-name>

```

``` azurepowershell-interactive

Remove-AzLoadBalancer -name <load-balancer> -ResourceGroupName <resource-group-name>

Remove-AzPublicIpAddress -Name <public-ip> -ResourceGroupName <resource-group-name>


```

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы переместили группу безопасности сети Azure из одного региона в другой и очистили исходные ресурсы.  Дополнительные сведения о перемещении ресурсов между регионами и аварийным восстановлением в Azure см. по следующим ссылке:


- [Перемещение ресурсов в новую группу ресурсов или подписку](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Перенос виртуальных машин Azure в другой регион](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
