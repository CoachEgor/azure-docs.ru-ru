---
title: Расширение виртуальной машины для диагностики производительности Azure для Windows | Документация Майкрософт
description: Эта статья содержит сведения о расширении виртуальной машины для диагностики производительности Azure для Windows.
services: virtual-machines-windows'
documentationcenter: ''
author: genlin
manager: cshepard
editor: na
tags: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 769305cc3d838832f8f445ac9623a1724603f968
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60307937"
---
# <a name="azure-performance-diagnostics-vm-extension-for-windows"></a>Расширение виртуальной машины для диагностики производительности Azure для Windows

Расширение виртуальной машины для диагностики производительности Azure позволяет собирать данные диагностики производительности с виртуальных машин Windows. Оно также выполняет анализ и предоставляет отчет с выводами и рекомендации для определения и решения проблем с производительностью виртуальных машин. Это расширение устанавливает инструмент устранения неполадок [PerfInsights](https://aka.ms/perfinsights).

> [!NOTE]
> Если вы хотите выполнить диагностику виртуальной машины на портале Azure, развернутой с помощью неклассической модели Azure, советуем использовать новый интерфейс. Дополнительные сведения см. в статье [Диагностика производительности для виртуальных машин Azure](performance-diagnostics.md). 

## <a name="prerequisites"></a>Технические условия

Расширение можно установить на Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 и Windows Server 2016, а также в Windows 8.1 и Windows 10.

## <a name="extension-schema"></a>Схема расширения
В указанном ниже объекте JSON показана схема для расширения виртуальной машины для диагностики производительности Azure. Для этого расширения требуется имя и ключ для учетной записи хранения, чтобы хранить выходные данные и отчет диагностики. Эти значения конфиденциальные. Ключ учетной записи хранения должен храниться внутри защищенной конфигурации параметров. Данные защищенных параметров расширения виртуальной машины Azure зашифрованы. Они расшифровываются только на целевой виртуальной машине. Обратите внимание, что в **storageAccountName** и **storageAccountKey** учитывается регистр. Другие необходимые параметры указаны в следующем разделе.

```JSON
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "performanceScenario": "[parameters('performanceScenario')]",
            "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
            "perfCounterTrace": "[parameters('perfCounterTrace')]",
            "networkTrace": "[parameters('networkTrace')]",
            "xperfTrace": "[parameters('xperfTrace')]",
            "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]",
            "resourceId": "[resourceId('Microsoft.Compute/virtualMachines', parameters('vmName'))]"
        },
        "protectedSettings": {
            "storageAccountKey": "[parameters('storageAccountKey')]"        
        }
      }
    }
```

### <a name="property-values"></a>Значения свойств

|   **Имя**   |**Значение и пример**|       **Описание**      |
|--------------|-------------------|----------------------------|
|версия_API|2015-06-15|Версия API.
|publisher|Microsoft.Azure.Performance.Diagnostics|Пространство имен издателя для расширения.
|Тип|AzurePerformanceDiagnostics|Тип расширения виртуальной машины.
|typeHandlerVersion|1.0|Версия обработчика расширения.
|performanceScenario|basic|Сценарий производительности, для которого требуется собрать данные. Допустимые значения: **basic**, **vmslow**, **azurefiles** и **custom**.
|traceDurationInSeconds|300|Длительность трассировки, если выбран любой из параметров трассировки.
|perfCounterTrace|p|Параметр, позволяющий включить трассировку счетчиков производительности. Допустимые значения: **p** или пустое значение. Если вы не хотите записывать данные трассировки, оставьте значение пустым.
|networkTrace|n|Параметр, позволяющий включить трассировку сети. Допустимые значения: **n** или пустое значение. Если вы не хотите записывать данные трассировки, оставьте значение пустым.
|xperfTrace|x|Параметр, позволяющий включить трассировку XPerf. Допустимые значения: **x** или пустое значение. Если вы не хотите записывать данные трассировки, оставьте значение пустым.
|storPortTrace|s|Параметр, позволяющий включить трассировку StorPort. Допустимые значения: **s** или пустое значение. Если вы не хотите записывать данные трассировки, оставьте значение пустым.
|srNumber|123452016365929|Номер запроса в службу поддержки (если доступно). Если его нет, оставьте значение поля пустым.
|requestTimeUtc|2017-09-28T22:08:53.736Z|Текущая дата и время в формате UTC. Если вы устанавливали расширение с помощью портала, это значение можно не указывать.
|resourceId|/subscriptions/{ИД_подписки}/resourceGroups/{имя_группы_ресурсов}/providers/{пространство_имен_поставщика_ресурса}/{тип_ресурса}/{имя_ресурса}|Уникальный идентификатор виртуальной машины.
|storageAccountName|mystorageaccount|Имя учетной записи хранения для хранения журналов диагностики и результатов.
|storageAccountKey|lDuVvxuZB28NNP…hAiRF3voADxLBTcc==|Ключ для учетной записи хранения.

## <a name="install-the-extension"></a>Установка расширения

Выполните указанные ниже действия, чтобы установить расширение на виртуальных машинах Windows.

1. Войдите на [портале Azure](https://portal.azure.com).
2. Выберите виртуальную машину, на которой необходимо установить расширение.

    ![Снимок экрана: портал Azure с выделенным пунктом меню "Виртуальные машины"](media/performance-diagnostics-vm-extension/select-the-virtual-machine.png)
3. Выберите колонку **Расширения**, а затем щелкните **Добавить**.

    ![Снимок экрана: колонка "Расширения" с выделенной кнопкой "Добавить"](media/performance-diagnostics-vm-extension/select-extensions.png)
4. Выберите расширение **диагностики производительности Azure**, просмотрите условия и нажмите кнопку **Создать**.

    ![Снимок экрана: колонка "Новый ресурс" с выделенным пунктом меню Azure Performance Diagnostics (Диагностика производительности Azure)](media/performance-diagnostics-vm-extension/create-azure-performance-diagnostics-extension.png)
5. Чтобы установить расширение, укажите значения параметров для установки и нажмите кнопку **ОК**. Дополнительные сведения о поддерживаемых сценариях см. в [этом разделе](how-to-use-perfInsights.md#supported-troubleshooting-scenarios). 

    ![Снимок экрана: диалоговое окно установки расширения](media/performance-diagnostics-vm-extension/install-the-extension.png)
6. После успешной установки появится сообщение об успешном состоянии.

    ![Снимок экрана: сообщение об успешной подготовке](media/performance-diagnostics-vm-extension/provisioning-succeeded-message.png)

    > [!NOTE]
    > Расширение запустится после успешного завершения подготовки. Для базового сценария оно выполняется не более двух минут. Для других сценариев расширение выполняется в течение периода, указанного во время установки.

## <a name="remove-the-extension"></a>Удаление расширения
Чтобы удалить расширение с виртуальной машины, сделайте следующее:

1. Войдите на [портал Azure](https://portal.azure.com), выберите виртуальную машину, из которой вы хотите удалить расширение, а затем выберите колонку **Расширения**. 
2. Выберите (**...**) из списка строки для расширения системы диагностики производительности, а затем выберите **Удалить**.

    ![Снимок экрана: колонка "Расширения" с выделенным пунктом меню "Удалить"](media/performance-diagnostics-vm-extension/uninstall-the-extension.png)

    > [!NOTE]
    > Вы также можете выбрать запись в списке, а затем — параметр **Удалить**.

## <a name="template-deployment"></a>Развертывание шаблона
Расширения виртуальной машины Azure можно развернуть с помощью шаблонов Azure Resource Manager. В шаблоне Azure Resource Manager можно использовать схему JSON, описанную в предыдущем разделе. Таким образом во время развертывания шаблона Azure Resource Manager запустится расширение виртуальной машины для диагностики производительности Azure. Вот пример шаблона:

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "defaultValue": "yourVMName"
    },
    "location": {
      "type": "string",
      "defaultValue": "southcentralus"
    },
    "storageAccountName": {
      "type": "securestring"
      "defaultValue": "yourStorageAccount"
    },
    "storageAccountKey": {
      "type": "securestring"
      "defaultValue": "yourStorageAccountKey"
    },
    "performanceScenario": {
      "type": "string",
      "defaultValue": "basic"
    },
    "srNumber": {
      "type": "string",
      "defaultValue": ""
    },
    "traceDurationInSeconds": {
      "type": "int",
    "defaultValue": 300
    },
    "perfCounterTrace": {
      "type": "string",
      "defaultValue": "p"
    },
    "networkTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "xperfTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "storPortTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "requestTimeUtc": {
      "type": "string",
      "defaultValue": "10/2/2017 11:06:00 PM"
    }       
  },
  "resources": [
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "performanceScenario": "[parameters('performanceScenario')]",
            "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
            "perfCounterTrace": "[parameters('perfCounterTrace')]",
            "networkTrace": "[parameters('networkTrace')]",
            "xperfTrace": "[parameters('xperfTrace')]",
            "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]",
            "resourceId": "[resourceId('Microsoft.Compute/virtualMachines', parameters('vmName'))]"
        },
        "protectedSettings": {
            "storageAccountKey": "[parameters('storageAccountKey')]"
        }
      }
    }
  ]
}
```

## <a name="powershell-deployment"></a>Развертывание с помощью PowerShell
Команду `Set-AzVMExtension` можно использовать для развертывания расширения виртуальной машины для диагностики производительности Azure на имеющейся виртуальной машине.

PowerShell

```
$PublicSettings = @{ "storageAccountName"="mystorageaccount";"performanceScenario"="basic";"traceDurationInSeconds"=300;"perfCounterTrace"="p";"networkTrace"="";"xperfTrace"="";"storPortTrace"="";"srNumber"="";"requestTimeUtc"="2017-09-28T22:08:53.736Z";"resourceId"="VMResourceId" }
$ProtectedSettings = @{"storageAccountKey"="mystoragekey" }

