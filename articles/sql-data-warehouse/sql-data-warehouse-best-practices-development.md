---
title: Рекомендации по разработке для Хранилища данных SQL Azure | Документация Майкрософт
description: Рекомендации, которые следует учитывать при разработке решений для хранилища данных SQL Azure.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 09/04/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 76297be79fca62b1f2f777f9cba4a0a8fe134768
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65851637"
---
# <a name="development-best-practices-for-azure-sql-data-warehouse"></a>Рекомендации по разработке для Хранилища данных SQL Azure
В этой статье приводятся руководство и рекомендации по разработке решения хранилища данных. 

## <a name="reduce-cost-with-pause-and-scale"></a>Снижение расходов за счет приостановки и масштабирования ресурсов
Дополнительные сведения о сокращении затрат при помощи приостановки и масштабирования см. в статье об [управлении вычислительными ресурсами](sql-data-warehouse-manage-compute-overview.md). 


## <a name="maintain-statistics"></a>Обеспечение статистики
Убедитесь, что статистика обновляется ежедневно или после каждой загрузки.  Однако всегда есть компромиссы между производительностью и затратами на создание и обновление статистики. Если ведение статистики занимает слишком много времени, нужно выбирать отдельные столбцы, для которых необходимо создавать статистику, или столбцы, требующие частого обновления статистики.  Например, можно ежедневно обновлять столбцы дат, в которые добавляются новые значения. **Статистику рекомендуется вести в столбцах, которые являются частью объединения, используются в предложении WHERE или GROUP BY**.

Дополнительные сведения см. в статьях [Управление статистикой таблиц в хранилище данных SQL][Manage table statistics], [CREATE STATISTICS (Transact-SQL)][CREATE STATISTICS] и [UPDATE STATISTICS (Transact-SQL)][UPDATE STATISTICS].

## <a name="hash-distribute-large-tables"></a>Хэш-распределение больших таблиц
По умолчанию таблицы распределяются по методу циклического перебора.  Это позволяет упростить процесс создания таблиц, так как пользователям не нужно принимать решение о типе распределения.  Таблицы с распределением по методу циклического перебора могут вполне годиться для некоторых рабочих нагрузок, но в большинстве случаев намного эффективнее использовать столбец распределения.  Наглядно это превосходство можно увидеть при объединении больших таблиц фактов.  Например, во время выполнения запроса на объединение таблицы Orders, распределенной по идентификатору order_id, с таблицей Transactions, распределенной по тому же идентификатору, этот запрос превращается в запрос к серверу, что исключает выполнение операций перемещения данных.  Чем меньше в запросе действий, тем быстрее он выполняется.  Скорость выполнения запроса также зависит от объема перемещаемых данных.  В этом разделе представлены только общие сведения. При загрузке распределенной таблицы входящие данные не должны быть отсортированы по ключу распределения, так как это замедлит процесс загрузки.  Ниже приведены ссылки на статьи, содержащие сведения о том, как с помощью столбца распределения можно улучшить производительность и как определить распределенную таблицу в предложение WITH инструкции CREATE TABLES.

Дополнительные сведения см. в статьях [Общие сведения о таблицах в хранилище данных SQL][Table overview], [Распределение таблиц в хранилище данных SQL][Table distribution], [Создание таблицы (хранилище данных Azure SQL)][CREATE TABLE] и [Создание TABLE AS SELECT (хранилище данных Azure SQL)][CREATE TABLE AS SELECT], а также в статье, посвященной [выбору распределения таблицы][Selecting table distribution].

## <a name="do-not-over-partition"></a>Недопущение избыточного секционирования
Не смотря на то, что секционирование данных — это эффективный способ управления данными, который реализуется благодаря переключению секций или оптимизации сканирования путем исключения секций, наличие большого количества секций может повлиять на производительность запросов.  Стратегия разделения данных на большое количество секций, как правило, эффективна в SQL Server, но она не всегда работает в хранилище данных SQL.  Слишком большое количество секций снижает эффективность кластеризованных индексов Columnstore, если в секции содержится менее миллиона строк.  Помните, что в хранилище данных SQL данные секционируются на 60 баз данных, поэтому при создании таблицы с 100 секциями фактически будет создана таблица с 6000 секций.  Все рабочие нагрузки отличаются, поэтому рекомендуется поэкспериментировать с секционированием, чтобы выбрать наиболее подходящее количество секций для вашей рабочей нагрузки.  В хранилище данных SQL можно использовать меньшее количество секций, чем в SQL Server.  Например, попробуйте использовать еженедельные или ежемесячные секции вместо ежедневных.

Дополнительные сведения см. в статье [Секционирование таблиц в хранилище данных SQL][Table partitioning].

