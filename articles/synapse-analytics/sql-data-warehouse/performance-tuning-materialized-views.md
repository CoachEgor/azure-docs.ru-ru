---
title: Performance tuning with materialized views (Настройка производительности с помощью материализованных представлений)
description: Рекомендации и соображения, которые следует знать при использовании материализованных представлений для повышения производительности запросов.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 09/05/2019
ms.author: xiaoyul
ms.reviewer: nibruno; jrasnick
ms.openlocfilehash: 6a3235d5edc5249bbbdc2e79dac8575ad26fd5e1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81417031"
---
# <a name="performance-tuning-with-materialized-views"></a>Performance tuning with materialized views (Настройка производительности с помощью материализованных представлений)

Материализованные представления в пуле синапсе SQL предоставляют метод низкого уровня обслуживания для сложных аналитических запросов, чтобы обеспечить высокую производительность без изменения запроса. В этой статье рассматриваются общие рекомендации по использованию материализованных представлений.

Материализованные представления в пуле SQL предоставляют низкий способ обслуживания сложных аналитических запросов, чтобы обеспечить высокую производительность без изменения запроса. В этой статье рассматриваются общие рекомендации по использованию материализованных представлений.

## <a name="materialized-views-vs-standard-views"></a>Материализованные представления и стандартные представления

Пул SQL поддерживает стандартные и материализованные представления.  Оба являются виртуальными таблицами, созданными с помощью выражений SELECT и представленными для запросов в виде логических таблиц.  Представления инкапсулируют сложность общих вычислений данных и добавляют уровень абстракции к вычислительным изменениям, поэтому нет необходимости переписывать запросы.  

Стандартное представление каждый раз рассчитывает данные при каждом использовании представления.  На диске не хранятся данные. Обычно пользователи используют стандартные представления в качестве средства, помогающего организовать логические объекты и запросы в базе данных.  Чтобы использовать стандартное представление, запрос должен создать прямую ссылку на него.

Материализованные представления предварительно вычисляются, сохраняют и сохраняют свои данные в пуле SQL так же, как и таблицы.  При использовании материализованных представлений повторное вычисление не требуется.  Именно поэтому запросы, использующие все или подмножество данных в материализованных представлениях, могут повысить производительность.  Более того, запросы могут использовать материализованные представления, не внося прямые ссылки на него, поэтому нет необходимости изменять код приложения.  

Большинство требований к стандартному представлению по-прежнему применимо к материализованным представлениям. Дополнительные сведения о синтаксисе материализованных представлений и других требованиях см. в статье [Создание материализованных ПРЕДСТАВЛЕНИЙ как SELECT](/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) .

| Сравнение                     | Просмотр                                         | Материализованное представление
|:-------------------------------|:---------------------------------------------|:--------------------------------------------------------------|
|Просмотр определений                 | Хранится в пуле SQL.              | Хранится в пуле SQL.
|Просмотр содержимого                    | Создается каждый раз при использовании представления.   | Предварительно обработаны и хранятся в пуле SQL во время создания представления. Обновляются по мере добавления данных в базовые таблицы.
|Обновление данных                    | Всегда Обновлено                               | Всегда Обновлено
|Скорость получения данных представления из сложных запросов     | Медленно                                         | Быстрый  
|Дополнительный объем хранилища                   | Нет                                           | Да
|Синтаксис                          | CREATE VIEW                                  | СОЗДАТЬ МАТЕРИАЛИЗОВАННЫЙ ВИД КАК SELECT

## <a name="benefits-of-using-materialized-views"></a>Преимущества использования материализованных представлений

Правильно спроектированное материализованные представление предоставляет следующие преимущества.

- Сократите время выполнения сложных запросов с помощью соединений и агрегатных функций. Чем сложнее запрос, тем выше вероятность сохранения во время выполнения. Наиболее выгодное преимущество заключается в том, что стоимость вычислений запроса высока и результирующий набор данных мал.  
- Оптимизатор в пуле SQL может автоматически использовать развернутые материализованные представления для улучшения планов выполнения запросов.  Этот процесс является прозрачным для пользователей, обеспечивающих более высокую производительность запросов и не требующий прямой ссылки на материализованные представления.
- Требовать низкое обслуживание представлений.  Все изменения добавочных данных из базовых таблиц автоматически добавляются в материализованные представления в синхронном режиме.  Такая схема позволяет выполнять запросы материализованных представлений для возврата тех же данных, что и непосредственное выполнение запросов к базовым таблицам.
- Данные в материализованных представлениях можно распределять по-разному из базовых таблиц.  
- Данные в материализованных представлениях получают те же высокие преимущества и надежность, что и данные в обычных таблицах.  

