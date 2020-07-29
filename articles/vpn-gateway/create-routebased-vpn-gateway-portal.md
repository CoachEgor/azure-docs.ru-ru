---
title: Создание VPN-шлюза на основе маршрутов. портал
titleSuffix: Azure VPN Gateway
description: Создание VPN-шлюза на основе маршрутов с помощью портала Azure
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 03/03/2020
ms.author: cherylmc
ms.openlocfilehash: 4409df576c23e0dbc3e663cc348b45bc23ad5460
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "84987705"
---
# <a name="create-a-route-based-vpn-gateway-using-the-azure-portal"></a>Создание VPN-шлюза на основе маршрутов с помощью портала Azure

Эта статья поможет быстро создать VPN-шлюз Azure на основе маршрутов, используя портал Azure.  VPN-шлюз используется при создании VPN-подключения к локальной сети. Также вы можете использовать VPN-шлюз для подключения виртуальных сетей. 

Инструкции в этой статье позволяют создать виртуальную сеть, подсеть, подсеть шлюза и VPN-шлюз на основе маршрутов (шлюз виртуальной сети). Создав шлюз, можно создавать подключения. Для этих действий требуется подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="create-a-virtual-network"></a><a name="vnet"></a>Создание виртуальной сети

[!INCLUDE [Basic Point-to-Site VNet](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="configure-and-create-the-gateway"></a><a name="gwvalues"></a>Настройка и создание шлюза

На этом шаге вы создадите шлюз для своей виртуальной сети. Создание шлюза часто занимает 45 минут и более, в зависимости от выбранного SKU шлюза.

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-portal-include.md)]

[!INCLUDE [Create a gateway](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

>[!NOTE]
>Базовый номер SKU шлюза не поддерживает проверку подлинности IKEv2 или RADIUS. Если вы планируете подключение клиентов Mac к виртуальной сети, не используйте SKU "базовый".

[!INCLUDE [NSG warning](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="view-the-vpn-gateway"></a><a name="viewgw"></a>Просмотр VPN-шлюза

1. После создания шлюза перейдите к сети VNet1 с помощью портала. На странице "Обзор" VPN-шлюз отобразится как подключенное устройство.

   ![Подключенные устройства](./media/create-routebased-vpn-gateway-portal/view-connected-devices.png "Подключенные устройства")

2. В списке устройств щелкните **VNet1GW**, чтобы получить дополнительные сведения.

   ![Просмотр VPN-шлюза](./media/create-routebased-vpn-gateway-portal/view-gateway.png "Просмотр VPN-шлюза")

## <a name="next-steps"></a>Дальнейшие шаги

Создав шлюз, можно создать подключение между вашей и другой виртуальной сетью. Также можно создать подключение между виртуальной сетью и локальным расположением.

> [!div class="nextstepaction"]
> [Создание подключения "сеть — сеть"](vpn-gateway-howto-site-to-site-resource-manager-portal.md)<br><br>
> [Создание подключения "точка — сеть"](vpn-gateway-howto-point-to-site-resource-manager-portal.md)<br><br>
> [Создание подключения к другой виртуальной сети](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
