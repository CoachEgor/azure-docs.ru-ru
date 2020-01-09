---
title: Использование секретов в учебных запусках
titleSuffix: Azure Machine Learning
description: Безопасная передача секретов в обучающий запуск с помощью рабочей области Key Vault
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: larryfr
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 11/08/2019
ms.custom: seodec18
ms.openlocfilehash: 722e329b1a2d069b614a301a5eb2f8642b697295
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/28/2019
ms.locfileid: "75538713"
---
# <a name="use-secrets-in-training-runs"></a>Использование секретов в учебных запусках
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Из этой статьи вы узнаете, как безопасно использовать секреты в учебных запусках. Например, чтобы подключиться к внешней базе данных для запроса обучающих данных, необходимо передать имя пользователя и пароль в контекст удаленного запуска. Написание таких значений в сценариях обучения в виде открытого текста не является безопасным, так как оно будет предоставлять секрет. 

Вместо этого рабочая область машинного обучения Azure имеет [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) как связанный ресурс. Этот Key Vault можно использовать для безопасной передачи секретов в удаленное выполнение с помощью набора API-интерфейсов в пакете SDK для Python Машинное обучение Azure.

Основной поток для использования секретов:
 1. Войдите в Azure на локальном компьютере и подключитесь к рабочей области.
 2. На локальном компьютере задайте секрет в рабочей области Key Vault.
 3. Отправка удаленного запуска.
 4. В удаленном запуске получите секрет из значения ключа и используйте его.

## <a name="set-secrets"></a>Задать секреты

В пакете SDK для Машинное обучение Azure Python класс [Keyvault](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py) содержит методы для настройки секретов. В локальном сеансе Python сначала получите ссылку на рабочую область Key Vault, а затем используйте метод [set_secret](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#set-secret-name--value-) , чтобы задать секрет по имени и значению.

```python
from azureml.core import Workspace
import os

ws = Workspace.from_config()
my_secret = os.environ.get("MY_SECRET")
keyvault = ws.get_default_keyvault()
keyvault.set_secret(name="mysecret", value = my_secret)
```

Не следует помещайте значение секрета в код Python, так как оно небезопасно для сохранения в файле в виде открытого текста. Вместо этого получите значение секрета из переменной среды, например, для секрета сборки Azure DevOps или из интерактивных входных данных пользователя.

Имена секретов можно перечислить с помощью метода [list_secrets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#list-secrets--) . Метод __set_secret__ обновляет значение секрета, если такое имя уже существует.

## <a name="get-secrets"></a>получение секретов;

В локальном коде можно использовать метод[Keyvault. get_secret](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#get-secret-name-) для получения значения секрета по имени.

В списке запуски, отправленные с помощью [эксперимента. Submit](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py#submit-config--tags-none----kwargs-), используйте метод [Run. get_secret](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-secret-name-) . Так как отправленный запуск осведомлен о своей рабочей области, этот метод назначит создание экземпляра рабочей области и возвращает значение секрета напрямую.

```python
# Code in submitted run
from azureml.core import Run

run = Run.get_context()
secret_value = run.get_secret(name="mysecret")
```

Будьте внимательны, чтобы не предоставлять секретное значение, написав или выполнив печать.

Методы Set и Get также имеют пакетные версии [set_secrets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#set-secrets-secrets-batch-) и [get_secrets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-secrets-secrets-) для доступа к нескольким секретам одновременно.

## <a name="next-steps"></a>Дальнейшие действия

 * [Просмотреть пример записной книжки](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/manage-azureml-service/authentication-in-azureml/authentication-in-azureml.ipynb)
 * [Сведения о безопасности в корпоративной среде с помощью Машинное обучение Azure](concept-enterprise-security.md)
