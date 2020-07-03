---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: 513de6d990884f9abf2378ea208ec1dbe556d397
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83587190"
---
[!INCLUDE [Prerequisites](prerequisites-csharp.md)]

[!INCLUDE [Setup and use environment variables](setup-env-variables.md)]

## <a name="create-a-net-core-project"></a>Создание проекта .NET Core

Откройте командную строку или сеанс терминала и выполните следующие команды:

```console
dotnet new console -o detect-sample
cd detect-sample
```

С помощью первой команды создается консольное приложение .NET и каталог с именем `detect-sample`. Вторая команда позволяет изменить каталог для проекта.

Вам нужно будет установить модуль вручную. В каталоге проекта выполните следующую команду:

```console
dotnet add package Newtonsoft.Json --version 11.0.2
```

## <a name="select-the-c-language-version"></a>Выбор версии языка C#

Для работы с этим кратким руководством потребуется C# версии 7.1 или более поздней. Есть несколько способов изменить версию C# для проекта. В этом руководстве мы покажем, как настроить файл `detect-sample.csproj`. Все доступные варианты, в том числе смена языка через Visual Studio, описаны в статье [Выбор версии языка C#](https://docs.microsoft.com/dotnet/csharp/language-reference/configure-language-version).

Откройте проект, а затем откройте `detect-sample.csproj`. Убедитесь, что для `LangVersion` установлено значение 7.1 или более высокое. Если для нужной версии языка не существует группы свойств, добавьте следующие строки:

```xml
<PropertyGroup>
   <LangVersion>7.1</LangVersion>
</PropertyGroup>
```

## <a name="add-required-namespaces-to-your-project"></a>Добавление обязательных пространств имен в проект

С помощью команды `dotnet new console`, которую вы выполнили ранее, был создан проект, включая `Program.cs`. Этот файл находится там, где будет размещаться код приложения. Откройте файл `Program.cs` и замените существующие инструкции using. Эти инструкции обеспечивают доступ ко всем типам, требуемым для создания и запуска примера приложения.

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
// Install Newtonsoft.Json with NuGet
using Newtonsoft.Json;
```

## <a name="create-classes-for-the-json-response"></a>Создание классов для ответа JSON

Далее мы создадим класс, который используется при десериализации ответа JSON, возвращаемого Переводчиком.

```csharp
/// <summary>
/// The C# classes that represents the JSON returned by the Translator.
/// </summary>
public class DetectResult
{
    public string Language { get; set; }
    public float Score { get; set; }
    public bool IsTranslationSupported { get; set; }
    public bool IsTransliterationSupported { get; set; }
    public AltTranslations[] Alternatives { get; set; }
}
public class AltTranslations
{
    public string Language { get; set; }
    public float Score { get; set; }
    public bool IsTranslationSupported { get; set; }
    public bool IsTransliterationSupported { get; set; }
}
```

## <a name="get-subscription-information-from-environment-variables"></a>Получение сведений о подписке из переменных среды

Добавьте следующие строки в класс `Program`. Эти строки считывают ключ подписки и конечную точку из переменных среды и выдают ошибку при возникновении каких-либо проблем.

```csharp
private const string key_var = "TRANSLATOR_TEXT_SUBSCRIPTION_KEY";
private static readonly string subscriptionKey = Environment.GetEnvironmentVariable(key_var);

private const string endpoint_var = "TRANSLATOR_TEXT_ENDPOINT";
private static readonly string endpoint = Environment.GetEnvironmentVariable(endpoint_var);

static Program()
{
    if (null == subscriptionKey)
    {
        throw new Exception("Please set/export the environment variable: " + key_var);
    }
    if (null == endpoint)
    {
        throw new Exception("Please set/export the environment variable: " + endpoint_var);
    }
}
// The code in the next section goes here.
```

## <a name="create-a-function-to-detect-the-source-texts-language"></a>Создание функции для определения языка исходного текста

В классе `Program` создайте функцию с именем `DetectTextRequest()`. Этот класс инкапсулирует код, используемый для вызова ресурса Detect, и выводит результат в консоль.

```csharp
static public async Task DetectTextRequest(string subscriptionKey, string endpoint, string route, string inputText)
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="serialize-the-detect-request"></a>Сериализация запроса определения

Далее необходимо создать и сериализовать объект JSON, содержащий текст на определяемом языке.

```csharp
System.Object[] body = new System.Object[] { new { Text = inputText } };
var requestBody = JsonConvert.SerializeObject(body);
```

## <a name="instantiate-the-client-and-make-a-request"></a>Создание экземпляра клиента и выполнение запроса

