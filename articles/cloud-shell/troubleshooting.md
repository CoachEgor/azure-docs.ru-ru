---
title: Устранение неполадок Azure Cloud Shell | Документация Майкрософт
description: Устранение неполадок Azure Cloud Shell.
services: azure
documentationcenter: ''
author: maertendMSFT
manager: hemantm
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: damaerte
ms.openlocfilehash: 0b1b22095c77344ed71762d3d51b12f19d9f1811
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "79458058"
---
# <a name="troubleshooting--limitations-of-azure-cloud-shell"></a>Устранение неполадок и ограничения в Azure Cloud Shell

Ниже описаны решения известных проблем в Azure Cloud Shell.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="general-troubleshooting"></a>Общие действия по устранению неполадок

### <a name="error-running-azuread-cmdlets-in-powershell"></a>Ошибка под управлением AzureAD cmdlets в PowerShell

- **Детали**: При запуске cmdlets AzureAD, как `Get-AzureADUser` в облачной оболочке, вы можете увидеть ошибку: `You must call the Connect-AzureAD cmdlet before calling any other cmdlets`. 
- **Разрешение**: `Connect-AzureAD` Запуск cmdlet. Ранее Cloud Shell автоматически запускала этот cmdlet во время запуска PowerShell. Чтобы ускорить время запуска, cmdlet больше не работает автоматически. Восстановить предыдущее поведение можно, `Connect-AzureAD` добавив в $PROFILE файл в PowerShell.

### <a name="early-timeouts-in-firefox"></a>Преждевременное истечение времени ожидания в FireFox

- **Сведения.** Cloud Shell использует открытый WebSocket для передачи входящих и исходящих значений в браузер. FireFox включает предварительно заданные политики, которые могут преждевременно закрыть подключение по протоколу WebSocket, что может стать причиной преждевременного истечения времени ожидания в Cloud Shell.
- **Решение.** Откройте FireFox и перейдите в область about:config в поле URL-адреса. Введите в строку поиска network.websocket.timeout.ping.request и измените значение 0 на 10.

### <a name="disabling-cloud-shell-in-a-locked-down-network-environment"></a>Отключение Cloud Shell в заблокированной среде сети

- **Сведения**. Администраторам может потребоваться отключить доступ к Cloud Shell для своих пользователей. Cloud Shell использует доступ `ux.console.azure.com` к домену, который может быть отказано, останавливая любой доступ к точкам входа ВХЛ, включая portal.azure.com, shell.azure.com, расширение кода Visual Studio Azure и docs.microsoft.com. В облаке правительства США `ux.console.azure.us`точка входа; соответствующих shell.azure.us нет.
- **Разрешение**: Ограничивайте доступ к `ux.console.azure.com` настройкам сети или `ux.console.azure.us` через нее в ней. Значок облачной оболочки будет по-прежнему существовать на портале Azure, но не будет успешно подключаться к службе.

### <a name="storage-dialog---error-403-requestdisallowedbypolicy"></a>Диалоговое окно службы хранилища. Ошибка: 403 RequestDisallowedByPolicy

