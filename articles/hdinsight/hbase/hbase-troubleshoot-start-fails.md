---
title: Не удается запустить Apache HBase Master в Azure HDInsight
description: Не удается запустить Apache HBase Master (HMaster) в Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/14/2019
ms.openlocfilehash: 290b541d9b5e86616373d2e426241fca07e780ed
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75887212"
---
# <a name="apache-hbase-master-hmaster-fails-to-start-in-azure-hdinsight"></a>Не удается запустить Apache HBase Master (HMaster) в Azure HDInsight

В этой статье описываются действия по устранению неполадок и возможные способы решения проблем при взаимодействии с кластерами Azure HDInsight.

## <a name="scenario-atomic-renaming-failure"></a>Сценарий: сбой атомарного переименования

### <a name="issue"></a>Проблема

В процессе запуска обнаружены непредвиденные файлы.

### <a name="cause"></a>Причина:

В процессе запуска HMaster выполняет множество шагов инициализации, включая перемещение данных из папки "Рабочая область" (. tmp) в папку данных. HMaster также просматривает папку с упреждающими журналами (WAL), чтобы узнать, есть ли Неотвечающие серверы региона.

HMaster выполняет команду Basic List в папках WAL. Каждый раз, когда в любой из этих папок обнаруживается непредвиденный файл, возникает исключение и сервер не будет запущен.

### <a name="resolution"></a>Разрешение

Проверьте стек вызовов и попробуйте определить, в какой папке может быть вызвана проблема (например, это может быть папка WAL или папка. tmp). Затем в Cloud Explorer или с помощью команд HDFS попытайтесь найти файл с ошибкой. Обычно это `*-renamePending.json` файл. ( `*-renamePending.json` Файл является файлом журнала, который используется для реализации атомарной операции переименования в драйвере WASB. Из-за ошибок в этой реализации эти файлы могут остаться после сбоев процесса и т. д.) Принудительно удалите этот файл либо в Cloud Explorer, либо с помощью команд HDFS.

