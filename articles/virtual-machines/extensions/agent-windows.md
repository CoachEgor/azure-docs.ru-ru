---
title: Обзор агента виртуальной машины Azure
description: Обзор агента виртуальной машины Azure
services: virtual-machines-windows
documentationcenter: virtual-machines
author: axayjo
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: 0a1f212e-053e-4a39-9910-8d622959f594
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/20/2019
ms.author: akjosh
ms.openlocfilehash: b003f2823ffceebecdb2af681a3bdbb4cf25704c
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/02/2020
ms.locfileid: "75615077"
---
# <a name="azure-virtual-machine-agent-overview"></a>Обзор агента виртуальной машины Azure
Агент виртуальной машины Microsoft Azure — это защищенный упрощенный процесс, который управляет взаимодействием виртуальной машины с контроллером структуры Azure. Основная роль агента виртуальной машины — это включение и выполнение расширений виртуальной машины Azure. Расширения виртуальной машины позволяют выполнять дополнительные действия по настройке виртуальной машины после развертывания, например устанавливать и настраивать программное обеспечение. Они также предоставляют возможности восстановления, такие как сброс пароля администратора виртуальной машины. Расширения виртуальной машины не могут выполняться без агента виртуальной машины Azure.

В этой статье описаны процессы установки, обнаружения и удаления агента виртуальной машины Azure.

## <a name="install-the-vm-agent"></a>"Установить агент виртуальной машины"

### <a name="azure-marketplace-image"></a>Образ из Azure Marketplace

Агент виртуальной машины Azure устанавливается по умолчанию на любой виртуальной машине Windows, развернутой с помощью образа из Azure Marketplace. При развертывании образа из Azure Marketplace с помощью портала, PowerShell, интерфейса командной строки или шаблона Azure Resource Manager также устанавливается агент виртуальной машины.

Пакет гостевого агента Windows состоит из двух частей:

- агент подготовки (PA);
- гостевой агент Windows (WinGA).

Для загрузки виртуальной машины на ней должен быть установлен агент подготовки. Устанавливать WinGA не требуется. Во время развертывания виртуальной машины можно выбрать вариант без установки WinGA. В следующем примере показано, как выбрать параметр *provisionVmAgent* с помощью шаблона Azure Resource Manager.

```json
"resources": [{
"name": "[parameters('virtualMachineName')]",
"type": "Microsoft.Compute/virtualMachines",
"apiVersion": "2016-04-30-preview",
"location": "[parameters('location')]",
"dependsOn": ["[concat('Microsoft.Network/networkInterfaces/', parameters('networkInterfaceName'))]"],
"properties": {
    "osProfile": {
    "computerName": "[parameters('virtualMachineName')]",
    "adminUsername": "[parameters('adminUsername')]",
    "adminPassword": "[parameters('adminPassword')]",
    "windowsConfiguration": {
        "provisionVmAgent": "false"
}
```

Если агенты не установлены, вы не сможете использовать некоторые службы Azure, такие как Azure Backup или Azure Security. Для работы этих служб требуется установить расширение. Если виртуальная машина была развернута без WinGA, установить последнюю версию агента можно позднее.

### <a name="manual-installation"></a>Ручная установка
Агент виртуальной машины Windows можно установить вручную с помощью пакета установщика Windows. Установка вручную может потребоваться при создании пользовательского образа виртуальной машины, который будет развернут в Azure. Чтобы вручную установить агент виртуальной машины Windows, [скачайте установщик агента виртуальной машины](https://go.microsoft.com/fwlink/?LinkID=394789). Агент виртуальной машины поддерживается в Windows Server 2008 R2 и более поздних версиях.

### <a name="prerequisites"></a>Технические условия
Для запуска агента виртуальной машины Windows требуется по меньшей мере Windows Server 2008 R2 (64-бит) с .NET Framework 4,0. См. раздел [Минимальная поддержка версий для агентов виртуальных машин в Azure](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) .

## <a name="detect-the-vm-agent"></a>Обнаружение агента виртуальной машины

### <a name="powershell"></a>PowerShell

Для получения сведений о виртуальных машинах Azure можно воспользоваться модулем PowerShell Azure Resource Manager. Чтобы просмотреть сведения о виртуальной машине, например состояние подготовки агента виртуальной машины, выполните командлет [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm):

```powershell
Get-AzVM
```

В следующем сокращенном примере выходных данных показано свойство *ProvisionVMAgent*, вложенное в *OSProfile*. С помощью этого свойства можно определить, развернут ли на виртуальной машине агент виртуальной машины.

```powershell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : myUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

Следующий скрипт можно использовать для получения краткого списка имен виртуальных машин и состояния агента виртуальной машины.

```powershell
$vms = Get-AzVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>Обнаружение вручную

После входа в виртуальную машину Windows для проверки запущенных процессов можно использовать диспетчер задач. Чтобы обнаружить агент виртуальной машины Azure, откройте диспетчер задач, перейдите на вкладку *Подробности* и найдите процесс с именем **WindowsAzureGuestAgent.exe**. Наличие этого процесса означает, что агент виртуальной машины установлен.


## <a name="upgrade-the-vm-agent"></a>Установка агента виртуальной машины
Агент виртуальной машины Azure для Windows обновляется автоматически. Так как новые виртуальные машины развертываются в Azure, во время подготовки виртуальной машины на них устанавливается последняя версия агента виртуальной машины. Чтобы во время создания образа на пользовательских образах виртуальных машин устанавливалась новая версия агента виртуальной машины, их необходимо обновлять вручную.

## <a name="windows-guest-agent-automatic-logs-collection"></a>Коллекция автоматических журналов гостевого агента Windows
Гостевой агент Windows имеет функцию для автоматического получения некоторых журналов. Этот компонент является контроллером процесса Коллектгуестлогс. exe. Он существует как для облачных служб PaaS, так и для виртуальных машин IaaS, и его цель — быстро & автоматически составлять некоторые журналы диагностики с виртуальной машины, чтобы их можно было использовать для автономного анализа. Собранные журналы — это журналы событий, журналы ОС, журналы Azure и некоторые разделы реестра. Он создает ZIP-файл, который передается на узел виртуальной машины. Этот ZIP-файл можно просмотреть в группах инженеров и специалистов службы поддержки, чтобы исследовать проблемы по запросу клиента, владеющего виртуальной машиной.

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о расширениях виртуальных машин см. в статье c [обзором расширений и компонентов виртуальной машины Azure](overview.md).
