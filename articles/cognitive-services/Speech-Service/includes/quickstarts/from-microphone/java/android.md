---
title: Краткое руководство. Распознавание речи с микрофона в службе "Речь" с помощью Java (Android)
titleSuffix: Azure Cognitive Services
description: Узнайте, как распознавать речь в Android, используя Java и пакет SDK службы "Речь"
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 11/05/2019
ms.author: wolfma
ms.openlocfilehash: 5832920ec491ce9d99219a7a423fcf0e38ba62cc
ms.sourcegitcommit: 152c522bb5ad64e5c020b466b239cdac040b9377
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/14/2020
ms.locfileid: "88226340"
---
## <a name="prerequisites"></a>Предварительные требования

Необходимые условия:

> [!div class="checklist"]
> * [Создать ресурс службы "Речь" Azure.](../../../../get-started.md)
> * [Настроить среду разработки.](../../../../quickstarts/setup-platform.md?tabs=android&pivots=programming-language-java)
> * Убедитесь, что у вас есть доступ к микрофону для аудиозахвата.

## <a name="create-a-user-interface"></a>Создание пользовательского интерфейса

Теперь для этого приложения мы создадим базовый пользовательский интерфейс. Измените макет для основного действия, `activity_main.xml`. В макете должны отображаться имя вашего приложения в заголовке окна и элемент TextView, содержащий текст "Hello World!".

* Выберите элемент TextView. Измените его атрибут ID в правом верхнем углу, указав `hello`.

* Перетащите элемент button (кнопка) из палитры в левом верхнем углу окна `activity_main.xml` на пустое место над текстом.

* В области атрибутов кнопки справа для атрибута `onClick` введите значение `onSpeechButtonClicked`. Мы создадим метод с таким именем для обработки события кнопки. Измените его атрибут ID в правом верхнем углу, указав `button`.

* Используйте значок волшебной палочки в верхней части окна конструктора, чтобы вывести ограничения макета.

  ![Снимок экрана со значком "волшебная палочка"](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-10-infer-layout-constraints.png)

Теперь текстовая и графическая версии пользовательского интерфейса должны выглядеть следующим образом:

![Пользовательский интерфейс](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-11-gui.png)

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java/android/from-microphone/app/src/main/res/layout/activity_main.xml)]

## <a name="add-sample-code"></a>Добавление примеров кода

1. Откройте исходный файл `MainActivity.java`. Замените все содержимое этого файла приведенным ниже кодом:

   [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/android/from-microphone/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/quickstart/MainActivity.java#code)]

   * Метод `onCreate` содержит код, который запрашивает разрешения на использование микрофона и подключения к интернету, а также инициализирует привязки собственной платформы. Настроить привязки собственной платформы нужно только один раз. Это следует сделать в начале инициализации приложения.

   * Метод `onSpeechButtonClicked`, как упоминалось ранее, является обработчиком нажатия кнопки. Нажатие кнопки активирует преобразование речи в текст.

1. В том же файле замените строку `YourSubscriptionKey` своим ключом подписки.

1. Кроме того, найдите строку `YourServiceRegion` и замените ее **идентификатором региона** из [региона](https://aka.ms/speech/sdkregion), связанного с вашей подпиской.

## <a name="build-and-run-the-app"></a>Создание и запуск приложения

1. Подключите устройство Android к компьютеру разработки. Убедитесь, что вы включили на устройстве [режим разработки и отладку по USB](https://developer.android.com/studio/debug/dev-options).

1. Чтобы выполнить сборку приложения, нажмите комбинацию клавиш Ctrl+F9 или выберите в строке меню **Сборка** > **Make Project** (Создать проект).

1. Чтобы запустить приложение, нажмите комбинацию клавиш Shift+F10 или выберите**Запуск** > **Run 'app'** (Запустить "приложение").

1. В отобразившемся окне целей развертывания выберите свое устройство Android.

   ![Снимок экрана окна Select Deployment Target (Выбор цели развертывания)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-12-deploy.png)

Чтобы начать процесс распознавания речи, нажмите кнопку в приложении. Последующие 15 секунд английской речи будут отправлены в службу "Речь" для распознавания. Его результат отображается в приложении Android и в окне "logcat" в Android Studio.

![Снимок экрана приложения Android](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-13-gui-on-device.png)

## <a name="next-steps"></a>Дальнейшие действия

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]
