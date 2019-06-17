---
title: Настройка решения "Сопоставление служб" в Azure | Документация Майкрософт
description: "\"Сопоставление служб\" — это решение Azure, которое автоматически обнаруживает компоненты приложений в системах Windows и Linux и сопоставляет взаимодействие между службами. В этой статье содержатся подробные сведения о развертывании схемы услуги в вашей среде и приведены разнообразные сценарии ее использования."
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: d3d66b45-9874-4aad-9c00-124734944b2e
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2019
ms.author: magoedte
ms.openlocfilehash: a50f080ee4d72ad098ca87f5b3a62b42f8cd5907
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65142736"
---
# <a name="configure-service-map-in-azure"></a>Настройка решения "Сопоставление служб" в Azure

Служба схемы услуги автоматически обнаруживает компоненты приложений в системах Windows и Linux и сопоставляет взаимодействие между службами. Она позволяет рассматривать серверы как взаимосвязанные системы, предоставляющие важные службы. Схема услуги отображает сведения о подключениях между серверами, процессами и портами в любой подключенной по протоколу TCP архитектуре без дополнительной настройки. Пользователям требуется только установить агент.

В этой статье подробно описывается настройка схемы услуги и подключение агентов. Сведения об использовании решения "Сопоставление служб" см. в статье [Использование решения схемы услуги в Operations Management Suite]( service-map.md).

## <a name="supported-azure-regions"></a>Поддерживаемые регионы Azure

Схема услуги в настоящее время доступна в следующих регионах Azure:
- Восточная часть США
- Западно-центральная часть США
- Центральная Канада
- Южная часть Великобритании
- Западная Европа
- Юго-Восточная Азия

## <a name="supported-windows-operating-systems"></a>Поддерживаемые операционные системы Windows

В следующих разделах представлены поддерживаемые операционные системы для Dependency Agent в Windows. 

>[!NOTE]
>Схема услуги поддерживает только 64-разрядные платформы.
>

### <a name="windows-server"></a>Windows Server

- Windows Server 2019
- Windows Server 2016 1803
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 с пакетом обновления 1

### <a name="windows-desktop"></a>Классические приложения Windows

- Windows 10 1803
- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

## <a name="supported-linux-operating-systems"></a>Поддерживаемые операционные системы Linux

В разделе ниже перечислены поддерживаемые операционные системы для агента зависимостей в Linux.  

- Поддерживаются только версии ядра по умолчанию и SMP для Linux.
- Нестандартные версии ядра, такие как PAE и Xen, не поддерживаются ни в одном дистрибутиве Linux. Например, система со строкой версии "2.6.16.21-0.8-xen" не поддерживается.
- Пользовательские ядра, включая повторные компиляции стандартных ядер, не поддерживаются.

### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| Версия ОС | Версия ядра |
|:--|:--|
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |
| 7.6 | 3.10.0-957 |

### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| Версия ОС | Версия ядра |
|:--|:--|
| 6.9 | 2.6.32-696 |
| 6.10 | 2.6.32-754 |

### <a name="centosplus"></a>CentOSPlus

| Версия ОС | Версия ядра |
|:--|:--|
| 6.9 | 2.6.32-696.18.7<br>2.6.32-696.30.1 |
| 6.10 | 2.6.32-696.30.1<br>2.6.32-754.3.5 |

### <a name="ubuntu-server"></a>Сервер Ubuntu

| Версия ОС | Версия ядра |
|:--|:--|
| Ubuntu 18.04 | ядро 4.15.\*<br>4.18* |
| Ubuntu 16.04.3 | ядро 4.15.* |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

### <a name="suse-linux-11-enterprise-server"></a>SUSE Linux 11 Enterprise Server

| Версия ОС | Версия ядра
|:--|:--|
| 11 SP4 | 3.0.* |

### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 Enterprise Server

| Версия ОС | Версия ядра
|:--|:--|
| 12 с пакетом обновления 2 | 4.4.* |
| 12 с пакетом обновления 3 | 4.4.* |

