---
title: Использование Apache Pig с SSH в кластере HDInsight в Azure
description: Узнайте, как установить SSH-подключение к кластеру Apache Hadoop под управлением Linux, а затем использовать команду Pig для выполнения операторов Pig Latin в интерактивном режиме или в виде пакетного задания.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: hrasheed
ms.openlocfilehash: 503449e36b2b11e5663449dd732fdaf785417570
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64727161"
---
# <a name="run-apache-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Выполнение заданий Apache Pig в кластере под управлением Linux с помощью команды Pig (SSH)

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Узнайте, как в интерактивном режиме выполнять задания Apache Pig с помощью SSH-подключения к кластеру HDInsight. Язык программирования Pig Latin позволяет описывать преобразования, применяемые к входным данным для получения требуемых выходных данных.

> [!IMPORTANT]  
> Для выполнения действий, описанных в этом документе, необходим кластер HDInsight под управлением Linux. Linux — это единственная операционная система, используемая для работы с HDInsight 3.4 или более поздних версий. Дополнительные сведения см. в разделе [Приближается дата прекращения сопровождения HDI версии 3.3](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="ssh"></a>Подключение по SSH

Подключитесь к кластеру HDInsight с помощью протокола SSH. Следующий пример подключается к кластеру **myhdinsight** с учетной записью **sshuser**.

```bash
ssh sshuser@myhdinsight-ssh.azurehdinsight.net
```

Дополнительные сведения см. в статье [Использование SSH с Hadoop на основе Linux в HDInsight из Linux, Unix или OS X](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="pig"></a>Использование команды Apache Pig

1. После установления подключения запустите интерфейс командной строки Pig с помощью следующей команды.

    ```bash
    pig
    ```

    Через некоторое время запрос изменится на `grunt>`.

2. Введите следующий оператор:

    ```piglatin
    LOGS = LOAD '/example/data/sample.log';
    ```

    Эта команда загружает содержимое файла sample.log в LOGS. Вы можете просмотреть содержимое файла с помощью следующей инструкции.

    ```piglatin
    DUMP LOGS;
    ```

3. Затем преобразуйте данные приведенной ниже инструкцией, применив регулярное выражение для извлечения из каждой записи только информации об уровне ведения журнала.

    ```piglatin
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    ```

    Вы можете использовать **DUMP** , чтобы просмотреть данные после преобразования. В этом случае используйте `DUMP LEVELS;`.

4. Продолжайте применение преобразований с помощью приведенных инструкций в следующей таблице.

    | Инструкция Pig Latin | Функция инструкции |
    | ---- | ---- |
    | `FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;` | Удаляет строки, содержащие значение NULL для уровня ведения журнала, и сохраняет результаты в `FILTEREDLEVELS`. |
    | `GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;` | Группирует строки по уровню ведения журнала и сохраняет результаты в `GROUPEDLEVELS`. |
    | `FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;` | Создает набор данных, который содержит каждое уникальное значение уровня ведения журнала и количество его вхождений. Набор данных сохраняется в `FREQUENCIES`. |
    | `RESULT = order FREQUENCIES by COUNT desc;` | Упорядочивает уровни ведения журнала по количеству (по убыванию) и сохраняет данные в `RESULT`. |

    > [!TIP]  
    > Используйте `DUMP` для просмотра результатов преобразования после каждого шага.

5. Можно также сохранить результаты преобразования с помощью оператора `STORE` . Например, следующая инструкция сохраняет `RESULT` в каталог `/example/data/pigout` в используемом по умолчанию хранилище для кластера.

    ```piglatin
    STORE RESULT into '/example/data/pigout';
    ```

   > [!NOTE]  
   > Данные хранятся в указанном каталоге в файлах с именем `part-nnnnn`. Если каталог уже существует, появится сообщение об ошибке.

6. Чтобы выйти из командной строки grunt, введите следующую инструкцию.

    ```piglatin
    QUIT;
    ```

### <a name="pig-latin-batch-files"></a>Пакетные файлы Pig Latin

Вы также можете использовать команду Pig для выполнения кода Pig Latin, содержащегося в файле.

1. После выхода из командной строки grunt используйте следующую команду, чтобы создать файл с именем `pigbatch.pig`:

    ```bash
    nano ~/pigbatch.pig
    ```

2. Введите или вставьте следующие строки:

    ```piglatin
    LOGS = LOAD '/example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;
    ```

    По завершении нажмите __CTRL__ + __X__, __Y__, а затем — клавишу __ВВОД__, чтобы сохранить файл.

3. Используйте следующую команду, чтобы запустить файл `pigbatch.pig` с помощью команды Pig.

    ```bash
    pig ~/pigbatch.pig
    ```

    После завершения пакетного задания можно увидеть результаты следующего вида.

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <a id="nextsteps"></a>Дальнейшие действия

Дополнительные общие сведения об использовании Pig в HDInsight см. в следующем документе:

* [Использование Apache Pig с Apache Hadoop в HDInsight](hdinsight-use-pig.md)

Дополнительные сведения о способах работы с Hadoop в HDInsight см. в следующих документах:

* [Использование Apache Hive с Apache Hadoop в HDInsight](hdinsight-use-hive.md)
* [Использование MapReduce в Apache Hadoop в HDInsight](hdinsight-use-mapreduce.md)
