---
title: Потребление ресурсов Synapse SQL
description: Сведения о моделях потребления ресурсов SQL Synapse в Azure Synapse Analytics.
services: synapse analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: overview
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick
ms.openlocfilehash: 0e3bbb2532340664d0fc54b29bea3ef3af75f9b6
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2020
ms.locfileid: "96459154"
---
# <a name="synapse-sql-resource-consumption"></a>Потребление ресурсов Synapse SQL

В этой статье описаны модели потребления ресурсов Synapse SQL.

## <a name="serverless-sql-pool"></a>Бессерверный пул SQL

Бессерверный пул SQL — это служба с оплатой за запрос, для использования которой не нужно выбирать правильный размер. Система автоматически подстраивается в соответствии с вашими требованиями, освобождая от необходимости управления инфраструктурой и выбора подходящего размера для вашего решения.

## <a name="dedicated-sql-pool---data-warehouse-units-dwus-and-compute-data-warehouse-units-cdwus"></a>Выделенный пул SQL — единицы использования хранилища данных (DWU) и вычислительные единицы использования хранилища данных (cDWU)

Далее приведены рекомендации по выбору идеального количества единиц использования хранилища данных для оптимизации затрат и производительности, а также по изменению количества единиц.

### <a name="data-warehouse-units"></a>Единицы использования хранилища данных

Пул SQL Synapse представляет собой коллекцию аналитических ресурсов, которые подготавливаются. Аналитические ресурсы определяются как сочетание ЦП, памяти и операций ввода-вывода. Эти три ресурса объединены в единицы вычисления, которые называются единицами использования хранилища данных (DWU). Единица DWU представляет собой абстрактный, нормализованный объем вычислительных ресурсов и производительности. Изменение уровня обслуживания приводит к изменению числа DWU, доступных для системы. В свою очередь, это изменение приводит к корректировке производительности и расходов для вашей системы.

Чтобы получить высокую производительность, вы можете увеличить количество DWU. Для уменьшения производительности необходимо уменьшить число DWU. Использование хранилища и операции вычисления оплачиваются отдельно, независимо от изменений количества единиц использования хранилища данных.

Производительность единиц DWU основана на следующих метриках рабочей нагрузки хранилища данных:

- Насколько быстро стандартным запросом хранения данных сканируется большое количество строк и выполняется их сложное агрегирование. Это требует интенсивного использования операций ввода-вывода и ресурсов ЦП.
- Насколько быстро хранилище данных может принять данные из больших двоичных объектов хранилища Azure или Azure Data Lake. Это сетевая операция, которая требует интенсивного использования ресурсов ЦП.
- Насколько быстро команда T-SQL [`CREATE TABLE AS SELECT`](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) может скопировать таблицу. Эта операция предусматривает чтение данных из хранилища, их распространение в различные узлы устройства и повторную запись в хранилище. Эта операция выполняется ЦП и требует интенсивного использования операций ввода-вывода и сетевых ресурсов.

При увеличении единиц DWU происходит следующее:

- линейное изменение производительности системы для операций сканирования, агрегирования и инструкций CTAS;
- увеличение количества модулей чтения и записи при загрузке с помощью PolyBase;
- увеличение максимального количества параллельных запросов и слотов выдачи.

### <a name="service-level-objective"></a>Целевой уровень обслуживания

Цель уровня обслуживания (SLO) — это параметр масштабируемости, который определяет уровень затрат и производительности хранилища данных. Уровни обслуживания для поколения 2 измеряются в вычислительных единицах использования хранилища данных (cDWU). Пример: DW2000c. Уровни обслуживания поколения 1 измеряются в DWU. Пример: DW2000.

Цель уровня обслуживания (SLO) — это параметр масштабируемости, который определяет уровень затрат и производительности хранилища данных. Уровни обслуживания для выделенного пула SQL 2-го поколения измеряются в единицах использования хранилища данных (DWU), например DW2000c.

