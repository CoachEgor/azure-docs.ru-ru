---
title: Передача файлов в учетную запись служб мультимедиа Azure v3 с помощью функции "ОСТАВШАЯся" | Документация Майкрософт
description: Узнайте, как включить мультимедийное содержимое в службы мультимедиа, создав и отправив ресурс.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: a9fd38b857f513ff67ae2fbe4ccd0c6c9540967e
ms.sourcegitcommit: 5b073caafebaf80dc1774b66483136ac342f7808
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75779167"
---
# <a name="upload-files-into-a-media-services-account-using-rest"></a>Передача файлов в учетную запись служб мультимедиа с помощью REST

В службах мультимедиа цифровые файлы отправляются в контейнер BLOB-объектов, связанный с ресурсом. Сущность [Ресурс](https://docs.microsoft.com/rest/api/media/operations/asset) может содержать видео, аудио, изображения, коллекции эскизов, текстовые каналы и файлы скрытых субтитров (а также метаданные этих файлов). После отправки этих файлов в контейнер ресурса содержимое сохраняется в безопасном расположении в облаке для дальнейшей обработки и потоковой передачи.

В этой статье показано, как передать локальный файл с помощью функции RESTFUL.

## <a name="prerequisites"></a>Технические условия

Чтобы выполнить действия, описанные в этом разделе, необходимо сделать следующее:

- Ознакомьтесь с [концепцией ресурсов](assets-concept.md).
- [Настройте Postman для вызовов REST API Служб мультимедиа Azure](media-rest-apis-with-postman.md).
    
    Не забудьте выполнить последний шаг, указанный в разделе [Получение токена Azure AD](media-rest-apis-with-postman.md#get-azure-ad-token). 

## <a name="create-an-asset"></a>Создание ресурса

В этом разделе показано, как создать новый ресурс.

1. Выберите **Ресурсы** -> **Создать или обновить ресурс**.
2. Нажмите кнопку **Отправить**.

    ![Создание ресурса](./media/upload-files/postman-create-asset.png)

Вы увидите **Ответ** с информацией о созданном ресурсе.

## <a name="get-a-sas-url-with-read-write-permissions"></a>Получение подписанного URL-адреса с разрешениями на чтение и запись 

В этом разделе показано, как получить подписанный URL-адрес для созданного ресурса. URL-адрес был создан с разрешениями на чтение и запись и может использоваться для передачи цифровых файлов в контейнер ресурса.

1. Выберите **Ресурсы** -> **Список URL-адресов ресурса**.
2. Нажмите кнопку **Отправить**.

    ![Отправка файла](./media/upload-files/postman-create-sas-locator.png)

Вы увидите **Ответ** с информацией о URL-адресах ресурса. Скопируйте первый URL-адрес и используйте его для отправки файла.

## <a name="upload-a-file-to-blob-storage-using-the-upload-url"></a>Отправка файла в хранилище больших двоичных объектов с помощью URL-адреса отправки.

Используйте API-интерфейсы или пакеты SDK службы хранилища Azure (например, [REST API хранилища](../../storage/common/storage-rest-api-auth.md) или [пакет SDK для .NET](../../storage/blobs/storage-quickstart-blobs-dotnet.md)).

## <a name="next-steps"></a>Дальнейшие действия

[Учебник. кодирование удаленного файла на основе URL-адреса и поток видео — остальное](stream-files-tutorial-with-rest.md)
