---
title: Использование Apache Hive с PowerShell в HDInsight — Azure
description: Использование PowerShell для выполнения Apache Hive запросов в Apache Hadoop в Azure HDInsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: hrasheed
ms.openlocfilehash: 89fa7976b922ba0e40e97b72de5d4eb9a02f0dfd
ms.sourcegitcommit: 97605f3e7ff9b6f74e81f327edd19aefe79135d2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70736070"
---
# <a name="run-apache-hive-queries-using-powershell"></a>Выполнение запросов Apache Hive с помощью PowerShell
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

В этом документе приведен пример использования Azure PowerShell в режиме группы ресурсов Azure для выполнения запросов Hive в Apache Hadoop в кластере HDInsight.

> [!NOTE]  
> В этом документе не приводится подробное описание процессов, которые выполняют инструкции HiveQL, используемые в примерах. Информацию об операторах HiveQL, используемых в этом примере, см. в статье [Обзор Apache Hive и HiveQL в Azure HDInsight](hdinsight-use-hive.md).

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

* Apache Hadoop на базе Linux в кластере HDInsight версии 3.4 или более поздней.

* Клиент с Azure PowerShell.

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-a-hive-query"></a>Выполнение запроса Hive

Azure PowerShell предоставляет *командлеты* , позволяющие удаленно запускать запросы Hive в HDInsight. Во внутренних командлет использует вызовы REST к [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) в кластере HDInsight.

При выполнении запросов Hive на удаленном кластере HDInsight используются следующие командлеты:

* `Connect-AzAccount`. выполняет аутентификацию Azure PowerShell для подписки Azure.
* `New-AzHDInsightHiveJobDefinition`. создает *определение задания* с использованием заданных операторов HiveQL.
* `Start-AzHDInsightJob`. отправляет определение задания в HDInsight и запускает задание. Будет возвращен объект *задания*.
* `Wait-AzHDInsightJob`. проверяет состояние задания с помощью объекта-задания. Он ожидает завершения задания или превышения времени ожидания.
* `Get-AzHDInsightJobOutput`. используется для получения выходных данных задания.
* `Invoke-AzHDInsightHiveJob`. используется для выполнения операторов HiveQL. Этот командлет блокирует выполнение запроса, после чего возвращаются результаты.
* `Use-AzHDInsightCluster`. указывает, что при выполнении команды `Invoke-AzHDInsightHiveJob` должен использоваться текущий кластер.

Следующие шаги показывают, как использовать эти командлеты для выполнения задания в кластере HDInsight:

1. С помощью редактора сохраните следующий код как `hivejob.ps1`.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]

2. Откройте новую командную строку **Azure PowerShell** . Перейдите к расположению файла `hivejob.ps1`, а затем используйте следующую команду для запуска скрипта:

        .\hivejob.ps1

    При запуске скрипта будет предложено ввести имя кластера, данные HTTPS и учетной записи администратора кластера. Вам также может быть предложено войти в подписку Azure.

3. Когда задание завершается, возвращается текст следующего содержания:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Как уже упоминалось, `Invoke-Hive` можно использовать для отправки запроса и ожидания ответа. Чтобы увидеть работу команды Invoke-Hive, используйте следующий сценарий:

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]

    Выходные данные выглядят так:

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]  
   > Для более длинных запросов HiveQL вы можете использовать командлет Azure PowerShell **Here-Strings** или файлы сценариев HiveQL. В приведенном ниже отрывке кода показано, как использовать командлет `Invoke-Hive` для запуска файла скрипта HiveQL. Файл скрипта HiveQL необходимо передать в wasb://.
   >
   > `Invoke-AzHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > Дополнительные сведения о командлете **Here-Strings** см. <a href="https://technet.microsoft.com/library/ee692792.aspx" target="_blank">здесь</a>.

## <a name="troubleshooting"></a>Устранение неполадок

Если данные не возвращаются при завершении задания, просмотрите журналы ошибок. Чтобы просмотреть информацию об ошибке для данного задания, добавьте следующий текст в конце файла `hivejob.ps1`, сохраните его, а затем запустите снова.

```powershell
# Print the output of the Hive job.
Get-AzHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Этот командлет возвращает информацию, которая записывается в STDERR на сервере при обработке задания.

## <a name="summary"></a>Сводка

Как можно видеть, Azure PowerShell позволяет с легкостью выполнять запросы Hive в кластере HDInsight, отслеживать состояние задания и получать выходные данные.

## <a name="next-steps"></a>Следующие шаги

Общая информация о Hive в HDInsight:

* [Использование Apache Hive с Apache Hadoop в HDInsight](hdinsight-use-hive.md)

Дополнительная информация о других способах работы с Hadoop в HDInsight:

* [Использование Apache Pig с Apache Hadoop в HDInsight](hdinsight-use-pig.md)
* [Использование MapReduce в Apache Hadoop в HDInsight](hdinsight-use-mapreduce.md)
