---
title: Настройка конфигураций безопасности операционной системы в центре безопасности Azure (предварительная версия) | Документация Майкрософт
description: В этой статье описано, как настроить внутренние оценки центра безопасности.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/20/2019
ms.author: rkarlin
ms.openlocfilehash: d9dc9f3a4ab964b66e3a2cb03f4aad442c5665e9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64573606"
---
# <a name="customize-os-security-configurations-in-azure-security-center-preview"></a>Настройка конфигураций безопасности операционной системы в центре безопасности Azure (предварительная версия)

В этом пошаговом руководстве описано, как настроить внутренние оценки конфигурации безопасности операционной системы в центре безопасности Azure.

> [!NOTE]
> Возможность настройки конфигураций безопасности Операционной системы будет прекращена 31 июля 2019 г. Дополнительные сведения и альтернативные службы, см. в разделе [вывода из эксплуатации центра безопасности функций (июль 2019)](security-center-features-retirement-july2019.md#menu_securityconfigurations).

## <a name="what-are-os-security-configurations"></a>Конфигурации безопасности операционной системы

Центр безопасности Azure отслеживает конфигурации безопасности на основе [более 150 рекомендуемых правил](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) усиления защиты операционной системы, в том числе правил, связанных с брандмауэрами, аудитом, политиками паролей и многими другими компонентами. Если конфигурация компьютера уязвима к атакам, центр безопасности формирует рекомендации по безопасности.

Настраивая правила, организации могут контролировать параметры конфигурации своей среды. Вы можете задать пользовательские политики оценки, а затем применить их на всех компьютерах в подписке.

> [!NOTE]
> - Сейчас настройка конфигурации безопасности поддерживается только в таких операционных системах: Windows Server 2008, 2008 R2, 2012, 2012 R2 и 2016.
> - Конфигурация применяется ко всем виртуальным машинам и компьютерам, подключенным ко всем рабочим областям в рамках выбранной подписки.
> - Настроить конфигурацию безопасности операционной системы можно только на уровне "Стандартный" центра безопасности.
>
>

Правила конфигурации безопасности ОС можно настроить несколькими способами: включив или отключив определенное правило, изменив нужный параметр имеющегося правила или добавив новое правило на основе поддерживаемых типов правил (реестр, политика аудита и политика безопасности). Сейчас нужный параметр должен быть точным значением.

Новые правила должны иметь такой же формат и структуру, как и другие имеющиеся правила того же типа.

> [!NOTE]
> Чтобы настроить конфигурацию безопасности операционной системы, необходимо иметь роль *Владелец подписки*, *Участник подписки* или *Администратор безопасности*.
>
>

## <a name="customize-the-default-os-security-configuration"></a>Настройка конфигурации безопасности операционной системы по умолчанию

Чтобы настроить конфигурацию безопасности операционной системы по умолчанию в центре безопасности, сделайте следующее:

1.  Откройте панель мониторинга **центра безопасности**.

2.  В области слева выберите **Политика безопасности**.      

    ![Список политик безопасности](media/security-center-customize-os-security-config/manual-provision.png)

3.  В строке подписки, которую нужно настроить, выберите **Изменить параметры**.

4. Выберите **Изменение настроек системы безопасности**.  

    ![Окно Edit security configurations (Изменение конфигураций безопасности)](media/security-center-customize-os-security-config/blade.png)

5. Следуйте инструкциям, чтобы скачать, изменить и отправить измененный файл.

   > [!NOTE]
   > По умолчанию скачанный файл конфигурации имеет формат *JSON*. Инструкции по изменению этого файла см. в разделе [Настройка файла конфигурации](#customize-the-configuration-file).
   >

6. Чтобы применить изменения, нажмите кнопку **Сохранить**. В противном случае политика не сохранится.

    ![Кнопка "Сохранить"](media/security-center-customize-os-security-config/save-successfully.png)

   После сохранения файла конфигурация применяется ко всем виртуальным машинам и компьютерам, подключенным к рабочим областям в рамках подписки. Этот процесс обычно занимает несколько минут, но в зависимости от размера инфраструктуры может выполняться дольше.

В любой момент текущую конфигурацию политики можно сбросить до состояния по умолчанию. Чтобы сделать это, в окне **Edit OS security configuration rules** (Изменение правил конфигурации безопасности операционной системы) выберите **Сбросить**. Подтвердите это действие, выбрав **Да** во всплывающем окне подтверждения.

![Окно подтверждения сброса](media/security-center-customize-os-security-config/edit-alert.png)

## <a name="customize-the-configuration-file"></a>Настройка файла конфигурации

В файле настройки каждой поддерживаемой версии операционной системы содержится набор правил. Каждый набор правил имеет собственное имя и уникальный идентификатор, как показано в следующем примере:

![Имя и идентификатор набора правил](media/security-center-customize-os-security-config/custom-file.png)

> [!NOTE]
> Этот пример файла изменен в Visual Studio, но, если на компьютере установлен подключаемый модуль средства просмотра JSON, можно также использовать Блокнот.
>
>

В файле настройки можно изменить как одно правило, так и все. Каждый набор содержит раздел *rules*, состоящий из трех категорий: Registry, Audit Policy и Security Policy, как показано ниже.

![Три категории набора правил](media/security-center-customize-os-security-config/rules-section.png)

Каждая категория имеет свой собственный набор атрибутов. Изменить можно следующие атрибуты:

- **expectedValue** Этот тип данных поля атрибута должен соответствовать поддерживаемым значениям *типа правила*, например:

  - **baselineRegistryRules** Это значение должно соответствовать значению [regValueType](https://msdn.microsoft.com/library/windows/desktop/ms724884), определенному в правиле.

  - **baselineAuditPolicyRules** Используйте одно из следующих строковых значений:

    - *Success and Failure*

    - *Success*

  - **baselineSecurityPolicyRules** Используйте одно из следующих строковых значений:

    - *No one*

    - Список разрешенных групп пользователей, например: *Администраторы*, *Операторы архива*

-   **state**. Эта строка может содержать параметры *Disabled* или *Enabled*. В этом выпуске строка с учетом регистра.

Изменить можно только значения этих полей. Если использовать неправильный формат или размер файла, вы не сможете сохранить изменения. Появится сообщение об ошибке, в котором указано, что необходимо передать допустимый файл конфигурации JSON.

Список других возможных ошибок см. в разделе [Коды ошибок](#error-codes).

Ниже приведены три раздела, содержащие примеры предыдущих правил. Изменить можно атрибуты *expectedValue* и *state*.

**baselineRegistryRules**
```json
    {
    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\LanManServer\\\\Parameters",
    "valueName": "restrictnullsessaccess",
    "ruleId": "f9020046-6340-451d-9548-3c45d765d06d",
    "originalId": "0f319931-aa36-4313-9320-86311c0fa623",
    "cceId": "CCE-10940-5",
    "ruleName": "Network access: Restrict anonymous access to Named Pipes and
    Shares",
    "ruleType": "Registry",
    "expectedValue": "1",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Disabled"

    }
```

**baselineAuditPolicyRules**
```json
    {
    "auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
    "ruleId": "37745508-95fb-44ec-ab0f-644ec0b16995",
    "originalId": "2ea0de1a-c71d-46c8-8350-a7dd4d447895",
    "cceId": "CCE-11001-5",
    "ruleName": "Audit Policy: Account Management: Other Account Management Events",
    "ruleType": "AuditPolicy",
    "expectedValue": "Success and Failure",
    "severity": "Critical",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Enabled"
    }
```

**baselineSecurityPolicyRules**
```json
    {
    "sectionName": "Privilege Rights",
    "settingName": "SeIncreaseWorkingSetPrivilege",
    "ruleId": "b0ec9d5e-916f-4356-83aa-c23522102b33",
    "originalId": "b61bd492-74b0-40f3-909d-36b9bf54e94c",
    "cceId": "CCE-10548-6",
    "ruleName": "Increase a process working set",
    "ruleType": "SecurityPolicy",
    "expectedValue": "Administrators, Local Service",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Enabled"
    }
```

Некоторые правила повторяются в различных типах операционных систем. Эти правила имеют одинаковый атрибут *originalId*.

## <a name="create-custom-rules"></a>Создание настраиваемых правил

Правила можно создавать. Но при этом следует учитывать следующие ограничения:

-   Версию схемы, а также атрибуты *baselineId* и *baselineName* нельзя изменять.

-   Набор правил нельзя удалить.

-   Набор правил нельзя добавить.

-   Максимально допустимое число правил (в том числе правила по умолчанию) составляет 1000.

В новых настраиваемых правилах указывается новый пользовательский источник (!= "Microsoft"). Поле *ruleId* может быть пустым или иметь значение NULL. Если оно пустое, идентификатор создает корпорация Майкрософт. В противном случае оно должно содержать допустимый глобальный идентификатор, уникальный среди всех правил (стандартных и настраиваемых). К основным полям применяются такие ограничения:

-   **originalId** Это поле может быть пустым или иметь значение NULL. Значением поля *originalId* может быть допустимый глобальный уникальный идентификатор.

-   **CCEID** Это поле может быть пустым или иметь значение NULL. Значение поля *cceId* должно быть уникальным.

-   **ruleType.** Допустимые значения (необходимо выбрать одно): Registry, AuditPolicy или SecurityPolicy.

-   **Severity.** Допустимые значения (необходимо выбрать одно): Unknown, Critical, Warning или Informational.

-   **analyzeOperation** Должно иметь значение *Equals*.

-   **auditPolicyId** Должно содержать допустимый GUID.

-   **regValueType.** Допустимые значения (необходимо выбрать одно): Int, Long, String или MultipleString.

> [!NOTE]
> Атрибут hive должен иметь значение *LocalMachine*.
>
>

Ниже приведены примеры нового настраиваемого правила.

**Реестр**:
```json
    {
    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\Netlogon\\\\MyKeyName",
    "valueName": "MyValueName",
    "originalId": "",
    "cceId": "",
    "ruleName": "My new registry rule", "baselineRuleType": "Registry",
    "expectedValue": "123", "severity": "Critical",
    "analyzeOperation": "Equals",
    "source": "MyCustomSource",
    "state": "Enabled"
    }
```
**Политика безопасности**:
```json
   {
   "sectionName": "Privilege Rights",
   "settingName": "SeDenyBatchLogonRight",
   "originalId": "",
   "cceId": "",
   "ruleName": "My new security policy rule", "baselineRuleType":
   "SecurityPolicy",
   "expectedValue": "Guests",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```
**Политика аудита**:
```json
   {
   "auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
   "originalId": "",
   "cceId": "",
   "ruleName": " My new audit policy rule ", "baselineRuleType": "AuditPolicy",
   "expectedValue": " Failure",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```

## <a name="file-upload-failures"></a>Ошибки передачи файлов

Если отправленный файл конфигурации содержит неправильные значения или ошибки форматирования, отображается сообщение об ошибке, например **Сбой действия сохранения**. Вы можете загрузить подробный CSV-отчет, чтобы исправить ошибки, а затем отправить правильный файл конфигурации.

Пример файла с ошибкой:

![Пример файла с ошибкой](media/security-center-customize-os-security-config/errors-file.png)

## <a name="error-codes"></a>Коды ошибок

В таблице ниже приведены все возможные ошибки.

| **Ошибка**                                | **Описание**                                                                                                                              |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| BaselineConfigurationSchemaVersionError  | Свойство *schemaVersion* недопустимое или пустое. Допустимое значение: *{0}* .                                                         |
| BaselineInvalidStringError               | Свойство *{0}* не может содержать символ *\\n*.                                                                                                         |
| BaselineNullRuleError                    | Список базовых правил конфигурации содержит правило со значением *NULL*.                                                                         |
| BaselineRuleCceIdNotUniqueError          | Идентификатор CCE-ID *{0}* не уникален.                                                                                                                  |
| BaselineRuleEmptyProperty                | Свойство *{0}* отсутствует или недопустимое.                                                                                                       |
| BaselineRuleIdNotInDefault               | Правило имеет свойство источника *Microsoft*, но его не удалось найти в наборе правил Microsoft по умолчанию.                                                   |
| BaselineRuleIdNotUniqueError             | Идентификатор правила не является уникальным.                                                                                                                       |
| BaselineRuleInvalidGuid                  | Свойство *{0}* недопустимое. Его значение не является допустимым идентификатором GUID.                                                                             |
| BaselineRuleInvalidHive                  | Атрибут hive должен иметь значение LocalMachine.                                                                                                                   |
| BaselineRuleNameNotUniqueError           | Имя правила не уникальное.                                                                                                                 |
| BaselineRuleNotExistInConfigError        | Не удалось найти правило в новой конфигурации. Правило нельзя удалить.                                                                     |
| BaselineRuleNotFoundError                | Не удалось найти правило в базовом наборе правил по умолчанию.                                                                                        |
| BaselineRuleNotInPlace                   | Правило соответствует правилу по умолчанию с типом {0} и внесено в список {1}.                                                                       |
| BaselineRulePropertyTooLong              | Свойство *{0}* слишком длинное. Максимально допустимая длина: {1}.                                                                                        |
| BaselineRuleRegTypeInvalidError          | Ожидаемое значение *{0}* не совпадает с определенным типом значения реестра.                                                              |
| BaselineRulesetAdded                     | Набор правил с идентификатором *{0}* не найден в конфигурации по умолчанию. Набор правил нельзя добавить.                                               |
| BaselineRulesetIdMustBeUnique            | Указанный базовый набор правил *{0}* должен быть уникальным.                                                                                           |
| BaselineRulesetNotFound                  | Набор правил с идентификатором *{0}* и имя *{1}* не найден в указанной конфигурации. Набор правил нельзя удалить.                                |
| BaselineRuleSourceNotMatch               | Правило с идентификатором *{0}* уже определен.                                                                                                       |
| BaselineRuleTypeDoesntMatch              | Тип правила по умолчанию — *{0}* .                                                                                                              |
| BaselineRuleTypeDoesntMatchError         | Фактический тип правила — *{0}* , но свойство *ruleType* имеет тип *{1}* .                                                                          |
| BaselineRuleUnpermittedChangesError      | Изменить можно только свойства *expectedValue* и *state*.                                                                       |
| BaselineTooManyRules                     | Максимально допустимое число настраиваемых правил — {0}. Указанная конфигурация содержит: правила ({1}), правила по умолчанию ({2}) и настраиваемые правила ({3}). |
| ErrorNoConfigurationStatus               | Состояние конфигурации не найдено. Требуемое состояние конфигурации: *Default* или *Custom*.                                    |
| ErrorNonEmptyRulesetOnDefault            | Состояние конфигурации имеет значение по умолчанию. Список *BaselineRulesets* должен быть пустым или иметь значение NULL.                                                          |
| ErrorNullRulesetsPropertyOnCustom        | Для состояния указанной конфигурации установлено значение *Custom*, но свойство *baselineRulesets* пустое или имеет значение NULL.                                             |
| ErrorParsingBaselineConfig               | Указанная конфигурация недействительная. Одно или несколько определенных значений имеют недопустимый тип или значение NULL.                                  |
| ErrorParsingIsDefaultProperty            | Указанное значение *configurationStatus* набора правил *{0}* недопустимое. Допустимые значения: *Default* или *Custom*.                                         |
| InCompatibleViewVersion                  | Версия представления *{0}* *не* поддерживается этим типом рабочей области.                                                                                   |
| InvalidBaselineConfigurationGeneralError | Указанная базовая конфигурация содержит одну или несколько ошибок проверки типа.                                                          |
| ViewConversionError                      | Представление имеет более старую версию, чем поддерживает рабочая область. Сбой при преобразовании представления: {0}.                                                                 |

Если у вас нет необходимых разрешений, может отобразиться такая ошибка:

![Сообщение об ошибке "Cбой действия сохранения"](media/security-center-customize-os-security-config/general-failure-error.png)

## <a name="next-steps"></a>Дальнейшие действия
В этой статье описано, как настроить внутренние оценки конфигурации безопасности операционной системы в центре безопасности. Дополнительные сведения о правилах конфигурации и ее исправлении см. на этой странице:

- [Security Center common configuration identifiers and baseline rules](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) (Базовые правила и идентификаторы общей конфигурации центра безопасности).
- Центр безопасности назначает уникальные идентификаторы правилам конфигурации на основе перечисления общей конфигурации (CCE). Дополнительные сведения см. [здесь](https://nvd.nist.gov/config/cce/index).
- Сведения об устранении уязвимостей в случае, если конфигурация операционной системы не соответствует рекомендуемым правилам конфигурации безопасности, см. в статье [Remediate security configurations in Azure Security Center](security-center-remediate-os-vulnerabilities.md) (Исправление конфигураций безопасности в центре безопасности Azure).
