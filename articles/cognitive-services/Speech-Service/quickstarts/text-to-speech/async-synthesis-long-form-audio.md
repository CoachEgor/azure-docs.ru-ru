---
title: Краткое руководство. асинхронное синтез для больших аудио (Предварительная версия) — служба речи
titleSuffix: Azure Cognitive Services
description: Используйте длинный аудио API для асинхронного преобразования текста в речь и получения звуковых выходных данных из URI, предоставленного службой. Этот REST API идеально подходит для поставщиков содержимого, которым необходимо преобразовать текстовые файлы длиннее 10 000 символов или 50 абзацев в синтезированный речевой перевод.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: erhopf
ms.openlocfilehash: 62236b472aa5c4812cd62af44a15b805b5326271
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592565"
---
# <a name="quickstart-asynchronous-synthesis-for-long-form-audio-in-python-preview"></a>Краткое руководство. асинхронное синтез для звука в длинном формате в Python (Предварительная версия)

В этом кратком руководстве вы используете длинный аудио API для асинхронного преобразования текста в речь и получения звуковых выходных данных из URI, предоставленного службой. Этот REST API идеально подходит для поставщиков содержимого, которым требуется синтезирование звука из текста более 5 000 символов (или более 10 минут в длину). Дополнительные сведения см. в разделе [Long Audio API](../../long-audio-api.md).

