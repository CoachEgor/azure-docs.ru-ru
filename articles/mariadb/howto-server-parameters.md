---
title: Настройка параметров сервера — портал Azure — база данных Azure для MariaDB
description: В этой статье описывается, как настроить параметры сервера MariaDB в базе данных Azure для MariaDB с помощью портала Azure.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 4/16/2020
ms.openlocfilehash: f39e9450fb922e5b93d7b4b809df73cf5ab007c1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81602403"
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mariadb-by-using-the-azure-portal"></a>Как настроить параметры сервера в базе данных Azure для MariaDB с помощью портала Azure

База данных Azure для MariaDB поддерживает настройку некоторых параметров сервера. В этой статье описывается настройка этих параметров с помощью портала Azure. Не все параметры сервера можно настроить.

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Переход к колонке параметров сервера на портале Azure

1. Войдите на портал Azure, а затем найдите свой сервер базы данных Azure для MariaDB.
2. В разделе **Параметры** щелкните **Параметры сервера**, чтобы открыть страницу параметров сервера для сервера базы данных Azure для MariaDB.
![Страница параметров сервера на портале Azure](./media/howto-server-parameters/azure-portal-server-parameters.png)
3. Найдите все параметры, которые необходимо настроить. Просмотрите столбец **Описание**, чтобы понять назначение и допустимые значения.
![Раскрывающийся список для перечисляемого типа](./media/howto-server-parameters/3-toggle_parameter.png)
4. Нажмите кнопку **сохранить** , чтобы сохранить изменения.
![Сохранение или отмена изменений](./media/howto-server-parameters/4-save_parameters.png)
5. Если вы сохранили новые значения параметров, всегда можно восстановить значения по умолчанию, выбрав **Сбросить все к значениям по умолчанию**.
![Сбросить все к значениям по умолчанию](./media/howto-server-parameters/5-reset_parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Список доступных для настройки параметров сервера

Список поддерживаемых параметров постоянно растет. Вы можете использовать вкладку параметров сервера на портале Azure, чтобы получить определение и настроить параметры сервера на основе требований своего приложения.

## <a name="non-configurable-server-parameters"></a>Ненастраиваемые параметры сервера

Буферный пул InnoDB и максимальное число подключений не подлежат настройке и привязаны к [ценовой категории](concepts-pricing-tiers.md).

|**Ценовая категория**| **Виртуальные ядра**|**Буферный пул InnoDB**|
|---|---|---|
|Basic| 1| 1024|
|Basic| 2| 2560|
|Общее назначение| 2| 3584|
|Общее назначение| 4| 7680|
|Общее назначение| 8| 15360|
|Общее назначение| 16| 31232|
|Общее назначение| 32| 62976|
|Общее назначение| 64| 125952|
|С оптимизацией для операций в памяти| 2| 7168|
|С оптимизацией для операций в памяти| 4| 15360|
|С оптимизацией для операций в памяти| 8| 30720|
|С оптимизацией для операций в памяти| 16| 62464|
|С оптимизацией для операций в памяти| 32| 125952|

Эти дополнительные параметры сервера нельзя настроить в системе:

|**Параметр**|**Фиксированное значение**|
| :------------------------ | :-------- |
|innodb_file_per_table для уровня "Базовый"|OFF|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|МБ|
|innodb_log_files_in_group|2|

Для остальных параметров сервера, которые не перечислены выше, устанавливаются значения MariaDB по умолчанию для [MariaDB](https://mariadb.com/kb/en/library/xtradbinnodb-server-system-variables/).

## <a name="working-with-the-time-zone-parameter"></a>Работа с параметром часового пояса

### <a name="populating-the-time-zone-tables"></a>Заполнение таблиц часовых поясов

Таблицы часовых поясов на сервере можно заполнить, вызвав хранимую процедуру `mysql.az_load_timezone` с помощью такого инструмента, как командная строка MySQL или MySQL Workbench.

> [!NOTE]
> Если вы используете команду `mysql.az_load_timezone` в MySQL Workbench, может потребоваться предварительно отключить режим безопасного обновления с помощью `SET SQL_SAFE_UPDATES=0;`.

```sql
CALL mysql.az_load_timezone();
```

> [!IMPORTANT]
> Необходимо перезапустить сервер, чтобы убедиться, что таблицы часовых поясов заполнены правильно. Чтобы перезапустить сервер, используйте [портал Azure](howto-restart-server-portal.md) или [CLI](howto-restart-server-cli.md).
Чтобы просмотреть доступные значения часового пояса, выполните следующую команду.

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>Установка часового пояса глобального уровня

Часовой пояс глобального уровня можно задать на странице **Параметры сервера** на портале Azure. Ниже приведен пример, который задает глобальный часовой пояс "US/Pacific" (США, Тихоокеанский регион).

![Настройка параметра часового пояса](./media/howto-server-parameters/timezone.png)

### <a name="setting-the-session-level-time-zone"></a>Настройка часового пояса уровня сеанса

Часовой пояс уровня сеанса можно задать, выполнив команду `SET time_zone` в командной строке MySQL или MySQL Workbench. В приведенном ниже примере задается часовой пояс **US/Pacific** (США, Тихоокеанский регион).

```sql
SET time_zone = 'US/Pacific';
```

Описание [Функций даты и времени](https://mariadb.com/kb/en/library/convert_tz/) см. в документации по MariaDB.

<!--
## Next steps

- [Connection libraries for Azure Database for MariaDB](concepts-connection-libraries.md).
-->