- **Детали:** При создании учетной записи хранилища через облачную оболочку она не работает из-за политики Azure, размещенной вашим администратором. Сообщение об ошибке будет включать в себя:`The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **Решение.** Обратитесь к администратору Azure с просьбой удалить или обновить политику Azure, которая отклоняет создание хранилища.

### <a name="storage-dialog---error-400-disallowedoperation"></a>Диалоговое окно службы хранилища. Ошибка: 400 DisallowedOperation

- **Сведения.** Не удается создать хранилище, используя подписку Azure Active Directory.
- **Устранение.** Используйте подписку Azure, которая позволяет создавать ресурсы хранилища. Подписки Azure AD не поддерживают создание ресурсов Azure.

### <a name="terminal-output---error-failed-to-connect-terminal-websocket-cannot-be-established-press-enter-to-reconnect"></a>Выходные данные терминала. Ошибка "Failed to connect terminal: websocket cannot be established. Press `Enter` to reconnect" (Не удалось подключиться к терминалу. Невозможно установить подключение по протоколу WebSocket. Нажмите клавишу ВВОД, чтобы подключиться еще раз)
- **Сведения.** Для Cloud Shell требуется возможность установить подключение по протоколу WebSocket к инфраструктуре Cloud Shell.
- **Решение.** Убедитесь, что параметры сети настроены для разрешения отправки HTTP-запросов и запросов WebSocket к доменам по адресу *.console.azure.com.

### <a name="set-your-cloud-shell-connection-to-support-using-tls-12"></a>Настройка поддержки TLS 1.2 в подключении Cloud Shell
 - **Детали**: Чтобы определить версию TLS для подключения к облачной оболочке, необходимо настроить настройки для браузера.
 - **Решение.** Перейдите к параметрам безопасности браузера и установите флажок "Использовать TLS 1.2".

## <a name="bash-troubleshooting"></a>Устранение неполадок в Bash

### <a name="cannot-run-the-docker-daemon"></a>Не удается запустить управляющую программу Docker

- **Сведения**. Cloud Shell использует контейнер для размещения оболочки среды, поэтому выполнение управляющей программы запрещено.
- **Устранение**. Используйте компонент [docker-machine](https://docs.docker.com/machine/overview/), установленный по умолчанию, для управления контейнерами Docker с удаленного узла Docker.

## <a name="powershell-troubleshooting"></a>Устранение неполадок в PowerShell

### <a name="gui-applications-are-not-supported"></a>Приложения с графическим пользовательским интерфейсом не поддерживаются

- **Сведения.** Если пользователь запускает приложение с графическим пользовательским интерфейсом, командная строка не отображается. Например, когда клонируется частный репозиторий GitHub с поддержкой двухфакторной проверки подлинности, отображается диалоговое окно выполнения двухфакторной проверки подлинности.
- **Разрешение.** Закройте и снова откройте оболочку.

### <a name="troubleshooting-remote-management-of-azure-vms"></a>Устранение неполадок удаленного управления виртуальными машинами Azure
> [!NOTE]
> Виртуальные машины Azure должны иметь общедоступный IP-адрес.

- **Сведения**. Из-за параметров брандмауэра Windows по умолчанию для WinRM может появиться следующее сообщение об ошибке: `Ensure the WinRM service is running. Remote Desktop into the VM for the first time and ensure it can be discovered.`.
- **Разрешение**. Запустите `Enable-AzVMPSRemoting`, чтобы включить на целевом компьютере все аспекты удаленного взаимодействия PowerShell.

### <a name="dir-does-not-update-the-result-in-azure-drive"></a>`dir` не обновляет результат на диске Azure

- **Сведения.** Чтобы оптимизировать взаимодействие с пользователем, по умолчанию результаты `dir` кэшируются на диск Azure.
- **Решение.** После создания, обновления или удаления ресурса Azure выполните команду `dir -force` для обновления результатов на диске Azure.

## <a name="general-limitations"></a>Общие ограничения

Azure Cloud Shell имеет следующие известные ограничения:

### <a name="quota-limitations"></a>Ограничения квот

Azure Cloud Shell имеет лимит в 20 одновременных пользователей на одного клиента в регионе. Если вы попытаетесь открыть более одновременные сеансы, чем предел, вы увидите ошибку "Арендатор пользователя над квотой". Если у вас есть законная потребность иметь больше сессий, открытых, чем это (например, для учебных занятий), обратитесь в службу поддержки до предполагаемого использования, чтобы запросить увеличение квоты.

Облачная оболочка предоставляется как бесплатная услуга и предназначена для настройки среды Azure, а не как вычислительная платформа общего назначения. Чрезмерное автоматизированное использование может быть рассмотрено как нарушение условий обслуживания Azure и может привести к блокировке доступа к оболочке облака.

### <a name="system-state-and-persistence"></a>Состояние системы и сохраняемость

Компьютер, предоставляющий сеанс Cloud Shell, является временным и перезапускается, если сеанс не используется в течение 20 минут. Для Cloud Shell требуется подключение файлового ресурса Azure. Поэтому ваша подписка должна иметь возможность настраивать ресурсы хранилища для доступа к Cloud Shell. Дополнительные рекомендации:

- Если подключено хранилище, то сохраняются только изменения в каталоге `clouddrive`. Кроме того, в Bash сохраняется ваш каталог `$HOME`.
- Файловые ресурсы Azure можно подключить только в пределах [назначенного региона](persisting-shell-storage.md#mount-a-new-clouddrive).
  - В Bash выполните команду `env`, чтобы найти регион, заданный для `ACC_LOCATION`.
- Файлы Azure поддерживают только локально избыточное хранилище в геоизбыточные учетные записи.

### <a name="browser-support"></a>Поддержка браузеров

Cloud Shell поддерживает последние версии следующих браузеров:

- Microsoft Edge
- Microsoft Internet Explorer
- Google Chrome
- Mozilla Firefox
- Apple Safari;
  - Использование Safari в режиме защищенного просмотра не поддерживается.

### <a name="copy-and-paste"></a>Копирование и вставка

[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="usage-limits"></a>Ограничения использования

Cloud Shell предназначен для интерактивного использования. В результате любые длительные неинтерактивные сеансы завершаются без предупреждения.

### <a name="user-permissions"></a>Разрешения пользователя

Разрешения задаются как для обычных пользователей без доступа к sudo. Все, что устанавливается за пределами каталога `$Home`, не сохраняется.

## <a name="bash-limitations"></a>Ограничения Bash

### <a name="editing-bashrc"></a>Изменение файла .bashrc

Будьте внимательны при изменении файла .bashrc, так как некоторые изменения могут привести к непредвиденным ошибкам в Cloud Shell.

## <a name="powershell-limitations"></a>Ограничения PowerShell

### <a name="preview-version-of-azuread-module"></a>предварительная версия модуля AzureAD

В настоящее время доступен модуль `AzureAD.Standard.Preview`. Это предварительная версия на основе .NET Standard. Этот модуль предоставляет такую же функциональность, как `AzureAD`.

### <a name="sqlserver-module-functionality"></a>Функции модуля `SqlServer`

Модуль `SqlServer`, включенный в Cloud Shell, поддерживает PowerShell Core только в режиме предварительного выпуска. В частности, пока недоступно `Invoke-SqlCmd`.

### <a name="default-file-location-when-created-from-azure-drive"></a>Расположение файла по умолчанию при создании с помощью диска Azure

С помощью командлетов PowerShell пользователи не могут создавать файлы на диске Azure. Когда пользователи создают новые файлы с помощью других инструментов, например vim или nano, по умолчанию файлы сохраняются в папке `$HOME`.

### <a name="tab-completion-can-throw-psreadline-exception"></a>Нажатием клавиши TAB для выполнения можно создать исключение PSReadline

Если для пользователя в PSReadline параметр EditMode (Режим редактирования) содержит значение Emacs, то при попытке пользователя отобразить все доступные возможности с помощью клавиши TAB в слишком маленьком для этого окне PSReadline создает необработанное исключение.

### <a name="large-gap-after-displaying-progress-bar"></a>Большое пустое пространство после отображения индикатора выполнения

Если для команды или действия пользователя отображается индикатор выполнения, например в случае запуска нажатием клавиши TAB на диске `Azure:`, может произойти ошибка настройки курсора и вместо индикатора выполнения появится пустое пространство.

### <a name="random-characters-appear-inline"></a>В строке появляются случайные символы

Во входных данных от пользователя могут появляться коды последовательности, управляющей позицией курсора, например `5;13R`. Эти символы можно удалить вручную.

## <a name="personal-data-in-cloud-shell"></a>Персональные данные в Cloud Shell

Azure Cloud Shell серьезно относится к личным данным. Данные, собранные и хранимые службой Azure Cloud Shell, используются для предоставления значений по умолчанию при работе пользователя. Это могут быть последние использованные оболочки, предпочтительный размер шрифта, предпочтительный тип шрифта и сведения о файловом ресурсе для облачного диска. На случай, если вам понадобится экспортировать или удалить эти данные, используйте следующие инструкции.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### <a name="export"></a>Экспорт
Чтобы **экспортировать** пользовательские настройки, сохраненные Cloud Shell, такие как предпочитаемая оболочка, размер и тип шрифта, выполните следующие команды.

1. [![](https://shell.azure.com/images/launchcloudshell.png "Launch Azure Cloud Shell")](https://shell.azure.com)
2. В Bash или PowerShell выполните следующие команды.

Bash:

  ```
  token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
  curl https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token" -s | jq
  ```

PowerShell.

  ```powershell
  $token= ((Invoke-WebRequest -Uri "$env:MSI_ENDPOINT`?resource=https://management.core.windows.net/" -Headers @{Metadata='true'}).content |  ConvertFrom-Json).access_token
  ((Invoke-WebRequest -Uri https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -Headers @{Authorization = "Bearer $token"}).Content | ConvertFrom-Json).properties | Format-List
