---
title: Создание концентратора событий с поддержкой Apache Kafka. Центры событий Azure | Документация Майкрософт
description: В этой статье представлено пошаговое руководство по созданию Apache Kafka с включенным пространством имен концентраторов событий Azure с помощью портал Azure.
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 9ce0f74ec6d4e536bfb3fe827ae6f8ae143b640e
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2019
ms.locfileid: "72555797"
---
# <a name="create-apache-kafka-enabled-event-hubs"></a>Создание Центров событий с поддержкой Apache Kafka

Центры событий Azure представляют собой платформу как услугу (PaaS) для потока больших данных, которая принимает миллионы событий в секунду и предоставляет небольшую задержку и высокий уровень пропускной способности для аналитики и визуализации в режиме реального времени.

Центры событий Azure предоставляют конечную точку Kafka. Эта конечная точка позволяет вашему пространству имен Центров событий распознавать протокол [Apache Kafka](https://kafka.apache.org/intro) и API в собственном коде. Эта возможность позволяет подключиться к концентраторам событий, как к темам Kafka, не изменяя клиентов протокола или запустив собственные кластеры. Центры событий поддерживают [Apache Kafka 1.0.](https://kafka.apache.org/10/documentation.html) и более поздние версии.

В этой статье описывается создание пространства имен Центров событий и получение строки подключения, необходимой для подключения приложений Kafka к концентраторам событий с поддержкой Kafka.

## <a name="prerequisites"></a>Технические условия

Если у вас еще нет подписки Azure, создайте [бесплатная учетная запись](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), прежде чем начинать работу.

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Создание пространства имен Центров событий с поддержкой Kafka

1. Войдите на [портал Azure][Azure portal] и щелкните **Создать ресурс** в левой верхней части экрана.

2. Найдите Центры событий и выберите отображаемые параметры:
    
    ![Поиск Центров событий на портале](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. Укажите уникальное имя и включите Kafka в пространстве имен. Щелкните **Create**(Создать).
    
    ![Создание пространства имен](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.jpg)
 
4. Создав пространство имен, на вкладке **Параметры** щелкните **Политики общего доступа** для получения строки подключения.

    ![Выбор политик общего доступа](./media/event-hubs-create/create-event-hub7.png)

5. Вы можете выбрать значение по умолчанию **RootManageSharedAccessKey** или добавить новую политику. Щелкните имя политики и скопируйте строку подключения. 
    
    ![Выбор политики](./media/event-hubs-create/create-event-hub8.png)
 
6. Добавьте эту строку подключения в конфигурацию приложения Kafka.

Теперь можно выполнять потоковую передачу событий из приложений, использующих протокол Kafka, в Центры событий.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о Центрах событий см. в следующих статьях:

* [Потоковая передача данных в Центры событий из приложений Kafka](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* [Azure Event Hubs for Apache Kafka (preview)](event-hubs-for-kafka-ecosystem-overview.md) (Центры событий Azure для Apache Kafka (предварительный просмотр))
* [Сведения о Центрах событий](event-hubs-what-is-event-hubs.md)


[Azure portal]: https://portal.azure.com/
