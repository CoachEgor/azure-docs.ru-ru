---
title: Запуск Virtual Kubelet в кластере службы Azure Kubernetes
description: Узнайте, как использовать Virtual Kubelet со службой Azure Kubernetes (AKS) для запуска контейнеров Linux и Windows в службе "Экземпляры контейнеров Azure".
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 08/14/2018
ms.author: iainfou
ms.openlocfilehash: f7a0269ff22987648d134cb7f4fba8e28e29fd8b
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956293"
---
# <a name="use-virtual-kubelet-with-azure-kubernetes-service-aks"></a>Использование Virtual Kubelet со службой Azure Kubernetes (AKS)

Экземпляры контейнеров Azure предоставляют размещенную среду для запуска контейнеров в Azure. При использовании экземпляров контейнеров Azure вам не требуется управлять базовой вычислительной инфраструктурой, это делает Azure. При запуске контейнеров в Экземплярах контейнеров Azure взимается плата за каждый второй запущенный контейнер.

При использовании поставщика Virtual Kubelet для Экземпляров контейнеров Azure вы можете назначить контейнеры Linux и Windows на экземпляр контейнера, как если бы это был стандартный узел Kubernetes. Эта конфигурация позволяет воспользоваться преимуществами Kubernetes и возможностью управления стоимостью и затратами экземпляров контейнеров.

> [!NOTE]
> Теперь в AKS есть встроенная поддержка планирования контейнеров в ACI (*виртуальные узлы*). Сейчас эти виртуальные узлы поддерживают экземпляры контейнеров Linux. Если вам нужно запланировать экземпляры контейнеров Windows, вы можете продолжить работу с Virtual Kubelet. В противном случае следует использовать виртуальные узлы и не выполнять вручную инструкции Virtual Kubelet, указанные в данной статье. Вы можете начать работу с виртуальными узлами с помощью [Azure CLI][virtual-nodes-cli] или [портала Azure][virtual-nodes-portal].
>
> Virtual Kubelet представляет собой экспериментальный открытый проект и должен использоваться таким образом. Чтобы внести свой вклад в проект, сообщить о проблемах и получить дополнительную информацию о Virtual Kubelet, зайдите на [страницу проекта Virtual Kubelet на GitHub][vk-github].

## <a name="before-you-begin"></a>Перед началом работы

Для выполнения этих инструкций у вас должен быть кластер AKS. Если вам необходим кластер AKS, обратитесь к [руководству по началу работы со службой Azure Kubernetes][aks-quick-start].

Вам также потребуется Azure CLI **2.0.33** или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0](/cli/azure/install-azure-cli).

