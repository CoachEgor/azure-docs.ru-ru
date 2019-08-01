---
title: Краткое руководство. Создание большого двоичного объекта с помощью пакета SDK службы хранилища для Java
description: В этом кратком руководстве описано создание контейнера в хранилище объектов (большой двоичный объект Azure), отправку файла, объекты списка и скачивание с помощью пакета SDK службы хранилища для Java.
services: storage
author: mhopkins-msft
ms.custom: mvc, seo-java-july2019
ms.service: storage
ms.topic: quickstart
ms.date: 11/14/2018
ms.author: mhopkins
ms.reviewer: seguler
ms.openlocfilehash: 12507ba87148bef6378542feb4ebde3b1c291a72
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565918"
---
# <a name="quickstart-upload-download-and-list-blobs-by-using-the-java-storage-sdk-v10"></a>Краткое руководство. Отправка, скачивание и отображение больших двоичных объектов с помощью пакета SDK службы хранилища для Java версии 10

Из этого краткого руководства вы узнаете, как передать, скачать и создать список блочных BLOB-объектов в контейнере в хранилище BLOB-объектов Azure с помощью нового пакета SDK службы хранилища для Java. Новый пакет SDK службы хранилища для Java использует модель реактивного программирования RxJava, которое обеспечивает асинхронные операции. Дополнительные сведения о RxJava см. в разделе [RxJava: Reactive Extensions for the JVM](https://github.com/ReactiveX/RxJava) (RxJava. Реактивные расширения для виртуальной машины Java). 

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

Прежде чем приступить к работе, убедитесь, что у вас установлены следующие дополнительные компоненты:

* [Maven](https://maven.apache.org/download.cgi) для использования средства командной строки или любой выбранной интегрированной среды разработки Java;
* [JDK](https://aka.ms/azure-jdks).

## <a name="download-the-sample-application"></a>Загрузка примера приложения

[Пример приложения](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart), используемый в этом кратком руководстве, является простым консольным приложением. 

Используйте команду [git](https://git-scm.com/), чтобы скачать копию приложения в среду разработки.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart.git
```

Эта команда клонирует репозиторий в локальную папку git.

После завершения импорта проекта откройте **Quickstart.java** в **src/main/java/quickstart**.

[!INCLUDE [storage-copy-account-key-portal](../../../includes/storage-copy-account-key-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Настройка строки подключения хранилища
Это решение требует безопасного хранения имени и ключа учетной записи хранения. Храните их в переменных среды локального компьютера, на котором выполняется пример. Чтобы создать переменные среды, в зависимости от операционной системы выполните либо пример для Linux, либо для Windows.

### <a name="linux-example"></a>Пример Linux

```bash
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

### <a name="windows-example"></a>Пример Windows

```CMD
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

## <a name="run-the-sample"></a>Запуск примера

В каталоге по умолчанию этот пример создает тестовый файл **AppData\Local\Temp** для пользователей Windows. Затем вам будет предложено выполнить следующие действия.

1. Введите команды для отправки тестового файла в хранилище BLOB-объектов Azure.
2. Создайте список BLOB-объектов в контейнере.
3. Загрузите отправленный файл с новым именем для сравнения старого и нового файлов. 

Если необходимо запустить пример, используя Maven в командной строке, откройте оболочку и просмотрите папку **storage-blobs-java-v10-quickstart** в клонированном каталоге. Затем введите `mvn compile exec:java`.

В этом примере показаны выходные данные при запуске приложения на системе Windows.

```Output
Created quickstart container
Enter a command
(P)utBlob | (L)istBlobs | (G)etBlob | (D)eleteBlobs | (E)xitSample
# Enter a command :
P
Uploading the sample file into the container: https://<storageaccount>.blob.core.windows.net/quickstart
# Enter a command :
L
Listing blobs in the container: https://<storageaccount>.blob.core.windows.net/quickstart
Blob name: SampleBlob.txt
# Enter a command :
G
Get the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
The blob was downloaded to C:\Users\<useraccount>\AppData\Local\Temp\downloadedFile13097087873115855761.txt
# Enter a command :
D
Delete the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt

# Enter a command :
>> Blob deleted: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
E
Cleaning up the sample and exiting!
```

Вы управляете образцом, поэтому введите команды для выполнения кода. Входные данные с учетом регистра.

Для просмотра файлов в хранилище BLOB-объектов можно также воспользоваться таким средством, как [Обозреватель службы хранилища Azure](https://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). Обозреватель службы хранилища Azure — это бесплатное кроссплатформенное средство для доступа к данным учетной записи хранения. 

Проверьте файлы. Затем выберите **E** и нажмите **Enter** для завершения демонстрации и удаления тестовых файлов. Теперь вы знаете, что делает этот пример. Давайте откроем файл **Quickstart.java** и изучим его код. 

## <a name="understand-the-sample-code"></a>Разбор примера кода

Следующие разделы содержат пример кода с описанием его работы.

### <a name="get-references-to-the-storage-objects"></a>Получение ссылок на объекты хранилища

Сначала создайте ссылки на объекты, используемые для доступа к хранилищу BLOB-объектов и управления им. Эти объекты компилируются друг с другом. Каждый используется следующим в списке.

1. Создайте экземпляр объекта **StorageURL**, который указывает на учетную запись хранения.

    * Объект [StorageURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._storage_u_r_l?view=azure-java-stable) является представлением учетной записи хранения. Он используется для создания конвейера. 
    * Конвейер — это набор политик, который используется для манипулирования запросами и ответами с помощью механизмов авторизации, ведения журналов и повторов. Дополнительные сведения см. в разделе [URL Types & Http Pipeline](https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview#url-types--http-pipeline) (Типы URL и конвейер HTTP).  
    * С помощью конвейера создайте экземпляр объекта [ServiceURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._service_u_r_l?view=azure-java-stable).
    * С помощью объекта **ServiceURL** создайте экземпляр [ContainerURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-stable).
    * **ContainerURL** требуется для выполнения операций в контейнерах больших двоичных объектов.

2. Создайте экземпляр объекта **ContainerURL**, представляющий контейнер, к которому осуществляется доступ. Контейнеры организуют BLOB-объекты аналогично папкам для упорядочивания файлов на компьютере.

    * **ContainerURL** предоставляет точку доступа к службе контейнеров. 
    * Можно создать экземпляр объекта [BlobURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._blob_u_r_l?view=azure-java-stable) с помощью [ContainerURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-stable).
    * **BlobURL** необходим для создания больших двоичных объектов.

3. Создайте экземпляр объекта **BlobURL**, который указывает на конкретный интересующий большой двоичный объект. 

> [!IMPORTANT]
> Имена контейнеров должны состоять из знаков нижнего регистра. Дополнительные сведения об именовании контейнеров и больших двоичных объектов см. в статье [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Именование контейнеров, больших двоичных объектов и метаданных и ссылка на них).

### <a name="create-a-container"></a>Создание контейнера 

В этом разделе создается экземпляр объекта **ContainerURL**. Вместе с ним можно создать контейнер. В примере контейнер называется **quickstartblobs**. 

В этом примере используется [ContainerURL.create](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l.create?view=azure-java-stable#com_microsoft_azure_storage_blob__container_u_r_l_create_Metadata_PublicAccessType_Context_), поэтому при каждом запуске примера требуется создавать новый контейнер. Или вы можете создать контейнер заранее, чтобы не создавать его в коде.

```java
// Create a ServiceURL to call the Blob service. We will also use this to construct the ContainerURL
SharedKeyCredentials creds = new SharedKeyCredentials(accountName, accountKey);
// We are using a default pipeline here, you can learn more about it at https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview
final ServiceURL serviceURL = new ServiceURL(new URL("http://" + accountName + ".blob.core.windows.net"), StorageURL.createPipeline(creds, new PipelineOptions()));

// Let's create a container using a blocking call to Azure Storage
// If container exists, we'll catch and continue
containerURL = serviceURL.createContainerURL("quickstart");

try {
    ContainersCreateResponse response = containerURL.create(null, null).blockingGet();
    System.out.println("Container Create Response was " + response.statusCode());
} catch (RestException e){
    if (e instanceof RestException && ((RestException)e).response().statusCode() != 409) {
        throw e;
    } else {
        System.out.println("quickstart container already exists, resuming...");
    }
}
```

### <a name="upload-blobs-to-the-container"></a>Отправка BLOB-объектов в контейнер

Хранилище BLOB-объектов поддерживает блочные, добавочные и страничные BLOB-объекты. Чаще всего используются блочные BLOB-объекты. Они используются также и в этом кратком руководстве. 

1. Чтобы отправить файл в BLOB-объект, получите ссылку на BLOB-объект в целевом контейнере. 
2. Получив ссылку на большой двоичный объект, можно отправить ему файл с помощью любого из следующих API.

   * Низкоуровневые интерфейсы API. Примерами являются [BlockBlobURL.upload](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.upload?view=azure-java-stable#com_microsoft_azure_storage_blob__block_blob_u_r_l_upload_Flowable_ByteBuffer__long_BlobHTTPHeaders_Metadata_BlobAccessConditions_Context_), также называемый PutBlob, и [BlockBlobURL.stageBlock](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.stageblock?view=azure-java-stable), также называемый PutBLock, в экземпляре **BlockBlobURL**. 

   * Высокоуровневые интерфейсы API предоставлены в [классе TransferManager](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._transfer_manager?view=azure-java-stable). Например, метод [TransferManager.uploadFileToBlockBlob](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._transfer_manager.uploadfiletoblockblob?view=azure-java-stable). 

     С помощью этой операции создается большой двоичный объект, если он не был создан ранее. Если большой двоичный объект уже существует, то он будет перезаписан.

Пример кода создает локальный файл для отправки и скачивания. Он сохраняет файл для отправки как **sourceFile**, а URL-адрес большого двоичного объекта в **blob**. В приведенном ниже примере файл отправляется в контейнер с именем **quickstart**.

```java
static void uploadFile(BlockBlobURL blob, File sourceFile) throws IOException {

    FileChannel fileChannel = FileChannel.open(sourceFile.toPath());

    // Uploading a file to the blobURL using the high-level methods available in TransferManager class
    // Alternatively call the Upload/StageBlock low-level methods from BlockBlobURL type
    TransferManager.uploadFileToBlockBlob(fileChannel, blob, 8*1024*1024, null)
        .subscribe(response-> {
            System.out.println("Completed upload request.");
            System.out.println(response.response().statusCode());
        });
}
```

Блочный BLOB-объект может представлять собой текстовый или двоичный файл любого типа. Страничные BLOB-объекты в основном используются для файлов виртуального жесткого диска, применяемых для поддержки виртуальных машин IaaS. Большие двоичные объекты добавления используются для добавления данных в конец. Они часто используются для ведения журнала. Большинство объектов, находящихся в хранилище BLOB-объектов, представляют собой блочные BLOB-объекты.

### <a name="list-the-blobs-in-a-container"></a>Перечисление BLOB-объектов в контейнере

С помощью [ContainerURL.listBlobsFlatSegment](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l.listblobsflatsegment?view=azure-java-stable) можно получить список объектов в контейнере. Этот метод возвращает до 5 000 объектов одновременно с маркером продолжения или далее, если в контейнере есть дополнительные объекты. Создайте вспомогательную функцию, которая вызывает себя саму несколько раз, когда в предыдущем ответе **listBlobsFlatSegment** не появляется следующий маркер.

```java
static void listBlobs(ContainerURL containerURL) {
    // Each ContainerURL.listBlobsFlatSegment call return up to maxResults (maxResults=10 passed into ListBlobOptions below).
    // To list all Blobs, we are creating a helper static method called listAllBlobs,
    // and calling it after the initial listBlobsFlatSegment call
    ListBlobsOptions options = new ListBlobsOptions(null, null, 10);

    containerURL.listBlobsFlatSegment(null, options)
        .flatMap(containersListBlobFlatSegmentResponse ->
            listAllBlobs(containerURL, containersListBlobFlatSegmentResponse))
                .subscribe(response-> {
                    System.out.println("Completed list blobs request.");
                    System.out.println(response.statusCode());
                });
}

private static Single <ContainersListBlobFlatSegmentResponse> listAllBlobs(ContainerURL url, ContainersListBlobFlatSegmentResponse response) {
    // Process the blobs returned in this result segment (if the segment is empty, blobs() will be null.
    if (response.body().blobs() != null) {
        for (Blob b : response.body().blobs().blob()) {
            String output = "Blob name: " + b.name();
            if (b.snapshot() != null) {
                output += ", Snapshot: " + b.snapshot();
            }
            System.out.println(output);
        }
    }
    else {
        System.out.println("There are no more blobs to list off.");
    }

    // If there is not another segment, return this response as the final response.
    if (response.body().nextMarker() == null) {
        return Single.just(response);
    } else {
        /*
        IMPORTANT: ListBlobsFlatSegment returns the start of the next segment; you MUST use this to get the next
        segment (after processing the current result segment
        */

        String nextMarker = response.body().nextMarker();

        /*
        The presence of the marker indicates that there are more blobs to list, so we make another call to
        listBlobsFlatSegment and pass the result through this helper function.
        */

        return url.listBlobsFlatSegment(nextMarker, new ListBlobsOptions(null, null,1))
            .flatMap(containersListBlobFlatSegmentResponse ->
                listAllBlobs(url, containersListBlobFlatSegmentResponse));
    }
}
```

### <a name="download-blobs"></a>Скачивание больших двоичных объектов

Чтобы скачать большие двоичные объекты на локальный диск, воспользуйтесь методом [BlobURL.download](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._blob_u_r_l.download?view=azure-java-stable).

Следующий код скачивает BLOB-объект, который мы передали в предыдущем разделе. К имени BLOB-объекта добавляется суффикс **_DOWNLOADED**, поэтому можно видеть оба файла на локальном диске. 

```java
static void getBlob(BlockBlobURL blobURL, File sourceFile) {
    try {
        // Get the blob using the low-level download method in BlockBlobURL type
        // com.microsoft.rest.v2.util.FlowableUtil is a static class that contains helpers to work with Flowable
        blobURL.download(new BlobRange(0, Long.MAX_VALUE), null, false)
            .flatMapCompletable(response -> {
                AsynchronousFileChannel channel = AsynchronousFileChannel.open(Paths
                    .get(sourceFile.getPath()), StandardOpenOption.CREATE,  StandardOpenOption.WRITE);
                        return FlowableUtil.writeFile(response.body(), channel);
            }).doOnComplete(()-> System.out.println("The blob was downloaded to " + sourceFile.getAbsolutePath()))
            // To call it synchronously add .blockingAwait()
            .subscribe();
    } catch (Exception ex){
        System.out.println(ex.toString());
    }
}
```

### <a name="clean-up-resources"></a>Очистка ресурсов

Если больше нет необходимости в больших двоичных объектах, отправленных при работе с этим руководством, удалите весь контейнер с помощью метода [ContainerURL.delete](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l.delete?view=azure-java-stable). Он также удаляет файлы в контейнере.

```java
containerURL.delete(null).blockingGet();
```

## <a name="next-steps"></a>Дополнительная информация

В этом кратком руководстве было описано, как передавать файлы между локальным диском и хранилищем BLOB-объектов Azure с помощью Java. 

> [!div class="nextstepaction"]
> [Хранилища пакета SDK, версия 10 для исходного кода Java ](https://github.com/Azure/azure-storage-java/)
> [Client](https://docs.microsoft.com/java/api/overview/azure/storage/client?view=azure-java-stable) (Клиент) 
> [Примеры для службы хранилища Azure с использованием Java](https://github.com/ReactiveX/RxJava)
