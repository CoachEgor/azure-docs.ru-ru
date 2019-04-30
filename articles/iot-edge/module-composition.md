---
title: Объявление модулей и маршрутов в манифестах развертывания — Azure IoT Edge | Документация Майкрософт
description: Узнайте, как в манифесте развертывания объявляются развертываемые модули и способы их развертывания и как создать маршруты для передачи сообщений между этими модулями.
author: kgremban
manager: philmea
ms.author: v-yiso
origin.date: 03/28/2019
ms.date: 04/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: f4a562cab445398986c1b8f379f6cb90ca843342
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61363214"
---
# <a name="learn-how-to-deploy-modules-and-establish-routes-in-iot-edge"></a>Сведения о развертывании модулей и установлении маршрутов в IoT Edge

На каждом устройстве IoT Edge работают по меньшей мере два модуля – $edgeAgent и $edgeHub, входящие в среду выполнения IoT Edge. Помимо этого, на любом устройстве IoT Edge можно запустить множество модулей для выполнения любого числа процессов. Эти модули развертываются на устройстве одновременно, поэтому IoT Edge позволяет объявить набор устанавливаемых модулей и настройки для совместной работы. 

*Манифест развертывания* — это документ JSON, который описывает:

* двойник модуля **агента IoT Edge**, который содержит образ контейнера для каждого модуля, учетные данные для доступа к частным реестрам контейнеров и инструкции по созданию и администрированию каждого модуля;
* двойник модуля **центра IoT Edge**, которая описывает поток сообщений между модулями и в конечном итоге к Центру Интернета вещей;
* (необязательно) требуемые свойства дополнительных двойников модулей.

Все устройства IoT Edge должны быть настроены с помощью манифеста развертывания. Недавно установленная среда выполнения IoT Edge будет сообщать код ошибки, пока не будет настроена с помощью допустимого манифеста. 

В руководствах по Azure IoT Edge манифест развертывания создается с помощью мастера на портале Azure IoT Edge. Также с помощью REST или пакета SDK службы Центра Интернета вещей можно применить манифест развертывания программно. Дополнительные сведения см. в статье [Основные сведения о развертываниях IoT Edge для отдельных устройств или в требуемом масштабе](module-deployment-monitoring.md).

## <a name="create-a-deployment-manifest"></a>Создание манифеста развертывания

В общих чертах манифест развертывания представляет собой список двойников модулей, для которых настроены требуемые свойства. Манифест развертывания сообщает устройству IoT Edge (или группе устройств), какие модули нужно установить и как их нужно настроить. Манифесты развертывания содержат *требуемые свойства* для каждого двойника модуля. Устройства IoT Edge передают *сообщаемые свойства* от каждого модуля. 

В каждый манифест должны быть включены два обязательных модуля: `$edgeAgent` и `$edgeHub`. Эти модули являются частью среды выполнения IoT Edge, которая управляет устройством IoT Edge и запущенными на нем модулями. См. дополнительные сведения о [среде выполнения Azure IoT Edge и ее архитектуре](iot-edge-runtime.md).

Кроме двух модулей среды выполнения, вы можете добавить до 20 собственных модулей, чтобы выполнять их на устройстве IoT Edge. 

Манифест развертывания, который содержит только среду выполнения IoT Edge (модули edgeAgent и edgeHub), тоже считается допустимым.

Манифесты развертывания имеют такую структуру:

```json
{
    "modulesContent": {
        "$edgeAgent": { // required
            "properties.desired": {
                // desired properties of the Edge agent
                // includes the image URIs of all modules
                // includes container registry credentials
            }
        },
        "$edgeHub": { //required
            "properties.desired": {
                // desired properties of the Edge hub
                // includes the routing information between modules, and to IoT Hub
            }
        },
        "module1": {  // optional
            "properties.desired": {
                // desired properties of module1
            }
        },
        "module2": {  // optional
            "properties.desired": {
                // desired properties of module2
            }
        },
        ...
    }
}
```

## <a name="configure-modules"></a>Настройка модулей

Определите, как среда выполнения IoT Edge устанавливает модули в развертывании. Агент IoT Edge — это компонент среды выполнения, который управляет установкой, обновлением и отчетами о состоянии для устройства IoT Edge. Таким образом, в двойник модуля $edgeAgent следует включить сведения по управлению и настройке для всех модулей. Эти сведения включают параметры конфигурации для самого агента Edge Интернета вещей. 

