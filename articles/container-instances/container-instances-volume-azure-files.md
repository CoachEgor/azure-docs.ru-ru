---
title: Подключение тома службы файлов Azure в службе "Экземпляры контейнеров Azure"
description: Узнайте, как подключить том файлов Azure для сохранения состояния с помощью Экземпляров контейнеров Azure
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 11/05/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 365264d40554f45533e2ddf0aeb9d85f3e8f8d2d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60564027"
---
# <a name="mount-an-azure-file-share-in-azure-container-instances"></a>Подключение общего файлового ресурса Azure с помощью службы "Экземпляры контейнеров Azure"

По умолчанию в Экземплярах контейнеров Azure не отслеживается состояние. Если происходит сбой в контейнере или он завершает работу, все сведения о состоянии будут утеряны. Чтобы сохранить состояние после истечения времени существования контейнера, необходимо подключить том из внешнего хранилища. В этой статье показано, как подключить файловый ресурс Azure, созданный с помощью [файлов Azure](../storage/files/storage-files-introduction.md), для использования с Экземплярами контейнеров Azure. Служба файлов Azure предоставляет полностью управляемые общие файловые ресурсы в облаке, доступ к которым можно получить с помощью стандартного отраслевого протокола SMB. Использование файлового ресурса Azure с Экземплярами контейнеров Azure предоставляет функции обмена файлами, похожие на использование файлового ресурса Azure совместно с виртуальными машинами Azure.

> [!NOTE]
> Подключение общего ресурса службы файлов Azure сейчас поддерживается только для контейнеров Linux. Мы работаем над тем, чтобы обеспечить все функции для контейнеров Windows, но для текущей платформы есть отличия в [квотах и доступности регионов для службы "Экземпляры контейнеров Azure"](container-instances-quotas.md).

## <a name="create-an-azure-file-share"></a>создать файловый ресурс Azure;

Перед использованием файлового ресурса Azure с Экземплярами контейнеров Azure необходимо создать его. Выполните приведенный ниже скрипт, чтобы создать учетную запись хранения для размещения файлового ресурса и общего ресурса. Имя учетной записи хранения должно быть глобально уникальным, поэтому скрипт добавляет случайное значение к базовой строке.

```azurecli-interactive
# Change these four parameters as needed
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --location $ACI_PERS_LOCATION \
    --sku Standard_LRS

# Create the file share
az storage share create --name $ACI_PERS_SHARE_NAME --account-name $ACI_PERS_STORAGE_ACCOUNT_NAME
```

## <a name="get-storage-credentials"></a>Получение учетных данных хранилища

Для подключения файлового ресурса Azure в качестве тома в Экземплярах контейнеров Azure требуется три значения: имя учетной записи хранения, имя общего ресурса и ключ доступа к хранилищу.

Если вы использовали сценарий выше, имя учетной записи хранения было сохранено в переменной ACI_PERS_STORAGE_ACCOUNT_NAME. Для отображения имени учетной записи введите:

```console
echo $ACI_PERS_STORAGE_ACCOUNT_NAME
```

Имя общего ресурса уже известно (определено как *acishare* в сценарии выше), так что остается только ключ учетной записи хранения, который можно найти с помощью следующей команды:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $ACI_PERS_STORAGE_ACCOUNT_NAME --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a>Развертывание контейнера и подключение тома

Чтобы подключить файловый ресурс Azure в качестве тома в контейнере, укажите точку подключения для общего ресурса и тома при создании контейнера с помощью команды [az container create][az-container-create]. Если вы выполнили предыдущие инструкции, можно подключить созданный ранее общий ресурс с помощью следующей команды для создания контейнера:

```azurecli-interactive
az container create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name hellofiles \
    --image mcr.microsoft.com/azuredocs/aci-hellofiles \
    --dns-name-label aci-demo \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

Значение `--dns-name-label` должно быть уникальным в пределах региона Azure, в котором создается экземпляр контейнера. Если при выполнении команды появится сообщение об ошибке **Метка DNS-имени**, обновите значение в предыдущей команде.

## <a name="manage-files-in-mounted-volume"></a>Управление файлами в подключенном томе

После запуска контейнера, можно использовать простое веб-приложение, развернутое с помощью Microsoft [aci-hellofiles] [ aci-hellofiles] изображение, чтобы создать небольшие текстовые файлы в файловом ресурсе Azure по указанному вами пути подключения. Получите полное доменное имя (FQDN) веб-приложения с помощью команды [az container show][az-container-show].

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles --query ipAddress.fqdn
```

С помощью [портала Azure][portal] или такого средства, как [обозреватель службы хранилища Microsoft Azure][storage-explorer], можно извлечь и проверить файл, записанный в файловый ресурс.

## <a name="mount-multiple-volumes"></a>Подключение нескольких томов

Чтобы подключить несколько томов в экземпляре контейнера, необходимо выполнить развертывание с помощью [шаблона Azure Resource Manager](/azure/templates/microsoft.containerinstance/containergroups) или yaml-файла.

Используйте шаблон, укажите сведения о файловом ресурсе и определите тома, заполнив массив `volumes` в разделе `properties` шаблона. Например, если вы создали два файловых ресурса Azure с именами *share1* и *share2* в учетной записи хранения *myStorageAccount*, массив `volumes` будет аналогичен приведенному ниже.

```JSON
"volumes": [{
  "name": "myvolume1",
  "azureFile": {
    "shareName": "share1",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
},
{
  "name": "myvolume2",
  "azureFile": {
    "shareName": "share2",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
}]
```

Затем для каждого контейнера в группе контейнеров, в которой нужно подключить тома, заполните массив `volumeMounts` в разделе `properties` определения контейнера. В результате этого, подключатся, например, тома *myvolume1* и *myvolume2*, определенные ранее.

```JSON
"volumeMounts": [{
  "name": "myvolume1",
  "mountPath": "/mnt/share1/"
},
{
  "name": "myvolume2",
  "mountPath": "/mnt/share2/"
}]
```

Пример развертывания экземпляра контейнера с помощью шаблона Azure Resource Manager см. в статье [Развертывание группы контейнеров](container-instances-multi-container-group.md). Пример с использованием YAML-файла см. в разделе [Развертывание нескольких контейнеров с использованием YAML-файла](container-instances-multi-container-yaml.md)

## <a name="next-steps"></a>Дальнейшие действия

Сведения о подключении других типов томов в службе "Экземпляры контейнеров Azure" см. в следующих статьях:

* [Mount an emptyDir volume in Azure Container Instances](container-instances-volume-emptydir.md) (Подключение тома emptyDir в службе "Экземпляры контейнеров Azure")
* [Подключение тома gitRepo в службе "Экземпляры контейнеров Azure"](container-instances-volume-gitrepo.md)
* [Подключение тома secret в службе "Экземпляры контейнеров Azure"](container-instances-volume-secret.md)

<!-- LINKS - External -->
[aci-hellofiles]: https://hub.docker.com/_/microsoft-azuredocs-aci-hellofiles 
[portal]: https://portal.azure.com
[storage-explorer]: https://storageexplorer.com

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show