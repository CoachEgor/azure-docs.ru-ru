---
title: Оптимизация транзакций для хранилища данных SQL Azure | Документация Майкрософт
description: В этой статье приведены сведения о том, как оптимизировать производительность кода обработки транзакций в хранилище данных SQL Azure, чтобы свести к минимуму риск длительных откатов.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/19/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 9ab1da9fce74359448311591986d57abbbcef066
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65873642"
---
# <a name="optimizing-transactions-in-azure-sql-data-warehouse"></a>Оптимизация транзакций для хранилища данных SQL Azure
В этой статье приведены сведения о том, как оптимизировать производительность кода обработки транзакций в хранилище данных SQL Azure, чтобы свести к минимуму риск длительных откатов.

## <a name="transactions-and-logging"></a>Транзакции и ведение журнала
Транзакции представляют собой важную часть ядра реляционной СУБД. Хранилище данных SQL использует транзакции во время изменения данных. Эти транзакции могут быть явными или неявными. Одиночные инструкции INSERT, UPDATE и DELETE являются примерами неявных транзакций. В явных транзакциях используются инструкции BEGIN TRAN, COMMIT TRAN или ROLLBACK TRAN. Явные транзакции обычно используются, когда несколько инструкций изменения нужно связать в одну атомарную единицу. 

Хранилище данных SQL Azure вносит изменения в базу данных с помощью журналов транзакций. У каждого распределения есть свой журнал транзакций. Запись в журналы транзакций выполняется автоматически. Настраивать что-либо не требуется. Этот процесс гарантирует запись, но в то же время увеличивает нагрузку на систему. Чтобы минимизировать этот эффект, нужно написать эффективный код транзакции. Такой код можно условно разделить на две категории.

* Используйте конструкции минимального ведения журналов, когда это возможно.
* Обрабатывайте данные в пакетах, чтобы избежать одиночных длительных транзакций.
* Пользуйтесь шаблоном переключения разделов при внесении значительных изменений в тот или иной раздел.

## <a name="minimal-vs-full-logging"></a>Минимальное ведение журнала и полное ведение журнала
В отличие от полностью регистрируемых операций, которые с помощью журнала транзакций отслеживают каждое изменение строки, минимально регистрируемые операции отслеживают только выделения экстента и изменения метаданных. Иными словами, в рамках минимального ведения журнала протоколируется только та информация, которая нужна, чтобы откатить транзакцию после сбоя или для явного запроса (ROLLBACK TRAN). Так как в журнал транзакций вносится гораздо меньше информации, минимально регистрируемая операция отличается от полностью регистрируемой операции аналогичного размера более высокой производительностью. Кроме того, так как в журнал транзакций вносится меньше записей, создается меньше данных журнала и процесс ввода-вывода становится эффективнее.

Ограничения безопасности транзакций применяются только к полностью регистрируемым операциям.

> [!NOTE]
> Минимально регистрируемые операции могут быть частью явных транзакций. Так как все изменения в структурах выделения отслеживаются, такие операции можно откатывать. 
> 
> 

## <a name="minimally-logged-operations"></a>Минимально регистрируемые операции
Минимально регистрируемыми могут быть такие операции:

* CREATE TABLE AS SELECT ([CTAS](sql-data-warehouse-develop-ctas.md))
* INSERT..SELECT
* CREATE INDEX
* ALTER INDEX REBUILD
* DROP INDEX
* TRUNCATE TABLE
* DROP TABLE
* ALTER TABLE SWITCH PARTITION

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

> [!NOTE]
> Ограничение безопасности транзакций не влияет на внутренние операции перемещения данных (например, BROADCAST и SHUFFLE).
> 
> 

## <a name="minimal-logging-with-bulk-load"></a>Минимальное ведение журнала и массовая загрузка
CTAS и INSERT...SELECT являются операциями массовой загрузки. Тем не менее обе они зависят от определения целевой таблицы и сценария загрузки. В следующей таблице объясняется, в каких случаях операции массовой загрузки будут регистрироваться полностью, а в каких минимально.  

| Первичный индекс | Сценарий загрузки | Режим ведения журнала |
| --- | --- | --- |
| Куча |Любой |**Минимальный** |
| Кластеризованный индекс |Пустая целевая таблица |**Минимальный** |
| Кластеризованный индекс |Загруженные строки не перекрывают существующие страницы в целевом объекте |**Минимальный** |
| Кластеризованный индекс |Загруженные строки перекрывают существующие страницы в целевом объекте |Полное |
| Кластеризованный индекс columnstore |Размер пакета >= 102 400 для распределения с выравниванием по разделам |**Минимальный** |
| Кластеризованный индекс columnstore |Размер пакета < 102 400 для распределения с выравниванием по разделам |Полное |