Полный список свойств, которые могут или должны быть включены, см. в разделе [свойства агента IoT Edge и концентратор Edge Интернета вещей](module-edgeagent-edgehub.md).

Свойства $edgeAgent имеют такую структуру:

```json
"$edgeAgent": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
            "settings":{
                "registryCredentials":{ // give the edge agent access to container images that aren't public
                    }
                }
            }
        },
        "systemModules": {
            "edgeAgent": {
                // configuration and management details
            },
            "edgeHub": {
                // configuration and management details
            }
        },
        "modules": {
            "module1": { // optional
                // configuration and management details
            },
            "module2": { // optional
                // configuration and management details
            }
        }
    }
},
```

## <a name="declare-routes"></a>Объявление маршрутов

Центр IoT Edge управляет взаимодействием между модулями, Центром Интернета вещей и всеми конечными устройствами. Для этой цели двойник модуля $edgeHub содержит требуемое свойство с именем *routes*, в котором описываются правила передачи сообщений в конкретном развертывании. Один экземпляр развертывания может иметь несколько маршрутов.

Маршруты объявляются в требуемых свойствах **$edgeHub**. Синтаксис объявления следующий:

```json
"$edgeHub": {
    "properties.desired": {
        "routes": {
            "route1": "FROM <source> WHERE <condition> INTO <sink>",
            "route2": "FROM <source> WHERE <condition> INTO <sink>"
        },
    }
}
```

Каждый маршрут имеет исходную и конечную точки. При необходимости можно также добавить условие, чтобы фильтровать сообщения. 


### <a name="source"></a>Источник

Исходная точка определяет то место, откуда поступают сообщения. IoT Edge может перенаправлять сообщения от конечных устройств или модулей.

Свойство source может принимать любое из следующих значений:

| Источник | Описание |
| ------ | ----------- |
| `/*` | Все сообщения, отправляемые с устройства в облако, и уведомления об изменениях двойника из любого модуля или конечного устройства. |
| `/twinChangeNotifications` | Любые изменения двойника (сообщаемые свойства) из любого модуля или конечного устройства. |
| `/messages/*` | Все сообщения, отправляемые с устройства в облако модулем или конечным устройством с выходными данными или без них. |
| `/messages/modules/*` | Все сообщения с устройства в облако, отправленные модулем с некоторыми выходными данными или без них. |
| `/messages/modules/<moduleId>/*` | Все сообщения, отправляемые с устройства в облако конкретным модулем с выходными данными или без них. |
| `/messages/modules/<moduleId>/outputs/*` | Все сообщения, отправляемые с устройства в облако конкретным модулем с выходными данными или без них. |
| `/messages/modules/<moduleId>/outputs/<output>` | Все сообщения, отправляемые с устройства в облако конкретным модулем с выходными данными или без них. |

### <a name="condition"></a>Условие
Условие является необязательным при объявлении маршрута. Если нужно передать все сообщения от конечной точки к исходной, просто не заполняйте предложение **WHERE**. Можно также воспользоваться [языком запросов Центра Интернета вещей](../iot-hub/iot-hub-devguide-routing-query-syntax.md) для фильтрации определенных сообщений или типов сообщений, удовлетворяющих заданному условию. Маршруты IoT Edge не поддерживает фильтрацию сообщений на основе тегов или свойств двойников. 

Сообщения, которые проходят между модулями в IoT Edge, форматируются так же, как сообщения, которые проходят между вашими устройствами и Центром Интернета вещей. Все сообщения представлены в формате JSON с параметрами **systemProperties**, **appProperties** и **body**. 

Запросы для всех трех параметров создаются с помощью следующего синтаксиса: 

* свойства системы — `$<propertyName>` или `{$<propertyName>}`;
* свойства приложения — `<propertyName>`;
* свойства текста —`$body.<propertyName>`. 

Примеры создания запросов к свойствам сообщений см. в разделе [Выражения запросов по маршрутам сообщений, отправляемых с устройства в облако](../iot-hub/iot-hub-devguide-routing-query-syntax.md).