## <a name="dependency-agent-downloads"></a>Скачиваемые файлы Dependency Agent

| Файл | ОС | Version (версия) | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.8.1 | 622C99924385CBF539988D759BCFDC9146BB157E7D577C997CDD2674E27E08DD |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.8.1 | 3037934A5D3FB7911D5840A9744AE9F980F87F620A7F7B407F05E276FE7AE4A8 |

## <a name="connected-sources"></a>Подключенные источники

Решение "Сопоставление служб" получает данные от Microsoft Dependency Agent. Dependency Agent является зависимым от агента Log Analytics ввиду подключений к Log Analytics. Это означает, что сначала на сервере нужно установить и настроить агент Log Analytics, после чего можно будет установить Dependency Agent.  В приведенной ниже таблице описаны подключенные источники, которые поддерживаются решением схемы услуги.

| Подключенный источник | Поддерживаются | Описание |
|:--|:--|:--|
| Агенты Windows | Да | Решение "Сопоставление служб" анализирует и собирает данные с компьютеров Windows. <br><br>Кроме [агента Log Analytics для Windows](../../azure-monitor/platform/log-analytics-agent.md) агентам Windows требуется Microsoft Dependency Agent. Полный список версий операционных систем см. в разделе, посвященном поддерживаемым операционным системам. |
| Агенты Linux | Да | Решение "Сопоставление служб" анализирует и собирает данные с компьютеров Linux. <br><br>Кроме [агента Log Analytics для Linux](../../azure-monitor/platform/log-analytics-agent.md) агентам Linux требуется Microsoft Dependency Agent. Полный список версий операционных систем см. в разделе, посвященном поддерживаемым операционным системам. |
| Группа управления System Center Operations Manager | Да | Служба схемы услуги анализирует и собирает данные из агентов Windows и Linux в подключенной [группе управления System Center Operations Manager](../../azure-monitor/platform/om-agents.md). <br><br>Требуется прямое подключение из агента System Center Operations Manager к Log Analytics. |
| Учетная запись хранения Azure. | Нет | Служба схемы услуги собирает данные с компьютеров с агентом, поэтому данные из службы хранилища Azure не собираются. |

В ОС Windows System Center Operations Manager и Log Analytics используют Microsoft Monitoring Agent для сбора и отправки данных мониторинга. (В зависимости от контекста этот агент может называться агентом System Center Operations Manager, агентом Log Analytics, агентом MMA или прямым агентом.) System Center Operations Manager и Log Analytics предоставляют разные версии MMA. В каждой из этих версий предусмотрена возможность отправлять отчеты в System Center Operations Manager, Log Analytics или в оба решения.  

В ОС Linux агент Log Analytics для Linux собирает и отправляет данные мониторинга в Log Analytics. Решение "Сопоставление служб" можно использовать на серверах с агентами Log Analytics, подключенными непосредственно к службе, или на серверах, которые передают данные группе управления Operations Manager, интегрированной с Log Analytics.  

В этой статье мы будем называть все эти агенты (как в Linux, так и в Windows), подключенные к группе управления System Center Operations Manager или непосредственно к Log Analytics, — *агентами Log Analytics*. 

Агент схемы услуги самостоятельно не передает данные и не требует внесения изменений в брандмауэры или порты. В решении "Сопоставление служб" данные всегда передаются агентом Log Analytics в службу Log Analytics напрямую или через шлюз Log Analytics.

![Агенты схемы услуги](media/service-map-configure/agents.png)

Если вы являетесь клиентом System Center Operations Manager с группой управления, подключенной к Log Analytics:

- Если у ваших агентов System Center Operations Manager есть доступ к Log Analytics через Интернет, никаких дополнительных настроек не требуется.  
- Если у агентов System Center Operations Manager нет доступа к Log Analytics через Интернет, нужно настроить шлюз Log Analytics для работы с System Center Operations Manager.
  
