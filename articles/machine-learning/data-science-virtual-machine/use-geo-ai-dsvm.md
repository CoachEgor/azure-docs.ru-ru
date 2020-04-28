---
title: Использование географического AI
titleSuffix: Azure Data Science Virtual Machine
description: Сведения об использовании виртуальной машины обработки и анализа географических данных с помощью ИИ для анализа данных и построения моделей на основе геопространственных данных.
keywords: deep learning, AI, data science tools, data science virtual machine, geospatial analytics
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 03/05/2018
ms.openlocfilehash: 2d7532c51e2963c0dc9f8d02e7a0e32864e80f92
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "70278418"
---
# <a name="using-the-geo-artificial-intelligence-data-science-virtual-machine"></a>Использование виртуальной машины для обработки и анализа геоданных средствами ИИ

Виртуальная машина для обработки и анализа геоданных средствами искусственного интеллекта позволяет осуществлять выборку данных для анализа, обрабатывать данные и создавать модели для приложений ИИ, использующих геопространственную информацию. После подготовки виртуальной машины географического анализа данных AI и входа в ArcGIS Pro через учетную запись ArcGIS можно начать взаимодействие с ArcGIS Desktop и ArcGIs Online. Вы также можете получить доступ к ArcGIS из интерфейсов Python и сервисного моста R, предварительно настроенных на виртуальной машине для обработки географических данных. Для создания многофункциональных приложений AI объедините виртуальную машину для обработки географических данных с помощью платформ машинного обучения и глубокого обучения и других доступных на нем программ по обработке и анализу данных.  


## <a name="configuration-details"></a>Сведения о конфигурации

Библиотека Python, [ArcPy](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm), которая используется для взаимодействия с ArcGIS, устанавливается в глобальном корневом окружении conda виртуальной машины для обработки и анализа данных, которую можно найти ```c:\anaconda```по адресу.

- Если вы используете Python в командной строке, выполните ```activate``` команду, чтобы активировать среду в корневой среде Python conda.
- Если вы используете записную книжку IDE или Jupyter, можно выбрать среду или ядро, чтобы убедиться, что вы находитесь в правильной среде conda.

Мост R для ArcGIS устанавливается как библиотека R с именем [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) в главном Microsoft Machine Learning Server изолированном экземпляре, расположенном по адресу ```C:\Program Files\Microsoft\ML Server\R_SERVER```. Visual Studio, RStudio и Jupyter уже предварительно настроены на использование этой среды R и будут иметь доступ к библиотеке ```arcgisbinding``` r. 


## <a name="geo-ai-data-science-vm-samples"></a>Примеры виртуальной машины для обработки и анализа геоданных средствами ИИ

Помимо примеров на основе платформы машинного обучения и глубокого обучения из базовой виртуальной машины для обработки и анализа данных, набор геопространственных примеров также предоставляется в составе виртуальной машины Geo AI для обработки и анализа данных. Эти примеры помогут вам быстро начать разработку приложений AI с помощью геопространственных данных и программного обеспечения ArcGIS:


1. Начало работы [с геопространственной аналитикой с помощью Python](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/Python%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): вводный пример, демонстрирующий работу с геопространственными данными через интерфейс Python для ArcGIS, предоставляется библиотекой [ArcPy](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm) . Также показано, как объединить традиционные машинное обучение с геопространственными данными, а затем визуализировать результат на карте в ArcGIS.

2. Начало работы [с геопространственной аналитикой с помощью r](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/R%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): вводный пример, демонстрирующий работу с геопространственными данными с использованием интерфейса R для ArcGIS, предоставляемого библиотекой [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) . 

3. [Pixel-level land use classification](https://github.com/Azure/pixel_level_land_classification) (Классификация использования земли на уровне пикселей): учебное руководство по созданию модели глубокой нейронной сети, которая принимает аэрофотоснимок и возвращает метку покрытия земли. Примерами наметок земельных участков являются *леса* и *вода*. Модель возвращает такую метку для каждого пикселя в изображении. 


## <a name="next-steps"></a>Следующие шаги

Дополнительные примеры использования виртуальной машины для обработки и анализа данных см. здесь:

* [Примеры](dsvm-samples-and-walkthroughs.md)