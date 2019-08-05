---
title: Загрузка больших объемов случайных данных из службы хранилища Azure | Документация Майкрософт
description: Сведения о скачивании больших объемов случайных данных из учетной записи хранения Azure с использованием пакета SDK Azure
author: roygara
ms.service: storage
ms.topic: tutorial
ms.date: 02/20/2018
ms.author: rogarana
ms.subservice: blobs
ms.openlocfilehash: 8d270485cef9fb6859de056bc364a36c054c0121
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/31/2019
ms.locfileid: "68699012"
---
# <a name="download-large-amounts-of-random-data-from-azure-storage"></a>Скачивание больших объемов случайных данных из службы хранилища Azure

Это руководство представляет собой первую часть цикла. Из этого руководства можно узнать, как скачать большие объемы данных из службы хранилища Azure.

В третьей части цикла вы узнаете, как выполнять такие задачи:

> [!div class="checklist"]
> * Обновление приложения
> * Выполнение приложения
> * Проверка количества подключений.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством необходимо изучить предыдущее руководство по использованию хранилища: [Передача больших объемов случайных данных в параллельном режиме в службу хранилища Azure][previous-tutorial].

## <a name="remote-into-your-virtual-machine"></a>Удаленное подключение к виртуальной машине

 Для создания сеанса удаленного рабочего стола с виртуальной машиной используйте следующую команду на вашем локальном компьютере. Замените IP-адрес значением publicIPAddress виртуальной машины. При появлении запроса введите учетные данные, использованные при создании виртуальной машины.

```
mstsc /v:<publicIpAddress>
```

## <a name="update-the-application"></a>Обновление приложения

Из предыдущего руководства вы узнали, как отправлять файлы в учетную запись хранения. Откройте `D:\git\storage-dotnet-perf-scale-app\Program.cs` в текстовом редакторе. Замените метод `Main` следующим примером. В этом примере комментируется задача отправки и расскомментируется задача скачивания, а также, по завершении, задача удаления содержимого в учетной записи хранения.

```csharp
public static void Main(string[] args)
{
    Console.WriteLine("Azure Blob storage performance and scalability sample");
    // Set threading and default connection limit to 100 to ensure multiple threads and connections can be opened.
    // This is in addition to parallelism with the storage client library that is defined in the functions below.
    ThreadPool.SetMinThreads(100, 4);
    ServicePointManager.DefaultConnectionLimit = 100; // (Or More)

    bool exception = false;
    try
    {
        // Call the UploadFilesAsync function.
        UploadFilesAsync().GetAwaiter().GetResult();

        // Uncomment the following line to enable downloading of files from the storage account.  This is commented out
        // initially to support the tutorial at https://docs.microsoft.com/azure/storage/blobs/storage-blob-scalable-app-download-files.
        // DownloadFilesAsync().GetAwaiter().GetResult();
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
        exception = true;
    }
    finally
    {
        // The following function will delete the container and all files contained in them.  This is commented out initially
        // As the tutorial at https://docs.microsoft.com/azure/storage/blobs/storage-blob-scalable-app-download-files has you upload only for one tutorial and download for the other. 
        if (!exception)
        {
            // DeleteExistingContainersAsync().GetAwaiter().GetResult();
        }
        Console.WriteLine("Press any key to exit the application");
        Console.ReadKey();
    }
}
```

После обновления приложения его снова необходимо создать. Откройте `Command Prompt` и перейдите к `D:\git\storage-dotnet-perf-scale-app`. Перестройте приложение, выполнив `dotnet build`, как показано в следующем примере:

```
dotnet build
```

## <a name="run-the-application"></a>Выполнение приложения

Перестроив приложение, можно запустить его с обновленным кодом. Откройте `Command Prompt` (если еще не сделали этого) и перейдите к `D:\git\storage-dotnet-perf-scale-app`.

Нажмите клавишу `dotnet run`, чтобы запустить приложение.

```
dotnet run
```

Приложение считывает контейнеры, расположенные в учетной записи хранения, указанной в **storageconnectionstring**. При помощи метода [ListBlobsSegmented](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer) за раз оно проходит по 10 больших двоичных объектов в контейнерах и скачивает их на локальный компьютер с помощью метода [DownloadToFileAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.downloadtofileasync).
В следующей таблице показаны [BlobRequestOptions](/dotnet/api/microsoft.azure.storage.blob.blobrequestoptions), которые определятся для каждого большого двоичного объекта при скачивании.

