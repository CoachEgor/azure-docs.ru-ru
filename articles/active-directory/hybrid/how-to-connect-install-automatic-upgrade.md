---
title: 'Azure AD Connect: автоматическое обновление | Документация Майкрософт'
description: В этом разделе описывается встроенная функция автоматического обновления в Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6b395e8f-fa3c-4e55-be54-392dd303c472
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: ae0632fbc3208befe197c15ffdbf2d9a4e7b2d7a
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2020
ms.locfileid: "82926482"
---
# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD Connect: автоматическое обновление
Эта функция появилась в сборке [1.1.105.0 (выпущенной в феврале 2016 года)](reference-connect-version-history.md#111050).  Эта функция была обновлена в [сборке 1.1.561](reference-connect-version-history.md#115610) и теперь поддерживает дополнительные, ранее не поддерживаемые, сценарии.

## <a name="overview"></a>Обзор
С функцией **автоматического обновления** контроль за обновлением установки Azure AD Connect максимально упростился. Эта функция по умолчанию включена для быстрой установки и обновления DirSync. При выпуске новой версии установка обновляется автоматически.
Автоматическое обновление по умолчанию включено в следующих случаях:

* Экспресс-установка параметров и обновление DirSync.
* Использование SQL Express LocalDB (всегда используется при экспресс-установке). DirSync с SQL Express также использует LocalDB.
* Учетной записью AD является учетная запись MSOL_ по умолчанию, создаваемая при экспресс-установке и обновлении DirSync.
* В метавселенной меньше 100 000 объектов.

Текущее состояние автоматического обновления можно узнать с помощью командлета PowerShell `Get-ADSyncAutoUpgrade`. Возможны следующие состояния:

| Состояние | Комментарий |
| --- | --- |
| Активировано |Автоматическое обновление включено. |
| Приостановлена |Устанавливается только системой. Сейчас система **не имеет возможности** получать автоматические обновления. |
| Выключено |Автоматическое обновление отключено. |

Переключаться между состояниями **Включено** и **Отключено** позволяет командлет `Set-ADSyncAutoUpgrade`. Состояние **Приостановлено**может устанавливаться только системой.  До 1.1.750.0 командлет Set-ADSyncAutoUpgrade заблокировал автоматическое обновление, если состояние автоматического обновления было установлено в suspended. Теперь эта функция изменена, поэтому она не блокирует автообновление.

В качестве инфраструктуры обновлений при автоматическом обновлении используется Azure AD Connect Health. Чтобы работало автоматическое обновление, откройте в прокси-сервере URL-адреса для **Azure AD Connect Health** , указанные в статье [URL-адреса и диапазоны IP-адресов Office 365](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).


Если на сервере запущен пользовательский интерфейс **диспетчера службы синхронизации** , обновление будет отложено до тех пор, пока он не будет закрыт.

## <a name="troubleshooting"></a>Устранение неполадок
Если установка Connect не обновляется автоматически, выполните следующие действия, чтобы узнать, в чем может быть проблема.

Во-первых, не следует ожидать попыток автоматического обновления в первый день выпуска новой версии. Попытки обновления выполняются в случайном порядке намеренно, поэтому не стоит беспокоиться, если обновление установки не начинается немедленно.

Если вы считаете, что возникла проблема, сначала запустите `Get-ADSyncAutoUpgrade` , чтобы убедиться, что автоматическое обновление включено.

Затем убедитесь, что вы открыли нужные URL-адреса на прокси-сервере или в брандмауэре. Автоматическое обновление использует Azure AD Connect Health, как описано в [обзоре](#overview). При использовании прокси-сервера убедитесь, что служба Health настроена для работы с [прокси-сервером](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). Кроме того, проверьте [подключение Health](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) к Azure AD.

После проверки подключения к Azure AD можно изучить журналы событий. Запустите средство просмотра событий и найдите журнал событий **Приложение** . Добавьте фильтр журнала событий для источника **Azure AD Connect Upgrade** и диапазона идентификаторов событий **300-399**.  
![Фильтр журнала событий для автоматического обновления](./media/how-to-connect-install-automatic-upgrade/eventlogfilter.png)  

Откроется список журналов событий, связанных с состоянием автоматического обновления.  
![Фильтр журнала событий для автоматического обновления](./media/how-to-connect-install-automatic-upgrade/eventlogresult.png)  

Итоговый код содержит префикс и общие сведения о состоянии.

| Префикс итогового кода | Описание: |
| --- | --- |
| Успех |Установка успешно обновлена. |
| UpgradeAborted |Временное состояние привело к остановке обновления. Будет выполнена повторная попытка установки, и ожидается, что она будет успешной. |
| UpgradeNotSupported |Конфигурация системы блокирует автоматическое обновление. Будет выполнена повторная попытка для проверки изменения состояния, однако ожидается, что систему потребуется обновить вручную. |

Ниже приведен список наиболее распространенных сообщений. В списке приведены не все сообщения, однако сообщение о результате должно явно указывать на проблему.

| Сообщение о результате | Описание |
| --- | --- |
| **UpgradeAborted** | |
| UpgradeAbortedCouldNotSetUpgradeMarker |Не удалось сделать запись в реестре. |
| UpgradeAbortedInsufficientDatabasePermissions |Встроенная группа администраторов не имеет разрешений в базе данных. Для решения этой проблемы необходимо установить обновление до новейшей версии Azure AD Connect вручную. |
| UpgradeAbortedInsufficientDiskSpace |Недостаточно дискового пространства для поддержки обновления. |
| UpgradeAbortedSecurityGroupsNotPresent |Не удалось найти и разрешить все группы безопасности, используемые модулем синхронизации. |
| UpgradeAbortedServiceCanNotBeStarted |Не удалось запустить **синхронизацию Microsoft Azure AD** для службы NT. |
| UpgradeAbortedServiceCanNotBeStopped |Не удалось остановить **синхронизацию Microsoft Azure AD** для службы NT. |
| UpgradeAbortedServiceIsNotRunning |**Синхронизация Microsoft Azure AD** для службы NT не выполняется. |
| UpgradeAbortedSyncCycleDisabled |Параметр SyncCycle в [планировщике](how-to-connect-sync-feature-scheduler.md) был отключен. |
| UpgradeAbortedSyncExeInUse |На сервере открыт [пользовательский интерфейс Synchronization Service Manager](how-to-connect-sync-service-manager-ui.md) . |
| UpgradeAbortedSyncOrConfigurationInProgress |Выполняется мастер установки, или синхронизация была запланирована вне планировщика. |
| **UpgradeNotSupported** | |
| UpgradeNotSupportedAdfsSignInMethod | Вы выбрали вход с помощью AD FS. |
| UpgradeNotSupportedCustomizedSyncRules |Пользователь добавил собственные правила в конфигурацию. |
| UpgradeNotSupportedDeviceWritebackEnabled |Включена функция [обратной записи устройства](how-to-connect-device-writeback.md) . |
| UpgradeNotSupportedGroupWritebackEnabled |Включена функция [обратной записи групп](how-to-connect-preview.md#group-writeback) . |
| UpgradeNotSupportedInvalidPersistedState |Установка не является экспресс-установкой параметров или обновлением DirSync. |
| UpgradeNotSupportedMetaverseSizeExceeeded |В метавселенной больше 100 000 объектов. |
| UpgradeNotSupportedMultiForestSetup |Выполняется подключение к нескольким лесам. Экспресс-установка подключается только к одному лесу. |
| UpgradeNotSupportedNonLocalDbInstall |Вы не используете базу данных SQL Server Express LocalDB. |
| UpgradeNotSupportedNonMsolAccount |[Учетная запись соединителя AD DS](reference-connect-accounts-permissions.md#ad-ds-connector-account) больше не является учетной записью MSOL_ по умолчанию. |
| UpgradeNotSupportedNotConfiguredSignInMethod | При настройке AAD Connect в качестве метода входа вы выбрали параметр *Не настраивать*. |
| UpgradeNotSupportedStagingModeEnabled |Сервер настроен для работы в [промежуточном режиме](how-to-connect-sync-staging-server.md). |
| UpgradeNotSupportedUserWritebackEnabled |Включена функция [обратной записи пользователей](how-to-connect-preview.md#user-writeback) . |

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения об [интеграции локальных удостоверений с Azure Active Directory](whatis-hybrid-identity.md).
