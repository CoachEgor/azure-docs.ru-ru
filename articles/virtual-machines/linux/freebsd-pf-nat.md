---
title: Используйте фильтр пакета FreeBSD для создания брандмауэра в Azure
description: Узнайте о том, как развернуть брандмауэр NAT с использованием PF FreeBSD в Azure.
author: KylieLiang
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/20/2017
ms.author: kyliel
ms.openlocfilehash: 9b78c0d93b57a3e3f4963088d0b93f121f57483c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "78945111"
---
# <a name="how-to-use-freebsds-packet-filter-to-create-a-secure-firewall-in-azure"></a>Создание защищенного брандмауэра в Azure с использованием PF FreeBSD
В этой статье рассматривается использование фильтра пакетов FreeBSD для развертывания брандмауэра для NAT с помощью шаблона Azure Resource Manager для общего сценария веб-сервера.

## <a name="what-is-pf"></a>Что такое PF?
PF — это лицензированный (по лицензии BSD) фильтр пакетов с отслеживанием состояния, а также основной элемент программного обеспечения для брандмауэра. Фильтр пакетов получил широкое распространение, так как он обладает рядом преимуществ по сравнению с другими доступными брандмауэрами. Возможность преобразования сетевых адресов (NAT) внедрена в PF с самого начала. Затем за счет интеграции платформы ALTQ и реализации возможности ее настройки в конфигурации в PF были интегрированы планировщик пакетов и активное управление очередями. В PF были добавлены такие функции, как pfsync и CARP — для отработки отказа и обеспечения избыточности, authpf — для аутентификации сеанса и ftp-proxy — чтобы упростить проверку FTP-протокола в брандмауэре. Короче говоря, PF — это брандмауэр с расширенными возможностями. 

## <a name="get-started"></a>Начало работы
Если вы хотите настроить безопасный брандмауэр в облаке для веб-серверов, тогда самое время начать. Чтобы настроить топологию сети, можно также применить сценарии, используемые в шаблоне Azure Resource Manager.
Шаблон Azure Resource Manager позволяет настроить виртуальную машину FreeBSD, выполняющую преобразование сетевых адресов (NAT) или перенаправление с помощью PF, а также две виртуальные машины FreeBSD с установленным и настроенным веб-сервером Nginx. Кроме преобразования сетевых адресов для исходящего трафика двух веб-серверов, виртуальная машина, выполняющая преобразование сетевых адресов или перенаправление, перехватывает HTTP-запросы и методом циклического перебора перенаправляет их к двум веб-серверам. Виртуальная сеть использует закрытое пространство IP-адресов 10.0.0.2/24 без поддержки маршрутизации. Параметры шаблона можно изменить. Шаблон Azure Resource Manager также определяет таблицу маршрутов для всей виртуальной сети, которая является набором отдельных маршрутов, используемым для переопределения маршрутов Azure по умолчанию на основе конечного IP-адреса. 

![pf_topology](./media/freebsd-pf-nat/pf_topology.jpg)
    
### <a name="deploy-through-azure-cli"></a>Развертывание с помощью Azure CLI
Нужно установить последнюю версию [ Azure CLI](/cli/azure/install-az-cli2) и войти в учетную запись Azure с помощью команды [az login](/cli/azure/reference-index). Создайте группу ресурсов с помощью команды [az group create](/cli/azure/group). В примере ниже создается имя группы ресурсов `myResourceGroup` в расположении `West US`.

```azurecli
az group create --name myResourceGroup --location westus
```

Далее разверните шаблон [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup), используя команду [az group deployment create](/cli/azure/group/deployment). Скачать [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/pf-freebsd-setup/azuredeploy.parameters.json) под тем же путем и `adminPassword`определить `networkPrefix`свои `domainNamePrefix`собственные значения ресурсов, такие как , и . 

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeploymentName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/pf-freebsd-setup/azuredeploy.json \
    --parameters '@azuredeploy.parameters.json' --verbose
```

По истечении пять минут отобразится сообщение `"provisioningState": "Succeeded"`. Затем вы можете подключиться к внешней виртуальной машине (NAT) по протоколу SSH или получить доступ к веб-серверу Nginx в браузере по общедоступному IP-адресу либо полному доменному имени виртуальной машины (NAT). В примере ниже указаны полное доменное имя и общедоступный IP-адрес, назначенные внешней виртуальной машине (NAT) в группе ресурсов `myResourceGroup`. 

```azurecli
az network public-ip list --resource-group myResourceGroup
```
    
## <a name="next-steps"></a>Дальнейшие действия
Если вы хотите настроить NAT в Azure, используя бесплатное и эффективное средство с открытым кодом, PF отлично подходит для этого. Шаблон [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup) позволит всего за пять минут настроить брандмауэр NAT с балансировкой нагрузки методом циклического перебора с помощью PF FreeBSD в Azure для общего сценария веб-сервера. 

Дополнительные сведения о предложениях FreeBSD в Azure см. в статье [Введение в FreeBSD в Azure](freebsd-intro-on-azure.md).

Дополнительные сведения о PF см. в [руководстве FreeBSD](https://www.freebsd.org/doc/handbook/firewalls-pf.html) или в [руководстве пользователя PF](https://www.freebsd.org/doc/handbook/firewalls-pf.html).
