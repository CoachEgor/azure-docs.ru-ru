---
title: Удаленное управление Windows по сравнению с HTTPS для Azure (ru) Лазурный рынок
description: Объясняет, как настроить VM на базе Windows, на основе Windows, чтобы управлять им удаленно с помощью PowerShell.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: dsindona
ms.openlocfilehash: b2a4bb107309894a7180e0a4585cdba6f04d1bee
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273041"
---
# <a name="windows-remote-management-over-https"></a>Служба удаленного управления Windows по протоколу HTTPS

> [!IMPORTANT]
> С 13 апреля 2020 года мы начнем перемещение управления вашими предложениями Azure Virtual Machine в Партнерский центр. После миграции вы будете создавать и управлять своими предложениями в Partner Center. Следуйте инструкциям в [Create an Azure Virtual Machine, чтобы](https://aka.ms/CreateAzureVMoffer) управлять мигрированными предложениями.

В этом разделе описывается настройка размещенной в Azure виртуальной машины на базе Windows для удаленного управления и развертывания с помощью PowerShell.  Чтобы включить удаленное взаимодействие посредством PowerShell, на целевой виртуальной машине должна предоставляться конечная точка HTTPS для службы удаленного управления Windows (WinRM).  Дополнительные сведения об удаленном взаимодействии посредством PowerShell см. в разделе [Running Remote Commands](https://docs.microsoft.com/powershell/scripting/learn/remoting/running-remote-commands) (Выполнение удаленных команд).  Дополнительные сведения о WinRM см. в статье [Windows Remote Management](https://docs.microsoft.com/windows/desktop/WinRM/portal) (Удаленное управление Windows).

Если вы создали VM с помощью одного из "классических" подходов Azure - либо портала управления службами Azure, либо изуродованного [API управления службой Azure,](https://docs.microsoft.com/previous-versions/azure/ee460799(v=azure.100))то он автоматически настраивается с конечной точкой WinRM.  Тем не менее, если вы создаете виртуальную машину любым из следующих "современных" для Azure способов, на ней WinRM по протоколу HTTPS *не* настраивается.

- С помощью [портала Azure](https://portal.azure.com/), обычно на основе утвержденного базового образа в соответствии с инструкциями в статье [Создание виртуального жесткого диска, совместимого с Azure](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-create-vhd).
- [С помощью шаблонов Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/ps-template)
- С помощью командной оболочки Azure PowerShell или Azure CLI.  Например, [см. квикстарт: Создайте виртуальную машину windows в Azure с PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell) и [quickstart: Создайте виртуальную машину Linux с Azure CLI.](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-cli)

Эта конечная точка WinRM также требуется для запуска набора инструментов сертификации при подключении виртуальной машины как описано в статье [Сертификация образа виртуальной машины](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-certify-vm).

Удаленное управление же виртуальными машинами Linux, напротив, осуществляется с помощью [Azure CLI](https://docs.microsoft.com/cli/azure) или команд Linux из консоли SSH.  В Azure также предусмотрено несколько альтернативных методов для [выполнения скриптов в виртуальной машине Linux](https://docs.microsoft.com/azure/virtual-machines/linux/run-scripts-in-vm).  В случае более сложных сценариев для виртуальных машин на платформе Windows или Linux существует ряд решений для автоматизации и интеграции.


## <a name="configure-and-deploy-with-winrm"></a>Настройка и развертывание с WinRM

Конечную точку WinRM для виртуальной машины на платформе Windows можно настроить на двух отдельных этапах разработки.

- Во время создания — при развертывании виртуальной машины на существующем виртуальном жестком диске.  Для новых предложений такой подход предпочтительнее.  Этот подход требует создания сертификата Azure с помощью предоставленных шаблонов Azure Resource Manager и выполнения пользовательских скриптов PowerShell.
- После развертывания — на имеющейся виртуальной машине, размещенной в Azure.  Этот подход используется в том случае, если у вас уже есть развернутое в Azure решение (виртуальная машина), для которого необходимо включить удаленное управление.  Этот подход требует внесения изменений вручную на портале Azure и выполнения скрипта на целевой виртуальной машине.


## <a name="next-steps"></a>Дальнейшие действия
Если вы создаете новую виртуальную машину, вы можете включить WinRM во время [развертывания виртуальной машины с виртуального жесткого диска](./cpp-deploy-vm-vhd.md).  В противном случае можно включить WinRM на имеющейся виртуальной машине.
