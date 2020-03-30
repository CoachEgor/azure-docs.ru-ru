---
author: dlepow
ms.service: container-service
ms.topic: include
ms.date: 11/09/2018
ms.author: danlep
ms.openlocfilehash: a4af53e035929a44f74a95b8e9897cb1dc0c6d8e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "66814814"
---
[!INCLUDE [ACS deprecation](container-service-deprecation.md)]

После того как вы [развернете кластер службы контейнера Azure](../articles/container-service/dcos-swarm/container-service-deployment.md), может потребоваться изменить число узлов агента. Например, будут нужны дополнительные узлы агентов для запуска большего количества контейнеров или экземпляров приложения. 

Количество узлов агентов в кластере DC/OS, Docker Swarm или Kubernetes можно изменить с помощью портала Azure или Azure CLI. 

## <a name="scale-with-the-azure-portal"></a>Масштабирование с помощью портала Azure

1. На [портале Azure](https://portal.azure.com) найдите **Службы контейнеров** и выберите службу контейнера, которую хотите изменить.
2. В колонке **службы контейнеров** щелкните **Агенты**.
3. В поле **Число виртуальных машин** введите нужное число узлов агентов.

    ![Масштабирование кластера на портале](./media/container-service-scale/container-service-scale-portal.png)

4. Чтобы сохранить конфигурацию, нажмите кнопку **Save** (Сохранить).

## <a name="scale-with-the-azure-cli"></a>Масштабирование с помощью Azure CLI

[Установите ClI Azure](/cli/azure/install-azure-cli) и вопийте `az login`на учетную запись Azure с помощью .

### <a name="see-the-current-agent-count"></a>Просмотр текущего числа агентов
Чтобы просмотреть количество агентов, входящих в кластер на текущий момент, запустите команду `az acs show`. Эта команда показывает конфигурацию кластера. Например, следующая команда отображает конфигурацию службы контейнеров с именем `containerservice-myACSName` в группе ресурсов `myResourceGroup`:

```azurecli
az acs show -g myResourceGroup -n containerservice-myACSName
```

Количество агентов содержится в параметре `Count` в разделе `AgentPoolProfiles`.

### <a name="use-the-az-acs-scale-command"></a>Использование команды az acs scale
Чтобы изменить число узлов агента, запустите команду `az acs scale`, указав для нее **группу ресурсов**, **имя службы контейнеров** и требуемое **новое число агентов**. Используя меньшее или большее количество агентов, можно уменьшать или увеличивать масштаб соответственно.

Например, чтобы для кластера из предыдущего примера установить значение 10 в качестве количества агентов, введите следующую команду:

```azurecli
az acs scale -g myResourceGroup -n containerservice-myACSName --new-agent-count 10
```

Azure CLI возвращает строку JSON, представляющую новую конфигурацию службы контейнеров, включая измененное количество агентов.

Чтобы увидеть дополнительные параметры команды, запустите `az acs scale --help`.

## <a name="scaling-considerations"></a>Рекомендации по масштабированию

* Количество узлов агентов должно находиться в диапазоне от 1 до 100 включительно. 

* Квота на использование ядер может налагать ограничения на количество узлов агентов в кластере.

* Операции масштабирования узлов агентов применяются к масштабируемому набору виртуальных машин Azure, который содержит пул агентов. В кластере DC/OS операции масштабирования, описанные в этой статье, влияют только на число узлов агентов в закрытом пуле.

* В зависимости от того, какой оркестратор развернут в кластере, можно отдельно масштабировать число экземпляров контейнера, работающих в кластере. Например, в кластере DC/OS [пользовательский интерфейс Marathon](../articles/container-service/dcos-swarm/container-service-mesos-marathon-ui.md) позволяет изменять число экземпляров для приложения контейнера.


## <a name="next-steps"></a>Дальнейшие действия
* Изучите [дополнительные примеры](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) использования команд Azure CLI для работы со Службой контейнеров Azure.
* См. дополнительные сведения о [пулах агентов DC/OS](../articles/container-service/dcos-swarm/container-service-dcos-agents.md) в службе контейнеров Azure.

