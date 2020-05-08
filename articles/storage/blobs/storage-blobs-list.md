---
title: Вывод списка больших двоичных объектов с помощью .NET в службе хранилища Azure
description: Узнайте, как получить список больших двоичных объектов в контейнере в учетной записи хранения Azure с помощью клиентской библиотеки .NET. В примерах кода показано, как составить список больших двоичных объектов в неструктурированном списке или как составить список больших двоичных объектов, как если бы они были организованы в каталоги или папки.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/30/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 76142838d1ec138b75fb6c594414b2ff5d8cd939
ms.sourcegitcommit: d815163a1359f0df6ebfbfe985566d4951e38135
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2020
ms.locfileid: "82883300"
---
# <a name="list-blobs-with-net"></a>Вывод списка больших двоичных объектов с помощью .NET

При перечислении больших двоичных объектов из кода можно указать ряд параметров для управления возвратом результатов из службы хранилища Azure. Можно указать число возвращаемых результатов в каждом наборе результатов, а затем извлечь последующие наборы. Можно указать префикс для возврата больших двоичных объектов, имена которых начинаются с этого символа или строки. Кроме того, можно составить список больших двоичных объектов в структуре плоского списка или иерархически. Иерархический список возвращает большие двоичные объекты, как будто они организованы в папки. 

В этой статье показано, как получить список больших двоичных объектов с помощью [клиентской библиотеки службы хранилища Azure для .NET](/dotnet/api/overview/azure/storage?view=azure-dotnet).  

## <a name="understand-blob-listing-options"></a>Общие сведения о параметрах перечисления BLOB-объектов

Чтобы получить список больших двоичных объектов в учетной записи хранения, вызовите один из следующих методов:

- [CloudBlobClient. ListBlobs](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listblobs)
- [CloudBlobClient. ListBlobsSegmented](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listblobssegmented)
- [CloudBlobClient. ListBlobsSegmentedAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listblobssegmentedasync)

Чтобы получить список больших двоичных объектов в контейнере, вызовите один из следующих методов:

- [CloudBlobContainer. ListBlobs](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobs)
- [CloudBlobContainer. ListBlobsSegmented](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobssegmented)
- [CloudBlobContainer. ListBlobsSegmentedAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobssegmentedasync)

Перегрузки для этих методов предоставляют дополнительные параметры для управления возвращением больших двоичных объектов операцией перечисления. Эти параметры описаны в следующих разделах.

### <a name="manage-how-many-results-are-returned"></a>Управление количеством возвращаемых результатов

По умолчанию операция перечисления возвращает до 5000 результатов за раз. Чтобы вернуть меньший набор результатов, укажите ненулевое значение `maxresults` параметра при вызове одного из методов **ListBlobs** .

Если операция перечисления возвращает более 5000 больших двоичных объектов или указано значение `maxresults` , чтобы операция перечисления возвращала подмножество контейнеров в учетной записи хранения, служба хранилища Azure возвращает *маркер продолжения* со списком больших двоичных объектов. Маркер продолжения — это непрозрачное значение, которое можно использовать для получения следующего набора результатов из службы хранилища Azure.

В коде проверьте значение маркера продолжения, чтобы определить, имеет ли он значение null. Если маркер продолжения имеет значение null, набор результатов будет завершен. Если токен продолжения не равен null, вызовите операцию List, передав маркер продолжения для получения следующего набора результатов, пока маркер продолжения не будет иметь значение null.

### <a name="filter-results-with-a-prefix"></a>Фильтрация результатов с помощью префикса

Чтобы отфильтровать список контейнеров, укажите строку для `prefix` параметра. Строка префикса может содержать один или несколько символов. Служба хранилища Azure возвращает только большие двоичные объекты, имена которых начинаются с этого префикса.

### <a name="return-metadata"></a>Возврат метаданных

Чтобы получить метаданные большого двоичного объекта с результатами, укажите значение **метаданных** для перечисления [блоблистингдетаилс](/dotnet/api/microsoft.azure.storage.blob.bloblistingdetails) . Служба хранилища Azure включает метаданные для каждого возвращенного большого двоичного объекта, поэтому вам не нужно вызывать один из методов **FetchAttributes** в этом контексте для получения метаданных большого двоичного объекта.

### <a name="flat-listing-versus-hierarchical-listing"></a>Плоский список и иерархический список

Большие двоичные объекты в службе хранилища Azure организованы с помощью плоской парадигмы, а не для иерархической парадигмы (например, классической файловой системы). Однако можно организовать большие двоичные объекты в *виртуальные каталоги* , чтобы имитировать структуру папок. Виртуальный каталог образует часть имени большого двоичного объекта и обозначается символом-разделителем.

Чтобы организовать большие двоичные объекты в виртуальные каталоги, используйте символ-разделитель в имени большого двоичного объекта. Символом-разделителем по умолчанию является косая черта (/), но в качестве разделителя можно указать любой символ.

Если вы назначите псевдонимы BLOB-объектов с помощью разделителя, то можете выбрать иерархический список больших двоичных объектов. Для выполнения иерархического перечисления служба хранилища Azure возвращает все виртуальные каталоги и большие двоичные объекты под родительским объектом. Операцию перечисления можно вызвать рекурсивно для прохода по иерархии, подобно тому, как будет осуществляться программный обход классической файловой системы.