```

### <a name="delete"></a>DELETE
Чтобы **удалить** пользовательские настройки, сохраненные Cloud Shell, такие как предпочитаемая оболочка, размер и тип шрифта, выполните следующие команды. При следующем запуске Cloud Shell вам будет предложено еще раз выставить файловый ресурс. 

>[!Note]
> При удалении параметров пользователя сам общий ресурс службы файлов Azure не будет удален. Перейдите к службе файлов Azure для выполнения этого действия.

1. [![](https://shell.azure.com/images/launchcloudshell.png "Launch Azure Cloud Shell")](https://shell.azure.com)
2. В Bash или PowerShell выполните следующие команды.

Bash:

  ```
  token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
  curl -X DELETE https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token"
  ```

PowerShell.

  ```powershell
  $token= ((Invoke-WebRequest -Uri "$env:MSI_ENDPOINT`?resource=https://management.core.windows.net/" -Headers @{Metadata='true'}).content |  ConvertFrom-Json).access_token
  Invoke-WebRequest -Method Delete -Uri https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -Headers @{Authorization = "Bearer $token"}
  ```
## <a name="azure-government-limitations"></a>Ограничения правительства Лазурного государства
Облачная оболочка Azure в правительстве Azure доступна только через портал Azure.

>[!Note]
> Подключение к GCC-High или правительство DOD Облака для обмена онлайн в настоящее время не поддерживается.
