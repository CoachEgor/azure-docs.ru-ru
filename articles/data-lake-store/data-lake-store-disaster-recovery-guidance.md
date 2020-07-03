---
title: Руководство по аварийному восстановлению Azure Data Lake Storage 1-го поколения | Документы Майкрософт
description: Руководство по аварийному восстановлению Azure Data Lake Storage 1-го поколения
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: twooley
ms.openlocfilehash: b33977ca5184ea07b5651be18e3a132d30ce4b39
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75966052"
---
# <a name="disaster-recovery-guidance-for-data-in-azure-data-lake-storage-gen1"></a>Руководство по аварийному восстановлению данных в Azure Data Lake Storage 1-го поколения

Azure Data Lake Storage 1-го поколения предоставляет локально избыточное хранилище (LRS). Таким образом, благодаря автоматическим репликам данные в учетной записи Data Lake Storage 1-го поколения устойчивы к временным сбоям оборудования в центре обработки данных. Это гарантирует устойчивость и высокий уровень доступности данных, а также соответствие требованиям соглашения об уровне обслуживания Data Lake Storage 1-го поколения. В этой статье приведены рекомендации по защите данных в случае редких сбоев оборудования в регионе или случайного удаления.

## <a name="disaster-recovery-guidance"></a>Руководство по аварийному восстановлению
Крайне важно, чтобы каждый клиент подготовил свой собственный план аварийного восстановления. Ознакомьтесь со сведениями в этой статье, чтобы составить свой план аварийного восстановления. Ниже приведены некоторые ресурсы, которые помогу составить план аварийного восстановления.

* [Аварийное восстановление и высокий уровень доступности для приложений Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Техническое руководство по обеспечению устойчивости в Azure](../resiliency/resiliency-technical-guidance.md)

### <a name="best-practices"></a>Рекомендации
Мы рекомендуем копировать важные данные в другую учетную запись Data Lake Storage 1-го поколения, расположенную в другом регионе. Выполняйте эту процедуру так часто, как того требует ваш план аварийного восстановления. Копировать данные можно несколькими способами, в том числе с помощью [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) или [фабрики данных Azure](../data-factory/connector-azure-data-lake-store.md). Фабрика данных Azure — это полезная служба для создания и развертывания конвейеров перемещения данных на регулярной основе.

В случае регионального сбоя можно получить доступ к данным в регионе, куда были скопированы данные. Чтобы определить состояние службы Azure в разных регионах мира, можно использовать [панель мониторинга работоспособности службы Azure](https://azure.microsoft.com/status/).

## <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Руководство по восстановлению данных после повреждения или случайного удаления
Несмотря на то, что Data Lake Storage 1-го поколения обеспечивает устойчивость данных за счет автоматических реплик, они по-прежнему не защищены от повреждения или случайного удаления в приложении (а также разработчиками или пользователями).

### <a name="best-practices"></a>Рекомендации
Чтобы предотвратить случайное удаление данных, для начала мы рекомендуем настроить для своей учетной записи Data Lake Storage 1-го поколения правильные политики доступа,  в том числе применение [блокировок ресурсов Azure](../azure-resource-manager/management/lock-resources.md) для блокировки важных ресурсов, а также контроль доступа на уровне учетной записи и файлов с помощью [компонентов обеспечения безопасности Data Lake Storage 1-го поколения](data-lake-store-security-overview.md). Кроме того, мы советуем регулярно создавать копии важных данных в других учетных записях Data Lake Storage 1-го поколения, папках и подписках Azure с помощью [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) или [фабрики данных Azure](../data-factory/connector-azure-data-lake-store.md).  Таким образом вы сможете восстановить данные после повреждения или случайного удаления. Фабрика данных Azure — это полезная служба для создания и развертывания конвейеров перемещения данных на регулярной основе.

Организации могут также включить [ведение журнала диагностики](data-lake-store-diagnostic-logs.md) для своих учетных записей Data Lake Storage 1-го поколения. Это позволит собирать журналы аудита доступа к данным, содержащие сведения о пользователях, которые могли удалить или обновить файл.

## <a name="next-steps"></a>Дальнейшие действия
* [Начало работы с Azure Data Lake Storage 1-го поколения](data-lake-store-get-started-portal.md)
* [Защита данных в Data Lake Storage Gen1](data-lake-store-secure-data.md)

