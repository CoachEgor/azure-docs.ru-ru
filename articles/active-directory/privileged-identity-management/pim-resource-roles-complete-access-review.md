---
title: Завершение проверки доступа для ролей ресурсов Azure в PIM — Azure Active Directory | Документация Майкрософт
description: Узнайте, как завершение проверки доступа для ролей ресурсов Azure в Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: aee8ac3c2638ede559f8a1f9c51f2d6e62604998
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65602298"
---
# <a name="complete-an-access-review-of-azure-resource-roles-in-pim"></a>Завершение проверки доступа для ролей ресурсов Azure в PIM
После [запуска проверки доступа](pim-resource-roles-start-access-review.md) администраторы привилегированных ролей могут проверить привилегированный доступ. Azure Active Directory (Azure AD) Privileged Identity Management (PIM) автоматически отправляет сообщение электронной почты с запросом на проверку доступа. Если пользователь не получил электронное письмо, ему можно отправить [инструкции по выполнению проверки доступа](pim-resource-roles-perform-access-review.md).

После окончания периода проверки доступа или после завершения всеми пользователями самостоятельной проверки выполните шаги, описанные в этой статье, для управления проверкой и просмотра результатов.

## <a name="manage-access-reviews"></a>Управление проверками доступа
1. Перейдите на [портал Azure](https://portal.azure.com/). Затем выберите приложение **ресурсов Azure** на панели мониторинга.

2. Выберите ресурс.

3. Выберите раздел **Проверки доступа** на панели мониторинга.
![Проверки доступа](media/pim-resource-roles-complete-access-review/rbac-access-review-home-list.png)

4. Выберите необходимую проверку доступа.

В колонке со сведениями о проверке доступа отобразятся параметры для управления проверкой. Вот они:

![Параметры для управления проверкой](media/pim-resource-roles-complete-access-review/rbac-access-review-menu.png)

### <a name="stop"></a>Остановить
Все проверки доступа имеют дату окончания, но можно воспользоваться кнопкой **Остановить** , чтобы завершить их выполнение раньше. Все пользователи, по которым еще не закончилась проверка к этому времени, не смогут ее пройти после остановки. Нельзя перезапустить проверку после ее остановки.

### <a name="reset"></a>Reset
Вы можете сбросить проверку доступа, чтобы удалить все соответствующие решения. После сброса проверки доступа все пользователи будут отмечены как непросмотренные. 

### <a name="apply"></a>Применить
После завершения проверки доступа используйте кнопку **Применить**, чтобы реализовать результат проверки. Если в процессе проверки пользователю было отказано в доступе, то это приведет к удалению назначенной пользователю роли.  

### <a name="delete"></a>Delete (Удалить)
Если проверка вам больше не нужна, удалите ее. При нажатии кнопки **Удалить** проверка будет удалена из приложения PIM.

## <a name="results"></a>Результаты
На вкладке **Результаты** просмотрите и скачайте список результатов проверки. 
![Вкладка результатов](media/pim-resource-roles-complete-access-review/rbac-access-review-results.png)

## <a name="reviewers"></a>Рецензенты
Просмотрите и добавьте рецензентов в существующие проверки доступа. Напомните рецензентам выполнить проверки.
![Добавить рецензентов](media/pim-resource-roles-complete-access-review/rbac-access-review-reviewers.png)

## <a name="next-steps"></a>Дальнейшие действия

- [Запуск проверки доступа для ролей ресурсов Azure в PIM](pim-resource-roles-start-access-review.md)
- [Выполнение проверки доступа своих ролей ресурсов Azure в PIM](pim-resource-roles-perform-access-review.md)
