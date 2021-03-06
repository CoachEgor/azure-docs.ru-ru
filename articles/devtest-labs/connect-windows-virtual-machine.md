---
title: Подключение к виртуальным машинам Windows в Azure DevTest Labs
description: Узнайте, как подключиться к виртуальной машине Windows в лаборатории (Azure DevTest Labs)
ms.topic: how-to
ms.date: 07/17/2020
ms.openlocfilehash: e1e786daa396548030976159d1b150caa4b24396
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86540719"
---
# <a name="connect-to-a-windows-vm-in-your-lab-azure-devtest-labs"></a>Подключение к виртуальной машине Windows в лаборатории (Azure DevTest Labs)
В этой статье показано, как подключиться к виртуальной машине Windows в лаборатории. 

## <a name="connect-to-a-windows-vm"></a>Подключение к виртуальной машине Windows
1. Войдите на [портал Azure](https://portal.azure.com).
1. В строке поиска найдите и выберите **DevTest Labs**. 

    :::image type="content" source="./media/connect-windows-virtual-machine/search-select.png" alt-text="Найдите и выберите DevTest Labs.":::    
1. В списке лабораторий выберите свою **лабораторию**.

    :::image type="content" source="./media/connect-windows-virtual-machine/select-lab.png" alt-text="Найдите и выберите DevTest Labs.":::            
1. На домашней странице лаборатории выберите виртуальную машину Windows из списка **мои виртуальные машины** . 

    :::image type="content" source="./media/connect-windows-virtual-machine/select-windows-vm.png" alt-text="Найдите и выберите DevTest Labs.":::                
1. На странице **виртуальной машины** выберите **подключить** на панели инструментов. Если виртуальная машина остановлена, выберите **запустить** сначала, чтобы запустить виртуальную машину.

    :::image type="content" source="./media/connect-windows-virtual-machine/select-connect.png" alt-text="Найдите и выберите DevTest Labs.":::                    
1. Если вы используете браузер Microsoft ребра, вы увидите ссылку на **скачанный RDP-файл** в нижней части браузера. 

    :::image type="content" source="./media/connect-windows-virtual-machine/rdp-download.png" alt-text="Найдите и выберите DevTest Labs.":::                        
1. Откройте RDP-файл и введите учетные данные виртуальной машины, введенные при создании виртуальной машины. Вы должны быть подключены к виртуальной машине Windows сейчас. 

## <a name="next-steps"></a>Следующие шаги
[Как подключиться к виртуальной машине Linux](connect-linux-virtual-machine.md)
