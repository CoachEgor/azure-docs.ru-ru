---
title: Разностный формат в фабрике данных Azure
description: Преобразование и перемещение данных из разностного метода Lake с использованием разностного формата
author: djpmsft
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/07/2020
ms.author: daperlov
ms.openlocfilehash: 794c9a0768a7b649ce4fb123c85f6cc0120764c8
ms.sourcegitcommit: 48cb2b7d4022a85175309cf3573e72c4e67288f5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/08/2020
ms.locfileid: "96854977"
---
# <a name="delta-format-in-azure-data-factory"></a>Разностный формат в фабрике данных Azure

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

В этой статье объясняется, как копировать данные в разностный и обратно сохраненный в [Azure Data Lake Store Gen2](connector-azure-data-lake-storage.md) или [хранилище BLOB-объектов Azure](connector-azure-blob-storage.md) с использованием разностного формата. Этот соединитель доступен как [встроенный набор данных](data-flow-source.md#inline-datasets) в потоке сопоставления данных в качестве источника и приемника.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4ALTs]

## <a name="mapping-data-flow-properties"></a>Свойства потока данных для сопоставления

Этот соединитель доступен как [встроенный набор данных](data-flow-source.md#inline-datasets) в потоке сопоставления данных в качестве источника и приемника.

### <a name="source-properties"></a>Свойства источника

В таблице ниже перечислены свойства, поддерживаемые источником разностных данных. Эти свойства можно изменить на вкладке **Параметры источника** .

| Название | Описание | Обязательный | Допустимые значения | Свойство сценария потока данных |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Формат | Формат должен быть `delta` | да | `delta` | format |
| Файловая система | Контейнер/файловая система разностной версии Lake | да | Строка | fileSystem |
| Путь к папке | Непосредственное от Дельта Lake | да | Строка | folderPath |
| Тип сжатия | Тип сжатия разностной таблицы | Нет | `bzip2`<br>`gzip`<br>`deflate`<br>`ZipDeflate`<br>`snappy`<br>`lz4` | компрессионтипе |
| Уровень сжатия | Выберите, будет ли сжатие выполняться как можно быстрее или если полученный файл должен быть оптимально сжат. | обязательный `compressedType` , если указан. | `Optimal` или `Fastest` | compressionLevel |
| Путешествие по времени | Выберите, следует ли запрашивать старый моментальный снимок разностной таблицы | Нет | Запрос по метке времени: timestamp <br> Запрос по версии: целое число | тиместампасоф <br> версионасоф |
| Разрешить не найдены файлы | Если значение — true, ошибка не возникает, если файлы не найдены | Нет | `true` или `false` | игноренофилесфаунд |

#### <a name="import-schema"></a>Импорт схемы

Дельта доступна только в качестве встроенного набора данных и по умолчанию не имеет связанной схемы. Чтобы получить метаданные столбца, нажмите кнопку **Импорт схемы** на вкладке **проекция** . Это позволит сослаться на имена столбцов и типы данных, заданные параметром совокупности. Чтобы импортировать схему, [сеанс отладки потока данных](concepts-data-flow-debug-mode.md) должен быть активным и иметь существующий файл определения сущности CDM для указания.
 

### <a name="delta-source-script-example"></a>Пример сценария Дельта исходного кода

```
source(output(movieId as integer,
            title as string,
            releaseDate as date,
            rated as boolean,
            screenedOn as timestamp,
            ticketPrice as decimal(10,2)
            ),
    store: 'local',
    format: 'delta',
    versionAsOf: 0,
    allowSchemaDrift: false,
    folderPath: $tempPath + '/delta'
  ) ~> movies
```

### <a name="sink-properties"></a>Свойства приемника

В таблице ниже перечислены свойства, поддерживаемые разностным приемником. Эти свойства можно изменить на вкладке **Параметры** .

| Название | Описание | Обязательный | Допустимые значения | Свойство сценария потока данных |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Формат | Формат должен быть `delta` | да | `delta` | format |
| Файловая система | Контейнер/файловая система разностной версии Lake | да | Строка | fileSystem |
| Путь к папке | Непосредственное от Дельта Lake | да | Строка | folderPath |
| Тип сжатия | Тип сжатия разностной таблицы | Нет | `bzip2`<br>`gzip`<br>`deflate`<br>`ZipDeflate`<br>`snappy`<br>`lz4` | компрессионтипе |
| Уровень сжатия | Выберите, будет ли сжатие выполняться как можно быстрее или если полученный файл должен быть оптимально сжат. | обязательный `compressedType` , если указан. | `Optimal` или `Fastest` | compressionLevel |
| Очистка | Укажите пороговое значение хранения в часах для старых версий таблицы. Значение по умолчанию (0 или меньше) — 30 дней. | да | Целое число | указывается |
| Update - метод | Укажите, какие операции обновления разрешены в Дельта Lake. Для методов, которые не вставляются, для пометки строк требуется предшествующее преобразование ALTER Row. | да | `true` или `false` | удаляемые <br> Insertable <br> обновляемые <br> merge |

### <a name="delta-sink-script-example"></a>Пример скрипта разностного приемника

Связанный сценарий потока данных:

```
moviesAltered sink(
          input(movieId as integer,
                title as string
            ),
           mapColumn(
                movieId,
                title
            ),
           insertable: true,
           updateable: true,
           deletable: true,
           upsertable: false,
           keys: ['movieId'],
            store: 'local',
           format: 'delta',
           vacuum: 180,
           folderPath: $tempPath + '/delta'
           ) ~> movieDB
```

### <a name="known-limitations"></a>Известные ограничения

При записи в разностный приемник существует известное ограничение, при котором количество записанных строк не будет возвращаться в выходных данных мониторинга.

## <a name="next-steps"></a>Дальнейшие действия

* Создайте [Преобразование «источник](data-flow-source.md) » в потоке данных сопоставления.
* Создайте [Преобразование приемника](data-flow-sink.md) в потоке данных сопоставления.
* Создайте [Преобразование «изменение строки](data-flow-alter-row.md) », чтобы пометить строки как вставки, обновления, Upsert или удаления.
