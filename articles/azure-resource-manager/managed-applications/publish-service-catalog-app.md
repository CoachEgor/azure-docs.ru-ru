---
title: Публикация управляемого приложения в каталоге услуг
description: Показано, как создать управляемое приложение Azure, предназначенное для членов вашей организации.
author: tfitzmac
ms.topic: tutorial
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: e756617a700d258078e84a3fa11c8aceb6f4dd88
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2020
ms.locfileid: "75903261"
---
# <a name="create-and-publish-a-managed-application-definition"></a>Создание и публикация определения управляемого приложения

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Вы можете создавать и публиковать [управляемые приложения Azure](overview.md), предназначенные для членов вашей организации. Например, отдел ИТ может публиковать управляемые приложения, которые соответствуют стандартам организации. Эти управляемые приложения доступны в каталоге служб, а не в Azure Marketplace.

Чтобы опубликовать управляемое приложение в каталоге услуг Azure, необходимо выполнить следующее.

* Создайте шаблон, который определяет ресурсы, развертываемые с управляемым приложением.
* Определите элементы пользовательского интерфейса для портала при развертывании управляемого приложения.
* Создайте ZIP-файл пакета, содержащий необходимые файлы шаблонов.
* Определите, каким пользователям, группам или приложениям нужен доступ к группе ресурсов в подписке пользователя.
* Создайте определение управляемого приложения, которое указывает на ZIP-файл пакета и запрашивает доступ для получения удостоверения.

Используемое в этой статье управляемое приложение имеет только учетную запись хранения. В этом руководстве мы покажем, как опубликовать управляемое приложение. Полные примеры приведены в статье [Примеры проектов для управляемых приложений Azure](sample-projects.md).

Для работы примеров PowerShell в этой статье требуется Azure PowerShell 6.2 или более поздняя версия. [Обновите свою версию](/powershell/azure/install-Az-ps), если необходимо.

## <a name="create-the-resource-template"></a>Создание шаблона ресурсов

В каждом определении управляемого приложения указан файл с именем **mainTemplate.json**. В нем определяются ресурсы Azure, которые необходимо развернуть. Этот шаблон ничем не отличается от обычного шаблона Resource Manager.

Создайте файл с именем **mainTemplate.json**. В нем учитывается регистр.

Добавьте в файл приведенный ниже код JSON. В нем определяются параметры для создания учетной записи хранения и задаются свойства для нее.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        },
        "storageAccountType": {
            "type": "string"
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "storageAccountName": "[concat(parameters('storageAccountNamePrefix'), uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            "apiVersion": "2016-01-01",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('storageAccountType')]"
            },
            "kind": "Storage",
            "properties": {}
        }
    ],
    "outputs": {
        "storageEndpoint": {
            "type": "string",
            "value": "[reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
        }
    }
}
```

Сохраните файл mainTemplate.json.

## <a name="defining-your-create-experience-using-createuidefinitionjson"></a>Определение процесса создания с помощью файла CreateUiDefinition.json

Как издатель вы определяете процесс создания с помощью файла **createUiDefinition.json**, который реализует интерфейс для пользователей, создающих управляемые приложения. Вы определяете, как пользователи вводят значения для каждого параметра, используя [элементы управления](create-uidefinition-elements.md), в том числе раскрывающиеся списки, текстовые поля и поля для пароля.

Создайте файл с именем **createUiDefinition.json** (в этом имени учитывается регистр).

Добавьте следующее содержимое JSON в файл и сохраните его.

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
   "handler": "Microsoft.Azure.CreateUIDef",
   "version": "0.1.2-preview",
   "parameters": {
        "basics": [
            {}
        ],
        "steps": [
            {
                "name": "storageConfig",
                "label": "Storage settings",
                "subLabel": {
                    "preValidation": "Configure the infrastructure settings",
                    "postValidation": "Done"
                },
                "bladeTitle": "Storage settings",
                "elements": [
                    {
                        "name": "storageAccounts",
                        "type": "Microsoft.Storage.MultiStorageAccountCombo",
                        "label": {
                            "prefix": "Storage account name prefix",
                            "type": "Storage account type"
                        },
                        "defaultValue": {
                            "type": "Standard_LRS"
                        },
                        "constraints": {
                            "allowedTypes": [
                                "Premium_LRS",
                                "Standard_LRS",
                                "Standard_GRS"
                            ]
                        }
                    }
                ]
            }
        ],
        "outputs": {
            "storageAccountNamePrefix": "[steps('storageConfig').storageAccounts.prefix]",
            "storageAccountType": "[steps('storageConfig').storageAccounts.type]",
            "location": "[location()]"
        }
    }
}
```