Если компьютеры Windows или Linux не могут напрямую подключиться к службе, нужно настроить агент Log Analytics таким образом, чтобы он подключался к рабочей области Log Analytics через шлюз. Дополнительные сведения о развертывании и настройке шлюза Log Analytics см. в статье [Подключения компьютеров с помощью шлюза OMS без доступа к Интернету](../../azure-monitor/platform/gateway.md).  

### <a name="management-packs"></a>Пакеты управления

Если решение "Сопоставление служб" активировано в рабочей области Log Analytics, то на все серверы в этой рабочей области перенаправляется пакет управления размером 300 КБ. Если агенты System Center Operations Manager используются в [подключенной группе управления](../../azure-monitor/platform/om-agents.md), то пакет управления схемы услуги развертывается из System Center Operations Manager. 

Пакет управления называется Microsoft.IntelligencePacks.ApplicationDependencyMonitor. Он записывается в папку %Programfiles%\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs\. Источник данных, используемый пакетом управления, — %Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources\<Автоматически_созданный_идентификатор>\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="data-collection"></a>Сбор данных

Каждый агент передает примерно 25 МБ данных в день в зависимости от сложности зависимостей системы. Каждый агент отправляет данные зависимостей схемы услуги каждые 15 секунд.  

Dependency Agent обычно потребляет 0,1 % системной памяти и 0,1 % ресурсов ЦП системы.

## <a name="diagnostic-and-usage-data"></a>Данные диагностики и использования

Корпорация Майкрософт автоматически собирает данные об использовании и производительности с помощью службы схемы услуги. Эти данные используются, чтобы улучшить качество, повысить безопасность и целостность службы схемы услуги. Они включают в себя сведения о конфигурации программного обеспечения, такие как версия операционной системы. Они также включают в себя IP-адрес, DNS-имя и имя рабочей станции с целью предоставления возможностей для точного и эффективного устранения неполадок. Корпорация Майкрософт не собирает сведения об именах, адресах и другую контактную информацию.

Дополнительные сведения о сборе и использовании данных см. в [заявлении о конфиденциальности служб Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).

## <a name="installation"></a>Установка

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

### <a name="azure-vm-extension"></a>Расширение виртуальной машины Azure

Для Windows (DependencyAgentWindows) и Linux (DependencyAgentLinux) доступно расширение, а Dependency Agent можно легко развернуть на виртуальных машинах Azure с использованием [расширения виртуальной машины Azure](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-features).  Расширение виртуальной машины Azure позволяет развернуть Dependency Agent на виртуальных машинах Windows и Linux с помощью скрипта PowerShell или напрямую в виртуальной машине с использованием шаблона виртуальной машины Azure Resource Manager.  При развертывании агента с помощью расширения виртуальной машины Azure агенты автоматически обновляются до последних версий.

Чтобы развернуть расширение виртуальной машины Azure с помощью PowerShell, можно использовать приведенный ниже пример.

```powershell
#
# Deploy the Dependency agent to every VM in a Resource Group
#

$version = "9.4"
$ExtPublisher = "Microsoft.Azure.Monitoring.DependencyAgent"
$OsExtensionMap = @{ "Windows" = "DependencyAgentWindows"; "Linux" = "DependencyAgentLinux" }
$rmgroup = "<Your Resource Group Here>"

Get-AzVM -ResourceGroupName $rmgroup |
ForEach-Object {
    ""
    $name = $_.Name
    $os = $_.StorageProfile.OsDisk.OsType
    $location = $_.Location
    $vmRmGroup = $_.ResourceGroupName
    "${name}: ${os} (${location})"
    Date -Format o
    $ext = $OsExtensionMap.($os.ToString())
    $result = Set-AzVMExtension -ResourceGroupName $vmRmGroup -VMName $name -Location $location `
    -Publisher $ExtPublisher -ExtensionType $ext -Name "DependencyAgent" -TypeHandlerVersion $version
    $result.IsSuccessStatusCode
}
```

Более простой способ обеспечить установку Dependency Agent на виртуальных машинах — добавить этот агент в шаблон Azure Resource Manager.  Приведенный ниже пример кода JSON можно добавить в раздел *resources* шаблона.

```JSON
"type": "Microsoft.Compute/virtualMachines/extensions",
"name": "[concat(parameters('vmName'), '/DependencyAgent')]",
"apiVersion": "2017-03-30",
"location": "[resourceGroup().location]",
"dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
],
"properties": {
    "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
    "type": "DependencyAgentWindows",
    "typeHandlerVersion": "9.4",
    "autoUpgradeMinorVersion": true
}

