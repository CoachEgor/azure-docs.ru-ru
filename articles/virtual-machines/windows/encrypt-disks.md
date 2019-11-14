---
title: Шифрование дисков на виртуальной машине Windows в Azure
description: Шифрование виртуальных дисков на виртуальной машине Windows для улучшения уровня безопасности с помощью Azure PowerShell
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/30/2018
ms.author: cynthn
ms.openlocfilehash: 096fe779c077424b01df3ead5965cb799866a03b
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2019
ms.locfileid: "74033527"
---
# <a name="encrypt-virtual-disks-on-a-windows-vm"></a>Шифрование виртуальных дисков на виртуальной машине Windows
Для улучшения уровня безопасности и соответствия требованиям виртуальной машины содержание виртуальных дисков в Azure можно зашифровать. Диски, зашифрованные с использованием криптографических ключей, защищенных в Azure Key Vault. Вы будете управлять этими криптографическими ключами и проводить аудит их использования. В этой статье описывается, как с помощью Azure PowerShell шифровать диски на виртуальной машине Windows. Вы также можете [шифровать виртуальные машины Linux](../linux/disk-encryption-overview.md).

 

## <a name="overview-of-disk-encryption"></a>Общие сведения о шифровании дисков
Виртуальные диски на виртуальных машинах Windows шифруются в состоянии покоя с помощью Bitlocker. В Azure плата не взимается за шифрование виртуальных дисков. Криптографические ключи хранятся в Azure Key Vault с применением защиты программного обеспечения, или можно импортировать или создать ключи аппаратных модулей безопасности (HSM), сертифицированных по стандартам уровня 2 FIPS 140-2. Для шифрования и расшифровки виртуальных дисков, подключенных к виртуальной машине, используются эти криптографические ключи. Вы сохраняете контроль над этими криптографическими ключами и можете проводить аудит их использования. 

Процесс шифрования виртуальной машины выполняется следующим образом.

1. Создайте криптографический ключ в хранилище ключей Azure.
1. Настройте криптографический ключ таким образом, чтобы его можно было использовать для шифрования дисков.
1. Включите шифрование для виртуальных дисков.
1. Необходимые ключи шифрования запрашиваются из Azure Key Vault.
1. Виртуальные диски зашифровываются с использованием предоставленного криптографического ключа.


## <a name="requirements-and-limitations"></a>Требования и ограничения

Поддерживаемые сценарии и требования для шифрования дисков:

* включение шифрования для новых виртуальных машин Windows из образов Azure Marketplace или пользовательского образа виртуального жесткого диска;
* включение шифрования для имеющихся виртуальных машин в Azure;
* включение шифрования для виртуальных машин Windows, использующих дисковые пространства;
* отключение шифрования дисков данных и дисков операционной системы виртуальных машин Windows;
* виртуальные машины уровня "Стандартный", такие как серии A, D, DS, G и GS.

    > [!NOTE]
    > Все ресурсы (такие как хранилище ключей, учетная запись хранения и виртуальная машина) должны относиться к одному региону и одной подписке Azure.

В следующих сценариях шифрование дисков сейчас не поддерживается:

* виртуальные машины уровня "Базовый";
* виртуальные машины, созданные с использованием классической модели развертывания;
* обновление криптографических ключей на уже зашифрованных виртуальных машинах;
* интеграция с локальной службой управления ключами.


## <a name="create-an-azure-key-vault-and-keys"></a>Создание Azure Key Vault и ключей
Перед началом работы убедитесь, что установлена последняя версия модуля Azure PowerShell. Дополнительные сведения см. в статье [Установка и настройка Azure PowerShell](/powershell/azure/overview). В следующих примерах команд замените все параметры примеров собственными именами, расположением и значением ключей, например, *myResourceGroup*, *myKeyVault*, *myVM*, и т. д.

Первым делом создайте хранилище ключей Azure для хранения криптографических ключей. Хранилища ключей Azure могут хранить ключи, секреты или пароли, что позволяет безопасно реализовать их в приложениях и службах. Для шифрования виртуальных дисков создайте Key Vault, чтобы хранить криптографический ключ, используемый для шифрования или расшифровки виртуальных дисков. 

