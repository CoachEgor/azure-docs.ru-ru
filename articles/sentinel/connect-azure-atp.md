---
title: Подключайте данные из Azure ATP предварительную версию Sentinel Azure | Документация Майкрософт
description: Узнайте, как соединиться с Azure ATP данных Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5bf3cc44-ecda-4c78-8a63-31ab42f43605
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 77f745f92133f4f43cd2a65f2b69ded1eff9e8ed
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620618"
---
# <a name="connect-data-from-azure-advanced-threat-protection-atp"></a>Подключайте данные из Azure Advanced Threat Protection (ATP)

> [!IMPORTANT]
> Сейчас Azure Sentinel предоставляется в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Можно осуществлять потоковую передачу журналов из [Azure Advanced Threat Protection](https://docs.microsoft.com/azure-advanced-threat-protection/what-is-atp) в Azure Sentinel одним щелчком.

## <a name="prerequisites"></a>Предварительные требования

- Пользователь с глобальным администратором или разрешения администратора безопасности
- Необходимо быть пользователем закрытой предварительной версии Azure ATP

## <a name="connect-to-azure-atp"></a>Подключение к Azure ATP

Убедитесь, что в частной предварительной версии Azure ATP [включена в сети](https://docs.microsoft.com/azure-advanced-threat-protection/install-atp-step1).
Если развертывается Azure ATP и приема данных, подозрительные оповещения можно легко настроить потоковую передачу в Azure Sentinel. Может занять до 24 часов для оповещений начать потоковую передачу в Azure Sentinel.



1. В Azure Sentinel, выберите **соединители данных** и нажмите кнопку **Azure ATP** плитку.

2. Щелкните **Подключить**.

6. Чтобы использовать соответствующей схемы в Log Analytics для оповещений Azure ATP, поиск **SecurityAlert**.

## <a name="next-steps"></a>Следующие шаги
В этом документе вы узнали, как подключиться к Azure Sentinel Azure Advanced Threat Protection. Ознакомьтесь с дополнительными сведениями об Azure Sentinel в соответствующих статьях.
- Узнайте, как [получить представление о данных и потенциальные угрозы](quickstart-get-visibility.md).
- Начало работы [обнаружение угроз с помощью Azure Sentinel](tutorial-detect-threats.md).

