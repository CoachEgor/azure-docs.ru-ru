---
title: Параметры сервера. база данных Azure для MySQL — гибкий сервер
description: В этом разделе приводятся рекомендации по настройке параметров сервера в службе "база данных Azure для MySQL — гибкий сервер".
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: conceptual
ms.date: 11/10/2020
ms.openlocfilehash: 58978f120578afeca129b0d8928713835def8418
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/11/2020
ms.locfileid: "94497008"
---
# <a name="server-parameters-in-azure-database-for-mysql---flexible-server"></a>Параметры сервера в базе данных Azure для MySQL — гибкий сервер

> [!IMPORTANT]
> Сейчас предоставляется общедоступная предварительная версия Гибкого сервера Базы данных Azure для MySQL.

В этой статье приводятся рекомендации и рекомендации по настройке параметров сервера в базе данных Azure для гибкого сервера MySQL.

## <a name="what-are-server-variables"></a>Что такое серверные переменные? 

Модуль MySQL предоставляет множество различных [переменных и параметров сервера](https://dev.mysql.com/doc/refman/5.7/en/server-option-variable-reference.html) , которые можно использовать для настройки и настройки поведения обработчика. Некоторые параметры могут быть заданы динамически во время выполнения, а другие — статическими, для применения которых требуется перезагрузка сервера.

База данных Azure для гибкого сервера MySQL предоставляет возможность изменять значения различных параметров сервера MySQL с помощью [портал Azure](./how-to-configure-server-parameters-portal.md) и [Azure CLI](./how-to-configure-server-parameters-cli.md) в соответствии с потребностями вашей рабочей нагрузки.

## <a name="configurable-server-parameters"></a>Настраиваемые параметры сервера

Вы можете управлять конфигурацией гибкого сервера базы данных Azure для MySQL с помощью параметров сервера. При создании сервера для параметров сервера настраиваются значения по умолчанию и Рекомендуемые. В колонке параметров сервера на портал Azure отображаются как изменяемые, так и неизменяемые параметры сервера. Неизменяемые параметры сервера выделяются серым цветом.

Список поддерживаемых параметров постоянно растет. Используйте вкладку Параметры сервера в портал Azure, чтобы просмотреть полный список и настроить значения параметров сервера.

Дополнительные сведения об ограничениях для нескольких часто обновляемых параметров сервера см. в следующих разделах ниже. Ограничения определяются уровнем вычислений и размером (виртуальных ядер) сервера.

> [!NOTE]
> Если вы хотите изменить параметр сервера, который не является изменяемым, но вы хотите просмотреть его в качестве изменяемой среды, откройте элемент [UserVoice](https://feedback.azure.com/forums/597982-azure-database-for-mysql) или Проголосуйте, если отзыв уже существует, что поможет нам определить приоритеты.

### <a name="log_bin_trust_function_creators"></a>log_bin_trust_function_creators

В базе данных Azure для гибкого сервера MySQL Двоичные журналы всегда включены (то есть `log_bin` имеет значение ON). Если вы хотите использовать триггеры, появится сообщение об ошибке, похожее на отсутствие *привилегий суперпользователя и ведение журнала в двоичном формате (возможно, вы захотите использовать менее надежную `log_bin_trust_function_creators` переменную)*. 

Двоичный формат ведения журнала всегда имеет значение **Row** , а все соединения с сервером **всегда** используют двоичное ведение журнала на основе строк. При ведении журнала в двоичном формате на основе строк проблемы безопасности не возникают, и регистрация в двоичном журнале не может быть нарушена, поэтому можно безопасно установить значение [`log_bin_trust_function_creators`](https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html#sysvar_log_bin_trust_function_creators) **true**.

### <a name="innodb_buffer_pool_size"></a>innodb_buffer_pool_size

Чтобы узнать больше об этом параметре, ознакомьтесь с документацией по [MySQL](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_buffer_pool_size).

|**Ценовая категория**|**Виртуальные ядра**|**Объем памяти (гиб)**|**Значение по умолчанию (в байтах)**|**Минимальное значение (байт)**|**Максимальное значение (байт)**|
|---|---|---|---|---|---|
|Пакетная (B1s)|1|1|134217728|33554432|134217728|
|Пакетная (B1ms)|1|2|536 870 912|134217728|536 870 912|
|Накапливаемыми ресурсами|2|4|2147483648|134217728|2147483648|
|Общее назначение|2|8|6442450944|134217728|6442450944|
|Общее назначение|4|16|12884901888|134217728|12884901888|
|Общее назначение|8|32|25769803776|134217728|25769803776|
|Общее назначение|16|64|51539607552|134217728|51539607552|
|Общее назначение|32|128|103079215104|134217728|103079215104|
|Общее назначение|48|192|154618822656|134217728|154618822656|
|Общее назначение|64|256|206158430208|134217728|206158430208|
|С оптимизацией для операций в памяти|2|16|12884901888|134217728|12884901888|
|С оптимизацией для операций в памяти|4|32|25769803776|134217728|25769803776|
|С оптимизацией для операций в памяти|8|64|51539607552|134217728|51539607552|
|С оптимизацией для операций в памяти|16|128|103079215104|134217728|103079215104|
|С оптимизацией для операций в памяти|32|256|206158430208|134217728|206158430208|
|С оптимизацией для операций в памяти|48|384|309237645312|134217728|309237645312|
|С оптимизацией для операций в памяти|64|504|405874409472|134217728|405874409472|

### <a name="innodb_file_per_table"></a>innodb_file_per_table

MySQL хранит таблицу InnoDB в разных табличных пространствах в зависимости от конфигурации, указанной при создании таблицы. [Табличное пространство системы](https://dev.mysql.com/doc/refman/5.7/en/innodb-system-tablespace.html) — это область хранения для словаря данных InnoDB. [Табличное пространство file-per-table](https://dev.mysql.com/doc/refman/5.7/en/innodb-file-per-table-tablespaces.html) содержит данные и индексы для одной таблицы InnoDB и хранится в файловой системе в собственном файле данных. Это поведение контролируется серверным параметром `innodb_file_per_table`. Если задать для `innodb_file_per_table` значение `OFF`, InnoDB создаст таблицы в системном табличном пространстве. В противном случае InnoDB создает таблицы в табличных пространствах file-per-table.

Для гибкого сервера базы данных Azure для MySQL поддерживается не более **4 ТБ** в одном файле данных. Если размер базы данных превышает 4 ТБ, следует создать таблицу в [innodb_file_per_table](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_file_per_table) табличном пространстве. Если размер одной таблицы превышает 4 ТБ, следует использовать таблицу секционирования.

### <a name="max_connections"></a>max_connections

Значение max_connection определяется объемом памяти сервера. 

|**Ценовая категория**|**Виртуальные ядра**|**Объем памяти (гиб)**|**Значение по умолчанию**|**Минимальное значение**|**Максимальное значение**|
|---|---|---|---|---|---|
|Пакетная (B1s)|1|1|85|10|171|
|Пакетная (B1ms)|1|2|171|10|341|
|Накапливаемыми ресурсами|2|4|341|10|683|
|Общее назначение|2|8|683|10|1365|
|Общее назначение|4|16|1365|10|2731|
|Общее назначение|8|32|2731|10|5461|
|Общее назначение|16|64|5461|10|10923|
|Общее назначение|32|128|10923|10|21845|
|Общее назначение|48|192|16384|10|32768|
|Общее назначение|64|256|21845|10|43691|
|С оптимизацией для операций в памяти|2|16|1365|10|2731|
|С оптимизацией для операций в памяти|4|32|2731|10|5461|
|С оптимизацией для операций в памяти|8|64|5461|10|10923|
|С оптимизацией для операций в памяти|16|128|10923|10|21845|
|С оптимизацией для операций в памяти|32|256|21845|10|43691|
|С оптимизацией для операций в памяти|48|384|32768|10|65536|
|С оптимизацией для операций в памяти|64|504|43008|10|86016|

При превышении предельного количества подключений может появиться следующая ошибка:
> ОШИБКА 1040 (08004): Слишком много подключений

> [!IMPORTANT]
> Для обеспечения оптимальной работы рекомендуется использовать пул подключений, например ProxySQL, чтобы эффективно управлять подключениями.

Создание клиентских подключений к MySQL занимает некоторое время, и после установки эти подключения занимают ресурсы базы данных даже при простое. Большинство приложений запрашивают много кратковременных соединений, из-за которых и возникает такая ситуация. В результате для реальной рабочей нагрузки будет меньше доступных ресурсов, что приведет к снижению производительности. Предотвратить такую ситуацию поможет использование пула подключений, который сокращает количество бездействующих подключений и повторно использует существующие подключения. Дополнительные сведения о настройке ProxySQL см. в [этой записи блога](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042).

>[!Note]
>Проксискл — это инструмент сообщества с открытым исходным кодом. Это решение поддерживается корпорацией Майкрософт. Чтобы получить техническую поддержку с помощью полномочного руководства, вы можете оценить и обратиться в [службу поддержки продуктов проксискл](https://proxysql.com/services/support/).

### <a name="innodb_strict_mode"></a>innodb_strict_mode

Если появляется сообщение об ошибке, похожее на "слишком большой размер строки (> 8126)", может потребоваться отключить параметр **innodb_strict_mode**. Параметр сервера **innodb_strict_mode** не может быть изменен глобально на уровне сервера, так как если размер данных строки превышает 8 КБ, данные будут обрезаны без ошибок, что может привести к возможной потере данных. Рекомендуется изменить схему в соответствии с предельным размером страницы. 

Этот параметр можно задать на уровне сеанса с помощью `init_connect` . Чтобы задать **innodb_strict_mode** на уровне сеанса, см. [параметр](./how-to-configure-server-parameters-portal.md#setting-non-modifiable-server-parameters)параметра, которого нет в списке.

> [!NOTE]
> При наличии сервера реплики чтения установка **INNODB_STRICT_MODE** Off на уровне сеанса на исходном сервере приведет к нарушению репликации. Мы рекомендуем оставить параметру значение OFF, если у вас есть реплики Read.

### <a name="time_zone"></a>time_zone

При первоначальном развертывании гибкий сервер Azure для MySQL включает в себя системные таблицы для сведений о часовом поясе, но эти таблицы не заполнены. Таблицы часовых поясов можно заполнить, вызвав хранимую процедуру `mysql.az_load_timezone` с помощью такого инструмента, как командная строка MySQL или MySQL Workbench. Инструкции по вызову хранимой процедуры и настройке часовых поясов глобального времени или уровня сеанса см. в статьях [Портал Azure](./how-to-configure-server-parameters-portal.md#working-with-the-time-zone-parameter) и [Интерфейс командной строки Azure](./how-to-configure-server-parameters-cli.md#working-with-the-time-zone-parameter).

## <a name="non-modifiable-server-parameters"></a>Неизменяемые параметры сервера

Колонка параметров сервера на портал Azure отображает изменяемые и неизменяемые параметры сервера. Неизменяемые параметры сервера выделяются серым цветом. Если вы хотите настроить неизменяемый параметр сервера на уровне сеанса, обратитесь к [портал Azure](./how-to-configure-server-parameters-portal.md#setting-non-modifiable-server-parameters) или [Azure CLI](./how-to-configure-server-parameters-cli.md#setting-non-modifiable-server-parameters) статье, чтобы задать параметр на уровне соединения с помощью `init_connect` .

## <a name="next-steps"></a>Дальнейшие действия

- Настройка [параметров сервера на портале Azure](./how-to-configure-server-parameters-portal.md)
- Настройка [параметров сервера в Azure CLI](./how-to-configure-server-parameters-cli.md)
