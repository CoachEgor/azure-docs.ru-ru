---
title: Сведения о службе Mobility Service для аварийного восстановления виртуальных машин и физических серверов VMware с помощью Azure Site Recovery | Документация Майкрософт
description: Узнайте об агенте службы Mobility Service для аварийного восстановления виртуальных машин и физических серверов VMware в Azure с помощью службы Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: ramamill
ms.openlocfilehash: c5acc9637fe5afe8f7dd32d23fbdbb80373b4f61
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256839"
---
# <a name="about-the-mobility-service-for-vmware-vms-and-physical-servers"></a>Сведения о службе Mobility Service на виртуальных машинах и физических серверах VMware

При настройке аварийного восстановления для виртуальных машин VMware и физических серверов используйте [Azure Site Recovery](site-recovery-overview.md). Требуется установить службу мобильности Site Recovery на каждую локальную виртуальную машину VMware и физический сервер.  Служба Mobility Service фиксирует операции записи данных на компьютере и перенаправляет их на сервер обработки Site Recovery. Вы можете запустить службу Mobility Service одним из следующих способов:

- [Нажмите установку](#push-installation): Восстановление сайта устанавливает средство передвижения на сервере, когда защита включена через портал Azure.
- Установите вручную: Вы можете установить услугу Мобильности вручную на каждой машине через [uI](#install-mobility-agent-through-ui) или [командный запрос.](#install-mobility-agent-through-command-prompt)
- [Автоматизированное развертывание:](vmware-azure-mobility-install-configuration-mgr.md)Вы можете автоматизировать установку с помощью средств развертывания программного обеспечения, таких как Configuration Manager.

> [!NOTE]
> Мобильный агент использует приблизительно 6%-10% памяти на исходных машинах для VMware VMs или физических машинах.

## <a name="anti-virus-on-replicated-machines"></a>Антивирусное ПО на реплицированных компьютерах

Если на компьютерах, которые нужно реплицировать, запущено антивирусное ПО, обязательно исключите установочную папку службы Mobility в список исключений из антивирусной проверки (*C:\ProgramData\ASR\agent*). Это обеспечит правильную работу репликации.

## <a name="push-installation"></a>Нажмите установку

Установка Push является неотъемлемой частью задания["Включить репликацию",](vmware-azure-enable-replication.md#enable-replication)срабатывающей на портале. После выбора набора виртуальных машин, которые вы хотите защитить и вызвать "Включить репликацию", конфигурация сервера толкает агента мобильности на серверы, устанавливает агента и полную регистрацию агента с конфигурацией сервера. Для успешного завершения этой операции

- Убедитесь, что все [предпосылки установки push выполнены.](vmware-azure-install-mobility-service.md)
- Убедитесь, что все конфигурации серверов подпадают под [матрицу поддержки VMware к сценарию Azure DR.](vmware-physical-azure-support-matrix.md)

Подробная информация о рабочем процессе установки push была описана в следующих разделах.

### <a name="from-923-version-onwards"></a>Начиная с [версии 9.23](https://support.microsoft.com/en-in/help/4494485/update-rollup-35-for-azure-site-recovery)

Во время установки подвижного агента выполняются следующие шаги

1. Толкает агента на исходную машину. Копирование агента на исходную машину может выйти из строя из-за многочисленных ошибок в окружающей среде. Посетите [наше руководство](vmware-azure-troubleshoot-push-install.md) для устранения неполадок push-установки.
2. После успешного копирования агента на сервере предварительные проверки выполняются на сервере. Установка завершается неудачей, если одна или несколько [предпосылок не выполнены.](vmware-physical-azure-support-matrix.md) При выполнения всех предпосылок происходит установка.
3. Поставщик Azure Site Recovery VSS установлен на сервере в рамках установки агента Mobility. Этот поставщик используется для генерации согласованных точек приложения. Если установка поставщика VSS не удается, этот шаг будет пропущен и установка агента будет продолжена.
4. Если установка агента удается, но установка поставщика VSS не удается, то статус задания помечается как "Предупреждение". Это не влияет на генерацию точек согласованности аварий.

    а. Для генерации согласованных точек приложения обратитесь к [нашим рекомендациям](vmware-physical-manage-mobility-service.md#install-site-recovery-vss-provider-on-source-machine) для завершения установки поставщика Site Recovery VSS вручную. </br>
    b.  Если вы не хотите, чтобы были созданы согласованные точки приложения, [измените политику репликации,](vmware-azure-set-up-replication.md#create-a-policy) чтобы отключить согласованные точки приложения.

### <a name="before-922-versions"></a>До 9.22 версий

1. Толкает агента на исходную машину. Копирование агента на исходную машину может выйти из строя из-за многочисленных ошибок в окружающей среде. Посетите [наше руководство](vmware-azure-troubleshoot-push-install.md) для устранения неполадок push-установки.
2. После успешного копирования агента на сервере предварительные проверки выполняются на сервере. Установка завершается неудачей, если одна или несколько [предпосылок не выполнены.](vmware-physical-azure-support-matrix.md) При выполнения всех предпосылок происходит установка.
3. Поставщик Azure Site Recovery VSS установлен на сервере в рамках установки агента Mobility. Этот поставщик используется для генерации согласованных точек приложения. Если установка поставщика VSS не удается, то установка агента не будет. Чтобы избежать сбоя установки агента мобильности, используйте [версию 9.23](https://support.microsoft.com/en-in/help/4494485/update-rollup-35-for-azure-site-recovery) или выше для генерации аварийных согласованных точек и установки поставщика VSS вручную.

## <a name="install-mobility-agent-through-ui"></a>Установка мобильного агента через uI

### <a name="prerequisite"></a>Предварительные требования

- Убедитесь, что все конфигурации серверов подпадают под [матрицу поддержки VMware к сценарию Azure DR.](vmware-physical-azure-support-matrix.md)
- [Найдите установщик на](#locate-installer-files) основе операционной системы сервера.

>[!IMPORTANT]
> Если вы воспроизводите Azure IaaS VM из одного региона Azure в другой, не используйте этот метод. Вместо этого используйте метод установки на основе командной строки.

1. Скопируйте файл установки на компьютер и запустите его.
2. В колонке **параметров установки** выберите **Install Mobility Service** (Установить Mobility Service).
3. Выберите расположение установщика > **Установить**.

    ![Страница параметров установки Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility1.png)

4. Ход состояния установки можно отслеживать в окне **Ход выполнения установки**. После завершения установки нажмите кнопку **Proceed to Configuration** (Перейти к конфигурации), чтобы зарегистрировать службу на сервере конфигурации.

    ![Страница регистрации Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility3.png)

5. В **настройке сервера Детали**, указать IP-адрес и пароль, который вы настроены.

    ![Страница регистрации Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility4.png)

6. Выберите **Зарегистрировать**, чтобы завершить регистрацию.

    ![Страница завершения регистрации Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility5.png)

## <a name="install-mobility-agent-through-command-prompt"></a>Установка агента мобильности с помощью командного запроса

### <a name="prerequisite"></a>Предварительные требования

- Убедитесь, что все конфигурации серверов подпадают под [матрицу поддержки VMware к сценарию Azure DR.](vmware-physical-azure-support-matrix.md)
- [Найдите установщик на](#locate-installer-files) основе операционной системы сервера.

### <a name="on-a-windows-machine"></a>На компьютере с Windows

- Скопируйте установщик в локальную папку (например, C:\Temp) на сервере, который необходимо защитить.

    ```
    cd C:\Temp
    ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
    MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
    cd C:\Temp\Extracted
    ```

- Выполните установку следующим образом.

    ```
    UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
    ```

- Зарегистрируйте агент на сервере конфигурации.

    ```
    cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
    UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
    ```

#### <a name="installation-settings"></a>Параметры установки
**Параметр** | **Подробно**
--- | ---
Использование | UnifiedAgent.exe /Role \<MS/MT> \</InstallLocation Install Location> /Platform "VmWare" /Silent
журналы установки; | В разделе %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log.
/Role | Параметр, обязательный для установки. Указывает, следует ли устанавливать службу Mobility Service или главный целевой сервер.
/InstallLocation| Необязательный параметр. Указывает расположение установки (любая папка).
/Platform | Mandatory. Указывает платформу, на которой будет установлена служба Mobility Service. **VMware** используется для физических серверов или виртуальных машин VMware, а **Azure** — для виртуальных машин Azure.<br/><br/> Если вы рассматриваете VMs Azure как физические машины, укажите **VMware.**
/Silent| Необязательный параметр. Указывает, разрешен ли запуск установщика в автоматическом режиме.

#### <a name="registration-settings"></a>Параметры регистрации
**Параметр** | **Подробно**
--- | ---
Использование | UnifiedAgentConfigurator.exe /CSEndPoint \<CSIP> /PassphraseFilePath \<PassphraseFilePath>
Журналы конфигурации агента | Журналы находятся в папке %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log.
/CSEndPoint | Обязательный параметр. Указывает IP-адрес сервера конфигурации. Используйте любой допустимый IP-адрес.
/PassphraseFilePath |  Mandatory. Расположение файла с парольной фразой. Используйте любой допустимый локальный путь к файлу или UNC.

### <a name="on-a-linux-machine"></a>На компьютере с Linux

1. Скопируйте установщик в локальную папку (например, /tmp) на сервере, который необходимо защитить. Выполните следующие команды в окне терминала.

    ```
    cd /tmp ;
    tar -xvf Microsoft-ASR_UA*release.tar.gz
    ```

2. Выполните установку следующим образом.

    ```
    sudo ./install -d <Install Location> -r MS -v VmWare -q
    ```

3. После завершения установки службу Mobility Service необходимо зарегистрировать на сервере конфигурации. Выполните следующую команду, чтобы зарегистрировать службу Mobility Service на сервере конфигурации.

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
    ```

#### <a name="installation-settings"></a>Параметры установки
**Параметр** | **Подробно**
--- | ---
Использование | ./установить \<месторасположение> \<-r MS/MT> -v VmWare -q
-r | Параметр, обязательный для установки. Указывает, следует ли устанавливать службу Mobility Service или главный целевой сервер.
-d | Необязательный параметр. Указывает расположение установки службы Mobility Service: /usr/local/ASR.
-v | Mandatory. Указывает платформу, на которой будет установлена служба Mobility Service. **VMware** используется для физических серверов или виртуальных машин VMware, а **Azure** — для виртуальных машин Azure.
-Q | Необязательный параметр. Указывает, разрешен ли запуск установщика в автоматическом режиме.

#### <a name="registration-settings"></a>Параметры регистрации
**Параметр** | **Подробно**
--- | ---
Использование | cd /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i \<CSIP \<> -P PassphraseFilePath>
-i | Обязательный параметр. Указывает IP-адрес сервера конфигурации. Используйте любой допустимый IP-адрес.
-P |  Mandatory. Полный путь к файлу, в котором хранится парольная фраза. Используйте любую допустимую папку.

## <a name="azure-virtual-machine-agent"></a>Агент виртуальной машины Azure

- **Виртуальные машины Windows**. Начиная с версии 9.7.0.0 Mobility Service, [агент виртуальной машины Azure](../virtual-machines/extensions/features-windows.md#azure-vm-agent) устанавливает установщик Mobility Service. Это гарантирует, что когда выполняется отработка отказа устройства в Azure, виртуальная машина Azure отвечает требованиям к установке агента для использования любого расширения виртуальной машины.
- **Виртуальные машины Linux**. [WALinuxAgent](https://docs.microsoft.com/azure/virtual-machines/extensions/update-linux-agent) следует установить вручную на виртуальной машине Azure после отработки отказа.

## <a name="locate-installer-files"></a>Найдите файлы установки

Перейдите к папке %ProgramData%-ASR-home-svsystems-pushinstallsvc-repository на сервере конфигурации. Проверьте, какой установщик вам нужен на основе операционной системы. В следующей таблице кратко излагаются файлы установки для каждого VMware VM и операционная система физического сервера. Вы можете просмотреть [поддерживаемые операционные системы](vmware-physical-azure-support-matrix.md#replicated-machines), прежде чем начать.

**Файл установщика** | **Операционная система (только 64-разрядная версия)**
--- | ---
Microsoft-ASR\_UA\*Windows\*release.exe | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 с пакетом обновления 1
Microsoft-ASR\_UA\*RHEL6-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 6.* </br> CentOS 6.*
Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.* </br> CentOS 7.*
Microsoft-ASR\_UA\*SLES12-64\*release.tar.gz | SUSE Linux Enterprise Server 12 (с пакетом обновления 1, 2 и 3)
Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3
Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4
Microsoft-ASR\_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5
Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz | Ubuntu Linux 14.04
Microsoft-ASR\_UA\*UBUNTU-16.04-64\*release.tar.gz | Сервер Ubuntu Linux 16.04 LTS
Microsoft-ASR_UA\*DEBIAN7-64\*release.tar.gz | Debian 7
Microsoft-ASR_UA\*DEBIAN8-64\*release.tar.gz | Debian 8

## <a name="next-steps"></a>Дальнейшие действия

[Установка службы Mobility Service для аварийного восстановления виртуальных машин VMware и физических серверов](vmware-azure-install-mobility-service.md)