Следует отметить, что любые записи, выполняемые для обновления вторичных или некластеризованных индексов, считаются полностью регистрируемыми операциями.

> [!IMPORTANT]
> В хранилище данных SQL есть 60 распределений. Поэтому, если исходить из того, что все строки распределены равномерно и их целью является один раздел, пакет должен содержать 6 144 000 строк (или больше), чтобы быть минимально регистрируемым, когда запись выполняется в кластеризованный индекс columnstore. Если таблица секционирована и вставляемые строки охватывают границы секции, потребуется 6 144 000 строк на границу секции (при условии, что данные распределяются равномерно). Чтобы вставка данных в распределении была минимально регистрируемой, размер каждого раздела в каждом распределении должен превышать пороговое значение (102 400 строк).
> 
> 

При загрузке данных в непустую таблицу с кластеризованным индексом некоторые строки могут быть полностью регистрируемыми, а некоторые — минимально. Кластеризованный индекс — это сбалансированное дерево страниц. Если страница, на которой выполняется запись, уже содержит строки другой транзакции, эти операции записи будут полностью регистрируемыми в журнале. Однако если страница пустая, то операция записи на ней будет минимально регистрируемой.

## <a name="optimizing-deletes"></a>Оптимизация операций удаления
DELETE является полностью регистрируемой операцией.  Если необходимо удалить большой объем данных в таблице или секции, часто разумнее применить к данным, которые вы хотите оставить, минимально регистрируемую операцию `SELECT` .  Чтобы выбрать данные, создайте таблицу с помощью [CTAS](sql-data-warehouse-develop-ctas.md).  После этого с помощью [RENAME](/sql/t-sql/statements/rename-transact-sql) замените старую таблицу только что созданной.

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only the records we want to kep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([ProductKey])
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,    20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,    20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,    20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,    20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT     *
FROM     [dbo].[FactInternetSales]
WHERE    [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename the Tables to replace the 
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] TO [FactInternetSales];
```

## <a name="optimizing-updates"></a>Оптимизация операций обновления
UPDATE является полностью регистрируемой операцией.  Если нужно обновить много строк в таблице или секции, для этого, как правило, намного эффективнее использовать минимально регистрируемую операцию, например [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse).

В примере ниже полное обновление таблицы преобразовано в операцию CTAS, что делает возможным минимальное ведение журнала.

В этом случае мы задним числом добавляем сумму скидки к сумме продаж в таблице.

```sql
--Step 01. Create a new table containing the "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(    CLUSTERED INDEX
,    DISTRIBUTION = HASH([ProductKey])
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,    20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,    20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,    20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,    20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,    [OrderDateKey] 
,    [DueDateKey]  
,    [ShipDateKey] 
,    [CustomerKey] 
,    [PromotionKey] 
,    [CurrencyKey] 
,    [SalesTerritoryKey]
,    [SalesOrderNumber]
,    [SalesOrderLineNumber]
,    [RevisionNumber]
,    [OrderQuantity]
,    [UnitPrice]
,    [ExtendedAmount]
,    [UnitPriceDiscountPct]
,    ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,    [ProductStandardCost]
,    [TotalProductCost]
,    ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,    [TaxAmt]
,    [Freight]
,    [CarrierTrackingNumber] 
,    [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename the tables
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] TO [FactInternetSales];

--Step 03. Drop the old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [!NOTE]
> Повторное создание больших таблиц может быть удобнее, если использовать функции управления рабочими нагрузками хранилища данных SQL. Дополнительные сведения см. в разделе [Классы ресурсов для управления рабочими нагрузками](resource-classes-for-workload-management.md).
> 
> 

## <a name="optimizing-with-partition-switching"></a>Оптимизация с помощью переключения секций
Если вы столкнулись с крупномасштабными изменениями в [секции таблицы](sql-data-warehouse-tables-partition.md), имеет смысл воспользоваться схемой переключения секций. Если изменения данных существенные и охватывают несколько секций, итерация по секциям приведет к тому же результату.

Чтобы переключить разделы, выполните следующие действия.

1. Создайте пустой выходной раздел.
2. Выполните обновление в виде операции CTAS.
3. Переключите существующие данные на выходную таблицу.
4. Подключите новые данные.
5. Очистите данные.

Тем не менее нужно уметь определять разделы, которые следует переключить. Для этого создайте следующую вспомогательную процедуру.  

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,    @table_name               NVARCHAR(128)
,    @boundary_value           INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (    DISTRIBUTION = ROUND_ROBIN
        ,    HEAP
        )
