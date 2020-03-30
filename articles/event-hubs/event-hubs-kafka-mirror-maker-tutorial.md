---
title: Использование Apache Kafka MirrorMaker в службе "Центры событий Azure" | Документация Майкрософт
description: В этой статье описано, как с помощью Kafka MirrorMaker настроить зеркальное отображение кластера Kafka в службе "Центры событий Azure".
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: conceptual
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 6dc902b6a26c175713381b4fce88934dca3f409e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "80283588"
---
# <a name="use-kafka-mirrormaker-with-event-hubs-for-apache-kafka"></a>Использование Apache Kafka MirrorMaker с Центрами событий

В этом руководстве показано, как выполнить зеркальное отображение брокера Kafka в концентраторе событий с поддержкой Kafka с помощью средства Kafka MirrorMaker.

   ![Kafka MirrorMaker и Центры событий](./media/event-hubs-kafka-mirror-maker-tutorial/evnent-hubs-mirror-maker1.png)

> [!NOTE]
> Этот пример можно найти на сайте [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/mirror-maker).


В этом руководстве описано следующее:
> [!div class="checklist"]
> * Создание пространства имен в Центрах событий
> * Клонирование примера проекта
> * Настройка кластера Kafka
> * Настройка Kafka MirrorMaker
> * Запуск Kafka MirrorMaker

## <a name="introduction"></a>Вступление
Одним из важных аспектов для современных облачных приложений является возможность обновления, улучшения и изменения инфраструктуры без прерывания работы службы. В этом учебнике показано, как концентратор событий и Kafka MirrorMaker могут интегрировать существующий конвейер Kafka в Azure, «отражая» поток ввода Kafka в службе концентраторов событий. 

Конечная точка Kafka Центров событий Azure позволяет подключаться к Центрам событий Azure с помощью протокола Kafka (т. е. клиентов Kafka). После внесения минимальных изменений в приложение Kafka вы сможете подключаться к Центрам событий Azure и пользоваться преимуществами экосистемы Azure. Сейчас Центры событий на основе Kafka поддерживают Kafka 1.0 и более поздние версии.

## <a name="prerequisites"></a>Предварительные требования

В рамках этого руководства вам потребуются:

