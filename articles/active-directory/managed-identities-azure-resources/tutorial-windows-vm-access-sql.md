---
title: Учебник. Получение доступа к Azure SQL для Windows в Azure AD с помощью управляемого удостоверения
description: Из этого руководства вы узнаете, как получить доступ к SQL Azure с помощью назначаемого системой управляемого удостоверения виртуальной машины Windows.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/14/2020
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2fc5596c6914b77b09db10528af891d7e6bd0159
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/15/2020
ms.locfileid: "75977865"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-sql"></a>Руководство. Использование назначаемого системой управляемого удостоверения на виртуальной машине Windows для доступа к SQL Azure

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

В этом руководстве описывается, как получить доступ к Azure SQL Server с помощью назначаемого системой удостоверения виртуальной машины Windows. Управляемыми удостоверениями службы автоматически управляет Azure. Они позволяют проходить проверку подлинности в службах, поддерживающих аутентификацию Azure AD, без указания учетных данных в коде. Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Предоставление виртуальной машине доступа к серверу SQL Azure.
> * Включение аутентификации Azure AD для сервера SQL
> * Создание в базе данных автономного пользователя, который представляет назначаемое системой удостоверение виртуальной машины
> * Получение маркера доступа с использованием идентификатора виртуальной машины и отправка запроса на сервер SQL Azure с его помощью.

## <a name="prerequisites"></a>предварительные требования

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]


## <a name="enable"></a>Включить

[!INCLUDE [msi-tut-enable](../../../includes/active-directory-msi-tut-enable.md)]


## <a name="grant-access"></a>Предоставление доступа

