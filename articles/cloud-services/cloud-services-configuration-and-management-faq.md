---
title: Вопросы, связанные с настройкой и управлением
titleSuffix: Azure Cloud Services
description: В этой статье приведены часто задаваемые вопросы по конфигурации и управлению для облачных служб Microsoft Azure.
services: cloud-services
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/23/2018
ms.author: genli
ms.openlocfilehash: 50dd6e2387abbf5b24adcee708dd2ff387e33261
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76845377"
---
# <a name="configuration-and-management-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Проблемы конфигурации и управления для облачных служб Azure. Вопросы и ответы (FAQ)

В этой статье приведены часто задаваемые вопросы по конфигурации и управлению для [облачных служб Microsoft Azure](https://azure.microsoft.com/services/cloud-services). Сведения о размерах приводятся в статье [Размеры для облачных служб](cloud-services-sizes-specs.md) .

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

**Сертификаты**

- [Почему у моего SSL-сертификата облачной службы неполная цепочка сертификатов?](#why-is-the-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete)
- [Для чего нужен сертификат шифрования Microsoft Azure Tools для расширений?](#what-is-the-purpose-of-the-windows-azure-tools-encryption-certificate-for-extensions)
- [Как создать запрос на подпись сертификата (CSR) без обращения к экземпляру с использованием RDP?](#how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-to-the-instance)
- [Срок действия сертификата управления облачными службами истекает. Как продлить его?](#my-cloud-service-management-certificate-is-expiring-how-to-renew-it)
- [Как автоматизировать установку основного SSL- сертификата (PFX) и промежуточного сертификата (P7B)?](#how-to-automate-the-installation-of-main-ssl-certificatepfx-and-intermediate-certificatep7b)
- [Каково назначение сертификата «Управление службами Microsoft Azure для MachineKey»?](#what-is-the-purpose-of-the-microsoft-azure-service-management-for-machinekey-certificate)

**Мониторинг и ведение журнала**

- [Какие будущие возможности облачной службы на портале Azure помогут наблюдать за приложениями и управлять ими?](#what-are-the-upcoming-cloud-service-capabilities-in-the-azure-portal-which-can-help-manage-and-monitor-applications)
- [Почему IIS останавливает запись в каталог журналов?](#why-does-iis-stop-writing-to-the-log-directory)
- [Как включить ведение журнала WAD для облачных служб?](#how-do-i-enable-wad-logging-for-cloud-services)

**Конфигурация сети**

- [Как задать время ожидания простоя для подсистемы балансировки нагрузки Azure?](#how-do-i-set-the-idle-timeout-for-azure-load-balancer)
- [Как привязать статический IP-адрес к облачной службе?](#how-do-i-associate-a-static-ip-address-to-my-cloud-service)
- [Какие функции и возможности предоставляют базовые IPS/IDS и DDOS Azure?](#what-are-the-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides)
- [Как включить HTTP/2 на виртуальной машине облачных служб?](#how-to-enable-http2-on-cloud-services-vm)

**Разрешения**

- [Могут ли инженеры Майкрософт без моего разрешения использовать удаленный рабочий стол для доступа к экземплярам облачной службы?](#can-microsoft-internal-engineers-remote-desktop-to-cloud-service-instances-without-permission)
- [Не удается выполнить удаленный рабочий стол для виртуальной машины облачной службы с помощью RDP-файла. Я получаю следующую ошибку: произошла ошибка проверки подлинности (код: 0x80004005)](#i-cannot-remote-desktop-to-cloud-service-vm--by-using-the-rdp-file-i-get-following-error-an-authentication-error-has-occurred-code-0x80004005)

**Масштабирование**

- [При масштабировании я не могу создать более X экземпляров](#i-cannot-scale-beyond-x-instances)
- [Как мне настроить автомасштабирование по метрикам памяти?](#how-can-i-configure-auto-scale-based-on-memory-metrics)

**Общие вопросы**

- [Как добавить на веб-сайт запрет на сканирование?](#how-do-i-add-nosniff-to-my-website)
- [Как настроить IIS для веб-роли?](#how-do-i-customize-iis-for-a-web-role)
- [Что такое квота для облачной службы?](#what-is-the-quota-limit-for-my-cloud-service)
- [Почему на диске виртуальной машины облачной службы отображается очень мало свободного места?](#why-does-the-drive-on-my-cloud-service-vm-show-very-little-free-disk-space)
- [Как автоматизировать добавление антивредоносного расширения для облачных служб?](#how-can-i-add-an-antimalware-extension-for-my-cloud-services-in-an-automated-way)
- [Как включить указание имени сервера для облачных служб?](#how-to-enable-server-name-indication-sni-for-cloud-services)
- [Как мне добавить теги к облачной службе Azure?](#how-can-i-add-tags-to-my-azure-cloud-service)
- [В портал Azure не отображается версия пакета SDK для моей облачной службы. Как это можно сделать?](#the-azure-portal-doesnt-display-the-sdk-version-of-my-cloud-service-how-can-i-get-that)
- [Я хочу завершить работу облачной службы в течение нескольких месяцев. Как уменьшить стоимость выставления счетов для облачной службы без потери IP-адреса?](#i-want-to-shut-down-the-cloud-service-for-several-months-how-to-reduce-the-billing-cost-of-cloud-service-without-losing-the-ip-address)


## <a name="certificates"></a>Сертификаты

### <a name="why-is-the-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete"></a>Почему у моего SSL-сертификата облачной службы неполная цепочка сертификатов?
    
Корпорация Майкрософт рекомендует пользователям установить полную цепочку сертификатов (конечный сертификат, промежуточные сертификаты и корневой сертификат) вместо только конечного сертификата. При установке только конечного сертификата вы зависите от того, как Windows построит цепочку сертификатов, проходя список доверия сертификатов. Если при попытке Windows проверить сертификат происходит перебой в сети или проблема DNS, сертификат может посчитаться недействительным. Установив полную цепочку сертификатов, можно избежать этой проблемы. В публикации в блоге [Как установить связанный в цепочку SSL-сертификат](https://blogs.msdn.microsoft.com/azuredevsupport/2010/02/24/how-to-install-a-chained-ssl-certificate/) показано, как это сделать.

### <a name="what-is-the-purpose-of-the-windows-azure-tools-encryption-certificate-for-extensions"></a>В чем заключается цель сертификата шифрования Microsoft Azure Tools для расширений?

Эти сертификаты автоматически создаются каждый раз при добавлении расширения в облачную службу. Чаще всего это расширение WAD или RDP, но может быть и другое расширение, например антивредоносное ПО или сборщик журналируемых данных. Эти сертификаты используются только для шифрования и расшифровки закрытой конфигурации расширения. Дата окончания срока действия никогда не проверяется, поэтому истечение срока действия сертификата не имеет значения. 

Эти сертификаты можно игнорировать. Если вы хотите очистить сертификаты, можно попробовать удалить их все. Azure будет выдавать ошибку при попытке удалить сертификат, который используется.

### <a name="how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-to-the-instance"></a>Как создать запрос подписи сертификата (CSR) без обращения к экземпляру с использованием RDP?

Ответ см. в следующем руководстве:

[Получение сертификата для использования с веб-сайтами Microsoft Azure (WAWS)](https://azure.microsoft.com/blog/obtaining-a-certificate-for-use-with-windows-azure-web-sites-waws/)

CSR-файл имеет простой текстовый формат. Его не обязательно создавать на том компьютере, на котором будет использоваться этот сертификат. Несмотря на то, что этот документ написан для службы приложений, создание CSR является универсальным и применяется также для облачных служб.

### <a name="my-cloud-service-management-certificate-is-expiring-how-to-renew-it"></a>Истекает срок действия сертификата управления моей облачной службы. Как его обновить?

Вы можете обновить сертификаты управления с помощью следующих команд PowerShell:

    Add-AzureAccount
    Select-AzureSubscription -Current -SubscriptionName <your subscription name>
    Get-AzurePublishSettingsFile

**Get-AzurePublishSettingsFile** создает новый сертификат управления в разделе **Подписки** > **Сертификаты управления** на портале Azure. Новый сертификат получает имя в формате "[имя_подписки]-[текущая_дата]-credentials".

### <a name="how-to-automate-the-installation-of-main-ssl-certificatepfx-and-intermediate-certificatep7b"></a>Как автоматизировать установку основного SSL- сертификата (PFX) и промежуточного сертификата (P7B)?

Эту задачу можно автоматизировать с помощью скрипта запуска (пакетный файл, командный файл или скрипт PowerShell), зарегистрировав этот скрипт в файле определения службы. Сам скрипт запуска и сертификат для него (P7B-файл) нужно разместить в папке проекта в том же каталоге, что и скрипт запуска.

### <a name="what-is-the-purpose-of-the-microsoft-azure-service-management-for-machinekey-certificate"></a>Каково назначение сертификата «Управление службами Microsoft Azure для MachineKey»?

Этот сертификат используется для шифрования ключей компьютера на веб-ролях Azure. Чтобы узнать больше, ознакомьтесь с [этой рекомендацией](https://docs.microsoft.com/security-updates/securityadvisories/2018/4092731).

Дополнительные сведения см. в следующих статьях:
- [Как настроить и выполнить задачи запуска для облачной службы](https://docs.microsoft.com/azure/cloud-services/cloud-services-startup-tasks)
- [Стандартные задачи запуска в облачной службе](https://docs.microsoft.com/azure/cloud-services/cloud-services-startup-tasks-common)

## <a name="monitoring-and-logging"></a>Мониторинг и ведение журналов

### <a name="what-are-the-upcoming-cloud-service-capabilities-in-the-azure-portal-which-can-help-manage-and-monitor-applications"></a>Какие будущие возможности облачной службы на портале Azure помогут наблюдать за приложениями и управлять ими?

В ближайшее время появится возможность создавать новый сертификат для удаленного рабочего стола (RDP). Также можно выполнить этот скрипт:

```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My" -KeyLength 20 48 -KeySpec "KeyExchange"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```
В ближайшее время появится возможность выбора BLOB-объектов или локального расположения в качестве расположения для передачи csdef и cscfg. С помощью командлета [New-AzureDeployment](/powershell/module/servicemanagement/azure/new-azuredeployment?view=azuresmps-4.0.0) можно задать значение каждого расположения.

Возможность наблюдать за метриками на уровне экземпляра. Дополнительные возможности мониторинга см. в статье [Мониторинг облачных служб](cloud-services-how-to-monitor.md).

### <a name="why-does-iis-stop-writing-to-the-log-directory"></a>Почему IIS останавливает запись в каталог журналов?
Вы израсходовали квоту локального хранилища для записи в каталог журналов. Чтобы исправить это, можно выполнить одно из трех действий.
* Включите диагностику для IIS и задайте периодическое перемещение этой диагностики в хранилище BLOB-объектов.
* Вручную удалите файлы журнала из каталога ведения журнала.
* Увеличьте квоту для локальных ресурсов.

Дополнительные сведения см. в следующих документах:
* [Хранение и просмотр диагностических данных в службе хранилища Azure](cloud-services-dotnet-diagnostics-storage.md)
* [IIS Logs stop writing in Cloud Service](https://blogs.msdn.microsoft.com/cie/2013/12/21/iis-logs-stops-writing-in-cloud-service/) (Журналы IIS прекращают запись в облачной службе)

### <a name="how-do-i-enable-wad-logging-for-cloud-services"></a>Как включить ведение журнала WAD для облачных служб?
Вы можете включить ведение журнала Диагностики Azure для Windows (WAD) такими способами:
1. [С помощью Visual Studio](https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#turn-on-diagnostics-in-cloud-service-projects-before-you-deploy-them).
2. [Включить с помощью кода .NET](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics)
3. [С помощью PowerShell](https://docs.microsoft.com/azure/cloud-services/cloud-services-diagnostics-powershell).

Чтобы получить текущие параметры WAD облачной службы, можно использовать команду PS [Get-AzureServiceDiagnosticsExtensions](https://docs.microsoft.com/azure/cloud-services/cloud-services-diagnostics-powershell#get-current-diagnostics-extension-configuration) или просмотреть их на портале в колонке "Облачные службы --> Расширения".


## <a name="network-configuration"></a>Конфигурация сети

### <a name="how-do-i-set-the-idle-timeout-for-azure-load-balancer"></a>Как задать время ожидания простоя для Azure Load Balancer?
Время ожидания можно указать в файле определения службы (CSDEF-файле) следующим образом.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="mgVS2015Worker" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10100"   idleTimeoutInMinutes="30" />
    </Endpoints>
  </WorkerRole>
```
Дополнительные сведения см. в статье [Новое: настраиваемое время ожидания простоя для Azure Load Balancer](https://azure.microsoft.com/blog/new-configurable-idle-timeout-for-azure-load-balancer/).

### <a name="how-do-i-associate-a-static-ip-address-to-my-cloud-service"></a>Как привязать статический IP-адрес к облачной службе?
Чтобы настроить статический IP-адрес, необходимо создать зарезервированный IP-адрес. Этот зарезервированный IP-адрес можно привязать к новой облачной службе или к существующему развертыванию. Дополнительные сведения см. в следующих документах.
* [Как создать зарезервированный IP-адрес](/previous-versions/azure/virtual-network/virtual-networks-reserved-public-ip.md#manage-reserved-vips)
* [Резервирование IP-адреса существующей облачной службы](/previous-versions/azure/virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
* [Связывание зарезервированного IP-адреса с новой облачной службой](/previous-versions/azure/virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-new-cloud-service)
* [Связывание зарезервированного IP-адреса с работающей развернутой системой](/previous-versions/azure/virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-running-deployment)
* [Связывание зарезервированного IP-адреса с облачной службой с помощью файла конфигурации службы](/previous-versions/azure/virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file)

### <a name="what-are-the-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides"></a>Каковы функции и возможности, предоставляемые базовыми IPS/IDS и DDOS Azure?
Azure имеет IPS/IDS на физических серверах в центре обработки данных для защиты от угроз. Кроме того, клиенты могут разворачивать сторонние решения по обеспечению безопасности, такие как брандмауэры приложений, сетевые брандмауэры, антивредоносное ПО, системы обнаружения вторжений, системы предотвращения (IDS/IPS) и многое другое. Дополнительные сведения см. в разделе [Защита данных и ресурсов и соответствие глобальным стандартам безопасности](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity).

Корпорация Майкрософт осуществляет постоянный мониторинг серверов, сетей и приложений для обнаружения угроз. Комплексный подход к управлению угрозами в Azure использует системы обнаружения вторжений, предотвращения распределенных атак типа "отказ в обслуживании" (DDoS), тесты на проникновение, поведенческую аналитику, обнаружение аномалий и машинное обучение для постоянного усиления защиты и снижения рисков. Антивредоносное ПО Майкрософт для Azure защищает облачные службы и виртуальные машины Azure. Вы можете разворачивать сторонние решения по обеспечению безопасности, такие как брандмауэры приложений, сетевые брандмауэры, антивредоносное ПО, системы обнаружения вторжений, системы предотвращения (IDS/IPS) и многое другое.

### <a name="how-to-enable-http2-on-cloud-services-vm"></a>Как включить HTTP/2 на виртуальной машине облачных служб?

Windows 10 и Windows Server 2016 в стандартной конфигурации поддерживают HTTP/2 как для клиента, так и для сервера. Если ваш клиент (браузер) подключается к серверу IIS через TLS и согласовывает HTTP/2 через расширения TLS, вам не нужно вносить никаких изменений на стороне сервера. Это связано с тем, что через TLS по умолчанию отсылается заголовок h2-14, описывающий использование HTTP/2. Если же ваш клиент отправляет заголовок Upgrade для обновления до HTTP/2, на стороне сервера следует внести указанные ниже изменения, чтобы обновление сработало правильно и вы смогли использовать подключение HTTP/2. 

1. Запустите regedit.exe.
2. Перейдите к разделу реестра HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HTTP\Parameters.
3. Создайте новое значение DWORD с именем **DuoEnabled**.
4. Задайте для него значение 1.
5. Перезапустите сервер.
6. Перейдите к **веб-сайту по умолчанию** и в разделе **Привязки** создайте новую привязку TLS с только что созданным самозаверяющим сертификатом. 

Дополнительные сведения см. здесь:

- [HTTP/2 on IIS](https://blogs.iis.net/davidso/http2) (HTTP/2 в IIS)
- [Video: HTTP/2 in Windows 10: Browser, Apps and Web Server](https://channel9.msdn.com/Events/Build/2015/3-88) (Видео: HTTP/2 в Windows 10 для браузера, приложений и веб-сервера)
         

Эти шаги можно автоматизировать через задачу запуска, чтобы описанные выше изменения в системном реестре применялись при каждом создании нового экземпляра PaaS. Дополнительные сведения см. в статье [Как настроить и выполнить задачи запуска для облачной службы](cloud-services-startup-tasks.md).

 
Когда все будет готово, проверьте наличие HTTP/2 с помощью одного из следующих методов.

- Включите версию протокола для журналов IIS и изучите журналы IIS. HTTP/2 будет отображаться в журналах. 
- Включите средство разработчика F12 в Internet Explorer или Microsoft ребр и перейдите на вкладку Сеть, чтобы проверить протокол. 

Дополнительные сведения см. в статье [HTTP/2 on IIS](https://blogs.iis.net/davidso/http2) (HTTP/2 в IIS).

## <a name="permissions"></a>Разрешения

### <a name="how-can-i-implement-role-based-access-for-cloud-services"></a>Как реализовать доступ на основе ролей для облачных служб?
Облачные службы не поддерживают модель управления доступом на основе ролей (RBAC), так как она не является службой на основе Azure Resource Manager.

См. [сведения о различных ролях в Azure](../role-based-access-control/rbac-and-directory-admin-roles.md).

## <a name="remote-desktop"></a>Удаленный рабочий стол

### <a name="can-microsoft-internal-engineers-remote-desktop-to-cloud-service-instances-without-permission"></a>Могут ли инженеры Майкрософт без моего разрешения использовать удаленный рабочий стол для доступа к экземплярам облачной службы?
Корпорация Майкрософт придерживается строгого правила, которое не позволит нашим инженерам осуществлять удаленный доступ к облачной службе без письменного разрешения (электронного письма или другого сообщения в письменной форме) от владельца или его уполномоченного лица.

### <a name="i-cannot-remote-desktop-to-cloud-service-vm--by-using-the-rdp-file-i-get-following-error-an-authentication-error-has-occurred-code-0x80004005"></a>Я не могу подключиться к виртуальной машине облачной службы через удаленный рабочий стол с помощью RDP-файла. Я вижу сообщение "Произошла ошибка проверки подлинности (код: 0x80004005)".

Эта ошибка может возникать, если вы используете RDP-файл с компьютера, присоединенного к Azure Active Directory. Проблему можно устранить следующим способом.

1. Щелкните правой кнопкой мыши скачанный RDP-файл и выберите действие **Изменить**.
2. Добавьте префикс "\" перед именем пользователя. Например, укажите **.\username** вместо **username**.

## <a name="scaling"></a>Масштабирование

### <a name="i-cannot-scale-beyond-x-instances"></a>Не удается выполнить масштабирование на более чем X экземпляров
В вашей подписке Azure есть ограничение на количество используемых ядер. При использовании всех доступных ядер масштабирование не сработает. Например, если установлено ограничение в 100 ядер, это означает, что в облачной службе можно создать 100 экземпляров виртуальной машины размера A1 или 50 экземпляров виртуальной машины размера A2.

### <a name="how-can-i-configure-auto-scale-based-on-memory-metrics"></a>Как мне настроить автомасштабирование по метрикам памяти?

Автомасштабирование на основе метрик памяти в настоящее время не поддерживается для облачных служб. 

Чтобы обойти эту проблему, используйте Application Insights. Автомасштабирование поддерживает Application Insights в качестве источника метрик и позволяет изменять число экземпляров роли по любой гостевой метрике, в том числе "Память".  Чтобы реализовать такой механизм, настройте параметры для Application Insights в файле пакета для проекта облачной службы (\*.CSPKG) и включите для службы расширение диагностики Azure.

Дополнительные сведения о применении пользовательских метрик Application Insights для автомасштабирования облачных служб вы найдете в статье [Get started with auto scale by custom metric in Azure](../azure-monitor/platform/autoscale-custom-metric.md) (Начало работы с автомасштабированием в Azure по пользовательским метрикам).

Дополнительные сведения о том, как интегрировать систему диагностики Azure с Application Insights для облачных служб, см. в статье [Send Cloud Service, Virtual Machine, or Service Fabric diagnostic data to Application Insights](../azure-monitor/platform/diagnostics-extension-to-application-insights.md) (Отправка в Application Insights диагностических данных облачной службы, виртуальной машины или Service Fabric).

Дополнительные сведения о том, как включить Application Insights для облачных служб, см. в статье [Application Insights для облачных служб Azure](https://docs.microsoft.com/azure/application-insights/app-insights-cloudservices).

Дополнительные сведения о том, как включить ведение журнала диагностики Azure для облачных служб, см. в разделе [Включение диагностики в проектах облачных служб перед их развертыванием](/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#turn-on-diagnostics-in-cloud-service-projects-before-you-deploy-them).

## <a name="generic"></a>Универсальный шаблон

### <a name="how-do-i-add-nosniff-to-my-website"></a>Как добавить на веб-сайт запрет на сканирование?
Чтобы помешать клиентам сканировать типы MIME, добавьте соответствующий параметр в файл *web.config*.

```xml
<configuration>
   <system.webServer>
      <httpProtocol>
         <customHeaders>
            <add name="X-Content-Type-Options" value="nosniff" />
         </customHeaders>
      </httpProtocol>
   </system.webServer>
</configuration>
```

Его можно также добавить как параметр в службах IIS. Используйте следующую команду из статьи [Распространенные задачи запуска](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe).

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

### <a name="how-do-i-customize-iis-for-a-web-role"></a>Как настроить IIS для веб-роли?
Используйте сценарий запуска IIS из статьи [Распространенные задачи запуска](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe).

### <a name="what-is-the-quota-limit-for-my-cloud-service"></a>Что такое квота для облачной службы?
См. раздел [Ограничения определенных служб](../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits).

### <a name="why-does-the-drive-on-my-cloud-service-vm-show-very-little-free-disk-space"></a>Почему на диске виртуальной машины облачной службы отображается очень мало свободного места?
Это ожидаемое поведение, и оно не должно приводить к проблемам с вашим приложением. Для диска %uproot% в виртуальных машинах PaaS Azure включено ведение журнала, что фактически вдвое увеличивает размер файлов. Однако существует несколько моментов, которые следует учитывать, чтобы это не стало проблемой.

Размер диска% approot% вычисляется как \<размер. cspkg + максимальный размер журнала + поле свободного пространства > или 1,5 ГБ, в зависимости от того, что больше. Размер вашей ВМ никак не влияет на этот расчет. (Размер виртуальной машины влияет на размер временного диска C.) 

Запись на диск %approot% не поддерживается. Если вы записываете данные на виртуальную машину Azure, это необходимо делать во временный ресурс LocalStorage (или выбрать другой вариант, например хранилище BLOB-объектов, файлы Azure и т. п.). Поэтому объем свободного пространства в папке %approot% не имеет значения. Если вы не уверены, записывает ли ваше приложение данные на диск %approot%, всегда можно позволить службе поработать в течение нескольких дней, а затем сравнить размеры до и после. 

Azure ничего не будет записывать на диск %approot%. После создания VHD из CSPKG-файла и подключения его к виртуальной машине Azure запись данных на этот виртуальный жесткий диск может выполнять только ваше приложение. 

Параметры ведения журнала не подлежат настройке, поэтому его нельзя отключить.

### <a name="how-can-i-add-an-antimalware-extension-for-my-cloud-services-in-an-automated-way"></a>Как автоматизировать добавление антивредоносного расширения для облачных служб?

Вы можете включить антивредоносное расширение с помощью сценария PowerShell в задаче запуска. Чтобы реализовать этот механизм, выполните действия в следующих статьях. 
 
- [Создание задачи запуска PowerShell](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).
- [Set-AzureServiceAntimalwareExtension](https://docs.microsoft.com/powershell/module/servicemanagement/azure/Set-AzureServiceAntimalwareExtension?view=azuresmps-4.0.0 ).

Дополнительные сведения о сценариях развертывания защиты от вредоносных программ и о том, как включить эту защиту на портале, вы найдете в разделе [Сценарии развертывания антивредоносного ПО](../security/fundamentals/antimalware.md#antimalware-deployment-scenarios).

### <a name="how-to-enable-server-name-indication-sni-for-cloud-services"></a>Как включить указание имени сервера для облачных служб?

Указание имени сервера в облачных службах можно включить, используя один из следующих методов.

**Метод 1. Использование PowerShell**

Указание имени сервера можно настроить с помощью командлета PowerShell **New-WebBinding**, включив его в задачу запуска для экземпляра роли облачной службы, как показано ниже:
    
    New-WebBinding -Name $WebsiteName -Protocol "https" -Port 443 -IPAddress $IPAddress -HostHeader $HostHeader -SslFlags $sslFlags 
    
Как описано [здесь](https://technet.microsoft.com/library/ee790567.aspx), $sslFlags может принимать одно из следующих значений.

|Значение|Значение|
------|------
|0|Указание имени сервера не используется|
|1|Указание имени сервера используется|
|2|Привязка без указания имени сервера, которая использует центральное хранилище сертификатов|
|3|Привязка с указанием имени сервера, которая использует центральное хранилище сертификатов|
 
**Метод 2: Использование кода**

Также привязку с указанием имени сервера можно настроить, выполняя программный код при запуске роли, как описано в этой [записи блога](https://blogs.msdn.microsoft.com/jianwu/2014/12/17/expose-ssl-service-to-multi-domains-from-the-same-cloud-service/):

    
    //<code snip> 
                    var serverManager = new ServerManager(); 
                    var site = serverManager.Sites[0]; 
                    var binding = site.Bindings.Add(":443:www.test1.com", newCert.GetCertHash(), "My"); 
                    binding.SetAttributeValue("sslFlags", 1); //enables the SNI 
                    serverManager.CommitChanges(); 
    //</code snip> 
    
При использовании любого из представленных выше методов для работы привязки с указанием имени узла нужно сначала установить на экземплярах ролей соответствующие сертификаты (*.pfx) для конкретных имен узлов. Это можно сделать с помощью задачи запуска или в программном коде.

### <a name="how-can-i-add-tags-to-my-azure-cloud-service"></a>Как можно добавить теги к облачной службе Azure? 

Облачная служба представляет собой классический ресурс. Теги поддерживаются только ресурсами, созданными с использованием Azure Resource Manager. К классическим ресурсам, в том числе к облачной службе, теги применить нельзя. 

### <a name="the-azure-portal-doesnt-display-the-sdk-version-of-my-cloud-service-how-can-i-get-that"></a>Портал Azure не отображает версию пакета SDK для моей облачной службы. Как узнать ее?

Мы работаем над тем, чтобы реализовать эту возможность на портале Azure. Пока вы можете узнать версию пакета SDK с помощью этих команд PowerShell:

    Get-AzureService -ServiceName "<Cloud Service name>" | Get-AzureDeployment | Where-Object -Property SdkVersion -NE -Value "" | select ServiceName,SdkVersion,OSVersion,Slot

### <a name="i-want-to-shut-down-the-cloud-service-for-several-months-how-to-reduce-the-billing-cost-of-cloud-service-without-losing-the-ip-address"></a>Я хочу приостановить работу облачной службы на несколько месяцев. Как снизить расходы на обслуживание облачной службы, не теряя при этом IP-адрес?

Любая развернутая облачная служба оплачивается по объему ресурсов вычисления и хранения, которые для нее выделены. Это означает, что вам придется оплачивать хранилище виртуальной машины Azure, даже если она отключена. 

Чтобы снизить затраты, не теряя выделенный для вашей службы IP-адрес, выполните следующие действия.

1. [Зарезервируйте IP-адрес](../virtual-network/virtual-networks-reserved-public-ip.md), а затем удалите развертывание.  Теперь вы будете оплачивать только IP-адрес. Дополнительные сведения о выставлении счетов за IP-адреса вы найдете на странице [Цены на IP-адреса](https://azure.microsoft.com/pricing/details/ip-addresses/).
2. Удалите развертывание. Не удаляйте домен xxx.cloudapp.net, чтобы его можно было использовать в будущем.
3. Когда вы решите повторно развернуть облачную службу, применив для нее зарезервированный в подписке IP-адрес, воспользуйтесь инструкциями из статьи [Reserved IP addresses for Cloud Services and Virtual Machines](https://azure.microsoft.com/blog/reserved-ip-addresses/) (Зарезервированные IP-адреса для облачных служб и виртуальных машин).

