---
title: Подключение к Базе данных Azure для MySQL с помощью Java
description: В этом кратком руководстве представлен пример кода Java, который можно использовать для подключения к базе данных Azure для MySQL и запроса данных из нее.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom: mvc, devcenter, seo-java-july2019, seo-java-august2019
ms.topic: quickstart
ms.devlang: java
ms.date: 3/18/2020
ms.openlocfilehash: b5f1cbf2f822f350b1eeba032199676651364d84
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "80983829"
---
# <a name="quickstart-use-java-to-connect-to-and-query-data-in-azure-database-for-mysql"></a>Краткое руководство. Подключение к Базе данных Azure для MySQL и запрос данных с помощью Java

В этом кратком руководстве вы подключитесь к Базе данных Azure для MySQL с помощью приложения Java и драйвера JDBC MariaDB Connector/J. Также вы узнаете, как использовать инструкции SQL для запроса, вставки, обновления и удаления данных в базе данных на платформах Windows, Mac и Ubuntu Linux. 

В этой статье предполагается, что у вас уже есть опыт разработки на языке Java, но вы только начали работу со службой "База данных Azure для MySQL".

## <a name="prerequisites"></a>Предварительные требования

- Учетная запись Azure с активной подпиской. [Создайте учетную запись](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) бесплатно.
- Сервер Базы данных Azure для MySQL. Создайте сервер Базы данных Azure для MySQL с помощью [портала Azure](quickstart-create-mysql-server-database-using-azure-portal.md) или [Azure CLI](quickstart-create-mysql-server-database-using-azure-cli.md).
- Безопасность подключения базы данных Azure для MySQL настроена с открытым брандмауэром и параметры SSL-подключения настроены для вашего приложения.

## <a name="obtain-the-mariadb-connector"></a>Получение соединителя MariaDB