## <a name="minimize-transaction-sizes"></a>Уменьшение размера транзакций
Инструкции INSERT, UPDATE и DELETE выполняются в транзакциях. В случае сбоя их необходимо откатить.  Чтобы сократить время выполнения отката, необходимо по возможности уменьшить размеры транзакций.  Это можно сделать, разделив инструкции INSERT, UPDATE и DELETE на части.  Например, если на выполнение инструкции INSERT требуется 1 час, рекомендуется разделить ее на 4 части. Таким образом, каждая часть будет выполняться 15 минут.  К пустым таблицам можно применять специальные операции, которые сопровождаются записью в журнал минимальных сведений, (такие как CTAS, TRUNCATE, DROP TABLE или INSERT), чтобы снизить риск отката.  Устранить откаты также можно, используя для управления данными только операции с метаданными (например, переключение секций).  Например, вместо выполнения инструкции DELETE для удаления всех строк в таблице, упорядоченной по идентификатору order_date (октябрь 2001 г.), данные можно секционировать ежемесячно, а потом переключить секцию с данными на пустую секцию из другой таблицы (см. примеры использования инструкции ALTER TABLE).  Используя инструкцию CTAS вместо DELETE, можно записать данные, которые необходимо сохранить в несекционированной таблице.  Если на выполнение этих двух инструкций требуется одинаковое количество времени, целесообразно использовать инструкцию CTAS, так как для ее выполнения необходима минимальная нагрузка ведения журнала транзакций, и ее можно быстро отменить.

Дополнительные сведения см. в статьях [Транзакции в хранилище данных SQL][Understanding transactions], [Оптимизация транзакций для хранилища данных SQL][Optimizing transactions], [Секционирование таблиц в хранилище данных SQL][Table partitioning], а также в статьях об инструкциях [TRUNCATE TABLE][TRUNCATE TABLE] и [ALTER TABLE][ALTER TABLE] и статье [Функция Create Table As Select (CTAS) в хранилище данных SQL][Create table as select (CTAS)].

## <a name="use-the-smallest-possible-column-size"></a>Использование минимального размера столбца
При определении DDL рекомендуется использовать поддерживаемый тип данных с наименьшим размером. Это позволит повысить производительность запросов.  Это особенно важно для столбцов CHAR и VARCHAR.  Если самое длинное значение в столбце состоит из 25 знаков, столбец необходимо определить как VARCHAR(25).  Не рекомендуется использовать по умолчанию длинные значения столбцов.  Кроме того, по возможности определяйте столбцы как VARCHAR, а не NVARCHAR.

Дополнительные сведения см. в статьях, посвященных [общим сведениям о таблицах][Table overview], [типам данных таблиц][Table data types] и [инструкции CREATE TABLE][CREATE TABLE].

## <a name="optimize-clustered-columnstore-tables"></a>Оптимизация таблиц с кластеризованными индексами columnstore
Использование кластеризованных индексов Columnstore — это один из наиболее эффективных способов хранения данных в хранилище данных SQL.  По умолчанию в хранилище данных SQL используются таблицы с кластеризованными индексами Columnstore.  Качество кластеризованного сегмента Columnstore существенно влияет на эффективность выполнения запросов в таблицах с кластеризованными индексами Columnstore.  Если во время записи строк в таблицы Columnstore возникает нехватка памяти, качество сегмента Columnstore может ухудшиться.  Качество сегмента можно изменить по числу строк в сжатой группе строк.  Дополнительные сведения об определении и улучшении качества сегмента для таблиц с кластеризованными индексами columnstore см. в разделе [Причины низкого качества индекса Columnstore][Causes of poor columnstore index quality] статьи [Индексирование таблиц в хранилище данных SQL][Table indexes].  Так как высококачественные сегменты columnstore важны, рекомендуется использовать идентификаторы пользователей, которым назначен класс ресурсов среднего или большого размера для загрузки данных. Использование [единиц использования хранилища данных](what-is-a-data-warehouse-unit-dwu-cdwu.md) меньшего размера требует присвоения пользователю загрузки класса ресурсов большего размера.

В основном таблицы Columnstore не передают данные в сжатый сегмент Columnstore, пока количество строк в каждой таблице не превысит миллион, а каждая таблица хранилища данных SQL не будет секционирована на 60 таблиц. Поэтому нецелесообразно применять к таблицам Columnstore запросы до тех пор, пока количество строк в таблице не превысит 60 миллионов.  Поэтому нецелесообразно применять к таблицам Columnstore запросы до тех пор, пока количество строк в таблице не превысит 60 миллионов.  Для таблицы с количеством строк менее 60 миллионов бессмысленно использовать индекс Columnstore, хотя он и не повлияет на производительность.  Более того, чтобы воспользоваться преимуществами кластеризованного индекса Columnstore при секционировании данных, каждая секция должна состоять из миллиона строк.  Если таблица состоит из 100 секций, чтобы использовать кластеризованный индекс Columnstore, она должна состоять из как минимум 6 миллиардов строк (60 распределений * 100 секций * 1 миллион строк).  Если таблица не содержит такого количества строк, рекомендуется уменьшить количество секций или использовать таблицу без кластеризованных индексов.  Чтобы получить более высокую производительность, возможно, вместо кластеризованной таблицы стоит использовать таблицу без кластеризованных индексов, содержащую вторичные индексы.