Set-AzVMExtension -ExtensionName "AzurePerformanceDiagnostics" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.Azure.Performance.Diagnostics" `
    -ExtensionType "AzurePerformanceDiagnostics" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS
```

## <a name="information-on-the-data-captured"></a>Сведения о собранных данных
В зависимости от выбранного сценария инструмент PerfInsights собирает различные журналы, конфигурации и диагностические данные. Дополнительные сведения см. в [документации по PerfInsights](https://aka.ms/perfinsights).

## <a name="view-and-share-the-results"></a>Просмотр результатов и предоставление к ним общего доступа

Выходные данные расширения можно найти в ZIP-файле, который передается в учетную запись хранения, указанную во время установки. Его можно совместно использовать на протяжении 30 дней с помощью [подписанного URL-адреса (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md). Этот ZIP-файл содержит журналы диагностики и отчет с результатами и рекомендациями. Ссылка SAS на ZIP-файл с выходными данными содержится в текстовом файле с именем *zipfilename*_saslink.txt в папке **C:\Packages\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics\\\<version>**. Любой пользователь, у которого есть эта ссылка, может скачать ZIP-файл.

Чтобы помочь сотрудникам службы поддержки, которые работают над вашим запросом, корпорация Майкрософт может использовать ссылку SAS для скачивания данных диагностики.

Чтобы просмотреть отчет, извлеките ZIP-файл и откройте файл **PerfInsights Report.html**.

Вы также можете скачать ZIP-файл непосредственно на портале, выбрав расширение.

![Снимок экрана: подробные сведения о диагностике производительности](media/performance-diagnostics-vm-extension/view-detailed-status.png)

> [!NOTE]
> Ссылка SAS, которая отображается на портале, в некоторых случаях может не работать. из-за неправильно сформированного URL-адреса во время кодирования и декодирования операции. Вместо этого можно получить ссылку непосредственно из файла *_saslink.txt виртуальной машины.

## <a name="troubleshoot-and-support"></a>Устранение неполадок и поддержка

- Состояние развертывания расширения (в области уведомлений) может отображать сообщение "Выполняется развертывание", даже если подготовка расширения успешно завершена.

    Эту проблему можно спокойно проигнорировать, если состояние расширения указывает, что подготовка расширения успешно завершена.
- Некоторые проблемы во время установки можно устранить с помощью журналов расширений. Выходные данные выполнения расширения регистрируются в файле, расположенном в следующем каталоге:

        C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics\<version>

Если в любой момент при изучении этой статьи вам потребуется дополнительная помощь, вы можете обратиться к экспертам по Azure на [форумах MSDN Azure и Stack Overflow](https://azure.microsoft.com/support/forums/). Кроме того, можно зарегистрировать обращение в службу поддержки Azure. Перейдите на [сайт поддержки Azure](https://azure.microsoft.com/support/options/) и выберите **Получить поддержку**. Дополнительные сведения об использовании службы поддержки Azure см. в статье [Часто задаваемые вопросы о поддержке Microsoft Azure](https://azure.microsoft.com/support/faq/).
