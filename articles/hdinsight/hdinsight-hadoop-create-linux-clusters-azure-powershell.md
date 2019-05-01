---
title: Создание кластеров Apache Hadoop с помощью PowerShell в Azure HDInsight
description: Узнайте, как создавать кластеры Apache Hadoop, Apache HBase, Apache Storm или Apache Spark на платформе Linux в HDInsight с помощью Azure PowerShell.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: hrasheed
ms.openlocfilehash: 757c3ef55e93397030b0fe847dc19d81a7bbe568
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64713820"
---
# <a name="create-linux-based-clusters-in-hdinsight-using-azure-powershell"></a>Создание кластеров под управлением Linux в HDInsight с помощью Azure PowerShell

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure PowerShell — это полнофункциональная среда сценариев, которую можно использовать для контроля и автоматизации развертывания и управления вашей рабочей нагрузкой в Microsoft Azure. В этой статье содержится информация о том, как создать кластер HDInsight под управлением Linux с помощью Azure PowerShell, а также приведен пример скрипта.

> [!NOTE]  
> Оболочка Azure PowerShell доступна только для клиентов Windows. Если вы используете клиент Linux, Unix или Mac OS X, сведения об использовании классического интерфейса командной строки Azure для создания кластера см. в статье [Создание кластеров HDInsight с помощью интерфейса командной строки Azure](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

## <a name="prerequisites"></a>Технические условия

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Прежде чем следовать инструкциям в этой статье, необходимо подготовить следующее.

* Подписка Azure. См. страницу [бесплатной пробной версии Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Azure PowerShell](/powershell/azure/install-Az-ps)

    > [!IMPORTANT]  
    > Поддержка Azure PowerShell для управления ресурсами HDInsight с помощью диспетчера служб Azure (ASM) объявлена **устаревшей** и будет прекращена с 1 января 2017 г. В описанных в этом документе инструкциях используются новые командлеты HDInsight, которые работают с Azure Resource Manager.
    >
    > Чтобы установить последнюю версию Azure PowerShell, выполните действия из статьи [Установка и настройка Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-Az-ps). Если у вас есть сценарии, в которые нужно добавить новые командлеты, работающие с Azure Resource Manager, см. статью [Переход к средствам разработки на основе Azure Resource Manager для кластеров HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md).

## <a name="create-cluster"></a>Создание кластера

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Для создания кластера HDInsight с помощью Azure PowerShell необходимо выполнить следующие процедуры:

* создание группы ресурсов Azure;
* Создание учетной записи хранения Azure
* Создание контейнера BLOB-объектов Azure
* Создание кластера HDInsight

Следующий сценарий демонстрирует создание нового кластера.

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster.ps1?range=5-71)]

Значения, указываемые для входа в кластер, используются для создания учетной записи пользователя Hadoop в кластере. Используйте эту учетную запись для подключения к службам, размещенным в кластере, например веб-интерфейсам пользователя или REST API.

Значения, указываемые для пользователя SSH, используются для создания пользователя SSH в кластере. Используйте эту учетную запись для запуска удаленного сеанса SSH в кластере и выполнения заданий. Дополнительные сведения см. в статье [Подключение к HDInsight (Hadoop) с помощью SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

> [!IMPORTANT]  
> Если вы планируете использовать более 32 узлов рабочей роли (при создании кластера или в ходе масштабирования после создания кластера), для головного узла потребуется минимум 8-ядерный процессор и 14 ГБ ОЗУ.
>
> Дополнительные сведения о размерах узлов и их стоимости см. на странице с [ценами на HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

Операция создания кластера может занять до 20 минут.

## <a name="create-cluster-configuration-object"></a>Создание кластера. Объект конфигурации

Можно также создать объект конфигурации HDInsight с помощью командлета `New-AzHDInsightClusterConfig`. Затем можно изменить этот объект конфигурации, чтобы включить дополнительные параметры конфигурации для кластера. Наконец, используйте параметр `-Config` командлета `New-AzHDInsightCluster`, чтобы использовать эту конфигурацию.

Приведенный ниже сценарий создает объект конфигурации для настройки R Server для типа кластера HDInsight. Конфигурация включает граничный узел, RStudio и дополнительную учетную запись хранения.

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster-with-config.ps1?range=59-99)]

> [!WARNING]  
> Использование учетной записи хранения, расположение которой отличается от расположения кластера HDInsight, не поддерживается. При использовании этого примера создайте дополнительную учетную запись хранения в том же расположении, что и сервер.

## <a name="customize-clusters"></a>Настройка кластеров

* Ознакомьтесь с разделом [Настройка кластеров HDInsight с помощью начальной загрузки](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
* См. статью [Настройка кластеров HDInsight под управлением Linux с помощью действий сценариев](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>Удаление кластера

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Устранение неполадок

Если при создании кластеров HDInsight возникли проблемы, см. раздел [Создание кластеров](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы успешно создали кластер HDInsight, обратитесь к следующим ресурсам, чтобы научиться с ним работать.

### <a name="apache-hadoop-clusters"></a>Кластеры Apache Hadoop

* [Использование Hive и HiveQL с Hadoop в HDInsight для анализа примера файла Apache log4j](hadoop/hdinsight-use-hive.md)
* [Использование Pig с Hadoop в HDInsight](hadoop/hdinsight-use-pig.md)
* [Использование MapReduce с HDInsight](hadoop/hdinsight-use-mapreduce.md)

### <a name="apache-hbase-clusters"></a>Кластеры Apache HBase

* [Начало работы с Apache HBase в HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md)
* [Разработка приложений Java для Apache HBase в HDInsight](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Кластеры Storm

* [Разработка приложений Java для Storm в HDInsight](storm/apache-storm-develop-java-topology.md)
* [Использование компонентов Python в Storm в HDInsight](storm/apache-storm-develop-python-topology.md)
* [Развертывание и мониторинг топологий с помощью Storm в HDInsight](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="apache-spark-clusters"></a>Кластеры Apache Spark

* [Создание автономного приложения с использованием Scala](spark/apache-spark-create-standalone-application.md)
* [Удаленный запуск заданий с помощью Apache Livy в кластере Apache Spark](spark/apache-spark-livy-rest-interface.md)
* [Руководство. Анализ данных Apache Spark с использованием Power BI в HDInsight](spark/apache-spark-use-bi-tools.md)
* [Использование Apache Spark MLlib для создания приложения машинного обучения и анализа набора данных](spark/apache-spark-machine-learning-mllib-ipython.md)

