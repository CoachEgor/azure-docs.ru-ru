---
title: Создание технических активов приложений Azure Лазурный рынок
description: Создание технических ресурсов для предложения приложения Azure.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: dsindona
ms.openlocfilehash: 59f9fecb3d949d9cdf48719e6329b066d9eb3fc5
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393535"
---
# <a name="prepare-your-azure-application-technical-assets"></a>Подготовка технических ресурсов для приложения Azure

В этой статье описываются средства подготовки технических ресурсов для предложения приложения Azure.

## <a name="before-you-begin"></a>Перед началом

Просмотрите видео ниже ([Building Solution Templates, and Managed Applications for the Azure Marketplace](https://channel9.msdn.com/Events/Build/2018/BRK3603) (Создание шаблонов решений и управление приложениями в Azure Marketplace)), в котором рассказывается, как создать шаблон Azure Resource Manager для определения решения Azure и как после этого выполнить публикацию предложения приложения в Azure Marketplace.

>[!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3603/player]


Ознакомьтесь с документацией по приложениям Azure, включающей руководства, примеры и другие материалы.

- [Описание структуры и синтаксиса шаблонов Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)
- Краткие руководства:

  - [Шаблоны Azure кикстарт](https://azure.microsoft.com/documentation/templates/)
  - [Шаблоны быстрого запуска Azure в GitHub](https://github.com/azure/azure-quickstart-templates)
  - [Публикация определения приложения](https://docs.microsoft.com/azure/managed-applications/publish-service-catalog-app)
  - [Развертывание приложения из каталога служб](https://docs.microsoft.com/azure/managed-applications/deploy-service-catalog-quickstart)

  
- Руководства.

  - [Создание файлов определений](https://docs.microsoft.com/azure/managed-applications/publish-service-catalog-app)
  - [Публикация приложения из marketplace](https://docs.microsoft.com/azure/managed-applications/publish-marketplace-app)

  - Примеры:

    - [Azure CLI](https://docs.microsoft.com/azure/managed-applications/cli-samples)
    - [Azure PowerShell](https://docs.microsoft.com/azure/managed-applications/powershell-samples)
    - [Решения для управляемых приложений](https://docs.microsoft.com/azure/managed-applications/sample-projects)

## <a name="fundamental-technical-knowledge"></a>Основные технические знания

Проектирование, сборка и тестирование этих ресурсов занимают время и требуют технических знаний и платформы Azure, и технологий, используемых для создания предложения.

Ваша команда разработчиков должна иметь опыт работы с технологиями Майкрософт, в частности:

- базовое представление о [службах Azure](https://azure.microsoft.com/services/);
- умение [разработать приложения Azure](https://azure.microsoft.com/solutions/architecture/);
- Рабочие знания [виртуальных машин Azure,](https://azure.microsoft.com/services/virtual-machines/) [хранения azure](https://azure.microsoft.com/services/?filter=storage)и [сети Azure](https://azure.microsoft.com/services/?filter=networking)
- опыт работы с [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/);
- Рабочие знания [JSON](https://www.json.org/)

## <a name="suggested-tools"></a>Рекомендуемые инструменты

Выберите одну или обе следующие среды создания скриптов, которые понадобятся вам при управлении приложением Azure:

- [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
- [Azure CLI](https://docs.microsoft.com/cli/azure)

Мы рекомендуем добавить следующие инструменты в среду разработки:

- [Обозреватель службы хранилища Azure](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
- [Visual Studio Code](https://code.visualstudio.com/) со следующими расширениями:

  - Расширение: [Инструменты Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools).
  - Расширение: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify).
  - Расширение: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json).

Мы также предлагаем проверить доступные средства на странице [Средства для разработчиков Azure](https://azure.microsoft.com/tools/) и, если вы используете Visual Studio, на странице [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

## <a name="next-steps"></a>Дальнейшие действия

[Создание предложения приложения Azure](./cpp-create-offer.md)

