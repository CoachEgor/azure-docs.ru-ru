---
title: Ошибки с поиском ресурсов Azure | Документы Microsoft
description: Описываются способы устранения ошибок с поиском ресурсов.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: troubleshooting
ms.date: 06/06/2018
ms.author: tomfitz
ms.openlocfilehash: 9c999a70ffdbed0c954cfc960b5febdaff06e4ae
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206187"
---
# <a name="resolve-not-found-errors-for-azure-resources"></a>Устранение ошибок с поиском ресурсов Azure

В этой статье описываются ошибки, которые могут возникнуть, когда во время развертывания не удается найти ресурс.

## <a name="symptom"></a>Симптом

Если шаблон содержит имя ресурса, которое не удается разрешить, появится сообщение об ошибке, аналогичное приведенному ниже.

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

При попытке использовать функции [reference](resource-group-template-functions-resource.md#reference) или [listKeys](resource-group-template-functions-resource.md#listkeys) с ресурсом, который не удается устранить, появится следующая ошибка.

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

## <a name="cause"></a>Причина:

Resource Manager нужно получить свойства ресурса, но он не может определить ресурс в вашей подписке.

## <a name="solution-1---set-dependencies"></a>Решение 1 — задание зависимостей

При попытке развернуть отсутствующий ресурс в шаблоне проверьте, нужно ли добавить зависимость. Resource Manager оптимизирует развертывание, создавая ресурсы параллельно, когда это возможно. Если один ресурс необходимо развернуть после другого, требуется использовать в шаблоне элемент **dependsOn**. Например, при развертывании веб-приложения вам нужно создать план службы приложений. Если вы не указали, что веб-приложение зависит от плана службы приложений, Resource Manager создаст оба ресурса одновременно. При попытке указать свойство веб-приложения появится сообщение о том, что невозможно найти ресурс плана службы приложений, так как он еще не существует. Чтобы предотвратить эту ошибку, в веб-приложении следует настроить зависимость.

```json
{
  "apiVersion": "2015-08-01",
  "type": "Microsoft.Web/sites",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

Но следует избегать задания ненужных зависимостей. Ненужные зависимости могут замедлить развертывание, мешая параллельному развертыванию независимых между собой ресурсов. Кроме того, возможно образование циклических зависимостей, которые блокируют развертывание. Функция [reference](resource-group-template-functions-resource.md#reference) и функции [list*](resource-group-template-functions-resource.md#list) создают неявную зависимость от ссылочного ресурса, когда этот ресурс развертывается в том же шаблоне и ссылается на его имя (а не на идентификатор ресурса). Таким образом можно использовать больше зависимостей, чем задано в свойстве **dependsOn**. Функция [resourceId](resource-group-template-functions-resource.md#resourceid) не создает неявную зависимость и не проверяет, существует ли ресурс. Функция [reference](resource-group-template-functions-resource.md#reference) и функции [list*](resource-group-template-functions-resource.md#list) не создают неявную зависимость, когда ресурс ссылается на свой идентификатор ресурса. Чтобы создать неявную зависимость, передайте имя ресурса, развернутого в том же шаблоне.

При возникновении проблем с зависимостями необходимо узнать, в каком порядке развертываются ресурсы. Вот как можно просмотреть порядок операций развертывания.

1. Выберите журнал развертывания для группы ресурсов.

   ![Выбор журнала развертывания](./media/resource-manager-not-found-errors/select-deployment.png)

2. Выберите развертывание в журнале, затем выберите **События**.

   ![Выбор событий развертывания](./media/resource-manager-not-found-errors/select-deployment-events.png)

3. Изучите последовательность событий для каждого ресурса. Обратите внимание на состояние каждой операции. Например, на следующем рисунке показаны три учетные записи хранения, которые были развернуты параллельно. Обратите внимание, что эти три учетные записи хранения были запущены одновременно.

   ![Параллельное развертывание](./media/resource-manager-not-found-errors/deployment-events-parallel.png)

   На следующем рисунке показаны три учетные записи хранения, которые не были развернуты параллельно. Вторая учетная запись хранения зависит от первой учетной записи хранения, а третья учетная запись хранения — от второй. Первая учетная запись хранения должна быть запущена, принята и завершена, прежде чем будет запущена следующая.

   ![Последовательное развертывание](./media/resource-manager-not-found-errors/deployment-events-sequence.png)

## <a name="solution-2---get-resource-from-different-resource-group"></a>Решение 2 — получение ресурса из другой группы ресурсов

Если ресурс существует в группе ресурсов, отличной от той, где выполняется развертывание, используйте [функцию resourceId](resource-group-template-functions-resource.md#resourceid), чтобы получить полное имя ресурса.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

## <a name="solution-3---check-reference-function"></a>Решение 3 — проверка функции reference

Найдите выражение с функцией [reference](resource-group-template-functions-resource.md#reference). Заданные значения различаются в зависимости от того, входит ли ресурс в тот же шаблон, группу ресурсов и подписку. Убедитесь, что вы предоставляете значения обязательного параметра для вашего сценария. Если ресурс находится в другой группе ресурсов, укажите идентификатор ресурса. Например, для создания ссылки на учетную запись хранения в другой группе ресурсов, используйте следующую команду:

```json
"[reference(resourceId('exampleResourceGroup', 'Microsoft.Storage/storageAccounts', 'myStorage'), '2017-06-01')]"
```