## <a name="use-a-flat-listing"></a>Использование неструктурированного списка

По умолчанию операция перечисления возвращает большие двоичные объекты в неструктурированном списке. В неструктурированном списке большие двоичные объекты не упорядочиваются по виртуальному каталогу.

В следующем примере выводится список больших двоичных объектов в указанном контейнере с помощью плоского списка с указанным необязательным размером сегмента и записывается имя большого двоичного объекта в окно консоли.

```csharp
private static async Task ListBlobsFlatListingAsync(CloudBlobContainer container, int? segmentSize)
{
    BlobContinuationToken continuationToken = null;
    CloudBlob blob;

    try
    {
        // Call the listing operation and enumerate the result segment.
        // When the continuation token is null, the last segment has been returned
        // and execution can exit the loop.
        do
        {
            BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(string.Empty,
                true, BlobListingDetails.Metadata, segmentSize, continuationToken, null, null);

            foreach (var blobItem in resultSegment.Results)
            {
                // A flat listing operation returns only blobs, not virtual directories.
                blob = (CloudBlob)blobItem;

                // Write out some blob properties.
                Console.WriteLine("Blob name: {0}", blob.Name);
            }

            Console.WriteLine();

           // Get the continuation token and loop until it is null.
           continuationToken = resultSegment.ContinuationToken;

        } while (continuationToken != null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

Пример выходных данных выглядит примерно так:

```
Blob name: FolderA/blob1.txt
Blob name: FolderA/blob2.txt
Blob name: FolderA/blob3.txt
Blob name: FolderA/FolderB/blob1.txt
Blob name: FolderA/FolderB/blob2.txt
Blob name: FolderA/FolderB/blob3.txt
Blob name: FolderA/FolderB/FolderC/blob1.txt
Blob name: FolderA/FolderB/FolderC/blob2.txt
Blob name: FolderA/FolderB/FolderC/blob3.txt
```

## <a name="use-a-hierarchical-listing"></a>Использование иерархического списка

При иерархическом вызове операции перечисления служба хранилища Azure Возвращает виртуальные каталоги и большие двоичные объекты на первом уровне иерархии. Свойство [prefix](/dotnet/api/microsoft.azure.storage.blob.cloudblobdirectory.prefix) каждого виртуального каталога задается таким образом, чтобы можно было передать префикс в рекурсивном вызове для получения следующего каталога.

Чтобы составить список больших двоичных объектов `useFlatBlobListing` в иерархическом порядке, задайте для параметра метода List значение **false**.

В следующем примере перечисляются большие двоичные объекты в указанном контейнере с помощью плоского списка с указанным необязательным размером сегмента и записывает имя большого двоичного объекта в окно консоли.

```csharp
private static async Task ListBlobsHierarchicalListingAsync(CloudBlobContainer container, string prefix)
{
    CloudBlobDirectory dir;
    CloudBlob blob;
    BlobContinuationToken continuationToken = null;

    try
    {
        // Call the listing operation and enumerate the result segment.
        // When the continuation token is null, the last segment has been returned and
        // execution can exit the loop.
        do
        {
            BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(prefix,
                false, BlobListingDetails.Metadata, null, continuationToken, null, null);
            foreach (var blobItem in resultSegment.Results)
            {
                // A hierarchical listing may return both virtual directories and blobs.
                if (blobItem is CloudBlobDirectory)
                {
                    dir = (CloudBlobDirectory)blobItem;

                    // Write out the prefix of the virtual directory.
                    Console.WriteLine("Virtual directory prefix: {0}", dir.Prefix);

                    // Call recursively with the prefix to traverse the virtual directory.
                    await ListBlobsHierarchicalListingAsync(container, dir.Prefix);
                }
                else
                {
                    // Write out the name of the blob.
                    blob = (CloudBlob)blobItem;

                    Console.WriteLine("Blob name: {0}", blob.Name);
                }
                Console.WriteLine();
            }

            // Get the continuation token and loop until it is null.
            continuationToken = resultSegment.ContinuationToken;

        } while (continuationToken != null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

Пример выходных данных выглядит примерно так:

```
Virtual directory prefix: FolderA/
Blob name: FolderA/blob1.txt
Blob name: FolderA/blob2.txt
Blob name: FolderA/blob3.txt

Virtual directory prefix: FolderA/FolderB/
Blob name: FolderA/FolderB/blob1.txt
Blob name: FolderA/FolderB/blob2.txt
Blob name: FolderA/FolderB/blob3.txt

Virtual directory prefix: FolderA/FolderB/FolderC/
Blob name: FolderA/FolderB/FolderC/blob1.txt
Blob name: FolderA/FolderB/FolderC/blob2.txt
Blob name: FolderA/FolderB/FolderC/blob3.txt
```

> [!NOTE]
> Моментальные снимки BLOB-объектов не могут быть перечислены в иерархической операции перечисления.

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="next-steps"></a>Дальнейшие действия

- [Вывод списка больших двоичных объектов](/rest/api/storageservices/list-blobs)
- [Перечисление ресурсов BLOB-объектов](/rest/api/storageservices/enumerating-blob-resources)
