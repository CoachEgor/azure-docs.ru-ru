---
author: jpconnock
ms.service: cloud-services
ms.topic: include
ms.date: 11/25/2018
ms.author: jeconnoc
ms.openlocfilehash: f23d5246401f23694755e63a51c52da5dbf5ac1b
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185629"
---
> [!WARNING]
> Когда вы включаете диагностику в существующей роли, при развертывании пакета все настроенные расширения будут отключены. в частности такие:
>
> * диагностика агента мониторинга Microsoft;
> * мониторинг системы безопасности Microsoft Azure;
> * Антивредоносное ПО Майкрософт                 
> * Microsoft Monitoring Agent
> * агент профилировщика службы Microsoft;      
> * расширение домена Microsoft Azure;        
> * расширение Диагностики Azure для Windows;   
> * расширение удаленного рабочего стола Microsoft Azure;
> * сборщик журналов Microsoft Azure.
>
> Развернув обновленную роль, вы сможете сбросить расширения с использованием портала Azure или PowerShell.
>
