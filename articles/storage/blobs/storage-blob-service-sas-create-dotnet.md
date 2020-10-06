---
title: Создание SAS службы для контейнера или большого двоичного объекта с помощью .NET
titleSuffix: Azure Storage
description: Узнайте, как создать подписанный URL-адрес службы (SAS) для контейнера или большого двоичного объекта с помощью клиентской библиотеки .NET.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 10/05/2020
ms.author: tamram
ms.reviewer: dineshm
ms.subservice: blobs
ms.custom: devx-track-csharp
ms.openlocfilehash: 6ebb81e6f5adce1dbc301ac905fed5af1ee6a825
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/06/2020
ms.locfileid: "91758311"
---
# <a name="create-a-service-sas-for-a-container-or-blob-with-net"></a>Создание SAS службы для контейнера или большого двоичного объекта с помощью .NET

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

В этой статье показано, как использовать ключ учетной записи хранения для создания SAS службы для контейнера или большого двоичного объекта с [клиентской библиотекой хранилища Azure для .NET](/dotnet/api/overview/azure/storage?view=azure-dotnet).

## <a name="create-a-service-sas-for-a-blob-container"></a>Создание SAS службы для контейнера больших двоичных объектов

В следующем примере кода создается SAS для контейнера. Если указано имя существующей хранимой политики доступа, то эта политика будет связана с SAS. Если хранимая политика доступа не указана, код создает нерегламентированный SAS в контейнере.

### <a name="net-v12"></a>[\..NET (версии 12)](#tab/dotnet)

Подписанный URL-адрес службы подписывается ключом доступа учетной записи. Используйте класс [сторажешаредкэйкредентиал](/dotnet/api/azure.storage.storagesharedkeycredential) для создания учетных данных, которые используются для подписания SAS. Затем создайте новый объект [блобсасбуилдер](/dotnet/api/azure.storage.sas.blobsasbuilder) и вызовите метод [тосаскуерипараметерс](/dotnet/api/azure.storage.sas.blobsasbuilder.tosasqueryparameters) , чтобы получить строку токена SAS.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Security.cs" id="Snippet_GetContainerSasUri":::

### <a name="net-v11"></a>[\..NET (версии 11)](#tab/dotnetv11)

Чтобы создать SAS службы для контейнера, вызовите метод [CloudBlobContainer. GetSharedAccessSignature](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.getsharedaccesssignature) .

```csharp
private static string GetContainerSasUri(CloudBlobContainer container, string storedPolicyName = null)
{
    string sasContainerToken;

    // If no stored policy is specified, create a new access policy and define its constraints.
    if (storedPolicyName == null)
    {
        // Note that the SharedAccessBlobPolicy class is used both to define the parameters of an ad hoc SAS, and
        // to construct a shared access policy that is saved to the container's shared access policies.
        SharedAccessBlobPolicy adHocPolicy = new SharedAccessBlobPolicy()
        {
            // When the start time for the SAS is omitted, the start time is assumed to be the time when the storage service receives the request.
            // Omitting the start time for a SAS that is effective immediately helps to avoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List
        };

        // Generate the shared access signature on the container, setting the constraints directly on the signature.
        sasContainerToken = container.GetSharedAccessSignature(adHocPolicy, null);

        Console.WriteLine("SAS for blob container (ad hoc): {0}", sasContainerToken);
        Console.WriteLine();
    }
    else
    {
        // Generate the shared access signature on the container. In this case, all of the constraints for the
        // shared access signature are specified on the stored access policy, which is provided by name.
        // It is also possible to specify some constraints on an ad hoc SAS and others on the stored access policy.
        sasContainerToken = container.GetSharedAccessSignature(null, storedPolicyName);

        Console.WriteLine("SAS for blob container (stored access policy): {0}", sasContainerToken);
        Console.WriteLine();
    }

    // Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}

```
---

## <a name="create-a-service-sas-for-a-blob"></a>Создание SAS службы для большого двоичного объекта

В следующем примере кода создается SAS для большого двоичного объекта. Если указано имя существующей хранимой политики доступа, то эта политика будет связана с SAS. Если хранимая политика доступа не указана, код создает нерегламентированный SAS в большом двоичном объекте.

### <a name="net-v12"></a>[\..NET (версии 12)](#tab/dotnet)

Подписанный URL-адрес службы подписывается ключом доступа учетной записи. Используйте класс [сторажешаредкэйкредентиал](/dotnet/api/azure.storage.storagesharedkeycredential) для создания учетных данных, которые используются для подписания SAS. Затем создайте новый объект [блобсасбуилдер](/dotnet/api/azure.storage.sas.blobsasbuilder) и вызовите метод [тосаскуерипараметерс](/dotnet/api/azure.storage.sas.blobsasbuilder.tosasqueryparameters) , чтобы получить строку токена SAS.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Security.cs" id="Snippet_GetBlobSasUri":::

### <a name="net-v11"></a>[\..NET (версии 11)](#tab/dotnetv11)

Чтобы создать SAS службы для большого двоичного объекта, вызовите метод [CloudBlob. GetSharedAccessSignature](/dotnet/api/microsoft.azure.storage.blob.cloudblob.getsharedaccesssignature) .

```csharp
private static string GetBlobSasUri(CloudBlobContainer container, string blobName, string policyName = null)
{
    string sasBlobToken;

    // Get a reference to a blob within the container.
    // Note that the blob may not exist yet, but a SAS can still be created for it.
    CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

    if (policyName == null)
    {
        // Create a new access policy and define its constraints.
        // Note that the SharedAccessBlobPolicy class is used both to define the parameters of an ad hoc SAS, and
        // to construct a shared access policy that is saved to the container's shared access policies.
        SharedAccessBlobPolicy adHocSAS = new SharedAccessBlobPolicy()
        {
            // When the start time for the SAS is omitted, the start time is assumed to be the time when the storage service receives the request.
            // Omitting the start time for a SAS that is effective immediately helps to avoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create
        };

        // Generate the shared access signature on the blob, setting the constraints directly on the signature.
        sasBlobToken = blob.GetSharedAccessSignature(adHocSAS);

        Console.WriteLine("SAS for blob (ad hoc): {0}", sasBlobToken);
        Console.WriteLine();
    }
    else
    {
        // Generate the shared access signature on the blob. In this case, all of the constraints for the
        // shared access signature are specified on the container's stored access policy.
        sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        Console.WriteLine("SAS for blob (stored access policy): {0}", sasBlobToken);
        Console.WriteLine();
    }

    // Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

---

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="next-steps"></a>Дальнейшие действия

- [Предоставление ограниченного доступа к ресурсам службы хранилища Azure с помощью подписанных URL-адресов (SAS)](../common/storage-sas-overview.md)
- [Create a service SAS](/rest/api/storageservices/create-service-sas) (Создание SAS на уровне службы)
