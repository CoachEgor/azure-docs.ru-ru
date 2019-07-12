---
title: Часто задаваемые вопросы о виртуальных машинах Linux в Azure | Документация Майкрософт
description: В этой статье содержатся ответы на некоторые распространенные вопросы о виртуальных машинах Linux, созданных с помощью модели Resource Manager.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-management
ms.assetid: 3648e09c-1115-4818-93c6-688d7a54a353
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/08/2019
ms.author: cynthn
ms.openlocfilehash: ae636917a32aae823d0d31b415f9ef9ac16e9c2f
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67667770"
---
# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Часто задаваемые вопросы по виртуальным машинам Linux
В этой статье содержатся ответы на некоторые распространенные вопросы о виртуальных машинах Linux, созданных в Azure посредством модели развертывания с помощью Resource Manager. Версия этой статьи для Windows — [Часто задаваемые вопросы по виртуальным машинам Windows](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="what-can-i-run-on-an-azure-vm"></a>Что можно запускать на виртуальной машине Azure?
Все подписчики могут запускать на виртуальной машине Azure серверное программное обеспечение. Дополнительные сведения см. в статье [Linux в Azure — рекомендованные дистрибутивы](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Какой объем памяти можно использовать с виртуальной машиной?
Каждый диск данных может иметь не более 32 767 Гиб. Количество дисков данных, которое можно использовать, зависит от размера виртуальной машины. Дополнительную информацию см. в статье [Размеры виртуальных машин](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Управляемые диски Azure — это рекомендуемые предложения хранилища дисков, используемые с виртуальными машинами Azure и предназначенные для постоянного хранения данных. Можно использовать несколько управляемых дисков для каждой виртуальной машины. Существуют два класса управляемых дисков с разными возможностями долговременного хранения: управляемые диски ценовых категорий "Премиум" и "Стандартный". Сведения о ценах см. на [странице с расценками на управляемые диски](https://azure.microsoft.com/pricing/details/managed-disks).

Учетная запись хранения Azure также предоставляет хранилище для диска операционной системы и любых дисков данных. Каждый из этих дисков представляет собой VHD-файл, хранящийся как страничный BLOB-объект. Информацию о ценах см. в статье [Информация о ценах на хранилища](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Как получить доступ к своей виртуальной машине?
Установите удаленное подключение для входа виртуальную машину, используя Secure Shell (SSH). Ознакомьтесь с инструкциями по подключению [из Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) или [Linux и Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). По умолчанию SSH поддерживает не более 10 параллельных подключений. Число доступных параллельных подключений можно увеличить, изменив файл конфигурации.

Если возникают проблемы, ознакомьтесь со статьей об [устранении неполадок с подключением Secure Shell (SSH)](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Можно ли использовать временный диск (/dev/sdb1) для хранения данных?
Не используйте временный диск (/dev/sdb1) для хранения данных. Он обеспечивает лишь временное хранение. Вы рискуете потерять данные, которые невозможно будет восстановить.

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Можно ли копировать или клонировать существующую виртуальную машину Azure?
Да. Указания доступны в статье [Создание копии виртуальной машины Linux, работающей в Azure](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Почему в Azure Resource Manager не отображаются регионы центральной и восточной Канады?
При создании виртуальных машин в существующих подписках Azure эти два новых региона не регистрируются автоматически. Регистрация осуществляется автоматически при развертывании виртуальной машины на портале Azure в любом другом регионе с помощью Azure Resource Manager. После развертывания виртуальной машины в любом другом регионе Azure новые регионы станут доступными для последующих виртуальных машин.

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Создав виртуальную машину, могу я добавить к ней сетевой адаптер?
Да, теперь это возможно. Сначала виртуальную машину нужно остановить и отменить ее распределение. Затем можно добавить или удалить сетевую карту (за исключением последней сетевой карты на виртуальной машине). 

## <a name="are-there-any-computer-name-requirements"></a>Есть ли какие-либо требования к имени компьютера?
Да. Длина имени компьютера не должна превышать 64 знака. Дополнительные сведения об именовании ресурсов см. в [правилах и ограничениях соглашений об именовании](/azure/architecture/best-practices/naming-conventions).

## <a name="are-there-any-resource-group-name-requirements"></a>Есть ли какие-либо требования к имени группы ресурсов?
Да. Длина имени группы ресурсов не должна превышать 90 знаков. Дополнительные сведения о группах ресурсов см. в [правилах и ограничениях соглашений об именовании](/azure/architecture/best-practices/naming-conventions).

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Какие требования к имени пользователя при создании виртуальной машины?

Имя пользователя должно содержать от 1 до 32 знаков.

Не допускаются следующие имена пользователей:

| | | | |
|-----------------|-----------|--------------------|----------|
| `administrator` | `admin`   | `user`             | `user1`  |
| `test`          | `user2`   | `test1`            | `user3`  |
| `admin1`        | `1`       | `123`              | `a`      |
| `actuser`       | `adm`     | `admin2`           | `aspnet` |
| `backup`        | `console` | `david`            | `guest`  |
| `john`          | `owner`   | `root`             | `server` |
| `sql`           | `support` | `support_388945a0` | `sys`    |
| `test2`         | `test3`   | `user4`            | `user5`  |
| `video`         |

## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Какие требования к паролю при создании виртуальной машины?

Существуют требования к длине, в зависимости от инструмента, который вы используете различных пароль:
 - Портал — от 12 до 72 символов
 - PowerShell — от 8 до 123 символов
 - Интерфейс командной строки — от 12 до 123
 

Пароли также должны удовлетворяться 3 из следующих 4 требований сложности:

* используются строчные знаки;
* используются прописные знаки;
* используется цифра;
* используется специальный знак (регулярное выражение [\W_]).

Не допускаются следующие пароли:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$$word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Password!</td>
        <td style="text-align:center">Password1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">iloveyou!</td>
    </tr>
</table>
