---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: 649af0d3b1503a5607717c4ccbadd8f1d822be32
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83586905"
---
[!INCLUDE [Prerequisites](prerequisites-nodejs.md)]

[!INCLUDE [Set up and use environment variables](setup-env-variables.md)]

## <a name="create-a-project-and-import-required-modules"></a>Создание проекта и импорт обязательных модулей

Создайте проект, используя любую IDE или любой текстовый редактор. Затем скопируйте в файл проекта с именем `get-languages.js` этот фрагмент кода.

```javascript
const request = require('request');
const uuidv4 = require('uuid/v4');
```

> [!NOTE]
> Если вы ранее не использовали эти модули, вам потребуется установить их перед запуском программы. Чтобы установить эти пакеты, выполните команду: `npm install request uuidv4`.

Эти модули необходимы для построения HTTP-запроса и создания уникального идентификатора для заголовка `'X-ClientTraceId'`.

## <a name="set-the-endpoint"></a>Задание конечной точки

В этом примере будет предпринята попытка считать конечную точку Переводчика из переменной среды `TRANSLATOR_TEXT_ENDPOINT`. Если вы не знакомы с переменными среды, можно задать `endpoint` в виде строки и закомментировать условный оператор.

```javascript
lorum ipsum
```

## <a name="configure-the-request"></a>Настройка запроса

Метод `request()`, доступный через модуль запросов, позволяет передавать метод HTTP, URL-адрес, параметры запроса, заголовки и текст JSON как объект `options`. В этом фрагменте кода мы настроим запрос:

>[!NOTE]
> Дополнительные сведения о конечных точках, маршрутах и параметрах запросов см. в руководстве по [Переводчику 3.0. Languages](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages).

```javascript
let options = {
    method: 'GET',
    baseUrl: endpoint,
    url: 'languages',
    qs: {
      'api-version': '3.0',
    },
    headers: {
      'Content-type': 'application/json',
      'X-ClientTraceId': uuidv4().toString()
    },
    json: true,
};
```

Если вы используете подписку на несколько служб Cognitive Services, необходимо также включить `Ocp-Apim-Subscription-Region` в параметрах запроса. [Дополнительные сведения об аутентификации с использованием подписки на несколько служб](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="make-the-request-and-print-the-response"></a>Выполнение запроса и вывод ответа

Теперь мы создадим запрос с помощью метода `request()`. Он принимает объект `options`, который мы создали в предыдущем разделе, в качестве первого аргумента, а затем выводит понятный ответ JSON.

```javascript
request(options, function(err, res, body){
    console.log(JSON.stringify(body, null, 4));
});
```

>[!NOTE]
> В этом примере мы определяем HTTP-запрос в объекте `options`. Однако модуль запроса также поддерживает удобные методы, такие как `.post` и `.get`. См. дополнительные сведения [об удобных методах](https://github.com/request/request#convenience-methods).

## <a name="put-it-all-together"></a>Сборка

Вот и все. Вы собрали простую программу, которая вызовет Переводчика и вернет ответ в формате JSON. Теперь пришло время запустить ее.

```console
node get-languages.js
```

Если вы хотите сравнить свой код с нашей версией, см. [пример кода на сайте GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-NodeJS).

## <a name="sample-response"></a>Пример ответа

Найдите сокращенное наименование страны или региона в этом [списке языков](https://docs.microsoft.com/azure/cognitive-services/translator/language-support).

Этот пример мы сократили, чтобы показать фрагмент результата:

```json
{
    "translation": {
        "af": {
            "name": "Afrikaans",
            "nativeName": "Afrikaans",
            "dir": "ltr"
        },
        "ar": {
            "name": "Arabic",
            "nativeName": "العربية",
            "dir": "rtl"
        },
        ...
    },
    "transliteration": {
        "ar": {
            "name": "Arabic",
            "nativeName": "العربية",
            "scripts": [
                {
                    "code": "Arab",
                    "name": "Arabic",
                    "nativeName": "العربية",
                    "dir": "rtl",
                    "toScripts": [
                        {
                            "code": "Latn",
                            "name": "Latin",
                            "nativeName": "اللاتينية",
                            "dir": "ltr"
                        }
                    ]
                },
                {
                    "code": "Latn",
                    "name": "Latin",
                    "nativeName": "اللاتينية",
                    "dir": "ltr",
                    "toScripts": [
                        {
                            "code": "Arab",
                            "name": "Arabic",
                            "nativeName": "العربية",
                            "dir": "rtl"
                        }
                    ]
                }
            ]
        },
      ...
    },
    "dictionary": {
        "af": {
            "name": "Afrikaans",
            "nativeName": "Afrikaans",
            "dir": "ltr",
            "translations": [
                {
                    "name": "English",
                    "nativeName": "English",
                    "dir": "ltr",
                    "code": "en"
                }
            ]
        },
        "ar": {
            "name": "Arabic",
            "nativeName": "العربية",
            "dir": "rtl",
            "translations": [
                {
                    "name": "English",
                    "nativeName": "English",
                    "dir": "ltr",
                    "code": "en"
                }
            ]
        },
      ...
    }
}
```

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы закодировали свой ключ подписки в программе, обязательно удалите его после завершения работы с этим кратким руководством.

## <a name="next-steps"></a>Дальнейшие действия

Просмотрите справочник по API, чтобы получить представление обо всех возможностях Переводчика.

> [!div class="nextstepaction"]
> [Справочник по API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
