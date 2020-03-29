---
title: включить файл
description: включить файл
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/15/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a7c696870e22e1692ca5ed778e47f8e4cc00615a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "67185188"
---
## <a name="set-up-the-development-environment"></a>Настройка среды разработки

В этом разделе описаны шаги по настройке среды разработки, включая создание приложения ASP.NET MVC, добавление подключения к подключенным службам, добавление контроллера и указание требуемых директив пространства имен.

### <a name="create-an-aspnet-mvc-app-project"></a>Создание проекта приложения ASP.NET MVC

1. Запустите Visual Studio.

1. Из основного меню выберите **Файл** > **Новый** > **проект**.

1. В диалоговом окне **Новый проект** выберите **Веб** > **Веб-приложение ASP.NET (.NET Framework)**. В поле **Имя** введите **StorageAspNet**. Нажмите кнопку **ОК**.

    ![Снимок экрана: диалоговое окно нового проекта](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. В диалоговом окне **Создание веб-приложения ASP.NET** выберите **MVC** и нажмите кнопку **ОК**.

    ![Снимок экрана: диалоговое окно "Создание веб-приложения ASP.NET"](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Подключение к учетной записи хранения Azure с помощью подключенных служб

1. В **обозревателе решений** щелкните проект правой кнопкой мыши.

1. Из контекстного меню выберите **Добавленную** > **службу**Connected Service.

1. В диалоговом окне **Подключенные службы** выберите **Cloud Storage with Azure Storage** (Облачное хранилище в службе хранения Azure).

    ![Снимок экрана: диалоговое окно "Подключенные службы"](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. В диалоговом окне **Служба хранилища Azure** выберите учетную запись хранения Azure, с которой вы будете работать в рамках этого руководства. Чтобы создать учетную запись хранения Azure, выберите **Создать новую учетную запись хранения** и заполните форму. Создав учетную запись или выбрав имеющуюся, нажмите кнопку **Добавить**. После этого Visual Studio установит пакет NuGet для службы хранилища Azure и создаст строку подключения хранилища к **Web.config**.

1. В **Solution Explorer**, правой кнопкой мыши на **зависимости,** выберите Управление **NuGet пакеты**, и добавить пакет NuGet ссылку на последнюю версию Microsoft.Azure.ConfigurationManager.

> [!TIP]
> Сведения о создании учетной записи хранения на [портале Azure](https://portal.azure.com) см. в разделе [Об учетных записях хранения Azure](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account).
>
> Учетную запись хранения можно также создать с помощью [Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md), [Azure CLI](../articles/storage/common/storage-azure-cli.md) или [Azure Cloud Shell](../articles/cloud-shell/overview.md).
