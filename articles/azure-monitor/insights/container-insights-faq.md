---
title: 'Azure Monitor для контейнеров: вопросы и ответы | Документация Майкрософт'
description: Решение Azure Monitor для контейнеров отслеживает состояние работоспособности кластера AKS и Экземпляров контейнеров в Azure. В этой статье приведены ответы на распространенные вопросы.
ms.topic: conceptual
ms.date: 10/15/2019
ms.openlocfilehash: b0d2be8b573dbbf047f4a27ae9ac9f611b76dc51
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/15/2020
ms.locfileid: "75977765"
---
# <a name="azure-monitor-for-containers-frequently-asked-questions"></a>Azure Monitor для контейнеров: вопросы и ответы

В этом списке корпорация Майкрософт собрала часто задаваемые вопросы с ответами об Azure Monitor для контейнеров. Если у вас есть другие вопросы об этом решении, вы можете задать их на [форуме для обсуждений](https://feedback.azure.com/forums/34192--general-feedback). Если вопрос задается часто, мы добавим его в эту статью, чтобы его можно было найти быстро и легко.

## <a name="i-dont-see-image-and-name-property-values-populated-when-i-query-the-containerlog-table"></a>Не отображаются значения свойств Image и Name, заполненные при запросе таблицы Контаинерлог.

Для агента версии ciprod12042019 и более поздних версий по умолчанию эти два свойства не заполняются для каждой строки журнала, чтобы максимально сокращать затраты на собираемые данные журнала. Существует два варианта запроса таблицы, включающей эти свойства со своими значениями:

### <a name="option-1"></a>Вариант 1 

Присоедините другие таблицы, чтобы включить эти значения свойств в результаты.

Измените запросы, включив свойства Image и Имажетаг из таблицы ```ContainerInventory```, присоединяясь к свойству ContainerID. Можно включить свойство Name (как оно было ранее появилось в ```ContainerLog``` таблице) из поля Контаиненаме таблицы Кубеподинвентори путем соединения по свойству ContainerID. Это рекомендуемый вариант.

Ниже приведен пример подробного запроса, посвященного получению значений полей с помощью соединений.

```
//lets say we are querying an hour worth of logs
let startTime = ago(1h);
let endTime = now();
//below gets the latest Image & ImageTag for every containerID, during the time window
let ContainerInv = ContainerInventory | where TimeGenerated >= startTime and TimeGenerated < endTime | summarize arg_max(TimeGenerated, *)  by ContainerID, Image, ImageTag | project-away TimeGenerated | project ContainerID1=ContainerID, Image1=Image ,ImageTag1=ImageTag;
//below gets the latest Name for every containerID, during the time window
let KubePodInv  = KubePodInventory | where ContainerID != "" | where TimeGenerated >= startTime | where TimeGenerated < endTime | summarize arg_max(TimeGenerated, *)  by ContainerID2 = ContainerID, Name1=ContainerName | project ContainerID2 , Name1;
//now join the above 2 to get a 'jointed table' that has name, image & imagetag. Outer left is safer in-case there are no kubepod records are if they are latent
let ContainerData = ContainerInv | join kind=leftouter (KubePodInv) on $left.ContainerID1 == $right.ContainerID2;
//now join ContainerLog table with the 'jointed table' above and project-away redundant fields/columns and rename columns that were re-written
//Outer left is safer so you dont lose logs even if we cannot find container metadata for loglines (due to latency, time skew between data types etc...)
ContainerLog
| where TimeGenerated >= startTime and TimeGenerated < endTime 
| join kind= leftouter (
   ContainerData
) on $left.ContainerID == $right.ContainerID2 | project-away ContainerID1, ContainerID2, Name, Image, ImageTag | project-rename Name = Name1, Image=Image1, ImageTag=ImageTag1 

```

### <a name="option-2"></a>Вариант 2

Повторно включите коллекцию для этих свойств для каждой строки журнала контейнера.

Если первый вариант неудобен из-за изменений запросов, можно снова включить сбор этих полей, включив параметр ```log_collection_settings.enrich_container_logs``` в карте конфигурации агента, как описано в разделе [Параметры конфигурации сбора данных](./container-insights-agent-config.md).

> [!NOTE]
> Второй вариант не рекомендуется для больших кластеров, имеющих более 50 узлов, так как он создает вызовы API-сервера из каждого узла > в кластере для выполнения этого дополнения. Этот параметр также увеличивает размер данных для каждой собираемой строки журнала.

## <a name="can-i-view-metrics-collected-in-grafana"></a>Можно ли просматривать метрики, собранные в Grafana?

Azure Monitor для контейнеров поддерживает просмотр метрик, хранящихся в рабочей области Log Analytics, на панелях мониторинга Grafana. Мы предоставили шаблон, который можно скачать из [репозитория панели мониторинга](https://grafana.com/grafana/dashboards?dataSource=grafana-azure-monitor-datasource&category=docker) Grafana, чтобы получить сведения о том, как запросить дополнительные данные из наблюдаемых кластеров для визуализации на пользовательских панелях мониторинга Grafana. 

## <a name="can-i-monitor-my-aks-engine-cluster-with-azure-monitor-for-containers"></a>Можно ли отслеживать кластер AKS Engine с Azure Monitor для контейнеров?

Azure Monitor для контейнеров поддерживает мониторинг рабочих нагрузок контейнеров, развернутых в кластерах AKS Engine (ранее известных как ядро ACS), размещенных в Azure. Дополнительные сведения и общие сведения о шагах, необходимых для включения мониторинга в этом сценарии, см. в разделе [использование Azure Monitor для контейнеров для AKS-Engine](https://github.com/microsoft/OMS-docker/tree/aks-engine).

## <a name="why-dont-i-see-data-in-my-log-analytics-workspace"></a>Почему данные в рабочей области Log Analytics не отображаются?

Если вы не можете просматривать какие-либо данные в Log Analytics рабочей области в определенное время ежедневно, возможно, достигнуто ограничение в 500 МБ или ежедневное ограничение, заданное для управления объемом данных, которые будут ежедневно собраны. При достижении этого ограничения сбор данных прекращается и возобновляется только на следующий день. Чтобы просмотреть сведения об использовании данных и обновить их до другой ценовой категории в зависимости от ожидаемых моделей использования, см. статью [ведение журнала за использование и стоимость данных](../platform/manage-cost-storage.md). 

## <a name="what-are-the-container-states-specified-in-the-containerinventory-table"></a>Какие состояния контейнеров указаны в таблице Контаинеринвентори?

Таблица ContainerInventory содержит сведения об остановленных и запущенных контейнерах. Эта таблица заполняется рабочим процессом агента, который получает из docker полный список контейнеров (работающих и остановленных) и пересылает эти данные в рабочую область Log Analytics.
 
## <a name="how-do-i-resolve-missing-subscription-registration-error"></a>Разделы справки устранить ошибку *регистрации отсутствующей подписки* ?

Если вы получаете ошибку **регистрация подписки для Microsoft. OperationsManagement**, ее можно устранить, зарегистрировав поставщик ресурсов **Microsoft. OperationsManagement** в подписке, в которой определена Рабочая область. Документацию по этому процессу можно найти [здесь](../../azure-resource-manager/templates/error-register-resource-provider.md).

## <a name="is-there-support-for-rbac-enabled-aks-clusters"></a>Поддерживается ли кластеры AKS, поддерживающие RBAC?

Решение для мониторинга контейнеров не поддерживает RBAC, но поддерживается с Azure Monitor для контейнеров. На странице сведений о решении может неправильно отображаться информация в колонках с данными о таких кластерах.

## <a name="how-do-i-enable-log-collection-for-containers-in-the-kube-system-namespace-through-helm"></a>Как включить сбор журналов для контейнеров в пространстве имен kube-system через Helm?

Сбор журналов из контейнеров в пространстве имен kube-system по умолчанию отключен. Вы можете включить сбор журналов, задав переменную среды для агента OMS. Дополнительные сведения см. на странице [Azure Monitor for Containers](https://github.com/helm/charts/tree/master/incubator/azuremonitor-containers) GitHub. 

## <a name="how-do-i-update-the-omsagent-to-the-latest-released-version"></a>Как обновить агент OMS до последней выпущенной версии?

Сведения об обновлении агента, см. в статье [Как управлять агентом Azure Monitor для контейнеров](container-insights-manage-agent.md).

## <a name="how-do-i-enable-multi-line-logging"></a>Как включить многострочное ведение журнала?

В настоящее время Azure Monitor для контейнеров не поддерживает многострочное ведение журнала, но вы можете применить обходные пути. Например, настройте вывод данных из всех служб в формате JSON, и тогда Docker/Moby будут сохранять эти данные в одну строку.

Следующий пример демонстрирует сохранение журнала в формате объекта JSON для тестового приложения node.js:

```
console.log(json.stringify({ 
      "Hello": "This example has multiple lines:",
      "Docker/Moby": "will not break this into multiple lines",
      "and you will receive":"all of them in log analytics",
      "as one": "log entry"
      }));
```

Эти данные будут выглядеть, как в следующем примере, в Azure Monitor для журналов при запросе.

```
LogEntry : ({“Hello": "This example has multiple lines:","Docker/Moby": "will not break this into multiple lines", "and you will receive":"all of them in log analytics", "as one": "log entry"}

```

Чтобы получить подробный обзор проблемы, просмотрите следующую [ссылку GitHub](https://github.com/moby/moby/issues/22920).

## <a name="how-do-i-resolve-azure-ad-errors-when-i-enable-live-logs"></a>Разделы справки устранить ошибки Azure AD при включении Live Logs? 

Может появиться следующее сообщение об ошибке: **URL-адрес ответа, указанный в запросе, не совпадает с URL-адреса ответа, настроенного для приложения: "< приложение с идентификатором\>"** . Решение для решения этой проблемы можно найти в статье [как просматривать данные контейнера в режиме реального времени с помощью Azure Monitor для контейнеров](container-insights-livedata-setup.md#configure-ad-integrated-authentication). 

## <a name="why-cant-i-upgrade-cluster-after-onboarding"></a>Почему не удается обновить кластер после адаптации?

Если после включения Azure Monitor для контейнеров в кластере AKS вы удалите Log Analytics рабочую область, в которую кластер отправит свои данные, при попытке обновления кластера произойдет сбой. Чтобы обойти это, необходимо отключить мониторинг, а затем снова включить его, указав другую действительную рабочую область в подписке. При повторном выполнении обновления кластера его необходимо обработать и завершить успешно.  

## <a name="which-ports-and-domains-do-i-need-to-openwhitelist-for-the-agent"></a>Какие порты и домены нужно открыть или список разрешений для агента?

Сведения о конфигурации прокси-сервера и брандмауэра, необходимые для контейнерного агента с помощью Azure, государственных организаций США и облаков Azure для Китая, см. в разделе [требования к сетевому брандмауэру](container-insights-onboard.md#network-firewall-requirements) .

## <a name="next-steps"></a>Дальнейшие действия

Чтобы получить представление о требованиях и доступных методах для мониторинга и начать мониторинг кластера AKS, просмотрите [How to onboard the Azure Monitor for containers](container-insights-onboard.md) (Как подключить Azure Monitor для контейнеров). 
