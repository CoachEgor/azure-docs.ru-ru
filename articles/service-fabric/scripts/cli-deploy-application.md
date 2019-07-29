---
title: Пример развертывания сценария интерфейса командной строки Azure Service Fabric (sfctl)
description: Развертывание приложения в кластер Azure Service Fabric с помощью интерфейса командной строки Azure Service Fabric
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 04/16/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: d25ed8d27c13e20641d1495c47fdcf6ce2edeb75
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/29/2019
ms.locfileid: "68592208"
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Развертывание приложения в кластера Service Fabric

Этот пример сценария копирует пакет приложения в хранилище образов кластера, регистрирует тип приложения в кластере и создает экземпляр приложения с типом приложения. В это же время также создаются все стандартные службы.

При необходимости установите [интерфейс командной строки Service Fabric](../service-fabric-cli.md).

## <a name="sample-script"></a>Пример скрипта

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Очистка развертывания

По завершении с помощью сценария [удаления](cli-remove-application.md) можно удалить приложение. Сценарий удаления удаляет экземпляр приложения, отменяет регистрацию типа приложения и удаляет пакет приложения из хранилища образов.

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения см. в [документации интерфейса командной строки Service Fabric](../service-fabric-cli.md).

Дополнительные примеры сценариев интерфейса командной строки Service Fabric для Azure Service Fabric см. в статье [Примеры сценариев Azure PowerShell](../samples-cli.md).
