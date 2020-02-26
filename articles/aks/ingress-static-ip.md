---
title: Создание контроллера входящего HTTP-трафика со статическим IP-адресом в Службе Azure Kubernetes (AKS)
description: Сведения об установке и настройке контроллера входящего трафика NGINX со статическим общедоступным IP-адресом в кластере Службы Azure Kubernetes (AKS).
services: container-service
ms.topic: article
ms.date: 05/24/2019
ms.openlocfilehash: a72312e2921b4721a4a5944cf62241b513da1e0a
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/25/2020
ms.locfileid: "77595524"
---
# <a name="create-an-ingress-controller-with-a-static-public-ip-address-in-azure-kubernetes-service-aks"></a>Создание контроллера входящего трафика со статическим общедоступным IP-адресом в Службе Azure Kubernetes (AKS)

Контроллер входящего трафика является программным компонентом, предоставляющим для служб Kubernetes обратный прокси-сервер, настраиваемую маршрутизацию трафика и обработку подключений TLS для последующей передачи. Ресурсы входящего трафика Kubernetes используются при настройке правил входящего трафика и маршрутов для отдельных служб Kubernetes. Применяя контроллер и правила входящего трафика, для маршрутизации трафика в несколько служб в кластере Kubernetes можно использовать один IP-адрес.

В этой статье показано, как развернуть контроллер входящего трафика [nginx][nginx-ingress] в кластере службы Kubernetes Azure (AKS). Контроллер входящего трафика настраивается со статическим общедоступным IP-адресом. Проект [диспетчера сертификатов][cert-manager] используется для автоматического создания и настройки [шифрования][lets-encrypt] сертификатов. Наконец, в кластере AKS запущено два приложения, каждое из которых доступно по одному IP-адресу.

Кроме того, вы можете сделать следующее:

- [Создание базового контроллера входящего трафика с внешним сетевым подключением][aks-ingress-basic]
- [Включение надстройки маршрутизации приложений HTTP][aks-http-app-routing]
- [Создание контроллера входящего трафика, использующего собственные сертификаты TLS][aks-ingress-own-tls]
- [Создание контроллера входящего трафика, использующего шифрование для автоматического создания сертификатов TLS с динамическим общедоступным IP-адресом][aks-ingress-tls]

## <a name="before-you-begin"></a>Перед началом

В этой статье предполагается, что у вас есть кластер AKS. Если вам нужен кластер AKS, ознакомьтесь с кратким руководством по AKS, [используя Azure CLI][aks-quickstart-cli] или [с помощью портал Azure][aks-quickstart-portal].

В этой статье для установки контроллера входящего трафика NGINX cert-manager и примера веб-приложения используется Helm. Поэтому необходимо инициализировать Helm в кластере AKS и использовать учетную запись службы для Tiller. Убедитесь, что используется последний выпуск Helm 3. Инструкции по обновлению см. в документации по [установке Helm][helm-install]. Дополнительные сведения о настройке и использовании Helm см. [в статье Установка приложений с помощью Helm в службе Kubernetes Azure (AKS)][use-helm].

В этой статье также предполагается, что вы используете Azure CLI версии 2.0.64 или более поздней. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Создание контроллера входящего трафика

По умолчанию контроллер входящего трафика NGINX создается с новым назначением общедоступного IP-адреса. Этот общедоступный IP-адрес является статическим только на время жизненного цикла контроллера входящего трафика и теряется, если контроллер удаляется и создается повторно. Общее требование конфигурации — предоставить контроллеру входящего трафика NGINX имеющийся статический общедоступный IP-адрес. Статический общедоступный IP-адрес останется, если контроллер входящего трафика будет удален. Такой подход позволяет согласованно использовать имеющиеся записи DNS и сетевые конфигурации на протяжении жизненного цикла приложений.

