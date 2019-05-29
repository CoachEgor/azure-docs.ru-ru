---
title: Краткое руководство. Распознавание речи с использованием Java и службы "Речь" (Android)
titleSuffix: Azure Cognitive Services
description: Узнайте, как распознавать речь в Android, используя Java и пакет SDK службы "Речь"
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 2/20/2019
ms.author: wolfma
ms.openlocfilehash: f856694a5063551ba94bb6d322614aa490f13adb
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/22/2019
ms.locfileid: "66001889"
---
# <a name="quickstart-recognize-speech-in-java-on-android-by-using-the-speech-sdk"></a>Краткое руководство. Распознавание речи в приложении Java для Android с помощью пакета SDK для службы "Речь"

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Из этой статьи вы узнаете, как с помощью пакета SDK службы "Речь" в Cognitive Services создать приложение Java для Android, которое будет преобразовывать речь в текст.
Приложение основано на пакете SDK Maven версии 1.5.1 для службы "Речь", а также Android Studio 3.3.
Пакет SDK службы "Речь" сейчас совместим с устройствами Android, в которых установлены 32- или 64-разрядные процессоры ARM и процессоры Intel, совместимые с набором команд x86-x64.

> [!NOTE]
> Ознакомьтесь с пакетом SDK службы "Речь" и устройством Roobo в разделе [Сведения о пакете SDK для речевых устройств](speech-devices-sdk.md).

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством вам потребуется ключ подписки службы "Речь". Его можно получить бесплатно. Дополнительные сведения см. в статье [Бесплатная пробная подписка на службу "Речь"](get-started.md).

## <a name="create-and-configure-a-project"></a>Создание и настройка проекта

1. Запустите Android Studio и в окне приветствия выберите **Start a new Android Studio project** (Создать проект Android Studio).

    ![Снимок экрана окна приветствия Android Studio](media/sdk/qs-java-android-01-start-new-android-studio-project.png)

1. Откроется мастер **Choose your project** (Выбор проекта), выберите **Phone and Tablet** (Телефон и планшет) и **Empty Activity** (Пустое действие) в поле выбора действия. Щелкните **Далее**.

   ![Снимок экрана мастера Choose your project (Выбор проекта)](media/sdk/qs-java-android-02-target-android-devices.png)

1. На экране **Configure your project** (Настройка проекта) введите **Quickstart** в поле **Name** (Имя), **samples.speech.cognitiveservices.microsoft.com** в поле **Package name** (Имя пакета) и выберите каталог проекта. Для параметра **Minimum API level** (Минимальный уровень API) задайте значение **API 23: Android 6.0 (Marshmallow)** , оставьте другие флажки снятыми и нажмите **Finish** (Готово).

   ![Снимок экрана мастера Configure your project (Настройка проекта)](media/sdk/qs-java-android-03-create-android-project.png)

Android Studio требуется несколько минут, чтобы подготовить новый проект Android. Затем настройте для проекта пакет SDK для службы "Речь" и использование Java 8.

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Текущая версия пакета SDK для распознавания речи для Cognitive Services — `1.5.1`.

