---
title: Скрыть приложение от взаимодействия с пользователем в Azure AD
description: Как скрыть приложение от пользователей на панелях доступа Azure Active Directory или в средствах запуска Office 365.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: mimart
ms.reviewer: kasimpso
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5718adf4fd76e2fbd0ff793dd2fa33ee08f7c0fe
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80295052"
---
# <a name="hide-applications-from-end-users-in-azure-active-directory"></a>Скрытие приложений от пользователей в Azure Active Directory

Инструкции о том, как скрыть приложения на панели MyApps пользователей или в средстве запуска Office 365. Когда приложение скрыто, у пользователей по-прежнему есть к нему доступ. 

## <a name="prerequisites"></a>Предварительные условия

Для того чтобы скрыть приложение на панели MyApps и в средстве запуска Office 365, необходимы права администратора приложения.

Чтобы скрыть все приложения Office 365, необходимы права глобального администратора.


## <a name="hide-an-application-from-the-end-user"></a>Скрытие приложения от пользователя
Чтобы скрыть приложение на панели MyApps и в средствах запуска приложений Office 365, выполните описанные ниже действия.

1.  Войдите на [портал Azure](https://portal.azure.com) как глобальный администратор каталога.
2.  Выберите **Azure Active Directory**.
3.  Выберите **Корпоративные приложения**. Откроется колонка **Корпоративные приложения — Все приложения**.
4.  В разделе **Тип приложения** выберите **Корпоративные приложения**, если этот параметр еще не выбран.
5.  Найдите приложение, которое нужно скрыть, а затем щелкните его.  Откроется колонка обзора приложения.
6.  Нажмите кнопку **Свойства**. 
7.  Для вопроса **Видно пользователям?** выберите **Нет**.
8.  Нажмите кнопку **Сохранить**.

## <a name="use-azure-ad-powershell-to-hide-an-application"></a>Использование Azure AD PowerShell для скрытия приложения

Чтобы скрыть приложение на панели "MyApps", можно вручную добавить тег Хидеапп к субъекту-службе для приложения. Выполните следующие команды [PowerShell AzureAD](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#service_principals) , чтобы задать для приложения значение **нет**. для свойства **Visible** . 

```PowerShell
Connect-AzureAD

$objectId = "<objectId>"
$servicePrincipal = Get-AzureADServicePrincipal -ObjectId $objectId
$tags = $servicePrincipal.tags
$tags += "HideApp"
Set-AzureADServicePrincipal -ObjectId $objectId -Tags $tags
```

## <a name="hide-office-365-applications-from-the-myapps-panel"></a>Скрытие приложений Office 365 на панели MyApps

Чтобы скрыть все приложения Office 365 на панели MyApps, выполните следующие действия. Приложения по-прежнему будут видны на портале Office 365.

1.  Войдите на [портал Azure](https://portal.azure.com) как глобальный администратор каталога.
2.  Выберите **Azure Active Directory**.
3.  Выберите **Параметры пользователя**.
4.  В разделе **корпоративных приложений** щелкните **Manage how end users launch and view their applications** (Управление запуском и просмотром приложений для пользователей).
5.  Для параметра **Пользователи могут видеть приложения Office 365 только на портале Office 365** выберите **Да**.
6.  Нажмите кнопку **Сохранить**.

## <a name="next-steps"></a>Дальнейшие шаги
* [Просмотр всех моих групп](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Назначение корпоративному приложению пользователя или группы](assign-user-or-group-access-portal.md)
* [Удаление назначения пользователя или группы из корпоративного приложения](remove-user-or-group-access-portal.md)
* [Изменение имени или логотипа корпоративного приложения](change-name-or-logo-portal.md)

