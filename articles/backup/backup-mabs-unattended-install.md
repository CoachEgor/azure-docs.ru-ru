---
title: Автоматическая установка Azure Backup Server версии 2
description: Использование скрипта PowerShell для автоматической установки Azure Backup Server версии 2. Этот тип установки также называется "тихой" установкой.
ms.topic: conceptual
ms.date: 11/13/2018
ms.openlocfilehash: 1539089e713bcf8e959707c6ff4a608f062a7c00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "74172249"
---
# <a name="run-an-unattended-installation-of-azure-backup-server"></a>Запуск автоматической установки Azure Backup Server

Узнайте, как запустить автоматическую установку Azure Backup Server.

Эти действия не применяются, если вы устанавливаете сервер резервного копирования Azure V1.

## <a name="install-backup-server"></a>Установка Backup Server

1. На сервере, где будет использоваться Azure Backup Server версии 2 или более поздней, создайте текстовый файл. (Вы можете создать файл в блокноте или в другом текстовом редакторе.) Сохранить файл как MABSSetup.ini.

2. Вставьте следующий код в файл MABSSetup.ini. Замените текст внутри скобки ()\< \>значениями из вашей среды. Ниже приведен пример текста.

   ```text
   [OPTIONS]
   UserName=administrator
   CompanyName=<Microsoft Corporation>
   SQLMachineName=localhost
   SQLInstanceName=<SQL instance name>
   SQLMachineUserName=administrator
   SQLMachinePassword=<admin password>
   SQLMachineDomainName=<machine domain>
   ReportingMachineName=localhost
   ReportingInstanceName=<reporting instance name>
   SqlAccountPassword=<admin password>
   ReportingMachineUserName=<username>
   ReportingMachinePassword=<reporting admin password>
   ReportingMachineDomainName=<domain>
   VaultCredentialFilePath=<vault credential full path and complete name>
   SecurityPassphrase=<passphrase>
   PassphraseSaveLocation=<passphrase save location>
   UseExistingSQL=<1/0 use or do not use existing SQL>
   ```

3. Сохраните файл. Затем в командной строке с повышенными привилегиями на сервере установки введите следующую команду:

   ```cmd
   start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
   ```

Для установки можно использовать следующие флаги:</br>
**/f** — путь к файлу.</br>
**/l** — путь к журналу.</br>
**/i** — путь для установки.</br>
**/x** — путь для удаления.</br>

## <a name="next-steps"></a>Дальнейшие действия

После установки Backup Server узнайте, как подготовить сервер или обеспечить защиту рабочей нагрузки.

- [Подготовка к резервному копированию рабочих нагрузок с использованием Azure Backup Server](backup-azure-microsoft-azure-backup.md)
- [Резервное копирование сервера VMware в Azure](backup-azure-backup-server-vmware.md)
- [Архивация баз данных SQL Server в Azure с помощью Azure Backup Server](backup-azure-sql-mabs.md)
- [Добавление хранилища на Azure Backup Server версии 2](backup-mabs-add-storage.md)
