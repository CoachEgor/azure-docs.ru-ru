---
title: Изучение данных на виртуальной машине сервера SQL Server — командный процесс обработки и анализа данных
description: Сведения о том, как просматривать данные, хранящиеся на виртуальной машине SQL Server в Azure, с помощью SQL или языка программирования, например Python.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: be75490e4e86956337ce38133df6095790b3a374
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60303661"
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Просмотр данных в виртуальной машине SQL Server на платформе Azure

В этой статье описывается, как просматривать данные, хранящиеся в виртуальной машине SQL Server на платформе Azure. Это можно сделать путем структурирования данных с помощью SQL или с использованием языка программирования, например Python.

Эта задача является одним из этапов [процесса обработки и анализа данных группы](overview.md).

> [!NOTE]
> В образцах инструкций SQL, содержащихся в данном документе, предполагается, что данные находятся на сервере SQL Server. Если это не так, обратитесь к карте обработки облачных данных, чтобы узнать, как переместить данные в SQL Server.
> 
> 

## <a name="sql-dataexploration"></a>Просмотр данных SQL с помощью сценариев SQL
Вот несколько примеров сценариев SQL, которые можно использовать для изучения хранилищ данных в SQL Server.

1. Получение количества наблюдений за день
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Получение уровней в столбце категорий
   
    `select  distinct <column_name> from <databasename>`
3. Получение числа уровней в сочетании двух столбцов категорий 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Получение распределения для числовых столбцов 
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> Для практического примера можно использовать [набор данных о такси Нью-Йорка](https://www.andresmh.com/nyctaxitrips/) и статью IPNB под названием [Структурирование данных Нью-Йорка с помощью IPython Notebook и SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb), содержащую полное пошаговое руководство.
> 
> 

## <a name="python"></a>Просмотр данных SQL с помощью Python
Использование языка Python для просмотра данных и создания характеристик, когда данные находятся в SQL Server, подобно обработке данных в большом двоичном объекте Azure с использованием Python, как описано в статье [Обработка больших двоичных данных Azure с применением методов расширенного анализа](data-blob.md). Данные необходимо загрузить из базы данных в кадр данных Pandas для последующей обработки. В этом разделе задокументирован процесс подключения к базе данных и загрузки данных в кадр данных.

Для подключения к базе данных SQL Server из языка Python с использованием pyodbc можно применить следующий формат строки подключения (замените servername, dbname, username и password соответствующими значениями имени сервера, имени БД, имени пользователя и пароля):

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Библиотека Pandas](https://pandas.pydata.org/) в языке Python предлагает большой выбор структур данных и средств анализа данных для манипуляций со значениями с помощью языке Python. Следующий код считывает результаты, возвращенные из базы данных SQL Server, в кадр данных Pandas:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <columnname2>... from <tablename>''', conn)

Теперь можно работать с кадром данных Pandas, как описано в статье [Обработка больших двоичных данных Azure с применением методов расширенного анализа](data-blob.md).

## <a name="the-team-data-science-process-in-action-example"></a>Пример применения командного процесса обработки и анализа данных на практике
Полноценный пошаговый пример применения процесса Cortana Analytics с использованием общедоступного набора данных см. в статье [Процесс обработки и анализа данных группы на практике: использование SQL Server](sql-walkthrough.md).

