---
title: Создание пользовательского образа Azure DevTest Labs из VHD-файла | Документация Майкрософт
description: Сведения о создании пользовательского образа в Azure DevTest Labs из VHD-файла с использованием портала Azure
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 853c138c8cf73b41b0cebb6c1d349865e18eab6a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "61296173"
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>Создание пользовательского образа из VHD-файла

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Пошаговые инструкции

Ниже приведены инструкции по созданию пользовательского образа из VHD-файла с помощью портала Azure.

1. Войдите на [портал Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Выберите **все службы,** а затем выберите **DevTest Labs** из списка.

1. Из списка лабораторий выберите нужную лабораторию.  

1. В главной области лаборатории выберите **Конфигурация и политики**. 

1. На панели **Конфигурация и политики** выберите **Пользовательские образы**.

1. На панели **Пользовательские образы** выберите **+ Добавить**.

    ![Добавить пользовательский образ](./media/devtest-lab-create-template/add-custom-image.png)

1. Введите имя пользовательского образа. Это имя отображается в списке базовых образов при создании виртуальной машины.

1. Введите описание пользовательского образа. Это описание отображается в списке базовых образов при создании виртуальной машины.

1. Для параметра **Тип ОС** выберите значение **Windows** или **Linux**.

    - Если выбираете **Windows**, укажите с помощью флажка, выполняется ли на компьютере *sysprep*. 
    - Если выбираете **Linux**, укажите с помощью флажка, выполняется ли на компьютере *deprovision*. 

1. В раскрывающемся меню выберите **VHD**. Это виртуальный жесткий диск, который будет использоваться для создания пользовательского образа. При необходимости выберите параметр **Отправить VHD с помощью PowerShell**.

1. Кроме того, вы можете указать имя, предложение и издателя плана, если образ, используемый для создания пользовательского образа, не является лицензированным (опубликованным корпорацией Майкрософт).

   - **Имя плана.** Введите имя образа Marketplace (SKU), на основе которого создан этот пользовательский образ. 
   - **Предложение плана.** Укажите продукт (предложение) для образа Marketplace, на основе которого создан этот пользовательский образ. 
   - **Издатель плана.** Укажите издателя образа Marketplace, на основе которого создан этот пользовательский образ.

   > [!NOTE]
   > Если образ, с помощью которого вы создаете пользовательский образ, **не** лицензирован, эти поля останутся пустыми. Вы можете заполнить их по желанию. Если образ **лицензирован**, в полях автоматически отобразятся сведения о плане. В таком случае при попытке их изменить появится предупреждающее сообщение.
   >
   >

1. Нажмите кнопку **ОК** , чтобы создать пользовательский образ.

Через несколько минут пользовательский образ создается и сохраняется в учетной записи хранения лаборатории. Когда пользователь лаборатории создает новую виртуальную машину, этот образ доступен в списке базовых.

![Пользовательский образ в списке базовых образов](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Связанные записи в блогах

- [Custom images or formulas? (Пользовательские изображения или формулы?)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Copying Custom Images between Azure DevTest Labs (Копирование пользовательских образов между лабораториями для разработки и тестирования Azure)](https://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Дальнейшие действия

- [Добавление виртуальной машины с артефактами в лабораторию в Azure DevTest Labs](./devtest-lab-add-vm.md)