Иногда также может быть временный файл с именем примерно `$$$.$$$` в этом месте. Чтобы увидеть этот файл, необходимо использовать команду `ls` HDFS. Этот файл не отображается в Cloud Explorer. Чтобы удалить этот файл, используйте команду HDFS `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.

После выполнения этих команд HMaster должен сразу запуститься.

---

## <a name="scenario-no-server-address-listed"></a>Сценарий: адрес сервера не указан.

### <a name="issue"></a>Проблема

Может появиться сообщение о том, что таблица не `hbase: meta` находится в режиме «в сети». Выполнение `hbck` может сообщить о `hbase: meta table replicaId 0 is not found on any region.` том, что в журналах HMaster может появиться сообщение: `No server address listed in hbase: meta for region hbase: backup <region name>`.  

### <a name="cause"></a>Причина:

Не удалось инициализировать HMaster после перезапуска HBase.

### <a name="resolution"></a>Разрешение

1. В оболочке HBase введите следующие команды (при необходимости измените на фактические значения):

    ```hbase
    scan 'hbase:meta'
    delete 'hbase:meta','hbase:backup <region name>','<column name>'
    ```

1. Удалите `hbase: namespace` запись. Эта запись может быть той же ошибкой, о которой сообщается при `hbase: namespace` сканировании таблицы.

1. Перезапустите активный сервер HMaster с помощью интерфейса Ambari, чтобы HBase стала работать.

1. В оболочке HBase выполните следующую команду, чтобы подключиться ко всем автономным таблицам:

    ```hbase
    hbase hbck -ignorePreCheckPermission -fixAssignments
    ```

---

## <a name="scenario-javaioioexception-timedout"></a>Сценарий: Java. IO. IOException: TimedOut

### <a name="issue"></a>Проблема

Время ожидания HMaster истекает с неустранимым `java.io.IOException: Timedout 300000ms waiting for namespace table to be assigned`исключением, аналогичным:.

### <a name="cause"></a>Причина:

Такая проблема может возникнуть при наличии большого количества таблиц и регионов, которые не были удалены при перезагрузке служб HMaster. Время ожидания является известным дефектом HMaster. Общие задачи запуска кластера могут занять много времени. HMaster завершает работу, если таблица пространства имен еще не назначена. Длительные задачи запуска выполняются, когда существует большой объем неочищенных данных и не хватает времени ожидания 5 минут.

### <a name="resolution"></a>Разрешение

1. В пользовательском интерфейсе Apache Ambari перейдите в раздел**конфигурации** **HBase** > . В пользовательском `hbase-site.xml` файле добавьте следующий параметр:

    ```
    Key: hbase.master.namespace.init.timeout Value: 2400000  
    ```

1. Перезапустите необходимые службы (HMaster или другие службы HBase).

---

## <a name="scenario-frequent-region-server-restarts"></a>Сценарий: частый перезапуск сервера региона

### <a name="issue"></a>Проблема

Периодически перезапускаются узлы. В журналах сервера регионов могут отображаться записи следующего вида:

```
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
```

### <a name="cause"></a>Причина:

Длительная `regionserver` виртуальной машины Java GC Pause. Приостановка приведет к `regionserver` тому, что она не будет отвечать на запросы и не сможет отправить пульс HMaster в течение ZK сеанса ожидания 40s. HMaster полагает `regionserver` , что это не так, `regionserver` и прекратит перезапуститься.

### <a name="resolution"></a>Разрешение

Измените время ожидания сеанса Zookeeper, а не `hbase-site` только `zookeeper.session.timeout` настройку, но `zoo.cfg` и `maxSessionTimeout` параметр Zookeeper необходимо изменить.

1. Доступ к пользовательскому интерфейсу Ambari, перейдите в раздел **HBase-> configs-> Settings**(время ожидания) и измените значение времени ожидания сеанса Zookeeper.

1. Получите доступ к пользовательскому интерфейсу Ambari, перейдите в раздел **Zookeeper-> configs (> Custom** `zoo.cfg`), добавьте или измените следующий параметр. Убедитесь, что значение совпадает с HBase `zookeeper.session.timeout`.

    ```
    Key: maxSessionTimeout Value: 120000  
    ```

1. Перезапустите необходимые службы.

---

## <a name="scenario-log-splitting-failure"></a>Сценарий: ошибка разделения журнала

### <a name="issue"></a>Проблема

Хмастерс не удалось подключиться к кластеру HBase.

### <a name="cause"></a>Причина:

Неправильно настроены параметры HDFS и HBase для дополнительной учетной записи хранения.

### <a name="resolution"></a>Разрешение

Установите HBase. рутдир: wasb://@.blob.core.windows.net/hbase и перезапустите службы на Ambari.

---

## <a name="next-steps"></a>Дальнейшие действия

Если вы не видите своего варианта проблемы или вам не удается ее устранить, дополнительные сведения можно получить, посетив один из следующих каналов.

* Получите ответы от экспертов Azure через [службу поддержки сообщества Azure](https://azure.microsoft.com/support/community/).

* Подключение с [@AzureSupport](https://twitter.com/azuresupport) — официальная учетная запись Microsoft Azure для улучшения качества обслуживания клиентов. Подключение сообщества Azure к нужным ресурсам: ответы, поддержка и эксперты.

* Если вам нужна дополнительная помощь, можно отправить запрос в службу поддержки из [портал Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Выберите пункт **Поддержка** в строке меню или откройте центр **справки и поддержки** . Для получения более подробных сведений см. статью [о создании запроса на поддержку Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Доступ к управлению подписками и поддержкой выставления счетов включен в вашу подписку Microsoft Azure, а техническая поддержка предоставляется через один из [планов поддержки Azure](https://azure.microsoft.com/support/plans/).
