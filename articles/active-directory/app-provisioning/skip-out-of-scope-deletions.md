---
title: Пропустить удаление пользователей вне области действия | Документация Майкрософт
description: Узнайте, как переопределить поведение по умолчанию для отмены подготовки для пользователей области.
services: active-directory
author: cmmdesai
documentationcenter: na
manager: daveba
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.subservice: app-provisioning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/10/2019
ms.author: chmutali
ms.collection: M365-identity-device-management
ms.openlocfilehash: a1668c022a0f067a381ba09b397c7d38c99ad074
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77522455"
---
# <a name="skip-deletion-of-user-accounts-that-go-out-of-scope"></a>Пропуск удаления учетных записей пользователей, которые выходят за пределы области

По умолчанию подсистема подготовки Azure AD удаляет или отключает пользователей, которые выходят за пределы области. Однако в некоторых сценариях, например, при входящей подготовке пользователя Workday к AD, такое поведение может отличаться от ожидаемого, и может потребоваться переопределить это поведение по умолчанию.  

В этом руководство описано, как использовать API Microsoft Graph и Microsoft Graph Explorer, чтобы установить флаг ***скипаутофскопеделетионс*** , управляющий обработкой учетных записей, которые выходят за пределы области. 
* Если для ***скипаутофскопеделетионс*** задано значение 0 (false), то учетные записи, выходящие за пределы области, будут отключены в целевом объекте
* Если для ***скипаутофскопеделетионс*** задано значение 1 (true), то учетные записи, выходящие за пределы области, не будут отключены в целевом объекте. Этот флаг устанавливается на уровне *приложения подготовки* и может быть настроен с помощью API Graph. 

Так как эта конфигурация широко используется с *workday для Active Directory приложения подготовки пользователей* , приведенные ниже действия включают снимки экрана приложения workday. Однако это также можно использовать со **всеми другими приложениями** (ServiceNow, Salesforce, Dropbox и т. д.).

## <a name="step-1-retrieve-your-provisioning-app-service-principal-id-object-id"></a>Шаг 1. Получение идентификатора субъекта-службы приложения подготовки (идентификатор объекта)

1. Запустите [портал Azure](https://portal.azure.com)и перейдите к разделу Properties приложения подготовки. Например, если вы хотите экспортировать *приложение "Workday в службу подготовки пользователей* ", перейдите в раздел свойств этого приложения. 
1. В разделе "Свойства" вашего приложения подготовки скопируйте значение GUID, связанное с полем *Идентификатор объекта*. Это значение также называется идентификатором **ServicePrincipalId** вашего приложения, и оно будет использоваться в операциях обозревателя Graph.

   ![Идентификатор субъекта-службы приложения Workday](./media/skip-out-of-scope-deletions/wd_export_01.png)

## <a name="step-2-sign-into-microsoft-graph-explorer"></a>Шаг 2. вход в Microsoft Graph Explorer

1. Запустите [обозреватель Microsoft Graph](https://developer.microsoft.com/graph/graph-explorer)
1. Нажмите кнопку "Вход с помощью учетной записи Майкрософт" и войдите, используя учетные данные глобального администратора Azure AD или администратора приложения.

    ![Выполнение входа в Graph](./media/skip-out-of-scope-deletions/wd_export_02.png)

1. После выполнения входа вы увидите данные учетной записи пользователя на левой панели.

## <a name="step-3-get-existing-app-credentials-and-connectivity-details"></a>Шаг 3. получение учетных данных и сведений о подключении для существующих приложений

В обозревателе Microsoft Graph выполните следующий запрос GET, заменив [servicePrincipalId] на атрибут **ServicePrincipalId**, извлеченный на [шаге 1](#step-1-retrieve-your-provisioning-app-service-principal-id-object-id).

```http
   GET https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/secrets
```

   ![ПОЛУЧИТЬ запрос задания](./media/skip-out-of-scope-deletions/skip-03.png)

Скопируйте ответ в текстовый файл. Он будет выглядеть как текст JSON, показанный ниже, со значениями, выделенными желтым цветом, относящимся к вашему развертыванию. Добавьте линии, выделенные зеленым цветом, в конец и обновите пароль подключения Workday, выделенный синим цветом. 

   ![ПОЛУЧЕНИЕ ответа задания](./media/skip-out-of-scope-deletions/skip-04.png)

Ниже приведен блок JSON для добавления к сопоставлению. 

```json
        {
            "key": "SkipOutOfScopeDeletions",
            "value": "True"
        }
```

## <a name="step-4-update-the-secrets-endpoint-with-the-skipoutofscopedeletions-flag"></a>Шаг 4. обновление конечной точки секреты с помощью флага Скипаутофскопеделетионс

В обозревателе Graph выполните следующую команду, чтобы обновить конечную точку секреты с помощью флага ***скипаутофскопеделетионс*** . 

В приведенном ниже URL-адресе замените [servicePrincipalId] на **servicePrincipalId** , извлеченный из [шага 1](#step-1-retrieve-your-provisioning-app-service-principal-id-object-id). 

```http
   PUT https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/secrets
```
Скопируйте обновленный текст из шага 3 в текст запроса и задайте для заголовка "Content-Type" значение "Application/JSON" в заголовках запроса. 

   ![запрос PUT](./media/skip-out-of-scope-deletions/skip-05.png)

Щелкните "выполнить запрос". 

Вы должны получить выходные данные в виде "Success – Code Status 204". 

   ![РАЗМЕСТИТЬ ответ](./media/skip-out-of-scope-deletions/skip-06.png)

## <a name="step-5-verify-that-out-of-scope-users-dont-get-disabled"></a>Шаг 5. Убедитесь, что пользователи вне области не отключены

Вы можете проверить, что этот флаг приводит к ожидаемому поведению, обновив правила области, чтобы пропустить определенного пользователя. В приведенном ниже примере мы исключены сотрудник с ИДЕНТИФИКАТОРом 21173 (который ранее находился в области действия), добавив новое правило области. 

   ![Пример области](./media/skip-out-of-scope-deletions/skip-07.png)

В следующем цикле подготовки Служба подготовки Azure AD определит, что пользователь 21173 выходит из области действия, и если свойство Скипаутофскопеделетионс включено, то правило синхронизации для этого пользователя отобразит сообщение, как показано ниже: 

   ![Пример области](./media/skip-out-of-scope-deletions/skip-08.png)