```

### <a name="install-the-dependency-agent-on-microsoft-windows"></a>Установка Dependency Agent в Microsoft Windows

Dependency Agent можно установить вручную на компьютерах Windows, выполнив `InstallDependencyAgent-Windows.exe`. Если запустить этот исполняемый файл без параметров, запустится мастер для установки в интерактивном режиме.  

>[!NOTE]
>Для установки или удаления агента требуются права администратора.

Выполните приведенные шаги, чтобы установить Dependency Agent на каждом компьютере Windows.

1.  Установите агент Log Analytics для Windows [одним из способов](../../azure-monitor/platform/log-analytics-agent.md).
2.  Скачайте агент Windows и запустите его с помощью следующей команды. 
    
    `InstallDependencyAgent-Windows.exe`

3.  Следуйте инструкциям мастера установки, чтобы установить агент.
4.  Если Dependency Agent не запускается, просмотрите подробные сведения об ошибке в записях журналов. В агентах Windows каталогом журналов является %Programfiles%\Microsoft Dependency Agent\logs. 

#### <a name="windows-command-line"></a>Командная строка Windows

Используйте параметры из следующей таблицы для установки из командной строки. Чтобы просмотреть список параметров установки, запустите установщик с параметром /? /? следующим образом.

    InstallDependencyAgent-Windows.exe /?

| Параметр | Описание |
|:--|:--|
| /? | Получает список параметров командной строки. |
| /S | Выполняет автоматическую установку без вывода сообщений для пользователя. |

Файлы для Windows Dependency Agent по умолчанию размещаются в папке C:\Program Files\Microsoft Dependency Agent.

### <a name="install-the-dependency-agent-on-linux"></a>Установка Dependency Agent в Linux

Dependency Agent устанавливается на компьютерах Linux из файла `InstallDependencyAgent-Linux64.bin`, скрипта оболочки с самоизвлекающимся двоичным файлом. Вы можете запустить этот файл с помощью `sh` или добавить разрешения на выполнение в сам файл.

>[!NOTE]
> Для установки или настройки агента требуется доступ с правами привилегированного пользователя.

Выполните приведенные шаги, чтобы установить Dependency Agent на каждом компьютере Linux.

1.  Установите агент Log Analytics [одним из способов](../../azure-monitor/platform/log-analytics-agent.md).
2.  Установите Linux Dependency Agent с правами привилегированного пользователя, выполнив следующую команду:
    
    `sh InstallDependencyAgent-Linux64.bin`

3.  Если Dependency Agent не запускается, просмотрите подробные сведения об ошибке в записях журналов. В агентах Linux каталог журналов находится в расположении /var/opt/microsoft/dependency-agent/log.

Чтобы просмотреть список параметров установки, запустите программу установки с параметров -help указанным ниже образом.

    InstallDependencyAgent-Linux64.bin -help

| Параметр | Описание |
|:--|:--|
| -help | Получение списка параметров командной строки. |
| -s | Выполняет автоматическую установку без вывода сообщений для пользователя. |
| --check | Проверяет разрешения и операционную систему, но не устанавливает агент. |

Файлы для Dependency Agent размещаются в следующих каталогах:

| Файлы | Расположение |
|:--|:--|
| Основные файлы | /opt/microsoft/dependency-agent |
| Файлы журналов | /var/opt/microsoft/dependency-agent/log |
| Файлы конфигурации | /etc/opt/microsoft/dependency-agent/config |
| Исполняемые файлы службы | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Двоичные файлы хранилища | /var/opt/microsoft/dependency-agent/storage |

## <a name="installation-script-examples"></a>Примеры скриптов установки

Чтобы легко развернуть Dependency Agent на нескольких серверах за один раз, воспользуйтесь следующим примером скрипта для загрузки и установки Dependency Agent в ОС Windows или Linux.

### <a name="powershell-script-for-windows"></a>Скрипт PowerShell для Windows

```powershell
Invoke-WebRequest "https://aka.ms/dependencyagentwindows" -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S
```

### <a name="shell-script-for-linux"></a>Скрипт оболочки для Linux

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
sudo sh InstallDependencyAgent-Linux64.bin -s
```

