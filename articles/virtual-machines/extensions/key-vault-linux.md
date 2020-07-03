---
title: Расширение виртуальной машины Azure Key Vault для Linux
description: Развертывание агента, выполняющего автоматическое обновление сертификатов Key Vault на виртуальных машинах с помощью расширения виртуальной машины.
services: virtual-machines-linux
author: msmbaldwin
tags: keyvault
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 12/02/2019
ms.author: mbaldwin
ms.openlocfilehash: add2d515e4f8e8c56a98a7292e137e601332d10c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80410864"
---
# <a name="key-vault-virtual-machine-extension-for-linux"></a>Расширение виртуальной машины Key Vault для Linux

Расширение Key Vault VM обеспечивает автоматическое обновление сертификатов, хранящихся в хранилище ключей Azure. В частности, расширение отслеживает список наблюдаемых сертификатов, хранящихся в хранилищах ключей.  При обнаружении изменения расширение получает и устанавливает соответствующие сертификаты. Расширение Key Vault VM Опубликовано и поддерживается корпорацией Майкрософт в настоящее время на виртуальных машинах Linux. В этом документе описаны поддерживаемые платформы, конфигурации и варианты развертывания для расширения Key Vault VM для Linux. 

### <a name="operating-system"></a>Операционная система

Расширение виртуальной машины Key Vault поддерживает следующие дистрибутивы Linux:

- Ubuntu-1604
- Ubuntu-1804
- Debian-9
- SUSE-15 

### <a name="supported-certificate-content-types"></a>Поддерживаемые типы содержимого сертификатов

- #12 PKCS
- PEM

## <a name="extension-schema"></a>Схема расширения

В следующем JSON-файле показана схема для расширения виртуальной машины Key Vault. Расширению не требуются защищенные параметры, так как все его параметры считаются информационными и не влияют на безопасность. Для расширения необходим список отслеживаемых секретов, указание частоты опроса и конечное хранилище сертификатов. В частности, внесены следующие изменения.  
```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KVVMExtensionForLinux",
      "apiVersion": "2019-07-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
      "publisher": "Microsoft.Azure.KeyVault",
      "type": "KeyVaultForLinux",
      "typeHandlerVersion": "1.0",
      "autoUpgradeMinorVersion": true,
      "settings": {
        "secretsManagementSettings": {
          "pollingIntervalInS": <polling interval in seconds, e.g. "3600">,
          "certificateStoreName": <certificate store name, e.g.: "MY">,
          "linkOnRenewal": <Not available on Linux e.g.: false>,
          "certificateStoreLocation": <certificate store location, currently it works locally only e.g.: "LocalMachine">,
          "requireInitialSync": <initial synchronization of certificates e..g: true>,
          "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net/secrets/mycertificate"
        }      
      }
      }
    }
```

> [!NOTE]
> Наблюдаемые URL-адреса сертификатов должны иметь форму `https://myVaultName.vault.azure.net/secrets/myCertName`.
> 
> Это происходит потому, `/secrets` что путь возвращает полный сертификат, включая закрытый ключ, а `/certificates` путь — нет. Дополнительные сведения о сертификатах можно найти здесь: [Key Vault Certificates](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-certificates) .


### <a name="property-values"></a>Значения свойств

| Имя | Значение и пример | Тип данных |
| ---- | ---- | ---- |
| версия_API | 2019-07-01 | Дата |
| publisher | Microsoft.Azure.KeyVault | строка |
| type | кэйваултфорлинукс | строка |
| typeHandlerVersion | 1.0 | INT |
| pollingIntervalInS | 3600 | строка |
| certificateStoreName | MY | строка |
| линконреневал | false | Логическое |
| certificateStoreLocation  | LocalMachine | строка |
| рекуирединитиалсинк | Да | Логическое |
| observedCertificates  | ["https://myvault.vault.azure.net/secrets/mycertificate"] | массив строк


## <a name="template-deployment"></a>Развертывание шаблона

Расширения виртуальной машины Azure можно развернуть с помощью шаблонов Azure Resource Manager. Шаблоны идеально подходят для развертывания одной или нескольких виртуальных машин, требующих обновления сертификатов, выполняемого после развертывания. Расширение можно развернуть на отдельных виртуальных машинах или в масштабируемых наборах виртуальных машин. Для обоих типов шаблонов используются общие схема и конфигурация. 

Конфигурация JSON для расширения виртуальной машины должна быть вложена в фрагмент ресурса виртуальной машины шаблона, а именно `"resources": []` объект для шаблона виртуальной машины и в случае масштабируемого набора виртуальных машин в разделе `"virtualMachineProfile":"extensionProfile":{"extensions" :[]` объект.

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KeyVaultForLinux",
      "apiVersion": "2019-07-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
      "publisher": "Microsoft.Azure.KeyVault",
      "type": "KeyVaultForLinux",
      "typeHandlerVersion": "1.0",
      "autoUpgradeMinorVersion": true,
      "settings": {
          "secretsManagementSettings": {
          "pollingIntervalInS": <polling interval in seconds, e.g. "3600">,
          "certificateStoreName": <certificate store name, e.g.: "MY">,
          "certificateStoreLocation": <certificate store location, currently it works locally only e.g.: "LocalMachine">,
          "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net/secrets/mycertificate"
        }      
      }
      }
    }