Асинхронное синтез для длинных аудио-данных можно использовать с [общедоступными нейронными](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#neural-voices) и [пользовательскими нейронными голоса](../../how-to-custom-voice.md#custom-neural-voices), каждый из которых поддерживает конкретный язык и диалект. 

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством вам понадобится:

* Python 2.7. x или 3. x.
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download)или ваш любимый текстовый редактор.
* Подписка Azure и ключ подписки на службу речи. [Создайте учетную запись Azure](../../get-started.md#new-resource) и [Создайте речевой ресурс](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#new-resource) для получения ключа. При создании ресурса речи убедитесь, что для ценовой категории задано значение **S0**, а для параметра расположение — [поддерживаемый регион](../../regions.md#standard-and-neural-voices).

## <a name="create-a-project-and-import-required-modules"></a>Создание проекта и импорт обязательных модулей

Создайте проект Python, используя любую IDE или любой текстовый редактор. Затем скопируйте этот фрагмент кода в файл с именем `voice_synthesis_client.py` .

```python
import argparse
import json
import ntpath
import urllib3
import requests
import time
from json import dumps, loads, JSONEncoder, JSONDecoder
import pickle

urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
```

> [!NOTE]
> Если вы не использовали эти модули, необходимо установить их перед запуском программы. Чтобы установить эти пакеты, выполните команду: `pip install requests urllib3`.

Эти модули используются для анализа аргументов, создают HTTP-запрос и вызывают длинные аудио REST API текста в речь.

## <a name="get-a-list-of-supported-voices"></a>Получение списка поддерживаемых голосов

Этот код позволяет получить полный список голосов для определенного региона или конечной точки, которые можно использовать. Проверьте [поддерживаемый регион или конечную точку](../../long-audio-api.md). Добавьте код в `voice_synthesis_client.py` :

```python
parser = argparse.ArgumentParser(description='Text-to-speech client tool to submit voice synthesis requests.')
parser.add_argument('--voices', action="store_true", default=False, help='print voice list')
parser.add_argument('-key', action="store", dest="key", required=True, help='the speech subscription key, like fg1f763i01d94768bda32u7a******** ')
parser.add_argument('-region', action="store", dest="region", required=True, help='the region information, could be centralindia, canadacentral or uksouth')
args = parser.parse_args()
baseAddress = 'https://%s.customvoice.api.speech.microsoft.com/api/texttospeech/v3.0-beta1/' % args.region

def getVoices():
    response=requests.get(baseAddress+"voicesynthesis/voices", headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    voices = json.loads(response.text)
    return voices

if args.voices:
    voices = getVoices()
    print("There are %d voices available:" % len(voices))
    for voice in voices:
        print ("Name: %s, Description: %s, Id: %s, Locale: %s, Gender: %s, PublicVoice: %s, Created: %s" % (voice['name'], voice['description'], voice['id'], voice['locale'], voice['gender'], voice['isPublicVoice'], voice['created']))
```

### <a name="test-your-code"></a>Тестирование кода

Проверим, что вы сделали до сих пор. В запросе ниже необходимо обновить несколько вещей:

* Замените `<your_key>` на ваш ключ подписки службы "Речь". Эти сведения доступны на вкладке **Обзор** ресурса в [портал Azure](https://aka.ms/azureportal).
* Замените на `<region>` регион, в котором был создан ваш речевой ресурс (например: `eastus` или `westus` ). Эти сведения доступны на вкладке **Обзор** ресурса в [портал Azure](https://aka.ms/azureportal).

Выполните следующую команду:

```console
python voice_synthesis_client.py --voices -key <your_key> -region <Region>
```

Вы увидите выходные данные следующего вида:

```console
There are xx voices available:

Name: Microsoft Server Speech Text to Speech Voice (en-US, xxx), Description: xxx , Id: xxx, Locale: en-US, Gender: Male, PublicVoice: xxx, Created: 2019-07-22T09:38:14Z
Name: Microsoft Server Speech Text to Speech Voice (zh-CN, xxx), Description: xxx , Id: xxx, Locale: zh-CN, Gender: Female, PublicVoice: xxx, Created: 2019-08-26T04:55:39Z
```

Если параметр **публиквоице** имеет **значение true**, речь является общедоступным нейронным голоса. В противном случае это пользовательский нейронный счет. 

## <a name="prepare-input-files"></a>Подготовка входных файлов

Подготовьте входной текстовый файл. Это может быть обычный текст или SSML текст. Требования к входному файлу см. в разделе [Подготовка содержимого для синтеза](https://docs.microsoft.com/azure/cognitive-services/speech-service/long-audio-api#prepare-content-for-synthesis).

## <a name="convert-text-to-speech"></a>Преобразование текста в речь

После подготовки входного текстового файла добавьте следующий код для синтеза речи `voice_synthesis_client.py` :

> [!NOTE]
> "Конкатенатересулт" является необязательным параметром. Если этот параметр не задан, выходные данные будут формироваться для каждого абзаца. Вы также можете объединить звуковые данные в 1 выход, задав параметр. По умолчанию для вывода звука задано значение Metallica-16khz-16-разрядный-Mono-PCM. Дополнительные сведения о поддерживаемых звуковых выходах см. в разделе [форматы выходных данных звука](https://docs.microsoft.com/azure/cognitive-services/speech-service/long-audio-api#audio-output-formats).

```python
parser.add_argument('--submit', action="store_true", default=False, help='submit a synthesis request')
parser.add_argument('--concatenateResult', action="store_true", default=False, help='If concatenate result in a single wave file')
parser.add_argument('-file', action="store", dest="file", help='the input text script file path')
parser.add_argument('-voiceId', action="store", nargs='+', dest="voiceId", help='the id of the voice which used to synthesis')
parser.add_argument('-locale', action="store", dest="locale", help='the locale information like zh-CN/en-US')
parser.add_argument('-format', action="store", dest="format", default='riff-16khz-16bit-mono-pcm', help='the output audio format')

def submitSynthesis():
    modelList = args.voiceId
    data={'name': 'simple test', 'description': 'desc...', 'models': json.dumps(modelList), 'locale': args.locale, 'outputformat': args.format}
    if args.concatenateResult:
        properties={'ConcatenateResult': 'true'}
        data['properties'] = json.dumps(properties)
    if args.file is not None:
        scriptfilename=ntpath.basename(args.file)
        files = {'script': (scriptfilename, open(args.file, 'rb'), 'text/plain')}
    response = requests.post(baseAddress+"voicesynthesis", data, headers={"Ocp-Apim-Subscription-Key":args.key}, files=files, verify=False)
    if response.status_code == 202:
        location = response.headers['Location']
        id = location.split("/")[-1]
        print("Submit synthesis request successful")
        return id
    else:
        print("Submit synthesis request failed")
        print("response.status_code: %d" % response.status_code)
        print("response.text: %s" % response.text)
        return 0

def getSubmittedSynthesis(id):
    response=requests.get(baseAddress+"voicesynthesis/"+id, headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    synthesis = json.loads(response.text)
    return synthesis

if args.submit:
    id = submitSynthesis()
    if (id == 0):
        exit(1)

    while(1):
        print("\r\nChecking status")
        synthesis=getSubmittedSynthesis(id)
        if synthesis['status'] == "Succeeded":
            r = requests.get(synthesis['resultsUrl'])
            filename=id + ".zip"
            with open(filename, 'wb') as f:  
                f.write(r.content)
                print("Succeeded... Result file downloaded : " + filename)
            break
        elif synthesis['status'] == "Failed":
            print("Failed...")
            break
        elif synthesis['status'] == "Running":
            print("Running...")
        elif synthesis['status'] == "NotStarted":
            print("NotStarted...")
        time.sleep(10)
```

### <a name="test-your-code"></a>Тестирование кода

Давайте создадим запрос на синтезирование текста, используя входной файл в качестве источника. В запросе ниже необходимо обновить несколько вещей:

* Замените `<your_key>` на ваш ключ подписки службы "Речь". Эти сведения доступны на вкладке **Обзор** ресурса в [портал Azure](https://aka.ms/azureportal).
* Замените на `<region>` регион, в котором был создан ваш речевой ресурс (например: `eastus` или `westus` ). Эти сведения доступны на вкладке **Обзор** ресурса в [портал Azure](https://aka.ms/azureportal).
* Замените на `<input>` путь к текстовому файлу, подготовленному для преобразования текста в речь.
* Замените на `<locale>` нужный языковой стандарт вывода. Дополнительные сведения см. в разделе [Поддержка языков](../../language-support.md#neural-voices).
* Замените на `<voice_guid>` нужный выходной поток. Используйте один из голосов, возвращаемых, чтобы [получить список поддерживаемых голосов](#get-a-list-of-supported-voices).

Преобразуйте текст в речь с помощью следующей команды:

```console
python voice_synthesis_client.py --submit -key <your_key> -region <Region> -file <input> -locale <locale> -voiceId <voice_guid>
```

> [!NOTE]
> При наличии более 1 входных файлов необходимо отправить несколько запросов. Существуют некоторые ограничения, которые необходимо учитывать. 
> * Клиент может отправлять до **5** запросов на сервер в секунду для каждой учетной записи подписки Azure. Если оно превышает ограничение, клиент получает код ошибки 429 (слишком много запросов). Сократите количество запросов в секунду.
> * Сервер может запуститься и поставить в очередь до **120** запросов для каждой учетной записи подписки Azure. Если оно превышает ограничение, сервер возвратит код ошибки 429 (слишком много запросов). Подождите и не отправляйте новый запрос, пока не будут завершены некоторые запросы

Вы увидите выходные данные следующего вида:

```console
Submit synthesis request successful

Checking status
NotStarted...

Checking status
Running...

Checking status
Running...

Checking status
Succeeded... Result file downloaded : xxxx.zip
```

Результат содержит входной и аудиофайлный выходные файлы, создаваемые службой. Эти файлы можно загрузить в ZIP-файл.

## <a name="remove-previous-requests"></a>Удалить предыдущие запросы

Сервер будет иметь до **20 000** запросов для каждой учетной записи подписки Azure. Если сумма запроса превышает это ограничение, удалите предыдущие запросы перед созданием новых. Если не удалить существующие запросы, вы получите уведомление об ошибке.

Добавьте код в `voice_synthesis_client.py` :

```python
parser.add_argument('--syntheses', action="store_true", default=False, help='print synthesis list')
parser.add_argument('--delete', action="store_true", default=False, help='delete a synthesis request')
parser.add_argument('-synthesisId', action="store", nargs='+', dest="synthesisId", help='the id of the voice synthesis which need to be deleted')

def getSubmittedSyntheses():
    response=requests.get(baseAddress+"voicesynthesis", headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    syntheses = json.loads(response.text)
    return syntheses

def deleteSynthesis(ids):
    for id in ids:
        print("delete voice synthesis %s " % id)
        response = requests.delete(baseAddress+"voicesynthesis/"+id, headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
        if (response.status_code == 204):
            print("delete successful")
        else:
            print("delete failed, response.status_code: %d, response.text: %s " % (response.status_code, response.text))

if args.syntheses:
    synthese = getSubmittedSyntheses()
    print("There are %d synthesis requests submitted:" % len(synthese))
    for synthesis in synthese:
        print ("ID : %s , Name : %s, Status : %s " % (synthesis['id'], synthesis['name'], synthesis['status']))

if args.delete:
    deleteSynthesis(args.synthesisId)
```

### <a name="test-your-code"></a>Тестирование кода

Теперь давайте проверим, какие запросы были отправлены ранее. Прежде чем продолжить, необходимо обновить несколько вещей в этом запросе:

* Замените `<your_key>` на ваш ключ подписки службы "Речь". Эти сведения доступны на вкладке **Обзор** ресурса в [портал Azure](https://aka.ms/azureportal).
* Замените на `<region>` регион, в котором был создан ваш речевой ресурс (например: `eastus` или `westus` ). Эти сведения доступны на вкладке **Обзор** ресурса в [портал Azure](https://aka.ms/azureportal).

Выполните следующую команду:

```console
python voice_synthesis_client.py --syntheses -key <your_key> -region <Region>
```

Будет возвращен список сделанных запросов синтеза. Вы увидите следующий результат:

```console
There are <number> synthesis requests submitted:
ID : xxx , Name : xxx, Status : Succeeded
ID : xxx , Name : xxx, Status : Running
ID : xxx , Name : xxx : Succeeded
```

Теперь удалим ранее отправленный запрос. В приведенном ниже коде необходимо обновить несколько вещей.

* Замените `<your_key>` на ваш ключ подписки службы "Речь". Эти сведения доступны на вкладке **Обзор** ресурса в [портал Azure](https://aka.ms/azureportal).
* Замените на `<region>` регион, в котором был создан ваш речевой ресурс (например: `eastus` или `westus` ). Эти сведения доступны на вкладке **Обзор** ресурса в [портал Azure](https://aka.ms/azureportal).
* Замените на `<synthesis_id>` значение, возвращенное в предыдущем запросе.

> [!NOTE]
> Запросы с состоянием "работает" или "ожидание" не могут быть удалены или удалены.

Выполните следующую команду:

```console
python voice_synthesis_client.py --delete -key <your_key> -region <Region> -synthesisId <synthesis_id>
```

Вы увидите следующий результат:

```console
delete voice synthesis xxx
delete successful
```

## <a name="get-the-full-client"></a>Получение полного клиента

Завершение `voice_synthesis_client.py` можно скачать на сайте [GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Python/voiceclient.py).

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Дополнительные сведения об API длинного звука](../../long-audio-api.md)
