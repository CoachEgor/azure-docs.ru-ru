---
title: Создание шаблона Azure Image Builder (Предварительная версия)
description: Узнайте, как создать шаблон для использования с Azure Image Builder.
author: danis
ms.author: danis
ms.date: 01/23/2020
ms.topic: article
ms.service: virtual-machines-linux
manager: gwallace
ms.openlocfilehash: 4ac2c1266933716697a658b1ba88efd8c2f05d34
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2020
ms.locfileid: "76905761"
---
# <a name="preview-create-an-azure-image-builder-template"></a>Предварительная версия: Создание шаблона Azure Image Builder 

Построитель образов Azure использует JSON-файл для передачи информации в службу "построитель изображений". В этой статье мы перейдем к разделам файла JSON, чтобы вы могли создавать собственные. Примеры полных JSON файлов см. в репозитории [Azure Image Builder GitHub](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts).

Это базовый формат шаблона:

```json
 { 
    "type": "Microsoft.VirtualMachineImages/imageTemplates", 
    "apiVersion": "2019-05-01-preview", 
    "location": "<region>", 
    "tags": {
        "<name": "<value>",
        "<name>": "<value>"
             }
    "identity":{},           
    "dependsOn": [], 
    "properties": { 
        "buildTimeoutInMinutes": <minutes>, 
        "vmProfile": 
            {
            "vmSize": "<vmSize>"
            },
        "build": {}, 
        "customize": {}, 
        "distribute": {} 
      } 
 } 
```



## <a name="type-and-api-version"></a>Тип и версия API

`type` — это тип ресурса, который должен быть `"Microsoft.VirtualMachineImages/imageTemplates"`. При изменении API `apiVersion` со временем будет изменяться, но его следует `"2019-05-01-preview"` для предварительного просмотра.

```json
    "type": "Microsoft.VirtualMachineImages/imageTemplates",
    "apiVersion": "2019-05-01-preview",
```

## <a name="location"></a>Расположение

Расположение — это регион, в котором будет создан пользовательский образ. Для предварительной версии построителя образов поддерживаются следующие регионы:

- Восточная часть США
- Восточная часть США 2
- Центрально-западная часть США
- Западная часть США
- Западная часть США 2


```json
    "location": "<region>",
```
## <a name="vmprofile"></a>вмпрофиле
По умолчанию в построителе образов используется виртуальная машина сборки "Standard_D1_v2". это можно переопределить, например, если вы хотите настроить образ для виртуальной машины GPU, потребуется размер виртуальной машины GPU. Водить описание не обязательно.

```json
 {
    "vmSize": "Standard_D1_v2"
 },
```

## <a name="osdisksizegb"></a>осдисксизегб

По умолчанию построитель образов не изменит размер изображения, он будет использовать размер из исходного изображения. Можно настроить размер диска операционной системы (Win и Linux), обратите внимание, что он не будет слишком мал, чем минимальный объем, необходимый для операционной системы. Это необязательный параметр, а значение 0 означает, что размер исходного изображения не отличается. Водить описание не обязательно.

```json
 {
    "osDiskSizeGB": 100
 },
```

## <a name="tags"></a>Теги

Это пары "ключ-значение", которые можно указать для создаваемого образа.

## <a name="depends-on-optional"></a>Зависит от (необязательно)

Этот необязательный раздел можно использовать для обеспечения завершения зависимостей перед продолжением. 

```json
    "dependsOn": [],
```

