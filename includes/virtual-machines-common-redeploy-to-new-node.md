---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 1c3996c3f40da496af0cd795d0873864667a1f19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "67185292"
---
## <a name="use-the-azure-portal"></a>Использование портала Azure
1. Выберите виртуальную машину для повторного развертывания и нажмите кнопку *Повторить развертывание* в колонке *Параметры*. Прокрутите вниз, чтобы отобразить раздел **Поддержка и устранение проблем**, содержащий кнопку "Повторить развертывание", как показано в примере ниже.
   
    ![Колонка виртуальной машины Azure](./media/virtual-machines-common-redeploy-to-new-node/vmoverview.png)
2. Для подтверждения операции нажмите кнопку *Повторить развертывание*.
   
    ![Колонка повторного развертывания виртуальной машины](./media/virtual-machines-common-redeploy-to-new-node/redeployvm.png)
3. Текущее **состояние** виртуальной машины изменится на *Выполняется обновление* при подготовке к повторному развертыванию, как показано ниже.
   
    ![Обновление виртуальной машины](./media/virtual-machines-common-redeploy-to-new-node/vmupdating.png)
4. Затем **состояние** изменится на *Запуск* при загрузке виртуальной машины на новый узел Azure, как показано ниже.
   
    ![Запуск виртуальной машины](./media/virtual-machines-common-redeploy-to-new-node/vmstarting.png)
5. По завершении процесса загрузки **состояние** виртуальной машины снова вернется к значению *Работает*. Это значит, что повторное развертывание виртуальной машины успешно завершено:
   
    ![Выполнение виртуальной машины](./media/virtual-machines-common-redeploy-to-new-node/vmrunning.png)

