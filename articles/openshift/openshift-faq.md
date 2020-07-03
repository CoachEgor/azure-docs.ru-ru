---
title: Часто задаваемые вопросы о Azure Red Hat OpenShift
description: Ниже приведены ответы на часто задаваемые вопросы о Microsoft Azure Red Hat OpenShift.
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 92529c2d60b32e9c8b57b897008b5333adc2a4d4
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2020
ms.locfileid: "82594973"
---
# <a name="azure-red-hat-openshift-faq"></a>Вопросы и ответы об Azure Red Hat OpenShift

В этой статье рассматриваются часто задаваемые вопросы о Microsoft Azure Red Hat OpenShift.

## <a name="which-azure-regions-are-supported"></a>Какие регионы Azure поддерживаются?

Список глобальных регионов, в которых поддерживается Azure Red Hat OpenShift, см. в разделе [Supported Resources](supported-resources.md#azure-regions) .

## <a name="can-i-deploy-a-cluster-into-an-existing-virtual-network"></a>Можно ли развернуть кластер в существующей виртуальной сети?

Нет. Но вы можете подключить кластер Azure Red Hat OpenShift к существующей виртуальной сети с помощью пиринга. Дополнительные сведения см. в статье [подключение виртуальной сети кластера к существующей виртуальной сети](tutorial-create-cluster.md#create-the-cluster) .

## <a name="what-cluster-operations-are-available"></a>Какие операции кластера доступны?

Можно увеличить или уменьшить масштаб только числа узлов вычислений. Другие изменения не разрешены для `Microsoft.ContainerService/openShiftManagedClusters` ресурса после его создания. Максимальное число узлов вычислений ограничено 20.

## <a name="what-virtual-machine-sizes-can-i-use"></a>Какие размеры виртуальных машин можно использовать?

Список размеров виртуальных машин, которые можно использовать с кластером Azure Red Hat OpenShift, см. в статье [размеры виртуальных машин Azure Red Hat OpenShift](supported-resources.md#virtual-machine-sizes) .

## <a name="is-data-on-my-cluster-encrypted"></a>Зашифрованы ли данные в кластере?

По умолчанию шифрование неактивных. Платформа службы хранилища Azure автоматически шифрует данные перед сохранением и расшифровывает их перед извлечением. Дополнительные сведения см. [в статье шифрование службы хранилища Azure для неактивных данных](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) .

## <a name="can-i-use-prometheusgrafana-to-monitor-my-applications"></a>Можно ли использовать Prometheus/Grafana для мониторинга приложений?

Да, можно развернуть Prometheus в пространстве имен и отслеживать приложения в пространстве имен.

## <a name="can-i-use-prometheusgrafana-to-monitor-metrics-related-to-cluster-health-and-capacity"></a>Можно ли использовать Prometheus/Grafana для мониторинга метрик, связанных с работоспособностью и емкостью кластера?

Нет, не в текущее время.

## <a name="is-the-docker-registry-available-externally-so-i-can-use-tools-such-as-jenkins"></a>Доступен ли реестр DOCKER извне, поэтому я могу использовать такие средства, как Jenkins?

Реестр DOCKER доступен, `https://docker-registry.apps.<clustername>.<region>.azmosa.io/` однако гарантия надежности хранилища не предоставляется. Вы также можете использовать [Реестр контейнеров Azure](https://azure.microsoft.com/services/container-registry/).

## <a name="is-cross-namespace-networking-supported"></a>Поддерживается ли поддержка сетей с несколькими пространствами имен?

Администраторы клиентов и индивидуальных проектов могут настраивать сеть между пространствами имен (включая отклонения) для каждого проекта с помощью `NetworkPolicy` объектов.

## <a name="can-an-admin-manage-users-and-quotas"></a>Может ли администратор управлять пользователями и квотами?

Да. Администратор Red Hat OpenShift Azure может управлять пользователями и квотами в дополнение к доступу ко всем созданным пользователем проектам.

## <a name="can-i-restrict-a-cluster-to-only-certain-azure-ad-users"></a>Можно ли ограничить кластер только определенными пользователями Azure AD?

Да. Вы можете ограничить, какие пользователи Azure AD могут входить в кластер, настроив приложение Azure AD. Дополнительные сведения см. в разделе [как ограничить приложение набором пользователей.](https://docs.microsoft.com/azure/active-directory/develop/howto-restrict-your-app-to-a-set-of-users)

## <a name="can-i-restrict-users-from-creating-projects"></a>Можно ли запретить пользователям создавать проекты?

Да. Войдите в кластер в качестве администратора Azure Red Hat OpenShift и выполните следующую команду:

```
oc adm policy \
    remove-cluster-role-from-group self-provisioner \
    system:authenticated:oauth
```

Дополнительные сведения см. в документации OpenShift по [отключению самостоятельной подготовки](https://docs.openshift.com/container-platform/3.11/admin_guide/managing_projects.html#disabling-self-provisioning).

## <a name="can-a-cluster-have-compute-nodes-across-multiple-azure-regions"></a>Может ли кластер вычислять узлы в нескольких регионах Azure?

Нет. Все узлы в кластере Azure Red Hat OpenShift должны исходить из одного региона Azure.

## <a name="are-master-and-infrastructure-nodes-abstracted-away-as-they-are-with-azure-kubernetes-service-aks"></a>Являются ли узлы главного и инфраструктуры абстрактными, как это было в службе Azure Kubernetes Service (AKS)?

Нет. Все ресурсы, включая главный хозяин кластера, выполняются в клиентской подписке. Эти типы ресурсов помещаются в группу ресурсов только для чтения.

## <a name="is-open-service-broker-for-azure-osba-supported"></a>Поддерживается ли открытый Service Broker для Azure (OSBA)?

Да. Вы можете использовать OSBA с Azure Red Hat OpenShift. Дополнительные сведения см. в статье [Open Service Broker for Azure](https://github.com/Azure/open-service-broker-azure#openshift-project-template) .

## <a name="i-am-trying-to-peer-into-a-virtual-network-in-a-different-subscription-but-getting-failed-to-get-vnet-cidr-error"></a>Я пытаюсь выполнить пиринг в виртуальной сети в другой подписке, но получить `Failed to get vnet CIDR` ошибку.

В подписке с виртуальной сетью обязательно Зарегистрируйте `Microsoft.ContainerService` поставщик с помощью`az provider register -n Microsoft.ContainerService --wait` 

## <a name="what-is-the-azure-red-hat-openshift-aro-maintenance-process"></a>Что такое процесс обслуживания Azure Red Hat OpenShift (АТО)?

Существует три типа обслуживания для АТО: обновления, резервное копирование и восстановление данных etcd, а затем обслуживание, инициированное поставщиком облачных служб.

+ Обновления включают обновления программного обеспечения и CVE. Обновление CVE происходит при запуске, запуская `yum update` и предоставляющая немедленное устранение проблем.  В параллельном режиме новая сборка образа будет создана для создания в будущем кластере.

+ Резервное копирование и управление данными etcd — это автоматизированный процесс, который может потребовать простоя кластера в зависимости от действия. Если база данных etcd восстанавливается из резервной копии, это приведет к простою. Мы создаем резервную копию etcd ежечасно и сохраняем последние 6 часов резервных копий.

+ Обслуживание, инициированное поставщиком облачных служб, включает в себя сети, хранение и региональные стандарты. Обслуживание зависит от поставщика облачных служб и зависит от предоставляемых поставщиком обновлений.

## <a name="what-is-the-general-upgrade-process"></a>Что такое общий процесс обновления?

Выполнение обновления должно быть надежным процессом и не нарушать работу служб кластеров. ВЫПОЛНЯЮТСЯ может активировать процесс обновления, когда доступны новые версии или CVE.

Доступные обновления тестируются в рабочей среде, а затем применяются к рабочим кластерам. При применении новый узел временно добавляется, а узлы обновляются в повернутом виде, чтобы модули Pod поддерживали счетчики реплик. Следующие рекомендации помогут обеспечить минимальное отсутствие простоев.

В зависимости от уровня серьезности ожидающего обновления или обновления процесс может быть различным в том, что обновления могут быть применены быстро, чтобы снизить уязвимость службы до CVE. Новый образ будет создаваться асинхронно, тестироваться и сводиться в качестве обновления кластера. Кроме того, между аварийным и плановым обслуживанием нет никаких различий. Плановое обслуживание не запланировано для клиента.

Уведомления могут отправляться через ICM и по электронной почте, если требуется взаимодействие с клиентом.

## <a name="what-about-emergency-vs-planned-maintenance-windows"></a>Как насчет экстренных и плановых периодов обслуживания?

Мы не будем различать два типа обслуживания. Наши команды доступны 24/7/365 и не используют традиционные запланированные периоды обслуживания "за нерабочее время".

## <a name="how-will-host-operating-system-and-openshift-software-be-updated"></a>Как будет обновляться операционная система и программное обеспечение OpenShift?

Операционная система узла и программное обеспечение OpenShift обновляются с помощью общего процесса сборки и создания образа.

## <a name="whats-the-process-to-reboot-the-updated-node"></a>В чем заключается процедура перезагрузки обновленного узла?

Это должно быть обработано как часть обновления.

## <a name="is-data-stored-in-etcd-encrypted-on-aro"></a>Хранятся ли данные в etcd, зашифрованные на АТО?

Он не шифруется на уровне etcd. Параметр, который необходимо включить, сейчас не поддерживается. OpenShift поддерживает эту функцию, но требуется инженерная деятельность, чтобы сделать ее на схеме дороги. Данные шифруются на уровне диска. Дополнительные сведения см. в разделе [Шифрование данных на уровне хранилища](https://docs.openshift.com/container-platform/3.11/admin_guide/encrypting_data.html) .

## <a name="can-logs-of-underlying-vms-be-streamed-out-to-a-customer-log-analysis-system"></a>Могут ли журналы базовых виртуальных машин передаваться в потоковую систему анализа журнала клиента?

Системный журнал, журналы DOCKER, журнал и дмесг обрабатываются управляемой службой и не предоставляются клиентам.

## <a name="how-can-a-customer-get-access-to-metrics-like-cpumemory-at-the-node-level-to-take-action-to-scale-debug-issues-etc-i-cannot-seem-to-run-kubectl-top-on-an-aro-cluster"></a>Как клиент может получить доступ к метрикам, таким как ЦП/память на уровне узла, чтобы выполнить действия по масштабированию, отладке проблем и т. д. Мне кажется, что я `kubectl top` не могу работать в кластере АТО.

Клиенты могут получить доступ к метрикам ЦП и памяти на уровне узла с помощью команды `oc adm top nodes` или `kubectl top nodes` клустерроле клиента.  Клиенты также могут получить доступ к метрикам ЦП и памяти `pods` с помощью команды `oc adm top pods` или`kubectl top pods`

## <a name="what-is-the-default-pod-scheduler-configuration-for-aro"></a>Какова конфигурация планировщика Pod по умолчанию для АТО?

АТО использует планировщик по умолчанию, который поставляется в OpenShift. Существует несколько дополнительных механизмов, которые не поддерживаются в АТО. Дополнительные сведения см. в документации [по по умолчанию](https://docs.openshift.com/container-platform/3.11/admin_guide/scheduling/scheduler.html#generic-scheduler) , документации по планировщику и [главному планировщику](https://github.com/openshift/openshift-azure/blob/master/pkg/startup/v16/data/master/etc/origin/master/scheduler.json) .

Расширенное или настраиваемое планирование в настоящее время не поддерживается. Дополнительные сведения см. в [документации по планированию](https://docs.openshift.com/container-platform/3.11/admin_guide/scheduling/index.html) .

## <a name="if-we-scale-up-the-deployment-how-do-azure-fault-domains-map-into-pod-placement-to-ensure-all-pods-for-a-service-do-not-get-knocked-out-by-a-failure-in-a-single-fault-domain"></a>Если развернуть развертывание, то как домены сбоя Azure сопоставляются с размещением Pod, чтобы не замаскировать все модули для службы в случае сбоя в одном домене сбоя?

При использовании масштабируемых наборов виртуальных машин в Azure по умолчанию используются пять доменов сбоя. Каждый экземпляр виртуальной машины в масштабируемом наборе будет помещен в один из этих доменов сбоя. Это гарантирует, что приложения, развернутые на вычисленных узлах в кластере, будут размещены в отдельных доменах сбоя.

Дополнительные сведения см. в разделе [Выбор правильного числа доменов сбоя для масштабируемого набора виртуальных машин](https://docs.microsoft.com//azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-fault-domains) .

## <a name="is-there-a-way-to-manage-pod-placement"></a>Существует ли способ управления размещением Pod?

Клиенты могут получать узлы и просматривать метки в качестве клиента-администратора.  Это обеспечит возможность выбрать любую виртуальную машину в масштабируемом наборе.

При использовании специальных меток необходимо использовать осторожность.

- Имя узла не должно использоваться. Имя узла часто поворачивается с обновлением и обновлениями и гарантированно изменяется.

- Если у клиента есть запрос на определенные метки или стратегию развертывания, это можно сделать, но в настоящее время потребуется выполнить проектирование и не поддерживаться сегодня.

## <a name="what-is-the-maximum-number-of-pods-in-an-aro-cluster-what-is-the-maximum-number-of-pods-per-node-in-aro"></a>Каково максимальное число модулей Pod в кластере АТО?Каково максимальное количество модулей Pod на узел в АТО?

 Azure Red Hat OpenShift 3,11 имеет ограничение в 50-Pod на узел с [АТО, имеющим 20-расчетный предел узлов](https://docs.microsoft.com/azure/openshift/openshift-faq#what-cluster-operations-are-available), чтобы максимальное число модулей Pod, поддерживаемое в кластере АТО, было равно 50 * 20 = 1000.

## <a name="can-we-specify-ip-ranges-for-deployment-on-the-private-vnet-avoiding-clashes-with-other-corporate-vnets-once-peered"></a>Можно ли указать диапазоны IP-адресов для развертывания в частной виртуальной сети, избегая конфликта с другими корпоративными виртуальных сетей после пиринга?

Azure Red Hat OpenShift поддерживает пиринг виртуальных сетей и позволяет клиенту предоставлять виртуальную сеть для пиринга с и CIDR виртуальной сети, в которой будет действовать сеть OpenShift.

Виртуальная сеть, созданная с помощью АТО, будет защищена и не будет принимать изменения конфигурации. Виртуальная сеть, для которой установлено одноранговое подключение, управляется клиентом и находится в своей подписке.

## <a name="does-the-cluster-reside-in-a-customer-subscription"></a>Находится ли кластер в клиентской подписке? 

Управляемое приложение Azure находится в заблокированной группе ресурсов с подпиской клиента. Клиент может просматривать объекты в этом RG, но не изменять их.

## <a name="is-the-sdn-module-configurable"></a>Можно ли настроить модуль SDN?

SDN является openshift-OVS-нетворкполици и не может быть настроен.

## <a name="which-unix-rights-in-iaas-are-available-for-mastersinfraapp-nodes"></a>Какие права UNIX (в IaaS) доступны для образцов, а так и для узлов с инфракрасными или приложениями?

Неприменимо к этому предложению. Доступ к узлу запрещен.

## <a name="which-ocp-rights-do-we-have-cluster-admin-project-admin"></a>Какие права OCP у нас есть? Кластер — администратор? Проект — администратор?

Дополнительные сведения см. в статье [Обзор администрирования кластера](https://docs.openshift.com/aro/admin_guide/index.html)Azure Red Hat OpenShift.

## <a name="which-kind-of-federation-with-ldap"></a>Какой тип Федерации с LDAP?

Это достигается благодаря интеграции с Azure AD. 

## <a name="is-there-any-element-in-aro-shared-with-other-customers-or-is-everything-independent"></a>Есть ли какой-либо элемент в АТО, совместно используемый с другими клиентами? Или является ли все независимым?

Каждый кластер Azure Red Hat OpenShift предназначен для конкретного клиента и находится в подписке клиента. 

## <a name="can-we-choose-any-persistent-storage-solution-like-ocs"></a>Можно ли выбрать любое решение постоянного хранения, например OCS? 

Для выбора доступны два класса хранения: диск Azure и файл Azure.

## <a name="how-is-a-cluster-updated-including-majors-and-minors-due-to-vulnerabilities"></a>Как обновляется кластер (включая основные и вспомогательные сведения из-за уязвимостей)?

Узнайте [, что такое общий процесс обновления?](https://docs.microsoft.com/azure/openshift/openshift-faq#what-is-the-general-upgrade-process)

## <a name="what-azure-load-balancer-is-used-by-aro-is-it-standard-or-basic-and-is-it-configurable"></a>Какую подсистему балансировки нагрузки Azure использует АТО?Является ли он стандартным или базовым и является ли он настраиваемым?

АТО использует стандартные Azure Load Balancer и не настраивается.

## <a name="can-aro-use-netapp-based-storage"></a>Можно ли АТО использовать хранилище на основе NetApp?

В настоящее время поддерживаются только такие варианты хранения, как диск Azure и классы хранилища файлов Azure. 


