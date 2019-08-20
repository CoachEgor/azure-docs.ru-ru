---
title: Краткое руководство. Определение и выделение лиц на изображении с помощью пакета SDK для Python
titleSuffix: Azure Cognitive Services
description: С помощью этого краткого руководства вы создадите скрипт Python, который использует API Распознавания лиц для определения и выделения лиц на удаленно расположенном изображении.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 08/08/2019
ms.author: sbowles
ms.openlocfilehash: 502523e3c62f993af4484c8ab922c36fa51a60f1
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/10/2019
ms.locfileid: "68945890"
---
# <a name="quickstart-create-a-python-script-to-detect-and-frame-faces-in-an-image"></a>Краткое руководство. Создание скрипта Python для определения и выделения лиц на изображении

С помощью этого краткого руководства вы создадите скрипт Python, который использует API Распознавания лиц Azure, доступный в пакете SDK для Python, для определения и выделения лиц на удаленно расположенном изображении. Приложение отображает выбранное изображение и рисует рамку вокруг каждого распознанного лица.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу. 

## <a name="prerequisites"></a>Предварительные требования

- Ключ подписки на API распознавания лиц. Вы можете получить ключ бесплатной пробной подписки на странице [Пробная версия Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Или следуйте инструкциям в руководстве по [созданию учетной записи Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account), чтобы получить подписку на API распознавания лиц и свой ключ.
- [Python 2.7 или 3.5+](https://www.python.org/downloads/).
- Средство [PIP](https://pip.pypa.io/en/stable/installing/).

## <a name="get-the-face-sdk"></a>Получение пакета SDK Распознавания лиц

Установите пакет SDK Распознавания лиц для Python. Для этого откройте командную строку и выполните следующую команду:

```shell
pip install azure-cognitiveservices-vision-face
```

## <a name="detect-faces-in-an-image"></a>Определение лиц на изображении

Создайте скрипт Python с именем _FaceQuickstart.py_ и добавьте в него приведенный ниже код. Этот код реализует основные функции определения лиц. Замените `<Subscription Key>` фактическим значением ключа. Вам также может потребоваться изменить значение `BASE_URL`, чтобы использовать правильный идентификатор региона для ключа (список конечных точек для всех регионов см. в документации по [API распознавания лиц](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)). Ключи бесплатной пробной подписки создаются в регионе **westus**. При необходимости задайте для `img_url` URL-адрес любого изображения, которое вы хотите использовать.

Скрипт обнаруживает лица, вызывая метод **FaceClient.face.detect_with_url**, который инкапсулирует REST API [обнаружения](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) и возвращает список лиц.

```python
from azure.cognitiveservices.vision.face import FaceClient

# Replace with a valid subscription key (keeping the quotes in place).
KEY = '<Subscription Key>'

# Replace with your regional Base URL
BASE_URL = 'https://westus.api.cognitive.microsoft.com/'

faceClient = FaceClient(BASE_URL, CognitiveServicesCredentials(KEY))

# You can use this example JPG or replace the URL below with your own URL to a JPEG image.
img_url = 'https://raw.githubusercontent.com/Microsoft/Cognitive-Face-Windows/master/Data/detection1.jpg'
faces = faceClient.face.detect_with_url(img_url)
print(faces)
```

### <a name="try-the-app"></a>Использование приложения

Запустите приложение с помощью команды `python FaceQuickstart.py`. В окне консоли должен появиться подобный текстовый ответ:

```console
[{'faceId': '26d8face-9714-4f3e-bfa1-f19a7a7aa240', 'faceRectangle': {'top': 124, 'left': 459, 'width': 227, 'height': 227}}]
```

Выходные данные содержат список обнаруженных лиц. Каждый элемент этого списка является экземпляром **dict**, в котором `faceId` представляет собой уникальный идентификатор распознанного лица, а `faceRectangle` описывает расположение этого лица на изображении. 

> [!NOTE]
> Срок действия идентификаторов лиц истекает через 24 часа. Если вы хотите хранить данные о лицах долгосрочно, их необходимо хранить явным образом.

## <a name="draw-face-rectangles"></a>Рисование прямоугольников для лиц

На основе координат, полученных в результате выполнения предыдущей команды, вы можете нанести на изображение прямоугольники, выделяющие каждое распознанное лицо. Вам понадобится установить средство Pillow (`pip install pillow`), чтобы использовать модуль изображений Pillow. В верхней части файла *FaceQuickstart.py* добавьте следующий код:

```python
import requests
from io import BytesIO
from PIL import Image, ImageDraw
```

Затем в нижней части скрипта добавьте следующий код. Этот код создает простую функцию для анализа координат прямоугольника и использует Pillow для рисования прямоугольников на исходном изображении. Затем он отображает изображение в средстве просмотра по умолчанию.

```python
# Convert width height to a point in a rectangle


def getRectangle(faceDictionary):
    rect = faceDictionary['faceRectangle']
    left = rect['left']
    top = rect['top']
    bottom = left + rect['height']
    right = top + rect['width']
    return ((left, top), (bottom, right))


# Download the image from the url
response = requests.get(img_url)
img = Image.open(BytesIO(response.content))

# For each face returned use the face rectangle and draw a red box.
draw = ImageDraw.Draw(img)
for face in faces:
    draw.rectangle(getRectangle(face), outline='red')

# Display the image in the users default image browser.
img.show()
```

## <a name="run-the-app"></a>Запуск приложения

Возможно, сначала вам будет предложено выбрать средство просмотра изображений по умолчанию. Затем появится изображение, подобное следующему. Вы также увидите данные прямоугольника в окне консоли.

![Молодая женщина, вокруг лица которой нарисован красный прямоугольник](../images/face-rectangle-result.png)

## <a name="next-steps"></a>Дополнительная информация

С помощью этого краткого руководства вы изучили базовый процесс использования пакета SDK Python для API распознавания лиц и создали скрипт, в котором лица на изображениях определяются и выделяются рамкой. Теперь можно изучить возможности пакета SDK для Python на более сложном примере. Перейдите к примеру Cognitive Face Python на GitHub, клонируйте его в папку проекта и следуйте инструкциям в файле _README.md_.

> [!div class="nextstepaction"]
> [Пример Cognitive Face Python](https://github.com/Microsoft/Cognitive-Face-Python)
