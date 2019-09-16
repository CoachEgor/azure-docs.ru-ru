---
title: Начало работы с API распознавания речи Microsoft с помощью библиотек служб С# | Документация Майкрософт
titlesuffix: Azure Cognitive Services
description: Используйте библиотеку службы "Распознавание речи Bing" для преобразования устной речи в текст.
services: cognitive-services
author: nitinme
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: nitinme
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 71c3e471a8844eb6c6b70921e40c94338a084a8b
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2019
ms.locfileid: "70965859"
---
# <a name="quickstart-use-the-bing-speech-recognition-service-library-in-c35-for-net-windows"></a>Краткое руководство. Использование библиотеки службы "Распознавание речи Bing" в C&#35; для .NET на платформе Windows

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Библиотека службы предназначена для разработчиков, у которых есть своя облачная служба и которым нужно вызывать из нее службу распознавания речи. Если вам нужно вызывать службу распознавания речи из приложений, связанных с устройством, не используйте этот пакет SDK. (Для этого используйте другие клиентские библиотеки или REST API.)

Чтобы использовать библиотеку службы C#, установите пакет [NuGet Microsoft.Bing.Speech](https://www.nuget.org/packages/Microsoft.Bing.Speech/). Сведения о API библиотеки см. в разделе [Библиотека службы распознавания речи C# Microsoft](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html).

В следующих разделах описывается, как устанавливать, создавать и запускать пример приложения C# с помощью библиотеки службы C#.

## <a name="prerequisites"></a>Предварительные требования

### <a name="platform-requirements"></a>Требования платформы

Следующий пример был разработан для Windows 8+ и .NET 4.5+ Framework с помощью [Visual Studio 2015, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs).

### <a name="get-the-sample-application"></a>Получите пример приложения

