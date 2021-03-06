---
title: Подготовка к развертыванию решения в рабочей среде Azure IoT Edge
description: Узнайте, как взять решение Azure IoT Edge из разработки в рабочую среду, включая настройку устройств с помощью соответствующих сертификатов и создание плана развертывания для будущих обновлений кода.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 07/10/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 7cabae837656611813d44017ce2e1112f06066ef
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96013298"
---
# <a name="prepare-to-deploy-your-iot-edge-solution-in-production"></a>Подготовка к развертыванию решения IoT Edge в рабочей среде

Когда вы будете готовы перейти от разработки решения IoT Edge к его развертыванию в рабочей среде, убедитесь, что оно настроено для непрерывной работы.

Информация, приведенная в этой статье, не равна. Чтобы помочь вам расставить приоритеты, каждый раздел начинается со списков, разделяющих работу на две части: **важные** задачи, которые следует выполнить перед переходом в рабочую среду, и **полезные** для вас сведения.

## <a name="device-configuration"></a>Конфигурация устройства

Устройством IoT Edge может быть что угодно — от Raspberry Pi до ноутбука или виртуальной машины, работающей на сервере. У вас может быть доступ к устройству (физически или через виртуальное подключение), либо оно может быть изолировано в течение продолжительного времени. В любом случае необходимо убедиться, что оно настроено для правильной работы.

* **Важно**
  * Установка рабочих сертификатов
  * Составление плана управления устройствами
  * Использование Moby в качестве модуля контейнеров

* **Полезное**
  * Выбор вышестоящего протокола

### <a name="install-production-certificates"></a>Установка рабочих сертификатов

На каждом устройстве IoT Edge в рабочей среде необходимо установить сертификат от центра сертификации (ЦС). Затем этот сертификат ЦС объявляется в файле config.yaml для среды выполнения IoT Edge. В сценариях разработки и тестирования среда выполнения IoT Edge создает временные сертификаты, если в файле config. YAML не объявлено ни одного сертификата. Однако срок действия этих сертификатов истекает спустя три месяца, и они недостаточно безопасны для рабочих сценариев. В рабочих сценариях необходимо предоставить собственный сертификат ЦС устройства либо из самозаверяющего центра сертификации, либо из коммерческого центра сертификации.

> [!NOTE]
> В настоящее время ограничение в либиоссм предотвращает использование сертификатов, срок действия которых истекает 1 января 2038.

Сведения о роли сертификатов ЦС для устройств см. в статье [Сведения об использовании сертификатов Azure IoT Edge](iot-edge-certs.md).

Дополнительные сведения об установке сертификатов на устройстве IoT Edge и о ссылках на них из файла config. YAML см. в статье [Управление сертификатами на устройстве IOT Edge](how-to-manage-device-certificates.md).

### <a name="have-a-device-management-plan"></a>Составление плана управления устройствами

Прежде чем внедрять устройство в рабочую среду, следует определить, как вы будете управлять будущими обновлениями. В случае устройства IoT Edge могут обновляться перечисленные ниже компоненты.

* Встроенное ПО устройства
* Библиотеки операционной системы
* Модуль контейнеров (например, Moby)
* Управляющая программа IoT Edge
* Сертификаты ЦС

Дополнительные сведения см. [в разделе Обновление среды выполнения IOT Edge](how-to-update-iot-edge.md). Для применения современных методик обновления управляющей программы IoT Edge требуется доступ к устройству IoT Edge (физически или по протоколу SSH). При наличии множества устройств для обновления рекомендуется добавить шаги обновления в скрипт или использовать средство автоматизации, например Ansible.

### <a name="use-moby-as-the-container-engine"></a>Использование Moby в качестве модуля контейнеров

Подсистема контейнеров является необходимым условием для любого IoT Edge устройства. В рабочей среде поддерживается только moby-engine. Другие модули контейнеров, например Docker, совместимы с IoT Edge, и их можно использовать при разработке. Модуль moby-engine можно распространять при использовании с Azure IoT Edge, а корпорация Майкрософт выполняет обслуживание этого модуля.

