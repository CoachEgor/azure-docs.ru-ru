---
title: Создание новой версии образа из существующей версии образа с помощью Azure Image Builder (Предварительная версия)
description: Создание новой версии образа виртуальной машины из существующей версии образа с помощью Azure Image Builder.
author: cynthn
ms.author: cynthn
ms.date: 05/05/2020
ms.topic: how-to
ms.service: virtual-machines-windows
ms.openlocfilehash: ee3e2a224789c899dcfabdbee56b949ea86f0a08
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2020
ms.locfileid: "82872273"
---
# <a name="preview-create-a-new-vm-image-version-from-an-existing-image-version-using-azure-image-builder"></a>Предварительная версия. Создание новой версии образа виртуальной машины из существующей версии образа с помощью Azure Image Builder

В этой статье показано, как взять существующую версию образа в [коллекции общих образов](shared-image-galleries.md), обновить ее и опубликовать в коллекции как новую версию образа.

Мы будем использовать шаблон Sample. JSON для настройки образа. JSON-файл, который мы используем: [хеллоимажетемплатефорсигфромвинсиг. JSON](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/2_Creating_a_Custom_Win_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromWinSIG.json). 

> [!IMPORTANT]
> Azure Image Builder сейчас находится в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="register-the-features"></a>Регистрация компонентов
Чтобы использовать Azure Image Builder во время предварительной версии, необходимо зарегистрировать новую функцию.

```azurecli-interactive
az feature register --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview
```

Проверьте состояние регистрации компонента.

```azurecli-interactive
az feature show --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview | grep state
```

Проверьте регистрацию.

```azurecli-interactive
az provider show -n Microsoft.VirtualMachineImages | grep registrationState
az provider show -n Microsoft.KeyVault | grep registrationState
az provider show -n Microsoft.Compute | grep registrationState
az provider show -n Microsoft.Storage | grep registrationState
```

Если они не зарегистрированы, выполните следующую команду:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages
az provider register -n Microsoft.Compute
az provider register -n Microsoft.KeyVault
az provider register -n Microsoft.Storage
```


## <a name="set-variables-and-permissions"></a>Задание переменных и разрешений

Если вы использовали [Создание образа и распространяете его в общую коллекцию образов](image-builder-gallery.md) для создания коллекции общих образов, вы уже создали нужные вам переменные. Если нет, настройте некоторые переменные для использования в этом примере.

Для предварительной версии построитель изображений поддерживает создание пользовательских образов в той же группе ресурсов, что и исходный управляемый образ. Обновите имя группы ресурсов в этом примере, чтобы оно совпадало с группой ресурсов исходного управляемого образа.

```azurecli-interactive
# Resource group name - we are using ibsigRG in this example
sigResourceGroup=myIBWinRG
# Datacenter location - we are using West US 2 in this example
location=westus
# Additional region to replicate the image to - we are using East US in this example
additionalregion=eastus
# name of the shared image gallery - in this example we are using myGallery
sigName=my22stSIG
# name of the image definition to be created - in this example we are using myImageDef
imageDefName=winSvrimages
# image distribution metadata reference name
runOutputName=w2019SigRo
# User name and password for the VM
username="user name for the VM"
vmpassword="password for the VM"
```

Создайте переменную для идентификатора подписки. Это можно сделать с помощью `az account show | grep id`.

```azurecli-interactive
subscriptionID=<Subscription ID>
```

Получение версии образа, которую требуется обновить.

```azurecli-interactive
sigDefImgVersionId=$(az sig image-version list \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID --query [].'id' -o json | grep 0. | tr -d '"' | tr -d '[:space:]')
```

## <a name="create-a-user-assigned-identity-and-set-permissions-on-the-resource-group"></a>Создание назначенного пользователем удостоверения и задание разрешений для группы ресурсов
Так как вы установили удостоверение пользователя в предыдущем примере, вам нужно просто получить его идентификатор ресурса, а затем добавить его к шаблону.

```azurecli-interactive
#get identity used previously
imgBuilderId=$(az identity list -g $sigResourceGroup --query "[?contains(name, 'aibBuiUserId')].id" -o tsv)
```

Если у вас уже есть собственная коллекция общих образов и вы не следовали предыдущему примеру, вам нужно будет назначить разрешения для доступа к группе ресурсов для построителя изображений, чтобы она могла получить доступ к коллекции. Ознакомьтесь с примером в статье [Создание образа и распространение в коллекцию общих образов](image-builder-gallery.md) .


## <a name="modify-helloimage-example"></a>Пример изменения Хеллоимаже
Вы можете ознакомиться с примером, который мы будем использовать, открыв JSON файл здесь: [хеллоимажетемплатефорсигфромсиг. JSON](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/2_Creating_a_Custom_Linux_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromSIG.json) и [ссылку на шаблон построителя образов](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 


Скачайте пример JSON и настройте его с помощью переменных. 

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/8_Creating_a_Custom_Win_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromWinSIG.json -o helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<rgName>/$sigResourceGroup/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<imageDefName>/$imageDefName/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<sharedImageGalName>/$sigName/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s%<sigDefImgVersionId>%$sigDefImgVersionId%g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<region1>/$location/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<region2>/$additionalregion/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s%<imgBuilderId>%$imgBuilderId%g" helloImageTemplateforSIGfromWinSIG.json
```

## <a name="create-the-image"></a>Создание образа

Отправьте конфигурацию образа в службу "Построитель образов виртуальных машин".

```azurecli-interactive
az resource create \
    --resource-group $sigResourceGroup \
    --properties @helloImageTemplateforSIGfromWinSIG.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n imageTemplateforSIGfromWinSIG01
```

Запустите сборку образа.

```azurecli-interactive
az resource invoke-action \
     --resource-group $sigResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n imageTemplateforSIGfromWinSIG01 \
     --action Run 
```

Дождитесь завершения сборки и репликации образа, прежде чем переходить к следующему шагу.


## <a name="create-the-vm"></a>Создание виртуальной машины

```azurecli-interactive
az vm create \
  --resource-group $sigResourceGroup \
  --name aibImgWinVm002 \
  --admin-username $username \
  --admin-password $vmpassword \
  --image "/subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup/providers/Microsoft.Compute/galleries/$sigName/images/$imageDefName/versions/latest" \
  --location $location
```

## <a name="verify-the-customization"></a>Проверка настройки
Создайте удаленный рабочий стол подключение к виртуальной машине, используя имя пользователя и пароль, заданные при создании виртуальной машины. В виртуальной машине откройте командную строку и введите следующую команду:

```console
dir c:\
```

Теперь вы увидите два каталога:
- `buildActions`, созданного в первой версии образа.
- `buildActions2`Это было создано как часть обновления первой версии образа для создания второй версии образа.


## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о компонентах JSON, использованных в этой статье, см. в разделе [Справочник по шаблонам для Image Builder](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
