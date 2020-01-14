---
title: Учебник. Подготовка реестра контейнеров для развертывания образа
description: Руководство по службе "Экземпляры контейнеров Azure" — часть 2 из 3. Подготовка реестра контейнеров Azure и отправка образа
ms.topic: tutorial
ms.date: 12/18/2019
ms.custom: seodec18, mvc
ms.openlocfilehash: 131ea39b382735423a1edff72774313c4096ea2b
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/31/2019
ms.locfileid: "75552430"
---
# <a name="tutorial-create-an-azure-container-registry-and-push-a-container-image"></a>Руководство. Создание реестра контейнеров Azure и отправка образа контейнера

Это вторая часть руководства, состоящего из трех частей. [Первая часть](container-instances-tutorial-prepare-app.md) руководства по созданию образа контейнера Docker для веб-приложения Node.js. В этом руководстве мы поместим образ в реестр контейнеров Azure. Если вы еще не создали образ контейнера, вернитесь к первой части этой серии — [руководству по созданию образа контейнера](container-instances-tutorial-prepare-app.md).

Реестр контейнеров Azure является частным реестром Docker в Azure. В этом учебнике, второй части серии, вы:

> [!div class="checklist"]
> * создадите экземпляр Реестра контейнеров Azure с помощью Azure CLI;
> * добавите образ контейнера для помещения в реестр контейнеров Azure;
> * отправите образ в реестр.

В следующей статье (последнем руководстве в серии) вы развернете контейнер из частного реестра в службе "Экземпляры контейнеров Azure".

## <a name="before-you-begin"></a>Перед началом

[!INCLUDE [container-instances-tutorial-prerequisites](../../includes/container-instances-tutorial-prerequisites.md)]

## <a name="create-azure-container-registry"></a>Создание реестра контейнеров Azure

Перед созданием реестра контейнеров необходимо создать *группу ресурсов*, куда он будет развернут. Группа ресурсов — это логическая коллекция, в которой выполняется развертывание и администрирование всех ресурсов Azure.

Создайте группу ресурсов с помощью команды [az group create][az-group-create]. В следующем примере создается группа ресурсов с именем *myResourceGroup* в регионе *eastus*.

```azurecli
az group create --name myResourceGroup --location eastus
```

После создания группы ресурсов создайте реестр контейнеров Azure с помощью команды [az acr create][az-acr-create]. Имя реестра контейнеров должно быть уникальным в пределах Azure и содержать от 5 до 50 буквенно-цифровых знаков. Замените `<acrName>` уникальным именем реестра.

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Ниже приведен пример выходных данных для нового реестра контейнеров Azure с именем *mycontainerregistry082* (показаны в сокращенном виде).

```console
$ az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic
...
{
  "creationDate": "2018-03-16T21:54:47.297875+00:00",
  "id": "/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/mycontainerregistry082",
  "location": "eastus",
  "loginServer": "mycontainerregistry082.azurecr.io",
  "name": "mycontainerregistry082",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

В остальной части руководства `<acrName>` используется как заполнитель для имени реестра контейнеров, выбранного на этом шаге.

## <a name="log-in-to-container-registry"></a>Вход в реестр контейнеров

Войдите в экземпляр реестра контейнеров Azure, прежде чем передавать в него образы. Используйте команду [az acr login][az-acr-login], чтобы выполнить операцию. Укажите уникальное имя реестра контейнеров, выбранное при его создании.

```azurecli
az acr login --name <acrName>
```

По завершении команда возвращает `Login Succeeded`.

```console
$ az acr login --name mycontainerregistry082
Login Succeeded
```

## <a name="tag-container-image"></a>Добавление тега к образу контейнера

Чтобы отправить образ контейнера в частный реестр, например в реестр контейнеров Azure, сначала необходимо добавить для образа тег с полным именем сервера входа в реестр.

Сначала получите полное имя сервера входа в реестр для реестра контейнеров Azure. Выполните следующую команду [az acr show][az-acr-show] и замените `<acrName>` именем только что созданного реестра:

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Например, если имя реестра — *mycontainerregistry082*:

```console
$ az acr show --name mycontainerregistry082 --query loginServer --output table
Result
------------------------
mycontainerregistry082.azurecr.io
```

Теперь с помощью команды [docker images][docker-images] отобразите список локальных образов:

```bash
docker images
```

Вместе с другими образами, присутствующими на компьютере, появится образ *aci-tutorial-app*, созданный вами при работе с [предыдущим руководством](container-instances-tutorial-prepare-app.md).

```console
$ docker images
REPOSITORY          TAG       IMAGE ID        CREATED           SIZE
aci-tutorial-app    latest    5c745774dfa9    39 minutes ago    68.1 MB
```

Добавьте к образу *aci-tutorial-app* тег сервера входа реестра контейнеров. Также добавьте тег `:v1` в конец имени образа, чтобы указать номер версии образа. Замените `<acrLoginServer>` полученным ранее результатом выполненной команды [az acr show][az-acr-show].

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Чтобы проверить операцию присвоения тегов, снова выполните команду `docker images`.

```console
$ docker images
REPOSITORY                                            TAG       IMAGE ID        CREATED           SIZE
aci-tutorial-app                                      latest    5c745774dfa9    39 minutes ago    68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app    v1        5c745774dfa9    7 minutes ago     68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a>Передача образа в реестр контейнеров Azure

