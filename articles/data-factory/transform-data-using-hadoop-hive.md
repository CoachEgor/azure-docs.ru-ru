---
title: Преобразование данных с помощью действия Hadoop Hive
description: Узнайте, как с помощью действия Hive в фабрике данных Azure выполнять запросы Hive к кластеру HDInsight по требованию или собственному кластеру HDInsight.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.devlang: na
ms.topic: conceptual
author: nabhishek
ms.author: abnarain
manager: anandsub
ms.custom: seo-lt-2019
ms.date: 01/15/2019
ms.openlocfilehash: 8c5c917e12b1314c40763f58a7723a4df787ffa0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81418938"
---
# <a name="transform-data-using-hadoop-hive-activity-in-azure-data-factory"></a>Преобразование данных с помощью действия Hadoop Hive в фабрике данных Azure

> [!div class="op_single_selector" title1="Выберите используемую версию службы "Фабрика данных":"]
> * [Версия 1](v1/data-factory-hive-activity.md)
> * [Текущая версия](transform-data-using-hadoop-hive.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Действие Hive HDInsight в [конвейере](concepts-pipelines-activities.md) фабрики данных выполняет запросы Hive к [вашему собственному](compute-linked-services.md#azure-hdinsight-linked-service) кластеру HDInsight или кластеру HDInsight [по запросу](compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Данная статья основана на материалах статьи о [действиях преобразования данных](transform-data.md) , в которой приведен общий обзор преобразования данных и список поддерживаемых действий преобразования.

Если вы не знакомы с фабрикой данных Azure, сначала ознакомьтесь со статьей [Введение в фабрику данных Azure](introduction.md) и руководством [Преобразование данных в облаке с помощью действия Spark в фабрике данных Azure](tutorial-transform-data-spark-powershell.md). 

## <a name="syntax"></a>Синтаксис

```json
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "scriptLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "scriptPath": "MyAzureStorage\\HiveScripts\\MyHiveSript.hql",
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ],
        "defines": {
            "param1": "param1Value"
        }
    }
}
```
## <a name="syntax-details"></a>Сведения о синтаксисе
| Свойство            | Описание                                                  | Обязательный |
| ------------------- | ------------------------------------------------------------ | -------- |
| name                | Имя действия.                                         | Да      |
| description         | Текст, описывающий, для чего используется действие                | Нет       |
| type                | Для действия Hive используется тип действия HDinsightHive.        | Да      |
| linkedServiceName   | Ссылка на кластер HDInsight, зарегистрированный в качестве связанной службы в фабрике данных. Дополнительные сведения об этой связанной службе см. в статье [Вычислительные среды, поддерживаемые фабрикой данных Azure](compute-linked-services.md). | Да      |
| scriptLinkedService | Ссылки на связанные службы хранилища Azure, используемые для хранения скрипта Hive, который следует выполнить. Если не указать эту связанную службу, будет использоваться связанная служба хранилища Azure, определенная в связанной службе HDInsight. | Нет       |
| scriptPath          | Укажите путь к файлу скрипта, который хранится в службе хранилища Azure, на который ссылается scriptLinkedService. В имени файла учитывается регистр знаков. | Да      |
| getDebugInfo        | Указывает, когда файлы журнала копируются в службу хранилища Azure, используемую кластером HDInsight или определенную scriptLinkedService. Допустимые значения: None, Always или Failure. Значение по умолчанию: None. | Нет       |
| аргументы           | Указывает массив аргументов для задания Hadoop. Аргументы передаются в качестве аргументов командной строки в каждую задачу. | Нет       |
| defines             | Параметры в виде пары "ключ — значение", ссылки на которые указываются в скрипте Hive. | Нет       |
| queryTimeout        | Значение времени ожидания запроса (в минутах). Применяется, если кластер HDInsight доступный с Корпоративными пакетами безопасности. | Нет       |

## <a name="next-steps"></a>Дальнейшие шаги
Ознакомьтесь со следующими ссылками, в которых описаны способы преобразования данных другими способами: 

* [Действие U-SQL](transform-data-using-data-lake-analytics.md)
* [Действие Pig](transform-data-using-hadoop-pig.md)
* [Действие MapReduce](transform-data-using-hadoop-map-reduce.md)
* [Действие потоковой передачи Hadoop](transform-data-using-hadoop-streaming.md)
* [Действие Spark](transform-data-using-spark.md)
* [Настраиваемое действие .NET](transform-data-using-dotnet-custom-activity.md)
* [Действие выполнения пакета Машинное обучение](transform-data-using-machine-learning.md)
* [Действие хранимой процедуры](transform-data-using-stored-procedure.md)
