---
title: Настройка частных IP-адресов для виртуальных машин (классическая модель) — CLI
titlesuffix: Azure Virtual Network
description: Узнайте, как настроить частные IP-адреса для классических виртуальных машин с помощью классического интерфейса командной строки Azure.
services: virtual-network
documentationcenter: na
author: genlin
manager: dcscontentpm
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: genli
ms.openlocfilehash: 9deaf4b1d80ad4e55e7c971998e8b1f5ea562257
ms.sourcegitcommit: 8e31a82c6da2ee8dafa58ea58ca4a7dd3ceb6132
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/19/2019
ms.locfileid: "74196578"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-classic-cli"></a>Настройка частных IP-адресов для классической виртуальной машины с помощью классического Azure CLI

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

В этой статье рассматривается классическая модель развертывания. Кроме того, вы можете [управлять статическим частным IP-адресом в модели развертывания для диспетчера ресурсов](virtual-networks-static-private-ip-arm-cli.md).

Для выполнения приведенных ниже примеров команд классического Azure CLI требуется созданная простая среда. Чтобы выполнять команды в соответствии с указаниями, представленными в этом документе, сначала постройте тестовую среду, как описано в статье [Создание виртуальной сети](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Как указать статический частный IP-адрес при создании виртуальной машины
Чтобы создать новую виртуальную машину *DNS01* в новой облачной службе *TestService* согласно приведенному ранее сценарию, выполните следующее:

1. Если вы еще не пользовались Azure CLI, ознакомьтесь со статьей [Установка и настройка CLI Azure](/cli/azure/install-cli-version-1.0) и следуйте инструкциям вплоть до выбора учетной записи Azure и подписки.
2. Выполните команду **azure service create** для создания облачной службы.
   
        azure service create TestService --location uscentral
   
    Ожидаемые выходные данные:
   
        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
3. Выполните команду **azure create vm** для создания виртуальной машины. Обратите внимание на значение статического частного IP-адреса. В списке, который откроется после выполнения команды, будут указаны используемые параметры.
   
        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd
   
    Ожидаемые выходные данные:
   
        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting to "Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK
   
   * **-l (или --location)** . Регион Azure для создания виртуальной машины. В данном случае это — *centralus*.
   * **-n (или --vm-name)** . Имя создаваемой виртуальной машины.
   * **-w (или --virtual-network-name)** . Имя виртуальной сети, в которой будет создана виртуальная машина. 
   * **-S (или --static-ip)** . Статический частный IP-адрес для виртуальной машины.
   * **TestService**. Имя облачной службы, в которой будет создана виртуальная машина.
   * **bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2.** Образ, используемый для создания виртуальной машины.
   * **adminuser**. Локальный администратор виртуальной машины Windows.
   * <strong>AdminP@ssw0rd</strong>. Пароль локального администратора виртуальной машины Windows.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Получение сведений о статическом частном IP-адресе виртуальной машины
Чтобы просмотреть информацию о статическом частном IP-адресе виртуальной машины, созданной с помощью приведенного выше сценария, выполните следующую команду Azure CLI и обратите внимание на значение *Network StaticIP*:

    azure vm static-ip show DNS01

Ожидаемые выходные данные:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Как удалить статический частный IP-адрес виртуальной машины
Чтобы удалить статический частный IP-адрес, добавленный на виртуальную машину в приведенном выше сценарии, выполните следующую команду Azure CLI:

    azure vm static-ip remove DNS01

Ожидаемые выходные данные:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-to-add-a-static-private-ip-to-an-existing-vm"></a>Как добавить статический частный IP-адрес для существующей виртуальной машины
Чтобы добавить статический частный IP-адрес для виртуальной машины, созданной с помощью приведенного ранее скрипта, выполните такую команду:

    azure vm static-ip set DNS01 192.168.1.101

Ожидаемые выходные данные:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="set-ip-addresses-within-the-operating-system"></a>Настройка IP-адресов в операционной системе

Не рекомендуется без необходимости статически назначать виртуальной машине Azure частный IP-адрес в ее операционной системе. Если вы будете вручную устанавливать частный IP-адрес в операционной системе, убедитесь, что он соответствует частному IP-адресу, назначенному виртуальной машине Azure, иначе соединение с виртуальной машиной может быть потеряно. Не следует вручную назначать общедоступный IP-адрес для виртуальной машины Azure в ее операционной системе.

## <a name="next-steps"></a>Дополнительная информация
* Ознакомьтесь с информацией о [зарезервированных общедоступных IP-адресах](virtual-networks-reserved-public-ip.md) .
* Узнайте об [общедоступных IP-адресах уровня экземпляра (ILPIP)](virtual-networks-instance-level-public-ip.md) .
* Ознакомьтесь с информацией о [REST API зарезервированных IP-адресов](https://msdn.microsoft.com/library/azure/dn722420.aspx).