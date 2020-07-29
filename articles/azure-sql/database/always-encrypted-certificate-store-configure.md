---
title: Настройка Always Encrypted с помощью хранилища сертификатов Windows
description: В этой статье показано, как защитить конфиденциальные данные в базе данных SQL Azure с помощью шифрования базы данных, используя мастер Always Encrypted в SQL Server Management Studio (SSMS). В ней также показано, как сохранить ключи в хранилище сертификатов Windows.
keywords: шифрование данных, шифрование SQL, шифрование базы данных, конфиденциальные данные, постоянное шифрование
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviwer: ''
ms.date: 04/23/2020
ms.openlocfilehash: 848a0c9817472086dbaf3973dad9c64e3ed74b10
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2020
ms.locfileid: "85954253"
---
# <a name="configure-always-encrypted-by-using-the-windows-certificate-store"></a>Настройка Always Encrypted с помощью хранилища сертификатов Windows

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

В этой статье показано, как защитить конфиденциальные данные в базе данных SQL Azure или SQL Azure Управляемый экземпляр с шифрованием базы данных с помощью [мастера Always encrypted](/sql/relational-databases/security/encryption/always-encrypted-wizard) в [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). В ней также показано, как сохранить ключи в хранилище сертификатов Windows.

Always Encrypted — это технология шифрования данных, которая помогает защитить конфиденциальные данные, находящиеся на сервере, во время перемещения между клиентом и сервером, а также во время использования данных, гарантируя, что конфиденциальные данные никогда не появятся в системе базы данных как открытый текст. После шифрования данных получить доступ к данным в виде открытого текста могут только клиентские приложения и серверы приложений, у которых есть доступ к ключам. Дополнительные сведения см. в статье [Always Encrypted (ядро СУБД)](https://msdn.microsoft.com/library/mt163865.aspx).

После настройки функции Always Encrypted для базы данных вы создадите клиентское приложение на языке C# для работы с зашифрованными данными, используя Visual Studio.

Выполните действия, описанные в этой статье, чтобы узнать, как настроить Always Encrypted для базы данных SQL или Управляемый экземпляр SQL. Здесь вы научитесь:

* Используйте мастер Always Encrypted в среде SSMS, чтобы создать [ключи Always encrypted](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
  * Создание [главного ключа столбца (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
  * Создайте [ключ шифрования столбцов (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
* Создавать таблицу базы данных и шифровать столбцы.
* Создавать приложение, которое вставляет, выбирает и отображает данные зашифрованных столбцов.

## <a name="prerequisites"></a>Предварительные условия

Для работы с этим руководством вам потребуется:

* Учетная запись и подписка Azure. Если у вас ее нет, зарегистрируйтесь для получения [бесплатной пробной версии](https://azure.microsoft.com/pricing/free-trial/).
- База данных в [базе данных SQL Azure](single-database-create-quickstart.md) или [Azure SQL управляемый экземпляр](../managed-instance/instance-create-quickstart.md).
* [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) версии 13.0.700.242 или более поздней версии.
* [NET Framework версии 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) или более поздней версии (на клиентском компьютере).
* [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).

## <a name="enable-client-application-access"></a>Разрешить доступ к клиентскому приложению

Необходимо включить клиентское приложение для доступа к базе данных SQL или Управляемый экземпляр SQL, настроив приложение Azure Active Directory (AAD) и скопировав *идентификатор приложения* и *ключ* , которые потребуется проверить подлинность приложения.

Чтобы получить *идентификатор приложения* и *ключ*, ознакомьтесь с процедурой в разделе [Создание приложения Azure Active Directory и субъекта-службы с доступом к ресурсам с помощью портала](../../active-directory/develop/howto-create-service-principal-portal.md).



## <a name="connect-with-ssms"></a>Подключение к SSMS

Откройте SQL Server Management Studio (SSMS) и подключитесь к серверу или под управлением базы данных.

1. Откройте SSMS. (Нажмите кнопку **подключить**  >  . **Ядро СУБД** , чтобы открыть окно **Подключение к серверу** , если оно не открыто.
2. Введите имя сервера и учетные данные.

    ![Копирование строки подключения](./media/always-encrypted-certificate-store-configure/ssms-connect.png)

Если откроется окно **Новое правило брандмауэра** , войдите в Azure, и SSMS создаст новое правило брандмауэра для вас.

## <a name="create-a-table"></a>Создание таблицы

В этом разделе вы создадите таблицу для хранения данных пациентов. Изначально это будет обычная таблица. Шифрование вы настроите в следующем разделе.

1. Разверните узел **Базы данных**.
2. Щелкните правой кнопкой мыши базу данных **Clinic** и выберите пункт **Создать запрос**.
3. Вставьте следующий сценарий Transact-SQL в окно нового запроса и нажмите кнопку **Выполнить** .
    
    ```tsql
    CREATE TABLE [dbo].[Patients](
    [PatientId] [int] IDENTITY(1,1),
    [SSN] [char](11) NOT NULL,
    [FirstName] [nvarchar](50) NULL,
    [LastName] [nvarchar](50) NULL,
    [MiddleName] [nvarchar](50) NULL,
    [StreetAddress] [nvarchar](50) NULL,
    [City] [nvarchar](50) NULL,
    [ZipCode] [char](5) NULL,
    [State] [char](2) NULL,
    [BirthDate] [date] NOT NULL
    PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
    GO
    ```

## <a name="encrypt-columns-configure-always-encrypted"></a>Шифрование столбцов (настройка Always Encrypted)

В SSMS есть мастер, с помощью которого можно легко настроить Always Encrypted. Он позволяет автоматически настроить главный ключ столбца (CMK), ключ шифрования столбца (CEK) и зашифрованные столбцы.

1. Разверните узел **базы данных**  >  **Clinic**  >  **таблицы**курса.
2. Щелкните правой кнопкой мыши таблицу **Patients** и выберите пункт **Зашифровать столбцы**, чтобы открыть мастер настройки Always Encrypted.

    ![Шифрование столбцов…](./media/always-encrypted-certificate-store-configure/encrypt-columns.png)

Мастер настройки Always Encrypted содержит следующие разделы: **Выбор столбца**, **Настройка главного ключа** (CMK), **Проверка** и **Сводка**.

### <a name="column-selection"></a>Выполните действия на странице Выбор столбцов.

На странице **Введение** нажмите кнопку **Далее**, чтобы открыть страницу **Выбор столбца**. На этой странице можно будет выбрать столбцы для шифрования, [тип шифрования и используемый ключ шифрования столбца (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) .

Для каждого пациента необходимо зашифровать данные в столбцах **SSN** и **BirthDate**. Для столбца **SSN** будет использоваться детерминированное шифрование, которое поддерживает уточняющие запросы на соответствие условию, операции объединения и группировки, а для столбца **BirthDate** — случайное шифрование, которое не поддерживает какие-либо операции.

Задайте для параметра **Тип шифрования** столбца **SSN** значение **Детерминированное**, а для столбца **BirthDate** — **Случайное**. Нажмите кнопку **Далее**.

![Шифрование столбцов…](./media/always-encrypted-certificate-store-configure/column-selection.png)

### <a name="master-key-configuration"></a>Настройка главного ключа

На странице **Настройка главного ключа** можно настроить CMK и выбрать поставщик хранилища ключей, в котором будет находиться CMK. В настоящее время CMK можно хранить в хранилище сертификатов Windows, хранилище ключей Azure или аппаратном модуле безопасности (HSM). В этом руководстве показано, как сохранить ключи в хранилище сертификатов Windows.

Убедитесь, что параметр **Хранилище сертификатов Windows** выбран, и нажмите кнопку **Далее**.

![Настройка главного ключа](./media/always-encrypted-certificate-store-configure/master-key-configuration.png)

### <a name="validation"></a>Проверка

Можно зашифровать столбцы сейчас или сохранить сценарий PowerShell и выполнить его позже. Для целей этого руководства выберите **Перейти к завершению** и нажмите кнопку **Далее**.

### <a name="summary"></a>Сводка

Убедитесь, что все параметры настроены правильно, и нажмите кнопку **Готово** , чтобы завершить настройку Always Encrypted.

![Сводка](./media/always-encrypted-certificate-store-configure/summary.png)

### <a name="verify-the-wizards-actions"></a>Проверка действий мастера

После завершения работы мастера для базы данных будет настроена функция Always Encrypted. Мастер выполнил следующие действия:

* создал CMK;
* создал CEK;
* Настройка шифрования для выбранных столбцов. Сейчас в таблице **Patients** нет данных, но как только они появятся, они будут зашифрованы в выбранных столбцах.

Вы можете проверить создание ключей в SSMS, перейдя **к**  >  **Security**  >  **разделу Безопасность Always encrypted ключи**. Таким образом можно увидеть новые ключи, созданные с помощью мастера.

## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Создание клиентского приложения, которое работает с зашифрованными данными

Теперь, когда функция Always Encrypted настроена, мы можем создать приложение, которое выполняет c зашифрованными столбцами операции *вставки* и *выборки*. Пример приложения необходимо запустить на том же компьютере, что и мастер настройки Always Encrypted. Чтобы выполнить клиентское приложение на другом компьютере, на нем требуется развернуть сертификаты Always Encrypted.  

> [!IMPORTANT]
> При передаче открытого текста на сервер со столбцами с постоянным шифрованием приложение должно использовать объекты [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) . Передача значений литералов без использования объектов SqlParameter приведет к возникновению исключения.

1. Откройте Visual Studio и создайте консольное приложение на языке C#. Убедитесь, что для проекта используется платформа **.NET Framework** версии 4.6 или более поздней.
2. Назовите проект **AlwaysEncryptedConsoleApp** и нажмите кнопку **ОК**.

![Новое консольное приложение](./media/always-encrypted-certificate-store-configure/console-app.png)

## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Изменение строки подключения для активации функции постоянного шифрования

В этом разделе объясняется, как включить функцию Always Encrypted в строке подключения к базе данных. Изменение только что созданного консольного приложения описывается в следующем разделе: "Пример консольного приложения с функцией постоянного шифрования".

Чтобы включить функцию Always Encrypted, необходимо добавить ключевое слово **Column Encryption Setting** в строку подключения и задать для него значение **Enabled**.

Это можно задать непосредственно в строке подключения или с помощью [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). В разделе ниже показано, как использовать **SqlConnectionStringBuilder**для примера приложения.

> [!NOTE]
> Это единственное изменение, которое нужно внести в клиентское приложение для использования функции постоянного шифрования. Если у вас есть приложение, которое хранит строки подключения во внешнем расположении (то есть в файле конфигурации), эту функцию можно включить в таком приложении, не изменяя его код.

### <a name="enable-always-encrypted-in-the-connection-string"></a>Активация функции постоянного шифрования в строке подключения

Добавьте в строку подключения следующее ключевое слово:

`Column Encryption Setting=Enabled`

### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Активация функции постоянного шифрования с помощью SqlConnectionStringBuilder

В коде ниже показано, как включить функцию Always Encrypted, указав параметр [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) со значением [Enabled](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

```csharp
// Instantiate a SqlConnectionStringBuilder.
SqlConnectionStringBuilder connStringBuilder =
    new SqlConnectionStringBuilder("replace with your connection string");

// Enable Always Encrypted.
connStringBuilder.ColumnEncryptionSetting =
    SqlConnectionColumnEncryptionSetting.Enabled;
```

## <a name="always-encrypted-sample-console-application"></a>Пример консольного приложения с функцией постоянного шифрования

В этом примере показано, как:

* Изменить строку подключения для активации функции постоянного шифрования.
* Вставить данные в зашифрованные столбцы.
* Выбрать запись, выполнив фильтрацию по конкретному значению в зашифрованном столбце.

Замените содержимое **Program.CS** следующим кодом. Замените строку подключения для глобальной переменной connectionString в строке прямо над методом Main действительной строкой подключения, указанной на портале Azure. Это единственное изменение, которое необходимо внести в этот код.

Запустите приложение, чтобы увидеть функцию Always Encrypted в действии.

```cs
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;
using System.Globalization;

namespace AlwaysEncryptedConsoleApp
{
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"Data Source = SPE-T640-01.sys-sqlsvr.local; Initial Catalog = Clinic; Integrated Security = true";

        static void Main(string[] args)
        {
            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();

            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            CultureInfo culture = CultureInfo.CreateSpecificCulture("en-US");
            InsertPatient(new Patient()
            {
                SSN = "999-99-0001",
                FirstName = "Orlando",
                LastName = "Gee",
                BirthDate = DateTime.Parse("01/04/1964", culture)
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0002",
                FirstName = "Keith",
                LastName = "Harris",
                BirthDate = DateTime.Parse("06/20/1977", culture)
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0003",
                FirstName = "Donna",
                LastName = "Carreras",
                BirthDate = DateTime.Parse("02/09/1973", culture)
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0004",
                FirstName = "Janet",
                LastName = "Gates",
                BirthDate = DateTime.Parse("08/31/1985", culture)
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0005",
                FirstName = "Lucy",
                LastName = "Harrington",
                BirthDate = DateTime.Parse("05/06/1993", culture)
            });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
     VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
}
```

## <a name="verify-that-the-data-is-encrypted"></a>Как проверить, что данные шифруются

Вы можете быстро проверить, действительно ли шифруются данные на сервере, запросив данные **Patients** с помощью SSMS. (Используйте текущее подключение, для которого параметр шифрования столбцов еще не включен.)

Выполните следующий запрос к базе данных Clinic.

```tsql
SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
```

Вы увидите, что в зашифрованных столбцах не содержатся данные в виде открытого текста.

   ![Новое консольное приложение](./media/always-encrypted-certificate-store-configure/ssms-encrypted.png)

Чтобы получить доступ к данным в виде открытого текста, можно добавить в строку подключения параметр **Column Encryption Setting=enabled** .

1. В **обозревателе объектов** SSMS щелкните сервер правой кнопкой мыши и выберите пункт **Отключить**.
2. Нажмите кнопку **подключить**  >  **ядро СУБД** , чтобы открыть окно **Подключение к серверу** , и выберите пункт **Параметры**.
3. Щелкните **Дополнительные параметры соединения** и введите **Column Encryption Setting=enabled**.

    ![Новое консольное приложение](./media/always-encrypted-certificate-store-configure/ssms-connection-parameter.png)
4. Выполните следующий запрос к базе данных **Clinic** .

    ```tsql
    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
    ```

     Теперь в зашифрованных столбцах отображаются незашифрованные данные.

    ![Новое консольное приложение](./media/always-encrypted-certificate-store-configure/ssms-plaintext.png)

> [!NOTE]
> При подключении с другого компьютера SSMS (или любой другой клиент) не сможет получить доступ к ключам шифрования, а значит, и расшифровать данные.

## <a name="next-steps"></a>Дальнейшие шаги

После создания базы данных с функцией Always Encrypted вы можете сделать следующее:

* Запустить этот пример с другого компьютера. В этом случае у приложения не будет доступа к ключам шифрования, без которых нельзя просмотреть данные в виде открытого текста, и операция завершится сбоем.
* [Сменить и очистить ключи](https://msdn.microsoft.com/library/mt607048.aspx).
* [Перенести данные, которые уже зашифрованы с использованием функции Always Encrypted.](https://msdn.microsoft.com/library/mt621539.aspx)
* [Развернуть сертификаты Always Encrypted на других клиентских компьютерах](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (ознакомьтесь с разделом "Предоставление приложениям и пользователям доступа к сертификатам").

## <a name="related-information"></a>Дополнительные сведения

* [Always Encrypted (Разработка клиентов)](https://msdn.microsoft.com/library/mt147923.aspx)
* [прозрачное шифрование данных.](https://msdn.microsoft.com/library/bb934049.aspx)
* [Шифрование SQL Server](https://msdn.microsoft.com/library/bb510663.aspx)
* [Мастер постоянного шифрования](https://msdn.microsoft.com/library/mt459280.aspx)
* [Блог Always Encrypted](https://docs.microsoft.com/archive/blogs/sqlsecurity/always-encrypted-key-metadata)
