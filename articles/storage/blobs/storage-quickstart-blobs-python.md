---
title: Краткое руководство. Использование библиотеки хранилища BLOB-объектов Azure версии 12 для Python
description: В этом кратком руководстве вы узнаете, как использовать клиентскую библиотеку службы хранилища BLOB-объектов Azure версии 12 для Python для создания контейнера и большого двоичного объекта в хранилище BLOB-объектов. Далее вы узнаете, как скачать большой двоичный объект на локальный компьютер и как получить список всех больших двоичных объектов в контейнере.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 07/24/2020
ms.service: storage
ms.subservice: blobs
ms.topic: quickstart
ms.custom: devx-track-python
ms.openlocfilehash: dce42f1b8bfe58e4d147ba1c982cb770590dfa92
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2020
ms.locfileid: "87845125"
---
# <a name="quickstart-manage-blobs-with-python-v12-sdk"></a>Краткое руководство. Управление большими двоичными объектами с помощью пакета SDK для Python версии 12

Из этого краткого руководства вы узнаете, как управлять большими двоичными объектами с использованием Python. Большие двоичные объекты — это объекты, которые могут содержать большие объемы текстовых или двоичных данных, включая изображения, документы, потоковое мультимедиа и архивные данные. Вы научитесь отправлять и скачивать большие двоичные объекты, получать список таких объектов, а также создавать и удалять контейнеры.

Дополнительные ресурсы:

