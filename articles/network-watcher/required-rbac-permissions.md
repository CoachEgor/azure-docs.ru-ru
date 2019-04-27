---
title: Разрешения, необходимые для использования возможностей службы "Наблюдатель за сетями Azure" | Документация Майкрософт
description: Узнайте, какие разрешения на управление доступом на основе ролей доступны для работы с возможностями Наблюдателя за сетями.
services: network-watcher
documentationcenter: ''
author: KumudD
manager: twooley
editor: ''
ms.assetid: ''
ms.service: network-watcher
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: kumud
ms.openlocfilehash: 429f7862901814fbd2017c395706fbfa2c345f72
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60652900"
---
# <a name="role-based-access-control-permissions-required-to-use-network-watcher-capabilities"></a>Разрешения на управление доступом на основе ролей, необходимые для использования возможностей Наблюдателя за сетями

Управление доступом Azure на основе ролей (RBAC) позволяет назначать только определенные действия членам вашей организации, которые необходимы им для выполнения назначенных им обязанностей. Чтобы использовать возможности Наблюдателя за сетями, учетной записи, в которую вы вошли, должна быть назначена встроенная роль [Владелец](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#owner), [Участник](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#contributor) или [Участник сетей](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#network-contributor) или назначена [пользовательская роль](../role-based-access-control/custom-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json), которая обладает свойствами, перечисленными для каждой возможности Наблюдателя за сетями в следующем разделе. Дополнительные сведения о Наблюдателе за сетями см. в [этой статье](network-watcher-monitoring-overview.md).

## <a name="network-watcher"></a>Наблюдатель за сетями

| Действие                                                              | ИМЯ                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/read                              | Получение Наблюдателя за сетями                                          |
| Microsoft.Network/networkWatchers/write                             | Создание или обновление Наблюдателя за сетями                             |
| Microsoft.Network/networkWatchers/delete                            | Удаление Наблюдателя за сетями                                       |

## <a name="nsg-flow-logs"></a>Журналы потоков NSG

| Действие                                                              | ИМЯ                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/configureFlowLog/action           | Настройка журнала потока                                           |
| Microsoft.Network/networkWatchers/queryFlowLogStatus/action         | Запрос состояния для журнала потока                                    |

## <a name="connection-troubleshoot"></a>Устранение неполадок подключения

| Действие                                                              | ИМЯ                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/connectivityCheck/action          | Инициирование теста на устранение неполадок подключения
| Microsoft.Network/networkWatchers/queryTroubleshootResult/action    | Запрос результатов теста устранения неполадок подключения                |
| Microsoft.Network/networkWatchers/troubleshoot/action               | Выполнение теста на устранение неполадок подключения                             |

## <a name="connection-monitor"></a>Монитор подключения

| Действие                                                              | ИМЯ                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/connectionMonitors/start/action   | Запуск монитора подключения                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/stop/action    | Остановка монитора подключения                                      |
| Microsoft.Network/networkWatchers/connectionMonitors/query/action   | Запрос монитора подключения                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/read           | Получение монитора подключения                                       |
| Microsoft.Network/networkWatchers/connectionMonitors/write          | Создание монитора подключения                                    |
| Microsoft.Network/networkWatchers/connectionMonitors/delete         | Удаление монитора подключения                                    |

## <a name="packet-capture"></a>Запись пакетов

| Действие                                                              | ИМЯ                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Запрос состояния записи пакета                           |
| Microsoft.Network/networkWatchers/packetCaptures/stop/action        | Прекращение записи пакета                                          |
| Microsoft.Network/networkWatchers/packetCaptures/read               | Получение записи пакета                                           |
| Microsoft.Network/networkWatchers/packetCaptures/write              | Создание записи пакетов                                        |
| Microsoft.Network/networkWatchers/packetCaptures/delete             | Удаление записи пакета                                        |

## <a name="ip-flow-verify"></a>Проверка IP-потока

| Действие                                                              | ИМЯ                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/ipFlowVerify/action               | Проверка IP-потока                                              |

## <a name="next-hop"></a>Следующий прыжок

| Действие                                                              | ИМЯ                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/nextHop/action                    | Получение следующего прыжка с виртуальной машины                                     |

## <a name="network-security-group-view"></a>Представление группы безопасности сети

| Действие                                                              | ИМЯ                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/securityGroupView/action          | Просмотр групп безопасности                                           |

## <a name="topology"></a>Топология

| Действие                                                              | ИМЯ                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/topology/action                   | Получение топологии                                                   |

## <a name="reachability-report"></a>Отчет о возможности доступа

| Действие                                                              | ИМЯ                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/azureReachabilityReport/action    | Получение отчета о возможности доступа Azure                               |

## <a name="additional-actions"></a>Дополнительные действия

Возможностям Наблюдателя за сетью также требуются следующие действия:

- Microsoft.Authorization/\*/Read
- Microsoft.Resources/subscriptions/resourceGroups/Read;
- Microsoft.Storage/storageAccounts/Read
- Microsoft.Storage/storageAccounts/listServiceSas/Action;
- Microsoft.Storage/storageAccounts/listAccountSas/Action;
- Microsoft.Storage/storageAccounts/listKeys/Action;
- Microsoft.Compute/virtualMachines/Read;
- Microsoft.Compute/virtualMachines/Write;
- Microsoft.Compute/virtualMachines/extensions/Read
- Microsoft.Compute/virtualMachines/extensions/Write
- Microsoft.Compute/virtualMachineScaleSets/Read;
- Microsoft.Compute/virtualMachineScaleSets/Write;
- Microsoft.Compute/virtualMachineScaleSets/extensions/Read
- Microsoft.Compute/virtualMachineScaleSets/extensions/Write
- Microsoft.Insights/alertRules/*
- Microsoft.Support/*
