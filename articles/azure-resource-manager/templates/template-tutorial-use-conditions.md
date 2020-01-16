---
title: Использование условия в шаблонах
description: Узнайте, как развернуть ресурсы Azure на основе условий. В этой статье также объясняется, как развернуть новый ресурс либо использовать существующий.
author: mumian
ms.date: 05/21/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 895d82eb79e4674ca95b9052d2384a257b296bf5
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/15/2020
ms.locfileid: "75980658"
---
# <a name="tutorial-use-condition-in-azure-resource-manager-templates"></a>Руководство. Использование условия в шаблонах Azure Resource Manager

Узнайте, как развернуть ресурсы Azure на основе условий.

В руководстве по [настройке порядка развертывания ресурсов](./template-tutorial-create-templates-with-dependent-resources.md) описано, как создать виртуальную машину, виртуальную сеть и некоторые другие зависимые ресурсы, включая учетную запись хранения. Чтобы не создавать учетную запись хранения каждый раз, вы можете позволить пользователям выбрать, создавать ли новую учетную запись хранения или использовать существующую. Для достижения этой цели нужно определить дополнительный параметр. Если значение параметра равно "new", будет создана новая учетная запись хранения. В противном случае используется учетная запись хранения с указанным именем.

![Схема использования условия в шаблонах Azure Resource Manager](./media/template-tutorial-use-conditions/resource-manager-template-use-condition-diagram.png)

В рамках этого руководства рассматриваются следующие задачи:

> [!div class="checklist"]
> * открытие шаблона быстрого запуска;
> * Изменение шаблона
> * Развертывание шаблона
> * Очистка ресурсов

В этом учебнике рассматривается только основной сценарий с использованием условий. Дополнительные сведения см. в разделе:

