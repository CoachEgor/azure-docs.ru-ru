---
title: Устранение распространенных ошибок развертывания в Azure | Документация Майкрософт
description: Описывается устранение распространенных ошибок при развертывании ресурсов в Azure с помощью Azure Resource Manager.
tags: top-support-issue
author: tfitzmac
keywords: ошибка развертывания, развертывание Azure, развернуть в Azure
ms.service: azure-resource-manager
ms.topic: troubleshooting
ms.date: 10/04/2019
ms.author: tomfitz
ms.openlocfilehash: ac700592a63e88936593c24f8f7ce06a08e289ce
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2019
ms.locfileid: "71972688"
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Устранение распространенных ошибок развертывания в Azure с помощью Azure Resource Manager | Microsoft Azure

В этой статье описаны некоторые распространенные ошибки при развертывании в Azure и предоставлены сведения об их устранении. Если не удается найти код ошибки развертывания, ознакомьтесь с разделом [Поиск кода ошибки](#find-error-code).

Если вам нужны сведения о коде ошибки и эта информация не указана в этой статье, сообщите нам об этом. В нижней части этой страницы можно оставить отзыв. Отзывы отписываются с проблемами GitHub.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="error-codes"></a>Коды ошибок

| Код ошибки | Предотвращение | Дополнительная информация |
| ---------- | ---------- | ---------------- |
| AccountNameInvalid | Следуйте ограничениям для имен учетных записей хранения. | [Устранение ошибок, связанных с именами учетных записей хранения](resource-manager-storage-account-name-errors.md) |
| AccountPropertyCannotBeSet | Проверьте доступные свойства учетной записи хранения. | [Справочник по шаблонам Microsoft.Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts) |
| AllocationFailed | Кластер или регион не имеют доступных ресурсов или не поддерживают запрашиваемый размер виртуальной машины. Повторите запрос позже или укажите другой размер виртуальной машины. | Проблемы подготовки и распределения для [Linux](../virtual-machines/linux/troubleshoot-deployment-new-vm.md) и [Windows](../virtual-machines/windows/troubleshoot-deployment-new-vm.md), и [устранение ошибок выделения ресурсов](../virtual-machines/troubleshooting/allocation-failure.md)|
| AnotherOperationInProgress | Дождитесь завершения параллельной операции. | |
| AuthorizationFailed | Учетная запись или субъект-служба не имеют необходимых прав доступа для выполнения развертывания. Проверьте роль, к которой принадлежит учетная запись, и ее права доступа к области развертывания.<br><br>Эта ошибка может возникать, если требуемый поставщик ресурсов не зарегистрирован. | [Управление доступом на основе ролей в Azure](../role-based-access-control/role-assignments-portal.md)<br><br>[Устранение ошибок регистрации](resource-manager-register-provider-errors.md) |
| BadRequest | Отправленные значения развертывания не соответствуют значениям, ожидаемым Resource Manager. Проверьте внутреннее сообщение о состоянии. Оно поможет вам в устранении неполадки. | [Справочник по шаблону](/azure/templates/) и [поддерживаемые расположения](resource-location.md) |
| Конфликт | Запрашиваемая операция не разрешена в текущем состоянии ресурса. Например, изменение размера диска разрешено только при создании или освобождении виртуальной машины. | |
| деплойментактивеандунедитабле | Дождитесь завершения параллельного развертывания в эту группу ресурсов. | |
| деплойментнамеинвалидчарактерс | Имя развертывания может содержать только буквы, цифры, символы "-", "." и "_". | |
| деплойментнамеленгслимитексцеедед | Длина имен развертывания ограничена 64 символами.  | |
| DeploymentFailed | Ошибка DeploymentFailed является общей ошибкой, которая не содержит сведений, необходимых для ее устранения. Найдите в сведениях об ошибке ее код, с помощью которого можно получить дополнительные сведения. | [Поиск кода ошибки](#find-error-code) |
| DeploymentQuotaExceeded | Если вы достигли предела в 800 развертываний на группу ресурсов, удалите из журнала те развертывания, которые больше не нужны. | [Устранять ошибку, если число развертываний превышает 800](deployment-quota-exceeded.md) |
| DnsRecordInUse | Имя записи DNS должно быть уникальным. Введите другое имя. | |
| ImageNotFound | Проверьте параметры образа виртуальной машины. |  |
| InUseSubnetCannotBeDeleted | Эта ошибка может возникать при попытке обновить ресурс, и запрос обрабатывается путем удаления и создания ресурса. Укажите все неизменяемые значения. | [Обновление ресурса](/azure/architecture/building-blocks/extending-templates/update-resource) |
| InvalidAuthenticationTokenTenant | Получите маркер доступа для соответствующего клиента. Маркер можно получить только из клиента, которому принадлежит учетная запись. | |
| InvalidContentLink | Скорее всего, вы попытались создать ссылку на вложенный шаблон, который недоступен. Внимательно проверьте URI, указанный для вложенного шаблона. Если шаблон существует в учетной записи хранения, убедитесь, что URI доступен. Возможно, потребуется передать маркер SAS. Сейчас невозможно создать ссылку на шаблон, который находится в учетной записи хранения, расположенной за [брандмауэром службы хранилища Azure](../storage/common/storage-network-security.md). Попробуйте переместить шаблон в другой репозиторий, например GitHub. | [Связанные шаблоны](resource-group-linked-templates.md) |
| InvalidParameter | Одно из значений, предоставленных для ресурса, не соответствует ожидаемому значению. Эта ошибка может возникнуть в результате многих различных состояний. Например, пароля может быть недостаточно или имя большого двоичного объекта может быть неверным. Сообщение об ошибке должно указывать, какое значение необходимо исправить. | |
| InvalidRequestContent | Значения развертывания либо содержат нераспознанные значения, либо отсутствуют необходимые значения. Проверьте значения для типа ресурса. | [Справочник по шаблонам](/azure/templates/) |
| InvalidRequestFormat | Включите ведение журнала отладки при выполнении развертывания и проверьте содержимое запроса. | [Ведение журнала отладки](#enable-debug-logging) |
| InvalidResourceNamespace | Проверьте пространство имен ресурсов, заданное в свойстве **type**. | [Справочник по шаблонам](/azure/templates/) |
| InvalidResourceReference | Ресурс не существует, или на него неверно ссылаются. Проверьте, следует ли добавить зависимость. Убедитесь, что для функции **reference** указаны параметры, необходимые для вашего сценария. | [Устранение ошибок, связанных с зависимостями](resource-manager-not-found-errors.md) |
| InvalidResourceType | Проверьте тип ресурсов, заданный в свойстве **type**. | [Справочник по шаблонам](/azure/templates/) |
| InvalidSubscriptionRegistrationState | Зарегистрируйте подписку в поставщике ресурсов. | [Устранение ошибок регистрации](resource-manager-register-provider-errors.md) |
| InvalidTemplate | Проверьте синтаксис шаблона на наличие ошибок. | [Устранение ошибок, связанных с недопустимым шаблоном](resource-manager-invalid-template-errors.md) |
| InvalidTemplateCircularDependency | Удалите ненужные зависимости. | [Устранение циклических зависимостей](resource-manager-invalid-template-errors.md#circular-dependency) |
| LinkedAuthorizationFailed | Проверьте, принадлежит ли ваша учетная запись к тому же клиенту, что и группа ресурсов, в которую выполняется развертывание. | |
| LinkedInvalidPropertyId | Идентификатор ресурса не удается разрешить правильно. Убедитесь, что указаны все обязательные значения для идентификатора ресурса, включая идентификатор подписки, имя группы ресурсов, тип ресурса, имя родительского ресурса (если необходимо) и имя ресурса. | |
| LocationRequired | Укажите расположение ресурса. | [Определение расположения](resource-location.md) |
| MismatchingResourceSegments | Убедитесь, что имя и тип вложенного ресурса содержат правильное количество сегментов. | [Разрешение сегментов ресурса](resource-manager-invalid-template-errors.md#incorrect-segment-lengths)
| MissingRegistrationForLocation | Проверьте состояние регистрации поставщика ресурсов и поддерживаемые расположения. | [Устранение ошибок регистрации](resource-manager-register-provider-errors.md) |
| MissingSubscriptionRegistration | Зарегистрируйте подписку в поставщике ресурсов. | [Устранение ошибок регистрации](resource-manager-register-provider-errors.md) |
| NoRegisteredProviderFound | Проверьте состояние регистрации поставщика ресурсов. | [Устранение ошибок регистрации](resource-manager-register-provider-errors.md) |
| Не найдено | Возможно, вы пытаетесь развернуть зависимый ресурс параллельно с родительским ресурсом. Проверьте, не нужно ли добавить зависимость. | [Устранение ошибок, связанных с зависимостями](resource-manager-not-found-errors.md) |
| OperationNotAllowed | Развертывание пытается выполнить операцию, которая превышает квоту для подписки, группы ресурсов или региона. Если это возможно, измените развертывание, чтобы не превышать квоты. В противном случае запросите изменение квот. | [Устранение ошибок квот ресурсов](resource-manager-quota-errors.md) |
| ParentResourceNotFound | Убедитесь, что имеется родительский ресурс, прежде чем создавать дочерние ресурсы. | [Устранение ошибок, связанных с родительскими ресурсами](resource-manager-parent-resource-errors.md) |
| PasswordTooLong | Возможно, вы выбрали пароль, содержащий слишком много символов, или преобразовали значение пароля в защищенную строку перед передачей его в качестве параметра. Если шаблон содержит параметр **защищенной строки**, не нужно преобразовывать значение в защищенную строку. Укажите значение пароля в виде текста. |  |
| PrivateIPAddressInReservedRange | Указанный IP-адрес включает диапазон адресов, необходимый Azure. Измените IP-адрес, чтобы не использовать зарезервированный диапазон. | [IP-адреса](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PrivateIPAddressNotInSubnet | Указанный IP-адрес находится вне диапазона подсети. Измените IP-адрес, чтобы он находился в пределах диапазона подсети. | [IP-адреса](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PropertyChangeNotAllowed | Некоторые свойства не могут быть изменены в развернутом ресурсе. При обновлении ресурса измените только допустимые свойства. | [Обновление ресурса](/azure/architecture/building-blocks/extending-templates/update-resource) |
| RequestDisallowedByPolicy | Ваша подписка включает в себя политику ресурсов, которая не позволяет выполнить действие, которое вы пытаетесь выполнять во время развертывания. Найдите политику, которая блокирует действие. Если это возможно, измените развертывание в соответствии с ограничениями политики. | [Устранение ошибок с политиками](resource-manager-policy-requestdisallowedbypolicy-error.md) |
| ReservedResourceName | Укажите ресурс, имя которого не включает в себя зарезервированное имя. | [Зарезервированные имена ресурсов](resource-manager-reserved-resource-name.md) |
| ResourceGroupBeingDeleted | Дождитесь завершения удаления. | |
| ResourceGroupNotFound | Проверьте имя целевой группы ресурсов для развертывания. Целевая группа ресурсов уже должна существовать в вашей подписке. Проверьте контекст подписки. | [Azure CLI](/cli/azure/account?#az-account-set), [PowerShell](/powershell/module/Az.Accounts/Set-AzContext) |
| ResourceNotFound | Развертывание ссылается на ресурс, который не может быть разрешен. Убедитесь, что для функции **reference** указаны параметры, необходимые для вашего сценария. | [Устранение ошибок с поиском ресурсов](resource-manager-not-found-errors.md) |
| ResourceQuotaExceeded | Развертывание пытается создать ресурсы, которые превышают квоту для подписки, группы ресурсов или региона. Если возможно, измените инфраструктуру, чтобы не превышать квоты. В противном случае запросите изменение квот. | [Устранение ошибок квот ресурсов](resource-manager-quota-errors.md) |
| SkuNotAvailable | Выберите номер SKU (например, размер виртуальной машины), который доступен в выбранном расположении. | [Устранение ошибок, связанных с недоступностью номера SKU](resource-manager-sku-not-available-errors.md) |
| StorageAccountAlreadyExists | Предоставьте уникальное имя учетной записи хранения. | [Устранение ошибок, связанных с именами учетных записей хранения](resource-manager-storage-account-name-errors.md)  |
| StorageAccountAlreadyTaken | Предоставьте уникальное имя учетной записи хранения. | [Устранение ошибок, связанных с именами учетных записей хранения](resource-manager-storage-account-name-errors.md) |
| StorageAccountNotFound | Проверьте подписку, группу ресурсов и имя учетной записи хранения, которую вы пытаетесь использовать. | |
| SubnetsNotInSameVnet | Виртуальная машина может иметь только одну виртуальную сеть. При развертывании нескольких сетевых адаптеров убедитесь, что они принадлежат той же виртуальной сети. | [Использование нескольких сетевых адаптеров](../virtual-machines/windows/multiple-nics.md) |
| TemplateResourceCircularDependency | Удалите ненужные зависимости. | [Устранение циклических зависимостей](resource-manager-invalid-template-errors.md#circular-dependency) |
| TooManyTargetResourceGroups | Уменьшите количество групп ресурсов для одного развертывания. | [Развертывание в нескольких группах ресурсов](resource-manager-cross-resource-group-deployment.md) |

## <a name="find-error-code"></a>Поиск кода ошибки

Существует два типа ошибок, которые могут произойти:

* ошибки проверки;
* ошибки развертывания.

Ошибки проверки возникают в сценариях, которые можно определить перед развертыванием. Это синтаксические ошибки в шаблоне или попытки развертывания ресурсов, которые приведут к превышению квот для подписки. Ошибки развертывания возникают из-за условий, возникающих во время развертывания. Это попытки получить доступ к ресурсу, который развертывается параллельно.

Ошибки обоих типов возвращают код ошибки, с помощью которого можно устранить неполадки развертывания. Ошибки обоих типов отображаются в [журнале действий](resource-group-audit.md). Однако ошибки проверки не отображаются в журнале развертывания, так как при их наличии развертывание не запускается.

### <a name="validation-errors"></a>Ошибки проверки

При развертывании через портал ошибка проверки появляется после отправки значений.

![Показ ошибки проверки через портал](./media/resource-manager-common-deployment-errors/validation-error.png)

Выберите сообщение для получения дополнительных сведений. На следующем изображении показана ошибка **InvalidTemplateDeployment** и сообщение, которое указывает, что развертывание заблокировано политикой.

![Отображение сведений о проверке](./media/resource-manager-common-deployment-errors/validation-details.png)

### <a name="deployment-errors"></a>Ошибки развертывания

Если операция прошла проверку, но завершилась сбоем во время развертывания, вы увидите ошибку развертывания.

Чтобы просмотреть коды и сообщения ошибок развертывания с помощью PowerShell, используйте следующую команду:

```azurepowershell-interactive
(Get-AzResourceGroupDeploymentOperation -DeploymentName exampledeployment -ResourceGroupName examplegroup).Properties.statusMessage
```

Чтобы просмотреть коды и сообщения ошибок развертывания с помощью Azure CLI, используйте следующую команду:

```azurecli-interactive
az group deployment operation list --name exampledeployment -g examplegroup --query "[*].properties.statusMessage"
```

Выберите уведомление в портале.

![уведомление об ошибке](./media/resource-manager-common-deployment-errors/notification.png)

Вы увидите больше сведений о развертывании. Щелкните область уведомления, чтобы получить дополнительные сведения об ошибке.

![сбой развертывания](./media/resource-manager-common-deployment-errors/deployment-failed.png)

Вы увидите сообщение об ошибке и ее коды. Обратите внимание, что имеется два кода ошибки. Первый код ошибки (**DeploymentFailed**) является общей ошибкой, которая не содержит сведений, необходимых для ее устранения. Второй код ошибки (**StorageAccountNotFound**) предоставляет необходимые сведения.

![Сведения об ошибке](./media/resource-manager-common-deployment-errors/error-details.png)

## <a name="enable-debug-logging"></a>Включение ведения журнала отладки

Иногда требуются дополнительные сведения о запросе и ответе, чтобы узнать, что пошло не так. Вы можете запросить, чтобы во время развертывания дополнительные сведения записывались в журнал.

### <a name="powershell"></a>PowerShell

В PowerShell для параметра **DeploymentDebugLogLevel** задайте значение All, ResponseContent или RequestContent.

```powershell
New-AzResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName examplegroup `
  -TemplateFile c:\Azure\Templates\storage.json `
  -DeploymentDebugLogLevel All
```

Проверьте содержимое запроса с помощью следующего командлета.

```powershell
(Get-AzResourceGroupDeploymentOperation `
-DeploymentName exampledeployment `
-ResourceGroupName examplegroup).Properties.request `
| ConvertTo-Json
```

Или проверьте содержимое ответа, выполнив команду, указанную ниже.

```powershell
(Get-AzResourceGroupDeploymentOperation `
-DeploymentName exampledeployment `
-ResourceGroupName examplegroup).Properties.response `
| ConvertTo-Json
```

Эти сведения помогут определить, правильно ли задано то или иное значение в шаблоне.

### <a name="azure-cli"></a>Инфраструктура CLI Azure

В настоящее время Azure CLI не поддерживает ведение журнала отладки, но вы можете его получить.

Для просмотра операций развертывания выполните следующую команду.

```azurecli
az group deployment operation list \
  --resource-group examplegroup \
  --name exampledeployment
```

Проверьте содержимое запроса с помощью следующей команды:

```azurecli
az group deployment operation list \
  --name exampledeployment \
  -g examplegroup \
  --query [].properties.request
```

Проверьте содержимое ответа с помощью следующей команды:

```azurecli
az group deployment operation list \
  --name exampledeployment \
  -g examplegroup \
  --query [].properties.response
```

### <a name="nested-template"></a>Вложенный шаблон

Чтобы вести журнал отладочной информации для вложенного шаблона, используйте элемент **debugSetting**.

```json
{
    "apiVersion": "2016-09-01",
    "name": "nestedTemplate",
    "type": "Microsoft.Resources/deployments",
    "properties": {
        "mode": "Incremental",
        "templateLink": {
            "uri": "{template-uri}",
            "contentVersion": "1.0.0.0"
        },
        "debugSetting": {
           "detailLevel": "requestContent, responseContent"
        }
    }
}
```

## <a name="create-a-troubleshooting-template"></a>Создание шаблона для устранения неполадок

В некоторых случаях устранить неполадки шаблона проще всего, проверяя его части. Можно создавать упрощенный шаблон, позволяющий сосредоточиться на части, которая, как вы считаете, вызывает ошибку. Для примера предположим, что вы получили сообщение об ошибке при указании ссылки на ресурс. Вместо того, чтобы проверять весь шаблон, создайте шаблон, возвращающий часть, которая может быть причиной проблемы. Это поможет определить, передаются ли правильные параметры, правильно ли используются функции шаблона и получается ли ожидаемый ресурс.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageName": {
        "type": "string"
    },
    "storageResourceGroup": {
        "type": "string"
    }
  },
  "variables": {},
  "resources": [],
  "outputs": {
    "exampleOutput": {
        "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageName')), '2016-05-01')]",
        "type" : "object"
    }
  }
}
```

Или предположим, что возникают ошибки развертывания, которые, как вы считаете, связаны с неправильно заданными зависимостями. Проверьте шаблон, разбив его на более простые шаблоны. Сначала создайте шаблон, который развертывает только один ресурс (например, SQL Server). Когда вы убедитесь, что этот ресурс определен правильно, добавьте зависящий от него ресурс (например, базу данных SQL). Проверив правильность определения этих двух ресурсов, добавьте другие зависимые ресурсы (например, политики аудита). В перерывах между тестовыми развертываниями удаляйте группу ресурсов, чтобы гарантировать адекватную проверку зависимостей.


## <a name="next-steps"></a>Следующие шаги

* Руководство по устранению неполадок см. в разделе [Tutorial: Troubleshoot Resource Manager template deployments](./resource-manager-tutorial-troubleshoot.md) (Руководство. Устранение неполадок развертывания шаблонов Resource Manager)
* Сведения о действиях аудита см. в статье [Операции аудита с помощью Resource Manager](resource-group-audit.md).
* Дополнительные сведения об определении ошибок во время развертывания см. в статье [Просмотр операций развертывания с помощью портала Azure](resource-manager-deployment-operations.md).
