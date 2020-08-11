---
title: Запись разговора в реальном времени (Предварительная версия) — служба речи
titleSuffix: Azure Cognitive Services
description: Узнайте, как использовать транскрипцию разговора в режиме реального времени с помощью речевого пакета SDK. Доступно для C++, C# и Java.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: weixu
ms.openlocfilehash: f9670733f8aa040b87cec20ac28aacc636c04698
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/10/2020
ms.locfileid: "88056216"
---
# <a name="real-time-conversation-transcription-preview"></a>Запись разговора в реальном времени (Предварительная версия)

API **конверсатионтранскрибер** для речевых средств позволяет транскрипция встречи и другие беседы с возможностью добавления, удаления и обнаружения нескольких участников путем потоковой передачи в службу распознавания речи с помощью `PullStream` или `PushStream` . В этом разделе вы узнаете, как использовать речь в тексте с пакетом SDK для распознавания речи (версия 1.8.0 или более поздняя). Дополнительные сведения см. в разделе [что такое речевые службы](overview.md).

## <a name="limitations"></a>Ограничения

- API Конверсатионтранскрибер поддерживается для C++, C# и Java в Windows, Linux и Android.
- В настоящее время доступны в языках "en-US" и "zh-CN" в следующих регионах: _centralus_ и _eastasia_.
- Требуется 7-MIC циклический массив из нескольких микрофонов с потоком ссылки для воспроизведения. Массив микрофонов должен соответствовать [нашей спецификации](https://aka.ms/sdsdk-microphone).
- [Пакет SDK для речевых устройств](speech-devices-sdk.md) предоставляет подходящие устройства и пример приложения, демонстрирующий запись разговора.

## <a name="optional-sample-code-resources"></a>Необязательный образец ресурсов кода

Пакет SDK для речевых устройств предоставляет пример кода в Java для записи звука в реальном времени с помощью 8 каналов.

- [Пример кода устройства РУБО](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK/blob/master/Samples/Java/Android/Speech%20Devices%20SDK%20Starter%20App/example/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/sdsdkstarterapp/ConversationTranscription.java)
- [Пример кода для пакета Azure Kinect Dev Kit](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK/blob/master/Samples/Java/Windows_Linux/SampleDemo/src/com/microsoft/cognitiveservices/speech/samples/Cts.java)

## <a name="prerequisites"></a>Обязательные условия

Подписка на службу распознавания речи. Если у вас ее нет, можно создать [бесплатную учетную запись Azure](https://azure.microsoft.com/free/cognitive-services/) .

## <a name="create-voice-signatures"></a>Создание подписок

Первым шагом является создание голосовых подписей для участников диалога для эффективной идентификации докладчика.

### <a name="audio-input-requirements"></a>Требования к входным аудио

- Входной звуковой WAV-файл для создания голосовых подписей должен быть в 16-разрядных примерах, частоте выборки 16 кГц и одном канале (Mono).
- Рекомендуемая длина для каждого звукового образца — от тридцати секунд до двух минут.

### <a name="sample-code"></a>Образец кода

В следующем примере показаны два разных способа создания цифровой подписи [с помощью REST API](https://aka.ms/cts/signaturegenservice) в C#. Обратите внимание, что необходимо заменить реальную информацию на "Йоурсубскриптионкэй", имя волнового файла для "Спеакервоице. wav", а также регион для `{region}` и "йоурсервицерегион" (_centralus_ или _eastasia_).

```csharp
class Program
{
    static async Task CreateVoiceSignatureByUsingFormData()
    {
        // Replace with your own region
        var region = "YourServiceRegion";
        // Change the name of the wave file to match yours
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var form = new MultipartFormDataContent();
        var content = new ByteArrayContent(fileBytes);
        form.Add(content, "file", "file");
        var client = new HttpClient();
        // Add your subscription key to the header Ocp-Apim-Subscription-Key directly
        // Replace "YourSubscriptionKey" with your own subscription key
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        // Edit with your desired region for `{region}`
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromFormData", form);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric string or integer value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static async Task CreateVoiceSignatureByUsingBody()
    {
        // Replace with your own region
        var region = "YourServiceRegion";
        // Change the name of the wave file to match yours
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var content = new ByteArrayContent(fileBytes);

        var client = new HttpClient();
        // Add your subscription key to the header Ocp-Apim-Subscription-Key directly
        // Replace "YourSubscriptionKey" with your own subscription key
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        // Edit with your desired region for `{region}`
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromByteArray", content);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric string or integer value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static void Main(string[] args)
    {
        CreateVoiceSignatureByUsingFormData().Wait();
        CreateVoiceSignatureByUsingBody().Wait();
    }
}
```

## <a name="transcribe-conversations"></a>Транскрипция беседы

В следующем примере кода показано, как транскрипция беседы в режиме реального времени для трех динамиков. Предполагается, что для каждого динамика уже созданы голосовые подписи, как показано выше. При создании объекта Спичконфиг замените реальную информацию на "Йоурсубскриптионкэй" и "Йоурсервицерегион".

Примеры кода:

- Создание `Conversation` объекта из `SpeechConfig` объекта с помощью идентификатора собрания, созданного с помощью`Guid.NewGuid()`
- Создание `ConversationTranscriber` объекта и присоединение к беседе с `JoinConversationAsync()` целью запуска транскрипции
- Регистрация интересующих событий
- Добавление или удаление участников диалога с помощью объекта CONVERSATION
- Потоковая передача звука
- В речевом пакете SDK версии 1.9.0 и более поздних версий `int` `string` поддерживаются в поле версия голосовой подписи.

Транскрипция и идентификатор динамика возвращаются в зарегистрированные события.

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;
using Microsoft.CognitiveServices.Speech.Transcription;

public class MyConversationTranscriber
{
    public static async Task ConversationWithPullAudioStreamAsync()
    {
        // Creates an instance of a speech config with specified subscription key and service region
        // Replace with your own subscription key and region
        var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
        config.SetProperty("ConversationTranscriptionInRoomAndOnline", "true");
        var stopTranscription = new TaskCompletionSource<int>();

        // Create an audio stream from a wav file or from the default microphone if you want to stream live audio from the supported devices
        // Replace with your own audio file name and Helper class which implements AudioConfig using PullAudioInputStreamCallback
        using (var audioInput = Helper.OpenWavFile(@"8channelsOfRecordedPCMAudio.wav"))
        {
            var meetingId = Guid.NewGuid().ToString();
            using (var conversation = await Conversation.CreateConversationAsync(config, meetingId).ConfigureAwait(false))
            {
                // Create a conversation transcriber using audio stream input
                using (var conversationTranscriber = new ConversationTranscriber(audioInput))
                {
                    await conversationTranscriber.JoinConversationAsync(conversation);

                    // Subscribe to events
                    conversationTranscriber.Transcribing += (s, e) =>
                    {
                            Console.WriteLine($"TRANSCRIBING: Text={e.Result.Text}");
                    };

                    conversationTranscriber.Transcribed += (s, e) =>
                    {
                        if (e.Result.Reason == ResultReason.RecognizedSpeech)
                        {
                            Console.WriteLine($"TRANSCRIBED: Text={e.Result.Text}, UserID={e.Result.UserId}");
                        }
                        else if (e.Result.Reason == ResultReason.NoMatch)
                        {
                            Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                        }
                    };

                    conversationTranscriber.Canceled += (s, e) =>
                    {
                        Console.WriteLine($"CANCELED: Reason={e.Reason}");

                        if (e.Reason == CancellationReason.Error)
                        {
                            Console.WriteLine($"CANCELED: ErrorCode={e.ErrorCode}");
                            Console.WriteLine($"CANCELED: ErrorDetails={e.ErrorDetails}");
                            Console.WriteLine($"CANCELED: Did you update the subscription info?");
                            stopTranscription.TrySetResult(0);
                        }
                    };

                    conversationTranscriber.SessionStarted += (s, e) =>
                    {
                        Console.WriteLine("\nSession started event.");
                    };

                    conversationTranscriber.SessionStopped += (s, e) =>
                    {
                        Console.WriteLine("\nSession stopped event.");
                        Console.WriteLine("\nStop recognition.");
                        stopTranscription.TrySetResult(0);
                    };

                    // Add participants to the conversation.
                    // Create voice signatures using REST API described in the earlier section in this document.
                    // Voice signature needs to be in the following format:
                    // { "Version": <Numeric string or integer value>, "Tag": "string", "Data": "string" }

                    var speakerA = Participant.From("Speaker_A", "en-us", signatureA);
                    var speakerB = Participant.From("Speaker_B", "en-us", signatureB);
                    var speakerC = Participant.From("SPeaker_C", "en-us", signatureC);
                    await conversation.AddParticipantAsync(speakerA);
                    await conversation.AddParticipantAsync(speakerB);
                    await conversation.AddParticipantAsync(speakerC);

                    // Starts transcribing of the conversation. Uses StopTranscribingAsync() to stop transcribing when all participants leave.
                    await conversationTranscriber.StartTranscribingAsync().ConfigureAwait(false);

                    // Waits for completion.
                    // Use Task.WaitAny to keep the task rooted.
                    Task.WaitAny(new[] { stopTranscription.Task });

                    // Stop transcribing the conversation.
                    await conversationTranscriber.StopTranscribingAsync().ConfigureAwait(false);
                 }
            }
       }
    }
}
```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Асинхронная транскрипция диалога](how-to-async-conversation-transcription.md)
