---
title: Установка кластеров для Apache Hadoop, Apache Spark, Apache Kafka, Apache HBase или Microsoft R Server — Azure HDInsight
description: Сведения об установке кластеров Hadoop, Kafka, Spark, HBase, Microsoft R Server или Storm для HDInsight из браузера с помощью классического Azure CLI, Azure PowerShell, REST или пакета SDK.
keywords: установка кластера hadoop, установка кластера kafka, установка кластера spark, что такое кластер hadoop
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017,seodec18
ms.topic: conceptual
ms.date: 01/28/2019
ms.openlocfilehash: 3ccff4409dde92a23ccdbb1c31af687449bf9d48
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/24/2019
ms.locfileid: "63766363"
---
# <a name="set-up-clusters-in-hdinsight-with-apache-hadoop-apache-spark-apache-kafka-and-more"></a>Установка кластеров в HDInsight с использованием Apache Hadoop, Apache Spark, Apache Kafka и других технологий

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Узнайте, как установить и настроить кластеры в HDInsight с использованием Apache Hadoop, Apache Spark, Apache Kafka, Interactive Query, Apache HBase, Служб машинного обучения или Apache Storm. Кроме того, вы узнаете, как повысить безопасность кластеров путем их присоединения к домену.

Кластер Hadoop включает в себя несколько виртуальных машин (узлов), которые используются для распределенной обработки задач. Azure HDInsight управляет сведениями об установке и настройке отдельных узлов, поэтому вам нужно указать только общие сведения о конфигурации. 

> [!IMPORTANT]  
> Начисление оплаты начинается после создания кластера HDInsight и прекращается только после его удаления. Кластеры оплачиваются поминутно, поэтому всегда следует удалять кластер, когда он больше не нужен. Узнайте, как [удалить кластер](hdinsight-delete-cluster.md).

## <a name="cluster-setup-methods"></a>Способы установки кластера
В приведенной ниже таблице представлены различные способы установки кластера HDInsight.