Если необходимо создать статический общедоступный IP-адрес, сначала получите имя группы ресурсов кластера AKS с помощью команды [AZ AKS-показывать][az-aks-show] :

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv
```

Затем создайте общедоступный IP-адрес со *статическим* методом выделения с помощью команды [AZ Network public-IP Create][az-network-public-ip-create] . В следующем примере создается общедоступный IP-адрес *myPublicIP* в группе ресурсов AKS, о которой говорилось на предыдущих шагах.

```azurecli-interactive
az network public-ip create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --sku Standard --allocation-method static --query publicIp.ipAddress -o tsv
```

Разверните диаграмму *nginx ingress* с помощью Helm. Добавьте параметр `--set controller.service.loadBalancerIP` и укажите собственный общедоступный IP-адрес, созданный на предыдущем шаге. Для обеспечения дополнительной избыточности развертываются две реплики контроллеров входящего трафика NGINX с использованием параметра `--set controller.replicaCount`. Чтобы максимально эффективно использовать реплики контроллера входящего трафика, убедитесь, что в кластере AKS используется несколько узлов.

Контроллер входящего трафика также необходимо запланировать на узле Linux. Узлы Windows Server (в настоящее время в предварительной версии в AKS) не должны запускать входной контроллер. Селектор узла указывается с помощью параметра `--set nodeSelector`, чтобы сообщить планировщику Kubernetes о необходимости запуска контроллера входящих данных NGINX на узле под управлением Linux.

> [!TIP]
> В следующем примере создается пространство имен Kubernetes для входящих ресурсов с именем входящие *-Basic*. При необходимости укажите пространство имен для своей среды. Если в кластере AKS не включен RBAC, добавьте `--set rbac.create=false` в команды Helm.

> [!TIP]
> Если вы хотите включить [Сохранение IP-адреса источника клиента][client-source-ip] для запросов к контейнерам в кластере, добавьте `--set controller.service.externalTrafficPolicy=Local` в команду Helm install. Исходный IP-адрес клиента хранится в заголовке запроса в разделе *X-forwardd-for*. При использовании контроллера входящего трафика с включенным сохранением IP-адресов источника клиента передача данных по протоколу SSL не будет работать.

```console
# Create a namespace for your ingress resources
kubectl create namespace ingress-basic

# Use Helm to deploy an NGINX ingress controller
helm install nginx-ingress stable/nginx-ingress \
    --namespace ingress-basic \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set controller.service.loadBalancerIP="40.121.63.72"
```

При создании службы распределения нагрузки Kubernetes входящего контроллера NGINX назначается статический IP-адрес, как показано в следующем примере выходных данных.

```
$ kubectl get service -l app=nginx-ingress --namespace ingress-basic

NAME                                        TYPE           CLUSTER-IP    EXTERNAL-IP    PORT(S)                      AGE
nginx-ingress-controller                    LoadBalancer   10.0.232.56   40.121.63.72   80:31978/TCP,443:32037/TCP   3m
nginx-ingress-default-backend               ClusterIP      10.0.95.248   <none>         80/TCP                       3m
```

Так как правила входящего трафика не созданы, при переходе к общедоступному IP-адресу по умолчанию будет отображаться страница контроллеров входящего трафика NGINX с ошибкой "404 — страница не найдена". В следующих шагах настраиваются правила входящего трафика.

## <a name="configure-a-dns-name"></a>Настройка DNS-имени

Чтобы сертификаты HTTPS работали правильно, настройте для IP-адреса контроллера входящего трафика полное доменное имя. Укажите в этом скрипте IP-адрес контроллера входящего трафика и уникальное имя, которое вы хотите использовать в качестве полного доменного имени.

```azurecli-interactive
#!/bin/bash

# Public IP address of your ingress controller
IP="40.121.63.72"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get the resource-id of the public ip
PUBLICIPID=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[id]" --output tsv)

# Update public ip address with DNS name
az network public-ip update --ids $PUBLICIPID --dns-name $DNSNAME
```

Теперь к контроллеру входящего трафика можно получить доступ по полному доменному имени.

## <a name="install-cert-manager"></a>Установка cert-manager

Контроллер входящего трафика NGINX поддерживает обработку подключений TLS для последующей передачи. Получить и настроить сертификаты для HTTPS можно несколькими способами. В этой статье демонстрируется использование [диспетчера сертификатов][cert-manager], который обеспечивает автоматическую возможность [шифрования][lets-encrypt] функций создания и управления сертификатами.

> [!NOTE]
> В этой статье используется среда для Let's Encrypt `staging`. В производственных развертываниях используйте `letsencrypt-prod` и `https://acme-v02.api.letsencrypt.org/directory` в определениях ресурсов и при установке диаграммы Helm.

