---
title: Включение автоматического создания разделов в Apache Kafka — Azure HDInsight
description: Узнайте, как настроить автоматическое создание разделов в Apache Kafka в HDInsight. В Kafka вы можете указать для параметра auto.create.topics.enable значение true, используя Ambari или создавая кластер с помощью PowerShell или шаблонов Resource Manager.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/18/2018
ms.openlocfilehash: 393087f4d5c5e7a52fd2dd10d20362a045a0075b
ms.sourcegitcommit: fad368d47a83dadc85523d86126941c1250b14e2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/19/2019
ms.locfileid: "71122667"
---
# <a name="how-to-configure-apache-kafka-on-hdinsight-to-automatically-create-topics"></a>Настройка автоматического создания разделов в Apache Kafka в HDInsight

По умолчанию в [Apache Kafka](https://kafka.apache.org/) в HDInsight автоматическое создание разделов отключено. Но вы можете включить автоматическое создание разделов для существующих кластеров с помощью [Apache Ambari](https://ambari.apache.org/). Также автоматическое создание разделов можно включить при создании нового кластера Kafka с помощью шаблона Azure Resource Manager.

## <a name="apache-ambari-web-ui"></a>Веб-интерфейс Apache Ambari

Чтобы включить автоматическое создание разделов в существующем кластере через пользовательский веб-интерфейс Ambari, выполните следующие действия:

1. На [портале Azure](https://portal.azure.com) выберите нужный кластер Kafka.

2. На странице __сведений о кластере__ выберите __Панель мониторинга кластера__.

    ![Изображение страницы портала с выбранной панелью мониторинга кластера](./media/apache-kafka-auto-create-topics/kafka-cluster-overview.png)

3. Затем выберите __Панель мониторинга кластера HDInsight__. Когда появится запрос, пройдите проверку подлинности, используя учетные данные (администратора) входа для кластера.

    ![Изображение процесса ввода на панели мониторинга кластера HDInsight](./media/apache-kafka-auto-create-topics/hdinsight-cluster-dashboard.png)

3. В списке в левой части страницы выберите службу Kafka.

    ![Вкладка списка служб Apache Ambari](./media/apache-kafka-auto-create-topics/hdinsight-service-list.png)

4. Выберите "Конфигурации" в середине страницы.

    ![Вкладка "конфигурации" службы Apache Ambari](./media/apache-kafka-auto-create-topics/hdinsight-service-config.png)

5. В поле "Фильтр" введите значение параметра `auto.create`.

    ![Поле фильтра поиска Apache Ambari](./media/apache-kafka-auto-create-topics/hdinsight-filter-field.png)

    Будет отфильтрован список свойств и отобразится параметр `auto.create.topics.enable`.

6. Измените значение параметра `auto.create.topics.enable` на `true` и выберите "Сохранить". Добавьте заметку и выберите "Сохранить" еще раз.

    ![Изображение ввода значения для параметра auto.create.topics.enable](./media/apache-kafka-auto-create-topics/auto-create-topics-enable.png)

7. Выберите службу Kafka и щелкните __Перезапустить__, а затем выберите __Restart all affected__ (Перезапустить все затронутые). Когда появится запрос, выберите __Conform Restart All__ (Подтвердить перезапуск всех).

    ![Apache Ambari перезапускает все затронутые](./media/apache-kafka-auto-create-topics/restart-all-affected.png)

> [!NOTE]  
> Можно также задать значения Ambari с помощью REST API Ambari. Обычно этот метод сложнее, так как требуется несколько вызовов REST, чтобы получить текущую конфигурацию, изменить ее и т. д. Дополнительные сведения см. в документе [Управление кластерами HDInsight с помощью REST API Ambari](../hdinsight-hadoop-manage-ambari-rest-api.md).

## <a name="resource-manager-templates"></a>Шаблоны Resource Manager

При создании кластера Kafka с помощью шаблона Azure Resource Manager вы можете напрямую задать значение параметра `auto.create.topics.enable`, добавив его в `kafka-broker`. В следующем фрагменте JSON для этого параметра устанавливается значение `true`:

```json
"clusterDefinition": {
    "kind": "kafka",
    "configurations": {
    "gateway": {
        "restAuthCredential.isEnabled": true,
        "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
        "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
    },
    "kafka-broker": {
        "auto.create.topics.enable": "true"
    }
}
```

## <a name="next-steps"></a>Следующие шаги

Из этого документа вы узнали, как включить автоматическое создание разделов для Apache Kafka в HDInsight. Дополнительные сведения о работе с Kafka вы можете получить по следующим ссылкам.

* [Анализ журналов для Apache Kafka в HDInsight](apache-kafka-log-analytics-operations-management.md)
* [Репликация данных между кластерами Apache Kafka](apache-kafka-mirroring.md)
