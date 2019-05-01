---
title: Использование Hadoop в HDInsight на компьютере с Windows в Azure
description: Сведения об использовании Hadoop в HDInsight на компьютере с Windows, а также о запросе кластеров и управлении ими с помощью PowerShell, Visual Studio и средств Linux. Разработка решений для работы с большими данными на языке .NET.
author: hrasheed-msft
ms.author: hrasheed
ms.topic: conceptual
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 04/24/2019
ms.openlocfilehash: 5045c48a00c51a16d37dcf4b7f72f25633f23b3f
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2019
ms.locfileid: "64926027"
---
# <a name="work-in-the-apache-hadoop-ecosystem-on-hdinsight-from-a-windows-pc"></a>Работа в экосистеме Apache Hadoop в HDInsight на компьютере с Windows

В этой статье приведены сведения о способах развертывания экосистемы Apache Hadoop в HDInsight и управления ею при работе на компьютере с Windows. 

Служба HDInsight включает в себя компоненты Apache Hadoop и Hadoop, технологии с открытым исходным кодом, разработанные на платформе Linux. HDInsight 3.4 и более поздних версий использует дистрибутив Ubuntu Linux в качестве базовой операционной системы кластера. Но с HDInsight можно также работать в клиенте или среде разработки Windows.

## <a name="use-powershell-for-deployment-and-management-tasks"></a>Задачи развертывания и управления с помощью PowerShell
Azure PowerShell — это среда сценариев, которая позволяет контролировать и автоматизировать задачи развертывания и управления в HDInsight на компьютере с Windows.

При помощи PowerShell можно выполнять такие задачи:

* [создание кластеров](hdinsight-hadoop-create-linux-clusters-azure-powershell.md);
* [выполнение запросов Apache Hive](hadoop/apache-hadoop-use-hive-powershell.md);
* [управление кластерами](hdinsight-administer-use-powershell.md).

