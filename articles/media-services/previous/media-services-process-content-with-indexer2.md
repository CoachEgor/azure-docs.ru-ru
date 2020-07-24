---
title: Индексирование файлов мультимедиа с помощью Azure Media Indexer 2 (предварительная версия) | Документация Майкрософт
description: Индексатор мультимедийных данных Azure позволяет искать содержимое файлов мультимедиа и создавать полнотекстовую транскрипцию для скрытых субтитров и ключевых слов. В этом разделе показано, как использовать индексатор мультимедийных данных 2 (предварительная версия).
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/22/2019
ms.author: juliako
ms.reviewer: adsolank
ms.openlocfilehash: d4589e2e923bf4068cc3a707c69f0304a1539745
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "87011555"
---
# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Индексирование файлов мультимедиа с помощью индексатора мультимедийных данных Azure 2 (предварительная версия)

> [!NOTE]
> Процессор мультимедиа **Azure Media indexer 2** будет снят с учета. Даты прекращения поддержки см. в разделе, посвященном [устаревшим компонентам](legacy-components.md). [Индексатор видео служб мультимедиа Azure](../video-indexer/index.yml) заменяет этот устаревший обработчик мультимедиа. Дополнительные сведения см. в статье [Переход с Azure Media Indexer и Azure Media Indexer 2 на Индексатор видео Служб мультимедиа Azure](migrate-indexer-v1-v2.md).

Обработчик мультимедиа **индексатор мультимедийных данных Azure 2 (предварительная версия)** позволяет сделать мультимедийные файлы и содержимое доступными для поиска, а также создавать дорожки для субтитров. По сравнению с предыдущей версией [индексатора мультимедийных данных](media-services-index-content.md), **индексатор мультимедийных данных Azure 2 (предварительная версия)** быстрее выполняет индексирование и предлагает более широкую поддержку языков. В число поддерживаемых языков входят английский, испанский, французский, немецкий, итальянский, китайский (мандаринский диалект, упрощенное письмо), португальский, арабский, русский и японский.

Сейчас обработчик мультимедиа **индексатор мультимедийных данных Azure 2 (предварительная версия)** доступен в предварительной версии.

В этой статье показано, как создавать задания индексирования с помощью **индексатора мультимедийных данных Azure 2 (предварительная версия)**.

## <a name="considerations"></a>Рекомендации

Действительны следующие условия.
 
* Индексатор 2 не поддерживается в Azure Китая 21Vianet и Azure для государственных организаций.
* При индексировании содержимого обязательно используйте файлы мультимедиа, содержащие отчетливую речь (без фоновой музыки, шума, эффектов или шипения микрофона). Примерами подходящего содержимого могут служить записи собраний, лекций или презентаций. Для индексирования, как правило, не подходит такое содержимое, как фильмы, телепередачи, любые материалы со смешанными аудио- и звуковыми эффектами, записи плохого качества с фоновым шумом (шипением).
 
## <a name="input-and-output-files"></a>Входные и выходные файлы
### <a name="input-files"></a>Входные файлы
Аудио- или видеофайлы

### <a name="output-files"></a>выходные файлы
Задание индексирования может создавать файлы скрытых субтитров в следующих форматах:  

* **TTML**
* **WebVTT**

С помощью файлов скрытых субтитров в этих форматах аудио- и видеофайлы можно сделать доступным для людей с нарушениями слуха.

## <a name="task-configuration-preset"></a>Конфигурация задачи (предустановка)
При создании задачи индексирования с помощью **предварительной версии индексатора мультимедийных данных Azure 2**необходимо указать предустановку конфигурации.

Следующий объект JSON задает доступные параметры.

```json
    {
      "version":"1.0",
      "Features":
        [
           {
           "Options": {
                "Formats":["WebVtt","ttml"],
                "Language":"enUs",
                "Type":"RecoOptions"
           },
           "Type":"SpReco"
        }]
    }
```

## <a name="supported-languages"></a>Поддерживаемые языки
Индексатор мультимедийных данных Azure 2 (предварительная версия) поддерживает распознавание речи для следующих языков (при указании названия языка в файле конфигурации задачи используйте 4-значный код в квадратных скобках, как показано ниже):

* английский [EnUs];
* испанский [EsEs];
* китайский (мандаринский диалект, упрощенное письмо) [ZhCn];
* французский [FrFr];
* немецкий [DeDe];
* итальянский [ItIt];
* португальский [PtBr];
* арабский (Египет) [ArEg].
* японский [JaJp].
* русский [RuRu];
* британский английский [EnGb];
* Испанский (Мексика) [Есмкс] 

