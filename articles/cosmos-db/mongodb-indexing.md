---
title: Индексирование в API Azure Cosmos DB для MongoDB
description: Обзор возможностей индексирования в API Azure Cosmos DB для MongoDB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 12/26/2018
author: sivethe
ms.author: sivethe
ms.openlocfilehash: de037316efa50dd25ea04c370fa0e5878fb52ba1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60448509"
---
# <a name="indexing-using-azure-cosmos-dbs-api-for-mongodb"></a>Индексирование с помощью API Azure Cosmos DB для MongoDB

API Azure Cosmos DB для MongoDB использует возможности автоматического управления индексами Cosmos DB. В результате у пользователей появляется доступ к стандартным политикам индексации Cosmos DB. Таким образом, если индексы не были определены пользователем и ни один индекс не был удален, тогда все поля будут автоматически проиндексированы по умолчанию в момент вставки в коллекцию. В большинстве случаев рекомендуется использовать набор политик индексирования по умолчанию, установленный в учетной записи.

## <a name="dropping-the-default-indexes"></a>Удаление индексов по умолчанию

Можно использовать следующую команду для удаления стандартных индексов коллекции ```coll```:

```JavaScript
> db.coll.dropIndexes()
{ "_t" : "DropIndexesResponse", "ok" : 1, "nIndexesWas" : 3 }
```

## <a name="creating-compound-indexes"></a>Создание составных индексов

Составные индексы содержат ссылки на множество полей документа. Логически это эквивалентно созданию нескольких отдельных индексов для каждого поля. Чтобы воспользоваться преимуществами оптимизации, предоставляемыми технологиями индексирования Cosmos DB, рекомендуется создать несколько отдельных индексов вместо одного (неуникального) составного индекса.

## <a name="creating-unique-indexes"></a>Создание уникальных индексов

[Уникальные индексы](unique-keys.md) позволяют гарантировать, что никакие два или больше документов не будут содержать одинаковые значения для индексированных полей. 
>[!important] 
> В настоящее время уникальные индексы можно создавать только в том случае, если коллекция пуста (не содержит документов). 

Следующая команда создает уникальный индекс для поля "student_id":

```JavaScript
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1 }, {unique:true} ) 
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 4
}
```

Для сегментированных коллекций при работе с MongoDB создание уникального индекса требует наличия ключа секции (раздела). Иными словами, все уникальные индексы для сегментированной коллекции — это составные индексы, где одно из полей является ключом раздела.

Следующие команды создают сегментированную коллекцию ```coll``` (ключ сегмента — ```university```) с уникальным индексом для полей "student_id" и "university":

```JavaScript
globaldb:PRIMARY> db.runCommand({shardCollection: db.coll._fullName, key: { university: "hashed"}});
{
        "_t" : "ShardCollectionResponse",
        "ok" : 1,
        "collectionsharded" : "test.coll"
}
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1, "university" : 1 }, {unique:true})
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 3,
        "numIndexesAfter" : 4
}
```

Если в приведенном выше примере пропустить предложение ```"university":1```, будет возвращено следующее сообщение об ошибке:

```"cannot create unique index over {student_id : 1.0} with shard key pattern { university : 1.0 }"```

## <a name="ttl-indexes"></a>Индексы срока жизни

Чтобы установить срок действия документа в определенной коллекции, нужно создать [индекс срока жизни](../cosmos-db/time-to-live.md). Это индекс для поля _ts со значением "expireAfterSeconds".
 
Пример:
```JavaScript
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
```

Предыдущая команда приведет к удалению всех документов коллекции ```db.coll```, которые не менялись последние 10 секунд. 
 
> [!NOTE]
> **_ts** является специфическим полем Cosmos DB и не доступно клиентам MongoDB. Это зарезервированное (системное) свойство, содержащее метку времени последнего изменения документа.
>

## <a name="migrating-collections-with-indexes"></a>Перемещение коллекций с индексами

В настоящее время создание уникальных индексов возможно только в том случае, если коллекция не содержит документов. Популярные средства перемещения MongoDB пытаются создавать уникальные индексы после импорта данных. Для обхода этой проблемы пользователям рекомендуется вручную создавать соответствующие коллекции и уникальные индексы, отключив это действие для средств перемещения (в ```mongorestore``` для этого в командной строке следует указать параметр --noIndexRestore).

## <a name="next-steps"></a>Дальнейшие действия
* [Индексирование в Azure Cosmos DB](../cosmos-db/index-policy.md)
* [Автоматическое завершение срока действия данных в Azure Cosmos DB с использованием срока жизни](../cosmos-db/time-to-live.md)
