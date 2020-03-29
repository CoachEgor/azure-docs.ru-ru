---
title: Конфиденциальность пользователей и сквозная аутентификация Azure Active Directory | Документация Майкрософт
description: В этой статье описывается сквозная аутентификация Azure Active Directory (Azure AD) и соответствие GDPR.
services: active-directory
keywords: Azure AD Connect Pass-through Authentication, GDPR, required components for Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/23/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0af1c42e7e2c163e7f9e7407d0236e35bfacf8e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "76931010"
---
# <a name="user-privacy-and-azure-active-directory-pass-through-authentication"></a>Конфиденциальность пользователей и сквозная аутентификация Azure Active Directory


[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview"></a>Обзор

Сквозная аутентификация Azure AD создает следующие типы журналов, которые могут содержать персональные данные:

- файлы журнала трассировки Azure AD Connect;
- файлы журнала трассировки агента проверки подлинности;
- файлы журнала событий Windows.

Конфиденциальность пользователей для сквозной аутентификации можно повысить двумя способами:

1.  по запросу извлекать данные для пользователя и удалять эти данные из установленных решений;
2.  Не хранить данные больше 48 часов.

Настоятельно рекомендуем использовать второй вариант, так как его проще реализовать и обслуживать. См. приведенные ниже инструкции по каждому типу журналов.

### <a name="delete-azure-ad-connect-trace-log-files"></a>Удаление файлов журнала трассировки Azure AD Connect

Проверьте содержимое папки **%ProgramData%\AADConnect** и удалите содержимое журнала трассировки (файлы **trace-\*.log**) из этой папки в течение 48 часов после установки или обновления Azure AD Connect либо изменения конфигурации для сквозной аутентификации, так как при этих действиях создаются данные, на которые распространяются требования GDPR.

>[!IMPORTANT]
>Не удаляйте файл **PersistedState.xml** из этой папки. Он нужен для хранения состояния предыдущей установки Azure AD Connect и используется при установке обновлений. В этом файле никогда не хранятся личные данные о пользователях, и его не следует удалять.

Вы можете просмотреть и удалить файлы журналов трассировки с помощью проводника Windows или использовать приведенный ниже скрипт PowerShell.

```
$Files = ((Get-Item -Path "$env:programdata\aadconnect\trace-*.log").VersionInfo).FileName 
 
Foreach ($file in $Files) { 
    {Remove-Item -Path $File -Force} 
}
```

Сохраните скрипт в файле с расширением PS1. Запускайте его по мере необходимости.

Дополнительные сведения о требованиях GDPR для Azure AD Connect см. [в этой статье](reference-connect-user-privacy.md).

### <a name="delete-authentication-agent-event-logs"></a>Удаление журналов событий агента аутентификации

Этот продукт может также создавать **журналы событий Windows**. Дополнительные сведения см. в [этой статье](https://msdn.microsoft.com/library/windows/desktop/aa385780(v=vs.85).aspx).

Чтобы посмотреть журналы, связанные с агентом сквозной аутентификации, откройте на сервере приложение **Просмотр событий** и проверьте расположение **Application and Service Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin**.

### <a name="delete-authentication-agent-trace-log-files"></a>Удаление файлов журнала трассировки агента проверки подлинности

Вы должны регулярно проверять содержимое **%ProgramData% -Microsoft-Azure AD Connect Агент аутентификации и** удалять содержимое этой папки каждые 48 часов. 

>[!IMPORTANT]
>Если запущена служба агента аутентификации, вы не сможете удалить текущий файл журнала в этой папке. Остановите службу и повторите попытку. Чтобы избежать ошибок со входом пользователей, следует заранее настроить [высокий уровень доступности](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability) для сквозной аутентификации.

Вы можете просмотреть и удалить эти файлы с помощью проводника Windows или использовать приведенный ниже сценарий.

```
$Files = ((Get-childitem -Path "$env:programdata\microsoft\azure ad connect authentication agent\trace" -Recurse).VersionInfo).FileName 
 
Foreach ($file in $files) { 
    {Remove-Item -Path $File -Force} 
}
```

Запланируйте выполнение этого сценария через каждые 48 часов, используя следующие шаги:

1.  Сохраните скрипт в файле с расширением PS1.
2.  Откройте **Панель управления** и выберите элемент **Система и безопасность**.
3.  Под заголовком **«Административные инструменты»** нажмите на **«Расписание задач».**
4.  В **планировщике задач** нажмите правой кнопкой мыши команду **Библиотека расписания задач** и щелкните **Создать простую задачу**.
5.  Введите имя для новой задачи и нажмите кнопку **Далее**.
6.  Выберите **Ежедневно** в поле **Триггер задачи** и щелкните **Далее**.
7.  Установите значение периодичности "2 дня" и щелкните **Далее**.
8.  Выберите действие **Запуск программы** и щелкните **Далее**.
9.  Введите "**PowerShell**" в поле для программы / скрипта, и в поле помечены "**Добавить аргументы (необязательно)**", введите полный путь к сценарию, который вы создали ранее, а затем нажмите **Далее**.
10. На следующем экране показана сводка задачи, которую вы собираетесь создать. Проверьте значения и нажмите **Закончить,** чтобы создать задачу:
 
### <a name="note-about-domain-controller-logs"></a>Примечание о журналах контроллеров домена

Если включено ведение журнала аудита, этот продукт может создавать журналы безопасности для контроллеров домена. Дополнительные сведения о настройке политик аудита см. в [этой статье](https://technet.microsoft.com/library/dd277403.aspx).

## <a name="next-steps"></a>Дальнейшие действия
* [Просмотр политики конфиденциальности корпорации Майкрософт в центре управления безопасностью](https://www.microsoft.com/trustcenter)
* [**Troubleshoot**](tshoot-connect-pass-through-authentication.md) - Узнайте, как решить общие проблемы с функцией.
