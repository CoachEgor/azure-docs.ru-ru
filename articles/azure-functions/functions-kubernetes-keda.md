---
title: Функции Azure в Kubernetes с Кеда
description: Узнайте, как выполнять функции Azure в Kubernetes в облаке или локальной среде с помощью Кеда, управляемого событиями автоматического масштабирования на основе Kubernetes.
author: jeffhollan
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: jehollan
ms.openlocfilehash: 83c57b27c1cd1d524805a92381a1ba9eb2e1fbd6
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2020
ms.locfileid: "76549042"
---
# <a name="azure-functions-on-kubernetes-with-keda"></a>Функции Azure в Kubernetes с Кеда

Среда выполнения функций Azure обеспечивает гибкость при размещении, где и как вам нужно.  [Кеда](https://keda.sh) (автоматическое масштабирование, управляемое событиями на основе Kubernetes) легко с помощью среды выполнения функций Azure и инструментария для обеспечения масштабирования на основе событий в Kubernetes.

## <a name="how-kubernetes-based-functions-work"></a>Принципы работы функций на основе Kubernetes

Служба "функции Azure" состоит из двух ключевых компонентов: среды выполнения и контроллера масштабирования.  Среда выполнения функций выполняет и выполняет код.  Среда выполнения включает в себя логику для активации, ведения журнала и управления выполнением функций.  Среда выполнения функций Azure может работать *где угодно*.  Другой компонент является контроллером масштабирования.  Контроллер масштабирования отслеживает скорость событий, предназначенных для функции, и заранее масштабирует количество экземпляров, в которых выполняется приложение.  Дополнительные сведения см. в статье [Масштабирование и размещение Функций Azure](functions-scale.md).

Функции на основе Kubernetes предоставляют среду выполнения функций в [контейнере DOCKER](functions-create-function-linux-custom-image.md) с масштабированием на основе событий через Кеда.  Кеда может масштабироваться до 0 экземпляров (если события не происходят) и до *n* экземпляров. Это достигается за счет предоставления пользовательских метрик для автомасштабирования Kubernetes (горизонтальный Автомасштабирование Pod).  Использование контейнеров функций с Кеда позволяет реплицировать функции бессерверных функций в любом кластере Kubernetes.  Эти функции также можно развернуть с помощью [виртуальных узлов Azure Kubernetes Services (AKS)](../aks/virtual-nodes-cli.md) для бессерверной инфраструктуры.

## <a name="managing-keda-and-functions-in-kubernetes"></a>Управление Кеда и функциями в Kubernetes

Для запуска функций в кластере Kubernetes необходимо установить компонент Кеда. Этот компонент можно установить с помощью [Azure functions Core Tools](functions-run-local.md).

### <a name="installing-with-helm"></a>Установка с помощью Helm

Существует несколько способов установки Кеда в любом кластере Kubernetes, включая Helm.  Параметры развертывания описаны на [сайте Кеда](https://keda.sh/deploy/).

## <a name="deploying-a-function-app-to-kubernetes"></a>Развертывание приложения-функции в Kubernetes

Вы можете развернуть любое приложение-функцию в кластере Kubernetes, работающем под Кеда.  Так как функции выполняются в контейнере DOCKER, вашему проекту требуется `Dockerfile`.  Если у вас ее еще нет, можно добавить Dockerfile, выполнив следующую команду в корне проекта функций:

```cli
func init --docker-only
```

Чтобы создать образ и развернуть функции в Kubernetes, выполните следующую команду:

> [!NOTE]
> Основные инструменты будут использовать DOCKER CLI для создания и публикации образа. Убедитесь, что DOCKER уже установлен и подключен к вашей учетной записи с помощью `docker login`.

```cli
func kubernetes deploy --name <name-of-function-deployment> --registry <container-registry-username>
```

> Замените `<name-of-function-deployment>` на имя приложения-функции.

При этом создается ресурс `Deployment` Kubernetes, ресурс `ScaledObject` и `Secrets`, который включает переменные среды, импортированные из файла `local.settings.json`.

### <a name="deploying-a-function-app-from-a-private-registry"></a>Развертывание приложения-функции из частного реестра

Приведенный выше поток работает и для закрытых реестров.  Если вы извлекаете образ контейнера из частного реестра, включите флаг `--pull-secret`, который ссылается на секрет Kubernetes, содержащий частные учетные данные реестра при запуске `func kubernetes deploy`.

## <a name="removing-a-function-app-from-kubernetes"></a>Удаление приложения-функции из Kubernetes

После развертывания можно удалить функцию, удалив связанную `Deployment`, `ScaledObject`, созданную `Secrets`.

```cli
kubectl delete deploy <name-of-function-deployment>
kubectl delete ScaledObject <name-of-function-deployment>
kubectl delete secret <name-of-function-deployment>
```

## <a name="uninstalling-keda-from-kubernetes"></a>Удаление Кеда из Kubernetes

Действия по удалению Кеда описаны [на сайте Кеда](https://keda.sh/deploy/).

## <a name="supported-triggers-in-keda"></a>Поддерживаемые триггеры в Кеда

Кеда поддерживает следующие триггеры функций Azure:

* [Очереди службы хранилища Azure](functions-bindings-storage-queue.md)
* [Очереди служебной шины Azure](functions-bindings-service-bus.md)
* [Центры событий Azure и центры Интернета вещей](functions-bindings-event-hubs.md)
* [Apache Kafka](https://github.com/azure/azure-functions-kafka-extension)
* [Очередь RabbitMQ](https://github.com/azure/azure-functions-rabbitmq-extension)

### <a name="http-trigger-support"></a>Поддержка триггеров HTTP

Вы можете использовать функции Azure, которые предоставляют триггеры HTTP, но Кеда не управляют ими напрямую.  Вы можете использовать триггер Prometheus Кеда для [масштабирования функций HTTP Azure от 1 до *n* экземпляров](https://dev.to/anirudhgarg_99/scale-up-and-down-a-http-triggered-function-app-in-kubernetes-using-keda-4m42).

## <a name="next-steps"></a>Следующие шаги
Для получения дополнительных сведений см. следующие ресурсы:

* [Создание функции с помощью пользовательского образа](functions-create-function-linux-custom-image.md)
* [Как программировать и тестировать функции Azure в локальной среде](functions-develop-local.md)
* [Как работает план потребления функций Azure](functions-scale.md)