---
title: Краткое руководство. Преобразование речи в текст с помощью C# (.NET Core в Windows) в службе "Речь"
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/04/2020
ms.author: erhopf
ms.openlocfilehash: a5ca40c9f0d39159c5d5a174fc16f9bc6d9b8bcf
ms.sourcegitcommit: 152c522bb5ad64e5c020b466b239cdac040b9377
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/14/2020
ms.locfileid: "88226438"
---
## <a name="prerequisites"></a>Предварительные требования

Перед началом работы нужно сделать следующее:

> [!div class="checklist"]
> * [Создать ресурс службы "Речь" Azure.](../../../../get-started.md)
> * [Настройте среду разработки и создайте пустой проект](../../../../quickstarts/setup-platform.md?tabs=dotnetcore&pivots=programming-language-csharp).

## <a name="add-sample-code"></a>Добавление примеров кода

1. Откройте файл **Program.cs** и замените имеющийся код приведенным ниже кодом.

   ```Csharp
   using System;
   using System.Threading.Tasks;
   using Microsoft.CognitiveServices.Speech;
   using Microsoft.CognitiveServices.Speech.Translation;

   namespace helloworld
   {
       class Program
       {
           public static async Task TranslateSpeechToText()
           {
               // Creates an instance of a speech translation config with specified subscription key and service region.
               // Replace with your own subscription key and region identifier from here: https://aka.ms/speech/sdkregion
               var config = SpeechTranslationConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

               // Sets source and target languages.
               // Replace with the languages of your choice, from list found here: https://aka.ms/speech/sttt-languages
               string fromLanguage = "en-US";
               string toLanguage = "de";
               config.SpeechRecognitionLanguage = fromLanguage;
               config.AddTargetLanguage(toLanguage);

               // Creates a translation recognizer using the default microphone audio input device.
               using (var recognizer = new TranslationRecognizer(config))
               {
                   // Starts translation, and returns after a single utterance is recognized. The end of a
                   // single utterance is determined by listening for silence at the end or until a maximum of 15
                   // seconds of audio is processed. The task returns the recognized text as well as the translation.
                   // Note: Since RecognizeOnceAsync() returns only a single utterance, it is suitable only for single
                   // shot recognition like command or query.
                   // For long-running multi-utterance recognition, use StartContinuousRecognitionAsync() instead.
                   Console.WriteLine("Say something...");
                   var result = await recognizer.RecognizeOnceAsync();

                   // Checks result.
                   if (result.Reason == ResultReason.TranslatedSpeech)
                   {
                       Console.WriteLine($"RECOGNIZED '{fromLanguage}': {result.Text}");
                       Console.WriteLine($"TRANSLATED into '{toLanguage}': {result.Translations[toLanguage]}");
                   }
                   else if (result.Reason == ResultReason.RecognizedSpeech)
                   {
                       Console.WriteLine($"RECOGNIZED '{fromLanguage}': {result.Text} (text could not be translated)");
                   }
                   else if (result.Reason == ResultReason.NoMatch)
                   {
                       Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                   }
                   else if (result.Reason == ResultReason.Canceled)
                   {
                       var cancellation = CancellationDetails.FromResult(result);
                       Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");

                       if (cancellation.Reason == CancellationReason.Error)
                       {
                           Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
                           Console.WriteLine($"CANCELED: ErrorDetails={cancellation.ErrorDetails}");
                           Console.WriteLine($"CANCELED: Did you update the subscription info?");
                       }
                   }
               }
           }

           static void Main(string[] args)
           {
               TranslateSpeechToText().Wait();
           }
       }
   }
   ```

1. В том же файле замените строку `YourSubscriptionKey` своим ключом подписки.

1. Строку `YourServiceRegion` замените [регионом](~/articles/cognitive-services/Speech-Service/regions.md).

1. В строке меню выберите **Файл** > **Сохранить все**.

## <a name="build-and-run-the-application"></a>Создание и запуск приложения

1. В строке меню выберите **Сборка** > **Построить решение**, чтобы создать приложение. Теперь код должен компилироваться без ошибок.

1. Выберите **Отладка** > **Начать отладку** (или нажмите клавишу **F5**), чтобы запустить приложение **helloworld**.

1. Произнесите фразу или предложение на английском языке. Приложение передает речь в службу "Речь", которая выполняет перевод и транскрибирование в текст (в нашем примере на немецкий язык). Затем служба "Речь" отправляет текст обратно в приложение для отображения.

````
Say something...
RECOGNIZED 'en-US': What's the weather in Seattle?
TRANSLATED into 'de': Wie ist das Wetter in Seattle?
````

## <a name="next-steps"></a>Дальнейшие действия

[!INCLUDE [footer](./footer.md)]
