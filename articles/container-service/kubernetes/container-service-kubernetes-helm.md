---
title: (УСТАРЕЛО) Развертывание контейнеров с помощью Helm в Azure Kubernetes
description: Использование средства упаковки Helm для развертывания контейнеров в кластере Kubernetes в Службе контейнеров Azure
author: sauryadas
ms.service: container-service
ms.topic: conceptual
ms.date: 04/10/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: a32c9fab3877a693d2df26571b9fae4aa7b4380c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "76271085"
---
# <a name="deprecated-use-helm-to-deploy-containers-on-a-kubernetes-cluster"></a>(УСТАРЕЛО) Использование Helm для развертывания контейнеров в кластере Kubernetes

> [!TIP]
> Обновленная версия этой статьи — [Установка приложения с помощью Helm в службе Azure Kubernetes](../../aks/kubernetes-helm.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

[Helm](https://github.com/kubernetes/helm/) — это средство упаковки с открытым кодом, которое помогает установить приложения Kubernetes и управлять их жизненным циклом. Аналогично диспетчерам пакетов Linux, таких как Apt-get и Yum, Helm используется для управления чартами Kubernetes, представляющими собой пакеты предварительно настроенных ресурсов Kubernetes. В этой статье показано, как работать с Helm в кластере Kubernetes, развернутом в Службе контейнеров Azure.

Helm состоит из двух компонентов: 
* **Helm CLI** — клиент, который выполняется на компьютере локально или в облаке.  

* **Tiller** — сервер, который выполняется в кластере Kubernetes и управляет жизненным циклом приложений Kubernetes. 
 
## <a name="prerequisites"></a>Предварительные требования

* [Создание кластера Kubernetes](container-service-kubernetes-walkthrough.md) в службе контейнеров Azure

* [Установка и настройка`kubectl`](../container-service-connect.md) на локальном компьютере

* [Установка Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) на локальном компьютере

## <a name="helm-basics"></a>Основы использования Helm 

Чтобы просмотреть сведения о кластере Kubernetes, где вы устанавливаете Tiller и развертываете приложения, введите следующую команду:

```bash
kubectl cluster-info 
```
![kubectl cluster-info](./media/container-service-kubernetes-helm/clusterinfo.png)
 
После установки Helm установите Tiller в кластере Kubernetes, введя следующую команду:

```bash
helm init --upgrade
```
После успешного завершения команды выводятся сведения следующего вида:

![Установка Tiller](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
Чтобы просмотреть все доступные чарты Helm в репозитории, введите следующую команду:

```bash 
helm search 
```

Выводятся сведения следующего вида:

![Поиск Helm](./media/container-service-kubernetes-helm/helm-search.png)
 
Чтобы обновить чарты для использования актуальных версий, введите:

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a>Развертывание чарта входящего контроллера Nginx 
 
Чтобы развернуть чарт входящего контроллера Nginx, введите одну команду:

```bash
helm install stable/nginx-ingress 
```
![Развертывание входящего контроллера](./media/container-service-kubernetes-helm/nginx-ingress.png)

Если ввести `kubectl get svc` для просмотра всех служб, запущенных на кластере, вы увидите, что IP-адрес назначен входящему контроллеру. (Пока идет назначение, отображается `<pending>`. Для завершения операции требуется несколько минут.) 

После назначения IP-адреса перейдите к значению внешнего IP-адреса, чтобы увидеть работу серверной части Nginx. 
 
![Входящий IP-адрес](./media/container-service-kubernetes-helm/ingress-ip-address.png)


Чтобы просмотреть список чартов, установленных в кластере, введите:

```bash
helm list 
```

Команду можно сократить до `helm ls`.
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a>Развертывание клиента и чарта MariaDB

Разверните чарт и клиент MariaDB для подключения к базе данных.

Чтобы развернуть чарт MariaDB, введите следующую команду:

```bash
helm install --name v1 stable/mariadb
```

здесь `--name` — тег, используемый для выпусков.

> [!TIP]
> Если развертывание завершается сбоем, запустите `helm repo update` и повторите попытку.
>
 
 
Для просмотра всех чартов, развернутых в кластере, введите:

```bash 
helm list
```
 
Для просмотра всех развертываний, запущенных в кластере, введите:

```bash
kubectl get deployments 
``` 
 
 
Наконец, чтобы запустить pod для доступа к клиенту, введите:

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
Чтобы подключиться к клиенту, введите следующую команду, заменив `v1-mariadb` на имя развертывания:

```bash
sudo mysql –h v1-mariadb
```
 
 
Теперь можно использовать стандартные команды SQL для создания баз данных, таблиц и т. д. Например, `Create DATABASE testdb1;` создает пустую базу данных. 
 
 
 
## <a name="next-steps"></a>Следующие шаги

* Дополнительные сведения об управлении чартами Kubernetes см. в [документации по Helm](https://github.com/kubernetes/helm/blob/master/docs/index.md). 

