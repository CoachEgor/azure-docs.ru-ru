---
title: Подключение к Azure Cosmos DB с помощью Compass
description: Узнайте, как использовать MongoDB Compass для хранения и администрирования данных в Azure Cosmos DB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 03/20/2020
author: LuisBosquez
ms.author: lbosq
ms.openlocfilehash: c683ec0c4b3a536b0627a7c1c8abf28ee4f83663
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "80757030"
---
# <a name="use-mongodb-compass-to-connect-to-azure-cosmos-dbs-api-for-mongodb"></a>Подключение к API Azure Cosmos DB для MongoDB с помощью MongoDB Compass

Этот учебник содержит сведения об использовании [MongoDB Compass](https://www.mongodb.com/products/compass) при хранении и администрировании данных в Cosmos DB. В этом пошаговом руководстве используется API Azure Cosmos DB для MongoDB. Для тех из вас, кто еще не знаком с Compass, — это графический пользовательский интерфейс для MongoDB. Обычно он используется для визуализации данных, выполнения ad-hoc-запросов, наряду с управлением данными.

Cosmos DB — это глобально распределенная многомодельная служба базы данных Майкрософт. Вы можете быстро создавать и запрашивать документы, пары "ключ-значение" и графовые базы данных, используя преимущества глобального распределения и горизонтального масштабирования Cosmos DB.

## <a name="pre-requisites"></a>Предварительные требования

Чтобы подключиться к учетной записи Cosmos DB с помощью MongoDB Compass, необходимо:

* Скачайте и установите [Compass](https://www.mongodb.com/download-center/compass?jmp=hero).
* Получите сведений о [строке подключения](connect-mongodb-account.md) Cosmos DB.

> [!NOTE]
> В настоящее время API API Azure Cosmos DB для версии MongoDB Server 3.2 поддерживается MongoDB Compass.

## <a name="connect-to-cosmos-dbs-api-for-mongodb"></a>Подключение к API Cosmos DB для MongoDB

Чтобы подключить учетную запись Cosmos DB к Compass, выполните следующие шаги:

1. Получите сведения о подключении для учетной записи Cosmos, настроенной с помощью API Azure Cosmos DB для MongoDB, следуя инструкциям [здесь](connect-mongodb-account.md).

    ![Снимок экрана колонки строки подключения](./media/mongodb-compass/mongodb-compass-connection.png)

2. Нажмите кнопку с текстом **Копировать в буфер обмена** рядом с **основной или дополнительной строкой подключения** в Cosmos DB. После нажатия этой кнопки строка подключения будет полностью скопирована в буфер обмена.

    ![Снимок экрана кнопки копирования в буфер обмена](./media/mongodb-compass/mongodb-connection-copy.png)

3. Откройте Compass на рабочем столе или компьютере и щелкните **Подключить**, а затем **Подключение к...**.

4. Compass автоматически обнаруживает строку подключения в буфере обмена и спросит, хотите ли вы ее использовать для подключения. Щелкните **Да**, как показано на следующем снимке экрана.

    ![Снимок экрана запроса Compass на подключение](./media/mongodb-compass/mongodb-compass-detect.png)

5. После нажатия кнопки **Да** на предыдущем шаге данные из строки подключения будут заполнены автоматически. Удалите автоматически заполненное значение в поле **Replica Set Name** (Имя набора реплик), убедитесь, что поле пустое.

    ![Снимок экрана запроса Compass на подключение](./media/mongodb-compass/mongodb-compass-replica.png)

6. В нижней части страницы щелкните **Подключиться**. Базы данных и учетная запись Cosmos DB теперь должны отображаться в MongoDB Compass.

## <a name="next-steps"></a>Дальнейшие действия

- Узнайте, как [использовать Studio 3T](mongodb-mongochef.md) с API Azure Cosmos DB для MongoDB.
- Ознакомьтесь с [примерами](mongodb-samples.md) MongoDB с API Azure Cosmos DB для MongoDB.
