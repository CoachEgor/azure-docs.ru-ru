---
title: Создание пула узлов клиентов виртуальных рабочих столов Windows — Azure
description: Устранение неполадок и разрешение проблем с пулом клиентов и узлов во время установки среды клиента виртуальных рабочих столов Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 5825466c099a8c57477f2d9d0420da74ccb2e96d
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2020
ms.locfileid: "82615400"
---
# <a name="tenant-and-host-pool-creation"></a>Создание пула узлов и клиента

>[!IMPORTANT]
>Это содержимое относится к выпуску 2019, который не поддерживает Azure Resource Manager объекты виртуальных рабочих столов Windows. Если вы пытаетесь управлять Azure Resource Manager объектов виртуальных рабочих столов Windows, представленных в обновлении пружины 2020, см. [эту статью](../troubleshoot-set-up-issues.md).

В этой статье рассматриваются проблемы во время первоначальной настройки клиента виртуальных рабочих столов Windows и соответствующей инфраструктуры пула узлов сеансов.

## <a name="provide-feedback"></a>Предоставление отзыва

Посетите [техническое сообщество Виртуального рабочего стола Windows](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop), чтобы обсудить службу "Виртуальный рабочий стол Windows" с группой разработчиков и активными членами сообщества.

## <a name="acquiring-the-windows-10-enterprise-multi-session-image"></a>Получение многосеансового образа Windows 10 Корпоративная

Чтобы использовать многосеансовый образ Windows 10 Enterprise, перейдите в Azure Marketplace, выберите начало **работы** > с**Microsoft Windows 10** > и [Windows 10 Корпоративная для виртуальных рабочих столов версии 1809](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsdesktop.windows-10?tab=PlansAndPrice).

![Снимок экрана: выбор Windows 10 Корпоративная для виртуальных рабочих столов версии 1809.](../media/AzureMarketPlace.png)

## <a name="creating-windows-virtual-desktop-tenant"></a>Создание клиента виртуальных рабочих столов Windows

В этом разделе рассматриваются потенциальные проблемы при создании клиента виртуальных рабочих столов Windows.

### <a name="error-the-user-isnt-authorized-to-query-the-management-service"></a>Ошибка: пользователь не имеет разрешения на запрос к службе управления

![Снимок экрана окна PowerShell, в котором пользователь не имеет права выполнять запросы к службе управления.](../media/UserNotAuthorizedNewTenant.png)

Пример необработанной ошибки:

```Error
   New-RdsTenant : User isn't authorized to query the management service.
   ActivityId: ad604c3a-85c6-4b41-9b81-5138162e5559
   Powershell commands to diagnose the failure:
   Get-RdsDiagnosticActivities -ActivityId ad604c3a-85c6-4b41-9b81-5138162e5559
   At line:1 char:1
   + New-RdsTenant -Name "testDesktopTenant" -AadTenantId "01234567-89ab-c ...
   + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
       + CategoryInfo          : FromStdErr: (Microsoft.RDInf...nt.NewRdsTenant:NewRdsTenant) [New-RdsTenant], RdsPowerSh
      ellException
       + FullyQualifiedErrorId : UnauthorizedAccess,Microsoft.RDInfra.RDPowershell.Tenant.NewRdsTenant
```

**Причина:** Пользователю, выполнившего вход, не назначена роль Тенанткреатор в своих Azure Active Directory.

