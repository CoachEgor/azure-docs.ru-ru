---
title: Пример скрипта для Azure CLI. Удаление хранилища конфигураций для приложения в Azure
titleSuffix: Azure App Configuration
description: Пример скрипта Azure CLI. Удаление хранилища конфигураций для приложения Azure
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: azure-app-configuration
ms.date: 02/24/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: 9f18cb79e4c160271fbb3855b1fc6e05589cf854
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2020
ms.locfileid: "76899439"
---
# <a name="delete-an-azure-app-configuration-store"></a>Удаление хранилища конфигураций приложения Azure

Этот пример скрипта удаляет экземпляр конфигурации приложения Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать интерфейс командной строки локально, для работы с этой статьей вам понадобится Azure CLI 2.0 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI](/cli/azure/install-azure-cli).

Сначала необходимо установить расширение CLI для службы "Конфигурация приложений Azure", выполнив следующую команду:

        az extension add -n appconfig

## <a name="sample-script"></a>Пример скрипта

```azurecli-interactive
#/bin/bash

# Delete an App Configuration store named myTestAppConfigStore from the Resource Group myResourceGroup
az appconfig delete --name myTestAppConfigStore --resource-group myResourceGroup
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Описание скрипта

В этом скрипте используются следующие команды для удаления хранилища Конфигурации приложения. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Примечания |
|---|---|
| [az appconfig delete](/cli/azure/ext/appconfig/appconfig#ext-appconfig-az-appconfig-delete) | Удаляет ресурс хранилища Конфигурации приложения. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](/cli/azure).

Дополнительные примеры скриптов Azure CLI для службы "Конфигурация приложений" см. в [этой статье](../cli-samples.md).
