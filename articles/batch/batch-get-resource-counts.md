---
title: Подсчет состояний для задач и узлов — пакетная служба Azure | Документы Майкрософт
description: Подсчет состояний задач пакетной службы Azure и вычислительных узлов для управления решениями пакетной службы и их мониторинга.
services: batch
author: ju-shim
manager: gwallace
ms.service: batch
ms.topic: article
ms.date: 09/07/2018
ms.author: jushiman
ms.custom: seodec18
ms.openlocfilehash: 5e90045b7863968e8c61c3cbc382434bc8be415a
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/15/2020
ms.locfileid: "76029702"
---
# <a name="monitor-batch-solutions-by-counting-tasks-and-nodes-by-state"></a>Мониторинг решений пакетной службы путем подсчета задач и узлов с определенным состоянием

Чтобы отслеживать крупномасштабные решения пакетной службы Azure и управлять ими, нужно точно подсчитать количество ресурсов в различных состояниях. Пакетная служба Azure предоставляет эффективные операции для получения этих счетчиков для *задач* и *вычислительных узлов* пакетной службы. Используйте эти операции вместо потенциально времязатратных запросов на вывод списка, которые возвращают подробные сведения о больших коллекциях задач или узлов.

* [Получение счетчиков задач][rest_get_task_counts] Возвращает совокупное число активных, выполняющихся и завершенных задач в задании, а также выполненных или неудачных задач. 

  Узнав количество задач в каждом из состояний, вы можете легко отобразить ход выполнения задания пользователю или обнаружить непредвиденные задержки или ошибки, которые могут повлиять на задание. Операция Get Task Counts доступна, начиная с версии 2017-06-01.5.1 API пакетной службы, а также связанных пакетов SDK и средств.

* Количество [узлов в составе пула][rest_get_node_counts] возвращает число выделенных и низкоуровневых вычислений узлов в каждом пуле, находящиеся в различных состояниях: создание, простое, автономное, вытесненное, перезагрузка, повторное создание образа, запуск и другие. 

  Подсчитав число узлов в каждом из состояний, можно определить, когда вы получите достаточно вычислительных ресурсов для выполнения заданий, а также выявлять потенциальные проблемы с пулами. Операция List Pool Node Counts доступна, начиная с версии 2018-03-01.6.1 API пакетной службы, а также связанных пакетов SDK и средств.

Если вы используете версию службы, которая не поддерживает операции подсчета задач или узлов, используйте для подсчета этих ресурсов запрос перечисления. Кроме того, используйте запрос перечисления для получения сведений о других ресурсах пакетной службы, таких как приложения, файлы и задания. Дополнительные сведения о применении фильтров к запросам перечисления см. в разделе [Эффективное создание запросов на вывод списка ресурсов пакетной службы](batch-efficient-list-queries.md).

## <a name="task-state-counts"></a>Подсчет состояний задач

Операция Get Task Counts подсчитывает задачи по следующим состояниям:

- **Активна** — задача находится в очереди и может быть запущена, но еще не назначена вычислительному узлу. Задача также является `active`, если она [зависит от родительской задачи](batch-task-dependencies.md), которая еще не завершена. 
- **Выполняется** — задача назначена вычислительному узлу, но еще не завершена. Задача считается `running`, если ее состояние имеет значение `preparing` или `running`, как указано в операции [получения сведений о задаче][rest_get_task] .
- **Завершенная** — задача, которую больше невозможно запустить, так как она завершились успешно или неудачно и исчерпала предел повторных попыток. 
- **Успешно выполненная** — задача, результатом выполнения которой является состояние `success`. Пакетная службы определяет, завершилась ли задача успешно или нет, проверив свойство `TaskExecutionResult` свойства [executionInfo][rest_get_exec_info] .
- **Неудачно выполненная** — задача, результатом выполнения которой является состояние `failure`.

В следующем примере кода .NET показано, как получить число задач по состоянию: 

```csharp
var taskCounts = await batchClient.JobOperations.GetJobTaskCountsAsync("job-1");

Console.WriteLine("Task count in active state: {0}", taskCounts.Active);
Console.WriteLine("Task count in preparing or running state: {0}", taskCounts.Running);
Console.WriteLine("Task count in completed state: {0}", taskCounts.Completed);
Console.WriteLine("Succeeded task count: {0}", taskCounts.Succeeded);
Console.WriteLine("Failed task count: {0}", taskCounts.Failed);
```

Чтобы получить количество задач для задания, можно использовать аналогичный шаблон для REST и других поддерживаемых языков. 

> [!NOTE]
> Версии API пакетной службы до 2018-08-01.7.0 также возвращают свойство `validationStatus` в ответе операции Get Task Counts. Это свойство указывает, проверила ли пакетная служба количество состояний на согласованность с состояниями, указанными в API перечисления задач. Значение `validated` указывает только на то, что пакетная служба выполнила для задания проверку на согласованность хотя бы один раз. Значение свойства `validationStatus` не указывает, актуальны ли в настоящее время подсчеты, возвращаемые операцией Get Task Counts.
>

## <a name="node-state-counts"></a>Подсчет состояний узлов

