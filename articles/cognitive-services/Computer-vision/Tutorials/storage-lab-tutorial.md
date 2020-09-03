---
title: Руководство по Генерирование метаданных для изображений Azure
titleSuffix: Azure Cognitive Services
description: В этом учебнике вы узнаете, как интегрировать службу "Компьютерное зрение Azure" в веб-приложение для создания метаданных изображений.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: tutorial
ms.date: 08/05/2020
ms.author: pafarley
ms.custom: devx-track-csharp
ms.openlocfilehash: f14bcc1b0576a5ad2f4dbbf30311f613607d3ee5
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/02/2020
ms.locfileid: "89322336"
---
# <a name="tutorial-use-computer-vision-to-generate-image-metadata-in-azure-storage"></a>Руководство по Создание метаданных изображений в службе хранилища Azure с помощью API компьютерного зрения

В этом руководстве вы узнаете, как интегрировать службу Azure "Компьютерное зрение" в веб-приложение для создания метаданных для переданных изображений. Это полезно для сценариев [управления цифровыми активами (DAM)](../overview.md#computer-vision-for-digital-asset-management), например, если компании нужно быстро создать для всех изображений описательные субтитры или ключевые слова с возможностью поиска.

Руководство по всему приложению можно найти на странице о [службе хранилища Azure и лаборатории Cognitive Services](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md) на GitHub, а это руководство в основном охватывает упражнение 5 лаборатории. Вы можете создать сквозное приложение, следуя каждому шагу, но если вам просто интересно, как службу "Компьютерное зрение" можно интегрировать в существующее веб-приложение, прочтите об этом здесь.

В этом учебнике описаны следующие процедуры.

> [!div class="checklist"]
> * Создание ресурса API компьютерного зрения в Azure.
> * Выполнение анализа изображений на изображениях службы хранилища Azure.
> * Прикрепление метаданных к изображениям, размещенным в службе хранилища Azure.
> * Проверка метаданных изображения с помощью Обозревателя службы хранилища Azure.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/cognitive-services), прежде чем начинать работу. 

## <a name="prerequisites"></a>Предварительные требования