## <a name="desired-state-configuration"></a>Настройка требуемого состояния

Чтобы развернуть Dependency Agent с помощью Desired State Configuration, можно использовать модуль xPSDesiredStateConfiguration со следующим примером кода.

```powershell
configuration ServiceMap {

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    $DAPackageLocalPath = "C:\InstallDependencyAgent-Windows.exe"

    Node localhost
    {
        # Download and install the Dependency agent
        xRemoteFile DAPackage 
        {
            Uri = "https://aka.ms/dependencyagentwindows"
            DestinationPath = $DAPackageLocalPath
        }

        xPackage DA
        {
            Ensure="Present"
            Name = "Dependency Agent"
            Path = $DAPackageLocalPath
            Arguments = '/S'
            ProductId = ""
            InstalledCheckRegKey = "HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent"
            InstalledCheckRegValueName = "DisplayName"
            InstalledCheckRegValueData = "Dependency Agent"
            DependsOn = "[xRemoteFile]DAPackage"
        }
    }
}
```

## <a name="remove-the-dependency-agent"></a>Удаление Dependency Agent

### <a name="uninstall-agent-on-windows"></a>Удаление агента в Windows

Администратор может удалить Dependency Agent для Windows через панель управления.

Для удаления Dependency Agent администратор может также запустить файл %Programfiles%\Microsoft Dependency Agent\Uninstall.exe.

### <a name="uninstall-agent-on-linux"></a>Удаление агента в Linux

Удалить Dependency Agent из среды Linux можно с помощью следующей команды.

RHEL, CentOS или Oracle:

```
sudo rpm -e dependency-agent
```

Ubuntu:

```
sudo apt -y purge dependency-agent
```

## <a name="troubleshooting"></a>Устранение неполадок

Если при установке или запуске схемы услуги возникли проблемы, в этом разделе приводятся сведения, которые могут помочь вам. Если по-прежнему не удается устранить проблему, обратитесь в службу поддержки Майкрософт.

### <a name="dependency-agent-installation-problems"></a>Проблемы при установке Dependency Agent

#### <a name="installer-prompts-for-a-reboot"></a>Установщик запрашивает перезагрузку

При установке или удалении Dependency Agent *обычно* не требует перезагрузки. Однако в некоторых редких случаях для продолжения установки Windows Server требуется перезагрузить. Это происходит, когда зависимость (как правило, распространяемые компоненты Microsoft VC ++) требует перезагрузки из-за заблокированного файла.

#### <a name="message-unable-to-install-dependency-agent-visual-studio-runtime-libraries-failed-to-install-code--codenumber-appears"></a>Поступает сообщение "Unable to install Dependency agent: Visual Studio Runtime libraries failed to install (code = [code_number])" (Не удалось установить Dependency Agent: сбой установки библиотек среды выполнения Visual Studio (код = [номер_кода]))

Microsoft Dependency Agent создан на основе библиотек среды выполнения Microsoft Visual Studio. Если во время установки этих библиотек возникла проблема, вы получите сообщение. 

