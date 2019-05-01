---
title: Шифрование дисков для масштабируемых наборов Azure с помощью Azure CLI | Документы Майкрософт
description: Узнайте, как использовать Azure CLI для шифрования экземпляров виртуальной машины и подключенные к ним диски в масштабируемом наборе виртуальных машин Linux.
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2019
ms.author: cynthn
ms.openlocfilehash: 1264c7e4ebaf5e948e624fa49dc5fb0b4cdb31f0
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2019
ms.locfileid: "64869057"
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-the-azure-cli"></a>Шифрование диска ОС и подключенных дисков данных в масштабируемый набор виртуальных машин с помощью Azure CLI

Масштабируемые наборы виртуальных машин поддерживают шифрование дисков Azure, позволяя защитить неактивные данные с помощью стандартной отраслевой технологии шифрования. Шифрование можно включить для масштабируемых наборов виртуальных машин Windows и Linux. Дополнительные сведения см. в статье [Шифрование дисков Azure для Linux и Windows](../security/azure-security-disk-encryption.md).

Шифрование дисков Azure поддерживается:
- для масштабируемых наборов, созданных с помощью управляемых дисков, и не поддерживается для масштабируемых наборов на основе собственных (или неуправляемых) дисков;
- для дисков ОС и томов данных в масштабируемых наборах Windows; отключение шифрования поддерживается для томов операционной системы и данных в масштабируемых наборах Windows;
- для томов данных в масштабируемых наборах Linux. Шифрование диска операционной системы не поддерживается для масштабируемых наборов Linux.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Чтобы установить и использовать интерфейс командной строки локально, для работы с этим руководством вам понадобится Azure CLI 2.0.31 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-scale-set"></a>Создание масштабируемого набора

Прежде чем создать масштабируемый набор, выполните команду [az group create](/cli/azure/group) для создания группы ресурсов. В следующем примере создается группа ресурсов с именем *myResourceGroup* в расположении *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Создайте масштабируемый набор виртуальных машин с помощью команды [az vmss create](/cli/azure/vmss). В следующем примере создается масштабируемый набор с именем *myScaleSet*, который настроен на автоматическое обновление при применении изменений. Также создаются ключи SSH, если они не существуют в *~/.ssh/id_rsa*. К каждому экземпляру виртуальной машины подключается диск данных объемом 32 ГБ. Диски данных подготавливаются с помощью [расширения пользовательского скрипта](../virtual-machines/linux/extensions-customscript.md) Azure. Для этого выполняется команда [az vmss extension set](/cli/azure/vmss/extension):

```azurecli-interactive
# Create a scale set with attached data disk
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --generate-ssh-keys \
  --data-disk-sizes-gb 32

# Prepare the data disk for use with the Custom Script Extension
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name myScaleSet \
  --settings '{"fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh"],"commandToExecute":"./prepare_vm_disks.sh"}'
```

Создание и настройка всех ресурсов и виртуальных машин масштабируемого набора занимает несколько минут.

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Создание хранилища ключей Azure с поддержкой шифрования дисков

В хранилище ключей Azure можно хранить ключи, секреты или пароли, что позволяет безопасно реализовать их в приложениях и службах. Криптографические ключи хранятся в хранилище ключей Azure с применением защиты программного обеспечения. В качестве альтернативы можно импортировать или создать ключи аппаратных модулей безопасности (HSM), сертифицированных по стандартам уровня 2 FIPS 140-2. Эти криптографические ключи используются для шифрования и расшифровки виртуальных дисков, подключенных к виртуальной машине. Вы сохраняете контроль над этими криптографическими ключами и можете проводить аудит их использования.

Определите уникальное значение *keyvault_name*. Затем создайте хранилище ключей с помощью команды [az keyvault create](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-create) в тех же подписке и регионе, что и масштабируемый набор, и настройте политику доступа *--enabled-for-disk-encryption*.

```azurecli-interactive
# Provide your own unique Key Vault name
keyvault_name=myuniquekeyvaultname

# Create Key Vault
az keyvault create --resource-group myResourceGroup --name $keyvault_name --enabled-for-disk-encryption
```

### <a name="use-an-existing-key-vault"></a>Использование существующего Key Vault

Этот шаг выполняется только в том случае, если у вас уже есть хранилище ключей и вы хотите применить его для шифрования дисков. Пропустите этот шаг, если в предыдущем разделе вы создали новый Key Vault.

Определите уникальное значение *keyvault_name*. Затем обновите хранилище ключей с помощью команды [az keyvault update](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-update) и настройте политику доступа *--enabled-for-disk-encryption*.

```azurecli-interactive
# Provide your own unique Key Vault name
keyvault_name=myuniquekeyvaultname

# Create Key Vault
az keyvault update --name $keyvault_name --enabled-for-disk-encryption
```

## <a name="enable-encryption"></a>Включение шифрования

