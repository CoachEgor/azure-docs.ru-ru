---
title: Создание связанных шаблонов
description: Сведения о создании связанных шаблонов Azure Resource Manager для создания виртуальной машины
author: mumian
ms.date: 12/03/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 903193b48f41b2ba25bc35ac245e5afb482e61d2
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75471419"
---
# <a name="tutorial-create-linked-azure-resource-manager-templates"></a>Руководство. Создание связанных шаблонов Azure Resource Manager

Узнайте, как создать связанные шаблоны Azure Resource Manager. Используя связанные шаблоны, можно одним шаблоном вызвать другой. Это отлично подходит для создания модулей шаблонов. В этом руководстве используется тот же шаблон, что и в статье [Руководство. Создание шаблонов Azure Resource Manager с зависимыми ресурсами](./template-tutorial-create-templates-with-dependent-resources.md), который создает виртуальную машину, виртуальную сеть и другие зависимые ресурсы, включая учетную запись хранения. Для создания ресурса учетной записи хранения можно использовать связанный шаблон.

Вызов связанного шаблона похож на вызов функции.  Вы также узнаете, как передавать значения параметров связанному шаблону, а также как получить "возвращаемые значения" из связанного шаблона.

В рамках этого руководства рассматриваются следующие задачи:

> [!div class="checklist"]
> * открытие шаблона быстрого запуска;
> * Создание связанного шаблона
> * Передача связанного шаблона
> * Ссылка на связанный шаблон
> * Настройка зависимостей
> * Развертывание шаблона
> * Дополнительные рекомендации

Дополнительные сведения см. в статье [Using linked and nested templates when deploying Azure resources](./linked-templates.md) (Использование связанных и вложенных шаблонов при развертывании ресурсов Azure).

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>предварительные требования

Для работы с этой статьей необходимо иметь следующее.

* Visual Studio Code с расширением средств Resource Manager. Дополнительные сведения см. в статье [Use Visual Studio Code to create Azure Resource Manager templates](use-vs-code-to-create-template.md) (Создание шаблонов Azure Resource Manager с помощью Visual Studio Code).
* Для повышения уровня безопасности используйте пароль, созданный для учетной записи администратора виртуальной машины. Ниже приведен пример создания пароля.

    ```azurecli-interactive
    openssl rand -base64 32
    ```

    Для защиты криптографических ключей и других секретов используйте Azure Key Vault. Дополнительные сведения см. в статье [Учебник. Интеграция с Azure Key Vault при развертывании шаблона Resource Manager](./template-tutorial-use-key-vault.md). Мы также рекомендуем обновлять пароль каждые три месяца.

## <a name="open-a-quickstart-template"></a>Открытие шаблона быстрого запуска

Шаблоны быстрого запуска Azure являются репозиторием для шаблонов Resource Manager. Вместо создания шаблона с нуля можно найти пример шаблона и настроить его. Шаблон, используемый в этом руководстве, называется [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Развертывание простой виртуальной машины Windows). Это тот же шаблон, который используется в статье [Руководство. Создание шаблонов Azure Resource Manager с зависимыми ресурсами](./template-tutorial-create-templates-with-dependent-resources.md). Можно сохранить две копии одного шаблона для использования как:

* **основной шаблон** — создает все ресурсы, за исключением учетной записи хранения;
* **связанный шаблон** — создает учетную запись хранения.

1. В Visual Studio Code выберите **Файл**>**Открыть файл**.
1. Скопируйте приведенный ниже URL-адрес и вставьте его в поле **Имя файла**.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

