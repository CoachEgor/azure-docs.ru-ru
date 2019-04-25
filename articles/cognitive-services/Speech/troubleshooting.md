---
title: Устранение неполадок Распознавания речи Bing | Документация Майкрософт
titlesuffix: Azure Cognitive Services
description: Как устранить проблемы при использовании Распознавания речи Bing.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: e70e7b79be7dd4ea55c56898eaf8007d25732366
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60513989"
---
# <a name="troubleshooting-bing-speech"></a>Устранение неполадок Распознавания речи Bing

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

## <a name="error-http-403-forbidden"></a>Ошибка `HTTP 403 Forbidden`

При использовании API распознавания речи возвращается ошибка `HTTP 403 Forbidden`.

### <a name="cause"></a>Причина:

Эта ошибка часто вызвана проблемами аутентификации. Запросы на подключение без допустимого заголовка `Ocp-Apim-Subscription-Key` или `Authorization` отклоняются службой с помощью ответа `HTTP 403 Forbidden`.

Если вы используете ключ подписки для аутентификации, причиной может быть

- отсутствие или недопустимость ключа подписки,
- превышение квоты использования ключа подписки,
- в заголовке запроса при вызове REST API не задано поле `Ocp-Apim-Subscription-Key`.

При использовании токена авторизации для проверки подлинности следующие причины могут привести к ошибке.

- Заголовок `Authorization` отсутствует в запросе при использовании REST,
- указан недопустимый токен в заголовке авторизации,
- срок действия токена авторизации истек. Срок действия токена истекает в течение 10 минут

Дополнительные сведения об аутентификации см. на странице [Аутентификация](How-to/how-to-authentication.md).

### <a name="troubleshooting-steps"></a>Действия по устранению неполадок

#### <a name="verify-that-your-subscription-key-is-valid"></a>Проверьте допустимость ключа подписки

Для проверки можно выполнить следующую команду. Примечание: измените запись  *YOUR_SUBSCRIPTION_KEY* своим ключом подписки. Если ключ подписки допустимый, вы получите ответ с токеном авторизации в формате JWT (JSON Web Token). В противном случае возникает ошибка в ответе.

> [!NOTE]
> Замените `YOUR_SUBSCRIPTION_KEY` фактическим ключом подписки.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```Powershell
$FetchTokenHeader = @{
  'Content-type'='application/x-www-form-urlencoded';
  'Content-Length'= '0';
  'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
}

$OAuthToken = Invoke-RestMethod -Method POST -Uri https://api.cognitive.microsoft.com/sts/v1.0/issueToken -Headers $FetchTokenHeader

# show the token received
$OAuthToken

```

# <a name="curltabcurl"></a>[curl](#tab/curl)

В этом примере используется curl под ОС Linux с помощью Bash. Если на вашей платформе нет служебной программы curl, установите ее. Этот пример будет работать также в Cygwin под ОС Windows, Git Bash, zsh и других оболочках.

```
curl -v -X POST "https://api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```
---

Убедитесь, что используется один и тот же ключ подписки в приложении или в запросе REST, который применялся выше.

#### <a name="verify-the-authorization-token"></a>Проверьте маркер проверки подлинности

Этот шаг требуется, если используется токен авторизации для проверки подлинности. Выполните следующую команду, чтобы проверить допустимость токена авторизации. Команда отправляет запрос POST в службу и ожидает ответное сообщение от нее. Если вы продолжаете получать ошибку HTTP `403 Forbidden`, дважды проверьте правильно ли установлен и не просрочен ли токен доступа.

> [!IMPORTANT]
> Срок действия токена истекает в течение 10 минут.
> [!NOTE]
> Замените `YOUR_AUDIO_FILE` на путь к предварительно записанному аудиофайлу и `YOUR_ACCESS_TOKEN` на токен авторизации, возвращенный на предыдущем шаге.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```Powershell

$SpeechServiceURI =
'https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed'

# $OAuthToken is the authrization token returned by the token service.
$RecoRequestHeader = @{
  'Authorization' = 'Bearer '+ $OAuthToken;
  'Transfer-Encoding' = 'chunked'
  'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
}

# Read audio into byte array
$audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")

$RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes

# Show the result
$RecoResponse

```

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -v -X POST "https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed" -H "Transfer-Encoding: chunked" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
```

---

## <a name="error-http-400-bad-request"></a>Ошибка `HTTP 400 Bad Request`

Эта ошибка обычно возникает, когда текст запроса содержит недопустимые аудиоданные. В настоящее время поддерживается только файл формата WAV.

## <a name="error-http-408-request-timeout"></a>Ошибка `HTTP 408 Request Timeout`

Ошибка, похоже, возникла из-за отсутствия аудиоданных в службе, и служба возвращает эту ошибку по истечении времени ожидания. Для REST API аудиоданные должны помещаться в тексте запроса.

## <a name="the-recognitionstatus-in-the-response-is-initialsilencetimeout"></a>`RecognitionStatus` в ответе — `InitialSilenceTimeout`

Аудиоданные обычно являются причиной возникновения проблемы. Например,

- аудио имеет длительное время тишины в начале. Служба прекратит распознавание через несколько секунд и вернет `InitialSilenceTimeout`.
- Аудио использует неподдерживаемый формат кодека, что приводит к тому, что аудиоданные воспринимаются как тишина.