Операция List Pool Node Counts подсчитывает число вычислительных узлов с указанными ниже состояниями в каждом пуле. Для выделенных узлов и низкоприоритетных узлов в каждом пуле предоставляются отдельные статистические счетчики.

- **Creating** — выделенная в Azure виртуальная машина, которая еще не запущена для присоединения к пулу.
- **Idle** — доступный вычислительный узел, который сейчас не выполняет никакую задачу.
- **LeavingPool** — узел, который покидает пул, так как пользователь явно удалил его либо выполняется изменение размера или автомасштабирование пула (в сторону уменьшения).
- **Offline** — узел, который пакетная служба не может использовать для планирования новых задач.
- **Preempted** — низкоприоритетный узел, который был удален из пула, так как служба Azure освободила виртуальную машину. Узел `preempted` можно повторно инициализировать, если доступна низкоприоритетная емкость виртуальной машины для замены.
- **Rebooting** — перезапускаемый узел.
- **Reimaging** — узел, на котором переустанавливается операционная система.
- **Running** — узел, на котором выполняется одна или несколько задач (кроме задачи запуска).
- **Starting** — узел, на котором запускается пакетная служба. 
- **Старттаскфаилед** — узел, на котором произошел сбой [задачи запуска][rest_start_task] и исчерпаны все повторные попытки, а также значение, на которое `waitForSuccess` задается в задаче запуска. Этот узел невозможно использовать для выполнения задач.
- **Unknown** — узел, который потерял связь с пакетной службой, и состояние которого неизвестно.
- **Unusable** — узел, который невозможно использовать для выполнения задачи из-за ошибок.
- **WaitingForStartTask** — узел, на котором начала выполняться задача запуска, однако задан `waitForSuccess` и задача запуска не была завершена.

В следующем фрагменте кода C# показано, как перечислить количество узлов для всех пулов в текущей учетной записи:

```csharp
foreach (var nodeCounts in batchClient.PoolOperations.ListPoolNodeCounts())
{
    Console.WriteLine("Pool Id: {0}", nodeCounts.PoolId);

    Console.WriteLine("Total dedicated node count: {0}", nodeCounts.Dedicated.Total);

    // Get dedicated node counts in Idle and Offline states; you can get additional states.
    Console.WriteLine("Dedicated node count in Idle state: {0}", nodeCounts.Dedicated.Idle);
    Console.WriteLine("Dedicated node count in Offline state: {0}", nodeCounts.Dedicated.Offline);

    Console.WriteLine("Total low priority node count: {0}", nodeCounts.LowPriority.Total);

    // Get low-priority node counts in Running and Preempted states; you can get additional states.
    Console.WriteLine("Low-priority node count in Running state: {0}", nodeCounts.LowPriority.Running);
    Console.WriteLine("Low-priority node count in Preempted state: {0}", nodeCounts.LowPriority.Preempted);
}
```
В следующем фрагменте кода C# показано, как перечислить количество узлов для заданного пула в текущей учетной записи.

```csharp
foreach (var nodeCounts in batchClient.PoolOperations.ListPoolNodeCounts(new ODATADetailLevel(filterClause: "poolId eq 'testpool'")))
{
    Console.WriteLine("Pool Id: {0}", nodeCounts.PoolId);

    Console.WriteLine("Total dedicated node count: {0}", nodeCounts.Dedicated.Total);

    // Get dedicated node counts in Idle and Offline states; you can get additional states.
    Console.WriteLine("Dedicated node count in Idle state: {0}", nodeCounts.Dedicated.Idle);
    Console.WriteLine("Dedicated node count in Offline state: {0}", nodeCounts.Dedicated.Offline);

    Console.WriteLine("Total low priority node count: {0}", nodeCounts.LowPriority.Total);

    // Get low-priority node counts in Running and Preempted states; you can get additional states.
    Console.WriteLine("Low-priority node count in Running state: {0}", nodeCounts.LowPriority.Running);
    Console.WriteLine("Low-priority node count in Preempted state: {0}", nodeCounts.LowPriority.Preempted);
}
```
Чтобы получить количество узлов для пулов, можно использовать аналогичный шаблон для REST и других поддерживаемых языков.
 
## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения об основных понятиях и функциях пакетной службы см. в статье [Разработка решений для крупномасштабных параллельных вычислений с использованием пакетной службы](batch-api-basics.md). В этой статье рассматриваются основные ресурсы пакетной службы, такие как пулы, вычислительные узлы, задания и задачи, а также приведены общие сведения о функциях этой службы.

* Сведения о применении фильтров к запросам, перечисляющим ресурсы пакетной службы, см. в разделе [Эффективное создание запросов на вывод списка ресурсов пакетной службы](batch-efficient-list-queries.md).


[rest_get_task_counts]: /rest/api/batchservice/job/gettaskcounts
[rest_get_node_counts]: /rest/api/batchservice/account/listpoolnodecounts
[rest_get_task]: /rest/api/batchservice/task/get
[rest_list_tasks]: /rest/api/batchservice/task/list
[rest_get_exec_info]: /rest/api/batchservice/task/get
[rest_start_task]: /rest/api/batchservice/pool/add#starttask

