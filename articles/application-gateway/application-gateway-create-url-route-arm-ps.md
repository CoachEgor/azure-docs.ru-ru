---
title: Правила маршрутизации на основе URL с помощью PowerShell
titleSuffix: Azure Application Gateway
description: Узнайте, как создать правила маршрутизации на основе URL-пути для шлюза приложений и масштабируемый набор виртуальных машин с помощью Azure PowerShell.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: victorh
ms.openlocfilehash: e7934ba0b33bff7ffb8e89e7b56c5b998a232289
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "74048054"
---
# <a name="create-an-application-gateway-with-url-path-based-routing-rules-using-azure-powershell"></a>Создание шлюза приложений с правилами маршрутизации на основе URL-пути при помощи Azure PowerShell

Для настройки [правил маршрутизации на основе URL-пути](application-gateway-url-route-overview.md) при создании [шлюза приложений](application-gateway-introduction.md) можно использовать Azure PowerShell. В этом руководстве мы создадим внутренние пулы с использованием [масштабируемого набора виртуальных машин](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Затем мы создадим правила маршрутизации, которые обеспечивают поступление веб-трафика на соответствующие серверы в пуле.

Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Настройка сети
> * создание шлюза приложений с сопоставлением URL-адресов;
> * создание масштабируемых наборов виртуальных машин с внутренними пулами.

![Пример маршрутизации для URL-адресов](./media/application-gateway-create-url-route-arm-ps/scenario.png)

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Чтобы установить и использовать PowerShell локально для работы с этим руководством, вам понадобится модуль Azure PowerShell. Чтобы узнать версию, выполните команду `Get-Module -ListAvailable Az`. Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](/powershell/azure/install-az-ps). Если модуль PowerShell запущен локально, необходимо также выполнить командлет `Connect-AzAccount`, чтобы создать подключение к Azure.

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Группа ресурсов — это логический контейнер, в котором происходит развертывание ресурсов Azure и управление ими. Создайте группу ресурсов Azure с помощью командлета [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup).  

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Создание сетевых ресурсов

Создайте конфигурации подсетей с именами *myAGSubnet* и *myBackendSubnet*, выполнив командлет [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig). Создайте виртуальную сеть с именем *myVNet*, используя командлет [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) с конфигурациями подсетей. Наконец, создайте общедоступный IP-адрес с именем *myAGPublicIPAddress*, выполнив командлет [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress). Эти ресурсы используются для обеспечения сетевого подключения к шлюзу приложений и связанным с ним ресурсам.

```azurepowershell-interactive
$backendSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24
$agSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.2.0/24
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $backendSubnetConfig, $agSubnetConfig
$pip = New-AzPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway"></a>Создание шлюза приложений

### <a name="create-the-ip-configurations-and-frontend-port"></a>Создание IP-конфигураций и интерфейсного порта

Свяжите созданную ранее подсеть *myAGSubnet* со шлюзом приложений, используя командлет [New-AzApplicationGatewayIPConfiguration](/powershell/module/az.network/new-azapplicationgatewayipconfiguration). Назначайте *myAGPublicIPАдрес* на шлюз приложения с помощью [New-AzApplicationGatewayFrontFRONTIPConfig](/powershell/module/az.network/new-azapplicationgatewayfrontendipconfig).

```azurepowershell-interactive
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet
$subnet=$vnet.Subnets[0]
$pip = Get-AzPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Name myAGPublicIPAddress
$gipconfig = New-AzApplicationGatewayIPConfiguration `
  -Name myAGIPConfig `
  -Subnet $subnet
