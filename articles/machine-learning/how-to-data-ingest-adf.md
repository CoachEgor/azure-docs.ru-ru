---
title: Прием данных с помощью Фабрики данных Azure
titleSuffix: Azure Machine Learning
description: Узнайте о доступных вариантах создания конвейера приема данных с помощью фабрики данных Azure и преимуществ каждого из них.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: iefedore
author: eedorenko
manager: davete
ms.reviewer: larryfr
ms.date: 03/01/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python, data4ml
ms.openlocfilehash: 73850d8022618dd6544d19564e425288aff09771
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93360638"
---
# <a name="data-ingestion-with-azure-data-factory"></a>Прием данных с помощью Фабрики данных Azure

В этой статье вы узнаете о доступных вариантах создания конвейера приема данных с помощью фабрики данных Azure (ADF). Этот конвейер используется для приема данных для использования с Машинное обучение Azure. Фабрика данных Azure позволяет легко извлекать, преобразовывать и загружать данные (ETL). После преобразования и загрузки данных в хранилище их можно использовать для обучения моделей машинного обучения.

Простое преобразование данных можно обрабатывать с помощью собственных действий ADF и инструментов, таких как [поток данных](../data-factory/control-flow-execute-data-flow-activity.md). Когда речь идет о более сложных сценариях, данные могут обрабатываться с помощью пользовательского кода. Например, код Python или R.

Существует несколько распространенных способов использования фабрики данных Azure для преобразования данных во время приема. Каждый метод имеет свои достоинства и недостатки, которые определяют, подходит ли он для конкретного варианта использования:

| Метод | Плюсы | Минусы |
| ----- | ----- | ----- |
| ADF + функции Azure | Низкая задержка, бессерверные вычислений</br>Функции с отслеживанием состояния</br>Многократно используемые функции | Хорошо работает только для кратковременной обработки |
| ADF + пользовательский компонент | Крупномасштабные параллельные вычисления</br>Подходит для интенсивных алгоритмов | Перенос кода в исполняемый файл</br>Сложность обработки зависимостей и операций ввода-вывода |
| Блокнот ADF + Azure Databricks | Apache Spark</br>Собственная среда Python | Может быть дорогостоящим</br>Создание кластеров изначально занимает некоторое время и увеличивает задержку

## <a name="adf-with-azure-functions"></a>ADF с функциями Azure

![На схеме показан конвейер фабрики данных Azure с помощью функции Azure и выполнения конвейера M L, а также конвейер Машинное обучение Azure, с обучением модели и их взаимодействия с необработанными данными и подготовленными данными.](media/how-to-data-ingest-adf/adf-function.png)

Функции Azure позволяют запускать небольшие фрагменты кода (функции), не беспокоясь о инфраструктуре приложений. В этом случае данные обрабатываются с помощью пользовательского кода Python, упакованного в функцию Azure. 

Функция вызывается с помощью [действия функции Azure ADF](../data-factory/control-flow-azure-function-activity.md). Этот подход является хорошим вариантом для упрощенных преобразований данных. 

* Преимущества.
    * Данные обрабатываются в бессерверных расчетах с относительно низкой задержкой.
    * Конвейер ADF может вызывать [устойчивую функцию Azure](../azure-functions/durable/durable-functions-overview.md) , которая может реализовать сложный поток преобразования данных. 
    * Сведения о преобразовании данных являются абстрактными для функции Azure, которую можно повторно использовать и вызывать из других мест.
* Недостатки.
    * Функции Azure необходимо создать перед использованием с ADF
    * Функции Azure хороши только для кратковременной обработки данных.

## <a name="adf-with-custom-component-activity"></a>ADF с действием настраиваемого компонента

![На схеме показан конвейер фабрики данных Azure с пользовательским компонентом и запуск конвейера M L, а также конвейер Машинное обучение Azure, с обучением модели и способ взаимодействия с необработанными данными и подготовленными данными.](media/how-to-data-ingest-adf/adf-customcomponent.png)

В этом случае данные обрабатываются с помощью пользовательского кода Python, упакованного в исполняемый файл. Он вызывается с помощью [действия настраиваемого компонента ADF](../data-factory/transform-data-using-dotnet-custom-activity.md). Этот подход лучше подходит для больших данных, чем предыдущий метод.

