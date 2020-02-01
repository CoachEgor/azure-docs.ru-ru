---
title: Создание кластера и базы данных Azure Data Explorer с помощью C#
description: Сведения о создании кластера и базы данных Azure Data Explorer с использованием C#
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 2d800dc401b0d85b26a71817a1a70d66539203ae
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2020
ms.locfileid: "76902120"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-c"></a>Создание кластера и базы данных Azure Data Explorer с помощью C#

> [!div class="op_single_selector"]
> * [Портал](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
> * [Шаблон Azure Resource Manager](create-cluster-database-resource-manager.md)

Azure Data Explorer — это быстрая и полностью управляемая служба для анализа большого объема потоковых данных в реальном времени из приложений, с веб-сайтов, с устройств Интернета вещей и т. д. Чтобы использовать обозреватель данных Azure, сначала нужно создать кластер и одну или несколько баз данных в этом кластере. Затем вы должны принять (загрузить) данные в базы данных, чтобы к ним можно было выполнять запросы. В этой статье вы создадите кластер и базу данных с помощью C#.

## <a name="prerequisites"></a>Технические условия

* Если вы не установили Visual Studio 2019, вы можете скачать и использовать **бесплатную** [версию Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/). При установке Visual Studio необходимо включить возможность **разработки для Azure**.
* Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись](https://azure.microsoft.com/free/) Azure, прежде чем начинать работу.

## <a name="install-c-nuget"></a>Установка C# NuGet

* Установите [пакет NuGet для Azure Data Explorer (Kusto)](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/).
* Установите [пакет NuGet Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) для проверки подлинности.

## <a name="authentication"></a>Проверка подлинности
Для выполнения примеров в этой статье нам потребуется приложение Azure AD и субъект-служба, которые могут получать доступ к ресурсам. Установите флажок [создать приложение Azure AD](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) , чтобы создать бесплатное приложение Azure AD и назначить роль в области действия подписки. В нем также показано, как получить `Directory (tenant) ID`, `Application ID`и `Client Secret`.

## <a name="create-the-azure-data-explorer-cluster"></a>Создание кластера Azure Data Explorer

1. Создайте кластер, используя приведенный ниже код:

    ```csharp
    var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
    var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
    var clientSecret = "xxxxxxxxxxxxxx";//Client Secret
    var subscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";
    var authenticationContext = new AuthenticationContext($"https://login.windows.net/{tenantId}");
    var credential = new ClientCredential(clientId, clientSecret);
    var result = await authenticationContext.AcquireTokenAsync(resource: "https://management.core.windows.net/", clientCredential: credential);

    var credentials = new TokenCredentials(result.AccessToken, result.AccessTokenType);

    var kustoManagementClient = new KustoManagementClient(credentials)
    {
        SubscriptionId = subscriptionId
    };

    var resourceGroupName = "testrg";
    var clusterName = "mykustocluster";
    var location = "Central US";
    var skuName = "Standard_D13_v2";
    var tier = "Standard";
    var capacity = 5;
    var sku = new AzureSku(skuName, tier, capacity);
    var cluster = new Cluster(location, sku);
    await kustoManagementClient.Clusters.CreateOrUpdateAsync(resourceGroupName, clusterName, cluster);
    ```

   |**Параметр** | **Рекомендуемое значение** | **Описание поля**|
   |---|---|---|
   | clusterName | *mykustocluster* | Необходимое имя кластера.|
   | skuName | *Standard_D13_v2* | Номер SKU, который будет использоваться для кластера. |
   | Уровень | *Standard Edition* | Уровень SKU. |
   | ресурсов | *number* | Число экземпляров кластера. |
   | имя_группы_ресурсов | *testrg* | Имя группы ресурсов, в которой будет создан кластер. |

    > [!NOTE]
    > **Создание кластера** — это длительная операция, поэтому настоятельно рекомендуется использовать CreateOrUpdateAsync вместо CreateOrUpdate. 

1. Чтобы проверить, успешно ли создан кластер, можно выполнить следующую команду:

    ```csharp
    kustoManagementClient.Clusters.Get(resourceGroupName, clusterName);
    ```

Если результат содержит параметр `ProvisioningState` со значением `Succeeded`, это означает, что кластер создан успешно.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Создание базы данных в кластере Azure Data Explorer

1. Создайте базу данных, используя приведенный ниже код:

    ```csharp
    var hotCachePeriod = new TimeSpan(3650, 0, 0, 0);
    var softDeletePeriod = new TimeSpan(3650, 0, 0, 0);
    var databaseName = "mykustodatabase";
    var database = new ReadWriteDatabase(location: location, softDeletePeriod: softDeletePeriod, hotCachePeriod: hotCachePeriod);

    await kustoManagementClient.Databases.CreateOrUpdateAsync(resourceGroupName, clusterName, databaseName, database);
    ```

        [!NOTE]
        If you are using C# version 2.0.0 or below, use Database instead of ReadWriteDatabase.

   |**Параметр** | **Рекомендуемое значение** | **Описание поля**|
   |---|---|---|
   | clusterName | *mykustocluster* | Имя кластера, в котором необходимо создать базу данных.|
   | databaseName | *mykustodatabase* | Имя базы данных.|
   | имя_группы_ресурсов | *testrg* | Имя группы ресурсов, в которой будет создан кластер. |
   | softDeletePeriod | *3650:00:00:00* | Время, на протяжении которого данные будут храниться таким образом, чтобы они были доступны для запроса. |
   | hotCachePeriod | *3650:00:00:00* | Время, на протяжении которого данные будут храниться в кэше. |

2. Выполните следующую команду, чтобы просмотреть созданную базу данных:

    ```csharp
    kustoManagementClient.Databases.Get(resourceGroupName, clusterName, databaseName) as ReadWriteDatabase;
    ```

Теперь у вас есть кластер и база данных.

## <a name="clean-up-resources"></a>Очистка ресурсов

* Если вы планируете следовать нашим другим статьям, не заполняйте созданные ресурсы.
* Чтобы очистить ресурсы, удалите кластер. При удалении кластера также удаляются все содержащиеся в нем базы данных. Для удаления кластера используйте следующую команду:

    ```csharp
    kustoManagementClient.Clusters.Delete(resourceGroupName, clusterName);
    ```

## <a name="next-steps"></a>Дальнейшие действия

* [Краткое руководство. Прием данных с помощью пакета SDK .NET Standard для Azure Data Explorer (предварительная версия)](net-standard-ingest-data.md)
