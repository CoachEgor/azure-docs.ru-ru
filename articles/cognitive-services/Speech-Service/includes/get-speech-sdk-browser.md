---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 5545b10a4228448d49849e7cd52728febf14ce2d
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399914"
---
:::row:::
    :::column span="3":::
        JavaScript речи SDK доступен в качестве пакета npm, см <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">Microsoft-когнитивных-речевой-sdk <span class="docon docon-navigate-external x-hidden-focus"></span> </a> и его компаньон GitHub репозиторий <a href="https://github.com/Microsoft/cognitive-services-speech-sdk-js" target="_blank">когнитивных услуг-речи-sdk-js <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="JavaScript" src="https://docs.microsoft.com/media/logos/logo_js.svg"  width="60px">
        </div>
    :::column-end:::
:::row-end:::

> [!TIP]
> Хотя JavaScript Speech SDK доступен в виде пакета npm, таким образом, оба веб-браузера и Node.js могут потреблять его - рассмотреть различные архитектурные последствия каждой среды. Например, <a href="https://en.wikipedia.org/wiki/Document_Object_Model" target="_blank">модель объекта документа <span class="docon docon-navigate-external x-hidden-focus"></span> (DOM)</a> недоступна для приложений на стороне сервера так же, как <a href="https://nodejs.org/api/fs.html" target="_blank">файловая система <span class="docon docon-navigate-external x-hidden-focus"></span> </a> недоступна для клиентских приложений.

### <a name="nodejs-package-manager-npm"></a>Менеджер пакета node.js (NPM)

Чтобы установить SDK речи JavaScript, запустите следующую `npm install` команду ниже.

```nodejs
npm install microsoft-cognitiveservices-speech-sdk
```

### <a name="html-script-tag"></a>Тег скрипта HTML

Кроме того, можно напрямую включить `<script>` тег `<head>` в элемент HTMLs, опираясь на <a href="https://www.jsdelivr.com/package/npm/microsoft-cognitiveservices-speech-sdk" target="_blank">синдикат <span class="docon docon-navigate-external x-hidden-focus"> </span> **JSDelivr** NPM. </a>

```html
<script src="https://cdn.jsdelivr.net/npm/microsoft-cognitiveservices-speech-sdk@1.10.1/distrib/lib/microsoft.cognitiveservices.speech.sdk.min.js">
</script>
```

Для получения дополнительной информации, см <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/browser" target="_blank">веб-браузер <span class="docon docon-navigate-external x-hidden-focus"> </span>речи SDK quickstart </a>.