* [Справочная документация по API](/python/api/azure-storage-blob)
* [Исходный код библиотеки](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-blob)
* [Пакет (индекс пакета Python)](https://pypi.org/project/azure-storage-blob/)
* [Примеры](https://docs.microsoft.com/azure/storage/common/storage-samples-python?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-samples)

## <a name="prerequisites"></a>Предварительные требования

- Учетная запись Azure с активной подпиской. [Создайте учетную запись](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) бесплатно.
- Учетная запись хранения Azure. [Создание учетной записи хранения](../common/storage-account-create.md).
- [Python](https://www.python.org/downloads/) 2.7, 3.5 или более поздней версии.

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="setting-up"></a>Настройка

В этом разделе рассматривается подготовка проекта для работы с клиентской библиотекой хранилища BLOB-объектов Azure версии 12 для Python.

### <a name="create-the-project"></a>Создание проекта

Создайте приложение Python с именем *blob-quickstart-v12*.

1. В окне консоли (командная строка, PowerShell или Bash) создайте каталог для проекта.

    ```console
    mkdir blob-quickstart-v12
    ```

1. Перейдите в только что созданный каталог *blob-quickstart-v12*.

    ```console
    cd blob-quickstart-v12
    ```

1. В каталоге *blob-quickstart-v12* создайте каталог *data*. Это каталог для создания и хранения файлов данных больших двоичных объектов.

    ```console
    mkdir data
    ```

### <a name="install-the-package"></a>Установка пакета

Оставаясь в каталоге приложения, установите клиентскую библиотеку хранилища BLOB-объектов Azure для пакета Python с помощью команды `pip install`.

```console
pip install azure-storage-blob
```

Эта команда устанавливает клиентскую библиотеку хранилища BLOB-объектов Azure для пакета Python и все библиотеки, от которых она зависит. В данном случае это только основная библиотека Azure для Python.

### <a name="set-up-the-app-framework"></a>Настройка платформы приложения

Из каталога проекта:

1. Откройте новый текстовый файл в редакторе кода.
1. Добавьте в него операторы `import`.
1. Создайте структуру программы, включая простую обработку исключений.

    Вот этот код:

    ```python
    import os, uuid
    from azure.storage.blob import BlobServiceClient, BlobClient, ContainerClient

    try:
        print("Azure Blob storage v12 - Python quickstart sample")
        # Quick start code goes here
    except Exception as ex:
        print('Exception:')
        print(ex)
    ```

1. Сохраните новый файл как *blob-quickstart-v12.py* в каталоге *blob-quickstart-v12*.

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="object-model"></a>Объектная модель

Хранилище BLOB-объектов Azure оптимизировано для хранения больших объемов неструктурированных данных. Неструктурированные данные — это данные, которые не соответствуют определенной модели данных или определению, например текстовых или двоичных данных. В хранилище BLOB-объектов предлагается три типа ресурсов:

* учетная запись хранения;
* контейнер в учетной записи хранения;
* большой двоичный объект в контейнере.

На следующей схеме показана связь между этими ресурсами.

![Схема архитектуры службы хранилища BLOB-объектов](./media/storage-blobs-introduction/blob1.png)

Используйте следующие классы Python для взаимодействия с этими ресурсами.

* [BlobServiceClient](/python/api/azure-storage-blob/azure.storage.blob.blobserviceclient). Класс `BlobServiceClient` позволяет управлять ресурсами службы хранилища Azure и контейнерами больших двоичных объектов.
* [ContainerClient](/python/api/azure-storage-blob/azure.storage.blob.containerclient). Класс `ContainerClient` позволяет управлять контейнерами службы хранилища Azure и содержащимися в них большими двоичными объектами.
* [BlobClient](/python/api/azure-storage-blob/azure.storage.blob.blobclient). Класс `BlobClient` позволяет управлять большими двоичными объектами службы хранилища Azure.

## <a name="code-examples"></a>Примеры кода

В этих примерах фрагментов кода показано, как выполнять следующие действия с помощью клиентской библиотеки хранилища BLOB-объектов Azure для Python:

* [Получение строки подключения](#get-the-connection-string)
* [Создание контейнера](#create-a-container)
* [отправка больших двоичных объектов в контейнер](#upload-blobs-to-a-container);
* [перечисление больших двоичных объектов в контейнере](#list-the-blobs-in-a-container);
* [скачивание больших двоичных объектов](#download-blobs);
* [Удаление контейнера](#delete-a-container)

### <a name="get-the-connection-string"></a>Получение строки подключения

Приведенный ниже код извлекает строку подключения для учетной записи хранения из переменной среды, созданной в разделе [Настройка строки подключения хранилища](#configure-your-storage-connection-string).

Добавьте этот код в блок `try`.

```python
# Retrieve the connection string for use with the application. The storage
# connection string is stored in an environment variable on the machine
# running the application called AZURE_STORAGE_CONNECTION_STRING. If the environment variable is
# created after the application is launched in a console or with Visual Studio,
# the shell or application needs to be closed and reloaded to take the
# environment variable into account.
connect_str = os.getenv('AZURE_STORAGE_CONNECTION_STRING')
```

### <a name="create-a-container"></a>Создание контейнера

Выберите имя нового контейнера. Приведенный ниже код добавляет к имени контейнера значение UUID, чтобы сделать это имя уникальным.

> [!IMPORTANT]
> Имена контейнеров должны состоять из знаков нижнего регистра. Дополнительные сведения об именовании контейнеров и больших двоичных объектов см. в статье [Naming and Referencing Containers, Blobs, and Metadata](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Именование контейнеров, больших двоичных объектов и метаданных и ссылка на них).

Создайте экземпляр класса [BlobServiceClient](/python/api/azure-storage-blob/azure.storage.blob.blobserviceclient), вызвав метод [from_connection_string](/python/api/azure-storage-blob/azure.storage.blob.blobserviceclient#from-connection-string-conn-str--credential-none----kwargs-). Затем вызовите метод [create_container](/python/api/azure-storage-blob/azure.storage.blob.blobserviceclient#create-container-name--metadata-none--public-access-none----kwargs-), чтобы создать контейнер в учетной записи хранения.

Добавьте следующий код в конец блока `try`.

```python
# Create the BlobServiceClient object which will be used to create a container client
blob_service_client = BlobServiceClient.from_connection_string(connect_str)

# Create a unique name for the container
container_name = "quickstart" + str(uuid.uuid4())

# Create the container
container_client = blob_service_client.create_container(container_name)
```

### <a name="upload-blobs-to-a-container"></a>Отправка больших двоичных объектов в контейнер

Приведенный ниже фрагмент кода:

1. Создает текстовый файл в локальном каталоге.
1. Возвращает ссылку на объект [BlobClient](/python/api/azure-storage-blob/azure.storage.blob.blobclient), вызывая метод [get_blob_client](/python/api/azure-storage-blob/azure.storage.blob.containerclient#get-blob-client-blob--snapshot-none-) для [BlobServiceClient](/python/api/azure-storage-blob/azure.storage.blob.blobserviceclient) из раздела [Создание контейнера](#create-a-container).
1. Передает локальный текстовый файл в большой двоичный объект, вызывая метод [upload_blob](/python/api/azure-storage-blob/azure.storage.blob.blobclient#upload-blob-data--blob-type--blobtype-blockblob---blockblob----length-none--metadata-none----kwargs-).

Добавьте следующий код в конец блока `try`.

```python
# Create a file in local data directory to upload and download
local_path = "./data"
local_file_name = "quickstart" + str(uuid.uuid4()) + ".txt"
upload_file_path = os.path.join(local_path, local_file_name)

# Write text to the file
file = open(upload_file_path, 'w')
file.write("Hello, World!")
file.close()

# Create a blob client using the local file name as the name for the blob
blob_client = blob_service_client.get_blob_client(container=container_name, blob=local_file_name)

print("\nUploading to Azure Storage as blob:\n\t" + local_file_name)

# Upload the created file
with open(upload_file_path, "rb") as data:
    blob_client.upload_blob(data)
```

### <a name="list-the-blobs-in-a-container"></a>Перечисление BLOB-объектов в контейнере

Выведите список больших двоичных объектов в контейнере, вызвав метод [list_blobs](/python/api/azure-storage-blob/azure.storage.blob.containerclient#list-blobs-name-starts-with-none--include-none----kwargs-). В этом случае в контейнер был добавлен лишь один большой двоичный объект, поэтому операция перечисления возвращает только его.

Добавьте следующий код в конец блока `try`.

```python
print("\nListing blobs...")

# List the blobs in the container
blob_list = container_client.list_blobs()
for blob in blob_list:
    print("\t" + blob.name)
```

### <a name="download-blobs"></a>Скачивание больших двоичных объектов

Скачайте созданный ранее большой двоичный объект, вызвав метод [download_blob](/python/api/azure-storage-blob/azure.storage.blob.blobclient#download-blob-offset-none--length-none----kwargs-). Пример кода добавляет суффикс "DOWNLOAD" к имени файла, чтобы в локальной файловой системе можно было просмотреть оба файла.

Добавьте следующий код в конец блока `try`.

```python
# Download the blob to a local file
# Add 'DOWNLOAD' before the .txt extension so you can see both files in the data directory
download_file_path = os.path.join(local_path, str.replace(local_file_name ,'.txt', 'DOWNLOAD.txt'))
print("\nDownloading blob to \n\t" + download_file_path)

with open(download_file_path, "wb") as download_file:
    download_file.write(blob_client.download_blob().readall())
```

### <a name="delete-a-container"></a>Удаление контейнера

Следующий код очищает созданные приложением ресурсы, полностью удаляя контейнер с помощью метода [delete_container](/python/api/azure-storage-blob/azure.storage.blob.containerclient#delete-container---kwargs-). Кроме того, при необходимости можно удалить локальные файлы.

Приложение приостанавливается для ввода пользователя, вызывая `input()`, перед удалением большого двоичного объекта, контейнера и локальных файлов. Это хорошая возможность проверить правильность создания ресурсов перед их удалением.

Добавьте следующий код в конец блока `try`.

```python
# Clean up
print("\nPress the Enter key to begin clean up")
input()

print("Deleting blob container...")
container_client.delete_container()

print("Deleting the local source and downloaded files...")
os.remove(upload_file_path)
os.remove(download_file_path)

print("Done")
```

## <a name="run-the-code"></a>Выполнение кода

В этом приложении тестовый файл создается в локальной папке, а затем передается в хранилище BLOB-объектов. После этого выводится список больших двоичных объектов в контейнере, а затем файл загружается с новым именем, чтобы можно было сравнить старый и новый файлы.

Перейдите в каталог, содержащий файл *blob-quickstart-v12.py*, а затем выполните указанную команду `python`, чтобы запустить приложение.

```console
python blob-quickstart-v12.py
```

Вы должны увидеть выходные данные приложения, как показано ниже.

```output
Azure Blob storage v12 - Python quickstart sample

Uploading to Azure Storage as blob:
        quickstartcf275796-2188-4057-b6fb-038352e35038.txt

Listing blobs...
        quickstartcf275796-2188-4057-b6fb-038352e35038.txt

Downloading blob to
        ./data/quickstartcf275796-2188-4057-b6fb-038352e35038DOWNLOAD.txt

Press the Enter key to begin clean up

Deleting blob container...
Deleting the local source and downloaded files...
Done
```

Прежде чем начать удаление, проверьте наличие двух файлов в папке *data*. Вы можете открыть их и убедиться, что они идентичны.

После проверки файлов нажмите клавишу **ВВОД**, чтобы завершить работу с демонстрационной версией и удалить тестовые файлы.

## <a name="next-steps"></a>Дальнейшие действия

В этом кратком руководстве вы узнали, как передавать и скачивать большие двоичные объекты, а также выводить их список с помощью Python.

Чтобы просмотреть примеры приложений для хранилища BLOB-объектов, перейдите к следующему разделу:

> [!div class="nextstepaction"]
> [Примеры для пакета SDK хранилища BLOB-объектов Azure версии 12 для Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-blob/samples)

* Чтобы узнать больше, ознакомьтесь с [клиентскими библиотеками службы хранилища Azure для Python](/azure/developer/python/sdk/storage/overview?view=storage-py-v12).
* Учебники, примеры, краткие руководства и другую документацию можно найти на странице [Azure для разработчиков Python](/azure/python/).