## <a name="supported-file-types"></a>Поддерживаемые типы файлов

Дополнительные сведения о поддерживаемых типах файлов см. в разделе [Контейнер ввода/ форматы файлов](media-services-media-encoder-standard-formats.md#input-containerfile-formats).

## <a name="net-sample-code"></a>Пример кода .NET

В следующей программе показано, как выполнить следующие задачи.

1. Создать ресурс-контейнера и отправить в него файл мультимедиа.
2. Создайте задание с задачей индексации на основе файла конфигурации, содержащего следующую предустановку JSON:

    ```json
            {
            "version":"1.0",
            "Features":
                [
                {
                "Options": {
                        "Formats":["WebVtt","ttml"],
                        "Language":"enUs",
                        "Type":"RecoOptions"
                },
                "Type":"SpReco"
                }]
            }
    ```
    
3. Скачивание выходных файлов. 
   
#### <a name="create-and-configure-a-visual-studio-project"></a>Создание и настройка проекта Visual Studio

Настройте среду разработки и заполните файл app.config данными о соединении, как описано в разделе [Разработка служб мультимедиа с помощью .NET](media-services-dotnet-how-to-use.md). 

#### <a name="example"></a>Пример

```csharp
using System;
using System.Configuration;
using System.IO;
using System.Linq;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.Threading;
using System.Threading.Tasks;

namespace IndexContent
{
    class Program
    {
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AMSAADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
        private static readonly string _AMSClientId =
            ConfigurationManager.AppSettings["AMSClientId"];
        private static readonly string _AMSClientSecret =
            ConfigurationManager.AppSettings["AMSClientSecret"];

        // Field for service context.
        private static CloudMediaContext _context = null;

        static void Main(string[] args)
        {
            AzureAdTokenCredentials tokenCredentials =
                new AzureAdTokenCredentials(_AADTenantDomain,
                    new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                    AzureEnvironments.AzureCloudEnvironment);

            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            // Run indexing job.
            var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
                                        @"C:\supportFiles\Indexer\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
        }

        static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                "My Indexing Input Asset",
                AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Indexing Job");

            // Get a reference to Azure Media Indexer 2 Preview.
            string MediaProcessorName = "Azure Media Indexer 2 Preview";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Indexing Task",
                processor,
                configuration,
                TaskOptions.None);

            // Specify the input asset to be indexed.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

            // Use the following event handler to check job progress.  
            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

            // Launch the job.
            job.Submit();

            // Check job execution and wait for job to finish.
            Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);

            progressJobTask.Wait();

            // If job state is Error, the event handling
            // method for job progress should log errors.  Here we check
            // for error state and exit if needed.
            if (job.State == JobState.Error)
            {
                ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                Console.WriteLine(string.Format("Error: {0}. {1}",
                                                error.Code,
                                                error.Message));
                return null;
            }

            return job.OutputMediaAssets[0];
        }

        static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
        {
            IAsset asset = _context.Assets.Create(assetName, options);

            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);

            return asset;
        }

        static void DownloadAsset(IAsset asset, string outputDirectory)
        {
            foreach (IAssetFile file in asset.AssetFiles)
            {
                file.Download(Path.Combine(outputDirectory, file.Name));
            }
        }

        static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors
                .Where(p => p.Name == mediaProcessorName)
                .ToList()
                .OrderBy(p => new Version(p.Version))
                .LastOrDefault();

            if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor",
                                                           mediaProcessorName));

            return processor;
        }

        static private void StateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);

            switch (e.CurrentState)
            {
                case JobState.Finished:
                    Console.WriteLine();
                    Console.WriteLine("Job is finished.");
                    Console.WriteLine();
                    break;
                case JobState.Canceling:
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    Console.WriteLine("Please wait...\n");
                    break;
                case JobState.Canceled:
                case JobState.Error:
                    // Cast sender as a job.
                    IJob job = (IJob)sender;
                    // Display or log error details as needed.
                    // LogJobStop(job.Id);
                    break;
                default:
                    break;
            }
        }
    }
}
```

## <a name="media-services-learning-paths"></a>Схемы обучения работе со службами мультимедиа
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Отзывы
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Связанные ссылки
[Общие сведения об аналитике служб мультимедиа Azure](media-services-analytics-overview.md)

[Демонстрационные материалы для медиааналитики Azure](https://azuremedialabs.azurewebsites.net/demos/Analytics.html)