Чтобы предоставить виртуальной машине доступ к базе данных на сервере SQL Azure, можно использовать существующий сервер SQL или создать новый. Чтобы создать сервер и базу данных с помощью портала Azure, следуйте указаниям в статье [Создание базы данных SQL Azure на портале Azure](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal). В [документации по SQL Azure](https://docs.microsoft.com/azure/sql-database/) также есть краткие руководства, описывающие использование Azure CLI и Azure PowerShell.

Предоставление виртуальной машине доступа к базе данных выполняется в два этапа:

1. Включить аутентификацию Azure AD для сервера SQL.
2. Создание в базе данных **автономного пользователя**, который представляет назначаемое системой удостоверение виртуальной машины.

### <a name="enable-azure-ad-authentication"></a>Включение аутентификации Azure AD

**Чтобы[ настроить аутентификацию с использованием Azure AD для сервера SQL, сделайте следующее](/azure/sql-database/sql-database-aad-authentication-configure):**

1.  На портале Azure выберите **SQL servers** (Серверы SQL Server) на левой панели навигации.
2.  Щелкните сервер SQL, для которого нужно включить аутентификацию Azure AD.
3.  В разделе колонки **Параметры** щелкните **Администратор Active Directory**.
4.  В командной строке щелкните **Задать администратора**.
5.  Выберите учетную запись пользователя Azure AD, которую необходимо сделать администратором сервера, а затем нажмите кнопку **Выбрать**.
6.  На панели команд нажмите кнопку **Сохранить**.

### <a name="create-contained-user"></a>Создание автономного пользователя

В этом разделе показано, как в базе данных создать автономного пользователя, который представляет назначаемое системой удостоверение виртуальной машины. На этом шаге вам потребуется [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). Прежде чем начать, советуем ознакомиться со следующими статьями, содержащими общие сведения об интеграции Azure AD:

- [Универсальная проверка подлинности для Базы данных SQL и хранилища данных SQL (поддержка SSMS для MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication)
- [Настройка аутентификации Azure Active Directory и управление ею с использованием базы данных SQL или хранилища данных SQL](/azure/sql-database/sql-database-aad-authentication-configure)

Для базы данных SQL в AAD требуются уникальные отображаемые имена. Поэтому учетные записи AAD, например пользователей, групп и субъектов-служб (приложения), и имена виртуальных машин, которые включены для управляемого удостоверения, должны быть уникальными в AAD в соответствии с их отображаемым именами. База данных SQL проверяет отображаемое имя AAD при создании таких пользователей на языке T-SQL. Если имя не является уникальным, команда завершается ошибкой и отображается запрос на ввод уникального отображаемого имени AAD для этой учетной записи.

**Чтобы создать автономного пользователя, сделайте следующее:**

1. Запустите среду SQL Server Management Studio.
2. В диалоговом окне **Подключение к серверу** в поле **Имя сервера** введите имя сервера SQL.
3. В поле **Authentication** (Аутентификация) выберите **Active Directory — универсальная с поддержкой MFA**.
4. В поле **Имя пользователя** введите имя учетной записи Azure AD, установленной в качестве администратора сервера, например helen@woodgroveonline.com.
5. Щелкните **Параметры**.
6. В поле **Подключение к базе данных** введите имя несистемной базы данных, которую требуется настроить.
7. Нажмите кнопку **Соединить**. Завершите процесс входа в систему.
8. В **обозревателе объектов** разверните папку **Базы данных**.
9. Щелкните правой кнопкой мыши пользовательскую базу данных и выберите **Создать запрос**.
10. В окне запроса введите следующую строку и на панели инструментов нажмите кнопку **Выполнить**.

    > [!NOTE]
    > В приведенной ниже команде `VMName` — это имя виртуальной машины, для которой вы ранее настроили назначаемое системой удостоверение.
    
    ```
    CREATE USER [VMName] FROM EXTERNAL PROVIDER
    ```
    
    В результате выполнения команды должен быть создан автономный пользователь для назначаемого системой удостоверения виртуальной машины.
11. Очистите окно запроса, введите следующую строку и на панели инструментов нажмите кнопку **Выполнить**.

    > [!NOTE]
    > В приведенной ниже команде `VMName` — это имя виртуальной машины, для которой вы ранее настроили назначаемое системой удостоверение.
    
    ```
    ALTER ROLE db_datareader ADD MEMBER [VMName]
    ```

    В результате выполнения команды автономному пользователю должна быть предоставлена возможность считывания всей базы данных.

Теперь код, запущенный на виртуальной машине, может с помощью назначаемого системой управляемого удостоверения получить маркер безопасности и использовать его для аутентификации на экземпляре SQL Server.

## <a name="access-data"></a>Доступ к данным

В этом разделе показано, как получить маркер доступа с помощью назначаемого системой управляемого удостоверения виртуальной машины и использовать этот маркер для обращения к SQL Azure. В SQL Azure реализована поддержка аутентификации Azure AD, поэтому поддерживается непосредственный прием маркеров доступа, полученных с использованием управляемого удостоверения. Для создания подключения к SQL используется метод на основе **маркера доступа**. Он реализуется как часть интеграции SQL Azure с Azure AD и отличается от указания учетных данных в строке подключения.

Ниже приведен пример кода .NET для установки подключения к SQL с помощью маркера доступа. Этот код должен выполняться на виртуальной машине, чтобы получить доступ к конечной точке назначаемого системой управляемого удостоверения виртуальной машины. Для использования метода с маркером доступа требуется **.NET Framework 4.6** или более поздней версии либо **.NET Core 2.2** или более поздней версии. Замените значения AZURE-SQL-SERVERNAME и DATABASE соответствующим образом. Обратите внимание на то, что идентификатор ресурса для SQL Azure — `https://database.windows.net/`.

```csharp
using System.Net;
using System.IO;
using System.Data.SqlClient;
using System.Web.Script.Serialization;

//
// Get an access token for SQL.
//
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://database.windows.net/");
request.Headers["Metadata"] = "true";
request.Method = "GET";
string accessToken = null;

try
{
    // Call managed identities for Azure resources endpoint.
    HttpWebResponse response = (HttpWebResponse)request.GetResponse();

    // Pipe response Stream to a StreamReader and extract access token.
    StreamReader streamResponse = new StreamReader(response.GetResponseStream());
    string stringResponse = streamResponse.ReadToEnd();
    JavaScriptSerializer j = new JavaScriptSerializer();
    Dictionary<string, string> list = (Dictionary<string, string>) j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
    accessToken = list["access_token"];
}
catch (Exception e)
{
    string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
}

//
// Open a connection to the SQL server using the access token.
//
if (accessToken != null) {
    string connectionString = "Data Source=<AZURE-SQL-SERVERNAME>; Initial Catalog=<DATABASE>;";
    SqlConnection conn = new SqlConnection(connectionString);
    conn.AccessToken = accessToken;
    conn.Open();
}
```

Кроме того, вы можете быстро протестировать сквозную настройку с помощью PowerShell без необходимости писать и развертывать приложение на виртуальной машине.

1.  На портале перейдите к разделу **Виртуальные машины**, выберите свою виртуальную машину Windows и в разделе **Обзор** щелкните **Подключить**.
2.  Введите **имя пользователя** и **пароль**, добавленные при создании виртуальной машины Windows.
3.  Теперь, когда создано **подключение к удаленному рабочему столу** с виртуальной машиной, откройте **PowerShell** в удаленном сеансе.
4.  С помощью команды PowerShell `Invoke-WebRequest` выполните запрос к локальной конечной точке управляемого удостоверения, чтобы получить маркер доступа к SQL Azure.

    ```powershell
        $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fdatabase.windows.net%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    Преобразуйте ответ из объекта JSON в объект PowerShell. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```

    Извлеките маркер доступа из ответа.
    
    ```powershell
    $AccessToken = $content.access_token
    ```

5. Откройте подключение к серверу SQL. Не забудьте заменить значения AZURE-SQL-SERVERNAME и DATABASE.
    
    ```powershell
    $SqlConnection = New-Object System.Data.SqlClient.SqlConnection
    $SqlConnection.ConnectionString = "Data Source = <AZURE-SQL-SERVERNAME>; Initial Catalog = <DATABASE>"
    $SqlConnection.AccessToken = $AccessToken
    $SqlConnection.Open()
    ```

    Затем создайте и отправьте запрос на сервер. Не забудьте заменить значение для TABLE.

    ```powershell
    $SqlCmd = New-Object System.Data.SqlClient.SqlCommand
    $SqlCmd.CommandText = "SELECT * from <TABLE>;"
    $SqlCmd.Connection = $SqlConnection
    $SqlAdapter = New-Object System.Data.SqlClient.SqlDataAdapter
    $SqlAdapter.SelectCommand = $SqlCmd
    $DataSet = New-Object System.Data.DataSet
    $SqlAdapter.Fill($DataSet)
    ```

Проверьте значение `$DataSet.Tables[0]`, чтобы просмотреть результаты запроса.


## <a name="disable"></a>Отключить

[!INCLUDE [msi-tut-disable](../../../includes/active-directory-msi-tut-disable.md)]


## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как создать назначаемое системой управляемое удостоверение и получить доступ к экземпляру Azure SQL Server. Дополнительные сведения о сервере Azure SQL Server см. здесь:

> [!div class="nextstepaction"]
> [Служба "База данных SQL Azure"](/azure/sql-database/sql-database-technical-overview)
