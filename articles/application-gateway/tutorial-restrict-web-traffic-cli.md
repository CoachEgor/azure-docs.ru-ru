---
title: Включение брандмауэра веб-приложения — Azure CLI
description: Узнайте, как ограничить веб-трафик с помощью брандмауэра веб-приложения и Azure CLI.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 08/01/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 698191355ab9e014693b01cfb6546fb764a2b647
ms.sourcegitcommit: d585cdda2afcf729ed943cfd170b0b361e615fae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/31/2019
ms.locfileid: "68688195"
---
# <a name="enable-web-application-firewall-using-the-azure-cli"></a>Включение брандмауэра веб-приложения с помощью Azure CLI

Вы можете ограничить трафик в [шлюзе приложений](overview.md) с помощью [брандмауэра веб-приложения](waf-overview.md) (WAF). Для защиты приложения WAF использует правила [OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project). Эти правила включают защиту от атак, например от внедрения кода SQL, межсайтовых скриптов и захватов сеанса.

В этой статье раскрываются следующие темы:

> [!div class="checklist"]
> * Настройка сети
> * Создание шлюза приложений с включенным WAF.
> * создавать масштабируемый набор виртуальных машин;
> * Создание учетной записи хранения и настройка диагностики.

![Пример брандмауэра веб-приложений](./media/tutorial-restrict-web-traffic-cli/scenario-waf.png)

При необходимости эти инструкции можно выполнить с помощью [Azure PowerShell](tutorial-restrict-web-traffic-powershell.md).

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать интерфейс командной строки локально, для работы с этой статьей вам понадобится Azure CLI 2.0.4 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Создать группу ресурсов

Группа ресурсов — это логический контейнер, в котором происходит развертывание ресурсов Azure и управление ими. Создайте группу ресурсов Azure с именем *myResourceGroupAG*, используя команду [az group create](/cli/azure/group#az-group-create).

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Создание сетевых ресурсов

Виртуальная сеть и подсети используются для предоставления сетевого подключения к шлюзу приложений и его связанным ресурсам. Создайте виртуальную сеть с именем *myVNet* и подсеть с именем *myAGSubnet*. Затем создайте общедоступный IP-адрес с именем *myAGPublicIPAddress*.

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myBackendSubnet \
  --subnet-prefix 10.0.1.0/24

az network vnet subnet create \
  --name myAGSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --address-prefix 10.0.2.0/24

az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --allocation-method Static \
  --sku Standard
```

## <a name="create-an-application-gateway-with-a-waf"></a>Создание шлюза приложений с WAF

Выполните команду [az network application-gateway create](/cli/azure/network/application-gateway), чтобы создать шлюз приложений *myAppGateway*. При создании шлюза приложений с помощью Azure CLI укажите такие сведения о конфигурации, как емкость, номер SKU и параметры HTTP. Шлюз приложений назначается *myAGSubnet* и *myAGPublicIPAddress*.

```azurecli-interactive
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --subnet myAGSubnet \
  --capacity 2 \
  --sku WAF_v2 \
  --http-settings-cookie-based-affinity Disabled \
  --frontend-port 80 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress

az network application-gateway waf-config set \
  --enabled true \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --firewall-mode Detection \
  --rule-set-version 3.0
```

Создание шлюза приложений может занять несколько минут. Когда шлюз приложений будет создан, вы увидите такие новые функции шлюза:

- *appGatewayBackendPool* — шлюз приложений должен иметь по крайней мере один внутренний пул адресов.
- *appGatewayBackendHttpSettings* — указывает, что для обмена данными используются порт 80 и протокол HTTP.
- *appGatewayHttpListener* — прослушиватель по умолчанию, связанный с *appGatewayBackendPool*.
- *appGatewayFrontendIP* — назначает адрес *myAGPublicIPAddress* для прослушивателя *appGatewayHttpListener*.
- *rule1* — правило маршрутизации по умолчанию, связанное с прослушивателем *appGatewayHttpListener*.

## <a name="create-a-virtual-machine-scale-set"></a>создавать масштабируемый набор виртуальных машин;

В этом примере вы создаете масштабируемый набор виртуальных машин, чтобы предоставить два сервера для внутреннего пула в шлюзе приложений. Виртуальные машины в масштабируемом наборе связаны с подсетью *myBackendSubnet*. Чтобы создать масштабируемый набор, выполните команду [az vmss create](/cli/azure/vmss#az-vmss-create).

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
  --settings '{ "fileUris": ["https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/install_nginx.sh"],"commandToExecute": "./install_nginx.sh" }'
```

## <a name="create-a-storage-account-and-configure-diagnostics"></a>Создание учетной записи хранения и настройка диагностики.

В этой статье шлюз приложений использует учетную запись хранения, чтобы хранить данные для выявления и предотвращения угроз. Для записи данных можно также использовать журналы Azure Monitor или концентратор событий. 

### <a name="create-a-storage-account"></a>Создать учетную запись хранения

Создайте учетную запись хранения с именем *myagstore1* с помощью команды [az storage account create](/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create).

```azurecli-interactive
az storage account create \
  --name myagstore1 \
  --resource-group myResourceGroupAG \
  --location eastus \
  --sku Standard_LRS \
  --encryption blob
```

### <a name="configure-diagnostics"></a>Настройка диагностики

Настройте диагностику для записи данных в журналы ApplicationGatewayAccessLog, ApplicationGatewayPerformanceLog и ApplicationGatewayFirewallLog. Замените `<subscriptionId>` идентификатором подписки, а затем настройте диагностику с помощью команды [AZ Monitor Diagnostic-Settings Create](/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az-monitor-diagnostic-settings-create).

```azurecli-interactive
appgwid=$(az network application-gateway show --name myAppGateway --resource-group myResourceGroupAG --query id -o tsv)

storeid=$(az storage account show --name myagstore1 --resource-group myResourceGroupAG --query id -o tsv)

az monitor diagnostic-settings create --name appgwdiag --resource $appgwid \
  --logs '[ { "category": "ApplicationGatewayAccessLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "ApplicationGatewayPerformanceLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "ApplicationGatewayFirewallLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } } ]' \
  --storage-account $storeid
```

## <a name="test-the-application-gateway"></a>Тестирование шлюза приложений

Чтобы получить общедоступный IP-адрес шлюза приложений, используйте команду [az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show). Скопируйте общедоступный IP-адрес и вставьте его в адресную строку браузера.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Тестирование базового URL-адреса в шлюзе приложений](./media/tutorial-restrict-web-traffic-cli/application-gateway-nginxtest.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

При необходимости вы можете удалить группу ресурсов, шлюз приложений и все связанные ресурсы.

```azurecli-interactive
az group delete --name myResourceGroupAG 
```

## <a name="next-steps"></a>Следующие шаги

[Создание шлюза приложений с завершением SSL-запросов](./tutorial-ssl-cli.md)
