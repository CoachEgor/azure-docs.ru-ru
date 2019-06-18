---
title: включение файла
description: включение файла
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: b6587a3928c2ddf0d00c90e07643525314690e42
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185424"
---
### <a name="virtual-machines"></a>Виртуальные машины

|  |  |
|---------|---------|
| [Разрешение использования пользовательского образа виртуальной машины из группы ресурсов](../articles/governance/policy/samples/allow-custom-vm-image.md) |  Требует, чтобы пользовательские образы были взяты из утвержденной группы ресурсов. Нужно указать имя утвержденной группы ресурсов. |
| [Допустимые номера SKU для учетных записей хранения и виртуальных машин](../articles/governance/policy/samples/allowed-skus-storage.md) | Требует, чтобы учетные записи хранения и виртуальные машины использовали утвержденные номера SKU. Для обеспечения утвержденных номеров SKU используются встроенные политики. Вы можете указать массив утвержденных номеров SKU виртуальных машин и учетных записей хранения. |
| [Утвержденные образы виртуальных машин:](../articles/governance/policy/samples/allowed-custom-images.md) | Требует, чтобы в среде развертывались только утвержденные пользовательские образы. Вы можете указать массив идентификаторов утвержденных образов. |
| [Проверка того, существует ли расширение](../articles/governance/policy/samples/audit-extension-not-exist.md) | Проверяет, развернуто ли расширение на виртуальной машине. Вы можете указать издателя расширения и тип, чтобы проверить, было ли оно развернуто. |
| [Запрет использования неразрешенных расширений виртуальной машины](../articles/governance/policy/samples/not-allowed-vm-extension.md) | Запрещает использование заданных расширений. Вы можете указать массив, содержащий типы запрещенных расширений. |