Чтобы зашифровать экземпляры виртуальных машин в масштабируемом наборе, сначала нужно получить некоторые сведения об идентификаторе ресурса для Key Vault с помощью команды [az keyvault show](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-show). Эти переменные применяются при запуске процесса шифрования с помощью команды [az vmss encryption enable](/cli/azure/vmss/encryption#az-vmss-encryption-enable):

```azurecli-interactive
# Get the resource ID of the Key Vault
vaultResourceId=$(az keyvault show --resource-group myResourceGroup --name $keyvault_name --query id -o tsv)

# Enable encryption of the data disks in a scale set
az vmss encryption enable \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --disk-encryption-keyvault $vaultResourceId \
    --volume-type DATA
```

На запуск процесса шифрования могут потребоваться одна-две минуты.

Так как для набора масштабирования, созданного ранее, задана политика *автоматического* обновления, процесс шифрования для экземпляров виртуальных машин запускается автоматически. Если для масштабируемого набора задана политика обновления вручную, запустите шифрование для экземпляров виртуальных машин с помощью команды [az vmss update-instances](/cli/azure/vmss#az-vmss-update-instances).

### <a name="enable-encryption-using-kek-to-wrap-the-key"></a>Включить шифрование с помощью ключа шифрования КЛЮЧЕЙ ключ

Также можно использовать ключ шифрования ключа для повышения безопасности при шифровании масштабируемый набор виртуальных машин.

```azurecli-interactive
# Get the resource ID of the Key Vault
vaultResourceId=$(az keyvault show --resource-group myResourceGroup --name $keyvault_name --query id -o tsv)

# Enable encryption of the data disks in a scale set
az vmss encryption enable \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --disk-encryption-keyvault $vaultResourceId \
    --key-encryption-key myKEK \
    --key-encryption-keyvault $vaultResourceId \
    --volume-type DATA
```

> [!NOTE]
>  Полный идентификатор строки используется следующий синтаксис для значения параметра дискового шифрования хранилище ключей:</br>
/ subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br></br>
> Полный URI для ключа шифрования КЛЮЧЕЙ, как показано на используется следующий синтаксис для значения параметра ключа шифрования ключа:</br>
https://[keyvault-name].Vault.Azure.NET/Keys/[kekname]/[kek-UNIQUE-ID]

## <a name="check-encryption-progress"></a>Проверка хода выполнения шифрования

Чтобы проверить состояние шифрования диска, используйте команду [az vmss encryption show](/cli/azure/vmss/encryption#az-vmss-encryption-show):

```azurecli-interactive
az vmss encryption show --resource-group myResourceGroup --name myScaleSet
```

Если экземпляры виртуальных машин зашифрованы, код состояния возвращает *EncryptionState/encrypted*, как показано в следующем примере выходных данных:

```bash
[
  {
    "disks": [
      {
        "encryptionSettings": null,
        "name": "myScaleSet_myScaleSet_0_disk2_3f39c2019b174218b98b3dfae3424e69",
        "statuses": [
          {
            "additionalProperties": {},
            "code": "EncryptionState/encrypted",
            "displayStatus": "Encryption is enabled on disk",
            "level": "Info",
            "message": null,
            "time": null
          }
        ]
      }
    ],
    "id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualMachines/0",
    "resourceGroup": "MYRESOURCEGROUP"
  }
]
```

## <a name="disable-encryption"></a>Отключение шифрования

Если вы решите отказаться от шифрования дисков для экземпляров виртуальных машин, его можно отключить с помощью команды [az vmss encryption disable](/cli/azure/vmss/encryption?view=azure-cli-latest#az-vmss-encryption-disable):

```azurecli-interactive
az vmss encryption disable --resource-group myResourceGroup --name myScaleSet
```

## <a name="next-steps"></a>Дальнейшие действия

- В этой статье вы зашифровали масштабируемый набор виртуальных машин с помощью Azure CLI. Для этого можно использовать также [Azure PowerShell](virtual-machine-scale-sets-encrypt-disks-ps.md) и шаблоны для [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) или [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).
- Если вы хотите иметь шифрования дисков Azure применяется после подготовки другое расширение, можно использовать [расширения виртуализации](virtual-machine-scale-sets-extension-sequencing.md). Можно использовать [эти примеры](../security/azure-security-disk-encryption-extension-sequencing.md#sample-azure-templates) Чтобы приступить к работе.
- Полный пример пакетного файла для шифрования дисков данных масштабируемого набора Linux можно найти [здесь](https://gist.githubusercontent.com/ejarvi/7766dad1475d5f7078544ffbb449f29b/raw/03e5d990b798f62cf188706221ba6c0c7c2efb3f/enable-linux-vmss.bat). Этот пример создает группу ресурсов и масштабируемый набор Linux, подключает диск данных размером 5 ГБ и шифрует этот масштабируемый набор виртуальных машин.
