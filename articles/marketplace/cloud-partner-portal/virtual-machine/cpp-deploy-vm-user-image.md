---
title: Развертывание VM Azure от пользователя VHD (ru) Лазурный рынок
description: В этой статье описывается развертывание пользовательского образа виртуального жесткого диска для создания экземпляра виртуальной машины Azure.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: dsindona
ms.openlocfilehash: 957bc187b0123338947f62333c913cf82a6c66a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "80277962"
---
# <a name="deploy-an-azure-vm-from-a-user-vhd"></a>Развертывание виртуальной машины Azure из виртуального жесткого диска пользователя

В этой статье описывается развертывание универсального образа виртуального жесткого диска для создания нового ресурса виртуальной машины Azure, используя указанный шаблон Azure Resource Manager и сценарий Azure PowerShell.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="vhd-deployment-template"></a>Шаблон развертывания VHD

Скопируйте шаблон Azure Resource Manager для [развертывания виртуального жесткого диска](cpp-deploy-json-template.md) в локальный файл с именем `VHDtoImage.json`.  Измените этот файл, чтобы задать значения для следующих параметров. 

|  **Параметр**             |   **Описание**                                                              |
|  -------------             |   ---------------                                                              |
| ResourceGroupName          | Имя существующей группы ресурсов Azure.  Обычно используется та же группа репликации, которая связана с хранилищем ключей  |
| TemplateFile               | Полный путь к файлу `VHDtoImage.json`                                    |
| userStorageAccountName     | Имя учетной записи хранения                                                    |
| sNameForPublicIP           | DNS-имя для общедоступного IP-адреса. Используются только строчные символы                                  |
| subscriptionId             | Идентификатор подписки Azure                                                  |
| Расположение                   | Стандартное географическое расположение Azure для группы ресурсов                       |
| vmName                     | Имя виртуальной машины                                                    |
| vaultName                  | Имя хранилища ключей.                                                          |
| vaultResourceGroup         | Группа ресурсов хранилища ключей.
| certificateUrl             | URL-адрес сертификата, включая версию, хранящуюся в хранилище ключей, например: `https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7` |
| vhdUrl                     | URL-адрес виртуального жесткого диска                                                   |
| vmSize                     | Размер экземпляра виртуальной машины                                           |
| publicIPAddressName        | Имя общедоступного IP-адреса                                                  |
| virtualNetworkName         | Имя виртуальной сети                                                    |
| nicName                    | Имя сетевого адаптера для виртуальной сети                     |
| adminUserName              | Имя пользователя учетной записи администратора                                          |
| adminPassword              | Пароль администратора                                                          |
|  |  |


## <a name="powershell-script"></a>Сценарий PowerShell

Скопируйте и измените приведенный ниже сценарий, чтобы задать значения переменных `$storageaccount` и `$vhdUrl`.  Выполните его, чтобы создать ресурс виртуальной машины Azure из существующего универсального диска VHD.

```powershell
# storage account of existing generalized VHD 
$storageaccount = "testwinrm11815"

# generalized VHD URL
$vhdUrl = "https://testwinrm11815.blob.core.windows.net/vhds/testvm1234562016651857.vhd"

echo "New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd"

#deploying VM with existing VHD
New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd 

```

## <a name="next-steps"></a>Дальнейшие действия

После развертывания виртуальной машины вы сможете [сертифицировать образ своей виртуальной машины](./cpp-certify-vm.md).