$fipconfig = New-AzApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -PublicIPAddress $pip
$frontendport = New-AzApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 80
```

### <a name="create-the-default-pool-and-settings"></a>Создание пула по умолчанию и настройка параметров

Создайте для шлюза приложений серверный пул по умолчанию с именем *appGatewayBackendPool* с помощью командлета [New-AzApplicationGatewayBackendAddressPool](/powershell/module/az.network/new-azapplicationgatewaybackendaddresspool). Настройте параметры для серверного пула, используя командлет [New-AzApplicationGatewayBackendHttpSettings](/powershell/module/az.network/new-azapplicationgatewaybackendhttpsetting).

```azurepowershell-interactive
$defaultPool = New-AzApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool 
$poolSettings = New-AzApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-default-listener-and-rule"></a>Создание прослушивателя по умолчанию и правила

Прослушиватель требуется для того, чтобы шлюз приложений правильно маршрутизировал трафик на внутренние пулы. В этом руководстве создается два прослушивателя. Первым создается базовый прослушиватель, который ожидает передачи данных по корневому URL-адресу. Вторым создается прослушиватель, который ожидает передачи данных по определенным URL-адресам.

Создайте прослушиватель по умолчанию с именем *myDefaultListener*, используя командлет [New-AzApplicationGatewayHttpListener](/powershell/module/az.network/new-azapplicationgatewayhttplistener), с конфигурацией внешнего интерфейса и интерфейсным портом, созданными ранее. Правило требуется для того, чтобы указать прослушивателю, какой внутренний пул использовать для входящего трафика. Создайте базовое правило *rule1* с помощью командлета [New-AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/new-azapplicationgatewayrequestroutingrule).

```azurepowershell-interactive
$defaultlistener = New-AzApplicationGatewayHttpListener `
  -Name myDefaultListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$frontendRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultlistener `
  -BackendAddressPool $defaultPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>Создание шлюза приложений

Теперь, когда вы создали необходимые вспомогательные ресурсы, укажите параметры для шлюза приложений *myAppGateway* с помощью командлета [New-AzApplicationGatewaySku](/powershell/module/az.network/new-azapplicationgatewaysku), а затем создайте шлюз с помощью командлета [New-AzApplicationGateway](/powershell/module/az.network/new-azapplicationgateway).

```azurepowershell-interactive
$sku = New-AzApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2
$appgw = New-AzApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $defaultPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $defaultlistener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku
```

### <a name="add-image-and-video-backend-pools-and-port"></a>Добавление внутреннего порта и внутренних пулов для изображений и видео

Вы можете добавить бэкэнд пулы под названием *imagesBackendPool* и *videoBackendPool* в шлюз приложения с помощью [Add-AzApplicationGatewayBackendAddressPool.](/powershell/module/az.network/add-azapplicationgatewaybackendaddresspool) Добавьте интерфейсный порт для пулов с помощью командлета [Add-AzApplicationGatewayFrontendPort](/powershell/module/az.network/add-azapplicationgatewayfrontendport). Затем отправьте изменения в шлюз приложений с помощью командлета [Set-AzApplicationGateway](/powershell/module/az.network/set-azapplicationgateway).

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
Add-AzApplicationGatewayBackendAddressPool `
  -ApplicationGateway $appgw `
  -Name imagesBackendPool 
Add-AzApplicationGatewayBackendAddressPool `
  -ApplicationGateway $appgw `
  -Name videoBackendPool
Add-AzApplicationGatewayFrontendPort `
  -ApplicationGateway $appgw `
  -Name bport `
  -Port 8080
Set-AzApplicationGateway -ApplicationGateway $appgw
```

### <a name="add-backend-listener"></a>Добавление серверного прослушивателя

Добавьте серверный прослушиватель с именем *backendListener*, необходимый для маршрутизации трафика, с помощью командлета [Add-AzApplicationGatewayHttpListener](/powershell/module/az.network/add-azapplicationgatewayhttplistener).

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
$backendPort = Get-AzApplicationGatewayFrontendPort `
  -ApplicationGateway $appgw `
  -Name bport
$fipconfig = Get-AzApplicationGatewayFrontendIPConfig `
  -ApplicationGateway $appgw
