---
title: Управление виртуальными машинами SQL Server в Azure с помощью портал Azure | Документация Майкрософт
description: Узнайте, как получить доступ к ресурсу виртуальной машины SQL в портал Azure для SQL Server виртуальной машины, размещенной в Azure.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 244ad7d079fd7baf25f8079557576c42d25ca785
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79243215"
---
# <a name="manage-sql-server-vms-in-azure-by-using-the-azure-portal"></a>Управление виртуальными машинами SQL Server в Azure с помощью портал Azure

В [портал Azure](https://portal.azure.com)ресурс **виртуальных машин SQL** является независимой службой управления. Его можно использовать для одновременного просмотра всех SQL Server виртуальных машин и изменения параметров, предназначенных для SQL Server: 

![Ресурс виртуальных машин SQL](media/virtual-machines-windows-sql-manage-portal/sql-vm-manage.png)


## <a name="remarks"></a>Remarks

- Мы рекомендуем использовать ресурс " **виртуальные машины SQL** " для просмотра SQL Server виртуальных машин в Azure и управления ими. Но в настоящее время ресурс " **виртуальные машины SQL** " не поддерживает управление [окончанием поддержки](virtual-machines-windows-sql-server-2008-eos-extend-support.md) SQL Server виртуальных машин. Для управления параметрами прекращения поддержки SQL Server виртуальных машин используйте вместо нее вкладку нерекомендуемая [конфигурация SQL Server](#access-the-sql-server-configuration-tab) . 
- Ресурс " **виртуальные машины SQL** " доступен только для SQL Server виртуальных машин, [зарегистрированных в ПОСТАВЩИКЕ ресурсов виртуальной машины SQL](virtual-machines-windows-sql-register-with-resource-provider.md). 


## <a name="access-the-sql-virtual-machines-resource"></a>Доступ к ресурсу виртуальных машин SQL
Чтобы получить доступ к ресурсу **виртуальных машин SQL** , выполните следующие действия.

1. Откройте [портал Azure](https://portal.azure.com). 
1. Выбор пункта **Все службы**. 
1. В поле поиска введите **виртуальные машины SQL** .
1. (Необязательно): выберите звездочку рядом с параметром **виртуальные машины SQL** , чтобы добавить этот параметр в меню **Избранное** . 
1. Выберите **виртуальные машины SQL**. 

   ![Поиск SQL Server виртуальных машин во всех службах](media/virtual-machines-windows-sql-manage-portal/sql-vm-search.png)

1. На портале перечислены все SQL Server виртуальные машины, доступные в рамках подписки. Выберите ту, которой вы хотите управлять, чтобы открыть ресурс " **виртуальные машины SQL** ". Используйте поле поиска, если виртуальная машина SQL Server не отображается. 

   ![Все доступные SQL Server виртуальные машины](media/virtual-machines-windows-sql-manage-portal/all-sql-vms.png)

   При выборе ВИРТУАЛЬНОЙ машины в SQL Server открывается ресурс " **виртуальные машины SQL** ": 


   ![Ресурс виртуальных машин SQL](media/virtual-machines-windows-sql-manage-portal/sql-vm-resource.png)

> [!TIP]
> Ресурс **виртуальных машин SQL** предназначен для выделенных параметров SQL Server. Выберите имя ВИРТУАЛЬНОЙ машины в поле **Виртуальная машина** , чтобы открыть параметры, относящиеся к виртуальной машине, но не только SQL Server. 

## <a name="access-the-sql-server-configuration-tab"></a>Доступ к вкладке "Конфигурация SQL Server"
Вкладка " **конфигурация SQL Server** " устарела. В настоящее время это единственный способ управления [окончанием поддержки](virtual-machines-windows-sql-server-2008-eos-extend-support.md) SQL Server виртуальных машин и SQL Server виртуальных машин, которые не были [зарегистрированы в ПОСТАВЩИКЕ ресурсов виртуальной машины SQL](virtual-machines-windows-sql-register-with-resource-provider.md).

Чтобы открыть вкладку нерекомендуемая **конфигурация SQL Server** , перейдите к ресурсу **виртуальных машин** . Выполните указанные ниже действия.

1. Откройте [портал Azure](https://portal.azure.com). 
1. Выбор пункта **Все службы**. 
1. Введите **виртуальные машины** в поле поиска.
1. (Необязательно): выберите звездочку рядом с пунктом **виртуальные машины** , чтобы добавить этот параметр в меню **Избранное** . 
1. Выберите **Виртуальные машины**. 

   ![Поиск виртуальных машин](media/virtual-machines-windows-sql-manage-portal/vm-search.png)

1. На портале выводится список всех виртуальных машин в подписке. Выберите ту, которой вы хотите управлять, чтобы открыть ресурс " **виртуальные машины** ". Используйте поле поиска, если виртуальная машина SQL Server не отображается. 
1. Выберите **SQL Server конфигурация** в области **Параметры** для управления виртуальной машиной SQL Server. 

   ![Конфигурация SQL Server](media/virtual-machines-windows-sql-manage-portal/sql-vm-configuration.png)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в следующих статьях: 

* [Обзор SQL Server на виртуальных машинах Windows](virtual-machines-windows-sql-server-iaas-overview.md).
* [Часто задаваемые вопросы о SQL Server на виртуальной машине Windows](virtual-machines-windows-sql-server-iaas-faq.md)
* [Руководство по ценам для SQL Server на виртуальной машине Windows](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Заметки о выпуске для SQL Server на виртуальной машине Windows](virtual-machines-windows-sql-server-iaas-release-notes.md)