Чтобы установить контроллер cert-manager в кластере с поддержкой RBAC, используйте следующую команду `helm install`:

```console
# Install the CustomResourceDefinition resources separately
kubectl apply --validate=false -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.12/deploy/manifests/00-crds.yaml

# Create the namespace for cert-manager
kubectl create namespace cert-manager

# Label the cert-manager namespace to disable resource validation
kubectl label namespace cert-manager cert-manager.io/disable-validation=true

# Add the Jetstack Helm repository
helm repo add jetstack https://charts.jetstack.io

# Update your local Helm chart repository cache
helm repo update

# Install the cert-manager Helm chart
helm install \
  cert-manager \
  --namespace cert-manager \
  --version v0.12.0 \
  jetstack/cert-manager
```

Дополнительные сведения о конфигурации диспетчера сертификатов см. в разделе [проект диспетчера сертификатов][cert-manager].

## <a name="create-a-ca-cluster-issuer"></a>Создание издателя кластера ЦС

Перед выпуском сертификатов диспетчеру сертификатов требуется [поставщик][cert-manager-issuer] или ресурс [клустериссуер][cert-manager-cluster-issuer] . Эти ресурсы Kubernetes имеют аналогичную функциональность, однако `Issuer` работает в отдельном пространстве имен, а `ClusterIssuer` — во всех. Дополнительные сведения см. в документации [поставщика CERT-Manager][cert-manager-issuer] .

Создайте издатель кластера, например `cluster-issuer.yaml`, используя приведенный ниже пример манифеста. Измените адрес электронной почты на действительный адрес вашей организации:

```yaml
apiVersion: cert-manager.io/v1alpha2
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
  namespace: ingress-basic
spec:
  acme:
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    email: user@contoso.com
    privateKeySecretRef:
      name: letsencrypt-staging
    solvers:
    - http01:
        ingress:
          class: nginx
```

Чтобы создать издатель, используйте команду `kubectl apply -f cluster-issuer.yaml`.

```
$ kubectl apply -f cluster-issuer.yaml

clusterissuer.cert-manager.io/letsencrypt-staging created
```

## <a name="run-demo-applications"></a>Запуск демонстрационных версий приложений

Контроллер входящего трафика и решение по управлению сертификатами настроены. Теперь запустите две демонстрационные версии приложения в своем кластере AKS. В этом примере для развертывания двух экземпляров простого приложения Hello World применяется Helm.

Чтобы установить примеры диаграмм Helm, добавьте репозиторий примеров Azure в свою среду Helm таким образом:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Создайте первую демонстрационную версию приложения из диаграммы Helm с помощью такой команды:

```console
helm install aks-helloworld azure-samples/aks-helloworld --namespace ingress-basic
```

Установите второй экземпляр демонстрационной версии приложения. Укажите новый заголовок для второго экземпляра, чтобы оба приложения визуально отличались. Также задайте уникальное имя службы:

```console
helm install aks-helloworld-2 azure-samples/aks-helloworld \
    --namespace ingress-basic \
    --set title="AKS Ingress Demo" \
    --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Создание маршрута для входящего трафика

Теперь оба приложения запущены на кластере Kubernetes, несмотря на то что они настроены с помощью службы типа `ClusterIP`. Таким образом приложения не доступны через Интернет. Чтобы сделать их доступными, создайте ресурс входящего трафика Kubernetes. Ресурс входящего трафика настраивает правила, по которым осуществляется маршрутизация трафика к одному из двух приложений.

В следующем примере трафик по адресу `https://demo-aks-ingress.eastus.cloudapp.azure.com/` направляется в службу `aks-helloworld`. Трафик по адресу `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` направляется в службу `ingress-demo`. Обновите *узлы* и *узел*, указав DNS-имя, которое вы создали на предыдущем шаге.

Создайте файл `hello-world-ingress.yaml` и скопируйте в него следующий пример кода YAML:

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt-staging
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  tls:
  - hosts:
    - demo-aks-ingress.eastus.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: demo-aks-ingress.eastus.cloudapp.azure.com
    http:
      paths:
      - backend:
          serviceName: aks-helloworld
          servicePort: 80
        path: /(.*)
      - backend:
          serviceName: ingress-demo
          servicePort: 80
        path: /hello-world-two(/|$)(.*)
