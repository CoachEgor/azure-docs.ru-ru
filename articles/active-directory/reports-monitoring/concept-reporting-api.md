---
title: Начало работы с API отчетов Azure AD | Документация Майкрософт
description: Как начать работу с API отчетов Azure Active Directory
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: c841195cacd8c8d7698f69435572587bf04c3d67
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2019
ms.locfileid: "74008359"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Начало работы с API отчетов Azure Active Directory

Azure Active Directory предоставляет разнообразные [отчеты](overview-reports.md), содержащий полезные сведения для приложений, таких как системы SIEM, средства аудита и бизнес-аналитики. 

API Microsoft Graph для отчетов Azure AD обеспечивает программный доступ к данным с помощью набора интерфейсов API на базе REST. Эти интерфейсы API можно вызвать, используя различные языки и инструменты программирования.

В этой статье представлены общие сведения об API отчетов, включая информацию о способах доступа к нему.

При возникновении проблем ознакомьтесь со статьей [Как получить поддержку для Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto).

## <a name="prerequisites"></a>предварительным требованиям

Чтобы получить доступ к API отчетов с вмешательством пользователя или без него, потребуется:

1. Назначение ролей (читателя сведений о безопасности, администратора безопасности, глобального администратора).
2. Регистрация приложения
3. Предоставление разрешений
4. соберите параметры конфигурации.

Подробные инструкции см. в [предварительных требованиях для доступа к API отчетов Azure Active Directory](howto-configure-prerequisites-for-reporting-api.md). 

## <a name="api-endpoints"></a>Конечные точки API 

`https://graph.microsoft.com/beta/auditLogs/directoryAudits` — это конечная точка API Microsoft Graph для журналов аудита, а `https://graph.microsoft.com/beta/auditLogs/signIns` — конечная точка API Microsoft Graph для входа в систему. Дополнительные сведения см. в справке по [API аудита](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) и [API входа](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signIn).

Кроме того, вы можете использовать [API обнаружения угроз для защиты идентификации](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent) , чтобы получить программный доступ к средствам обнаружения безопасности с помощью Microsoft Graph. Дополнительные сведения см. в статье [Начало работы с защитой идентификации Azure Active Directory и Microsoft Graph](../identity-protection/graph-get-started.md). 

> [!NOTE]
>  Конечная точка **https:\/\/graph.windows.net\/\<имя_клиента\>\/reports\/** является нерекомендуемой. Используйте описанные выше новые конечные точки API для получения программного доступа к действиям и отчетам о безопасности.
  
## <a name="apis-with-graph-explorer"></a>Использование API с песочницей Graph

Вы можете использовать [песочницу MSGraph](https://developer.microsoft.com/graph/graph-explorer) для проверки входа и аудита данных API. Войдите в учетную запись, используя одну из кнопок входа в пользовательском интерфейсе песочницы Graph и задав для клиента разрешения **AuditLog.Read.All** и **Directory.ReadAll**, как показано ниже.   

![Песочница Graph](./media/concept-reporting-api/graph-explorer.png)

![Пользовательский интерфейс для изменения разрешений](./media/concept-reporting-api/modify-permissions.png)

## <a name="use-certificates-to-access-the-azure-ad-reporting-api"></a>Использование сертификатов для доступа к API отчетов Azure AD 

Если вы планируете получать данные отчетов без вмешательства пользователя, используйте API отчетов Azure AD с сертификатами.

Подробные инструкции см. в статье о [получении данных с помощью API отчетов Azure AD с сертификатами](tutorial-access-api-with-certificates.md).

## <a name="next-steps"></a>Дополнительная информация

 * [Предварительные требования для доступа к API отчетов](howto-configure-prerequisites-for-reporting-api.md) 
 * [Получение данных, используя API отчетов Azure AD с сертификатами](tutorial-access-api-with-certificates.md)
 * [Устранение ошибок в API отчетов Azure Active Directory](troubleshoot-graph-api.md)


