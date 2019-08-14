---
title: Настройка Azure Active Directory для проверки подлинности клиента Service Fabric | Документация Майкрософт
description: Сведения о настройке Azure Active Directory (Azure AD) для проверки подлинности клиентов для кластеров Service Fabric.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/28/2019
ms.author: atsenthi
ms.openlocfilehash: 6c195357c4a037534307571a53589b2ae861d88b
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/12/2019
ms.locfileid: "67486020"
---
# <a name="set-up-azure-active-directory-for-client-authentication"></a>Настройка Azure Active Directory для проверки подлинности клиента

Для кластеров, работающих в Azure, рекомендуется использовать Azure Active Directory (Azure AD), чтобы обеспечить безопасность доступа к конечным точкам управления.  В этой статье описывается, как настроить Azure AD для проверки подлинности клиентов для кластера Service Fabric и что нужно сделать перед [созданием кластера](service-fabric-cluster-creation-via-arm.md).  Azure AD позволяет организациям (известным как клиенты) управлять доступом пользователей к приложениям. Приложения можно разделить на две группы: те, в которых есть пользовательский веб-интерфейс входа в систему, и те, в которых вход выполняется с помощью собственного клиентского приложения. 

Кластеры Service Fabric предлагают несколько точек входа для управления функциями кластеров, включая веб-интерфейс [Service Fabric Explorer][service-fabric-visualizing-your-cluster] и [Visual Studio][service-fabric-manage-application-in-visual-studio]. В итоге вы создаете два приложения Azure AD для управления доступом к кластеру: одно веб-приложение и одно собственное приложение.  Создав приложения, сопоставьте пользователей с ролями, разрешающими доступ только для чтения и администрирования.

> [!NOTE]
> Перед созданием кластера необходимо выполнить следующие действия. Поскольку в сценариях предварительно заданы имена кластеров и конечные точки, эти имена нужно выбрать заблаговременно, при этом они должны отличаться от созданных ранее имен.

## <a name="prerequisites"></a>предварительные требования
В этой статье предполагается, что клиент уже создан. Если это не так, обратитесь к статье [Краткое руководство. Настройка среды разработки][active-directory-howto-tenant].

Чтобы упростить некоторые шаги по настройке Azure AD с кластером Service Fabric, мы создали набор сценариев Windows PowerShell.