Характерным примером для IoT Edge является ситуация, когда вам нужно выполнить фильтрацию для сообщений, поступивших на устройство шлюза с конечного устройства. Поступающие от модулей сообщения содержат системное свойство с именем **connectionModuleId**. Так что, если нужно направить сообщения от конечных устройств к Центру Интернета вещей, используйте следующий маршрут, чтобы исключить сообщения от модулей.

```query
FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO $upstream
```

### <a name="sink"></a>Приемник
Конечная точка определяет пункт назначения отправленных сообщений. Только модули и Центр Интернета вещей могут получать сообщения. Сообщения невозможно перенаправить на другие устройства. Для свойства приемника не поддерживаются подстановочные знаки. 

Свойство sink может принимать любое из следующих значений:

| Приемник | Описание |
| ---- | ----------- |
| `$upstream` | Отправляет сообщение в Центр Интернета вещей. |
| `BrokeredEndpoint("/modules/<moduleId>/inputs/<input>")` | Отправка сообщений на определенный вход конкретного модуля |

IoT Edge предоставляет гарантии как минимум однократной доставки. Концентратор IoT Edge хранит сообщения локально в случае, если маршрут не может доставить сообщение в качестве приемника. Например если концентратор IoT Edge не удается подключиться к центру Интернета вещей или целевой модуль не подключен.

Концентратор IoT Edge сохраняет сообщения до времени, указанного в `storeAndForwardConfiguration.timeToLiveSecs` свойство [требуемых свойств концентратора Edge Интернета вещей](module-edgeagent-edgehub.md).

## <a name="define-or-update-desired-properties"></a>Определение или обновление требуемых свойств 

Манифест развертывания определяет требуемые средства для каждого модуля, развернутого в устройстве IoT Edge. Требуемые свойства в манифесте развертывания переопределяют любые требуемые свойства, находящиеся на данный момент в двойнике модуля.

Если вы не укажете требуемые свойства двойника модуля в манифесте развертывания, Центр Интернета вещей не будет изменять двойник модуля. Вместо этого требуемые свойства можно задать программным способом.

Для изменения двойников модуля используются те же механизмы, что и для двойников устройства. Дополнительные сведения см. в статье [Общие сведения о двойниках модулей и их использование в Центре Интернета вещей](../iot-hub/iot-hub-devguide-module-twins.md).   

## <a name="deployment-manifest-example"></a>Пример манифеста развертывания

Ниже демонстрируется пример допустимого документа с манифестом развертывания.

```json
{
  "modulesContent": {
    "$edgeAgent": {
      "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
          "type": "docker",
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": "",
            "registryCredentials": {
              "ContosoRegistry": {
                "username": "myacr",
                "password": "<password>",
                "address": "myacr.azurecr.io"
              }
            }
          }
        },
        "systemModules": {
          "edgeAgent": {
            "type": "docker",
            "settings": {
              "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
              "createOptions": ""
            }
          },
          "edgeHub": {
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
              "createOptions": ""
            }
          }
        },
        "modules": {
          "tempSensor": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
              "createOptions": "{}"
            }
          },
          "filtermodule": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "myacr.azurecr.io/filtermodule:latest",
              "createOptions": "{}"
            }
          }
        }
      }
    },
    "$edgeHub": {
      "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {
          "sensorToFilter": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
          "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
        },
        "storeAndForwardConfiguration": {
          "timeToLiveSecs": 10
        }
      }
    }
  }
}
```

## <a name="next-steps"></a>Дальнейшие действия

* Полный список свойств, которые могут или должны быть включены в $edgeAgent и $edgeHub, см. в разделе [свойства агента IoT Edge и концентратор Edge Интернета вещей](module-edgeagent-edgehub.md).

* Теперь, когда вы знаете, как используются модули IoT Edge, ознакомьтесь со статьей [Сведения о требованиях и средствах разработки модулей IoT Edge](module-development.md).

[lnk-deploy]: module-deployment-monitoring.md
[lnk-iothub-query]: ../iot-hub/iot-hub-devguide-routing-query-syntax.md
[lnk-docker-create-options]: https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate
[lnk-docker-logging-options]: https://docs.docker.com/engine/admin/logging/overview/
[lnk-module-dev]: module-development.md
