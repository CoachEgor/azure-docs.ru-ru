---
title: Установка драйвера GPU на виртуальные машины Azure серии N для Windows | Документация Майкрософт
description: Как установить драйверы NVIDIA GPU для виртуальных машин серии N под управлением Windows Server или Windows в Azure
services: virtual-machines-windows
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: f3950c34-9406-48ae-bcd9-c0418607b37d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 24189fa8e0f6c31d7fbd3779f666eb85e24dc8f7
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723114"
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-windows"></a>Установка драйверов GPU NVIDIA на виртуальные машины серии N под управлением Windows 

Чтобы воспользоваться преимуществами GPU виртуальных машин Azure серии N под управлением Windows, необходимо установить графические драйверы GPU NVIDIA. [Расширение драйвера GPU NVIDIA](../extensions/hpccompute-gpu-windows.md) устанавливает необходимые драйверы CUDA или GRID NVIDIA на виртуальную машину серии N. Для установки расширения и управления им можно использовать портал Azure или такие инструменты, как Azure PowerShell и шаблоны Azure Resource Manager. Сведения о поддерживаемых операционных системах и этапах развертывания см. в [документации по расширению драйвера GPU NVIDIA](../extensions/hpccompute-gpu-windows.md).

Если вы решили установить драйверы GPU вручную, то в этой статье вы найдете сведения о поддерживаемых операционных системах, ссылки на драйвера и инструкции по установке и проверке. Сведения о ручной установке драйверов также доступны для [виртуальных машин Linux](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Основные характеристики, сведения о дисках и объеме памяти см. в статье [Графический процессор](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]

## <a name="driver-installation"></a>Установка драйвера

1. Подключитесь к каждой виртуальной машине серии N с помощью удаленного рабочего стола.

2. Скачайте, извлеките и установите поддерживаемый драйвер для своей операционной системы Windows.

После установки драйвера GRID на виртуальной машине ее требуется перезагрузить. После установки драйвера CUDA перезапуск не требуется.

## <a name="verify-driver-installation"></a>Проверка установки драйверов

Установку драйвера можно проверить в диспетчере устройств. В следующем примере показана успешная конфигурация карты Tesla K80 на виртуальной машине Azure серии NC.

![Свойства драйвера GPU](./media/n-series-driver-setup/GPU_driver_properties.png)

Чтобы запросить состояние устройства GPU, выполните служебную программу командной строки [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface), установленную вместе с драйвером.

1. Откройте командную строку и измените каталог на **:\Program Files\NVIDIA Corporation\NVSMI**.

2. Запустите `nvidia-smi`. Если драйвер установлен, то отобразятся выходные данные, аналогичные приведенным ниже. **GPU-Util** отобразит **0 %** , если только в этот момент графический процессор не выполняет рабочую нагрузку на виртуальной машине. Версия драйвера и сведения о GPU могут отличаться от показанных на изображении.

![Состояние устройства NVIDIA](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-connectivity"></a>Сетевое подключение RDMA

Сетевое подключение RDMA можно включить на виртуальных машинах серии N с поддержкой RDMA, таких как NC24r, развернутых в одной группе доступности или в одной группе размещения в масштабируемом наборе виртуальных машин. Необходимо добавить расширение HpcVmDrivers для установки драйверов сетевых устройств Windows, обеспечивающих подключения RDMA. Чтобы в виртуальную машину серии N с поддержкой RDMA добавить расширение виртуальной машины, используйте командлеты [Azure PowerShell](/powershell/azure/overview) для Azure Resource Manager.

Чтобы установить последнюю версию расширения HpcVMDrivers 1.1 на существующую виртуальную машину myVM с поддержкой RDMA, размещенную в регионе "Западная часть США", выполните следующее.
  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Дополнительные сведения см. в разделе [Обзор расширений и компонентов виртуальной машины под управлением Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Сеть RDMA поддерживает трафик MPI (Message Passing Interface) для приложений, использующих [Microsoft MPI](https://docs.microsoft.com/message-passing-interface/microsoft-mpi) или Intel MPI 5.x. 


## <a name="next-steps"></a>Следующие шаги

* Разработчики приложений с ускорением GPU, предназначенных для графических процессоров NVIDIA Tesla, могут также скачать и установить последнюю версию [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads). Дополнительные сведения см. в [руководстве по установке CUDA](https://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).