AS
WITH CTE
AS
(
SELECT     s.name                            AS [schema_name]
,        t.name                            AS [table_name]
,         p.partition_number                AS [ptn_nmbr]
,        p.[rows]                        AS [ptn_rows]
,        CAST(r.[value] AS INT)            AS [boundary_value]
FROM        sys.schemas                    AS s
JOIN        sys.tables                    AS t    ON  s.[schema_id]        = t.[schema_id]
JOIN        sys.indexes                    AS i    ON     t.[object_id]        = i.[object_id]
JOIN        sys.partitions                AS p    ON     i.[object_id]        = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes        AS h    ON     i.[data_space_id]    = h.[data_space_id]
JOIN        sys.partition_functions        AS f    ON     h.[function_id]        = f.[function_id]
LEFT JOIN    sys.partition_range_values    AS r     ON     f.[function_id]        = r.[function_id] 
                                                AND r.[boundary_id]        = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT    *
FROM    CTE
WHERE    [schema_name]        = @schema_name
AND        [table_name]        = @table_name
AND        [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Эта процедура увеличивает число повторных использований кода и делает пример переключения секций более компактным.

В приведенном ниже примере кода показаны действия для получения полноценной процедуры переключения разделов.

```sql
--Create a partitioned aligned empty table to switch out the data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(    DISTRIBUTION = HASH([ProductKey])
,    CLUSTERED COLUMNSTORE INDEX
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update the data in the select portion of the CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(    DISTRIBUTION = HASH([ProductKey])
,    CLUSTERED COLUMNSTORE INDEX
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,    [OrderDateKey] 
,    [DueDateKey]  
,    [ShipDateKey] 
,    [CustomerKey] 
,    [PromotionKey] 
,    [CurrencyKey] 
,    [SalesTerritoryKey]
,    [SalesOrderNumber]
,    [SalesOrderLineNumber]
,    [RevisionNumber]
,    [OrderQuantity]
,    [UnitPrice]
,    [ExtendedAmount]
,    [UnitPriceDiscountPct]
,    ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,    [ProductStandardCost]
,    [TotalProductCost]
,    ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,    [TaxAmt]
,    [Freight]
,    [CarrierTrackingNumber] 
,    [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE    OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use the helper procedure to identify the partitions
--The source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--The "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--The "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch the partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]    SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))    +' TO [dbo].[FactInternetSales_out] PARTITION '    +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))    +' TO [dbo].[FactInternetSales] PARTITION '        +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform the clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>Сведение к минимуму ведения журнала с помощью небольших пакетов
Операции по изменению больших объемов данных бывает полезно разбить на блоки или пакеты, чтобы охватить единицу работы.

Ниже приведен рабочий пример кода. В целях демонстрации указан небольшой размер пакета. В реальной среде он был бы гораздо больше. 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (    DISTRIBUTION = ROUND_ROBIN
        ,    HEAP
        )
AS
SELECT    ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,        SalesOrderNumber
,        SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE    [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE    @seq_start        INT = 1
,        @batch_iterator    INT = 1
,        @batch_size        INT = 50
,        @max_seq_nmbr    INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE    @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,        @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE    @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT    1
            FROM    #t t
            WHERE    seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND        FactInternetSales.SalesOrderNumber        = t.SalesOrderNumber
            AND        FactInternetSales.SalesOrderLineNumber    = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Рекомендации по приостановке и масштабированию
Работу хранилища данных SQL Azure можно по запросу [приостановить, возобновить. Также его масштабировать.](sql-data-warehouse-manage-compute-overview.md) Когда вы приостанавливаете работу хранилища данных SQL или масштабируете его, важно понимать, что запущенные транзакции немедленно прекращаются, что, в свою очередь, откатывает открытые транзакции. Если в рамках рабочей нагрузки выполнено длительное и незавершенное изменение данных перед операцией приостановки или масштабирования, это изменение нужно отменить. Такая отмена изменений может повлиять на время, требующееся для приостановки или масштабирования базы данных хранилища данных SQL Azure. 

> [!IMPORTANT]
> `UPDATE` и `DELETE` — это полностью регистрируемые операции, поэтому упомянутые операции отмены и повтора могут выполняться значительно дольше, чем такие же минимально регистрируемые операции. 
> 
> 

Лучше всего перед приостановкой работы или масштабированием хранилища данных SQL подождать, пока не завершатся текущие транзакции по изменению данных. Но это не всегда удобно. Чтобы снизить риск длительного отката, выполните одно из следующих действий.

* Повторно создайте долго выполняющиеся операции, используя [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse).
* Разбейте операцию на блоки для работы с подмножествами строк.

## <a name="next-steps"></a>Дальнейшие действия
Сведения об уровнях изоляции и ограничениях транзакций см. в статье [Транзакции в хранилище данных SQL](sql-data-warehouse-develop-transactions.md).  Рекомендации по использованию хранилища данных SQL Azure см. в [этой статье](sql-data-warehouse-best-practices.md).

