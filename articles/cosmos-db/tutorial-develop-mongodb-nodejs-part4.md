---
title: Создание приложения Angular с помощью API Azure Cosmos DB для MongoDB (часть 1)
description: Часть 4 серии руководств по созданию в Azure Cosmos DB приложения MongoDB с помощью Angular и Node и тех же API, которые используются для MongoDB.
author: johnpapa
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 12/06/2018
ms.author: jopapa
ms.custom: seodec18
ms.reviewer: sngun
ms.openlocfilehash: d5ed706a0577423f0a95b8eb5f7a563da74a0e7f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "75441168"
---
# <a name="create-an-angular-app-with-azure-cosmos-dbs-api-for-mongodb---create-a-cosmos-account"></a>Создание приложения Angular с помощью API Azure Cosmos DB для MongoDB. Создание учетной записи Cosmos

Из этого руководства в нескольких частях вы узнаете, как создать новое приложение, написанное на Node.js с использованием Express и Angular, а затем подключить его к [учетной записи Cosmos, настроенной с помощью API Cosmos DB для MongoDB](mongodb-introduction.md).

Часть 4 руководства основана на [части 3](tutorial-develop-mongodb-nodejs-part3.md). Здесь рассматриваются следующие задачи:

> [!div class="checklist"]
> * создание группы ресурсов Azure с помощью Azure CLI;
> * создание учетной записи Cosmos с помощью Azure CLI.

## <a name="video-walkthrough"></a>Видеоруководство

> [!VIDEO https://www.youtube.com/embed/hfUM-AbOh94]

## <a name="prerequisites"></a>предварительные требования

Перед переходом к этой части руководства убедитесь, что выполнены все задачи из [части 3](tutorial-develop-mongodb-nodejs-part3.md). 

В этом разделе руководства можно использовать Azure Cloud Shell (в браузере) или установленный локально [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)]

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)]

> [!TIP]
> В этом руководстве изложены пошаговые инструкции по созданию приложения. Готовое приложение можно скачать из [репозитория angular-cosmosdb](https://github.com/Azure-Samples/angular-cosmosdb) на GitHub.

## <a name="create-an-azure-cosmos-db-account"></a>создание учетной записи Azure Cosmos DB;

Создайте учетную запись Azure Cosmos DB с помощью команды [`az cosmosdb create`](/cli/azure/cosmosdb#az-cosmosdb-create).

```azurecli-interactive
az cosmosdb create --name <cosmosdb-name> --resource-group myResourceGroup --kind MongoDB
```

* Для `<cosmosdb-name>` используйте уникальное имя учетной записи Azure Cosmos DB. Имя должно быть уникальным среди всех учетных записей Azure Cosmos DB в Azure.
* Параметр `--kind MongoDB` разрешает Azure Cosmos DB использовать клиентские подключения MongoDB.

Выполнение команды может занять одну-две минуты. По завершении в окне терминала отобразятся сведения о новой базе данных. 

Создав учетную запись Azure Cosmos DB, сделайте следующее.
1. Откройте новое окно браузера и перейдите на портал по адресу [https://portal.azure.com](https://portal.azure.com).
1. Щелкните логотип Azure Cosmos DB ![Значок Azure Cosmos DB на портале Azure](./media/tutorial-develop-mongodb-nodejs-part4/azure-cosmos-db-icon.png) на левой панели. Отобразятся все ваши базы данных Azure Cosmos DB.
1. Выберите учетную запись Azure Cosmos DB, которую вы только что создали, перейдите на вкладку **Обзор** и прокрутите страницу вниз, чтобы просмотреть карту расположения базы данных. 

    ![Новая учетная запись Azure Cosmos DB на портале Azure](./media/tutorial-develop-mongodb-nodejs-part4/azure-cosmos-db-angular-portal.png)

4. Прокрутите вниз в области навигации слева и щелкните вкладку **Глобальная репликация данных**. Отобразится карта с регионами, в которые можно реплицировать данные. Например, можете щелкнуть регион "Юго-восточная Австралия" или "Восточная Австралия" и реплицировать свои данные в Австралию. Дополнительные сведения о глобальной репликации см. в статье [Как работает глобальное распределение данных в Azure с помощью Cosmos DB](distribute-data-globally.md). Пока что давайте просто сохраним один экземпляр, а если потребуется репликация, мы уже будем знать, как это сделать.

    ![Новая учетная запись Azure Cosmos DB на портале Azure](./media/tutorial-develop-mongodb-nodejs-part4/azure-cosmos-db-replicate-portal.png)

## <a name="next-steps"></a>Дальнейшие действия

В этой части руководства мы выполнили следующую задачу:

> [!div class="checklist"]
> * создали группу ресурсов Azure с помощью Azure CLI;
> * создали учетную запись Azure Cosmos DB с помощью Azure CLI.

Теперь можно приступить к следующей части руководства, чтобы подключить учетную запись Azure Cosmos DB к приложению с помощью Mongoose.

> [!div class="nextstepaction"]
> [подключение к Azure Cosmos DB с помощью Mongoose](tutorial-develop-mongodb-nodejs-part5.md);