```


## <a name="azure-powershell-deployment"></a>Развертывание с помощью Azure PowerShell

Azure PowerShell можно использовать для развертывания расширения виртуальной машины Key Vault на имеющейся виртуальной машине или в масштабируемых наборах виртуальных машин. 

* Развертывание расширения на виртуальной машине:
    
    ```powershell
        # Build settings
        $settings = '{"secretsManagementSettings": 
        { "pollingIntervalInS": "' + <pollingInterval> + 
        '", "certificateStoreName": "' + <certStoreName> + 
        '", "certificateStoreLocation": "' + <certStoreLoc> + 
        '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName =  "KeyVaultForLinux"
        $extPublisher = "Microsoft.Azure.KeyVault"
        $extType = "KeyVaultForLinux"
       
    
        # Start the deployment
        Set-AzVmExtension -TypeHandlerVersion "1.0" -ResourceGroupName <ResourceGroupName> -Location <Location> -VMName <VMName> -Name $extName -Publisher $extPublisher -Type $extType -SettingString $settings
    
    ```

* Чтобы развернуть расширение в масштабируемом наборе виртуальных машин, выполните следующие действия.

    ```powershell
    
        # Build settings
        $settings = '{"secretsManagementSettings": 
        { "pollingIntervalInS": "' + <pollingInterval> + 
        '", "certificateStoreName": "' + <certStoreName> + 
        '", "certificateStoreLocation": "' + <certStoreLoc> + 
        '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName = "KeyVaultForLinux"
        $extPublisher = "Microsoft.Azure.KeyVault"
        $extType = "KeyVaultForLinux"
        
        # Add Extension to VMSS
        $vmss = Get-AzVmss -ResourceGroupName <ResourceGroupName> -VMScaleSetName <VmssName>
        Add-AzVmssExtension -VirtualMachineScaleSet $vmss  -Name $extName -Publisher $extPublisher -Type $extType -TypeHandlerVersion "1.0" -Setting $settings

        # Start the deployment
        Update-AzVmss -ResourceGroupName <ResourceGroupName> -VMScaleSetName <VmssName> -VirtualMachineScaleSet $vmss 
    
    ```

## <a name="azure-cli-deployment"></a>Развертывание с помощью Azure CLI

Azure CLI можно использовать для развертывания расширения виртуальной машины Key Vault в существующей виртуальной машине или в масштабируемом наборе виртуальных машин. 
 
* Развертывание расширения на виртуальной машине:
    
    ```azurecli
       # Start the deployment
         az vm extension set -n "KeyVaultForLinux" `
         --publisher Microsoft.Azure.KeyVault `
         -g "<resourcegroup>" `
         --vm-name "<vmName>" `
         --settings '{\"secretsManagementSettings\": { \"pollingIntervalInS\": \"<pollingInterval>\", \"certificateStoreName\": \"<certStoreName>\", \"certificateStoreLocation\": \"<certStoreLoc>\", \"observedCertificates\": [\ <observedCerts>\"] }}'
    ```

* Чтобы развернуть расширение в масштабируемом наборе виртуальных машин, выполните следующие действия.

   ```azurecli
        # Start the deployment
        az vmss extension set -n "KeyVaultForLinux" `
        --publisher Microsoft.Azure.KeyVault `
        -g "<resourcegroup>" `
        --vm-name "<vmName>" `
        --settings '{\"secretsManagementSettings\": { \"pollingIntervalInS\": \"<pollingInterval>\", \"certificateStoreName\": \"<certStoreName>\", \"certificateStoreLocation\": \"<certStoreLoc>\", \"observedCertificates\": [\ <observedCerts>\"] }}'
    ```

Ознакомьтесь со следующими ограничениями и требованиями.
- Ограничения, касающиеся решения Key Vault:
  - оно должно существовать во время развертывания; 
  - Политика доступа Key Vault настроена для удостоверения виртуальной машины или VMSS с помощью MSI


## <a name="troubleshoot-and-support"></a>Устранение неполадок и поддержка

### <a name="troubleshoot"></a>Устранение неполадок

Данные о состоянии развертываний расширения можно получить на портале Azure, а также с помощью Azure PowerShell. Чтобы просмотреть состояние развертывания расширений для определенной виртуальной машины, выполните следующую команду в Azure PowerShell.

## <a name="azure-powershell"></a>Azure PowerShell
```powershell
Get-AzVMExtension -VMName <vmName> -ResourceGroupname <resource group name>
```

## <a name="azure-cli"></a>Azure CLI
```azurecli
 az vm get-instance-view --resource-group <resource group name> --name  <vmName> --query "instanceView.extensions"
```

### <a name="support"></a>Поддержка

Если вам нужна дополнительная помощь в любой момент в этой статье, вы можете обратиться к экспертам по Azure на [форумах MSDN Azure и Stack overflow](https://azure.microsoft.com/support/forums/). Кроме того, можно зарегистрировать обращение в службу поддержки Azure. Перейдите на [сайт поддержки Azure](https://azure.microsoft.com/support/options/) и выберите получить поддержку. Дополнительные сведения об использовании службы поддержки Azure см. в статье [Часто задаваемые вопросы о поддержке Microsoft Azure](https://azure.microsoft.com/support/faq/).
