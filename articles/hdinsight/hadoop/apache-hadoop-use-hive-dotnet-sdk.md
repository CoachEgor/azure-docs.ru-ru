---
title: Выполнение запросов Apache Hive с использованием пакета SDK .NET для HDInsight — Azure
description: Узнайте, как отправлять задания Apache Hadoop в Azure HDInsight с использованием пакета SDK .NET для HDInsight.
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: 947e797842000e4da2f9e22077bc32c24d6c6a74
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64718842"
---
# <a name="run-apache-hive-queries-using-hdinsight-net-sdk"></a>Выполнение запросов Apache Hive с использованием пакета SDK .NET для HDInsight
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Узнайте, как отправлять запросы Apache Hive с использованием пакета SDK .NET для HDInsight. Напишите программу на языке C#, чтобы отправить запрос Hive для перечисления таблиц Hive и отобразить результаты.

> [!NOTE]  
> Действия, описанные в этой статье, необходимо выполнять из клиента Windows. Чтобы получить сведения об использовании клиента Linux, OS X или Unix для работы с Hive, воспользуйтесь выбором вкладок в верхней части статьи.

## <a name="prerequisites"></a>Технические условия
Перед началом работы с этой статьей необходимо иметь следующее:

* **Кластер Apache Hadoop в HDInsight**. См. статью [Руководство по Hadoop. Начало работы с Hadoop в HDInsight на платформе Linux](apache-hadoop-linux-tutorial-get-started.md).

    > [!WARNING]  
    > Начиная с 15 сентября 2017 г. пакет SDK для HDInsight .NET поддерживает возвращение результатов запроса Hive только из учетных записей хранения Azure. Если использовать этот пример с кластером HDInsight, в котором Azure Data Lake Storage применяется в качестве основного хранилища, вы не сможете получить результаты поиска с помощью пакета SDK для .NET.

* **Visual Studio 2013, Visual Studio 2015, Visual Studio 2017**.

## <a name="run-a-hive-query"></a>Выполнение запроса Hive
Пакет SDK для HDInsight .NET содержит клиентские библиотеки .NET, которые упрощают работу с кластерами HDInsight из .NET. 

**Отправка заданий**

1. Создайте в Visual Studio консольное приложение C#.
2. Введите следующую команду в окне консоли диспетчера пакетов NuGet:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. Используйте следующий код:

    ```csharp
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
   
        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";
                
                // Only Azure Storage accounts are supported by the SDK
                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitHiveJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "tez" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };
   
                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);
   
                    System.Console.WriteLine("Waiting for the job completion ...");
   
                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }
   
                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure
   
                    System.Console.WriteLine("Job output is: ");
   
                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }
    ```
4. Нажмите клавишу **F5** для запуска приложения.

Выходные данные приложения должны иметь следующий вид:

![Выходные данные задания Hadoop Hive в HDInsight](./media/apache-hadoop-use-hive-dotnet-sdk/hdinsight-hadoop-use-hive-net-sdk-output.png)

## <a name="next-steps"></a>Дальнейшие действия
В этой статье вы ознакомились с несколькими способами создания кластера HDInsight. Для получения дополнительных сведений ознакомьтесь со следующими статьями:

* [Приступая к работе с Azure HDInsight](apache-hadoop-linux-tutorial-get-started.md)
* [Создание кластеров Apache Hadoop в HDInsight](../hdinsight-hadoop-provision-linux-clusters.md)
* [Справочник по пакетам SDK HDInsight для .NET](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight)
* [Использование Pig с Hadoop в HDInsight](hdinsight-use-pig.md)
* [Использование Apache Sqoop с HDInsight](apache-hadoop-use-sqoop-mac-linux.md)
* [Создание приложений .NET HDInsight с неинтерактивной проверкой подлинности](../hdinsight-create-non-interactive-authentication-dotnet-applications.md)
 


