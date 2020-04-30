---
title: Развертывание виртуальной машины из Azure Marketplace
description: В этой статье объясняется, как развернуть виртуальную машину из предварительно настроенной виртуальной машины Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: dsindona
ms.openlocfilehash: 2fb04e2e953e2bcbe479f7685b8042fdc7ea1767
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82146988"
---
# <a name="deploy-a-virtual-machine-from-the-azure-marketplace"></a>Развертывание виртуальной машины из Azure Marketplace

> [!IMPORTANT]
> Начиная с 13 апреля 2020 г. Мы начинаем перемещать Управление предложениями виртуальных машин Azure в центр партнеров. После миграции вы создадите предложения в центре партнеров и будете управлять ими. Чтобы управлять перенесенными предложениями, следуйте инструкциям в статье [сертификация образа виртуальной машины Azure](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-image-certification) .

В этой статье объясняется, как развернуть предварительно настроенную виртуальную машину из Azure Marketplace, используя скрипт Azure PowerShell.  Этот скрипт также предоставляет на виртуальной машине конечные точки WinRM HTTP и HTTPS.  Для работы скрипта необходимо заранее передать сертификат в Azure Key Vault, как описано в статье [Создание сертификатов для Azure Key Vault](./cpp-create-key-vault-cert.md). 

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="vm-deployment-template"></a>Шаблон развертывания виртуальной машины

Шаблон для быстрого развертывания виртуальной машины Azure доступен в виде файла [azuredeploy.json](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json).  Он содержит следующие параметры:

|  **Параметр**        |   **Описание**                                 |
|  -------------        |   ---------------                                 |
| newStorageAccountName    | Имя учетной записи хранения                       |
| dnsNameForPublicIP    | DNS-имя общедоступного IP-адреса. Используются только строчные символы.    |
| adminUserName            | Имя пользователя администратора.                          |
| adminPassword            | Пароль администратора.                          |
| imagePublisher        | Издатель образа.                                   |
| imageOffer            | Предложение образа.                                       |
| imageSKU                | Номер SKU образа.                                         |
| vmSize                | Размер виртуальной машины.                                    |
| vmName                | Имя виртуальной машины.                                    |
| vaultName                | Имя хранилища ключей.                             |
| vaultResourceGroup    | Группа ресурсов хранилища ключей.                   |
| certificateUrl        | URL-адрес сертификата, включая версию в хранилище ключей, например `https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7`. |
|  |  |


## <a name="deployment-script"></a>Скрипт развертывания

Измените приведенный ниже скрипт Azure PowerShell и выполните его, чтобы развернуть указанную виртуальную машину из Azure Marketplace.

```powershell

New-AzResourceGroupDeployment -Name "dplvm$postfix" -ResourceGroupName "$rgName" -TemplateUri "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json" -newStorageAccountName "test$postfix" -dnsNameForPublicIP $vmName -adminUserName "isv" -adminPassword $pwd -vmSize "Standard_A2" -vmName $vmName -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id 

```


## <a name="next-steps"></a>Дальнейшие шаги

Развернув предварительно настроенную виртуальную машину, вы можете настроить включенные в нее решения и службы или использовать ее для дальнейшей разработки. 
