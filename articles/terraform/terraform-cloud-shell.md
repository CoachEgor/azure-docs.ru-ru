---
title: Использование Terraform с Azure Cloud Shell
description: Используйте Terraform с Azure Cloud Shell, чтобы упростить аутентификацию и конфигурацию шаблона.
services: terraform
ms.service: azure
keywords: terraform, devops, масштабируемый набор, виртуальная машина, сеть, хранилище, модули
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 10/19/2017
ms.openlocfilehash: ab2fd0c7fa546201d6eb19f727053a9ac54fa854
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "66169925"
---
# <a name="terraform-cloud-shell-development"></a>Разработка Cloud Shell Terraform 

Модуль Terraform прекрасно работает из командной строки Bash терминала macOS или Bash на компьютерах под управлением Windows или Linux. Выполнение конфигураций Terraform в Bash [Azure Cloud Shell](/azure/cloud-shell/overview) имеет ряд уникальных преимуществ, ускоряющих цикл разработки.

В этой статье с основными понятиями рассматриваются возможности Cloud Shell, которые помогают писать сценарии Terraform, развертываемые в Azure.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Автоматическая настройка учетных данных

Модуль Terraform установлен и сразу же доступен в Cloud Shell. Сценарии Terraform выполняют аутентификацию в Azure при входе в Cloud Shell для управления инфраструктурой без дополнительной настройки. Автоматическая аутентификация обходит необходимость вручную создавать субъект-службу Active Directory и настраивать переменные поставщика Azure Terraform.


## <a name="using-modules-and-providers"></a>Использование модулей и поставщиков

Модулям Azure Terraform требуются учетные данные для доступа к ресурсам в подписке Azure и внесения изменений в них. При работе в Cloud Shell добавьте следующий код в сценарий, чтобы использовать модули Azure Terraform в Cloud Shell:

```tf
# Configure the Microsoft Azure Provider
provider "azurerm" {
}
```

Cloud Shell передает необходимые значения поставщику `azurerm` посредством переменных среды при использовании любой из команд интерфейса командной строки `terraform`.

## <a name="other-cloud-shell-developer-tools"></a>Другие инструменты разработки Cloud Shell

Файлы и состояния оболочки сохраняются в службе хранилища Azure между сеансами Cloud Shell. Используйте [Обозреватель службы хранилища Azure](/azure/vs-azure-tools-storage-manage-with-storage-explorer), чтобы скопировать и отправить файлы в Cloud Shell с локального компьютера.

Azure CLI доступен в Cloud Shell и является мощным инструментом для тестирования конфигурации и проверки работы после завершения `terraform apply` или `terraform destroy`.


## <a name="next-steps"></a>Дополнительная информация

[Создание кластера виртуальных машин с помощью Terraform с использованием реестра модулей](terraform-create-vm-cluster-module.md)
[Создание кластера виртуальной машины с Terraform с помощью настраиваемого HCL](terraform-create-vm-cluster-with-infrastructure.md)
