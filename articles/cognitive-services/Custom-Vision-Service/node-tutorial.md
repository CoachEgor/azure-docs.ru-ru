---
title: Краткое руководство. Создание проекта классификации изображений с помощью пакета SDK Пользовательского визуального распознавания для Node.js
titleSuffix: Azure Cognitive Services
description: Создайте проект, добавьте теги, загрузите изображения, обучите проект и выполните прогнозирование с помощью пакета SDK для Node.js.
services: cognitive-services
author: areddish
manager: daauld
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 12/05/2019
ms.author: areddish
ms.openlocfilehash: f1c0d8f72fe59ff9a8c0fdba86d97ea588a9a808
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366626"
---
# <a name="quickstart-create-an-image-classification-project-with-the-custom-vision-nodejs-sdk"></a>Краткое руководство. Создание проекта классификации изображений с помощью пакета SDK Пользовательского визуального распознавания для Node.js

В этой статье показано, как с помощью пакета SDK Пользовательского визуального распознавания для Node.js создать модель классификации изображений. Создав проект, вы можете добавить теги, загрузить изображения, обучить проект, получить URL-адрес опубликованной конечной точки прогнозирования и с помощью конечной точки программными средствами протестировать изображение. Этот пример можно использовать как шаблон для создания приложения Node.js. Если вы хотите создать модель классификации и использовать ее _без кода_, ознакомьтесь со статьей [Как создать классификатор с помощью Пользовательской службы визуального распознавания](getting-started-build-a-classifier.md).

## <a name="prerequisites"></a>Предварительные требования

