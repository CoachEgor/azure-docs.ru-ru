---
title: Обновление до последнего поколения
description: Обновите пул SQL Azure синапсе Analytics до последнего поколения оборудования и архитектуры хранилища Azure.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 02/19/2019
ms.author: martinle
ms.reviewer: jrasnick
ms.custom: seo-lt-2019
ms.openlocfilehash: 97cbae93b1ee2dd6ca4916f4efbb964141b33a3f
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/29/2020
ms.locfileid: "78200836"
---
# <a name="optimize-performance-by-upgrading-azure-synapse-analytics-sql-pool"></a>Оптимизация производительности путем обновления пула SQL Azure синапсе Analytics

Обновите пул SQL до последнего поколения архитектуры оборудования и хранилища Azure.

## <a name="why-upgrade"></a>Зачем выполнять обновление

Теперь можно легко выполнить обновление до уровня Gen2, оптимизированного для пула SQL, в портал Azure для [поддерживаемых регионов](gen2-migration-schedule.md#automated-schedule-and-region-availability-table). Если ваш регион не поддерживает самостоятельное обновление, можно перейти на поддерживаемый регион или дождаться, когда функция самостоятельного обновления станет доступна в вашем регионе. Выполните обновление сейчас, чтобы воспользоваться преимуществами последнего поколения оборудования Azure и усовершенствованной архитектуры хранилища, включая повышенную производительность, масштабируемость и неограниченное хранение по столбцам. 

> [!VIDEO https://www.youtube.com/embed/9B2F0gLoyss]

## <a name="applies-to"></a>Применяется к

Это обновление применяется к Gen1 пулам SQL, оптимизированным для вычислений, в [поддерживаемых регионах](gen2-migration-schedule.md#automated-schedule-and-region-availability-table).

## <a name="before-you-begin"></a>Перед началом

1. Проверьте, поддерживает ли ваш [регион](gen2-migration-schedule.md#automated-schedule-and-region-availability-table) перенос хранилища 1-го поколения в хранилище 2-го поколения. Обратите внимание на даты автоматического переноса. Чтобы избежать конфликтов с автоматизированным процессом, запланируйте ручной перенос на дату, предшествующую дате запуска автоматизированного процесса.
2. Если вы находитесь в регионе, который еще не поддерживается, регулярно проверяйте, не был ли ваш регион добавлен в список поддерживаемых, или выполните [обновление с помощью восстановления](#upgrade-from-an-azure-geographical-region-using-restore-through-the-azure-portal) в поддерживаемый регион.
3. Если ваш регион не поддерживается, выполните [обновление на портале Azure](#upgrade-in-a-supported-region-using-the-azure-portal).
4. **Выберите рекомендуемый уровень производительности** для пула SQL на основе текущего уровня производительности на уровне Gen1, оптимизированного для вычислений, используя приведенное ниже сопоставление.

   | Уровень 1-го поколения "Оптимизировано для вычислений" | Уровень 2-го поколения "Оптимизировано для вычислений" |
   | :-------------------------: | :-------------------------: |
   |            DW100            |           DW100c            |
   |            DW200            |           DW200c            |
   |            DW300            |           DW300c            |
   |            DW400            |           DW400c            |
   |            DW500            |           DW500c            |
   |            DW600            |           DW500c            |
   |           DW1000            |           DW1000c           |
   |           DW1200            |           DW1000c           |
   |           DW1500            |           DW1500c           |
   |           DW2000            |           DW2000c           |
   |           DW3000            |           DW3000c           |
   |           DW6000            |           DW6000c           |

> [!Note]
> Предлагаемые уровни производительности не гарантируют прямое преобразование. Например, мы рекомендуем перейти с DW600 на DW500c.

## <a name="upgrade-in-a-supported-region-using-the-azure-portal"></a>Обновление в поддерживаемом регионе с помощью портала Azure

## <a name="before-you-begin"></a>Перед началом

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!NOTE]
> Миграция с GEN1 на Gen2 с помощью портал Azure является постоянной. Не существует процесса для возврата в Gen1.  

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портал Azure](https://portal.azure.com/).

1. Если обновляемый пул SQL для оптимизированного уровня Gen1 будет приостановлен, [возобновите работу пула SQL](pause-and-resume-compute-portal.md).

   > [!NOTE]
   > Чтобы выполнить миграцию в Gen2, необходимо запустить пул SQL.

2. Будьте готовы к непродолжительному простою. 

3. Определите все ссылки на код, связанные с уровнями производительности 1-го поколения "Оптимизировано для вычислений" и измените их на эквивалентный уровень производительности 2-го поколения "Оптимизировано для вычислений" Ниже представлены два примера, в которых нужно обновить ссылки на код перед обновлением:

   Исходная команда PowerShell 1-го поколения:

   ```powershell
   Set-AzSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300"
   ```

   Изменено на:

   ```powershell
   Set-AzSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300c"
   ```

   > [!NOTE] 
   > -RequestedServiceObjectiveName "DW300" изменено на -RequestedServiceObjectiveName "DW300**c**"
   >

   Исходная команда T-SQL 1-го поколения:

   ```SQL
   ALTER DATABASE mySampleDataWarehouse MODIFY (SERVICE_OBJECTIVE = 'DW300') ;
   ```

   Изменено на:

   ```sql
   ALTER DATABASE mySampleDataWarehouse MODIFY (SERVICE_OBJECTIVE = 'DW300c') ; 
   ```
   > [!NOTE] 
   > SERVICE_OBJECTIVE = 'DW300' изменено на SERVICE_OBJECTIVE = 'DW300**c**'

## <a name="start-the-upgrade"></a>Начало обновления

1. Перейдите в пул Gen1 SQL, оптимизированный для вычислений, в портал Azure. Если обновляемый пул SQL для оптимизированного уровня Gen1 будет приостановлен, [возобновите работу пула SQL](pause-and-resume-compute-portal.md). 
2. На вкладке "задачи" выберите **Обновить до Gen2** : ![Upgrade_1](./media/sql-data-warehouse-upgrade-to-latest-generation/upgrade-to-gen2-1.png)
    
    > [!NOTE]
    > Если карта **Повысить до Gen2** не отображается на вкладке "Задачи", для типа вашей подписки действуют ограничения в текущем регионе.
    > [Отправьте запрос в службу поддержки](sql-data-warehouse-get-started-create-support-ticket.md) на добавление вашей подписки в список разрешений.

3. Перед обновлением убедитесь, что рабочая нагрузка завершена и приостановлена. В течение нескольких минут вы получите простой, прежде чем пул SQL будет восстановлен в сети как пул SQL Gen2 уровня, оптимизированного для вычислений. Выберите **Обновить**.

   ![Upgrade_2](./media/sql-data-warehouse-upgrade-to-latest-generation/upgrade-to-gen2-2.png)

4. **Наблюдайте за процессом обновления**, проверяя состояние на портале Azure.

   ![Upgrade3](./media/sql-data-warehouse-upgrade-to-latest-generation/upgrade-to-gen2-3.png)

   Первым шагом процесса обновления является масштабирование ("Обновление — автономный режим"), при котором все сеансы будут прерваны, а соединение разорвано. 

   Второй шаг процесса обновления — перенос данных ("Обновление — в сети"). Перенос данных является малозаметным фоновым сетевым процессом. Он медленно перемещает данные столбцов из старой архитектуры службы хранилища в новую архитектуру службы хранилища, использующую локальный кэш SSD. В течение этого времени пул SQL будет подключен к сети для выполнения запросов и загрузки. При этом все данные будут доступны для запросов, вне зависимости от того, были ли они перенесены. Перенос данных происходит с разной скоростью в зависимости от их размера, уровня производительности и количества сегментов columnstore. 

5. **Необязательная рекомендация:** После завершения операции масштабирования можно ускорить фоновый процесс переноса данных. Вы можете сразу инициировать перемещение данных, выполнив инструкцию [Alter Index rebuild](sql-data-warehouse-tables-index.md) для всех основных таблиц columnstore, которые вы сможете запрашивать при наличии более высокого целевого уровня обслуживания и оптимального класса ресурсов. Эта операция является **автономной** по сравнению с малозаметным фоновым процессом, который может потребовать несколько часов в зависимости от количества и размеров таблиц. Тем не менее после завершения этой операции перенос данных будет происходить намного быстрее благодаря новой улучшенной архитектуре хранилища с высококачественными группами строк.
 
> [!NOTE]
> Операция "Alter Index rebuild" — это автономная операция. Таблицы будут недоступны до завершения повторной сборки.

Следующий запрос формирует необходимые команды Alter Index Rebuild для ускорения процесса переноса данных.

```sql
SELECT 'ALTER INDEX [' + idx.NAME + '] ON [' 
       + Schema_name(tbl.schema_id) + '].[' 
       + Object_name(idx.object_id) + '] REBUILD ' + ( CASE 
                                                         WHEN ( 
                                                     (SELECT Count(*) 
                                                      FROM   sys.partitions 
                                                             part2 
                                                      WHERE  part2.index_id 
                                                             = idx.index_id 
                                                             AND 
                                                     idx.object_id = 
                                                     part2.object_id) 
                                                     > 1 ) THEN 
              ' PARTITION = ' 
              + Cast(part.partition_number AS NVARCHAR(256)) 
              ELSE '' 
                                                       END ) + '; SELECT ''[' + 
              idx.NAME + '] ON [' + Schema_name(tbl.schema_id) + '].[' + 
              Object_name(idx.object_id) + '] ' + ( 
              CASE 
                WHEN ( (SELECT Count(*) 
                        FROM   sys.partitions 
                               part2 
                        WHERE 
                     part2.index_id = 
                     idx.index_id 
                     AND idx.object_id 
                         = part2.object_id) > 1 ) THEN 
              ' PARTITION = ' 
              + Cast(part.partition_number AS NVARCHAR(256)) 
              + ' completed'';' 
              ELSE ' completed'';' 
                                                    END ) 
FROM   sys.indexes idx 
       INNER JOIN sys.tables tbl 
               ON idx.object_id = tbl.object_id 
       LEFT OUTER JOIN sys.partitions part 
                    ON idx.index_id = part.index_id 
                       AND idx.object_id = part.object_id 
WHERE  idx.type_desc = 'CLUSTERED COLUMNSTORE'; 
```

## <a name="upgrade-from-an-azure-geographical-region-using-restore-through-the-azure-portal"></a>Обновление из географического региона Azure с помощью восстановления на портале Azure

## <a name="create-a-user-defined-restore-point-using-the-azure-portal"></a>Создание определяемой пользователем точки восстановления с помощью портала Azure

1. Войдите на [портал Azure](https://portal.azure.com/).

2. Перейдите в пул SQL, для которого необходимо создать точку восстановления.

3. В верхней области колонки "Обзор" выберите **Новая точка восстановления**.

    ![Новая точка восстановления](./media/sql-data-warehouse-restore-database-portal/creating_restore_point_0.png)

4. Укажите имя точки восстановления.

    ![Имя точки восстановления](./media/sql-data-warehouse-restore-database-portal/creating_restore_point_1.png)

## <a name="restore-an-active-or-paused-database-using-the-azure-portal"></a>Восстановление активной или приостановленной базы данных с помощью портала Azure

1. Войдите на [портал Azure](https://portal.azure.com/).
2. Перейдите к пулу SQL, из которого требуется выполнить восстановление.
3. В верхней области колонки "Обзор" выберите **Восстановление**.

    ![ Обзор восстановления](./media/sql-data-warehouse-restore-database-portal/restoring_0.png)

4. Выберите **точки автоматического восстановления** или **определенные пользователем точки восстановления**. Для определяемых пользователем точек восстановления **выберите определенную пользователем точку восстановления** или **Создайте новую, определенную пользователем точку восстановления**. Для сервера выберите **создать** и выберите сервер в Gen2 поддерживаемом географическом регионе. 

    ![Точки автоматического восстановления](./media/sql-data-warehouse-restore-database-portal/restoring_1.png)

## <a name="restore-from-an-azure-geographical-region-using-powershell"></a>Восстановление из географического региона Azure с помощью PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Чтобы восстановить базу данных, используйте командлет [RESTORE-азсклдатабасе](/powershell/module/az.sql/restore-azsqldatabase) .

> [!NOTE]
> Можно выполнить геовосстановление в хранилище 2-го поколения! Для этого в качестве необязательного параметра укажите имя ServiceObjectiveName 2-го поколения (например, DW1000**c**).

1. Откройте Windows PowerShell.
2. Подключитесь к своей учетной записи Azure и выведите список всех подписок, связанных с ней.
3. Выберите подписку, содержащую базу данных, которую нужно восстановить.
4. Получите базу данных, которую требуется восстановить.
5. Создайте запрос на восстановление для базы данных, указав ServiceObjectiveName 2-го поколения.
6. Проверьте состояние геовосстановленной базы данных.

```Powershell
Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID -ServiceObjectiveName "<YourTargetServiceLevel>" -RequestedServiceObjectiveName "DW300c"

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> Чтобы настроить базу данных после восстановления, см. раздел [Настройка базы данных после восстановления](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).

Восстановленная база данных будет поддерживать прозрачное шифрование данных, если исходная база данных поддерживает прозрачное шифрование данных.


При возникновении проблем с пулом SQL создайте [запрос в службу поддержки](sql-data-warehouse-get-started-create-support-ticket.md) и в качестве возможной причины щелкните "Gen2 Upgrade".

## <a name="next-steps"></a>Дальнейшие действия

Обновленный пул SQL находится в режиме "в сети". Чтобы воспользоваться преимуществами новой, усовершенствованной архитектуры, см. [Классы ресурсов для управления рабочей нагрузкой](resource-classes-for-workload-management.md).
