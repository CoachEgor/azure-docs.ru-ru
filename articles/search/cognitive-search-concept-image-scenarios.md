---
title: Извлечение текста из изображений
titleSuffix: Azure Cognitive Search
description: Обработка и извлечение текста и другой информации из изображений в конвейерах Azure Cognitive Search.
manager: nitinme
author: LuisCabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 98054060210f55803d6e2811e1f494fd3ff00e48
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "76838264"
---
# <a name="how-to-process-and-extract-information-from-images-in-ai-enrichment-scenarios"></a>Как обрабатывать и извлекать информацию из изображений в сценариях обогащения ИИ

Azure Cognitive Search имеет несколько возможностей для работы с изображениями и файлами изображений. При открытии документа можно использовать параметр *imageAction* для извлечения текста из фотографий и изображений, содержащих буквенно-цифровой текст, например слово "STOP" на изображении соответствующего знака. Другие сценарии включают создание текстового представления изображения, например текста "одуванчик" или "желтый" для изображения одуванчика. Также можно извлечь такие метаданные изображения, как его размер.

Эта статья более подробно охватывает обработку изображений и содержит рекомендации по работе с изображениями в конвейере обогащения ИИ.

<a name="get-normalized-images"></a>

## <a name="get-normalized-images"></a>Получение нормализованных изображений

При открытии документа можно использовать новый набор параметров конфигурации индексатора для обработки файлов изображений или изображений, внедренных в файлы. Эти параметры используются для нормализации изображений для их дальнейшей обработки. Нормализация делает изображения более удобными для использования. Например, большие изображения уменьшаются до допустимых для использования размеров. Для изображений с метаданными об ориентации используется вертикальная загрузка. Изменения метаданных записываются в сложном типе, создаваемом для каждого изображения. 

Нормализацию изображений отключить нельзя. Для навыков, которые обрабатывают изображения, требуются нормализованные изображения. Включение нормализации изображения на индексаторе требует, чтобы к этому индексатору был прикреплен набор навыков.

| Параметр конфигурации | Описание |
|--------------------|-------------|
| imageAction   | Установите значение none, чтобы не выполнять какие-либо действия при обнаружении внедренных изображений или файлов изображений. <br/>Установите значение generateNormalizedImages, чтобы создать массив нормализованных изображений в рамках открытия документа.<br/>Установить для "generateNormalizedImagePerPage" для создания массива нормализованных изображений, где для PDF в источнике данных, каждая страница отображается на одном изображении вывода.  Функциональность ничем не отличается от значения generateNormalizedImages в типах файлов, отличных от PDF.<br/>Для любого значения параметра, которое отличается от "none", изображения будут отображаться в поле *normalized_images*. <br/>Значение по умолчанию — none. Эта конфигурация используется только для источников данных BLOB-объектов, если для параметра dataToExtract задано значение contentAndMetadata. <br/>Из данного документа будет извлечено не более 1000 изображений. Если в документе содержится более 1000 изображений, первые 1000 будут извлечены и будет получено предупреждение. |
|  normalizedImageMaxWidth | Максимальная ширина (в пикселях) для созданного нормализованного изображения. Значение по умолчанию — 2000. Максимальное допустимое значение составляет 10000. | 
|  normalizedImageMaxHeight | Максимальная высота (в пикселях) для созданных нормализованных изображений. Значение по умолчанию — 2000. Максимальное допустимое значение составляет 10000.|

> [!NOTE]
> Если вы установите свойство *imageAction* на что-либо, кроме "никто", вы не сможете установить свойство *parsingMode* на что-либо, кроме "по умолчанию".  В конфигурации индексатора настраиваемое значение можно задать только одному свойству.

Задайте для параметра **parsingMode** значение `json` (для индексации каждого большого двоичного объекта в виде единого документа) или `jsonArray` (если большой двоичный объект содержит массивы JSON и требуется каждый элемент массива обрабатывать как отдельный документ).

По умолчанию навыками [OCR](cognitive-search-skill-ocr.md) и [анализа изображений](cognitive-search-skill-image-analysis.md) поддерживаются нормализованные изображения с максимальными шириной и высотой в 2000 пикселей. [Навык OCR](cognitive-search-skill-ocr.md) поддерживает максимальную ширину и высоту 4200 для неанглоязычных языков и 10000 для английского языка.  Если увеличить максимальные лимиты, обработка может вывести из строя большие изображения в зависимости от определения набора навыков и языка документов. 