Материализованные представления, реализованные в пуле SQL, также предоставляют следующие дополнительные преимущества.

По сравнению с другими поставщиками хранилища данных материализованные представления, реализованные в хранилище данных SQL Azure, также предоставляют следующие дополнительные преимущества:

- Автоматическое и синхронное обновление данных с изменениями данных в базовых таблицах. Вмешательство пользователя не требуется.
- Широкая поддержка агрегатных функций. См. раздел [CREATE материализованный вид как SELECT (Transact-SQL)](/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).
- Поддержка конкретной рекомендации по материализованным представлениям запросов.  См. раздел [объяснить (Transact-SQL)](/sql/t-sql/queries/explain-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="common-scenarios"></a>Распространенные сценарии  

Материализованные представления обычно используются в следующих сценариях:

**Необходимость повышения производительности сложных аналитических запросов к большим объемам данных**

Сложные аналитические запросы обычно используют больше агрегатных функций и соединений таблиц, что приводит к большим объемам вычислительных операций, таким как перезапись в случайный порядок и соединение при выполнении запроса.  Именно поэтому выполнение запросов занимает больше времени, чем в больших таблицах.  

Пользователи могут создавать материализованные представления для данных, возвращаемых из общих вычислений запросов, поэтому нет необходимости выполнять перерасчеты, если эти данные необходимы для запросов, что позволяет снизить стоимость вычислений и ускорить отклики на запросы.

**Требуется более высокая производительность при отсутствии или минимальных изменениях запроса**

Изменения в схемах и запросах в пулах SQL обычно хранятся как минимум для поддержки регулярных операций ETL и создания отчетов.  Пользователи могут использовать материализованные представления для настройки производительности запросов, если затраты, понесенные представлениями, могут быть смещены по росту производительности запросов.

В сравнении с другими параметрами настройки, такими как управление масштабированием и статистикой, это значительно менее интенсивное изменение рабочей среды для создания и обслуживания материализованных представлений, а также его потенциального выигрыша в производительности.

- Создание или обслуживание материализованных представлений не влияет на запросы, выполняемые с базовыми таблицами.
- Оптимизатор запросов может автоматически использовать развернутые материализованные представления без прямой ссылки на просмотр в запросе. Эта возможность снижает потребность в изменении запроса при настройке производительности.

**Необходима другая стратегия распределения данных для повышения производительности запросов.**

Пул SQL — это распределенная система массовой параллельной обработки (MPP).   Данные в таблице пула SQL распределяются между узлами по 60, используя одну из трех [стратегий распределения](sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) (хэширование, round_robin или репликация).  

Распределение данных указывается в момент создания таблицы и остается неизменным до тех пор, пока таблица не будет удалена. Материализованные представления, являющиеся виртуальными таблицами на диске, поддерживают распределение данных по хэшу и round_robin.  Пользователи могут выбрать распределение данных, отличное от базовых таблиц, но оптимальное для производительности запросов, использующих эти представления.  

## <a name="design-guidance"></a>Руководство по проектированию

Ниже приведены общие рекомендации по использованию материализованных представлений для повышения производительности запросов.

**Разработка для рабочей нагрузки**

Прежде чем приступать к созданию материализованных представлений, важно иметь глубокое понимание рабочей нагрузки с точки зрения шаблонов запросов, важности, частоты и размера результирующих данных.  

Пользователи могут выполнять объяснения WITH_RECOMMENDATIONS <SQL_statement> для материализованных представлений, рекомендованных оптимизатором запросов.  Так как эти рекомендации относятся к конкретным запросам, материализованный вид, который выгодно для одного запроса, может быть неоптимальным для других запросов в той же рабочей нагрузке.  

Оцените эти рекомендации с учетом потребностей рабочей нагрузки.  Идеальными материализованными представлениями являются преимущества, которые приводят к повышению производительности рабочей нагрузки.  

**Учитывайте компромисс между быстрыми запросами и стоимостью**

Для каждого материализованных представлений имеется стоимость хранения данных и стоимость обслуживания представления.  При изменении данных в базовых таблицах размер материализованных представлений увеличивается, а его физическая структура также изменяется.  Чтобы избежать снижения производительности запросов, каждое материализованные представление сохраняется отдельно в подсистеме пула SQL.  

Рабочая нагрузка обслуживания увеличивается при увеличении числа материализованных представлений и изменений базовой таблицы.   Пользователи должны проверить, могут ли затраты, понесенные во все материализованные представления, быть смещены на рост производительности запроса.  

Этот запрос можно выполнить для списка материализованных представлений в базе данных:

```sql
SELECT V.name as materialized_view, V.object_id
FROM sys.views V
JOIN sys.indexes I ON V.object_id= I.object_id AND I.index_id < 2;
```

Параметры для сокращения числа материализованных представлений:

- Выявление общих наборов данных, часто используемых сложными запросами в рабочей нагрузке.  Создавайте материализованные представления для хранения этих наборов данных, чтобы оптимизатор мог использовать их в качестве стандартных блоков при создании планов выполнения.  

- Удалите материализованные представления, которые имеют низкий уровень использования или больше не нужны.  Отключенное материализованные представление не поддерживается, но по-прежнему приводит к затратам на хранение.  

- Объедините материализованные представления, созданные в тех же или аналогичных базовых таблицах, даже если их данные не пересекаются.  Объединение материализованных представлений может привести к увеличению размера представления, чем сумма отдельных представлений, однако стоимость обслуживания представления должна сократиться.  Пример:

```sql

-- Query 1 would benefit from having a materialized view created with this SELECT statement

SELECT A, SUM(B)
FROM T
GROUP BY A

-- Query 2 would benefit from having a materialized view created with this SELECT statement

SELECT C, SUM(D)
FROM T
GROUP BY C

-- You could create a single mateiralized view of this form

SELECT A, C, SUM(B), SUM(D)
FROM T
GROUP BY A, C

```

**Не все настройки производительности требуют изменения запроса.**

Оптимизатор пула SQL может автоматически использовать развернутые материализованные представления для повышения производительности запросов.  Эта поддержка прозрачно применяется к запросам, которые не ссылаются на представления и запросы, использующие агрегаты, не поддерживаемые при создании материализованных представлений.  Изменение запроса не требуется. Вы можете проверить предполагаемый план выполнения запроса, чтобы подтвердить использование материализованных представлений.  

**Мониторинг материализованных представлений**

Материализованный вид хранится в пуле SQL так же, как таблица с кластеризованным индексом columnstore (CCI).  Чтение данных из материализованных представлений включает сканирование сегментов индекса CCI и применение добавочных изменений из базовых таблиц. Если число добавочных изменений слишком велико, разрешение запроса из материализованных представлений может занять больше времени, чем непосредственное выполнение запросов к базовым таблицам.  

Чтобы избежать снижения производительности запросов, рекомендуется выполнить [инструкцию DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD](/sql/t-sql/database-console-commands/dbcc-pdw-showmaterializedviewoverhead-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) для мониторинга overhead_ratio представления (total_rows/Max (1, base_view_row)).  Пользователи должны перестроить материализованные представления, если его overhead_ratio слишком высокий.

**Материализованные представления и кэширование результирующего набора**

Эти две функции представлены в пуле SQL примерно в одно и то же время для настройки производительности запросов.  Кэширование результирующего набора используется для получения высокого параллелизма и быстрого ответа от повторяющихся запросов к статическим данным.  

Чтобы использовать кэшированный результат, форма запроса кэша должна совпадать с запросом, создавшим кэш.  Кроме того, кэшированный результат должен применяться ко всему запросу.  

Материализованные представления позволяют изменять данные в базовых таблицах.  Данные в материализованных представлениях могут быть применены к части запроса.  Эта поддержка позволяет использовать одни и те же материализованные представления для разных запросов, которые используют некоторые вычисления для повышения производительности.

## <a name="example"></a>Пример

В этом примере используется запрос, подобный ТПКДС, который находит клиентов, занимающих больше денег по каталогу, чем в магазинах, и определяет предпочтительных клиентов и их страну происхождения.   Запрос включает выбор первых 100 записей из объединения трех вложенных инструкций SELECT, включающих SUM () и GROUP BY.

```sql
WITH year_total AS (
SELECT c_customer_id customer_id
       ,c_first_name customer_first_name
       ,c_last_name customer_last_name
       ,c_preferred_cust_flag customer_preferred_cust_flag
       ,c_birth_country customer_birth_country
       ,c_login customer_login
       ,c_email_address customer_email_address
       ,d_year dyear
       ,sum(isnull(ss_ext_list_price-ss_ext_wholesale_cost-ss_ext_discount_amt+ss_ext_sales_price, 0)/2) year_total
       ,'s' sale_type
FROM customer
     ,store_sales
     ,date_dim
WHERE c_customer_sk = ss_customer_sk
   AND ss_sold_date_sk = d_date_sk
GROUP BY c_customer_id
         ,c_first_name
         ,c_last_name
         ,c_preferred_cust_flag
         ,c_birth_country
         ,c_login
         ,c_email_address
         ,d_year
UNION ALL
SELECT c_customer_id customer_id
       ,c_first_name customer_first_name
       ,c_last_name customer_last_name
       ,c_preferred_cust_flag customer_preferred_cust_flag
       ,c_birth_country customer_birth_country
       ,c_login customer_login
       ,c_email_address customer_email_address
       ,d_year dyear
       ,sum(isnull(cs_ext_list_price-cs_ext_wholesale_cost-cs_ext_discount_amt+cs_ext_sales_price, 0)/2) year_total
       ,'c' sale_type
FROM customer
     ,catalog_sales
     ,date_dim
WHERE c_customer_sk = cs_bill_customer_sk
   AND cs_sold_date_sk = d_date_sk
GROUP BY c_customer_id
         ,c_first_name
         ,c_last_name
         ,c_preferred_cust_flag
         ,c_birth_country
         ,c_login
         ,c_email_address
         ,d_year
UNION ALL
SELECT c_customer_id customer_id
       ,c_first_name customer_first_name
       ,c_last_name customer_last_name
       ,c_preferred_cust_flag customer_preferred_cust_flag
       ,c_birth_country customer_birth_country
       ,c_login customer_login
       ,c_email_address customer_email_address
       ,d_year dyear
       ,sum(isnull(ws_ext_list_price-ws_ext_wholesale_cost-ws_ext_discount_amt+ws_ext_sales_price, 0)/2) year_total
       ,'w' sale_type
FROM customer
     ,web_sales
     ,date_dim
WHERE c_customer_sk = ws_bill_customer_sk
   AND ws_sold_date_sk = d_date_sk
GROUP BY c_customer_id
         ,c_first_name
         ,c_last_name
         ,c_preferred_cust_flag
         ,c_birth_country
         ,c_login
         ,c_email_address
         ,d_year
         )
  SELECT TOP 100
                  t_s_secyear.customer_id
                 ,t_s_secyear.customer_first_name
                 ,t_s_secyear.customer_last_name
                 ,t_s_secyear.customer_birth_country
FROM year_total t_s_firstyear
     ,year_total t_s_secyear
     ,year_total t_c_firstyear
     ,year_total t_c_secyear
     ,year_total t_w_firstyear
     ,year_total t_w_secyear
WHERE t_s_secyear.customer_id = t_s_firstyear.customer_id
   AND t_s_firstyear.customer_id = t_c_secyear.customer_id
   AND t_s_firstyear.customer_id = t_c_firstyear.customer_id
   AND t_s_firstyear.customer_id = t_w_firstyear.customer_id
   AND t_s_firstyear.customer_id = t_w_secyear.customer_id
   AND t_s_firstyear.sale_type = 's'
   AND t_c_firstyear.sale_type = 'c'
   AND t_w_firstyear.sale_type = 'w'
   AND t_s_secyear.sale_type = 's'
   AND t_c_secyear.sale_type = 'c'
   AND t_w_secyear.sale_type = 'w'
   AND t_s_firstyear.dyear+0 =  1999
   AND t_s_secyear.dyear+0 = 1999+1
   AND t_c_firstyear.dyear+0 =  1999
   AND t_c_secyear.dyear+0 =  1999+1
   AND t_w_firstyear.dyear+0 = 1999
   AND t_w_secyear.dyear+0 = 1999+1
   AND t_s_firstyear.year_total > 0
   AND t_c_firstyear.year_total > 0
   AND t_w_firstyear.year_total > 0
   AND CASE WHEN t_c_firstyear.year_total > 0 THEN t_c_secyear.year_total / t_c_firstyear.year_total ELSE NULL END
           > CASE WHEN t_s_firstyear.year_total > 0 THEN t_s_secyear.year_total / t_s_firstyear.year_total ELSE NULL END
   AND CASE WHEN t_c_firstyear.year_total > 0 THEN t_c_secyear.year_total / t_c_firstyear.year_total ELSE NULL END
           > CASE WHEN t_w_firstyear.year_total > 0 THEN t_w_secyear.year_total / t_w_firstyear.year_total ELSE NULL END
ORDER BY t_s_secyear.customer_id
         ,t_s_secyear.customer_first_name
         ,t_s_secyear.customer_last_name
         ,t_s_secyear.customer_birth_country
OPTION ( LABEL = 'Query04-af359846-253-3');
```

Проверьте предполагаемый план выполнения запроса.  18 в случайном порядке и 17 операций присоединяется, что требует больше времени на выполнение. Теперь давайте создадим одно материализованные представление для каждой из трех инструкций подзапроса ВЫБОРки.

```sql
CREATE materialized view nbViewSS WITH (DISTRIBUTION=HASH(customer_id)) AS
SELECT c_customer_id customer_id
       ,c_first_name customer_first_name
       ,c_last_name customer_last_name
       ,c_preferred_cust_flag customer_preferred_cust_flag
       ,c_birth_country customer_birth_country
       ,c_login customer_login
       ,c_email_address customer_email_address
       ,d_year dyear
       ,sum(isnull(ss_ext_list_price-ss_ext_wholesale_cost-ss_ext_discount_amt+ss_ext_sales_price, 0)/2) year_total
          , count_big(*) AS cb
FROM dbo.customer
     ,dbo.store_sales
     ,dbo.date_dim
WHERE c_customer_sk = ss_customer_sk
   AND ss_sold_date_sk = d_date_sk
GROUP BY c_customer_id
         ,c_first_name
         ,c_last_name
         ,c_preferred_cust_flag
         ,c_birth_country
         ,c_login
         ,c_email_address
         ,d_year
GO
CREATE materialized view nbViewCS WITH (DISTRIBUTION=HASH(customer_id)) AS
SELECT c_customer_id customer_id
       ,c_first_name customer_first_name
       ,c_last_name customer_last_name
       ,c_preferred_cust_flag customer_preferred_cust_flag
       ,c_birth_country customer_birth_country
       ,c_login customer_login
       ,c_email_address customer_email_address
       ,d_year dyear
       ,sum(isnull(cs_ext_list_price-cs_ext_wholesale_cost-cs_ext_discount_amt+cs_ext_sales_price, 0)/2) year_total
          , count_big(*) as cb
FROM dbo.customer
     ,dbo.catalog_sales
     ,dbo.date_dim
WHERE c_customer_sk = cs_bill_customer_sk
   AND cs_sold_date_sk = d_date_sk
GROUP BY c_customer_id
         ,c_first_name
         ,c_last_name
         ,c_preferred_cust_flag
         ,c_birth_country
         ,c_login
         ,c_email_address
         ,d_year

GO
CREATE materialized view nbViewWS WITH (DISTRIBUTION=HASH(customer_id)) AS
SELECT c_customer_id customer_id
       ,c_first_name customer_first_name
       ,c_last_name customer_last_name
       ,c_preferred_cust_flag customer_preferred_cust_flag
       ,c_birth_country customer_birth_country
       ,c_login customer_login
       ,c_email_address customer_email_address
       ,d_year dyear
       ,sum(isnull(ws_ext_list_price-ws_ext_wholesale_cost-ws_ext_discount_amt+ws_ext_sales_price, 0)/2) year_total
          , count_big(*) AS cb
FROM dbo.customer
     ,dbo.web_sales
     ,dbo.date_dim
WHERE c_customer_sk = ws_bill_customer_sk
   AND ws_sold_date_sk = d_date_sk
GROUP BY c_customer_id
         ,c_first_name
         ,c_last_name
         ,c_preferred_cust_flag
         ,c_birth_country
         ,c_login
         ,c_email_address
         ,d_year

```

Проверьте план выполнения исходного запроса еще раз.  Теперь количество присоединений изменяется с 17 до 5 и больше не существует.  Щелкните значок операции фильтрации в плане, его список выходных данных показывает, что данные считываются из материализованных представлений, а не из базовых таблиц.  

 ![Plan_Output_List_with_Materialized_Views](./media/performance-tuning-materialized-views/output-list.png)

При использовании материализованных представлений один и тот же запрос выполняется гораздо быстрее без изменения кода.  

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные советы по разработке см. в статье [Общие сведения о разработке пула SQL синапсе](sql-data-warehouse-overview-develop.md).
