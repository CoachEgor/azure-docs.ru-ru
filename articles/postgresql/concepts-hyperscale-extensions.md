---
title: Расширения — масштабирование (Цитус) — база данных Azure для PostgreSQL
description: Описывает возможность расширения функциональных возможностей базы данных с помощью расширений в базе данных Azure для PostgreSQL-Scale (Цитус).
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 04/16/2020
ms.openlocfilehash: ba8f4591782a4e34fbde26d9669ef01f24450486
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82146418"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql--hyperscale-citus"></a>Расширения PostgreSQL в базе данных Azure для PostgreSQL — масштабирование (Цитус)

PostgreSQL предоставляет возможность расширения функциональных возможностей базы данных с помощью расширений. Расширения позволяют объединить несколько связанных объектов SQL в одном пакете, который затем можно загрузить или удалить из базы данных с помощью одной команды. После загрузки в базу данных расширения могут работать, как встроенные функции. Дополнительные сведения о расширениях PostgreSQL см. [в разделе Упаковка связанных объектов в расширение](https://www.postgresql.org/docs/current/static/extend-extensions.html).

## <a name="use-postgresql-extensions"></a>Использование расширений PostgreSQL

Чтобы использовать расширения PostgreSQL, их необходимо сначала установить в базу данных. Чтобы установить определенное расширение, выполните команду " [создать расширение](https://www.postgresql.org/docs/current/static/sql-createextension.html) " из средства PSQL, чтобы загрузить упакованные объекты в базу данных.

В настоящее время база данных Azure для PostgreSQL-Scale (Цитус) поддерживает подмножество расширений ключей, как указано здесь. Расширения, отличные от перечисленных, не поддерживаются. Вы не можете создать собственное расширение с базой данных Azure для PostgreSQL.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>Расширения, поддерживаемые базой данных Azure для PostgreSQL

В таблицах ниже приведен список стандартных расширений PostgreSQL, которые в настоящее время поддерживаются базой данных Azure для PostgreSQL. Эти сведения также можно получить, выполнив `SELECT * FROM pg_available_extensions;`.

### <a name="data-types-extensions"></a>Расширения типов данных

> [!div class="mx-tableFixed"]
> | **Расширение** | **Описание** |
> |---|---|
> | [citext](https://www.postgresql.org/docs/current/static/citext.html) | Предоставляет тип строки символов без учета регистра. |
> | [Куба](https://www.postgresql.org/docs/current/static/cube.html) | Предоставляет тип данных для многомерных кубов. |
> | [hstore](https://www.postgresql.org/docs/current/static/hstore.html) | Предоставляет тип данных для хранения наборов пар "ключ-значение". |
> | [хлл](https://github.com/citusdata/postgresql-hll) | Предоставляет структуру данных Хиперлоглог. |
> | [isn](https://www.postgresql.org/docs/current/static/isn.html) | Предоставляет типы данных для международных стандартов нумерации продуктов. |
> | [Lo](https://www.postgresql.org/docs/current/lo.html) | Обслуживание больших объектов. |
> | [ltree](https://www.postgresql.org/docs/current/static/ltree.html) | Предоставляет тип данных для иерархических древовидных структур. |
> | [SEG](https://www.postgresql.org/docs/current/seg.html) | Тип данных для представления сегментов линии или интервалов с плавающей точкой. |
> | [TopN](https://github.com/citusdata/postgresql-topn/) | Введите для TOP-n ЖСОНБ. |

### <a name="full-text-search-extensions"></a>Расширения для полнотекстового поиска

> [!div class="mx-tableFixed"]
> | **Расширение** | **Описание** |
> |---|---|
> | [dict\_int](https://www.postgresql.org/docs/current/static/dict-int.html) | Предоставляет шаблон словаря текстового поиска для целых чисел. |
> | [словарь\_кссин](https://www.postgresql.org/docs/current/dict-xsyn.html) | Шаблон словаря поиска текста для обработки расширенных синонимов. |
> | [unaccent](https://www.postgresql.org/docs/current/static/unaccent.html) | Словарь для текстового поиска, который удаляет из лексем знаки ударения (диакритические знаки). |

### <a name="functions-extensions"></a>Расширения функций

> [!div class="mx-tableFixed"]
> | **Расширение** | **Описание** |
> |---|---|
> | [аутоинк](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.7) | Функции для полей с автоприращением. |
> | [earthdistance](https://www.postgresql.org/docs/current/static/earthdistance.html) | Предоставляет средства для вычисления ортодромических расстояний на поверхности Земли. |
> | [fuzzystrmatch](https://www.postgresql.org/docs/current/static/fuzzystrmatch.html) | Предоставляет несколько функций для определения сходства и расстояния между строками. |
> | [вставить\_имя пользователя](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.8) | Функции для отслеживания изменений в таблице. |
> | [интагг](https://www.postgresql.org/docs/current/intagg.html) | Целочисленный агрегат и перечислитель (устарело). |
> | [intarray](https://www.postgresql.org/docs/current/static/intarray.html) | Предоставляет функции и операторы для управления массивами целых чисел, не содержащими значений null. |
> | [моддатетиме](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.9) | Функции для отслеживания времени последнего изменения. |
> | [pgcrypto](https://www.postgresql.org/docs/current/static/pgcrypto.html) | Предоставляет функции шифрования. |
> | [pg\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Управляет секционированными таблицами по времени или идентификатору. |
> | [pg\_trgm](https://www.postgresql.org/docs/current/static/pgtrgm.html) | Предоставляет функции и операторы для определения сходства буквенно-цифрового текста на основе сопоставления триграмм. |
> | [уточнение](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.5) | Функции для реализации ссылочной целостности (устарела). |
> | анализ\_сеансов | Функции для запроса массивов хсторе. |
> | [tablefunc](https://www.postgresql.org/docs/current/static/tablefunc.html) | Предоставляет функции для полного управления таблицами, включая перекрестные таблицы. |
> | [ткн](https://www.postgresql.org/docs/current/tcn.html) | Активированные уведомления об изменениях. |
> | [тиметравел](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.6) | Функции для реализации поездок по времени. |
> | [uuid-ossp](https://www.postgresql.org/docs/current/static/uuid-ossp.html) | Создает глобально уникальные идентификаторы (UUID). |

### <a name="hyperscale-extensions"></a>Расширения с масштабированием

> [!div class="mx-tableFixed"]
> | **Расширение** | **Описание** |
> |---|---|
> | [Цитус](https://github.com/citusdata/citus) | Распределенная база данных Цитус. |
> | Подсистема балансировки сегментов\_ | Безопасно Перераспределите данные в группе серверов в случае добавления или удаления узла. |

### <a name="index-types-extensions"></a>Расширения типов индексов

> [!div class="mx-tableFixed"]
> | **Расширение** | **Описание** |
> |---|---|
> | [раскрытия](https://www.postgresql.org/docs/current/bloom.html) | Метод доступа раскрытия — индекс на основе файла сигнатур. |
> | [btree\_gin](https://www.postgresql.org/docs/current/static/btree-gin.html) | Предоставляет примеры классов операторов Ло, реализующих поведение типа «сбалансированное дерево» для определенных типов данных. |
> | [btree\_gist](https://www.postgresql.org/docs/current/static/btree-gist.html) | Предоставляет классы оператора индекса GiST, которые реализуют сбалансированное дерево. |

### <a name="language-extensions"></a>Расширения языка

> [!div class="mx-tableFixed"]
> | **Расширение** | **Описание** |
> |---|---|
> | [plpgsql](https://www.postgresql.org/docs/current/static/plpgsql.html) | Загружаемый процедурный язык PL/pgSQL. |

### <a name="miscellaneous-extensions"></a>Прочие расширения

> [!div class="mx-tableFixed"]
> | **Расширение** | **Описание** |
> |---|---|
> | [админпакк](https://www.postgresql.org/docs/current/adminpack.html) | Административные функции для PostgreSQL. |
> | [амчекк](https://www.postgresql.org/docs/current/amcheck.html) | Функции для проверки целостности связи. |
> | [ФДВ\_файла](https://www.postgresql.org/docs/current/file-fdw.html) | Оболочка внешних данных для доступа к неструктурированным файлам. |
> | [пажеинспект](https://www.postgresql.org/docs/current/pageinspect.html) | Проверьте содержимое страниц базы данных на низком уровне. |
> | [pg\_buffercache](https://www.postgresql.org/docs/current/static/pgbuffercache.html) | Предоставляет средства для анализа того, что происходит в общем буферном кэше в режиме реального времени. |
> | [PG\_cron](https://github.com/citusdata/pg_cron) | Планировщик заданий для PostgreSQL. |
> | [PG\_фриспацемап](https://www.postgresql.org/docs/current/pgfreespacemap.html) | Изучите карту свободного места (FSM). |
> | [pg\_prewarm](https://www.postgresql.org/docs/current/static/pgprewarm.html) | Предоставляет способ загрузки реляционных данных в буферный кэш. |
> | [pg\_stat\_statements](https://www.postgresql.org/docs/current/static/pgstatstatements.html) | Предоставляет средства для отслеживания статистики выполнения всех инструкций SQL, выполняемых сервером. Сведения об этом расширении см. в разделе "pg_stat_statements". |
> | [видимость\_PG](https://www.postgresql.org/docs/current/pgvisibility.html) | Проверьте сведения о видимости (на виртуальной машине) и видимость на уровне страницы. |
> | [pgrowlocks](https://www.postgresql.org/docs/current/static/pgrowlocks.html) | Предоставляет средства для отображения сведений о блокировке на уровне строк. |
> | [pgstattuple](https://www.postgresql.org/docs/current/static/pgstattuple.html) | Предоставляет средства для отображения статистических данных на уровне кортежей. |
> | [postgres\_fdw](https://www.postgresql.org/docs/current/static/postgres-fdw.html) | Оболочка для внешних данных, используемая для доступа к данным на внешних серверах PostgreSQL. Сведения об этом расширении см. в разделе "дблинк и postgres_fdw".|
> | [сслинфо](https://www.postgresql.org/docs/current/sslinfo.html) | Сведения о сертификатах TLS/SSL. |
> | [ТСМ\_строки\_системы](https://www.postgresql.org/docs/current/tsm-system-rows.html) | TABLESAMPLE метод, который принимает количество строк в качестве ограничения. |
> | [ТСМ\_системное\_время](https://www.postgresql.org/docs/current/tsm-system-time.html) | TABLESAMPLE метод, который принимает в качестве ограничения время в миллисекундах. |
> | [hypopg](https://hypopg.readthedocs.io/en/latest/) | Предоставляет способ создания гипотетических индексов, которые используют ЦП или диск. |
> | [dblink](https://www.postgresql.org/docs/current/dblink.html) | Модуль, который поддерживает подключения к другим базам данных PostgreSQL из сеанса базы данных. Сведения об этом расширении см. в разделе "дблинк и postgres_fdw". |
> | [xml2](https://www.postgresql.org/docs/current/xml2.html) | Запросы XPath и XSLT. |


### <a name="postgis-extensions"></a>Расширения PostGIS

> [!div class="mx-tableFixed"]
> | **Расширение** | **Описание** |
> |---|---|
> | [PostGIS](https://www.postgis.net/), postgis\_topology, postgis\_tiger\_geocoder, postgis\_sfcgal | Пространственные и географические объекты для PostgreSQL. |
> | address\_standardizer, address\_standardizer\_data\_us | Используются для анализа адреса в составных элементы. Используется для поддержки шага нормализации геокодирования адресов. |
> | PostGIS\_сфкгал | Функции СФКГАЛ PostGIS. |
> | PostGIS\_Tiger\_геокодировании | PostGIS Tiger для геокодирования и инвертированного геокодирования. |
> | Топология PostGIS\_ | Пространственные типы и функции топологии PostGIS. |


## <a name="pg_stat_statements"></a>pg_stat_statements
[Расширение инструкций\_PG\_stat](https://www.postgresql.org/docs/current/pgstatstatements.html) предварительно загружается на каждом сервере базы данных Azure для PostgreSQL для предоставления средств отслеживания статистики выполнения инструкций SQL.

Параметр `pg_stat_statements.track` определяет, какие инструкции подсчитываются расширением. По умолчанию используется `top`значение, то есть выполняется трассировка всех инструкций, выдаваемых непосредственно клиентами. Два других уровня отслеживания: `none` и `all`. Этот параметр можно настроить в качестве параметра сервера с помощью [портала Azure](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-portal) или [Azure CLI](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-cli).

Существует компромисс между сведениями о выполнении запроса pg_stat_statements предоставляет и влияния на производительность сервера при регистрации каждой инструкции SQL. Если вы активно не используете расширение pg_stat_statements, рекомендуется установить для `pg_stat_statements.track` `none`значение. Некоторые сторонние службы мониторинга могут полагаться на pg_stat_statements для предоставления аналитических сведений о производительности запросов, поэтому убедитесь в том, что это так.

## <a name="dblink-and-postgres_fdw"></a>дблинк и postgres_fdw
Дблинк и postgres_fdw можно использовать для подключения от одного сервера PostgreSQL к другому или к другой базе данных на том же сервере. Принимающий сервер должен разрешать подключения с отправляющего сервера при помощи его брандмауэра. Чтобы использовать эти расширения для подключения между серверами базы данных Azure для PostgreSQL, установите для параметра **Разрешить доступ к службам Azure** значение Вкл. Также необходимо включить этот параметр, если нужно использовать расширения для обратного прохода на тот же сервер. Параметр **Разрешить доступ к службам Azure** можно найти на портал Azure странице для сервера postgres в разделе **Безопасность подключения**. Включение **доступа к службам Azure** на добавляются всех IP-адресах Azure.

В настоящее время исходящие подключения из базы данных Azure для PostgreSQL не поддерживаются, за исключением подключений к другим серверам базы данных Azure для PostgreSQL.
