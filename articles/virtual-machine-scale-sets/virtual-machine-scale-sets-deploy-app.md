---
title: Развертывание приложения в наборе виртуальной машины Azure
description: Узнайте, как развертывать приложения в экземплярах виртуальных машин Windows и Linux, размещенных в масштабируемом наборе.
author: ju-shim
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: jushiman
ms.openlocfilehash: e157db79f1835a8ca891dd360a99e3319565d1d4
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2020
ms.locfileid: "81011469"
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>Развертывание приложения в масштабируемых наборах виртуальных машин

Для запуска приложений в экземплярах виртуальных машин в масштабируемом наборе необходимо сначала установить компоненты и необходимые файлы этих приложений. В этой статье описано, как создать настраиваемый образ виртуальной машины для экземпляров в масштабируемом наборе или автоматически запустить сценарии установки на существующих экземплярах виртуальных машин. Вы также узнаете, как управлять приложением или обновлениями ОС в масштабируемом наборе.


## <a name="build-a-custom-vm-image"></a>Создание настраиваемого образа виртуальной машины
При использовании одного из образов платформы Azure для создания экземпляров в масштабируемом наборе не выполняется установка или настройка какого-либо дополнительного программного обеспечения. Можно автоматизировать установку этих компонентов, хотя это увеличит время подготовки экземпляров виртуальных машин для масштабируемого набора. Если внести много изменений в конфигурацию экземпляров виртуальных машин, это увеличит число операций управления, связанных с этими сценариями и задачами настройки.

Чтобы уменьшить число операций управления конфигурацией и время подготовки виртуальной машины, можно создать настраиваемый образ виртуальной машины, который позволит подготавливать в масштабируемом наборе экземпляры, уже готовые к запуску вашего приложения. Дополнительные сведения о том, как создать и использовать настраиваемый образ виртуальной машины с масштабируемым набором см. в документах ниже:

- [Azure CLI](tutorial-use-custom-image-cli.md)
- [Azure PowerShell](tutorial-use-custom-image-powershell.md)


## <a name="install-an-app-with-the-custom-script-extension"></a><a name="already-provisioned"></a>Установка приложения с помощью расширения настраиваемых сценариев
Расширение настраиваемых сценариев скачивает и выполняет сценарии на виртуальных машинах Azure. Это расширение можно использовать для настройки после развертывания, установки программного обеспечения и других задач настройки или управления. Сценарии можно скачать из службы хранилища Azure или GitHub или передать на портал Azure во время выполнения расширения. Для получения дополнительной информации о том, как установить приложение с расширением пользовательского сценария, см.

- [Azure CLI](tutorial-install-apps-cli.md)
- [Azure PowerShell](tutorial-install-apps-powershell.md)
- [Шаблон Azure Resource Manager](tutorial-install-apps-template.md)


## <a name="install-an-app-to-a-windows-vm-with-powershell-dsc"></a>Установка приложения на виртуальную машину Windows с помощью PowerShell DSC
[PowerShell Desired State Configuration](/powershell/scripting/dsc/overview/overview) — это платформа управления, которая позволяет определить конфигурацию целевых виртуальных машин. Конфигурации DSC определяют компоненты, которые следует установить на виртуальной машине, а также параметры настройки узла. На каждом целевом узле, который обрабатывает запрашиваемые действия на основе отправленных конфигураций, выполняется модуль локального диспетчера конфигураций (LCM).

Расширение PowerShell DSC позволяет настроить экземпляры виртуальных машин в масштабируемом наборе с помощью PowerShell. Следующий пример:

- Инструктирует экземпляры VM для загрузки пакета DSC с GitHub -*https://github.com/Azure-Samples/compute-automation-configurations/raw/master/dsc.zip*
- Настраивает расширение для запуска сценария установки: `configure-http.ps1`.
- Получает сведения о масштабируемом наборе с помощью командлета [Get-AzVmss](/powershell/module/az.compute/get-azvmss).
- Применяет расширение к экземплярам виртуальных машин с помощью командлета [Update-AzVmss](/powershell/module/az.compute/update-azvmss).

Расширение DSC применяется к экземплярам виртуальных машин набора *myScaleSet* в группе ресурсов *myResourceGroup*. Введите свои имена следующим образом.

