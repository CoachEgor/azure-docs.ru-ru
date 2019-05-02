---
title: Использование MapReduce в Apache Hadoop в HDInsight
description: Узнайте, как запускать задания MapReduce в Apache Hadoop в кластере HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/20/2019
ms.openlocfilehash: 9da6b6ba3ab697887e55f9077b44cf6fa100a981
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64707964"
---
# <a name="use-mapreduce-in-apache-hadoop-on-hdinsight"></a>Использование MapReduce в Apache Hadoop в HDInsight

Узнайте, как выполнять задания MapReduce в кластерах HDInsight. 

## <a id="whatis"></a>Что такое MapReduce

Apache Hadoop MapReduce — это программная платформа для создания заданий, обрабатывающих большие объемы данных. Входные данные разбиваются на независимые блоки, которые затем обрабатываются параллельно на узлах кластера. Задание MapReduce состоит из двух функций.

* **Mapper** (Модуль сопоставления) — принимает входные данные, анализирует их (обычно с помощью фильтрации и сортировки) и передает кортежи (пары "ключ — значение").

* **Reducer** (Редуктор) — принимает кортежи, сформированные в модуле сопоставления, и выполняет операцию сводки, которая создает результат меньшего размера, объединяющий данные модуля сопоставления.

На следующей диаграмме показан пример задания MapReduce, которое выполняет простую операцию подсчета слов:

![HDI.WordCountDiagram][image-hdi-wordcountdiagram]

Выходные данные этого задания представляют собой частоту использования каждого слова в тексте.

* Процедура map берет каждую строку из входного текста в качестве входных данных и разбивает ее на слова. Она генерирует пару «ключ-значение» каждый раз, когда встречается слово, за которым следует 1. Перед отправкой на обработку редуктором выходные данные сортируются.
* Затем редуктор суммирует эти отдельные счетчики для каждого слова и выдает одну пару «ключ-значение», содержащую слово, за которым следует частота его использования.

Задание MapReduce может быть реализовано на различных языках. Java — это наиболее распространенная реализация, которая используется в данном документе для примера.

## <a name="development-languages"></a>Языки разработки

Языки или платформы на основе Java или виртуальной машины Java можно запускать непосредственно как задание MapReduce. В качестве примера в этом документе приведено приложение MapReduce на языке Java. Прочие языки и платформы, например C# или Python, или изолированные исполняемые файлы должны использовать **потоковую передачу Hadoop**.

Потоковая передача Hadoop взаимодействует с модулями сопоставления и редукции через потоки STDIN и STDOUT. Модули сопоставления и редукции построчно считывают данные из потока STDIN и записывают выходные данные в поток STDOUT. Каждая строка, которая читается или генерируется модулем сопоставления или редукции, должна быть в формате пар "ключ-значение", разделенных знаком табуляции.

    [key]/t[value]

Дополнительные сведения см. в документации по [потоковой передаче Hadoop](https://hadoop.apache.org/docs/r1.2.1/streaming.html).

Примеры использования потоковой передачи Hadoop с HDInsight см. в следующем документе:

* [Использование языка C# для потоковой передачи MapReduce в Hadoop в HDInsight](apache-hadoop-dotnet-csharp-mapreduce-streaming.md)

## <a id="data"></a>Демонстрационные данные

HDInsight предоставляет различные наборы демонстрационных данных, которые хранятся в каталогах `/example/data` и `/HdiSamples`. Эти каталоги находятся в хранилище по умолчанию для кластера. В этом документе мы используем файл `/example/data/gutenberg/davinci.txt`. Этот файл содержит записные книжки Леонардо да Винчи.

## <a id="job"></a>Пример MapReduce

Пример приложения MapReduce для подсчета слов входит в состав кластера HDInsight. Этот примере расположен в каталоге `/example/jars/hadoop-mapreduce-examples.jar` в хранилище по умолчанию для кластера.

Ниже приведен исходный код Java приложения MapReduce из файла `hadoop-mapreduce-examples.jar`.

```java
package org.apache.hadoop.examples;

import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.GenericOptionsParser;

public class WordCount {

    public static class TokenizerMapper
        extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
        StringTokenizer itr = new StringTokenizer(value.toString());
        while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
        }
    }
    }

    public static class IntSumReducer
        extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                        Context context
                        ) throws IOException, InterruptedException {
        int sum = 0;
        for (IntWritable val : values) {
        sum += val.get();
        }
        result.set(sum);
        context.write(key, result);
    }
    }

    public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
    if (otherArgs.length != 2) {
        System.err.println("Usage: wordcount <in> <out>");
        System.exit(2);
    }
    Job job = new Job(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
    FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```

Инструкции по написанию приложений MapReduce см. следующий документ:

* [Разработка программ MapReduce на Java для Hadoop в HDInsight на платформе Linux](apache-hadoop-develop-deploy-java-mapreduce-linux.md)

## <a id="run"></a>Запуск MapReduce

HDInsight может выполнять задания HiveQL, используя различные методы. Используйте следующую таблицу, чтобы решить, какой метод подходит вам, а затем перейдите по ссылке к пошаговому руководству.

| **Используется**... | **... чтобы сделать** | ...with этим **кластером операционной системы** | ...из этого **кластера операционной системы** |
|:--- |:--- |:--- |:--- |
| [SSH](apache-hadoop-use-mapreduce-ssh.md) |Использование команды Hadoop через **SSH** |Linux |Linux, Unix, Mac OS X или Windows |
| [Curl](apache-hadoop-use-mapreduce-curl.md) |Удаленная отправка заданий с помощью **REST** |Windows или Linux |Linux, Unix, Mac OS X или Windows |
| [Windows PowerShell](apache-hadoop-use-mapreduce-powershell.md) |Удаленная отправка заданий с помощью **Windows PowerShell** |Windows или Linux | Windows |

## <a id="nextsteps"></a>Дальнейшие действия

Чтобы узнать больше о работе с данными в HDInsight, ознакомьтесь со следующими документами:

* [Разработка программ MapReduce на Java для HDInsight](apache-hadoop-develop-deploy-java-mapreduce-linux.md)

* [Использование Apache Hive с HDInsight][hdinsight-use-hive]

* [Использование Apache Pig с HDInsight][hdinsight-use-pig]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: apache-hadoop-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]:../hdinsight-use-hive.md
[hdinsight-use-pig]:hdinsight-use-pig.md


[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