Add-AzApplicationGatewayHttpListener `
  -ApplicationGateway $appgw `
  -Name backendListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $backendPort
Set-AzApplicationGateway -ApplicationGateway $appgw
```

### <a name="add-url-path-map"></a>Добавление сопоставления URL-путей

Сопоставления URL-путей гарантируют, что определенные URL-адрес маршрутизируются в определенные внутренние пулы. Вы можете создавать карты URL-маршрутов под названием *imagePathRule* и *videoPathRule* с помощью [New-AzApplicationGatewayPathRuleConfig](/powershell/module/az.network/new-azapplicationgatewaypathruleconfig) и [Add-AzApplicationGatewayUrlPathMapConfig.](/powershell/module/az.network/add-azapplicationgatewayurlpathmapconfig)

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
$poolSettings = Get-AzApplicationGatewayBackendHttpSettings `
  -ApplicationGateway $appgw `
  -Name myPoolSettings
$imagePool = Get-AzApplicationGatewayBackendAddressPool `
  -ApplicationGateway $appgw `
  -Name imagesBackendPool
$videoPool = Get-AzApplicationGatewayBackendAddressPool `
  -ApplicationGateway $appgw `
  -Name videoBackendPool
$defaultPool = Get-AzApplicationGatewayBackendAddressPool `
  -ApplicationGateway $appgw `
  -Name appGatewayBackendPool
$imagePathRule = New-AzApplicationGatewayPathRuleConfig `
  -Name imagePathRule `
  -Paths "/images/*" `
  -BackendAddressPool $imagePool `
  -BackendHttpSettings $poolSettings
$videoPathRule = New-AzApplicationGatewayPathRuleConfig `
  -Name videoPathRule `
    -Paths "/video/*" `
    -BackendAddressPool $videoPool `
    -BackendHttpSettings $poolSettings
Add-AzApplicationGatewayUrlPathMapConfig `
  -ApplicationGateway $appgw `
  -Name urlpathmap `
  -PathRules $imagePathRule, $videoPathRule `
  -DefaultBackendAddressPool $defaultPool `
  -DefaultBackendHttpSettings $poolSettings
Set-AzApplicationGateway -ApplicationGateway $appgw
```

### <a name="add-routing-rule"></a>Добавление правила маршрутизации

Правило маршрутизации связывает сопоставление URL-адресов с созданным прослушивателем. Вы можете добавить правило под названием no*rule2* с помощью [Add-AzApplicationGatewayRequestRoutingRule.](/powershell/module/az.network/add-azapplicationgatewayrequestroutingrule)

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
$backendlistener = Get-AzApplicationGatewayHttpListener `
  -ApplicationGateway $appgw `
  -Name backendListener
$urlPathMap = Get-AzApplicationGatewayUrlPathMapConfig `
  -ApplicationGateway $appgw `
  -Name urlpathmap
Add-AzApplicationGatewayRequestRoutingRule `
  -ApplicationGateway $appgw `
  -Name rule2 `
  -RuleType PathBasedRouting `
  -HttpListener $backendlistener `
  -UrlPathMap $urlPathMap
Set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="create-virtual-machine-scale-sets"></a>Создание масштабируемых наборов виртуальных машин

В этом примере вы создадите три масштабируемых набора виртуальных машин, которые поддерживают три созданных внутренних пула. Имена создаваемых масштабируемых наборов — *myvmss1*, *myvmss2* и *myvmss3*. Каждый масштабируемый набор содержит два экземпляра виртуальной машины, на которых устанавливаются службы IIS. Масштабируемый набор назначается серверному пулу при настройке параметров IP-адреса.

```azurepowershell-interactive
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
$backendPool = Get-AzApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool `
  -ApplicationGateway $appgw
$imagesPool = Get-AzApplicationGatewayBackendAddressPool `
  -Name imagesBackendPool `
  -ApplicationGateway $appgw