Дополнительные сведения см. в статье [CreateUiDefitinion.json for Azure managed application's create experience](create-uidefinition-overview.md) (Файл CreateUiDefitinion.json для создания интерфейсов управляемых приложений Azure).

## <a name="package-the-files"></a>Упаковка файлов

Добавьте два файла в ZIP-файл с именем app.zip. Приведенные выше два файла должны быть расположены на корневом уровне ZIP-файла. Если поместить их в папку, то при создании определения управляемого приложения появится сообщение об ошибке из-за отсутствия необходимых файлов. 

Передайте пакет в доступное расположение, из которого его можно будет использовать. 

```powershell
New-AzResourceGroup -Name storageGroup -Location eastus
$storageAccount = New-AzStorageAccount -ResourceGroupName storageGroup `
  -Name "mystorageaccount" `
  -Location eastus `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context

New-AzStorageContainer -Name appcontainer -Context $ctx -Permission blob

Set-AzStorageBlobContent -File "D:\myapplications\app.zip" `
  -Container appcontainer `
  -Blob "app.zip" `
  -Context $ctx 
```

## <a name="create-the-managed-application-definition"></a>Создание определения управляемого приложения

### <a name="create-an-azure-active-directory-user-group-or-application"></a>Создание группы пользователей или приложения Azure Active Directory

Следующий шаг — выбор группы пользователей или приложения для управления ресурсами от имени клиента. Этой группе ресурсов или приложению предоставляются разрешения в управляемой группе ресурсов в соответствии с назначенной ролью. Это может быть любая встроенная роль управления доступом на основе ролей (RBAC), например "Владелец" или "Участник". Можно также предоставить разрешение на управление ресурсами отдельному пользователю, но обычно это разрешение назначается группе пользователей. Чтобы создать группу пользователей Active Directory, ознакомьтесь с разделом [Создание группы и добавление в нее пользователей в Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Требуется идентификатор объекта группы пользователей для управления ресурсами. 

```powershell
$groupID=(Get-AzADGroup -DisplayName mygroup).Id
```

### <a name="get-the-role-definition-id"></a>Получение идентификатора определения роли

Далее необходимо получить идентификатор определения встроенной роли RBAC, к которой вы хотите предоставить доступ пользователю, группе пользователей или приложению. Обычно это роль "Владелец", "Участник" или "Читатель". Следующая команда позволяет получить идентификатор определения роли "Владелец".

```powershell
$ownerID=(Get-AzRoleDefinition -Name Owner).Id
```

### <a name="create-the-managed-application-definition"></a>Создание определения управляемого приложения

Если у вас еще нет группы ресурсов для хранения определения управляемого приложения, создайте ее.

```powershell
New-AzResourceGroup -Name appDefinitionGroup -Location westcentralus
```

Теперь создайте ресурс определения управляемого приложения.

```powershell
$blob = Get-AzStorageBlob -Container appcontainer -Blob app.zip -Context $ctx

New-AzManagedApplicationDefinition `
  -Name "ManagedStorage" `
  -Location "westcentralus" `
  -ResourceGroupName appDefinitionGroup `
  -LockLevel ReadOnly `
  -DisplayName "Managed Storage Account" `
  -Description "Managed Azure Storage Account" `
  -Authorization "${groupID}:$ownerID" `
  -PackageFileUri $blob.ICloudBlob.StorageUri.PrimaryUri.AbsoluteUri
```

## <a name="bring-your-own-storage-for-the-managed-application-definition"></a>Предоставление собственного хранилища для определения управляемого приложения
Вы можете сохранить определение управляемого приложения в учетной записи хранения, предоставленной вами во время создания, чтобы полностью контролировать его расположение и доступ в соответствии с вашими нормативными потребностями.

> [!NOTE]
> Подключение к собственному хранилищу поддерживается только с шаблоном ARM или развертыванием REST API для определения управляемого приложения.

### <a name="select-your-storage-account"></a>Выбор учетной записи хранения
Необходимо [создать учетную запись хранения](../../storage/common/storage-account-create.md), в которой будет содержаться определение управляемого приложения для использования с каталогом услуг.

Скопируйте идентификатор ресурса учетной записи хранения. Он будет использоваться позже при развертывании определения.

### <a name="set-the-role-assignment-for-appliance-resource-provider-in-your-storage-account"></a>Задание назначения роли "Поставщик ресурсов устройства" в учетной записи хранения
Перед развертыванием определения управляемого приложения в учетной записи хранения необходимо предоставить разрешения участника для роли **Поставщик ресурсов устройства**, чтобы предоставить разрешение на запись файлов определений в контейнер вашей учетной записи хранения.

1. Войдите в свою учетную запись хранения на [портале Azure](https://portal.azure.com).
1. Выберите **Управление доступом (IAM)** , чтобы отобразить параметры управления доступом для учетной записи хранения. Выберите вкладку **Назначения ролей**, чтобы просмотреть список назначений ролей.
1. В окне **Добавить назначение ролей** выберите роль **Участник**. 
1. Из поля **Назначение доступа к** выберите **пользователя, группу или субъект-службу Azure AD**.
1. В разделе **Выбрать**, найдите **Поставщик ресурсов устройства** и выберите его.
1. Сохраните назначение роли.

### <a name="deploy-the-managed-application-definition-with-an-arm-template"></a>Развертывание определения управляемого приложения с помощью шаблона ARM 

Используйте следующий шаблон ARM для развертывания упакованного управляемого приложения в качестве нового определения управляемого приложения в каталоге услуг, файлы определения которого хранятся и обслуживаются в вашей учетной записи хранения.
   
```json
    {
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        },
        "applicationName": {
            "type": "string",
            "metadata": {
                "description": "Managed Application name"
            }
        },
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
        "definitionStorageResourceID": {
            "type": "string",
            "metadata": {
                "description": "Storage account resource ID for where you're storing your definition"
            }
        },
        "_artifactsLocation": {
            "type": "string",
            "metadata": {
                "description": "The base URI where artifacts required by this template are located."
            }
        }
    },
    "variables": {
        "lockLevel": "None",
        "description": "Sample Managed application definition",
        "displayName": "Sample Managed application definition",
        "managedApplicationDefinitionName": "[parameters('applicationName')]",
        "packageFileUri": "[parameters('_artifactsLocation')]",
        "defLocation": "[parameters('definitionStorageResourceID')]",
        "managedResourceGroupId": "[concat(subscription().id,'/resourceGroups/', concat(parameters('applicationName'),'_managed'))]",
        "applicationDefinitionResourceId": "[resourceId('Microsoft.Solutions/applicationDefinitions',variables('managedApplicationDefinitionName'))]"
    },
    "resources": [
        {
            "type": "Microsoft.Solutions/applicationDefinitions",
            "apiVersion": "2019-07-01",
            "name": "[variables('managedApplicationDefinitionName')]",
            "location": "[parameters('location')]",
            "properties": {
                "lockLevel": "[variables('lockLevel')]",
                "description": "[variables('description')]",
                "displayName": "[variables('displayName')]",
                "packageFileUri": "[variables('packageFileUri')]",
                "storageAccountId": "[variables('defLocation')]"
            }
        }
    ],
    "outputs": {}
}
```

Мы добавили новое свойство с именем **storageAccountId** в свойства applicationDefintion и предоставляем идентификатор учетной записи хранения, в качестве значения которого вы хотите сохранить определение.

Вы можете убедиться, что файлы определения приложения сохранены в предоставленной учетной записи хранения в контейнере, под именем **applicationdefinitions**.

> [!NOTE]
> Для повышения безопасности можно создать определение управляемых приложений, чтобы сохранить его в [большом двоичном объекте учетной записи хранения Azure, где включено шифрование](../../storage/common/storage-service-encryption.md). Содержимое определений шифруется с помощью параметров шифрования учетной записи хранения. Определение в каталоге услуг могут видеть только пользователи с правами доступа к файлу.

### <a name="make-sure-users-can-see-your-definition"></a>Убедитесь, что пользователи могут видеть ваше определение.

У вас есть доступ к определению управляемого приложения, но вы хотите, чтобы другие пользователи в вашей организации могли получить к нему доступ. Предоставьте им роль читателя в определении. Они могут унаследовать этот уровень доступа из группы подписки или ресурса. Чтобы узнать, как проверить, кто имеет доступ к определению, и добавить пользователей или группы, см. статью [Использование управления доступом на основе ролей для доступа к ресурсам подписки Azure](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Дальнейшие действия

* Сведения о том, как опубликовать управляемое приложение в Azure Marketplace, см. в статье [Управляемые приложения Azure в Marketplace](publish-marketplace-app.md).
* Сведения о том, как развернуть экземпляр управляемого приложения, см. в статье [Deploy service catalog app through Azure portal](deploy-service-catalog-quickstart.md) (Развертывание приложения из каталога служб с помощью портала Azure).
