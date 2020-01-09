---
title: Обновление средств Azure Dev Spaces
services: azure-dev-spaces
ms.date: 07/03/2018
ms.topic: conceptual
description: Узнайте, как обновить средства командной строки Azure Dev Spaces, расширение Visual Studo Code и расширение Visual Studio.
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, containers
ms.openlocfilehash: 07d55689ac94a865527f4b595765d67b28ddb97a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75438419"
---
# <a name="how-to-upgrade-azure-dev-spaces-tools"></a>Обновление средств Azure Dev Spaces

Если вы уже используете Azure Dev Spaces, при выходе нового выпуска вам следует обновить клиентские средства Azure Dev Spaces.

## <a name="update-the-azure-cli"></a>Обновление Azure CLI

При обновлении до последней версии Azure CLI вы автоматически получаете последнюю версию расширения интерфейса командной строки для Azure Dev Spaces.

Предыдущую версию удалять не нужно, просто найдите нужный файл для скачивания на странице [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).


## <a name="update-the-dev-spaces-cli-extension-and-command-line-tools"></a>Обновление расширения CLI для Azure Dev Spaces и средств командной строки

Выполните следующую команду:

```cmd
az aks use-dev-spaces -n <your-aks-cluster> -g <your-aks-cluster-resource-group> --update
```

## <a name="update-the-vs-code-extension"></a>Обновление расширения VS Code

После установки расширение автоматически обновляется. Возможно, потребуется перезагрузить расширение, чтобы использовать новые возможности. В VS Code откройте панель **Расширения**, выберите расширения **Azure Dev Spaces** и щелкните **Перезагрузить**.

## <a name="update-the-visual-studio-extension"></a>Обновление расширения Visual Studio

Как и для других расширений и обновлений, Visual Studio сообщит вам о наличии обновления средств Visual Studio для Kubernetes, в пакет которых входит Azure Dev Spaces. Найдите значок флага в верхней правой части экрана.

Чтобы обновить средства в Visual Studio, выберите пункты меню **Сервис > Расширения и обновления**, а затем слева выберите **Обновления**. Найдите **Средства Visual Studio для Kubernetes** и нажмите кнопку **Обновить**.

## <a name="next-steps"></a>Дальнейшие действия

Протестируйте новые средства, создав новый кластер. Изучите руководства по [Azure Dev Spaces](/azure/dev-spaces).