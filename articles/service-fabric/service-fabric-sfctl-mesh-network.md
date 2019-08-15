---
title: Интерфейс командной строки Azure Service Fabric — sfctl mesh network | Документация Майкрософт
description: Сведения о командах sfctl mesh network интерфейса командной строки Service Fabric.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: e92fbc3856bba69db11aba6ad5d85e814bf30a81
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035043"
---
# <a name="sfctl-mesh-network"></a>sfctl mesh network
Здесь приведены сведения о получении и удалении ресурсов mesh network.

## <a name="commands"></a>Команды

|Command|Описание|
| --- | --- |
| delete | Удаляет сетевой ресурс. |
| list | Составляет список всех сетевых ресурсов. |
| show | Предоставляет сетевой ресурс с заданным именем. |

## <a name="sfctl-mesh-network-delete"></a>sfctl mesh network delete
Удаляет сетевой ресурс.

Удаляет ресурс приложения, идентифицированного по имени.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --name -n [обязательный параметр] | Имя сети. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-mesh-network-list"></a>sfctl mesh network list
Составляет список всех сетевых ресурсов.

Возвращает сведения обо всех сетевых ресурсах в указанной группе ресурсов. Эти сведения содержат описание и другие свойства сети.

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-mesh-network-show"></a>sfctl mesh network show
Предоставляет сетевой ресурс с заданным именем.

Предоставляет сведения о сетевых ресурсах с заданным именем. Эти сведения содержат описание и другие свойства сети.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --name -n [обязательный параметр] | Имя сети. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |


## <a name="next-steps"></a>Следующие шаги
- [Настройте](service-fabric-cli.md) интерфейс командной строки Service Fabric.
- Узнайте, как использовать интерфейс командной строки Service Fabric, с помощью [примеров сценариев](/azure/service-fabric/scripts/sfctl-upgrade-application).