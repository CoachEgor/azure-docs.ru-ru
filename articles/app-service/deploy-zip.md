---
title: Развертывание кода в Службе приложений Azure с помощью ZIP- или WAR-файла | Документация Майкрософт
description: Узнайте, как развернуть приложение в Службе приложений Azure с помощью ZIP-файла (или WAR-файла для разработчиков Java).
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: cephalin;sisirap
ms.custom: seodec18
ms.openlocfilehash: a48a72fe36b7925936758e844d959968ea921c65
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130778"
---
# <a name="deploy-your-app-to-azure-app-service-with-a-zip-or-war-file"></a>Развертывание приложения в Службе приложений Azure с помощью ZIP- или WAR-файла

В этой статье показано, как с помощью ZIP- или WAR-файла развернуть веб-приложение в [Службе приложений Azure](overview.md). 

Развертывание из ZIP-файла осуществляется с помощью той же службы Kudu, которая обеспечивает непрерывное развертывание на основе интеграции. Kudu поддерживает следующие возможности развертывания из ZIP-файла: 

- удаление файлов, оставшихся после предыдущего развертывания;
- включение процесса сборки по умолчанию, в том числе восстановления пакетов;
- [настройку развертывания](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings), в том числе выполнение скриптов развертывания;  
- журналы развертывания. 
- Предельный размер 2048 МБ.

Дополнительные сведения см. в [документации по Kudu](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

[WAR](https://wikipedia.org/wiki/WAR_(file_format))-файл развертывается в службе приложений для запуска веб-приложения Java. См. раздел [Развертывание WAR-файла](#deploy-war-file).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Технические условия

Чтобы выполнить действия, описанные в этой статье, сделайте следующее:

* [Создайте приложение службы приложений](/azure/app-service/) или используйте приложение, созданное для работы с другим руководством.

## <a name="create-a-project-zip-file"></a>Создание ZIP-файла проекта

>[!NOTE]
> Если вы загрузили ZIP-файл, сначала извлеките файлы из него. Например, если вы загрузили ZIP-файл из GitHub, его нельзя развернуть "как есть". GitHub добавляет дополнительные вложенные каталоги, которые не работают со Службой приложений. 
>

В окне терминала на локальном компьютере перейдите к корневому каталогу проекта приложения. 

Этот каталог должен содержать файл записи веб-приложения, например _index.html_, _index.php_ и _app.js_. Кроме того, в нем могут содержаться файлы управления пакетами, например _project.json_, _composer.json_, _package.json_, _bower.json_ и _requirements.txt_.

Создайте ZIP-архив всего содержимого проекта. При выполнении следующей команды в окне терминала используется инструмент по умолчанию:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

[!INCLUDE [Deploy ZIP file](../../includes/app-service-web-deploy-zip.md)]

## <a name="deploy-zip-file-with-azure-cli"></a>Развертывание ZIP-файла с помощью с Azure CLI

Используйте Azure CLI 2.0.21 или более поздней версии. Чтобы просмотреть текущую версию, выполните команду `az --version` в окне терминала.

Разверните загруженный ZIP-файл в веб-приложение с помощью команды [az webapp deployment source config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-zip).  

Следующий пример развертывает загруженный ZIP-файл. При использовании локальной установки Azure CLI укажите путь к локальному ZIP-файлу `--src`.

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResourceGroup --name <app_name> --src clouddrive/<filename>.zip
```

Эта команда позволяет развертывать файлы и каталоги из ZIP-файлов в папку для приложения службы приложений по умолчанию (`\home\site\wwwroot`). Затем приложение перезапускается.

По умолчанию подсистема развертывания предполагает, что ZIP-файл готов к выполнению как-не запускать любой автоматизации сборки. Чтобы включить тот же сборки автоматизации, как показано на [развертывание Git](deploy-local-git.md), задайте `SCM_DO_BUILD_DURING_DEPLOYMENT` параметр приложения, выполнив следующую команду в [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings SCM_DO_BUILD_DURING_DEPLOYMENT=true
```



Дополнительные сведения см. в [документации по Kudu](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file-or-url).

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]  

## <a name="deploy-war-file"></a>Развертывание WAR-файла

Чтобы развернуть WAR-файл в службе приложений, отправьте запрос POST по адресу https://<имя_приложения>.scm.azurewebsites.net/api/zipdeploy. В тексте сообщения запроса POST должен содержаться WAR-файл. Учетные данные развертывания для приложения указываются в запросе с использованием обычной проверки подлинности HTTP. 

Для обычной аутентификации HTTP требуются учетные данные развертывания службы приложений. См. дополнительные сведения об [установке и сбросе учетных данных на уровне пользователя](deploy-configure-credentials.md#userscope).

### <a name="with-curl"></a>Использование cURL

В примере ниже для развертывания WAR-файла используется средство cURL. Замените заполнители `<username>`, `<war_file_path>` и `<app_name>`. Когда в cURL появится запрос, введите пароль.

```bash
curl -X POST -u <username> --data-binary @"<war_file_path>" https://<app_name>.scm.azurewebsites.net/api/wardeploy
```

### <a name="with-powershell"></a>С помощью PowerShell

В примере ниже для отправки запроса, содержащего WAR-файл, используется [Invoke-RestMethod](/powershell/module/microsoft.powershell.utility/invoke-restmethod). Замените заполнители `<deployment_user>`, `<deployment_password>`, `<zip_file_path>` и `<app_name>`.

```powershell
$username = "<deployment_user>"
$password = "<deployment_password>"
$filePath = "<war_file_path>"
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/wardeploy"
$base64AuthInfo = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $username, $password)))
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -Method POST -InFile $filePath -ContentType "multipart/form-data"
```

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Дальнейшие действия

Чтобы изучить более сложные сценарии развертывания, ознакомьтесь с [развертыванием в Azure с помощью Git](deploy-local-git.md). Развертывание в Azure на основе Git обеспечивает систему управления версиями, восстановление пакета, MSBuild и многое другое.

## <a name="more-resources"></a>Дополнительные ресурсы

* [Kudu: Deploying from a zip file](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file) (Kudu. Развертывание из ZIP-файла)
* [Учетные данные развертывания службы приложений Azure](deploy-ftp.md)
