---
title: Apache ZooKeeper серверу не удается сформировать кворум в Azure HDInsight
description: Apache ZooKeeper серверу не удается сформировать кворум в Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/20/2019
ms.openlocfilehash: 4e46efaf17ae9bad5df6f1f61f401d3e6de58a85
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2020
ms.locfileid: "78250242"
---
# <a name="apache-zookeeper-server-fails-to-form-a-quorum-in-azure-hdinsight"></a>Apache ZooKeeper серверу не удается сформировать кворум в Azure HDInsight

В этой статье описываются действия по устранению неполадок и возможные способы решения проблем при взаимодействии с кластерами Azure HDInsight.

## <a name="issue"></a>Проблема

Apache ZooKeeper Server является неработоспособным, симптомы могут включать в себя: диспетчеры ресурсов и узлы имен находятся в режиме ожидания, простые операции HDFS не работают, `zkFailoverController` остановлена и не могут быть запущены, Yarn/Spark/Livy задания завершаются сбоем из-за ошибок Zookeeper. Управляющие программы LLAP также могут не запускаться в защищенных кластерах Spark и Interactive. Может появиться сообщение об ошибке следующего вида:

```
19/06/19 08:27:08 ERROR ZooKeeperStateStore: Fatal Zookeeper error. Shutting down Livy server.
19/06/19 08:27:08 INFO LivyServer: Shutting down Livy server.
```

В журнале сервера Zookeeper на любом узле Zookeeper в/Вар/лог/зукипер/зукипер-зукипер-сервер-\*. out может также появиться следующая ошибка:

```
2020-02-12 00:31:52,513 - ERROR [CommitProcessor:1:NIOServerCnxn@178] - Unexpected Exception:
java.nio.channels.CancelledKeyException
```

## <a name="cause"></a>Причина

Если объем файлов моментальных снимков слишком большой или файлы моментальных снимков повреждены, ZooKeeper Server не сможет сформировать кворум, что приводит к неработоспособности связанных служб ZooKeeper. Сервер ZooKeeper не удаляет старые файлы моментальных снимков из своего каталога данных. это периодическая задача, выполняемая пользователями для поддержания работоспособности ZooKeeper. Дополнительные сведения см. в разделе [ZooKeeper сильные стороны и ограничения](https://zookeeper.apache.org/doc/r3.3.5/zookeeperAdmin.html#sc_strengthsAndLimitations).

## <a name="resolution"></a>Решение

Проверьте каталог данных ZooKeeper `/hadoop/zookeeper/version-2` и `/hadoop/hdinsight-zookeeper/version-2`, чтобы выяснить, достаточно ли размера файла моментальных снимков. При наличии крупных моментальных снимков выполните следующие действия.

1. Резервное копирование моментальных снимков в `/hadoop/zookeeper/version-2` и `/hadoop/hdinsight-zookeeper/version-2`.

1. Очистка моментальных снимков в `/hadoop/zookeeper/version-2` и `/hadoop/hdinsight-zookeeper/version-2`.

1. Перезапустите все серверы ZooKeeper из пользовательского интерфейса Apache Ambari.

## <a name="next-steps"></a>Дальнейшие действия

Если вы не видите своего варианта проблемы или вам не удается ее устранить, дополнительные сведения можно получить, посетив один из следующих каналов.

- Получите ответы от экспертов Azure через [службу поддержки сообщества Azure](https://azure.microsoft.com/support/community/).

- Подключайтесь с [@AzureSupport](https://twitter.com/azuresupport) — официальная учетная запись Microsoft Azure для улучшения качества взаимодействия с клиентами. Подключение сообщества Azure к нужным ресурсам: ответы, поддержка и эксперты.

- Если вам нужна дополнительная помощь, можно отправить запрос в службу поддержки из [портал Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Выберите пункт **Поддержка** в строке меню или откройте центр **справки и поддержки** . Для получения более подробных сведений см. статью [о создании запроса на поддержку Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Доступ к управлению подписками и поддержкой выставления счетов включен в вашу подписку Microsoft Azure, а техническая поддержка предоставляется через один из [планов поддержки Azure](https://azure.microsoft.com/support/plans/).
