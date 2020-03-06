---
title: Функции и конфигурация синхронизации Azure AD Connect | Документация Майкрософт
description: Описываются функциональные возможности службы синхронизации Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 213aab20-0a61-434a-9545-c4637628da81
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/25/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5486a8d8bd4c295f49e0ab847daf45d0fcab47ad
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2020
ms.locfileid: "78300542"
---
# <a name="azure-ad-connect-sync-service-features"></a>Функции службы синхронизации Azure AD Connect

Средство синхронизации Azure AD Connect состоит из двух компонентов.

* Локальный компонент под названием **Синхронизация Azure AD Connect**, который также называется **модулем синхронизации**.
* Размещенная в Azure AD служба, которая также называется **службой синхронизации Azure AD Connect**

В этом разделе описаны следующие функции **службы синхронизации Azure AD Connect** и их настройка с помощью Windows PowerShell.

Для их настройки нужен [Модуль Azure Active Directory для Windows PowerShell](https://aka.ms/aadposh). Он загружается и устанавливается отдельно от Azure AD Connect. Описанные в этом разделе командлеты появились в [версии за март 2016 г. (сборка 9031.1)](https://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Если у вас нет командлетов, описанных в этом разделе, или они дают другой результат, проверьте, используете ли вы последнюю версию.

Для просмотра конфигурации каталога Azure AD выполните следующую команду: `Get-MsolDirSyncFeatures`.  
![Результат вызова командлета Get-MsolDirSyncFeatures](./media/how-to-connect-syncservice-features/getmsoldirsyncfeatures.png)

Многие из этих параметров могут быть изменены только службой Azure AD Connect.

Следующие параметры можно настроить с помощью `Set-MsolDirSyncFeature`:

| DirSyncFeature | Комментарий |
| --- | --- |
| [EnableSoftMatchOnUpn](#userprincipalname-soft-match) |Позволяет присоединять объекты по атрибуту userPrincipalName в дополнение к основному адресу SMTP. |
| [SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) |Позволяет модулю синхронизации обновлять атрибут userPrincipalName для управляемых или лицензированных пользователей (не являющихся федеративными). |

После включения функции отключить ее нельзя.

> [!NOTE]
> Начиная с 24 августа 2016 г. функция *Устойчивость повторяющихся атрибутов* включена по умолчанию для всех новых каталогов Azure AD. Эта функция будет постепенно активирована и включена для каталогов, созданных до этой даты. Вы получите по электронной почте уведомление о включении этой функции для вашего каталога.
> 
> 

Следующие параметры настраиваются службой Azure AD Connect и не могут изменяться командлетом `Set-MsolDirSyncFeature`:

| DirSyncFeature | Комментарий |
| --- | --- |
| DeviceWriteback |[Azure AD Connect: включение обратной записи устройств](how-to-connect-device-writeback.md) |
| DirectoryExtensions |[Синхронизация Azure AD Connect: расширения каталогов](how-to-connect-sync-feature-directory-extensions.md) |
| [DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) |Позволяет поместить атрибут, являющийся копией другого объекта, на карантин, вместо того чтобы прерывать весь процесс экспорта для этого объекта. |
| Синхронизация хэша паролей |[Реализация синхронизации хэшированных паролей в службе синхронизации Azure AD Connect](how-to-connect-password-hash-synchronization.md) |
|Сквозная проверка подлинности|[Вход пользователей с помощью сквозной проверки подлинности Azure Active Directory](how-to-connect-pta.md)|
| UnifiedGroupWriteback |[Предварительная версия. Обратная запись групп](how-to-connect-preview.md#group-writeback) |
| UserWriteback |Не поддерживается в текущей версии. |

## <a name="duplicate-attribute-resiliency"></a>Устойчивость повторяющихся атрибутов

При подготовке объектов с повторяющимися именами участников-пользователей (UPN) или адресами прокси-сервера (proxyAddress) процесс не прерывается ошибкой, а повторяющийся атрибут помещается на карантин, и ему присваивается временное значение. После разрешения конфликта временное имя участника-пользователя (UPN) будет автоматически исправлено на соответствующее значение. Дополнительные сведения см. в статье [Синхронизация удостоверений и устойчивость повторяющихся атрибутов](how-to-connect-syncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>Мягкое сопоставление атрибута userPrincipalName

При включении этой функции к имени участника-пользователя, а также к [основному адресу SMTP](https://support.microsoft.com/kb/2641663), который всегда активен, может применяться мягкое сопоставление. Оно используется для сопоставления существующих облачных пользователей в Azure AD с локальными пользователями.

Эта функция удобна для сопоставления локальных учетных записей AD с существующими учетными записями, созданными в облаке, если вы не используете Exchange Online. В такой ситуации устанавливать атрибут SMTP для облака обычно не нужно.

Эта функция по умолчанию включена для создаваемых каталогов Azure AD. Чтобы увидеть, включена ли эта функция, выполните следующую команду:  

```powershell
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Если эта функция отключена для вашего каталога Azure AD, ее можно включить, выполнив следующую команду:  

```powershell
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>Синхронизация обновлений атрибута userPrincipalName

Обновления атрибута userPrincipalName с помощью службы синхронизации из локальной среды блокируются за исключением тех случаев, когда выполняются два следующих условия:

* пользователь является управляемым (нефедеративным);
* пользователю не назначена лицензия.

Дополнительные сведения см. в статье [User names in Office 365, Azure, or Intune don't match the on-premises UPN or alternate login ID](https://support.microsoft.com/kb/2523192) (Имена пользователей в Office 365, Azure или Intune не совпадают с локальным именем участника-пользователя или альтернативным именем для входа).

Включение этой функции позволяет модулю синхронизации обновлять userPrincipalName при изменении в локальной среде, а также использовать синхронизацию хэша паролей или сквозную проверку подлинности.

Эта функция по умолчанию включена для создаваемых каталогов Azure AD. Чтобы увидеть, включена ли эта функция, выполните следующую команду:  

```powershell
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Если эта функция отключена для вашего каталога Azure AD, ее можно включить, выполнив следующую команду:  

```powershell
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

После включения этой функции существующие значения атрибут userPrincipalName останутся неизменными. Если в дальнейшем атрибут userPrincipalName изменится в локальной среде, то имя участника-пользователя будет обновлено при обычной синхронизации изменений для пользователей.  

## <a name="see-also"></a>См. также раздел

* [Службы синхронизации Azure AD Connect](how-to-connect-sync-whatis.md)
* [Интеграция локальных удостоверений с Azure Active Directory](whatis-hybrid-identity.md).