Получите соединитель [MariaDB Connector/J](https://mariadb.com/kb/en/library/mariadb-connector-j/), выполнив одно из следующих действий:
   - С помощью пакета Maven [mariadb-java-client](https://search.maven.org/search?q=a:mariadb-java-client) включите [зависимость mariadb-java-client](https://mvnrepository.com/artifact/org.mariadb.jdbc/mariadb-java-client) в файл POM для проекта.
   - Скачайте драйвер JDBC [MariaDB Connector/J](https://downloads.mariadb.org/connector-java/) и включите JAR-файл JDBC (например, mariadb-java-client-2.4.3.jar) в путь к классу приложения. Изучите в документации по своей среде информацию о путях классов (например, [Apache Tomcat](https://tomcat.apache.org/tomcat-7.0-doc/class-loader-howto.html) или [Java SE](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/classpath.html))

## <a name="get-connection-information"></a>Получение сведений о подключении

Получите сведения о подключении, необходимые для подключения к базе данных Azure.для MySQL. Вам потребуется полное имя сервера и учетные данные для входа.

1. Войдите на [портал Azure](https://portal.azure.com/).
2. В меню слева на портале Azure выберите **Все ресурсы** и выполните поиск по имени созданного сервера (например, **mydemoserver**).
3. Выберите имя сервера.
4. Запишите **имя сервера** и **имя для входа администратора сервера** с панели сервера **Обзор**. Если вы забыли свой пароль, можно также сбросить пароль с помощью этой панели.
 ![Имя сервера базы данных Azure для MySQL](./media/connect-java/azure-database-mysql-server-name.png)

## <a name="connect-create-table-and-insert-data"></a>Подключение, создание таблицы и вставка данных

Используйте указанный ниже код для подключения и загрузки данных с помощью функции с инструкцией SQL **INSERT**. Метод [getConnection()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#using-drivermanager) используется для подключения к MySQL. Методы [createStatement()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#creating-a-table-on-a-mariadb-or-mysql-server) и execute() используются для удаления и создания таблицы. Объект prepareStatement используется для создания команд вставки, а методы setString() и setInt() — для привязки значений параметров. Метод executeUpdate() запускает команду для каждого набора параметров, чтобы вставить значения. 

Замените значения параметров host, database, user и password значениями, указанными при создании сервера и базы данных.

```java
import java.sql.*;
import java.util.Properties;

public class CreateTableInsertRows {

    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables. 
        String host = "mydemoserver.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@mydemoserver";
        String password = "<server_admin_password>";

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mariadb://%s/%s", host, database);

            // Set connection properties.
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Drop previous table of same name if one exists.
                Statement statement = connection.createStatement();
                statement.execute("DROP TABLE IF EXISTS inventory;");
                System.out.println("Finished dropping table (if existed).");
    
                // Create table.
                statement.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);");
                System.out.println("Created table.");
                
                // Insert some data into table.
                int nRowsInserted = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("INSERT INTO inventory (name, quantity) VALUES (?, ?);");
                preparedStatement.setString(1, "banana");
                preparedStatement.setInt(2, 150);
                nRowsInserted += preparedStatement.executeUpdate();

                preparedStatement.setString(1, "orange");
                preparedStatement.setInt(2, 154);
                nRowsInserted += preparedStatement.executeUpdate();

                preparedStatement.setString(1, "apple");
                preparedStatement.setInt(2, 100);
                nRowsInserted += preparedStatement.executeUpdate();
                System.out.println(String.format("Inserted %d row(s) of data.", nRowsInserted));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
    
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}

```

## <a name="read-data"></a>Чтение данных

Используйте указанный ниже код с инструкцией SQL **SELECT** для чтения данных. Метод [getConnection()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#using-drivermanager) используется для подключения к MySQL. Методы [createStatement()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#creating-a-table-on-a-mariadb-or-mysql-server) и executeQuery() используются для подключения и выполнения инструкции SELECT. Результаты обрабатываются с помощью объекта ResultSet. 

Замените значения параметров host, database, user и password значениями, указанными при создании сервера и базы данных.

```java
import java.sql.*;
import java.util.Properties;

public class ReadTable {

    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables.
        String host = "mydemoserver.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@mydemoserver";
        String password = "<server_admin_password>";

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mariadb://%s/%s", host, database);

            // Set connection properties.
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");
            
            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
    
                Statement statement = connection.createStatement();
                ResultSet results = statement.executeQuery("SELECT * from inventory;");
                while (results.next())
                {
                    String outputString = 
                        String.format(
                            "Data row = (%s, %s, %s)",
                            results.getString(1),
                            results.getString(2),
                            results.getString(3));
                    System.out.println(outputString);
                }
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database."); 
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="update-data"></a>Обновление данных

Используйте указанный ниже код с инструкцией SQL **UPDATE** для изменения данных. Метод [getConnection()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#using-drivermanager) используется для подключения к MySQL. Методы [prepareStatement()](https://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) и executeUpdate() используются для подготовки и выполнения инструкции UPDATE. 

Замените значения параметров host, database, user и password значениями, указанными при создании сервера и базы данных.

```java
import java.sql.*;
import java.util.Properties;

public class UpdateTable {
    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables. 
        String host = "mydemoserver.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@mydemoserver";
        String password = "<server_admin_password>";

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mariadb://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Modify some data in table.
                int nRowsUpdated = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?;");
                preparedStatement.setInt(1, 200);
                preparedStatement.setString(2, "banana");
                nRowsUpdated += preparedStatement.executeUpdate();
                System.out.println(String.format("Updated %d row(s) of data.", nRowsUpdated));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="delete-data"></a>Удаление данных

Используйте указанный ниже код с инструкцией SQL **DELETE** для удаления данных. Метод [getConnection()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#using-drivermanager) используется для подключения к MySQL.  Методы [prepareStatement()](https://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) и executeUpdate() используются для подготовки и выполнения инструкции DELETE. 

Замените значения параметров host, database, user и password значениями, указанными при создании сервера и базы данных.

```java
import java.sql.*;
import java.util.Properties;

public class DeleteTable {
    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables.
        String host = "mydemoserver.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@mydemoserver";
        String password = "<server_admin_password>";

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mariadb://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");
            
            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Delete some data from table.
                int nRowsDeleted = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("DELETE FROM inventory WHERE name = ?;");
                preparedStatement.setString(1, "orange");
                nRowsDeleted += preparedStatement.executeUpdate();
                System.out.println(String.format("Deleted %d row(s) of data.", nRowsDeleted));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Перенос базы данных MySQL в базу данных Azure для MySQL с помощью резервного копирования и восстановления](concepts-migrate-dump-restore.md)