- [Выпуск Visual Studio Community 2017](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) или более поздней версии со следующими установленными рабочими нагрузками: "ASP.NET и веб-разработка" и "Разработка для Azure".
- Учетная запись хранения Azure с контейнером больших двоичных объектов, настроенным для хранения изображений (выполните [упражнения 1 лаборатории службы хранилища Azure](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md#Exercise1), если вам нужна помощь с этим шагом).
- Средство Обозревателя службы хранилища Azure (выполните [упражнение 2 лаборатории службы хранилища Azure](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md#Exercise2), если вам нужна помощь с этим шагом).
- Веб-приложение ASP.NET с доступом к службе хранилища Azure (выполните [упражнение 3 лаборатории службы хранилища Azure](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md#Exercise3) для быстрого создания такого приложения).

## <a name="create-a-computer-vision-resource"></a>Создание ресурса API компьютерного зрения

Вам необходимо будет создать ресурс службы "Компьютерное зрение" для учетной записи Azure. Этот ресурс управляет вашим доступом к службе API компьютерного зрения Azure. 

1. Чтобы создать ресурс API "Компьютерное зрение", следуйте инструкциям из раздела [Подписка на одну службу](../../cognitive-services-apis-create-account.md).

1. Перейдите в меню для группы ресурсов и щелкните имя подписки API "Компьютерное зрение", которую вы только что создали. Скопируйте URL-адрес в разделе **Конечная точка** в другое расположение, где вы можете легко извлечь его позже. Щелкните **Показать ключи доступа**.

    ![Страница портала Azure с URL-адресом конечной точки и выделенной ссылкой на ключи доступа](../Images/copy-vision-endpoint.png)
    
    [!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]


1. В следующем окне скопируйте значение **KEY 1** в буфер обмена.

    ![Диалоговое окно "Управление ключами" с выделенной кнопкой копирования](../Images/copy-vision-key.png)

## <a name="add-computer-vision-credentials"></a>Добавление учетных данных API компьютерного зрения

Далее вы добавите необходимые учетные данные в приложение таким образом, чтобы оно могло получить доступ к ресурсам службы "Компьютерное зрение".

Откройте веб-приложение ASP.NET в Visual Studio и перейдите к файлу **Web.config** в корневой папке проекта. Добавьте следующие инструкции в раздел `<appSettings>` файла, заменив значение `VISION_KEY` ключом, скопированным на предыдущем шаге, а значение `VISION_ENDPOINT` — URL-адресом, сохраненным на шаге до этого.

```xml
<add key="SubscriptionKey" value="VISION_KEY" />
<add key="VisionEndpoint" value="VISION_ENDPOINT" />
```

Затем в обозревателе решений щелкните правой кнопкой мыши проект и используйте команду **Управление пакетами NuGet**, чтобы установить пакет **Microsoft.Azure.CognitiveServices.Vision.ComputerVision**. Этот пакет содержит типы, необходимые для вызова API компьютерного зрения.

## <a name="add-metadata-generation-code"></a>Добавление кода для создания метаданных

Далее вы добавите код, который фактически использует службу API компьютерного зрения для создания метаданных для изображений. Эти действия будут применяться для приложения ASP.NET в лаборатории, но их можно адаптировать для собственных приложений. Важно то, что на этом этапе у вас есть веб-приложение ASP.NET, которое может отправлять изображения в контейнер службы хранилища Azure, считывать их из него и отображать их в представлении. Если вы не уверены, как выполнить эти действия, перейдите к [упражнению 3 лаборатории службы хранилища Azure](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md#Exercise3). 

1. Откройте файл *HomeController.cs* в папке **Контроллеры** проекта и добавьте следующие инструкции `using` в верхней части файла:

    ```csharp
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;
    ```

1. Перейдите к методу **Upload**. Этот метод преобразует и отправляет изображения в хранилище BLOB-объектов. Добавьте следующий код сразу после блока, который начинается с `// Generate a thumbnail` (или в конце процесса создания большого двоичного объекта с изображением). Этот код принимает большой двоичный объект, содержащий изображение(`photo`), и использует API компьютерного зрения для создания описания этого изображения. API компьютерного зрения также создает список ключевых слов, которые применяются к изображению. Созданное описание и ключевые слова хранятся в метаданных большого двоичного объекта, чтобы можно было получить их позже.

    ```csharp
    // Submit the image to Azure's Computer Vision API
    ComputerVisionClient vision = new ComputerVisionClient(
        new ApiKeyServiceClientCredentials(ConfigurationManager.AppSettings["SubscriptionKey"]),
        new System.Net.Http.DelegatingHandler[] { });
    vision.Endpoint = ConfigurationManager.AppSettings["VisionEndpoint"];

    VisualFeatureTypes[] features = new VisualFeatureTypes[] { VisualFeatureTypes.Description };
    var result = await vision.AnalyzeImageAsync(photo.Uri.ToString(), features);

    // Record the image description and tags in blob metadata
    photo.Metadata.Add("Caption", result.Description.Captions[0].Text);

    for (int i = 0; i < result.Description.Tags.Count; i++)
    {
        string key = String.Format("Tag{0}", i);
        photo.Metadata.Add(key, result.Description.Tags[i]);
    }

    await photo.SetMetadataAsync();
    ```

1. Перейдите к методу **Index** в том же файле. Этот метод перечисляет сохраненные большие двоичные объекты с изображениями в целевом контейнере больших двоичных объектов (как экземпляры **IListBlobItem**) и передает их в представление приложений. Замените блок `foreach` в этом методе следующим кодом. Этот код вызывает **CloudBlockBlob.FetchAttributes** для получения присоединенных метаданных каждого большого двоичного объекта. Извлекает описание, созданное компьютером, (`caption`) из метаданных и добавляет его к объекту **BlobInfo**, который передается в представление.
    
    ```csharp
    foreach (IListBlobItem item in container.ListBlobs())
    {
        var blob = item as CloudBlockBlob;
    
        if (blob != null)
        {
            blob.FetchAttributes(); // Get blob metadata
            var caption = blob.Metadata.ContainsKey("Caption") ? blob.Metadata["Caption"] : blob.Name;
    
            blobs.Add(new BlobInfo()
            {
                ImageUri = blob.Uri.ToString(),
                ThumbnailUri = blob.Uri.ToString().Replace("/photos/", "/thumbnails/"),
                Caption = caption
            });
        }
    }
    ```

## <a name="test-the-app"></a>Тестирование приложения

Сохраните изменения в Visual Studio и нажмите клавиши **CTRL+F5**, чтобы запустить приложение в браузере. Используйте приложение для отправки нескольких изображений: либо из папки photos в ресурсах лаборатории, либо из собственной папки. При наведении указателя мыши на одно из изображений в представлении появится окно всплывающей подсказки с отображением созданного компьютером заголовка этого изображения.

![Заголовок, созданный компьютером](../Images/thumbnail-with-tooltip.png)

Чтобы просмотреть все присоединенные метаданные, в Обозревателе службы хранилища Azure просмотрите контейнер хранилища, в котором вы храните изображения. Щелкните правой кнопкой мыши любой большой двоичный объект в контейнере и выберите пункт **Свойства**. В диалоговом окне вы увидите список пар "ключ — значение". Созданное компьютером описание изображений хранится в элементе Caption, а ключевые слова для поиска хранятся в тегах Tag0, Tag1 и т. д. Завершив операцию, щелкните **Отмена**, чтобы закрыть диалоговое окно.

![Диалоговое окно "Свойства изображения" со списком тегов метаданных](../Images/blob-metadata.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы хотите продолжать работать с веб-приложением, перейдите к разделу [Дополнительная информация](#next-steps). Если вы не планируете продолжать использование этого приложения, вам следует удалить все ресурсы, связанные с приложением. Чтобы удалить ресурсы, вы можете удалить группу ресурсов, содержащую вашу подписку службы хранилища Azure и ресурс API компьютерного зрения. Это приведет к удалению учетной записи хранения, отправленных в нее больших двоичных объектов и ресурса Службы приложений, необходимого для подключения к веб-приложению ASP.NET. 

Чтобы удалить группу ресурсов, откройте вкладку **Группы ресурсов** на портале, перейдите к группе ресурсов, используемой для данного проекта, и щелкните **Удалить группу ресурсов** в верхней части представления. Вам будет предложено ввести имя группы ресурсов, чтобы подтвердить, что вы хотите ее удалить, так как после удаления восстановить группу ресурсов будет невозможно.

## <a name="next-steps"></a>Дальнейшие действия

Работая с этим учебником, вы настроили службу "Компьютерное зрение Azure" в существующем веб-приложении для автоматического создания заголовков и ключевых слов для изображений, хранящихся в больших двоичных объектах, во время их отправки. Затем перейдите к лаборатории службы хранилища Azure, упражнению 6, чтобы узнать, как добавлять функции поиска в веб-приложение. В этих функциях используются ключевые слова для поиска, создаваемые службой API компьютерного зрения.

> [!div class="nextstepaction"]
> [Добавление поиска в приложение](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md#Exercise6)
