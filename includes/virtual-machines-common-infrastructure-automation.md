---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/11/2019
ms.author: cynthn
ms.openlocfilehash: 90ccc89f80da7c2abce324c077f0fb1a436b8d44
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72171036"
---
# <a name="use-infrastructure-automation-tools-with-virtual-machines-in-azure"></a>Использование средств автоматизации инфраструктуры на виртуальных машинах в Azure
Средства автоматизации, как правило, позволяют быстро и согласованно создавать виртуальные машины Azure в нужном масштабе, а также управлять ими. Существует множество средств и решений, которые помогают полностью автоматизировать жизненный цикл развертывания и администрирования инфраструктуры Azure. В этой статье приведены сведения о некоторых средствах автоматизации инфраструктуры Azure. Обычно их используют с такими целями:

- Автоматизация настройки виртуальных машин:
    - [Ansible](#ansible), [Chef](#chef) и [Puppet](#puppet);
    - средства для настройки определенных виртуальных машин, например [cloud-init](#cloud-init) для виртуальных машин Linux, [PowerShell Desired State Configuration (DSC)](#powershell-dsc) и [расширение пользовательских скриптов Azure](#azure-custom-script-extension) для всех виртуальных машин Azure.
 
- Автоматизация управления инфраструктурой:
    - средства автоматизации сборки настраиваемых образов виртуальных машин, например [Packer](#packer), и инфраструктуры, например [Terraform](#terraform);
    - [служба автоматизации Azure](#azure-automation), которая может выполнять действия в инфраструктуре Azure и локальной среде.

- Автоматизация развертывания и доставки приложений:
    - [Azure DevOps Services](#azure-devops-services) и [Jenkins](#jenkins).

## <a name="ansible"></a>Ansible
[Ansible](https://www.ansible.com/) — это модуль автоматизации управления конфигурацией, создания виртуальных машин и развертывания приложений. Это средство выполняет проверку подлинности целевых машин и управление ими на основе модели без агентов (обычно с помощью ключей SSH). Задачи конфигурации и количество модулей Ansible для их выполнения определены в сборниках схем. Дополнительные сведения см. в статье [How Ansible works](https://www.ansible.com/how-ansible-works) (Принципы работы Ansible).

Вы узнаете, как выполнять следующие задачи:

- [Установка и настройка Ansible для управления виртуальными машинами в Azure](../articles/virtual-machines/linux/ansible-install-configure.md)
- [Создание виртуальной машины Linux](../articles/virtual-machines/linux/ansible-create-vm.md)
- [Управление виртуальной машиной Linux](../articles/virtual-machines/linux/ansible-manage-linux-vm.md)


## <a name="chef"></a>Chef
[Chef](https://www.chef.io/) — это платформа автоматизации, которая помогает определить параметры настройки и развертывания инфраструктуры, а также управления ею. Дополнительный компонент Chef Habitat позволяет автоматизировать управление жизненным циклом приложения (не инфраструктуры), а Chef InSpec — обеспечение соответствия требованиям политик и безопасности. Клиенты Chef установлены на целевых виртуальных машинах, а на одном или нескольких центральных серверах Chef хранится конфигурация и осуществляется управление ею. Дополнительные сведения см. в статье [An Overview of Chef](https://docs.chef.io/chef_overview.html) (Общие сведения о Chef).

Вы узнаете, как выполнять следующие задачи:

- [Build, deploy, and manage with Chef Automate, the Continuous Automation Platform](https://azuremarketplace.microsoft.com/marketplace/apps/chef-software.chef-automate?tab=Overview) (Создание, развертывание и управление с помощью платформы непрерывной автоматизации Chef Automate)
- [Автоматизация развертывания виртуальной машины Azure с помощью Chef](../articles/virtual-machines/windows/chef-automation.md)


## <a name="puppet"></a>Puppet
[Puppet](https://www.puppet.com) — это платформа автоматизации корпоративного уровня, обрабатывающая доставку и развертывание приложений. На целевых виртуальных машинах установлены агенты, которые позволяют главному серверу Puppet выполнять манифесты, определяющие требуемую конфигурацию инфраструктуры и виртуальных машин Azure. Puppet можно интегрировать с другими решениями, такими как Jenkins и GitHub, что позволяет улучшить рабочие процессы DevOps. Дополнительные сведения см. в статье [How Puppet works](https://puppet.com/products/how-puppet-works) (Принципы работы Puppet).

Вы узнаете, как выполнять следующие задачи:

- [Puppet Enterprise lets you automate the entire lifecycle of your Azure infrastructure](https://azuremarketplace.microsoft.com/marketplace/apps/puppet.puppet-enterprise-2017-2?tab=Overview) (Автоматизация жизненного цикла инфраструктуры Azure с помощью Puppet Enterprise)


## <a name="cloud-init"></a>Cloud-init
[Пакет cloud-init](https://cloudinit.readthedocs.io) — широко используемое средство, используемое для настройки виртуальной машины Linux при ее первой загрузке. Вы можете использовать cloud-init для установки пакетов, записи файлов или настройки пользователей и параметров безопасности. Так как cloud-init вызывается при начальной загрузке, к вашей конфигурации не нужно применять какие-либо дополнительные действия или агентов.  Дополнительные сведения о том, как правильно отформатировать файлы, `#cloud-config`, см. на [сайте документации по cloud-init](https://cloudinit.readthedocs.io/en/latest/topics/format.html#cloud-config-data).  Файлы `#cloud-config` — это текстовые файлы, закодированные в формате base64.

Кроме того, cloud-init работает с разными дистрибутивами. Например, для установки пакета не используется **apt-get install** или **yum install**. Вместо этого можно определить список пакетов для установки. Файл cloud-init автоматически использует собственный инструмент управления пакетами из выбранного дистрибутива.

Мы активно сотрудничаем с нашими утвержденными партнерами, работающими над дистрибутивами Linux, чтобы образы с поддержкой cloud-init стали доступными в Azure Marketplace. Эти образы обеспечивают бесперебойную работу развертываний и конфигураций cloud-init с виртуальными машинами и масштабируемыми наборами виртуальных машин (VMSS). Дополнительные сведения о cloud-init в Azure:

- [Поддержка cloud-init для виртуальных машин Linux в Azure](../articles/virtual-machines/linux/using-cloud-init.md)
- [Руководство по автоматизации настройки виртуальных машин при помощи cloud-init](../articles/virtual-machines/linux/tutorial-automate-vm-deployment.md)


## <a name="powershell-dsc"></a>PowerShell DSC
[PowerShell Desired State Configuration](/powershell/scripting/dsc/overview/overview) — это платформа управления, которая позволяет определить конфигурацию целевых виртуальных машин. DSC также можно использовать в Linux через [сервер Open Management Infrastructure](https://collaboration.opengroup.org/omi/).

Конфигурации DSC определяют компоненты, которые следует установить на виртуальной машине, а также параметры настройки узла. На каждом целевом узле, который обрабатывает запрашиваемые действия на основе отправленных конфигураций, выполняется модуль локального диспетчера конфигураций (LCM). Опрашивающий сервер — это веб-служба, запущенная на центральном узле lkz хранения конфигурации DSC и связанных ресурсов. Этот сервер взаимодействует с модулем LCM на каждом целевом узле и тем самым предоставляет требуемую конфигурацию и отчеты о соответствии.

Вы узнаете, как выполнять следующие задачи:

- [Краткое руководство по настройке требуемого состояния](https://msdn.microsoft.com/powershell/dsc/quickstarts/website-quickstart)
- [Настройка опрашивающего веб-сервера DSC](/powershell/scripting/dsc/pull-server/pullserver)
- [Начало работы с настройкой требуемого состояния (DSC) для Linux](/powershell/scripting/dsc/getting-started/lnxgettingstarted)


## <a name="azure-custom-script-extension"></a>Расширение пользовательских скриптов Azure
Расширение пользовательских скриптов Azure для [Linux](../articles/virtual-machines/linux/extensions-customscript.md) или [Windows](../articles/virtual-machines/windows/extensions-customscript.md) скачивает и выполняет скрипты на виртуальных машинах Azure. Это расширение можно использовать при создании виртуальной машины или с любой другой целью после ее создания. 

Скрипты можно скачать из хранилища Azure или любого общедоступного расположения, например репозитория GitHub. Расширение пользовательских скриптов позволяет создавать скрипты на любом языке, выполняющемся на исходной виртуальной машине. С помощью этих скриптов вы можете устанавливать приложения или нужным образом настраивать виртуальную машину. Чтобы защитить учетные данные, конфиденциальные сведения, например пароли, можно хранить в защищенной конфигурации. Эти учетные данные расшифровываются только на виртуальной машине.

Вы узнаете, как выполнять следующие задачи:

- [Создание виртуальной машины с помощью NGINX](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fcli%2fazure%2ftoc.json)
- [Создание виртуальной машины IIS с помощью PowerShell](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fpowershell%2fmodule%2ftoc.json)


## <a name="packer"></a>Packer
[Packer](https://www.packer.io) автоматизирует процесс сборки при создании настраиваемого образа виртуальной машины Azure. Это средство позволяет определить операционную систему и выполнить скрипты настройки виртуальной машины, выполняемые после задания конфигурации. После настройки виртуальная машина записывается как образ управляемого диска. Packer автоматизирует создание ресурсов исходной виртуальной машины, сети и хранилища, выполнение скриптов конфигурации и создание образа виртуальной машины.

Вы узнаете, как выполнять следующие задачи:

- [Создание образов виртуальных машин Linux в Azure с помощью Packer](../articles/virtual-machines/linux/build-image-with-packer.md)
- [Использование Packer для создания образов виртуальных машин Windows в Azure](../articles/virtual-machines/windows/build-image-with-packer.md)


## <a name="terraform"></a>Terraform
[Terraform](https://www.terraform.io) — это средство автоматизации, которое позволяет определить и создать целую инфраструктуру Azure на языке HashiCorp Configuration Language (HCL). Это средство помогает определить шаблоны автоматизации создания ресурсов виртуальной машины, хранилища и сети определенного приложения. Имеющиеся шаблоны Terraform можно использовать на других платформах с Azure. Это позволяет обеспечить согласованность и упростить развертывание инфраструктуры, не переходя на шаблон Azure Resource Manager.

Вы узнаете, как выполнять следующие задачи:

- [Установка и настройка Terraform для подготовки виртуальных машин и другой инфраструктуры в Azure](../articles/virtual-machines/linux/terraform-install-configure.md)
- [Создание базовой инфраструктуры в Azure с помощью Terraform](../articles/virtual-machines/linux/terraform-create-complete-vm.md)


## <a name="azure-automation"></a>Служба автоматизации Azure
[Служба автоматизации Azure](https://azure.microsoft.com/services/automation/) обрабатывает набор задач, выполняемых на целевых виртуальны машинах, с помощью модулей runbook. Эта служба позволяет управлять имеющимися виртуальными машинами. С ее помощью вы не сможете создать инфраструктуру. Службу автоматизации Azure можно запустить на виртуальных машинах Windows и Linux, а также на локальных виртуальных или физических виртуальных машинах с помощью гибридной рабочей роли Runbook. Модули runbook можно хранить в репозитории системы управления версиями, например GitHub. Эти модули можно выполнять вручную или по заданному расписанию.

Служба автоматизации Azure также предоставляет службу DSC, которая позволяет создавать определения настройки заданного набора виртуальных машин. DSC гарантирует применение необходимой конфигурации и обеспечивает согласованность виртуальной машины. Azure Automation DSC можно запустить на виртуальных машинах Windows и Linux.

Вы узнаете, как выполнять следующие задачи:

- [Мой первый модуль Runbook PowerShell](../articles/automation/automation-first-runbook-textual-powershell.md)
- [Использование гибридной рабочей роли Runbook для управления локальными ресурсами](../articles/automation/automation-hybrid-runbook-worker.md).
- [Приступая к работе с DSC службы автоматизации Azure](../articles/automation/automation-dsc-getting-started.md)


## <a name="azure-devops-services"></a>Azure DevOps Services
[Azure DevOps Services](https://www.visualstudio.com/team-services/) — это набор средств, позволяющих отслеживать код и предоставлять к нему общий доступ, использовать автоматизированные сборки и создавать конвейеры непрерывной интеграции и разработки (CI/CD). Azure DevOps Services интегрируется с Visual Studio и другими редакторами, что упрощает его использование. Кроме того, этот набор средств позволяет создавать и настраивать виртуальные машины Azure, а затем развертывать код на них.

См. также:

- [Azure DevOps Services](https://docs.microsoft.com/azure/devops/user-guide/index?view=vsts).


## <a name="jenkins"></a>Jenkins
[Jenkins](https://www.jenkins.io) — это сервер непрерывной интеграции, который помогает развертывать и тестировать приложения, а также создать автоматизированные конвейеры доставки кода. Базовую платформу Jenkins можно расширить с помощью большого количества подключаемых модулей, а веб-перехватчики позволяют интегрировать ее с другими продуктами. Вы можете вручную установить Jenkins на виртуальной машине Azure, запустить Jenkins в контейнере Docker или использовать готовый образ Azure Marketplace.

Вы узнаете, как выполнять следующие задачи:

- [Как создать инфраструктуру непрерывной интеграции и непрерывного развертывания на виртуальной машине Linux в Azure с помощью Jenkins, GitHub и Docker](../articles/virtual-machines/linux/tutorial-jenkins-github-docker-cicd.md)


## <a name="next-steps"></a>Следующие шаги
Средства автоматизации инфраструктуры в Azure позволяют выполнять разные задачи. Вы можете использовать решения, которые максимально отвечают вашим потребностям и требованиям среды. Чтобы приступить к работе и оценить несколько стандартных средств Azure, ознакомьтесь с руководством по автоматизации настройки виртуальной машины [Linux](../articles/virtual-machines/linux/tutorial-automate-vm-deployment.md) или [Windows](../articles/virtual-machines/windows/tutorial-automate-vm-deployment.md).
