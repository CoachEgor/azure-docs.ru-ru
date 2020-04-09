---
title: Руководство по Kubernetes в Azure. Развертывание кластера
description: В этом руководстве по Службе Azure Kubernetes (AKS) вы создаете кластер AKS и используете kubectl для подключения к главному узлу Kubernetes.
services: container-service
ms.topic: tutorial
ms.date: 02/25/2020
ms.custom: mvc
ms.openlocfilehash: 72d7d3b8a4dc2831f397326d54560358c19b9b92
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/02/2020
ms.locfileid: "80616809"
---
# <a name="tutorial-deploy-an-azure-kubernetes-service-aks-cluster"></a>Руководство по Развертывание кластера службы Azure Kubernetes (AKS)

Kubernetes предоставляет распределенную платформу для контейнерных приложений. AKS позволяет быстро создать готовый производственный кластер Kubernetes. В этом руководстве (третья часть из восьми) кластер Kubernetes развертывается в AKS. Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Развертывание кластера Kubernetes AKS, который может проходить проверку подлинности в Реестре контейнеров Azure
> * Установка интерфейса командной строки Kubernetes (kubectl).
> * Настройка kubectl для подключения к кластеру AKS.

В дополнительных руководствах приложение Azure для голосования развертывается в кластере, масштабируется и обновляется.

## <a name="before-you-begin"></a>Перед началом

В предыдущих руководствах образ контейнера был создан и передан в экземпляр реестра контейнеров Azure. Если вы не выполнили эти действия, вы можете начать с раздела руководства 1 [Создание образов контейнеров][aks-tutorial-prepare-app].

Для выполнения задач из этого руководства требуется Azure CLI 2.0.53 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0][azure-cli-install].

## <a name="create-a-kubernetes-cluster"></a>Создание кластера Kubernetes

Кластеры AKS могут использовать управление доступом на основе ролей (RBAC) Kubernetes. Эти элементы управления позволяют определять доступ к ресурсам на основе ролей, назначенных пользователям. Разрешения объединяются, если пользователю назначено несколько ролей. Разрешения могут охватывать одно пространство имен или весь кластер. По умолчанию Azure CLI автоматически включает RBAC при создании кластера AKS.

Создайте кластер AKS с помощью команды [az aks create][]. В следующем примере в группе ресурсов *myResourceGroup* создается кластер с именем *myAKSCluster*. Эта группа ресурсов была создана в [предыдущем руководстве][aks-tutorial-prepare-acr]. Чтобы разрешить кластеру AKS взаимодействовать с другими ресурсами Azure, автоматически создается субъект-служба Azure Active Directory (если вы не указывали ее). Этому субъекту-службе [предоставляется право на извлечение образов][container-registry-integration] из экземпляра Реестра контейнеров Azure (ACR), созданного при работе с предыдущим руководством.

```azurecli
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 2 \
    --generate-ssh-keys \
    --attach-acr <acrName>
```

Вы также можете вручную настроить субъект-службу на извлечение образов из реестра контейнеров. Дополнительные сведения см. в статьях [Аутентификация в реестре контейнеров Azure с помощью субъектов-служб](../container-registry/container-registry-auth-service-principal.md) и [Проверка подлинности в Kubernetes с использованием секрета для извлечения](../container-registry/container-registry-auth-kubernetes.md).

Через несколько минут развертывание завершится, и отобразятся сведения о развертывании AKS в формате JSON.

> [!NOTE]
> Чтобы обеспечить надежную работу кластера, необходимо запустить не менее двух узлов.

## <a name="install-the-kubernetes-cli"></a>Установка интерфейса командной строки Kubernetes

Для подключения к кластеру Kubernetes с локального компьютера используйте средство [kubectl][kubectl] (клиент командной строки Kubernetes).

Если вы используете Azure Cloud Shell, `kubectl` уже установлен. Его также можно установить локально с помощью команды [az aks install-cli][]:

```azurecli
az aks install-cli
```

## <a name="connect-to-cluster-using-kubectl"></a>Подключение к кластеру с помощью kubectl

Чтобы настроить `kubectl` на подключение к кластеру Kubernetes, выполните команду [az aks get-credentials][]. В следующем примере возвращаются учетные данные для кластера AKS с именем *myAKSCluster* в группе ресурсов *myResourceGroup*:

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Чтобы проверить подключение к кластеру, используйте команду [kubectl get nodes][kubectl-get] для получения списка узлов кластера.

```
$ kubectl get nodes

NAME                       STATUS   ROLES   AGE   VERSION
aks-nodepool1-12345678-0   Ready    agent   32m   v1.14.8
```

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы развернули кластер Kubernetes в AKS и настроили `kubectl` для подключения к нему. Вы ознакомились с выполнением следующих задач:

> [!div class="checklist"]
> * Развертывание кластера Kubernetes AKS, который может проходить проверку подлинности в Реестре контейнеров Azure
> * Установка интерфейса командной строки Kubernetes (kubectl).
> * Настройка kubectl для подключения к кластеру AKS.

Перейдите к следующему руководству, чтобы узнать, как развертывать приложения в кластер.

> [!div class="nextstepaction"]
> [Запуск приложений в Kubernetes][aks-tutorial-deploy-app]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-acr]: ./tutorial-kubernetes-prepare-acr.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[az ad sp create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az acr show]: /cli/azure/acr#az-acr-show
[az role assignment create]: /cli/azure/role/assignment#az-role-assignment-create
[az aks create]: /cli/azure/aks#az-aks-create
[az aks install-cli]: /cli/azure/aks#az-aks-install-cli
[az aks get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-cli-install]: /cli/azure/install-azure-cli
[container-registry-integration]: ./cluster-container-registry-integration.md
