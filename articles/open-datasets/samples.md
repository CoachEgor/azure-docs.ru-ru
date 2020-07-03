---
title: Примеры записных книжек Jupyter с использованием данных от Национального управления океанических и атмосферных исследований США
titleSuffix: Azure Open Datasets
description: Воспользуйтесь примерами записных книжек Jupyter для открытых наборов данных Azure, чтобы научиться загружать открытые наборы данных и обогащать с их помощью демонстрационные данные. Для обработки данных используются такие техники, как Spark и Pandas.
ms.service: open-datasets
ms.topic: sample
author: cjgronlund
ms.author: cgronlun
ms.date: 05/06/2020
ms.openlocfilehash: a25f0ddeb35aaf880682780fd61bcbc3cedbad08
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2020
ms.locfileid: "82930324"
---
# <a name="example-jupyter-notebooks-show-how-to-enrich-data-with-open-datasets"></a>Примеры записных книжек Jupyter демонстрируют, как обогащать данные с помощью открытых наборов данных 
Примеры записных книжек Jupyter для открытых наборов данных Azure демонстрируют, как загружать отрытые наборы данных и обогащать с их помощью демонстрационные данные. Для обработки данных используются такие техники, как Apache Spark и Pandas.

>[!IMPORTANT]
>При работе в среде, отличной от Spark, открытые наборы данных позволяют скачать за раз данные определенных классов только за один месяц. Это ограничение помогает избежать ошибок памяти при использовании больших наборов данных.

## <a name="load-noaa-integrated-surface-database-isd-data"></a>Загрузка данных из базы данных Integrated Surface Database (ISD) Национального управления океанических и атмосферных исследований США (NOAA) 
|Записная книжка        | Описание                                    |
|----------------|------------------------------------------------|
|[Загрузка метеоданных за один последний месяц в кадр данных Pandas](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-access/02-weather-to-pandas-dataframe.ipynb) | Узнайте, как загрузить хронологические метеоданные в предпочитаемый кадр данных Pandas. |
|[Загрузка одного последнего месяца метеоданных в кадр данных Spark](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-access/01-weather-to-spark-dataframe.ipynb) | Узнайте, как загрузить исторические метеоданные в предпочитаемый кадр данных Spark.  |

## <a name="join-demo-data-with-noaa-isd-data"></a>Объединение демонстрационных данных с данными ISD NOAA 
|Записная книжка        | Описание                                    |
|----------------|------------------------------------------------|
|[Объединение демонстрационных данных с метеоданными — Pandas](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/02-weather-join-in-pandas.ipynb) | Объедините демонстрационный набор данных о положениях датчиков за 1 месяц с метеоданными в кадре данных Pandas.  |
|[Объединение демонстрационных данных с метеоданными — Spark](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/01-weather-join-in-spark.ipynb) | Объедините демонстрационный набор данных о положениях датчиков с метеоданными в кадре данных Spark. |

## <a name="join-nyc-taxi-data-with-noaa-isd-data"></a>Объединение данных о такси Нью-Йорка с данными ISD NOAA 
|Записная книжка        | Описание                                    |
|----------------|------------------------------------------------|
|[Данные о поездках в такси, обогащенные метеоданными — Pandas](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/04-nyc-taxi-join-weather-in-pandas.ipynb) | Загрузите данные о зеленых такси в Нью-Йорке (более чем за 1 месяц) и обогатите их метеоданными в кадре данных Pandas. Этот пример переопределяет метод `get_pandas_limit` и выполняет балансировку производительности нагрузки с учетом объема данных.|
|[Данные о поездках в такси, обогащенные метеоданными — Spark](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/03-nyc-taxi-join-weather-in-spark.ipynb) | Загрузите данные о зеленых такси в Нью-Йорке и обогатите их метеоданными в кадре данных Spark.  |

## <a name="next-steps"></a>Дальнейшие действия

* [Руководство. Создание регрессионной модели с помощью автоматизированного машинного обучения и открытого набора данных](/azure/machine-learning/tutorial-auto-train-models?context=azure/open-datasets/context/open-datasets-context)
* [Пакет SDK для Python для открытых наборов данных](/python/api/azureml-opendatasets/azureml.opendatasets?view=azure-ml-py)
* [Каталог открытых наборов данных Azure](https://azure.microsoft.com/services/open-datasets/catalog/)
