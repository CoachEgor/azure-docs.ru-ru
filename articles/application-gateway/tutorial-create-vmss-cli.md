---
title: Создание шлюза приложений с масштабируемым набором виртуальных машин — Azure CLI | Документация Майкрософт
description: Узнайте, как создать шлюз приложений с масштабируемым набором виртуальных машин с помощью Azure CLI.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 7/14/2018
ms.author: victorh
ms.openlocfilehash: 792d6da36851f74429d97a9779aff1727e8f64db
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66133654"
---
# <a name="create-an-application-gateway-with-a-virtual-machine-scale-set-using-the-azure-cli"></a>Создание шлюза приложений с масштабируемым набором виртуальных машин с помощью Azure CLI

С помощью Azure CLI вы можете создать [шлюз приложений](application-gateway-introduction.md), использующий [масштабируемый набор виртуальных машин](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) для внутренних серверов. В этом примере масштабируемый набор содержит два экземпляра виртуальных машин, которые добавляются в серверный пул шлюза приложений по умолчанию.

В этой статье раскрываются следующие темы:

> [!div class="checklist"]
> * Настройка сети
> * Создание шлюза приложений
> * создание масштабируемого набора виртуальных машин с серверным пулом, используемым по умолчанию.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать CLI локально, для выполнения инструкций в этом руководстве вам понадобится Azure CLI 2.0.4 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Группа ресурсов — это логический контейнер, в котором происходит развертывание ресурсов Azure и управление ими. Создайте группу ресурсов, используя команду [az group create](/cli/azure/group#az-group-create). 

В следующем примере создается группа ресурсов с именем *myResourceGroupAG* в расположении *eastus*.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Создание сетевых ресурсов 

Создайте виртуальную сеть с именем *myVNet* и подсеть *myAGSubnet* с помощью команды [az network vnet create](/cli/azure/network/vnet). Затем добавьте подсеть с именем *myBackendSubnet*, необходимую для внутренних серверов, используя команду [az network vnet subnet create](/cli/azure/network/vnet/subnet). Создайте общедоступный IP-адрес с именем *myAGPublicIPAddress*, используя команду [az network public-ip create](/cli/azure/network/public-ip).

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
az network vnet subnet create \
  --name myBackendSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --address-prefix 10.0.2.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway"></a>Создание шлюза приложений

Выполните команду [az network application-gateway create](/cli/azure/network/application-gateway), чтобы создать шлюз приложений *myAppGateway*. При создании шлюза приложений с помощью Azure CLI укажите такие сведения о конфигурации, как емкость, номер SKU и параметры HTTP. Шлюз приложений назначается подсети *myAGSubnet* и адресу *myPublicIPAddressddress*, созданным ранее. 

```azurecli-interactive
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --subnet myAGsubnet \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Disabled \
  --frontend-port 80 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress
```

 Создание шлюза приложений может занять несколько минут. Когда шлюз приложений будет создан, вы увидите такие новые функции шлюза:

- *appGatewayBackendPool* — шлюз приложений должен иметь по крайней мере один внутренний пул адресов.
- *appGatewayBackendHttpSettings* — указывает, что для обмена данными используются порт 80 и протокол HTTP.
- *appGatewayHttpListener* — прослушиватель по умолчанию, связанный с *appGatewayBackendPool*.
- *appGatewayFrontendIP* — назначает адрес *myAGPublicIPAddress* для прослушивателя *appGatewayHttpListener*.
- *rule1* — правило маршрутизации по умолчанию, связанное с прослушивателем *appGatewayHttpListener*.

## <a name="create-a-virtual-machine-scale-set"></a>создавать масштабируемый набор виртуальных машин;

В этом примере вы создаете масштабируемый набор виртуальных машин, чтобы предоставить серверы для внутреннего пула в шлюзе приложений. Виртуальные машины в масштабируемом наборе связаны с подсетью *myBackendSubnet* и пулом *appGatewayBackendPool*. Чтобы создать масштабируемый набор, выполните команду [az vmss create](/cli/azure/vmss#az-vmss-create).

```azurecli-interactive
az vmss create \
  --name myvmss \
  --resource-group myResourceGroupAG \
  --image UbuntuLTS \
  --admin-username azureuser \
  --admin-password Azure123456! \
  --instance-count 2 \
  --vnet-name myVNet \
  --subnet myBackendSubnet \
  --vm-sku Standard_DS2 \
  --upgrade-policy-mode Automatic \
  --app-gateway myAppGateway \
  --backend-pool-name appGatewayBackendPool
```

### <a name="install-nginx"></a>Установка nginx

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroupAG \
  --vmss-name myvmss \
  --settings '{ "fileUris": ["https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/install_nginx.sh"], "commandToExecute": "./install_nginx.sh" }'
```

## <a name="test-the-application-gateway"></a>Тестирование шлюза приложений

Чтобы получить общедоступный IP-адрес шлюза приложений, используйте команду [az network public-ip show](/cli/azure/network/public-ip). Скопируйте общедоступный IP-адрес и вставьте его в адресную строку браузера.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Тестирование базового URL-адреса в шлюзе приложений](./media/tutorial-create-vmss-cli/tutorial-nginxtest.png)

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы узнали, как:

> [!div class="checklist"]
> * Настройка сети
> * Создание шлюза приложений
> * создание масштабируемого набора виртуальных машин с серверным пулом, используемым по умолчанию.

Чтобы узнать больше о шлюзах приложений и связанных с ними ресурсах, перейдите к статьям с инструкциями.
