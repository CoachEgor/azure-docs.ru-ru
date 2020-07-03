---
title: Установка & развертывание агента Linux C
description: Узнайте, как установить центр безопасности Azure для агента Интернета вещей на 32-и 64-разрядной платформе Linux.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 3ccf2aec-106a-4d2c-8079-5f3e8f2afdcb
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: d9f9602a19a266c70b17422e90566f72de2978f6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81311187"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>Развертывание агента безопасности Центра безопасности Azure для Интернета вещей на основе C для Linux

В этом руководство объясняется, как установить и развернуть центр безопасности Azure для агента безопасности на основе IoT C в Linux.

Из этого руководства вы узнаете, как выполнить следующие задачи:

> [!div class="checklist"]
> * Установить
> * Проверка развертывания
> * Удаление агента
> * Устранение неполадок

## <a name="prerequisites"></a>Предварительные условия

Сведения о других платформах и разновидностях агентов см. в разделе Выбор подходящего [агента безопасности](how-to-deploy-agent.md).

1. Для развертывания агента безопасности требуются права локального администратора на компьютере, который вы хотите установить (sudo).

1. [Создайте модуль безопасности](quickstart-create-security-twin.md) для устройства.

## <a name="installation"></a>Установка

Чтобы установить и развернуть агент безопасности, используйте следующий рабочий процесс:

1. Скачайте последнюю версию на компьютер с сайта [GitHub](https://aka.ms/iot-security-github-c).

1. Извлеките содержимое пакета и перейдите в папку _/СРК/инсталлатион_ .

1. Добавьте разрешения на выполнение **скрипта инсталлсекуритяжент** , выполнив следующую команду:

   ```
   chmod +x InstallSecurityAgent.sh
   ```

1. Затем выполните приведенную ниже команду:

   ```
   ./InstallSecurityAgent.sh -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -i
   ```

   Дополнительные сведения о параметрах проверки подлинности см. в [этой статье](concept-security-agent-authentication-methods.md).

Этот скрипт выполняет следующую функцию:

1. Установка необходимых компонентов.

1. Добавляет пользователя службы (с отключенным интерактивным входом).

1. Установка агента в качестве **управляющей программы** (предполагается, что устройство использует **systemd** для управления службой).

1. Настройка агента с предоставленными параметрами проверки подлинности.

Для получения дополнительной справки запустите скрипт с параметром –help:

```./InstallSecurityAgent.sh --help```

### <a name="uninstall-the-agent"></a>Удаление агента

Чтобы удалить агент, запустите сценарий с параметром –-uninstall:

```./InstallSecurityAgent.sh -–uninstall```

## <a name="troubleshooting"></a>Устранение неполадок

Проверьте состояние развертывания, выполнив следующую команду:

```systemctl status ASCIoTAgent.service```

## <a name="next-steps"></a>Дальнейшие шаги

- Ознакомьтесь с [обзором](overview.md) центра безопасности Azure для службы IOT
- Узнайте больше о центре безопасности Azure для [архитектуры](architecture.md) IOT
- Включите [службу](quickstart-onboard-iot-hub.md).
- Ознакомьтесь с [вопросами и](resources-frequently-asked-questions.md) ответами
- Общие сведения об [оповещениях системы безопасности](concept-security-alerts.md)
