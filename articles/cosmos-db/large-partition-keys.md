---
title: Создание контейнеров Azure Cosmos с большим ключом секции
description: Узнайте, как создать контейнер в Azure Cosmos DB с большим ключом раздела, используя портал Azure и различные пакеты SDK.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/28/2019
ms.author: mjbrown
ms.openlocfilehash: 7184a6b85e93c41dfe914813301a4b1a0c88f2cd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75887688"
---
# <a name="create-containers-with-large-partition-key"></a>Создание контейнеров с большим ключом секции

Azure Cosmos DB использует схему секционирования на основе хэша для достижения горизонтального масштабирования данных. Все контейнеры Azure Cosmos, созданные до 3 2019, могут использовать хэш-функцию, которая вычислит хэш на основе первых 100 байт ключа секции. Если имеется несколько ключей разделов, имеющих один и тот же первый 100 байта, то эти логические секции рассматриваются службой как один и тот же логический раздел. Это может привести к проблемам, например к квотам на размер секций, а также к применению уникальных индексов по ключам секций. Для решения этой проблемы введены крупные ключи разделов. Azure Cosmos DB теперь поддерживает большие ключи разделов со значениями до 2 КБ.

Большие ключи разделов поддерживаются с помощью функций улучшенной версии хэш-функции, которая может формировать уникальный хэш-код из больших ключей разделов размером до 2 КБ. Эту версию хэша также рекомендуется использовать для сценариев с высокой кратностью ключа секции независимо от размера ключа секции. Количество элементов ключа секции определяется как число уникальных логических секций, например в порядке ~ 30000 логических секций в контейнере. В этой статье описывается создание контейнера с большим ключом секции с помощью портал Azure и различных пакетов SDK.

## <a name="create-a-large-partition-key-azure-portal"></a>Создание большого ключа секции (портал Azure)

Чтобы создать большой ключ секции, при создании нового контейнера с помощью портал Azure установите флажок **ключ раздела "больше 100 байт** ". Снимите флажок, если не требуются ключи разделов большого размера, или если у вас есть приложения, работающие в пакетах SDK более ранних версий, чем 1,18.

![Создание больших ключей разделов с помощью портал Azure](./media/large-partition-keys/large-partition-key-with-portal.png)

## <a name="create-a-large-partition-key-powershell"></a>Создание большого ключа секции (PowerShell)

Сведения о создании контейнера с поддержкой ключей больших разделов см. в разделе.

* [Создание контейнера Azure Cosmos с большим размером ключа раздела](manage-with-powershell.md#create-container-big-pk)

## <a name="create-a-large-partition-key-net-sdk"></a>Создание большого ключа секции (пакет SDK для .NET)

Чтобы создать контейнер с большим ключом секции с помощью пакета SDK для .NET, укажите `PartitionKeyDefinitionVersion.V2` свойство. В следующем примере показано, как указать свойство Version в объекте Партитионкэйдефинитион и задать для него значение Партитионкэйдефинитионверсион. v2.

### <a name="v3-net-sdk"></a>v3 .NET SDK

```csharp
await database.CreateContainerAsync(
    new ContainerProperties(collectionName, $"/longpartitionkey")
    {
        PartitionKeyDefinitionVersion = PartitionKeyDefinitionVersion.V2,
    })
```

### <a name="v2-net-sdk"></a>пакет SDK для .NET v2

```csharp
DocumentCollection collection = await newClient.CreateDocumentCollectionAsync(
database,
     new DocumentCollection
        {
           Id = Guid.NewGuid().ToString(),
           PartitionKey = new PartitionKeyDefinition
           {
             Paths = new Collection<string> {"/longpartitionkey" },
             Version = PartitionKeyDefinitionVersion.V2
           }
         },
      new RequestOptions { OfferThroughput = 400 });
```

## <a name="supported-sdk-versions"></a>Поддерживаемые версии пакета SDK

Ключи больших разделов поддерживаются со следующими минимальными версиями пакетов SDK:

|Тип пакета SDK  | Минимальная версия   |
|---------|---------|
|.Net     |    1,18     |
|Синхронизация Java     |   2.4.0      |
|Java Async   |  2.5.0        |
| REST API | версия выше, `2017-05-03` чем с помощью `x-ms-version` заголовка запроса.|
| Шаблон Resource Manager | Версия 2 с помощью `"version":2` свойства в `partitionKey` объекте. |

В настоящее время нельзя использовать контейнеры с большим ключом секции в в Power BI и Azure Logic Apps. В этих приложениях можно использовать контейнеры без большого ключа секции.

## <a name="next-steps"></a>Следующие шаги

* [Секционирование в базе данных Azure Cosmos DB](partitioning-overview.md)
* [Единицы запроса в Azure Cosmos DB](request-units.md)
* [Обеспечение необходимой пропускной способности для контейнеров и баз данных](set-throughput.md)
* [Работа с учетной записью Azure Cosmos](account-overview.md)
