---
title: Устранение неполадок службы файлов Azure в Windows | Документация Майкрософт
description: Сведения об устранении неполадок со службой файлов Azure в Windows.
author: jeffpatt24
ms.service: storage
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: jeffpatt
ms.subservice: files
ms.openlocfilehash: 17ecc80fee3b024c334b8d36533663f1f3cebe4d
ms.sourcegitcommit: 05a650752e9346b9836fe3ba275181369bd94cf0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2020
ms.locfileid: "79136911"
---
# <a name="troubleshoot-azure-files-problems-in-windows"></a>Устранение неполадок службы файлов Azure в Windows

В этой статье приведен список распространенных проблем, возникающих в службе файлов Microsoft Azure при подключении из клиентов Windows. Кроме того, здесь представлены возможные причины этих проблем и способы их устранения. Помимо действий по устранению неполадок, описываемых в этой статье, можно также использовать [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) , чтобы обеспечить выполнение необходимых условий для среды клиента Windows. AzFileDiagnostics автоматизирует обнаружение большинства симптомов, упомянутых в этой статье, и помогает настроить среду для достижения оптимальной производительности. Эту информацию можно также найти в статье [Troubleshooter for Azure Files storage problems](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) (Устранение неполадок в хранилище файлов Azure), где приведены пошаговые инструкции по устранению проблем с подключением и сопоставлением общих ресурсов Azure.

<a id="error5"></a>
## <a name="error-5-when-you-mount-an-azure-file-share"></a>Ошибка 5 при подключении файлового ресурса Azure

При попытке подключить файловый ресурс может выдаваться следующая ошибка:

- "Произошла системная ошибка 5. Отказано в доступе".

### <a name="cause-1-unencrypted-communication-channel"></a>Причина 1: незашифрованный коммуникационный канал

Из соображений безопасности, если коммуникационный канал не зашифрован, а попытка подключения осуществляется не из того же центра обработки данных, в котором находятся файловые ресурсы Azure, то подключение к ним Azure будет заблокировано. Незашифрованные подключения в одном центре обработки данных также могут быть заблокированы, если в учетной записи хранения включен параметр [Требуется безопасное перемещение](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer). Шифрование коммуникационного канала выполняется, только если клиентская ОС пользователя поддерживает шифрование SMB.

Windows 8, Windows Server 2012 или более поздние версии этих ОС отправляют запрос на согласование, который включает протокол в себя SMB 3.0, поддерживающий шифрование.

### <a name="solution-for-cause-1"></a>Решение для причины 1

1. Подключитесь из клиента, поддерживающего шифрование SMB (Windows 8, Windows Server 2012 или более поздней версии), или из виртуальной машины, размещенной в том же центре обработки данных, что и учетная запись хранения Azure, используемая для файлового ресурса Azure.
2. Убедитесь, что параметр [Требуется безопасное перемещение](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) отключен в учетной записи хранения, если клиент не поддерживает шифрование SMB.

### <a name="cause-2-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>Причина 2. в учетной записи хранения включены виртуальная сеть или правила брандмауэра. 

Если для учетной записи хранения настроены правила виртуальной сети и брандмауэра, доступ сетевого трафика будет запрещен, если IP-адрес клиента или виртуальная сеть не разрешают доступ.

### <a name="solution-for-cause-2"></a>Решение для причины 2