Дополнительные сведения см. в разделе [Определение зависимостей ресурсов](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies#dependson).

## <a name="identity"></a>Удостоверение
По умолчанию построитель образов поддерживает использование скриптов или копирование файлов из нескольких расположений, таких как GitHub и хранилище Azure. Для использования этих данных они должны быть общедоступными.

Вы также можете использовать управляемое пользователем удостоверение Azure, определяемое вами, чтобы разрешить построителю образов доступ к хранилищу Azure при условии, что удостоверение было предоставлено как минимум "модуль чтения данных BLOB-объекта хранилища" в учетной записи хранения Azure. Это означает, что вам не нужно делать большие двоичные объекты хранилища доступными извне или настраивать маркеры SAS.


```json
    "identity": {
    "type": "UserAssigned",
          "userAssignedIdentities": {
            "<imgBuilderId>": {}
        }
        },
```

Полный пример см. в статье [использование управляемого удостоверения, назначаемого пользователем Azure для доступа к файлам в службе хранилища Azure](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage).

Поддержка построителя изображений для назначенного пользователю удостоверения: • поддерживает только одно удостоверение • не поддерживает пользовательские доменные имена

Дополнительные сведения см. в статье [что такое управляемые удостоверения для ресурсов Azure?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).
Дополнительные сведения о развертывании этой функции см. в статье [Настройка управляемых удостоверений для ресурсов Azure на виртуальной машине Azure с помощью Azure CLI](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm#user-assigned-managed-identity).

## <a name="properties-source"></a>Свойства: источник

В разделе `source` содержатся сведения об исходном образе, который будет использоваться построителем образов.

Для API требуется "SourceType", определяющий источник для сборки образа, в настоящее время существует три типа:
- ISO — используйте его, если источником является RHEL ISO.
- Платформимаже — указывает, что исходный образ является образом Marketplace.
- Манажедимаже — используйте его при запуске из обычного управляемого образа.
- Шаредимажеверсион — используется при использовании версии образа в коллекции общих образов в качестве источника.

### <a name="iso-source"></a>Исходный код ISO

Построитель образов Azure поддерживает только опубликованные Red Hat Enterprise Linux 7. x двоичные DVD ISO, предварительная версия. Построитель образов поддерживает:
- RHEL 7.3 
- RHEL 7,4 
- RHEL 7.5 
 
```json
"source": {
       "type": "ISO",
       "sourceURI": "<sourceURI from the download center>",
       "sha256Checksum": "<checksum associated with ISO>"
}
```

Чтобы получить значения `sourceURI` и `sha256Checksum`, перейдите в `https://access.redhat.com/downloads`, выберите **Red Hat Enterprise Linux**продукта и поддерживаемую версию. 

В списке **установщиков и образов для Red Hat Enterprise Linux Server**необходимо скопировать ссылку для Red Hat Enterprise Linux 7. x двоичный DVD и контрольную сумму.

> [!NOTE]
> Маркеры доступа ссылок обновляются с частыми интервалами, поэтому каждый раз, когда необходимо отправить шаблон, необходимо проверить, изменился ли адрес ссылки RH.
 
### <a name="platformimage-source"></a>Источник Платформимаже 
Построитель образов Azure поддерживает образы Windows Server и клиента и Linux Azure Marketplace. полный список см. [здесь](https://docs.microsoft.com/azure/virtual-machines/windows/image-builder-overview#os-support) . 

```json
        "source": {
            "type": "PlatformImage",
                "publisher": "Canonical",
                "offer": "UbuntuServer",
                "sku": "18.04-LTS",
                "version": "18.04.201903060"
        },
```


Здесь используются те же свойства, которые используются для создания виртуальной машины с помощью команды AZ CLI. чтобы получить свойства, выполните приведенную ниже команду. 
 
```azurecli-interactive
az vm image list -l westus -f UbuntuServer -p Canonical --output table –-all 
```

> [!NOTE]
> Версия не может быть "последней". для получения номера версии необходимо использовать указанную выше команду. 

### <a name="managedimage-source"></a>Источник Манажедимаже

Задает исходный образ как существующий управляемый образ универсального виртуального жесткого диска или виртуальной машины. Исходный управляемый образ должен быть поддерживаемой ОС и находиться в том же регионе, что и шаблон Azure Image Builder. 

```json
        "source": { 
            "type": "ManagedImage", 
                "imageId": "/subscriptions/<subscriptionId>/resourceGroups/{destinationResourceGroupName}/providers/Microsoft.Compute/images/<imageName>"
        }
```

`imageId` должен быть идентификатором ResourceId управляемого образа. Для вывода списка доступных образов используйте `az image list`.


### <a name="sharedimageversion-source"></a>Источник Шаредимажеверсион
Задает исходный образ существующей версии образа в коллекции общих образов. Версия образа должна быть поддерживаемой операционной системой, и образ должен быть реплицирован в тот же регион, что и шаблон Azure Image Builder. 

```json
        "source": { 
            "type": "SharedImageVersion", 
            "imageVersionID": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/p  roviders/Microsoft.Compute/galleries/<sharedImageGalleryName>/images/<imageDefinitionName/versions/<imageVersion>" 
   } 
```

`imageVersionId` должен быть ResourceId версии образа. Для вывода списка версий образа используйте команду [AZ SIG Image-Version List](/cli/azure/sig/image-version#az-sig-image-version-list) .

## <a name="properties-buildtimeoutinminutes"></a>Свойства: Буилдтимеаутинминутес

По умолчанию построитель образов будет выполняться в течение 240 минут. После этого он будет исполняться и останавливаться, независимо от того, завершена ли сборка образа. При достижении времени ожидания появится сообщение об ошибке следующего вида:

```text
[ERROR] Failed while waiting for packerizer: Timeout waiting for microservice to
[ERROR] complete: 'context deadline exceeded'
```

Если не указать значение Буилдтимеаутинминутес или установить его в 0, будет использоваться значение по умолчанию. Вы можете увеличить или уменьшить значение до максимума, равного 960mins (16hrs). Для Windows не рекомендуется устанавливать этот параметр ниже 60 минут. Если вы обнаружите, что достигается время ожидания, просмотрите [журналы](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#collecting-and-reviewing-aib-image-build-logs), чтобы проверить, ожидает ли шаг настройки действия, подобные введенным пользователем. 

Если вам требуется больше времени для завершения настройки, задайте для этого параметра значение, которое вы считаете необходимым, с небольшой нагрузкой. Но не задавайте слишком высокие значения, так как может потребоваться подождать, пока не будет выводится сообщение об ошибке. 


## <a name="properties-customize"></a>Свойства: Настройка

Построитель образов поддерживает несколько "настраиваемых" настроек. Пользовательские настройки — это функции, используемые для настройки образа, такие как выполнение сценариев или перезагрузка серверов. 

При использовании `customize`: 
- Можно использовать несколько настраиваемых настроек, но они должны иметь уникальный `name`.
- Настраиваемые шаблоны выполняются в порядке, указанном в шаблоне.
- В случае сбоя одного из средств настройки происходит сбой всего компонента настройки и выводится сообщение об ошибке.
- Настоятельно рекомендуется тщательно протестировать сценарий, прежде чем использовать его в шаблоне. Отладка сценария на собственной виртуальной машине будет проще.
- Не помещайте конфиденциальные данные в скрипты. 
- Расположения сценариев должны быть общедоступными, если не используется [MSI](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage).

```json
        "customize": [
            {
                "type": "Shell",
                "name": "<name>",
                "scriptUri": "<path to script>",
                "sha256Checksum": "<sha256 checksum>"
            },
            {
                "type": "Shell",
                "name": "<name>",
                "inline": [
                    "<command to run inline>",
                ]
            }

        ],
```     

 
Раздел настройки является массивом. Построитель образов Azure будет выполняться через настройки в последовательном порядке. Любой сбой в любом из настроек приведет к сбою процесса сборки. 
 
 
### <a name="shell-customizer"></a>Настройка оболочки

Программа настройки оболочки поддерживает выполнение скриптов оболочки, поэтому они должны быть общедоступными для доступа к ним.

```json
    "customize": [ 
        { 
            "type": "Shell", 
            "name": "<name>", 
            "scriptUri": "<link to script>",
            "sha256Checksum": "<sha256 checksum>"       
        }, 
    ], 
        "customize": [ 
        { 
            "type": "Shell", 
            "name": "<name>", 
            "inline": "<commands to run>"
        }, 
    ], 
```

Поддержка ОС: Linux 
 
Настройка свойств:

- **тип** — Shell 
- **Name** — имя для отслеживания настройки. 
- **скриптури** -URI в расположение файла 
- **встроенный** массив команд оболочки, разделенных запятыми.
- **sha256Checksum** — значение контрольной суммы SHA256 файла, создается локально, а затем построитель образов выполнит контрольную сумму и проверит.
    * Чтобы создать sha256Checksum с помощью терминала в Mac/Linux, выполните команду: `sha256sum <fileName>`


Чтобы команды выполнялись с привилегиями суперпользователя, они должны иметь префикс `sudo`.

> [!NOTE]
> При запуске программы настройки оболочки с использованием источника RHEL ISO необходимо убедиться, что первая оболочка настройки обрабатывает регистрацию с помощью сервера Red Hat, прежде чем выполнять настройку. После завершения настройки сценарий должен отменить регистрацию на сервере обслуживания.

### <a name="windows-restart-customizer"></a>Настройка перезапуска Windows 
Настройка перезапуска позволяет перезапустить виртуальную машину Windows и дождаться ее возвращения в режим «в сети». Это позволит установить программное обеспечение, требующее перезагрузки.  

```json 
     "customize": [ 

            {
                "type": "WindowsRestart",
                "restartCommand": "shutdown /r /f /t 0 /c", 
                "restartCheckCommand": "echo Azure-Image-Builder-Restarted-the-VM  > c:\\buildArtifacts\\azureImageBuilderRestart.txt",
                "restartTimeout": "5m"
            }
  
        ],
```

Поддержка ОС: Windows
 
Настройка свойств:
- **Тип**: виндовсрестарт
- **рестарткомманд** — команда для выполнения перезапуска (необязательно). Значение по умолчанию — `'shutdown /r /f /t 0 /c \"packer restart\"'`.
- **рестартчекккомманд** — команда для проверки успешности перезапуска (необязательно). 
- **рестарттимеаут** — время ожидания перезапуска указано как строка величины и единица измерения. Например, `5m` (5 минут) или `2h` (2 часа). Значение по умолчанию: "5 мин"

### <a name="linux-restart"></a>Перезапуск Linux  
Настройка перезапуска Linux отсутствует. Однако если вы устанавливаете драйверы или компоненты, требующие перезагрузки, вы можете установить их и вызвать перезагрузку с помощью средства настройки оболочки. для виртуальной машины сборки существует тайм-аут 20min SSH.

### <a name="powershell-customizer"></a>Настройка PowerShell 
Программа настройки оболочки поддерживает выполнение скриптов PowerShell и встроенных команд, поэтому сценарии должны быть общедоступными для доступа к ним.

```json 
     "customize": [
        { 
             "type": "PowerShell",
             "name":   "<name>",  
             "scriptUri": "<path to script>",
             "runElevated": "<true false>",
             "sha256Checksum": "<sha256 checksum>" 
        },  
        { 
             "type": "PowerShell", 
             "name": "<name>", 
             "inline": "<PowerShell syntax to run>", 
             "valid_exit_codes": "<exit code>",
             "runElevated": "<true or false>" 
         } 
    ], 
```

Поддержка ОС: Windows и Linux

Настройка свойств:

- **тип** — PowerShell.
- **скриптури** -URI в расположение файла сценария PowerShell. 
- **встроенные** встроенные команды для выполнения, разделенные запятыми.
- **valid_exit_codes** — необязательные, допустимые коды, которые могут быть возвращены из команды script/Inline, это не позволит сообщить о сбое команды script/Inline.
- **runElevated** — необязательный, логический, поддержка выполнения команд и скриптов с повышенными разрешениями.
- **sha256Checksum** — значение контрольной суммы SHA256 файла, создается локально, а затем построитель образов выполнит контрольную сумму и проверит.
    * Создание sha256Checksum с помощью PowerShell в Windows [Get-hash](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-filehash?view=powershell-6)


### <a name="file-customizer"></a>Настройка файлов

Настройка файлов позволяет построителю образов Скачать файл из GitHub или службы хранилища Azure. Если у вас есть конвейер сборки образа, основанный на артефактах сборки, можно настроить для программы настройки файлов загрузку из общего ресурса сборки и переместить артефакты в образ.  

```json
     "customize": [ 
         { 
            "type": "File", 
             "name": "<name>", 
             "sourceUri": "<source location>",
             "destination": "<destination>",
             "sha256Checksum": "<sha256 checksum>"
         }
     ]
```

Поддержка ОС: Linux и Windows 

Свойства настройки файлов:

- **sourceUri** — доступная конечная точка хранилища, это может быть GitHub или служба хранилища Azure. Вы можете скачать только один файл, а не весь каталог. Если необходимо загрузить каталог, используйте сжатый файл, а затем распакуйте его с помощью оболочки или настраиваемых средств PowerShell. 
- **назначение** — это полный путь назначения и имя файла. Все пути и подкаталоги, на которые указывают ссылки, должны существовать. для их настройки используйте оболочку или настройки PowerShell. Для создания пути можно использовать настраиваемые вами сценарии. 

Это поддерживается каталогами Windows и путями Linux, но существуют некоторые отличия. 
- ОС Linux — единственный построитель изображений Path может выполнять запись в/ТМП.
- Windows — нет ограничения пути, но путь должен существовать.
 
 
Если при попытке загрузить файл или разместить его в указанном каталоге произошла ошибка, то настройка шага настройки завершится ошибкой, а это будет в файле настройки. log.

> [!NOTE]
> Настройка файлов подходит только для небольших Скачиваний файлов, < 20 МБ. Для загрузки больших файлов используйте сценарий или встроенную команду, используя код для загрузки файлов, например, Linux `wget` или `curl`, Windows, `Invoke-WebRequest`.

Файлы в средстве настройки файлов можно скачать из службы хранилища Azure с помощью [MSI](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage).

### <a name="generalize"></a>Generalize 
По умолчанию построитель образов Azure также выполняет код отмены подготовки в конце каждого этапа настройки образа, чтобы подготовить образ к использованию. Обобщение — это процесс, в котором образ настраивается так, чтобы его можно было повторно использовать для создания нескольких виртуальных машин. Для виртуальных машин Windows в построителе образов Azure используется Sysprep. Для Linux в Azure Image Builder выполняется "waagent-unготовить". 

Команды, которые пользователи построителя образов в generalize, могут не подойти для каждой ситуации, поэтому при необходимости построитель образов Azure позволит настроить эту команду. 

Если выполняется миграция существующей настройки и используются разные команды Sysprep и waagent, можно использовать универсальные команды Image Builder, а при сбое создания виртуальной машины воспользоваться собственными командами Sysprep или waagent.

Если построитель образов Azure успешно создает пользовательский образ Windows и создает на его основе виртуальную машину, то обнаружит, что создание виртуальной машины завершилось сбоем или не завершится успешно, необходимо ознакомиться с документацией по Windows Server Sysprep или создать запрос на поддержку с помощью Группа поддержки пользователей Windows Server Sysprep, которая может устранять неполадки и рекомендовать использование Sysprep.


#### <a name="default-sysprep-command"></a>Команда Sysprep по умолчанию
```powershell
echo '>>> Waiting for GA to start ...'
while ((Get-Service RdAgent).Status -ne 'Running') { Start-Sleep -s 5 }
while ((Get-Service WindowsAzureTelemetryService).Status -ne 'Running') { Start-Sleep -s 5 }
while ((Get-Service WindowsAzureGuestAgent).Status -ne 'Running') { Start-Sleep -s 5 }
echo '>>> Sysprepping VM ...'
if( Test-Path $Env:SystemRoot\\windows\\system32\\Sysprep\\unattend.xml ){ rm $Env:SystemRoot\\windows\\system32\\Sysprep\\unattend.xml -Force} & $Env:SystemRoot\\System32\\Sysprep\\Sysprep.exe /oobe /generalize /quiet /quit
while($true) { $imageState = Get-ItemProperty HKLM:\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Setup\\State | Select ImageState; if($imageState.ImageState -ne 'IMAGE_STATE_GENERALIZE_RESEAL_TO_OOBE') { Write-Output $imageState.ImageState; Start-Sleep -s 5  } else { break } }
```
#### <a name="default-linux-deprovision-command"></a>Команда отмены инициализации по умолчанию Linux

```bash
/usr/sbin/waagent -force -deprovision+user && export HISTSIZE=0 && sync
```

#### <a name="overriding-the-commands"></a>Переопределение команд
Чтобы переопределить команды, используйте PowerShell или профилировщики скриптов оболочки, чтобы создать командные файлы с точным именем файла и разместить их в соответствующих каталогах:

* Windows: c:\DeprovisioningScript.ps1
* Linux:/ТМП/депровисионингскрипт.ш

Построитель образов считывает эти команды, они записываются в журналы AIB, "Настройка. log". См. раздел [Устранение неполадок](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#collecting-and-reviewing-aib-logs) со сведениями о том, как вести журнал.
 
## <a name="properties-distribute"></a>Свойства: распространение

Построитель образов Azure поддерживает три цели распространения: 

- управляемый образ **манажедимаже** .
- **шаредимаже** — общая Коллекция образов.
- **VHD** — VHD в учетной записи хранения.

Вы можете распространить изображение на оба типа целевого объекта в той же конфигурации. см. [примеры](https://github.com/danielsollondon/azvmimagebuilder/blob/7f3d8c01eb3bf960d8b6df20ecd5c244988d13b6/armTemplates/azplatform_image_deploy_sigmdi.json#L80).

Так как у вас может быть несколько целевых объектов для распространения, построитель образов поддерживает состояние для каждого целевого объекта распределения, доступ к которому можно получить, запросив `runOutputName`.  `runOutputName` — это объект, который можно запросить после распределения, чтобы получить сведения об этом распространении. Например, можно запросить расположение виртуального жесткого диска или регионы, в которых была выполнена репликация версии образа, или созданную версию SIG. Это свойство каждого целевого объекта распределения. `runOutputName` должны быть уникальными для каждого целевого объекта распространения. Ниже приведен пример, в котором запрашиваются дистрибутивы общей коллекции образов.

```bash
subscriptionID=<subcriptionID>
imageResourceGroup=<resourceGroup of image template>
runOutputName=<runOutputName>

az resource show \
        --ids "/subscriptions/$subscriptionID/resourcegroups/$imageResourceGroup/providers/Microsoft.VirtualMachineImages/imageTemplates/ImageTemplateLinuxRHEL77/runOutputs/$runOutputName"  \
        --api-version=2019-05-01-preview
```

Выходные данные:
```json
{
  "id": "/subscriptions/xxxxxx/resourcegroups/rheltest/providers/Microsoft.VirtualMachineImages/imageTemplates/ImageTemplateLinuxRHEL77/runOutputs/rhel77",
  "identity": null,
  "kind": null,
  "location": null,
  "managedBy": null,
  "name": "rhel77",
  "plan": null,
  "properties": {
    "artifactId": "/subscriptions/xxxxxx/resourceGroups/aibDevOpsImg/providers/Microsoft.Compute/galleries/devOpsSIG/images/rhel/versions/0.24105.52755",
    "provisioningState": "Succeeded"
  },
  "resourceGroup": "rheltest",
  "sku": null,
  "tags": null,
  "type": "Microsoft.VirtualMachineImages/imageTemplates/runOutputs"
}
```

### <a name="distribute-managedimage"></a>Распространение: Манажедимаже

Выходным изображением будет ресурс управляемого изображения.

```json
"distribute": [
        {
"type":"managedImage",
       "imageId": "<resource ID>",
       "location": "<region>",
       "runOutputName": "<name>",
       "artifactTags": {
            "<name": "<value>",
             "<name>": "<value>"
               }
         }]
```
 
Свойства распространения:
- **тип** — манажедимаже 
- **имажеид** — идентификатор ресурса конечного образа, ожидаемый формат:/Subscriptions/\<SubscriptionId >/ResourceGroups/\<дестинатионресаурцеграупнаме >/Провидерс/Микрософт.компуте/имажес/\<imagename >
- **Location** — расположение управляемого образа.  
- **рунаутпутнаме** — уникальное имя для идентификации распределения.  
- **артифакттагс** — необязательные теги пар "ключ-значение", заданные пользователем.
 
 
> [!NOTE]
> Целевая группа ресурсов должна существовать.
> Если вы хотите, чтобы изображение распространялось в другой регион, это увеличит время развертывания. 

### <a name="distribute-sharedimage"></a>Распространение: Шаредимаже 
Коллекция общих образов Azure — это новая служба управления образами, которая позволяет управлять репликацией в области образа, управлением версиями и предоставлением общего доступа к пользовательским образам. Построитель образов Azure поддерживает распространение с помощью этой службы, что позволяет распространять образы в регионы, поддерживаемые общими галереями образов. 
 
Общая Коллекция образов состоит из: 
 
- Коллекция — контейнер для нескольких общих образов. Коллекция развертывается в одном регионе.
- Определения изображений — концептуальное группирование для изображений. 
- Версии изображений — это тип образа, используемый для развертывания виртуальной машины или масштабируемого набора. Версии образов можно реплицировать в другие регионы, в которых необходимо развернуть виртуальные машины.
 
Перед распространением в коллекцию образов необходимо создать коллекцию и определение образа. см. раздел [Общие образы](shared-images.md). 

```json
{
    "type": "sharedImage",
    "galleryImageId": "<resource ID>",
    "runOutputName": "<name>",
    "artifactTags": {
        "<name>": "<value>",
        "<name>": "<value>"
    },
    "replicationRegions": [
        "<region where the gallery is deployed>",
        "<region>"
    ]
}
``` 

Распространять свойства для коллекций общих изображений:

- **Type** -шаредимаже  
- **галлеримажеид** — идентификатор общей коллекции образов. Формат:/Subscriptions/\<subscriptionId >/resourceGroups/\<resourceGroupName >/Провидерс/Микрософт.компуте/галлериес/\<Шаредимажегаллеринаме >/Images/\<Имажегаллеринаме >.
- **рунаутпутнаме** — уникальное имя для идентификации распределения.  
- **артифакттагс** — необязательные теги пар "ключ-значение", заданные пользователем.
- **репликатионрегионс** — массив регионов для репликации. Один из регионов должен быть регионом, в котором развернута коллекция.
 
> [!NOTE]
> Вы можете использовать построитель образов Azure в другом регионе для коллекции, но службе "Построитель образов Azure" потребуется переместить образ между центрами обработки данных, и это займет больше времени. Построитель образов автоматически получит версию изображения на основе монотонного целого числа, его нельзя указать в данный момент. 

### <a name="distribute-vhd"></a>Распространение: VHD  
Вы можете вывести данные на виртуальный жесткий диск. Затем можно скопировать виртуальный жесткий диск и использовать его для публикации в Azure MarketPlace или использовать с Azure Stack.  

```json
{ 
    "type": "VHD",
    "runOutputName": "<VHD name>",
    "tags": {
        "<name": "<value>",
        "<name>": "<value>"
    }
}
```
 
Поддержка ОС: Windows и Linux

Распределение параметров виртуального жесткого диска:

- **введите** -VHD.
- **рунаутпутнаме** — уникальное имя для идентификации распределения.  
- **теги** — необязательные теги пар "ключ-значение", заданные пользователем.
 
Построитель образов Azure не позволяет пользователю указывать расположение учетной записи хранения, но вы можете запросить состояние `runOutputs`, чтобы получить расположение.  

```azurecli-interactive
az resource show \
   --ids "/subscriptions/$subscriptionId/resourcegroups/<imageResourceGroup>/providers/Microsoft.VirtualMachineImages/imageTemplates/<imageTemplateName>/runOutputs/<runOutputName>"  | grep artifactUri 
```

> [!NOTE]
> После создания виртуального жесткого диска скопируйте его в другое расположение, как можно скорее. Виртуальный жесткий диск хранится в учетной записи хранения во временной группе ресурсов, созданной при отправке шаблона образа в службу Azure Image Builder. При удалении шаблона образа виртуальный жесткий диск будет утерян. 
 
## <a name="next-steps"></a>Дальнейшие действия

Существуют образцы JSON-файлов для различных сценариев в GitHub в [Azure Image Builder](https://github.com/danielsollondon/azvmimagebuilder).
 
