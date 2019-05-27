---
title: Управление секретами при работе со службой Azure Dev Space
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 05/11/2018
ms.topic: conceptual
description: Быстрая разработка в Kubernetes с использованием контейнеров и микрослужб в Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, containers
ms.openlocfilehash: 8ee50289083b12b7b2abd3b9ece2c8de345df9fe
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/17/2019
ms.locfileid: "65851433"
---
# <a name="how-to-manage-secrets-when-working-with-an-azure-dev-space"></a>Управление секретами при работе со службой Azure Dev Space

Вашим службам могут потребоваться определенные пароли, строки подключения и другие секреты для баз данных или других защищенных служб Azure. Задавая значения этих секретов в файле конфигурации, их можно сделать доступными в коде в качестве переменных среды.  С ними следует обращаться с осторожностью, чтобы не нарушать безопасность секретов.

Служба Azure Dev Spaces предоставляет два рекомендуемых варианта хранения секретов: в файле values.dev.yaml и встроенными непосредственно в azds.yaml. Секреты не рекомендуется хранить в файле values.yaml.
 
## <a name="method-1-valuesdevyaml"></a>Метод 1. Файл values.dev.yaml
1. Откройте VS Code с вашим проектом, который настроен для Azure Dev Spaces.
2. Добавьте в файл с именем _values.dev.yaml_ в той же папке, где существующих _azds.yaml_ и определите секретного ключа и значения, как показано в следующем примере:

    ```yaml
    secrets:
      redis:
        port: "6380"
        host: "contosodevredis.redis.cache.windows.net"
        key: "secretkeyhere"
    ```
     
3. _azds.yaml_ уже ссылается на _values.dev.yaml_ файла, если он существует. Если вы предпочитаете другое имя файла, обновите раздел «install.values»:

    ```yaml
    install:
      values:
      - values.dev.yaml?
      - secrets.dev.yaml?
    ```
 
4. Измените код службы, чтобы ссылаться на эти секреты в качестве переменных среды, как показано в следующем примере:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
    
5. Обновите службы, выполняемые в кластере, чтобы активировать эти изменения. В командной строке запустите команду:

    ```
    azds up
    ```
 
6. В командной строке проверьте, что эти секреты были созданы (необязательно):

      ```
      kubectl get secret --namespace default -o yaml 
      ```

7. Убедитесь, что _values.dev.yaml_ добавлен в файл _GITIGNORE_, чтобы избежать фиксации секретов в системе управления версиями.
 
 
## <a name="method-2-inline-directly-in-azdsyaml"></a>Метод 2. Встраивание непосредственно в azds.yaml
1.  В файле _azds.yaml_ задайте секреты в разделе YAML configurations/develop/install. Хотя вы можете вводить секретные значения прямо там, это не рекомендуется, потому что _azds.yaml_ фиксируется в системе управления версиями. Вместо этого добавьте заполнители, используя синтаксис "$PLACEHOLDER".

    ```yaml
    configurations:
      develop:
        ...
        install:
          set:
            secrets:
              redis:
                port: "$REDIS_PORT"
                host: "$REDIS_HOST"
                key: "$REDIS_KEY"
    ```
     
2.  Создайте _ENV_-файл в той же папке, что и _azds.yaml_. Введите секреты с помощью стандартной нотации пары "ключ=значение". Не фиксируйте _ENV_-файл в системе управления версиями. (Чтобы исключить из системы управления версиями на основе git, добавьте в файл _GITIGNORE_.) В следующем примере показан _ENV_-файл:

    ```
    REDIS_PORT=3333
    REDIS_HOST=myredishost
    REDIS_KEY=myrediskey
    ```
2.  Измените исходный код службы, чтобы ссылаться на эти секреты в коде, как показано в следующем примере:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
 
3.  Обновите службы, выполняемые в кластере, чтобы активировать эти изменения. В командной строке запустите команду:

    ```
    azds up
    ```

4.  Просмотрите секреты из kubectl (необязательно):

    ```
    kubectl get secret --namespace default -o yaml
    ```

## <a name="next-steps"></a>Дальнейшие действия

С помощью этих методов можно безопасно подключиться к базе данных, кэшу Azure для Redis или получить защищенный доступ к службам Azure.
 
