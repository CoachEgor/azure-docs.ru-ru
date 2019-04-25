---
title: Подготовка виртуального жесткого диска Windows к передаче в Azure | Документация Майкрософт
description: Подготовка диска VHD или VHDX для Windows перед отправкой в Azure.
services: virtual-machines-windows
documentationcenter: ''
author: glimoli
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 7802489d-33ec-4302-82a4-91463d03887a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 12/13/2018
ms.author: genli
ms.openlocfilehash: 0988902e0a2154f2935a01ddcfb6a460be693df3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60460061"
---
# <a name="prepare-a-windows-vhd-or-vhdx-to-upload-to-azure"></a>Подготовка диска VHD или VHDX для Windows к отправке в Azure
Перед тем как передать виртуальные машины Windows из локальной среды в Microsoft Azure, следует правильно подготовить виртуальный жесткий диск (VHD или VHDX). В Azure поддерживаются **только виртуальные машины первого поколения**, использующие формат файла VHD и фиксированный размер диска. Максимально допустимый размер виртуального жесткого диска составляет 1023 ГБ. Вы можете преобразовать виртуальную машину первого поколения, заменив файловую систему VHDX на VHD, а динамически расширяемый диск на диск фиксированного размера. Но вы не можете изменить поколение виртуальной машины. Дополнительные сведения см. в статье о том, [как выбрать поколение для виртуальной машины в Hyper-V](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

Дополнительные сведения о политике поддержки виртуальной машины Azure см. в статье [Microsoft server software support for Microsoft Azure virtual machines](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) (Поддержка программного обеспечения Microsoft Server для виртуальных машин Microsoft Azure).

> [!Note]
> Инструкции в этой статье применимы к 64-разрядной версии Windows Server 2008 R2 и более поздним версиям операционной системы Windows Server. Сведения о запуске 32-разрядной версии операционной системы в Azure см. в статье [Support for 32-bit operating systems in Azure virtual machines](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines) (Поддержка 32-разрядных операционных систем на виртуальных машинах Azure).

## <a name="convert-the-virtual-disk-to-vhd-and-fixed-size-disk"></a>Преобразование виртуального жесткого диска в формат VHD с фиксированным размером 
Если вы хотите преобразовать виртуальный диск в формат, поддерживаемый Azure, используйте один из методов, указанных в этом разделе. Прежде чем запускать процесс преобразования виртуального диска, создайте резервную копию виртуальной машины и убедитесь, что виртуальный жесткий диск Windows правильно работает на локальном сервере. Устраните все ошибки на виртуальной машине, прежде чем преобразовывать ее или отправлять в Azure.

После преобразования диска создайте виртуальную машину, которая использует этот преобразованный диск. Запустите виртуальную машину и войдите в нее для завершения подготовки виртуальной машины к отправке.

### <a name="convert-disk-using-hyper-v-manager"></a>Преобразование диска с помощью диспетчера Hyper-V
1. Откройте диспетчер Hyper-V и выберите свой локальный компьютер в левой части окна. В меню над списком компьютеров щелкните **Действие** > **Изменить диск**.
2. На экране **Поиск виртуального жесткого диска** найдите и выберите свой виртуальный диск.
3. На экране **Выбрать действие** щелкните **Преобразовать**, а затем — **Далее**.
4. Если необходимо преобразовать диск в формате VHDX, выберите **VHD** и нажмите кнопку **Далее**.
5. Если необходимо преобразовать динамически расширяемый диск, выберите **Фиксированный размер** и нажмите кнопку **Далее**.
6. Укажите путь к папке, в которой нужно сохранить новый VHD-файл.
7. Нажмите кнопку **Готово**

>[!NOTE]
>Команды в этой статье нужно выполнить в сеансе PowerShell с повышенными правами.

### <a name="convert-disk-by-using-powershell"></a>Преобразование диска с помощью PowerShell
Виртуальный диск можно преобразовать с помощью команды [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) в Windows PowerShell. При запуске PowerShell выберите **Запуск от имени администратора**. 

В следующем примере показано преобразование диска VHDX в VHD и динамически расширяемого диска — в диск фиксированного размера.

```Powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```
В этой команде замените значение -Path путем к виртуальному жесткому диску, который нужно преобразовать, а значение -DestinationPath — новыми путем и именем преобразованного диска.

### <a name="convert-from-vmware-vmdk-disk-format"></a>Преобразование диска VMware в формате VMDK
Если у вас есть образ виртуальной машины Windows [в формате файла VMDK](https://en.wikipedia.org/wiki/VMDK), преобразуйте его в формат VHD с помощью [Microsoft VM Converter](https://www.microsoft.com/download/details.aspx?id=42497). Дополнительные сведения см. в записи блога [How to Convert a VMware VMDK to Hyper-V VHD](https://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx) (Преобразование диска VMDK VMware в VHD Hyper-V).

## <a name="set-windows-configurations-for-azure"></a>Настройка конфигурации Windows для Azure

На виртуальной машине, которую вы планируете отправить в Azure, выполните все команды, описанные ниже, используя [окно командной строки с повышенными привилегиями](https://technet.microsoft.com/library/cc947813.aspx):

1. Удалите все постоянные статические маршруты из таблицы маршрутизации.
   
   * Чтобы просмотреть таблицу маршрутов, запустите `route print` из окна командной строки.
   * Проверьте разделы **Persistence Routes** (Сохраняемые маршруты). При наличии постоянного маршрута удалите его с помощью команды **route delete**.
2. Удалите прокси-сервер WinHTTP.
   
    ```PowerShell
    netsh winhttp reset proxy
    ```

    Если виртуальной машине нужно работать с конкретным прокси-сервером, необходимо добавить исключение прокси-сервера в IP-адрес Azure ([168.63.129.16](https://blogs.msdn.microsoft.com/mast/2015/05/18/what-is-the-ip-address-168-63-129-16/
)), чтобы обеспечить подключение виртуальной машины к Azure:
    ```
    $proxyAddress="<your proxy server>"
    $proxyBypassList="<your list of bypasses>;168.63.129.16"

    netsh winhttp set proxy $proxyAddress $proxyBypassList
    ```

3. Установите для политики SAN дисков значение [Onlineall](https://technet.microsoft.com/library/gg252636.aspx):
   
    ```PowerShell
    diskpart 
    ```
    В открытом окне командной строки введите следующие команды:

     ```DISKPART
    san policy=onlineall
    exit   
    ```

4. Установите время в формате UTC для Windows, а для типа запуска службы времени Windows (w32time) — значение **Автоматически**.
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\TimeZoneInformation' -name "RealTimeIsUniversal" -Value 1 -Type DWord -force

    Set-Service -Name w32time -StartupType Automatic
    ```
5. Для профиля управления питанием установите **Высокая производительность**:

    ```PowerShell
    powercfg /setactive SCHEME_MIN
    ```
6. Убедитесь, что для переменных среды **TEMP** и **TMP** заданы значения по умолчанию.

    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -name "TEMP" -Value "%SystemRoot%\TEMP" -Type ExpandString -force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -name "TMP" -Value "%SystemRoot%\TEMP" -Type ExpandString -force
    ```

## <a name="check-the-windows-services"></a>Настройка служб Windows
Убедитесь, что для каждой из приведенных ниже служб Windows заданы **значения Windows по умолчанию**. Это минимальное количество служб, которые необходимо настроить для обеспечения возможности подключения виртуальной машины. Чтобы сбросить параметры загрузки, можно использовать приведенные ниже команды.
   
```PowerShell
Set-Service -Name bfe -StartupType Automatic
Set-Service -Name dhcp -StartupType Automatic
Set-Service -Name dnscache -StartupType Automatic
Set-Service -Name IKEEXT -StartupType Automatic
Set-Service -Name iphlpsvc -StartupType Automatic
Set-Service -Name netlogon -StartupType Manual
Set-Service -Name netman -StartupType Manual
Set-Service -Name nsi -StartupType Automatic
Set-Service -Name termService -StartupType Manual
Set-Service -Name MpsSvc -StartupType Automatic
Set-Service -Name RemoteRegistry -StartupType Automatic
```

## <a name="update-remote-desktop-registry-settings"></a>Обновление параметров реестра для удаленного рабочего стола
Проверьте, чтобы для подключения к удаленному рабочему столу были правильно настроены параметры ниже:

>[!Note] 
>Вы можете получить сообщение об ошибке при выполнении команды **Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services -name &lt;имя объекта&gt; -value &lt;значение&gt;** на этих шагах. Это сообщение можно проигнорировать. Оно означает, что домен не отправляет эту конфигурацию через объект групповой политики.
>
>

1. Включите протокол удаленного рабочего стола (RDP):
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0 -Type DWord -force

    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDenyTSConnections" -Value 0 -Type DWord -force
    ```
   
2. Порт RDP должен быть настроен правильно (по умолчанию это порт 3389):
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "PortNumber" -Value 3389 -Type DWord -force
    ```
    При развертывании виртуальной машины для порта 3389 создаются правила по умолчанию. Если вы хотите изменить номер порта, сделайте это после развертывания виртуальной машины в Azure.

3. Прослушиватель должен работать в каждом сетевом интерфейсе:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "LanAdapter" -Value 0 -Type DWord -force
   ```
4. Настройте режим проверки подлинности на уровне сети для подключений RDP:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" -Value 1 -Type DWord -force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SecurityLayer" -Value 1 -Type DWord -force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "fAllowSecProtocolNegotiation" -Value 1 -Type DWord -force
     ```

5. Задайте значение для проверки активности:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveEnable" -Value 1  -Type DWord -force
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveInterval" -Value 1  -Type DWord -force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "KeepAliveTimeout" -Value 1 -Type DWord -force
    ```
6. Выполните переподключение:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDisableAutoReconnect" -Value 0 -Type DWord -force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fInheritReconnectSame" -Value 1 -Type DWord -force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fReconnectSame" -Value 0 -Type DWord -force
    ```
7. Ограничьте количество одновременных подключений:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "MaxInstanceCount" -Value 4294967295 -Type DWord -force
    ```
8. Если к прослушивателю RDP привязаны какие-либо самозаверяющие сертификаты, удалите их:
    
    ```PowerShell
    Remove-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SSLCertificateSHA1Hash" -force
    ```
    Это необходимо для обеспечения подключения вначале развертывания виртуальной машины. При необходимости проверку можно также выполнить позже — после развертывания виртуальной машины в Azure.

9. Если виртуальная машина будет частью домена, проверьте все параметры ниже, чтобы убедиться, что они установлены. Задайте следующие политики:
    
    | Цель                                     | Политика                                                                                                                                                       | Value                                                                                    |
    |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
    | Включение RDP                           | Конфигурация компьютера\Политики\Параметры Windows\Административные шаблоны\Компоненты\Службы удаленных рабочих столов\Узел сеансов удаленных рабочих столов\Подключения         | Разрешить пользователям удаленное подключение с использованием служб удаленных рабочих столов                                  |
    | Групповая политика с проверкой подлинности на уровне сети                         | Параметры\Административные шаблоны\Компоненты\Службы удаленных рабочих столов\Узел сеансов удаленных рабочих столов\Безопасность                                                    | Требовать проверку подлинности пользователя для удаленных подключений путем проверки подлинности на уровне сети |
    | Параметры проверки активности                      | Конфигурация компьютера\Политики\Параметры Windows\Административные шаблоны\Компоненты Windows\Службы удаленных рабочих столов\Узел сеансов удаленных рабочих столов\Подключения | Настроить интервал проверяемых на активность подключений                                                 |
    | Параметры переподключения                       | Конфигурация компьютера\Политики\Параметры Windows\Административные шаблоны\Компоненты Windows\Службы удаленных рабочих столов\Узел сеансов удаленных рабочих столов\Подключения | Автоматическое переподключение                                                                   |
    | Параметры ограничения количества подключений | Конфигурация компьютера\Политики\Параметры Windows\Административные шаблоны\Компоненты Windows\Службы удаленных рабочих столов\Узел сеансов удаленных рабочих столов\Подключения | Ограничить количество подключений                                                              |

## <a name="configure-windows-firewall-rules"></a>Настройка правил брандмауэра Windows
1. Включите брандмауэр Windows для трех профилей ("Домен", "Стандартный" и "Общий"):

   ```PowerShell
    Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True
   ```

2. Выполните следующие команды PowerShell, чтобы разрешить трафик WinRM в трех профилях брандмауэра ("Домен", "Частный" и "Общий") и включить удаленное управление PowerShell:
   
   ```PowerShell
    Enable-PSRemoting -force

    Set-NetFirewallRule -DisplayName "Windows Remote Management (HTTP-In)" -Enabled True
   ```
3. Включите следующие правила брандмауэра, чтобы разрешить трафик RDP:

   ```PowerShell
    Set-NetFirewallRule -DisplayGroup "Remote Desktop" -Enabled True
   ```   
4. Включите правило общего доступа к файлам и принтерам, чтобы виртуальная машина могла ответить на команду ping внутри виртуальной сети:

   ```PowerShell
   Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -Enabled True
   ``` 
5. Если виртуальная машина будет частью профиля "Домен", проверьте параметры ниже, чтобы убедиться, что они установлены. Задайте следующие политики домена приложения:

    | Цель                                 | Политика                                                                                                                                                  | Value                                   |
    |--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
    | Включите профили брандмауэра Windows: | Конфигурация компьютера\Политики\Параметры Windows\Административные шаблоны\Сеть\Сетевое подключение\Брандмауэр Windows\Профиль домена\Брандмауэр Windows   | Защита всех сетевых подключений         |
    | Включите RDP:                           | Конфигурация компьютера\Политики\Параметры Windows\Административные шаблоны\Сеть\Сетевое подключение\Брандмауэр Windows\Профиль домена\Брандмауэр Windows   | Разрешить исключения для входящих сообщений удаленного управления рабочим столом |
    |                                      | Конфигурация компьютера\Политики\Параметры Windows\Административные шаблоны\Сеть\Сетевое подключение\Брандмауэр Windows\Стандартный профиль\Брандмауэр Windows | Разрешить исключения для входящих сообщений удаленного управления рабочим столом |
    | Включите ICMP-V4:                       | Конфигурация компьютера\Политики\Параметры Windows\Административные шаблоны\Сеть\Сетевое подключение\Брандмауэр Windows\Профиль домена\Брандмауэр Windows   | Разрешить исключения ICMP                   |
    |                                      | Конфигурация компьютера\Политики\Параметры Windows\Административные шаблоны\Сеть\Сетевое подключение\Брандмауэр Windows\Стандартный профиль\Брандмауэр Windows | Разрешить исключения ICMP                   |

## <a name="verify-vm-is-healthy-secure-and-accessible-with-rdp"></a>Проверка виртуальной машины: работоспособность, защищенность и доступ по протоколу RDP 
1. Чтобы проверить работоспособность и согласованность диска, выполните операцию проверки диска при следующей перезагрузке виртуальной машины:

    ```PowerShell
    Chkdsk /f
    ```
    В отчете должен быть указан исправный диск без данных.

2. Задайте параметры данных конфигурации загрузки. 

    > [!Note]
    > Обязательно выполните приведенные ниже команды в окне PowerShell с повышенными привилегиями.
   
   ```powershell
    cmd

    bcdedit /set {bootmgr} integrityservices enable
    bcdedit /set {default} device partition=C:
    bcdedit /set {default} integrityservices enable
    bcdedit /set {default} recoveryenabled Off
    bcdedit /set {default} osdevice partition=C:
    bcdedit /set {default} bootstatuspolicy IgnoreAllFailures

    #Enable Serial Console Feature
    bcdedit /set {bootmgr} displaybootmenu yes
    bcdedit /set {bootmgr} timeout 5
    bcdedit /set {bootmgr} bootems yes
    bcdedit /ems {current} ON
    bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200

    exit
   ```
3. Журнал дампа может быть полезен при устранении критических неполадок Windows. Включите сбор журнала дампа, как показано ниже.

    ```powershell
    # Setup the Guest OS to collect a kernel dump on an OS crash event
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name CrashDumpEnabled -Type DWord -force -Value 2
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name DumpFile -Type ExpandString -force -Value "%SystemRoot%\MEMORY.DMP"
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name NMICrashDump -Type DWord -force -Value 1

    #Setup the Guest OS to collect user mode dumps on a service crash event
    $key = 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps'
    if ((Test-Path -Path $key) -eq $false) {(New-Item -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting' -Name LocalDumps)}
    New-ItemProperty -Path $key -name DumpFolder -Type ExpandString -force -Value "c:\CrashDumps"
    New-ItemProperty -Path $key -name CrashCount -Type DWord -force -Value 10
    New-ItemProperty -Path $key -name DumpType -Type DWord -force -Value 2
    Set-Service -Name WerSvc -StartupType Manual
    ```
4. Проверьте согласованность репозитория инструментария управления Windows. Для этого выполните следующую команду:

    ```PowerShell
    winmgmt /verifyrepository
    ```
    Если репозиторий поврежден, ознакомьтесь с [этим разделом о репозитории WMI](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

5. Никакие сторонние приложения не должны использовать порт 3389. Этот порт используется для службы RDP в Azure. Чтобы узнать, какие порты используются на виртуальной машине, запустите команду **netstat -anob**.

    ```PowerShell
    netstat -anob
    ```

6. Если требуется передать виртуальный жесткий диск Windows, который является контроллером домена, сделайте следующее:

    1. Выполните [эти дополнительные шаги](https://support.microsoft.com/kb/2904015), чтобы подготовить диск.

    1. Если на каком-то этапе вам понадобится запустить виртуальную машину в режиме DSRM, вы должны знать пароль DSRM. Чтобы задать пароль DSRM, используйте эту [ссылку](https://technet.microsoft.com/library/cc754363(v=ws.11).aspx).

7. Вы должны знать встроенную учетную запись администратора и пароль. Вы можете сбросить текущий пароль локального администратора и использовать эту учетную запись для входа в Windows через подключение к удаленному рабочему столу. Это разрешение на доступ контролируется объектом групповой политики "Разрешить вход в систему через службу удаленных рабочих столов". Этот объект можно просмотреть в редакторе локальной групповой политики здесь:

    Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment

8. Проверьте следующие политики AD, чтобы убедиться в том, что доступ удаленному рабочему столу через протокол RDP или из сети не блокируется:

    - Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment\Отказ в доступе к компьютеру из сети;

    - Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment\Запретить вход в систему через службу удаленных рабочих столов.


9. Проверьте следующую политику AD, чтобы убедиться в том, что вы не удаляете какую-либо из следующих учетных записей, необходимых для доступа:

   - Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment\Доступ к этому компьютеру из сети

     В этой политике должны быть указаны следующие группы.

   - Администраторы
   - Операторы архивации
   - Все
   - Пользователи

10. Перезапустите виртуальную машину — ОС Windows должна быть по-прежнему работоспособна и доступна с помощью подключения к удаленному рабочему столу. На этом этапе можно создать виртуальную машину в локальной Hyper-V для обеспечения ее полного запуска, а затем проверить ее доступность через протокол RDP.

11. Удалите все дополнительные фильтры TDI, например программное обеспечение, которое анализирует пакеты TCP, или дополнительные брандмауэры. При необходимости проверку можно также выполнить позже — после развертывания виртуальной машины в Azure.

12. Удалите все стороннее программное обеспечение и драйверы, связанные с физическими компонентами или какой-либо другой технологией виртуализации.

### <a name="install-windows-updates"></a>Установка обновлений Windows
Лучше всего, когда у вас есть **последний уровень исправлений машины**. Если это невозможно, установите следующие обновления:

| Компонент               | Binary         | Windows 7 с пакетом обновления 1 (SP1), Windows Server 2008 R2 с пакетом обновления 1 (SP1) | Windows Server 8, Windows Server 2012               | Windows Server 8.1, Windows Server 2012 R2 | Windows 10 версии 1607 Windows Server 2016 версии 1607 | Windows 10 версии 1703    | Windows 10 версии 1709, Windows Server 2016 версии 1709 | Windows 10 версии 1803 Windows Server 2016 версии 1803 |
|-------------------------|----------------|-------------------------------------------|---------------------------------------------|------------------------------------|---------------------------------------------------------|----------------------------|-------------------------------------------------|-------------------------------------------------|
| Хранилище                 | disk.sys       | 6.1.7601.23403 — KB3125574                | 6.2.9200.17638 / 6.2.9200.21757 — KB3137061 | 6.3.9600.18203 — KB3137061         | -                                                       | -                          | -                                               | -                                               |
|                         | storport.sys   | 6.1.7601.23403 — KB3125574                | 6.2.9200.17188 / 6.2.9200.21306 — KB3018489 | 6.3.9600.18573 — KB4022726         | 10.0.14393.1358 — KB4022715                             | 10.0.15063.332             | -                                               | -                                               |
|                         | ntfs.sys       | 6.1.7601.23403 — KB3125574                | 6.2.9200.17623 / 6.2.9200.21743 — KB3121255 | 6.3.9600.18654 — KB4022726         | 10.0.14393.1198 — KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | Iologmsg.dll   | 6.1.7601.23403 — KB3125574                | 6.2.9200.16384 — KB2995387                  | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | Classpnp.sys   | 6.1.7601.23403 — KB3125574                | 6.2.9200.17061 / 6.2.9200.21180 — KB2995387 | 6.3.9600.18334 — KB3172614         | 10.0.14393.953 — KB4022715                              | -                          | -                                               | -                                               |
|                         | Volsnap.sys    | 6.1.7601.23403 — KB3125574                | 6.2.9200.17047 / 6.2.9200.21165 — KB2975331 | 6.3.9600.18265 — KB3145384         | -                                                       | 10.0.15063.0               | -                                               | -                                               |
|                         | partmgr.sys    | 6.1.7601.23403 — KB3125574                | 6.2.9200.16681 — KB2877114                  | 6.3.9600.17401 — KB3000850         | 10.0.14393.953 — KB4022715                              | 10.0.15063.0               | -                                               | -                                               |
|                         | volmgr.sys     |                                           |                                             |                                    |                                                         | 10.0.15063.0               | -                                               | -                                               |
|                         | Volmgrx.sys    | 6.1.7601.23403 — KB3125574                | -                                           | -                                  | -                                                       | 10.0.15063.0               | -                                               | -                                               |
|                         | Msiscsi.sys    | 6.1.7601.23403 — KB3125574                | 6.2.9200.21006 — KB2955163                  | 6.3.9600.18624 — KB4022726         | 10.0.14393.1066 — KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | Msdsm.sys      | 6.1.7601.23403 — KB3125574                | 6.2.9200.21474 — KB3046101                  | 6.3.9600.18592 — KB4022726         | -                                                       | -                          | -                                               | -                                               |
|                         | Mpio.sys       | 6.1.7601.23403 — KB3125574                | 6.2.9200.21190 — KB3046101                  | 6.3.9600.18616 — KB4022726         | 10.0.14393.1198 — KB4022715                             | -                          | -                                               | -                                               |
|                         | vmstorfl.sys   | 6.3.9600.18907 — KB4072650                | 6.3.9600.18080 — KB3063109                  | 6.3.9600.18907 — KB4072650         | 10.0.14393.2007 — KB4345418                             | 10.0.15063.850 — KB4345419 | 10.0.16299.371 — KB4345420                      | -                                               |
|                         | Fveapi.dll     | 6.1.7601.23311 — KB3125574                | 6.2.9200.20930 — KB2930244                  | 6.3.9600.18294 — KB3172614         | 10.0.14393.576 — KB4022715                              | -                          | -                                               | -                                               |
|                         | Fveapibase.dll | 6.1.7601.23403 — KB3125574                | 6.2.9200.20930 — KB2930244                  | 6.3.9600.17415 — KB3172614         | 10.0.14393.206 — KB4022715                              | -                          | -                                               | -                                               |
| Сеть                 | netvsc.sys     | -                                         | -                                           | -                                  | 10.0.14393.1198 — KB4022715                             | 10.0.15063.250 — KB4020001 | -                                               | -                                               |
|                         | mrxsmb10.sys   | 6.1.7601.23816 — KB4022722                | 6.2.9200.22108 — KB4022724                  | 6.3.9600.18603 — KB4022726         | 10.0.14393.479 — KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | mrxsmb20.sys   | 6.1.7601.23816 — KB4022722                | 6.2.9200.21548 — KB4022724                  | 6.3.9600.18586 — KB4022726         | 10.0.14393.953 — KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | mrxsmb.sys     | 6.1.7601.23816 — KB4022722                | 6.2.9200.22074 — KB4022724                  | 6.3.9600.18586 — KB4022726         | 10.0.14393.953 — KB4022715                              | 10.0.15063.0               | -                                               | -                                               |
|                         | tcpip.sys      | 6.1.7601.23761 — KB4022722                | 6.2.9200.22070 — KB4022724                  | 6.3.9600.18478 — KB4022726         | 10.0.14393.1358 — KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | http.sys       | 6.1.7601.23403 — KB3125574                | 6.2.9200.17285 — KB3042553                  | 6.3.9600.18574 — KB4022726         | 10.0.14393.251 — KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | vmswitch.sys   | 6.1.7601.23727 — KB4022719                | 6.2.9200.22117 — KB4022724                  | 6.3.9600.18654 — KB4022726         | 10.0.14393.1358 — KB4022715                             | 10.0.15063.138             | -                                               | -                                               |
| Core                    | ntoskrnl.exe   | 6.1.7601.23807 — KB4022719                | 6.2.9200.22170 — KB4022718                  | 6.3.9600.18696 — KB4022726         | 10.0.14393.1358 — KB4022715                             | 10.0.15063.483             | -                                               | -                                               |
| Службы удаленных рабочих столов | rdpcorets.dll  | 6.2.9200.21506 — KB4022719                | 6.2.9200.22104 — KB4022724                  | 6.3.9600.18619 — KB4022726         | 10.0.14393.1198 — KB4022715                             | 10.0.15063.0               | -                                               | -                                               |
|                         | termsrv.dll    | 6.1.7601.23403 — KB3125574                | 6.2.9200.17048 — KB2973501                  | 6.3.9600.17415 — KB3000850         | 10.0.14393.0 — KB4022715                                | 10.0.15063.0               | -                                               | -                                               |
|                         | termdd.sys     | 6.1.7601.23403 — KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | win32k.sys     | 6.1.7601.23807 — KB4022719                | 6.2.9200.22168 — KB4022718                  | 6.3.9600.18698 — KB4022726         | 10.0.14393.594 — KB4022715                              | -                          | -                                               | -                                               |
|                         | rdpdd.dll      | 6.1.7601.23403 — KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | rdpwd.sys      | 6.1.7601.23403 — KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
| Безопасность                | MS17-010       | KB4012212                                 | KB4012213                                   | KB4012213                          | KB4012606                                               | KB4012606                  | -                                               | -                                               |
|                         |                |                                           | KB4012216                                   |                                    | KB4013198                                               | KB4013198                  | -                                               | -                                               |
|                         |                | KB4012215                                 | KB4012214                                   | KB4012216                          | KB4013429                                               | KB4013429                  | -                                               | -                                               |
|                         |                |                                           | KB4012217                                   |                                    | KB4013429                                               | KB4013429                  | -                                               | -                                               |
|                         | CVE-2018-0886  | KB4103718               | KB4103730                | KB4103725       | KB4103723                                               | KB4103731                  | KB4103727                                       | KB4103721                                       |
|                         |                | KB4103712          | KB4103726          | KB4103715|                                                         |                            |                                                 |                                                 |
       
### Когда использовать Sysprep <a id="step23"></a>    

Sysprep — это процесс, который можно выполнить при установке Windows. Он приводит к сбросу установки системы, удаляя персональные данные, а также сбрасывая некоторые компоненты. Вы можете запустить его, если нужно создать шаблон, из которого можно развернуть несколько виртуальных машин с определенной конфигурацией. Этот шаблон называется **универсальным образом**.

Если же вы хотите просто создать одну виртуальную машину с помощью отдельного диска, нет необходимости использовать Sysprep. В этом случае ее можно создать с помощью так называемого **специализированного образа**.

Дополнительные сведения о создании виртуальной машины с помощью специализированного диска см. в руководствах ниже:

- [Создание виртуальной машины Windows из специализированного диска](create-vm-specialized.md)
- [Create a VM from a specialized VHD disk](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-specialized-portal?branch=master) (Создание виртуальной машины из специализированного VHD-диска)

Если вы хотите создать универсальный образ, необходимо запустить Sysprep. Дополнительные сведения об использовании Sysprep см. в [этой статье](https://technet.microsoft.com/library/bb457073.aspx). 

Не все роли или приложения, установленные на компьютере с Windows, поддерживают этот образ. Поэтому перед выполнением этой процедуры ознакомьтесь со статьей ниже, чтобы узнать о поддержке процессом Sysprep определенных ролей компьютера. [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles) (Поддержка серверных ролей в Sysprep).

### <a name="steps-to-generalize-a-vhd"></a>Шаги по подготовке VHD

>[!NOTE]
> После выполнения файла sysprep.exe, как описано ниже, выключите виртуальную машину, а после создания ее образа в Azure включите ее снова.

1. Войдите на виртуальную машину Windows.
2. Запустите **командную строку** от имени администратора. 
3. Измените каталог на **%windir%\system32\sysprep** и запустите файл **sysprep.exe**.
3. В диалоговом окне **Программа подготовки системы** выберите **Переход в окно приветствия системы (OOBE)** и убедитесь, что установлен флажок **Подготовка к использованию**.

    ![Средство SysPrep](media/prepare-for-upload-vhd-image/syspre.png)
4. В разделе **Параметры завершения работы** выберите **Завершение работы**.
5. Последовательно выберите **ОК**.
6. После завершения работы Sysprep выключите виртуальную машину. Не используйте **перезапуск** для завершения работы виртуальной машины.
7. Теперь диск VHD можно отправлять. Дополнительные сведения о создании виртуальной машины из универсального образа см. в статье [Отправка универсального виртуального жесткого диска в Azure для создания новой виртуальной машины](sa-upload-generalized.md).


>[!NOTE]
> Настраиваемый файл unattend.xml не поддерживается. Но поддерживается свойство additionalUnattendContent, обеспечивающее ограниченную поддержку для добавления параметров [microsoft-windows-shell-setup](https://docs.microsoft.com/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup) в файл unattend.xml, который используется агентом подготовки Azure. (например,  [additionalUnattendContent](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.compute.models.additionalunattendcontent?view=azure-dotnet) можно использовать для добавления FirstLogonCommands и LogonCommands. См. пример [additionalUnattendContent FirstLogonCommands](https://github.com/Azure/azure-quickstart-templates/issues/1407).


## <a name="complete-recommended-configurations"></a>Рекомендуемые настройки
Приведенные ниже параметры не влияют на передачу VHD. Тем не менее, мы настоятельно рекомендуем их настроить.

* Установите [агент виртуальных машин Azure](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Затем можно включить расширения виртуальной машины. Расширения виртуальной машины реализуют большую часть важных функций, которые могут вам понадобиться при работе с виртуальными машинами, например сброс паролей, настройка протокола RDP и множество других функций. Дополнительные сведения см. в разделе [Обзор агента виртуальной машины Azure](../extensions/agent-windows.md).
* После создания виртуальной машины в Azure мы рекомендуем разместить файл подкачки в томе "временного диска" для повышения производительности. Вы можете это сделать, как показано ниже:

   ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management' -name "PagingFiles" -Value "D:\pagefile.sys" -Type MultiString -force
   ```
  Если к виртуальной машине подключен какой-либо диск данных, том временного диска обычно получает букву "D". Это назначение может отличаться в зависимости от числа доступных дисков и заданных настроек.

## <a name="next-steps"></a>Дальнейшие действия
* [Отправка образа виртуальной машины Windows в Azure для развертываний Resource Manager](upload-generalized-managed.md)
* [Устранение неполадок при активации виртуальных машин Windows в Azure](troubleshoot-activation-problems.md)

