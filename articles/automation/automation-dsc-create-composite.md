---
title: Преобразование конфигураций в составные ресурсы для конфигурации состояния — служба автоматизации Azure
description: Узнайте, как преобразовать конфигурации в составные ресурсы для конфигурации состояния в службе автоматизации Azure.
keywords: dsc,powershell,конфигурация,установка
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: a39b038d31d1b4a614ff0acf7df2586706bb0404
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80585513"
---
# <a name="convert-configurations-to-composite-resources"></a>Преобразование конфигураций в составные ресурсы

> Область применения: Windows PowerShell 5,1

После начала создания конфигураций можно быстро создать сценарии, которые управляют группами параметров.
Пример будет следующим:

- Создание веб-сервера
- Создание DNS-сервера
- Создание сервера SharePoint
- Настройка кластера SQL
- Управление параметрами брандмауэра
- Управление параметрами паролей

Если вы заинтересованы в совместном использовании этой работы с другими пользователями, лучшим вариантом является упаковка конфигурации как [составного ресурса](/powershell/scripting/dsc/resources/authoringresourcecomposite).
Создание составных ресурсов в первый раз может быть огромным.

> [!NOTE]
> В этой статье рассматривается решение, поддерживаемое сообществом с открытым исходным кодом.
> Поддержка доступна только в форме службы совместной работы GitHub, а не корпорацией Майкрософт.

## <a name="community-project-compositeresource"></a>Проект сообщества: Компоситересаурце

Для решения этой проблемы было создано решение, поддерживаемое сообществом с именем [компоситересаурце](https://github.com/microsoft/compositeresource) .

Компоситересаурце автоматизирует процесс создания нового модуля из конфигурации.
Сначала с помощью [Dot](https://blogs.technet.microsoft.com/heyscriptingguy/2010/08/10/how-to-reuse-windows-powershell-functions-in-scripts/) настраивается сценарий конфигурации на рабочей станции (или на сервере сборки), поэтому он загружается в память.
Далее, вместо того чтобы запускать конфигурацию для создания MOF-файла, используйте функцию, предоставляемую модулем Компоситересаурце, для автоматизации преобразования.
Командлет загрузит содержимое конфигурации, получите список параметров и создаст новый модуль со всеми необходимыми данными.

После создания модуля можно увеличить его версию и добавить заметки о выпуске каждый раз при внесении изменений и публикации в собственном [репозитории PowerShellGet](https://powershellexplained.com/2018-03-03-Powershell-Using-a-NuGet-server-for-a-PSRepository/?utm_source=blog&utm_medium=blog&utm_content=psscriptrepo).

После создания составного модуля ресурсов, содержащего конфигурацию (или несколько конфигураций), вы можете использовать их в [интерфейсе разработки](/azure/automation/compose-configurationwithcompositeresources) с поддержкой наборов символов в Azure или добавить их в [скрипты конфигурации DSC](/powershell/scripting/dsc/configurations/configurations) для создания MOF-файлов и [передачи MOF-файлов в службу автоматизации Azure](/azure/automation/tutorial-configure-servers-desired-state#create-and-upload-a-configuration-to-azure-automation).
Затем зарегистрируйте серверы из [локальной](/azure/automation/automation-dsc-onboarding#onboarding-physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances) среды или [в Azure](/azure/automation/automation-dsc-onboarding#onboarding-azure-vms) для извлечения конфигураций.
Последнее обновление проекта также опубликовало [модули Runbook](https://www.powershellgallery.com/packages?q=DscGallerySamples) для службы автоматизации Azure, чтобы автоматизировать процесс импорта конфигураций из коллекция PowerShell.

Чтобы испытать автоматизацию создания составных ресурсов для DSC, посетите [коллекция PowerShell](https://www.powershellgallery.com/packages/compositeresource/) и скачайте решение или щелкните "сайт проекта", чтобы просмотреть [документацию](https://github.com/microsoft/compositeresource).

## <a name="next-steps"></a>Дальнейшие шаги

- [Общие сведения о службе настройки требуемого состояния Windows PowerShell](/powershell/scripting/dsc/overview/overview)
- [Ресурсы DSC](/powershell/scripting/dsc/resources/resources)
- [Настройка локального Configuration Manager](/powershell/scripting/dsc/managing-nodes/metaconfig)
