---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: d14cfb82ae74f85425dbd3e8a365e8b99969641d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61485194"
---
## <a name="using-vm-extensions"></a>Использование расширений виртуальной машины
Расширения VM Azure реализуют реакции на события или функции, которые помогают другим программам работать на виртуальных машинах Azure (например, расширение **WebDeployForVSDevTest** позволяет Visual Studio использовать веб-развертывание решений на VM Azure) или предоставляют возможность взаимодействия с виртуальной машиной для поддержки другой реакции на события (например, вы можете использовать расширения доступа к виртуальной машине через клиенты Powershell, Azure CLI или REST для сброса или изменения значений удаленного доступа на VM Azure).

> [!IMPORTANT]
> Полный список расширений с разбивкой по поддерживаемым ими функциям см. в статье [Обзор расширений и компонентов виртуальной машины](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Так как каждое расширение виртуальной машины поддерживает конкретную функцию, возможности и ограничения каждого расширения индивидуальны. Поэтому перед любым изменением ВМ вам следует обязательно прочитать документацию по конкретному расширению ВМ, которое вы собираетесь использовать. Одни расширения ВМ нельзя удалить, а другие имеют такие свойства, изменение которых радикальным образом меняет реакцию ВМ на события.
> 
> 

Ниже перечислены наиболее распространенные задачи:

1. Поиск доступных расширений.
2. Обновление загруженных расширений.
3. Добавление расширений.
4. Удаление расширений.

## <a name="find-available-extensions"></a>Поиск доступных расширений
Вы можете найти расширения и дополнительную информацию, используя:

* PowerShell
* Использование кросс-платформенного интерфейса командной строки Azure (Azure CLI)
* Интерфейс API REST управления службой

### <a name="azure-powershell"></a>Azure PowerShell
Некоторые расширения имеют собственные командлеты Powershell, что позволяет упростить их настройку через PowerShell. Следующие командлеты будут работать для всех расширений виртуальных машин.

Для получения сведений о доступных расширениях вы можете использовать следующие командлеты:

* Для экземпляров веб-ролей или рабочих ролей можно использовать командлет [Get AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) .
* Для экземпляров виртуальных машин можно использовать командлет [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) .
  
   Например, в следующем примере кода показано, как с помощью PowerShell отобразить информацию о расширении **IaaSDiagnostics** .
  
      PS C:\> Get-AzureVMAvailableExtension -ExtensionName IaaSDiagnostics
  
      Publisher                   : Microsoft.Azure.Diagnostics
      ExtensionName               : IaaSDiagnostics
      Version                     : 1.2
      Label                       : Microsoft Monitoring Agent Diagnostics
      Description                 : Microsoft Monitoring Agent Extension
      PublicConfigurationSchema   :
      PrivateConfigurationSchema  :
      IsInternalExtension         : False
      SampleConfig                :
      ReplicationCompleted        : True
      Eula                        :
      PrivacyUri                  :
      HomepageUri                 :
      IsJsonExtension             : True
      DisallowMajorVersionUpgrade : False
      SupportedOS                 :
      PublishedDate               :
      CompanyName                 :

### <a name="azure-command-line-interface-azure-cli"></a>Интерфейс командной строки Azure (Azure CLI)
Некоторые расширения имеют собственные команды Azure CLI (например, расширение VM Docker), что упрощает их настройку. Указанные ниже команды поддерживаются всеми расширениями VM.

Вы можете использовать команду **azure vm extension list** для получения информации о доступных расширениях, а также применить параметр **–-json** для отображения всех доступных сведений об одном или нескольких расширениях. Если не указывать в команде имя расширения, будет выдано описание JSON для всех доступных расширений.

Например, в следующем примере кода показано, как с помощью команды **azure vm extension list** интерфейса командной строки Azure отобразить сведения о расширении **IaaSDiagnostics** и использовать параметр **–-json** для получения полной информации.

    $ azure vm extension list -n IaaSDiagnostics --json
    [
      {
        "publisher": "Microsoft.Azure.Diagnostics",
        "name": "IaaSDiagnostics",
        "version": "1.2",
        "label": "Microsoft Monitoring Agent Diagnostics",
        "description": "Microsoft Monitoring Agent Extension",
        "replicationCompleted": true,
        "isJsonExtension": true
      }
    ]



### <a name="service-management-rest-apis"></a>API-интерфейсы REST управления службами
Для получения сведений о доступных расширениях вы можете использовать следующие REST API:

* Для экземпляров веб-ролей или рабочих ролей можно использовать операцию [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx) . Чтобы вывести список доступных расширений, можно использовать операцию [List Extension Versions](https://msdn.microsoft.com/library/dn495437.aspx).
* Для экземпляров виртуальных машин можно использовать операцию [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx) . Чтобы вывести список версий доступных расширений, можно использовать операцию [List Resource Extension Versions](https://msdn.microsoft.com/library/dn495440.aspx).

## <a name="add-update-or-disable-extensions"></a>Добавление, обновление или отключение расширений
Вы можете добавить расширения во время создания экземпляра или добавить их позднее к запущенному экземпляру. Расширения можно обновить, отключить или удалить. Эти действия вы можете выполнить с помощью командлетов Azure PowerShell или с помощью операций REST API управления службами. Для установки и настройки некоторых расширений используются определенные параметры. Для расширений поддерживаются общедоступные и частные параметры.

### <a name="azure-powershell"></a>Azure PowerShell
Самым простым способом добавления и обновления расширений является использование командлетов Azure PowerShell. При использовании командлетов расширения они выполнят за вас основную часть работы по настройке расширения. В некоторых случаях может потребоваться добавить расширение программно. В таких случаях вам нужно будет предоставить конфигурацию расширения.

Чтобы выяснить, требуется ли настройка общедоступных и частных параметров для определенного расширения, вы можете использовать следующие командлеты:

* Для экземпляров веб-ролей или рабочих ролей можно использовать командлет **Get AzureServiceAvailableExtension** .
* Для экземпляров виртуальных машин можно использовать командлет **Get-AzureVMAvailableExtension** .

### <a name="service-management-rest-apis"></a>REST API управления службой
При запросе списка доступных расширений через интерфейсы REST API вы получите информацию о том, как следует настраивать каждое расширение. Возвращаемая информация может содержать сведения о параметрах в виде общедоступной схемы и частной схемы. Значения общедоступных параметров возвращаются в запросах об экземплярах. Значения частных параметров не возвращаются.

Чтобы выяснить, требуется ли настройка общедоступных и частных параметров для определенного расширения, вы можете использовать следующие REST API:

* Для экземпляров веб-ролей или рабочих ролей элементы **PublicConfigurationSchema** и **PrivateConfigurationSchema** содержат информацию, полученную в ответе на операцию [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx).
* Для экземпляров виртуальных машин элементы **PublicConfigurationSchema** и **PrivateConfigurationSchema** содержат информацию, полученную в ответе на операцию [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx).

> [!NOTE]
> Кроме того, расширения могут использовать конфигурации, заданные с помощью JSON. При использовании расширений такого типа применяется только элемент **SampleConfig** .
> 
> 