Теперь, когда вы отметили образ *aci-tutorial-app* с полным именем сервера входа в частный реестр, его можно отправить в реестр с помощью команды [docker push][docker-push]. Замените `<acrLoginServer>` полным именем сервера входа, полученным на предыдущем шаге.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

Операция `push` займет от нескольких секунд до нескольких минут, в зависимости от подключения к Интернету. Ее выходные данные будут иметь следующий вид.

```console
$ docker push mycontainerregistry082.azurecr.io/aci-tutorial-app:v1
The push refers to a repository [mycontainerregistry082.azurecr.io/aci-tutorial-app]
3db9cac20d49: Pushed
13f653351004: Pushed
4cd158165f4d: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:ed67fff971da47175856505585dcd92d1270c3b37543e8afd46014d328f05715 size: 1576
```

## <a name="list-images-in-azure-container-registry"></a>Получение списка образов в реестре контейнеров Azure

Чтобы проверить, находится ли отправленный образ в реестре контейнеров Azure, выведите список образов в реестре с помощью команды [az acr repository list][az-acr-repository-list]. Замените `<acrName>` именем реестра контейнеров.

```azurecli
az acr repository list --name <acrName> --output table
```

Пример:

```console
$ az acr repository list --name mycontainerregistry082 --output table
Result
----------------
aci-tutorial-app
```

Чтобы просмотреть *теги* для конкретного образа, воспользуйтесь командой [az acr repository show-tags][az-acr-repository-show-tags].

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

Выходные данные должны иметь следующий вид.

```console
$ az acr repository show-tags --name mycontainerregistry082 --repository aci-tutorial-app --output table
Result
--------
v1
```

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы подготовили реестр контейнеров Azure к использованию со службой "Экземпляры контейнеров Azure", а также передали образ контейнера в реестр. Были выполнены следующие действия:

> [!div class="checklist"]
> * Создание экземпляра Реестра контейнеров Azure с помощью Azure CLI
> * добавление тегов к образу контейнера для помещения в реестр контейнеров Azure;
> * отправка образа в реестр контейнеров Azure.

Перейдите к следующему руководству, чтобы узнать, как развертывать контейнер в Azure с помощью службы "Экземпляры контейнеров Azure".

> [!div class="nextstepaction"]
> [Развертывание контейнера в службе "Экземпляры контейнеров Azure"](container-instances-tutorial-deploy-app.md)

<!-- LINKS - External -->
[docker-build]: https://docs.docker.com/engine/reference/commandline/build/
[docker-get-started]: https://docs.docker.com/get-started/
[docker-hub-nodeimage]: https://store.docker.com/images/node
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/
[nodejs]: https://nodejs.org

<!-- LINKS - Internal -->
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-repository-list]: /cli/azure/acr/repository
[az-acr-repository-show-tags]: /cli/azure/acr/repository#az-acr-repository-show-tags
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-group-create]: /cli/azure/group#az-group-create
[azure-cli-install]: /cli/azure/install-azure-cli