$videoPool = Get-AzApplicationGatewayBackendAddressPool `
  -Name videoBackendPool `
  -ApplicationGateway $appgw
for ($i=1; $i -le 3; $i++)
{
  if ($i -eq 1)
  {
     $poolId = $backendPool.Id
  }
  if ($i -eq 2) 
  {
    $poolId = $imagesPool.Id
  }
  if ($i -eq 3)
  {
    $poolId = $videoPool.Id
  }
  $ipConfig = New-AzVmssIpConfig `
    -Name myVmssIPConfig$i `
    -SubnetId $vnet.Subnets[1].Id `
    -ApplicationGatewayBackendAddressPoolsId $poolId
  $vmssConfig = New-AzVmssConfig `
    -Location eastus `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic
  Set-AzVmssStorageProfile $vmssConfig `
    -ImageReferencePublisher MicrosoftWindowsServer `
    -ImageReferenceOffer WindowsServer `
    -ImageReferenceSku 2016-Datacenter `
    -ImageReferenceVersion latest
    -OsDiskCreateOption FromImage
  Set-AzVmssOsProfile $vmssConfig `
    -AdminUsername azureuser `
    -AdminPassword "Azure123456!" `
    -ComputerNamePrefix myvmss$i
  Add-AzVmssNetworkInterfaceConfiguration `
    -VirtualMachineScaleSet $vmssConfig `
    -Name myVmssNetConfig$i `
    -Primary $true `
    -IPConfiguration $ipConfig
  New-AzVmss `
    -ResourceGroupName myResourceGroupAG `
    -Name myvmss$i `
    -VirtualMachineScaleSet $vmssConfig
}
```

### <a name="install-iis"></a>Установка служб IIS

```azurepowershell-interactive
$publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }

for ($i=1; $i -le 3; $i++)
{
  $vmss = Get-AzVmss -ResourceGroupName myResourceGroupAG -VMScaleSetName myvmss$i
  Add-AzVmssExtension -VirtualMachineScaleSet $vmss `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings

  Update-AzVmss `
    -ResourceGroupName myResourceGroupAG `
    -Name myvmss$i `
    -VirtualMachineScaleSet $vmss
}
```

## <a name="test-the-application-gateway"></a>Тестирование шлюза приложений

Вы можете использовать командлет [Get-AzPublicIPAddress](/powershell/module/az.network/get-azpublicipaddress), чтобы получить общедоступный IP-адрес шлюза приложений. Скопируйте общедоступный IP-адрес и вставьте его в адресную строку браузера. Например, `http://52.168.55.24`, `http://52.168.55.24:8080/images/test.htm` или `http://52.168.55.24:8080/video/test.htm`.

```azurepowershell-interactive
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Тестирование базового URL-адреса в шлюзе приложений](./media/application-gateway-create-url-route-arm-ps/application-gateway-iistest.png)

Измените URL-адрес на, `http://<ip-address>:8080/video/test.htm`заменив `<ip-address>`свой IP-адрес на, и вы увидите что-то вроде следующего примера:

![Тестирование URL-адреса изображений в шлюзе приложений](./media/application-gateway-create-url-route-arm-ps/application-gateway-iistest-images.png)

Измените URL-адрес, `http://<ip-address>:8080/video/test.htm` и вы увидите что-то вроде следующего примера:

![Тестирование URL-адреса видео в шлюзе приложений](./media/application-gateway-create-url-route-arm-ps/application-gateway-iistest-video.png)

## <a name="next-steps"></a>Дальнейшие действия

Из этой статьи вы узнали, как выполнять следующие задачи:

> [!div class="checklist"]
> * Настройка сети
> * создание шлюза приложений с сопоставлением URL-адресов;
> * создание масштабируемых наборов виртуальных машин с внутренними пулами.

Чтобы узнать больше о шлюзах приложений и связанных с ними ресурсах, перейдите к статьям с инструкциями.