|Свойство|Значение|ОПИСАНИЕ|
|---|---|---|
|[DisableContentMD5Validation](/dotnet/api/microsoft.azure.storage.blob.blobrequestoptions.disablecontentmd5validation)| Да| Это свойство отключает проверку хэша MD5 отправляемого содержимого. При этом передача ускоряется. Но без проверки MD5 не будет подтверждения о достоверности или целостности передаваемых файлов. |
|[StoreBlobContentMD5](/dotnet/api/microsoft.azure.storage.blob.blobrequestoptions.storeblobcontentmd5)| false| Это свойство определяет, будет ли хэш MD5 вычисляться и сохраняться.   |

Задание `DownloadFilesAsync`, показано в следующем примере:

```csharp
private static async Task DownloadFilesAsync()
{
    CloudBlobClient blobClient = GetCloudBlobClient();

    // Define the BlobRequestOptions on the download, including disabling MD5 hash validation for this example, this improves the download speed.
    BlobRequestOptions options = new BlobRequestOptions
    {
        DisableContentMD5Validation = true,
        StoreBlobContentMD5 = false
    };

    // Retrieve the list of containers in the storage account.  Create a directory and configure variables for use later.
    BlobContinuationToken continuationToken = null;
    List<CloudBlobContainer> containers = new List<CloudBlobContainer>();
    do
    {
        var listingResult = await blobClient.ListContainersSegmentedAsync(continuationToken);
        continuationToken = listingResult.ContinuationToken;
        containers.AddRange(listingResult.Results);
    }
    while (continuationToken != null);

    var directory = Directory.CreateDirectory("download");
    BlobResultSegment resultSegment = null;
    Stopwatch time = Stopwatch.StartNew();

    // Download the blobs
    try
    {
        List<Task> tasks = new List<Task>();
        int max_outstanding = 100;
        int completed_count = 0;

        // Create a new instance of the SemaphoreSlim class to define the number of threads to use in the application.
        SemaphoreSlim sem = new SemaphoreSlim(max_outstanding, max_outstanding);

        // Iterate through the containers
        foreach (CloudBlobContainer container in containers)
        {
            do
            {
                // Return the blobs from the container lazily 10 at a time.
                resultSegment = await container.ListBlobsSegmentedAsync(null, true, BlobListingDetails.All, 10, continuationToken, null, null);
                continuationToken = resultSegment.ContinuationToken;
                {
                    foreach (var blobItem in resultSegment.Results)
                    {

                        if (((CloudBlob)blobItem).Properties.BlobType == BlobType.BlockBlob)
                        {
                            // Get the blob and add a task to download the blob asynchronously from the storage account.
                            CloudBlockBlob blockBlob = container.GetBlockBlobReference(((CloudBlockBlob)blobItem).Name);
                            Console.WriteLine("Downloading {0} from container {1}", blockBlob.Name, container.Name);
                            await sem.WaitAsync();
                            tasks.Add(blockBlob.DownloadToFileAsync(directory.FullName + "\\" + blockBlob.Name, FileMode.Create, null, options, null).ContinueWith((t) =>
                            {
                                sem.Release();
                                Interlocked.Increment(ref completed_count);
                            }));

                        }
                    }
                }
            }
            while (continuationToken != null);
        }

        // Creates an asynchronous task that completes when all the downloads complete.
        await Task.WhenAll(tasks);
    }
    catch (Exception e)
    {
        Console.WriteLine("\nError encountered during transfer: {0}", e.Message);
    }

    time.Stop();
    Console.WriteLine("Download has been completed in {0} seconds. Press any key to continue", time.Elapsed.TotalSeconds.ToString());
    Console.ReadLine();
}
```

### <a name="validate-the-connections"></a>Проверка подключений

Во время скачивания файлов можно проверить число одновременных подключений к учетной записи хранения. Откройте `Command Prompt`, а затем введите команду `netstat -a | find /c "blob:https"`. Эта команда показывает количество подключений, которые в настоящее время открыты с помощью `netstat`. В следующем примере показан результат, аналогичный тому, который отобразится при самостоятельном запуске руководства. Как видно из примера, при скачивании произвольных файлов из учетной записи хранения было открыто более 280 подключений.

```
C:\>netstat -a | find /c "blob:https"
289

C:\>
```

## <a name="next-steps"></a>Дополнительная информация

Из третьей части в серии вы узнали не только о скачивании больших объемов случайных данных из учетной записи хранения, но и о следующем:

> [!div class="checklist"]
> * Выполнение приложения
> * Проверка количества подключений.

Перейдите к четвертой части в серии, чтобы проверить показатели пропускной способности и задержки на портале.

> [!div class="nextstepaction"]
> [Verify throughput and latency metrics for a storage account](storage-blob-scalable-app-verify-metrics.md) (Проверка показателей пропускной способности и задержки для учетной записи хранения)

[previous-tutorial]: storage-blob-scalable-app-upload-files.md
