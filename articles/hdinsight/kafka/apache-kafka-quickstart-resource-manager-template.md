---
title: Краткое руководство. Использование Apache Kafka с Azure Resource Manager в HDInsight
description: Из этого краткого руководства вы узнаете, как создать кластер Apache Kafka в Azure HDInsight с помощью шаблона Resource Manager. Вы также узнаете, что такое разделы, подписчики и потребители в Kafka.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: mvc
ms.topic: quickstart
ms.date: 06/12/2019
ms.openlocfilehash: d908d210ff0448069a9abc76209c72d9b2a7595c
ms.sourcegitcommit: 3486e2d4eb02d06475f26fbdc321e8f5090a7fac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/31/2019
ms.locfileid: "73242020"
---
# <a name="quickstart-create-apache-kafka-cluster-in-azure-hdinsight-using-resource-manager-template"></a>Краткое руководство. Создание кластера Apache Kafka в Azure HDInsight с помощью шаблонов Resource Manager

[Apache Kafka](https://kafka.apache.org/) — это распределенная платформа потоковой передачи с открытым кодом. Она часто используется как брокер сообщений, предоставляя такие же функциональные возможности, как и очередь сообщений типа "публикация-подписка". 

В этом кратком руководстве вы узнаете, как создать кластер [Apache Kafka](https://kafka.apache.org) с помощью шаблона Azure Resource Manager. Вы также узнаете, как с помощью предоставленных служебных программ отправлять и получать сообщения, используя Kafka. Похожие шаблоны можно просмотреть на странице [Шаблоны быстрого запуска Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Hdinsight&pageNumber=1&sort=Popular). Ссылку на шаблон можно найти [здесь](https://docs.microsoft.com/azure/templates/microsoft.hdinsight/allversions).

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

API Kafka доступен только ресурсам, размещенным в той же виртуальной сети. В этом кратком руководстве вы напрямую обращаетесь к кластеру по протоколу SSH. Чтобы подключить к Kafka другие службы, сети или виртуальные машины, необходимо сначала создать виртуальную сеть, а затем создать в ней эти ресурсы. Дополнительные сведения см. в документе [Подключение к Kafka в HDInsight с помощью виртуальной сети Azure](apache-kafka-connect-vpn-gateway.md).

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Клиент SSH. Дополнительные сведения см. в руководстве по [подключению к HDInsight (Apache Hadoop) с помощью SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-an-apache-kafka-cluster"></a>Создание кластера Apache Kafka

1. Щелкните следующее изображение, чтобы открыть шаблон на портале Azure.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-kafka-java-get-started%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="./media/apache-kafka-quickstart-resource-manager-template/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

2. Чтобы создать кластер Kafka, используйте следующие значения.

    | Свойство | Значение |
    | --- | --- |
    | Subscription | Вашу подписку Azure. |
    | группа ресурсов. | Группа ресурсов, в которой будет создан кластер. |
    | Location | Регион Azure, в котором будет создан кластер. |
    | Имя кластера, | Имя кластера Kafka. |
    | Имя пользователя для входа в кластер | Имя учетной записи для входа в службы на основе HTTPS, размещенные в кластере. |
    | Пароль для входа в кластер | Пароль для имени для входа. |
    | Имя пользователя SSH | Имя пользователя SSH. Эта учетная запись может обращаться к кластеру по протоколу SSH. |
    | Пароль SSH | Пароль пользователя SSH. |

    ![Снимок экрана свойств шаблона](./media/apache-kafka-quickstart-resource-manager-template/kafka-template-parameters.png)

3. Установите флажок **Я принимаю указанные выше условия** и **Закрепить на панели мониторинга**, а затем нажмите кнопку **Приобрести**. Операция создания кластера может занять до 20 минут.

## <a name="connect-to-the-cluster"></a>Подключение к кластеру

1. Чтобы подключиться к первичному головному узлу кластера Kafka, выполните приведенную ниже команду. Замените `sshuser` именем пользователя SSH. Замените `mykafka` именем кластера Kafka.

    ```bash
    ssh sshuser@mykafka-ssh.azurehdinsight.net
    ```

2. При первом подключении к кластеру клиент SSH может отобразить предупреждение о том, что не удается установить подлинность узла. При появлении запроса введите __yes__ (Да) и нажмите клавишу __ВВОД__, чтобы добавить узел в список доверенных серверов клиента SSH.

3. При появлении запроса введите пароль пользователя SSH.

    После подключения отобразятся сведения, аналогичные приведенному ниже тексту.
    
    ```output
    Authorized uses only. All activity may be monitored and reported.
    Welcome to Ubuntu 16.04.4 LTS (GNU/Linux 4.13.0-1011-azure x86_64)
    
     * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
     * Support:        https://ubuntu.com/advantage
    
      Get cloud support with Ubuntu Advantage Cloud Guest:
        https://www.ubuntu.com/business/services/cloud
    
    83 packages can be updated.
    37 updates are security updates.
    
    
    Welcome to Kafka on HDInsight.
    
    Last login: Thu Mar 29 13:25:27 2018 from 108.252.109.241
    ```

## <a id="getkafkainfo"></a>Получение сведений об узлах Apache Zookeeper и брокера

Для работы с Kafka необходимы сведения об узле *Apache Zookeeper* и узле *брокера*. Эти узлы используются Kafka API и многими другими служебными программами, поставляемыми с платформой Kafka.

В этом разделе для получения сведений об узле используется REST API Ambari в кластере.

1. Используя SSH-подключение к кластеру, выполните следующую команду, чтобы установить служебную программу `jq`. Эта служебная программа используется для анализа документов JSON. С ее помощью также удобно получать сведения об узлах.
   
    ```bash
    sudo apt -y install jq
    ```

2. Чтобы задать переменную среды с именем кластера, выполните следующую команду.

    ```bash
    read -p "Enter the Kafka on HDInsight cluster name: " CLUSTERNAME
    ```

    При появлении запроса введите имя кластера Kafka.

3. Чтобы задать переменную среды со сведениями об узле Zookeeper, выполните следующую команду. Эта команда извлекает все узлы Zookeeper, а затем возвращает только первые две записи. Причина этого состоит в том, что требуется обеспечить избыточность на случай, если узел станет недоступным.

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    ```

    При появлении запроса введите пароль учетной записи для входа в кластер (но не учетной записи SSH).

4. Чтобы убедиться, что переменную среды задано верно, выполните следующую команду.

    ```bash
     echo '$KAFKAZKHOSTS='$KAFKAZKHOSTS
    ```

    Эта команда возвращает сведения аналогичные следующим:

    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`

5. Чтобы задать переменную среды со сведениями об узле брокера Kafka, выполните следующую команду.

    ```bash
    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    ```

    При появлении запроса введите пароль учетной записи для входа в кластер (но не учетной записи SSH).

6. Чтобы убедиться, что переменную среды задано верно, выполните следующую команду.

    ```bash   
    echo '$KAFKABROKERS='$KAFKABROKERS
    ```

    Эта команда возвращает сведения аналогичные следующим:
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

## <a name="manage-apache-kafka-topics"></a>Управление разделами Apache Kafka

Kafka хранит потоки данных в *разделах*. Для управления разделами можно использовать служебную программу `kafka-topics.sh`.

* **Чтобы создать раздел**, в сеансе SSH-подключения выполните следующую команду.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
    ```

    Эта команда создает подключение к Zookeeper, используя хранящиеся в `$KAFKAZKHOSTS` сведения об узле. Затем она создает раздел Kafka с именем **test**. 

    * Данные, хранящиеся в этом разделе, разделены на восемь секций.

    * Каждая секция реплицируются на три рабочих узла в кластере.

        Если кластер был создан в регионе Azure, который предоставляет три домена сбоя, то следует использовать коэффициент репликации, равный 3. В противном случае следует использовать коэффициент репликации, равный 4.
        
        В регионах с тремя доменами сбоя коэффициент репликации, равный 3, обеспечивает распределение реплик по доменам сбоя. В регионах с двумя доменами сбоя коэффициент репликации, равный 4, обеспечивает равномерное распределение реплик по доменам сбоя.
        
        Дополнительные сведения о количестве доменов сбоя в регионе см. в разделе [Использование управляемых дисков для виртуальных машин в группе доступности](../../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set).

        В Kafka нет сведений о доменах сбоя Azure. При создании реплик секций для разделов реплики могут быть неправильно распределены с точки зрения высокой доступности.

        Для обеспечения высокого уровня доступности используйте [средство перераспределения секций Apache Kafka](https://github.com/hdinsight/hdinsight-kafka-tools). Этот инструмент следует запускать из сеанса SSH-подключения на головном узле кластера Kafka.

        Чтобы обеспечить максимально высокий уровень доступности данных Kafka, следует перераспределять реплики секций для раздела в следующих случаях:

        * при создании раздела или секции;

        * при масштабировании кластера.

* **Чтобы вывести список разделов**, введите следующую команду.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
    ```

    Она выводит список разделов на кластере Kafka.

* **Чтобы удалить раздел**, используйте следующую команду.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --delete --topic topicname --zookeeper $KAFKAZKHOSTS
    ```

    Она удаляет раздел `topicname`.

    > [!WARNING]  
    > Если вы уже удалили раздел `test`, то необходимо создать его заново. Он используется в последующих действиях в этом документе.

Чтобы получить дополнительные сведения о командах, доступных в служебной программе `kafka-topics.sh`, используйте следующую команду.

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh
```

## <a name="produce-and-consume-records"></a>Создание и использование записей

Kafka хранит *записи* в разделах. Записи создаются *производителями*, а используются *потребителями*. Производители и потребители взаимодействуют со службой *брокера Kafka*. Каждый рабочий узел в кластере HDInsight — это узел брокера Kafka.

Чтобы сохранить записи в созданный ранее раздел test, а затем считать их с помощью потребителя, сделайте следующее:

1. Чтобы записать записи в раздел, используйте служебную программу `kafka-console-producer.sh` из сеанса SSH-подключения.
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    После выполнения этой команды вы перейдете к пустой строке.

2. Введите текстовое сообщение в эту строку и нажмите клавишу ВВОД. Введите несколько таких сообщений, а затем нажмите клавиши **CTRL+C**, чтобы вернуться к обычной командной строке. Каждая строка отправляется в раздел Kafka как отдельная запись.

3. Чтобы считать записи из раздела, используйте служебную программу `kafka-console-consumer.sh` из сеанса SSH-подключения.
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```
   
    Эта команда извлекает записи из раздела, а затем отображает их. Параметр `--from-beginning` указывает потребителю считывать данные с самого начала потока, поэтому будут извлечены все записи.

    Если вы используете более раннюю версию Kafka, замените `--bootstrap-server $KAFKABROKERS` на `--zookeeper $KAFKAZKHOSTS`.

4. Нажмите клавиши __Ctrl+C__, чтобы остановить потребитель.

Вы также можете создать отправители и получатели программными средствами. Пример использования этого API см. в руководстве [Использование API производителя и потребителя Apache Kafka](apache-kafka-producer-consumer-api.md).

## <a name="clean-up-resources"></a>Очистка ресурсов

Если нужно очистить ресурсы, созданные при работе с этим кратким руководством, можно удалить группу ресурсов. При этом будет удален связанный кластер HDInsight и другие ресурсы, связанные с этой группой ресурсов.

Чтобы удалить группу ресурсов с помощью портала Azure, сделайте следующее:

1. На портале Azure разверните меню слева, чтобы открыть меню служб, а затем выберите __Группы ресурсов__, чтобы просмотреть список групп ресурсов.
2. Найдите группу ресурсов, которую нужно удалить, и щелкните правой кнопкой мыши кнопку __Дополнительно__ (…) справа от списка.
3. Выберите __Удалить группу ресурсов__ и подтвердите выбор.

> [!WARNING]  
> Начисление оплаты начинается после создания кластера HDInsight и прекращается только после его удаления. Кластеры оплачиваются поминутно, поэтому всегда следует удалять кластер, когда он больше не нужен.
> 
> При удалении кластера Kafka в HDInsight удаляются все данные, хранящиеся в Kafka.

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Совместное использование Apache Spark и Apache Kafka](../hdinsight-apache-kafka-spark-structured-streaming.md)