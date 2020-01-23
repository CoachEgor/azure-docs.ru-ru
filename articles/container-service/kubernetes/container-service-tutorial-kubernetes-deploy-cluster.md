---
title: 'Руководство по Службе контейнеров Azure: развертывание кластера (не рекомендуется)'
description: 'Руководство по Службе контейнеров Azure: развертывание кластера'
author: iainfoulds
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 5cb21bff2834751843061910184499f37bde834e
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2020
ms.locfileid: "76275441"
---
# <a name="deprecated-deploy-a-kubernetes-cluster-in-azure-container-service"></a>Развертывание кластера Kubernetes в службе контейнеров Azure (не рекомендуется)

> [!TIP]
> Обновленная версия этого руководства, в котором используется служба Azure Kubernetes — [Руководство. Развертывание кластера службы Azure Kubernetes (AKS)](../../aks/tutorial-kubernetes-deploy-cluster.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

Kubernetes предоставляет распределенную платформу для контейнерных приложений. Служба контейнеров Azure упрощает и убыстряет подготовку кластера Kubernetes для использования в рабочей среде. В этом руководстве (часть 3 из 7) развертывается кластер Kubernetes службы контейнеров Azure. В частности, рассматриваются такие шаги:

> [!div class="checklist"]
> * развертывание кластера ACS Kubernetes;
> * установка интерфейса командной строки Kubernetes (kubectl);
> * настройка kubectl.

При помощи последующих руководств приложение Azure для голосования развертывается в кластере, масштабируется и обновляется, а Log Analytics настраивается для отслеживания кластера Kubernetes.

## <a name="before-you-begin"></a>Перед началом

В предыдущих руководствах образ контейнера был создан и передан в экземпляр реестра контейнеров Azure. Если вы не выполнили эти действия, вы можете ознакомиться со статьей [Создание образов контейнеров для использования со службой контейнеров Azure](./container-service-tutorial-kubernetes-prepare-app.md).

## <a name="create-kubernetes-cluster"></a>Создание кластера Kubernetes

Создание кластера Kubernetes в Службе контейнеров Azure с помощью команды [az acs create](/cli/azure/acs#az-acs-create). 

В следующем примере создается кластер `myK8sCluster` в группе ресурсов `myResourceGroup`. Эта группа ресурсов была создана в [предыдущем руководстве](./container-service-tutorial-kubernetes-prepare-acr.md).

```azurecli-interactive 
az acs create --orchestrator-type kubernetes --resource-group myResourceGroup --name myK8SCluster --generate-ssh-keys 
```

В некоторых случаях, например при использовании ограниченной пробной версии, доступ подписки Azure к ресурсам Azure ограничен. Если происходит сбой развертывания из-за ограничения доступных ядер, уменьшите количество агентов по умолчанию, добавив `--agent-count 1` в команду [az acs create](/cli/azure/acs#az-acs-create). 

Через несколько минут развертывание завершится и отобразятся сведения о развертывании ACS в формате JSON.

## <a name="install-the-kubectl-cli"></a>Установка интерфейса командной строки kubectl

Для подключения к кластеру Kubernetes с клиентского компьютера используйте [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), клиент командной строки Kubernetes. 

Если вы используете Azure CloudShell, клиент kubectl уже установлен. Для локальной установки можно использовать команду [az acs kubernetes install-cli](/cli/azure/acs/kubernetes).

Для работы в macOS или Linux может потребоваться запуск с помощью sudo. В Windows убедитесь, что оболочка запущена от имени администратора.

```azurecli-interactive 
az acs kubernetes install-cli 
```

В Windows установка по умолчанию выполняется в папку *c:\program files (x86)\kubectl.exe*. Может потребоваться добавить этот файл в путь Windows. 

## <a name="connect-with-kubectl"></a>Подключение с помощью kubectl

Чтобы настроить kubectl для подключения к кластеру Kubernetes, выполните команду [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes).

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group myResourceGroup --name myK8SCluster
```

Чтобы проверить подключение к кластеру, выполните команду [kubectl get nodes](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get).

```azurecli-interactive
kubectl get nodes
```

Выходные данные:

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

По завершении руководства вы получите кластер ACS Kubernetes, готовый к обработке рабочих нагрузок. В следующих руководствах в этот кластер будет развернуто многоконтейнерное приложение, которое затем будет масштабировано, обновлено и отслежено.

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве был развернут кластер Kubernetes Службы контейнеров Azure. Были выполнены следующие действия:

> [!div class="checklist"]
> * развертывание кластера ACS Kubernetes;
> * установка интерфейса командной строки Kubernetes (kubectl);
> * настройка kubectl.

Перейдите к следующему руководству, чтобы узнать о выполнении приложения в кластере.

> [!div class="nextstepaction"]
> [Запуск приложений в Kubernetes](./container-service-tutorial-kubernetes-deploy-application.md)
