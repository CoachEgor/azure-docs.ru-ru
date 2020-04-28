---
title: Настройка расширенной защиты от угроз — управляемый экземпляр
description: Расширенная защита от угроз обнаруживает аномальные действия базы данных, указывающие потенциальные угрозы безопасности для базы данных в управляемом экземпляре.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
ms.date: 08/05/2019
ms.openlocfilehash: 69292a934af8b8777f11ab58ed3fe306abf8b408
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2020
ms.locfileid: "73822548"
---
# <a name="configure-advanced-threat-protection-in-azure-sql-database-managed-instance"></a>Настройка расширенной защиты от угроз в управляемом экземпляре базы данных SQL Azure

[Расширенная защита от угроз](sql-database-threat-detection-overview.md) для [управляемого экземпляра](sql-database-managed-instance-index.yml) определяет аномальные действия, указывающие на необычные и потенциально опасные попытки доступа к базам данных или их использования. Расширенная защита от угроз позволяет определить **потенциальное внедрение кода SQL**, **получить доступ из необычного расположения или центра обработки данных**, **получить доступ из незнакомого основного или потенциально опасного приложения**и **выполнить подбор учетных данных SQL** [. Дополнительные](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts)сведения см.

Вы можете получать уведомления об обнаруженных угрозах с помощью [уведомлений по электронной почте](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) или [портала Azure](sql-database-threat-detection-overview.md#explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal)

[Расширенная защита от угроз](sql-database-threat-detection-overview.md) является частью предложения [расширенной системы безопасности данных](sql-database-advanced-data-security.md) (ADS), которое является единым пакетом для расширенных возможностей обеспечения безопасности SQL. Доступ к расширенной защите от угроз можно получить через центральный портал SQL ADS. Там же ею можно управлять.

## <a name="set-up-advanced-threat-protection-in-the-azure-portal"></a>Настройка расширенной защиты от угроз в портал Azure

1. Запустите портал Azure по адресу [https://portal.azure.com](https://portal.azure.com).
2. Перейдите на страницу настройки управляемого экземпляра, который требуется защитить. На странице **Параметры** выберите пункт **Расширенная защита данных**.
3. На странице настройки дополнительной защиты данных
   - Включите **расширенную** защиту данных.
   - Настройте **список сообщений электронной почты** для получения оповещений системы безопасности при обнаружении аномальных действий базы данных.
   - Выберите **учетную запись хранения Azure** для сохранения записей аудита аномальных угроз.
   - Выберите **типы расширенной защиты от угроз** , которые вы хотите настроить. Дополнительные сведения об [оповещениях Advanced Threat protection](sql-database-threat-detection-overview.md).
4. Нажмите кнопку **сохранить** , чтобы сохранить новую или обновленную расширенную политику безопасности данных.

   ![Расширенная защита от угроз](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

   > [!NOTE]
   > Цены на снимках экрана не всегда соответствуют текущей цене и являются примером.

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [Advanced Threat protection](sql-database-threat-detection-overview.md).
- Дополнительные сведения см. в статье [Использование Управляемого экземпляра базы данных SQL с виртуальными сетями и почти полной совместимостью](sql-database-managed-instance.md).
- Дополнительные сведения о [Advanced Threat Protection для отдельной базы данных](sql-database-threat-detection.md).
- Дополнительные сведения об [аудите управляемого экземпляра](https://go.microsoft.com/fwlink/?linkid=869430).
- Дополнительные сведения о [центре безопасности Azure](https://docs.microsoft.com/azure/security-center/security-center-intro).
