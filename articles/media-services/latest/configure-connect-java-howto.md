---
title: Подключение к API служб мультимедиа Azure v3 — Java
description: В этой статье описывается подключение к API-интерфейсу служб мультимедиа Azure v3 с помощью Java.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2019
ms.custom: devx-track-java
ms.author: juliako
ms.openlocfilehash: 098e1db7470124dc7c15b3ee65d6ab9cb3fadabd
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/28/2020
ms.locfileid: "87319836"
---
# <a name="connect-to-media-services-v3-api---java"></a>Подключение к API служб мультимедиа v3 — Java

В этой статье показано, как подключиться к пакету SDK для Java служб мультимедиа Azure v3 с помощью метода входа субъекта-службы.

В этой статье Visual Studio Code используется для разработки примера приложения.

## <a name="prerequisites"></a>Предварительные требования

- Следуйте инструкциям по [написанию Java с Visual Studio Code](https://code.visualstudio.com/docs/java/java-tutorial) для установки:

   - JDK
   - Apache Maven
   - Пакет расширений Java
- Убедитесь, что заданы `JAVA_HOME` `PATH` переменные среды и.
- [Создание учетной записи Служб мультимедиа](./create-account-howto.md). Обязательно запомните имя группы ресурсов и имя учетной записи служб мультимедиа.
- Выполните действия, описанные в разделе [API Access](./access-api-howto.md) . Запишите идентификатор подписки, идентификатор приложения (идентификатор клиента), ключ проверки подлинности (секрет) и идентификатор клиента, которые понадобятся вам на более позднем этапе.

Также ознакомьтесь:

- [Java в Visual Studio Code](https://code.visualstudio.com/docs/languages/java)
- [Управление проектами Java в VS Code](https://code.visualstudio.com/docs/java/java-project)

> [!IMPORTANT]
> Проверьте [соглашения об именовании](media-services-apis-overview.md#naming-conventions).

## <a name="create-a-maven-project"></a>Создание проекта Maven

Откройте программу командной строки и `cd` в каталог, в котором нужно создать проект.
    
```
mvn archetype:generate -DgroupId=com.azure.ams -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

При выполнении команды `pom.xml` `App.java` создаются файлы, и другие. 

## <a name="add-dependencies"></a>Добавление зависимостей

1. В Visual Studio Code откройте папку, в которой находится проект
1. Найдите и откройте`pom.xml`
1. Добавление необходимых зависимостей

    ```xml
   <dependency>
     <groupId>com.microsoft.azure.mediaservices.v2018_07_01</groupId>
     <artifactId>azure-mgmt-media</artifactId>
     <version>1.0.0-beta-3</version>
   </dependency>
   <dependency>
     <groupId>com.microsoft.rest</groupId>
     <artifactId>client-runtime</artifactId>
     <version>1.6.6</version>
   </dependency>
   <dependency>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>azure-client-authentication</artifactId>
     <version>1.6.6</version>
   </dependency>
    ```

## <a name="connect-to-the-java-client"></a>Подключение к клиенту Java

1. Откройте `App.java` файл в разделе `src\main\java\com\azure\ams` и убедитесь, что пакет включен в начало:

    ```java
    package com.azure.ams;
    ```
1. Под оператором statement добавьте следующие операторы import:
   
   ```java
   import com.microsoft.azure.AzureEnvironment;
   import com.microsoft.azure.credentials.ApplicationTokenCredentials;
   import com.microsoft.azure.management.mediaservices.v2018_07_01.implementation.MediaManager;
   import com.microsoft.rest.LogLevel;
   ```
1. Чтобы создать Active Directory учетные данные, необходимые для выполнения запросов, добавьте следующий код в метод Main класса App и задайте значения, полученные из [API доступа](./access-api-howto.md):
   
   ```java
   final String clientId = "00000000-0000-0000-0000-000000000000";
   final String tenantId = "00000000-0000-0000-0000-000000000000";
   final String clientSecret = "00000000-0000-0000-0000-000000000000";
   final String subscriptionId = "00000000-0000-0000-0000-000000000000";

   try {
      ApplicationTokenCredentials credentials = new ApplicationTokenCredentials(clientId, tenantId, clientSecret, AzureEnvironment.AZURE);
      credentials.withDefaultSubscriptionId(subscriptionId);

      MediaManager manager = MediaManager
              .configure()
              .withLogLevel(LogLevel.BODY_AND_HEADERS)
              .authenticate(credentials, credentials.defaultSubscriptionId());
      System.out.println("signed in");
   }
   catch (Exception e) {
      System.out.println("Exception encountered.");
      System.out.println(e.toString());
   }
   ```
1. Запустите приложение.

## <a name="see-also"></a>См. также статью

- [Основные понятия служб мультимедиа Azure](concepts-overview.md)
- [пакет SDK для Java](https://aka.ms/ams-v3-java-sdk)
- [Справочник по Java](https://aka.ms/ams-v3-java-ref)
- [com. Microsoft. Azure. mediaservices. v2018_07_01: Azure-руководства-Media](https://search.maven.org/artifact/com.microsoft.azure.mediaservices.v2018_07_01/azure-mgmt-media/1.0.0-beta/jar)

## <a name="next-steps"></a>Дальнейшие действия

Теперь можно включать `import com.microsoft.azure.management.mediaservices.v2018_07_01.*;` и приступить к управлению сущностями.

Дополнительные примеры кода см. в репозитории [примеров пакета SDK для Java](/samples/azure-samples/media-services-v3-java/azure-media-services-v3-samples-using-java/) .