Скопируйте образец из репозитория [образцов библиотеки службы распознавания речи C#](https://github.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary).

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Подпишитесь на API распознавания речи и получите ключ бесплатной пробной версии подписки

Speech API — элемент Cognitive Services (ранее Project Oxford). Вы можете получить ключи бесплатной пробной версии подписки на странице [подписки на Cognitive Services](https://azure.microsoft.com/try/cognitive-services/). Чтобы получить ключ, после выбора SAPI выберите **Получить API-ключ**. Страница возвращает первичный и вторичный ключ. Оба ключа привязаны к одной и той же квоте, поэтому вы можете использовать любой из них.

> [!IMPORTANT]
> * Получить ключ подписки. Прежде чем использовать клиентские библиотеки для работы с речью, вам необходимо получить [ключ подписки](https://azure.microsoft.com/try/cognitive-services/).
>
> * Используйте свой ключ подписки С предоставленным примером приложения библиотеки службы C # вам необходимо предоставить ваш ключ подписки в качестве одного из параметров командной строки. Для дополнительных сведений см. [Запустите образец приложения](#step-3-run-the-sample-application).

## <a name="step-1-install-the-sample-application"></a>Шаг 1. Установите пример приложения

1. Запустите Visual Studio 2015 и выберите **Файл** > **Открыть** > **Project/Solution** (Проект/решение).

2. Чтобы открыть SLN-файл Visual Studio 2015 Solution SpeechClient.sln, дважды щелкните его. Решение откроется в Visual Studio.

## <a name="step-2-build-the-sample-application"></a>Шаг 2. Создайте пример приложения.

Нажмите клавиши CTRL+SHIFT+B или выберите действие **Cобрать** в меню ленты. Затем выберите **Собрать решение**.

## <a name="step-3-run-the-sample-application"></a>Шаг 3. Запуск примера приложения

1. После завершения сборки нажмите клавишу F5 или щелкните **Запустить** в меню ленты, чтобы выполнить созданный пример.

2. Откройте каталог выходных данных для образца, например SpeechClientSample\bin\Debug. Нажмите клавиши Shift + стрелка вправо и выберите **Open command window here** (Открыть окно команд здесь).

3. Выполните команду `SpeechClientSample.exe` со следующими аргументами:

   * Arg[0]: укажите входной WAV-файл аудиозаписи.
   * Arg[1]: укажите языковой стандарт аудиозаписи.
   * Arg[2]: укажите режимы распознавания. *Короткий* для режима `ShortPhrase` и *Длинный* для режима `LongDictation`.
   * Arg[3]: укажите ключ подписки для доступа к службе распознавания речи.

## <a name="samples-explained"></a>Описание образцов

### <a name="recognition-modes"></a>Режимы распознавания

* Режим `ShortPhrase`: речевой фрагмент продолжительностью до 15 секунд. Когда данные отправляются на сервер, клиент получает несколько частичных результатов и один конечный самый лучший результат.
* Режим `LongDictation`: речевой фрагмент не длиннее 10 минут. После отправки данных на сервер клиент получит несколько промежуточных и несколько конечных результатов в зависимости от того, как сервер разбивает предложение.

### <a name="supported-audio-formats"></a>Поддерживаемые аудиоформаты

API распознавания речи поддерживает аудио/WAV-файлы, используя следующие кодеки:

* Одноканальный, PCM
* Siren
* SirenSR

### <a name="preferences"></a>Preferences

Чтобы создать SpeechClient, необходимо сначала создать объект Preferences. Объект Preferences — это набор параметров, предназначенных для настройки поведения службы распознавания речи. Он состоит из таких полей:

* `SpeechLanguage`. языковой стандарт аудиозаписи, отправленный в службу распознавания речи.
* `ServiceUri`. конечная точка, используемая для вызова службы распознавания речи.
* `AuthorizationProvider`. реализация IAuthorizationProvider, которую используют для получения токенов и доступа к службе распознавания речи. Хотя этот образец предоставляет поставщик авторизации Cognitive Services, мы настоятельно рекомендуем создать собственную реализацию, чтобы осуществить кэширование маркеров.
* `EnableAudioBuffering`. Дополнительный параметр См. [Управление соединениями](#connection-management).

### <a name="speech-input"></a>Ввод речи

Объект SpeechInput состоит из двух полей:

* **Звук:** реализация потока по вашему выбору, из которого пакет SDK получает аудиозапись. Это может быть любой [поток](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), что поддерживает чтение.
   > [!NOTE]
   > Пакет SDK обнаруживает конец потока при возврате потока к **0** при чтении.

* **RequestMetadata**: метаданные о запросе речи. Дополнительные сведения см. по [ссылке](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html).

### <a name="speech-request"></a>Запрос речи

После того как вы создали объекты SpeechClient и SpeechInput, используйте RecognizeAsync, чтобы сделать запрос к Speech Service.

```cs
    var task = speechClient.RecognizeAsync(speechInput);
```

По завершении запроса возвращается также и задача RecognizeAsync. Последний RecognitionResult (результат распознавания) – это конец распознавания. Задача может завершиться ошибкой, если пакет SDK неожиданно выдаст ошибку.

### <a name="speech-recognition-events"></a>События в распознавании речи

#### <a name="partial-results-event"></a>Событие с частичными результатами

Это событие вызывается каждый раз, когда Speech Service предсказывает, что вы можете сказать, даже до того, как вы закончите разговор (если вы используете `MicrophoneRecognitionClient`) или закончите отправку данных (если вы используете `DataRecognitionClient`). Можете подписаться на событие с помощью `SpeechClient.SubscribeToPartialResult()`. Также вы можете использовать метод `SpeechClient.SubscribeTo<RecognitionPartialResult>()` для подписки на универсальные события.

**Return format** | Описание |
------|------
**LexicalForm** | Эта форма оптимальна для использования приложениями, которые нуждаются в необработанных результатах распознавания речи.
**DisplayText** | Распознанная фраза с применением нормализации обратного текста, регистра букв, знаков пунктуации и со скрытой ненормативной лексикой. Ненормативную лексику скрыто звездочками, поставленными после первой буквы слова, например "х***". Эта форма оптимальна для использования приложениями, которые отображают пользователю результаты распознавания речи.
**Confidence** | Уровень достоверности распознанной фразы представляет собой связанный звук, определенный сервером распознавания речи.
**MediaTime** | Текущее время относительно начала звукового потока (в 100-наносекундных единицах времени).
**MediaDuration** | Длительность/длина текущей фразы относительно аудиосегмента (по 100 нс).

#### <a name="result-event"></a>Result event
После завершения разговора (в режиме `ShortPhrase`) это событие вызывается. Вам предоставлен список лучших вариантов для результата. В режиме `LongDictation` это событие может вызываться несколько раз в зависимости от того, где сервер указывает на приостановку предложения. Можете подписаться на событие с помощью `SpeechClient.SubscribeToRecognitionResult()`. Также вы можете использовать метод `SpeechClient.SubscribeTo<RecognitionResult>()` для подписки на универсальные события.

**Return format** | Описание |
------|------|
**RecognitionStatus** | Состояние о том, как было получено распознавание. Например, был ли он создан в результате успешного распознавания или в результате отмены подключения, и т. д.
**Phrases** | Набор наиболее распознаваемых фраз с достоверностью распознавания.

Для получения дополнительных сведений см. [Выходной формат](../Concepts.md#output-format).

### <a name="speech-recognition-response"></a>Ответ распознавания речи

Пример ответа речи:
```
--- Partial result received by OnPartialResult  
---what  
--- Partial result received by OnPartialResult  
--what's  
--- Partial result received by OnPartialResult  
---what's the web  
--- Partial result received by OnPartialResult   
---what's the weather like  
---***** Phrase Recognition Status = [Success]   
***What's the weather like? (Confidence:High)  
What's the weather like? (Confidence:High)
```

## <a name="connection-management"></a>Управление соединениями

API использует одно соединение WebSocket каждого запроса. Для обеспечения оптимального пользовательского опыта пакет SDK пытается повторно подключиться к Speech Service и начать распознавание с последнего полученного RecognitionResult. Например, если запрос на аудио длится две минуты, пакет SDK получил признак распознавания на отметке одна минута, а после пяти секунд произошел сбой сети, то пакет SDK запускает новое соединение, которое начинается с отметки в одну минуту.

>[!NOTE]
>Пакет SDK не обращается к отметке в одну минуту, потому что поток может не поддерживать обращение. Вместо этого, пакет SDK хранит внутренний буфер, который он использует для буферизации звука и очищает его, когда он принимает события RecognitionResult.

## <a name="buffering-behavior"></a>Режим буферизации

По умолчанию SDK буферизует звук, чтобы он мог восстанавливаться при возникновении прерывания сети. В сценарии, где предпочтительнее отменить звук, потерянный во время отключения сети, и перезапускать соединение, лучше отключить буферизацию звука, установив `EnableAudioBuffering` в объекте Preferences (предпочтения) на `false`.

## <a name="related-topics"></a>См. также

[Справка о библиотеке службы распознавания речи C# Microsoft](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html)
