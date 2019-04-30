---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: ae29451e3f7ec263f296e69656a5c66045334687
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61126725"
---
1. Войдите в подписку Azure, выполнив действия, перечисленные в статье [Connect to an Azure subscription from the Azure Command-Line Interface (Azure CLI)](/cli/azure/authenticate-azure-cli) (Подключение к подписке Azure с помощью интерфейса командной строки Azure (Azure CLI)).

2. Убедитесь, что вы находитесь в режиме классического развертывания, следующим образом:

    ```azurecli
    azure config mode asm
    ```

3. Среди доступных образов найдите образ Linux, который нужно загрузить, следующим образом:

   ```azurecli   
    azure vm image list | grep "Linux"
    ```
   
    В окне командной строки Windows вместо оператора grep используйте **find** .
   
4. С помощью команды `azure vm create` создайте виртуальную машину на основе образа Linux из приведенного выше списка. На этом шаге создаются облачная служба и учетная запись хранения. Вы также можете подключить эту виртуальную машину к существующей облачной службе с помощью параметра `-c`. Создайте конечную точку SSH для входа на виртуальную машину Linux с помощью параметра `-e`. В следующем примере создается виртуальная машина с именем `myVM` с помощью образа `Ubuntu-14_04_4-LTS` в расположение `West US` и добавляется имя пользователя `ops`:
   
    ```azurecli
    azure vm create myVM \
        b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB \
        -g ops -p P@ssw0rd! -z "Small" -e -l "West US"
    ```

    Вы должны увидеть результат, аналогичный приведенному ниже.

    ```azurecli
    info:    Executing command vm create
    + Looking up image b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB
    + Looking up cloud service
    info:    cloud service myVM not found.
    + Creating cloud service
    + Retrieving storage accounts
    + Creating VM
    info:    vm create command OK
    ```
   
   > [!NOTE]
   > Для виртуальной машины Linux в `vm create` необходимо указать параметр `-e`. После создания виртуальной машины включить протокол SSH невозможно. Дополнительные сведения см. в статье [Использование SSH с Linux в Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

5. Вы можете проверить атрибуты виртуальной машины с помощью команды `azure vm show`. В следующем примере предоставляются сведения о виртуальной машине с именем `myVM`:

    ```azurecli   
    azure vm show myVM
    ```

6. Запустите виртуальную машину с помощью команды `azure vm start` следующим образом:

    ```azurecli
    azure vm start myVM
    ```

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о всех командах классического Azure CLI для виртуальных машин см. в статье [Using the Azure classic CLI with the Classic deployment API](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) (Использование классического Azure CLI с API классического развертывания).

