---
title: Создание масштабируемого набора виртуальных машин Azure с помощью Terraform
description: Руководство, объясняющее, как с помощью Terraform настроить масштабируемый набор виртуальных машин Azure, дополненных виртуальной сетью и управляемыми присоединенными дисками, а также управлять их версиями
services: terraform
ms.service: azure
keywords: terraform, devops, виртуальная машина, Azure, масштабируемый набор, сеть, хранилище, модули
author: tomarchermsft
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2019
ms.openlocfilehash: a6bc0879d07cadc6c5b0b1a21b11b3075ec69719
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71169876"
---
# <a name="use-terraform-to-create-an-azure-virtual-machine-scale-set"></a>Создание масштабируемого набора виртуальных машин Azure с помощью Terraform

С помощью [масштабируемых наборов виртуальных машин Azure](/azure/virtual-machine-scale-sets) вы можете создать группу идентичных виртуальных машин со сбалансированной нагрузкой, а также управлять ею. В этой группе количество экземпляров виртуальных машин может автоматически увеличиваться или уменьшаться согласно потребностям или предварительно созданному расписанию.

В этом руководстве рассказано, как с помощью [Azure Cloud Shell](/azure/cloud-shell/overview) выполнять следующие задачи:

> [!div class="checklist"]
> * настройка развертывания Terraform;
> * использование переменных и выходных данных для развертывания Terraform;
> * создание и развертывание сетевой инфраструктуры;
> * создание и развертывание масштабируемого набора виртуальной машины и присоединение его к сети;
> * создание и развертывание Jumpbox для подключения к виртуальным машинам по протоколу SSH.

> [!NOTE]
> Последнюю версию файлов конфигурации Terraform, используемых в этой статье, можно скачать из [репозитория Awesome Terraform на GitHub](https://github.com/Azure/awesome-terraform/tree/master/codelab-vmss).

## <a name="prerequisites"></a>Предварительные требования

- **Подписка Azure.** Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) , прежде чем начинать работу.

- **Наличие Terraform.** Следуйте указаниям в статье [Terraform и настройка доступа к Azure](/azure/virtual-machines/linux/terraform-install-configure).

- **Наличие пары ключей SSH.** Если у вас еще нет пары ключей SSH, выполните инструкции в статье [Краткая инструкция: создание и использование пары из открытого и закрытого ключей SSH для виртуальных машин Linux в Azure](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys).

## <a name="create-the-directory-structure"></a>Создание структуры каталога