- Установленный компонент [Node.js 8](https://www.nodejs.org/en/download/) или более поздней версии.
- Установленный компонент [npm](https://www.npmjs.com/).
- [!INCLUDE [create-resources](includes/create-resources.md)]

## <a name="install-the-custom-vision-sdk"></a>Установка пакета SDK Пользовательского визуального распознавания

Чтобы установить пакет SDK Пользовательского визуального распознавания для Node.js, выполните следующую команду в PowerShell.

```shell
npm install @azure/cognitiveservices-customvision-training
npm install @azure/cognitiveservices-customvision-prediction
```

[!INCLUDE [get-keys](includes/get-keys.md)]

[!INCLUDE [node-get-images](includes/node-get-images.md)]

## <a name="add-the-code"></a>Добавление кода

Создайте файл с именем *sample.js* в необходимом каталоге проекта.

### <a name="create-the-custom-vision-service-project"></a>Создание проекта Пользовательской службы визуального распознавания

Добавьте в скрипт следующий код, чтобы создать проект Пользовательской службы визуального распознавания. Вставьте ключи подписки в соответствующие определения и задайте путь к папке образа в качестве значения пути sampleDataRoot. Убедитесь, что значение endPoint соответствует конечным точкам обучения и прогнозирования, созданным в [Customvision.ai](https://www.customvision.ai/). Обратите внимание, что разница между созданием проекта поиска объектов и классификации изображений состоит в домене, установленном в вызове **createProject**.

```javascript
const util = require('util');
const fs = require('fs');
const TrainingApi = require("@azure/cognitiveservices-customvision-training");
const PredictionApi = require("@azure/cognitiveservices-customvision-prediction");

const setTimeoutPromise = util.promisify(setTimeout);

const trainingKey = "<your training key>";
const predictionKey = "<your prediction key>";
const predictionResourceId = "<your prediction resource id>";
const sampleDataRoot = "<path to image files>";

const endPoint = "https://<my-resource-name>.cognitiveservices.azure.com/"

const publishIterationName = "classifyModel";

const trainer = new TrainingApi.TrainingAPIClient(trainingKey, endPoint);

(async () => {
    console.log("Creating project...");
    const sampleProject = await trainer.createProject("Sample Project")
```

### <a name="create-tags-in-the-project"></a>Создание тегов в проекте

Чтобы создать теги классификации в проекте, добавьте в конец *sample.go* следующий код:

```javascript
const hemlockTag = await trainer.createTag(sampleProject.id, "Hemlock");
const cherryTag = await trainer.createTag(sampleProject.id, "Japanese Cherry");
```

### <a name="upload-and-tag-images"></a>Отправка и снабжение тегами изображений

Чтобы добавить примеры изображений в проект, вставьте следующий код после создания тегов. Этот код загружает каждое изображение с соответствующим тегом. В одном пакете можно передать до 64 изображений.

> [!NOTE]
> Вам также понадобится изменить параметр *sampleDataRoot* на путь к изображениям в зависимости от того, куда вы ранее скачали проект с образцами пакета SDK Cognitive Services для Node.js.

```javascript
console.log("Adding images...");
let fileUploadPromises = [];

const hemlockDir = `${sampleDataRoot}/Hemlock`;
const hemlockFiles = fs.readdirSync(hemlockDir);
hemlockFiles.forEach(file => {
    fileUploadPromises.push(trainer.createImagesFromData(sampleProject.id, fs.readFileSync(`${hemlockDir}/${file}`), { tagIds: [hemlockTag.id] }));
});

const cherryDir = `${sampleDataRoot}/Japanese Cherry`;
const japaneseCherryFiles = fs.readdirSync(cherryDir);
japaneseCherryFiles.forEach(file => {
    fileUploadPromises.push(trainer.createImagesFromData(sampleProject.id, fs.readFileSync(`${cherryDir}/${file}`), { tagIds: [cherryTag.id] }));
});

await Promise.all(fileUploadPromises);
```

### <a name="train-the-classifier-and-publish"></a>Обучение и публикация классификатора

Этот код создает первую итерацию модели прогнозирования и публикует итерацию в конечной точке прогнозирования. Имя, присвоенное опубликованной итерации, можно использовать для отправки запросов на прогнозирование. Итерация недоступна в конечной точке прогнозирования, пока она не будет опубликована.

```javascript
console.log("Training...");
let trainingIteration = await trainer.trainProject(sampleProject.id);

// Wait for training to complete
console.log("Training started...");
while (trainingIteration.status == "Training") {
    console.log("Training status: " + trainingIteration.status);
    await setTimeoutPromise(1000, null);
    trainingIteration = await trainer.getIteration(sampleProject.id, trainingIteration.id)
}
console.log("Training status: " + trainingIteration.status);

// Publish the iteration to the end point
await trainer.publishIteration(sampleProject.id, trainingIteration.id, publishIterationName, predictionResourceId);
```

### <a name="get-and-use-the-published-iteration-on-the-prediction-endpoint"></a>Получение и использование опубликованной итерации в конечной точке прогнозирования

Чтобы отправить изображение в конечную точку прогнозирования и извлечь прогнозирование, добавьте в конец файла следующий код:

```javascript
    const predictor = new PredictionApi.PredictionAPIClient(predictionKey, endPoint);
    const testFile = fs.readFileSync(`${sampleDataRoot}/Test/test_image.jpg`);

    const results = await predictor.classifyImage(sampleProject.id, publishIterationName, testFile);

    // Step 6. Show results
    console.log("Results:");
    results.predictions.forEach(predictedResult => {
        console.log(`\t ${predictedResult.tagName}: ${(predictedResult.probability * 100.0).toFixed(2)}%`);
    });
})()
```

## <a name="run-the-application"></a>Выполнение приложения

Выполните *sample.js*.

```shell
node sample.js
```

Выходные данные приложения должны выглядеть приблизительно так:

```console
Creating project...
Adding images...
Training...
Training started...
Training status: Training
Training status: Training
Training status: Training
Training status: Completed
Results:
         Hemlock: 94.97%
         Japanese Cherry: 0.01%
```

Вы можете убедиться, что тестовое изображение (в **<base_image_url>/Images/Test/** ) помечено соответствующим образом. Вы также можете вернуться к [веб-сайту Пользовательской службы визуального распознавания](https://customvision.ai) и просмотреть текущее состояние созданного проекта.

[!INCLUDE [clean-ic-project](includes/clean-ic-project.md)]

## <a name="next-steps"></a>Дальнейшие действия

Теперь вы узнали, как выполнять в коде каждый шаг процесса классификации изображений. В этом примере выполняется одна итерация обучения, но часто нужно несколько раз обучать и тестировать модель, чтобы сделать ее более точной.

> [!div class="nextstepaction"]
> [Тестирование и переобучение модели с помощью Пользовательской службы визуального распознавания](test-your-model.md)
