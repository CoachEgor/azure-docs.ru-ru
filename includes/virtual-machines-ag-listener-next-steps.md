---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 27b09ea9cc7ce3cf2559304a26d0c04f3ea30b54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "67185239"
---
Кроме автоматического подключения клиентов к первичной реплике, прослушиватель можно использовать для перенаправления рабочих нагрузок только для чтения на вторичные реплики. Это может повысить производительность и масштабируемость решения в целом. Дополнительные сведения см. в записи блога [Use ReadIntent Routing with Azure AlwaysOn Availability Group Listener](https://go.microsoft.com/fwlink/?LinkId=522515) (Использование маршрутизации ReadIntent с прослушивателем группы доступности AlwaysOn Azure).

> [!NOTE]
> Советы по устранению неполадок прослушивателей Azure см. в записи [Troubleshooting Internal Load Balancer Listener Connectivity in Azure](https://blogs.msdn.microsoft.com/alwaysonpro/2017/02/22/troubleshooting-internal-load-balancer-listener-connectivity-in-azure) (Устранение неполадок прослушивателя внутреннего балансировщика нагрузки Azure) [блога](https://blogs.msdn.com/b/alwaysonpro/) группы поддержки AlwaysOn.
> 
> 

Дополнительные сведения об использовании SQL Server в Azure см. в статье [Приступая к работе с SQL Server в виртуальных машинах Azure](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).