### <a name="choose-upstream-protocol"></a>Выбор вышестоящего протокола

Можно настроить протокол (который определяет используемый порт) для вышестоящей связи с центром Интернета вещей как для агента IoT Edge, так и для центра IoT Edge. По умолчанию используется протокол AMQP, но его можно изменить в зависимости от конфигурации сети.

У двух модулей среды выполнения есть переменная среды **UpstreamProtocol**. Допустимые значения переменной:

* MQTT
* AMQP
* MQTTWS
* AMQPWS

Настройте переменную Упстреампротокол для агента IoT Edge в файле config. YAML на самом устройстве. Например, если устройство IoT Edge находится за прокси-сервером, который блокирует порты AMQP, может потребоваться настроить агент IoT Edge для использования AMQP через WebSocket (АМКПВС) для установления начального подключения к центру Интернета вещей.

После подключения устройства IoT Edge обязательно настраивайте переменную UpstreamProtocol для обоих модулей среды выполнения в будущих развертываниях. Пример этого процесса представлен в статье [Настройка устройства IoT Edge для обмена данными через прокси-сервер](how-to-configure-proxy-support.md).

## <a name="deployment"></a>Развертывание

* **Полезное**
  * Согласование с вышестоящим протоколом
  * Настройка хранилища узлов для системных модулей
  * Уменьшение объема памяти, используемого центром IoT Edge
  * Не используйте отладочные версии образов модулей

### <a name="be-consistent-with-upstream-protocol"></a>Согласование с вышестоящим протоколом

Если вы настроили агент IoT Edge на устройстве IoT Edge для использования протокола, отличного от AMQP по умолчанию, то следует объявить тот же протокол во всех последующих развертываниях. Например, если устройство IoT Edge защищено прокси-сервером, блокирующим порты AMQP, то оно наверняка настроено на подключение по протоколу AMQP через WebSocket (AMQPWS). При развертывании модулей на устройстве настройте один и тот же протокол АМКПВС для агента IoT Edge и центра IoT Edge, иначе AMQP по умолчанию переопределит параметры и не сможет снова подключиться.

Необходимо настроить только переменную среды Упстреампротокол для IoT Edgeного агента и IoT Edge модулей концентратора. Все остальные модули используют протокол, заданный в модулях среды выполнения.

Пример этого процесса представлен в статье [Настройка устройства IoT Edge для обмена данными через прокси-сервер](how-to-configure-proxy-support.md).

### <a name="set-up-host-storage-for-system-modules"></a>Настройка хранилища узлов для системных модулей

Модули концентратора IoT Edge и агента используют локальное хранилище для поддержания состояния и обеспечивают обмен сообщениями между модулями, устройствами и облаком. Для повышения надежности и производительности настройте системные модули для использования хранилища в файловой системе узла.

Дополнительные сведения см. в разделе [хранилище узлов для системных модулей](how-to-access-host-storage-from-module.md).

### <a name="reduce-memory-space-used-by-iot-edge-hub"></a>Уменьшение объема памяти, используемого центром IoT Edge

При развертывании ограниченных устройств с ограниченным объемом доступной памяти можно настроить центр IoT Edge для работы в более упрощенной емкости и использовать меньше места на диске. Однако эти конфигурации ограничивают производительность центра IoT Edge, поэтому нужно найти подходящий баланс, который подходит для вашего решения.

#### <a name="dont-optimize-for-performance-on-constrained-devices"></a>Не оптимизируйте устройства с ограниченными ресурсами для высокой производительности

Центр IoT Edge по умолчанию оптимизирован для повышения производительности, поэтому он пытается выделить большие блоки памяти. Такая конфигурация может привести к проблемам со стабильностью на небольших устройствах, таких как Raspberry Pi. При развертывании устройств с ограниченными ресурсами может потребоваться задать для переменной среды **оптимизефорперформанце** **значение false** в центре IOT Edge.

