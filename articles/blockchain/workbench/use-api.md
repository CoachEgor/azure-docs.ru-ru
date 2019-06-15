---
title: Использование REST API Azure Blockchain Workbench
description: Сценарии использования API REST Azure Blockchain Workbench
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 04/15/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 7fa72ad62d7d11c795422a203d13a4dd45484c19
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60896054"
---
# <a name="using-the-azure-blockchain-workbench-rest-api"></a>Использование REST API Azure Blockchain Workbench

REST API Azure Blockchain Workbench предоставляет разработчикам и информационным работникам способ создания тесной интеграции для приложений на базе блокчейна. В этом документе рассматриваются несколько основных методов REST API Workbench. Например, предположим, что разработчику необходимо создать пользовательский клиент на базе блокчейна. Этот клиент на базе блокчейна разрешает зарегистрированным пользователям просматривать назначенные блокчейн-приложения и взаимодействовать с ними. Клиент позволяет пользователям просматривать экземпляры контрактов и выполнять действия со смарт-контрактами. Клиент использует REST API Workbench в контексте зарегистрированного пользователя для выполнения таких действий:

* Список приложений
* Получение списка рабочих процессов для приложения
* Вывести список экземпляров смарт-контракта для рабочего процесса.
* Вывести список доступных действий для контракта.
* Выполнение действия для контракта

