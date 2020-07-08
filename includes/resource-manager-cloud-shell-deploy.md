---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: aac2f3ea2b52ac0319f96279deed13c1145749bd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "74451494"
---
## <a name="deploy-template-from-cloud-shell"></a>Развертывание шаблона из Cloud Shell

Для развертывания шаблона можно использовать [Cloud Shell](../articles/cloud-shell/overview.md). Чтобы развернуть внешний шаблон, укажите URI шаблона точно так же, как и для любого внешнего развертывания. Чтобы развернуть локальный шаблон, сначала необходимо загрузить шаблон для Cloud Shell в учетную запись хранения. В этом разделе описывается, как загрузить шаблон в свою учетную запись облачной оболочки и развернуть его как локальный файл. Если вы еще не использовали Cloud Shell, ознакомьтесь со статьей [Обзор Azure Cloud Shell](../articles/cloud-shell/overview.md), чтобы узнать о настройке службы.

1. Войдите на [портал Azure](https://portal.azure.com).

1. Выберите свою группу ресурсов Cloud Shell. Шаблон имени — `cloud-shell-storage-<region>`.

   ![Выбор группы ресурсов](./media/resource-manager-cloud-shell-deploy/select-cloud-shell-resource-group.png)

1. Выберите учетную запись хранения для Cloud Shell.

   ![Выбор учетной записи хранения](./media/resource-manager-cloud-shell-deploy/select-storage.png)

1. Выберите **Большие двоичные объекты**.

   ![Выберите "Большие двоичные объекты"](./media/resource-manager-cloud-shell-deploy/select-blobs.png)

1. Выберите **+ Container** (+ Контейнер).

   ![Добавление контейнера](./media/resource-manager-cloud-shell-deploy/add-container.png)

1. Задайте контейнеру название и уровень доступа. Образец шаблона в данной статье не содержит конфиденциальных сведений и поэтому разрешает анонимный доступ для чтения. Нажмите кнопку **ОК**.

   ![Указание значений для контейнера](./media/resource-manager-cloud-shell-deploy/provide-container-values.png)

1. Выберите созданный контейнер.

   ![Выберите новый контейнер](./media/resource-manager-cloud-shell-deploy/select-container.png)

1. Выберите команду **Отправить**.

   ![Отправка большого двоичного объекта](./media/resource-manager-cloud-shell-deploy/upload-blob.png)

1. Найдите и отправьте свой шаблон.

   ![Отправка файла](./media/resource-manager-cloud-shell-deploy/find-and-upload-template.png)

1. После отправки выберите шаблон.

   ![Выберите новый шаблон](./media/resource-manager-cloud-shell-deploy/select-new-template.png)

1. Скопируйте URL-адрес.

   ![Копирование URL-адреса](./media/resource-manager-cloud-shell-deploy/copy-url.png)

1. Откройте командную строку.

   ![Открытие Cloud Shell](./media/resource-manager-cloud-shell-deploy/start-cloud-shell.png)