1. Чтобы открыть файл, выберите **Открыть**.
1. Шаблоном определено шесть ресурсов:

   * [`Microsoft.Storage/storageAccounts`](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts)
   * [`Microsoft.Network/publicIPAddresses`](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses)
   * [`Microsoft.Network/networkSecurityGroups`](https://docs.microsoft.com/azure/templates/microsoft.network/networksecuritygroups)
   * [`Microsoft.Network/virtualNetworks`](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks)
   * [`Microsoft.Network/networkInterfaces`](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces)
   * [`Microsoft.Compute/virtualMachines`](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines)

     Прежде чем настраивать шаблон, рекомендуется ознакомиться с основными возможностями схемы шаблонов.
1. Выберите **Файл**>**Сохранить как**, чтобы сохранить файл на локальный компьютер с именем **azuredeploy.json**.
1. Для создания другой копии файла с именем **linkedTemplate.json** выберите **Файл**>**Сохранить как**.

## <a name="create-the-linked-template"></a>Создание связанного шаблона

Связанный шаблон создает учетную запись хранения. Связанный шаблон можно использовать в качестве автономного шаблона, чтобы создать учетную запись хранения. В этом руководстве связанный шаблон получает два параметра и передает значение обратно основному шаблону. Такое "возвращаемое" значение определено в элементе `outputs`.

1. Откройте файл **linkedTemplate.json** в Visual Studio Code (если он еще не открыт).
1. Выполните следующие изменения:

    * Удалите все параметры, кроме **location**.
    * Добавьте параметр с именем **storageAccountName**.

      ```json
      "storageAccountName":{
        "type": "string",
        "metadata": {
            "description": "Azure Storage account name."
        }
      },
      ```

      Имя учетной записи хранения и расположение передаются из основного в связанный шаблон в качестве параметров.

    * Удалите элемент **variables** и все определения переменных.
    * Удалите все ресурсы, за исключением учетной записи хранения. Удаляются всего четыре ресурса.
    * Измените значение элемента **name** учетной записи хранения ресурса на:

        ```json
          "name": "[parameters('storageAccountName')]",
        ```

    * Обновите элемент **выходных данных**, чтобы он выглядел так, как изображено ниже.

        ```json
        "outputs": {
          "storageUri": {
              "type": "string",
              "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
            }
        }
        ```

       Для определения ресурсов виртуальной машины в основном шаблоне необходим **storageUri**.  В качестве значения выходных данных передайте значения обратно в основной шаблон.

        Когда все готово, шаблон должен выглядеть, как показано ниже.

        ```json
        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "storageAccountName": {
              "type": "string",
              "metadata": {
                "description": "Azure Storage account name."
              }
            },
            "location": {
              "type": "string",
              "defaultValue": "[resourceGroup().location]",
              "metadata": {
                "description": "Location for all resources."
              }
            }
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "[parameters('storageAccountName')]",
              "location": "[parameters('location')]",
              "apiVersion": "2018-11-01",
              "sku": {
                "name": "Standard_LRS"
              },
              "kind": "Storage",
              "properties": {}
            }
          ],
          "outputs": {
            "storageUri": {
              "type": "string",
              "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
            }
          }
        }
        ```

1. Сохраните изменения.

## <a name="upload-the-linked-template"></a>Передача связанного шаблона

Основной шаблон и связанный шаблон должны быть доступны из любой точки выполнения развертывания. В этом руководстве используется метод развертывания Cloud Shell, который применялся в статье [Руководство. Создание шаблонов Azure Resource Manager с зависимыми ресурсами](./template-tutorial-create-templates-with-dependent-resources.md). Основной шаблон (azuredeploy.json) загружается в оболочку. Связанный шаблон (linkedTemplate.json) должен быть размещен в общем безопасном расположении. Следующий сценарий PowerShell создает учетную запись хранения Azure, передает в нее шаблон, а затем создает маркер SAS для предоставления ограниченного доступа к файлу шаблона. Для упрощения руководства этот скрипт скачивает полный связанный шаблон из репозитория GitHub. Если вы хотите использовать созданный связанный шаблон, можно отправить его с помощью [Cloud Shell](https://shell.azure.com), а затем изменить скрипт для использования этого шаблона.

> [!NOTE]
> Сценарий ограничивает срок использования маркера SAS до восьми часов. Если для работы с этим руководством необходимо больше времени, увеличьте срок действия.

```azurepowershell-interactive
$projectNamePrefix = Read-Host -Prompt "Enter a project name:"   # This name is used to generate names for Azure resources, such as storage account name.
$location = Read-Host -Prompt "Enter a location (i.e. centralus)"

$resourceGroupName = $projectNamePrefix + "rg"
$storageAccountName = $projectNamePrefix + "store"
$containerName = "linkedtemplates" # The name of the Blob container to be created.

$linkedTemplateURL = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-linked-templates/linkedStorageAccount.json" # A completed linked template used in this tutorial.
$fileName = "linkedStorageAccount.json" # A file name used for downloading and uploading the linked template.

# Download the tutorial linked template
Invoke-WebRequest -Uri $linkedTemplateURL -OutFile "$home/$fileName"

# Create a resource group
New-AzResourceGroup -Name $resourceGroupName -Location $location

# Create a storage account
$storageAccount = New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -Location $location `
    -SkuName "Standard_LRS"

$context = $storageAccount.Context

# Create a container
New-AzStorageContainer -Name $containerName -Context $context

# Upload the linked template
Set-AzStorageBlobContent `
    -Container $containerName `
    -File "$home/$fileName" `
    -Blob $fileName `
    -Context $context

# Generate a SAS token
$templateURI = New-AzStorageBlobSASToken `
    -Context $context `
    -Container $containerName `
    -Blob $fileName `
    -Permission r `
    -ExpiryTime (Get-Date).AddHours(8.0) `
    -FullUri

Write-Host "You need the following values later in the tutorial:"
Write-Host "Resource Group Name: $resourceGroupName"
Write-Host "Linked template URI with SAS token: $templateURI"
Write-Host "Press [ENTER] to continue ..."
```

1. Нажмите зеленую кнопку **Попробовать**, чтобы открыть панель Azure Cloud Shell.
2. Нажмите кнопку **Копировать**, чтобы скопировать сценарий PowerShell.
3. Щелкните правой кнопкой мыши внутри панели оболочки (область темно-синего цвета), а затем выберите **Вставить**.
4. Запишите эти два значения (имя группы ресурсов и URI связанного шаблона), содержащиеся в конце области оболочки. Они понадобятся вам позже при работе с этим руководством.
5. Выберите **Выйти из режима фокусировки**, чтобы закрыть панель оболочки.

На практике вы создаете маркер SAS при развертывании основного шаблона и настраиваете для этого маркера небольшой срок действия, чтобы сделать его более безопасным. Дополнительные сведения см. в разделе [Предоставление маркера SAS во время развертывания](./secure-template-with-sas-token.md#provide-sas-token-during-deployment).

## <a name="call-the-linked-template"></a>Вызов связанного шаблона

Основной шаблон находится в файле с именем azuredeploy.json.

1. Откройте файл **azuredeploy.json** в Visual Studio Code (если он еще не открыт).
1. Замените определение ресурса учетной записи хранения следующим фрагментом кода JSON:

    ```json
    {
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2018-05-01",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri":""
          },
          "parameters": {
              "storageAccountName":{"value": "[variables('storageAccountName')]"},
              "location":{"value": "[parameters('location')]"}
          }
      }
    },
    ```

    Обратите внимание на следующие детали.

    * В основном шаблоне используется ресурс `Microsoft.Resources/deployments` для ссылки на другой шаблон.
    * Ресурс `deployments` имеет имя `linkedTemplate`. Это имя используется для [настройки зависимостей](#configure-dependency).
    * Для обоих связанного и вложенного шаблонов можно использовать только [добавочный](./deployment-modes.md) режим развертывания.
    * `templateLink/uri` содержит URI связанного шаблона. Обновите это значение, указав URI, полученный при отправке связанного шаблона (тот, который с маркером SAS).
    * Чтобы передать значение из основного шаблона в связанный, используйте `parameters`.
1. Укажите для элемента `uri` значение, полученное при отправке связанного шаблона (тот, который с маркером SAS). На практике вам необходимо указать параметр для универсального кода ресурса (URI).
1. Сохраните измененный шаблон.

## <a name="configure-dependency"></a>Настройка зависимостей

Как следует из статьи [Руководство. Создание шаблонов Azure Resource Manager с зависимыми ресурсами](./template-tutorial-create-templates-with-dependent-resources.md), ресурс виртуальной машины зависит от учетной записи хранения:

![Схема зависимости шаблонов Azure Resource Manager в Visual Studio Code](./media/template-tutorial-create-linked-templates/resource-manager-template-visual-studio-code-dependency-diagram.png)

Так как учетная запись хранения теперь определяется в связанном шаблоне, необходимо обновить следующие два элемента ресурса `Microsoft.Compute/virtualMachines`.

* Перенастройте элемент `dependsOn`. Определение учетной записи хранения перемещается на связанный шаблон.
* Перенастройте элемент `properties/diagnosticsProfile/bootDiagnostics/storageUri`. В разделе [Создать связанный шаблон](#create-the-linked-template) вы добавили значение выходных данных.

    ```json
    "outputs": {
        "storageUri": {
            "type": "string",
            "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
            }
    }
    ```

    Основной шаблон требует это значение.

1. Откройте файл azuredeploy.json в Visual Studio Code, если он еще не открыт.
2. Разверните определение ресурса виртуальной машины, обновив **dependsOn**, как показано на следующем снимке экрана.

    ![Связанные шаблоны Azure Resource Manager настраивают зависимости](./media/template-tutorial-create-linked-templates/resource-manager-template-linked-templates-configure-dependency.png)

    *linkedTemplate* — имя ресурса развертываний.
3. Обновите **properties/diagnosticsProfile/bootDiagnostics/storageUri**, как показано на предыдущем снимке экрана.
4. Сохраните измененный шаблон.

## <a name="deploy-the-template"></a>Развертывание шаблона

Дополнительные сведения о процедуре развертывания шаблона см. в [этом разделе](./template-tutorial-create-templates-with-dependent-resources.md#deploy-the-template). Используйте для хранения связанного шаблона ту же группу ресурсов, к которой принадлежит учетная запись хранения. Это упростит очистку ресурсов, выполняемую при работе со следующим разделом. Для повышения уровня безопасности используйте пароль, созданный для учетной записи администратора виртуальной машины. См. раздел [Предварительные требования](#prerequisites).

## <a name="clean-up-resources"></a>Очистка ресурсов

Если ресурсы Azure больше не нужны, их можно удалить. Для этого необходимо удалить группу ресурсов.

1. На портале Azure в меню слева выберите **Группа ресурсов**.
2. В поле **Фильтровать по имени** введите имя группы ресурсов.
3. Выберите имя группы ресурсов.  В группе ресурсов должно появится шесть ресурсов.
4. В главном меню выберите **Удалить группу ресурсов**.

## <a name="additional-practice"></a>Дополнительные рекомендации

Чтобы улучшить готовый проект, внесите в него следующие дополнительные изменения:

1. Измените основной шаблон (azuredeploy.json) таким образом, чтобы он принимал значение URI связанного шаблона с помощью параметра.
2. Вместо создания маркера SAS при передаче связанного шаблона создайте его при развертывании основного шаблона. Дополнительные сведения см. в разделе [Предоставление маркера SAS во время развертывания](./secure-template-with-sas-token.md#provide-sas-token-during-deployment).

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы разделили шаблон на основной и связанный. Чтобы узнать, как использовать расширения виртуальной машины для задач, выполняемых после развертывания, перейдите к следующей статье:

> [!div class="nextstepaction"]
> [Развертывание расширений виртуальной машины](./template-tutorial-deploy-vm-extensions.md)