Убедитесь, что правила виртуальной сети и брандмауэра настроены надлежащим образом для учетной записи хранения. Чтобы проверить, связана ли проблема с правилами виртуальной сети или брандмауэра, временно задайте для учетной записи хранения параметр **Разрешить доступ из всех сетей**. Подробнее см. в статье [Настройка брандмауэров службы хранилища Azure и виртуальных сетей (предварительная версия)](https://docs.microsoft.com/azure/storage/common/storage-network-security).

### <a name="cause-3-share-level-permissions-are-incorrect-when-using-identity-based-authentication"></a>Причина 3. при использовании проверки подлинности на основе удостоверений неправильные разрешения на уровне общего ресурса

Если пользователи обращаются к файловому ресурсу Azure с помощью проверки подлинности Active Directory (AD) или Azure Active Directory доменных служб (Azure AD DS), доступ к общей папке завершится ошибкой "доступ запрещен", если разрешения уровня общего доступа неверны. 

### <a name="solution-for-cause-3"></a>Решение для причины 3

Сведения об обновлении разрешений на уровне общего ресурса см. в разделе [Назначение разрешений на доступ к удостоверению](https://docs.microsoft.com/azure/storage/files/storage-files-identity-auth-active-directory-domain-service-enable#assign-access-permissions-to-an-identity).

<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a>Ошибка 53, 67 или 87 при попытке подключения или отключения файлового ресурса Azure

При попытке подключить файловый ресурс из локальной среды или другого центра обработки данных могут возникнуть следующие ошибки:

- "Произошла системная ошибка 53. Сетевой путь не найден".
- "Произошла системная ошибка 67. Не найдено сетевое имя".
- "Произошла системная ошибка 87. Неправильный параметр".

### <a name="cause-1-port-445-is-blocked"></a>Причина 1: порт 445 заблокирован

Системная ошибка 53 или 67 может произойти, если исходящие подключения через порт 445 к центру обработки данных службы файлов Azure заблокированы. Чтобы просмотреть сведения о поставщиках услуг Интернета, поддерживающих (или не поддерживающих) подключение через порт 445, перейдите на сайт [TechNet](https://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).

Чтобы проверить, не блокирует ли брандмауэр или поставщик услуг Интернета порт 445, используйте средство [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) или командлет `Test-NetConnection`. 

Чтобы использовать командлет `Test-NetConnection`, необходимо установить модуль Azure PowerShell. Дополнительные сведения см. в разделе об [установке Azure PowerShell Module](/powershell/azure/install-Az-ps) . Не забудьте заменить `<your-storage-account-name>` и `<your-resource-group-name>` соответствующими именами для вашей учетной записи хранения.

   
    $resourceGroupName = "<your-resource-group-name>"
    $storageAccountName = "<your-storage-account-name>"

    # This command requires you to be logged into your Azure account, run Login-AzAccount if you haven't
    # already logged in.
    $storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName

    # The ComputerName, or host, is <storage-account>.file.core.windows.net for Azure Public Regions.
    # $storageAccount.Context.FileEndpoint is used because non-Public Azure regions, such as sovereign clouds
    # or Azure Stack deployments, will have different hosts for Azure file shares (and other storage resources).
    Test-NetConnection -ComputerName ([System.Uri]::new($storageAccount.Context.FileEndPoint).Host) -Port 445
    
Если установка прошла успешно, отобразятся следующие выходные данные.
    
  
    ComputerName     : <your-storage-account-name>
    RemoteAddress    : <storage-account-ip-address>
    RemotePort       : 445
    InterfaceAlias   : <your-network-interface>
    SourceAddress    : <your-ip-address>
    TcpTestSucceeded : True
 

> [!Note]  
> Вышеуказанная команда возвращает текущий IP-адрес учетной записи хранения. Не гарантируется, что IP-адрес будет постоянным. Он может измениться в любое время. Не следует жестко задавать этот IP-адрес в скриптах или в конфигурации брандмауэра.

### <a name="solution-for-cause-1"></a>Решение для причины 1

#### <a name="solution-1---use-azure-file-sync"></a>Решение 1. использование Синхронизация файлов Azure
Синхронизация файлов Azure можете преобразовать локальный сервер Windows Server в оперативный кэш файлового ресурса Azure. Для локального доступа к данным вы можете использовать любой протокол, доступный в Windows Server, в том числе SMB, NFS и FTPS. Синхронизация файлов Azure работает через порт 443 и, таким образом, может использоваться в качестве обходного пути для доступа к файлам Azure с клиентов с заблокированным портом 445. [Узнайте, как настроить синхронизация файлов Azure](https://docs.microsoft.com/azure/storage/files/storage-sync-files-extend-servers).

#### <a name="solution-2---use-vpn"></a>Решение 2. использование VPN
Настроив VPN для конкретной учетной записи хранения, трафик будет проходить через защищенный туннель, а не через Интернет. Следуйте [инструкциям, чтобы настроить VPN](storage-files-configure-p2s-vpn-windows.md) для доступа к службе файлов Azure из Windows.

#### <a name="solution-3---unblock-port-445-with-help-of-your-ispit-admin"></a>Решение 3. Разблокируйте порт 445 с помощью поставщика услуг Интернета или ИТ-администратора
Обратитесь к своему ИТ-отделу или поставщику услуг Интернета, чтобы открыть порт 445 для исходящего трафика в [диапазоны IP Azure](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="solution-4---use-rest-api-based-tools-like-storage-explorerpowershell"></a>Решение 4. Использование средств на основе REST API, таких как Обозреватель службы хранилища/PowerShell
Кроме того, служба "файлы Azure" поддерживает все остальное в дополнение к SMB. Доступ к ОСТАВШИМся данным осуществляется через порт 443 (стандартный протокол TCP). Существуют различные средства, написанные с помощью REST API, которые позволяют работать с многофункциональным ИНТЕРФЕЙСом. [Обозреватель службы хранилища](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) является одним из них. [Скачайте и установите обозреватель службы хранилища](https://azure.microsoft.com/features/storage-explorer/) и подключитесь к общей папке с помощью службы файлов Azure. Также можно использовать [PowerShell](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-powershell) , который также REST API пользователем.

### <a name="cause-2-ntlmv1-is-enabled"></a>Причина 2: Аутентификация ntlmv1 включен

Системная ошибка 53 или 87 может произойти, если в клиенте включена аутентификация NTLMv1. Служба файлов Azure поддерживает только проверку подлинности NTLMv2. Протокол проверки подлинности NTLMv1 делает клиент менее безопасным, в результате чего взаимодействие со службой файлов Azure блокируется. 

Чтобы определить, заключается ли причина ошибки в этом, убедитесь, что для следующего подраздела реестра задано значение 3:

**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**

Дополнительные сведения см. в статье [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) на сайте TechNet.

### <a name="solution-for-cause-2"></a>Решение для причины 2

Верните параметру **LmCompatibilityLevel** значение по умолчанию (3) в следующем подразделе реестра:

  **HKLM\SYSTEM\CurrentControlSet\Control\Lsa**

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-to-process-this-command-when-you-copy-to-an-azure-file-share"></a>Ошибка 1816 "Недостаточно квот для обработки команды" при копировании в файловый ресурс Azure

### <a name="cause"></a>Причина

Ошибка 1816 возникает, когда достигнуто максимальное количество одновременно открытых дескрипторов для файла на компьютере, к которому подключается файловый ресурс.

### <a name="solution"></a>Решение

Сократите количество одновременно открытых дескрипторов, закрыв некоторые из них, и повторите попытку. Дополнительные сведения см. в статье [Производительность хранилища Microsoft Azure и контрольный список масштабируемости](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

Чтобы просмотреть открытые дескрипторы для общей папки, каталога или файла, используйте командлет PowerShell [Get-азсторажефилехандле](https://docs.microsoft.com/powershell/module/az.storage/get-azstoragefilehandle) .  

Чтобы закрыть открытые дескрипторы для общей папки, каталога или файла, используйте командлет PowerShell [Close-азсторажефилехандле](https://docs.microsoft.com/powershell/module/az.storage/close-azstoragefilehandle) .

> [!Note]  
> Командлеты Get-Азсторажефилехандле и Close-Азсторажефилехандле включены в раздел AZ PowerShell Module версии 2,4 или более поздней. Чтобы установить последний модуль AZ PowerShell, см. статью [Установка модуля Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

<a id="noaaccessfailureportal"></a>
## <a name="error-no-access-when-you-try-to-access-or-delete-an-azure-file-share"></a>Ошибка "нет доступа" при попытке доступа или удаления файлового ресурса Azure  
При попытке доступа или удаления файлового ресурса Azure на портале может появиться следующее сообщение об ошибке:

Нет доступа  
Код ошибки: 403 

### <a name="cause-1-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>Причина 1. в учетной записи хранения включены виртуальная сеть или правила брандмауэра

### <a name="solution-for-cause-1"></a>Решение для причины 1

Убедитесь, что правила виртуальной сети и брандмауэра настроены надлежащим образом для учетной записи хранения. Чтобы проверить, связана ли проблема с правилами виртуальной сети или брандмауэра, временно задайте для учетной записи хранения параметр **Разрешить доступ из всех сетей**. Подробнее см. в статье [Настройка брандмауэров службы хранилища Azure и виртуальных сетей (предварительная версия)](https://docs.microsoft.com/azure/storage/common/storage-network-security).

### <a name="cause-2-your-user-account-does-not-have-access-to-the-storage-account"></a>Причина 2. у вашей учетной записи пользователя нет доступа к учетной записи хранения

### <a name="solution-for-cause-2"></a>Решение для причины 2

Перейдите в учетную запись хранения, в которой размещается общий файловый ресурс Azure, щелкните **Управление доступом (IAM)** и убедитесь, что учетная запись пользователя имеет доступ к учетной записи хранения. Подробнее см. в статье [Защита учетной записи хранения с помощью управления доступом на основе ролей (RBAC)](https://docs.microsoft.com/azure/storage/blobs/security-recommendations#data-protection).

<a id="open-handles"></a>
## <a name="unable-to-delete-a-file-or-directory-in-an-azure-file-share"></a>Не удается удалить файл или каталог в общем файловом ресурсе Azure
При попытке удалить файл может появиться следующее сообщение об ошибке:

Указанный ресурс помечен для удаления клиентом SMB.

### <a name="cause"></a>Причина
Эта проблема обычно возникает, если файл или каталог имеет открытый обработчик. 

### <a name="solution"></a>Решение

Если клиенты SMB закрыли все открытые дескрипторы и проблема будет повторяться, выполните следующие действия.

- Используйте командлет PowerShell [Get-азсторажефилехандле](https://docs.microsoft.com/powershell/module/az.storage/get-azstoragefilehandle) для просмотра открытых дескрипторов.

- Чтобы закрыть открытые дескрипторы, используйте командлет PowerShell [Close-азсторажефилехандле](https://docs.microsoft.com/powershell/module/az.storage/close-azstoragefilehandle) . 

> [!Note]  
> Командлеты Get-Азсторажефилехандле и Close-Азсторажефилехандле включены в раздел AZ PowerShell Module версии 2,4 или более поздней. Чтобы установить последний модуль AZ PowerShell, см. статью [Установка модуля Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-windows"></a>Медленное копирование файлов в службе файлов Azure и из нее в Windows

При попытке передать файлы в службу файлов Azure можно наблюдать низкую производительность.

- Если определенные требования к минимальному размеру операций ввода-вывода отсутствуют, для оптимальной производительности мы рекомендуем использовать 1 МиБ.
-   Если окончательный размер файла, в который выполняются операции записи, известен, а у программного обеспечения нет проблем совместимости, и если еще не записанный заключительный фрагмент файла содержит нули, укажите размер файла заранее вместо того, чтобы расширять его для каждой операции записи.
-   Используйте правильный метод копирования:
    -   Используйте [AZCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) для передачи данных между двумя файловыми ресурсами.
    -   Используйте [Robocopy](/azure/storage/files/storage-files-deployment-guide#robocopy) для передачи данных между файловыми ресурсами и локальным компьютером.

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a>Рекомендации для Windows 8.1 или Windows Server 2012 R2

Для клиентов, использующих Windows 8.1 или Windows Server 2012 R2, следует установить исправление [KB3114025](https://support.microsoft.com/help/3114025). Это исправление повышает производительность операций создания и закрытия дескрипторов.

Выполните следующий сценарий, чтобы проверить, установлено ли это исправление.

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Если это так, выводятся следующие данные:

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> Начиная с декабря 2015 года в образах Windows Server 2012 R2, содержащихся в Azure Marketplace, исправление KB3114025 установлено по умолчанию.

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer-or-this-pc"></a>Нет папки с буквой диска в "Мой компьютер" или "этом компьютере"

Если вы сопоставили файловый ресурс Azure от имени администратора с помощью команды net use, то может показаться, что он отсутствует.

### <a name="cause"></a>Причина

По умолчанию проводник не запускается от имени администратора. При выполнении команды net use из командной строки администрирования пользователь подключает сетевой диск от имени администратора. Подключенные диски ориентированы на пользователя. Если для их подключения использовалась одна учетная запись, а пользователь вошел в систему с помощью другой, то диски отображаться не будут.

### <a name="solution"></a>Решение
Подключите общую папку из командной строки без прав администратора. Кроме того, настройте значение регистра [EnableLinkedConnections](https://technet.microsoft.com/library/ee844140.aspx), следуя указаниям в **этой статье TechNet**.

<a id="netuse"></a>
## <a name="net-use-command-fails-if-the-storage-account-contains-a-forward-slash"></a>Если учетная запись хранения содержит косую черту (/), то выполнение команды net use завершается сбоем

### <a name="cause"></a>Причина

Команда net use интерпретирует косую черту (/) как параметр командной строки. Если имя учетной записи пользователя начинается с косой черты, то сопоставление диска завершится сбоем.

### <a name="solution"></a>Решение

Чтобы обойти эту проблему, выполните одно из следующих действий.

- Выполните следующую команду PowerShell:

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc"`

  Из пакетного файла выполнить команду можно следующим образом.

  `Echo new-smbMapping ... | powershell -command –`

- Заключите значение ключа в двойные прямые кавычки (если первый знак не является косой чертой). Если значение ключа начинается с косой черты (/), используйте интерактивный режим и введите пароль отдельно или повторно создайте ключи, которые не начинаются с этого знака.

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-files-drive"></a>Приложение или служба не может получить доступ к подключенному диску службы файлов Azure

### <a name="cause"></a>Причина

Диски подключаются для каждого пользователя. Если приложение или служба выполняется не под той учетной записью, к которой относится подключенный диск, то приложение не увидит этот диск.

### <a name="solution"></a>Решение

Используйте одно из следующих решений:

-   Используйте для подключения дисков учетную запись пользователя, содержащую это приложение. Можно использовать такой инструмент, как PsExec.
- Передайте имя и ключ учетной записи хранения в параметры имени пользователя и пароля команды net use.
- Чтобы добавить учетные данные в диспетчере учетных данных, используйте команду cmdkey. Выполните эту команду из командной строки в контексте учетной записи службы либо с помощью интерактивного входа, либо с помощью `runas`.
  
  `cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>`
- Сопоставьте общий ресурс напрямую, не используя букву подключенного диска. Если используется буква диска, некоторые приложения могут неправильно подключиться к диску, поэтому надежнее использовать полный UNC-путь. 

  `net use * \\storage-account-name.file.core.windows.net\share`

После выполнения этих инструкций при выполнении команды net use для учетной записи системы или сети может появиться следующее сообщение об ошибке: "Произошла системная ошибка 1312. A specified logon session does not exist. Возможно, он уже завершен". В этом случае убедитесь, что имя пользователя, переданное в net use, содержит сведения о домене (например, [имя_учетной_записи_хранения].file.core.windows.net).

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>Ошибка "Вы копируете файл в место, которое не поддерживает шифрование"

Когда файл копируется по сети, он расшифровывается на исходном компьютере, передается в виде обычного текста и повторно шифруется в месте назначения. Тем не менее при попытке скопировать зашифрованный файл может появиться следующее сообщение об ошибке: "Вы копируете файл в место, которое не поддерживает шифрование".

### <a name="cause"></a>Причина
Эта проблема может возникнуть при использовании шифрованной файловой системы (EFS). Файлы с шифрованием BitLocker нельзя копировать в службу файлов Azure. Однако эта служба не поддерживает шифрованную файловую систему (EFS) NTFS.

### <a name="workaround"></a>Обходной путь
Чтобы скопировать файл по сети, его сначала необходимо расшифровать. Используйте один из следующих методов.

- Используйте команду **copy /d**. Она позволяет сохранить зашифрованные файлы как расшифрованные файлы в месте назначения.
- Настройте следующий раздел реестра:
  - путь: HKLM\Software\Policies\Microsoft\Windows\System;
  - тип значения: DWORD;
  - Name = CopyFileAllowDecryptedRemoteDestination;
  - Value = 1.

Помните, что настройка раздела реестра влияет на все операции копирования в сетевые общие папки.

## <a name="slow-enumeration-of-files-and-folders"></a>Медленное перечисление файлов и папок

### <a name="cause"></a>Причина

Эта проблема может возникнуть, если на клиентском компьютере для больших каталогов недостаточно кэша.

### <a name="solution"></a>Решение

Чтобы устранить эту проблему, настройте значение реестра **DirectoryCacheEntrySizeMax** разрешать кэшировать большие списки каталогов на компьютере клиента.

- Расположение: HKLM\System\CCS\Services\Lanmanworkstation\Parameters
- Название значения: DirectoryCacheEntrySizeMax 
- Тип значения: DWORD
 
 
Например, его можно присвоить 0x100000 и просмотреть станет ли производительность лучше.

## <a name="error-aaddstenantnotfound-in-enabling-azure-active-directory-domain-service-aad-ds-authentication-for-azure-files-unable-to-locate-active-tenants-with-tenant-id-aad-tenant-id"></a>Ошибка Ааддстенантнотфаунд при включении проверки подлинности службы домена Azure Active Directory (AAD DS) для файлов Azure "не удается нахождение активных клиентов с идентификатором клиента AAD-клиент-ID"

### <a name="cause"></a>Причина

Ошибка Ааддстенантнотфаунд возникает при попытке [включить проверку подлинности Azure Active Directory доменных служб (Azure AD DS) в службе файлов Azure](storage-files-identity-auth-active-directory-domain-service-enable.md) в учетной записи хранения, где [Служба домена AAD (AAD DS)](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) не создана в клиенте AAD связанной подписки.  

### <a name="solution"></a>Решение

Включите доменную службу AAD в клиенте AAD подписки, в которой развернута учетная запись хранения. Вам необходимы права администратора клиента AAD, чтобы создать управляемый домен. Если вы не администратор клиента Azure AD, обратитесь к администратору и следуйте пошаговым инструкциям, чтобы [включить доменные службы Azure Active Directory на портале Azure](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started).

[!INCLUDE [storage-files-condition-headers](../../../includes/storage-files-condition-headers.md)]

## <a name="error-system-error-1359-has-occurred-an-internal-error-received-over-smb-access-to-file-shares-with-azure-active-directory-domain-service-aad-ds-authentication-enabled"></a>Произошла ошибка "системная ошибка 1359. Внутренняя ошибка "полученный через SMB доступ к общим папкам с включенной аутентификацией службы домена Azure Active Directory (AAD DS)

### <a name="cause"></a>Причина

Произошла ошибка "системная ошибка 1359. Произошла внутренняя ошибка "при попытке подключения к файловому ресурсу с использованием проверки подлинности AAD DS в доменных СЛУЖБах AAD с DNS-именем домена, начинающимся с числового символа. Например, если DNS-имя домена доменных служб AAD — "1domain", эта ошибка возникает при попытке подключения общей папки с помощью учетных данных AAD. 

### <a name="solution"></a>Решение

Сейчас можно рассмотреть возможность повторного развертывания Azure DS с помощью нового DNS-имени домена, которое применяется в следующих правилах:
- Имена не могут начинаться с числового символа.
- Имена должны иметь длину от 3 до 63 символов.

## <a name="need-help-contact-support"></a>Требуется помощь? Обратитесь в службу поддержки.
Если вам все еще нужна помощь, [обратитесь в службу поддержки](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), которая поможет быстро устранить проблему.
