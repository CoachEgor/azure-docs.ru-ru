---
title: Подключение приложения Mongoose Node.js к Azure Cosmos DB
description: Узнайте, как использование платформу Mongoose для хранения и администрирования данных в Azure Cosmos DB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 12/26/2018
author: sivethe
ms.author: sivethe
ms.custom: seodec18
ms.openlocfilehash: 3955b84df401e5832668fa091274caea9af2466e
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876602"
---
# <a name="connect-a-nodejs-mongoose-application-to-azure-cosmos-db"></a>Подключение приложения Mongoose Node.js к Azure Cosmos DB

Это руководство содержит сведения об использовании [платформы Mongoose](https://mongoosejs.com/) при сохранении данных в Cosmos DB. В этом пошаговом руководстве используется API Azure Cosmos DB для MongoDB. Для тех из вас, кто еще не знаком с Mongoose, — это платформа объектного моделирования для MongoDB в Node.js, которая представляет собой простое решение на основе схемы для моделирования данных приложения.

Cosmos DB — это глобально распределенная многомодельная служба базы данных Майкрософт. Вы можете быстро создавать и запрашивать документы, пары "ключ — значение" и базы данных графов, используя преимущества возможностей глобального распределения и горизонтального масштабирования базы данных Cosmos DB.

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

[Node.js](https://nodejs.org/) версии v0.10.29 или выше.

## <a name="create-a-cosmos-account"></a>Создание учетной записи Cosmos

Создадим учетную запись Cosmos. Если у вас уже есть учетная запись, которую вы собираетесь использовать, можно перейти к шагу "Настройка приложения Node.js". Если вы используете эмулятор Azure Cosmos DB, выполните действия, описанные в [этой статье](local-emulator.md), чтобы настроить эмулятор и сразу перейти к настройке приложения Node.js.

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="set-up-your-nodejs-application"></a>Настройка приложения Node.js

>[!Note]
> Если вы просто хотите ознакомиться с примером кода, а не настраивать приложение, клонируйте [пример](https://github.com/Azure-Samples/Mongoose_CosmosDB), используемый для этого руководства, и создайте приложение Mongoose на Node.js в Azure Cosmos DB.

1. Чтобы создать приложение Node.js в выбранной папке, выполните следующую команду в командной строке узла.

    ```npm init```

    Ответьте на вопросы, чтобы приступить к работе с проектом.

1. Добавьте новый файл в папку и назовите его ```index.js```.
1. Установите необходимые пакеты, используя один из параметров ```npm install```.
   * Mongoose: ```npm install mongoose@5 --save```.

     > [!Note]
     > Подключение примера Mongoose, приведенного ниже, основанно на Mongoose 5+. Процесс подключения изменился с выходом более поздних версий.
    
   * Dotenv (если вы хотите загрузить секреты из ENV-файла): ```npm install dotenv --save```.

     >[!Note]
     > Параметр ```--save``` позволяет добавить зависимость в файл package.json.

1. Импортируйте зависимости в файл index.js.
    ```JavaScript
    var mongoose = require('mongoose');
    var env = require('dotenv').load();    //Use the .env file to load the variables
    ```

1. Добавьте строку подключения и имя Cosmos DB в файл ```.env```. Замените заполнители {Cosmos-Account-Name} и {dbname} собственным именем учетной записи Cosmos и именем базы данных, без символов фигурных скобок.

    ```JavaScript
    COSMOSDB_CONNSTR=mongodb://{cosmos-account-name}.documents.azure.com:10255/{dbname}
    COSMODDB_USER=cosmos-account-name
    COSMOSDB_PASSWORD=cosmos-secret
    ```

1. Подключитесь к Cosmos DB с помощью платформы Mongoose, добавив следующий код в конец файла index.js.
    ```JavaScript
    mongoose.connect(process.env.COSMOSDB_CONNSTR+"?ssl=true&replicaSet=globaldb", {
      auth: {
        user: process.env.COSMODDB_USER,
        password: process.env.COSMOSDB_PASSWORD
      }
    })
    .then(() => console.log('Connection to CosmosDB successful'))
    .catch((err) => console.error(err));
    ```
    >[!Note]
    > Здесь переменные среды загружаются в виде process.env.{имя_переменной}, используя пакет npm dotenv.

    Подключившись к Azure Cosmos DB, вы можете приступить к настройке объектных моделей в Mongoose.

## <a name="caveats-to-using-mongoose-with-cosmos-db"></a>Предостережения относительно использования Mongoose с Cosmos DB

Для каждой создаваемой вами модели Mongoose создает новую коллекцию. Тем не менее, учитывая модель выставления счетов Cosmos DB с тарификацией за каждую коллекцию, это может оказаться не самым выгодным решением, если у вас есть несколько объектных моделей, которые неплотно заполнены.

В этом руководстве рассматриваются обе модели. Сначала ознакомимся с пошаговым руководством по хранению одного типа данных в коллекции. Это фактическое поведение для Mongoose.

Mongoose также использует концепцию, называемую [Дискриминаторы](https://mongoosejs.com/docs/discriminators.html). "Дискриминаторы" — это механизм наследования схемы. Он позволяет иметь несколько моделей с пересекающимися схемами поверх той же базовой коллекции MongoDB.

Вы можете хранить различные модели данных в одной и той же коллекции, а затем использовать предложения фильтра во время запроса, чтобы извлекать только необходимые данные.

### <a name="one-collection-per-object-model"></a>Одна коллекция на объектную модель

Поведение Mongoose по умолчанию — создавать коллекцию MongoDB каждый раз, когда создается объектная модель. В этом разделе описано, как это сделать с помощью API Azure Cosmos DB для MongoDB. Этот способ рекомендуется, если у вас есть объектные модели с большими объемами данных. Это стандартная операционная модель для Mongoose, поэтому вы можете быть с ней знакомы, если работали с Mongoose.

1. Откройте ```index.js``` еще раз.

1. Создайте определение схемы Family (семья).

    ```JavaScript
    const Family = mongoose.model('Family', new mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
            gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }));
    ```

1. Создайте объект Family (семья).

    ```JavaScript
    const family = new Family({
        lastName: "Volum",
        parents: [
            { firstName: "Thomas" },
            { firstName: "Mary Kay" }
        ],
        children: [
            { firstName: "Ryan", gender: "male", grade: 8 },
            { firstName: "Patrick", gender: "male", grade: 7 }
        ],
        pets: [
            { givenName: "Blackie" }
        ],
        address: { country: "USA", state: "WA", city: "Seattle" }
    });
    ```

1. Наконец, сохраните объект в Cosmos DB. При этом в фоновом режиме будет создана коллекция.

    ```JavaScript
    family.save((err, saveFamily) => {
        console.log(JSON.stringify(saveFamily));
    });
    ```

1. Теперь создайте другие схему и объект. В этот раз создайте их для мест отдыха (VacationDestinations), которые могут заинтересовать семьи.
   1. Как и в прошлом примере, создайте схему.
      ```JavaScript
      const VacationDestinations = mongoose.model('VacationDestinations', new mongoose.Schema({
       name: String,
       country: String
      }));
      ```

   1. Создайте и сохраните пример объекта (к этой схеме можно добавить несколько объектов).
      ```JavaScript
      const vacaySpot = new VacationDestinations({
       name: "Honolulu",
       country: "USA"
      });

      vacaySpot.save((err, saveVacay) => {
       console.log(JSON.stringify(saveVacay));
      });
      ```

1. Теперь при входе на портал Azure вы увидите две коллекции, созданные в Cosmos DB.

    ![Руководство по Node. js. снимок экрана портал Azure, отображающий учетную запись Azure Cosmos DB, с несколькими именами коллекций, выделенными — база данных узла][multiple-coll]

1. Наконец, выполните считывание данных из Cosmos DB. Так как используется стандартная операционная модель Mongoose, операции чтения идентичны таким же операциям с Mongoose.

    ```JavaScript
    Family.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family: " + JSON.stringify(fam)));
    });
    ```

### <a name="using-mongoose-discriminators-to-store-data-in-a-single-collection"></a>Хранение данных в одной коллекции с помощью дискриминаторов Mongoose

Чтобы оптимизировать затраты каждой коллекции, в этом методе используются [дискриминаторы Mongoose](https://mongoosejs.com/docs/discriminators.html). Дискриминаторы дают возможность определить отличительный ключ, который позволяет хранить, различать и фильтровать разные объектные модели.

В этом примере вы создадите базовую объектную модель, определите отличительный ключ и добавите Family и VacationDestinations в качестве расширения к базовой модели.

1. Настройте базовую конфигурацию и определите ключ дискриминатора.

    ```JavaScript
    const baseConfig = {
        discriminatorKey: "_type", //If you've got a lot of different data types, you could also consider setting up a secondary index here.
        collection: "alldata"   //Name of the Common Collection
    };
    ```

1. Затем определите общую объектную модель.

    ```JavaScript
    const commonModel = mongoose.model('Common', new mongoose.Schema({}, baseConfig));
    ```

1. Теперь определите модель Family. Обратите внимание, что вместо ```mongoose.model``` используется ```commonModel.discriminator```. Кроме того, к схеме Mongoose добавлена базовая конфигурация. Итак, ```FamilyType``` — это ключ дискриминатора.

    ```JavaScript
    const Family_common = commonModel.discriminator('FamilyType', new     mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
           gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }, baseConfig));
    ```

1. Аналогичным образом добавьте другую схему, на этот раз для VacationDestinations. В этом случае ключ дискриминатора — это ```VacationDestinationsType```.

    ```JavaScript
    const Vacation_common = commonModel.discriminator('VacationDestinationsType', new mongoose.Schema({
        name: String,
        country: String
    }, baseConfig));
    ```

1. Наконец, создайте объекты для модели и сохраните ее.
   1. Добавьте объекты к модели Family.
      ```JavaScript
      const family_common = new Family_common({
       lastName: "Volum",
       parents: [
           { firstName: "Thomas" },
           { firstName: "Mary Kay" }
       ],
       children: [
           { firstName: "Ryan", gender: "male", grade: 8 },
           { firstName: "Patrick", gender: "male", grade: 7 }
       ],
       pets: [
           { givenName: "Blackie" }
       ],
       address: { country: "USA", state: "WA", city: "Seattle" }
      });

      family_common.save((err, saveFamily) => {
       console.log("Saved: " + JSON.stringify(saveFamily));
      });
      ```

   1. Затем добавьте объекты к модели VacationDestinations и сохраните ее.
      ```JavaScript
      const vacay_common = new Vacation_common({
       name: "Honolulu",
       country: "USA"
      });

      vacay_common.save((err, saveVacay) => {
       console.log("Saved: " + JSON.stringify(saveVacay));
      });
      ```

1. Теперь, если вернуться на портал Azure, вы заметите, что у вас есть лишь одна коллекция ```alldata``` с данными Family и VacationDestinations.

    ![Руководство по Node. js. снимок экрана портал Azure, отображающий учетную запись Azure Cosmos DB, с именем коллекции, выделенной для базы данных узла][alldata]

1. Кроме того, обратите внимание, что каждый объект имеет еще один атрибут с именем ```__type```, который позволяет различать две разные объектные модели.

1. Наконец, выполните считывание данных, хранящихся в Azure Cosmos DB. Mongoose позволяет фильтровать данные на основе модели. Таким образом, вам не нужно будет выполнять другие действия при считывании данных. Просто укажите свою модель (в этом случае ```Family_common```), и Mongoose выполнит фильтрацию по DiscriminatorKey.

    ```JavaScript
    Family_common.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family (using discriminator): " + JSON.stringify(fam)));
    });
    ```

Как видите, работать с дискриминаторами Mongoose очень легко. Если у вас есть приложение, использующее платформу Mongoose, это руководство поможет настроить и запустить его с помощью API Azure Cosmos DB для MongoDB без большого количества изменений.

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Следующие шаги

- Узнайте, как [использовать Studio 3T](mongodb-mongochef.md) с API Azure Cosmos DB для MongoDB.
- Узнайте, как [использовать Robo 3T](mongodb-robomongo.md) с API Azure Cosmos DB для MongoDB.
- Ознакомьтесь с [примерами](mongodb-samples.md) MongoDB с API Azure Cosmos DB для MongoDB.

[alldata]: ./media/mongodb-mongoose/mongo-collections-alldata.png
[multiple-coll]: ./media/mongodb-mongoose/mongo-mutliple-collections.png
