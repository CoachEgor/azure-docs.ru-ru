---
title: 'Руководство по Службе контейнеров Azure: масштабирование приложения (не рекомендуется)'
description: 'Руководство по Службе контейнеров Azure: масштабирование приложения'
author: dlepow
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 2ea8a5428c1fabdfda4f2298c0559792537df481
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "78273983"
---
# <a name="deprecated-scale-kubernetes-pods-and-kubernetes-infrastructure"></a>Масштабирование pod и инфраструктуры Kubernetes (не рекомендуется)

> [!TIP]
> Обновленная версия этого руководства, в котором используется служба Azure Kubernetes — [Руководство. Масштабирование приложений в службе Azure Kubernetes (AKS)](../../aks/tutorial-kubernetes-scale.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

Если вы выполнили инструкции в руководствах, то у вас имеется работающий кластер Kubernetes в Службе контейнеров Azure и вы развернули в нем приложение Vote Azure. 

В этом руководстве (часть 5 из 7) описывается горизонтальное увеличение масштаба pod, содержащихся в приложении, и их автомасштабирование. Вы также узнаете, как масштабировать количество узлов агентов виртуальной машины Azure, чтобы менять емкость кластера для размещения рабочих нагрузок. Вам предстоят следующие задачи:

> [!div class="checklist"]
> * масштабирование pod Kubernetes вручную;
> * настройка автомасштабирования pod, в которых выполняется интерфейсная часть приложения;
> * масштабирование узлов агентов Kubernetes в Azure.

В последующих руководствах описывается, как обновить приложение Azure для голосования, а также настроить Log Analytics для мониторинга кластера Kubernetes.

## <a name="before-you-begin"></a>Перед началом

В предыдущих руководствах приложение было упаковано в образ контейнера, этот образ был передан в реестр контейнеров Azure и был создан кластер Kubernetes. Затем приложение было запущено в кластере Kubernetes. 

Если вы не выполнили эти действия, то можете ознакомиться со статьей [Создание образов контейнеров для использования со службой контейнеров Azure](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="manually-scale-pods"></a>Масштабирование pod вручную

К настоящему моменту было развернуто по отдельной реплике внешнего приложения Vote Azure и экземпляра Redis. Чтобы проверить это, выполните команду [kubectl get](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get).

Перейдите по адресу [https://shell.azure.com](https://shell.azure.com), чтобы открыть Cloud Shell в браузере.

```console
kubectl get pods
```

Выходные данные:

```output
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Вручную измените число pod в развертывании `azure-vote-front`, выполнив команду [kubectl scale](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale). В этом примере их число увеличивается до 5.

```console
kubectl scale --replicas=5 deployment/azure-vote-front
```

Выполните команду [kubectl get pods](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get), чтобы убедиться, что Kubernetes создает новые pod. Дополнительные pod будут запущены примерно через минуту.

```console
kubectl get pods
```

Выходные данные:

```output
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Автомасштабирование pod

Kubernetes поддерживает [горизонтальное автомасштабирование pod](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) для изменения числа pod в развертывании в зависимости от использования ЦП или других выбранных метрик. 

Чтобы использовать инструмент автомасштабирования, для ваших pod необходимо определить запросы и лимиты ресурсов ЦП. В развертывании `azure-vote-front` каждый контейнер внешнего приложения запрашивает 0,25 ресурсов ЦП с лимитом в 0,5 ресурсов ЦП. Параметры имеют следующий вид.

```yaml
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

В следующем примере используется команда [kubectl autoscale](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale) для автомасштабирования числа модулей в развертывании `azure-vote-front`. Если использование ЦП превышает 50 %, то инструмент автомасштабирования увеличивает число pod максимум до 10.


```console
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Выполните следующую команду, чтобы просмотреть состояние инструмента автомасштабирования.

```console
kubectl get hpa
```

Выходные данные:

```output
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Через несколько минут минимальной нагрузки на приложение Vote Azure число реплик pod автоматически уменьшится до 3.

## <a name="scale-the-agents"></a>Масштабирование агентов

Если вы создали кластер Kubernetes с помощью команд по умолчанию в предыдущем руководстве, то у него три узла агентов. Если вы планируете увеличение или уменьшение рабочих нагрузок контейнеров в кластере, то можете соответствующим образом изменить число агентов вручную. Выполните команду [az acs scale](/cli/azure/acs#az-acs-scale), указав количество агентов с помощью параметра `--new-agent-count`.

В следующем примере в кластере Kubernetes *myK8sCluster* число узлов агентов увеличивается до 4. Для выполнения этой команды требуется несколько минут.

```azurecli-interactive
az acs scale --resource-group=myResourceGroup --name=myK8SCluster --new-agent-count 4
```

Выходные данные команды содержат число узлов агентов в значении `agentPoolProfiles:count`.

```output
{
  "agentPoolProfiles": [
    {
      "count": 4,
      "dnsPrefix": "myK8SCluster-myK8SCluster-e44f25-k8s-agents",
      "fqdn": "",
      "name": "agentpools",
      "vmSize": "Standard_D2_v2"
    }
  ],
...

```

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы использовали различные возможности масштабирования кластера Kubernetes. в частности такие задачи:

> [!div class="checklist"]
> * масштабирование pod Kubernetes вручную;
> * настройка автомасштабирования pod, в которых выполняется интерфейсная часть приложения;
> * масштабирование узлов агентов Kubernetes в Azure.

Перейдите к следующему руководству, чтобы узнать об обновлении приложения в Kubernetes.

> [!div class="nextstepaction"]
> [Обновление приложения в Kubernetes](./container-service-tutorial-kubernetes-app-update.md)