* [Структура файла шаблона: условие](conditional-resource-deployment.md).
* [Conditionally deploy a resource in an Azure Resource Manager template](/azure/architecture/building-blocks/extending-templates/conditional-deploy) (Условное развертывание ресурсов в шаблоне Azure Resource Manager).
* [Функция шаблона: If](./template-functions-logical.md#if).
* [Comparison functions for Azure Resource Manager templates](./template-functions-comparison.md) (Функции сравнения для шаблонов Azure Resource Manager)

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>предварительные требования

Для работы с этой статьей необходимо иметь следующее.

* Visual Studio Code с расширением средств Resource Manager. Дополнительные сведения см. в статье [Use Visual Studio Code to create Azure Resource Manager templates](use-vs-code-to-create-template.md) (Создание шаблонов Azure Resource Manager с помощью Visual Studio Code).
* Для повышения уровня безопасности используйте пароль, созданный для учетной записи администратора виртуальной машины. Ниже приведен пример создания пароля.

    ```azurecli-interactive
    openssl rand -base64 32
    ```

    Для защиты криптографических ключей и других секретов используйте Azure Key Vault. Дополнительные сведения см. в статье [Учебник. Интеграция с Azure Key Vault при развертывании шаблона Resource Manager](./template-tutorial-use-key-vault.md). Мы также рекомендуем обновлять пароль каждые три месяца.

## <a name="open-a-quickstart-template"></a>Открытие шаблона быстрого запуска

Шаблоны быстрого запуска Azure являются репозиторием для шаблонов Resource Manager. Вместо создания шаблона с нуля можно найти пример шаблона и настроить его. Шаблон, используемый в этом руководстве, называется [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Развертывание простой виртуальной машины Windows).

1. В Visual Studio Code выберите **Файл**>**Открыть файл**.
2. Скопируйте приведенный ниже URL-адрес и вставьте его в поле **Имя файла**.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

3. Чтобы открыть файл, выберите **Открыть**.
4. Шаблоном определено пять ресурсов:

   * `Microsoft.Storage/storageAccounts`. Ознакомьтесь со статьей о [справочнике по шаблонам](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
   * `Microsoft.Network/publicIPAddresses`. Ознакомьтесь со статьей о [справочнике по шаблонам](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
   * `Microsoft.Network/virtualNetworks`. Ознакомьтесь со статьей о [справочнике по шаблонам](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
   * `Microsoft.Network/networkInterfaces`. Ознакомьтесь со статьей о [справочнике по шаблонам](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
   * `Microsoft.Compute/virtualMachines`. Ознакомьтесь со статьей о [справочнике по шаблонам](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

     Прежде чем настраивать шаблон, рекомендуется получить основные сведения о нем.
5. Выберите **Файл**>**Сохранить как**, чтобы сохранить файл на локальный компьютер с именем **azuredeploy.json**.

## <a name="modify-the-template"></a>Изменение шаблона

Внесите два изменения в имеющийся шаблон:

* Добавьте параметр имени для учетной записи хранения. Пользователи могут указать имя новой или существующей учетной записи хранения.
* Добавьте новый параметр с именем **newOrExisting**. Развертывание использует этот параметр для определения того, нужно ли создавать новую учетную запись хранения или использовать имеющуюся.

Вот что нужно сделать, чтобы внести изменения:

1. Откройте файл **azuredeploy.json** в Visual Studio Code.
2. Замените во всем шаблоне **three variables('storageAccountName')** на **parameters('storageAccountName')** .
3. Удалите следующее определение переменной:

    ![Схема использования условия в шаблонах Azure Resource Manager](./media/template-tutorial-use-conditions/resource-manager-tutorial-use-condition-template-remove-storageaccountname.png)

4. Добавьте следующие два параметра в шаблон:

    ```json
    "storageAccountName": {
      "type": "string"
    },
    "newOrExisting": {
      "type": "string",
      "allowedValues": [
        "new",
        "existing"
      ]
    },
    ```

    Определение обновленных параметров выглядит так:

    ![Условие использования Resource Manager](./media/template-tutorial-use-conditions/resource-manager-tutorial-use-condition-template-parameters.png)

5. Добавьте следующую строку в начало определения учетной записи хранения.

    ```json
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    ```

    Условие проверяет значение параметра с именем **newOrExisting**. Если значение параметра равно **new**, то в результате развертывания создается учетная запись хранения.

    Обновленное определение учетной записи хранения выглядит так:

    ![Условие использования Resource Manager](./media/template-tutorial-use-conditions/resource-manager-tutorial-use-condition-template.png)
6. Обновите свойство **storageUri** определения ресурса виртуальной машины следующим значением:

    ```json
    "storageUri": "[concat('https://', parameters('storageAccountName'), '.blob.core.windows.net')]"
    ```

    Это изменение необходимо при использовании существующей учетной записи хранения с другой группой ресурсов.

7. Сохраните изменения.

## <a name="deploy-the-template"></a>Развертывание шаблона

Следуйте инструкциям из раздела о [развертывании шаблона](./template-tutorial-create-templates-with-dependent-resources.md#deploy-the-template), чтобы открыть Cloud Shell и отправить измененный шаблон, а затем выполните скрипт PowerShell для развертывания шаблона.

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"
$newOrExisting = Read-Host -Prompt "Create new or use existing (Enter new or existing)"
$location = Read-Host -Prompt "Enter the Azure location (i.e. centralus)"
$vmAdmin = Read-Host -Prompt "Enter the admin username"
$vmPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString
$dnsLabelPrefix = Read-Host -Prompt "Enter the DNS Label prefix"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -adminUsername $vmAdmin `
    -adminPassword $vmPassword `
    -dnsLabelPrefix $dnsLabelPrefix `
    -storageAccountName $storageAccountName `
    -newOrExisting $newOrExisting `
    -TemplateFile "$HOME/azuredeploy.json"
```

> [!NOTE]
> Развертывание завершается сбоем, если **newOrExisting** — **new**, а учетная запись хранения с указанным именем уже существует.

Попробуйте сделать другое развертывания, задав для **newOrExisting** значение "existing" и указав имеющуюся учетную запись хранения. Чтобы создать учетную запись хранения, обратитесь к [этой статье](../../storage/common/storage-account-create.md).

## <a name="clean-up-resources"></a>Очистка ресурсов

Если ресурсы Azure больше не нужны, их можно удалить. Для этого необходимо удалить группу ресурсов. Чтобы удалить группу ресурсов, выберите **Попробовать**, чтобы открыть Cloud Shell. Чтобы вставить сценарий PowerShell, щелкните панель оболочки правой кнопкой мыши и выберите **Вставить**.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the same resource group name you used in the last procedure"
Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве описано, как разработать шаблон, позволяющий пользователю выбирать между созданием учетной записи хранения и использованием существующей. Чтобы узнать, как получать секреты из Azure Key Vault и использовать их в шаблоне развертывания в качестве паролей, обратитесь к статье:

> [!div class="nextstepaction"]
> [Руководство. Интеграция Azure Key Vault в развертывание шаблона Resource Manager](./template-tutorial-use-key-vault.md)
