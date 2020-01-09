---
title: Создание нескольких экземпляров ресурсов
description: Узнайте, как создать шаблон Azure Resource Manager для создания нескольких экземпляров ресурса Azure.
author: mumian
ms.date: 03/04/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 94a7218933935aa1a9d38a9fc420bb8d3f669492
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75471369"
---
# <a name="tutorial-create-multiple-resource-instances-with-resource-manager-templates"></a>Руководство. Создание нескольких экземпляров ресурса с помощью шаблонов Resource Manager

Узнайте, как выполнить итерацию в шаблоне Azure Resource Manager для создания нескольких экземпляров ресурса Azure. В этом руководстве описано, как изменить шаблон для создания трех экземпляров учетной записи хранения.

![Создание нескольких экземпляров с помощью Azure Resource Manager](./media/template-tutorial-create-multiple-instances/resource-manager-template-create-multiple-instances-diagram.png)

В рамках этого руководства рассматриваются следующие задачи:

> [!div class="checklist"]
> * открытие шаблона быстрого запуска;
> * Изменение шаблона
> * Развертывание шаблона

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>предварительные требования

Для работы с этой статьей необходимо иметь следующее.

* Visual Studio Code с расширением средств Resource Manager. Дополнительные сведения см. в статье [Use Visual Studio Code to create Azure Resource Manager templates](use-vs-code-to-create-template.md) (Создание шаблонов Azure Resource Manager с помощью Visual Studio Code).

## <a name="open-a-quickstart-template"></a>Открытие шаблона быстрого запуска

[Шаблоны быстрого запуска Azure](https://azure.microsoft.com/resources/templates/) — это репозиторий для шаблонов Resource Manager. Вместо создания шаблона с нуля можно найти пример шаблона и настроить его. Шаблон, используемый в этом кратком руководстве, называется [Create a standard storage account](https://azure.microsoft.com/resources/templates/101-storage-account-create/) (Создание стандартной учетной записи хранения). Шаблон определяет ресурс учетной записи службы хранилища Azure.

1. В Visual Studio Code выберите **Файл**>**Открыть файл**.
2. Скопируйте приведенный ниже URL-адрес и вставьте его в поле **Имя файла**.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Чтобы открыть файл, выберите **Открыть**.
4. Вы увидите ресурс Microsoft.Storage/storageAccounts, определенный в шаблоне. Сравните шаблон с примером в [справочнике по шаблону](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts). Прежде чем настраивать шаблон, рекомендуется получить основные сведения о нем.
5. Выберите **Файл**>**Сохранить как**, чтобы сохранить файл в качестве **azuredeploy.json** на локальном компьютере.

## <a name="edit-the-template"></a>Изменение шаблона

С помощью существующего шаблона можно создать одну учетную запись хранения. Можно настроить шаблон, чтобы создать три учетные записи хранения.

Внесите следующие четыре изменения в Visual Studio Code.

![Создание нескольких экземпляров с помощью Azure Resource Manager](./media/template-tutorial-create-multiple-instances/resource-manager-template-create-multiple-instances.png)

1. Добавьте элемент `copy` к определению ресурса учетной записи хранения. В элементе копирования укажите число итераций и переменную для этого цикла. Значение count должно быть положительным целым числом не больше 800.
2. Функция `copyIndex()` возвращает текущую итерацию в цикле. Индекс используется в качестве префикса имени. `copyIndex()` отсчитывается, начиная с нуля. Чтобы сместить значение индекса, можно передать нужное значение в функцию copyIndex(). Например, *copyIndex(1)* .
3. Удалите элемент **Переменные**, поскольку он больше не используется.
4. Удалите элемент **Выходные данные**. Он больше не требуется.

Завершенный шаблон выглядит следующим образом.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
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
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "apiVersion": "2018-02-01",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": 3
      }
    }
  ]
}
```

Дополнительные сведения о создании нескольких экземпляров см. в разделе [Развертывание нескольких экземпляров ресурса или свойства в шаблонах Azure Resource Manager](./create-multiple-instances.md).

## <a name="deploy-the-template"></a>Развертывание шаблона

Дополнительные сведения о [развертывании шаблона](quickstart-create-templates-use-visual-studio-code.md#deploy-the-template) см. в кратком руководстве по Visual Studio Code.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Чтобы получить список всех трех учетных записей хранения, опустите параметр --name.

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)
```azurecli
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az storage account list --resource-group $resourceGroupName
```

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
Get-AzStorageAccount -ResourceGroupName $resourceGroupName
```

---

Сравните имена учетных записей хранения с определением имени в шаблоне.

## <a name="clean-up-resources"></a>Очистка ресурсов

Если ресурсы Azure больше не нужны, их можно удалить. Для этого необходимо удалить группу ресурсов.

1. На портале Azure в меню слева выберите **Группа ресурсов**.
2. В поле **Фильтровать по имени** введите имя группы ресурсов.
3. Выберите имя группы ресурсов.  В группе ресурсов должно появится шесть ресурсов.
4. В главном меню выберите **Удалить группу ресурсов**.

## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как создать несколько экземпляров учетной записи хранения.  С помощью следующего руководства вы разработаете шаблон с несколькими ресурсами и несколькими типами ресурсов. Некоторые ресурсы обладают зависимыми ресурсами.

> [!div class="nextstepaction"]
> [Создание шаблонов Azure Resource Manager с зависимыми ресурсами](./template-tutorial-create-templates-with-dependent-resources.md)