```powershell
# Define the script for your Desired Configuration to download and run
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/Azure-Samples/compute-automation-configurations/raw/master/dsc.zip";
    "script" = "configure-http.ps1";
    "function" = "WebsiteTest";
  };
}

# Get information about the scale set
$vmss = Get-AzVmss `
                -ResourceGroupName "myResourceGroup" `
                -VMScaleSetName "myScaleSet"

# Add the Desired State Configuration extension to install IIS and configure basic website
$vmss = Add-AzVmssExtension `
    -VirtualMachineScaleSet $vmss `
    -Publisher Microsoft.Powershell `
    -Type DSC `
    -TypeHandlerVersion 2.24 `
    -Name "DSC" `
    -Setting $dscConfig

# Update the scale set and apply the Desired State Configuration extension to the VM instances
Update-AzVmss `
    -ResourceGroupName "myResourceGroup" `
    -Name "myScaleSet"  `
    -VirtualMachineScaleSet $vmss
```

Если для масштабируемого набора задана политика обновления *вручную*, обновите экземпляры виртуальных машин с помощью командлета [Update-AzVmssInstance](/powershell/module/az.compute/update-azvmssinstance). Этот командлет применит обновленную конфигурацию масштабируемого набора к экземплярам виртуальных машин и установит ваше приложение.


## <a name="install-an-app-to-a-linux-vm-with-cloud-init"></a>Установка приложения на виртуальную машину Linux с помощью cloud-init
[Cloud-init](https://cloudinit.readthedocs.io/en/latest/index.html) — это широко используемый подход к настройке Linux VM, так как он загружается впервый раз. Вы можете использовать cloud-init для установки пакетов, записи файлов или настройки пользователей и параметров безопасности. Так как cloud-init выполняется при начальной загрузке, для применения вашей конфигурации не требуются какие-либо дополнительные действия или обязательные агенты.

Кроме того, cloud-init работает с разными дистрибутивами. Например, для установки пакета не используется **apt-get install** или **yum install**. Вместо этого можно определить список пакетов для установки. Файл cloud-init автоматически использует собственный инструмент управления пакетами из выбранного дистрибутива.

Дополнительные сведения, включая пример файла *cloud-init.txt*, доступны в разделе [Настройка виртуальной машины Linux в Azure с помощью cloud-init](../virtual-machines/linux/using-cloud-init.md).

Чтобы создать масштабируемый набор и использовать файл cloud-init, добавьте параметр `--custom-data` в команду [az vmss create](/cli/azure/vmss) и укажите имя файла cloud-init. Следующий пример создает масштабируемый набор *myScaleSet* в группе ресурсов *myResourceGroup* и настраивает экземпляры виртуальных машин, используя файл *cloud-init.txt*. Введите свои имена следующим образом.

```azurecli
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys
```


### <a name="install-applications-with-os-updates"></a>Установка приложений с обновлениями ОС
Если доступны новые выпуски ОС, можно использовать имеющийся или создать новый настраиваемый образ и [развернуть обновления ОС](virtual-machine-scale-sets-upgrade-scale-set.md) в масштабируемом наборе. Каждый экземпляр виртуальной машины будет обновлен до последней версии указанного образа. Можно использовать настраиваемый образ с предварительно установленным приложением, расширение настраиваемых сценариев или PowerShell DSC, чтобы автоматически предоставить свое приложение при выполнении обновления. Может потребоваться спланировать обслуживание приложения при выполнении этого процесса, чтобы избежать проблем совместимости версий.

При использовании настраиваемого образа виртуальной машины с предварительно установленным приложением обновления приложения можно интегрировать в конвейер развертывания, чтобы создавать новые образы и развертывать обновления ОС в масштабируемом наборе. Такой подход позволяет конвейеру извлечь последние сборки приложения, создать и проверить образ виртуальной машины, а затем обновить экземпляры виртуальных машин в масштабируемом наборе. Чтобы запустить конвейер развертывания, который выполняет сборку обновлений приложения и развертывает их в настраиваемых образах виртуальных машин, можно [создать образ Packer и развернуть его с помощью Azure DevOps Services](/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset) или использовать другую платформу (например, [Spinnaker](https://www.spinnaker.io/) или [Jenkins](https://jenkins.io/)).


## <a name="next-steps"></a>Следующие шаги
Сведения о создании и развертывании приложений в масштабируемых наборах вы можете получить из раздела [Рекомендации по проектированию масштабируемых наборов](virtual-machine-scale-sets-design-overview.md). Дополнительные сведения об управлении масштабируемым набором см. в разделе [Управление масштабируемым набором виртуальных машин с помощью Azure PowerShell](virtual-machine-scale-sets-windows-manage.md).