Пакет SDK службы "Речь" для Android входит в состав [AAR (библиотека Android)](https://developer.android.com/studio/projects/android-library), которая содержит необходимые библиотеки и требуемые разрешения Android.
Он доступен в репозитории Maven по адресу https:\//csspeechstorage.blob.core.windows.net/maven/.

Настройте проект для использования пакета SDK службы "Речь". Откройте окно Project Structure (Структура проекта), выбрав в строке меню Android Studio **Файл** > **Project Structure** (Структура проекта). В окне Project Structure (Структура проекта) внесите следующие изменения:

1. В списке в левой части окна выберите **Project** (Проект). Измените параметры **Default Library Repository** (Репозиторий библиотек по умолчанию), добавив запятую и URL-адрес репозитория Maven в одиночных кавычках. 'https:\//csspeechstorage.blob.core.windows.net/maven/'

   ![Снимок экрана окна Project Structure (Структура проекта)](media/sdk/qs-java-android-06-add-maven-repository.png)

1. В том же экране слева выберите **app** (приложение). Затем в верхней части окна выберите вкладку **Зависимости**. Нажмите зеленый знак плюс (+) и выберите в раскрывающемся меню **Library dependency** (Зависимость библиотек).

   ![Снимок экрана окна Project Structure (Структура проекта)](media/sdk/qs-java-android-07-add-module-dependency.png)

1. В появившемся окне введите имя и версию нашего пакета SDK службы "Речь" для Android, `com.microsoft.cognitiveservices.speech:client-sdk:1.5.1`. Нажмите кнопку **ОК**.
   Пакет SDK для службы "Речь" должен быть добавлен в список зависимостей, как показано ниже.

   ![Снимок экрана окна Project Structure (Структура проекта)](media/sdk/qs-java-android-08-dependency-added-1.0.0.png)

1. Выберите вкладку **Свойства**. Выберите значение **1.8** для обоих параметров **Source Compability** (Совместимость источника) и **Target Compatibility** (Совместимость цели).

   ![](media/sdk/qs-java-android-09-dependency-added.png)

1. Чтобы закрыть окно Project Structure (Структура проекта) и применить изменения к проекту, нажмите **OК**.

## <a name="create-user-interface"></a>Создание пользовательского интерфейса

Для этого приложения мы создадим базовый пользовательский интерфейс. Измените макет для основного действия, `activity_main.xml`. В макете должны отображаться имя вашего приложения в заголовке окна и элемент TextView, содержащий текст "Hello World!".

* Щелкните элемент TextView. Измените его атрибут ID в правом верхнем углу, указав `hello`.

* Перетащите элемент button (кнопка) из палитры в левом верхнем углу окна `activity_main.xml` на пустое место над текстом.

* В области атрибутов кнопки справа для атрибута `onClick` введите значение `onSpeechButtonClicked`. Мы создадим метод с таким именем для обработки события кнопки.  Измените его атрибут ID в правом верхнем углу, указав `button`.

* Используйте значок волшебной палочки в верхней части окна конструктора, чтобы вывести ограничения макета.

  ![Снимок экрана со значком "волшебная палочка"](media/sdk/qs-java-android-10-infer-layout-constraints.png)

Теперь текстовая и графическая версии пользовательского интерфейса должны выглядеть следующим образом:

![](media/sdk/qs-java-android-11-gui.png)

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/res/layout/activity_main.xml)]

## <a name="add-sample-code"></a>Добавление примеров кода

1. Откройте исходный файл `MainActivity.java`. Замените все содержимое этого файла приведенным ниже кодом.

   [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/quickstart/MainActivity.java#code)]

   * Метод `onCreate` содержит код, который запрашивает разрешения на использование микрофона и подключения к интернету, а также инициализирует привязки собственной платформы. Настроить привязки собственной платформы нужно только один раз. Это следует сделать в начале инициализации приложения.

   * Метод `onSpeechButtonClicked`, как упоминалось ранее, является обработчиком нажатия кнопки. Нажатие кнопки активирует преобразование речи в текст.

1. В том же файле замените строку `YourSubscriptionKey` своим ключом подписки.

1. Замените строку `YourServiceRegion` значением [региона](regions.md), связанного с подпиской (например, `westus` для бесплатной пробной подписки).

## <a name="build-and-run-the-app"></a>Создание и запуск приложения

1. Подключите устройство Android к компьютеру разработки. Убедитесь, что вы включили на устройстве [режим разработки и отладку по USB](https://developer.android.com/studio/debug/dev-options).

1. Чтобы выполнить сборку приложения, нажмите комбинацию клавиш Ctrl + F9 или выберите в строке меню **Сборка** > **Make Project** (Создать проект).

1. Чтобы запустить приложение, нажмите комбинацию клавиш Shift+F10 или выберите**Запуск** > **Run 'app'** (Запустить "приложение").

1. В отобразившемся окне целей развертывания выберите свое устройство Android.

   ![Снимок экрана окна Select Deployment Target (Выбор цели развертывания)](media/sdk/qs-java-android-12-deploy.png)

Чтобы начать процесс распознавания речи, нажмите кнопку в приложении. Последующие 15 секунд английской речи будут отправлены в службу "Речь" для распознавания. Его результат отображается в приложении Android и в окне "logcat" в Android Studio.

![Снимок экрана приложения Android](media/sdk/qs-java-android-13-gui-on-device.png)

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Примеры для Java на сайте GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>См. также

- [Настройка акустических моделей](how-to-customize-acoustic-models.md)
- [Настройка языковых моделей](how-to-customize-language-model.md)