Включите поставщик Azure Key Vault в подписке Azure, выполнив командлет [Register-AzResourceProvider](https://docs.microsoft.com/powershell/module/az.resources/register-azresourceprovider), и создайте группу ресурсов с помощью [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup). В следующем примере создается имя группы ресурсов *myResourceGroup* в расположении *East US*:

```azurepowershell-interactive
$rgName = "myResourceGroup"
$location = "East US"

Register-AzResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzResourceGroup -Location $location -Name $rgName
```

Azure Key Vault, содержащее криптографические ключи и связанные вычислительные ресурсы, такие как хранилище и виртуальная машина, должны находиться в одном и том же регионе. Создайте Azure Key Vault с помощью [New-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/new-azkeyvault) и включите Key Vault для использования при шифровании дисков. Укажите уникальное имя Key Vault для параметра *keyVaultName*, выполнив следующую команду:

```azurepowershell-interactive
$keyVaultName = "myKeyVault$(Get-Random)"
New-AzKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

С помощью программного обеспечения или защиты HSM можно хранить криптографические ключи.  Стандартный Key Vault хранит только защищенные ключи. Использование HSM за дополнительную плату требует Key Vault уровня "Премиум". Чтобы создать Key Vault уровня "Премиум", на предыдущем шаге добавьте параметр *-Sku "Premium"* . В следующем примере используются ключи, защищенные программным обеспечением, так как мы создали хранилище ключей уровня "Стандартный". 

Для обеих моделей защиты платформе Azure необходимо предоставить доступ на запрос криптографических ключей при загрузке виртуальной машины для расшифровки виртуальных дисков. Создайте криптографический ключ в своем Key Vault, выполнив команду [Add-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/add-azkeyvaultkey). В следующем примере создается ключ с именем *myKey*:

```azurepowershell-interactive
Add-AzKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```

## <a name="create-a-virtual-machine"></a>Создание виртуальной машины
Чтобы проверить процесс шифрования, создайте виртуальную машину с помощью командлета [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm). В следующем примере создается виртуальная машина *myVM* с использованием образа *Windows Server 2016 Datacenter*. Когда появится запрос на ввод учетных данных, введите имя пользователя и пароль, которые будут использоваться для виртуальной машины.

```azurepowershell-interactive
$cred = Get-Credential

New-AzVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred
```


## <a name="encrypt-a-virtual-machine"></a>Шифрование виртуальной машины
Зашифруйте виртуальную машину, выполнив командлет [Set-AzVMDiskEncryptionExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdiskencryptionextension) и используя ключ Azure Key Vault. В следующем примере извлекаются все сведения о ключе, а затем шифруется виртуальная машина *myVM*:

```azurepowershell-interactive
$keyVault = Get-AzKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $keyVaultName -Name myKey).Key.kid;

Set-AzVMDiskEncryptionExtension -ResourceGroupName $rgName `
    -VMName "myVM" `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId
```

Примите приглашение, чтобы продолжить шифрование виртуальной машины. Во время этого процесса виртуальная машина перезагружается. После завершения процесса шифрования и перезагрузки виртуальной машины просмотрите состояние шифрования, выполнив командлет [Get-AzVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/az.compute/get-azvmdiskencryptionstatus):

```azurepowershell-interactive
Get-AzVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName "myVM"
```

Вы должны увидеть результат, аналогичный приведенному ниже.

```azurepowershell-interactive
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a>Дополнительная информация
* Дополнительные сведения об управлении Azure Key Vault см. в статье [Настройка хранилища ключей для виртуальных машин в Azure Resource Manager](key-vault-setup.md).
* Дополнительные сведения о шифровании дисков, а именно о подготовке зашифрованной настраиваемой виртуальной машины к передаче в Azure, см. в статье [Дисковое шифрование Azure для виртуальных машин IaaS под управлением Windows и Linux](../../security/fundamentals/encryption-overview.md).