Указания по установке и настройке последней версии Azure PowerShell см. [здесь](https://docs.microsoft.com/powershell/azure/install-az-ps).

## <a name="utilities-you-can-run-in-a-browser"></a>Браузерные служебные программы
Ниже приведены примеры служебных программ с веб-интерфейсом, выполняемым в браузере.
* **[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)**  — это интерактивная оболочка командной строки, работающей в браузере, а также из портала Azure.
* **[Веб-интерфейс Apache Ambari](hdinsight-hadoop-manage-ambari.md)**  — служебная программа для управления и мониторинга, доступная на портале Azure. Она позволяет управлять разными видами заданий, например:
    * [интеграция Apache Ambari с REST API;](hdinsight-hadoop-manage-ambari-rest-api.md)
    * [представление Apache Hive в Apache Ambari;](hadoop/apache-hadoop-use-hive-ambari-view.md)
    * [представление Apache Tez в Apache Ambari.](hdinsight-debug-ambari-tez-view.md)

## <a name="data-lake-hadoop-tools-for-visual-studio"></a>Средства Data Lake (Hadoop) для Visual Studio
Средства Data Lake для Visual Studio позволяют развертывать топологии Storm и управлять ими. Эти средства также устанавливают пакет SDK для SCP.NET, который позволяет разрабатывать топологии Storm на языке C# с помощью Visual Studio.

Перед выполнением приведенных ниже примеров [установите средства Data Lake для Visual Studio и попробуйте работать с ними](hadoop/apache-hadoop-visual-studio-tools-get-started.md). 

При помощи средств Data Lake и Visual Studio можно выполнять следующие задачи:
* [Развертывать топологии Storm и управлять ими](storm/apache-storm-deploy-monitor-topology-linux.md).
* [Разрабатывать топологии Storm на языке C#](storm/apache-storm-develop-csharp-visual-studio-topology.md). Биты содержат примеры шаблонов топологий Storm, которые можно подключать к базам данных, таким как Azure Cosmos DB и база данных SQL.

## <a name="visual-studio-and-the-net-sdk"></a>Visual Studio и пакет SDK для .NET 

Visual Studio с пакетом SDK для .NET позволяет управлять кластерами и разрабатывать приложения больших данных. Ниже приведены примеры задач, которые можно выполнять в Visual Studio. Но их также можно выполнять и с помощью других интерфейсов IDE.

При помощи пакета SDK для .NET в Visual Studio можно выполнять следующие задачи:
* [создавать кластеры и работать в HDInsight из приложения .NET Framework](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md);
* [выполнять запросы Apache Hive с помощью пакета SDK для .NET](hadoop/apache-hadoop-use-hive-dotnet-sdk.md);
* [использовать определяемые пользователем функций C# при потоковой передаче Apache Hive и Apache Pig в Apache Hadoop](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md).

## <a name="intellij-idea-and-eclipse-ide-for-spark-clusters"></a>Intellij IDEA и Eclipse IDE для кластеров Spark
При помощи [Intellij IDEA](https://www.jetbrains.com/idea/download) и [Eclipse IDE](https://www.eclipse.org/downloads/) можно выполнять следующие задачи:
* разрабатывать и отправлять приложения Scala Spark в кластер HDInsight Spark;
* получать доступ к ресурсам кластера Spark;
* разрабатывать и запускать приложения Scala Spark в локальной среде.

В этих статьях описывается: 
* IntelliJ IDEA: [Создание приложений Apache Spark для кластера HDInsight с помощью Azure Toolkit for IntelliJ](spark/apache-spark-intellij-tool-plugin.md).
* Интегрированная среда разработки Eclipse или интегрированная среда разработки Scala для Eclipse: [Создание приложений Apache Spark для кластера HDInsight с помощью Azure Toolkit for Eclipse](spark/apache-spark-eclipse-tool-plugin.md) 


## <a name="notebooks-on-spark-for-data-scientists"></a>Записные книжки в Spark для специалистов по обработке и анализу данных 
Кластеры Apache Spark в HDInsight включают записные книжки Apache Zeppelin и ядра, которые можно использовать с записными книжками Jupyter. 

* [Узнайте, как использовать ядра в кластерах Apache Spark с записными книжками Jupyter для тестирования приложений Spark](spark/apache-spark-zeppelin-notebook.md)
* [Узнайте, как использовать записные книжки Apache Zeppelin в кластерах Apache Spark для выполнения заданий Spark](spark/apache-spark-jupyter-notebook-kernels.md) 

## <a name="run-linux-based-tools-and-technologies-on-windows"></a>Запуск средств и технологии Linux в Windows

Если возникнет ситуация, когда необходимо использовать средства или технологии, доступные только в Linux, рассмотрите следующие варианты:

* **Bash на Ubuntu в Windows 10** предоставляет подсистему Linux на Windows. Это средство позволяет напрямую запускать служебные программы Linux, не устанавливая выделенный сервер Linux. См. в разделе [подсистемы Windows для руководство по установке Linux для Windows 10](https://docs.microsoft.com/windows/wsl/install-win10) для действия по установке.  Другие [оболочек Unix](https://www.gnu.org/software/bash/) также будут работать.
* **Docker для Windows** предоставляет доступ ко многим средствам Linux. Эту платформу можно запустить непосредственно из Windows. Например, с помощью Docker можно запустить клиент Beeline для Hive непосредственно из Windows. Кроме того, Docker позволяет запускать локальные записные книжки Jupyter и удаленно подключаться к Spark в HDInsight. [Начните работу с Docker для Windows.](https://docs.docker.com/docker-for-windows/)
* **[MobaXTerm](https://mobaxterm.mobatek.net/)** позволяет графически просматривать файловую систему кластера через SSH-подключение.

## <a name="cross-platform-tools"></a>Кросс платформенные средства

Azure CLI — это кроссплатформенный интерфейс командной строки от Майкрософт для управления ресурсами Azure.  Дополнительные сведения см. в разделе [интерфейса командной строки Azure (CLI)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

## <a name="next-steps"></a>Дальнейшие действия
Если вы только начали работать с кластерами под управлением Linux, см. сведения в следующих статьях:
* [Установка кластеров в HDInsight с использованием Hadoop, Spark, Kafka и других технологий](hdinsight-hadoop-provision-linux-clusters.md)
* [Сведения об использовании HDInsight в Linux](hdinsight-hadoop-linux-information.md)