* Прочтите статью [Центры событий Azure для Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md). 
* Подписка Azure. Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), прежде чем начать работу.
* [Комплект разработки Java (JDK) 1.7](https://aka.ms/azure-jdks)
    * В Ubuntu выполните команду `apt-get install default-jdk`, чтобы установить JDK.
    * Обязательно настройте переменную среды JAVA_HOME так, чтобы она указывала на папку, в которой установлен пакет JDK.
* [Скачать](https://maven.apache.org/download.cgi) и [установить](https://maven.apache.org/install.html) двоичный архив Maven
    * В Ubuntu выполните команду `apt-get install maven`, чтобы установить Maven.
* [Git](https://www.git-scm.com/downloads);
    * В Ubuntu выполните команду `sudo apt-get install git`, чтобы установить Git.

## <a name="create-an-event-hubs-namespace"></a>Создание пространства имен в Центрах событий

Для отправки и получения данных из любой службы Центров событий требуется пространство имен Центров событий. Инструкции о получении конечной точки Kafka концентраторов событий см. в статье [Creating a Kafka enabled Event Hub](event-hubs-create.md) (Создание концентраторов событий с поддержкой Kafka). Скопируйте строку подключения к Центрам событий для дальнейшего использования.

## <a name="clone-the-example-project"></a>Клонирование примера проекта

Теперь, когда у вас есть строка подключения к Центрам событий с поддержкой Kafka, клонируйте репозиторий Центров событий Azure для Kafka и перейдите ко вложенной папке `mirror-maker`.

```shell
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/mirror-maker
```

## <a name="set-up-a-kafka-cluster"></a>Настройка кластера Kafka

Инструкции по настройке кластера с требуемыми параметрами (или использовании существующего кластера Kafka) см. в [кратком руководстве по Kafka](https://kafka.apache.org/quickstart).

## <a name="configure-kafka-mirrormaker"></a>Настройка Kafka MirrorMaker

Средство Kafka MirrorMaker позволяет выполнять зеркальное отображение потока. При наличии исходного и конечного кластеров Kafka средство MirrorMaker гарантирует, что любые сообщения, отправляемые в исходный кластер, поступают в исходный и конечный кластеры. В этом примере показано, как отразить кластер Kafka источника с концентратором событий назначения. Этот сценарий можно использовать для отправки данных из существующего конвейера Kafka в Центры событий без прерывания потока данных. 

Более подробные сведения о средстве Kafka MirrorMaker см. в [руководстве по зеркальному отображению Kafka и средству MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330).

Чтобы настроить Kafka MirrorMaker, дайте ему кластер Kafka в качестве потребителя/источника и центр событий в качестве производителя/назначения.

#### <a name="consumer-configuration"></a>Конфигурация потребителя

Обновите файл конфигурации потребителя `source-kafka.config`, из которого средство MirrorMaker получает свойства исходного кластера Kafka.

##### <a name="source-kafkaconfig"></a>source-kafka.config

```
bootstrap.servers={SOURCE.KAFKA.IP.ADDRESS1}:{SOURCE.KAFKA.PORT1},{SOURCE.KAFKA.IP.ADDRESS2}:{SOURCE.KAFKA.PORT2},etc
group.id=example-mirrormaker-group
exclude.internal.topics=true
client.id=mirror_maker_consumer
```

#### <a name="producer-configuration"></a>Конфигурация производителя

Теперь обновите файл конфигурации производителя `mirror-eventhub.config`, который указывает средству MirrorMaker отправлять повторяющиеся (или зеркальные) данные в службу Центров событий. В частности, измените `bootstrap.servers` и `sasl.jaas.config` так, чтобы они указывали на конечную точку Kafka Центров событий. Для работы службы Центров событий требуется безопасное подключение (SASL), которое обеспечивается путем установки последних трех свойств в следующей конфигурации: 

##### <a name="mirror-eventhubconfig"></a>mirror-eventhub.config

```
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
client.id=mirror_maker_producer

#Required for Event Hubs
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

## <a name="run-kafka-mirrormaker"></a>Запуск Kafka MirrorMaker

Запустите сценарий Kafka MirrorMaker из корневого каталога Kafka с помощью только что обновленных файлов конфигурации. Скопируйте файлы конфигурации в корневой каталог Kafka или обновите пути к ним в следующей команде.

```shell
bin/kafka-mirror-maker.sh --consumer.config source-kafka.config --num.streams 1 --producer.config mirror-eventhub.config --whitelist=".*"
```

Чтобы убедиться, что события достигают концентратора событий, см. статистику входа на [портале Azure](https://azure.microsoft.com/features/azure-portal/)или запустите потребителя против концентратора событий.

Во время работы средства MirrorMaker события, отправляемые в исходный кластер Kafka, поступают на кластер Kafka и в зеркальную службу концентраторов событий с поддержкой Kafka. С помощью MirrorMaker и конечной точки Kafka Центров событий можно перенести существующий конвейер Kafka в управляемую службу Центров событий Azure, не изменяя существующий кластер и не прерывая текущий поток данных.

## <a name="samples"></a>Примеры
Смотрите следующие образцы на GitHub:

- [Пример кода для этого учебника на GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/mirror-maker)
- [Концентраторы событий Azure Kafka MirrorMaker работают на экземпляре контейнера Azure](https://github.com/djrosanova/EventHubsMirrorMaker)

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве описано следующее:
> [!div class="checklist"]
> * Создание пространства имен в Центрах событий
> * Клонирование примера проекта
> * Настройка кластера Kafka
> * Настройка Kafka MirrorMaker
> * Запуск Kafka MirrorMaker

Дополнительные сведения о Центрах событий и Центрах событий для Kafka см. в статьях:  

- [Сведения о Центрах событий](event-hubs-what-is-event-hubs.md)
- [Центры событий Azure для Apache Kafka (предварительная версия)](event-hubs-for-kafka-ecosystem-overview.md)
- [Как создать центр событий](event-hubs-create.md)
- [Потоковая передача данных в Центры событий из приложений Kafka](event-hubs-quickstart-kafka-enabled-event-hubs.md)
- [Подключение Apache Spark к концентратору событий](event-hubs-kafka-spark-tutorial.md)
- [Подключение Apache Flink к концентратору событий](event-hubs-kafka-flink-tutorial.md)
- [Интеграция Kafka Connect с центром событий](event-hubs-kafka-connect-tutorial.md)
- [Подключение Потоков Akka к центру событий](event-hubs-kafka-akka-streams-tutorial.md)
- [Migrating to Azure Event Hubs for Apache Kafka Ecosystems](https://github.com/Azure/azure-event-hubs-for-kafka) (Переход в Центры событий Azure для экосистем Apache Kafka)