| Метод создания кластеров | браузер | Командная строка | REST API | SDK | 
| --- |:---:|:---:|:---:|:---:|
| [портал Azure](hdinsight-hadoop-create-linux-clusters-portal.md) |✔ |&nbsp; |&nbsp; |&nbsp; |
| [Фабрика данных Azure](hdinsight-hadoop-create-linux-clusters-adf.md) |✔ |✔ |✔ |✔ |
| [Классический Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [cURL](hdinsight-hadoop-create-linux-clusters-curl-rest.md) |&nbsp; |✔ |✔ |&nbsp; |
| [Пакет SDK для .NET](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |&nbsp; |&nbsp; |&nbsp; |✔ |
| [Шаблоны диспетчера ресурсов Azure](hdinsight-hadoop-create-linux-clusters-arm-templates.md) |&nbsp; |✔ |&nbsp; |&nbsp; |

## <a name="quick-create-basic-cluster-setup"></a>Быстрое создание: установка базового кластера
В этой статье приводятся пошаговые инструкции по выполнению установки на [портале Azure](https://portal.azure.com), где можно создать кластер HDInsight, выбрав *Быстрое создание* или *Пользовательский*. 

![hdinsight create options custom quick create](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-creation-options.png)

Чтобы установить базовый кластер, следуйте инструкциям на экране. Ниже приведены сведения для следующих элементов:

* [Имя группы ресурсов](#resource-group-name)
* [Типы и конфигурация кластеров](#cluster-types) 
* Имя входа в кластер и имя пользователя SSH
* [Местоположение.](#location)

> [!IMPORTANT]  
> Linux — это единственная операционная система, используемая для работы с HDInsight 3.4 или более поздних версий. Дополнительные сведения см. в разделе о [прекращении поддержки HDInsight 3.3](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>

## <a name="resource-group-name"></a>Имя группы ресурсов 

[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) позволяет работать с ресурсами в приложении в виде группы, которая называется группой ресурсов Azure. Вы можете развертывать, обновлять, отслеживать или удалять все ресурсы для приложения в рамках одной скоординированной операции.

## <a name="cluster-types"></a> Типы и конфигурация кластеров
В настоящее время Azure HDInsight предоставляет указанные ниже типы кластеров, каждый из которых имеет набор компонентов для предоставления определенных функциональных возможностей.

> [!IMPORTANT]  
> Доступны различные типы кластеров HDInsight, каждый из которых предназначен для отдельной рабочей нагрузки или технологии. Создать кластер, в котором бы объединились несколько типов, например Storm и HBase, нельзя. Если для решения нужны технологии, распределенные по нескольким типам кластеров HDInsight, [виртуальная сеть Azure](https://docs.microsoft.com/azure/virtual-network) может объединять необходимые типы кластеров. 

| Тип кластера | Функции |
| --- | --- |
| [Hadoop](hadoop/apache-hadoop-introduction.md) |Пакетный запрос и анализ хранимых данных |
| [HBase](hbase/apache-hbase-overview.md) |Обработка больших объемов данных NoSQL без схемы |
| [Интерактивный запрос](./interactive-query/apache-interactive-query-get-started.md) |Кэширование в памяти для обеспечения интерактивных и ускоренных запросов Hive |
| [Kafka](kafka/apache-kafka-introduction.md) | Распределенная платформа потоковой передачи с открытым кодом, которую можно использовать для создания конвейеров и приложений потоковой передачи данных в режиме реального времени. |
| [Службы машинного обучения](r-server/r-server-overview.md) |Разнообразная статистика больших данных, прогнозное моделирование и возможности машинного обучения |
| [Spark](spark/apache-spark-overview.md) |Обработка в памяти, интерактивные запросы, обработка потоков микро-пакетов |
| [Storm](storm/apache-storm-overview.md) |Обработка событий в режиме реального времени |


### <a name="hdinsight-version"></a>Версия HDInsight
Выберите версию HDInsight для этого кластера. Дополнительные сведения см. в разделе [Поддерживаемые версии HDInsight](hdinsight-component-versioning.md#supported-hdinsight-versions).


## <a name="cluster-login-and-ssh-username"></a>Имя входа в кластер и имя пользователя SSH
Во время создания кластера HDInsight можно настроить две учетные записи пользователя.

* Пользователь HTTP: Имя пользователя по умолчанию — *admin*. Эта учетная запись использует базовую конфигурацию на портале Azure. Иногда его называют "пользователем кластера".
* Пользователь SSH: это имя пользователя используется для подключения к кластеру через SSH. Дополнительные сведения см. в статье [Использование SSH с Hadoop на основе Linux в HDInsight из Linux, Unix или OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

Пакет безопасности предприятия позволяет интегрировать HDInsight с Active Directory и Apache Ranger. При помощи пакета безопасности корпоративного уровня можно создать нескольких пользователей.

## <a name="location"></a>Расположение (регионы) для кластеров и хранилища

Расположение кластера не обязательно указывать явным образом: кластер использует то же расположение, что и хранилище по умолчанию. Просмотреть поддерживаемые регионы можно в раскрывающемся списке **Регион** на странице [Цены на HDInsight](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

## <a name="storage-endpoints-for-clusters"></a>Конечные точки хранилища для кластеров

Хотя при локальной установке кластера Hadoop для хранилища используется распределенная файловая система Hadoop (HDFS), в облаке применяются конечные точки хранилища, подключенные к кластеру. Для кластеров HDInsight используется [Azure Data Lake Storage](hdinsight-hadoop-use-data-lake-store.md) или [BLOB-объекты в службе хранилища Azure](hdinsight-hadoop-use-blob-storage.md). Использование службы хранилища Azure или Data Lake Storage означает, что вы можете спокойно удалять кластеры HDInsight, используемые для вычислений, не теряя данные. 

> [!WARNING]  
> Использование дополнительной учетной записи хранения, местоположение которой отличается от местоположения кластера HDInsight, не поддерживается.

Во время настройки вы указываете контейнер BLOB-объектов в учетной записи хранения Azure или хранилище Data Lake Storage для конечной точки хранилища по умолчанию. Хранилище по умолчанию содержит журналы приложений и системный журнал. При необходимости вы можете указать дополнительные связанные учетные записи хранения Azure и учетные записи Data Lake Storage, к которым кластер может получать доступ. Кластер HDInsight должен находиться в том же расположении Azure, что и зависимые учетные записи хранения.

![Параметры системы хранения данных кластера: конечные точки хранилища, совместимого с HDFS](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-creation-storage.png)

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


### <a name="optional-metastores"></a>Дополнительные хранилища метаданных
Вы можете создать дополнительные хранилища метаданных Hive или Apache Oozie. Однако не все типы кластеров поддерживают хранилища метаданных, а хранилище данных SQL Azure не совместимо с хранилищами метаданных. 

Дополнительную информацию см. в статье [Использование внешних хранилищ метаданных в Azure HDInsight](./hdinsight-use-external-metadata-stores.md).

> [!IMPORTANT]  
> Создавая пользовательское хранилище метаданных, не используйте в имени базы данных тире, дефисы и пробелы. Это может привести к сбою при создании кластера.

### <a name="use-hiveoozie-metastore"></a>Хранилище метаданных Hive

Если вы хотите сохранить таблицы Hive после удаления кластера HDInsight, используйте пользовательское хранилище метаданных. Затем можно будет подключить это хранилище к другому кластеру HDInsight.

Хранилище метаданных HDInsight, созданное для одной версии кластера HDInsight, не может использоваться в других версиях такого кластера. Список версий HDInsight см. в разделе [Поддерживаемые версии HDInsight](hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="oozie-metastore"></a>Хранилище метаданных Oozie

Для повышения производительности Oozie используйте пользовательское хранилище метаданных. Хранилище метаданных также позволяет осуществлять доступ к данным задания Oozie после удаления кластера. 

> [!IMPORTANT]  
> Повторно использовать хранилище метаданных Oozie невозможно. Чтобы использовать пользовательское хранилище метаданных Oozie, при создании кластера HDInsight необходимо предоставить пустую базу данных SQL Azure.


## <a name="custom-cluster-setup"></a>Пользовательская установка кластера
Пользовательская установка кластера основана на параметрах быстрого создания, к которым добавляются следующие параметры:
- [Корпоративный пакет безопасности](#enterprise-security-package)
- [Приложения HDInsight](#install-hdinsight-applications-on-clusters)
- [Размер кластера](#configure-cluster-size)
- [Действия скриптов](#advanced-settings-script-actions)
- [Виртуальная сеть](#advanced-settings-extend-clusters-with-a-virtual-network)
 
## <a name="enterprise-security-package"></a>Пакет безопасности корпоративного уровня

Для кластеров типа Hadoop, Spark, HBase, Kafka и Interactive Query вы можете включить **Корпоративный пакет безопасности**. Этот пакет содержит параметр для более безопасной настройки кластера с помощью Apache Ranger и интеграции с Azure Active Directory. Дополнительные сведения см. в статье о [корпоративном пакете безопасности в Azure HDInsight](./domain-joined/apache-domain-joined-introduction.md).

![hdinsight create options choose enterprise security package](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-creation-enterprise-security-package.png)

Дополнительные сведения о создании присоединенного к домену кластера HDInsight см. в статье о [создании присоединенной к домену среды песочницы HDInsight](./domain-joined/apache-domain-joined-configure.md). 

## <a name="install-hdinsight-applications-on-clusters"></a>Установка приложений HDInsight в кластерах

Пользователи могут устанавливать приложения HDInsight в кластере HDInsight под управлением Linux. Вы можете использовать приложения, предоставляемые корпорацией Майкрософт, сторонними производителями или разработанные самостоятельно. Дополнительные сведения см. в статье [Установка сторонних приложений Apache Hadoop в Azure HDInsight](hdinsight-apps-install-applications.md).

Большинство приложений HDInsight устанавливаются в пустой граничный узел.  Пустой граничный узел — это виртуальная машина Linux, на которой установлены и настроены те же клиентские инструменты, что и на головном узле. Граничный узел можно использовать для доступа к кластеру, а также тестирования и размещения клиентских приложений. Подробные сведения см. в статье [Использование пустых граничных узлов в HDInsight](hdinsight-apps-use-edge-node.md).

## <a name="configure-cluster-size"></a>Настройка размера кластера

Плата за использование узлов взимается, пока существует кластер. Начисление оплаты начинается после создания кластера и прекращается только после его удаления. Перевести кластер в режим ожидания или отменить его выделение невозможно.

### <a name="number-of-nodes-for-each-cluster-type"></a>Количество узлов для каждого типа кластера
Для каждого типа кластера используется своя терминология. Кроме того, типы отличаются количеством узлов и стандартными размерами виртуальных машин. В следующей таблице число узлов каждого типа указано в скобках.

| type | Nodes | Схема |
| --- | --- | --- |
| Hadoop |Головной узел (2), узел данных (от 1) |![Узлы кластера HDInsight Hadoop](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hadoop-cluster-type-nodes.png) |
| hbase |Головной сервер (2), региональный сервер (от 1), основной узел или узел Zookeeper (3) |![Узлы кластера HDInsight HBase](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hbase-cluster-type-setup.png) |
| Storm |Узел Nimbus (2), сервер супервизора (от 1), узел Zookeeper (3) |![Узлы кластера HDInsight Storm](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-storm-cluster-type-setup.png) |
| Spark |Головной узел (2), рабочий узел (от 1), узел Zookeeper (3) (кат. "Бесплатный" для размера виртуальной машины Zookeeper A1) |![Узлы кластера HDInsight Spark](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-spark-cluster-type-setup.png) |

Дополнительные сведения см. в разделе [Конфигурация узлов и размеры виртуальных машин по умолчанию для кластеров](hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) статьи "Что представляют собой компоненты и версии Hadoop, доступные в HDInsight?"

Затраты на кластеры HDInsight определяются числом узлов и размерами виртуальных машин на узлах. 

Кластеры разных типов отличаются типами, количеством и размерами узлов.
* Тип кластера Hadoop по умолчанию: 
    * два *головных узла*;  
    * четыре *узла данных*.
* Тип кластера Storm по умолчанию: 
    * два *узла Nimbus*;
    * три *узла ZooKeeper*;
    * четыре *узла супервизора*. 

При пробном использовании HDInsight рекомендуем применять один узел данных. Подробные сведения о ценах на HDInsight см. на [этой странице](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

> [!NOTE]  
> Максимальный размер кластера зависит от подписки Azure. Чтобы увеличить лимит, обратитесь в [службу поддержки по вопросам выставления счетов Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request).
>

При настройке кластера на портале Azure размер узла можно выбрать в колонке **Ценовые категории узлов**. На портале также отображаются расценки на узлы разных размеров. 

![Размеры узла виртуальной машины HDInsight](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-node-sizes.png)

### <a name="virtual-machine-sizes"></a>Размер виртуальных машин 
При развертывании кластеров выберите вычислительные ресурсы в зависимости от решения, которое планируется развернуть. Для кластеров HDInsight используются следующие виртуальные машины:
* виртуальные машины серий A и D1–4: [размеры виртуальных машин Linux общего назначения](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general);
* виртуальные машины серии D11–14: [размеры виртуальных машин, оптимизированных для памяти](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

Сведения о том, какие значения следует указывать для размера виртуальной машины при создании кластера с помощью различных пакетов SDK или Azure PowerShell, см. в разделе [Таблицы размеров](../cloud-services/cloud-services-sizes-specs.md#size-tables). Используйте значение из столбца **Размер** таблиц, приведенных в этой статье.

> [!IMPORTANT]  
> Если в кластере требуется более 32 рабочих узлов, для головного узла следует выбрать размер с по крайней мере 8 ядрами и 14 ГБ ОЗУ.
>
>

Дополнительные сведения см. в разделе [Размеры виртуальных машин](../virtual-machines/windows/sizes.md). Сведения о расценках на разные размеры см. [здесь](https://azure.microsoft.com/pricing/details/hdinsight).   

## <a name="advanced-settings-script-actions"></a>Расширенные параметры. Действия сценария

Можно установить дополнительные компоненты или настроить конфигурацию кластера с помощью сценариев во время создания. Такие скрипты вызываются с помощью **действия скрипта** — параметра конфигурации, который может использоваться с помощью портала Azure, командлетов HDInsight PowerShell или пакета SDK для HDInsight .NET. Дополнительные сведения см. в статье [Настройка кластеров HDInsight под управлением Linux с помощью действия сценария](hdinsight-hadoop-customize-cluster-linux.md).

Некоторые собственные компоненты Java, такие как Apache Mahout и Cascading, могут выполняться в кластере как архивные файлы (JAR) Java. Эти JAR-файлы можно распространить в службе хранилища Azure и отправить в кластеры HDInsight с помощью механизмов отправки заданий Hadoop. См. дополнительные сведения об [отправке заданий Apache Hadoop программными средствами](hadoop/submit-apache-hadoop-jobs-programmatically.md).

> [!NOTE]  
> Если при развертывании или вызове JAR-файлов в кластерах HDInsight возникают проблемы, обратитесь в [службу поддержки Майкрософт](https://azure.microsoft.com/support/options/).
>
> Компонент Cascading не поддерживается службой HDInsight и на него не распространяется техническая поддержка Майкрософт. Списки поддерживаемых компонентов см. в статье [Что представляют собой различные компоненты Hadoop, доступные в HDInsight?](hdinsight-component-versioning.md)
>
>

В некоторых случаях требуется изменить следующие файлы конфигурации в процессе создания:

* clusterIdentity.xml
* core-site.xml
* gateway.xml
* hbase-env.xml
* hbase-site.xml
* hdfs-site.xml
* hive-env.xml
* hive-site.xml
* mapred-site
* oozie-site.xml
* oozie-env.xml
* storm-site.xml
* tez-site.xml
* webhcat-site.xml
* yarn-site.xml

Подробные сведения см. в статье [Настройка кластеров HDInsight с помощью начальной загрузки](hdinsight-hadoop-customize-cluster-bootstrap.md).

## <a name="advanced-settings-extend-clusters-with-a-virtual-network"></a>Расширенные параметры. Расширение кластеров с помощью виртуальной сети
Если для решения нужны технологии, распределенные по нескольким типам кластеров HDInsight, [виртуальная сеть Azure](https://docs.microsoft.com/azure/virtual-network) может объединять необходимые типы кластеров. Благодаря такой конфигурации кластеры и любой развернутый в них код могут взаимодействовать друг с другом напрямую.

Подробные сведения об использовании виртуальных сетей Azure в HDInsight см. в статье [Расширение возможностей HDInsight с помощью виртуальной сети Azure](hdinsight-extend-hadoop-virtual-network.md).

Пример использования двух типов кластера в виртуальной сети Azure см. в статье об [использовании структурированного потока Apache Spark при помощи Apache Kafka](hdinsight-apache-kafka-spark-structured-streaming.md). Дополнительные сведения об использовании HDInsight в виртуальной сети, включая требования к конфигурации виртуальной сети, см. в статье [Расширение возможностей HDInsight с помощью виртуальной сети Azure](hdinsight-extend-hadoop-virtual-network.md).


## <a name="next-steps"></a>Дальнейшие действия

- [Что такое Azure HDInsight и стек технологий Apache Hadoop](hadoop/apache-hadoop-introduction.md)
- [Краткое руководство. Использование Apache Hadoop и Apache Hive в Azure HDInsight с шаблоном Resource Manager](hadoop/apache-hadoop-linux-tutorial-get-started.md)
- [Работа в экосистеме Hadoop в HDInsight на компьютере с Windows](hdinsight-hadoop-windows-tools.md)