Настройте параметр imageAction в [определении индексатора](https://docs.microsoft.com/rest/api/searchservice/create-indexer) следующим образом:

```json
{
  //...rest of your indexer definition goes here ...
  "parameters":
  {
    "configuration": 
    {
        "dataToExtract": "contentAndMetadata",
        "imageAction": "generateNormalizedImages"
    }
  }
}
```

Если параметру *imageAction* задать значение отменное от "none", новое поле *normalized_images* будет содержать массив изображений. Каждое изображение представляет сложный тип со следующими элементами:

| Элемент изображения       | Описание                             |
|--------------------|-----------------------------------------|
| .               | Строка нормализованного изображения в формате JPEG в кодировке Base64.   |
| width              | Ширина нормализованного изображения в пикселях. |
| height             | Высота нормализованного изображения в пикселях. |
| originalWidth      | Исходная ширина изображения перед нормализацией. |
| originalHeight      | Исходная высота изображения перед нормализацией. |
| rotationFromOriginal |  Поворот против часовой стрелки (в градусах), выполняемый для создания нормализованного изображения. Значение от 0 до 360 градусов. На этом этапе считываются метаданные изображения, созданного камерой или сканером. Обычно поворот выполняется несколько раз на 90 градусов. |
| contentOffset | Смещение символов в поле содержимого, откуда было извлечено изображение. Это поле используется только для файлов с внедренными изображениями. |
| pageНомер | Если изображение было извлечено или отрисовано из PDF, это поле содержит номер страницы в PDF, из котором оно было извлечено или отрисовано, начиная с 1.  Если изображение не было из PDF, это поле будет 0.  |

 Пример значения *normalized_images*:
```json
[
  {
    "data": "BASE64 ENCODED STRING OF A JPEG IMAGE",
    "width": 500,
    "height": 300,
    "originalWidth": 5000,  
    "originalHeight": 3000,
    "rotationFromOriginal": 90,
    "contentOffset": 500,
    "pageNumber": 2
  }
]
```

## <a name="image-related-skills"></a>Навыки, связанные с изображениями

Есть два встроенных когнитивных навыка, которые принимают изображения в качестве входных данных: [OCR](cognitive-search-skill-ocr.md) и [Анализ образов](cognitive-search-skill-image-analysis.md). 

Сейчас эти навыки используются только с изображениями, созданными на шаге открытия документа. Таким образом, поддерживаемые входные данные — это только `"/document/normalized_images"`.

### <a name="image-analysis-skill"></a>Навыки, связанные с анализом изображений

[Навык анализа изображений](cognitive-search-skill-image-analysis.md) извлекает богатый набор визуальных особенностей на основе содержания изображения. Например, можно создать теги или заголовок для изображения, а также определить знаменитостей или ориентиры.

### <a name="ocr-skill"></a>Навык OCR

[Навык OCR](cognitive-search-skill-ocr.md) извлекает текст из файлов изображений, например JPG, PNG и растровых изображений. Он может извлекать текст и сведения о макете. Сведения о макете предоставляют ограничивающие прямоугольники для каждой из определенных строк.

## <a name="embedded-image-scenario"></a>Сценарий с внедренными изображениями

Типичный сценарий предусматривает создание одной строки, содержащей все содержимое файла (текст и текст, полученный из изображения), следующим образом:  

1. [Извлеките normalized_images](#get-normalized-images).
1. Запустите навык OCR, используя `"/document/normalized_images"` как входные данные.
1. Объедините текстовое представление и изображения с необработанным текстом, извлеченные из файла. Можно использовать навык [слияния текста](cognitive-search-skill-textmerger.md) для объединения обоих фрагментов текста в одну большую строку.

В следующем примере набор навыков создает поле *merged_text*, содержащее текстовое содержимое документа. Он также включает текст OCRed, полученный из каждого внедренного изображения. 

#### <a name="request-body-syntax"></a>Синтаксис текста запроса
```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
        "description": "Extract text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": "en",
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text"
          }
        ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " ",
      "inputs": [
        {
          "name":"text", "source": "/document/content"
        },
        {
          "name": "itemsToInsert", "source": "/document/normalized_images/*/text"
        },
        {
          "name":"offsets", "source": "/document/normalized_images/*/contentOffset" 
        }
      ],
      "outputs": [
        {
          "name": "mergedText", "targetName" : "merged_text"
        }
      ]
    }
  ]
}
```

Теперь, когда у вас есть поле merged_text, можно включить его как доступное для поиска поле в определение индексатора. Все содержимое файлов, включая текст изображений, будет доступно для поиска.

## <a name="visualize-bounding-boxes-of-extracted-text"></a>Визуализация ограничивающих прямоугольников извлеченного текста

Другой распространенный сценарий — визуализация сведений о макете результатов поиска. Например, можно выделить участок с обнаруженным в изображении фрагментом текста как часть результатов поиска.

Так как шаг OCR выполняется с нормализованными изображениями, координаты макета указываются в пространстве таких изображений. При отображении нормализованного изображения наличие координат обычно не является проблемой. Но в некоторых ситуациях нужно отобразить исходное изображение. В этом случае преобразуйте все точки координат в макете в систему координат исходного изображения. 

Для преобразования нормализованных координат в исходное пространство координат можно использовать следующий алгоритм:

```csharp
        /// <summary>
        ///  Converts a point in the normalized coordinate space to the original coordinate space.
        ///  This method assumes the rotation angles are multiples of 90 degrees.
        /// </summary>
        public static Point GetOriginalCoordinates(Point normalized,
                                    int originalWidth,
                                    int originalHeight,
                                    int width,
                                    int height,
                                    double rotationFromOriginal)
        {
            Point original = new Point();
            double angle = rotationFromOriginal % 360;

            if (angle == 0 )
            {
                original.X = normalized.X;
                original.Y = normalized.Y;
            } else if (angle == 90)
            {
                original.X = normalized.Y;
                original.Y = (width - normalized.X);
            } else if (angle == 180)
            {
                original.X = (width -  normalized.X);
                original.Y = (height - normalized.Y);
            } else if (angle == 270)
            {
                original.X = height - normalized.Y;
                original.Y = normalized.X;
            }

            double scalingFactor = (angle % 180 == 0) ? originalHeight / height : originalHeight / width;
            original.X = (int) (original.X * scalingFactor);
            original.Y = (int)(original.Y * scalingFactor);

            return original;
        }
```

## <a name="see-also"></a>См. также
+ [Создание индекса (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
+ [Навыки, связанные с анализом изображений](cognitive-search-skill-image-analysis.md)
+ [Навык OCR](cognitive-search-skill-ocr.md)
+ [Навык объединения текста](cognitive-search-skill-textmerger.md)
+ [Определение набора навыков](cognitive-search-defining-skillset.md)
+ [Сопоставление обогащенных полей](cognitive-search-output-field-mapping.md)
