---
title: Использование ключей SSH для Windows для подключения к виртуальным машинам Linux
description: Узнайте, как создавать и использовать ключи SSH на компьютере Windows для подключения к виртуальной машине Linux в Azure.
author: cynthn
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 11/26/2018
ms.author: cynthn
ms.openlocfilehash: cdf901ca56c150cfed6ba3d462ce493d40bd2488
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2020
ms.locfileid: "81757995"
---
# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Как использовать ключи SSH с Windows в Azure

В этой статье описаны способы создания и использования ключей *Secure Shell* (SSH) на компьютере Windows для создания виртуальной машины Linux в Azure и подключения к ней. Вы можете ознакомиться с [кратким](mac-create-ssh-keys.md) или [подробным](create-ssh-keys-detailed.md) руководством по использованию ключей SSH в клиенте Linux или macOS.

[!INCLUDE [virtual-machines-common-ssh-overview](../../../includes/virtual-machines-common-ssh-overview.md)]

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="windows-packages-and-ssh-clients"></a>Пакеты Windows и SSH-клиенты
Вы подключаетесь к Linux VMs и управляете им в Azure с помощью *клиента SSH.* Компьютеры под управлением Linux или macOS обычно используют набор команд SSH для создания ключей SSH и управления ими, а также для установления SSH-подключений. 