1. [Клонировать репозиторий](https://github.com/Azure-Samples/service-fabric-aad-helpers) на компьютер.
2. Убедитесь, что для установленных скриптов установлены [все необходимые компоненты](https://github.com/Azure-Samples/service-fabric-aad-helpers#getting-started) .

## <a name="create-azure-ad-applications-and-assign-users-to-roles"></a>Создание приложения Azure AD и назначение ролей пользователям

Мы будем использовать эти сценарии для создания двух приложений Azure AD для управления доступом к кластеру: одно веб-приложение и одно собственное приложение. Создав приложения для представления кластера, вы создадите пользователей для [ролей, поддерживаемых Service Fabric](service-fabric-cluster-security-roles.md): только для чтения и для администратора.

Запустите `SetupApplications.ps1` и укажите идентификатор клиента, имя кластера и URL-адрес ответа веб-приложения в качестве параметров.  Укажите также имена пользователей и пароли для пользователей. Пример:

```powershell
$Configobj = .\SetupApplications.ps1 -TenantId '0e3d2646-78b3-4711-b8be-74a381d9890c' -ClusterName 'mysftestcluster' -WebApplicationReplyUrl 'https://mysftestcluster.eastus.cloudapp.azure.com:19080/Explorer/index.html' -AddResourceAccess
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestUser' -Password 'P@ssword!123'
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestAdmin' -Password 'P@ssword!123' -IsAdmin
```

> [!NOTE]
> Для национальных облаков (например, Azure для государственных организаций, Azure для Китая и Azure для Германии) также необходимо указать параметр `-Location`.

Чтобы узнать *TenantId*, можно выполнить команду PowerShell `Get-AzureSubscription`. После выполнения этой команды для каждой подписки отображается TenantId.

Значение *ClusterName* используется в качестве префикса для приложений Azure AD, создаваемых сценарием. Точное совпадение с именем реального кластера не требуется. Оно предназначено только для упрощения сопоставления артефактов Azure AD с кластером Service Fabric, с которым они используются.

*WebApplicationReplyUrl* является конечной точкой по умолчанию, которую Azure AD возвращает пользователям после завершения ими входа. Эту конечную точку следует назначить конечной точкой Service Fabric Explorer для кластера, по умолчанию это:

https://&lt;cluster_domain&gt;:19080/Explorer

Вам будет предложено войти в учетную запись с правами администратора для клиента Azure AD. После входа сценарий начнет создавать веб-приложение и собственное приложение для представления кластера Service Fabric. Если посмотреть на список приложений клиента на [портале Azure][azure-portal], вы увидите две новые записи:

   * *имя_кластера*\_Кластер
   * *имя_кластера*\_Клиент

Этот сценарий выводит код JSON для шаблона Azure Resource Manager, который вы будете использовать в следующем разделе для [создания кластера](service-fabric-cluster-creation-create-template.md#add-azure-ad-configuration-to-use-azure-ad-for-client-access). Рекомендуем пока не закрывать окно PowerShell.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="troubleshooting-help-in-setting-up-azure-active-directory"></a>Устранение неполадок при настройке Azure Active Directory
Настройка и использование Azure AD может оказаться сложной задачей, поэтому воспользуйтесь следующими советами для отладки проблемы.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer предлагает выбрать сертификат
#### <a name="problem"></a>Проблема
После успешного входа в Azure AD в Service Fabric Explorer браузер отображает домашнюю страницу, но на экране отображается предложение выбрать сертификат.

![Диалоговое окно сертификата SFX][sfx-select-certificate-dialog]

#### <a name="reason"></a>Причина
Пользователю не назначена роль в приложении кластера Azure AD. Таким образом, аутентификация Azure AD в кластере Service Fabric завершается ошибкой. Service Fabric Explorer воспользуется проверкой подлинности на основе сертификата.

#### <a name="solution"></a>Решение
Следуйте инструкциям по настройке Azure AD и назначению ролей пользователей. Кроме того, рекомендуется включить параметр "Для доступа к приложению требуется назначение пользователей", как делает сценарий `SetupApplications.ps1`.

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>Подключение с помощью PowerShell завершается с ошибкой: "Указаны недопустимые учетные данные"
#### <a name="problem"></a>Проблема
После успешного входа в Azure AD при использовании PowerShell для подключения к кластеру с помощью режима безопасности AzureActiveDirectory подключение завершается с ошибкой: "Указанные учетные данные недействительны".

#### <a name="solution"></a>Решение
Решение этой проблемы такое же, как и в предыдущем случае.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer при входе возвращает ошибку "AADSTS50011"
#### <a name="problem"></a>Проблема
При попытке входа в Azure AD в Service Fabric Explorer страница возвращает ошибку: "AADSTS50011: адрес ответа &lt;URL-адрес&gt; не совпадает с настроенными адресами ответа приложения: &lt;GUID&gt;".

![Несовпадение адреса ответа в Service Fabric Explorer][sfx-reply-address-not-match]

#### <a name="reason"></a>Причина
Приложение кластера (веб-приложение), представляющее Service Fabric Explorer, пытается выполнить аутентификацию в Azure AD и в составе запроса предоставляет URL-адрес ответа для перенаправления. URL-адрес не указан в списке **URL-АДРЕС ОТВЕТА** приложения Azure AD.

#### <a name="solution"></a>Решение
Щелкните "Регистрация приложений" на странице AAD, выберите приложение кластера и нажмите кнопку **URL-адреса ответа**. На странице "URL-адреса ответа" добавьте URL-адрес Service Fabric Explorer в список или замените один из элементов в списке. После завершения сохраните изменения.

![URL-адрес ответа веб-приложения][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>Подключение к кластеру с помощью аутентификации Azure AD с использованием PowerShell
Чтобы подключить кластер Service Fabric, воспользуйтесь следующим примером команды PowerShell:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

Дополнительные сведения о командлете Connect-ServiceFabricCluster см. в [этой статье](https://docs.microsoft.com/powershell/module/servicefabric/connect-servicefabriccluster).

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>Можно ли повторно использовать один и тот же клиент Azure AD в нескольких кластерах?
Да. Не забудьте добавить URL-адрес Service Fabric Explorer в (веб-)приложение кластера. В противном случае — Service Fabric Explorer не работает.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Почему мне все еще нужен сертификат сервера при включенном Azure AD?
FabricClient и FabricGateway выполняют взаимную аутентификацию. Во время аутентификации Azure AD интеграция Azure AD предоставляет удостоверение клиента серверу и для аутентификации сервера используется сертификат сервера. Дополнительные сведения о Service Fabric сертификатах см. в разделе [сертификаты X. 509 и Service Fabric][x509-certificates-and-service-fabric].

## <a name="next-steps"></a>Следующие шаги
После настройки приложений Azure Active Directory и ролей для пользователей [настройте и разверните кластер](service-fabric-cluster-creation-via-arm.md).


<!-- Links -->
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[azure-portal]: https://portal.azure.com/
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]:../active-directory/develop/quickstart-create-new-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-setup-aad/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-setup-aad/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-setup-aad/web-application-reply-url.png