Примеры блокчейн-приложений, используемых в этих сценариях, можно [скачать с портала GitHub](https://github.com/Azure-Samples/blockchain).

## <a name="list-applications"></a>Список приложений

После входа в клиент на базе блокчейна пользователь сначала должен извлечь все приложения Blockchain Workbench. В этом сценарии пользователь имеет доступ к двум приложениям:

1. [Asset Transfer.](https://github.com/Azure-Samples/blockchain/blob/master/blockchain-workbench/application-and-smart-contract-samples/asset-transfer/readme.md)
2. [Refrigerated Transportation.](https://github.com/Azure-Samples/blockchain/blob/master/blockchain-workbench/application-and-smart-contract-samples/refrigerated-transportation/readme.md)

Выполните вызов [GET в API приложений](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/applicationsget):

``` http
GET /api/v1/applications
Authorization : Bearer {access token}
```

Ответ содержит список всех приложений на базе блокчейна, к которым пользователь имеет доступ в Blockchain Workbench. Администраторы Blockchain Workbench получают все блокчейн-приложения. Остальные администраторы (не Workbench) получают все блокчейны, для которых у них есть по крайней мере одна роль связанного приложения или экземпляра смарт-контракта.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/applications?skip=2",
    "applications": [
        {
            "id": 1,
            "name": "AssetTransfer",
            "description": "Allows transfer of assets between a buyer and a seller, with appraisal/inspection functionality",
            "displayName": "Asset Transfer",
            "createdByUserId": 1,
            "createdDtTm": "2018-04-28T05:59:14.4733333",
            "enabled": true,
            "applicationRoles": null
        },
        {
            "id": 2,
            "name": "RefrigeratedTransportation",
            "description": "Application to track end-to-end transportation of perishable goods.",
            "displayName": "Refrigerated Transportation",
            "createdByUserId": 7,
            "createdDtTm": "2018-04-28T18:25:38.71",
            "enabled": true,
            "applicationRoles": null
        }
    ]
}
```

## <a name="list-workflows-for-an-application"></a>Получение списка рабочих процессов для приложения

Когда пользователь выбирает применимое приложение на базе блокчейна (такое как **Asset Transfer**), клиент на базе блокчейна извлекает все рабочие процессы конкретного приложения. Затем пользователи могут выбрать применимый рабочий процесс, прежде чем отобразятся все экземпляры смарт-контракта для рабочего процесса. Для каждого приложения на базе блокчейна доступен один или несколько рабочих процессов, и для каждого рабочего процесса могут быть доступны экземпляры смарт-контракта. Для клиентского приложения на базе блокчейна, у которого есть только один рабочий процесс, мы рекомендуем пропустить поток взаимодействия с пользователем, позволяющий пользователям выбрать соответствующий рабочий процесс. В этом случае для **Asset Transfer** доступен только один рабочий процесс, также называемый **Asset Transfer**.

Выполните вызов [GET в API рабочих процессов приложений](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/workflowsget):

``` http
GET /api/v1/applications/{applicationId}/workflows
Authorization: Bearer {access token}
```

Ответ содержит все рабочие процессы указанного приложения на базе блокчейна, к которому пользователь имеет доступ в Blockchain Workbench. Администраторы Blockchain Workbench получают все рабочие процессы блокчейна. Остальные администраторы (не Workbench) получают все рабочие процессы, для которых у них есть по крайней мере одна роль связанного приложения или экземпляра смарт-контракта.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/applications/1/workflows?skip=1",
    "workflows": [
        {
            "id": 1,
            "name": "AssetTransfer",
            "description": "Handles the business logic for the asset transfer scenario",
            "displayName": "Asset Transfer",
            "applicationId": 1,
            "constructorId": 1,
            "startStateId": 1
        }
    ]
}
```

## <a name="list-smart-contract-instances-for-a-workflow"></a>Вывести список экземпляров смарт-контракта для рабочего процесса.

Когда пользователь выбирает соответствующий рабочий процесс (в данном случае **Asset Transfer**), клиент на базе блокчейна извлечет все экземпляры смарт-контракта для указанного рабочего процесса. Эти сведения можно использовать для отображения всех экземпляров смарт-контрактов для рабочего процесса. Кроме того, можно разрешить пользователям изучить любой из отображаемых экземпляров смарт-контрактов. В этом примере пользователь применит один из смарт-контрактов для выполнения действий.

Выполните вызов [GET в API контрактов](/rest/api/azure-blockchain-workbench/contractsv2/contractsget):

``` http
GET api/v1/contracts?workflowId={workflowId}
Authorization: Bearer {access token}
```

Ответ содержит перечень всех экземпляров смарт-контракта указанного рабочего процесса. Администраторы Workbench получают все экземпляры смарт-контрактов. Остальные администраторы (не Workbench) получают все экземпляры смарт-контрактов, для которых у них есть по крайней мере одна роль связанного приложения или экземпляра смарт-контракта.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/contracts?skip=3&workflowId=1",
    "contracts": [
        {
            "id": 1,
            "provisioningStatus": 2,
            "connectionID": 1,
            "ledgerIdentifier": "0xbcb6127be062acd37818af290c0e43479a153a1c",
            "deployedByUserId": 1,
            "workflowId": 1,
            "contractCodeId": 1,
            "contractProperties": [
                {
                    "workflowPropertyId": 1,
                    "value": "0"
                },
                {
                    "workflowPropertyId": 2,
                    "value": "My first car"
                },
                {
                    "workflowPropertyId": 3,
                    "value": "54321"
                },
                {
                    "workflowPropertyId": 4,
                    "value": "0"
                },
                {
                    "workflowPropertyId": 5,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 6,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 7,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 8,
                    "value": "0xd882530eb3d6395e697508287900c7679dbe02d7"
                }
            ],
            "transactions": [
                {
                    "id": 1,
                    "connectionId": 1,
                    "transactionHash": "0xf3abb829884dc396e03ae9e115a770b230fcf41bb03d39457201449e077080f4",
                    "blockID": 241,
                    "from": "0xd882530eb3d6395e697508287900c7679dbe02d7",
                    "to": null,
                    "value": 0,
                    "isAppBuilderTx": true
                }
            ],
            "contractActions": [
                {
                    "id": 1,
                    "userId": 1,
                    "provisioningStatus": 2,
                    "timestamp": "2018-04-29T23:41:14.9333333",
                    "parameters": [
                        {
                            "name": "Description",
                            "value": "My first car"
                        },
                        {
                            "name": "Price",
                            "value": "54321"
                        }
                    ],
                    "workflowFunctionId": 1,
                    "transactionId": 1,
                    "workflowStateId": 1
                }
            ]
        }
    ]
}
```

## <a name="list-available-actions-for-a-contract"></a>Получение списка доступных действий для контракта

Если пользователь решает изучить контракт, клиент на базе блокчейна может показать доступные пользователю действия, учитывая состояние контракта. В этом примере пользователь просматривает все доступные действия для созданного смарт-контракта.

* Измените: Позволяет пользователю изменить описание и цену актива.
* Завершить: Пользователь может завершить контракт данных ресурса.

Выполните вызов [GET в API действия контракта](/rest/api/azure-blockchain-workbench/contractsv2/contractactionget):

``` http
GET /api/v1/contracts/{contractId}/actions
Authorization: Bearer {access token}
```

Ответ содержит список всех действий, к которым пользователь может применить текущее состояние указанного экземпляра смарт-контракта. Пользователи получают все необходимые действия, если они имеют роль связанного приложения или экземпляра смарт-контракта для текущего состояния указанного экземпляра смарт-контракта.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/contracts/1/actions?skip=2",
    "workflowFunctions": [
        {
            "id": 2,
            "name": "Modify",
            "description": "Modify the description/price attributes of this asset transfer instance",
            "displayName": "Modify",
            "parameters": [
                {
                    "id": 1,
                    "name": "description",
                    "description": "The new description of the asset",
                    "displayName": "Description",
                    "type": {
                        "id": 2,
                        "name": "string",
                        "elementType": null,
                        "elementTypeId": 0
                    }
                },
                {
                    "id": 2,
                    "name": "price",
                    "description": "The new price of the asset",
                    "displayName": "Price",
                    "type": {
                        "id": 3,
                        "name": "money",
                        "elementType": null,
                        "elementTypeId": 0
                    }
                }
            ],
            "workflowId": 1
        },
        {
            "id": 3,
            "name": "Terminate",
            "description": "Used to cancel this particular instance of asset transfer",
            "displayName": "Terminate",
            "parameters": [],
            "workflowId": 1
        }
    ]
}
```

## <a name="execute-an-action-for-a-contract"></a>Выполнение действия для контракта

Затем пользователь может предпринять действия для указанного экземпляра смарт-контракта. В этом случае рассмотрим сценарий, в котором пользователь хочет изменить описание и цену ресурса на следующие значения:

* Описание: «Мой автомобиль обновленные»
* Цена: 54321

Выполните вызов [POST в API действия контракта](/rest/api/azure-blockchain-workbench/contractsv2/contractactionpost):

``` http
POST /api/v1/contracts/{contractId}/actions
Authorization: Bearer {access token}
actionInformation: {
    "workflowFunctionId": 2,
    "workflowActionParameters": [
        {
            "name": "description",
            "value": "My updated car"
        },
        {
            "name": "price",
            "value": "54321"
        }
    ]
}
```

Пользователи могут выполнить действие только с учетом текущего состояния указанного экземпляра смарт-контракта и роли связанного приложения или экземпляра смарт-контракта пользователя. При успешном выполнении вызова POST возвращается ответ HTTP 200 OK без текста ответа.

``` http
HTTP/1.1 200 OK
Content-type: application/json
```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench REST API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench) (REST API Azure Blockchain Workbench)