1. Перейдите на [портал Azure](https://portal.azure.com).

1. Откройте [Azure Cloud Shell](/azure/cloud-shell/overview). Если ранее среда не была выбрана, то в качестве среды необходимо выбрать **Bash**.

    ![Командная строка Cloud Shell](./media/terraform-create-vm-scaleset-network-disks-hcl/azure-portal-cloud-shell-button-min.png)

1. Перейдите в каталог `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Создайте каталог с именем `vmss`.

    ```bash
    mkdir vmss
    ```

1. Перейдите в новый каталог:

    ```bash
    cd vmss
    ```

## <a name="create-the-variables-definitions-file"></a>Создание файла определений переменных
Работая с этим разделом, вы определите переменные, с помощью которых можно настроить ресурсы, созданные Terraform.

В Azure Cloud Shell выполните указанные ниже действия.

1. Создайте файл с именем `variables.tf`.

    ```bash
    vi variables.tf
    ```

1. Нажмите клавишу I, чтобы войти в режим вставки.

1. Скопируйте приведенный ниже код и вставьте его в редактор.

   ```hcl
   variable "location" {
    description = "The location where resources will be created"
   }

   variable "tags" {
    description = "A map of the tags to use for the resources that are deployed"
    type        = "map"

    default = {
      environment = "codelab"
    }
   }

   variable "resource_group_name" {
    description = "The name of the resource group in which the resources will be created"
    default     = "myResourceGroup"
   }
   ```

1. Выйдите из режима вставки, нажав клавишу ESC.

1. Сохраните файл и закройте редактор VI. Для этого введите приведенную ниже команду.

    ```bash
    :wq
    ```

## <a name="create-the-output-definitions-file"></a>Создание выходного файла определений
Работая с этим разделом, вы создадите файл, в котором описаны выходные данные после развертывания.

В Azure Cloud Shell выполните указанные ниже действия.

1. Создайте файл с именем `output.tf`.

    ```bash
    vi output.tf
    ```

1. Нажмите клавишу I, чтобы войти в режим вставки.

1. Вставьте приведенный ниже код в редактор, чтобы предоставить сведения о полном доменном имени (FQDN) виртуальным машинам.
   :

   ```hcl
    output "vmss_public_ip" {
        value = "${azurerm_public_ip.vmss.fqdn}"
    }
   ```

1. Выйдите из режима вставки, нажав клавишу ESC.

1. Сохраните файл и закройте редактор VI. Для этого введите приведенную ниже команду.

    ```bash
    :wq
    ```

## <a name="define-the-network-infrastructure-in-a-template"></a>Определение сетевой инфраструктуры в шаблоне
Работая с этим разделом, вы создадите указанную ниже сетевую инфраструктуру в новой группе ресурсов Azure.

  - Одна виртуальная сеть с диапазоном адресов 10.0.0.0/16.
  - Одна подсеть с пространством адресов 10.0.2.0/24.
  - Два общедоступных IP-адреса. Один используется подсистемой балансировки нагрузки масштабируемого набора виртуальных машин, другой — для подключения к SSH Jumpbox.

В Azure Cloud Shell выполните указанные ниже действия.

1. Создайте файл с именем `vmss.tf` для описания инфраструктуры масштабируемого набора виртуальных машин.

    ```bash
    vi vmss.tf
    ```

1. Нажмите клавишу I, чтобы войти в режим вставки.

1. Вставьте приведенный ниже код в конец файла, чтобы предоставить сведения о полном доменном имени (FQDN) виртуальным машинам.

   ```hcl
   resource "azurerm_resource_group" "vmss" {
    name     = "${var.resource_group_name}"
    location = "${var.location}"
    tags     = "${var.tags}"
   }

   resource "random_string" "fqdn" {
    length  = 6
    special = false
    upper   = false
    number  = false
   }

   resource "azurerm_virtual_network" "vmss" {
    name                = "vmss-vnet"
    address_space       = ["10.0.0.0/16"]
    location            = "${var.location}"
    resource_group_name = "${azurerm_resource_group.vmss.name}"
    tags                = "${var.tags}"
   }

   resource "azurerm_subnet" "vmss" {
    name                 = "vmss-subnet"
    resource_group_name  = "${azurerm_resource_group.vmss.name}"
    virtual_network_name = "${azurerm_virtual_network.vmss.name}"
    address_prefix       = "10.0.2.0/24"
   }

   resource "azurerm_public_ip" "vmss" {
    name                         = "vmss-public-ip"
    location                     = "${var.location}"
    resource_group_name          = "${azurerm_resource_group.vmss.name}"
    allocation_method = "Static"
    domain_name_label            = "${random_string.fqdn.result}"
    tags                         = "${var.tags}"
   }
   ```

1. Выйдите из режима вставки, нажав клавишу ESC.

1. Сохраните файл и закройте редактор VI. Для этого введите приведенную ниже команду.

   ```bash
   :wq
   ```

## <a name="provision-the-network-infrastructure"></a>Подготовка сетевой инфраструктуры
С помощью Azure Cloud Shell в каталоге, в котором вы создали файлы конфигурации (TF-файлы), выполните указанные ниже действия.

1. Инициализируйте Terraform.

   ```bash
   terraform init
   ```

1. Разверните определенную инфраструктуру в Azure, выполнив указанную ниже команду.

   ```bash
   terraform apply
   ```

   Terraform запросит значение расположения, так как переменная **location** определена в файле `variables.tf`, но еще не задана. Вы можете ввести любое допустимое расположение, например West US (Запад США), и нажать клавишу ВВОД. (Значения с пробелами необходимо заключить в круглые скобки.)

1. Terraform печатает выходные данные, как задано в файле `output.tf`. Как показано на снимке экрана ниже, полное доменное имя имеет следующий вид: &lt;id>.&lt;location>.cloudapp.azure.com. Здесь id — это вычисленное значение, а location — значение, которое вы указали при запуске Terraform.

   ![Полное доменное имя масштабируемого набора виртуальных машин для общедоступного IP-адреса](./media/terraform-create-vm-scaleset-network-disks-hcl/fqdn.png)

1. В главном меню на портале Azure выберите **Группы ресурсов**.

1. Чтобы отобразить список ресурсов, созданных Terraform, на вкладке **Группы ресурсов** щелкните **myResourceGroup**.
   ![Сетевые ресурсы масштабируемого набора виртуальных машин](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-resources.png)

## <a name="add-a-virtual-machine-scale-set"></a>Добавление масштабируемого набора виртуальных машин

Работая с этим разделом, вы научитесь добавлять указанные ниже ресурсы в шаблон.

- Подсистема балансировки нагрузки Azure и правила для предоставления приложения и присоединения его к общедоступному IP-адресу, созданному ранее в этой статье
- Внутренний пул адресов Azure и назначение его подсистеме балансировки нагрузки
- Порт проверки работоспособности, используемый приложением и настраиваемый в подсистеме балансировки нагрузки.
- Масштабируемый набор виртуальных машин, защищенный подсистемой балансировки нагрузки, которая работает в виртуальной сети, развертывание которой описано ранее в этой статье
- [Nginx](https://nginx.org/) на узлах масштабируемого набора виртуальных машин, для которого используется [cloud-init](https://cloudinit.readthedocs.io/en/latest/).

В Azure Cloud Shell выполните указанные ниже действия.

1. Откройте файл конфигурации `vmss.tf`.

   ```bash
   vi vmss.tf
   ```

1. Перейдите в конец файла и перейдите в режим добавления, нажав клавишу A.

1. Добавьте следующий код в конец файла:

   ```hcl
   resource "azurerm_lb" "vmss" {
    name                = "vmss-lb"
    location            = "${var.location}"
    resource_group_name = "${azurerm_resource_group.vmss.name}"

    frontend_ip_configuration {
      name                 = "PublicIPAddress"
      public_ip_address_id = "${azurerm_public_ip.vmss.id}"
    }

    tags = "${var.tags}"
   }

   resource "azurerm_lb_backend_address_pool" "bpepool" {
    resource_group_name = "${azurerm_resource_group.vmss.name}"
    loadbalancer_id     = "${azurerm_lb.vmss.id}"
    name                = "BackEndAddressPool"
   }

   resource "azurerm_lb_probe" "vmss" {
    resource_group_name = "${azurerm_resource_group.vmss.name}"
    loadbalancer_id     = "${azurerm_lb.vmss.id}"
    name                = "ssh-running-probe"
    port                = "${var.application_port}"
   }

   resource "azurerm_lb_rule" "lbnatrule" {
      resource_group_name            = "${azurerm_resource_group.vmss.name}"
      loadbalancer_id                = "${azurerm_lb.vmss.id}"
      name                           = "http"
      protocol                       = "Tcp"
      frontend_port                  = "${var.application_port}"
      backend_port                   = "${var.application_port}"
      backend_address_pool_id        = "${azurerm_lb_backend_address_pool.bpepool.id}"
      frontend_ip_configuration_name = "PublicIPAddress"
      probe_id                       = "${azurerm_lb_probe.vmss.id}"
   }

   resource "azurerm_virtual_machine_scale_set" "vmss" {
    name                = "vmscaleset"
    location            = "${var.location}"
    resource_group_name = "${azurerm_resource_group.vmss.name}"
    upgrade_policy_mode = "Manual"

    sku {
      name     = "Standard_DS1_v2"
      tier     = "Standard"
      capacity = 2
    }

    storage_profile_image_reference {
      publisher = "Canonical"
      offer     = "UbuntuServer"
      sku       = "16.04-LTS"
      version   = "latest"
    }

    storage_profile_os_disk {
      name              = ""
      caching           = "ReadWrite"
      create_option     = "FromImage"
      managed_disk_type = "Standard_LRS"
    }

    storage_profile_data_disk {
      lun          = 0
      caching        = "ReadWrite"
      create_option  = "Empty"
      disk_size_gb   = 10
    }

    os_profile {
      computer_name_prefix = "vmlab"
      admin_username       = "${var.admin_user}"
      admin_password       = "${var.admin_password}"
      custom_data          = "${file("web.conf")}"
    }

    os_profile_linux_config {
      disable_password_authentication = false
    }

    network_profile {
      name    = "terraformnetworkprofile"
      primary = true

      ip_configuration {
        name                                   = "IPConfiguration"
        subnet_id                              = "${azurerm_subnet.vmss.id}"
        load_balancer_backend_address_pool_ids = ["${azurerm_lb_backend_address_pool.bpepool.id}"]
        primary = true
      }
    }

    tags = "${var.tags}"
   }
   ```

1. Выйдите из режима вставки, нажав клавишу ESC.

1. Сохраните файл и закройте редактор VI. Для этого введите приведенную ниже команду.

    ```bash
    :wq
    ```

1. Создайте файл `web.conf`, который будет использоваться в качестве конфигурации cloud-init для виртуальных машин, входящих в масштабируемый набор.

    ```bash
    vi web.conf
    ```

1. Нажмите клавишу I, чтобы войти в режим вставки.

1. Скопируйте приведенный ниже код и вставьте его в редактор.

   ```hcl
   #cloud-config
   packages:
    - nginx
   ```

1. Выйдите из режима вставки, нажав клавишу ESC.

1. Сохраните файл и закройте редактор VI. Для этого введите приведенную ниже команду.

     ```bash
     :wq
     ```

1. Откройте файл конфигурации `variables.tf`.

    ```bash
    vi variables.tf
    ```

1. Перейдите в конец файла и перейдите в режим добавления, нажав клавишу A.

1. Настройте развертывание, вставив следующий код в конец файла:

    ```hcl
    variable "application_port" {
       description = "The port that you want to expose to the external load balancer"
       default     = 80
    }

    variable "admin_user" {
       description = "User name to use as the admin account on the VMs that will be part of the VM Scale Set"
       default     = "azureuser"
    }

    variable "admin_password" {
       description = "Default password for admin account"
    }
    ```

1. Выйдите из режима вставки, нажав клавишу ESC.

1. Сохраните файл и закройте редактор VI. Для этого введите приведенную ниже команду.

     ```bash
     :wq
     ```

1. Создайте план Terraform, чтобы визуализировать развертывание масштабируемого набора виртуальных машин. (Вам потребуется указать выбранный вами пароль, а также расположение для ваших ресурсов.)

    ```bash
    terraform plan
    ```

    Выходные данные этой команды должны быть приблизительно такие, как на снимке экрана ниже.

    ![Выходные данные при создании масштабируемого набора виртуальных машин](./media/terraform-create-vm-scaleset-network-disks-hcl/add-mvss-plan.png)

1. Разверните новые ресурсы в Azure.

    ```bash
    terraform apply
    ```

    Выходные данные этой команды должны быть приблизительно такие, как на снимке экрана ниже.

    ![Группа ресурсов масштабируемого набора виртуальных машин Terraform](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-contents.png)

1. Откройте браузер и подключитесь к полному доменному имени, которое было возвращено командой.

    ![Результаты перехода по полному доменному имени](./media/terraform-create-vm-scaleset-network-disks-hcl/browser-fqdn.png)

## <a name="add-an-ssh-jumpbox"></a>Добавление точки перехода SSH
*Точка перехода* SSH — это единый сервер, с помощью которого можно перейти на другие серверы в сети. На этом шаге настраиваются следующие ресурсы:

- Сетевой интерфейс (или точка перехода) подключен к той же подсети, что и масштабируемый набор виртуальных машин.

- Виртуальная машина подключена с помощью данного сетевого интерфейса. jumpbox доступен в удаленном режиме. После подключения можно использовать SSH для подключения к любой из виртуальных машин в масштабируемом наборе.

1. Откройте файл конфигурации `vmss.tf`.

   ```bash
   vi vmss.tf
   ```

1. Перейдите в конец файла и перейдите в режим добавления, нажав клавишу A.

1. Добавьте следующий код в конец файла:

   ```hcl
   resource "azurerm_public_ip" "jumpbox" {
    name                         = "jumpbox-public-ip"
    location                     = "${var.location}"
    resource_group_name          = "${azurerm_resource_group.vmss.name}"
    allocation_method = "Static"
    domain_name_label            = "${random_string.fqdn.result}-ssh"
    tags                         = "${var.tags}"
   }

   resource "azurerm_network_interface" "jumpbox" {
    name                = "jumpbox-nic"
    location            = "${var.location}"
    resource_group_name = "${azurerm_resource_group.vmss.name}"

    ip_configuration {
      name                          = "IPConfiguration"
      subnet_id                     = "${azurerm_subnet.vmss.id}"
      private_ip_address_allocation = "dynamic"
      public_ip_address_id          = "${azurerm_public_ip.jumpbox.id}"
    }

    tags = "${var.tags}"
   }

   resource "azurerm_virtual_machine" "jumpbox" {
    name                  = "jumpbox"
    location              = "${var.location}"
    resource_group_name   = "${azurerm_resource_group.vmss.name}"
    network_interface_ids = ["${azurerm_network_interface.jumpbox.id}"]
    vm_size               = "Standard_DS1_v2"

    storage_image_reference {
      publisher = "Canonical"
      offer     = "UbuntuServer"
      sku       = "16.04-LTS"
      version   = "latest"
    }

    storage_os_disk {
      name              = "jumpbox-osdisk"
      caching           = "ReadWrite"
      create_option     = "FromImage"
      managed_disk_type = "Standard_LRS"
    }

    os_profile {
      computer_name  = "jumpbox"
      admin_username = "${var.admin_user}"
      admin_password = "${var.admin_password}"
    }

    os_profile_linux_config {
      disable_password_authentication = false
    }

    tags = "${var.tags}"
   }
   ```

1. Откройте файл конфигурации `output.tf`.

   ```bash
   vi output.tf
   ```

1. Перейдите в конец файла и перейдите в режим добавления, нажав клавишу A.

1. Вставьте указанный ниже код в конец файла, чтобы отобразить имя узла точки перехода по завершении развертывания.

   ```hcl
   output "jumpbox_public_ip" {
      value = "${azurerm_public_ip.jumpbox.fqdn}"
   }
   ```

1. Выйдите из режима вставки, нажав клавишу ESC.

1. Сохраните файл и закройте редактор VI. Для этого введите приведенную ниже команду.

    ```bash
    :wq
    ```

1. Разверните jumpbox.

   ```bash
   terraform apply
   ```

По завершении развертывания содержимое группы ресурсов выглядит, как показано на снимке экрана ниже.

![Группа ресурсов масштабируемого набора виртуальных машин Terraform](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-contents-final.png)

> [!NOTE]
> Возможность выполнения входа с указанием пароля отключена в точке перехода и в развернутом вами масштабируемом наборе виртуальных машин. Чтобы получить доступ к виртуальным машинам, выполните вход с помощью SSH.

## <a name="environment-cleanup"></a>Очистка среды

Чтобы удалить ресурсы Terraform, которые вы создали при работе с этим руководством, введите следующую команду в Cloud Shell:

```bash
terraform destroy
```

Процесс удаления может занять несколько минут.

## <a name="next-steps"></a>Дополнительная информация
Изучив эту статью, вы научились создавать масштабируемые наборы виртуальных машин Azure с помощью Terraform. Ниже приведены ресурсы, содержащие дополнительные сведения о Terraform в Azure.

[Документация по Terraform в Azure](https://docs.microsoft.com/azure/terraform/)
[Документации по поставщику Azure для Terraform](https://aka.ms/terraform)
[Источник поставщика Azure для Terraform](https://aka.ms/tfgit)
[Модули Terraform для Azure](https://aka.ms/tfmodules)
