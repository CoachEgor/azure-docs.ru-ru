---
title: Разработка определяемых пользователем операторов U-SQL — Azure Data Lake Analytics
description: Узнайте, как разрабатывать определяемые пользователем операторы (с возможностью повторного использования) в заданиях Azure Data Lake Analytics.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: e5189e4e-9438-46d1-8686-ed4836bf3356
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: b2d1293b06b4d8791138ed666bc3cb4abe3adf40
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "71316541"
---
# <a name="develop-u-sql-user-defined-operators-udos"></a>Разработка определяемых пользователем операторов U-SQLU (UDO)
В этой статье описано, как разрабатывать определяемые пользователем операторы для обработки данных в задании U-SQL.

## <a name="define-and-use-a-user-defined-operator-in-u-sql"></a>Определение и использование определяемых пользователем операторов в U-SQL
**Создание и отправка задания U-SQL**

1. В Visual Studio выберите **Файл > Создать > Проект > Проект U-SQL**.
2. Нажмите кнопку **ОК**. Visual Studio создаст решение с помощью файла Script.usql.
3. В **обозревателе решений** разверните узел Script.usql и дважды щелкните файл **Script.usql.cs**.
4. Скопируйте приведенный ниже код и вставьте его в файл.

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;

        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Suisse", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };

                public override IRow Process(IRow input, IUpdatableRow output)
                {

                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");

                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);

                    return output.AsReadOnly();
                }
            }
        }
6. Откройте файл **Script.usql** и вставьте следующий сценарий U-SQL:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);

        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    

        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);
7. Укажите учетную запись аналитики озера данных, базу данных и схему.
8. В **обозревателе решений** щелкните правой кнопкой мыши файл **Script.usql** и выберите команду **Создать сценарий**.
9. В **обозревателе решений** щелкните правой кнопкой мыши файл **Script.usql** и выберите команду **Отправить сценарий**.
10. Если вы еще не подключились к своей подписке Azure, вам будет предложено ввести учетные данные Azure.
11. Щелкните **Отправить**. Итоги отправки и ссылка на задание появятся в окне результатов, когда операция отправки будет завершена.
12. Чтобы увидеть последнее состояние задания, нажмите кнопку **Обновить**.

**Просмотр выходных данных**

1. В **обозревателе сервера** разверните узлы **Azure**, **Data Lake Analytics**, а также учетную запись Data Lake Analytics. Затем разверните узел **Учетные записи хранения**, щелкните хранилище по умолчанию правой кнопкой мыши и выберите **Обозреватель**.
2. Разверните узлы «Примеры» и «Выходные данные», а затем дважды щелкните **Drivers.csv**.

## <a name="see-also"></a>См. также
* [Extending U-SQL Expressions with User-Code](/u-sql/concepts/extending-u-sql-expressions-with-user-code) (Расширение выражений U-SQL с помощью пользовательского кода)
* [Использование инструментов озера данных для Visual Studio для разработки приложений U-SQL](data-lake-analytics-data-lake-tools-get-started.md)