Для установки Virtual Kubelet также необходим набор инструментов [Helm](https://docs.helm.sh/using_helm/#installing-helm).

### <a name="register-container-instances-feature-provider"></a>Зарегистрировать поставщик функций экземпляры контейнеров

Если вы не использовали ранее службы экземпляра контейнера Azure (ACI), необходимо зарегистрируйте поставщик служб с вашей подпиской. Можно проверить состояние регистрации поставщика ACI, с помощью команды [az provider list] [az provider list], как показано в следующем примере:

```azurecli-interactive
az provider list --query "[?contains(namespace,'Microsoft.ContainerInstance')]" -o table
```

Поставщик *Microsoft.ContainerInstance* должен иметь состояние *Registered*, как показано в следующем примере выходных данных.

```
Namespace                    RegistrationState
---------------------------  -------------------
Microsoft.ContainerInstance  Registered
```

Если поставщик отображается в том, что *NotRegistered*, зарегистрируйте поставщик с помощью [az зарегистрировать поставщик] [az provider register], как показано в следующем примере:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerInstance
```

### <a name="for-rbac-enabled-clusters"></a>Кластеры с поддержкой RBAC

Если используется кластер AKS с поддержкой RBAC, необходимо создать учетную запись службы и привязку роли для использования с Tiller. Дополнительные сведения см. в статье об [управлении доступом на основе ролей в Helm][helm-rbac]. Чтобы создать учетную запись службы и привязку роли, создайте файл с именем *rbac-virtual-kubelet.yaml* и вставьте следующее определение.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Примените учетную запись службы и привязку с помощью команды [kubectl apply][kubectl-apply] и укажите файл *rbac-virtual-kubelet.yaml*, как показано в следующем примере.

```
$ kubectl apply -f rbac-virtual-kubelet.yaml

clusterrolebinding.rbac.authorization.k8s.io/tiller created
```

Настройка Helm для использования учетной записи службы Tiller.

```console
helm init --service-account tiller
```

Теперь можно продолжить установку Virtual Kubelet в кластере AKS.

## <a name="installation"></a>Установка

Для установки Virtual Kubelet выполните команду [az aks install-connector][aks-install-connector]. В следующем примере разворачиваются соединители как для Linux, так и для Windows.

```azurecli-interactive
az aks install-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet --os-type Both
```

В команде `aks install-connector` могут использоваться следующие аргументы.

| Аргумент: | Описание | Обязательно для заполнения |
|---|---|:---:|
| `--connector-name` | Имя соединителя ACI.| Да |
| `--name` `-n` | Имя управляемого кластера. | Да |
| `--resource-group` `-g` | Имя группы ресурсов. | Да |
| `--os-type` | Тип операционной системы экземпляров контейнера. Допустимые значения: Оба, Linux, Windows. Значение по умолчанию: Linux. | Нет |
| `--aci-resource-group` | Группа ресурсов, в которой будут созданы группы контейнеров ACI. | Нет |
| `--location` `-l` | Расположение для создания групп контейнеров ACI. | Нет |
| `--service-principal` | Субъект-служба, используемый для проверки подлинности в API-интерфейсах Azure. | Нет |
| `--client-secret` | Секрет, связанный с субъектом-службой. | Нет |
| `--chart-url` | URL-адрес диаграммы Helm для установки соединителя ACI. | Нет |
| `--image-tag` | Тег образа контейнера Virtual Kubelet. | Нет |

## <a name="validate-virtual-kubelet"></a>Проверка Virtual Kubelet

Чтобы проверить установку Virtual Kubelet, получите список узлов Kubernetes, выполнив команду [kubectl get nodes][kubectl-get].

```
$ kubectl get nodes

NAME                                    STATUS    ROLES     AGE       VERSION
aks-nodepool1-23443254-0                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-1                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-2                Ready     agent     16d       v1.9.6
virtual-kubelet-virtual-kubelet-linux   Ready     agent     4m        v1.8.3
virtual-kubelet-virtual-kubelet-win     Ready     agent     4m        v1.8.3
```

## <a name="run-linux-container"></a>Запуск контейнера Linux

Создайте файл `virtual-kubelet-linux.yaml` и скопируйте в него следующий код YAML. Обратите внимание, что [nodeSelector][node-selector] и [toleration][toleration] используются для назначения контейнера на узел.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aci-helloworld
  template:
    metadata:
      labels:
        app: aci-helloworld
    spec:
      containers:
      - name: aci-helloworld
        image: microsoft/aci-helloworld
        ports:
        - containerPort: 80
      nodeSelector:
        beta.kubernetes.io/os: linux
        kubernetes.io/role: agent
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Equal
        value: azure
        effect: NoSchedule
```

Запустите приложение, выполнив команду [kubectl create][kubectl-create].

```console
kubectl create -f virtual-kubelet-linux.yaml
```

Чтобы вывести список pod для назначенного узла, выполните команду [kubectl get pods][kubectl-get] с аргументом `-o wide`. Обратите внимание, что pod `aci-helloworld` был назначен на узел `virtual-kubelet-virtual-kubelet-linux`.

```
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
aci-helloworld-2559879000-8vmjw     1/1       Running   0          39s       52.179.3.180   virtual-kubelet-virtual-kubelet-linux
```

## <a name="run-windows-container"></a>Запуск контейнера Windows

Создайте файл `virtual-kubelet-windows.yaml` и скопируйте в него следующий код YAML. Обратите внимание, что [nodeSelector][node-selector] и [toleration][toleration] используются для назначения контейнера на узел.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nanoserver-iis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nanoserver-iis
  template:
    metadata:
      labels:
        app: nanoserver-iis
    spec:
      containers:
      - name: nanoserver-iis
        image: microsoft/iis:nanoserver
        ports:
        - containerPort: 80
      nodeSelector:
        beta.kubernetes.io/os: windows
        kubernetes.io/role: agent
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Equal
        value: azure
        effect: NoSchedule
```

Запустите приложение, выполнив команду [kubectl create][kubectl-create].

```console
kubectl create -f virtual-kubelet-windows.yaml
```

Чтобы вывести список pod для назначенного узла, выполните команду [kubectl get pods][kubectl-get] с аргументом `-o wide`. Обратите внимание, что pod `nanoserver-iis` был назначен на узел `virtual-kubelet-virtual-kubelet-win`.

```
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
nanoserver-iis-868bc8d489-tq4st     1/1       Running   8         21m       138.91.121.91   virtual-kubelet-virtual-kubelet-win
```

## <a name="remove-virtual-kubelet"></a>Удаление Virtual Kubelet

Для удаления Virtual Kubelet выполните команду [az aks remove-connector][aks-remove-connector]. Замените значения аргументов на имя соединителя, кластера AKS и группы ресурсов кластера AKS.

```azurecli-interactive
az aks remove-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet
```

> [!NOTE]
> Если возникают ошибки при удалении соединителей обоих ОС или вы хотите удалить только соединитель Windows или Linux, тип операционной системы можно указать вручную. Добавьте параметр `--os-type` к предыдущей команде `az aks remove-connector` и укажите ОС: `Windows` или `Linux`.

## <a name="next-steps"></a>Дальнейшие действия

Возможные причины этой проблемы с Virtual Kubelet см. в разделе [Known quirks and workarounds][vk-troubleshooting] (Известные особенности и обходные пути). Чтобы сообщить о проблемах с Virtual Kubelet, [откройте раздел репозитория GitHub "Проблемы"][vk-issues].

Дополнительные сведения о Virtual Kubelet см. на [странице проекта Virtual Kubelet в GitHub][vk-github].

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-remove-connector]: /cli/azure/aks#az-aks-remove-connector
[az-container-list]: /cli/azure/aks#az-aks-list
[aks-install-connector]: /cli/azure/aks#az-aks-install-connector
[virtual-nodes-cli]: virtual-nodes-cli.md
[virtual-nodes-portal]: virtual-nodes-portal.md

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[vk-github]: https://github.com/virtual-kubelet/virtual-kubelet
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[vk-troubleshooting]: https://github.com/virtual-kubelet/virtual-kubelet#known-quirks-and-workarounds
[vk-issues]: https://github.com/virtual-kubelet/virtual-kubelet/issues
