---
title: Решения службы хранилища Azure для Служб машинного обучения в HDInsight в Azure
description: Сведения о разных вариантах хранилищ, доступных для служб машинного обучения в HDInsight.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/27/2018
ms.openlocfilehash: cb0c350df3056636701b5ff5d3962e2a0e96f40d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64696354"
---
# <a name="azure-storage-solutions-for-ml-services-on-azure-hdinsight"></a>Решения службы хранилища Azure для служб машинного обучения в Azure HDInsight

Службы машинного обучения в HDInsight могут использовать множество решений для хранения данных, кода и объектов с результатами анализа. К ним относятся следующие решения:

- [Хранилище BLOB-объектов Azure](https://azure.microsoft.com/services/storage/blobs/)
- [Хранилище озера данных Azure](https://azure.microsoft.com/services/storage/data-lake-storage/)
- [Хранилище файлов Azure](https://azure.microsoft.com/services/storage/files/)

Кроме того, из кластера HDInsight вы можете обращаться к нескольким учетным записям хранения или контейнерам Azure. Хранилище файлов Azure — это удобный способ хранения данных на граничном узле, который позволяет подключить общую папку службы хранилища Azure, например, к файловой системе Linux. Общие папки Azure можно подключить и использовать на любом устройстве с поддерживаемой операционной системой, например Windows или Linux. 

При создании кластера Apache Hadoop в HDInsight нужно указать **учетную запись хранения Azure** или **Data Lake Storage**. Определенный контейнер хранилища в этой учетной записи используется для хранения файловой системы создаваемого кластера, например распределенной файловой системы Hadoop (HDFS). Дополнительные сведения и рекомендации см. в следующих статьях:

- [Использование службы хранилища Azure с HDInsight](../hdinsight-hadoop-use-blob-storage.md)
- [Использование Data Lake Store с кластерами Azure HDInsight](../hdinsight-hadoop-use-data-lake-store.md)

## <a name="use-azure-blob-storage-accounts-with-ml-services-cluster"></a>Использование учетных записей хранения BLOB-объектов Azure с кластером служб машинного обучения

Ниже объясняется, как получить доступ к данным и операциям кластера служб машинного обучения с помощью дополнительной учетной записи, если при создании кластера этих служб вы указали несколько учетных записей хранения. Предположим, что у вас есть две учетные записи хранения (**storage1** для контейнера по умолчанию **container1** и **storage2** для **container2**).

> [!WARNING]  
> Для повышения производительности кластер HDInsight создается в том же центре обработки данных, где находится заданная вами основная учетная запись хранения. Использование учетной записи хранения, расположение которой отличается от расположения кластера HDInsight, не поддерживается.

### <a name="use-the-default-storage-with-ml-services-on-hdinsight"></a>Использование хранилища по умолчанию для служб машинного обучения в HDInsight

1. С помощью клиента SSH подключитесь к граничному узлу кластера. Сведения об использовании протокола SSH с кластерами HDInsight см. в статье [Подключение к HDInsight (Hadoop) с помощью SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).
  
2. Скопируйте пример файла mysamplefile.csv в каталог /share. 

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal mycsv.scv /share  

3. Подключитесь к R Studio или другой консоли R, введите код R, чтобы задать узлу имени **значение по умолчанию** и расположение файла, к которому нужно получить доступ.  

        myNameNode <- "default"
        myPort <- 0

        #Location of the data:  
        bigDataDirRoot <- "/share"  

        #Define Spark compute context:
        mySparkCluster <- RxSpark(nameNode=myNameNode, consoleOutput=TRUE)

        #Set compute context:
        rxSetComputeContext(mySparkCluster)

        #Define the Hadoop Distributed File System (HDFS) file system:
        hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

        #Specify the input file to analyze in HDFS:
        inputFile <-file.path(bigDataDirRoot,"mysamplefile.csv")

Все ссылки на каталоги и файлы указывают на учетную запись хранения `wasb://container1@storage1.blob.core.windows.net`. Это **учетная запись хранения по умолчанию**, связанная с кластером HDInsight.

### <a name="use-the-additional-storage-with-ml-services-on-hdinsight"></a>Использование дополнительного хранилища для служб машинного обучения в HDInsight

Теперь мы рассмотрим ситуацию, в которой вам нужно обработать файл с именем mysamplefile1.csv, расположенный в каталоге /private контейнера **container2** в учетной записи хранения **storage2**.

В коде R измените ссылку на узел имени так, чтобы она указывала на учетную запись хранения **storage2** .

    myNameNode <- "wasb://container2@storage2.blob.core.windows.net"
    myPort <- 0

    #Location of the data:
    bigDataDirRoot <- "/private"

    #Define Spark compute context:
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    #Set compute context:
    rxSetComputeContext(mySparkCluster)

    #Define HDFS file system:
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    #Specify the input file to analyze in HDFS:
    inputFile <-file.path(bigDataDirRoot,"mysamplefile1.csv")

Теперь все ссылки на каталоги и файлы указывают на учетную запись хранения `wasb://container2@storage2.blob.core.windows.net`. Это указанный вами **узел имени**.

Вы должны настроить `/user/RevoShare/<SSH username>` каталог_установки **storage2** следующим образом:


    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>

## <a name="use-azure-data-lake-storage-with-ml-services-cluster"></a>Использование Azure Data Lake Storage в кластере служб машинного обучения 

Чтобы использовать Data Lake Storage в кластере HDInsight, следует предоставить кластеру доступ к каждому из хранилищ Azure Data Lake Storage, которые вы хотите использовать. Инструкции по созданию кластера HDInsight с учетной записью Azure Data Lake Storage в качестве хранилища по умолчанию или дополнительного хранилища на портале Azure см. в статье [Создание кластеров HDInsight, использующих Data Lake Storage, с помощью портала Azure](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

Затем вы можете использовать такое хранилище в скрипте R примерно так же, как дополнительную учетную запись хранения Azure, как описано в предыдущей процедуре.

### <a name="add-cluster-access-to-your-azure-data-lake-storage"></a>Предоставление кластеру доступа к Azure Data Lake Storage
Доступ к Data Lake Storage осуществляется с помощью субъекта-службы Azure Active Directory (Azure AD), связанного с кластером HDInsight.

1. При создании кластера HDInsight выберите на вкладке **Источник данных** вариант **Удостоверение кластера AAD**.

2. В диалоговом окне **Удостоверение кластера AAD** в разделе **Выбрать субъект-службу AD** выберите **Создать**.

Присвоив имя субъекту-службе и создав для него пароль, щелкните **Управление доступом ADLS**, чтобы связать субъект-службу с Data Lake Storage.

Вы также можете добавить доступ кластера к одной учетной записи Data Lake Storage или нескольким после создания кластера. На портале Azure откройте запись Data Lake Storage и выберите пункты **Обозреватель данных > Доступ > Добавить**. 

### <a name="how-to-access-data-lake-storage-gen1-from-ml-services-on-hdinsight"></a>Доступ к Data Lake Storage 1-го поколения из служб машинного обучения в HDInsight

Когда вы получите доступ к Data Lake Storage 1-го поколения, вы сможете применить это хранилище для кластера Служб машинного обучения в HDInsight так же, как дополнительную учетную запись хранения Azure. Единственное отличие состоит в том, что вместо префикса **wasb://** используется **adl://** .


    # Point to the ADL Storage (e.g. ADLtest)
    myNameNode <- "adl://rkadl1.azuredatalakestore.net"
    myPort <- 0

    # Location of the data (assumes a /share directory on the ADL account)
    bigDataDirRoot <- "/share"  

    # Define Spark compute context
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    # Set compute context
    rxSetComputeContext(mySparkCluster)

    # Define HDFS file system
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    # Specify the input file in HDFS to analyze
    inputFile <-file.path(bigDataDirRoot,"mysamplefile.csv")

Приведенные ниже команды позволяют настроить учетную запись хранилища Data Lake Storage 1-го поколения c каталогом RevoShare и добавить образец CSV-файла из предыдущего примера.


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/mysamplefile.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-ml-services-on-hdinsight"></a>Использование хранилища файлов Azure для служб машинного обучения в HDInsight

Для граничных узлов есть еще один удобный вариант хранения — [служба файлов Azure](https://azure.microsoft.com/services/storage/files/). Они позволяют подключить общую папку службы хранения Azure к файловой системе Linux. Этот вариант может быть удобен для хранения файлов данных, скриптов R и объектов результатов, которые могут понадобиться позже, особенно если на граничном узле есть смысл использовать собственную файловую систему, а не HDFS. 

Основное преимущество файлов Azure заключается в том, что общие папки можно подключить и использовать на любом устройстве с поддерживаемой операционной системой, например Windows или Linux. Например, их можно использовать в другом кластере HDInsight, принадлежащем вам или коллеге, в виртуальной машине Azure, или даже в локальной системе. Дополнительные сведения можно найти в разделе

- [Использование хранилища файлов Azure в Linux](../../storage/files/storage-how-to-use-files-linux.md)
- [Использование хранилища файлов Azure в Windows](../../storage/files/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a>Дальнейшие действия

* [Основные сведения о кластере служб машинного обучения в HDInsight](r-server-overview.md)
* [Начало работы со службами машинного обучения в Azure HDInsight](r-server-get-started.md)
* [Варианты контекста вычислений для кластера служб машинного обучения в HDInsight](r-server-compute-contexts.md)
* [Использование Azure Data Lake Storage Gen2 с кластерами Azure HDInsight](../hdinsight-hadoop-use-data-lake-storage-gen2.md)