Установщики библиотек среды выполнения создают журналы в папке %LOCALAPPDATA%\temp. Файл будет иметь вид dd_vcredist_arch_yyyymmddhhmmss.log, где вместо *arch* будет указано x86 или amd64, а вместо *yyyymmddhhmmss* — дата и время создания журнала (в 24-часовом формате). В журнале содержатся подробные сведения о проблеме, из-за которой блокируется установка.

Сначала лучше всего установить [последнюю версию библиотек среды выполнения](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads).

В таблице ниже приведены некоторые номера кодов и рекомендации по устранению проблем.

| Код | Описание | Способы устранения: |
|:--|:--|:--|
| 0x17 | Установщику библиотек требуется обновление Windows, которое не было установлено. | Проверьте сведения в последнем журнале установщика библиотек.<br><br>Если за ссылкой на "Windows8.1-KB2999226-x64.msu" следует строка "Error 0x80240017: Failed to execute MSU package" (Ошибка 0x80240017: не удалось выполнить пакет MSU), значит, что у вас нет необходимых компонентов для установки обновления KB2999226. Следуйте инструкциям в разделе предварительных требований статьи [Обновление для универсальной среды выполнения C в Windows](https://support.microsoft.com/kb/2999226). Может потребоваться запустить Центр обновления Windows и несколько раз выполнить перезагрузку, чтобы установить необходимые компоненты.<br><br>Запустите установщик Microsoft Dependency Agent повторно. |

### <a name="post-installation-issues"></a>Проблемы после установки

#### <a name="server-doesnt-appear-in-service-map"></a>Сервер не отображается в службе схемы услуги

Если Dependency Agent успешно установлен, но в решении "Сопоставление служб" не отображается сервер, ответьте на следующие вопросы:
* Успешно ли установлен Dependency Agent? Для этого проверьте, установлена и запущена ли служба.<br><br>
**Windows**: найдите службу с именем Microsoft Dependency Agent.<br>
**Linux**: найдите выполняющийся процесс microsoft-dependency-agent.

* Используете ли вы [ценовую категорию "Бесплатный" Operations Management Suite и Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions)? План "Бесплатный" предусматривает использование пяти уникальных серверов решения "Схема услуги". Все последующие серверы не будут отображаться в решении "Схема услуги", даже если предыдущие пять больше не отправляют данные.

* Отправляет ли сервер данные журналов и данные о производительности в Log Analytics? Перейдите к поиску по журналам и выполните следующий запрос для своего компьютера: 

    Usage | where Computer == "computer-name" | summarize sum(Quantity), any(QuantityUnit) by DataType

Вы получили множество событий в результатах? Это последние данные? В этом случае агент Log Analytics работает правильно и обменивается данными со службой Log Analytics. Если это не так, проверьте агент на сервере. Дополнительные сведения см. в разделе [об устранении неполадок с агентом Log Analytics для Windows](https://support.microsoft.com/help/3126513/how-to-troubleshoot-monitoring-onboarding-issues) или статье [Устранение неполадок с агентом Log Analytics для Linux](../../azure-monitor/platform/agent-linux-troubleshoot.md).

#### <a name="server-appears-in-service-map-but-has-no-processes"></a>Сервер отображается в решении "Схема услуги", но для него нет процессов

Если сервер отображается в решении "Сопоставление служб", но для него нет процессов или данных о подключении, это означает, что Dependency Agent установлен и запущен, но не удалось загрузить драйвер ядра. 

Проверьте файл C:\Program Files\Microsoft Dependency Agent\logs\wrapper.log (Windows) или /var/opt/microsoft/dependency-agent/log/service.log (Linux). В последних строках файла должно быть указано, почему не удалось загрузить ядро. Например, если вы обновили ядро, оно может не поддерживаться в Linux.

## <a name="next-steps"></a>Дальнейшие действия

- Узнайте, как [использовать схему услуги]( service-map.md) после ее развертывания и настройки.