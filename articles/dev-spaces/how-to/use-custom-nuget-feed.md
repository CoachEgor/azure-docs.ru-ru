---
title: Как использовать пользовательские NuGet веб-канала в пробелах разработки Azure
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: johnsta
ms.author: johnsta
ms.date: 05/11/2018
ms.topic: conceptual
description: Использование настраиваемого веб-канала NuGet для предоставления доступа и использования пакетов NuGet в Azure Dev Spaces.
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, containers
manager: ghogen
ms.openlocfilehash: 1a000e378a9b8ecfb09d778fd6444e3f24b3df7b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60686463"
---
#  <a name="use-a-custom-nuget-feed-in-an-azure-dev-space"></a>Использование настраиваемого веб-канала NuGet в Azure Dev Spaces

Веб-канал NuGet обеспечивает удобный способ добавления источников пакетов в проект. Служба Azure Dev Spaces должна иметь доступ к этому веб-каналу, чтобы обеспечить правильную установку зависимостей в контейнере Docker.

## <a name="set-up-a-nuget-feed"></a>Настройка веб-канала NuGet

Чтобы настроить веб-канал NuGet, сделайте следующее:
1. Добавьте [ссылку на пакет](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) в файл `*.csproj` в узле `PackageReference`.

   ```xml
   <ItemGroup>
       <!-- ... -->
       <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
       <!-- ... -->
   </ItemGroup>
   ```

2. Создайте файл [NuGet.Config](https://docs.microsoft.com/nuget/reference/nuget-config-file) в папке проекта.
     * В разделе `packageSources` укажите ссылки на расположение веб-канала NuGet. Внимание! Веб-канал NuGet должен быть общедоступным.
     * В разделе `packageSourceCredentials` настройте учетные данные: имя пользователя и пароль. 

   ```xml
   <packageSources>
       <add key="Contoso" value="https://contoso.com/packages/" />
   </packageSources>

   <packageSourceCredentials>
       <Contoso>
           <add key="Username" value="user@contoso.com" />
           <add key="ClearTextPassword" value="33f!!lloppa" />
       </Contoso>
   </packageSourceCredentials>
   ```

3. Если вы используете управление исходным кодом, сделайте следующее:
    - Укажите ссылку на `NuGet.Config` в файле `.gitignore`, чтобы вы случайно не передали учетные данные в исходный репозиторий.
    - Откройте файл `azds.yaml` в своем проекте и найдите раздел `build`, вставьте следующий фрагмент кода, чтобы убедиться, что файл `NuGet.Config` будет синхронизирован с Azure для использования во время процесса сборки образа контейнера. (По умолчанию служба Azure Dev Spaces не синхронизирует файлы, которые соответствуют правилам `.gitignore` и `.dockerignore`.)

        ```yaml
        build:
        useGitIgnore: true
        ignore:
        - “!NuGet.Config”
        ```


## <a name="next-steps"></a>Дальнейшие действия

После того, как вы выполнили вышеуказанные шаги, при следующем запуске `azds up` (или нажатии `F5` в VSCode или Visual Studio) служба Azure Dev Spaces синхронизирует с Azure файл `NuGet.Config`, который затем `dotnet restore` будет использовать для установки зависимостей пакетов в контейнере.

