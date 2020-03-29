---
title: Преобразование данных с помощью активности Spark
description: Узнайте, как преобразовать данные, запустив программы Spark из конвейера фабрики данных Azure с помощью действия Spark.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: nabhishek
ms.author: abnarain
manager: shwang
ms.custom: seo-lt-2019
ms.date: 05/31/2018
ms.openlocfilehash: eb887a7d9081875c28964ddb1e3d1b2e609862fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "74912972"
---
# <a name="transform-data-using-spark-activity-in-azure-data-factory"></a>Преобразование данных с помощью действия Spark в фабрике данных Azure
> [!div class="op_single_selector" title1="Выберите используемую версию службы "Фабрика данных":"]
> * [Версия 1](v1/data-factory-spark.md)
> * [Текущая версия](transform-data-using-spark.md)

Действие Spark в [конвейере](concepts-pipelines-activities.md) фабрики данных выполняет программу Spark в [вашем](compute-linked-services.md#azure-hdinsight-linked-service) кластере или в кластере [по требованию](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight. Данная статья основана на материалах статьи о [действиях преобразования данных](transform-data.md) , в которой приведен общий обзор преобразования данных и список поддерживаемых действий преобразования. Если вы используете связанную службу Spark по требованию, фабрика данных автоматически создает кластер Spark для обработки данных, когда это необходимо, а затем удаляет кластер после завершения обработки. 


## <a name="spark-activity-properties"></a>Свойства действия Spark
Ниже приведен пример определения JSON действия Spark.    

```json
{
    "name": "Spark Activity",
    "description": "Description",
    "type": "HDInsightSpark",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "sparkJobLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "rootPath": "adfspark",
        "entryFilePath": "test.py",
        "sparkConfig": {
            "ConfigItem1": "Value"
        },
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ]
    }
}
```

В следующей таблице приведено описание свойств, используемых в определении JSON.

| Свойство              | Описание                              | Обязательно |
| --------------------- | ---------------------------------------- | -------- |
| name                  | Имя действия в конвейере.    | Да      |
| description           | Описание действия.  | нет       |
| type                  | Для действия Spark используется тип действия HDInsightSpark. | Да      |
| linkedServiceName     | Имя связанной службы Spark HDInsight, в которой выполняется программа Spark. Дополнительные сведения об этой связанной службе см. в статье [Вычислительные среды, поддерживаемые фабрикой данных Azure](compute-linked-services.md). | Да      |
| SparkJobLinkedService | Связанная служба службы хранилища Azure, в которой хранятся файл задания Spark, зависимости и журналы.  Если значение этого свойства не указано, используется хранилище, связанное с кластером HDInsight. Значением этого свойства может быть только связанная служба хранилища Azure. | нет       |
| rootPath              | Контейнер BLOB-объектов Azure и папка, содержащая файл Spark. В имени файла учитывается регистр знаков. Подробнее о структуре этой папки см. в разделе о структуре папок (следующий раздел). | Да      |
| entryFilePath         | Относительный путь к корневой папке пакета и кода Spark. Файл записи должен быть файлом Python или JAR-файлом. | Да      |
| className             | Основной класс Java или Spark приложения.      | нет       |
| аргументы             | Список аргументов командной строки для программы Spark. | нет       |
| proxyUser             | Учетная запись пользователя для олицетворения, используемая для выполнения программы Spark. | нет       |
| sparkConfig           | Укажите значения для свойств конфигурации Spark, перечисленных в разделе [Конфигурация Spark — свойства приложения](https://spark.apache.org/docs/latest/configuration.html#available-properties). | нет       |
| getDebugInfo          | Указывает, когда файлы журнала Spark копируются в службу хранилища Azure, используемое кластером HDInsight или определенное sparkJobLinkedService. Допустимые значения: None, Always или Failure. Значение по умолчанию: None. | нет       |

## <a name="folder-structure"></a>Структура папок
Задания Spark обеспечивают большую гибкость, чем задания Pig и Hive. Для заданий Spark можно указать несколько зависимостей, например пакеты JAR (размещаются в CLASSPATH Java), файлы Python (размещаются в PYTHONPATH) и другие файлы.

Создайте следующую структуру папок в хранилище BLOB-объектов Azure, на которое ссылается связанная служба HDInsight. Затем передайте зависимые файлы в соответствующие вложенные папки в корневой папке, определенной значением **entryFilePath**. Например, передайте файлы Python во вложенную папку pyFiles, а JAR-файлы — во вложенную папку jars, расположенный в корневой папке. Во время выполнения служба фабрики данных ожидает в хранилище BLOB-объектов Azure следующую структуру папок.     

| путь                  | Описание                              | Обязательно | Тип   |
| --------------------- | ---------------------------------------- | -------- | ------ |
| `.` (root)            | Путь к корневому каталогу задания Spark в хранилище связанной службы. | Да      | Папка |
| &lt;Определяется пользователем&gt; | Путь к файлу записи задания Spark. | Да      | Файл   |
| ./jars                | Все файлы в этой папке передаются и помещаются в папку CLASSPATH Java для кластера. | нет       | Папка |
| ./pyFiles             | Все файлы в этой папке передаются и помещаются в папку PYTHONPATH для кластера. | нет       | Папка |
| ./files               | Все файлы в этой папке передаются и помещаются в рабочий каталог исполнителя. | нет       | Папка |
| ./archives            | Все файлы в этой папке не сжаты. | нет       | Папка |
| ./logs                | Папка, в которой содержатся журналы из кластера Spark. | нет       | Папка |

Ниже приведен пример хранилища, содержащего два файла заданий Spark в хранилище BLOB-объектов Azure, на которое ссылается связанная служба HDInsight.

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
## <a name="next-steps"></a>Дальнейшие действия
Ознакомьтесь со следующими ссылками, в которых описаны способы преобразования данных другими способами: 

* [Действие U-SQL](transform-data-using-data-lake-analytics.md)
* [Действие Hive](transform-data-using-hadoop-hive.md)
* [Свинья деятельность](transform-data-using-hadoop-pig.md)
* [Действие MapReduce](transform-data-using-hadoop-map-reduce.md)
* [Действие потоковой передачи Hadoop](transform-data-using-hadoop-streaming.md)
* [Активность искры](transform-data-using-spark.md)
* [пользовательская деятельность .NET](transform-data-using-dotnet-custom-activity.md)
* [Деятельность по выполнению пакета машинного обучения](transform-data-using-machine-learning.md)
* [Сохраненная процедура деятельности](transform-data-using-stored-procedure.md)
