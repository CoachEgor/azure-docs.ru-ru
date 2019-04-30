---
title: Обнаружение встроенного брандмауэра Управляемого экземпляра Базы данных SQL Azure | Документация Майкрософт
description: Узнайте, как проверить защиту встроенного брандмауэра в Управляемом экземпляре Базы данных SQL.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
manager: digimobile
origin.date: 12/04/2018
ms.date: 12/31/2018
ms.openlocfilehash: 774455a2901782ef52b213c6a13c17636e28b1a4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60699652"
---
# <a name="verifying-the-managed-instance-built-in-firewall"></a>Проверка встроенного брандмауэра Управляемого экземпляра

[Обязательные правила безопасности входящего трафика](sql-database-managed-instance-connectivity-architecture.md#mandatory-inbound-security-rules) Управляемого экземпляра требуют, чтобы порты управления 9000, 9003, 1438, 1440, 1452 были открыты из **любого источника** в группе безопасности сети (NSG), защищающей Управляемый экземпляр. Несмотря на то, что эти порты открыты на уровне группы безопасности сети, они защищены на уровне сети встроенным брандмауэром.

## <a name="verify-firewall"></a>Проверка брандмауэра

Чтобы проверить эти порты, используйте любое средство сканирования системы безопасности для проверки этих портов. На следующем снимке экрана показано, как использовать одно из этих средств.

![Проверка встроенного брандмауэра](./media/sql-database-managed-instance-management-endpoint/03_verify_firewall.png)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Управляемых экземплярах и подключениях см. в статье [Архитектура подключения к Управляемому экземпляру Базы данных SQL Azure](sql-database-managed-instance-connectivity-architecture.md).