Если выбрать только необходимые столбцы, запросы к таблице ColumnStore будут выполняться быстрее.  

Ознакомьтесь также со статьями [Индексирование таблиц в хранилище данных SQL][Table indexes], [Руководство по индексам columnstore][Columnstore indexes guide] и разделом с [описанием повторной сборки индексов columnstore][Rebuilding columnstore indexes].

## <a name="next-steps"></a>Дальнейшие действия
Если вы не нашли нужных сведений в этой статье, попробуйте использовать поиск по документации в левой части страницы, чтобы найти все документы, связанные с хранилищем данных SQL Azure.  На [форуме по хранилищу данных SQL Azure][Azure SQL Data Warehouse MSDN Forum] можно задать вопросы другим пользователям и разработчикам хранилища данных SQL.  Мы регулярно просматриваем этот форум и следим за тем, чтобы другие пользователи или наши специалисты ответили на интересующие вас вопросы.  Вопросы также можно задавать на [форуме по хранилищу данных SQL Azure на сайте Stack Overflow][Azure SQL Data Warehouse Stack Overflow Forum].

Наконец, используйте страницу [отзывов о хранилище данных SQL Azure][Azure SQL Data Warehouse Feedback], чтобы оставить запросы на функции.  Ваши отзывы и голоса за отзывы, оставленные другими пользователями, помогут нам определить, какие улучшения функций наиболее приоритетные.

<!--Image references-->

<!--Article references-->
[Create a support ticket]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Create table as select (CTAS)]: ./sql-data-warehouse-develop-ctas.md
[Table overview]: ./sql-data-warehouse-tables-overview.md
[Table data types]: ./sql-data-warehouse-tables-data-types.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexes]: ./sql-data-warehouse-tables-index.md
[Causes of poor columnstore index quality]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuilding columnstore indexes]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Manage table statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary tables]: ./sql-data-warehouse-tables-temporary.md
[Guide for using PolyBase]: ./guidance-for-loading-data.md
[Load data]: ./design-elt-data-loading.md
[Move data with Azure Data Factory]: ../data-factory/transform-data-using-machine-learning.md
[Load data with Azure Data Factory]: ../data-factory/load-azure-sql-data-warehouse.md
[Load data with bcp]: /sql/tools/bcp-utility
[Load data with PolyBase]: ./load-data-wideworldimportersdw.md
[Monitor your workload using DMVs]: ./sql-data-warehouse-manage-monitor.md
[Pause compute resources]: ./sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Resume compute resources]: ./sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[Scale compute resources]: ./sql-data-warehouse-manage-compute-overview.md#scale-compute
[Understanding transactions]: ./sql-data-warehouse-develop-transactions.md
[Optimizing transactions]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Troubleshooting]: ./sql-data-warehouse-troubleshoot.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->
[ALTER TABLE]: https://msdn.microsoft.com/library/ms190273.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[CREATE TABLE AS SELECT]: https://msdn.microsoft.com/library/mt204041.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: https://msdn.microsoft.com/library/mt204017.aspx
[INSERT]: https://msdn.microsoft.com/library/ms174335.aspx
[OPTION]: https://msdn.microsoft.com/library/ms190322.aspx
[TRUNCATE TABLE]: https://msdn.microsoft.com/library/ms177570.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx
[sys.dm_exec_sessions]: https://msdn.microsoft.com/library/ms176013.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_request_steps]: https://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: https://msdn.microsoft.com/library/mt203889.aspx
[sys.dm_pdw_dms_workers]: https://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_waits]: https://msdn.microsoft.com/library/mt203893.aspx
[Columnstore indexes guide]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
[Selecting table distribution]: https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/
[Azure SQL Data Warehouse Feedback]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Azure SQL Data Warehouse MSDN Forum]: https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse
[Azure SQL Data Warehouse Stack Overflow Forum]:  https://stackoverflow.com/questions/tagged/azure-sqldw
[Azure SQL Data Warehouse loading patterns and strategies]: https://blogs.msdn.microsoft.com/sqlcat/20../../azure-sql-data-warehouse-loading-patterns-and-strategies/
