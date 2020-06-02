---
title: Обнаружение лиц на изображении
titleSuffix: Azure Cognitive Services
description: В этом руководстве показано, как использовать обнаружение лиц для извлечения таких атрибутов, как Gender, Age или of a, из определенного изображения.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: sbowles
ms.openlocfilehash: a3fd308ede17f510b3e9188f2bebc93a08a61181
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/01/2020
ms.locfileid: "84263197"
---
# <a name="get-face-detection-data"></a>Получение данных обнаружения лиц

В этом руководстве показано, как использовать обнаружение лиц для извлечения таких атрибутов, как Gender, Age или of a, из определенного изображения. Фрагменты кода в этом руководство написаны на языке C# с помощью клиентской библиотеки Azure Cognitive Services Face. Те же функциональные возможности доступны в [REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

В этом учебнике показано, как:

- Получение расположений и размеров граней в изображении.
- Получите расположения различных ориентиров, таких как пупилс, нос и рот, в образе.
- Догадаться пол, возраст, распознавания эмоций и другие атрибуты обнаруженного лица.

## <a name="setup"></a>Установка

В этом учебнике предполагается, что вы уже создавали объект [фацеклиент](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet) `faceClient` с именем с ключом подписки лица и URL-адресом конечной точки. Здесь можно использовать функцию обнаружения лиц, вызвав либо [детектвисурласинк](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithurlasync?view=azure-dotnet), которая используется в этом руководством, либо [детектвисстреамасинк](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithstreamasync?view=azure-dotnet). Инструкции по настройке этой функции см. в одном из кратких руководств.

В этом разделе рассматриваются особенности вызова метода обнаружения, например, какие аргументы можно передавать и что можно делать с возвращаемыми данными. Рекомендуется запрашивать только необходимые компоненты. Для выполнения каждой операции требуется дополнительное время.

## <a name="get-basic-face-data"></a>Получение основных данных о грани

Чтобы найти грани и получить их расположения в изображении, вызовите метод с параметром _ретурнфацеид_ , для которого задано значение **true**. Это значение по умолчанию.

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, false, null);
```

Вы можете запросить возвращенные объекты [детектедфаце](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet) для их уникальных идентификаторов и прямоугольника, который дает координаты точки.

```csharp
foreach (var face in faces)
{
    string id = face.FaceId.ToString();
    FaceRectangle rect = face.FaceRectangle;
}
```

Сведения о том, как анализировать расположение и размеры лица, см. в разделе [фацеректангле](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.facerectangle?view=azure-dotnet). Обычно этот прямоугольник содержит глаза, бровей, нос и рот. Верхняя часть Head, ушки и Чин не обязательно включена. Чтобы использовать прямоугольник лицевой стороны для обрезки полного заголовка или получения середины рисунка книжной ориентации, возможно, для изображения типа "идентификатор фотографии", можно развернуть прямоугольник в каждом направлении.

## <a name="get-face-landmarks"></a>Получение ориентиров для лиц

[Ориентиры](../concepts/face-detection.md#face-landmarks) — это набор удобных для поиска точек на лицевой стороне, например пупилс или Совет нос. Чтобы получить данные ориентиров, установите для параметра _ретурнфацеландмаркс_ значение **true**.

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, true, null);
```

В следующем коде показано, как можно извлечь расположения нос и пупилс:

```csharp
foreach (var face in faces)
{
    var landmarks = face.FaceLandmarks;

    double noseX = landmarks.NoseTip.X;
    double noseY = landmarks.NoseTip.Y;

    double leftPupilX = landmarks.PupilLeft.X;
    double leftPupilY = landmarks.PupilLeft.Y;

    double rightPupilX = landmarks.PupilRight.X;
    double rightPupilY = landmarks.PupilRight.Y;
}
```

Можно также использовать данные ориентиров для точного вычисления направления лица. Например, можно определить поворот грани в виде вектора от центра рот до центра глаз. Следующий код вычисляет этот вектор:

```csharp
var upperLipBottom = landmarks.UpperLipBottom;
var underLipTop = landmarks.UnderLipTop;

var centerOfMouth = new Point(
    (upperLipBottom.X + underLipTop.X) / 2,
    (upperLipBottom.Y + underLipTop.Y) / 2);

var eyeLeftInner = landmarks.EyeLeftInner;
var eyeRightInner = landmarks.EyeRightInner;

var centerOfTwoEyes = new Point(
    (eyeLeftInner.X + eyeRightInner.X) / 2,
    (eyeLeftInner.Y + eyeRightInner.Y) / 2);

Vector faceDirection = new Vector(
    centerOfTwoEyes.X - centerOfMouth.X,
    centerOfTwoEyes.Y - centerOfMouth.Y);
```

Если вы знакомы с направлением лица, вы можете повернуть прямоугольную рамку, чтобы правильно выстроить ее. Чтобы обрезать фрагменты изображения, можно программно повернуть изображение таким образом, чтобы они всегда отображались вертикально.

## <a name="get-face-attributes"></a>Получение атрибутов лица

Помимо прямоугольников и ориентиров, API обнаружения лиц может анализировать несколько концептуальных атрибутов лица. Полный список см. в разделе Общие сведения о [атрибутах лиц](../concepts/face-detection.md#attributes) .

Чтобы проанализировать атрибуты лиц, задайте для параметра _ретурнфацеаттрибутес_ список значений [перечисления фацеаттрибутетипе](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet) .

```csharp
var requiredFaceAttributes = new FaceAttributeType[] {
    FaceAttributeType.Age,
    FaceAttributeType.Gender,
    FaceAttributeType.Smile,
    FaceAttributeType.FacialHair,
    FaceAttributeType.HeadPose,
    FaceAttributeType.Glasses,
    FaceAttributeType.Emotion
};
var faces = await faceClient.DetectWithUrlAsync(imageUrl, true, false, requiredFaceAttributes);
```

Затем получите ссылки на возвращенные данные и выполните больше операций в соответствии с вашими потребностями.

```csharp
foreach (var face in faces)
{
    var attributes = face.FaceAttributes;
    var age = attributes.Age;
    var gender = attributes.Gender;
    var smile = attributes.Smile;
    var facialHair = attributes.FacialHair;
    var headPose = attributes.HeadPose;
    var glasses = attributes.Glasses;
    var emotion = attributes.Emotion;
}
```

Дополнительные сведения о каждом из атрибутов см. в разделе Общие сведения об [обнаружении и атрибутах лиц](../concepts/face-detection.md) .

## <a name="next-steps"></a>Дальнейшие действия

В этом руководство вы узнали, как использовать различные функции обнаружения лиц. Затем интегрируйте эти функции в приложение, выполнив подробное руководство.

- [Руководство. Создание приложения WPF для отображения данных о лицах на изображении](../Tutorials/FaceAPIinCSharpTutorial.md)

## <a name="related-topics"></a>См. также

- [Справочная документация (ОСТАВШАЯся)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
- [Справочная документация (пакет SDK для .NET)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/faceapi?view=azure-dotnet)