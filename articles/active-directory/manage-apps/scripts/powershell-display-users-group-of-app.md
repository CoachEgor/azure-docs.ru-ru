---
title: Пример PowerShell. Вывод данных о пользователях и группах для приложения Application Proxy
description: Пример PowerShell, который выводит данные обо всех пользователях и группах, назначенных конкретному приложению Azure Active Directory (Azure AD) Application Proxy.
services: active-directory
author: kenwith
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 12/05/2019
ms.author: kenwith
ms.reviewer: japere
ms.openlocfilehash: c9885e42dc81d81dddea6205ed85be8276ffbf94
ms.sourcegitcommit: 21c3363797fb4d008fbd54f25ea0d6b24f88af9c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/08/2020
ms.locfileid: "96861720"
---
# <a name="display-users-and-groups-assigned-to-an-application-proxy-application"></a>Отображение пользователей и групп, назначенных приложению прокси приложения

Этот пример скрипта PowerShell выводит данные о пользователях и группах, назначенных конкретному приложению Azure Active Directory (Azure AD) Application Proxy.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Для работы с этим примером требуется [модуль Azure AD PowerShell (версии 2) для Graph](/powershell/azure/active-directory/install-adv2) (AzureAD) или [предварительная версия модуля Azure AD PowerShell (версии 2) для Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) (AzureADPreview).

## <a name="sample-script"></a>Пример скрипта

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/display-users-group-of-an-app.ps1 "Display users and groups assigned to an Application Proxy application")]

## <a name="script-explanation"></a>Описание скрипта

| Get-Help | Примечания |
|---|---|
| [Get-AzureADUser](/powershell/module/AzureAD/get-azureaduser)| Возвращает пользователя. |
| [Get-AzureADGroup](/powershell/module/AzureAD/get-azureadgroup)| Получает группу. |
| [Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal) | Получает субъект-службу. |
| [Get-AzureADUserAppRoleAssignment](/powershell/module/AzureAD/get-azureaduserapproleassignment) | Получает назначение роли приложения для пользователя. |
| [Get-AzureADGroupAppRoleAssignment](/powershell/module/AzureAD/get-azureadgroupapproleassignment) | Получает назначение роли приложения для группы. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о модуле Azure AD PowerShell см. в этом [обзоре](/powershell/azure/active-directory/overview).

См. другие [примеры Azure AD PowerShell для Azure AD Application Proxy](../application-proxy-powershell-samples.md).
