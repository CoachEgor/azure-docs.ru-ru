---
title: Назначение переменных в хранилище данных SQL Azure | Документация Майкрософт
description: Советы по присваиванию значений переменных T-SQL в хранилище данных SQL Azure для разработки решений.
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
origin.date: 04/17/2017
ms.date: 04/01/2019
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: ee97f7e5cda8b954fb697f73746e416d88d38c2d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61480839"
---
# <a name="assigning-variables-in-azure-sql-data-warehouse"></a>Назначение переменных в хранилище данных SQL Azure

Советы по присваиванию значений переменных T-SQL в хранилище данных SQL Azure для разработки решений.

## <a name="setting-variables-with-declare"></a>Задание переменных с помощью DECLARE

Переменные в хранилище данных SQL задаются с помощью инструкции `DECLARE` или инструкции `SET`. Инициализация переменных с помощью DECLARE — один из наиболее гибких способов задать значение переменной в хранилище данных SQL.

```sql
DECLARE @v  int = 0
;
```

С помощью DECLARE можно задать одновременно несколько переменных. SELECT или UPDATE нельзя использовать для выполнения следующего кода:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Нельзя инициализировать и использовать переменную в одной и той же инструкции DECLARE. Чтобы проиллюстрировать это, ниже приведен **недопустимый** пример, так как @p1 инициализируется и используется в одной и той же инструкции DECLARE. Следующий пример приводит к возникновению ошибки.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Задание значений с помощью SET

SET — это очень распространенный метод задания одной переменной.

Ниже перечислены допустимые способы задания значения с помощью инструкции SET:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

С помощью SET можно одновременно задать только одну переменную. Тем не менее допускаются составные операторы.

## <a name="limitations"></a>Ограничения

Нельзя использовать UPDATE для присваивания значения переменной.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные советы по разработке приведены в [обзоре разработки](sql-data-warehouse-overview-develop.md).
