---
title: Создание группы безопасности сети (классической) с помощью классической инфраструктуры CLI Azure
description: Узнайте, как создавать и развертывать группы безопасности сети (классические) с помощью классического интерфейса командной строки Azure CLI.
services: virtual-network
documentationcenter: na
author: genlin
manager: dcscontentpm
editor: ''
tags: azure-service-management
ms.assetid: 17d98950-5fbb-4653-bef6-d822ab37541e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: genli
ms.openlocfilehash: c17e75849d1127c9c1001fd87af64e3ffe0eba14
ms.sourcegitcommit: 8e31a82c6da2ee8dafa58ea58ca4a7dd3ceb6132
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/19/2019
ms.locfileid: "74196714"
---
# <a name="create-a-network-security-group-classic-using-the-azure-classic-cli"></a>Создание группы безопасности сети (классической) с помощью классического интерфейса командной строки Azure CLI
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

В этой статье рассматривается классическая модель развертывания. Вы также можете [создавать группы безопасности сети с использованием модели развертывания на основе диспетчера ресурсов](tutorial-filter-network-traffic-cli.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Для приведенных ниже примеров команд Azure CLI требуется уже созданная простая среда, основанная на приведенном сценарии. Чтобы выполнять команды в соответствии с инструкциями, представленными в этом документе, сначала создайте тестовую среду, [создав виртуальную сеть](virtual-networks-create-vnet-classic-cli.md).

## <a name="create-an-nsg-for-the-front-end-subnet"></a>Создание NSG для интерфейсной подсети

1. Если вы никогда не использовали Azure CLI, выполните действия по [установке и настройке Azure CLI](/cli/azure/install-cli-version-1.0).
2. Переключитесь в классический режим.

    ```azurecli
    azure config mode asm
    ```   

3. Создайте NSG.
   
    ```azurecli   
     azure network nsg create -l uswest -n NSG-FrontEnd
    ```
   
4. Создайте правило, которое разрешает доступ к порту 3389 (RDP) из Интернета.
   
    ```azurecli
    azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   ```

5. Создайте правило, которое разрешает доступ к порту 80 (HTTP) из Интернета.
   
    ```azurecli
    azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
    ```   

6. Свяжите NSG с интерфейсной подсетью.
   
    ```azurecli
    azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd
   ```

## <a name="create-the-nsg-for-the-back-end-subnet"></a>Создание NSG для внутренней подсети

1. Создайте группу безопасности сети.
   
    ```azurecli
    azure network nsg create -l uswest -n NSG-BackEnd
   ```

2. Создайте правило, которое разрешает доступ к порту 1433 (SQL) из интерфейсной подсети.
   
    ```azurecli
    azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   ```

3. Создайте правило, которое запрещает доступ к Интернету.
   
    ```azurecli
    azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80
   ```

4. Свяжите NSG с внутренней подсетью.
   
    ```azurecli
    azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd
    ```