> [!NOTE]
> Недавно в Azure Synapse Analytics 2-го поколения были расширены возможности масштабирования, и теперь эта служба поддерживает низкие уровни вычислительных ресурсов от 100 cDWU. Существующие хранилища данных 1-го поколения, которым необходима поддержка более низких уровней вычислительных ресурсов, можно обновить до 2-го поколения в регионах, где оно сейчас доступно, без дополнительной платы.  Если эта возможность пока не доступна для вашего региона, можно обновить хранилище, выполнив геовосстановление в поддерживаемый регион. Дополнительные сведения см. в статье [Оптимизация производительности путем обновления хранилища данных SQL](../sql-data-warehouse/upgrade-to-latest-generation.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

В T-SQL параметр SERVICE_OBJECTIVE определяет уровень обслуживания и уровень производительности для выделенного пула SQL.

```sql
CREATE DATABASE mySQLDW
(Edition = 'Datawarehouse'
 ,SERVICE_OBJECTIVE = 'DW1000c'
)
;
```

### <a name="performance-tiers-and-data-warehouse-units"></a>Уровни производительности и единицы использования хранилища данных

Для каждого уровня производительности используются единицы измерения DWU с незначительными различиями. В счете эта разница представлена как единица масштабирования, преобразованная непосредственно в сумму к оплате.

- Хранилища данных поколения 1 измеряются в единицах использования хранилища данных (DWU).
- Хранилища данных поколения 2 измеряются в вычислительных единицах использования хранилища данных (cDWU).

Эти единицы поддерживают масштабирование вверх или вниз вычислительных ресурсов, а также приостановку вычислений, когда не нужно использовать хранилище данных. Все эти операции можно использовать по запросу. Чтобы улучшить уровень производительности, на вычислительных узлах поколения 2 используется локальный кэш на основе диска. При масштабировании или остановке системы кэш становится недействительным. Это значит, что для достижения оптимальной производительности требуется период подготовки кэша.  

При увеличении единиц использования хранилища данных вы линейно увеличиваете объем вычислительных ресурсов. Поколение 2 обеспечивает лучшую производительность запросов и самую высокую масштабируемость. Системы 2-го поколения также максимально используют кэш.

#### <a name="capacity-limits"></a>Ограничения емкости

Для каждого сервера SQL (например, myserver.database.windows.net) предусмотрена квота [DTU (единицы передачи данных)](../../azure-sql/database/service-tiers-dtu.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), которая позволяет применить только определенное число единиц использования хранилища данных. Дополнительные сведения см. в статье об [ограничениях емкости для управления рабочей нагрузкой](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#workload-management).

### <a name="assess-the-number-of-data-warehouse-units-you-need"></a>Оценка необходимого числа хранилищ данных

Оптимальное количество единиц использования хранилища данных в значительной степени зависит от рабочей нагрузки и объема данных, загруженных в систему.

Шаги для поиска оптимального количества единиц DWU рабочей нагрузки:

1. Начните, выбрав меньшее число DWU.
2. Отслеживайте производительность приложения по мере загрузки тестовых данных в систему, сравнивая ее с количеством выбранных DWU.
3. Определите любые дополнительные требования для периодов пиковой активности. Для рабочих нагрузок, в которых наблюдаются значительные пики и спады, может потребоваться частое масштабирование.

Пул SQL — это система горизонтального масштабирования, с помощью которой можно подготовить большие объемы вычислительных ресурсов и запрашивать большое количество данных. Чтобы ознакомиться с реальными возможностями масштабирования, особенно при использовании большого количества DWU, мы рекомендуем во время масштабирования выполнить масштабирование набора данных, чтобы проверить, достаточно ли используется данных для ЦП. Для проверки масштабирования мы рекомендуем использовать как минимум 1 TБ.

> [!NOTE]
>
> Высокий уровень параллелизма только увеличивает производительность обработки запросов, если нагрузку можно разделить между вычислительными узлами. Если обнаружится, что масштабирование не влияет на производительность, вам может потребоваться настроить конструктор таблиц или запросы. Инструкции по настройке запросов см. в статье [Памятка для хранилища данных SQL Azure](../overview-cheat-sheet.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="permissions"></a>Разрешения

Чтобы изменить единицы использования хранилища данных, требуются разрешения, описанные в статье об [инструкции ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

Встроенные роли Azure, такие как участник базы данных SQL и участник SQL Server, позволяют изменять параметры DWU.

#### <a name="view-current-dwu-settings"></a>для просмотра текущих параметров DWU;

Чтобы просмотреть текущие параметры DWU, сделайте следующее:

1. Откройте обозреватель объектов SQL Server в Visual Studio.
2. Подключитесь к базе данных master, связанной с логическим сервером SQL Server.
3. Выберите в sys.database_service_objectives динамическое административное представление. Например:

```sql
SELECT  db.name [Database]
,        ds.edition [Edition]
,        ds.service_objective [Service Objective]
FROM    sys.database_service_objectives   AS ds
JOIN    sys.databases                     AS db ON ds.database_id = db.database_id
;
```

### <a name="change-data-warehouse-units"></a>Изменение единиц использования хранилища данных

#### <a name="azure-portal"></a>Портал Azure

Чтобы изменить единицы DWU, сделайте следующее:

1. Откройте [портал Azure](https://portal.azure.com), затем откройте нужную базу данных и выберите **Масштаб**.

2. В колонке **Масштаб** передвиньте ползунок влево или вправо, чтобы изменить число единиц DWU.

3. Щелкните **Сохранить**. Появится окно подтверждения. Выберите **Да** для подтверждения или **Нет** для отмены.

#### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Чтобы изменить число единиц DWU, используйте командлет PowerShell [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json). В приведенном ниже примере для базы данных MySQLDW, размещенной на сервере MyServer, устанавливается цель уровня обслуживания DW1000.

```Powershell
Set-AzSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000c"
```

Дополнительные сведения см. в статье [Использование командлетов PowerShell для Azure Synapse Analytics](../sql-data-warehouse/sql-data-warehouse-reference-powershell-cmdlets.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

#### <a name="t-sql"></a>T-SQL

С помощью команды T-SQL можно просмотреть текущие параметры DWU, изменить их и сравнить результат.

Изменение DWU:

1. Подключитесь к базе данных master, связанной с вашим сервером.
2. Используйте оператор TSQL [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). В приведенном ниже примере для базы данных MySQLDW устанавливается целевой уровень обслуживания DW1000c.

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000c')
;
```

#### <a name="rest-apis"></a>Интерфейсы REST API

Чтобы изменить число DWU, используйте REST API [создания или обновления базы данных](/sql/t-sql/statements/alter-database-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). В приведенном ниже примере для базы данных MySQLDW, размещенной на сервере MyServer, устанавливается целевой уровень обслуживания DW1000c. Сервер находится в группе ресурсов Azure с именем ResourceGroup1.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

Дополнительные примеры REST API см. в статье [REST API для хранилища данных Azure Synapse Analytics](../sql-data-warehouse/sql-data-warehouse-manage-compute-rest-api.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="check-status-of-dwu-changes"></a>Проверка состояния изменений единиц DWU

Изменение количества единиц DWU может занять несколько минут. При выполнении автоматического масштабирования можно реализовать логику, позволяющую приступать к выполнению другого действия только после завершения определенных операций.

Проверка состояния базы данных с использованием различных конечных точек позволяет правильно реализовать автоматизацию этих операций. На портале отображается уведомление о завершении операции и текущее состояние баз данных, однако здесь нельзя проверять состояние программным способом.

Вы не можете проверить состояние базы данных для операций горизонтального увеличения масштаба с помощью портала Azure.

Чтобы проверить состояние изменений DWU, сделайте следующее:

1. Подключитесь к базе данных master, связанной с вашим сервером.
2. Отправьте запрос ниже, чтобы проверить состояние базы данных.

```sql
SELECT    *
FROM      sys.databases
;
```

1. Отправьте запрос ниже, чтобы проверить состояние операции.

```sql
SELECT    *
FROM      sys.dm_operation_status
WHERE     resource_type_desc = 'Database'
AND       major_resource_id = 'MySQLDW'
;
```

Это динамическое административное представление возвращает сведения о различных операциях управления выделенным пулом SQL, например об операции и ее состоянии IN_PROGRESS или COMPLETED.

### <a name="the-scaling-workflow"></a>Масштабирование рабочего процесса

При активации масштабирования, чтобы обеспечить согласованное состояние, система сначала прерывает все открытые сеансы и выполняется откат всех открытых транзакций. Операции масштабирования можно выполнить только после завершения отката транзакций.  

- Чтобы вертикально увеличить масштаб, система отсоединяет все вычислительные узлы, подготавливает дополнительные вычислительные узлы, а затем повторно подключается к уровню хранилища.
- Чтобы вертикально уменьшить масштаб, система отсоединяет все вычислительные узлы, а затем повторно подключает только необходимые узлы к уровню хранилища.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об управлении производительностью см. в статьях [Классы ресурсов для управления рабочими нагрузками](../sql-data-warehouse/resource-classes-for-workload-management.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) и [Memory and concurrency limits for Azure SQL Data Warehouse](../sql-data-warehouse/memory-concurrency-limits.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) (Ограничения памяти и параллелизма для хранилища данных SQL Azure).