```

Создайте ресурс входящего трафика, используя команду `kubectl apply -f hello-world-ingress.yaml --namespace ingress-basic`.

```
$ kubectl apply -f hello-world-ingress.yaml --namespace ingress-basic

ingress.extensions/hello-world-ingress created
```

## <a name="create-a-certificate-object"></a>Создание объекта сертификата

Далее нужно создать ресурс сертификата. Этот ресурс сертификата определяет необходимый сертификат X.509. Дополнительные сведения см. в разделе [Сертификаты-менеджеры сертификатов][cert-manager-certificates].

cert-manager скорее всего автоматически создал объект сертификата с помощью ingress-shim — оболочки совместимости, которая автоматически развертывается с помощью cert-manager, начиная с версии 0.2.2. Дополнительные сведения см. в [документации по входящим и оболочкам][ingress-shim].

Чтобы убедиться, что сертификат был успешно создан, используйте команду `kubectl describe certificate tls-secret --namespace ingress-basic`.

Если сертификат был выдан, отобразятся выходные данные, аналогичные этим.
```
Type    Reason          Age   From          Message
----    ------          ----  ----          -------
  Normal  CreateOrder     11m   cert-manager  Created new ACME order, attempting validation...
  Normal  DomainVerified  10m   cert-manager  Domain "demo-aks-ingress.eastus.cloudapp.azure.com" verified with "http-01" validation
  Normal  IssueCert       10m   cert-manager  Issuing certificate...
  Normal  CertObtained    10m   cert-manager  Obtained certificate from ACME server
  Normal  CertIssued      10m   cert-manager  Certificate issued successfully
```

Если вам нужно создать дополнительный ресурс сертификата, вы можете это сделать с помощью приведенного ниже примера манифеста. Обновите *dnsNames* и *домены*, указав для них DNS-имя, которое вы создали на предыдущем шаге. Если вы используете только внутренний контроллер входящего трафика, укажите внутреннее имя DNS для службы.

```yaml
apiVersion: cert-manager.io/v1alpha2
kind: Certificate
metadata:
  name: tls-secret
  namespace: ingress-basic
spec:
  secretName: tls-secret
  dnsNames:
  - demo-aks-ingress.eastus.cloudapp.azure.com
  acme:
    config:
    - http01:
        ingressClass: nginx
      domains:
      - demo-aks-ingress.eastus.cloudapp.azure.com
  issuerRef:
    name: letsencrypt-staging
    kind: ClusterIssuer
```

Чтобы создать ресурс сертификата, используйте команду `kubectl apply -f certificates.yaml`.

```
$ kubectl apply -f certificates.yaml

certificate.cert-manager.io/tls-secret created
```

## <a name="test-the-ingress-configuration"></a>Проверка конфигурации входящего трафика

Откройте в браузере адрес с полным доменным именем вашего контроллера входящего трафика Kubernetes, например *https://demo-aks-ingress.eastus.cloudapp.azure.com* .

Поскольку в этих примерах используется `letsencrypt-staging`, браузер не доверяет выданному сертификату SSL. Чтобы продолжить работу с приложением, примите предупреждение. Информация о сертификате свидетельствует о том, что этот сертификат *Fake LE Intermediate X1* выдан Let's Encrypt. Этот поддельный сертификат указывает на то, что `cert-manager` правильно обработал запрос и получил сертификат от поставщика.

![Промежуточный сертификат Let's Encrypt](media/ingress/staging-certificate.png)

Когда вы меняете настройки Let's Encrypt, чтобы использовать `prod` вместо `staging`, применяется доверенный сертификат, выданный Let's Encrypt, как показано в следующем примере:

![Сертификат Let's Encrypt](media/ingress/certificate.png)

В браузере отображается демонстрационная версия приложения:

![Первый пример приложения](media/ingress/app-one.png)

Теперь добавьте к FQDN путь */hello-world-two*, например *https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two* . Отобразится второе демонстрационное приложение с пользовательским заголовком:

![Второй пример приложения](media/ingress/app-two.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

В этой статье для установки компонентов обработки входящего трафика, сертификатов и примеров приложений используется Helm. При развертывании диаграммы Helm создается несколько ресурсов Kubernetes. К ним относятся элементы pod, развертывания и службы. Чтобы очистить эти ресурсы, можно либо удалить весь пример пространства имен, либо отдельные ресурсы.

### <a name="delete-the-sample-namespace-and-all-resources"></a>Удаление образца пространства имен и всех ресурсов

Чтобы удалить весь пример пространства имен, используйте команду `kubectl delete` и укажите имя пространства имен. Все ресурсы в пространстве имен удаляются.

```console
kubectl delete namespace ingress-basic
```

Затем удалите репозиторий Helm для приложения AKS Hello World:

```console
helm repo remove azure-samples
```

### <a name="delete-resources-individually"></a>Удаление ресурсов по отдельности

Кроме того, более детализированный подход заключается в удалении отдельных созданных ресурсов. Сначала удалите ресурсы сертификата:

```console
kubectl delete -f certificates.yaml
kubectl delete -f cluster-issuer.yaml
```

Теперь получите список выпусков Helm с помощью команды `helm list`. Найдите диаграммы *nginx-ingress*, *cert-manager* и *aks-helloworld*, как показано в следующем примере выходных данных.

```
$ helm list --all-namespaces