**Исправление:** Следуйте инструкциям в статье [назначение роли приложения тенанткреатор пользователю в клиенте Azure Active Directory](tenant-setup-azure-active-directory.md#assign-the-tenantcreator-application-role). После выполнения инструкций Вы получите пользователя, назначенного роли Тенанткреатор.

![Снимок экрана с назначенной ролью Тенанткреатор.](../media/TenantCreatorRoleAssigned.png)

## <a name="creating-windows-virtual-desktop-session-host-vms"></a>Создание виртуальных машин узла сеансов виртуальных рабочих столов Windows

Виртуальные машины узла сеансов могут создаваться несколькими способами, но группа виртуальных рабочих столов Windows поддерживает только проблемы подготовки виртуальных машин, связанные с предложением [Azure Marketplace](https://azuremarketplace.microsoft.com/) . Дополнительные сведения см. [в статье проблемы с использованием виртуальных рабочих столов Windows. предоставление пула узлов Azure Marketplace](#issues-using-windows-virtual-desktop--provision-a-host-pool-azure-marketplace-offering).

## <a name="issues-using-windows-virtual-desktop--provision-a-host-pool-azure-marketplace-offering"></a>Проблемы с использованием виртуального рабочего стола Windows — предоставление пула узлов Azure Marketplace

Шаблон виртуального рабочего стола Windows — предоставление шаблона пула узлов доступен в Azure Marketplace.

### <a name="error-when-using-the-link-from-github-the-message-create-a-free-account-appears"></a>Ошибка. при использовании ссылки из GitHub появляется сообщение "создание бесплатной учетной записи"

![Снимок экрана для создания бесплатной учетной записи.](../media/be615904ace9832754f0669de28abd94.png)

**Причина 1:** В учетной записи, используемой для входа в Azure, нет активных подписок, или используемая учетная запись не имеет разрешений на просмотр подписок.

**Исправление 1:** Войдите в систему, используя учетную запись с правами участника (как минимум) на подписку, в которой будут развернуты виртуальные машины узла сеансов.

**Причина 2:** Используемая подписка является частью клиента поставщика служб (CSP) Microsoft Cloud.

**Исправление 2:** Перейдите в каталог GitHub для **создания и инициализации нового пула узлов виртуальных рабочих столов Windows** и выполните следующие инструкции:

1. Щелкните правой кнопкой мыши **развертывание в Azure** и выберите **Копировать адрес ссылки**.
2. Откройте **Блокнот** и вставьте ссылку.
3. Перед символом # вставьте имя клиента конечного клиента CSP.
4. Откройте новую ссылку в браузере, после чего портал Azure загрузит шаблон.

    ```Example
    Example: https://portal.azure.com/<CSP end customer tenant name>
    #create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%
    2FRDS-Templates%2Fmaster%2Fwvd-templates%2FCreate%20and%20provision%20WVD%20host%20pool%2FmainTemplate.json
    ```

### <a name="error-you-receive-template-deployment-is-not-valid-error"></a>Ошибка: появляется сообщение об ошибке "развертывание шаблона недопустимо"

![Снимок экрана: "развертывание шаблона... является недопустимым "ошибка](../media/troubleshooting-marketplace-validation-error-generic.png)

Перед выполнением конкретных действий необходимо проверить журнал действий, чтобы просмотреть подробные сведения об ошибке при проверке развертывания.

Чтобы просмотреть ошибку в журнале действий, выполните следующие действия.

1. Выйдите из текущего предложения по развертыванию Azure Marketplace.
2. В верхней строке поиска найдите и выберите **Журнал действий**.
3. Найдите действие **Проверка развертывания** с состоянием **Ошибка** и выберите действие.
   ![Снимок экрана: отдельное действие * * проверить развертывание * * с состоянием * * ошибка * *](../media/troubleshooting-marketplace-validation-error-activity-summary.png)

4. Выберите JSON, а затем прокрутите вниз до нижней части экрана, пока не увидите поле "statusMessage".
   ![Снимок экрана невыполненной активности с красной рамкой вокруг свойства statusMessage текста JSON.](../media/troubleshooting-marketplace-validation-error-json-boxed.png)

Если шаблон операции выходит за пределы квоты, можно выполнить одно из следующих действий, чтобы устранить эту проблему.

 - Запустите Azure Marketplace с параметрами, которые вы использовали впервые, но на этот раз используйте меньше виртуальных машин и ядер виртуальных машин.
 - Откройте ссылку, которая отображается в поле **statusMessage** в браузере, чтобы отправить запрос на увеличение квоты для подписки Azure для указанного номера SKU виртуальной машины.

## <a name="azure-resource-manager-template-and-powershell-desired-state-configuration-dsc-errors"></a>Ошибки Azure Resource Manager шаблона и настройки требуемого состояния PowerShell (DSC)

Выполните эти инструкции, чтобы устранить неудачные развертывания шаблонов Azure Resource Manager и PowerShell DSC.

1. Просмотрите ошибки в развертывании, используя [Просмотр операций развертывания с Azure Resource Manager](../../azure-resource-manager/resource-manager-deployment-operations.md).
2. Если в развертывании нет ошибок, просмотрите ошибки в журнале действий, используя [Просмотр журналов действий для аудита действий с ресурсами](../../azure-resource-manager/resource-group-audit.md).
3. После определения ошибки используйте сообщение об ошибке и ресурсы в статье [Устранение распространенных ошибок развертывания Azure с Azure Resource Manager](../../azure-resource-manager/resource-manager-common-deployment-errors.md) для решения проблемы.
4. Удалите все ресурсы, созданные во время предыдущего развертывания, и повторите попытку развертывания шаблона.

### <a name="error-your-deployment-failedhostnamejoindomain"></a>Ошибка: не удалось выполнить развертывание...\<. имя узла>/жоиндомаин

![Снимок экрана сбоя развертывания.](../media/e72df4d5c05d390620e07f0d7328d50f.png)

Пример необработанной ошибки:

```Error
 {"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details.
 Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\": \"Failed\",\r\n \"error\":
 {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n \"message\": \"The resource operation completed with terminal provisioning state 'Failed'.
 \",\r\n \"details\": [\r\n {\r\n \"code\": \"VMExtensionProvisioningError\",\r\n \"message\": \"VM has reported a failure when processing
 extension 'joindomain'. Error message: \\\"Exception(s) occurred while joining Domain 'diamondsg.onmicrosoft.com'\\\".\"\r\n }\r\n ]\r\n }\r\n}"}]}
```

**Причина 1:** Указаны неправильные учетные данные для присоединения виртуальных машин к домену.

**Исправление 1:** См. ошибку "неверные учетные данные" для виртуальных машин не присоединены к домену в [конфигурации виртуальной машины узла сеансов](troubleshoot-vm-configuration-2019.md).

**Причина 2:** Доменное имя не разрешается.

**Исправление 2:** См. [ошибку: доменное имя не разрешается](troubleshoot-vm-configuration-2019.md#error-domain-name-doesnt-resolve) в [конфигурации виртуальной машины узла сеансов](troubleshoot-vm-configuration-2019.md).

**Причина 3:** Для конфигурации DNS виртуальной сети задано значение **по умолчанию**.

Чтобы устранить эту проблему, выполните следующие действия.

1. Откройте портал Azure и перейдите на вкладку **виртуальные сети** .
2. Найдите виртуальную сеть, а затем выберите **DNS-серверы**.
3. Меню DNS-серверы должно отображаться в правой части экрана. В этом меню выберите **Пользовательский**.
4. Убедитесь, что DNS-серверы, указанные в поле Пользовательский, соответствуют вашему контроллеру домена или домену Active Directory. Если DNS-сервер не отображается, его можно добавить, введя его значение в поле **добавить DNS-сервер** .

### <a name="error-your-deployment-failedunauthorized"></a>Ошибка: сбой развертывания. ..\Унаусоризед

```Error
{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"Unauthorized","message":"{\r\n \"Code\": \"Unauthorized\",\r\n \"Message\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\",\r\n \"Target\": null,\r\n \"Details\": [\r\n {\r\n \"Message\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\"\r\n },\r\n {\r\n \"Code\": \"Unauthorized\"\r\n },\r\n {\r\n \"ErrorEntity\": {\r\n \"ExtendedCode\": \"52020\",\r\n \"MessageTemplate\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\",\r\n \"Parameters\": [\r\n \"default\"\r\n ],\r\n \"Code\": \"Unauthorized\",\r\n \"Message\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\"\r\n }\r\n }\r\n ],\r\n \"Innererror\": null\r\n}"}]}
```

**Причина:** Используемая Подписка — это тип, который не может получить доступ к необходимым функциям в регионе, в котором клиент пытается выполнить развертывание. Например, подписки MSDN, Free или образовательных учреждений могут показывать эту ошибку.

**Исправление:** Измените тип подписки или регион на другой, который может получить доступ к необходимым функциям.

### <a name="error-vmextensionprovisioningerror"></a>Ошибка: Вмекстенсионпровисионинжеррор

![Снимок экрана развертывания: не удалось выполнить состояние подготовки терминала.](../media/7aaf15615309c18a984673be73ac969a.png)

**Причина 1:** Временная ошибка в среде виртуальных рабочих столов Windows.

**Причина 2:** Временная ошибка с подключением.

**Исправление:** Подтвердите работоспособность среды виртуальных рабочих столов Windows, выполнив вход с помощью PowerShell. Завершите регистрацию виртуальной машины вручную в окне [Создание пула узлов с помощью PowerShell](create-host-pools-powershell-2019.md).

### <a name="error-the-admin-username-specified-isnt-allowed"></a>Ошибка: указанное имя администратора не разрешено

![Снимок экрана развертывания, в котором указанное административное сообщение не разрешено.](../media/f2b3d3700e9517463ef88fa41875bac9.png)

Пример необработанной ошибки:

```Error
 { "id": "/subscriptions/EXAMPLE/resourceGroups/demoHostDesktop/providers/Microsoft.
  Resources/deployments/vmCreation-linkedTemplate/operations/EXAMPLE", "operationId": "EXAMPLE", "properties": { "provisioningOperation":
 "Create", "provisioningState": "Failed", "timestamp": "2019-01-29T20:53:18.904917Z", "duration": "PT3.0574505S", "trackingId":
 "1f460af8-34dd-4c03-9359-9ab249a1a005", "statusCode": "BadRequest", "statusMessage": { "error": { "code": "InvalidParameter", "message":
 "The Admin Username specified is not allowed.", "target": "adminUsername" } }, "targetResource": { "id": "/subscriptions/EXAMPLE
 /resourceGroups/demoHostDesktop/providers/Microsoft.Compute/virtualMachines/demo", "resourceType": "Microsoft.Compute/virtualMachines", "resourceName": "demo" } }}
```

**Причина:** Предоставленный пароль содержит запрещенные подстроки (администратор, администратор, корень).

**Исправление:** Обновите имя пользователя или используйте других пользователей.

### <a name="error-vm-has-reported-a-failure-when-processing-extension"></a>Ошибка: виртуальная машина сообщила об ошибке при обработке расширения

![Снимок экрана: операция с ресурсом завершена с состоянием подготовки терминала в развертывании.](../media/49c4a1836a55d91cd65125cf227f411f.png)

Пример необработанной ошибки:

```Error
{ "id": "/subscriptions/EXAMPLE/resourceGroups/demoHostD/providers/Microsoft.Resources/deployments/
 rds.wvd-provision-host-pool-20190129132410/operations/5A0757AC9E7205D2", "operationId": "5A0757AC9E7205D2", "properties":
 { "provisioningOperation": "Create", "provisioningState": "Failed", "timestamp": "2019-01-29T21:43:05.1416423Z",
 "duration": "PT7M56.8150879S", "trackingId": "43c4f71f-557c-4abd-80c3-01f545375455", "statusCode": "Conflict",
 "statusMessage": { "status": "Failed", "error": { "code": "ResourceDeploymentFailure", "message":
 "The resource operation completed with terminal provisioning state 'Failed'.", "details": [ { "code":
 "VMExtensionProvisioningError", "message": "VM has reported a failure when processing extension 'dscextension'.
 Error message: \"DSC Configuration 'SessionHost' completed with error(s). Following are the first few:
 PowerShell DSC resource MSFT_ScriptResource failed to execute Set-TargetResource functionality with error message:
 One or more errors occurred. The SendConfigurationApply function did not succeed.\"." } ] } }, "targetResource":
 { "id": "/subscriptions/EXAMPLE/resourceGroups/demoHostD/providers/Microsoft.
 Compute/virtualMachines/desktop-1/extensions/dscextension",
 "resourceType": "Microsoft.Compute/virtualMachines/extensions", "resourceName": "desktop-1/dscextension" } }}
```

**Причина:** Модулю PowerShell DSC не удалось получить доступ к администратору на виртуальной машине.

**Исправление:** Убедитесь, что имя пользователя и пароль имеют административный доступ к виртуальной машине, и снова запустите шаблон Azure Resource Manager.

### <a name="error-deploymentfailed--powershell-dsc-configuration-firstsessionhost-completed-with-errors"></a>Ошибка: DeploymentFailed — конфигурация PowerShell DSC "Фирстсессионхост" завершена с ошибками

![Снимок экрана развертывания с конфигурацией PowerShell DSC "Фирстсессионхост" завершился с ошибками.](../media/64870370bcbe1286906f34cf0a8646ab.png)

Пример необработанной ошибки:

```Error
{
    "code": "DeploymentFailed",
   "message": "At least one resource deployment operation failed. Please list
 deployment operations for details. 4 Please see https://aka.ms/arm-debug for usage details.",
 "details": [
         { "code": "Conflict",  
         "message": "{\r\n \"status\": \"Failed\",\r\n \"error\": {\r\n \"code\":
         \"ResourceDeploymentFailure\",\r\n \"message\": \"The resource
         operation completed with terminal provisioning state 'Failed'.\",\r\n
         \"details\": [\r\n {\r\n \"code\":
        \"VMExtensionProvisioningError\",\r\n \"message\": \"VM has
              reported a failure when processing extension 'dscextension'.
              Error message: \\\"DSC Configuration 'FirstSessionHost'
              completed with error(s). Following are the first few:
              PowerShell DSC resource MSFT ScriptResource failed to
              execute Set-TargetResource functionality with error message:
              One or more errors occurred. The SendConfigurationApply
              function did not succeed.\\\".\"\r\n }\r\n ]\r\n }\r\n}"  }

```

**Причина:** Модулю PowerShell DSC не удалось получить доступ к администратору на виртуальной машине.

**Исправление:** Убедитесь, что указанные имя пользователя и пароль имеют административный доступ к виртуальной машине, и снова запустите шаблон Azure Resource Manager.

### <a name="error-deploymentfailed--invalidresourcereference"></a>Ошибка: DeploymentFailed – Инвалидресаурцереференце

Пример необработанной ошибки:

```Error
{"code":"DeploymentFailed","message":"At least one resource deployment operation
failed. Please list deployment operations for details. Please see https://aka.ms/arm-
debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\":
\"Failed\",\r\n \"error\": {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n
\"message\": \"The resource operation completed with terminal provisioning state
'Failed'.\",\r\n \"details\": [\r\n {\r\n \"code\": \"DeploymentFailed\",\r\n
\"message\": \"At least one resource deployment operation failed. Please list
deployment operations for details. Please see https://aka.ms/arm-debug for usage
details.\",\r\n \"details\": [\r\n {\r\n \"code\": \"BadRequest\",\r\n \"message\":
\"{\\r\\n \\\"error\\\": {\\r\\n \\\"code\\\": \\\"InvalidResourceReference\\\",\\r\\n
\\\"message\\\": \\\"Resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/virtualNetworks/wvd-vnet/subnets/default
referenced by resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/networkInterfaces/erd. Please make sure that
the referenced resource exists, and that both resources are in the same
region.\\\",\\r\\n\\\"details\\\": []\\r\\n }\\r\\n}\"\r\n }\r\n ]\r\n }\r\n ]\r\n }\r\n}"}]}
```

**Причина:** Часть имени группы ресурсов используется для определенных ресурсов, создаваемых шаблоном. Из-за имени, соответствующего существующим ресурсам, шаблон может выбрать существующий ресурс из другой группы.

**Исправление:** При запуске шаблона Azure Resource Manager для развертывания виртуальных машин узла сеансов сделайте первые два символа уникальными для имени группы ресурсов подписки.

### <a name="error-deploymentfailed--invalidresourcereference"></a>Ошибка: DeploymentFailed – Инвалидресаурцереференце

Пример необработанной ошибки:

```Error
{"code":"DeploymentFailed","message":"At least one resource deployment operation
failed. Please list deployment operations for details. Please see https://aka.ms/arm-
debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\":
\"Failed\",\r\n \"error\": {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n
\"message\": \"The resource operation completed with terminal provisioning state
'Failed'.\",\r\n \"details\": [\r\n {\r\n \"code\": \"DeploymentFailed\",\r\n
\"message\": \"At least one resource deployment operation failed. Please list
deployment operations for details. Please see https://aka.ms/arm-debug for usage
details.\",\r\n \"details\": [\r\n {\r\n \"code\": \"BadRequest\",\r\n \"message\":
\"{\\r\\n \\\"error\\\": {\\r\\n \\\"code\\\": \\\"InvalidResourceReference\\\",\\r\\n
\\\"message\\\": \\\"Resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/virtualNetworks/wvd-vnet/subnets/default
referenced by resource /subscriptions/EXAMPLE/resourceGroups/DEMO/providers/Microsoft.Network/networkInterfaces
/EXAMPLE was not found. Please make sure that the referenced resource exists, and that both
resources are in the same region.\\\",\\r\\n \\\"details\\\": []\\r\\n }\\r\\n}\"\r\n
}\r\n ]\r\n }\r\n ]\r\n }\r\n\
```

**Причина:** Эта ошибка вызвана тем, что сетевая карта, созданная с помощью шаблона Azure Resource Manager, имеет то же имя, что и другая сетевая карта, уже подключенная к ВИРТУАЛЬНОЙ

**Исправление:** Используйте другой префикс узла.

### <a name="error-deploymentfailed--error-downloading"></a>Ошибка: DeploymentFailed — ошибка при скачивании

Пример необработанной ошибки:

```Error
\\\"The DSC Extension failed to execute: Error downloading
https://catalogartifact.azureedge.net/publicartifacts/rds.wvd-provision-host-pool-
2dec7a4d-006c-4cc0-965a-02bbe438d6ff-prod
/Artifacts/DSC/Configuration.zip after 29 attempts: The remote name could not be
resolved: 'catalogartifact.azureedge.net'.\\nMore information about the failure can
be found in the logs located under
'C:\\\\WindowsAzure\\\\Logs\\\\Plugins\\\\Microsoft.Powershell.DSC\\\\2.77.0.0' on
the VM.\\\"
```

**Причина:** Эта ошибка возникает из-за статического маршрута, правила брандмауэра или NSG блокировки загрузки ZIP-файла, привязанного к шаблону Azure Resource Manager.

**Исправление:** Удалите заблокированный статический маршрут, правило брандмауэра или NSG. При необходимости откройте JSON-файл шаблона Azure Resource Manager в текстовом редакторе, перейдите по ссылке к ZIP-файлу и скачайте ресурс в разрешенное расположение.

### <a name="error-the-user-isnt-authorized-to-query-the-management-service"></a>Ошибка: пользователь не имеет разрешения на запрос к службе управления

Пример необработанной ошибки:

```Error
"response": { "content": { "startTime": "2019-04-01T17:45:33.3454563+00:00", "endTime": "2019-04-01T17:48:52.4392099+00:00",
"status": "Failed", "error": { "code": "VMExtensionProvisioningError", "message": "VM has reported a failure when processing
extension 'dscextension'. Error message: \"DSC Configuration 'FirstSessionHost' completed with error(s).
Following are the first few: PowerShell DSC resource MSFT_ScriptResource failed to execute Set-TargetResource
 functionality with error message: User is not authorized to query the management service.
\nActivityId: 1b4f2b37-59e9-411e-9d95-4f7ccd481233\nPowershell commands to diagnose the failure:
\nGet-RdsDiagnosticActivities -ActivityId 1b4f2b37-59e9-411e-9d95-4f7ccd481233\n
The SendConfigurationApply function did not succeed.\"." }, "name": "2c3272ec-d25b-47e5-8d70-a7493e9dc473" } } }}
```

**Причина:** Указанный администратор клиента виртуальных рабочих столов Windows не имеет допустимого назначения роли.

**Исправление:** Пользователь, создавший клиента виртуальных рабочих столов Windows, должен войти в Windows Virtual Desktop PowerShell и назначить предпринятому пользователю назначение роли. Если вы используете параметры шаблона Azure Resource Manager GitHub, выполните следующие инструкции с помощью команд PowerShell:

```PowerShell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
New-RdsRoleAssignment -TenantName <Windows Virtual Desktop tenant name> -RoleDefinitionName "RDS Contributor" -SignInName <UPN>
```

### <a name="error-user-requires-azure-multi-factor-authentication-mfa"></a>Ошибка: пользователю требуется многофакторная идентификация Azure (MFA)

![Снимок экрана развертывания не удалось выполнить из-за отсутствия многофакторной проверки подлинности (MFA)](../media/MFARequiredError.png)

Пример необработанной ошибки:

```Error
"message": "{\r\n  \"status\": \"Failed\",\r\n  \"error\": {\r\n    \"code\": \"ResourceDeploymentFailure\",\r\n    \"message\": \"The resource operation completed with terminal provisioning state 'Failed'.\",\r\n    \"details\": [\r\n      {\r\n        \"code\": \"VMExtensionProvisioningError\",\r\n        \"message\": \"VM has reported a failure when processing extension 'dscextension'. Error message: \\\"DSC Configuration 'FirstSessionHost' completed with error(s). Following are the first few: PowerShell DSC resource MSFT_ScriptResource  failed to execute Set-TargetResource functionality with error message: One or more errors occurred.  The SendConfigurationApply function did not succeed.\\\".\"\r\n      }\r\n    ]\r\n  }\r\n}"
```

**Причина:** Для входа указанному администратору клиента виртуальных рабочих столов Windows требуется служба многофакторной идентификации Azure (MFA).

**Исправление:** Создайте субъект-службу и назначьте ему роль для вашего клиента виртуальных рабочих столов Windows, выполнив действия, описанные в разделе [учебник. создание субъектов-служб и назначение ролей с помощью PowerShell](create-service-principal-role-powershell.md). Убедившись, что вы можете войти в виртуальный рабочий стол Windows с помощью субъекта-службы, перезапустите предложение Azure Marketplace или шаблон Azure Resource Manager GitHub в зависимости от того, какой метод вы используете. Следуйте приведенным ниже инструкциям, чтобы ввести правильные параметры для метода.

Если вы используете предложение Azure Marketplace, укажите значения следующих параметров для правильной проверки подлинности в виртуальном рабочем столе Windows:

- Владелец RDS для клиента виртуальных рабочих столов Windows: субъект-служба
- Идентификатор приложения: идентификатор нового созданного субъекта-службы.
- Пароль/подтверждение пароля. секрет пароля, созданный для субъекта-службы.
- Идентификатор клиента Azure AD: идентификатор клиента Azure AD созданного субъекта-службы.

Если вы используете шаблон Azure Resource Manager GitHub, укажите значения следующих параметров для правильной проверки подлинности в виртуальном рабочем столе Windows:

- Имя участника-пользователя (UPN) или идентификатор приложения администратора клиента. идентификатор приложения для нового созданного субъекта-службы
- Пароль администратора клиента: секрет пароля, созданный для субъекта-службы.
- ИссервицепринЦипал: **true**
- Aadtenantid и: идентификатор клиента Azure AD созданного субъекта-службы.

## <a name="next-steps"></a>Дальнейшие действия

- Общие сведения об устранении неполадок с виртуальным рабочим столом Windows и сведениями о эскалации см. в разделе [Обзор устранения неполадок, обратная связь и поддержка](troubleshoot-set-up-overview-2019.md).
- Сведения об устранении неполадок при настройке виртуальной машины в виртуальном рабочем столе Windows см. в разделе [Конфигурация виртуальной машины узла сеанса](troubleshoot-vm-configuration-2019.md).
- Сведения об устранении неполадок подключения клиентов к виртуальным рабочим столам Windows см. в статье [подключения к службе виртуальных рабочих столов Windows](troubleshoot-service-connection-2019.md).
- Сведения об устранении неполадок, связанных с удаленный рабочий стол клиентами, см. [в разделе Устранение неполадок клиента удаленный рабочий стол](../troubleshoot-client.md)
- Сведения об устранении неполадок при использовании PowerShell с виртуальным рабочим столом Windows см. в статье [Windows Virtual Desktop PowerShell](troubleshoot-powershell-2019.md).
- Дополнительные сведения о службе см. в разделе [Среда виртуальных рабочих столов Windows](environment-setup-2019.md).
- Руководство по устранению неполадок см. в разделе [учебник. Устранение неполадок диспетчер ресурсов развертываний шаблонов](../../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
- Сведения о действиях аудита см. в статье [Операции аудита с помощью Resource Manager](../../azure-resource-manager/management/view-activity-logs.md).
- Дополнительные сведения об определении ошибок во время развертывания см. в статье [Просмотр операций развертывания с помощью портала Azure](../../azure-resource-manager/templates/deployment-history.md).
