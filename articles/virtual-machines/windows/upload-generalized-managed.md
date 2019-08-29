---
title: Создание управляемой виртуальной машины Azure на основе универсальных локальных дисков VHD | Документация Майкрософт
description: Отправка универсального диска VHD в Azure и создание виртуальных машин с его помощью в модели развертывания Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 09/25/2018
ms.author: cynthn
ms.openlocfilehash: be3ccfd0c562763d0968398ddb042dc5f07dbdcf
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/28/2019
ms.locfileid: "70101564"
---
# <a name="upload-a-generalized-vhd-and-use-it-to-create-new-vms-in-azure"></a>Отправка универсального диска VHD и создание виртуальных машин с его помощью в Azure

В этой статье описывается, как с помощью PowerShell отправить VHD универсальной виртуальной машины в Azure, создать образ на основе VHD и виртуальную машину на основе этого образа. Вы можете отправить диск VHD, экспортированный из локального инструмента виртуализации или из другого облака. При использовании [Управляемых дисков](managed-disks-overview.md) для новой виртуальной машины упрощается управление виртуальными машинами и обеспечивается более высокий уровень доступности виртуальной машины, размещенной в группе доступности. 

Если необходим пример сценария, см. статью [Пример сценария для отправки VHD в Azure и создания виртуальной машины](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md).

## <a name="before-you-begin"></a>Перед началом работы

- Прежде чем передавать в Azure какой-либо виртуальный жесткий диск, следует выполнить инструкции из статьи [Подготовка диска VHD или VHDX для Windows к отправке в Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- Прежде чем начать миграцию на [Управляемые диски](managed-disks-overview.md), внимательно изучите [этот раздел](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]


## <a name="generalize-the-source-vm-by-using-sysprep"></a>Подготовка исходной виртуальной машины к использованию с помощью Sysprep

Помимо прочих действий Sysprep удаляет все сведения о вашей учетной записи и подготавливает машину к использованию в качестве образа. Дополнительные сведения о Sysprep приведены в [обзоре Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview).

Убедитесь, что Sysprep поддерживает роли сервера, запущенные на компьютере. Дополнительные сведения см. в разделе [Поддержка ролей сервера в Sysprep](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [!IMPORTANT]
> Если вы хотите запустить Sysprep перед отправкой виртуального жесткого диска в Azure в первый раз, [подготовьте виртуальную машину](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
> 
> 

1. Выполните вход на виртуальную машину Windows.
2. Откройте окно командной строки с правами администратора. Измените каталог на %windir%\system32\sysprep и запустите файл `sysprep.exe`.
3. В диалоговом окне **Программа подготовки системы** выберите **Переход в окно приветствия системы (OOBE)** и убедитесь, что установлен флажок **Подготовка к использованию**.
4. В разделе **Параметры завершения работы** выберите **Завершение работы**.
5. Нажмите кнопку **ОК**.
   
    ![Запуск Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. После выполнения всех необходимых действий Sysprep завершает работу виртуальной машины. Не перезапускайте виртуальную машину.


## <a name="get-a-storage-account"></a>Получение учетной записи хранения

Вам необходима учетная запись хранения Azure для хранения переданного образа виртуальной машины. Можно использовать существующую учетную запись хранения или создать новую. 

Если для создания управляемого диска для виртуальной машины будет использоваться виртуальный жесткий диск, то расположение учетной записи хранения должно совпадать с расположением, в котором будет создаваться виртуальная машина.

Чтобы отобразить список доступных учетных записей хранения, введите:

```azurepowershell
Get-AzStorageAccount | Format-Table
```

## <a name="upload-the-vhd-to-your-storage-account"></a>Передача виртуального жесткого диска в учетную запись хранения

Используйте командлет [Add-AzVhd](https://docs.microsoft.com/powershell/module/az.compute/add-azvhd), чтобы передать VHD в контейнер в учетной записи хранения. В этом примере файл *myVHD.vhd* передается из расположения *C:\Users\Public\Documents\Virtual hard disks\\* в учетную запись хранения *mystorageaccount*, входящую в группу ресурсов *myResourceGroup*. Файл будет помещен в контейнер с именем *mycontainer*; новое имя файла — *myUploadedVHD.vhd*.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


В случае успешного выполнения отобразится ответ, который выглядит следующим образом.

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

В зависимости от сетевого подключения и размера VHD-файла выполнение этой команды может занять некоторое время.

### <a name="other-options-for-uploading-a-vhd"></a>Другие возможности загрузки виртуального жесткого диска
 
Вы также можете передать виртуальный жесткий диск в учетную запись хранения с помощью одного из следующих инструментов:

- [AzCopy](https://aka.ms/downloadazcopy)
- [API копирования BLOB-объекта хранилища Azure](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Обозреватель хранилищ Azure для передачи больших двоичных объектов](https://azurestorageexplorer.codeplex.com/)
- [Справочник по API REST служб хранилища импорта и экспорта](https://msdn.microsoft.com/library/dn529096.aspx)
-   Мы рекомендуем использовать службу импорта и экспорта, если предполагаемое время передачи больше 7 дней. Чтобы оценить предполагаемое время передачи на основе размера данных и средства передачи, используйте компонент [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html). 
    Импорт и экспорт можно использовать для копирования в учетную запись хранения класса Standard. Необходимо осуществить копирование из учетной записи хранения (цен. категория "Стандартный") в учетную запись хранения (цен. категория "Премиум") с помощью схожего с AzCopy инструмента.

> [!IMPORTANT]
> Если для отправки виртуального жесткого диска в Azure вы используете службу AzCopy, перед запуском сценария убедитесь, что задано значение [ **/BlobType:page**](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-blobs#upload-a-file). Если в качестве места назначения выступает большой двоичный объект и этот параметр не указан, то по умолчанию AzCopy создает блочный BLOB-объект.
> 
> 



## <a name="create-a-managed-image-from-the-uploaded-vhd"></a>Создание управляемого образа на основе переданного виртуального жесткого диска 

Создайте управляемый образ с помощью универсального виртуального жесткого диска ОС. Подставьте собственные значения.


Сначала задайте некоторые параметры.

```powershell
$location = "East US" 
$imageName = "myImage"
```

Создайте образ с помощью универсального виртуального жесткого диска ОС.

```powershell
$imageConfig = New-AzImageConfig `
   -Location $location
$imageConfig = Set-AzImageOsDisk `
   -Image $imageConfig `
   -OsType Windows `
   -OsState Generalized `
   -BlobUri $urlOfUploadedImageVhd `
   -DiskSizeGB 20
New-AzImage `
   -ImageName $imageName `
   -ResourceGroupName $rgName `
   -Image $imageConfig
```


## <a name="create-the-vm"></a>Создание виртуальной машины

Теперь, когда образ готов, из него можно создать одну или несколько виртуальных машин. В этом примере создается виртуальная машина *myVM* из образа *myImage* в группе ресурсов *myResourceGroup*.


```powershell
New-AzVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -ImageName $imageName `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNSG" `
    -PublicIpAddressName "myPIP" `
    -OpenPorts 3389
```


## <a name="next-steps"></a>Следующие шаги

Войдите на свою новую виртуальную машину. Дополнительные сведения см. в статье [Как подключиться к виртуальной машине Azure под управлением Windows и войти на нее](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