* Преимущества.
    * Данные обрабатываются в пуле [пакетной службы Azure](../batch/batch-technical-overview.md) , который обеспечивает крупномасштабные параллельные и высокопроизводительные вычисления.
    * Может использоваться для запуска интенсивных алгоритмов и обработки значительных объемов данных.
* Недостатки.
    * Пул пакетной службы Azure необходимо создать перед использованием с ADF
    * Помимо проектирования, связанной с переносом кода Python в исполняемый файл. Сложность обработки зависимостей и входных и выходных параметров

## <a name="adf-with-azure-databricks-python-notebook"></a>Файл ADF с записной книжкой Azure Databricks Python

![На схеме показан конвейер фабрики данных Azure с Azure Databricks Python и выполнением конвейера M L, а также конвейером Машинное обучение Azure, с обучением модели и способом взаимодействия с необработанными данными и подготовленными данными.](media/how-to-data-ingest-adf/adf-databricks.png)

[Azure Databricks](https://azure.microsoft.com/services/databricks/) — это платформа аналитики на основе Apache Spark в Microsoft Cloud.

В этом методе преобразование данных выполняется в [записной книжке Python](../data-factory/transform-data-using-databricks-notebook.md), которая выполняется в кластере Azure Databricks. Возможно, это самый распространенный подход, использующий всю мощь службы Azure Databricks. Он предназначен для распределенной обработки данных в масштабе.

* Преимущества.
    * Данные преобразуются в самую мощную службу Azure для обработки данных, резервная копия которой задается средой Apache Spark
    * Собственная поддержка Python вместе с платформами и библиотеками для обработки и анализа данных, включая TensorFlow, PyTorch и scikit-учиться
    * Нет необходимости упаковывать код Python в функции или исполняемые модули. Код работает как есть.
* Недостатки.
    * Azure Databricks инфраструктуру необходимо создать перед использованием с ADF
    * Может быть дорогостоящей в зависимости от конфигурации Azure Databricks
    * Вращение кластеров вычислений из "холодного" режима занимает некоторое время, что приводит к высокой задержке решения. 
    

## <a name="consuming-data-in-azure-machine-learning-pipelines"></a>Использование данных в конвейерах Машинное обучение Azure

![На схеме показан конвейер фабрики данных Azure и конвейер Машинное обучение Azure, а также взаимодействие с необработанными данными и подготовленными данными. Конвейер фабрики данных передает данные в подготовленную базу данных, которая передает данные в хранилище данных, которые являются каналами в рабочей области Машинное обучение.](media/how-to-data-ingest-adf/aml-dataset.png)

Преобразованные данные из конвейера ADF сохраняются в хранилище данных (например, в большом двоичном объекте Azure). Машинное обучение Azure может получать доступ к этим данным с помощью [хранилищ](./how-to-access-data.md#create-and-register-datastores) данных и [DataSets](./how-to-create-register-datasets.md).

При каждом запуске конвейера ADF данные сохраняются в другом месте в хранилище. Чтобы передать расположение в Машинное обучение Azure, конвейер ADF вызывает конвейер Машинное обучение Azure. При вызове конвейера машинного обучения расположение данных и идентификатор запуска отправляются в качестве параметров. Затем конвейер машинного обучения может создать хранилище данных или набор данных с помощью расположения. 

> [!TIP]
> Наборы данных [поддерживают управление версиями](./how-to-version-track-datasets.md), поэтому конвейер машинного обучения может зарегистрировать новую версию DataSet, которая указывает на самые последние данные из конвейера ADF.

После доступа к данным через хранилище данных или набор данных их можно использовать для обучения модели машинного обучения. Процесс обучения может быть частью того же конвейера ML, который вызывается из ADF. Или это может быть отдельный процесс, например экспериментирование в записной книжке Jupyter.

Так как наборы данных поддерживают управление версиями, и каждый запуск из конвейера создает новую версию, легко понять, какая версия используется для обучения модели.

## <a name="next-steps"></a>Дальнейшие действия

* [Запуск записной книжки кирпичей в фабрике данных Azure](../data-factory/transform-data-using-databricks-notebook.md)
* [Доступ к данным в службах хранилища Azure](./how-to-access-data.md#create-and-register-datastores)
* [Обучение моделей с помощью наборов данных в Машинное обучение Azure](./how-to-train-with-datasets.md)
* [DevOps для конвейера приема данных](./how-to-cicd-data-ingestion.md)