NAME                    NAMESPACE       REVISION        UPDATED                        STATUS          CHART                   APP VERSION
aks-helloworld          ingress-basic   1               2020-01-11 15:02:21.51172346   deployed        aks-helloworld-0.1.0
aks-helloworld-2        ingress-basic   1               2020-01-11 15:03:10.533465598  deployed        aks-helloworld-0.1.0
nginx-ingress           ingress-basic   1               2020-01-11 14:51:03.454165006  deployed        nginx-ingress-1.28.2    0.26.2
cert-manager            cert-manager    1               2020-01-06 21:19:03.866212286  deployed        cert-manager-v0.12.0            v0.12.0
```

Удалить выпуски командой `helm uninstall`. В следующем примере удаляются развертывание контроллера входящего трафика NGINX, диспетчер сертификатов и два примера приложений hello world для AKS.

```
$ helm uninstall aks-helloworld aks-helloworld-2 nginx-ingress -n ingress-basic

release "aks-helloworld" deleted
release "aks-helloworld-2" deleted
release "nginx-ingress" deleted

$ helm uninstall cert-manager -n cert-manager

release "cert-manager" deleted
```

Затем удалите репозиторий Helm, используемый для приложения hello world для AKS.

```console
helm repo remove azure-samples
```

Удалите само пространство имен. Используйте команду `kubectl delete` и укажите имя пространства имен:

```console
kubectl delete namespace ingress-basic
```

Наконец, удалите статический общедоступный IP-адрес, созданный для контроллера входящего трафика. Укажите имя группы ресурсов кластера *MC_* , полученное на первом шаге в этой статье, например *MC_myResourceGroup_myAKSCluster_eastus*.

```azurecli-interactive
az network public-ip delete --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP
```

## <a name="next-steps"></a>Дальнейшие действия

В данной статье упоминаются некоторые внешние компоненты для AKS. Чтобы узнать больше об этих компонентах, см. следующие страницы проекта:

- [Helm CLI][helm-cli]
- [Контроллер входящего трафика NGINX][nginx-ingress]
- [Диспетчер сертификатов][cert-manager]

Кроме того, вы можете сделать следующее:

- [Создание базового контроллера входящего трафика с внешним сетевым подключением][aks-ingress-basic]
- [Включение надстройки маршрутизации приложений HTTP][aks-http-app-routing]
- [Создание контроллера входящего трафика, использующего внутреннюю, частную сеть и IP-адрес][aks-ingress-internal]
- [Создание контроллера входящего трафика, использующего собственные сертификаты TLS][aks-ingress-own-tls]
- [Создание контроллера входящего трафика с динамическим общедоступным IP-адресом и Настройка шифрования для автоматического создания сертификатов TLS][aks-ingress-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[cert-manager-cluster-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/clusterissuers.html
[cert-manager-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/issuers.html
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm
[ingress-shim]: https://docs.cert-manager.io/en/latest/tasks/issuing-certificates/ingress-shim.html

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[client-source-ip]: concepts-network.md#ingress-controllers
[install-azure-cli]: /cli/azure/install-azure-cli