С помощью этих строк создается экземпляр `HttpClient` и `HttpRequestMessage`:

```csharp
using (var client = new HttpClient())
using (var request = new HttpRequestMessage())
{
  // In the next few sections you'll add code to construct the request.
}
```

## <a name="construct-the-request-and-print-the-response"></a>Создание запроса и вывод ответа

При использовании `HttpRequestMessage` выполняются следующие операции:

* объявление метода HTTP;
* создание URI запроса;
* вставка текста запроса (сериализованный объект JSON);
* добавление требуемых заголовков;
* выполнение асинхронного запроса;
* Вывод ответа

Добавьте следующий код в `HttpRequestMessage`:

```csharp
// Build the request.
request.Method = HttpMethod.Post;
// Construct the URI and add headers.
request.RequestUri = new Uri(endpoint + route);
request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

// Send the request and get response.
HttpResponseMessage response = await client.SendAsync(request).ConfigureAwait(false);
// Read response as a string.
string result = await response.Content.ReadAsStringAsync();
// Deserialize the response using the classes created earlier.
DetectResult[] deserializedOutput = JsonConvert.DeserializeObject<DetectResult[]>(result);
// Iterate over the deserialized response.
foreach (DetectResult o in deserializedOutput)
{
    Console.WriteLine("The detected language is '{0}'. Confidence is: {1}.\nTranslation supported: {2}.\nTransliteration supported: {3}.\n",
        o.Language, o.Score, o.IsTranslationSupported, o.IsTransliterationSupported);
    // Create a counter
    int counter = 0;
    // Iterate over alternate translations.
    foreach (AltTranslations a in o.Alternatives)
    {
        counter++;
        Console.WriteLine("Alternative {0}", counter);
        Console.WriteLine("The detected language is '{0}'. Confidence is: {1}.\nTranslation supported: {2}.\nTransliteration supported: {3}.\n",
            a.Language, a.Score, a.IsTranslationSupported, a.IsTransliterationSupported);
    }
}
```

Если вы используете подписку на несколько служб Cognitive Services, необходимо также включить `Ocp-Apim-Subscription-Region` в параметрах запроса. [Дополнительные сведения об аутентификации с использованием подписки на несколько служб](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="put-it-all-together"></a>Сборка

Последний этап — вызов `DetectTextRequest()` в функции `Main`. Найдите строку `static void Main(string[] args)` и замените ее этим кодом:

```csharp
static async Task Main(string[] args)
{
    // This is our main function.
    // Output languages are defined in the route.
    // For a complete list of options, see API reference.
    string route = "/detect?api-version=3.0";
    string detectSentenceText = @"How are you doing today? The weather is pretty pleasant. Have you been to the movies lately?";
    await DetectTextRequest(subscriptionKey, endpoint, route, detectSentenceText);
    Console.WriteLine("Press any key to continue.");
    Console.ReadKey();
}
```
## <a name="run-the-sample-app"></a>Запуск примера приложения

Теперь все готово к запуску примера приложения. В командной строке или сеансе терминала перейдите к каталогу проекта и выполните следующую команду:

```console
dotnet run
```

## <a name="sample-response"></a>Пример ответа

После выполнения примера в окне терминала должны быть выведены такие данные:

> [!NOTE]
> Найдите сокращенное наименование страны или региона в этом [списке языков](https://docs.microsoft.com/azure/cognitive-services/translator/language-support).

```bash
The detected language is 'en'. Confidence is: 1.
Translation supported: True.
Transliteration supported: False.

Alternative 1
The detected language is 'fil'. Confidence is: 0.82.
Translation supported: True.
Transliteration supported: False.

Alternative 2
The detected language is 'ro'. Confidence is: 1.
Translation supported: True.
Transliteration supported: False.
```

Это сообщение строится на основе необработанных данных JSON, которые будут выглядеть следующим образом:

```json
[  
    {  
        "language":"en",
        "score":1.0,
        "isTranslationSupported":true,
        "isTransliterationSupported":false,
        "alternatives":[  
            {  
                "language":"fil",
                "score":0.82,
                "isTranslationSupported":true,
                "isTransliterationSupported":false
            },
            {  
                "language":"ro",
                "score":1.0,
                "isTranslationSupported":true,
                "isTransliterationSupported":false
            }
        ]
    }
]
```

## <a name="clean-up-resources"></a>Очистка ресурсов

Удалите из исходного кода примера приложения все конфиденциальные сведения, например ключи подписки.

## <a name="next-steps"></a>Дальнейшие действия

Просмотрите справочник по API, чтобы получить представление обо всех возможностях Переводчика.

> [!div class="nextstepaction"]
> [Справочник по API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