Если для **оптимизефорперформанце** задано **значение true**, головка протокола MQTT использует пуледбитебуффераллокатор, что повышает производительность, но выделяет больше памяти. Распределитель не работает хорошо в 32-разрядных операционных системах или на устройствах с нехваткой памяти. Кроме того, при оптимизации производительности Рокксдб выделяет больше памяти для своей роли в качестве локального поставщика хранилища.

Дополнительные сведения см. [в статье проблемы стабильности на небольших устройствах](troubleshoot-common-errors.md#stability-issues-on-smaller-devices).

#### <a name="disable-unused-protocols"></a>Отключение неиспользуемых протоколов

Другой способ оптимизации производительности центра IoT Edge и снижения использования памяти заключается в отключении заголовков протоколов для всех протоколов, которые не используются в решении.

Заголовки протоколов настраиваются путем настройки переменных среды для модуля IoT Edge концентратора в манифестах развертывания. Вот эти переменные:

* **amqpSettings__enabled**
* **mqttSettings__enabled**
* **httpSettings__enabled**

Имена всех трех переменных содержат *два подчеркивания*, и для них можно задавать значение true или false.

#### <a name="reduce-storage-time-for-messages"></a>Сокращение времени хранения сообщений

Модуль IoT Edge Hub временно хранит сообщения, если они не могут быть доставлены в центр Интернета вещей по какой-либо причине. Вы можете настроить период, в течение которого центр IoT Edge удерживается для недоставленных сообщений, прежде чем истечет срок их действия. При наличии проблем с памятью на устройстве можно уменьшить значение **тиметоливесекс** в модуле центра IOT Edge двойника.

Для параметра timeToLiveSecs по умолчанию задано значение 7200 секунд, то есть два часа.

### <a name="do-not-use-debug-versions-of-module-images"></a>Не используйте отладочные версии образов модулей

Переходя от разработки к рабочей среде, обязательно удалите конфигурации отладки из манифестов развертывания. Убедитесь, что ни один из образов модулей в манифестах развертывания не имеет суффикса **\. отладки** . Если вы добавили параметры создания, чтобы открыть порты в модулях для отладки, удалите и эти параметры.

## <a name="container-management"></a>Управление контейнерами

* **Важно**
  * Управление доступом к реестру контейнеров
  * Использование тегов для управления версиями
* **Полезное**
  * Сохранение контейнеров среды выполнения в частном реестре

### <a name="manage-access-to-your-container-registry"></a>Управление доступом к реестру контейнеров

Прежде чем развертывать модули на рабочих устройствах IoT Edge, убедитесь, что вы контролируете доступ к реестру контейнеров, чтобы посторонние не могли получать доступ к образам контейнеров и вносить в них изменения. Используйте частный, а не общедоступный, реестр контейнеров для управления их образами.

В руководствах и другой документации говорится, что следует использовать на устройстве IoT Edge те же учетные данные реестра контейнеров, что и на компьютере для разработки. Эти инструкции призваны лишь помочь вам настраивать среды тестирования и разработки. Их не следует соблюдать в рабочей среде.

Для более безопасного доступа к реестру можно выбрать [Параметры проверки подлинности](../container-registry/container-registry-authentication.md). Популярная и Рекомендуемая проверка подлинности — использование субъекта-службы Active Directory, который хорошо подходит для приложений и служб для извлечения образов контейнеров в автоматическом или ином автоматическом режиме (без монитора), как и IoT Edge устройств.

Чтобы создать субъект-службу, выполните два сценария, как описано в разделе [Создание субъекта-службы](../container-registry/container-registry-auth-service-principal.md#create-a-service-principal). Эти сценарии выполняют следующие задачи:

* Первый скрипт создает субъект-службу. Он выводит идентификатор субъекта-службы и пароль субъекта-службы. Храните эти значения в записях безопасно.

* Второй скрипт создает назначения ролей для предоставления субъекту-службе, который при необходимости может выполняться в дальнейшем. Мы рекомендуем применить роль пользователя **акрпулл** для `role` параметра. Список ролей см. в статье [роли и разрешения реестра контейнеров Azure](../container-registry/container-registry-roles.md).

Для проверки подлинности с помощью субъекта-службы укажите идентификатор и пароль субъекта-службы, полученные из первого скрипта. Укажите эти учетные данные в манифесте развертывания.

* В качестве имени пользователя или идентификатора клиента укажите идентификатор субъекта-службы.

* Для пароля или секрета клиента укажите пароль субъекта-службы.

> [!NOTE]
> После реализации расширенной проверки подлинности безопасности отключите параметр **администратора** , чтобы доступ к имени пользователя и паролю по умолчанию больше не был доступен. В реестре контейнеров в портал Azure в меню слева в разделе **Параметры** выберите **ключи доступа**.

### <a name="use-tags-to-manage-versions"></a>Использование тегов для управления версиями

Тег — это концепция DOCKER, которую можно использовать для различения версий контейнеров DOCKER. Теги — это суффиксы (например, **1.0**), указываемые в конце имени репозитория контейнеров. Пример: **mcr.microsoft.com/azureiotedge-agent:1.0**. Теги можно в любой момент менять, чтобы указывать на другой контейнер, поэтому вашей команде следует договориться о правилах обновления образов модулей в будущем.

Теги также помогают принудительно устанавливать обновления на устройствах IoT Edge. Отправляя обновленную версию модуля в реестр контейнеров, увеличивайте тег. Затем отправьте новое развертывание на устройства с увеличенным тегом. Модуль контейнеров распознает увеличенный тег как новую версию и извлечет последнюю версию модуля на устройство.

Пример соглашения о тегах представлен в разделе [Сведения о тегах IoT Edge](how-to-update-iot-edge.md#understand-iot-edge-tags). Он поможет вам понять, как IoT Edge использует последовательные и конкретные теги для отслеживания версий.

### <a name="store-runtime-containers-in-your-private-registry"></a>Сохранение контейнеров среды выполнения в частном реестре

Вам известно о хранении образов контейнеров для пользовательских модулей кода в частном реестре Azure, но его также можно использовать для хранения общедоступных образов контейнеров, например для модулей среды выполнения edgeAgent и Едгхуб. Это может потребоваться, если у вас очень строгие ограничения брандмауэра, так как эти контейнеры среды выполнения хранятся в реестре контейнеров (мкр) Microsoft.

Получите образы с помощью команды DOCKER Pull для размещения в частном реестре. Имейте в виду, что вам потребуется обновить образы с каждым новым выпуском IoT Edge среды выполнения.

| Контейнер среды выполнения IoT Edge | Команда DOCKER Pull |
| --- | --- |
| [Агент Azure IoT Edge](https://hub.docker.com/_/microsoft-azureiotedge-agent) | `docker pull mcr.microsoft.com/azureiotedge-agent` |
| [Концентратор Azure IoT Edge](https://hub.docker.com/_/microsoft-azureiotedge-hub) | `docker pull mcr.microsoft.com/azureiotedge-hub` |

Затем обязательно обновите ссылки на образ в deployment.template.jsв файле для системных модулей edgeAgent и edgeHub. Замените на `mcr.microsoft.com` имя и сервер реестра для обоих модулей.

* EdgeAgent

    `"image": "<registry name and server>/azureiotedge-agent:1.0",`

* EdgeHub

    `"image": "<registry name and server>/azureiotedge-hub:1.0",`

## <a name="networking"></a>Сеть

* **Полезное**
  * Просмотр конфигурации исходящих и входящих подключений
  * Разрешить подключения от устройств IoT Edge
  * Настройка связи через прокси-сервер

### <a name="review-outboundinbound-configuration"></a>Просмотр конфигурации исходящих и входящих подключений

Каналы связи между Центром Интернета вещей Azure и IoT Edge всегда настраиваются как исходящие. Для большинства сценариев IoT Edge достаточно трех подключений. Модуль контейнеров должен соединяться с реестрами контейнеров, в которых хранятся образы модулей. Среда выполнения IoT Edge должна подключиться к Центру Интернета вещей, чтобы получить сведения о конфигурации устройств, а также отправить сообщение и данные телеметрии. А если вы используете автоматическую подготовку, управляющая программа IoT Edge должна подключиться к службе подготовки устройств. Дополнительные сведения см. в разделе [Правила конфигурации брандмауэра и порта для развертывания IoT Edge](troubleshoot.md#check-your-firewall-and-port-configuration-rules).

### <a name="allow-connections-from-iot-edge-devices"></a>Разрешить подключения от устройств IoT Edge

Если для настройки сети требуется явное разрешение подключений с IoT Edge устройств, ознакомьтесь со следующим списком компонентов IoT Edge:

* **Агент IoT Edge** открывает постоянное подключение AMQP/MQTT к Центру Интернета вещей (возможно, через объекты WebSocket).
* **Центр IoT Edge** открывает одно постоянное подключение AMQP или несколько подключений MQTT к Центру Интернета вещей (возможно, через объекты WebSocket).
* **Управляющая программа IoT Edge** совершает прерывистые HTTPS-вызовы к Центру Интернета вещей.

Во всех трех случаях имя DNS соответствует шаблону \*.azure-devices.net.

Кроме того, **модуль контейнеров** совершает вызовы реестров контейнеров по протоколу HTTPS. Чтобы получить образы контейнеров для среды выполнения IoT Edge, используйте имя DNS mcr.microsoft.com. Модуль контейнеров подключается к другим реестрам в соответствии с настройками развертывания.

Этот контрольный список является отправной точкой для настройки правил брандмауэра:

   | URL-адрес (\* = подстановочный знак) | Исходящие порты TCP | Использование |
   | ----- | ----- | ----- |
   | mcr.microsoft.com  | 443 | Реестр контейнеров Майкрософт |
   | global.azure-devices-provisioning.net  | 443 | Доступ к DPS (необязательно) |
   | \*.azurecr.io | 443 | Личные и сторонние реестры контейнеров |
   | \*.blob.core.windows.net | 443 | Скачивание изменений образа реестра контейнеров Azure из хранилища BLOB-объектов |
   | \*.azure-devices.net | 5671, 8883, 443 | Доступ к Центру Интернета вещей |
   | \*.docker.io  | 443 | Доступ к концентратору DOCKER (необязательно) |

Некоторые из этих правил брандмауэра наследуются от реестра контейнеров Azure. Дополнительные сведения см. в статье [Настройка правил для доступа к реестру контейнеров Azure за брандмауэром](../container-registry/container-registry-firewall-access-rules.md).

> [!NOTE]
> Чтобы обеспечить согласованное полное доменное имя между конечной точкой и конечными точками данных, начиная с **15 июня, 2020** конечная точка данных реестра контейнеров Майкрософт изменится с `*.cdn.mscr.io` на `*.data.mcr.microsoft.com`  
> Дополнительные сведения см. в [статье Настройка правил брандмауэра для клиента реестра контейнеров Microsoft](https://github.com/microsoft/containerregistry/blob/master/client-firewall-rules.md) .

Если вы не хотите настраивать брандмауэр для разрешения доступа к общедоступным реестрам контейнеров, вы можете хранить образы в частном реестре контейнеров, как описано в разделе [Хранение контейнеров среды выполнения в частном реестре](#store-runtime-containers-in-your-private-registry).

### <a name="configure-communication-through-a-proxy"></a>Настройка связи через прокси-сервер

Если устройства будут развернуты в сети, использующей прокси-сервер, они должны иметь возможность взаимодействовать через прокси-сервер, чтобы получать доступ к Центру Интернета вещей и реестрам контейнеров. Дополнительные сведения см. в статье [Настройка устройства IoT Edge для обмена данными через прокси-сервер](how-to-configure-proxy-support.md).

## <a name="solution-management"></a>Управление решениями

* **Полезное**
  * Настройка журналов и диагностики
  * Тесты и конвейеры CI/CD

### <a name="set-up-logs-and-diagnostics"></a>Настройка журналов и диагностики

В Linux управляющая программа IoT Edge использует журналы в качестве драйвера ведения журнала по умолчанию. Чтобы запрашивать журналы управляющей программы, можно использовать программу командной строки `journalctl`. В Windows управляющая программа IoT Edge использует диагностику PowerShell. Чтобы запрашивать журналы из управляющей программы, используйте команду `Get-IoTEdgeLog`. Модули IoT Edge используют для ведения журнала JSON-драйвер, который используется по умолчанию.  

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

При тестировании развертывания IoT Edge обычно можно получать доступ к своим устройствам для извлечения журналов и устранения неполадок. В случае развертывания эта возможность может отсутствовать. Подумайте, как вы будете собирать сведения об устройствах в рабочей среде. Один вариант — использовать модуль ведения журнала, который собирает сведения от других модулей и отправляет их в облако. К примерам модулей ведения журнала относится [logspout-loganalytics](https://github.com/veyalla/logspout-loganalytics). Вы также можете создать собственный модуль.

### <a name="place-limits-on-log-size"></a>Ограничить размер журнала

По умолчанию подсистема контейнеров значок Кита не устанавливает ограничения размера журнала контейнера. Со временем это может привести к заполнению устройством журналов и исчерпанию дискового пространства. Чтобы избежать этого, рассмотрите следующие варианты.

#### <a name="option-set-global-limits-that-apply-to-all-container-modules"></a>Параметр: задает глобальные ограничения, которые применяются ко всем модулям контейнеров.

Можно ограничить размер файлов журнала контейнера в параметрах журнала модуля контейнера. В следующем примере для драйвера журнала задается значение `json-file` (рекомендуется) с ограничениями на размер и число файлов:

```JSON
{
    "log-driver": "json-file",
    "log-opts": {
        "max-size": "10m",
        "max-file": "3"
    }
}
```

Добавьте (или добавьте) эти сведения в файл с именем `daemon.json` и поместите его в нужное расположение для платформы устройства.

| Платформа | Расположение |
| -------- | -------- |
| Linux | `/etc/docker/` |
| Windows | `C:\ProgramData\iotedge-moby\config\` |

Чтобы изменения вступили в силу, необходимо перезапустить модуль контейнера.

#### <a name="option-adjust-log-settings-for-each-container-module"></a>Параметр: Настройка параметров журнала для каждого модуля контейнера

Это можно сделать в **креатеоптионс** каждого модуля. Пример:

```yml
"createOptions": {
    "HostConfig": {
        "LogConfig": {
            "Type": "json-file",
            "Config": {
                "max-size": "10m",
                "max-file": "3"
            }
        }
    }
}
```

#### <a name="additional-options-on-linux-systems"></a>Дополнительные параметры в системах Linux

* Настройте модуль контейнеров для отправки журналов в `systemd` [Журнал](https://docs.docker.com/config/containers/logging/journald/) , указав `journald` в качестве используемого по умолчанию драйвера ведения журнала.

* Периодически удаляйте старые журналы с устройства, установив средство logrotate. Используйте следующую спецификацию файла:

   ```txt
   /var/lib/docker/containers/*/*-json.log{
       copytruncate
       daily
       rotate7
       delaycompress
       compress
       notifempty
       missingok
   }
   ```

### <a name="consider-tests-and-cicd-pipelines"></a>Тесты и конвейеры CI/CD

Для максимально эффективного развертывания IoT Edge рекомендуем интегрировать рабочую среду с конвейерами CI/CD и тестирования. Azure IoT Edge поддерживает множество платформ CI/CD, включая Azure DevOps. Дополнительные сведения см. в статье [Непрерывная интеграция и непрерывное развертывание в Azure IoT Edge](how-to-continuous-integration-continuous-deployment.md).

## <a name="next-steps"></a>Следующие шаги

* Дополнительные сведения см. в статье об [автоматическом развертывании IoT Edge](module-deployment-monitoring.md).
* Узнайте о поддержке [непрерывных интеграции и развертывания](how-to-continuous-integration-continuous-deployment.md) в IoT Edge.