На компьютерах Windows не всегда установлены сравнимые команды SSH. Последние версии Windows 10 предоставляют [клиентские команды OpenSSH](https://blogs.msdn.microsoft.com/commandline/2018/03/07/windows10v1803/) для создания и управления sSH-ключами и создания SSH-соединений из командного запроса. Последние версии Windows 10 с [подсистемой Windows для Linux](https://docs.microsoft.com/windows/wsl/about) позволяют запускать и использовать такие служебные программы, как SSH-клиент, в собственной оболочке Bash. 

Другие общераспространенные SSH-клиенты для Windows, доступные для установки локально, входят в состав следующих пакетов:

* [PuTTY;](https://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [Git для Windows](https://git-for-windows.github.io/)
* [MobaXterm](https://mobaxterm.mobatek.net/)
* [Cygwin](https://cygwin.com/)

Вы можете также использовать служебные программы SSH, доступные в Bash в [Azure Cloud Shell](../../cloud-shell/overview.md). 

* Доступ к облачной оболочке в веб-браузере на [https://shell.azure.com](https://shell.azure.com) портале Azure или на [ней.](https://portal.azure.com) 
* Чтобы использовать окно терминала для Cloud Shell в Visual Studio Code, установите [расширение для учетных записей Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

## <a name="create-an-ssh-key-pair"></a>Создание пары ключей SSH
В этом разделе ниже описано два способа создания пары ключей SSH в Windows. Вы можете использовать команду оболочки (`ssh-keygen`) или средство графического пользовательского инструмента (PuTTYgen). Также обратите внимание, при использовании Powershell для создания ключа, загрузить открытый ключ как ssh.com (SECSH) формат. При использовании CLI преобразуйте ключ в формат OpenSSH перед загрузкой. 

### <a name="create-ssh-keys-with-ssh-keygen"></a>Создание ключей SSH с помощью ssh-keygen

Использование командной оболочки на Windows, которая поддерживает клиентские средства SSH (или использует Azure Cloud Shell), создайте пару ключей SSH с помощью команды `ssh-keygen`. Введите приведенную ниже команду и укажите сведения в ответ на запросы. Если в выбранном каталоге существует пара ключей SSH, они будут перезаписаны. 

```bash
ssh-keygen -t rsa -b 2048
```

Чтобы получить больше общих сведений, ознакомьтесь с [краткими](mac-create-ssh-keys.md) или [подробными](create-ssh-keys-detailed.md) инструкциями по созданию ключей SSH с помощью `ssh-keygen`.

### <a name="create-ssh-keys-with-puttygen"></a>Создание ключей SSH с помощью PuTTYgen

Если вы предпочитаете создать ключи SSH с помощью инструмента с графическим пользовательским интерфейсом, можно использовать генератор ключей PuTTYgen, входящий в состав [скачиваемого пакета PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). 

Чтобы создать пару ключей SSH-RSA с помощью PuTTYgen, выполните следующее.

1. Запустите PuTTYgen.

2. Щелкните **Создать**. По умолчанию PuTTYgen создает 2048-разрядный ключ SSH-2 RSA.

4. Переместите указатель мыши на пустую область, чтобы обеспечить случайность ключа.

5. После создания открытого ключа при необходимости введите и подтвердите парольную фразу. Она будет запрашиваться при аутентификации на виртуальной машине с помощью вашего закрытого ключа SSH. Если не создать парольную фразу, пользователь, получивший ваш закрытый ключ, сможет войти на любую виртуальную машину или в службу, использующую этот ключ. Мы рекомендуем создать ее. Однако если вы забудете парольную фразу, восстановить ее будет невозможно.

6. Открытый ключ отображается в верхней части окна. Скопируйте этот весь открытый ключ и вставьте его на портале Azure или в шаблон Azure Resource Manager при создании виртуальной машины Linux. Кроме того, нажмите кнопку **Save public key** (Сохранить открытый ключ), чтобы сохранить копию ключа на компьютере:

    ![Сохранение файла PuTTY с открытым ключом](./media/ssh-from-windows/save-public-key.png)

7. При необходимости сохранить закрытый ключ в формате закрытого ключа PuTTy (PPK-файл) щелкните **Save private key** (Сохранить закрытый ключ). PPK-файл требуется, если вы хотите использовать PuTTY позже для установления SSH-подключения к виртуальной машине.

    ![Сохранение файла закрытого ключа PuTTY](./media/ssh-from-windows/save-ppk-file.png)

    Если вы хотите сохранить закрытый ключ в формате OpenSSH, формат частного ключа, используемый многими клиентами SSH, выберите ключ **Конверсии** > **Export OpenSSH.**

## <a name="provide-an-ssh-public-key-when-deploying-a-vm"></a>Предоставление открытого ключа SSH при развертывании виртуальной машины

Чтобы создать виртуальную машину Linux, которая использует ключи SSH для аутентификации, укажите свой открытый ключ SSH при создании виртуальной машины с помощью портала Azure или других методов.

В следующем примере показано, как скопировать и вставить открытый ключ на портале Azure при создании виртуальной машины Linux. Открытый ключ обычно хранится в каталоге ~/.ssh/authorized_key на новой виртуальной машине.

   ![Использование открытого ключа при создании виртуальной машины на портале Azure](./media/ssh-from-windows/use-public-key-azure-portal.png)


## <a name="connect-to-your-vm"></a>Подключение к виртуальной машине

Один из способов установить SSH-подключение к виртуальной машине Linux из Windows — использовать клиент SSH. Это предпочтительный метод, если в вашей системе Windows установлен клиент SSH или вы используете инструменты SSH в Bash в Azure Cloud Shell. Если вы предпочитаете использовать инструмент с графическим пользовательским интерфейсом, то можете установить подключение с помощью PuTTY.  

### <a name="use-an-ssh-client"></a>Использование клиента SSH
С помощью открытого ключа, развернутого на виртуальной машине Azure, и закрытого ключа в локальной системе установите SSH-подключение к виртуальной машине, используя ее IP-адрес или DNS-имя. Замените *azureuser* и *myvm.westus.cloudapp.azure.com* в приведенной команде, указав имя пользователя администратора и полное доменное имя (или IP-адрес).

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Если при создании пары ключей вы настроили парольную фразу, введите ее при появлении запроса во время входа в систему.

Если виртуальная машина использует политику доступа JIT, запросите доступ, прежде чем подключиться к виртуальной машине. Дополнительные сведения о политике JIT см. в статье [Управление доступом к виртуальным машинам с помощью JIT-доступа](../../security-center/security-center-just-in-time.md).

### <a name="connect-with-putty"></a>Подключение с помощью PuTTY

Если вы установили [скачиваемый пакет PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) и ранее создали закрытый ключ PuTTY (PPK-файл), то можете подключиться к виртуальной машине Linux с помощью PuTTY.

1. Запустите PuTTY.

2. Введите имя узла или IP-адрес виртуальной машины на портале Azure.

    ![Открытие нового подключения PuTTY](./media/ssh-from-windows/putty-new-connection.png)

3. Выберите категорию **Connection** > **SSH** > **Auth.** Укажите закрытый ключ PuTTY (PPK-файл).

    ![Выбор закрытого ключа PuTTY для аутентификации](./media/ssh-from-windows/putty-auth-dialog.png)

4. Щелкните **Open** (Открыть) для подключения к виртуальной машине.

## <a name="next-steps"></a>Следующие шаги

* Подробные инструкции, параметры и дополнительные примеры работы с ключами SSH приведены в статье [Подробные инструкции: создание ключей SSH для аутентификации на виртуальной машине Linux в Azure и управление этими ключами](create-ssh-keys-detailed.md).

* Для создания ключей SSH и установления SSH-подключений к виртуальным машинам Linux можно также использовать PowerShell в Azure Cloud Shell. Ознакомьтесь с разделом [Краткое руководство по использованию PowerShell в Azure Cloud Shell (предварительная версия)](../../cloud-shell/quickstart-powershell.md#ssh).

* Если при подключении к виртуальным машинам Linux по протоколу SSH возникают трудности, см. статью [Troubleshoot SSH connections to an Azure Linux VM that fails, errors out, or is refused](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Устранение неполадок с ошибками, сбоем или отклонением SSH-подключения к виртуальной машине Azure Linux).
