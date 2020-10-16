---
title: Контейнеризация существующего приложения .NET для сетки Service Fabric
description: Добавление Service Fabric поддержки оркестрации контейнера сети в проекты ASP.NET и консоли, использующие полную версию .NET Framework.
author: georgewallace
ms.author: gwallace
ms.date: 11/08/2018
ms.topic: conceptual
ms.openlocfilehash: a995f30872216a8b704d3d1714bbece4bb8271f8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91840070"
---
# <a name="containerize-an-existing-net-app-for-service-fabric-mesh"></a>Контейнеризация существующего приложения .NET для сетки Service Fabric

В этой статье показано, как добавить поддержку оркестрации контейнеров сетки Service Fabric в существующее приложение .NET.

В Visual Studio 2017 можно добавить поддержку контейнеризации для ASP.NET и консольных проектов, использующих полную версию платформы .NET.

> [!NOTE]
> Проекты .NET **Core** сейчас не поддерживаются.

## <a name="prerequisites"></a>Предварительные требования

* Если у вас еще нет Azure подписки до начала работы, можно [создать бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* Убедитесь в том, что вы [настроили среду разработки](service-fabric-mesh-howto-setup-developer-environment-sdk.md). Это подразумевает также установку среды выполнения Service Fabric, пакета SDK, Docker, Visual Studio 2017 и создание локального кластера.

## <a name="open-an-existing-net-app"></a>Открытие существующего приложения .NET

Откройте приложение, к которому нужно добавить поддержку оркестрации контейнеров.

Если вы хотите попробовать, можно использовать пример кода [eShop](https://github.com/MikkelHegn/ContainersSFLab). Дальше в этой статье предполагается, что мы используем этот проект, но вы можете применить эти действия к своему собственному проекту.

Получите копию проекта **eShop**.

```git
git clone https://github.com/MikkelHegn/ContainersSFLab.git
```

После загрузки в Visual Studio 2017 откройте **ContainersSFLab\eShopLegacyWebFormsSolution\eShopLegacyWebForms.sln**.

## <a name="add-container-support"></a>Добавление поддержки контейнеров
 
Добавьте поддержку оркестрации контейнеров в существующий консольный или ASP.NET проект с помощью средств сетки Service Fabric, как описано ниже.

В обозревателе решений Visual Studio щелкните правой кнопкой мыши имя проекта (в примере это **eShopLegacyWebForms**), а затем выберите **Добавить** > **Container Orchestrator Support** (Поддержка оркестратора контейнеров).
Откроется диалоговое окно **Add Container Orchestrator Support** (Добавление поддержки оркестратора контейнеров).

![Диалоговое окно добавления оркестратора контейнеров Visual Studio](./media/service-fabric-mesh-howto-containerize-vs/add-container-orchestration-support.png)

Из раскрывающегося списка выберите **Сетка Service Fabric**, а затем щелкните **ОК**.

Затем средство проверяет, установлен ли Docker, добавляет в проект файл Dockerfile и извлекает образ Docker для проекта.  
В решение добавляется проект приложения сетки Service Fabric. Он содержит профили публикации сетки и файлы конфигурации. Имя этого проекта совпадает с именем вашего проекта, но в конце добавляется Application, например, **eShopLegacyWebFormsApplication**. 

В новом проекте сетки появятся две папки, на которые следует обратить внимание.
- **Ресурсы приложений** содержит файлы YAML, которые описывают дополнительные ресурсы сетки, например, сеть.
- **Ресурсы службы** содержит файл service.yaml, который описывает, каким образом приложение должно выполняться при развертывании.

После добавления поддержки оркестрации контейнеров в приложение можно нажать клавишу **F5** для отладки приложения .NET в локальном кластере сетки Service Fabric. Ниже показан пример приложения eShop ASP.NET, выполняющегося в кластере сетки Service Fabric. 

![приложение eShop](./media/service-fabric-mesh-howto-containerize-vs/eshop-running.png)

Теперь можно опубликовать приложение в сетку Azure Service Fabric.

## <a name="next-steps"></a>Дальнейшие шаги

Узнайте, как опубликовать приложение в сетку Service Fabric: [Tutorial — Deploy a Service Fabric Mesh application](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md) (Руководство по развертыванию приложения сетки Service Fabric)