---
title: Включение Azure Monitor для контейнеров | Документация Майкрософт
description: В этой статье описывается, как включить и настроить Azure Monitor для контейнеров, чтобы вы могли понять, как работает контейнер и какие проблемы с производительностью были обнаружены.
ms.topic: conceptual
ms.date: 11/18/2019
ms.openlocfilehash: 7aad7e7dd5ec2569377f9276c2e4793c7afd631a
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79275312"
---
# <a name="how-to-enable-azure-monitor-for-containers"></a>Включение Azure Monitor для контейнеров

В этой статье приведены общие сведения о параметрах, доступных для установки Azure Monitor контейнерах для наблюдения за производительностью рабочих нагрузок, развернутых в средах Kubernetes и размещенных в.

- [Служба Kubernetes Azure](https://docs.microsoft.com/azure/aks/) (AKS)

- Самостоятельно управляемые кластеры Kubernetes, размещенные в Azure с помощью [механизма AKS](https://github.com/Azure/aks-engine).

- Самостоятельно управляемые кластеры Kubernetes, размещенные на [Azure Stack](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1910) или локально с помощью AKS Engine.

- [Azure Red Hat OpenShift](../../openshift/intro-openshift.md)

Azure Monitor для контейнеров можно включить для новых или нескольких существующих развертываний Kubernetes, используя следующие поддерживаемые методы:

- Из портал Azure, Azure PowerShell или с Azure CLI

- используя [Terraform и AKS](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>предварительные требования

Чтобы начать, у вас должны быть следующие компоненты:

- **Рабочая область Log Analytics.**

    Azure Monitor для контейнеров поддерживает рабочую область Log Analytics в регионах, перечисленных в списке [продуктов Azure по регионам](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor).

    Вы можете создать рабочую область, включив наблюдение за новым кластером AKS или добавим рабочую область по умолчанию в группе ресурсов по умолчанию для подписки кластера AKS. Если вы хотите создать ее самостоятельно, используйте [Azure Resource Manager](../platform/template-workspace-configuration.md), [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json) или [портал Azure](../learn/quick-create-workspace.md). Список поддерживаемых пар сопоставлений, используемых для рабочей области по умолчанию, см. в разделе [сопоставление регионов для Azure Monitor для контейнеров](container-insights-region-mapping.md).

- Вы являетесь членом **роли участника log Analytics** , чтобы включить мониторинг контейнеров. Дополнительные сведения о том, как управлять доступом к рабочей области Log Analytics, см. в статье [Управление рабочими областями](../platform/manage-access.md).

- Вы являетесь членом роли " **[владелец](../../role-based-access-control/built-in-roles.md#owner)** " в ресурсе кластера AKS.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

* Метрики Prometheus по умолчанию не собираются. Перед [настройкой агента](container-insights-prometheus-integration.md) для их получения важно ознакомиться с [документацией](https://prometheus.io/) по Prometheus, чтобы понять, что может быть списано и какие методы поддерживаются.

## <a name="supported-configurations"></a>Поддерживаемые конфигурации

Ниже официально поддерживаются Azure Monitor для контейнеров.

- Среды: Azure Red Hat OpenShift, Kubernetes в локальной среде и ядро AKS в Azure и Azure Stack. Дополнительные сведения см. [в разделе AKS Engine on Azure Stack](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1908).
- Версии Kubernetes и политики поддержки совпадают с [поддерживаемыми версиями AKS](../../aks/supported-kubernetes-versions.md). 

## <a name="network-firewall-requirements"></a>Требования к брандмауэру в сети

В следующей таблице перечислены сведения о конфигурации прокси-сервера и брандмауэра, необходимые для взаимодействия агента контейнера с Azure Monitor для контейнеров. Весь сетевой трафик от агента исходит от Azure Monitor.

|Ресурс агента|порты; |
|--------------|------|
| *.ods.opinsights.azure.com | 443 |  
| *.oms.opinsights.azure.com | 443 | 
| *.blob.core.windows.net | 443 |
| dc.services.visualstudio.com | 443 |
| *.microsoftonline.com | 443 |
| *. monitoring.azure.com | 443 |
| login.microsoftonline.com | 443 |

В следующей таблице приведены сведения о конфигурации прокси-сервера и брандмауэра для Azure для Китая.

|Ресурс агента|порты; |Description | 
|--------------|------|-------------|
| *. ods.opinsights.azure.cn | 443 | Прием данных |
| *. oms.opinsights.azure.cn | 443 | Адаптация OMS |
| *.blob.core.windows.net | 443 | Используется для мониторинга исходящих подключений. |
| microsoft.com | 80 | Используется для сетевого подключения. Это необходимо только в том случае, если версия образа агента — ciprod09262019 или более ранняя. |
| dc.services.visualstudio.com | 443 | Для телеметрии агента с помощью общедоступного облака Azure Application Insights. |

В следующей таблице приведены сведения о конфигурации прокси-сервера и брандмауэра для государственных организаций США.

|Ресурс агента|порты; |Description | 
|--------------|------|-------------|
| *.ods.opinsights.azure.us | 443 | Прием данных |
| *.oms.opinsights.azure.us | 443 | Адаптация OMS |
| *.blob.core.windows.net | 443 | Используется для мониторинга исходящих подключений. |
| microsoft.com | 80 | Используется для сетевого подключения. Это необходимо только в том случае, если версия образа агента — ciprod09262019 или более ранняя. |
| dc.services.visualstudio.com | 443 | Для телеметрии агента с помощью общедоступного облака Azure Application Insights. |

## <a name="components"></a>Components

Возможность мониторинга производительности зависит от контейнера Log Analytics агента для Linux, специально разработанного для Azure Monitor для контейнеров. Этот специализированный агент собирает данные о производительности и событиях со всех узлов кластера, а потом автоматически развертывается и регистрируется в указанной рабочей области Log Analytics во время развертывания. Версия агента — microsoft/oms:ciprod04202018 и выше. Номер представлен датой в формате *ммддгггг*.

>[!NOTE]
>В предварительном выпуске поддержки Windows Server для AKS в кластере AKS с узлами Windows Server не установлен агент для получения данных и пересылки в Azure Monitor. Вместо этого узел Linux, автоматически развернутый в кластере в рамках стандартного развертывания, собирает и пересылает данные Azure Monitor от имени всех узлов Windows в кластере.  
>

При выпуске новой версии агент автоматически обновляется в управляемых кластерах Kubernetes, размещенных в Службе Azure Kubernetes (AKS). Выпущенные версии см. в [объявлениях о выпусках агента](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).

>[!NOTE]
>Если вы уже развернули кластер AKS, то можете включить мониторинг с помощью Azure CLI или предоставив шаблон Azure Resource Manager, как показано далее в этой статье. Невозможно использовать `kubectl` для обновления, удаления, повторного развертывания или развертывания агента.
>Развертывание шаблона должно проходить в той же группе ресурсов, что и у кластера.

Включить Azure Monitor для контейнеров можно с помощью одного из следующих методов, описанных в следующей таблице.

| Состояние развертывания | Метод | Description |
|------------------|--------|-------------|
| Новый кластер AKS Kubernetes | [Создание кластера AKS с помощью Azure CLI](../../aks/kubernetes-walkthrough.md#create-aks-cluster)| Вы можете включить мониторинг нового кластера AKS, созданного с помощью Azure CLI. |
| | [Создание кластера AKS с помощью terraform](container-insights-enable-new-cluster.md#enable-using-terraform)| Вы можете включить мониторинг нового кластера AKS, созданного с помощью средства с открытым кодом terraform. |
| | [Создание кластера OpenShift с помощью шаблона Azure Resource Manager](container-insights-azure-redhat-setup.md#enable-for-a-new-cluster-using-an-azure-resource-manager-template) | Вы можете включить мониторинг нового кластера OpenShift, созданного с помощью предварительно настроенного шаблона Azure Resource Manager. |
| | [Создание кластера OpenShift с помощью Azure CLI](https://docs.microsoft.com/cli/azure/openshift?view=azure-cli-latest#az-openshift-create) | Мониторинг можно включить при развертывании нового кластера OpenShift с помощью Azure CLI. |
| Существующий кластер AKS Kubernetes | [Включение для кластера AKS с помощью Azure CLI](container-insights-enable-existing-clusters.md#enable-using-azure-cli) | Вы можете включить мониторинг кластера AKS, уже развернутого с помощью Azure CLI. |
| |[Включение для кластера AKS с помощью terraform](container-insights-enable-existing-clusters.md#enable-using-terraform) | Вы можете включить мониторинг кластера AKS, который уже развернут с помощью средства с открытым кодом terraform. |
| | [Включить для кластера AKS из Azure Monitor](container-insights-enable-existing-clusters.md#enable-from-azure-monitor-in-the-portal)| Вы можете включить мониторинг одного или нескольких кластеров AKS, уже развернутых на странице с несколькими кластерами Azure Monitor. |
| | [Включить из кластера AKS](container-insights-enable-existing-clusters.md#enable-directly-from-aks-cluster-in-the-portal)| Мониторинг можно включить непосредственно из кластера AKS в портал Azure. |
| | [Включение для кластера AKS с помощью шаблона Azure Resource Manager](container-insights-enable-existing-clusters.md#enable-using-an-azure-resource-manager-template)| Вы можете включить мониторинг кластера AKS с помощью предварительно настроенного шаблона Azure Resource Manager. |
| | [Включить для гибридного кластера Kubernetes](container-insights-hybrid-setup.md) | Вы можете включить мониторинг подсистемы AKS, размещенной в Azure Stack или для Kubernetes, размещенного локально. |
| | [Включение для кластера OpenShift с помощью шаблона Azure Resource Manager](container-insights-azure-redhat-setup.md#enable-using-an-azure-resource-manager-template) | Вы можете включить мониторинг существующего кластера OpenShift с помощью предварительно настроенного шаблона Azure Resource Manager. |
| | [Включить для кластера OpenShift из Azure Monitor](container-insights-azure-redhat-setup.md#from-the-azure-portal) | Вы можете включить мониторинг одного или нескольких кластеров OpenShift, уже развернутых на странице с несколькими кластерами Azure Monitor. |

## <a name="next-steps"></a>Дальнейшие действия

- С включенным наблюдением можно начать анализ производительности кластеров Kubernetes, размещенных в службе Kubernetes Azure (AKS), Azure Stack или другой среде. Сведения об использовании Azure Monitor для контейнеров см. в статье [Просмотр производительности кластера Kubernetes](container-insights-analyze.md).
