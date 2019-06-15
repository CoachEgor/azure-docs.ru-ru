---
title: Операции поставщиков ресурсов Azure Resource Manager | Документация Майкрософт
description: Списки операций, доступных для поставщиков ресурсов Microsoft Azure Resource Manager.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 07c1a726e33eb8287634b63ef2e309483c05c3f3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65962115"
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Операции поставщиков ресурсов Azure Resource Manager

В этой статье перечислены операции, доступные в каждом поставщике ресурсов Azure Resource Manager. Эти операции могут использоваться в [настраиваемых ролях](custom-roles.md), чтобы обеспечить детализированное [управление доступом на основе ролей (RBAC)](overview.md) для ресурсов в Azure. Строки операций имеют следующий формат: `{Company}.{ProviderName}/{resourceType}/{action}`

Операции в поставщиках ресурсов всегда развиваются. Чтобы получить список последних операций, используйте команду [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) или [az provider operation list](/cli/azure/provider/operation#az-provider-operation-list).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.AAD/domainServices/delete | Удаление службы домена |
> | Действие | Microsoft.AAD/domainServices/oucontainer/delete | Удаление контейнера подразделения |
> | Действие | Microsoft.AAD/domainServices/oucontainer/read | Чтение контейнеров подразделения |
> | Действие | Microsoft.AAD/domainServices/oucontainer/write | Запись в контейнер подразделения |
> | Действие | Microsoft.AAD/domainServices/read | Чтение доменных служб |
> | Действие | Microsoft.AAD/domainServices/ReplicaSets/delete | Удалить узел кластера |
> | Действие | Microsoft.AAD/domainServices/ReplicaSets/read | Узел кластера для чтения |
> | Действие | Microsoft.AAD/domainServices/ReplicaSets/write | Запись узла кластера |
> | Действие | Microsoft.AAD/domainServices/write | Запись службы домена |
> | Действие | Microsoft.AAD/locations/operationresults/read |  |
> | Действие | Microsoft.AAD/Operations/read |  |
> | Действие | Microsoft.AAD/register/action | Регистрация службы домена |
> | Действие | Microsoft.AAD/unregister/action | Отмена регистрации службы домена |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | microsoft.aadiam/diagnosticsettings/delete | Удаление параметра диагностики. |
> | Действие | microsoft.aadiam/diagnosticsettings/read | Чтение параметра диагностики. |
> | Действие | microsoft.aadiam/diagnosticsettings/write | Запись параметра диагностики. |
> | Действие | microsoft.aadiam/diagnosticsettingscategories/read | Чтение категорий параметра диагностики. |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Addons/operations/read | Получает поддерживаемые операции RP. |
> | Действие | Microsoft.Addons/register/action | Регистрирует указанную подписку в Microsoft.Addons. |
> | Действие | Microsoft.Addons/supportProviders/listsupportplaninfo/action | Выводит сведения о текущем плане поддержки для указанной подписки. |
> | Действие | Microsoft.Addons/supportProviders/supportPlanTypes/delete | Удаляет указанный план поддержки Canonical. |
> | Действие | Microsoft.Addons/supportProviders/supportPlanTypes/read | Получает состояние указанного плана поддержки Canonical. |
> | Действие | Microsoft.Addons/supportProviders/supportPlanTypes/write | Добавляет указанный тип плана поддержки Canonical. |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.ADHybridHealthService/addsservices/action | Создает лес для клиента. |
> | Действие | Microsoft.ADHybridHealthService/addsservices/addomainservicemembers/read | Получает все серверы для указанного имени службы. |
> | Действие | Microsoft.ADHybridHealthService/addsservices/alerts/read | Получает сведения об оповещениях для леса, например идентификатор оповещения, дата получения, последнее обнаружение, описание, последнее обновление, уровень и состояние оповещения, а также ссылки на устранение неполадок с оповещением и т. д. |
> | Действие | Microsoft.ADHybridHealthService/addsservices/configuration/read | Получает конфигурацию службы для леса. Например, имя леса, функциональный уровень, роль хозяина именования доменов FSMO, роль хозяина схемы FSMO и т. д. |
> | Действие | Microsoft.ADHybridHealthService/addsservices/delete | Удаляет службу и ее серверы, а также данные о работоспособности. |
> | Действие | Microsoft.ADHybridHealthService/addsservices/dimensions/read | Получает сведения о доменах и сайтах для леса. Например, состояние работоспособности, активные оповещения, разрешенные оповещения, такие свойства, как функциональный уровень домена, лес, хозяин инфраструктуры, PDC, хозяин RID и т. д.  |
> | Действие | Microsoft.ADHybridHealthService/addsservices/features/userpreference/read | Получает параметр настройки пользователя для леса.<br>Например, значения MetricCounterName, такие как ldapsuccessfulbinds, ntlmauthentications, kerberosauthentications, addsinsightsagentprivatebytes, ldapsearches.<br>Параметры диаграммы пользовательского интерфейса и т. д. |
> | Действие | Microsoft.ADHybridHealthService/addsservices/forestsummary/read | Получает сводку для данного леса, например имя леса, количество доменов в лесу, количество сайтов, сведения о сайтах и т. д. |
> | Действие | Microsoft.ADHybridHealthService/addsservices/metricmetadata/read | Получает список поддерживаемых метрик для данной службы.<br>Например, число блокировок учетных записей из экстрасети, общее число невыполненных запросов, число невыполненных запросов маркера (прокси), число запросов токенов/с и т. д. для службы ADFS.<br>Количество проверок подлинности NTLM/с, число успешных привязок LDAP/с, время привязки LDAP, количество активных потоков LDAP, число проверок подлинности Kerberos/с, общее количество потоков ATQ и т. д. для службы ADDomain.<br>Задержка запуска профиля, установленные TCP-подключения, частные байты агента получения информации, статистика по экспорту в Azure AD для службы ADSync. |
> | Действие | Microsoft.ADHybridHealthService/addsservices/metrics/groups/read | Для данной службы этот API получает информацию о метриках.<br>Например, этот API можно использовать для получения следующей информации: число блокировок учетных записей из экстрасети, общее число невыполненных запросов, число невыполненных запросов маркера (прокси), число запросов токенов/с и т. д. для службы ADFS.<br>Количество проверок подлинности NTLM/с, число успешных привязок LDAP/с, время привязки LDAP, количество активных потоков LDAP, число проверок подлинности Kerberos/с, общее количество потоков ATQ и т. д. для службы ADDomain.<br>Задержка запуска профиля, установленные TCP-подключения, частные байты агента получения информации, статистика по экспорту в Azure AD для службы синхронизации. |
> | Действие | Microsoft.ADHybridHealthService/addsservices/premiumcheck/read | Этот API возвращает список всех подключенных служб ADDomainServices для клиента категории "Премиум". |
> | Действие | Microsoft.ADHybridHealthService/addsservices/read | Получает сведения о службе для указанного имени службы. |
> | Действие | Microsoft.ADHybridHealthService/addsservices/replicationdetails/read | Получает сведения о репликации всех серверов для указанного имени службы. |
> | Действие | Microsoft.ADHybridHealthService/addsservices/replicationstatus/read | Получает число контроллеров домена и их ошибок репликации, если таковые имеются. |
> | Действие | Microsoft.ADHybridHealthService/addsservices/replicationsummary/read | Получает полный список контроллеров домена и сведений о репликации для данного леса. |
> | Действие | Microsoft.ADHybridHealthService/addsservices/servicemembers/action | Добавляет экземпляр сервера в службу. |
> | Действие | Microsoft.ADHybridHealthService/addsservices/servicemembers/credentials/read | Во время регистрации сервера ADDomainService этот API вызывается, чтобы получить учетные данные для подключения новых серверов. |
> | Действие | Microsoft.ADHybridHealthService/addsservices/servicemembers/delete | Удаляет сервер для данной службы и клиента. |
> | Действие | Microsoft.ADHybridHealthService/addsservices/write | Создает или обновляет экземпляр ADDomainService для клиента. |
> | Действие | Microsoft.ADHybridHealthService/configuration/action | Обновляет конфигурацию клиента. |
> | Действие | Microsoft.ADHybridHealthService/configuration/read | Считывает конфигурацию клиента. |
> | Действие | Microsoft.ADHybridHealthService/configuration/write | Создает конфигурацию клиента. |
> | Действие | Microsoft.ADHybridHealthService/logs/contents/read | Получает содержимое журналов установки и регистрации агента, хранимых в большом двоичном объекте. |
> | Действие | Microsoft.ADHybridHealthService/logs/read | Получает журналы установки и регистрации агента для клиента. |
> | Действие | Microsoft.ADHybridHealthService/operations/read | Возвращает список операций, поддерживаемых системой. |
> | Действие | Microsoft.ADHybridHealthService/register/action | Регистрирует поставщик ресурсов службы работоспособности ADHybrid и позволяет создавать ресурсы службы работоспособности ADHybrid. |
> | Действие | Microsoft.ADHybridHealthService/reports/availabledeployments/read | Получает список доступных регионов, используемых командой DevOps для поддержки инцидентов клиентов. |
> | Действие | Microsoft.ADHybridHealthService/reports/badpassword/read | Получает список попыток неправильного ввода пароля для всех пользователей в службе федерации Active Directory. |
> | Действие | Microsoft.ADHybridHealthService/reports/badpassworduseridipfrequency/read | Получает универсальный код ресурса SAS большого двоичного объекта, содержащий состояние и конечный результат недавно поставленного в очередь задания отчета для определения частоты попыток неправильного ввода имени пользователя или пароля на каждый идентификатор пользователя по каждому IP-адресу в день для данного клиента. |
> | Действие | Microsoft.ADHybridHealthService/reports/consentedtodevopstenants/read | Получает список клиентов, получивших согласие DevOps. Обычно используется для поддержки клиентов. |
> | Действие | Microsoft.ADHybridHealthService/reports/isdevops/read | Получение значения, указывающего, имеет ли клиент согласие DevOps. |
> | Действие | Microsoft.ADHybridHealthService/reports/selectdevopstenant/read | Обновляет идентификатор пользователя (идентификатор объекта) выбранного клиента DevOps. |
> | Действие | Microsoft.ADHybridHealthService/reports/selecteddeployment/read | Возвращает выбранное развертывание для данного клиента. |
> | Действие | Microsoft.ADHybridHealthService/reports/tenantassigneddeployment/read | Получает расположение хранилища клиента на основе идентификатора клиента. |
> | Действие | Microsoft.ADHybridHealthService/reports/updateselecteddeployment/read | Возвращает географическое расположение, из которого будет осуществляться доступ к данных. |
> | Действие | Microsoft.ADHybridHealthService/services/action | Обновляет экземпляр службы в клиенте. |
> | Действие | Microsoft.ADHybridHealthService/services/alerts/read | Считывает оповещения для службы. |
> | Действие | Microsoft.ADHybridHealthService/services/alerts/read | Считывает оповещения для службы. |
> | Действие | Microsoft.ADHybridHealthService/services/checkservicefeatureavailibility/read | На основе имени функции проверяет, имеется ли у службы все необходимое для использования этой функции. |
> | Действие | Microsoft.ADHybridHealthService/services/delete | Удаляет экземпляр службы из клиента. |
> | Действие | Microsoft.ADHybridHealthService/services/exporterrors/read | Возвращает ошибки экспорта для данной службы синхронизации. |
> | Действие | Microsoft.ADHybridHealthService/services/exportstatus/read | Возвращает состояние экспорта для данной службы. |
> | Действие | Microsoft.ADHybridHealthService/services/feedbacktype/feedback/read | Получает обратную связь об оповещениях для данной службы и сервера. |
> | Действие | Microsoft.ADHybridHealthService/services/metricmetadata/read | Получает список поддерживаемых метрик для данной службы.<br>Например, число блокировок учетных записей из экстрасети, общее число невыполненных запросов, число невыполненных запросов маркера (прокси), число запросов токенов/с и т. д. для службы ADFS.<br>Количество проверок подлинности NTLM/с, число успешных привязок LDAP/с, время привязки LDAP, количество активных потоков LDAP, число проверок подлинности Kerberos/с, общее количество потоков ATQ и т. д. для службы ADDomain.<br>Задержка запуска профиля, установленные TCP-подключения, частные байты агента получения информации, статистика по экспорту в Azure AD для службы ADSync. |
> | Действие | Microsoft.ADHybridHealthService/services/metrics/groups/average/read | Этот API получает среднее значение метрик для данной службы.<br>Например, этот API можно использовать для получения следующей информации: число блокировок учетных записей из экстрасети, общее число невыполненных запросов, число невыполненных запросов маркера (прокси), число запросов токенов/с и т. д. для службы ADFS.<br>Количество проверок подлинности NTLM/с, число успешных привязок LDAP/с, время привязки LDAP, количество активных потоков LDAP, число проверок подлинности Kerberos/с, общее количество потоков ATQ и т. д. для службы ADDomain.<br>Задержка запуска профиля, установленные TCP-подключения, частные байты агента получения информации, статистика по экспорту в Azure AD для службы синхронизации. |
> | Действие | Microsoft.ADHybridHealthService/services/metrics/groups/read | Для данной службы этот API получает информацию о метриках.<br>Например, этот API можно использовать для получения следующей информации: число блокировок учетных записей из экстрасети, общее число невыполненных запросов, число невыполненных запросов маркера (прокси), число запросов токенов/с и т. д. для службы ADFS.<br>Количество проверок подлинности NTLM/с, число успешных привязок LDAP/с, время привязки LDAP, количество активных потоков LDAP, число проверок подлинности Kerberos/с, общее количество потоков ATQ и т. д. для службы ADDomain.<br>Задержка запуска профиля, установленные TCP-подключения, частные байты агента получения информации, статистика по экспорту в Azure AD для службы синхронизации. |
> | Действие | Microsoft.ADHybridHealthService/services/metrics/groups/sum/read | Этот API получает агрегированное представление о метриках для данной службы.<br>Например, этот API можно использовать для получения следующей информации: число блокировок учетных записей из экстрасети, общее число невыполненных запросов, число невыполненных запросов маркера (прокси), число запросов токенов/с и т. д. для службы ADFS.<br>Количество проверок подлинности NTLM/с, число успешных привязок LDAP/с, время привязки LDAP, количество активных потоков LDAP, число проверок подлинности Kerberos/с, общее количество потоков ATQ и т. д. для службы ADDomain.<br>Задержка запуска профиля, установленные TCP-подключения, частные байты агента получения информации, статистика по экспорту в Azure AD для службы синхронизации. |
> | Действие | Microsoft.ADHybridHealthService/services/monitoringconfiguration/write | Добавляет или обновляет конфигурацию мониторинга для службы. |
> | Действие | Microsoft.ADHybridHealthService/services/monitoringconfigurations/read | Получает конфигурации мониторинга для данной службы. |
> | Действие | Microsoft.ADHybridHealthService/services/monitoringconfigurations/write | Добавляет или обновляет конфигурации мониторинга для службы. |
> | Действие | Microsoft.ADHybridHealthService/services/premiumcheck/read | Этот API возвращает список всех подключенных служб для клиента категории "Премиум". |
> | Действие | Microsoft.ADHybridHealthService/services/read | Считывает экземпляры службы в клиенте. |
> | Действие | Microsoft.ADHybridHealthService/services/reports/blobUris/read | Получает все ненадежном IP-адресе отчета идентификаторы URI для за последние 7 дней. |
> | Действие | Microsoft.ADHybridHealthService/services/reports/details/read | Возвращает отчет о первых 50 пользователях, вводивших неправильный пароль, за последние 7 дней. |
> | Действие | Microsoft.ADHybridHealthService/services/reports/generateBlobUri/action | Создает отчет об опасных IP-адрес и возвращает URI, указывающий на него. |
> | Действие | Microsoft.ADHybridHealthService/services/servicemembers/action | Создает экземпляр службы в клиенте. |
> | Действие | Microsoft.ADHybridHealthService/services/servicemembers/alerts/read | Считывает оповещения для сервера. |
> | Действие | Microsoft.ADHybridHealthService/services/servicemembers/credentials/read | Во время регистрации сервера этот API вызывается, чтобы получить учетные данные для подключения новых серверов. |
> | Действие | Microsoft.ADHybridHealthService/services/servicemembers/datafreshness/read | Для данного сервера этот API возвращает список типов данных, которые передаются серверами, и самое позднее время каждой отправки. |
> | Действие | Microsoft.ADHybridHealthService/services/servicemembers/delete | Удаляет экземпляр службы из службы. |
> | Действие | Microsoft.ADHybridHealthService/services/servicemembers/exportstatus/read | Возвращает сведения об ошибке экспорта данных синхронизации для указанной службы синхронизации. |
> | Действие | Microsoft.ADHybridHealthService/services/servicemembers/metrics/groups/read | Для данной службы этот API получает информацию о метриках.<br>Например, этот API можно использовать для получения следующей информации: число блокировок учетных записей из экстрасети, общее число невыполненных запросов, число невыполненных запросов маркера (прокси), число запросов токенов/с и т. д. для службы ADFS.<br>Количество проверок подлинности NTLM/с, число успешных привязок LDAP/с, время привязки LDAP, количество активных потоков LDAP, число проверок подлинности Kerberos/с, общее количество потоков ATQ и т. д. для службы ADDomain.<br>Задержка запуска профиля, установленные TCP-подключения, частные байты агента получения информации, статистика по экспорту в Azure AD для службы синхронизации. |
> | Действие | Microsoft.ADHybridHealthService/services/servicemembers/metrics/read | Возвращает список соединителей и имена профилей выполнения для данной службы и службы элемента. |
> | Действие | Microsoft.ADHybridHealthService/services/servicemembers/read | Считывает экземпляр сервера в службе. |
> | Действие | Microsoft.ADHybridHealthService/services/servicemembers/serviceconfiguration/read | Получает конфигурацию службы для данного клиента. |
> | Действие | Microsoft.ADHybridHealthService/services/tenantwhitelisting/read | Получает состояние разрешенных функций для данного клиента. |
> | Действие | Microsoft.ADHybridHealthService/services/write | Создает экземпляр службы в клиенте. |
> | Действие | Microsoft.ADHybridHealthService/unregister/action | Отменяет регистрацию подписки для поставщика ресурсов службы работоспособности ADHybrid. |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Advisor/configurations/read | Получение конфигураций |
> | Действие | Microsoft.Advisor/configurations/write | Создание или обновление конфигурации. |
> | Действие | Microsoft.Advisor/generateRecommendations/action | Создает рекомендации. |
> | Действие | Microsoft.Advisor/generateRecommendations/read | Возвращает состояние создания рекомендаций. |
> | Действие | Microsoft.Advisor/operations/read | Получение операций для Microsoft Advisor. |
> | Действие | Microsoft.Advisor/recommendations/available/action | В Microsoft Advisor доступна новая рекомендация. |
> | Действие | Microsoft.Advisor/recommendations/read | Считывает рекомендации. |
> | Действие | Microsoft.Advisor/recommendations/suppressions/delete | Удаляет подавления. |
> | Действие | Microsoft.Advisor/recommendations/suppressions/read | Возвращает подавления. |
> | Действие | Microsoft.Advisor/recommendations/suppressions/write | Создает или обновляет подавления. |
> | Действие | Microsoft.Advisor/register/action | Регистрирует подписку для Microsoft Advisor. |
> | Действие | Microsoft.Advisor/suppressions/delete | Удаляет подавления. |
> | Действие | Microsoft.Advisor/suppressions/read | Возвращает подавления. |
> | Действие | Microsoft.Advisor/suppressions/write | Создает или обновляет подавления. |
> | Действие | Microsoft.Advisor/unregister/action | Отмена регистрации подписки для Microsoft Advisor. |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.AlertsManagement/actionRules/delete | Удаление правила действия в рамках заданной подписки. |
> | Действие | Microsoft.AlertsManagement/actionRules/read | Получение всех правил действий для фильтров входящих данных. |
> | Действие | Microsoft.AlertsManagement/actionRules/write | Создание или обновление правила действия в рамках заданной подписки. |
> | Действие | Microsoft.AlertsManagement/alerts/changestate/action | Изменение состояния оповещения. |
> | Действие | Microsoft.AlertsManagement/alerts/diagnostics/read | Получение всех данных диагностики для оповещения. |
> | Действие | Microsoft.AlertsManagement/alerts/history/read | Получение журнала оповещения. |
> | Действие | Microsoft.AlertsManagement/alerts/read | Получение всех оповещений для фильтров входа. |
> | Действие | Microsoft.AlertsManagement/alertsList/read | Получение всех оповещений для фильтров входящих данных в рамках подписок. |
> | Действие | Microsoft.AlertsManagement/alertsSummary/read | Получение сводки оповещений. |
> | Действие | Microsoft.AlertsManagement/alertsSummaryList/read | Получение сводки оповещений для подписок. |
> | Действие | Microsoft.AlertsManagement/Operations/read | Чтение предоставленных операций. |
> | Действие | Microsoft.AlertsManagement/register/action | Регистрация подписки для Управления оповещениями (Майкрософт). |
> | Действие | Microsoft.AlertsManagement/smartDetectorAlertRules/delete | Удаление правила генерации оповещений интеллектуального обнаружения в рамках одной подписки |
> | Действие | Microsoft.AlertsManagement/smartDetectorAlertRules/read | Получить все правила генерации оповещений интеллектуального обнаружения для фильтров входа |
> | Действие | Microsoft.AlertsManagement/smartDetectorAlertRules/write | Создание или обновление правила генерации оповещений интеллектуального обнаружения в рамках одной подписки |
> | Действие | Microsoft.AlertsManagement/smartGroups/changestate/action | Изменение состояния смарт-группы. |
> | Действие | Microsoft.AlertsManagement/smartGroups/history/read | Получение журнала смарт-группы. |
> | Действие | Microsoft.AlertsManagement/smartGroups/read | Получение всех смарт-групп для фильтров ввода. |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.AnalysisServices/locations/checkNameAvailability/action | Проверяет, является ли данное имя сервера анализа данных допустимым и неиспользуемым. |
> | Действие | Microsoft.AnalysisServices/locations/operationresults/read | Получение сведений о результате указанной операции. |
> | Действие | Microsoft.AnalysisServices/locations/operationstatuses/read | Извлечение сведений о состоянии указанной операции. |
> | Действие | Microsoft.AnalysisServices/operations/read | Извлечение сведений об операциях. |
> | Действие | Microsoft.AnalysisServices/register/action | Регистрация поставщика ресурсов для служб Analysis Services. |
> | Действие | Microsoft.AnalysisServices/servers/delete | Удаляет сервер анализа данных. |
> | Действие | Microsoft.AnalysisServices/servers/listGatewayStatus/action | Отображение состояния шлюза, связанного с сервером. |
> | Действие | Microsoft.AnalysisServices/servers/read | Извлекает информацию об указанном сервере анализа данных. |
> | Действие | Microsoft.AnalysisServices/servers/resume/action | Возобновляет работу сервера анализа данных. |
> | Действие | Microsoft.AnalysisServices/servers/skus/read | Получение доступных сведений о номере SKU для сервера. |
> | Действие | Microsoft.AnalysisServices/servers/suspend/action | Приостанавливает сервер анализа данных. |
> | Действие | Microsoft.AnalysisServices/servers/write | Создает или обновляет указанный сервер анализа данных. |
> | Действие | Microsoft.AnalysisServices/skus/read | Извлечение сведений о номерах SKU. |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.ApiManagement/checkNameAvailability/read | Проверяет, доступно ли указанное имя службы. |
> | Действие | Microsoft.ApiManagement/operations/read | Чтение всех операций API для ресурса Microsoft.ApiManagement. |
> | Действие | Microsoft.ApiManagement/register/action | Регистрирует подписку для поставщика ресурсов Microsoft.ApiManagement. |
> | Действие | Microsoft.ApiManagement/reports/read | Получение отчетов, объединенных по периодам времени, географическим регионам, разработчикам, продуктам, API-интерфейсам, операциям, подпискам и запросам. |
> | Действие | Microsoft.ApiManagement/service/apis/delete | Удаляет указанный API экземпляра службы управления API. |
> | Действие | Microsoft.ApiManagement/service/apis/diagnostics/delete | Удаляет указанный диагностики с помощью API. |
> | Действие | Microsoft.ApiManagement/service/apis/diagnostics/read | Список всех диагностических сведений из API. или получение параметров диагностики для API, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/apis/diagnostics/write | Создает новый диагностики для API или обновляет существующий. или обновление параметров диагностики для API, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/apis/issues/attachments/delete | Удаляет указанный комментарий с проблемой. |
> | Действие | Microsoft.ApiManagement/service/apis/issues/attachments/read | Списки все вложения, указывающие на данную проблему, связанный с указанного API. или получает сведения о проблеме вложения для API, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/apis/issues/attachments/write | Создает новое вложение проблемы в интерфейсе API или обновляет существующий. |
> | Действие | Microsoft.ApiManagement/service/apis/issues/comments/delete | Удаляет указанный комментарий с проблемой. |
> | Действие | Microsoft.ApiManagement/service/apis/issues/comments/read | Списки все комментарии, проблемы, связанные с указанного API. или получает сведения о проблеме комментарий для API, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/apis/issues/comments/write | Создает новый комментарий для проблемы в API или обновляет существующий. |
> | Действие | Microsoft.ApiManagement/service/apis/issues/delete | Удаляет указанный проблему с помощью API. |
> | Действие | Microsoft.ApiManagement/service/apis/issues/read | Список всех проблем, связанных с указанного API. или получает сведения о проблеме для API, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/apis/issues/write | Создает новую проблему для API или обновляет существующий. или обновления существующего выпуска для API. |
> | Действие | Microsoft.ApiManagement/service/apis/operations/delete | Удаляет указанную операцию в API. |
> | Действие | Microsoft.ApiManagement/service/apis/operations/policies/delete | Удаляет конфигурацию политики во время операции Api. |
> | Действие | Microsoft.ApiManagement/service/apis/operations/policies/read | Получение списка конфигурации политики на уровне операций API. или возвращает конфигурацию политики на уровне операций API. |
> | Действие | Microsoft.ApiManagement/service/apis/operations/policies/write | Создает или обновляет конфигурацию политики для уровня API-операции. |
> | Действие | Microsoft.ApiManagement/service/apis/operations/policy/delete | Удалить конфигурацию политики на уровне операции |
> | Действие | Microsoft.ApiManagement/service/apis/operations/policy/read | Возвращает конфигурацию политики на уровне операции |
> | Действие | Microsoft.ApiManagement/service/apis/operations/policy/write | Создание политики конфигурации во время операции уровня |
> | Действие | Microsoft.ApiManagement/service/apis/operations/read | Перечисляет коллекцию операций для указанного API. или получает сведения об операции API, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/apis/operations/tags/delete | Отсоединение тега из операции. |
> | Действие | Microsoft.ApiManagement/service/apis/operations/tags/read | Перечислены все теги, связанные с операцией. или тег, связанный с операцией. |
> | Действие | Microsoft.ApiManagement/service/apis/operations/tags/write | Назначьте тег операцию. |
> | Действие | Microsoft.ApiManagement/service/apis/operations/write | Создает новую операцию в API или обновляет существующий. или обновления, сведения об операции в API, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/apis/operationsByTags/read | Перечисляет коллекцию операций, связанных с тегами. |
> | Действие | Microsoft.ApiManagement/service/apis/policies/delete | Удаляет конфигурацию политики в Api. |
> | Действие | Microsoft.ApiManagement/service/apis/policies/read | Возвращает конфигурацию политики на уровне API. или возвращает конфигурацию политики на уровне API. |
> | Действие | Microsoft.ApiManagement/service/apis/policies/write | Создает или обновляет конфигурацию политики для API. |
> | Действие | Microsoft.ApiManagement/service/apis/policy/delete | Удалить конфигурацию политики на уровне API |
> | Действие | Microsoft.ApiManagement/service/apis/policy/read | Возвращает конфигурацию политики на уровне API |
> | Действие | Microsoft.ApiManagement/service/apis/policy/write | Создание политики конфигурации в API уровня |
> | Действие | Microsoft.ApiManagement/service/apis/products/read | Перечислены все продукты, которые API-Интерфейс является частью. |
> | Действие | Microsoft.ApiManagement/service/apis/read | Перечисляет все API-интерфейсы экземпляра службы управления API. или получает сведения об интерфейсе API, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/apis/releases/delete | Удаление всех версий API или удаляет указанным выпуском в API. |
> | Действие | Microsoft.ApiManagement/service/apis/releases/read | Перечислены все выпуски API.<br>В выпуске API создается при выполнении текущего редакцию API.<br>Выпуски также используются для отката к предыдущей редакции.<br>Результаты будут выгружаться и может быть ограничена параметрами $top и $skip.<br>или возвращает сведения о API выпуска. |
> | Действие | Microsoft.ApiManagement/service/apis/releases/write | Создает новый выпуск для API. или обновления, сведения о выпуске API-интерфейса, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/apis/revisions/delete | Удаление всех редакций API. |
> | Действие | Microsoft.ApiManagement/service/apis/revisions/read | Список всех редакций API. |
> | Действие | Microsoft.ApiManagement/service/apis/schemas/delete | Удаляет конфигурацию схемы в Api. |
> | Действие | Microsoft.ApiManagement/service/apis/schemas/read | Получите конфигурацию схемы на уровне API. или получить конфигурацию схемы на уровне API. |
> | Действие | Microsoft.ApiManagement/service/apis/schemas/write | Создает или обновляет конфигурацию схемы для API. |
> | Действие | Microsoft.ApiManagement/service/apis/tagDescriptions/delete | Удаление описания тега для Api. |
> | Действие | Microsoft.ApiManagement/service/apis/tagDescriptions/read | Список всех описаний тегов в области API. Модель, аналогичную swagger — tagDescription определен на уровне API, но тега может быть назначен операции или получить описания тега в области API. |
> | Действие | Microsoft.ApiManagement/service/apis/tagDescriptions/write | Создание или обновление описания тега в области API-интерфейса. |
> | Действие | Microsoft.ApiManagement/service/apis/tags/delete | Отсоединение тега из Api. |
> | Действие | Microsoft.ApiManagement/service/apis/tags/read | Список всех тегов, сопоставленных с помощью API. или тег, связанный с интерфейсом API. |
> | Действие | Microsoft.ApiManagement/service/apis/tags/write | Назначьте тег Api. |
> | Действие | Microsoft.ApiManagement/service/apis/write | Создает новые или обновляет существующий указанный API экземпляра службы управления API. или обновления указанный API экземпляра службы управления API. |
> | Действие | Microsoft.ApiManagement/service/apisByTags/read | Перечисляет коллекцию API, связанных с тегами. |
> | Действие | Microsoft.ApiManagement/service/apiVersionSets/delete | Удаляет набор определенной версии Api. |
> | Действие | Microsoft.ApiManagement/service/apiVersionSets/read | Перечисляет коллекцию наборов API версии в указанном экземпляре службы. или получает сведения о версии Api, значение указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/apiVersionSets/versions/read | Получение списка сущностей версии. |
> | Действие | Microsoft.ApiManagement/service/apiVersionSets/write | Создает или обновляет набор версию Api. или обновления параметров набора версий Api, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/applynetworkconfigurationupdates/action | Обновляет ресурсы Microsoft.ApiManagement, работающие в виртуальной сети, для применения обновленных параметров сети. |
> | Действие | Microsoft.ApiManagement/service/authorizationServers/delete | Удаляет экземпляр сервера специфической авторизации. |
> | Действие | Microsoft.ApiManagement/service/authorizationServers/read | Перечисляет коллекцию серверов авторизации в указанном экземпляре службы. или получение сведений о сервере авторизации, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/authorizationServers/write | Создает новый сервер авторизации или обновляет существующий сервер авторизации. или обновления сведений о сервере авторизации, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/backends/delete | Удаляет указанную серверную часть. |
> | Действие | Microsoft.ApiManagement/service/backends/read | Перечисляет коллекцию внутренних серверов в указанном экземпляре службы. или получает сведения о серверной части, указанной ее идентификатором. |
> | Действие | Microsoft.ApiManagement/service/backends/reconnect/action | Уведомляет прокси-сервера службы управления API, создайте новое соединение в серверную часть после указанного времени ожидания. Если время ожидания не указано, используется время ожидания в течение 2 минут. |
> | Действие | Microsoft.ApiManagement/service/backends/write | Создание или обновление серверной части. или обновляет существующую. |
> | Действие | Microsoft.ApiManagement/service/backup/action | Архивирует службу управления API в указанный контейнер в предоставленной пользователем учетной записи хранения. |
> | Действие | Microsoft.ApiManagement/service/caches/delete | Удаляет указанным кэшем. |
> | Действие | Microsoft.ApiManagement/service/caches/read | Перечисляет коллекцию всех внешних кэшей в указанном экземпляре службы. или получение сведений о кэше, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/caches/write | Создает или обновляет External Cache для использования в экземпляр службы управления Api. или обновления сведений о кэше, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/certificates/delete | Удаляет указанный сертификат. |
> | Действие | Microsoft.ApiManagement/service/certificates/read | Перечисляет коллекцию всех сертификатов в указанном экземпляре службы. или получение сведений о сертификате, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/certificates/write | Создает или обновляет сертификат, используемый для проверки подлинности с серверной частью. |
> | Действие | Microsoft.ApiManagement/service/contentTypes/contentItems/delete | Удаляет указанный элемент содержимого. |
> | Действие | Microsoft.ApiManagement/service/contentTypes/contentItems/read | Возвращает список элементов содержимого или сведения об элементе содержимого. |
> | Действие | Microsoft.ApiManagement/service/contentTypes/contentItems/write | Создает новый элемент содержимого или обновляет указанный элемент содержимого. |
> | Действие | Microsoft.ApiManagement/service/contentTypes/read | Возвращает список типов содержимого. |
> | Действие | Microsoft.ApiManagement/service/delete | Удаляет экземпляр службы управления API. |
> | Действие | Microsoft.ApiManagement/service/diagnostics/delete | Удаляет указанный диагностики. |
> | Действие | Microsoft.ApiManagement/service/diagnostics/read | Перечислены все средства диагностики экземпляра службы управления API. или получение данных диагностики, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/diagnostics/write | Создает новый диагностики или обновляет существующий. или обновление параметров диагностики, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/getssotoken/action | Возвращает маркер единого входа, который может использоваться для входа на портал прежней версии службы управления API от имени администратора. |
> | Действие | Microsoft.ApiManagement/service/groups/delete | Удаляет определенную группу в экземпляр службы управления API. |
> | Действие | Microsoft.ApiManagement/service/groups/read | Перечисляет коллекцию групп в указанном экземпляре службы. или получение сведений о группе, указанной ее идентификатором. |
> | Действие | Microsoft.ApiManagement/service/groups/users/delete | Удаляет пользователя из существующей группы. |
> | Действие | Microsoft.ApiManagement/service/groups/users/read | Перечисляет коллекцию пользовательские сущности, связанные с группой. |
> | Действие | Microsoft.ApiManagement/service/groups/users/write | Добавляет существующего пользователя в существующий продукт. |
> | Действие | Microsoft.ApiManagement/service/groups/write | Создает или обновляет группу. или обновления сведений о группе, указанной ее идентификатором. |
> | Действие | Microsoft.ApiManagement/service/identityProviders/delete | Удаляет конфигурацию поставщика указанное удостоверение. |
> | Действие | Microsoft.ApiManagement/service/identityProviders/read | Перечисляет коллекцию поставщика удостоверений, настроенного в указанном экземпляре службы. или возвращает информацию о конфигурации поставщика удостоверений настроен в указанном экземпляре службы. |
> | Действие | Microsoft.ApiManagement/service/identityProviders/write | Создает или обновляет конфигурацию поставщика удостоверений. или обновляет существующую конфигурацию поставщика удостоверений. |
> | Действие | Microsoft.ApiManagement/service/issues/read | Перечисляет коллекцию проблемы в указанном экземпляре службы. или сведения о проблеме управления API. |
> | Действие | Microsoft.ApiManagement/service/locations/networkstatus/read | Получение состояния доступа к сетевым ресурсам, от которых зависит служба в расположении. |
> | Действие | Microsoft.ApiManagement/service/loggers/delete | Удаляет указанного регистратора. |
> | Действие | Microsoft.ApiManagement/service/loggers/read | Перечисляет коллекцию средств ведения журнала в указанном экземпляре службы. или получение сведений средства ведения журнала, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/loggers/write | Создание или обновление средства ведения журнала. или обновляет существующее средство ведения журнала. |
> | Действие | Microsoft.ApiManagement/service/managedeployments/action | Позволяет изменить номер SKU или единицы, а также добавить или удалить региональные развертывания службы управления API. |
> | Действие | Microsoft.ApiManagement/service/networkstatus/read | Получение состояния доступа к сетевым ресурсам, от которых зависит служба. |
> | Действие | Microsoft.ApiManagement/service/notifications/action | Отправление уведомления указанному пользователю. |
> | Действие | Microsoft.ApiManagement/service/notifications/read | Перечисляет коллекцию свойств, определенных в экземпляре службы. или получает сведения об уведомлении, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/notifications/recipientEmails/delete | Удаляет сообщения электронной почты из списка уведомлений. |
> | Действие | Microsoft.ApiManagement/service/notifications/recipientEmails/read | Получает список получателей уведомлений по электронной почте, которые подписаны на уведомления. |
> | Действие | Microsoft.ApiManagement/service/notifications/recipientEmails/write | Добавляет адрес электронной почты в список получателей уведомления. |
> | Действие | Microsoft.ApiManagement/service/notifications/recipientUsers/delete | Удаление пользователя службы управления API в списке уведомлений. |
> | Действие | Microsoft.ApiManagement/service/notifications/recipientUsers/read | Получает список пользователя получателя уведомлений подписаться на уведомления. |
> | Действие | Microsoft.ApiManagement/service/notifications/recipientUsers/write | Добавляет пользователя API управления в список получателей уведомления. |
> | Действие | Microsoft.ApiManagement/service/notifications/write | Создание или уведомления издателя управления API обновления. |
> | Действие | Microsoft.ApiManagement/service/openidConnectProviders/delete | Удаляет конкретного поставщика OpenID Connect экземпляра службы управления API. |
> | Действие | Microsoft.ApiManagement/service/openidConnectProviders/read | Содержит список всех поставщика OpenId Connect. или получает конкретного поставщика OpenID Connect. |
> | Действие | Microsoft.ApiManagement/service/openidConnectProviders/write | Создает или обновляет поставщик OpenID Connect. или обновления конкретного поставщика OpenID Connect. |
> | Действие | Microsoft.ApiManagement/service/operationresults/read | Возвращает текущее состояние длительной операции. |
> | Действие | Microsoft.ApiManagement/service/policies/delete | Удаляет конфигурацию глобальной политики службы управления Api. |
> | Действие | Microsoft.ApiManagement/service/policies/read | Список всех определений глобальной политики службы управления Api. или получения определения глобальной политики службы управления Api. |
> | Действие | Microsoft.ApiManagement/service/policies/write | Создает или обновляет конфигурацию глобальной политики службы управления Api. |
> | Действие | Microsoft.ApiManagement/service/policy/delete | Удалить конфигурацию политики на уровне клиента |
> | Действие | Microsoft.ApiManagement/service/policy/read | Возвращает конфигурацию политики на уровне клиента |
> | Действие | Microsoft.ApiManagement/service/policy/write | Создание уровня конфигурации политики в клиент |
> | Действие | Microsoft.ApiManagement/service/policySnippets/read | Список всех фрагментов кода политики. |
> | Действие | Microsoft.ApiManagement/service/portalsettings/read | Получите входа в параметры для портала или Get входа вверх параметры для портала или параметров делегирования для портала. |
> | Действие | Microsoft.ApiManagement/service/portalsettings/write | Обновите параметры входа в систему. или параметры Create или Update входа в систему. или параметров регистрации обновления или обновления регистрации параметры или параметров делегирования. или параметры Create или Update делегирования. |
> | Действие | Microsoft.ApiManagement/service/products/apis/delete | Удаляет указанный API из указанного продукта. |
> | Действие | Microsoft.ApiManagement/service/products/apis/read | Списки коллекция API-интерфейсов, связанных с продуктом. |
> | Действие | Microsoft.ApiManagement/service/products/apis/write | Добавляет API к указанному продукту. |
> | Действие | Microsoft.ApiManagement/service/products/delete | Удалите продукт. |
> | Действие | Microsoft.ApiManagement/service/products/groups/delete | Удаляет связь между указанной группой и продуктом. |
> | Действие | Microsoft.ApiManagement/service/products/groups/read | Перечисляет коллекцию групп разработчиков, связанных с указанным продуктом. |
> | Действие | Microsoft.ApiManagement/service/products/groups/write | Добавляет связь между указанную группу разработчиков с указанным продуктом. |
> | Действие | Microsoft.ApiManagement/service/products/policies/delete | Удаляет конфигурацию политики на продукт. |
> | Действие | Microsoft.ApiManagement/service/products/policies/read | Возвращает конфигурацию политики на уровне продукта. или возвращает конфигурацию политики на уровне продукта. |
> | Действие | Microsoft.ApiManagement/service/products/policies/write | Создает или обновляет конфигурацию политики для продукта. |
> | Действие | Microsoft.ApiManagement/service/products/policy/delete | Удалить конфигурацию политики на уровне продукта |
> | Действие | Microsoft.ApiManagement/service/products/policy/read | Возвращает конфигурацию политики на уровне продукта |
> | Действие | Microsoft.ApiManagement/service/products/policy/write | Создание политики конфигурации продукт уровня |
> | Действие | Microsoft.ApiManagement/service/products/read | Перечисляет коллекцию продуктов в указанном экземпляре службы. или получение сведений о продукте, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/products/subscriptions/read | Перечисляет коллекцию подписок на указанный продукт. |
> | Действие | Microsoft.ApiManagement/service/products/tags/delete | Отсоединение тега из продукта. |
> | Действие | Microsoft.ApiManagement/service/products/tags/read | Перечислены все теги, связанные с продуктом. или тег, связанный с продуктом. |
> | Действие | Microsoft.ApiManagement/service/products/tags/write | Назначьте тег продукта. |
> | Действие | Microsoft.ApiManagement/service/products/write | Создает или обновляет продукта. или обновляет данные существующего. |
> | Действие | Microsoft.ApiManagement/service/productsByTags/read | Перечисляет коллекцию продуктов, связанных с тегами. |
> | Действие | Microsoft.ApiManagement/service/properties/delete | Удаляет указанное свойство из экземпляра службы управления API. |
> | Действие | Microsoft.ApiManagement/service/properties/read | Перечисляет коллекцию свойств, определенных в экземпляре службы. или получает сведения о свойстве, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/properties/write | Создает или обновляет свойства. или обновляет определенные свойства. |
> | Действие | Microsoft.ApiManagement/service/quotas/periods/read | Получение значения счетчика квоты за период. |
> | Действие | Microsoft.ApiManagement/service/quotas/periods/write | Настройка текущего значения счетчика квоты. |
> | Действие | Microsoft.ApiManagement/service/quotas/read | Получение значений квоты. |
> | Действие | Microsoft.ApiManagement/service/quotas/write | Настройка текущего значения счетчика квоты. |
> | Действие | Microsoft.ApiManagement/service/read | Чтение метаданных для экземпляра службы управления API. |
> | Действие | Microsoft.ApiManagement/service/regions/read | Список всех регионах azure, в которых служба существует. |
> | Действие | Microsoft.ApiManagement/service/reports/read | Получение отчета, объединенного по периодам времени, по географическим регионам или по разработчикам;<br>получение отчета, объединенного по продуктам;<br>получение отчета, объединенного по API-интерфейсам, операциям или по подпискам;<br>получение запросов, передающих данные. |
> | Действие | Microsoft.ApiManagement/service/restore/action | Восстановление службы управления API из указанного контейнера в предоставленной пользователем учетной записи хранения. |
> | Действие | Microsoft.ApiManagement/service/subscriptions/delete | Удаляет указанную подписку. |
> | Действие | Microsoft.ApiManagement/service/subscriptions/read | Перечислены все подписки экземпляра службы управления API. Получает указанную сущность подписки или. |
> | Действие | Microsoft.ApiManagement/service/subscriptions/regeneratePrimaryKey/action | Повторно создает первичный ключ из существующей подписки экземпляра службы управления API. |
> | Действие | Microsoft.ApiManagement/service/subscriptions/regenerateSecondaryKey/action | Повторно создает вторичный ключ из существующей подписки экземпляра службы управления API. |
> | Действие | Microsoft.ApiManagement/service/subscriptions/write | Создание или обновление подписки указанного пользователя на указанный продукт. или обновления сведений о подписке, указанной ее идентификатором. |
> | Действие | Microsoft.ApiManagement/service/tagResources/read | Перечисляет коллекцию ресурсов, связанных с тегами. |
> | Действие | Microsoft.ApiManagement/service/tags/delete | Удаляет указанный тег экземпляра службы управления API. |
> | Действие | Microsoft.ApiManagement/service/tags/read | Перечисляет коллекцию тегов в указанном экземпляре службы. или получает сведения о теге, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/tags/write | Создает тег. или обновления сведений о теге, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/templates/delete | Сброс к шаблону электронного сообщения по умолчанию службы управления API. |
> | Действие | Microsoft.ApiManagement/service/templates/read | Получение всех шаблонов сообщений или сведений о шаблоне сообщения службы управления API. |
> | Действие | Microsoft.ApiManagement/service/templates/write | Создание или обновление шаблона сообщения службы управления API. |
> | Действие | Microsoft.ApiManagement/service/tenant/delete | Удаляет конфигурацию политики для клиента. |
> | Действие | Microsoft.ApiManagement/service/tenant/deploy/action | Выполняет задачу развертывания для применения изменений из указанной ветви Git к конфигурации в базе данных. |
> | Действие | Microsoft.ApiManagement/service/tenant/operationResults/read | Возвращает список результатов операций или результат определенной операции. |
> | Действие | Microsoft.ApiManagement/service/tenant/read | Получение определения глобальной политики службы управления Api. или сведения о доступе клиента Get |
> | Действие | Microsoft.ApiManagement/service/tenant/regeneratePrimaryKey/action | Повторно создает первичный ключ доступа. |
> | Действие | Microsoft.ApiManagement/service/tenant/regenerateSecondaryKey/action | Повторно создает вторичный ключ доступа. |
> | Действие | Microsoft.ApiManagement/service/tenant/save/action | Создает фиксацию с использованием моментального снимка конфигурации для указанной ветви в репозитории. |
> | Действие | Microsoft.ApiManagement/service/tenant/syncState/read | Возвращает состояние последней синхронизации с Git. |
> | Действие | Microsoft.ApiManagement/service/tenant/validate/action | Проверяет изменения из указанной ветви Git. |
> | Действие | Microsoft.ApiManagement/service/tenant/write | Установка конфигурации политики для клиента или обновление сведений о доступе к клиенту. |
> | Действие | Microsoft.ApiManagement/service/updatecertificate/action | Передает SSL-сертификата для службы управления API. |
> | Действие | Microsoft.ApiManagement/service/updatehostname/action | Позволяет настроить, обновить или удалить имена личных доменов для службы управления API. |
> | Действие | Microsoft.ApiManagement/service/users/action | Регистрация нового пользователя. |
> | Действие | Microsoft.ApiManagement/service/users/confirmations/send/action | Отправляет подтверждение. |
> | Действие | Microsoft.ApiManagement/service/users/delete | Удаляет конкретного пользователя. |
> | Действие | Microsoft.ApiManagement/service/users/generateSsoUrl/action | Извлекает URL-адрес перенаправления, содержащий маркер проверки подлинности для входа данного пользователя на портал разработчика. |
> | Действие | Microsoft.ApiManagement/service/users/groups/read | Список всех групп пользователей. |
> | Действие | Microsoft.ApiManagement/service/users/identities/read | Список всех удостоверений пользователей. |
> | Действие | Microsoft.ApiManagement/service/users/keys/read | Получить ключи, связанные с пользователем |
> | Действие | Microsoft.ApiManagement/service/users/read | Перечисляет коллекцию пользователей, зарегистрированных в указанном экземпляре службы. или получает данные пользователя, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/users/subscriptions/read | Перечисляет коллекцию подписки указанного пользователя. |
> | Действие | Microsoft.ApiManagement/service/users/token/action | Получает маркер авторизации общего доступа для пользователя. |
> | Действие | Microsoft.ApiManagement/service/users/write | Создает или обновляет пользователя. или обновления данных пользователя, указанного по его идентификатору. |
> | Действие | Microsoft.ApiManagement/service/write | Создание экземпляра службы управления API. |
> | Действие | Microsoft.ApiManagement/unregister/action | Отменяет регистрацию подписки для поставщика ресурсов Microsoft.ApiManagement. |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Authorization/classicAdministrators/delete | Удаляет учетные данные администратора из подписки. |
> | Действие | Microsoft.Authorization/classicAdministrators/operationstatuses/read | Получает сведения о состоянии операции администратора подписки. |
> | Действие | Microsoft.Authorization/classicAdministrators/read | Считывает администраторов для подписки. |
> | Действие | Microsoft.Authorization/classicAdministrators/write | Добавляет или изменяет администратора подписки. |
> | Действие | Microsoft.Authorization/denyAssignments/delete | Удаление запрета назначения в указанной области. |
> | Действие | Microsoft.Authorization/denyAssignments/read | Получение сведений о запрете назначения. |
> | Действие | Microsoft.Authorization/denyAssignments/write | Создание запрета назначения в указанной области. |
> | Действие | Microsoft.Authorization/elevateAccess/action | Предоставляет вызывающему доступ с правами администратора для области действия клиента. |
> | Действие | Microsoft.Authorization/locks/delete | Удаляет блокировки в указанной области. |
> | Действие | Microsoft.Authorization/locks/read | Возвращает блокировки в указанной области. |
> | Действие | Microsoft.Authorization/locks/write | Добавляет блокировки в указанной области. |
> | Действие | Microsoft.Authorization/operations/read | Получает список операций. |
> | Действие | Microsoft.Authorization/permissions/read | Выводит список всех разрешений, которые есть у вызывающего в заданной области. |
> | Действие | Microsoft.Authorization/policyAssignments/delete | Удаляет назначение политики в указанной области. |
> | Действие | Microsoft.Authorization/policyAssignments/read | Возвращает сведения о назначении политики. |
> | Действие | Microsoft.Authorization/policyAssignments/write | Создает назначение политики в указанной области. |
> | Действие | Microsoft.Authorization/policyDefinitions/delete | Удаляет определение политики. |
> | Действие | Microsoft.Authorization/policyDefinitions/read | Возвращает сведения об определении политики. |
> | Действие | Microsoft.Authorization/policyDefinitions/write | Создает определение настраиваемой политики. |
> | Действие | Microsoft.Authorization/policySetDefinitions/delete | Удаление определения набора политик. |
> | Действие | Microsoft.Authorization/policySetDefinitions/read | Получение сведений об определении набора политик. |
> | Действие | Microsoft.Authorization/policySetDefinitions/write | Создание пользовательского определения набора политик. |
> | Действие | Microsoft.Authorization/providerOperations/read | Возвращает операции для всех поставщиков ресурсов, которые могут быть использованы в определениях ролей. |
> | Действие | Microsoft.Authorization/roleAssignments/delete | Здесь описывается удаление назначения роли в указанной области. |
> | Действие | Microsoft.Authorization/roleAssignments/read | Возвращает сведения о назначении роли. |
> | Действие | Microsoft.Authorization/roleAssignments/write. | Создает назначение роли в указанной области. |
> | Действие | Microsoft.Authorization/roleDefinitions/delete | Удаляет указанное определение настраиваемой роли. |
> | Действие | Microsoft.Authorization/roleDefinitions/read | Возвращает сведения об определении роли. |
> | Действие | Microsoft.Authorization/roleDefinitions/write | Создает или обновляет определение настраиваемой роли с помощью указанных разрешений и назначаемых областей. |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Automation/automationAccounts/agentRegistrationInformation/read | Чтение сведений о регистрации Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/agentRegistrationInformation/regenerateKey/action | Запись запроса на повторное создание ключей Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/certificates/delete | Удаляет ресурс сертификата службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/certificates/getCount/action | Считывает число сертификатов. |
> | Действие | Microsoft.Automation/automationAccounts/certificates/read | Возвращает ресурс сертификата службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/certificates/write | Создает или обновляет ресурс сертификата службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/compilationjobs/read | Чтение содержимого компиляции Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/compilationjobs/read | Чтение содержимого компиляции Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/compilationjobs/write | Запись содержимого компиляции Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/compilationjobs/write | Запись содержимого компиляции Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/configurations/content/read | Считывает мультимедийное содержимое конфигурации. |
> | Действие | Microsoft.Automation/automationAccounts/configurations/delete | Удаление содержимого Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/configurations/getCount/action | Чтение количества содержимого Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/configurations/read | Возвращает содержимое Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/configurations/write | Запись содержимого Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/connections/delete | Удаляет ресурс подключения службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/connections/getCount/action | Считывает число подключений. |
> | Действие | Microsoft.Automation/automationAccounts/connections/read | Возвращает ресурс подключения службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/connections/write | Создает или обновляет ресурс подключения службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/connectionTypes/delete | Удаляет ресурс типа подключения службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/connectionTypes/read | Возвращает ресурс типа подключения службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/connectionTypes/write | Создает ресурс типа подключения службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/credentials/delete | Удаляет ресурс учетных данных службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/credentials/getCount/action | Считывает количество учетных данных. |
> | Действие | Microsoft.Automation/automationAccounts/credentials/read | Возвращает ресурс учетных данных службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/credentials/write | Создает или обновляет ресурс учетных данных службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/delete | Удаляет учетную запись службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/delete | Удаляет ресурсы гибридной рабочей роли Runbook. |
> | Действие | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Считывает ресурсы гибридной рабочей роли Runbook. |
> | Действие | Microsoft.Automation/automationAccounts/jobs/output/read | Возвращает выходные данные задания. |
> | Действие | Microsoft.Automation/automationAccounts/jobs/read | Возвращает задание службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/jobs/resume/action | Возобновляет выполнение задания службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/jobs/runbookContent/action | Возвращает содержимое runbook службы автоматизации Azure во время выполнения задания. |
> | Действие | Microsoft.Automation/automationAccounts/jobs/stop/action | Останавливает задание службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/jobs/streams/read | Возвращает поток задания службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/jobs/streams/read | Возвращает поток задания службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/jobs/suspend/action | Приостанавливает задание службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/jobs/write | Создает задание службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/jobSchedules/delete | Удаляет расписание заданий службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/jobSchedules/read | Возвращает расписание заданий службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/jobSchedules/write | Создает расписание заданий службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/linkedWorkspace/read | Получает рабочую область, связанную с учетной записью службы автоматизации. |
> | Действие | Microsoft.Automation/automationAccounts/listKeys/action | Считывает ключи для учетной записи службы автоматизации. |
> | Действие | Microsoft.Automation/automationAccounts/modules/activities/read | Получение действий службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/modules/delete | Удаляет модуль PowerShell службы автоматизации Azure |
> | Действие | Microsoft.Automation/automationAccounts/modules/getCount/action | Возвращает число модулей PowerShell в рамках учетной записи службы автоматизации |
> | Действие | Microsoft.Automation/automationAccounts/modules/read | Получает модуль PowerShell службы автоматизации Azure |
> | Действие | Microsoft.Automation/automationAccounts/modules/write | Создает или обновляет модуль PowerShell службы автоматизации Azure |
> | Действие | Microsoft.Automation/automationAccounts/nodeConfigurations/delete | Удаление конфигурации узла Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/nodeConfigurations/rawContent/action | Чтение содержимого конфигурации узла Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/nodeConfigurations/read | Чтение конфигурации узла Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/nodeConfigurations/write | Запись конфигурации узла Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/nodecounts/read | Считывает общее число узлов указанного типа. |
> | Действие | Microsoft.Automation/automationAccounts/nodes/delete | Удаление узлов Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/nodes/read | Чтение узлов Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/nodes/reports/content/read | Считывает содержимое отчета Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/nodes/reports/read | Чтение отчетов Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/nodes/write | Создает или обновляет узлы Azure Automation DSC. |
> | Действие | Microsoft.Automation/automationAccounts/objectDataTypes/fields/read | Получение полей типов службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/python2Packages/delete | Удаляет пакет Python 2 службы автоматизации Azure |
> | Действие | Microsoft.Automation/automationAccounts/python2Packages/read | Получает пакет Python 2 службы автоматизации Azure |
> | Действие | Microsoft.Automation/automationAccounts/python2Packages/write | Создает или обновляет пакет Python 2 службы автоматизации Azure |
> | Действие | Microsoft.Automation/automationAccounts/python3Packages/delete | Удаляет пакет Python 3 службы автоматизации Azure |
> | Действие | Microsoft.Automation/automationAccounts/python3Packages/read | Получает пакет Python 3 службы автоматизации Azure |
> | Действие | Microsoft.Automation/automationAccounts/python3Packages/write | Создает или обновляет пакет Python 3 службы автоматизации Azure |
> | Действие | Microsoft.Automation/automationAccounts/read | Возвращает учетную запись службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/runbooks/content/read | Возвращает содержимое runbook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/runbooks/delete | Удаляет runbook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/runbooks/draft/content/write | Создает содержимое черновика runbook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/runbooks/draft/operationResults/read | Возвращает результаты операции черновика runbook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/runbooks/draft/read | Возвращает черновик runbook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/read | Возвращает тестовое задание черновика runbook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/resume/action | Возобновляет выполнение тестового задания черновика runbook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/stop/action | Останавливает тестовое задание черновика runbook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/suspend/action | Приостанавливает тестовое задание черновика runbook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/write | Создает тестовое задание черновика runbook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/runbooks/draft/undoEdit/action | Отменяет правки в черновике runbook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/runbooks/getCount/action | Возвращает число модулей runbook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/runbooks/publish/action | Публикует черновик runbook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/runbooks/read | Возвращает runbook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/runbooks/write | Создает или обновляет runbook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/schedules/delete | Удаляет ресурс расписания службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/schedules/getCount/action | Возвращает количество расписаний службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/schedules/read | Возвращает ресурс расписания службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/schedules/write | Создает или обновляет ресурс расписания службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/delete | Удаляет конфигурацию обновления программного обеспечения Azure Automation |
> | Действие | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/read | Получает конфигурации обновления программного обеспечения Azure Automation |
> | Действие | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/write | Создает или обновляет конфигурацию обновлений программного обеспечения Azure Automation |
> | Действие | Microsoft.Automation/automationAccounts/statistics/read | Получение статистики из службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/updateDeploymentMachineRuns/read | Получает компьютер для развертывания обновления службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/updateManagementPatchJob/read | Получает задание исправления управления обновлениями для службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/usages/read | Получение данных использования из службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/variables/delete | Удаляет ресурс переменной службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/variables/read | Считывает ресурс переменной службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/variables/write | Создает или обновляет ресурс переменной службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/watchers/delete | Удаляет задание наблюдателя из службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/watchers/read | Получает задание наблюдателя из службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/watchers/start/action | Запускает задание наблюдателя из службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/watchers/stop/action | Останавливает выполнение заданий наблюдателя из службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/watchers/streams/read | Получение потока заданий наблюдателя из службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/watchers/watcherActions/delete | Удаляет действия заданий наблюдателя из службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/watchers/watcherActions/read | Получает действия заданий наблюдателя из службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/watchers/watcherActions/write | Создает действия заданий наблюдателя из службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/watchers/write | Создает задание наблюдателя из службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/webhooks/action | Создает универсальный код ресурса (URI) для webhook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/webhooks/delete | Удаляет webhook службы автоматизации Azure.  |
> | Действие | Microsoft.Automation/automationAccounts/webhooks/read | Считывает runbook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/webhooks/write | Создает или обновляет webhook службы автоматизации Azure. |
> | Действие | Microsoft.Automation/automationAccounts/write | Создает или обновляет учетную запись службы автоматизации Azure. |
> | Действие | Microsoft.Automation/operations/read | Получает доступные операции для ресурсов службы автоматизации Azure. |
> | Действие | Microsoft.Automation/register/action | Регистрирует подписку в службе автоматизации Azure. |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.AzureActiveDirectory/b2cDirectories/delete | Удаление ресурса каталога B2C. |
> | Действие | Microsoft.AzureActiveDirectory/b2cDirectories/read | Просмотр ресурса каталога B2C. |
> | Действие | Microsoft.AzureActiveDirectory/b2cDirectories/write | Создание или обновление ресурса каталога B2C. |
> | Действие | Microsoft.AzureActiveDirectory/b2ctenants/read | Отображает список всех клиентов B2C, членом которых является пользователь. |
> | Действие | Microsoft.AzureActiveDirectory/operations/read | Чтение всех операций API, доступных для поставщика ресурсов Microsoft.AzureActiveDirectory. |
> | Действие | Microsoft.AzureActiveDirectory/register/action | Регистрация подписки в поставщике ресурсов Microsoft.AzureActiveDirectory. |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.AzureStack/Operations/read | Получение свойств операции поставщика ресурсов. |
> | Действие | Microsoft.AzureStack/register/action | Регистрация подписки в поставщике ресурсов Microsoft.AzureStack. |
> | Действие | Microsoft.AzureStack/registrations/customerSubscriptions/delete | Удаление подписки клиента Azure Stack. |
> | Действие | Microsoft.AzureStack/registrations/customerSubscriptions/read | Получение свойств подписки клиента Azure Stack. |
> | Действие | Microsoft.AzureStack/registrations/customerSubscriptions/write | Создание или обновление подписки клиента Azure Stack. |
> | Действие | Microsoft.AzureStack/registrations/delete | Удаление регистрации в Azure Stack. |
> | Действие | Microsoft.AzureStack/registrations/getActivationKey/action | Получение последнего созданного ключа активации Azure Stack. |
> | Действие | Microsoft.AzureStack/registrations/products/listDetails/action | Возвращает подробные сведения о продукте из Azure Stack Marketplace. |
> | Действие | Microsoft.AzureStack/registrations/products/read | Получает свойства продукта из Azure Stack Marketplace. |
> | Действие | Microsoft.AzureStack/registrations/read | Получает свойства регистрации Azure Stack. |
> | Действие | Microsoft.AzureStack/registrations/write | Создание или обновление регистрации в Azure Stack. |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Batch/batchAccounts/applications/delete | Удаляет приложение. |
> | Действие | Microsoft.Batch/batchAccounts/applications/read | Выводит список приложений или возвращает свойства приложения. |
> | Действие | Microsoft.Batch/batchAccounts/applications/versions/activate/action | Активирует пакет приложения. |
> | Действие | Microsoft.Batch/batchAccounts/applications/versions/delete | Удаляет пакет приложения. |
> | Действие | Microsoft.Batch/batchAccounts/applications/versions/read | Возвращает свойства пакета приложения. |
> | Действие | Microsoft.Batch/batchAccounts/applications/versions/write | Создает новый пакет приложения или обновляет существующий. |
> | Действие | Microsoft.Batch/batchAccounts/applications/write | Создает новое приложение или обновляет существующее. |
> | Действие | Microsoft.Batch/batchAccounts/certificateOperationResults/read | Получение результатов длительных операций с сертификатом в учетной записи пакетной службы. |
> | Действие | Microsoft.Batch/batchAccounts/certificates/cancelDelete/action | Отмена неудачного удаления сертификата в учетной записи пакетной службы. |
> | Действие | Microsoft.Batch/batchAccounts/certificates/delete | Удаление сертификата из учетной записи пакетной службы. |
> | Действие | Microsoft.Batch/batchAccounts/certificates/read | Вывод списка сертификатов в учетной записи пакетной службы или получение свойств сертификата. |
> | Действие | Microsoft.Batch/batchAccounts/certificates/write | Создание нового сертификата в учетной записи пакетной службы или обновление существующего сертификата. |
> | Действие | Microsoft.Batch/batchAccounts/delete | Удаляет учетную запись пакетной службы. |
> | Действие | Microsoft.Batch/batchAccounts/listkeys/action | Отображает ключи доступа для учетной записи пакетной службы. |
> | Действие | Microsoft.Batch/batchAccounts/operationResults/read | Получение результатов длительных операций в учетной записи пакетной службы. |
> | Действие | Microsoft.Batch/batchAccounts/poolOperationResults/read | Получение результатов длительных операций с пулом в учетной записи пакетной службы. |
> | Действие | Microsoft.Batch/batchAccounts/pools/delete | Удаление пула из учетной записи пакетной службы. |
> | Действие | Microsoft.Batch/batchAccounts/pools/disableAutoscale/action | Отключение автоматического масштабирования в пуле учетной записи пакетной службы. |
> | Действие | Microsoft.Batch/batchAccounts/pools/read | Вывод списка пулов в учетной записи пакетной службы или свойств пула. |
> | Действие | Microsoft.Batch/batchAccounts/pools/stopResize/action | Остановка текущей операции изменения размера в пуле учетной записи пакетной службы. |
> | Действие | Microsoft.Batch/batchAccounts/pools/write | Создание нового пула в учетной записи пакетной службы или обновление существующего пула. |
> | Действие | Microsoft.Batch/batchAccounts/read | Выводит список учетных записей пакетной службы или возвращает свойства учетной записи пакетной службы. |
> | Действие | Microsoft.Batch/batchAccounts/regeneratekeys/action | Повторно создает ключи доступа для учетной записи пакетной службы. |
> | Действие | Microsoft.Batch/batchAccounts/syncAutoStorageKeys/action | Синхронизирует ключи доступа для автоматической учетной записи хранения, настроенной для учетной записи пакетной службы. |
> | Действие | Microsoft.Batch/batchAccounts/write | Создает новую учетную запись пакетной службы или обновляет существующую. |
> | Действие | Microsoft.Batch/locations/accountOperationResults/read | Получение результатов длительных операций в учетной записи пакетной службы. |
> | Действие | Microsoft.Batch/locations/checkNameAvailability/action | Проверка того, является ли имя учетной записи допустимым и неиспользуемым. |
> | Действие | Microsoft.Batch/locations/quotas/read | Возвращает квоты пакетной службы для указанной подписки в заданном регионе Azure. |
> | Действие | Microsoft.Batch/operations/read | Перечисляет операции, доступные в поставщике ресурсов Microsoft.Batch. |
> | Действие | Microsoft.Batch/register/action | Регистрирует подписку для поставщика ресурсов пакетной службы и позволяет создавать учетные записи пакетной службы. |
> | Действие | Microsoft.Batch/unregister/action | Отмена регистрации подписки для поставщика ресурсов пакетной службы, которая не позволяет создавать учетные записи пакетной службы. |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Billing/billingAccounts/departments/read | Вывод списка всех отделов, относящихся к учетной записи выставления счетов. |
> | Действие | Microsoft.Billing/billingAccounts/enrollmentAccounts/read | Вывод списка всех учетных записей регистрации, относящихся к учетной записи выставления счетов. |
> | Действие | Microsoft.Billing/billingAccounts/read | Вывод списка всех учетных записей выставления счетов, к которым пользователь имеет доступ. |
> | Действие | Microsoft.Billing/billingPeriods/read | Вывод списка доступных интервалов выставления счетов. |
> | Действие | Microsoft.Billing/departments/read | Вывод списка всех отделов, к которым пользователь имеет доступ. |
> | Действие | Microsoft.Billing/invoices/read | Выводит список доступных счетов. |
> | Действие | Microsoft.Billing/register/action | Регистрация подписки в поставщике ресурсов Microsoft.Billing. |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.BingMaps/mapApis/Delete | Операция удаления. |
> | Действие | Microsoft.BingMaps/mapApis/listSecrets/action | Отображает секреты. |
> | Действие | Microsoft.BingMaps/mapApis/listSingleSignOnToken/action | Считывает маркер авторизации единого входа для ресурса. |
> | Действие | Microsoft.BingMaps/mapApis/Read | Операция чтения. |
> | Действие | Microsoft.BingMaps/mapApis/regenerateKey/action | Повторно создает ключ. |
> | Действие | Microsoft.BingMaps/mapApis/Write | Операции записи. |
> | Действие | Microsoft.BingMaps/Operations/read | Описание операции. |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Blockchain/blockchainMembers/delete | Удаляет существующий член Блокчейна. |
> | Действие | Microsoft.Blockchain/blockchainMembers/read | Получает или перечисляет существующие элементы Блокчейна. |
> | DataAction | Microsoft.Blockchain/blockchainMembers/transactionNodes/connect/action | Подключается к узлу транзакции Блокчейн член. |
> | Действие | Microsoft.Blockchain/blockchainMembers/transactionNodes/delete | Удаляет существующий узел транзакции Блокчейн член. |
> | Действие | Microsoft.Blockchain/blockchainMembers/transactionNodes/read | Получает или перечисляет существующие узлы транзакции член Блокчейна. |
> | Действие | Microsoft.Blockchain/blockchainMembers/transactionNodes/write | Создает или обновляет узел члена Блокчейн транзакции. |
> | Действие | Microsoft.Blockchain/blockchainMembers/write | Создает или обновляет элемент Блокчейна. |
> | Действие | Microsoft.Blockchain/locations/blockchainMemberOperationResults/read | Возвращает результаты операции Блокчейн членов. |
> | Действие | Microsoft.Blockchain/locations/checkNameAvailability/action | Проверяет имя ресурса является допустимым и не используется. |
> | Действие | Microsoft.Blockchain/operations/read | Вывод списка всех операций в поставщике ресурсов Microsoft Блокчейна. |
> | Действие | Microsoft.Blockchain/register/action | Регистрирует подписку для поставщика ресурсов Блокчейна. |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Blueprint/blueprintAssignments/assignmentOperations/read | Чтение артефактов схемы. |
> | Действие | Microsoft.Blueprint/blueprintAssignments/delete | Удаление любых артефактов схемы. |
> | Действие | Microsoft.Blueprint/blueprintAssignments/read | Чтение артефактов схемы. |
> | Действие | Microsoft.Blueprint/blueprintAssignments/whoisblueprint/action | Получите идентификатор объекта субъекта-службы Azure Blueprints. |
> | Действие | Microsoft.Blueprint/blueprintAssignments/write | Создание или изменение любых артефактов схемы. |
> | Действие | Microsoft.Blueprint/blueprints/artifacts/delete | Удаление любых артефактов схемы. |
> | Действие | Microsoft.Blueprint/blueprints/artifacts/read | Чтение артефактов схемы. |
> | Действие | Microsoft.Blueprint/blueprints/artifacts/write | Создание или изменение любых артефактов схемы. |
> | Действие | Microsoft.Blueprint/blueprints/delete | Удаление схем. |
> | Действие | Microsoft.Blueprint/blueprints/read | Чтение схем. |
> | Действие | Microsoft.Blueprint/blueprints/versions/artifacts/read | Чтение артефактов схемы. |
> | Действие | Microsoft.Blueprint/blueprints/versions/delete | Удаление схем. |
> | Действие | Microsoft.Blueprint/blueprints/versions/read | Чтение схем. |
> | Действие | Microsoft.Blueprint/blueprints/versions/write | Создание или обновление схем. |
> | Действие | Microsoft.Blueprint/blueprints/write | Создание или обновление схем. |
> | Действие | Microsoft.Blueprint/register/action | Регистрация поставщика ресурсов схемы Azure. |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.BotService/botServices/channels/delete | Удаление службы бота. |
> | Действие | Microsoft.BotService/botServices/channels/read | Чтение службы бота. |
> | Действие | Microsoft.BotService/botServices/channels/write | Запись службы бота. |
> | Действие | Microsoft.BotService/botServices/connections/delete | Удаление службы бота. |
> | Действие | Microsoft.BotService/botServices/connections/read | Чтение службы бота. |
> | Действие | Microsoft.BotService/botServices/connections/write | Запись службы бота. |
> | Действие | Microsoft.BotService/botServices/delete | Удаление службы бота. |
> | Действие | Microsoft.BotService/botServices/read | Чтение службы бота. |
> | Действие | Microsoft.BotService/botServices/write | Запись службы бота. |
> | Действие | Microsoft.BotService/locations/operationresults/read | Чтение состояния асинхронной операции. |
> | Действие | Microsoft.BotService/Operations/read | Чтение операций для всех типов ресурсов. |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Cache/checknameavailability/action | Проверяет, доступно ли имя для нового кэша Redis. |
> | Действие | Microsoft.Cache/locations/operationresults/read | Получение результата длительных операций, для которых заголовок Location был ранее возвращен клиенту. |
> | Действие | Microsoft.Cache/operations/read | Вывод списка операций, которые поддерживает поставщик Microsoft.Cache. |
> | Действие | Microsoft.Cache/redis/delete | Удаляет весь кэш Redis. |
> | Действие | Microsoft.Cache/redis/export/action | Экспортирует данные Redis в большие двоичные объекты с префиксом в службе хранилища в указанном формате. |
> | Действие | Microsoft.Cache/redis/firewallRules/delete | Удаляет правила брандмауэра для IP-адресов для кэша Redis. |
> | Действие | Microsoft.Cache/redis/firewallRules/read | Возвращает правила брандмауэра для IP-адресов для кэша Redis. |
> | Действие | Microsoft.Cache/redis/firewallRules/write | Изменяет правила брандмауэра для IP-адресов для кэша Redis. |
> | Действие | Microsoft.Cache/redis/forceReboot/action | Принудительно перезапускает экземпляр кэша. При этом возможна потеря данных. |
> | Действие | Microsoft.Cache/redis/import/action | Импортирует в Redis данные в указанном формате из нескольких больших двоичных объектов. |
> | Действие | Microsoft.Cache/redis/linkedservers/delete | Удаляет связанный сервер из кэша Redis. |
> | Действие | Microsoft.Cache/redis/linkedservers/read | Возвращает связанные серверы для кэша Redis. |
> | Действие | Microsoft.Cache/redis/linkedservers/write | Добавляет связанный сервер для кэша Redis. |
> | Действие | Microsoft.Cache/redis/listKeys/action | Отображает значения ключей доступа к кэшу Redis на портале управления. |
> | Действие | Microsoft.Cache/redis/listUpgradeNotifications/read | Выводит список последних уведомлений об обновлениях для клиента кэша. |
> | Действие | Microsoft.Cache/redis/metricDefinitions/read | Возвращает доступные метрики для кэша Redis. |
> | Действие | Microsoft.Cache/redis/patchSchedules/delete | Удаляет расписание исправления для кэша Redis. |
> | Действие | Microsoft.Cache/redis/patchSchedules/read | Возвращает расписание исправления для кэша Redis. |
> | Действие | Microsoft.Cache/redis/patchSchedules/write | Изменяет расписание исправления для кэша Redis. |
> | Действие | Microsoft.Cache/redis/read | Отображает параметры и конфигурацию кэша Redis на портале управления. |
> | Действие | Microsoft.Cache/redis/regenerateKey/action | Изменяет значения ключей доступа к кэшу Redis на портале управления. |
> | Действие | Microsoft.Cache/redis/start/action | Запускает экземпляр кэша. |
> | Действие | Microsoft.Cache/redis/stop/action | Останавливает экземпляр кэша. |
> | Действие | Microsoft.Cache/redis/write | Изменяет параметры и конфигурацию кэша Redis на портале управления. |
> | Действие | Microsoft.Cache/register/action | Регистрирует поставщик ресурсов Microsoft.Cache для подписки. |
> | Действие | Microsoft.Cache/unregister/action | Отменяет регистрацию поставщика ресурсов Microsoft.Cache для подписки. |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Capacity/appliedreservations/read | Чтение всех резервирований. |
> | Действие | Microsoft.Capacity/calculateexchange/action | Вычисляет сумму exchange и цена новой покупки и возвращает политику ошибок. |
> | Действие | Microsoft.Capacity/calculateprice/action | Вычисление цены резервирования. |
> | Действие | Microsoft.Capacity/catalogs/read | Чтение каталога резервирования. |
> | Действие | Microsoft.Capacity/checkoffers/action | Проверка наличия предложений для подписок. |
> | Действие | Microsoft.Capacity/checkscopes/action | Проверка наличия подписок. |
> | Действие | Microsoft.Capacity/commercialreservationorders/read | Получение заказов на резервирование, созданных в любом клиенте. |
> | Действие | Microsoft.Capacity/exchange/action | Exchange резервирования. |
> | Действие | Microsoft.Capacity/operations/read | Чтение любой операции. |
> | Действие | Microsoft.Capacity/register/action | Регистрация поставщика ресурсов емкости и разрешение создания ресурсов емкости. |
> | Действие | Microsoft.Capacity/reservationorders/action | Обновление резервирования. |
> | Действие | Microsoft.Capacity/reservationorders/availablescopes/action | Поиск любой доступной области. |
> | Действие | Microsoft.Capacity/reservationorders/calculaterefund/action | Вычисляет сумму возмещения и цена покупки нового и возвращает политику ошибок. |
> | Действие | Microsoft.Capacity/reservationorders/delete | Удаление резервирования. |
> | Действие | Microsoft.Capacity/reservationorders/merge/action | Объединение любого резервирования. |
> | Действие | Microsoft.Capacity/reservationorders/read | Чтение всех резервирований. |
> | Действие | Microsoft.Capacity/reservationorders/reservations/action | Обновление резервирования. |
> | Действие | Microsoft.Capacity/reservationorders/reservations/delete | Удаление резервирования. |
> | Действие | Microsoft.Capacity/reservationorders/reservations/read | Чтение всех резервирований. |
> | Действие | Microsoft.Capacity/reservationorders/reservations/revisions/read | Чтение всех резервирований. |
> | Действие | Microsoft.Capacity/reservationorders/reservations/write | Создание резервирования. |
> | Действие | Microsoft.Capacity/reservationorders/return/action | Возвращение любого резервирования. |
> | Действие | Microsoft.Capacity/reservationorders/split/action | Разделение любого резервирования. |
> | Действие | Microsoft.Capacity/reservationorders/swap/action | Переключение любого резервирования. |
> | Действие | Microsoft.Capacity/reservationorders/write | Создание резервирования. |
> | Действие | Microsoft.Capacity/tenants/register/action | Регистрация любого клиента. |
> | Действие | Microsoft.Capacity/unregister/action | Отмена регистрации любого клиента. |
> | Действие | Microsoft.Capacity/validatereservationorder/action | Проверка любого резервирования. |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Cdn/CheckNameAvailability/action |  |
> | Действие | Microsoft.Cdn/CheckResourceUsage/action |  |
> | Действие | Microsoft.Cdn/edgenodes/delete |  |
> | Действие | Microsoft.Cdn/edgenodes/read |  |
> | Действие | Microsoft.Cdn/edgenodes/write |  |
> | Действие | Microsoft.Cdn/operationresults/delete |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/CheckResourceUsage/action |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/delete |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/endpointresults/CheckResourceUsage/action |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/delete |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/DisableCustomHttps/action |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/EnableCustomHttps/action |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/read |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/write |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/endpointresults/delete |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/endpointresults/Load/action |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/delete |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/read |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/write |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/endpointresults/Purge/action |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/endpointresults/read |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/endpointresults/Start/action |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/endpointresults/Stop/action |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/endpointresults/ValidateCustomDomain/action |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/endpointresults/write |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/GenerateSsoUri/action |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/GetSupportedOptimizationTypes/action |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/read |  |
> | Действие | Microsoft.Cdn/operationresults/profileresults/write |  |
> | Действие | Microsoft.Cdn/operationresults/read |  |
> | Действие | Microsoft.Cdn/operationresults/write |  |
> | Действие | Microsoft.Cdn/operations/read |  |
> | Действие | Microsoft.Cdn/profiles/CheckResourceUsage/action |  |
> | Действие | Microsoft.Cdn/profiles/delete |  |
> | Действие | Microsoft.Cdn/profiles/endpoints/CheckResourceUsage/action |  |
> | Действие | Microsoft.Cdn/profiles/endpoints/customdomains/delete |  |
> | Действие | Microsoft.Cdn/profiles/endpoints/customdomains/DisableCustomHttps/action |  |
> | Действие | Microsoft.Cdn/profiles/endpoints/customdomains/EnableCustomHttps/action |  |
> | Действие | Microsoft.Cdn/profiles/endpoints/customdomains/read |  |
> | Действие | Microsoft.Cdn/profiles/endpoints/customdomains/write |  |
> | Действие | Microsoft.Cdn/profiles/endpoints/delete |  |
> | Действие | Microsoft.Cdn/profiles/endpoints/Load/action |  |
> | Действие | Microsoft.Cdn/profiles/endpoints/origins/delete |  |
> | Действие | Microsoft.Cdn/profiles/endpoints/origins/read |  |
> | Действие | Microsoft.Cdn/profiles/endpoints/origins/write |  |
> | Действие | Microsoft.Cdn/profiles/endpoints/Purge/action |  |
> | Действие | Microsoft.Cdn/profiles/endpoints/read |  |
> | Действие | Microsoft.Cdn/profiles/endpoints/Start/action |  |
> | Действие | Microsoft.Cdn/profiles/endpoints/Stop/action |  |
> | Действие | Microsoft.Cdn/profiles/endpoints/ValidateCustomDomain/action |  |
> | Действие | Microsoft.Cdn/profiles/endpoints/write |  |
> | Действие | Microsoft.Cdn/profiles/GenerateSsoUri/action |  |
> | Действие | Microsoft.Cdn/profiles/GetSupportedOptimizationTypes/action |  |
> | Действие | Microsoft.Cdn/profiles/read |  |
> | Действие | Microsoft.Cdn/profiles/write |  |
> | Действие | Microsoft.Cdn/register/action | Регистрация подписки для поставщика ресурсов CDN и разрешение создания профилей CDN. |
> | Действие | Microsoft.Cdn/ValidateProbe/action |  |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.CertificateRegistration/certificateOrders/certificates/Delete | Удаляет существующий сертификат. |
> | Действие | Microsoft.CertificateRegistration/certificateOrders/certificates/Read | Возвращает список сертификатов. |
> | Действие | Microsoft.CertificateRegistration/certificateOrders/certificates/Write | Добавляет новый сертификат или обновляет существующий. |
> | Действие | Microsoft.CertificateRegistration/certificateOrders/Delete | Удаляет существующий сертификат службы приложений. |
> | Действие | Microsoft.CertificateRegistration/certificateOrders/Read | Возвращает список заказов сертификатов. |
> | Действие | Microsoft.CertificateRegistration/certificateOrders/reissue/Action | Повторно отправляет существующий заказ сертификата. |
> | Действие | Microsoft.CertificateRegistration/certificateOrders/renew/Action | Обновляет существующий заказ сертификата. |
> | Действие | Microsoft.CertificateRegistration/certificateOrders/resendEmail/Action | Повторно отправляет сообщение сертификата. |
> | Действие | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Повторно отправляет сообщения запроса на другой электронный адрес. |
> | Действие | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Извлекает печать сайта для выданного сертификата службы приложений. |
> | Действие | Microsoft.CertificateRegistration/certificateOrders/retrieveCertificateActions/Action | Извлекает список действий сертификата. |
> | Действие | Microsoft.CertificateRegistration/certificateOrders/retrieveEmailHistory/Action | Извлекает журнал электронных сообщений для сертификата. |
> | Действие | Microsoft.CertificateRegistration/certificateOrders/verifyDomainOwnership/Action | проверка принадлежности домена; |
> | Действие | Microsoft.CertificateRegistration/certificateOrders/Write | Добавляет новый заказ сертификата или обновляет существующий. |
> | Действие | Microsoft.CertificateRegistration/operations/Read | Вывод списка всех операций из регистрации сертификата службы приложений. |
> | Действие | Microsoft.CertificateRegistration/provisionGlobalAppServicePrincipalInUserTenant/Action | Подготавливает субъект-службу для субъекта приложения-службы. |
> | Действие | Microsoft.CertificateRegistration/register/action | Регистрирует поставщик сертификатов Майкрософт для подписки. |
> | Действие | Microsoft.CertificateRegistration/validateCertificateRegistrationInformation/Action | Проверяет объект покупки сертификата без его отправки. |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.ClassicCompute/capabilities/read | Отображает возможности. |
> | Действие | Microsoft.ClassicCompute/checkDomainNameAvailability/action | Проверяет доступность заданного доменного имени. |
> | Действие | Microsoft.ClassicCompute/checkDomainNameAvailability/read | Получает сведения о доступности указанного доменного имени. |
> | Действие | Microsoft.ClassicCompute/domainNames/active/write | Задает активное доменное имя. |
> | Действие | Microsoft.ClassicCompute/domainNames/availabilitySets/read | Отображает группу доступности для ресурса. |
> | Действие | Microsoft.ClassicCompute/domainNames/capabilities/read | Отображает возможности доменного имени. |
> | Действие | Microsoft.ClassicCompute/domainNames/delete | Удаляет доменные имена для ресурсов. |
> | Действие | Microsoft.ClassicCompute/domainNames/deploymentslots/read | Отображает слоты развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/deploymentslots/roles/read | Получение роли в слоте развертывания доменного имени |
> | Действие | Microsoft.ClassicCompute/domainNames/deploymentslots/roles/roleinstances/read | Получение экземпляра роли в слоте развертывания доменного имени |
> | Действие | Microsoft.ClassicCompute/domainNames/deploymentslots/state/read | Получение состояния слота развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/deploymentslots/state/write | Добавление состояния слота развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/deploymentslots/upgradedomain/read | Получение домена обновления для слота развертывания доменного имени |
> | Действие | Microsoft.ClassicCompute/domainNames/deploymentslots/upgradedomain/write | Обновление домена обновления для слота развертывания доменного имени |
> | Действие | Microsoft.ClassicCompute/domainNames/deploymentslots/write | Создает или обновляет развертывание. |
> | Действие | Microsoft.ClassicCompute/domainNames/extensions/delete | Удаляет расширения доменных имен. |
> | Действие | Microsoft.ClassicCompute/domainNames/extensions/operationStatuses/read | Считывает состояние операции расширений доменных имен. |
> | Действие | Microsoft.ClassicCompute/domainNames/extensions/read | Возвращает расширения доменных имен. |
> | Действие | Microsoft.ClassicCompute/domainNames/extensions/write | Добавляет расширения доменных имен. |
> | Действие | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/delete | Удаляет внутреннюю подсистему балансировки нагрузки. |
> | Действие | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/operationStatuses/read | Считывает состояние операции внутренних подсистем балансировки нагрузки доменных имен. |
> | Действие | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/read | Возвращает внутренние подсистемы балансировки нагрузки. |
> | Действие | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/write | Создает внутреннюю подсистему балансировки нагрузки. |
> | Действие | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/operationStatuses/read | Считывает состояние операции наборов конечных точек с балансировкой нагрузки доменных имен. |
> | Действие | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/read | Получение наборов конечных точек с балансировкой нагрузки. |
> | Действие | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/write | Добавление наборов конечных точек с балансировкой нагрузки. |
> | Действие | Microsoft.ClassicCompute/domainNames/operationstatuses/read | Получение состояния операции доменного имени. |
> | Действие | Microsoft.ClassicCompute/domainNames/operationStatuses/read | Считывает состояние операции расширений доменных имен. |
> | Действие | Microsoft.ClassicCompute/domainNames/read | Возвращает доменные имена для ресурсов. |
> | Действие | Microsoft.ClassicCompute/domainNames/serviceCertificates/delete | Удаляет используемые сертификаты службы. |
> | Действие | Microsoft.ClassicCompute/domainNames/serviceCertificates/operationStatuses/read | Считывает состояние операции сертификатов службы доменных имен. |
> | Действие | Microsoft.ClassicCompute/domainNames/serviceCertificates/read | Возвращает используемые сертификаты службы. |
> | Действие | Microsoft.ClassicCompute/domainNames/serviceCertificates/write | Добавляет или изменяет используемые сертификаты службы. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/abortMigration/action | Прерывает миграцию слота развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/commitMigration/action | Фиксирует миграцию слота развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/delete | Удаляет заданный слот развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/operationStatuses/read | Считывает состояние операции слотов доменных имен. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/prepareMigration/action | Подготавливает миграцию слота развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/read | Отображает слоты развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/delete | Удаляет ссылку на расширение для роли слота развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/operationStatuses/read | Считывает состояние операции ссылок на расширение ролей слотов доменных имен. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/read | Возвращает ссылку на расширение для роли слота развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/write | Добавляет или изменяет ссылку на расширение для роли слота развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/metricdefinitions/read | Получение определения метрики роли для доменного имени. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/metrics/read | Получение метрики роли для доменного имени. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/operationstatuses/read | Получение состояния операции для роли слота доменных имен. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/read | Возвращает параметры диагностики. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/write | Добавляет или изменяет параметры диагностики. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/metricDefinitions/read | Возвращает определения метрик. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/read | Возвращает роль для слота развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/downloadremotedesktopconnectionfile/action | Скачивает файл подключения к удаленному рабочему столу для экземпляра роли в роли слота доменных имен. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/operationStatuses/read | Получает состояние операции для экземпляра роли в роли слота доменных имен. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/read | Возвращает экземпляр роли. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/rebuild/action | Повторная сборка экземпляра роли. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/reimage/action | Пересоздает образ экземпляра роли. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/restart/action | Перезапускает экземпляры роли. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/skus/read | Получение номера SKU роли для слота развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/roles/write | Добавление роли для слота развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/start/action | Запускает слот развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/state/start/write | Задает состояние остановки для слота развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/state/stop/write | Задает состояние запуска для слота развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/stop/action | Приостанавливает слот развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/upgradeDomain/write | Обновляет домен. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/validateMigration/action | Проверяет миграцию слота развертывания. |
> | Действие | Microsoft.ClassicCompute/domainNames/slots/write | Создает или обновляет развертывание. |
> | Действие | Microsoft.ClassicCompute/domainNames/swap/action | Переключает промежуточный слот на рабочий слот. |
> | Действие | Microsoft.ClassicCompute/domainNames/write | Добавляет или изменяет доменные имена для ресурсов. |
> | Действие | Microsoft.ClassicCompute/moveSubscriptionResources/action | Перемещает все классические ресурсы в другую подписку. |
> | Действие | Microsoft.ClassicCompute/operatingSystemFamilies/read | Вывод списка семейств гостевых операционных систем, доступных в Microsoft Azure, а также версий операционных систем в каждом таком семействе. |
> | Действие | Microsoft.ClassicCompute/operatingSystems/read | Выводит список версий гостевой операционной системы, доступных в Microsoft Azure. |
> | Действие | Microsoft.ClassicCompute/operations/read | Получает список операций. |
> | Действие | Microsoft.ClassicCompute/operationStatuses/read | Считывает состояние операции ресурса. |
> | Действие | Microsoft.ClassicCompute/quotas/read | Возвращает квоту для подписки. |
> | Действие | Microsoft.ClassicCompute/register/action | Выполняет регистрацию в классической вычислительной среде. |
> | Действие | Microsoft.ClassicCompute/resourceTypes/skus/read | Возвращает список номеров SKU для поддерживаемых типов ресурсов. |
> | Действие | Microsoft.ClassicCompute/validateSubscriptionMoveAvailability/action | Проверяет доступность классической операции перемещения для подписки. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/delete | Удаляет группу безопасности сети, связанную с виртуальной машиной. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/read | Считывает состояние операции групп безопасности сети, связанных с виртуальными машинами. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/read | Возвращает группу безопасности сети, связанную с виртуальной машиной. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/write | Добавляет группу безопасности сети, связанную с виртуальной машиной. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/asyncOperations/read | Возвращает возможные асинхронные операции. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/attachDisk/action | Подключает диск данных к виртуальной машине. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/capture/action | Запись виртуальной машины. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/delete | Удаляет виртуальные машины. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/detachDisk/action | Отключает диск данных от виртуальной машины. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/diagnosticsettings/read | Получение параметров диагностики виртуальной машины. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/disks/read | Извлекает список дисков данных. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/downloadRemoteDesktopConnectionFile/action | Скачивает RDP-файл для виртуальной машины. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/extensions/operationStatuses/read | Считывает состояние операции расширений виртуальной машины. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/extensions/read | Возвращает расширение виртуальной машины. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/extensions/write | Размещает расширение виртуальной машины. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/metricdefinitions/read | Получение определения метрики виртуальной машины. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/metrics/read | Возвращает метрики. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/delete | Удаляет группу безопасности сети, связанную с сетевым интерфейсом. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/operationStatuses/read | Считывает состояние операции групп безопасности сети, связанных с виртуальными машинами. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/read | Возвращает группу безопасности сети, связанную с сетевым интерфейсом. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/write | Добавляет группу безопасности сети, связанную с сетевым интерфейсом. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/operationStatuses/read | Считывает состояние операции виртуальных машин. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/performMaintenance/action | Выполнение обслуживания виртуальной машины. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/read | Возвращает параметры диагностики. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/write | Добавляет или изменяет параметры диагностики. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Возвращает определения метрик. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/read | Извлекает список виртуальных машин. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/redeploy/action | Повторно развертывает виртуальную машину. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/restart/action | Перезапускает виртуальные машины. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/shutdown/action | Завершает работу виртуальной машины. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/start/action | Запустите виртуальную машину. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/stop/action | Останавливает виртуальную машину. |
> | Действие | Microsoft.ClassicCompute/virtualMachines/write | Добавляет или изменяет виртуальные машины. |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.ClassicNetwork/expressroutecrossconnections/operationstatuses/read | Получение состояния операции для перекрестных подключений ExpressRoute. |
> | Действие | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/delete | Удаление пиринга перекрестных подключений ExpressRoute. |
> | Действие | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/operationstatuses/read | Получение состояния операции для пиринга перекрестных подключений ExpressRoute. |
> | Действие | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/read | Получение пиринга перекрестных подключений ExpressRoute. |
> | Действие | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/write | Добавление пиринга перекрестных подключений ExpressRoute. |
> | Действие | Microsoft.ClassicNetwork/expressroutecrossconnections/read | Получение перекрестных подключений ExpressRoute. |
> | Действие | Microsoft.ClassicNetwork/expressroutecrossconnections/write | Добавление перекрестных подключений ExpressRoute. |
> | Действие | Microsoft.ClassicNetwork/gatewaySupportedDevices/read | Извлекает список поддерживаемых устройств. |
> | Действие | Microsoft.ClassicNetwork/networkSecurityGroups/delete | Удаляет группу безопасности сети. |
> | Действие | Microsoft.ClassicNetwork/networkSecurityGroups/operationStatuses/read | Считывает состояние операции группы безопасности сети. |
> | Действие | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметров диагностики групп безопасности сети. |
> | Действие | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/write | Создание или обновление параметров диагностики для групп безопасности сети. Эта операция осуществляется при задействовании поставщика ресурсов аналитических сведений. |
> | Действие | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/logDefinitions/read | Получение событий группы безопасности сети. |
> | Действие | Microsoft.ClassicNetwork/networkSecurityGroups/read | Возвращает группу безопасности сети. |
> | Действие | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/delete | Удаляет правило безопасности. |
> | Действие | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/operationStatuses/read | Считывает состояние операции правил безопасности группы безопасности сети. |
> | Действие | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/read | Возвращает правило безопасности. |
> | Действие | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/write | Добавляет или обновляет правило безопасности. |
> | Действие | Microsoft.ClassicNetwork/networkSecurityGroups/write | Добавляет новую группу безопасности сети. |
> | Действие | Microsoft.ClassicNetwork/operations/read | Получение операций классической сети. |
> | Действие | Microsoft.ClassicNetwork/quotas/read | Возвращает квоту для подписки. |
> | Действие | Microsoft.ClassicNetwork/register/action | Выполняет регистрацию в классической сети. |
> | Действие | Microsoft.ClassicNetwork/reservedIps/delete | Удаляет зарезервированный IP-адрес. |
> | Действие | Microsoft.ClassicNetwork/reservedIps/join/action | Присоединяет зарезервированный IP-адрес. |
> | Действие | Microsoft.ClassicNetwork/reservedIps/link/action | Связь зарезервированного IP-адреса |
> | Действие | Microsoft.ClassicNetwork/reservedIps/operationStatuses/read | Считывает состояние операции для зарезервированных IP-адресов. |
> | Действие | Microsoft.ClassicNetwork/reservedIps/read | Получает зарезервированные IP-адреса |
> | Действие | Microsoft.ClassicNetwork/reservedIps/write | Добавляет новый зарезервированный IP-адрес. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/abortMigration/action | Прерывает перенос виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/capabilities/read | Отображает возможности. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/checkIPAddressAvailability/action | Проверяет доступность заданного IP-адреса в виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/commitMigration/action | Фиксирует перенос виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/delete | Удаляет виртуальную сеть. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/delete | Отменяет отмену сертификата клиента. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/read | Считывает отозванные сертификаты клиента. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/write | Отменяет сертификат клиента. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/delete | Удаляет сертификат клиента шлюза виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/download/action | Скачивает сертификат по отпечатку. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/listPackage/action | Отображает пакет сертификата шлюза виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/read | Выполняет поиск корневых сертификатов клиента. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/write | Передает новый корневой сертификат клиента. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/connect/action | Устанавливает подключение шлюза типа "сеть — сеть". |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/disconnect/action | Отключает подключение шлюза типа "сеть — сеть". |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/read | Извлекает список подключений. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/test/action | Тестирует подключение шлюза типа "сеть — сеть". |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/delete | Удаляет шлюз виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/downloadDeviceConfigurationScript/action | Скачивает сценарий конфигурации устройства. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/downloadDiagnostics/action | Скачивает данные диагностики шлюза. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/listCircuitServiceKey/action | Извлекает ключ службы канала. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/listPackage/action | Отображает пакет шлюза виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/operationStatuses/read | Считывает состояние операции шлюзов виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/packages/read | Возвращает пакет шлюза виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/read | Возвращает шлюзы виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/startDiagnostics/action | Запускает диагностику шлюза виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/stopDiagnostics/action | Останавливает диагностику шлюза виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/gateways/write | Добавляет шлюз виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/join/action | Присоединение к виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/operationStatuses/read | Считывает состояние операции виртуальных сетей. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/peer/action | Создает пиринг между виртуальными сетями. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/prepareMigration/action | Подготавливает перенос виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/read | Получает виртуальную сеть. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/delete | Удаляет удаленный прокси-сервер пиринга виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/read | Получает удаленный прокси-сервер пиринга виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/write | Добавляет или обновляет удаленный прокси-сервер пиринга виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/delete | Удаляет группу безопасности сети, связанную с подсетью. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/operationStatuses/read | Считывает состояние операции группы безопасности сети, связанной с подсетью виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/read | Возвращает группу безопасности сети, связанную с подсетью. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/write | Добавляет группу безопасности сети, связанную с подсетью. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/validateMigration/action | Проверяет перенос виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/virtualNetworkPeerings/read | Получает пиринг виртуальной сети. |
> | Действие | Microsoft.ClassicNetwork/virtualNetworks/write | Добавляет новую виртуальную сеть. |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.ClassicStorage/capabilities/read | Отображает возможности. |
> | Действие | Microsoft.ClassicStorage/checkStorageAccountAvailability/action | Проверяет доступность учетной записи хранения. |
> | Действие | Microsoft.ClassicStorage/checkStorageAccountAvailability/read | Получает сведения о доступности учетной записи хранения. |
> | Действие | Microsoft.ClassicStorage/disks/read | Возврат диска учетной записи хранения. |
> | Действие | Microsoft.ClassicStorage/images/operationstatuses/read | Получает состояние операции создания образа. |
> | Действие | Microsoft.ClassicStorage/images/read | Возвращает образ. |
> | Действие | Microsoft.ClassicStorage/operations/read | Получает операции классического хранилища. |
> | Действие | Microsoft.ClassicStorage/osImages/read | Возвращает образ операционной системы. |
> | Действие | Microsoft.ClassicStorage/osPlatformImages/read | Получает образ платформы операционной системы. |
> | Действие | Microsoft.ClassicStorage/publicImages/read | Возвращает общедоступный образ виртуальной машины. |
> | Действие | Microsoft.ClassicStorage/quotas/read | Возвращает квоту для подписки. |
> | Действие | Microsoft.ClassicStorage/register/action | Выполняет регистрацию в классической службе хранилища. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/abortMigration/action | Прерывает перенос учетной записи хранения. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/commitMigration/action | Фиксирует перенос учетной записи хранения. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/delete | Удаляет учетную запись хранения. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/disks/delete | Удаляет заданный диск из учетной записи хранения. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/disks/operationStatuses/read | Считывает состояние операции ресурса. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/disks/read | Возврат диска учетной записи хранения. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/disks/write | Добавляет диск учетной записи хранения. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/images/delete | Удаляет заданный образ из учетной записи хранения. (Не рекомендуется к использованию. Следует использовать Microsoft.ClassicStorage/storageAccounts/vmImages.) |
> | Действие | Microsoft.ClassicStorage/storageAccounts/images/operationstatuses/read | Возвращает состояние операции создания образа учетной записи хранения. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/images/read | Возвращает образ в учетной записи хранения. (Не рекомендуется к использованию. Следует использовать Microsoft.ClassicStorage/storageAccounts/vmImages.) |
> | Действие | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Выводит список ключей доступа для учетных записей хранения. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/operationStatuses/read | Считывает состояние операции ресурса. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/osImages/delete | Удаляет заданный образ операционной системы из учетной записи хранения. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/osImages/read | Возвращает заданный образ операционной системы в учетной записи хранения. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/osImages/write | Добавляет образ операционной системы указанной учетной записи хранения. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/prepareMigration/action | Подготавливает перенос учетной записи хранения. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/read | Возвращение учетной записи хранения с заданной учетной записью. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/regenerateKey/action | Повторно создает ключи доступа для учетных записей хранения. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/read | Возвращает параметры диагностики. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/write | Добавляет или изменяет параметры диагностики. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/services/metricDefinitions/read | Возвращает определения метрик. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/services/metrics/read | Возвращает метрики. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/services/read | Возвращает доступные службы. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/validateMigration/action | Проверяет перенос учетной записи хранения. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/vmImages/delete | Удаляет указанный образ виртуальной машины. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/vmImages/operationstatuses/read | Получает состояние операции создания указанного образа виртуальной машины. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/vmImages/read | Возвращает образ виртуальной машины. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/vmImages/write | Добавляет указанный образ виртуальной машины. |
> | Действие | Microsoft.ClassicStorage/storageAccounts/write | Добавляет новую учетную запись хранения. |
> | Действие | Microsoft.ClassicStorage/vmImages/read | Выводит список образов виртуальных машин. |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.CognitiveServices/accounts/delete | Удаляет учетные записи API. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/detect/action | Обнаружения лиц на изображении, возвращаемое лицевым прямоугольникам и, при необходимости, с faceIds ориентиров и атрибуты. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/delete | Удаление лиц указанного списка. Связанные лица изображений в списке лиц будут удалены, слишком. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/persistedfaces/delete | Удалите фрагмент из списка лиц, указанный faceListId и persisitedFaceId. Связанные изображения будут удалены, слишком. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/persistedfaces/write | Добавьте фрагмент списка указанного лиц, не более 1000 лиц. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/read | Получить список лиц faceListId, имя, userData и лиц, в списке лиц. Список faceListId списки лиц, имя и userData. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/write | Создание списка пустой лиц с помощью пользовательской faceListId, имя и необязательное userData. До 64 лиц списки разрешены сведения об обновлении список лиц, включая имя и userData. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/findsimilars/action | Учитывая faceId лиц запроса, чтобы найти похожие лица из массива faceId, список лиц или большой лицевой. faceId |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/group/action | Разделите по сходству лиц кандидата лиц. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/identify/action | 1-ко многим код для поиска ближайшего поверхности конкретного запроса пользователя из группы пользователя или группы больших person. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/delete | Удаление указанного большого лиц списка. Связанные лица изображений в списке больших лиц будут удалены, слишком. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/persistedfaces/delete | Удалите фрагмент из списка больших лиц, указанный largeFaceListId и persisitedFaceId. Связанные изображения будут удалены, слишком. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/persistedfaces/read | Получить сохраненных лиц в списке больших лиц, largeFaceListId и persistedFaceId. Список persistedFaceId и userData в указанный большой лицевой список лиц. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/persistedfaces/write | Добавьте фрагмент к списку указанный большой лицевой до 1 000 000 лиц. Обновите поля userData указанный фрагмент в списке больших лиц, его persistedFaceId. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/read | Получите список больших лиц largeFaceListId "," имя "," userData. Список сведений списки большой лицевой largeFaceListId "," имя "и" userData. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/train/action | Отправьте задачей обучения списка больших лиц. Обучение является ключевым этапом, можно использовать только список обученной больших лиц. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/training/read | Чтобы проверить состояние обучение списка большой лицевой завершенные или по-прежнему постоянная. Обучение LargeFaceList является асинхронной операцией |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/write | Создание списка пустой большой лиц с помощью пользовательской largeFaceListId, имя и необязательное userData. Обновите информацию списка больших лиц, включая имя и userData. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/delete | Удаление существующей группы больших пользователя с указанным personGroupId. Материализованные данные в эту группу больших лица будут удалены. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/action | Создание нового пользователя в группе указанного больших лица. Чтобы добавить лицо этого человека, обратитесь в |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/delete | Удалите его из группы больших person. Все сохраненные данные пользователя и изображений лиц в записи пользователя будут удалены. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/persistedfaces/delete | Удалите фрагмент от лица в группе большого person. Данные лиц и изображения, связанные с этим элементом лиц будут также удалены. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/persistedfaces/read | Получить сведения о менеджере лиц. LargePersonGroupId, personId и persistedFaceId указывается лиц материализованных person. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/persistedfaces/write | Добавьте образ лиц человека в больших лица группу для подтверждения или идентификацию лиц. Для работы с изображением обновления человека сохраняются поле userData лиц по. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/read | Получить имя пользователя и userData и сохраненные faceIds, представляющий изображение лиц зарегистрированного пользователя. Просмотреть сведения о всех лиц в указанное лицо больших групп, включая personId "," имя "," userData "и" persistedFaceIds. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/write | Обновить имя или userData человека. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/read | Получите сведения о больших person группы, включая его имя и userData. Этот API возвращает большой лица группу сведения список всех существующих largePesonGroupId person больших групп, имя и userData. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/train/action | Отправьте задачу больших лица группу обучения. Обучение является ключевым этапом, можно использовать только группе обученной больших person. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/training/read | Для проверки больших лица группу обучения состояния завершенного или по-прежнему постоянная. Обучение LargePersonGroup является асинхронной операцией |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/write | Создайте группу больших person с largePersonGroupId определяемое пользователем имя и необязательное userData. Обновите имя существующей группы больших person и userData. Свойства оставьте без изменений, если они не в тексте запроса. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/delete | Удаление существующей группы пользователя с указанным personGroupId. Материализованные данные в эту группу пользователя будут удалены. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/action | Создание нового пользователя в группе указанному пользователю. Чтобы добавить лицо этого человека, обратитесь в |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/delete | Удалите его из группы пользователя. Все сохраненные данные пользователя и изображений лиц в записи пользователя будут удалены. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/persistedfaces/delete | Удалите фрагмент от лица пользователя группы. Данные лиц и изображения, связанные с этим элементом лиц будут также удалены. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/persistedfaces/read | Получить сведения о менеджере лиц. PersonGroupId, personId и persistedFaceId указывается лиц материализованных person. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/persistedfaces/write | Добавьте образ лиц в группу пользователя для подтверждения или идентификацию лиц человека. Для работы с изображением нескольких обновления человека сохраняются поле userData лиц. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/read | Получить имя пользователя и userData и сохраненные faceIds, представляющий изображение лиц зарегистрированного пользователя. Просмотреть сведения о всех лиц в группе указанному пользователю, включая personId "," имя "," userData "и" persistedFaceIds из регистрации. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/write | Обновить имя или userData человека. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/read | Чтобы получить имя группы и userData. Чтобы получить сведения о менеджере под этой каждой группе людей, используйте pesonGroupId person групп списка, имя и userData. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/train/action | Отправьте задачу обучение пользователя группы. Обучение является ключевым этапом, можно использовать только группе обученной person. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/training/read | Чтобы проверить person группы обучения состояние завершенного или по-прежнему постоянная. Каждой группе людей обучения является асинхронной операцией активации |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/write | Создайте группу пользователя с указанным personGroupId, именем и предоставляемое пользователем userData. Обновите имя и userData существующую группу пользователя. Свойства оставьте без изменений, если они не в тексте запроса. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/verify/action | Проверьте ли два портрета принадлежат одному человеку или от того, принадлежит ли одной грани пользователю. |
> | Действие | Microsoft.CognitiveServices/accounts/listKeys/action | Список ключей |
> | Действие | Microsoft.CognitiveServices/accounts/read | Считывает учетные записи API. |
> | Действие | Microsoft.CognitiveServices/accounts/regenerateKey/action | Повторно создает ключ. |
> | Действие | Microsoft.CognitiveServices/accounts/skus/read | Считывает доступные номера SKU для существующего ресурса. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/entities/action | API возвращает список известных сущностей и общие именованных сущностей (\"Person\", \"расположение\", \"организации\" и т.д.) в документы. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/keyphrases/action | API возвращает список строк с обозначением ключевых тем разговора во входном тексте. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/languages/action | API возвращает определенный язык и оценку в баллах между 0 и 1. Оценки, близкие к 1, указывают 100 % достоверность, что определенный язык имеет значение true. Поддерживается всего 120 языков. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/sentiment/action | API возвращает числовую оценку в диапазоне от 0 до 1.<br>Если оценка приближается к 1, то у текста позитивная тональность, а если к 0, то негативная.<br>Оценка 0,5 указывает отсутствие тональности (например)<br>оператор factoid). |
> | Действие | Microsoft.CognitiveServices/accounts/usages/read | Возвращает данные об использовании квоты для существующего ресурса. |
> | Действие | Microsoft.CognitiveServices/accounts/write | Записывает учетные записи API. |
> | Действие | Microsoft.CognitiveServices/checkDomainAvailability/action | Чтение доступных номеров SKU для подписки. |
> | Действие | Microsoft.CognitiveServices/locations/checkSkuAvailability/action | Чтение доступных номеров SKU для подписки. |
> | Действие | Microsoft.CognitiveServices/locations/deleteVirtualNetworkOrSubnets/action | Уведомление о из Microsoft.Network удалять рамках или подсети. |
> | Действие | Microsoft.CognitiveServices/Operations/read | Вывод списка всех доступных операций. |
> | Действие | Microsoft.CognitiveServices/register/action | Регистрация подписки для Cognitive Services. |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Commerce/RateCard/read | Возвращает данные предложения, метаданные ресурсов и единиц измерения и тарифы для заданной подписки. |
> | Действие | Microsoft.Commerce/UsageAggregates/read | Извлекает данные о потреблении ресурсов Microsoft Azure по подписке. Результат содержит статистические данные об использовании, а также сведения о подписке и ресурсах за определенный диапазон времени. |

## <a name="microsoftcompute"></a>Microsoft.Compute;

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Compute/availabilitySets/delete | Удаляет группу доступности. |
> | Действие | Microsoft.Compute/availabilitySets/read | Получение свойств группы доступности |
> | Действие | Microsoft.Compute/availabilitySets/vmSizes/read | Выводит список доступных размеров для создания или обновления виртуальной машины в группе доступности. |
> | Действие | Microsoft.Compute/availabilitySets/write | Создает новую группу доступности или обновляет существующую. |
> | Действие | Microsoft.Compute/disks/beginGetAccess/action | Возвращает универсальный код ресурса (URI) SAS диска для доступа к большому двоичному объекту. |
> | Действие | Microsoft.Compute/disks/delete | Удаляет диск. |
> | Действие | Microsoft.Compute/disks/endGetAccess/action | Отменяет универсальный код ресурса (URI) SAS диска. |
> | Действие | Microsoft.Compute/disks/read | Возвращает свойства диска. |
> | Действие | Microsoft.Compute/disks/write | Создает новый диск или обновляет существующий. |
> | Действие | Microsoft.Compute/galleries/delete | Удаляет коллекцию. |
> | Действие | Microsoft.Compute/galleries/images/delete | Удаляет образ из коллекции. |
> | Действие | Microsoft.Compute/galleries/images/read | Получает свойства образа коллекции. |
> | Действие | Microsoft.Compute/galleries/images/versions/delete | Удаляет версию образа коллекции. |
> | Действие | Microsoft.Compute/galleries/images/versions/read | Получает свойства версии образа коллекции. |
> | Действие | Microsoft.Compute/galleries/images/versions/write | Создает новую версию образа коллекции или обновляет существующую версию. |
> | Действие | Microsoft.Compute/galleries/images/write | Создает новый образ коллекции или обновляет существующий. |
> | Действие | Microsoft.Compute/galleries/read | Получает свойства коллекции. |
> | Действие | Microsoft.Compute/galleries/write | Создает новую коллекцию или обновляет существующую. |
> | Действие | Microsoft.Compute/hostGroups/delete | Удаляет группу узлов |
> | Действие | Microsoft.Compute/hostGroups/read | Получение свойств группы узлов |
> | Действие | Microsoft.Compute/hostGroups/write | Создает новую группу узлов или обновляет существующую группу узлов |
> | Действие | Microsoft.Compute/hosts/delete | Удаляет узел |
> | Действие | Microsoft.Compute/hosts/read | Получение свойств узла |
> | Действие | Microsoft.Compute/hosts/write | Создает новый узел или обновляет существующий узел |
> | Действие | Microsoft.Compute/images/delete | Удаляет кластер. |
> | Действие | Microsoft.Compute/images/read | Возвращает свойства образа. |
> | Действие | Microsoft.Compute/images/write | Создает новый образ или обновляет существующий. |
> | Действие | Microsoft.Compute/locations/capsOperations/read | Получение состояния асинхронной операции ограничения. |
> | Действие | Microsoft.Compute/locations/diskOperations/read | Получение состояния асинхронной операции диска. |
> | Действие | Microsoft.Compute/locations/logAnalytics/getRequestRateByInterval/action | Создание журналов, позволяющих показать общее количество запросов за интервал времени, чтобы помочь в диагностике регулирования. |
> | Действие | Microsoft.Compute/locations/logAnalytics/getThrottledRequests/action | Создание журналов, позволяющих показать вычисленное количество регулируемых запросов, сгруппированных по имени ресурса, имени операции или примененной политике регулирования. |
> | Действие | Microsoft.Compute/locations/operations/read | Возвращает состояние асинхронной операции. |
> | Действие | Microsoft.Compute/locations/publishers/artifacttypes/offers/read | Получение свойств предложения образа платформы. |
> | Действие | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/read | Получение свойств номера Sku образа платформы. |
> | Действие | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/versions/read | Получение свойств версии образа платформы. |
> | Действие | Microsoft.Compute/locations/publishers/artifacttypes/types/read | Получение свойств типа расширения виртуальной машины. |
> | Действие | Microsoft.Compute/locations/publishers/artifacttypes/types/versions/read | Получение свойств версии расширения виртуальной машины. |
> | Действие | Microsoft.Compute/locations/publishers/read | Получение свойств издателя. |
> | Действие | Microsoft.Compute/locations/runCommands/read | Вывод списка доступных команд запуска в расположении. |
> | Действие | Microsoft.Compute/locations/usages/read | Возвращает ограничения службы и текущее количество использованных вычислительных ресурсов подписки в расположении. |
> | Действие | Microsoft.Compute/locations/vmSizes/read | Выводит список доступных размеров виртуальных машин в расположении. |
> | Действие | Microsoft.Compute/operations/read | Выводит список операций, доступных в поставщике ресурсов Microsoft.Compute. |
> | Действие | Microsoft.Compute/proximityPlacementGroups/delete | Удаление группы с учетом расположения размещения |
> | Действие | Microsoft.Compute/proximityPlacementGroups/read | Получение свойств группы размещения с учетом расположения |
> | Действие | Microsoft.Compute/proximityPlacementGroups/write | Создает новую группу с учетом расположения размещения или обновляет существующий |
> | Действие | Microsoft.Compute/register/action | Регистрирует подписку в поставщике ресурсов Microsoft.Compute. |
> | Действие | Microsoft.Compute/restorePointCollections/delete | Удаляет коллекцию точек восстановления и содержащиеся в ней точки восстановления. |
> | Действие | Microsoft.Compute/restorePointCollections/read | Возвращает свойства коллекции точек восстановления. |
> | Действие | Microsoft.Compute/restorePointCollections/restorePoints/delete | Удаляет точку восстановления. |
> | Действие | Microsoft.Compute/restorePointCollections/restorePoints/read | Возвращает свойства точки восстановления. |
> | Действие | Microsoft.Compute/restorePointCollections/restorePoints/retrieveSasUris/action | Возвращает свойства точки восстановления вместе с универсальными кодами ресурсов (URI) SAS больших двоичных объектов. |
> | Действие | Microsoft.Compute/restorePointCollections/restorePoints/write | Создает точку восстановления. |
> | Действие | Microsoft.Compute/restorePointCollections/write | Создает новую коллекцию точек восстановления или обновляет существующую. |
> | Действие | Microsoft.Compute/sharedVMImages/delete | Удаление образа общей виртуальной машины. |
> | Действие | Microsoft.Compute/sharedVMImages/read | Получение свойств образа общей виртуальной машины. |
> | Действие | Microsoft.Compute/sharedVMImages/versions/delete | Удаление версии образа общей виртуальной машины. |
> | Действие | Microsoft.Compute/sharedVMImages/versions/read | Получение свойств версии образа общей виртуальной машины. |
> | Действие | Microsoft.Compute/sharedVMImages/versions/replicate/action | Репликация версии образа общей виртуальной машины в целевые регионы. |
> | Действие | Microsoft.Compute/sharedVMImages/versions/write | Создание новой версии образа общей виртуальной машины или обновление существующей. |
> | Действие | Microsoft.Compute/sharedVMImages/write | Создание нового образа общей виртуальной машины или обновление существующего. |
> | Действие | Microsoft.Compute/skus/read | Получение списка номеров SKU Microsoft.Compute для вашей подписки. |
> | Действие | Microsoft.Compute/snapshots/beginGetAccess/action | Получение универсального кода ресурса (URI) SAS моментального снимка для доступа к большому двоичному объекту. |
> | Действие | Microsoft.Compute/snapshots/delete | Удаляет моментальный снимок. |
> | Действие | Microsoft.Compute/snapshots/endGetAccess/action | Отмена универсального кода ресурса (URI) SAS моментального снимка. |
> | Действие | Microsoft.Compute/snapshots/read | Возвращает свойства моментального снимка. |
> | Действие | Microsoft.Compute/snapshots/write | Создает новый моментальный снимок или обновляет существующий. |
> | Действие | Microsoft.Compute/unregister/action | Отмена регистрации подписки в поставщике ресурсов Microsoft.Compute |
> | Действие | Microsoft.Compute/virtualMachines/capture/action | Записывает виртуальную машину, копируя ее виртуальных жесткие диски, и создает шаблон, который может использоваться для создания идентичных виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachines/convertToManagedDisks/action | Преобразует диски на основе больших двоичных объектов виртуальной машины в управляемые диски. |
> | Действие | Microsoft.Compute/virtualMachines/deallocate/action | Отключение питания виртуальной машины и освобождение вычислительных ресурсов |
> | Действие | Microsoft.Compute/virtualMachines/delete | Удаляет виртуальную машину. |
> | Действие | Microsoft.Compute/virtualMachines/extensions/delete | Удаляет расширение виртуальной машины. |
> | Действие | Microsoft.Compute/virtualMachines/extensions/read | Возвращает свойства расширения виртуальной машины. |
> | Действие | Microsoft.Compute/virtualMachines/extensions/write | Создает новое расширение виртуальной машины или обновляет существующее. |
> | Действие | Microsoft.Compute/virtualMachines/generalize/action | Задает состояние "Универсальная" для виртуальной машины и подготавливает ее для записи. |
> | Действие | Microsoft.Compute/virtualMachines/instanceView/read | Возвращает состояние выполнения виртуальной машины и ее ресурсов. |
> | DataAction | Microsoft.Compute/virtualMachines/login/action | Вход в систему на виртуальной машине в качестве обычного пользователя |
> | DataAction | Microsoft.Compute/virtualMachines/loginAsAdmin/action | Войдите в систему на виртуальной машине с правами администратора Windows или привилегированного пользователя Linux |
> | Действие | Microsoft.Compute/virtualMachines/performMaintenance/action | Выполнение операции обслуживания виртуальной машины. |
> | Действие | Microsoft.Compute/virtualMachines/powerOff/action | Завершает работу виртуальной машины. Обратите внимание, что плата за виртуальную машину по-прежнему будет взиматься. |
> | Действие | Microsoft.Compute/virtualMachines/read | Получение свойств виртуальной машины |
> | Действие | Microsoft.Compute/virtualMachines/redeploy/action | Повторно развертывает виртуальную машину. |
> | Действие | Microsoft.Compute/virtualMachines/reimage/action | Пересоздание образа виртуальной машины, которая использует разностный диск. |
> | Действие | Microsoft.Compute/virtualMachines/restart/action | Перезапуск виртуальной машины |
> | Действие | Microsoft.Compute/virtualMachines/runCommand/action | Выполнение предопределенного скрипта на виртуальной машине. |
> | Действие | Microsoft.Compute/virtualMachines/start/action | Запуск виртуальной машины |
> | Действие | Microsoft.Compute/virtualMachines/vmSizes/read | Получение списка доступных размеров виртуальной машины, до которых ее можно обновить |
> | Действие | Microsoft.Compute/virtualMachines/write | Создает новую виртуальную машину или обновляет существующую. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/deallocate/action | Завершение работы вычислительных ресурсов и их освобождение для экземпляров виртуальной машины в масштабируемом наборе виртуальных машин.  |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/delete | Удаление масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/delete/action | Удаление экземпляров масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/extensions/delete | Удаление расширения масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/extensions/read | Получение свойств расширения масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/extensions/write | Создание расширения масштабируемого набора виртуальных машин или обновление существующего. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/forceRecoveryServiceFabricPlatformUpdateDomainWalk/action | Ручное выполнение зависшего обновления доменов обновления платформы в масштабируемом наборе виртуальных машин Service Fabric. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/instanceView/read | Получение представления экземпляров масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/manualUpgrade/action | Выполнение ручного обновления экземпляров до последней модели масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/networkInterfaces/read | Получение свойства всех сетевых интерфейсов масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/osRollingUpgrade/action | Запускает последовательное обновление для перевода всех экземпляров масштабируемых наборов виртуальных машин на последнюю доступную версию ОС образа платформы. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/osUpgradeHistory/read | Получение журнала обновлений ОС для масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/performMaintenance/action | Выполнение планового обслуживания экземпляров масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/powerOff/action | Завершение работы экземпляров масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/publicIPAddresses/read | Получение свойств всех общедоступных IP-адресов масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/read | Получение свойств масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/redeploy/action | Повторное развертывание экземпляров масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/reimage/action | Повторное создание образов экземпляров масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/reimageAll/action | Пересоздает образы всех дисков (диска ОС и дисков данных) для экземпляров масштабируемого набора виртуальных машин.  |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/restart/action | Перезапуск экземпляров масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/cancel/action | Отмена последовательного обновления масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/read | Получение состояния последнего последовательного обновления масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/scale/action | Проверка того, возможно ли увеличение или уменьшение числа экземпляров масштабируемого набора виртуальных машин до указанного значения. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/skus/read | Вывод списка действительных номеров SKU для существующего масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/start/action | Запуск экземпляров масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/deallocate/action | Завершает работу вычислительных ресурсов и освобождает их для виртуальной машины в масштабируемом наборе виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/delete | Удаляет конкретную виртуальную машину в масштабируемом наборе виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/instanceView/read | Извлекает представление экземпляров виртуальной машины в масштабируемом наборе виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/publicIPAddresses/read | Получение свойства общедоступного IP-адреса, созданного с использованием масштабируемого набора виртуальных машин. Масштабируемый набор виртуальных машин может создать не более одного общедоступного IP-адреса на IP-конфигурацию (частный IP-адрес). |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/read | Получение свойств одной или всех IP-конфигураций сетевого интерфейса, созданного с помощью масштабируемого набора виртуальных машин. IP-конфигурации представляют частные IP-адреса. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/read | Получение свойств одного или всех сетевых интерфейсов виртуальной машины, созданных с помощью масштабируемого набора виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/performMaintenance/action | Выполнение планового обслуживания экземпляров виртуальных машин в масштабируемом наборе. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/powerOff/action | Завершает работу экземпляра виртуальной машины в масштабируемом наборе виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/read | Извлекает свойства виртуальной машины в масштабируемом наборе виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/redeploy/action | Повторное развертывание образа виртуальной машины в масштабируемом наборе. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/reimage/action | Повторное создание образа экземпляра виртуальной машины в масштабируемом наборе. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/reimageAll/action | Пересоздает образы всех дисков (диска ОС и дисков данных) для экземпляра виртуальной машины в масштабируемом наборе виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/restart/action | Перезапускает экземпляр виртуальной машины в масштабируемом наборе виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/runCommand/action | Выполняет предварительно определенный сценарий в экземпляре виртуальной машины в масштабируемом наборе виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/start/action | Запускает экземпляр виртуальной машины в масштабируемом наборе виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/write | Обновление свойства виртуальной машины в масштабируемом наборе виртуальных машин. |
> | Действие | Microsoft.Compute/virtualMachineScaleSets/write | Создание нового масштабируемого набора виртуальных машин или обновление существующего. |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Consumption/balances/read | Получение списка сводки использования за период выставления счетов для группы управления. |
> | Действие | Microsoft.Consumption/budgets/delete | Удаление бюджетов подписки или группы управления. |
> | Действие | Microsoft.Consumption/budgets/read | Получение списка бюджетов подписки или группы управления. |
> | Действие | Microsoft.Consumption/budgets/write | Создание или обновление бюджетов подписки или группы управления. |
> | Действие | Microsoft.Consumption/charges/read | Вывод списка расходов. |
> | Действие | Microsoft.Consumption/credits/read | Вывод списка кредитов. |
> | Действие | Microsoft.Consumption/events/read | Вывод списка событий. |
> | Действие | Microsoft.Consumption/forecasts/read | Вывод списка прогнозов. |
> | Действие | Microsoft.Consumption/slots/read | Вывод списка слотов. |
> | Действие | Microsoft.Consumption/marketplaces/read | Вывод сведений об использовании ресурсов Marketplace для области подписок EA и WebDirect. |
> | Действие | Microsoft.Consumption/operationresults/read | Вывод списка результатов операций. |
> | Действие | Microsoft.Consumption/operations/read | Вывод списка всех поддерживаемых операций для поставщика ресурсов Microsoft.Consumption. |
> | Действие | Microsoft.Consumption/operationstatus/read | Вывод списка состояний операций. |
> | Действие | Microsoft.Consumption/pricesheets/read | Вывод расценок для подписки или группы управления. |
> | Действие | Microsoft.Consumption/register/action | Регистрация в поставщике используемых ресурсов. |
> | Действие | Microsoft.Consumption/reservationDetails/read | Вывод сведений об использовании зарезервированных экземпляров по порядку резервирования или группам управления. Данные относятся к одному экземпляру на уровне дня. |
> | Действие | Microsoft.Consumption/reservationRecommendations/read | Вывод одной рекомендации или нескольких общих рекомендаций для зарезервированных экземпляров в подписке. |
> | Действие | Microsoft.Consumption/reservationSummaries/read | Вывод сводки использования зарезервированных экземпляров по порядку резервирования или группам управления. Сводка формируется на ежемесячном или ежедневном уровне. |
> | Действие | Microsoft.Consumption/reservationTransactions/read | Вывод журнала транзакций для зарезервированных экземпляров по группам управления. |
> | Действие | Microsoft.Consumption/tags/read | Вывод списка тегов для EA и подписок. |
> | Действие | Microsoft.Consumption/tenants/read | Список клиентов |
> | Действие | Microsoft.Consumption/tenants/register/action | Регистрация действия для области Microsoft.Consumption клиентом. |
> | Действие | Microsoft.Consumption/terms/read | Вывод условий подписки или группы управления. |
> | Действие | Microsoft.Consumption/usageDetails/read | Вывод сведений об использовании для области подписок EA и WebDirect. |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.ContainerInstance/containerGroups/containers/exec/action | Выполнение в указанном контейнере. |
> | Действие | Microsoft.ContainerInstance/containerGroups/containers/logs/read | Получение журналов для конкретного контейнера. |
> | Действие | Microsoft.ContainerInstance/containerGroups/delete | Удаление конкретной группы контейнеров. |
> | Действие | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметра диагностики для группы контейнеров. |
> | Действие | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/write | Создание или обновление параметра диагностики для группы контейнеров. |
> | Действие | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/metricDefinitions/read | Получение доступных метрик для группы контейнеров. |
> | Действие | Microsoft.ContainerInstance/containerGroups/read | Получение всех групп контейнеров. |
> | Действие | Microsoft.ContainerInstance/containerGroups/restart/action | Перезапускает конкретную группу контейнеров. |
> | Действие | Microsoft.ContainerInstance/containerGroups/start/action | Запуск конкретной группы контейнеров. |
> | Действие | Microsoft.ContainerInstance/containerGroups/stop/action | Останавливает конкретную группу контейнеров. Вычислительные ресурсы будут отозваны, и выставление счетов прекратится. |
> | Действие | Microsoft.ContainerInstance/containerGroups/write | Создание или обновление конкретной группы контейнеров. |
> | Действие | Microsoft.ContainerInstance/locations/cachedImages/read | Получение кэшированных изображений для подписки в регионе. |
> | Действие | Microsoft.ContainerInstance/locations/capabilities/read | Получение возможностей для региона. |
> | Действие | Microsoft.ContainerInstance/locations/deleteVirtualNetworkOrSubnets/action | Уведомление Microsoft.ContainerInstance о том, что идет удаление виртуальной сети или подсети. |
> | Действие | Microsoft.ContainerInstance/locations/usages/read | Получение данных об использовании для определенного региона. |
> | Действие | Microsoft.ContainerInstance/register/action | Регистрирует подписку в поставщике ресурсов экземпляра контейнера и включает возможность создания групп контейнеров. |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.ContainerRegistry/checkNameAvailability/read | Проверка возможности использования имени реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/locations/deleteVirtualNetworkOrSubnets/action | Уведомляет Microsoft.ContainerRegistry о том, что удаляется виртуальная сеть или подсеть. |
> | Действие | Microsoft.ContainerRegistry/locations/operationResults/read | Получение результата асинхронной операции. |
> | Действие | Microsoft.ContainerRegistry/operations/read | Вывод списка всех доступных операций API REST реестра контейнеров Azure. |
> | Действие | Microsoft.ContainerRegistry/register/action | Регистрирует подписку для поставщика ресурсов реестра контейнеров и позволяет создавать реестры контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/artifacts/delete | Удалите артефакт в реестре контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/builds/cancel/action | Отменяет существующую сборку. |
> | Действие | Microsoft.ContainerRegistry/registries/builds/getLogLink/action | Возвращает ссылку для скачивания журналов сборки. |
> | Действие | Microsoft.ContainerRegistry/registries/builds/read | Получает свойства указанной сборки или списка сборок для указанного реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/builds/write | Обновляет сборку для реестра контейнеров с указанными параметрами. |
> | Действие | Microsoft.ContainerRegistry/registries/buildTasks/delete | Удаляет задание сборки из реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/buildTasks/listSourceRepositoryProperties/action | Перечисляет свойства системы управления версиями для задачи сборки. |
> | Действие | Microsoft.ContainerRegistry/registries/buildTasks/read | Получает свойства указанной задачи сборки или списка задач сборок для указанного реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/buildTasks/steps/delete | Удаляет шаг создания из задачи сборки. |
> | Действие | Microsoft.ContainerRegistry/registries/buildTasks/steps/listBuildArguments/action | Перечисляет аргументы сборки для шага сборки, включая аргументы секретов. |
> | Действие | Microsoft.ContainerRegistry/registries/buildTasks/steps/read | Получает свойства указанного шага сборки или списка шагов сборок для указанной задачи сборки. |
> | Действие | Microsoft.ContainerRegistry/registries/buildTasks/steps/write | Создает или обновляет шаг сборки для задачи сборки с указанными параметрами. |
> | Действие | Microsoft.ContainerRegistry/registries/buildTasks/write | Создает или обновляет задачу сборки для реестра контейнеров с указанными параметрами. |
> | Действие | Microsoft.ContainerRegistry/registries/delete | Удаление реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/eventGridFilters/delete | Удаление фильтра сетки событий из реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/eventGridFilters/read | Получение свойств указанного фильтра сетки событий или списка всех фильтров сетки событий для указанного реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/eventGridFilters/write | Создание или обновление фильтра сетки событий для реестра контейнеров с указанными параметрами. |
> | Действие | Microsoft.ContainerRegistry/registries/getBuildSourceUploadUrl/action | Получает расположение для передачи, чтобы пользователь мог передать источник. |
> | Действие | Microsoft.ContainerRegistry/registries/importImage/action | Импорт образа в реестр контейнеров с указанными параметрами. |
> | Действие | Microsoft.ContainerRegistry/registries/listBuildSourceUploadUrl/action | Получение расположения URL-адреса передачи источника для реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/listCredentials/action | Выводит список учетных данных входа для указанного реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/listPolicies/read | Выводит список политик для указанного реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/listUsages/read | Получение сведений об использовании квот для указанного реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/metadata/read | Получает метаданные для заданного репозитория для реестра контейнеров |
> | Действие | Microsoft.ContainerRegistry/registries/metadata/write | Обновляет метаданные репозитория для реестра контейнеров |
> | Действие | Microsoft.ContainerRegistry/registries/operationStatuses/read | Получение состояния асинхронной операции реестра. |
> | Действие | Microsoft.ContainerRegistry/registries/pull/read | Извлечение или получение образов из реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/push/write | Передача или запись образов в реестр контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/quarantineRead/read | Извлечение или получение помещенных в карантин образов из реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/quarantineWrite/write | Запись и изменение состояния карантина образов, помещенных в карантин. |
> | Действие | Microsoft.ContainerRegistry/registries/queueBuild/action | Создает новую сборку на основе параметров запроса и добавляет ее в очередь сборки. |
> | Действие | Microsoft.ContainerRegistry/registries/read | Получение свойств указанного реестра контейнеров или вывод списка всех реестров контейнеров в указанной группе ресурсов или подписке. |
> | Действие | Microsoft.ContainerRegistry/registries/regenerateCredential/action | Повторное создание учетных данных входа для указанного реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/replications/delete | Удаление репликации из реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/replications/operationStatuses/read | Получение состояния асинхронной операции репликации. |
> | Действие | Microsoft.ContainerRegistry/registries/replications/read | Получение свойств указанной репликации или списка репликаций для указанного реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/replications/write | Создание или обновление репликаций для реестра контейнеров с указанными параметрами. |
> | Действие | Microsoft.ContainerRegistry/registries/runs/cancel/action | Отмена существующего выполнения. |
> | Действие | Microsoft.ContainerRegistry/registries/runs/listLogSasUrl/action | Получение URL-адреса SAS журнала для выполнения. |
> | Действие | Microsoft.ContainerRegistry/registries/runs/read | Получение свойств выполнения по реестру контейнера или списку запусков. |
> | Действие | Microsoft.ContainerRegistry/registries/runs/write | Обновление выполнения. |
> | Действие | Microsoft.ContainerRegistry/registries/scheduleRun/action | Планирование запуска по реестру контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/sign/write | Передача или извлечение метаданных доверия к содержимому для реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/tasks/delete | Удаление задачи для реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/tasks/listDetails/action | Список всех сведений о задаче для реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/tasks/read | Получение задачи для реестра контейнеров или списка всех задач. |
> | Действие | Microsoft.ContainerRegistry/registries/tasks/write | Создание или обновление задачи для реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/updatePolicies/write | Обновляет политики для указанного реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/webhooks/delete | Удаление веб-перехватчика из реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/webhooks/getCallbackConfig/action | Получение конфигурации URI службы и пользовательских заголовков для веб-перехватчика. |
> | Действие | Microsoft.ContainerRegistry/registries/webhooks/listEvents/action | Получение списка последних событий для указанного веб-перехватчика. |
> | Действие | Microsoft.ContainerRegistry/registries/webhooks/operationStatuses/read | Получение состояния асинхронной операции веб-перехватчика. |
> | Действие | Microsoft.ContainerRegistry/registries/webhooks/ping/action | Запуск события проверки связи для отправки веб-перехватчику. |
> | Действие | Microsoft.ContainerRegistry/registries/webhooks/read | Получение свойств указанного веб-перехватчика или списка веб-перехватчиков для указанного реестра контейнеров. |
> | Действие | Microsoft.ContainerRegistry/registries/webhooks/write | Создание или обновление веб-перехватчика для реестра контейнеров с указанными параметрами. |
> | Действие | Microsoft.ContainerRegistry/registries/write | Создание или обновление реестра контейнеров с указанными параметрами. |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.ContainerService/containerServices/delete | Удаление службы контейнеров. |
> | Действие | Microsoft.ContainerService/containerServices/read | Получение службы контейнеров. |
> | Действие | Microsoft.ContainerService/containerServices/write | Создание новой службы контейнеров или обновление существующей. |
> | Действие | Microsoft.ContainerService/locations/operationresults/read | Получает состояние результата асинхронной операции. |
> | Действие | Microsoft.ContainerService/locations/operations/read | Возвращает состояние асинхронной операции. |
> | Действие | Microsoft.ContainerService/locations/orchestrators/read | Выводит список поддерживаемых операций. |
> | Действие | Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action | Получение профиля доступа управляемого кластера по имени роли с помощью вывода учетных данных. |
> | Действие | Microsoft.ContainerService/managedClusters/accessProfiles/read | Получение профиля доступа управляемого кластера по имени роли. |
> | Действие | Microsoft.ContainerService/managedClusters/agentPools/delete | Удаляет пул агентов |
> | Действие | Microsoft.ContainerService/managedClusters/agentPools/read | Возвращает пул агентов |
> | Действие | Microsoft.ContainerService/managedClusters/agentPools/write | Создает новый пул агентов или обновляет существующий |
> | Действие | Microsoft.ContainerService/managedClusters/delete | Удаление управляемого кластера. |
> | Действие | Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action | Перечислить учетные данные clusterAdmin управляемого кластера |
> | Действие | Microsoft.ContainerService/managedClusters/listClusterUserCredential/action | Перечислить учетные данные clusterUser управляемого кластера |
> | Действие | Microsoft.ContainerService/managedClusters/read | Получение управляемого кластера. |
> | Действие | Microsoft.ContainerService/managedClusters/resetAADProfile/action | Сброс профиля AAD управляемого кластера. |
> | Действие | Microsoft.ContainerService/managedClusters/resetServicePrincipalProfile/action | Сброс профиля субъекта-службы управляемого кластера. |
> | Действие | Microsoft.ContainerService/managedClusters/upgradeprofiles/read | Возвращает профиль обновления кластера. |
> | Действие | Microsoft.ContainerService/managedClusters/write | Создание нового управляемого кластера или обновление имеющегося. |
> | Действие | Microsoft.ContainerService/openShiftClusters/delete | Удалить кластер Open Shift |
> | Действие | Microsoft.ContainerService/openShiftClusters/read | Получить кластер Open Shift |
> | Действие | Microsoft.ContainerService/openShiftClusters/write | Создание нового кластера Open Shift или обновление имеющегося |
> | Действие | Microsoft.ContainerService/openShiftManagedClusters/delete | Удаление управляемого кластера Open Shift |
> | Действие | Microsoft.ContainerService/openShiftManagedClusters/read | Получить управляемый кластер Open Shift |
> | Действие | Microsoft.ContainerService/openShiftManagedClusters/write | Создание нового управляемого кластера Open Shift или обновление имеющегося |
> | Действие | Microsoft.ContainerService/operations/read | Выводит список операций, доступных в поставщике ресурсов Microsoft.ContainerService. |
> | Действие | Microsoft.ContainerService/register/action | Регистрирует подписку в поставщике ресурсов Microsoft.ContainerService. |
> | Действие | Microsoft.ContainerService/unregister/action | Отменяет регистрацию подписки в поставщике ресурсов Microsoft.ContainerService. |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.ContentModerator/applications/delete | Операция удаления. |
> | Действие | Microsoft.ContentModerator/applications/listSecrets/action | Выводит список секретов. |
> | Действие | Microsoft.ContentModerator/applications/listSingleSignOnToken/action | Считывает маркеры единого входа. |
> | Действие | Microsoft.ContentModerator/applications/read | Операция чтения. |
> | Действие | Microsoft.ContentModerator/applications/write | Операции записи. |
> | Действие | Microsoft.ContentModerator/applications/write | Операции записи. |
> | Действие | Microsoft.ContentModerator/listCommunicationPreference/action | Отображает параметры связи. |
> | Действие | Microsoft.ContentModerator/operations/read | Считывает операции. |
> | Действие | Microsoft.ContentModerator/updateCommunicationPreference/action | Обновляет параметры связи. |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.CostManagement/cloudConnectors/delete | Удаление указанного cloudConnector. |
> | Действие | Microsoft.CostManagement/cloudConnectors/read | Список cloudConnectors для проверенного пользователя. |
> | Действие | Microsoft.CostManagement/cloudConnectors/write | Создание или обновление указанной cloudConnector. |
> | Действие | Microsoft.CostManagement/dimensions/read | Вывод списка всех поддерживаемых измерений в области. |
> | Действие | Microsoft.CostManagement/exports/action | Выполнение указанной операции экспорта. |
> | Действие | Microsoft.CostManagement/exports/delete | Удаление указанной операции экспорта. |
> | Действие | Microsoft.CostManagement/exports/read | Вывод списка операций экспорта по области. |
> | Действие | Microsoft.CostManagement/exports/run/action | Выполните экспорт. |
> | Действие | Microsoft.CostManagement/exports/write | Создание или обновление указанной операции экспорта. |
> | Действие | Microsoft.CostManagement/externalBillingAccounts/externalSubscriptions/read | Список externalSubscriptions в externalBillingAccount для проверенного пользователя. |
> | Действие | Microsoft.CostManagement/externalBillingAccounts/read | Список externalBillingAccounts для проверенного пользователя. |
> | Действие | Microsoft.CostManagement/externalSubscriptions/read | Список externalSubscriptions для проверенного пользователя. |
> | Действие | Microsoft.CostManagement/externalSubscriptions/write | Обновление группы управления связанного externalSubscription |
> | Действие | Microsoft.CostManagement/query/action | Запрос данных об использовании в области. |
> | Действие | Microsoft.CostManagement/query/read | Запрос данных об использовании в области. |
> | Действие | Microsoft.CostManagement/register/action | Зарегистрируйте действие для области Microsoft.CostManagement подпиской. |
> | Действие | Microsoft.CostManagement/reports/action | Планирование создания отчетов о данных об использовании в области. |
> | Действие | Microsoft.CostManagement/reports/read | Планирование создания отчетов о данных об использовании в области. |
> | Действие | Microsoft.CostManagement/tenants/register/action | Зарегистрируйте действие для области Microsoft.CostManagement клиента. |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.CustomerInsights/hubs/adobemetadata/action | Создание или обновление метаданных Adobe для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/adobemetadata/read | Чтение метаданных Adobe для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/authorizationPolicies/delete | Удаляет политику подписанного URL-адреса Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/authorizationPolicies/read | Считывает политику подписанного URL-адреса Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/authorizationPolicies/regeneratePrimaryKey/action | Повторно создает первичный ключ политики подписанного URL-адреса Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/authorizationPolicies/regenerateSecondaryKey/action | Повторно создает вторичный ключ политики подписанного URL-адреса Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/authorizationPolicies/write | Создает или обновляет политику подписанного URL-адреса Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/connectors/activate/action | Активация соединителя Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/connectors/activate/action | Активация соединителя Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/connectors/delete | Удаляет соединитель Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/connectors/getruntimestatus/action | Получение состояния выполнения соединителя Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/connectors/mappings/activate/action | Активация сопоставления соединителя Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/connectors/mappings/delete | Удаляет сопоставление соединителя Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/connectors/mappings/operations/read | Чтение результата операции сопоставления соединителя для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/connectors/mappings/read | Считывает сопоставление соединителя Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/connectors/mappings/write | Создает или обновляет сопоставление соединителя Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/connectors/operations/read | Чтение результата операции соединителя для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/connectors/read | Считывает соединитель Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/connectors/saveauthinfo/action | Создание или обновление данных аутентификации соединителя Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/connectors/update/action | Обновление соединителя Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/connectors/write | Создает или обновляет соединитель Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/crmmetadata/action | Создание или обновление метаданных CRM для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/crmmetadata/read | Чтение метаданных CRM для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/delete | Удаляет концентратор Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/gdpr/delete | Удаление GDPR Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/gdpr/read | Чтение GDPR Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/gdpr/write | Создание или обновление GDPR Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/getbillingcredits/read | Получение данных о кредитах на концентратор Azure Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/getbillinghistory/read | Получение журнала выставленных счетов за использование концентратора Azure Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/images/delete | Удаление образа Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/images/read | Чтение образа Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/images/write | Создание или обновление образа Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/interactions/delete | Удаление взаимодействий Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/interactions/operations/read | Чтение результата операции взаимодействия для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/interactions/read | Считывает взаимодействие Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/interactions/suggestrelationshiplinks/action | Предложение ссылок связи для взаимодействий Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/interactions/write | Создает или обновляет взаимодействие Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/kpi/delete | Удаляет ключевой показатель эффективности Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/kpi/operations/read | Чтение результата операции получения ключевых показателей эффективности для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/kpi/read | Считывает ключевой показатель эффективности Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/kpi/reprocess/action | Повторная обработка ключевых показателей эффективности Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/kpi/write | Создает или обновляет ключевой показатель эффективности Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/links/delete | Удаление ссылок Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/links/operations/read | Чтение результата операции связей соединителя для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/links/read | Чтение ссылок Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/links/write | Создание или обновление ссылок Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/msemetadata/action | Создание или обновление метаданных MSE для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/msemetadata/read | Чтение метаданных MSE для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/operationresults/read | Получение результатов операции концентратора для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/predictions/delete | Удаление прогнозов Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/predictions/operations/read | Чтение результата операции получения прогнозов для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/predictions/read | Чтение прогнозов Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/predictions/write | Создание или обновление прогнозов Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/predictivematchpolicies/delete | Удаление политик прогнозного сопоставления Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/predictivematchpolicies/operations/read | Чтение результата операции политик прогнозного сопоставления для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/predictivematchpolicies/read | Чтение политик прогнозного сопоставления Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/predictivematchpolicies/write | Создание или обновление политик прогнозного сопоставления Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/profiles/delete | Удаление профиля Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/profiles/operations/read | Чтение результата операции профиля для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/profiles/read | Считывает профиль Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/profiles/write | Записывает профиль Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/diagnosticSettings/read | Возвращает параметр диагностики для ресурса. |
> | Действие | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/diagnosticSettings/write | Создает или обновляет параметр диагностики для ресурса. |
> | Действие | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/logDefinitions/read | Возвращает доступные журналы для ресурса. |
> | Действие | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/metricDefinitions/read | Возвращает доступные метрики для ресурса. |
> | Действие | Microsoft.CustomerInsights/hubs/read | Считывает концентратор Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/relationshiplinks/delete | Удаление ссылок связи Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/relationshiplinks/operations/read | Чтение результата операции ссылок связи для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/relationshiplinks/read | Чтение ссылок связи Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/relationshiplinks/write | Создание или обновление ссылок связи Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/relationships/delete | Удаление связей Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/relationships/operations/read | Чтение результата операции связей для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/relationships/read | Чтение связей Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/relationships/write | Создание или обновление связей Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/roleAssignments/delete | Удаляет назначение RBAC для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/roleAssignments/operations/read | Чтение результатов операции назначения RBAC для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/roleAssignments/read | Считывает назначение RBAC для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/roleAssignments/write | Создает или обновляет назначение RBAC для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/roles/read | Чтение ролей RBAC для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/salesforcemetadata/action | Создание или обновление метаданных SalesForce для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/salesforcemetadata/read | Чтение метаданных SalesForce для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/segments/delete | Удаление сегментов Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/segments/dynamic/action | Управление динамическими сегментами Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/segments/read | Чтение сегментов Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/segments/static/action | Управление статическими сегментами Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/segments/write | Создание или обновление сегментов Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/delete | Удаление строк подключения SQL для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/read | Чтение строк подключения SQL для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/write | Создание или обновление строк подключения SQL для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/suggesttypeschema/action | Создание предложения схемы типа из примера данных. |
> | Действие | Microsoft.CustomerInsights/hubs/tenantmanagement/read | Управление параметрами концентратора Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/views/delete | Удаляет представление приложений Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/views/read | Считывает представление приложений Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/views/write | Создает или обновляет представление приложений Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/widgettypes/read | Чтение типов мини-приложений приложения Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/hubs/write | Создает или обновляет концентратор Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/operations/read | Чтение метаданных API для Azure Customer Insights. |
> | Действие | Microsoft.CustomerInsights/register/action | Регистрация подписки для поставщика ресурсов Customer Insights и разрешение создания ресурсов Customer Insights. |
> | Действие | Microsoft.CustomerInsights/unregister/action | Отмена регистрации подписки для поставщика ресурсов Customer Insights. |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.DataBox/jobs/bookShipmentPickUp/action | Позволяет заказать прием посылки для обратной отправки. |
> | Действие | Microsoft.DataBox/jobs/cancel/action | Отменяет выполняемый заказ. |
> | Действие | Microsoft.DataBox/jobs/delete | Удаляет заказы. |
> | Действие | Microsoft.DataBox/jobs/listCredentials/action | Выводит список незашифрованных учетных данных, связанных с заказом. |
> | Действие | Microsoft.DataBox/jobs/read | Выводит список заказов или получает их. |
> | Действие | Microsoft.DataBox/jobs/write | Создает или обновляет заказы. |
> | Действие | Microsoft.DataBox/locations/availableSkus/action | Этот метод возвращает список доступных номеров SKU. |
> | Действие | Microsoft.DataBox/locations/operationResults/read | Вывод списка или получение результатов операции. |
> | Действие | Microsoft.DataBox/locations/validateAddress/action | Проверяет адрес доставки и предлагает альтернативные адреса, если они доступны. |
> | Действие | Microsoft.DataBox/register/action | Регистрация поставщика Microsoft.Databox. |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/alerts/read | Выводит список оповещений или возвращает их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/alerts/read | Выводит список оповещений или возвращает их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/delete | Удаляет расписания пропускной способности |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/operationResults/read | Возвращает результат операции или выводит их список |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/read | Выводит список расписаний пропускной способности или возвращает их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/read | Выводит список расписаний пропускной способности или возвращает их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/write | Создает или обновляет расписания пропускной способности |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/delete | Удаляет устройства Data Box Edge |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/downloadUpdates/action | Загружает обновления на устройстве |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/extendedInformation/action | Извлекает расширенные сведения о ресурсе |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/getExtendedInformation/action | Извлекает расширенные сведения о ресурсе |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/installUpdates/action | Устанавливает обновления на устройство |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/jobs/read | Выводит список заданий или возвращает их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/networkSettings/read | Выводит список параметров сети устройства или возвращает их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/operationResults/read | Возвращает результат операции или выводит их список |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/operationsStatus/read | Выводит список или возвращает состояние операции |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/delete | Удаляет заказы |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/operationResults/read | Возвращает результат операции или выводит их список |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/read | Выводит список или возвращает заказы |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/read | Выводит список или возвращает заказы |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/write | Создает или обновляет заказы |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | Выводит список устройств Data Box Edge или возвращает их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | Выводит список устройств Data Box Edge или возвращает их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | Выводит список устройств Data Box Edge или возвращает их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/delete | Удаление ролей |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/operationResults/read | Возвращает результат операции или выводит их список |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/read | Получает роли или выводит их список |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/read | Получает роли или выводит их список |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/write | Создает или обновляет роли |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/scanForUpdates/action | Проверяет на наличие обновлений |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/securitySettings/operationResults/read | Возвращает результат операции или выводит их список |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/securitySettings/update/action | Обновляет параметры безопасности |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/delete | Удаляет общедоступные ресурсы |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/operationResults/read | Возвращает результат операции или выводит их список |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/read | Выводит список общедоступных ресурсов или возвращает их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/read | Выводит список общедоступных ресурсов или возвращает их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/refresh/action | Обновить метаданные общих папок данными из облака |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/write | Создает общие папки или обновляет их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/delete | Удаляет данные учетной записи хранения |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/operationResults/read | Возвращает результат операции или выводит их список |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/read | Выводит список данных учетной записи хранения или возвращает их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/read | Выводит список данных учетной записи хранения или возвращает их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/write | Создает данные учетной записи хранения или обновляет их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/delete | Удаление триггеров |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/operationResults/read | Возвращает результат операции или выводит их список |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/read | Выводит список или получает триггеры |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/read | Выводит список или получает триггеры |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/write | Создает или обновляет триггеры |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/updateSummary/read | Выводит сводку обновлений или возвращает ее |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/uploadCertificate/action | Отправляет сертификат для регистрации устройства |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/delete | Удаляет пользователей общих ресурсов |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/operationResults/read | Возвращает результат операции или выводит их список |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/read | Выводит список пользователей общих ресурсов или возвращает их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/read | Выводит список пользователей общих ресурсов или возвращает их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/write | Создает пользователей общих ресурсов или обновляет их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/write | Создает устройства Data Box Edge или обновляет их |
> | Действие | Microsoft.DataBoxEdge/dataBoxEdgeDevices/write | Создает устройства Data Box Edge или обновляет их |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Databricks/locations/getNetworkPolicies/action | Получение политики намерение сети для подсети, в зависимости от расположения, используемые NRP |
> | Действие | Microsoft.Databricks/register/action | Регистрация в Databricks. |
> | Действие | Microsoft.Databricks/workspaces/delete | Удаление рабочей области Databricks. |
> | Действие | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/diagnosticSettings/read | Задание доступных параметров диагностики для рабочей области Databricks |
> | Действие | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/diagnosticSettings/write | Добавляет или изменяет параметры диагностики. |
> | Действие | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/logDefinitions/read | Получение доступных определений журнала для рабочего пространства Databricks |
> | Действие | Microsoft.Databricks/workspaces/read | Извлечение списка рабочих областей Databricks. |
> | Действие | Microsoft.Databricks/workspaces/write | Создание рабочей области Databricks. |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.DataCatalog/datacatalogs/delete | Удалите ресурс DataCatalog для поставщика ресурсов каталога данных. |
> | Действие | Microsoft.DataCatalog/datacatalogs/read | Чтение ресурса DataCatalog для поставщика ресурсов каталога данных. |
> | Действие | Microsoft.DataCatalog/datacatalogs/write | Написание DataCatalog ресурсов для поставщика ресурсов каталога данных. |
> | Действие | Microsoft.DataCatalog/operations/read | Считывает все доступные операции в поставщике ресурсов каталога данных. |
> | Действие | Microsoft.DataCatalog/register/action | Зарегистрировать подписку для поставщика ресурсов каталога данных |
> | Действие | Microsoft.DataCatalog/unregister/action | Отмените регистрацию подписки для поставщика ресурсов каталога данных |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.DataFactory/checkazuredatafactorynameavailability/read | Проверяет, доступно ли имя фабрики данных для использования. |
> | Действие | Microsoft.DataFactory/datafactories/activitywindows/read | Считывает окна действий в фабрике данных с указанными параметрами. |
> | Действие | Microsoft.DataFactory/datafactories/datapipelines/activities/activitywindows/read | Считывает окна действий для действия конвейера с указанными параметрами. |
> | Действие | Microsoft.DataFactory/datafactories/datapipelines/activitywindows/read | Считывает окна действий для конвейера с указанными параметрами. |
> | Действие | Microsoft.DataFactory/datafactories/datapipelines/delete | Удаляет конвейер. |
> | Действие | Microsoft.DataFactory/datafactories/datapipelines/pause/action | Приостанавливает конвейер. |
> | Действие | Microsoft.DataFactory/datafactories/datapipelines/read | Считывает конвейер. |
> | Действие | Microsoft.DataFactory/datafactories/datapipelines/resume/action | Возобновляет работу конвейера. |
> | Действие | Microsoft.DataFactory/datafactories/datapipelines/update/action | Обновляет конвейер. |
> | Действие | Microsoft.DataFactory/datafactories/datapipelines/write | Создает или обновляет конвейер. |
> | Действие | Microsoft.DataFactory/datafactories/datasets/activitywindows/read | Считывает окна действий для набора данных с указанными параметрами. |
> | Действие | Microsoft.DataFactory/datafactories/datasets/delete | Удаляет набор данных. |
> | Действие | Microsoft.DataFactory/datafactories/datasets/read | Считывает набор данных. |
> | Действие | Microsoft.DataFactory/datafactories/datasets/sliceruns/read | Считывает выполнение среза данных для заданного набора данных с указанным временем запуска. |
> | Действие | Microsoft.DataFactory/datafactories/datasets/slices/read | Возвращает срезы данных за указанный период. |
> | Действие | Microsoft.DataFactory/datafactories/datasets/slices/write | Обновляет состояние среза данных. |
> | Действие | Microsoft.DataFactory/datafactories/datasets/write | Создает или обновляет набор данных. |
> | Действие | Microsoft.DataFactory/datafactories/delete | Удаляет фабрику данных. |
> | Действие | Microsoft.DataFactory/datafactories/gateways/connectioninfo/action | Считывает сведения о подключении для шлюза. |
> | Действие | Microsoft.DataFactory/datafactories/gateways/delete | Удаляет шлюз. |
> | Действие | Microsoft.DataFactory/datafactories/gateways/listauthkeys/action | Выводит список ключей аутентификации для шлюза. |
> | Действие | Microsoft.DataFactory/datafactories/gateways/read | Считывает шлюз. |
> | Действие | Microsoft.DataFactory/datafactories/gateways/regenerateauthkey/action | Повторно создает ключи аутентификации для шлюза. |
> | Действие | Microsoft.DataFactory/datafactories/gateways/write | Создает или обновляет шлюз. |
> | Действие | Microsoft.DataFactory/datafactories/linkedServices/delete | Удаляет связанную службу. |
> | Действие | Microsoft.DataFactory/datafactories/linkedServices/read | Считывает связанную службу. |
> | Действие | Microsoft.DataFactory/datafactories/linkedServices/write | Создает или обновляет связанную службу. |
> | Действие | Microsoft.DataFactory/datafactories/read | Считывает фабрику данных. |
> | Действие | Microsoft.DataFactory/datafactories/runs/loginfo/read | Считывает универсальный код ресурса SAS контейнера больших двоичных объектов, содержащего журналы. |
> | Действие | Microsoft.DataFactory/datafactories/tables/delete | Удаляет набор данных. |
> | Действие | Microsoft.DataFactory/datafactories/tables/read | Считывает набор данных. |
> | Действие | Microsoft.DataFactory/datafactories/tables/write | Создает или обновляет набор данных. |
> | Действие | Microsoft.DataFactory/datafactories/write | Создает или обновляет фабрику данных. |
> | Действие | Microsoft.DataFactory/factories/cancelpipelinerun/action | Отменяет запуск конвейера, указанный идентификатором запуска. |
> | Действие | Microsoft.DataFactory/factories/createdataflowdebugsession/action | Создает сеанс отладки потока данных. |
> | Действие | Microsoft.DataFactory/factories/dataflows/delete | Удаляет поток данных. |
> | Действие | Microsoft.DataFactory/factories/dataflows/read | Считывает поток данных. |
> | Действие | Microsoft.DataFactory/factories/dataflows/write | Создать или обновить поток данных |
> | Действие | Microsoft.DataFactory/factories/datasets/delete | Удаляет набор данных. |
> | Действие | Microsoft.DataFactory/factories/datasets/read | Считывает набор данных. |
> | Действие | Microsoft.DataFactory/factories/datasets/write | Создает или обновляет набор данных. |
> | Действие | Microsoft.DataFactory/factories/delete | Удаляет фабрику данных. |
> | Действие | Microsoft.DataFactory/factories/deletedataflowdebugsession/action | Удаляет сеанс отладки потока данных. |
> | Действие | Microsoft.DataFactory/factories/getDataPlaneAccess/action | Получает доступ к службе DataPlane ADF. |
> | Действие | Microsoft.DataFactory/factories/getDataPlaneAccess/read | Считывает данные о доступе к службе DataPlane ADF. |
> | Действие | Microsoft.DataFactory/factories/getFeatureValue/action | Получает значение функции управления раскрытием для конкретного расположения. |
> | Действие | Microsoft.DataFactory/factories/getFeatureValue/read | Считывает значение функции управления раскрытием для конкретного расположения. |
> | Действие | Microsoft.DataFactory/factories/getGitHubAccessToken/action | Получает маркер доступа GitHub. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/delete | Удаляет среду выполнения интеграции. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/getconnectioninfo/read | Считывает сведения о подключении к среде выполнения интеграции. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/getObjectMetadata/action | Получает метаданные среды выполнения интеграции SSIS для указанной среды Integration Runtime. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/getstatus/read | Считывает состояние среды выполнения интеграции. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/linkedIntegrationRuntime/action | Создает ссылку на связанную Integration Runtime для указанной общей среды Integration Runtime. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/listauthkeys/read | Выводит список ключей аутентификации для среды выполнения интеграции. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/monitoringdata/read | Возвращает данные мониторинга для среды выполнения интеграции. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/nodes/delete | Удаляет узел для указанной среды выполнения интеграции. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/nodes/ipAddress/action | Возвращает IP-адрес для указанного узла среды выполнения интеграции. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/nodes/read | Считывает узел для указанной среды Integration Runtime. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/nodes/write | Обновляет узел локальной среды выполнения интеграции. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/read | Считывает среду выполнения интеграции. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/refreshObjectMetadata/action | Обновляет метаданные среды выполнения интеграции SSIS для указанной среды Integration Runtime. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/regenerateauthkey/action | Повторно создает ключи проверки подлинности для указанной среды выполнения интеграции. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/removelinks/action | Удаляет связанные ссылки Integration Runtime из указанной среды Integration Runtime. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/start/action | Запускает среду выполнения интеграции. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/stop/action | Останавливает среду выполнения интеграции. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/synccredentials/action | Синхронизирует учетные данные для указанной среды выполнения интеграции. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/upgrade/action | Обновляет указанную среду выполнения интеграции. |
> | Действие | Microsoft.DataFactory/factories/integrationruntimes/write | Создает или обновляет среду выполнения интеграции. |
> | Действие | Microsoft.DataFactory/factories/linkedServices/delete | Удаляет связанную службу. |
> | Действие | Microsoft.DataFactory/factories/linkedServices/read | Считывает связанную службу. |
> | Действие | Microsoft.DataFactory/factories/linkedServices/write | Создает или обновляет связанную службу. |
> | Действие | Microsoft.DataFactory/factories/pipelineruns/activityruns/read | Считывает выполнения действий для указанного идентификатора запуска конвейера. |
> | Действие | Microsoft.DataFactory/factories/pipelineruns/cancel/action | Отменяет запуск конвейера, указанный идентификатором запуска. |
> | Действие | Microsoft.DataFactory/factories/pipelineruns/queryactivityruns/action | Запрашивает выполнения действий для указанного идентификатора запуска конвейера. |
> | Действие | Microsoft.DataFactory/factories/pipelineruns/queryactivityruns/read | Считывает результат выполнения действий запросов для указанного идентификатора запуска конвейера. |
> | Действие | Microsoft.DataFactory/factories/pipelineruns/read | Считывает выполнения конвейера. |
> | Действие | Microsoft.DataFactory/factories/pipelines/createrun/action | Создает выполнение для конвейера. |
> | Действие | Microsoft.DataFactory/factories/pipelines/delete | Удаляет конвейер. |
> | Действие | Microsoft.DataFactory/factories/pipelines/pipelineruns/activityruns/progress/read | Возвращает ход выполнения действий. |
> | Действие | Microsoft.DataFactory/factories/pipelines/pipelineruns/read | Считывает выполнение конвейера. |
> | Действие | Microsoft.DataFactory/factories/pipelines/read | Считывает конвейер. |
> | Действие | Microsoft.DataFactory/factories/pipelines/write | Создает или обновляет конвейер. |
> | Действие | Microsoft.DataFactory/factories/querydebugpipelineruns/action | Запрашивает данные о выполнении конвейера отладки. |
> | Действие | Microsoft.DataFactory/factories/querypipelineruns/action | Запрашивает выполнения конвейера. |
> | Действие | Microsoft.DataFactory/factories/querypipelineruns/read | Считывает результат выполнения конвейера запросов. |
> | Действие | Microsoft.DataFactory/factories/querytriggerruns/action | Запрашивает выполнения триггеров. |
> | Действие | Microsoft.DataFactory/factories/querytriggerruns/read | Считывает результат запусков триггера. |
> | Действие | Microsoft.DataFactory/factories/read | Считывает фабрику данных. |
> | Действие | Microsoft.DataFactory/factories/startdataflowdebugsession/action | Запускает сеанс отладки потока данных. |
> | Действие | Microsoft.DataFactory/factories/triggerruns/read | Считывает выполнения триггеров. |
> | Действие | Microsoft.DataFactory/factories/triggers/delete | Удаляет триггер. |
> | Действие | Microsoft.DataFactory/factories/triggers/read | Считывает триггер. |
> | Действие | Microsoft.DataFactory/factories/triggers/start/action | Запускает триггер. |
> | Действие | Microsoft.DataFactory/factories/triggers/stop/action | Останавливает триггер. |
> | Действие | Microsoft.DataFactory/factories/triggers/triggerruns/read | Считывает выполнения триггеров. |
> | Действие | Microsoft.DataFactory/factories/triggers/write | Создает или обновляет триггер. |
> | Действие | Microsoft.DataFactory/factories/write | Создает или обновляет фабрику данных. |
> | Действие | Microsoft.DataFactory/locations/configureFactoryRepo/action | Настройка репозитория для производства. |
> | Действие | Microsoft.DataFactory/locations/getFeatureValue/action | Получает значение функции управления раскрытием для конкретного расположения. |
> | Действие | Microsoft.DataFactory/locations/getFeatureValue/read | Считывает значение функции управления раскрытием для конкретного расположения. |
> | Действие | Microsoft.DataFactory/operations/read | Считывает все операции в поставщике Фабрики данных Майкрософт. |
> | Действие | Microsoft.DataFactory/register/action | Регистрирует подписку для поставщика ресурсов фабрики данных. |
> | Действие | Microsoft.DataFactory/unregister/action | Отменяет регистрацию подписки для поставщика ресурсов фабрики данных. |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.DataLakeAnalytics/accounts/computePolicies/delete | Удаление политики вычислений. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/computePolicies/read | Получение сведений о политике вычислений. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/computePolicies/write | Создание или обновление политики вычислений. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/delete | Отмена связи учетной записи Data Lake Store с учетной записью Data Lake Analytics. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/read | Получение данных о связанной учетной записи Data Lake Store для учетной записи Data Lake Analytics. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/write | Создание или обновление связанной учетной записи Data Lake Store для учетной записи Data Lake Analytics. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/delete | Удаляет учетную запись Data Lake Analytics. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/firewallRules/delete | Удаляет правило брандмауэра. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/firewallRules/read | Возвращает сведения о правиле брандмауэра. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/firewallRules/write | Создает или обновляет правило брандмауэра. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/operationResults/read | Получение результата операции учетной записи Data Lake Analytics. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/read | Получение сведений о существующей учетной записи Data Lake Analytics. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/listSasTokens/action | Получение списка токенов SAS для контейнеров хранилища учетной записи хранения, связанной с учетной записью Data Lake Analytics. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/read | Получение контейнеров учетной записи хранения, связанной с учетной записью Data Lake Analytics. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/storageAccounts/delete | Удаление связи учетной записи хранения с учетной записью Data Lake Analytics. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/storageAccounts/read | Получение сведений об учетной записи хранения, связанной с учетной записью Data Lake Analytics. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/storageAccounts/write | Создание или обновление связанной учетной записи хранения для учетной записи Data Lake Analytics. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Предоставляет разрешения на отмену заданий, отправленных другими пользователями. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/transferAnalyticsUnits/action | Передача значения SystemMaxAnalyticsUnits для учетных записей DataLakeAnalytics. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/virtualNetworkRules/delete | Удалите правило виртуальной сети. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/virtualNetworkRules/read | Получение сведений о правило виртуальной сети. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/virtualNetworkRules/write | Создать или обновить правило виртуальной сети. |
> | Действие | Microsoft.DataLakeAnalytics/accounts/write | Создание или обновление учетной записи Data Lake Analytics. |
> | Действие | Microsoft.DataLakeAnalytics/locations/capability/read | Получение сведений о возможностях подписки в отношении использования Data Lake Analytics. |
> | Действие | Microsoft.DataLakeAnalytics/locations/checkNameAvailability/action | Проверка доступности имени учетной записи Data Lake Analytics. |
> | Действие | Microsoft.DataLakeAnalytics/locations/operationResults/read | Получение результата операции учетной записи Data Lake Analytics. |
> | Действие | Microsoft.DataLakeAnalytics/locations/usages/read | Получение сведений об использовании квот подписки, относящихся к Data Lake Analytics. |
> | Действие | Microsoft.DataLakeAnalytics/operations/read | Получение доступных операций Data Lake Analytics. |
> | Действие | Microsoft.DataLakeAnalytics/register/action | Регистрация подписки для Data Lake Analytics. |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.DataLakeStore/accounts/delete | Удаление учетной записи Data Lake Store. |
> | Действие | Microsoft.DataLakeStore/accounts/enableKeyVault/action | Включение хранилища ключей для учетной записи Data Lake Store. |
> | Действие | Microsoft.DataLakeStore/accounts/eventGridFilters/delete | Удаление фильтра EventGrid. |
> | Действие | Microsoft.DataLakeStore/accounts/eventGridFilters/read | Получение фильтра EventGrid. |
> | Действие | Microsoft.DataLakeStore/accounts/eventGridFilters/write | Создание или обновление фильтра EventGrid. |
> | Действие | Microsoft.DataLakeStore/accounts/firewallRules/delete | Удаляет правило брандмауэра. |
> | Действие | Microsoft.DataLakeStore/accounts/firewallRules/read | Возвращает сведения о правиле брандмауэра. |
> | Действие | Microsoft.DataLakeStore/accounts/firewallRules/write | Создает или обновляет правило брандмауэра. |
> | Действие | Microsoft.DataLakeStore/accounts/operationResults/read | Получение результата операции учетной записи Data Lake Store. |
> | Действие | Microsoft.DataLakeStore/accounts/read | Получение сведений о существующей учетной записи Data Lake Store. |
> | Действие | Microsoft.DataLakeStore/accounts/Superuser/action; | Предоставление прав суперпользователя в хранилище Data Lake Store при предоставлении разрешения Microsoft.Authorization/roleAssignments/write. |
> | Действие | Microsoft.DataLakeStore/accounts/trustedIdProviders/delete | Удаляет доверенный поставщик удостоверений. |
> | Действие | Microsoft.DataLakeStore/accounts/trustedIdProviders/read | Возвращает сведения о доверенном поставщике удостоверений. |
> | Действие | Microsoft.DataLakeStore/accounts/trustedIdProviders/write | Создает или обновляет доверенный поставщик удостоверений. |
> | Действие | Microsoft.DataLakeStore/accounts/virtualNetworkRules/delete | Удалите правило виртуальной сети. |
> | Действие | Microsoft.DataLakeStore/accounts/virtualNetworkRules/read | Получение сведений о правило виртуальной сети. |
> | Действие | Microsoft.DataLakeStore/accounts/virtualNetworkRules/write | Создать или обновить правило виртуальной сети. |
> | Действие | Microsoft.DataLakeStore/accounts/write | Создание или обновление учетной записи Data Lake Store. |
> | Действие | Microsoft.DataLakeStore/locations/capability/read | Получение сведений о возможностях подписки в отношении использования Data Lake Store. |
> | Действие | Microsoft.DataLakeStore/locations/checkNameAvailability/action | Проверка доступности имени учетной записи Data Lake Store. |
> | Действие | Microsoft.DataLakeStore/locations/operationResults/read | Получение результата операции учетной записи Data Lake Store. |
> | Действие | Microsoft.DataLakeStore/locations/usages/read | Получение сведений об использовании квот подписки, относящихся к Data Lake Store. |
> | Действие | Microsoft.DataLakeStore/operations/read | Получение доступных операций Data Lake Store. |
> | Действие | Microsoft.DataLakeStore/register/action | Регистрация подписки для Data Lake Store. |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.DataMigration/locations/operationResults/read | Получение состояния длительной операции, связанной с ответом 202 (принято) |
> | Действие | Microsoft.DataMigration/locations/operationStatuses/read | Получение состояния длительной операции, связанной с ответом 202 (принято) |
> | Действие | Microsoft.DataMigration/register/action | Регистрация подписки в поставщике Azure Database Migration Service. |
> | Действие | Microsoft.DataMigration/services/addWorker/action | Добавляет рабочую роль DMS в список доступных рабочих ролей службы. |
> | Действие | Microsoft.DataMigration/services/checkStatus/action | Проверка развертывания и выполнения службы. |
> | Действие | Microsoft.DataMigration/services/configureWorker/action | Настраивает рабочую роль DMS для списка доступных рабочих ролей службы. |
> | Действие | Microsoft.DataMigration/services/delete | Удаление ресурса и всех его дочерних элементов. |
> | Действие | Microsoft.DataMigration/services/projects/accessArtifacts/action | Создание URL-адреса для операций GET и PUT с артефактами проекта. |
> | Действие | Microsoft.DataMigration/services/projects/delete | Удаление ресурса и всех его дочерних элементов. |
> | Действие | Microsoft.DataMigration/services/projects/files/delete | Удаление ресурса и всех его дочерних элементов. |
> | Действие | Microsoft.DataMigration/services/projects/files/read | Считывание сведений о ресурсах. |
> | Действие | Microsoft.DataMigration/services/projects/files/read/action | Получает URL-адрес для чтения содержимого файла. |
> | Действие | Microsoft.DataMigration/services/projects/files/readWrite/action | Получает URL-адрес для чтения или записи содержимого файла. |
> | Действие | Microsoft.DataMigration/services/projects/files/write | Создание или обновление ресурсов и их свойств. |
> | Действие | Microsoft.DataMigration/services/projects/read | Считывание сведений о ресурсах. |
> | Действие | Microsoft.DataMigration/services/projects/tasks/cancel/action | Отмена задачи (если она выполняется). |
> | Действие | Microsoft.DataMigration/services/projects/tasks/delete | Удаление ресурса и всех его дочерних элементов. |
> | Действие | Microsoft.DataMigration/services/projects/tasks/read | Считывание сведений о ресурсах. |
> | Действие | Microsoft.DataMigration/services/projects/tasks/write | Запуск задач Azure Database Migration Service. |
> | Действие | Microsoft.DataMigration/services/projects/write | Запуск задач Azure Database Migration Service. |
> | Действие | Microsoft.DataMigration/services/read | Считывание сведений о ресурсах. |
> | Действие | Microsoft.DataMigration/services/removeWorker/action | Удаляет рабочую роль DMS из списка доступных ролей службы. |
> | Действие | Microsoft.DataMigration/services/serviceTasks/cancel/action | Отмена задачи (если она выполняется). |
> | Действие | Microsoft.DataMigration/services/serviceTasks/delete | Удаление ресурса и всех его дочерних элементов. |
> | Действие | Microsoft.DataMigration/services/serviceTasks/read | Считывание сведений о ресурсах. |
> | Действие | Microsoft.DataMigration/services/serviceTasks/write | Запуск задач Azure Database Migration Service. |
> | Действие | Microsoft.DataMigration/services/slots/delete | Удаление ресурса и всех его дочерних элементов. |
> | Действие | Microsoft.DataMigration/services/slots/read | Считывание сведений о ресурсах. |
> | Действие | Microsoft.DataMigration/services/slots/write | Создание или обновление ресурсов и их свойств. |
> | Действие | Microsoft.DataMigration/services/start/action | Запуск службы DMS для возобновления обработки миграции. |
> | Действие | Microsoft.DataMigration/services/stop/action | Остановка службы DMS для сокращения затрат на нее. |
> | Действие | Microsoft.DataMigration/services/updateAgentConfig/action | Обновляет конфигурацию агента DMS с предоставленными значениями. |
> | Действие | Microsoft.DataMigration/services/write | Создание или обновление ресурсов и их свойств. |
> | Действие | Microsoft.DataMigration/skus/read | Получение списка номеров SKU, поддерживаемых ресурсами DMS. |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.DBforMariaDB/locations/azureAsyncOperation/read | Результаты операции возврата сервера MariaDB |
> | Действие | Microsoft.DBforMariaDB/locations/operationResults/read | Возвращаемое группа ресурсов на основе результатов операции сервера MariaDB |
> | Действие | Microsoft.DBforMariaDB/locations/operationResults/read | Результаты операции возврата сервера MariaDB |
> | Действие | Microsoft.DBforMariaDB/locations/performanceTiers/read | Получение списка доступных уровней производительности. |
> | Действие | Microsoft.DBforMariaDB/locations/securityAlertPoliciesAzureAsyncOperation/read | Возвращение списка результат операции обнаружения угроз на сервер. |
> | Действие | Microsoft.DBforMariaDB/locations/securityAlertPoliciesOperationResults/read | Возвращение списка результат операции обнаружения угроз на сервер. |
> | Действие | Microsoft.DBforMariaDB/operations/read | Возвращение списка операций MariaDB. |
> | Действие | Microsoft.DBforMariaDB/performanceTiers/read | Получение списка доступных уровней производительности. |
> | Действие | Microsoft.DBforMariaDB/register/action | Регистрация поставщика ресурсов MariaDB |
> | Действие | Microsoft.DBforMariaDB/servers/configurations/read | Возвращает список конфигураций для сервера или получает свойства для указанной конфигурации. |
> | Действие | Microsoft.DBforMariaDB/servers/configurations/write | Обновляет значение для указанной конфигурации. |
> | Действие | Microsoft.DBforMariaDB/servers/databases/delete | Удаляет существующую базу данных MariaDB. |
> | Действие | Microsoft.DBforMariaDB/servers/databases/read | Возвращение списка баз данных MariaDB или получение свойств для указанной базы данных. |
> | Действие | Microsoft.DBforMariaDB/servers/databases/write | Создает базу данных MariaDB с указанными параметрами либо обновление свойств указанной базы данных. |
> | Действие | Microsoft.DBforMariaDB/servers/delete | Удаление существующего сервера. |
> | Действие | Microsoft.DBforMariaDB/servers/firewallRules/delete | Удаление существующего правила брандмауэра. |
> | Действие | Microsoft.DBforMariaDB/servers/firewallRules/read | Возвращение списка правил брандмауэра для сервера или получение свойств для указанного правила брандмауэра. |
> | Действие | Microsoft.DBforMariaDB/servers/firewallRules/write | Создание правила брандмауэра с указанными параметрами или обновление существующего правила. |
> | Действие | Microsoft.DBforMariaDB/servers/logFiles/read | Возвращение списка MariaDB LogFiles. |
> | Действие | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметра диагностики для ресурса. |
> | Действие | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/diagnosticSettings/write | Создает или обновляет параметр диагностики для ресурса. |
> | Действие | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/logDefinitions/read | Получает доступные журналы для серверов MariaDB. |
> | Действие | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/metricDefinitions/read | Возвращает типы метрик, доступных для баз данных. |
> | Действие | Microsoft.DBforMariaDB/servers/queryTexts/action | Возвращать тексты список запросов |
> | Действие | Microsoft.DBforMariaDB/servers/read | Возвращение списка серверов или получение свойств для указанного сервера. |
> | Действие | Microsoft.DBforMariaDB/servers/recoverableServers/read | Возвращает восстанавливаемые данные сервера MariaDB |
> | Действие | Microsoft.DBforMariaDB/servers/securityAlertPolicies/read | Извлекает сведения о политике обнаружения угроз, настроенной на данном сервере. |
> | Действие | Microsoft.DBforMariaDB/servers/securityAlertPolicies/write | Изменение политики обнаружения угроз для заданного сервера. |
> | Действие | Microsoft.DBforMariaDB/servers/topQueryStatistics/read | Возвращает список статистики запросов для наиболее ресурсоемких запросов. |
> | Действие | Microsoft.DBforMariaDB/servers/updateConfigurations/action | Обновляет конфигурации для указанного сервера. |
> | Действие | Microsoft.DBforMariaDB/servers/virtualNetworkRules/delete | Удаление существующего правила виртуальной сети. |
> | Действие | Microsoft.DBforMariaDB/servers/virtualNetworkRules/read | Возвращение списка правил виртуальной сети или свойств указанного правила виртуальной сети. |
> | Действие | Microsoft.DBforMariaDB/servers/virtualNetworkRules/write | Создание правила виртуальной сети с указанными параметрами либо обновление свойств или тегов указанного правила виртуальной сети. |
> | Действие | Microsoft.DBforMariaDB/servers/waitStatistics/read | Возвращает статистику ожидания для экземпляра |
> | Действие | Microsoft.DBforMariaDB/servers/write | Создание сервера с указанными параметрами либо обновление свойств или тегов указанного сервера. |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.DBforMySQL/locations/azureAsyncOperation/read | Результаты операции возврата сервера MySQL |
> | Действие | Microsoft.DBforMySQL/locations/operationResults/read | Возвращаемое группа ресурсов на основе результатов операции сервера MySQL |
> | Действие | Microsoft.DBforMySQL/locations/operationResults/read | Результаты операции возврата сервера MySQL |
> | Действие | Microsoft.DBforMySQL/locations/performanceTiers/read | Получение списка доступных уровней производительности. |
> | Действие | Microsoft.DBforMySQL/locations/securityAlertPoliciesAzureAsyncOperation/read | Возвращение списка результат операции обнаружения угроз на сервер. |
> | Действие | Microsoft.DBforMySQL/locations/securityAlertPoliciesOperationResults/read | Возвращение списка результат операции обнаружения угроз на сервер. |
> | Действие | Microsoft.DBforMySQL/operations/read | Возвращение списка операций MySQL. |
> | Действие | Microsoft.DBforMySQL/performanceTiers/read | Получение списка доступных уровней производительности. |
> | Действие | Microsoft.DBforMySQL/register/action | Регистрация поставщика ресурсов MySQL |
> | Действие | Microsoft.DBforMySQL/servers/configurations/read | Возвращает список конфигураций для сервера или получает свойства для указанной конфигурации. |
> | Действие | Microsoft.DBforMySQL/servers/configurations/write | Обновляет значение для указанной конфигурации. |
> | Действие | Microsoft.DBforMySQL/servers/databases/delete | Удаляет существующую базу данных MySQL. |
> | Действие | Microsoft.DBforMySQL/servers/databases/read | Возвращение списка баз данных MySQL или получение свойств для указанной базы данных. |
> | Действие | Microsoft.DBforMySQL/servers/databases/write | Создает базу данных MySQL с указанными параметрами либо обновление свойств указанной базы данных. |
> | Действие | Microsoft.DBforMySQL/servers/delete | Удаление существующего сервера. |
> | Действие | Microsoft.DBforMySQL/servers/firewallRules/delete | Удаление существующего правила брандмауэра. |
> | Действие | Microsoft.DBforMySQL/servers/firewallRules/read | Возвращение списка правил брандмауэра для сервера или получение свойств для указанного правила брандмауэра. |
> | Действие | Microsoft.DBforMySQL/servers/firewallRules/write | Создание правила брандмауэра с указанными параметрами или обновление существующего правила. |
> | Действие | Microsoft.DBforMySQL/servers/logFiles/read | Возвращение списка PostgreSQL LogFiles. |
> | Действие | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметра диагностики для ресурса. |
> | Действие | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Создает или обновляет параметр диагностики для ресурса. |
> | Действие | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/logDefinitions/read | Получает доступные журналы для серверов MySQL |
> | Действие | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Возвращает типы метрик, доступных для баз данных. |
> | Действие | Microsoft.DBforMySQL/servers/queryTexts/action | Возвращать тексты список запросов |
> | Действие | Microsoft.DBforMySQL/servers/read | Возвращение списка серверов или получение свойств для указанного сервера. |
> | Действие | Microsoft.DBforMySQL/servers/recoverableServers/read | Возвращение восстанавливаемых данных MySQL Server. |
> | Действие | Microsoft.DBforMySQL/servers/securityAlertPolicies/read | Извлекает сведения о политике обнаружения угроз, настроенной на данном сервере. |
> | Действие | Microsoft.DBforMySQL/servers/securityAlertPolicies/write | Изменение политики обнаружения угроз для заданного сервера. |
> | Действие | Microsoft.DBforMySQL/servers/topQueryStatistics/read | Возвращает список статистики запросов для наиболее ресурсоемких запросов. |
> | Действие | Microsoft.DBforMySQL/servers/updateConfigurations/action | Обновляет конфигурации для указанного сервера. |
> | Действие | Microsoft.DBforMySQL/servers/virtualNetworkRules/delete | Удаление существующего правила виртуальной сети. |
> | Действие | Microsoft.DBforMySQL/servers/virtualNetworkRules/read | Возвращение списка правил виртуальной сети или свойств указанного правила виртуальной сети. |
> | Действие | Microsoft.DBforMySQL/servers/virtualNetworkRules/write | Создание правила виртуальной сети с указанными параметрами либо обновление свойств или тегов указанного правила виртуальной сети. |
> | Действие | Microsoft.DBforMySQL/servers/waitStatistics/read | Возвращает статистику ожидания для экземпляра |
> | Действие | Microsoft.DBforMySQL/servers/write | Создание сервера с указанными параметрами либо обновление свойств или тегов указанного сервера. |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.DBforPostgreSQL/locations/azureAsyncOperation/read | Результаты операции возврата сервера PostgreSQL |
> | Действие | Microsoft.DBforPostgreSQL/locations/operationResults/read | Возвращаемое группа ресурсов на основе результатов операции сервера PostgreSQL |
> | Действие | Microsoft.DBforPostgreSQL/locations/operationResults/read | Результаты операции возврата сервера PostgreSQL |
> | Действие | Microsoft.DBforPostgreSQL/locations/performanceTiers/read | Получение списка доступных уровней производительности. |
> | Действие | Microsoft.DBforPostgreSQL/locations/securityAlertPoliciesAzureAsyncOperation/read | Возвращение списка результат операции обнаружения угроз на сервер. |
> | Действие | Microsoft.DBforPostgreSQL/locations/securityAlertPoliciesOperationResults/read | Возвращение списка результат операции обнаружения угроз на сервер. |
> | Действие | Microsoft.DBforPostgreSQL/operations/read | Возвращение списка операций PostgreSQL. |
> | Действие | Microsoft.DBforPostgreSQL/performanceTiers/read | Получение списка доступных уровней производительности. |
> | Действие | Microsoft.DBforPostgreSQL/register/action | Регистрация поставщика ресурсов PostgreSQL |
> | Действие | Microsoft.DBforPostgreSQL/servers/advisors/read | Возвращает список консультантов. |
> | Действие | Microsoft.DBforPostgreSQL/servers/advisors/recommendedActions/read | Возвращает список рекомендуемых действий |
> | Действие | Microsoft.DBforPostgreSQL/servers/advisors/recommendedActionSessions/action | Предоставление рекомендаций |
> | Действие | Microsoft.DBforPostgreSQL/servers/configurations/read | Возвращает список конфигураций для сервера или получает свойства для указанной конфигурации. |
> | Действие | Microsoft.DBforPostgreSQL/servers/configurations/write | Обновляет значение для указанной конфигурации. |
> | Действие | Microsoft.DBforPostgreSQL/servers/databases/delete | Удаляет существующую базу данных PostgreSQL. |
> | Действие | Microsoft.DBforPostgreSQL/servers/databases/read | Возвращение списка базы данных PostgreSQL или получение свойств для указанной базы данных. |
> | Действие | Microsoft.DBforPostgreSQL/servers/databases/write | Создает базу данных PostgreSQL с указанными параметрами либо обновление свойств указанной базы данных. |
> | Действие | Microsoft.DBforPostgreSQL/servers/delete | Удаление существующего сервера. |
> | Действие | Microsoft.DBforPostgreSQL/servers/firewallRules/delete | Удаление существующего правила брандмауэра. |
> | Действие | Microsoft.DBforPostgreSQL/servers/firewallRules/read | Возвращение списка правил брандмауэра для сервера или получение свойств для указанного правила брандмауэра. |
> | Действие | Microsoft.DBforPostgreSQL/servers/firewallRules/write | Создание правила брандмауэра с указанными параметрами или обновление существующего правила. |
> | Действие | Microsoft.DBforPostgreSQL/servers/logFiles/read | Возвращение списка PostgreSQL LogFiles. |
> | Действие | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметра диагностики для ресурса. |
> | Действие | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Создает или обновляет параметр диагностики для ресурса. |
> | Действие | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/logDefinitions/read | Получает доступные журналы серверов Postgres. |
> | Действие | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Возвращает типы метрик, доступных для баз данных. |
> | Действие | Microsoft.DBforPostgreSQL/servers/queryTexts/action | Возвращает текст запроса. |
> | Действие | Microsoft.DBforPostgreSQL/servers/queryTexts/read | Возвращать тексты список запросов |
> | Действие | Microsoft.DBforPostgreSQL/servers/read | Возвращение списка серверов или получение свойств для указанного сервера. |
> | Действие | Microsoft.DBforPostgreSQL/servers/recoverableServers/read | Возвращение восстанавливаемых данных PostgreSQL Server. |
> | Действие | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/read | Извлекает сведения о политике обнаружения угроз, настроенной на данном сервере. |
> | Действие | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/write | Изменение политики обнаружения угроз для заданного сервера. |
> | Действие | Microsoft.DBforPostgreSQL/servers/topQueryStatistics/read | Возвращает список статистики запросов для наиболее ресурсоемких запросов. |
> | Действие | Microsoft.DBforPostgreSQL/servers/updateConfigurations/action | Обновляет конфигурации для указанного сервера. |
> | Действие | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/delete | Удаление существующего правила виртуальной сети. |
> | Действие | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/read | Возвращение списка правил виртуальной сети или свойств указанного правила виртуальной сети. |
> | Действие | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/write | Создание правила виртуальной сети с указанными параметрами либо обновление свойств или тегов указанного правила виртуальной сети. |
> | Действие | Microsoft.DBforPostgreSQL/servers/waitStatistics/read | Возвращает статистику ожидания для экземпляра |
> | Действие | Microsoft.DBforPostgreSQL/servers/write | Создание сервера с указанными параметрами либо обновление свойств или тегов указанного сервера. |
> | Действие | Microsoft.DBforPostgreSQL/serversv2/configurations/read | Возвращает список конфигураций для сервера или получает свойства для указанной конфигурации. |
> | Действие | Microsoft.DBforPostgreSQL/serversv2/configurations/write | Обновляет значение для указанной конфигурации. |
> | Действие | Microsoft.DBforPostgreSQL/serversv2/delete | Удаление существующего сервера. |
> | Действие | Microsoft.DBforPostgreSQL/serversv2/firewallRules/delete | Удаление существующего правила брандмауэра. |
> | Действие | Microsoft.DBforPostgreSQL/serversv2/firewallRules/read | Возвращение списка правил брандмауэра для сервера или получение свойств для указанного правила брандмауэра. |
> | Действие | Microsoft.DBforPostgreSQL/serversv2/firewallRules/write | Создание правила брандмауэра с указанными параметрами или обновление существующего правила. |
> | Действие | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметра диагностики для ресурса. |
> | Действие | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/diagnosticSettings/write | Создает или обновляет параметр диагностики для ресурса. |
> | Действие | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/logDefinitions/read | Получает доступные журналы серверов Postgres. |
> | Действие | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/metricDefinitions/read | Возвращает типы метрик, доступных для баз данных. |
> | Действие | Microsoft.DBforPostgreSQL/serversv2/read | Возвращение списка серверов или получение свойств для указанного сервера. |
> | Действие | Microsoft.DBforPostgreSQL/serversv2/updateConfigurations/action | Обновляет конфигурации для указанного сервера. |
> | Действие | Microsoft.DBforPostgreSQL/serversv2/write | Создание сервера с указанными параметрами либо обновление свойств или тегов указанного сервера. |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Devices/checkNameAvailability/Action | Проверяет, доступно ли имя Центра Интернета вещей. |
> | Действие | Microsoft.Devices/checkProvisioningServiceNameAvailability/Action | Проверяет, доступно ли имя службы подготовки. |
> | Действие | Microsoft.Devices/ElasticPools/diagnosticSettings/read | Возвращает параметр диагностики для ресурса. |
> | Действие | Microsoft.Devices/ElasticPools/diagnosticSettings/write | Создает или обновляет параметр диагностики для ресурса. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Delete | Удаляет сертификат. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/certificates/generateVerificationCode/Action | Создание кода проверки. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Read | Возвращает сертификат. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/certificates/verify/Action | Проверка ресурса сертификата. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Write | Создание или обновление сертификата. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/Delete | Удаление ресурса клиента Центра Интернета вещей. |
> | Действие | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/read | Возвращает параметр диагностики для ресурса. |
> | Действие | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/write | Создает или обновляет параметр диагностики для ресурса. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Delete | Удаляет группу потребителей концентратора событий. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Read | Возвращает группы потребителей концентратора событий. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Write | Создает группу потребителей концентратора событий. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/exportDevices/Action | Экспортирует устройства. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/getStats/Read | Получение ресурса статистики клиента Центра Интернета вещей. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/importDevices/Action | Импортирует устройства. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/iotHubKeys/listkeys/Action | Получение ключа клиента Центра Интернета вещей. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/jobs/Read | Возвращает сведения о заданиях, отправленных в заданный Центр Интернета вещей. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/listKeys/Action | Получение ключей клиента Центра Интернета вещей. |
> | Действие | Microsoft.Devices/ElasticPools/IotHubTenants/logDefinitions/read | Получение доступных определений журналов для службы Центра Интернета вещей. |
> | Действие | Microsoft.Devices/ElasticPools/IotHubTenants/metricDefinitions/read | Возвращает доступные метрики для службы Центра Интернета вещей. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/quotaMetrics/Read | Возвращает метрики квоты. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/Read | Получение ресурса клиента Центра Интернета вещей. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testall/Action | Тестирует сообщение с помощью всех существующих маршрутов. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testnew/Action | Тестирует сообщение с помощью предоставленного тестового маршрута. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/routingEndpointsHealth/Read | Возвращает сведения о работоспособности всех конечных точек маршрутизации для Центра Интернета вещей. |
> | Действие | Microsoft.Devices/elasticPools/iotHubTenants/Write | Создание или обновление ресурса клиента Центра Интернета вещей. |
> | Действие | Microsoft.Devices/ElasticPools/metricDefinitions/read | Возвращает доступные метрики для службы Центра Интернета вещей. |
> | Действие | Microsoft.Devices/iotHubs/certificates/Delete | Удаляет сертификат. |
> | Действие | Microsoft.Devices/iotHubs/certificates/generateVerificationCode/Action | Создание кода проверки. |
> | Действие | Microsoft.Devices/iotHubs/certificates/Read | Возвращает сертификат. |
> | Действие | Microsoft.Devices/iotHubs/certificates/verify/Action | Проверка ресурса сертификата. |
> | Действие | Microsoft.Devices/iotHubs/certificates/Write | Создание или обновление сертификата. |
> | Действие | Microsoft.Devices/iotHubs/Delete | Удаляет ресурс Центра Интернета вещей. |
> | Действие | Microsoft.Devices/IotHubs/diagnosticSettings/read | Возвращает параметр диагностики для ресурса. |
> | Действие | Microsoft.Devices/IotHubs/diagnosticSettings/write | Создает или обновляет параметр диагностики для ресурса. |
> | Действие | Microsoft.Devices/iotHubs/eventGridFilters/Delete | Удаление фильтра сетки событий. |
> | Действие | Microsoft.Devices/iotHubs/eventGridFilters/Read | Получение фильтра сетки событий. |
> | Действие | Microsoft.Devices/iotHubs/eventGridFilters/Write | Создание фильтра сетки событий или обновление существующего. |
> | Действие | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Delete | Удаляет группу потребителей концентратора событий. |
> | Действие | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Read | Возвращает группы потребителей концентратора событий. |
> | Действие | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Write | Создает группу потребителей концентратора событий. |
> | Действие | Microsoft.Devices/iotHubs/exportDevices/Action | Экспортирует устройства. |
> | Действие | Microsoft.Devices/iotHubs/importDevices/Action | Импортирует устройства. |
> | Действие | Microsoft.Devices/iotHubs/iotHubKeys/listkeys/Action | Возвращает ключ Центра Интернета вещей для указанного имени. |
> | Действие | Microsoft.Devices/iotHubs/iotHubStats/Read | Возвращает статистику Центра Интернета вещей. |
> | Действие | Microsoft.Devices/iotHubs/jobs/Read | Возвращает сведения о заданиях, отправленных в заданный Центр Интернета вещей. |
> | Действие | Microsoft.Devices/iotHubs/listkeys/Action | Возвращает все ключи Центра Интернета вещей. |
> | Действие | Microsoft.Devices/IotHubs/logDefinitions/read | Получение доступных определений журналов для службы Центра Интернета вещей. |
> | Действие | Microsoft.Devices/IotHubs/metricDefinitions/read | Возвращает доступные метрики для службы Центра Интернета вещей. |
> | Действие | Microsoft.Devices/iotHubs/operationresults/Read | Получение результата операции (устаревший API). |
> | Действие | Microsoft.Devices/iotHubs/quotaMetrics/Read | Возвращает метрики квоты. |
> | Действие | Microsoft.Devices/iotHubs/Read | Возвращает ресурсы Центра Интернета вещей. |
> | Действие | Microsoft.Devices/iotHubs/routing/$testall/Action | Тестирует сообщение с помощью всех существующих маршрутов. |
> | Действие | Microsoft.Devices/iotHubs/routing/$testnew/Action | Тестирует сообщение с помощью предоставленного тестового маршрута. |
> | Действие | Microsoft.Devices/iotHubs/routingEndpointsHealth/Read | Возвращает сведения о работоспособности всех конечных точек маршрутизации для Центра Интернета вещей. |
> | Действие | Microsoft.Devices/iotHubs/skus/Read | Возвращает допустимые номера SKU Центра Интернета вещей. |
> | Действие | Microsoft.Devices/iotHubs/Write | Создает или обновляет ресурс Центра Интернета вещей. |
> | Действие | Microsoft.Devices/locations/operationresults/Read | Получает результат операции на основе расположения. |
> | Действие | Microsoft.Devices/operationresults/Read | Получение результата операции. |
> | Действие | Microsoft.Devices/operations/Read | Возвращает все операции поставщика ресурсов. |
> | Действие | Microsoft.Devices/provisioningServices/certificates/Delete | Удаляет сертификат. |
> | Действие | Microsoft.Devices/provisioningServices/certificates/generateVerificationCode/Action | Создание кода проверки. |
> | Действие | Microsoft.Devices/provisioningServices/certificates/Read | Возвращает сертификат. |
> | Действие | Microsoft.Devices/provisioningServices/certificates/verify/Action | Проверка ресурса сертификата. |
> | Действие | Microsoft.Devices/provisioningServices/certificates/Write | Создание или обновление сертификата. |
> | Действие | Microsoft.Devices/provisioningServices/Delete | Удаление ресурса службы политики диагностики (DPS) Центра Интернета вещей. |
> | Действие | Microsoft.Devices/provisioningServices/diagnosticSettings/read | Возвращает параметр диагностики для ресурса. |
> | Действие | Microsoft.Devices/provisioningServices/diagnosticSettings/write | Создает или обновляет параметр диагностики для ресурса. |
> | Действие | Microsoft.Devices/provisioningServices/keys/listkeys/Action | Получение ключей DPS Центра Интернета вещей для имени ключа. |
> | Действие | Microsoft.Devices/provisioningServices/listkeys/Action | Получение ключей DPS Центра Интернета вещей. |
> | Действие | Microsoft.Devices/provisioningServices/logDefinitions/read | Получение доступных определений журналов для службы подготовки. |
> | Действие | Microsoft.Devices/provisioningServices/metricDefinitions/read | Получение доступных метрик для службы подготовки. |
> | Действие | Microsoft.Devices/provisioningServices/operationresults/Read | Получение результата операции DPS. |
> | Действие | Microsoft.Devices/provisioningServices/Read | Получение ресурса DPS Центра Интернета вещей. |
> | Действие | Microsoft.Devices/provisioningServices/skus/Read | Получение действительных номеров SKU для DPS Центра Интернета вещей. |
> | Действие | Microsoft.Devices/provisioningServices/Write | Создание ресурса DPS для Центра Интернета вещей. |
> | Действие | Microsoft.Devices/register/action | Регистрирует подписку в поставщике ресурсов Центра Интернета вещей и позволяет создавать ресурсы Центра Интернета вещей. |
> | Действие | Microsoft.Devices/register/action | Регистрирует подписку в поставщике ресурсов Центра Интернета вещей и позволяет создавать ресурсы Центра Интернета вещей. |
> | Действие | Microsoft.Devices/usages/Read | Возвращает данные об использовании подписки для данного поставщика. |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.DevSpaces/controllers/delete | Удаление контроллера Azure Dev Spaces Controller и служб плоскости данных |
> | Действие | Microsoft.DevSpaces/controllers/listConnectionDetails/action | Сведения о подключении списка для инфраструктуры контроллера Azure Dev Spaces |
> | Действие | Microsoft.DevSpaces/controllers/read | Чтение свойств контроллера Azure Dev Spaces |
> | Действие | Microsoft.DevSpaces/controllers/write | Создание или обновление свойств контроллера Azure Dev Spaces |
> | Действие | Microsoft.DevSpaces/locations/checkContainerHostMapping/action | Проверьте существующие сопоставления контроллера для узла контейнера |
> | Действие | Microsoft.DevSpaces/locations/operationresults/read | Чтение состояния асинхронной операции |
> | Действие | Microsoft.DevSpaces/register/action | Создание поставщика ресурсов Microsoft Dev Spaces с подпиской |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.DevTestLab/labCenters/delete | Удаление центров лабораторий. |
> | Действие | Microsoft.DevTestLab/labCenters/read | Чтение центров лабораторий. |
> | Действие | Microsoft.DevTestLab/labCenters/write | Добавление или изменение центров лабораторий. |
> | Действие | Microsoft.DevTestLab/labs/artifactSources/armTemplates/read | Считывает шаблоны Azure Resource Manager. |
> | Действие | Microsoft.DevTestLab/labs/artifactSources/artifacts/GenerateArmTemplate/action | Создает шаблон ARM для заданного артефакта, передает необходимые файлы в учетную запись хранения и проверяет созданный артефакт. |
> | Действие | Microsoft.DevTestLab/labs/artifactSources/artifacts/read | Считывает артефакты. |
> | Действие | Microsoft.DevTestLab/labs/artifactSources/delete | Удаляет источники артефактов. |
> | Действие | Microsoft.DevTestLab/labs/artifactSources/read | Считывает источники артефактов. |
> | Действие | Microsoft.DevTestLab/labs/artifactSources/write | Добавляет или изменяет источники артефактов. |
> | Действие | Microsoft.DevTestLab/labs/ClaimAnyVm/action | Отправляет заявку на доступ к случайной запрашиваемой виртуальной машине в лаборатории. |
> | Действие | Microsoft.DevTestLab/labs/costs/read | Считывает затраты. |
> | Действие | Microsoft.DevTestLab/labs/costs/write | Добавляет или изменяет затраты. |
> | Действие | Microsoft.DevTestLab/labs/CreateEnvironment/action | Создает виртуальные машины в лаборатории. |
> | Действие | Microsoft.DevTestLab/labs/customImages/delete | Удаляет пользовательские образы. |
> | Действие | Microsoft.DevTestLab/labs/customImages/read | Считывает пользовательские образы. |
> | Действие | Microsoft.DevTestLab/labs/customImages/write | Добавляет или изменяет пользовательские образы. |
> | Действие | Microsoft.DevTestLab/labs/delete | Удаляет лаборатории. |
> | Действие | Microsoft.DevTestLab/labs/EnsureCurrentUserProfile/action | Убедитесь, что текущий пользователь имеет допустимый профиль в лаборатории. |
> | Действие | Microsoft.DevTestLab/labs/ExportResourceUsage/action | Экспортирует данные об использовании ресурсов лаборатории в учетную запись хранения. |
> | Действие | Microsoft.DevTestLab/labs/formulas/delete | Удаляет формулы. |
> | Действие | Microsoft.DevTestLab/labs/formulas/read | Считывает формулы. |
> | Действие | Microsoft.DevTestLab/labs/formulas/write | Добавляет или изменяет формулы. |
> | Действие | Microsoft.DevTestLab/labs/galleryImages/read | Считывает образы из коллекции. |
> | Действие | Microsoft.DevTestLab/labs/GenerateUploadUri/action | Создает универсальный код ресурса (URI) для передачи настраиваемых образов дисков в лабораторию. |
> | Действие | Microsoft.DevTestLab/labs/ImportVirtualMachine/action | Импорт виртуальной машины в другую лабораторию. |
> | Действие | Microsoft.DevTestLab/labs/ListVhds/action | Выводит список образов дисков, доступных для создания пользовательского образа. |
> | Действие | Microsoft.DevTestLab/labs/notificationChannels/delete | Удаляет каналы уведомлений. |
> | Действие | Microsoft.DevTestLab/labs/notificationChannels/Notify/action | Отправляет уведомление в указанный канал. |
> | Действие | Microsoft.DevTestLab/labs/notificationChannels/read | Считывает каналы уведомлений. |
> | Действие | Microsoft.DevTestLab/labs/notificationChannels/write | Добавляет или изменяет каналы уведомлений. |
> | Действие | Microsoft.DevTestLab/labs/policySets/EvaluatePolicies/action | Оценивает политику лаборатории. |
> | Действие | Microsoft.DevTestLab/labs/policySets/policies/delete | Удаляет политики. |
> | Действие | Microsoft.DevTestLab/labs/policySets/policies/read | Считывает политики. |
> | Действие | Microsoft.DevTestLab/labs/policySets/policies/write | Добавляет или изменяет политики. |
> | Действие | Microsoft.DevTestLab/labs/policySets/read | Чтение наборов политик. |
> | Действие | Microsoft.DevTestLab/labs/read | Считывает лаборатории. |
> | Действие | Microsoft.DevTestLab/labs/schedules/delete | Удаляет расписания. |
> | Действие | Microsoft.DevTestLab/labs/schedules/Execute/action | Выполняет расписание. |
> | Действие | Microsoft.DevTestLab/labs/schedules/ListApplicable/action | Выводит список всех применимых расписаний. |
> | Действие | Microsoft.DevTestLab/labs/schedules/read | Считывает расписания. |
> | Действие | Microsoft.DevTestLab/labs/schedules/write | Добавляет или изменяет расписания. |
> | Действие | Microsoft.DevTestLab/labs/serviceRunners/delete | Удаляет средства выполнения служб. |
> | Действие | Microsoft.DevTestLab/labs/serviceRunners/read | Считывает средства выполнения служб. |
> | Действие | Microsoft.DevTestLab/labs/serviceRunners/write | Добавляет или изменяет средства выполнения служб. |
> | Действие | Microsoft.DevTestLab/labs/sharedGalleries/delete | Удаление общей коллекции. |
> | Действие | Microsoft.DevTestLab/labs/sharedGalleries/read | Чтение общего галереи. |
> | Действие | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/delete | Удалите общие образы. |
> | Действие | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/read | Прочитайте общие образы. |
> | Действие | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/write | Добавляет или изменяет общие образы. |
> | Действие | Microsoft.DevTestLab/labs/sharedGalleries/write | Добавление или изменение общих коллекций. |
> | Действие | Microsoft.DevTestLab/labs/users/delete | Удаляет профили пользователей. |
> | Действие | Microsoft.DevTestLab/labs/users/disks/Attach/action | Подключает диск к виртуальной машине и создает аренду для него. |
> | Действие | Microsoft.DevTestLab/labs/users/disks/delete | Удаляет диски. |
> | Действие | Microsoft.DevTestLab/labs/users/disks/Detach/action | Отключает диск от виртуальной машины и прерывает его аренду. |
> | Действие | Microsoft.DevTestLab/labs/users/disks/read | Считывает диски. |
> | Действие | Microsoft.DevTestLab/labs/users/disks/write | Добавляет или изменяет диски. |
> | Действие | Microsoft.DevTestLab/labs/users/environments/delete | Удаляет среды. |
> | Действие | Microsoft.DevTestLab/labs/users/environments/read | Считывает среды. |
> | Действие | Microsoft.DevTestLab/labs/users/environments/write | Добавляет или изменяет среды. |
> | Действие | Microsoft.DevTestLab/labs/users/read | Считывает профили пользователей. |
> | Действие | Microsoft.DevTestLab/labs/users/secrets/delete | Удаляет секреты. |
> | Действие | Microsoft.DevTestLab/labs/users/secrets/read | Считывает секреты. |
> | Действие | Microsoft.DevTestLab/labs/users/secrets/write | Добавляет или изменяет секреты. |
> | Действие | Microsoft.DevTestLab/labs/users/serviceFabrics/delete | Удаление платформ Service Fabric. |
> | Действие | Microsoft.DevTestLab/labs/users/serviceFabrics/ListApplicableSchedules/action | Выводит список применимых расписаний запуска и остановки, если они есть. |
> | Действие | Microsoft.DevTestLab/labs/users/serviceFabrics/read | Чтение платформ Service Fabric. |
> | Действие | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/delete | Удаляет расписания. |
> | Действие | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/Execute/action | Выполняет расписание. |
> | Действие | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/read | Считывает расписания. |
> | Действие | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/write | Добавляет или изменяет расписания. |
> | Действие | Microsoft.DevTestLab/labs/users/serviceFabrics/Start/action | Запуск платформы Service Fabric. |
> | Действие | Microsoft.DevTestLab/labs/users/serviceFabrics/Stop/action | Остановка платформы Service Fabric. |
> | Действие | Microsoft.DevTestLab/labs/users/serviceFabrics/write | Добавление или изменение платформ Service Fabric. |
> | Действие | Microsoft.DevTestLab/labs/users/write | Добавляет или изменяет профили пользователей. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/AddDataDisk/action | Подключает новый или существующий диск данных к виртуальной машине. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/ApplyArtifacts/action | Применяет артефакты к виртуальной машине. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/Claim/action | Присваивает владение существующей виртуальной машиной. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/delete | Удаляет виртуальные машины. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/DetachDataDisk/action | Отключает указанный диск от виртуальной машины. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/GetRdpFileContents/action | Получает строку, которая представляет содержание RDP-файла для виртуальной машины. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/ListApplicableSchedules/action | Выводит список применимых расписаний запуска и остановки, если они есть. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/read | Считывает виртуальные машины. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/Redeploy/action | Повторное развертывание виртуальной машины. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/Resize/action | Изменение размера виртуальной машины. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/Restart/action | Перезапуск виртуальной машины. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/schedules/delete | Удаляет расписания. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/schedules/Execute/action | Выполняет расписание. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/schedules/read | Считывает расписания. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/schedules/write | Добавляет или изменяет расписания. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/Start/action | Запускает виртуальную машину. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/Stop/action | Остановка виртуальной машины |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/TransferDisks/action | Переносит все диски с данными, подключенные к виртуальной машине, к текущему пользователю. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/UnClaim/action | Отказ от владения существующей виртуальной машиной. |
> | Действие | Microsoft.DevTestLab/labs/virtualMachines/write | Добавляет или изменяет виртуальные машины. |
> | Действие | Microsoft.DevTestLab/labs/virtualNetworks/delete | Удаляет виртуальные сети. |
> | Действие | Microsoft.DevTestLab/labs/virtualNetworks/read | Считывает виртуальные сети. |
> | Действие | Microsoft.DevTestLab/labs/virtualNetworks/write | Добавляет или изменяет виртуальные сети. |
> | Действие | Microsoft.DevTestLab/labs/vmPools/delete | Удаление пулов виртуальных машин. |
> | Действие | Microsoft.DevTestLab/labs/vmPools/read | Считывание пулов виртуальных машин. |
> | Действие | Microsoft.DevTestLab/labs/vmPools/write | Добавление или изменение пулов виртуальных машин. |
> | Действие | Microsoft.DevTestLab/labs/write | Добавляет или изменяет лаборатории. |
> | Действие | Microsoft.DevTestLab/locations/operations/read | Считывает операции. |
> | Действие | Microsoft.DevTestLab/register/action | Регистрирует подписку. |
> | Действие | Microsoft.DevTestLab/schedules/delete | Удаляет расписания. |
> | Действие | Microsoft.DevTestLab/schedules/Execute/action | Выполняет расписание. |
> | Действие | Microsoft.DevTestLab/schedules/read | Считывает расписания. |
> | Действие | Microsoft.DevTestLab/schedules/Retarget/action | Обновляет идентификатор целевого ресурса расписания. |
> | Действие | Microsoft.DevTestLab/schedules/write | Добавляет или изменяет расписания. |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.DocumentDB/databaseAccountNames/read | Проверяет доступность имени. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/delete | Удаление коллекции. Применимо только к типам API: «mongodb». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/read | Чтение коллекцию или перечисление всех коллекций. Применимо только к типам API: «mongodb». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/settings/throughput/read | Чтение коллекции еще и пропускную способность. Применимо только к типам API: «mongodb». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/settings/throughput/write | Обновление коллекции еще и пропускную способность. Применимо только к типам API: «mongodb». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/write | Создание или обновление коллекции. Применимо только к типам API: «mongodb». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/delete | Удаление контейнера. Применимо только к типам API: «sql». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/read | Чтение контейнера или перечислить все контейнеры. Применимо только к типам API: «sql». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/settings/throughput/read | Чтение пропускной способности контейнера. Применимо только к типам API: «sql». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/settings/throughput/write | Обновление пропускной способности контейнера. Применимо только к типам API: «sql». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/write | Создание или обновление контейнера. Применимо только к типам API: «sql». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/delete | Удаление базы данных. Применимо только к типам API: «sql», «mongodb», «gremlin». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/delete | Удаление графа. Применимо только к типам API: «gremlin». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/read | Чтение графа или перечисление всех диаграмм. Применимо только к типам API: «gremlin». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/settings/throughput/read | Чтение графа еще и пропускную способность. Применимо только к типам API: «gremlin». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/settings/throughput/write | Обновите пропускную способность графа. Применимо только к типам API: «gremlin». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/write | Создать или обновить диаграмму. Применимо только к типам API: «gremlin». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/read | Чтение базы данных или список всех баз данных. Применимо только к типам API: «sql», «mongodb», «gremlin». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/settings/throughput/read | Чтение пропускную способность баз данных. Применимо только к типам API: «sql», «mongodb», «gremlin». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/settings/throughput/write | Обновите пропускную способность баз данных. Применимо только к типам API: «sql», «mongodb», «gremlin». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/databases/write | создавать базу данных; Применимо только к типам API: «sql», «mongodb», «gremlin». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/delete | Удалите ключевое пространство. Применимо только к типам API: «cassandra». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/read | Чтение пространства ключей или перечисление всех пространств ключей. Применимо только к типам API: «cassandra». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/settings/throughput/read | Чтение пространства ключей еще и пропускную способность. Применимо только к типам API: «cassandra». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/settings/throughput/write | Обновите пространства ключей еще и пропускную способность. Применимо только к типам API: «cassandra». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/delete | Удаление таблицы. Применимо только к типам API: «cassandra». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/read | Чтение таблицы или списка всех таблиц. Применимо только к типам API: «cassandra». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/settings/throughput/read | Чтение таблицы еще и пропускную способность. Применимо только к типам API: «cassandra». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/settings/throughput/write | Обновите таблицу еще и пропускную способность. Применимо только к типам API: «cassandra». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/write | Создание или обновление таблицы. Применимо только к типам API: «cassandra». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/write | Создайте пространство ключей. Применимо только к типам API: «cassandra». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/tables/delete | Удаление таблицы. Применимо только к типам API: «table». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/tables/read | Чтение таблицы или списка всех таблиц. Применимо только к типам API: «table». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/tables/settings/throughput/read | Чтение таблицы еще и пропускную способность. Применимо только к типам API: «table». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/tables/settings/throughput/write | Обновите таблицу еще и пропускную способность. Применимо только к типам API: «table». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/apis/tables/write | Создание или обновление таблицы. Применимо только к типам API: «table». |
> | Действие | Microsoft.DocumentDB/databaseAccounts/backup/action | Отправка запроса на настройку резервного копирования |
> | Действие | Microsoft.DocumentDB/databaseAccounts/changeResourceGroup/action | Изменяет группу ресурсов для учетной записи базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/databases/collections/metricDefinitions/read | Считывает определения метрик коллекции. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/databases/collections/metrics/read | Считывает метрики коллекции. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitionKeyRangeId/metrics/read | Чтение метрик уровня ключа раздела для учетной записи базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/metrics/read | Чтение метрик уровня раздела для учетной записи базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/read | Чтение разделов учетной записи базы данных в коллекции. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/usages/read | Чтение сведений об использовании уровня раздела для учетной записи базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/databases/collections/usages/read | Считывает данные об использовании коллекции. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/databases/metricDefinitions/read | Считывает определения метрик базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/databases/metrics/read | Считывает метрики базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/databases/usages/read | Считывает данные об использовании базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/delete | Удаляет учетные записи базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/failoverPriorityChange/action | Изменяет приоритеты отработки отказа для регионов учетной записи базы данных. Используется для выполнения операции отработки отказа вручную. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/action | Возвращает строки подключения для учетной записи базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/listKeys/action | Выводит список ключей учетной записи базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/metricDefinitions/read | Считывает определения метрик учетной записи базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/metrics/read | Считывает метрики учетной записи базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/offlineRegion/action | Отключение региона учетной записи базы данных.  |
> | Действие | Microsoft.DocumentDB/databaseAccounts/onlineRegion/action | Подключение региона учетной записи базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/operationResults/read | Чтение состояния асинхронной операции. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/percentile/metrics/read | Чтение метрик задержки. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/percentile/read | Чтение процентилей задержек репликации. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/percentile/sourceRegion/targetRegion/metrics/read | Чтение метрик задержки для определенного источника и целевого региона. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/percentile/targetRegion/metrics/read | Чтение метрик задержки для определенного целевого региона. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/delete | Удаление записи-посредника подключения частной конечной точки учетной записи базы данных |
> | Действие | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/operationResults/read | Чтение состояния privateEndpointConnectionProxies асинхронной операции |
> | Действие | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/read | Чтение прокси-сервер подключения частной конечной точки учетной записи базы данных |
> | Действие | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/validate/action | Проверить прокси-сервер подключения частной конечной точки учетной записи базы данных |
> | Действие | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/write | Создание или обновление прокси-сервер подключения частной конечной точки учетной записи базы данных |
> | Действие | Microsoft.DocumentDB/databaseAccounts/read | Считывает учетную запись базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Считывает ключи только для чтения учетной записи базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/readonlykeys/read | Считывает ключи только для чтения учетной записи базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/regenerateKey/action | Выполняет ротацию ключей учетной записи базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/metrics/read | Чтение метрик региональной коллекции. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitionKeyRangeId/metrics/read | Чтение метрик уровня ключа раздела для учетной записи региональной базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/metrics/read | Чтение метрик уровня раздела для учетной записи региональной базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/read | Чтение разделов учетной записи региональной базы данных в коллекции. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/region/metrics/read | Чтение метрик учетной записи базы данных и региона. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/restore/action | Отправка запроса восстановления |
> | Действие | Microsoft.DocumentDB/databaseAccounts/usages/read | Считывает данные об использовании учетной записи базы данных. |
> | Действие | Microsoft.DocumentDB/databaseAccounts/write | Обновляет учетные записи базы данных. |
> | Действие | Microsoft.DocumentDB/locations/asyncoperations/read | Считывает состояние асинхронных операций |
> | Действие | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/action | Уведомление Microsoft.DocumentDB о том, что идет удаление виртуальной сети или подсети. |
> | Действие | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/operationResults/read | Чтение состояния асинхронной операции удаления виртуальной сети или подсетей. |
> | Действие | Microsoft.DocumentDB/operationResults/read | Чтение состояния асинхронной операции. |
> | Действие | Microsoft.DocumentDB/operations/read | Чтение операций, доступных для Microsoft DocumentDB.  |
> | Действие | Microsoft.DocumentDB/register/action |  Регистрация поставщика ресурсов Microsoft DocumentDB для подписки. |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.DomainRegistration/checkDomainAvailability/Action | Проверяет, доступен ли домен для приобретения. |
> | Действие | Microsoft.DomainRegistration/domains/Delete | Удаляет существующий домен. |
> | Действие | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Delete | Удаление идентификатора владения. |
> | Действие | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Вывод списка идентификаторов владения. |
> | Действие | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Получение идентификатора владения. |
> | Действие | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Write | Создание или обновление идентификатора. |
> | Действие | Microsoft.DomainRegistration/domains/operationresults/Read | Возвращает операцию домена. |
> | Действие | Microsoft.DomainRegistration/domains/Read | Возвращает список доменов. |
> | Действие | Microsoft.DomainRegistration/domains/Read | Получение домена. |
> | Действие | Microsoft.DomainRegistration/domains/renew/Action | Обновление существующего домена. |
> | Действие | Microsoft.DomainRegistration/domains/Write | Добавляет новый домен или обновляет существующий. |
> | Действие | Microsoft.DomainRegistration/generateSsoRequest/Action | Создает запрос на вход в центр управления доменами. |
> | Действие | Microsoft.DomainRegistration/listDomainRecommendations/Action | Извлекает список рекомендуемых доменов на основе ключевых слов. |
> | Действие | Microsoft.DomainRegistration/operations/Read | Вывод списка всех операций из регистрации домена службы приложений. |
> | Действие | Microsoft.DomainRegistration/register/action | Регистрирует поставщик ресурсов доменов Майкрософт для подписки. |
> | Действие | Microsoft.DomainRegistration/topLevelDomains/listAgreements/Action | Вывод списка соглашений. |
> | Действие | Microsoft.DomainRegistration/topLevelDomains/Read | Получение доменов верхнего уровня. |
> | Действие | Microsoft.DomainRegistration/topLevelDomains/Read | Получение домена верхнего уровня. |
> | Действие | Microsoft.DomainRegistration/validateDomainRegistrationInformation/Action | Проверяет объект покупки домена без его отправки. |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.EventGrid/domains/delete | Удаление домена |
> | Действие | Microsoft.EventGrid/domains/listKeys/action | Получение списка ключей для домена |
> | Действие | Microsoft.EventGrid/domains/providers/Microsoft.Insights/metricDefinitions/read | Получает доступные метрики для доменов |
> | Действие | Microsoft.EventGrid/domains/read | Чтение домена |
> | Действие | Microsoft.EventGrid/domains/regenerateKey/action | Повторное создание ключа для домена |
> | Действие | Microsoft.EventGrid/domains/topics/read | Чтение раздела домена |
> | Действие | Microsoft.EventGrid/domains/write | Создание или обновление домена |
> | Действие | Microsoft.EventGrid/eventSubscriptions/delete | Удаление подписки на события, выполните следующие действия. |
> | Действие | Microsoft.EventGrid/eventSubscriptions/getFullUrl/action | Получение полного URL-адреса подписки на события. |
> | Действие | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметра диагностики для подписок на события. |
> | Действие | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/write | Получение или обновление параметра диагностики для подписок на события. |
> | Действие | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/metricDefinitions/read | Получение доступных метрик для подписок на события. |
> | Действие | Microsoft.EventGrid/eventSubscriptions/read | Чтение подписки на события, выполните следующие действия. |
> | Действие | Microsoft.EventGrid/eventSubscriptions/write | Создание или обновление подписки на события, выполните следующие действия. |
> | Действие | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметра диагностики для разделов. |
> | Действие | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/write | Создание или обновление параметра диагностики для разделов. |
> | Действие | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/metricDefinitions/read | Получение доступных метрик для разделов. |
> | Действие | Microsoft.EventGrid/extensionTopics/read | Чтение extensionTopic. |
> | Действие | Microsoft.EventGrid/locations/eventSubscriptions/read | Создание списка подписок на события в регионе |
> | Действие | Microsoft.EventGrid/locations/operationResults/read | Чтение результата региональной операции |
> | Действие | Microsoft.EventGrid/locations/operationsStatus/read | Чтение состояния региональной операции |
> | Действие | Microsoft.EventGrid/locations/topictypes/eventSubscriptions/read | Создание списка подписок на события в регионе по типу темы |
> | Действие | Microsoft.EventGrid/operationResults/read | Чтение результата операции |
> | Действие | Microsoft.EventGrid/operations/read | Вывод списка операций EventGrid. |
> | Действие | Microsoft.EventGrid/operationsStatus/read | Чтение состояния операции |
> | Действие | Microsoft.EventGrid/register/action | Регистрирует подписку для поставщика ресурсов EventGrid. |
> | Действие | Microsoft.EventGrid/topics/delete | Удаление раздела |
> | Действие | Microsoft.EventGrid/topics/listKeys/action | Получение списка ключей для раздела |
> | Действие | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметра диагностики для разделов. |
> | Действие | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/write | Создание или обновление параметра диагностики для разделов. |
> | Действие | Microsoft.EventGrid/topics/providers/Microsoft.Insights/metricDefinitions/read | Получение доступных метрик для разделов. |
> | Действие | Microsoft.EventGrid/topics/read | Чтение раздела. |
> | Действие | Microsoft.EventGrid/topics/regenerateKey/action | Повторное создание ключа для раздела |
> | Действие | Microsoft.EventGrid/topics/write | Создание или обновление раздела. |
> | Действие | Microsoft.EventGrid/topictypes/eventSubscriptions/read | Создание списка подписок на глобальные события по типу темы |
> | Действие | Microsoft.EventGrid/topictypes/eventtypes/read | Чтение типов eventtype, поддерживаемых topictype |
> | Действие | Microsoft.EventGrid/topictypes/read | Чтение типа раздела (topictype) |
> | Действие | Microsoft.EventGrid/unregister/action | Отмена регистрации подписки для поставщика ресурсов EventGrid. |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.EventHub/checkNameAvailability/action | Проверяет доступность пространства имен в заданной подписке. |
> | Действие | Microsoft.EventHub/checkNamespaceAvailability/action | Проверяет доступность пространства имен в заданной подписке. Этот API не рекомендуется. Вместо него используйте CheckNameAvailability. |
> | Действие | Microsoft.EventHub/clusters/providers/Microsoft.Insights/metricDefinitions/read | Возвращает список описаний ресурсов метрик кластера. |
> | Действие | Microsoft.EventHub/locations/deleteVirtualNetworkOrSubnets/action | Удаление правил виртуальной сети в поставщике ресурсов концентратора событий для указанной виртуальной сети |
> | Действие | Microsoft.EventHub/namespaces/authorizationRules/action | Обновление правила авторизации для пространства имен. Это нерекомендуемый API. Чтобы обновить правило авторизации для пространства имен, используйте вызов PUT. Эта операция не поддерживается для API версии 2017-04-01. |
> | Действие | Microsoft.EventHub/namespaces/authorizationRules/delete | Удаляет правило авторизации для пространства имен. Невозможно удалить правило авторизации пространства имен по умолчанию.  |
> | Действие | Microsoft.EventHub/namespaces/authorizationRules/listkeys/action | Возвращает строку подключения к пространству имен. |
> | Действие | Microsoft.EventHub/namespaces/authorizationRules/read | Возвращает список описаний правил авторизации пространства имен. |
> | Действие | Microsoft.EventHub/namespaces/authorizationRules/regenerateKeys/action | Повторно создает первичный или вторичный ключ для ресурса. |
> | Действие | Microsoft.EventHub/namespaces/authorizationRules/write | Создает правила авторизации уровня пространства имен и обновляет его свойства. Можно обновить права доступа, первичный ключ и вторичный ключ для правил авторизации. |
> | Действие | Microsoft.EventHub/namespaces/Delete | Удаляет ресурс пространства имен. |
> | Действие | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Получение ключей правил авторизации основного пространства имен для аварийного восстановления. |
> | Действие | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/read | Получение правил авторизации основного пространства имен для аварийного восстановления. |
> | Действие | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/breakPairing/action | Отключение аварийного восстановления и остановка репликации изменений из основного пространства имен в дополнительное. |
> | Действие | Microsoft.EventHub/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Проверка доступности псевдонима пространства имен в заданной подписке. |
> | Действие | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/delete | Удаление конфигурации аварийного восстановления, связанной с пространством имен. Эта операция может быть вызвана только через основное пространство имен. |
> | Действие | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/failover/action | Вызов отработки отказа географического аварийного восстановления и перенастройка псевдонима пространства имен для указания на дополнительное пространство. |
> | Действие | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/read | Получение конфигурации аварийного восстановления, связанной с пространством имен. |
> | Действие | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/write | Создание или обновление конфигурации аварийного восстановления, связанной с пространством имен. |
> | Действие | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/action | Операция обновления концентратора событий. Эта операция не поддерживается для API версии 2017-04-01. Правила авторизации. Чтобы обновить правило авторизации, используйте вызов PUT. |
> | Действие | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/delete | Операция удаления правил авторизации концентратора событий. |
> | Действие | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/listkeys/action | Возвращает строку подключения к концентратору событий. |
> | Действие | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/read |  Возвращает список правил авторизации концентратора событий. |
> | Действие | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/regenerateKeys/action | Повторно создает первичный или вторичный ключ для ресурса. |
> | Действие | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/write | Создает правила авторизации концентратора событий и обновляет его свойства. Можно обновить права доступа к правилам авторизации. |
> | Действие | Microsoft.EventHub/namespaces/eventHubs/consumergroups/Delete | Операция удаления ресурса группы потребителей. |
> | Действие | Microsoft.EventHub/namespaces/eventHubs/consumergroups/read | Возвращает список описаний ресурсов группы потребителей. |
> | Действие | Microsoft.EventHub/namespaces/eventHubs/consumergroups/write | Создает или обновляет свойства группы потребителей. |
> | Действие | Microsoft.EventHub/namespaces/eventhubs/Delete | Операция удаления ресурса концентратора событий. |
> | Действие | Microsoft.EventHub/namespaces/eventhubs/read | Возвращает список описаний ресурсов концентратора событий. |
> | Действие | Microsoft.EventHub/namespaces/eventhubs/write | Создает или обновляет свойства концентратора событий. |
> | Действие | Microsoft.EventHub/namespaces/ipFilterRules/delete | Удаление ресурса фильтра IP-адресов |
> | Действие | Microsoft.EventHub/namespaces/ipFilterRules/read | Получение ресурса фильтра IP-адресов |
> | Действие | Microsoft.EventHub/namespaces/ipFilterRules/write | Создание ресурса фильтра IP-адресов |
> | DataAction | Microsoft.EventHub/namespaces/messages/receive/action | Получение сообщений |
> | DataAction | Microsoft.EventHub/namespaces/messages/send/action | Отправка сообщений |
> | Действие | Microsoft.EventHub/namespaces/messagingPlan/read | Получение плана обмена сообщениями для пространства имен.<br>Это нерекомендуемый API.<br>Свойства, предоставляемые через ресурс MessagingPlan, перемещены в ресурс (родительского) пространства имен в более поздних версиях API.<br>Эта операция не поддерживается для API версии 2017-04-01. |
> | Действие | Microsoft.EventHub/namespaces/messagingPlan/write | Обновление плана обмена сообщениями для пространства имен.<br>Это нерекомендуемый API.<br>Свойства, предоставляемые через ресурс MessagingPlan, перемещены в ресурс (родительского) пространства имен в более поздних версиях API.<br>Эта операция не поддерживается для API версии 2017-04-01. |
> | Действие | Microsoft.EventHub/namespaces/networkrulesets/delete | Удаление ресурса правила виртуальной сети |
> | Действие | Microsoft.EventHub/namespaces/networkrulesets/read | Возвращает ресурс NetworkRuleSet |
> | Действие | Microsoft.EventHub/namespaces/networkrulesets/write | Создание ресурса правила виртуальной сети |
> | Действие | Microsoft.EventHub/namespaces/operationresults/read | Получение состояния операции пространства имен. |
> | Действие | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Возвращает список описаний параметров диагностики пространства имен. |
> | Действие | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Возвращает список описаний параметров диагностики пространства имен. |
> | Действие | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/logDefinitions/read | Возвращает список описаний ресурсов журналов пространства имен. |
> | Действие | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Возвращает список описаний ресурсов метрик пространства имен. |
> | Действие | Microsoft.EventHub/namespaces/read | Возвращает список описаний ресурсов пространства имен. |
> | Действие | Microsoft.EventHub/namespaces/removeAcsNamepsace/action | Удаляет пространство имен ACS. |
> | Действие | Microsoft.EventHub/namespaces/virtualNetworkRules/delete | Удаление ресурса правила виртуальной сети |
> | Действие | Microsoft.EventHub/namespaces/virtualNetworkRules/read | Получение ресурса правила виртуальной сети |
> | Действие | Microsoft.EventHub/namespaces/virtualNetworkRules/write | Создание ресурса правила виртуальной сети |
> | Действие | Microsoft.EventHub/namespaces/write | Создает ресурс пространства имен и обновляет его свойства. К этим свойствам относятся теги и пространство имен. |
> | Действие | Microsoft.EventHub/operations/read | Получение операций. |
> | Действие | Microsoft.EventHub/register/action | Регистрирует подписку для поставщика ресурсов концентратора событий и позволяет создавать ресурсы концентратора событий. |
> | Действие | Microsoft.EventHub/sku/read | Получение списка описаний ресурсов номеров SKU. |
> | Действие | Microsoft.EventHub/sku/regions/read | Получение списка описаний ресурсов регионов для номеров SKU. |
> | Действие | Microsoft.EventHub/unregister/action | Регистрация поставщика ресурсов концентратора событий. |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Features/features/read | Получает функции подписки. |
> | Действие | Microsoft.Features/operations/read | Получает список операций. |
> | Действие | Microsoft.Features/providers/features/read | Получает функцию подписки в заданном поставщике ресурсов. |
> | Действие | Microsoft.Features/providers/features/register/action | Регистрирует функцию для подписки в заданном поставщике ресурсов. |
> | Действие | Microsoft.Features/providers/features/unregister/action | Отмена регистрации функции для подписки в заданном поставщике ресурсов. |
> | Действие | Microsoft.Features/register/action | Регистрирует компонент подписки. |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.GuestConfiguration/guestConfigurationAssignments/read | Получение назначения конфигурации гостевой системы. |
> | Действие | Microsoft.GuestConfiguration/guestConfigurationAssignments/reports/read | Получение отчета о назначении конфигурации гостевой системы. |
> | Действие | Microsoft.GuestConfiguration/guestConfigurationAssignments/write | Создание назначения конфигурации гостевой системы. |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.HDInsight/clusters/applications/delete | Удаление приложения для кластера HDInsight |
> | Действие | Microsoft.HDInsight/clusters/applications/read | Получение приложения для кластера HDInsight |
> | Действие | Microsoft.HDInsight/clusters/applications/write | Создание приложения или его обновление для кластера HDInsight |
> | Действие | Microsoft.HDInsight/clusters/changerdpsetting/action | Изменяет параметры RDP для кластера HDInsight. |
> | Действие | Microsoft.HDInsight/clusters/configurations/action | Обновляет конфигурацию кластера HDInsight. |
> | Действие | Microsoft.HDInsight/clusters/configurations/action | Возвращает конфигурации кластеров HDInsight. |
> | Действие | Microsoft.HDInsight/clusters/configurations/read | Возвращает конфигурации кластеров HDInsight. |
> | Действие | Microsoft.HDInsight/clusters/delete | Удаляет кластер HDInsight. |
> | Действие | Microsoft.HDInsight/clusters/getGatewaySettings/action | Получить параметры шлюза для кластера HDInsight |
> | Действие | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметра диагностики для ресурса кластера HDInsight. |
> | Действие | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/write | Создание или обновление параметра диагностики для ресурса кластера HDInsight. |
> | Действие | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/metricDefinitions/read | Получение доступных метрик для кластера HDInsight. |
> | Действие | Microsoft.HDInsight/clusters/read | Возвращает сведения о кластере HDInsight. |
> | Действие | Microsoft.HDInsight/clusters/roles/resize/action | Изменяет размер кластера HDInsight. |
> | Действие | Microsoft.HDInsight/clusters/updateGatewaySettings/action | Обновите параметры шлюза для кластера HDInsight |
> | Действие | Microsoft.HDInsight/clusters/write | Создает или обновляет кластер HDInsight. |
> | Действие | Microsoft.HDInsight/locations/capabilities/read | Возвращает возможности подписки. |
> | Действие | Microsoft.HDInsight/locations/checkNameAvailability/read | Проверяет доступность имени. |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.ImportExport/jobs/delete | Удаляет существующее задание. |
> | Действие | Microsoft.ImportExport/jobs/listBitLockerKeys/action | Возвращает ключи BitLocker для указанного задания. |
> | Действие | Microsoft.ImportExport/jobs/read | Возвращает свойства для указанного задания или возвращает список заданий. |
> | Действие | Microsoft.ImportExport/jobs/write | Создает задание с указанными параметрами либо обновляет свойства или теги указанного задания. |
> | Действие | Microsoft.ImportExport/locations/read | Возвращает свойства для указанного расположения или возвращает список расположений. |
> | Действие | Microsoft.ImportExport/register/action | Регистрирует подписку для поставщика ресурсов импорта и экспорта и позволяет создавать задания импорта и экспорта. |

## <a name="microsoftinsights"></a>Microsoft.Insights

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Insights/ActionGroups/Delete | Удаление группы действий. |
> | Действие | Microsoft.Insights/ActionGroups/Read | Чтение группы действий. |
> | Действие | Microsoft.Insights/ActionGroups/Write | Создание или изменение группы действий. |
> | Действие | Microsoft.Insights/ActivityLogAlerts/Activated/Action | Активировано оповещение журнала действий. |
> | Действие | Microsoft.Insights/ActivityLogAlerts/Delete | Удаление оповещения для журнала действий. |
> | Действие | Microsoft.Insights/ActivityLogAlerts/Read | Чтение оповещения из журнала действий. |
> | Действие | Microsoft.Insights/ActivityLogAlerts/Write | Создание или изменение оповещения для журнала действий. |
> | Действие | Microsoft.Insights/AlertRules/Activated/Action | Активировано классическое оповещение метрики. |
> | Действие | Microsoft.Insights/AlertRules/Delete | Удаление классического оповещения метрики. |
> | Действие | Microsoft.Insights/AlertRules/Incidents/Read | Чтение инцидента классического оповещения метрики. |
> | Действие | Microsoft.Insights/AlertRules/Read | Чтение классического оповещения метрики |
> | Действие | Microsoft.Insights/AlertRules/Resolved/Action | Классическое оповещение метрики разрешено. |
> | Действие | Microsoft.Insights/AlertRules/Throttled/Action | Правило классического оповещения метрики отрегулировано. |
> | Действие | Microsoft.Insights/AlertRules/Write | Создание или изменение классического оповещения метрики. |
> | Действие | Microsoft.Insights/AutoscaleSettings/Delete | Удаление параметра автомасштабирования. |
> | Действие | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/diagnosticSettings/Read | Чтение параметра диагностики для ресурсов |
> | Действие | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/diagnosticSettings/Write | Создание или изменение параметра для диагностики ресурсов. |
> | Действие | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/logDefinitions/Read | Чтение определений в журнале |
> | Действие | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read | Чтение определений метрик |
> | Действие | Microsoft.Insights/AutoscaleSettings/Read | Чтение параметра автомасштабирования. |
> | Действие | Microsoft.Insights/AutoscaleSettings/Scaledown/Action | Автоматическое уменьшение масштаба запущено. |
> | Действие | Microsoft.Insights/AutoscaleSettings/ScaledownResult/Action | Автоматическое уменьшение масштаба завершено. |
> | Действие | Microsoft.Insights/AutoscaleSettings/Scaleup/Action | Автоматическое увеличение масштаба запущено. |
> | Действие | Microsoft.Insights/AutoscaleSettings/ScaleupResult/Action | Автоматическое увеличение масштаба завершено. |
> | Действие | Microsoft.Insights/AutoscaleSettings/Write | Создание или изменение параметра автомасштабирования. |
> | Действие | Microsoft.Insights/Baseline/Read | Чтение базовой метрики (предварительная версия) |
> | Действие | Microsoft.Insights/CalculateBaseline/Read | Вычисление базового показателя для значений метрик (предварительная версия) |
> | Действие | Microsoft.Insights/Components/AnalyticsItems/Delete | Удаление элемента аналитики Application Insights. |
> | Действие | Microsoft.Insights/Components/AnalyticsItems/Read | Чтение элемента аналитики Application Insights. |
> | Действие | Microsoft.Insights/Components/AnalyticsItems/Write | Запись элемента аналитики Application Insights. |
> | Действие | Microsoft.Insights/Components/AnalyticsTables/Action | Действие таблицы аналитики Application Insights. |
> | Действие | Microsoft.Insights/Components/AnalyticsTables/Delete | Удаление схемы таблицы аналитики Application Insights. |
> | Действие | Microsoft.Insights/Components/AnalyticsTables/Read | Чтение схемы таблицы аналитики Application Insights. |
> | Действие | Microsoft.Insights/Components/AnalyticsTables/Write | Запись схемы таблицы аналитики Application Insights. |
> | Действие | Microsoft.Insights/Components/Annotations/Delete | Удаление заметки Application Insights. |
> | Действие | Microsoft.Insights/Components/Annotations/Read | Чтение заметки Application Insights. |
> | Действие | Microsoft.Insights/Components/Annotations/Write | Запись заметки Application Insights. |
> | Действие | Microsoft.Insights/Components/Api/Read | Чтение API данных компонента Application Insights. |
> | Действие | Microsoft.Insights/Components/ApiKeys/Action | Создание ключа API Application Insights. |
> | Действие | Microsoft.Insights/Components/ApiKeys/Delete | Удаление ключа API Application Insights. |
> | Действие | Microsoft.Insights/Components/ApiKeys/Read | Чтение ключа API Application Insights. |
> | Действие | Microsoft.Insights/Components/BillingPlanForComponent/Read | Чтение плана выставления счетов для компонента Application Insights. |
> | Действие | Microsoft.Insights/Components/CurrentBillingFeatures/Read | Чтение текущих функций выставления счетов для компонента Application Insights. |
> | Действие | Microsoft.Insights/Components/CurrentBillingFeatures/Write | Запись текущих функций выставления счетов для компонента Application Insights. |
> | Действие | Microsoft.Insights/Components/DefaultWorkItemConfig/Read | Чтение конфигурации интеграции управления жизненным циклом приложений по умолчанию для Application Insights. |
> | Действие | Microsoft.Insights/Components/Delete | Удаление конфигурации компонента Application Insights. |
> | Действие | Microsoft.Insights/Components/Events/Read | Получение журналов Application Insights с использованием формата запроса OData. |
> | Действие | Microsoft.Insights/Components/ExportConfiguration/Action | Действие параметров экспорта Application Insights. |
> | Действие | Microsoft.Insights/Components/ExportConfiguration/Delete | Удаление параметров экспорта Application Insights. |
> | Действие | Microsoft.Insights/Components/ExportConfiguration/Read | Чтение параметров экспорта Application Insights. |
> | Действие | Microsoft.Insights/Components/ExportConfiguration/Write | Запись параметров экспорта Application Insights. |
> | Действие | Microsoft.Insights/Components/ExtendQueries/Read | Чтение результатов расширенного запроса компонента Application Insights. |
> | Действие | Microsoft.Insights/Components/Favorites/Delete | Удаление избранного объекта Application Insights. |
> | Действие | Microsoft.Insights/Components/Favorites/Read | Чтение избранного объекта Application Insights. |
> | Действие | Microsoft.Insights/Components/Favorites/Write | Запись избранного объекта Application Insights. |
> | Действие | Microsoft.Insights/Components/FeatureCapabilities/Read | Чтение возможностей функций компонента Application Insights. |
> | Действие | Microsoft.Insights/Components/GetAvailableBillingFeatures/Read | Чтение доступных функций выставления счетов для компонента Application Insights. |
> | Действие | Microsoft.Insights/Components/GetToken/Read | Чтение токена компонента Application Insights. |
> | Действие | Microsoft.Insights/Components/MetricDefinitions/Read | Чтение определений метрик компонента Application Insights. |
> | Действие | Microsoft.Insights/Components/Metrics/Read | Чтение метрик компонента Application Insights. |
> | Действие | Microsoft.Insights/Components/Move/Action | Перемещение компонента Application Insights в другую группу ресурсов или подписку. |
> | Действие | Microsoft.Insights/Components/MyAnalyticsItems/Delete | Удаление персонального элемента аналитики Application Insights. |
> | Действие | Microsoft.Insights/Components/MyAnalyticsItems/Read | Чтение персонального элемента аналитики Application Insights. |
> | Действие | Microsoft.Insights/Components/MyAnalyticsItems/Write | Запись персонального элемента аналитики Application Insights. |
> | Действие | Microsoft.Insights/Components/MyFavorites/Read | Чтение персонального избранного объекта Application Insights. |
> | Действие | Microsoft.Insights/Components/Operations/Read | Получение состояния длительных операций в Application Insights. |
> | Действие | Microsoft.Insights/Components/PricingPlans/Read | Чтение плана ценообразования компонента Application Insights. |
> | Действие | Microsoft.Insights/Components/PricingPlans/Write | Запись плана ценообразования компонента Application Insights. |
> | Действие | Microsoft.Insights/Components/ProactiveDetectionConfigs/Read | Чтение конфигурации упреждающего обнаружения Application Insights. |
> | Действие | Microsoft.Insights/Components/ProactiveDetectionConfigs/Write | Запись конфигурации упреждающего обнаружения Application Insights. |
> | Действие | Microsoft.Insights/Components/providers/Microsoft.Insights/MetricDefinitions/Read | Чтение определений метрик |
> | Действие | Microsoft.Insights/Components/Purge/Action | Очистка данных Application Insights |
> | Действие | Microsoft.Insights/Components/Query/Read | Выполнение запросов к журналам Application Insights. |
> | Действие | Microsoft.Insights/Components/QuotaStatus/Read | Чтение состояния квоты компонента Application Insights. |
> | Действие | Microsoft.Insights/Components/Read | Чтение конфигурации компонента Application Insights. |
> | Действие | Microsoft.Insights/Components/SyntheticMonitorLocations/Read | Чтение расположений веб-тестов Application Insights. |
> | Действие | Microsoft.Insights/Components/Webtests/Read | Чтение конфигурации веб-теста. |
> | Действие | Microsoft.Insights/Components/WorkItemConfigs/Delete | Удаление конфигурации интеграции управления жизненным циклом приложений для Application Insights. |
> | Действие | Microsoft.Insights/Components/WorkItemConfigs/Read | Чтение конфигурации интеграции управления жизненным циклом приложений для Application Insights. |
> | Действие | Microsoft.Insights/Components/WorkItemConfigs/Write | Запись конфигурации интеграции управления жизненным циклом приложений для Application Insights. |
> | Действие | Microsoft.Insights/Components/Write | Запись конфигурации компонента Application Insights. |
> | Действие | Microsoft.Insights/DiagnosticSettings/Delete | Удаление параметра для диагностики ресурсов. |
> | Действие | Microsoft.Insights/DiagnosticSettings/Read | Чтение параметра диагностики для ресурсов |
> | Действие | Microsoft.Insights/DiagnosticSettings/Write | Создание или изменение параметра для диагностики ресурсов. |
> | Действие | Microsoft.Insights/EventCategories/Read | Чтение доступных категорий событий для журнала действий. |
> | Действие | Microsoft.Insights/eventtypes/digestevents/Read | Считывает дайджест типов событий управления. |
> | Действие | Microsoft.Insights/eventtypes/values/Read | Чтение событий из журнала действий. |
> | Действие | Microsoft.Insights/ExtendedDiagnosticSettings/Delete | Удаление параметра диагностики журнала сетевых потоков. |
> | Действие | Microsoft.Insights/ExtendedDiagnosticSettings/Read | Чтение параметра диагностики журнала сетевых потоков. |
> | Действие | Microsoft.Insights/ExtendedDiagnosticSettings/Write | Создание или изменение параметра диагностики журнала сетевых потоков. |
> | Действие | Microsoft.Insights/ListMigrationDate/Action | Возврат даты перевода подписки. |
> | Действие | Microsoft.Insights/ListMigrationDate/Read | Возврат даты перевода подписки. |
> | Действие | Microsoft.Insights/LogDefinitions/Read | Чтение определений в журнале |
> | Действие | Microsoft.Insights/LogProfiles/Delete | Удаление профиля журнала действий. |
> | Действие | Microsoft.Insights/LogProfiles/Read | Чтение профиля журнала действий. |
> | Действие | Microsoft.Insights/LogProfiles/Write | Создание или изменение профиля журнала действий. |
> | Действие | Microsoft.Insights/Logs/ADAssessmentRecommendation/Read | Чтение данных из таблицы ADAssessmentRecommendation. |
> | Действие | Microsoft.Insights/Logs/ADReplicationResult/Read | Чтение данных из таблицы ADReplicationResult. |
> | Действие | Microsoft.Insights/Logs/ADSecurityAssessmentRecommendation/Read | Чтение данных из таблицы ADSecurityAssessmentRecommendation. |
> | Действие | Microsoft.Insights/Logs/Alert/Read | Чтение данных из таблицы Alert. |
> | Действие | Microsoft.Insights/Logs/AlertHistory/Read | Чтение данных из таблицы AlertHistory. |
> | Действие | Microsoft.Insights/Logs/ApplicationInsights/Read | Чтение данных из таблицы ApplicationInsights. |
> | Действие | Microsoft.Insights/Logs/AzureActivity/Read | Чтение данных из таблицы AzureActivity. |
> | Действие | Microsoft.Insights/Logs/AzureMetrics/Read | Чтение данных из таблицы AzureMetrics. |
> | Действие | Microsoft.Insights/Logs/BoundPort/Read | Чтение данных из таблицы BoundPort. |
> | Действие | Microsoft.Insights/Logs/CommonSecurityLog/Read | Чтение данных из таблицы CommonSecurityLog. |
> | Действие | Microsoft.Insights/Logs/ComputerGroup/Read | Чтение данных из таблицы ComputerGroup. |
> | Действие | Microsoft.Insights/Logs/ConfigurationChange/Read | Чтение данных из таблицы ConfigurationChange. |
> | Действие | Microsoft.Insights/Logs/ConfigurationData/Read | Чтение данных из таблицы ConfigurationData. |
> | Действие | Microsoft.Insights/Logs/ContainerImageInventory/Read | Чтение данных из таблицы ContainerImageInventory. |
> | Действие | Microsoft.Insights/Logs/ContainerInventory/Read | Чтение данных из таблицы ContainerInventory. |
> | Действие | Microsoft.Insights/Logs/ContainerLog/Read | Чтение данных из таблицы ContainerLog. |
> | Действие | Microsoft.Insights/Logs/ContainerServiceLog/Read | Чтение данных из таблицы ContainerServiceLog. |
> | Действие | Microsoft.Insights/Logs/DeviceAppCrash/Read | Чтение данных из таблицы DeviceAppCrash. |
> | Действие | Microsoft.Insights/Logs/DeviceAppLaunch/Read | Чтение данных из таблицы DeviceAppLaunch. |
> | Действие | Microsoft.Insights/Logs/DeviceCalendar/Read | Чтение данных из таблицы DeviceCalendar. |
> | Действие | Microsoft.Insights/Logs/DeviceCleanup/Read | Чтение данных из таблицы DeviceCleanup. |
> | Действие | Microsoft.Insights/Logs/DeviceConnectSession/Read | Чтение данных из таблицы DeviceConnectSession. |
> | Действие | Microsoft.Insights/Logs/DeviceEtw/Read | Чтение данных из таблицы DeviceEtw. |
> | Действие | Microsoft.Insights/Logs/DeviceHardwareHealth/Read | Чтение данных из таблицы DeviceHardwareHealth. |
> | Действие | Microsoft.Insights/Logs/DeviceHealth/Read | Чтение данных из таблицы DeviceHealth. |
> | Действие | Microsoft.Insights/Logs/DeviceHeartbeat/Read | Чтение данных из таблицы DeviceHeartbeat. |
> | Действие | Microsoft.Insights/Logs/DeviceSkypeHeartbeat/Read | Чтение данных из таблицы DeviceSkypeHeartbeat. |
> | Действие | Microsoft.Insights/Logs/DeviceSkypeSignIn/Read | Чтение данных из таблицы DeviceSkypeSignIn. |
> | Действие | Microsoft.Insights/Logs/DeviceSleepState/Read | Чтение данных из таблицы DeviceSleepState. |
> | Действие | Microsoft.Insights/Logs/DHAppFailure/Read | Чтение данных из таблицы DHAppFailure. |
> | Действие | Microsoft.Insights/Logs/DHAppReliability/Read | Чтение данных из таблицы DHAppReliability. |
> | Действие | Microsoft.Insights/Logs/DHDriverReliability/Read | Чтение данных из таблицы DHDriverReliability. |
> | Действие | Microsoft.Insights/Logs/DHLogonFailures/Read | Чтение данных из таблицы DHLogonFailures. |
> | Действие | Microsoft.Insights/Logs/DHLogonMetrics/Read | Чтение данных из таблицы DHLogonMetrics. |
> | Действие | Microsoft.Insights/Logs/DHOSCrashData/Read | Чтение данных из таблицы DHOSCrashData. |
> | Действие | Microsoft.Insights/Logs/DHOSReliability/Read | Чтение данных из таблицы DHOSReliability. |
> | Действие | Microsoft.Insights/Logs/DHWipAppLearning/Read | Чтение данных из таблицы DHWipAppLearning. |
> | Действие | Microsoft.Insights/Logs/DnsEvents/Read | Чтение данных из таблицы DnsEvents. |
> | Действие | Microsoft.Insights/Logs/DnsInventory/Read | Чтение данных из таблицы DnsInventory. |
> | Действие | Microsoft.Insights/Logs/ETWEvent/Read | Чтение данных из таблицы ETWEvent. |
> | Действие | Microsoft.Insights/Logs/Event/Read | Чтение данных из таблицы Event. |
> | Действие | Microsoft.Insights/Logs/ExchangeAssessmentRecommendation/Read | Чтение данных из таблицы ExchangeAssessmentRecommendation. |
> | Действие | Microsoft.Insights/Logs/ExchangeOnlineAssessmentRecommendation/Read | Чтение данных из таблицы ExchangeOnlineAssessmentRecommendation. |
> | Действие | Microsoft.Insights/Logs/Heartbeat/Read | Чтение данных из таблицы Heartbeat. |
> | Действие | Microsoft.Insights/Logs/IISAssessmentRecommendation/Read | Чтение данных из таблицы IISAssessmentRecommendation. |
> | Действие | Microsoft.Insights/Logs/InboundConnection/Read | Чтение данных из таблицы InboundConnection. |
> | Действие | Microsoft.Insights/Logs/KubeNodeInventory/Read | Чтение данных из таблицы KubeNodeInventory. |
> | Действие | Microsoft.Insights/Logs/KubePodInventory/Read | Чтение данных из таблицы KubePodInventory. |
> | Действие | Microsoft.Insights/Logs/LinuxAuditLog/Read | Чтение данных из таблицы LinuxAuditLog. |
> | Действие | Microsoft.Insights/Logs/MAApplication/Read | Чтение данных из таблицы MAApplication. |
> | Действие | Microsoft.Insights/Logs/MAApplicationHealth/Read | Чтение данных из таблицы MAApplicationHealth. |
> | Действие | Microsoft.Insights/Logs/MAApplicationHealthAlternativeVersions/Read | Считывает данные из таблицы MAApplicationHealthAlternativeVersions. |
> | Действие | Microsoft.Insights/Logs/MAApplicationHealthIssues/Read | Считывает данные из таблицы MAApplicationHealthIssues. |
> | Действие | Microsoft.Insights/Logs/MAApplicationInstance/Read | Чтение данных из таблицы MAApplicationInstance. |
> | Действие | Microsoft.Insights/Logs/MAApplicationInstanceReadiness/Read | Считывает данные из таблицы MAApplicationInstanceReadiness. |
> | Действие | Microsoft.Insights/Logs/MAApplicationReadiness/Read | Чтение данных из таблицы MAApplicationReadiness. |
> | Действие | Microsoft.Insights/Logs/MADeploymentPlan/Read | Чтение данных из таблицы MADeploymentPlan. |
> | Действие | Microsoft.Insights/Logs/MADevice/Read | Чтение данных из таблицы MADevice. |
> | Действие | Microsoft.Insights/Logs/MADevicePnPHealth/Read | Считывает данные из таблицы MADevicePnPHealth. |
> | Действие | Microsoft.Insights/Logs/MADevicePnPHealthAlternativeVersions/Read | Считывает данные из таблицы MADevicePnPHealthAlternativeVersions. |
> | Действие | Microsoft.Insights/Logs/MADevicePnPHealthIssues/Read | Считывает данные из таблицы MADevicePnPHealthIssues. |
> | Действие | Microsoft.Insights/Logs/MADeviceReadiness/Read | Чтение данных из таблицы MADeviceReadiness. |
> | Действие | Microsoft.Insights/Logs/MADriverInstanceReadiness/Read | Считывает данные из таблицы MADriverInstanceReadiness. |
> | Действие | Microsoft.Insights/Logs/MADriverReadiness/Read | Чтение данных из таблицы MADriverReadiness. |
> | Действие | Microsoft.Insights/Logs/MAOfficeAddin/Read | Чтение данных из таблицы MAOfficeAddin. |
> | Действие | Microsoft.Insights/Logs/MAOfficeAddinHealth/Read | Чтение данных из таблицы MAOfficeAddinHealth. |
> | Действие | Microsoft.Insights/Logs/MAOfficeAddinHealthIssues/Read | Считывает данные из таблицы MAOfficeAddinHealthIssues. |
> | Действие | Microsoft.Insights/Logs/MAOfficeAddinInstance/Read | Чтение данных из таблицы MAOfficeAddinInstance. |
> | Действие | Microsoft.Insights/Logs/MAOfficeAddinInstanceReadiness/Read | Считывает данные из таблицы MAOfficeAddinInstanceReadiness. |
> | Действие | Microsoft.Insights/Logs/MAOfficeAddinReadiness/Read | Чтение данных из таблицы MAOfficeAddinReadiness. |
> | Действие | Microsoft.Insights/Logs/MAOfficeApp/Read | Чтение данных из таблицы MAOfficeApp. |
> | Действие | Microsoft.Insights/Logs/MAOfficeAppHealth/Read | Чтение данных из таблицы MAOfficeAppHealth. |
> | Действие | Microsoft.Insights/Logs/MAOfficeAppInstance/Read | Чтение данных из таблицы MAOfficeAppInstance. |
> | Действие | Microsoft.Insights/Logs/MAOfficeAppReadiness/Read | Чтение данных из таблицы MAOfficeAppReadiness. |
> | Действие | Microsoft.Insights/Logs/MAOfficeBuildInfo/Read | Чтение данных из таблицы MAOfficeBuildInfo. |
> | Действие | Microsoft.Insights/Logs/MAOfficeCurrencyAssessment/Read | Чтение данных из таблицы MAOfficeCurrencyAssessment. |
> | Действие | Microsoft.Insights/Logs/MAOfficeCurrencyAssessmentDailyCounts/Read | Чтение данных из таблицы MAOfficeCurrencyAssessmentDailyCounts. |
> | Действие | Microsoft.Insights/Logs/MAOfficeDeploymentStatus/Read | Чтение данных из таблицы MAOfficeDeploymentStatus. |
> | Действие | Microsoft.Insights/Logs/MAOfficeMacroHealth/Read | Считывает данные из таблицы MAOfficeMacroHealth. |
> | Действие | Microsoft.Insights/Logs/MAOfficeMacroHealthIssues/Read | Считывает данные из таблицы MAOfficeMacroHealthIssues. |
> | Действие | Microsoft.Insights/Logs/MAOfficeMacroIssueInstanceReadiness/Read | Считывает данные из таблицы MAOfficeMacroIssueInstanceReadiness. |
> | Действие | Microsoft.Insights/Logs/MAOfficeMacroIssueReadiness/Read | Чтение данных из таблицы MAOfficeMacroIssueReadiness. |
> | Действие | Microsoft.Insights/Logs/MAOfficeMacroSummary/Read | Чтение данных из таблицы MAOfficeMacroSummary. |
> | Действие | Microsoft.Insights/Logs/MAOfficeSuite/Read | Чтение данных из таблицы MAOfficeSuite. |
> | Действие | Microsoft.Insights/Logs/MAOfficeSuiteInstance/Read | Чтение данных из таблицы MAOfficeSuiteInstance. |
> | Действие | Microsoft.Insights/Logs/MAProposedPilotDevices/Read | Чтение данных из таблицы MAProposedPilotDevices. |
> | Действие | Microsoft.Insights/Logs/MAWindowsBuildInfo/Read | Чтение данных из таблицы MAWindowsBuildInfo. |
> | Действие | Microsoft.Insights/Logs/MAWindowsCurrencyAssessment/Read | Чтение данных из таблицы MAWindowsCurrencyAssessment. |
> | Действие | Microsoft.Insights/Logs/MAWindowsCurrencyAssessmentDailyCounts/Read | Чтение данных из таблицы MAWindowsCurrencyAssessmentDailyCounts. |
> | Действие | Microsoft.Insights/Logs/MAWindowsDeploymentStatus/Read | Чтение данных из таблицы MAWindowsDeploymentStatus. |
> | Действие | Microsoft.Insights/Logs/MAWindowsSysReqInstanceReadiness/Read | Считывает данные из таблицы MAWindowsSysReqInstanceReadiness. |
> | Действие | Microsoft.Insights/Logs/NetworkMonitoring/Read | Чтение данных из таблицы NetworkMonitoring. |
> | Действие | Microsoft.Insights/Logs/OfficeActivity/Read | Чтение данных из таблицы OfficeActivity. |
> | Действие | Microsoft.Insights/Logs/Operation/Read | Чтение данных из таблицы Operation. |
> | Действие | Microsoft.Insights/Logs/OutboundConnection/Read | Чтение данных из таблицы OutboundConnection. |
> | Действие | Microsoft.Insights/Logs/Perf/Read | Чтение данных из таблицы Perf. |
> | Действие | Microsoft.Insights/Logs/ProtectionStatus/Read | Чтение данных из таблицы ProtectionStatus. |
> | Действие | Microsoft.Insights/Logs/Read | Чтение данных из всех журналов. |
> | Действие | Microsoft.Insights/Logs/ReservedAzureCommonFields/Read | Чтение данных из таблицы ReservedAzureCommonFields. |
> | Действие | Microsoft.Insights/Logs/ReservedCommonFields/Read | Чтение данных из таблицы ReservedCommonFields. |
> | Действие | Microsoft.Insights/Logs/SCCMAssessmentRecommendation/Read | Чтение данных из таблицы SCCMAssessmentRecommendation. |
> | Действие | Microsoft.Insights/Logs/SCOMAssessmentRecommendation/Read | Чтение данных из таблицы SCOMAssessmentRecommendation. |
> | Действие | Microsoft.Insights/Logs/SecurityAlert/Read | Чтение данных из таблицы SecurityAlert. |
> | Действие | Microsoft.Insights/Logs/SecurityBaseline/Read | Чтение данных из таблицы SecurityBaseline. |
> | Действие | Microsoft.Insights/Logs/SecurityBaselineSummary/Read | Чтение данных из таблицы SecurityBaselineSummary. |
> | Действие | Microsoft.Insights/Logs/SecurityDetection/Read | Чтение данных из таблицы SecurityDetection. |
> | Действие | Microsoft.Insights/Logs/SecurityEvent/Read | Чтение данных из таблицы SecurityEvent. |
> | Действие | Microsoft.Insights/Logs/ServiceFabricOperationalEvent/Read | Чтение данных из таблицы ServiceFabricOperationalEvent. |
> | Действие | Microsoft.Insights/Logs/ServiceFabricReliableActorEvent/Read | Чтение данных из таблицы ServiceFabricReliableActorEvent. |
> | Действие | Microsoft.Insights/Logs/ServiceFabricReliableServiceEvent/Read | Чтение данных из таблицы ServiceFabricReliableServiceEvent. |
> | Действие | Microsoft.Insights/Logs/SfBAssessmentRecommendation/Read | Чтение данных из таблицы SfBAssessmentRecommendation. |
> | Действие | Microsoft.Insights/Logs/SfBOnlineAssessmentRecommendation/Read | Чтение данных из таблицы SfBOnlineAssessmentRecommendation. |
> | Действие | Microsoft.Insights/Logs/SharePointOnlineAssessmentRecommendation/Read | Чтение данных из таблицы SharePointOnlineAssessmentRecommendation. |
> | Действие | Microsoft.Insights/Logs/SPAssessmentRecommendation/Read | Чтение данных из таблицы SPAssessmentRecommendation. |
> | Действие | Microsoft.Insights/Logs/SQLAssessmentRecommendation/Read | Чтение данных из таблицы SQLAssessmentRecommendation. |
> | Действие | Microsoft.Insights/Logs/SQLQueryPerformance/Read | Чтение данных из таблицы SQLQueryPerformance. |
> | Действие | Microsoft.Insights/Logs/Syslog/Read | Чтение данных из таблицы Syslog. |
> | Действие | Microsoft.Insights/Logs/SysmonEvent/Read | Чтение данных из таблицы SysmonEvent. |
> | Действие | Microsoft.Insights/Logs/UAApp/Read | Чтение данных из таблицы UAApp. |
> | Действие | Microsoft.Insights/Logs/UAComputer/Read | Чтение данных из таблицы UAComputer. |
> | Действие | Microsoft.Insights/Logs/UAComputerRank/Read | Чтение данных из таблицы UAComputerRank. |
> | Действие | Microsoft.Insights/Logs/UADriver/Read | Чтение данных из таблицы UADriver. |
> | Действие | Microsoft.Insights/Logs/UADriverProblemCodes/Read | Чтение данных из таблицы UADriverProblemCodes. |
> | Действие | Microsoft.Insights/Logs/UAFeedback/Read | Чтение данных из таблицы UAFeedback. |
> | Действие | Microsoft.Insights/Logs/UAHardwareSecurity/Read | Чтение данных из таблицы UAHardwareSecurity. |
> | Действие | Microsoft.Insights/Logs/UAIESiteDiscovery/Read | Чтение данных из таблицы UAIESiteDiscovery. |
> | Действие | Microsoft.Insights/Logs/UAOfficeAddIn/Read | Чтение данных из таблицы UAOfficeAddIn. |
> | Действие | Microsoft.Insights/Logs/UAProposedActionPlan/Read | Чтение данных из таблицы UAProposedActionPlan. |
> | Действие | Microsoft.Insights/Logs/UASysReqIssue/Read | Чтение данных из таблицы UASysReqIssue. |
> | Действие | Microsoft.Insights/Logs/UAUpgradedComputer/Read | Чтение данных из таблицы UAUpgradedComputer. |
> | Действие | Microsoft.Insights/Logs/Update/Read | Чтение данных из таблицы Update. |
> | Действие | Microsoft.Insights/Logs/UpdateRunProgress/Read | Чтение данных из таблицы UpdateRunProgress. |
> | Действие | Microsoft.Insights/Logs/UpdateSummary/Read | Чтение данных из таблицы UpdateSummary. |
> | Действие | Microsoft.Insights/Logs/Usage/Read | Чтение данных из таблицы Usage. |
> | Действие | Microsoft.Insights/Logs/W3CIISLog/Read | Чтение данных из таблицы W3CIISLog. |
> | Действие | Microsoft.Insights/Logs/WaaSDeploymentStatus/Read | Чтение данных из таблицы WaaSDeploymentStatus. |
> | Действие | Microsoft.Insights/Logs/WaaSInsiderStatus/Read | Чтение данных из таблицы WaaSInsiderStatus. |
> | Действие | Microsoft.Insights/Logs/WaaSUpdateStatus/Read | Чтение данных из таблицы WaaSUpdateStatus. |
> | Действие | Microsoft.Insights/Logs/WDAVStatus/Read | Чтение данных из таблицы WDAVStatus. |
> | Действие | Microsoft.Insights/Logs/WDAVThreat/Read | Чтение данных из таблицы WDAVThreat. |
> | Действие | Microsoft.Insights/Logs/WindowsClientAssessmentRecommendation/Read | Чтение данных из таблицы WindowsClientAssessmentRecommendation. |
> | Действие | Microsoft.Insights/Logs/WindowsFirewall/Read | Чтение данных из таблицы WindowsFirewall. |
> | Действие | Microsoft.Insights/Logs/WindowsServerAssessmentRecommendation/Read | Чтение данных из таблицы WindowsServerAssessmentRecommendation. |
> | Действие | Microsoft.Insights/Logs/WireData/Read | Чтение данных из таблицы WireData. |
> | Действие | Microsoft.Insights/Logs/WUDOAggregatedStatus/Read | Чтение данных из таблицы WUDOAggregatedStatus. |
> | Действие | Microsoft.Insights/Logs/WUDOStatus/Read | Чтение данных из таблицы WUDOStatus. |
> | Действие | Microsoft.Insights/MetricAlerts/Delete | Удаление оповещения метрики. |
> | Действие | Microsoft.Insights/MetricAlerts/Read | Чтение оповещения метрики. |
> | Действие | Microsoft.Insights/MetricAlerts/Status/Read | Чтение состояния оповещения для метрики. |
> | Действие | Microsoft.Insights/MetricAlerts/Write | Создание или изменение оповещения метрики. |
> | Действие | Microsoft.Insights/MetricDefinitions/Microsoft.Insights/Read | Чтение определений метрик |
> | Действие | Microsoft.Insights/MetricDefinitions/providers/Microsoft.Insights/Read | Чтение определений метрик |
> | Действие | Microsoft.Insights/MetricDefinitions/Read | Чтение определений метрик |
> | Действие | Microsoft.Insights/Metricnamespaces/Read | Чтение метрик пространства имен |
> | Действие | Microsoft.Insights/Metrics/Action | Действие метрики |
> | Действие | Microsoft.Insights/Metrics/Microsoft.Insights/Read | Метрик считано |
> | Действие | Microsoft.Insights/Metrics/providers/Metrics/Read | Метрик считано |
> | Действие | Microsoft.Insights/Metrics/Read | Метрик считано |
> | DataAction | Microsoft.Insights/Metrics/Write | Запись показателей |
> | Действие | Microsoft.Insights/MigrateToNewpricingModel/Action | Перевод подписки на новую модель ценообразования. |
> | Действие | Microsoft.Insights/Operations/Read | Считывает операции. |
> | Действие | Microsoft.Insights/Register/Action | Регистрация поставщика Microsoft Insights |
> | Действие | Microsoft.Insights/RollbackToLegacyPricingModel/Action | Откат подписки к прежней версии модели ценообразования. |
> | Действие | Microsoft.Insights/ScheduledQueryRules/Delete | Удаление запланированного правила запроса. |
> | Действие | Microsoft.Insights/ScheduledQueryRules/Read | Считывание запланированного правила запроса. |
> | Действие | Microsoft.Insights/ScheduledQueryRules/Write | Запись запланированного правила запроса. |
> | Действие | Microsoft.Insights/Tenants/Register/Action | Инициализирует поставщик Microsoft Insights. |
> | Действие | Microsoft.Insights/Unregister/Action | Регистрация поставщика Microsoft Insights |
> | Действие | Microsoft.Insights/Webtests/Delete | Удаление конфигурации веб-теста. |
> | Действие | Microsoft.Insights/Webtests/GetToken/Read | Чтение токена веб-теста. |
> | Действие | Microsoft.Insights/Webtests/MetricDefinitions/Read | Чтение определений метрик веб-теста. |
> | Действие | Microsoft.Insights/Webtests/Metrics/Read | Чтение метрик веб-теста. |
> | Действие | Microsoft.Insights/Webtests/Read | Чтение конфигурации веб-теста. |
> | Действие | Microsoft.Insights/Webtests/Write | Запись конфигурации веб-теста. |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Intune/diagnosticsettings/delete | Удаление параметра диагностики. |
> | Действие | Microsoft.Intune/diagnosticsettings/read | Чтение параметра диагностики. |
> | Действие | Microsoft.Intune/diagnosticsettings/write | Запись параметра диагностики. |
> | Действие | Microsoft.Intune/diagnosticsettingscategories/read | Чтение категорий параметра диагностики. |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.IoTCentral/appTemplates/action | Возвращает все шаблоны приложений, доступных на Azure IoT Central |
> | Действие | Microsoft.IoTCentral/checkNameAvailability/action | Проверка доступности для имени приложения IoT Central |
> | Действие | Microsoft.IoTCentral/checkSubdomainAvailability/action | Проверка доступности для поддомена приложения IoT Central |
> | Действие | Microsoft.IoTCentral/IoTApps/delete | Удаление приложений IoT Central |
> | Действие | Microsoft.IoTCentral/IoTApps/read | Получение одного приложения IoT Central |
> | Действие | Microsoft.IoTCentral/IoTApps/write | Создание или обновлений приложений IoT Central |
> | Действие | Microsoft.IoTCentral/operations/read | Получение всех доступных операций в приложениях IoT Central |
> | Действие | Microsoft.IoTCentral/register/action | Регистрация подписки для поставщика ресурсов IoT Central Azure |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.IoTSpaces/Graph/delete | Удаляет ресурс Graph Microsoft.IoTSpaces |
> | Действие | Microsoft.IoTSpaces/Graph/read | Получает ресурсы Graph Microsoft.IoTSpaces |
> | Действие | Microsoft.IoTSpaces/Graph/write | Создает ресурс Graph Microsoft.IoTSpaces |
> | Действие | Microsoft.IoTSpaces/register/action | Регистрация подписки для поставщика ресурсов Graph Microsoft.IoTSpaces, чтобы обеспечить создание ресурсов. |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.KeyVault/checkNameAvailability/read | Проверяет, является ли имя Key Vault допустимым и неиспользуемым. |
> | Действие | Microsoft.KeyVault/deletedVaults/read | Отображает свойства обратимо удаленных хранилищ Key Vault. |
> | Действие | Microsoft.KeyVault/hsmPools/delete | Удаление пула HSM. |
> | Действие | Microsoft.KeyVault/hsmPools/joinVault/action | Присоединение хранилища ключей к пулу HSM. |
> | Действие | Microsoft.KeyVault/hsmPools/read | Просмотр свойств пула HSM. |
> | Действие | Microsoft.KeyVault/hsmPools/write | Создание нового пула HSM или обновление свойств существующего пула HSM. |
> | Действие | Microsoft.KeyVault/locations/deletedVaults/purge/action | Очищает обратимо удаленное хранилище Key Vault. |
> | Действие | Microsoft.KeyVault/locations/deletedVaults/read | Отображает свойства обратимо удаленного Key Vault. |
> | Действие | Microsoft.KeyVault/locations/deleteVirtualNetworkOrSubnets/action | Уведомление Microsoft.KeyVault о том, что идет удаление виртуальной сети или подсети. |
> | Действие | Microsoft.KeyVault/locations/operationResults/read | Проверяет результат длительной операции. |
> | Действие | Microsoft.KeyVault/operations/read | Получение списка операций, доступных в поставщике ресурсов Microsoft.KeyVault. |
> | Действие | Microsoft.KeyVault/register/action | Регистрирует подписку. |
> | Действие | Microsoft.KeyVault/unregister/action | Отмена регистрации подписки. |
> | Действие | Microsoft.KeyVault/vaults/accessPolicies/write | Обновляет существующую политику доступа путем объединения, замены или добавления новой политики доступа в хранилище. |
> | Действие | Microsoft.KeyVault/vaults/delete | Удаляет Key Vault. |
> | Действие | Microsoft.KeyVault/vaults/deploy/action | Включает доступ к секретам в Key Vault при развертывании ресурсов Azure. |
> | Действие | Microsoft.KeyVault/vaults/eventGridFilters/delete | Уведомляет Microsoft.KeyVault об удалении подписки EventGrid (Сетка событий) для Key Vault |
> | Действие | Microsoft.KeyVault/vaults/eventGridFilters/read | Уведомляет Microsoft.KeyVault о просмотре подписки EventGrid (Сетка событий) для Key Vault |
> | Действие | Microsoft.KeyVault/vaults/eventGridFilters/write | Уведомляет Microsoft.KeyVault о создании новой подписки EventGrid (Сетка событий) для Key Vault |
> | Действие | Microsoft.KeyVault/vaults/read | Отображает свойства Key Vault. |
> | Действие | Microsoft.KeyVault/vaults/secrets/read | Отображает свойства секрета, но не его значение. |
> | Действие | Microsoft.KeyVault/vaults/secrets/write | Создает новый секрет или обновляет значение существующего. |
> | Действие | Microsoft.KeyVault/vaults/write | Создает новое хранилище Key Vault или обновить свойства существующего. |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Kusto/Clusters/Activate/action | Кластер запускается. |
> | Действие | Microsoft.Kusto/Clusters/CheckNameAvailability/action | Проверяет доступность имени кластера. |
> | Действие | Microsoft.Kusto/Clusters/Databases/AddPrincipals/action | Добавляет участников базы данных. |
> | Действие | Microsoft.Kusto/Clusters/Databases/CheckNameAvailability/action | Проверяет доступность имени для данного типа. |
> | Действие | Microsoft.Kusto/Clusters/Databases/DataConnections/delete | Удаление ресурса подключений к данным. |
> | Действие | Microsoft.Kusto/Clusters/Databases/DataConnections/read | Чтение ресурса подключений к данным. |
> | Действие | Microsoft.Kusto/Clusters/Databases/DataConnections/write | Записывает ресурс подключений данных. |
> | Действие | Microsoft.Kusto/Clusters/Databases/DataConnectionValidation/action | Проверяет подключение к данным базы данных. |
> | Действие | Microsoft.Kusto/Clusters/Databases/delete | Удаляет ресурс базы данных. |
> | Действие | Microsoft.Kusto/Clusters/Databases/EventHubConnections/delete | Удаляет ресурс подключения концентратора событий. |
> | Действие | Microsoft.Kusto/Clusters/Databases/EventHubConnections/read | Считывает ресурс подключения концентратора событий. |
> | Действие | Microsoft.Kusto/Clusters/Databases/EventHubConnections/write | Записывает ресурс подключения концентратора событий. |
> | Действие | Microsoft.Kusto/Clusters/Databases/EventHubConnectionValidation/action | Проверяет подключение к базе данных концентратора событий. |
> | Действие | Microsoft.Kusto/Clusters/Databases/ListPrincipals/action | Списки участники баз данных. |
> | Действие | Microsoft.Kusto/Clusters/Databases/read | Читает ресурс базы данных. |
> | Действие | Microsoft.Kusto/Clusters/Databases/RemovePrincipals/action | Удаляет участники баз данных. |
> | Действие | Microsoft.Kusto/Clusters/Databases/write | Записывает ресурс базы данных. |
> | Действие | Microsoft.Kusto/Clusters/Deactivate/action | Останавливает кластера. |
> | Действие | Microsoft.Kusto/Clusters/delete | Удаляет кластерный ресурс. |
> | Действие | Microsoft.Kusto/Clusters/read | Читает кластерный ресурс. |
> | Действие | Microsoft.Kusto/Clusters/SKUs/read | Считывает SKU ресурса кластера. |
> | Действие | Microsoft.Kusto/Clusters/Start/action | Кластер запускается. |
> | Действие | Microsoft.Kusto/Clusters/Stop/action | Останавливает кластера. |
> | Действие | Microsoft.Kusto/Clusters/write | Записывает кластерный ресурс. |
> | Действие | Microsoft.Kusto/Locations/CheckNameAvailability/action | Проверяет доступность имени ресурса. |
> | Действие | Microsoft.Kusto/locations/operationresults/read | Чтение ресурсов операций |
> | Действие | Microsoft.Kusto/Operations/read | Чтение ресурсов операций |
> | Действие | Microsoft.Kusto/Register/action | Регистрирует подписку в поставщике ресурсов Kusto. |
> | Действие | Microsoft.Kusto/SKUs/read | Считывает ресурс SKU ". |
> | Действие | Microsoft.Kusto/Unregister/action | Отмена регистрации подписки в поставщике ресурсов Kusto. |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.LabServices/labAccounts/CreateLab/action | Создание лаборатории в учетной записи лаборатории. |
> | Действие | Microsoft.LabServices/labAccounts/delete | Удаление учетных записей лабораторий. |
> | Действие | Microsoft.LabServices/labAccounts/galleryImages/delete | Удаляет образы коллекции. |
> | Действие | Microsoft.LabServices/labAccounts/galleryImages/read | Считывает образы из коллекции. |
> | Действие | Microsoft.LabServices/labAccounts/galleryImages/write | Добавление или изменение образов из коллекции. |
> | Действие | Microsoft.LabServices/labAccounts/GetRegionalAvailability/action | Позволяет получать сведения о доступности по регионам для каждой категории размеров, настроенной в учетной записи лаборатории. |
> | Действие | Microsoft.LabServices/labAccounts/labs/AddUsers/action | Добавление пользователей в лабораторию |
> | Действие | Microsoft.LabServices/labAccounts/labs/delete | Удаляет лаборатории. |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/ClaimAny/action | Утверждение случайной среды пользователя в параметрах среды. |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/delete | Удаление параметра среды. |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Claim/action | Утверждение среды и назначение ее пользователю. |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/delete | Удаляет среды. |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/read | Считывает среды. |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/ResetPassword/action | Сброс пароля пользователя в среде |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Start/action | Запускает среду, запустив все ресурсы в ней. |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Stop/action | Останавливает выполнение среды, остановив все ресурсы в ней. |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/write | Добавляет или изменяет среды. |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/Publish/action | Подготовка и отзыв необходимых ресурсов для параметра среды на основе текущего состояния параметра лаборатории или среды. |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/read | Чтение параметра среды. |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/SaveImage/action | Сохраняет текущий образ шаблона в общей коллекции в учетной записи лаборатории |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/delete | Удаляет расписания. |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/read | Считывает расписания. |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/write | Добавляет или изменяет расписания. |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/Start/action | Запускает шаблон, запустив все ресурсы внутри него. |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/Stop/action | Останавливает шаблон путем остановки всех ресурсов внутри него. |
> | Действие | Microsoft.LabServices/labAccounts/labs/environmentSettings/write | Добавление или изменение параметра среды. |
> | Действие | Microsoft.LabServices/labAccounts/labs/read | Считывает лаборатории. |
> | Действие | Microsoft.LabServices/labAccounts/labs/Register/action | Регистрация в управляемой лаборатории. |
> | Действие | Microsoft.LabServices/labAccounts/labs/SendEmail/action | Отправка электронного письма со ссылкой для регистрации в лаборатории. |
> | Действие | Microsoft.LabServices/labAccounts/labs/users/delete | Удаление пользователей. |
> | Действие | Microsoft.LabServices/labAccounts/labs/users/read | Чтение пользователей. |
> | Действие | Microsoft.LabServices/labAccounts/labs/users/write | Добавление или изменение пользователей. |
> | Действие | Microsoft.LabServices/labAccounts/labs/write | Добавляет или изменяет лаборатории. |
> | Действие | Microsoft.LabServices/labAccounts/read | Чтение учетных записей лабораторий. |
> | Действие | Microsoft.LabServices/labAccounts/sharedGalleries/delete | Удаление общих коллекций. |
> | Действие | Microsoft.LabServices/labAccounts/sharedGalleries/read | Чтение общих коллекций. |
> | Действие | Microsoft.LabServices/labAccounts/sharedGalleries/write | Добавление или изменение общих коллекций. |
> | Действие | Microsoft.LabServices/labAccounts/sharedImages/delete | Удаление общих образов. |
> | Действие | Microsoft.LabServices/labAccounts/sharedImages/read | Чтение общих образов. |
> | Действие | Microsoft.LabServices/labAccounts/sharedImages/write | Добавление или изменение общих образов. |
> | Действие | Microsoft.LabServices/labAccounts/write | Добавление или изменение учетных записей лабораторий. |
> | Действие | Microsoft.LabServices/locations/operations/read | Считывает операции. |
> | Действие | Microsoft.LabServices/register/action | Регистрирует подписку. |
> | Действие | Microsoft.LabServices/users/GetEnvironment/action | Получает сведения о виртуальных машинах. |
> | Действие | Microsoft.LabServices/users/GetOperationBatchStatus/action | Получает состояние пакетной операции. |
> | Действие | Microsoft.LabServices/users/GetOperationStatus/action | Получение состояния длительной операции. |
> | Действие | Microsoft.LabServices/users/GetPersonalPreferences/action | Получение личных настроек для пользователя |
> | Действие | Microsoft.LabServices/users/ListAllEnvironments/action | Список всех сред для пользователя |
> | Действие | Microsoft.LabServices/users/ListEnvironments/action | Перечисление сред для пользователя. |
> | Действие | Microsoft.LabServices/users/ListLabs/action | Перечисление лабораторий для пользователя. |
> | Действие | Microsoft.LabServices/users/Register/action | Регистрация пользователя в управляемой лаборатории. |
> | Действие | Microsoft.LabServices/users/ResetPassword/action | Сброс пароля пользователя в среде |
> | Действие | Microsoft.LabServices/users/StartEnvironment/action | Запускает среду, запустив все ресурсы в ней. |
> | Действие | Microsoft.LabServices/users/StopEnvironment/action | Останавливает выполнение среды, остановив все ресурсы в ней. |

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | DataAction | Microsoft.LogAnalytics/logs/ADAssessmentRecommendation/read | Чтение данных из таблицы ADAssessmentRecommendation. |
> | DataAction | Microsoft.LogAnalytics/logs/ADReplicationResult/read | Чтение данных из таблицы ADReplicationResult. |
> | DataAction | Microsoft.LogAnalytics/logs/ADSecurityAssessmentRecommendation/read | Чтение данных из таблицы ADSecurityAssessmentRecommendation. |
> | DataAction | Microsoft.LogAnalytics/logs/Alert/read | Чтение данных из таблицы Alert. |
> | DataAction | Microsoft.LogAnalytics/logs/AlertHistory/read | Чтение данных из таблицы AlertHistory. |
> | DataAction | Microsoft.LogAnalytics/logs/AppCenterError/read | Чтение данных из таблицы AppCenterError |
> | DataAction | Microsoft.LogAnalytics/logs/ApplicationInsights/read | Чтение данных из таблицы ApplicationInsights. |
> | DataAction | Microsoft.LogAnalytics/logs/AuditLogs/read | Чтение данных из таблицы AuditLogs |
> | DataAction | Microsoft.LogAnalytics/logs/AzureActivity/read | Чтение данных из таблицы AzureActivity. |
> | DataAction | Microsoft.LogAnalytics/logs/AzureMetrics/read | Чтение данных из таблицы AzureMetrics. |
> | DataAction | Microsoft.LogAnalytics/logs/BoundPort/read | Чтение данных из таблицы BoundPort. |
> | DataAction | Microsoft.LogAnalytics/logs/CommonSecurityLog/read | Чтение данных из таблицы CommonSecurityLog. |
> | DataAction | Microsoft.LogAnalytics/logs/ComputerGroup/read | Чтение данных из таблицы ComputerGroup. |
> | DataAction | Microsoft.LogAnalytics/logs/ConfigurationChange/read | Чтение данных из таблицы ConfigurationChange. |
> | DataAction | Microsoft.LogAnalytics/logs/ConfigurationData/read | Чтение данных из таблицы ConfigurationData. |
> | DataAction | Microsoft.LogAnalytics/logs/ContainerImageInventory/read | Чтение данных из таблицы ContainerImageInventory. |
> | DataAction | Microsoft.LogAnalytics/logs/ContainerInventory/read | Чтение данных из таблицы ContainerInventory. |
> | DataAction | Microsoft.LogAnalytics/logs/ContainerLog/read | Чтение данных из таблицы ContainerLog. |
> | DataAction | Microsoft.LogAnalytics/logs/ContainerServiceLog/read | Чтение данных из таблицы ContainerServiceLog. |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceAppCrash/read | Чтение данных из таблицы DeviceAppCrash. |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceAppLaunch/read | Чтение данных из таблицы DeviceAppLaunch. |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceCalendar/read | Чтение данных из таблицы DeviceCalendar. |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceCleanup/read | Чтение данных из таблицы DeviceCleanup. |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceConnectSession/read | Чтение данных из таблицы DeviceConnectSession. |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceEtw/read | Чтение данных из таблицы DeviceEtw. |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceHardwareHealth/read | Чтение данных из таблицы DeviceHardwareHealth. |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceHealth/read | Чтение данных из таблицы DeviceHealth. |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceHeartbeat/read | Чтение данных из таблицы DeviceHeartbeat. |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceSkypeHeartbeat/read | Чтение данных из таблицы DeviceSkypeHeartbeat. |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceSkypeSignIn/read | Чтение данных из таблицы DeviceSkypeSignIn. |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceSleepState/read | Чтение данных из таблицы DeviceSleepState. |
> | DataAction | Microsoft.LogAnalytics/logs/DHAppFailure/read | Чтение данных из таблицы DHAppFailure. |
> | DataAction | Microsoft.LogAnalytics/logs/DHAppReliability/read | Чтение данных из таблицы DHAppReliability. |
> | DataAction | Microsoft.LogAnalytics/logs/DHDriverReliability/read | Чтение данных из таблицы DHDriverReliability. |
> | DataAction | Microsoft.LogAnalytics/logs/DHLogonFailures/read | Чтение данных из таблицы DHLogonFailures. |
> | DataAction | Microsoft.LogAnalytics/logs/DHLogonMetrics/read | Чтение данных из таблицы DHLogonMetrics. |
> | DataAction | Microsoft.LogAnalytics/logs/DHOSCrashData/read | Чтение данных из таблицы DHOSCrashData. |
> | DataAction | Microsoft.LogAnalytics/logs/DHOSReliability/read | Чтение данных из таблицы DHOSReliability. |
> | DataAction | Microsoft.LogAnalytics/logs/DHWipAppLearning/read | Чтение данных из таблицы DHWipAppLearning. |
> | DataAction | Microsoft.LogAnalytics/logs/DnsEvents/read | Чтение данных из таблицы DnsEvents. |
> | DataAction | Microsoft.LogAnalytics/logs/DnsInventory/read | Чтение данных из таблицы DnsInventory. |
> | DataAction | Microsoft.LogAnalytics/logs/ETWEvent/read | Чтение данных из таблицы ETWEvent. |
> | DataAction | Microsoft.LogAnalytics/logs/Event/read | Чтение данных из таблицы Event. |
> | DataAction | Microsoft.LogAnalytics/logs/ExchangeAssessmentRecommendation/read | Чтение данных из таблицы ExchangeAssessmentRecommendation. |
> | DataAction | Microsoft.LogAnalytics/logs/ExchangeOnlineAssessmentRecommendation/read | Чтение данных из таблицы ExchangeOnlineAssessmentRecommendation. |
> | DataAction | Microsoft.LogAnalytics/logs/Heartbeat/read | Чтение данных из таблицы Heartbeat. |
> | DataAction | Microsoft.LogAnalytics/logs/IISAssessmentRecommendation/read | Чтение данных из таблицы IISAssessmentRecommendation. |
> | DataAction | Microsoft.LogAnalytics/logs/InboundConnection/read | Чтение данных из таблицы InboundConnection. |
> | DataAction | Microsoft.LogAnalytics/logs/KubeEvents/read | Чтение данных из таблицы KubeEvents |
> | DataAction | Microsoft.LogAnalytics/logs/KubeNodeInventory/read | Чтение данных из таблицы KubeNodeInventory. |
> | DataAction | Microsoft.LogAnalytics/logs/KubePodInventory/read | Чтение данных из таблицы KubePodInventory. |
> | DataAction | Microsoft.LogAnalytics/logs/KubeServices/read | Чтение данных из таблицы KubeServices |
> | DataAction | Microsoft.LogAnalytics/logs/LinuxAuditLog/read | Чтение данных из таблицы LinuxAuditLog. |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplication/read | Чтение данных из таблицы MAApplication. |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationHealth/read | Чтение данных из таблицы MAApplicationHealth. |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationHealthAlternativeVersions/read | Считывает данные из таблицы MAApplicationHealthAlternativeVersions. |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationHealthIssues/read | Считывает данные из таблицы MAApplicationHealthIssues. |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationInstance/read | Чтение данных из таблицы MAApplicationInstance. |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationInstanceReadiness/read | Считывает данные из таблицы MAApplicationInstanceReadiness. |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationReadiness/read | Чтение данных из таблицы MAApplicationReadiness. |
> | DataAction | Microsoft.LogAnalytics/logs/MADeploymentPlan/read | Чтение данных из таблицы MADeploymentPlan. |
> | DataAction | Microsoft.LogAnalytics/logs/MADevice/read | Чтение данных из таблицы MADevice. |
> | DataAction | Microsoft.LogAnalytics/logs/MADeviceNotEnrolled/read | Считывание данных из таблицы MADeviceNotEnrolled |
> | DataAction | Microsoft.LogAnalytics/logs/MADevicePnPHealth/read | Считывает данные из таблицы MADevicePnPHealth. |
> | DataAction | Microsoft.LogAnalytics/logs/MADevicePnPHealthAlternativeVersions/read | Считывает данные из таблицы MADevicePnPHealthAlternativeVersions. |
> | DataAction | Microsoft.LogAnalytics/logs/MADevicePnPHealthIssues/read | Считывает данные из таблицы MADevicePnPHealthIssues. |
> | DataAction | Microsoft.LogAnalytics/logs/MADeviceReadiness/read | Чтение данных из таблицы MADeviceReadiness. |
> | DataAction | Microsoft.LogAnalytics/logs/MADriverInstanceReadiness/read | Считывает данные из таблицы MADriverInstanceReadiness. |
> | DataAction | Microsoft.LogAnalytics/logs/MADriverReadiness/read | Чтение данных из таблицы MADriverReadiness. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddin/read | Чтение данных из таблицы MAOfficeAddin. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddinHealth/read | Чтение данных из таблицы MAOfficeAddinHealth. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddinHealthIssues/read | Считывает данные из таблицы MAOfficeAddinHealthIssues. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddinInstance/read | Чтение данных из таблицы MAOfficeAddinInstance. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddinInstanceReadiness/read | Считывает данные из таблицы MAOfficeAddinInstanceReadiness. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddinReadiness/read | Чтение данных из таблицы MAOfficeAddinReadiness. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeApp/read | Чтение данных из таблицы MAOfficeApp. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAppHealth/read | Чтение данных из таблицы MAOfficeAppHealth. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAppInstance/read | Чтение данных из таблицы MAOfficeAppInstance. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAppReadiness/read | Чтение данных из таблицы MAOfficeAppReadiness. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeBuildInfo/read | Чтение данных из таблицы MAOfficeBuildInfo. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeCurrencyAssessment/read | Чтение данных из таблицы MAOfficeCurrencyAssessment. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeCurrencyAssessmentDailyCounts/read | Чтение данных из таблицы MAOfficeCurrencyAssessmentDailyCounts. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeDeploymentStatus/read | Чтение данных из таблицы MAOfficeDeploymentStatus. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeMacroHealth/read | Считывает данные из таблицы MAOfficeMacroHealth. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeMacroHealthIssues/read | Считывает данные из таблицы MAOfficeMacroHealthIssues. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeMacroIssueInstanceReadiness/read | Считывает данные из таблицы MAOfficeMacroIssueInstanceReadiness. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeMacroIssueReadiness/read | Чтение данных из таблицы MAOfficeMacroIssueReadiness. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeMacroSummary/read | Чтение данных из таблицы MAOfficeMacroSummary. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeSuite/read | Чтение данных из таблицы MAOfficeSuite. |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeSuiteInstance/read | Чтение данных из таблицы MAOfficeSuiteInstance. |
> | DataAction | Microsoft.LogAnalytics/logs/MAProposedPilotDevices/read | Чтение данных из таблицы MAProposedPilotDevices. |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsBuildInfo/read | Чтение данных из таблицы MAWindowsBuildInfo. |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsCurrencyAssessment/read | Чтение данных из таблицы MAWindowsCurrencyAssessment. |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsCurrencyAssessmentDailyCounts/read | Чтение данных из таблицы MAWindowsCurrencyAssessmentDailyCounts. |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsDeploymentStatus/read | Чтение данных из таблицы MAWindowsDeploymentStatus. |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsSysReqInstanceReadiness/read | Считывает данные из таблицы MAWindowsSysReqInstanceReadiness. |
> | DataAction | Microsoft.LogAnalytics/logs/NetworkMonitoring/read | Чтение данных из таблицы NetworkMonitoring. |
> | DataAction | Microsoft.LogAnalytics/logs/OfficeActivity/read | Чтение данных из таблицы OfficeActivity. |
> | DataAction | Microsoft.LogAnalytics/logs/Operation/read | Чтение данных из таблицы Operation. |
> | DataAction | Microsoft.LogAnalytics/logs/OutboundConnection/read | Чтение данных из таблицы OutboundConnection. |
> | DataAction | Microsoft.LogAnalytics/logs/Perf/read | Чтение данных из таблицы Perf. |
> | DataAction | Microsoft.LogAnalytics/logs/ProtectionStatus/read | Чтение данных из таблицы ProtectionStatus. |
> | Действие | Microsoft.LogAnalytics/logs/read | Чтение данных из всех журналов. |
> | DataAction | Microsoft.LogAnalytics/logs/ReservedAzureCommonFields/read | Чтение данных из таблицы ReservedAzureCommonFields. |
> | DataAction | Microsoft.LogAnalytics/logs/ReservedCommonFields/read | Чтение данных из таблицы ReservedCommonFields. |
> | DataAction | Microsoft.LogAnalytics/logs/SCCMAssessmentRecommendation/read | Чтение данных из таблицы SCCMAssessmentRecommendation. |
> | DataAction | Microsoft.LogAnalytics/logs/SCOMAssessmentRecommendation/read | Чтение данных из таблицы SCOMAssessmentRecommendation. |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityAlert/read | Чтение данных из таблицы SecurityAlert. |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityBaseline/read | Чтение данных из таблицы SecurityBaseline. |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityBaselineSummary/read | Чтение данных из таблицы SecurityBaselineSummary. |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityDetection/read | Чтение данных из таблицы SecurityDetection. |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityEvent/read | Чтение данных из таблицы SecurityEvent. |
> | DataAction | Microsoft.LogAnalytics/logs/ServiceFabricOperationalEvent/read | Чтение данных из таблицы ServiceFabricOperationalEvent. |
> | DataAction | Microsoft.LogAnalytics/logs/ServiceFabricReliableActorEvent/read | Чтение данных из таблицы ServiceFabricReliableActorEvent. |
> | DataAction | Microsoft.LogAnalytics/logs/ServiceFabricReliableServiceEvent/read | Чтение данных из таблицы ServiceFabricReliableServiceEvent. |
> | DataAction | Microsoft.LogAnalytics/logs/SfBAssessmentRecommendation/read | Чтение данных из таблицы SfBAssessmentRecommendation. |
> | DataAction | Microsoft.LogAnalytics/logs/SfBOnlineAssessmentRecommendation/read | Чтение данных из таблицы SfBOnlineAssessmentRecommendation. |
> | DataAction | Microsoft.LogAnalytics/logs/SharePointOnlineAssessmentRecommendation/read | Чтение данных из таблицы SharePointOnlineAssessmentRecommendation. |
> | DataAction | Microsoft.LogAnalytics/logs/SigninLogs/read | Чтение данных из таблицы SigninLogs |
> | DataAction | Microsoft.LogAnalytics/logs/SPAssessmentRecommendation/read | Чтение данных из таблицы SPAssessmentRecommendation. |
> | DataAction | Microsoft.LogAnalytics/logs/SQLAssessmentRecommendation/read | Чтение данных из таблицы SQLAssessmentRecommendation. |
> | DataAction | Microsoft.LogAnalytics/logs/SQLQueryPerformance/read | Чтение данных из таблицы SQLQueryPerformance. |
> | DataAction | Microsoft.LogAnalytics/logs/Syslog/read | Чтение данных из таблицы Syslog. |
> | DataAction | Microsoft.LogAnalytics/logs/SysmonEvent/read | Чтение данных из таблицы SysmonEvent. |
> | DataAction | Microsoft.LogAnalytics/logs/Tables.Custom/read | Чтение данных из любого пользовательского журнала. |
> | DataAction | Microsoft.LogAnalytics/logs/UAApp/read | Чтение данных из таблицы UAApp. |
> | DataAction | Microsoft.LogAnalytics/logs/UAComputer/read | Чтение данных из таблицы UAComputer. |
> | DataAction | Microsoft.LogAnalytics/logs/UAComputerRank/read | Чтение данных из таблицы UAComputerRank. |
> | DataAction | Microsoft.LogAnalytics/logs/UADriver/read | Чтение данных из таблицы UADriver. |
> | DataAction | Microsoft.LogAnalytics/logs/UADriverProblemCodes/read | Чтение данных из таблицы UADriverProblemCodes. |
> | DataAction | Microsoft.LogAnalytics/logs/UAFeedback/read | Чтение данных из таблицы UAFeedback. |
> | DataAction | Microsoft.LogAnalytics/logs/UAHardwareSecurity/read | Чтение данных из таблицы UAHardwareSecurity. |
> | DataAction | Microsoft.LogAnalytics/logs/UAIESiteDiscovery/read | Чтение данных из таблицы UAIESiteDiscovery. |
> | DataAction | Microsoft.LogAnalytics/logs/UAOfficeAddIn/read | Чтение данных из таблицы UAOfficeAddIn. |
> | DataAction | Microsoft.LogAnalytics/logs/UAProposedActionPlan/read | Чтение данных из таблицы UAProposedActionPlan. |
> | DataAction | Microsoft.LogAnalytics/logs/UASysReqIssue/read | Чтение данных из таблицы UASysReqIssue. |
> | DataAction | Microsoft.LogAnalytics/logs/UAUpgradedComputer/read | Чтение данных из таблицы UAUpgradedComputer. |
> | DataAction | Microsoft.LogAnalytics/logs/Update/read | Чтение данных из таблицы Update. |
> | DataAction | Microsoft.LogAnalytics/logs/UpdateRunProgress/read | Чтение данных из таблицы UpdateRunProgress. |
> | DataAction | Microsoft.LogAnalytics/logs/UpdateSummary/read | Чтение данных из таблицы UpdateSummary. |
> | DataAction | Microsoft.LogAnalytics/logs/Usage/read | Чтение данных из таблицы Usage. |
> | DataAction | Microsoft.LogAnalytics/logs/VMBoundPort/read | Считывает данные из таблицы VMBoundPort. |
> | DataAction | Microsoft.LogAnalytics/logs/VMConnection/read | Считывает данные из таблицы VMConnection. |
> | DataAction | Microsoft.LogAnalytics/logs/W3CIISLog/read | Чтение данных из таблицы W3CIISLog. |
> | DataAction | Microsoft.LogAnalytics/logs/WaaSDeploymentStatus/read | Чтение данных из таблицы WaaSDeploymentStatus. |
> | DataAction | Microsoft.LogAnalytics/logs/WaaSInsiderStatus/read | Чтение данных из таблицы WaaSInsiderStatus. |
> | DataAction | Microsoft.LogAnalytics/logs/WaaSUpdateStatus/read | Чтение данных из таблицы WaaSUpdateStatus. |
> | DataAction | Microsoft.LogAnalytics/logs/WDAVStatus/read | Чтение данных из таблицы WDAVStatus. |
> | DataAction | Microsoft.LogAnalytics/logs/WDAVThreat/read | Чтение данных из таблицы WDAVThreat. |
> | DataAction | Microsoft.LogAnalytics/logs/WindowsClientAssessmentRecommendation/read | Чтение данных из таблицы WindowsClientAssessmentRecommendation. |
> | DataAction | Microsoft.LogAnalytics/logs/WindowsEvent/read | Чтение данных из таблицы WindowsEvent |
> | DataAction | Microsoft.LogAnalytics/logs/WindowsFirewall/read | Чтение данных из таблицы WindowsFirewall. |
> | DataAction | Microsoft.LogAnalytics/logs/WindowsServerAssessmentRecommendation/read | Чтение данных из таблицы WindowsServerAssessmentRecommendation. |
> | DataAction | Microsoft.LogAnalytics/logs/WireData/read | Чтение данных из таблицы WireData. |
> | DataAction | Microsoft.LogAnalytics/logs/WorkloadMonitoringPerf/read | Чтение данных из таблицы WorkloadMonitoringPerf. |
> | DataAction | Microsoft.LogAnalytics/logs/WUDOAggregatedStatus/read | Чтение данных из таблицы WUDOAggregatedStatus. |
> | DataAction | Microsoft.LogAnalytics/logs/WUDOStatus/read | Чтение данных из таблицы WUDOStatus. |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Logic/integrationAccounts/agreements/delete | Удаление соглашения в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/agreements/listContentCallbackUrl/action | Получение URL-адреса обратного вызова для содержимого соглашения в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/agreements/read | Чтение соглашения в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/agreements/write | Создание или обновление соглашения в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/assemblies/delete | Удаление сборки в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/assemblies/listContentCallbackUrl/action | Получение URL-адреса обратного вызова для содержимого сборки в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/assemblies/read | Чтение сборки в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/assemblies/write | Создание или обновление сборки в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/batchConfigurations/delete | Удаление конфигурации пакета в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/batchConfigurations/read | Чтение конфигурации пакета в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/batchConfigurations/write | Создание или обновление конфигурации пакета в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/certificates/delete | Удаление сертификата в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/certificates/read | Чтение сертификата в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/certificates/write | Создание или обновление сертификата в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/delete | Удаление учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/join/action | Присоединение к учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/listCallbackUrl/action | Получение URL-адреса обратного вызова для учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/listKeyVaultKeys/action | Получение ключей в хранилище ключей. |
> | Действие | Microsoft.Logic/integrationAccounts/logTrackingEvents/action | Регистрация событий отслеживания в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/maps/delete | Удаление сопоставления в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/maps/listContentCallbackUrl/action | Получение URL-адреса обратного вызова для содержимого сопоставления в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/maps/read | Чтение сопоставления в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/maps/write | Создание или обновление сопоставления в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/partners/delete | Удаление партнера в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/partners/listContentCallbackUrl/action | Получение URL-адреса обратного вызова для содержимого партнера в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/partners/read | Чтение партнера в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/partners/write | Создание или обновление партнера в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/providers/Microsoft.Insights/logDefinitions/read | Чтение определений журналов для учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/read | Чтение учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/regenerateAccessKey/action | Повторно создает секреты ключей доступа. |
> | Действие | Microsoft.Logic/integrationAccounts/schemas/delete | Удаление схемы в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/schemas/listContentCallbackUrl/action | Получение URL-адреса обратного вызова для содержимого схемы в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/schemas/read | Чтение схемы в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/schemas/write | Создание или обновление схемы в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/sessions/delete | Удаление сеанса в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/sessions/read | Чтение конфигурации пакета в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/sessions/write | Создание или обновление сеанса в учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationAccounts/write | Создание или обновление учетной записи интеграции. |
> | Действие | Microsoft.Logic/integrationServiceEnvironments/delete | Удаляет среду службы интеграции. |
> | Действие | Microsoft.Logic/integrationServiceEnvironments/join/action | Выполняет присоединение к среде службы интеграции. |
> | Действие | Microsoft.Logic/integrationServiceEnvironments/managedApis/apiOperations/read | Считывает операцию управляемого API для среды службы интеграции. |
> | Действие | Microsoft.Logic/integrationServiceEnvironments/managedApis/read | Считывает управляемый API для среды службы интеграции. |
> | Действие | Microsoft.Logic/integrationServiceEnvironments/operationStatuses/read | Считывание состояния операции среды службы интеграции. |
> | Действие | Microsoft.Logic/integrationServiceEnvironments/providers/Microsoft.Insights/metricDefinitions/read | Считывает определения метрик среды службы интеграции. |
> | Действие | Microsoft.Logic/integrationServiceEnvironments/read | Считывает среду службы интеграции. |
> | Действие | Microsoft.Logic/integrationServiceEnvironments/write | Создает или обновляет среду службы интеграции. |
> | Действие | Microsoft.Logic/locations/workflows/recommendOperationGroups/action | Получает рекомендуемые группы операций для рабочего процесса. |
> | Действие | Microsoft.Logic/locations/workflows/validate/action | Проверяет рабочий процесс. |
> | Действие | Microsoft.Logic/operations/read | Получение операции. |
> | Действие | Microsoft.Logic/register/action | Регистрация поставщика ресурсов Microsoft.Logic для заданной подписки. |
> | Действие | Microsoft.Logic/workflows/accessKeys/delete | Удаляет ключ доступа. |
> | Действие | Microsoft.Logic/workflows/accessKeys/list/action | Выводит список секретов ключей доступа. |
> | Действие | Microsoft.Logic/workflows/accessKeys/read | Считывает ключ доступа. |
> | Действие | Microsoft.Logic/workflows/accessKeys/regenerate/action | Повторно создает секреты ключей доступа. |
> | Действие | Microsoft.Logic/workflows/accessKeys/write | Создает или обновляет ключ доступа. |
> | Действие | Microsoft.Logic/workflows/delete | Удаляет рабочий процесс. |
> | Действие | Microsoft.Logic/workflows/disable/action | Отключение рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/enable/action | Включение рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/listCallbackUrl/action | Возвращает URL-адрес обратного вызова для рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/listSwagger/action | Возвращает определения Swagger рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/move/action | Назначает рабочему процессу, для которого задан идентификатор существующей подписки, группа ресурсов и (или) имя, другой идентификатор подписки, группу ресурсов и (или) имя. |
> | Действие | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/read | Чтение параметров диагностики рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/write | Создание или обновление параметров диагностики рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/providers/Microsoft.Insights/logDefinitions/read | Чтение определений журналов рабочих процессов. |
> | Действие | Microsoft.Logic/workflows/providers/Microsoft.Insights/metricDefinitions/read | Чтение определений метрик рабочих процессов. |
> | Действие | Microsoft.Logic/workflows/read | Считывает рабочий процесс. |
> | Действие | Microsoft.Logic/workflows/regenerateAccessKey/action | Повторно создает секреты ключей доступа. |
> | Действие | Microsoft.Logic/workflows/run/action | Начинает выполнение рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/runs/actions/listExpressionTraces/action | Получение трассировок выражений для действия запуска рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/runs/actions/read | Считывает действие выполнения рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/runs/actions/repetitions/listExpressionTraces/action | Получение трассировок выражений повторов для действия запуска рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/runs/actions/repetitions/read | Чтение повтора действия запуска рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/runs/actions/repetitions/requestHistories/read | Чтение журнала запросов, связанных с действиями повторного запуска рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/runs/actions/requestHistories/read | Чтение журнала запросов действий, связанных с запуском рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/runs/actions/scoperepetitions/read | Чтение повтора области для действия запуска рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/runs/cancel/action | Отменяет выполнение рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/runs/delete | Удаляет запуска рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/runs/operations/read | Считывает состояние операции выполнения рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/runs/read | Считывает данные о выполнении рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/suspend/action | Приостановка рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/triggers/histories/read | Считывает журналы триггера. |
> | Действие | Microsoft.Logic/workflows/triggers/histories/resubmit/action | Повторяет отправку триггера рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/triggers/listCallbackUrl/action | Возвращает URL-адрес обратного вызова для триггера. |
> | Действие | Microsoft.Logic/workflows/triggers/read | Считывает триггер. |
> | Действие | Microsoft.Logic/workflows/triggers/reset/action | Сброс триггера. |
> | Действие | Microsoft.Logic/workflows/triggers/run/action | Выполняет триггер. |
> | Действие | Microsoft.Logic/workflows/triggers/setState/action | Настройка состояния триггера. |
> | Действие | Microsoft.Logic/workflows/validate/action | Проверяет рабочий процесс. |
> | Действие | Microsoft.Logic/workflows/versions/read | Считывает версию рабочего процесса. |
> | Действие | Microsoft.Logic/workflows/versions/triggers/listCallbackUrl/action | Возвращает URL-адрес обратного вызова для триггера. |
> | Действие | Microsoft.Logic/workflows/write | Создает или обновляет рабочий процесс. |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/move/action | Перемещает связь плана предложения машинного обучения. |
> | Действие | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/read | Считывает связь плана предложения машинного обучения. |
> | Действие | Microsoft.MachineLearning/commitmentPlans/delete | Удаляет план предложения машинного обучения. |
> | Действие | Microsoft.MachineLearning/commitmentPlans/join/action | Присоединяет план предложения машинного обучения. |
> | Действие | Microsoft.MachineLearning/commitmentPlans/read | Считывает план предложения машинного обучения. |
> | Действие | Microsoft.MachineLearning/commitmentPlans/write | Создает или обновляет план предложения машинного обучения. |
> | Действие | Microsoft.MachineLearning/locations/operationresults/read | Получение результата операции машинного обучения. |
> | Действие | Microsoft.MachineLearning/locations/operationsstatus/read | Получение состояния текущей операции машинного обучения. |
> | Действие | Microsoft.MachineLearning/operations/read | Получение операций машинного обучения. |
> | Действие | Microsoft.MachineLearning/register/action | Регистрирует подписку для поставщика ресурсов веб-службы машинного обучения и позволяет создавать веб-службы. |
> | Действие | Microsoft.MachineLearning/skus/read | Получение номеров SKU плана предложения машинного обучения. |
> | Действие | Microsoft.MachineLearning/webServices/action | Создает свойства региональной веб-службы для поддерживаемых регионов. |
> | Действие | Microsoft.MachineLearning/webServices/delete | Удаляет веб-службу машинного обучения. |
> | Действие | Microsoft.MachineLearning/webServices/listkeys/read | Получает ключи к веб-службе машинного обучения. |
> | Действие | Microsoft.MachineLearning/webServices/read | Считывает веб-службу машинного обучения. |
> | Действие | Microsoft.MachineLearning/webServices/write | Создает или обновляет веб-службу машинного обучения. |
> | Действие | Microsoft.MachineLearning/Workspaces/delete | Удаляет рабочую область машинного обучения. |
> | Действие | Microsoft.MachineLearning/Workspaces/listworkspacekeys/action | Выводит список ключей для рабочей области машинного обучения. |
> | Действие | Microsoft.MachineLearning/Workspaces/read | Считывает рабочую область машинного обучения. |
> | Действие | Microsoft.MachineLearning/Workspaces/resyncstoragekeys/action | Повторно синхронизирует ключи учетной записи хранения, настроенной для рабочей области машинного обучения. |
> | Действие | Microsoft.MachineLearning/Workspaces/write | Создает или обновляет рабочую область машинного обучения. |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.MachineLearningCompute/operationalizationClusters/checkUpdate/action | Проверка наличия обновлений для системных служб кластера операционализации. |
> | Действие | Microsoft.MachineLearningCompute/operationalizationClusters/delete | Удаление учетной записи размещения. |
> | Действие | Microsoft.MachineLearningCompute/operationalizationClusters/listKeys/action | Вывод списка ключей, связанных с кластером операционализации. |
> | Действие | Microsoft.MachineLearningCompute/operationalizationClusters/read | Чтение учетной записи размещения. |
> | Действие | Microsoft.MachineLearningCompute/operationalizationClusters/updateSystem/action | Обновление системных служб в кластере операционализации. |
> | Действие | Microsoft.MachineLearningCompute/operationalizationClusters/write | Создание или обновление учетной записи размещения. |
> | Действие | Microsoft.MachineLearningCompute/register/action | Регистрация идентификатора подписки в поставщике ресурсов и разрешение создания вычислительных ресурсов машинного обучения. |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.MachineLearningModelManagement/accounts/delete | Удаление учетной записи размещения. |
> | Действие | Microsoft.MachineLearningModelManagement/accounts/read | Чтение учетной записи размещения. |
> | Действие | Microsoft.MachineLearningModelManagement/accounts/write | Создание или обновление учетной записи размещения. |
> | Действие | Microsoft.MachineLearningModelManagement/register/action | Регистрация идентификатора подписки в поставщике ресурсов и разрешение создания учетной записи размещения. |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.MachineLearningServices/locations/computeoperationsstatus/read | Получает состояние определенной вычислительной операции. |
> | Действие | Microsoft.MachineLearningServices/locations/usages/read | Отчет об использовании для вычислительных ресурсов Машинного обучения Azure в подписке. |
> | Действие | Microsoft.MachineLearningServices/locations/vmsizes/read | Возвращает поддерживаемые размеры виртуальных машин. |
> | Действие | Microsoft.MachineLearningServices/locations/workspaceOperationsStatus/read | Получает состояние определенной операции рабочей области. |
> | Действие | Microsoft.MachineLearningServices/register/action | Регистрирует подписку для поставщика ресурсов служб машинного обучения. |
> | Действие | Microsoft.MachineLearningServices/workspaces/computes/delete | Удаляет вычислительные ресурсы в рабочих областях служб машинного обучения. |
> | Действие | Microsoft.MachineLearningServices/workspaces/computes/listKeys/action | Выводит список секретов для вычислительных ресурсов в рабочей области служб машинного обучения. |
> | Действие | Microsoft.MachineLearningServices/workspaces/computes/listNodes/action | Выводит список узлов для вычислительного ресурса в рабочей области Служб машинного обучения. |
> | Действие | Microsoft.MachineLearningServices/workspaces/computes/read | Получает вычислительные ресурсы в рабочих областях служб машинного обучения. |
> | Действие | Microsoft.MachineLearningServices/workspaces/computes/write | Создает или обновляет вычислительные ресурсы в рабочих областях служб машинного обучения. |
> | Действие | Microsoft.MachineLearningServices/workspaces/delete | Удаляет рабочие области служб машинного обучения. |
> | Действие | Microsoft.MachineLearningServices/workspaces/listKeys/action | Выводит список секретов для рабочей области служб машинного обучения. |
> | Действие | Microsoft.MachineLearningServices/workspaces/read | Получает рабочие области служб машинного обучения. |
> | Действие | Microsoft.MachineLearningServices/workspaces/write | Создает или обновляет рабочие области служб машинного обучения. |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.ManagedIdentity/register/action | Регистрирует подписку для поставщика ресурсов управляемых удостоверений. |
> | Действие | Microsoft.ManagedIdentity/userAssignedIdentities/assign/action | Действие RBAC для назначения существующего пользовательского удостоверения для ресурса. |
> | Действие | Microsoft.ManagedIdentity/userAssignedIdentities/delete | Удаление существующего пользовательского удостоверения. |
> | Действие | Microsoft.ManagedIdentity/userAssignedIdentities/read | Получение существующего пользовательского удостоверения. |
> | Действие | Microsoft.ManagedIdentity/userAssignedIdentities/write | Создание существующего пользовательского удостоверения или обновление связанных с ним тегов. |

## <a name="microsoftmanagedlab"></a>Microsoft.ManagedLab

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.ManagedLab/labAccounts/CreateLab/action | Создание лаборатории в учетной записи лаборатории. |
> | Действие | Microsoft.ManagedLab/labAccounts/delete | Удаление учетных записей лабораторий. |
> | Действие | Microsoft.ManagedLab/labAccounts/labs/delete | Удаляет лаборатории. |
> | Действие | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/delete | Удаление параметра среды. |
> | Действие | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/delete | Удаляет среды. |
> | Действие | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/read | Считывает среды. |
> | Действие | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/write | Добавляет или изменяет среды. |
> | Действие | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/read | Чтение параметра среды. |
> | Действие | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/write | Добавление или изменение параметра среды. |
> | Действие | Microsoft.ManagedLab/labAccounts/labs/labVms/delete | Удаление виртуальных машин лаборатории. |
> | Действие | Microsoft.ManagedLab/labAccounts/labs/labVms/read | Чтение виртуальных машин лаборатории. |
> | Действие | Microsoft.ManagedLab/labAccounts/labs/labVms/write | Добавление или изменение виртуальных машин лаборатории. |
> | Действие | Microsoft.ManagedLab/labAccounts/labs/read | Считывает лаборатории. |
> | Действие | Microsoft.ManagedLab/labAccounts/labs/write | Добавляет или изменяет лаборатории. |
> | Действие | Microsoft.ManagedLab/labAccounts/read | Чтение учетных записей лабораторий. |
> | Действие | Microsoft.ManagedLab/labAccounts/write | Добавление или изменение учетных записей лабораторий. |
> | Действие | Microsoft.ManagedLab/locations/operations/read | Считывает операции. |
> | Действие | Microsoft.ManagedLab/register/action | Регистрирует подписку. |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Management/checkNameAvailability/action | Проверка того, является ли имя указанной группы управления допустимым и уникальным. |
> | Действие | Microsoft.Management/getEntities/action | Вывод списка всех сущностей (групп управления, подписок и т. д.) для прошедшего проверку подлинности пользователя. |
> | Действие | Microsoft.Management/managementGroups/delete | Удаление группы управления. |
> | Действие | Microsoft.Management/managementGroups/read | Вывод списка групп управления для пользователя, прошедшего проверку подлинности. |
> | Действие | Microsoft.Management/managementGroups/subscriptions/delete | Отмена связывания подписки с группой управления. |
> | Действие | Microsoft.Management/managementGroups/subscriptions/write | Связывание существующей подписки с группой управления. |
> | Действие | Microsoft.Management/managementGroups/write | Создание или обновление группы управления. |
> | Действие | Microsoft.Management/register/action | Регистрация указанной подписки в Microsoft.Management. |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | DataAction | Microsoft.Maps/accounts/data/read | Предоставляет доступ на чтение данных учетных записей карт. |
> | Действие | Microsoft.Maps/accounts/delete | Удаление учетной записи Maps. |
> | Действие | Microsoft.Maps/accounts/eventGridFilters/delete | Удаление фильтра сетки событий |
> | Действие | Microsoft.Maps/accounts/eventGridFilters/read | Получение фильтра сетки событий |
> | Действие | Microsoft.Maps/accounts/eventGridFilters/write | Создание или изменение фильтра сетки событий |
> | Действие | Microsoft.Maps/accounts/listKeys/action | Вывод списка ключей учетной записи Maps. |
> | Действие | Microsoft.Maps/accounts/read | Получение учетной записи Maps. |
> | Действие | Microsoft.Maps/accounts/regenerateKey/action | Создание нового первичного или вторичного ключа учетной записи Maps. |
> | Действие | Microsoft.Maps/accounts/write | Создание или обновление учетной записи Maps. |
> | Действие | Microsoft.Maps/operations/read | Чтение операций поставщика |
> | Действие | Microsoft.Maps/register/action | Регистрация поставщика. |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/read | Возвращает соглашение. |
> | Действие | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/write | Принимает подписанное соглашение. |
> | Действие | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/importImage/action | Импортирует образ в запись контроля доступа конечного пользователя. |
> | Действие | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/read | Возвращает файл конфигурации. |
> | Действие | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/write | Сохраняет файл конфигурации. |
> | Действие | Microsoft.Marketplace/register/action | Регистрирует поставщик ресурсов Microsoft.Marketplace в подписке. |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.MarketplaceApps/ClassicDevServices/delete | Выполнение операции DELETE в ресурсе классической службы разработки. |
> | Действие | Microsoft.MarketplaceApps/ClassicDevServices/listSecrets/action | Получение ключей управления для ресурса классической службы разработки. |
> | Действие | Microsoft.MarketplaceApps/ClassicDevServices/listSingleSignOnToken/action | Получение URL-адреса единого входа для классической службы разработки. |
> | Действие | Microsoft.MarketplaceApps/ClassicDevServices/read | Выполнение операции GET в классической службе разработки. |
> | Действие | Microsoft.MarketplaceApps/ClassicDevServices/regenerateKey/action | Создание ключей управления для ресурса классической службы разработки. |
> | Действие | Microsoft.MarketplaceApps/Operations/read | Чтение операций для всех типов ресурсов. |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.MarketplaceOrdering/agreements/offers/plans/cancel/action | Отменяет соглашение для заданного элемента Marketplace. |
> | Действие | Microsoft.MarketplaceOrdering/agreements/offers/plans/read | Возвращает соглашение для заданного элемента Marketplace. |
> | Действие | Microsoft.MarketplaceOrdering/agreements/offers/plans/sign/action | Подписывает соглашение для заданного элемента Marketplace. |
> | Действие | Microsoft.MarketplaceOrdering/agreements/read | Возвращение всех соглашений в указанной подписке. |
> | Действие | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/read | Получение соглашения для заданного элемента виртуальной машины Marketplace. |
> | Действие | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/write | Подписывание соглашения или его отмена для заданного элемента виртуальной машины Marketplace. |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Media/checknameavailability/action | Проверка доступности имени учетной записи служб мультимедиа. |
> | Действие | Microsoft.Media/locations/checkNameAvailability/action | Проверка доступности имени учетной записи служб мультимедиа. |
> | Действие | Microsoft.Media/mediaservices/accountfilters/delete | Удаление любого фильтра учетных записей. |
> | Действие | Microsoft.Media/mediaservices/accountfilters/read | Чтение любого фильтра учетных записей. |
> | Действие | Microsoft.Media/mediaservices/accountfilters/write | Создание или обновление фильтра учетных записей. |
> | Действие | Microsoft.Media/mediaservices/assets/assetfilters/delete | Удаление любого фильтра ресурсов. |
> | Действие | Microsoft.Media/mediaservices/assets/assetfilters/read | Чтение любого фильтра ресурсов. |
> | Действие | Microsoft.Media/mediaservices/assets/assetfilters/write | Создание или обновление фильтра ресурсов. |
> | Действие | Microsoft.Media/mediaservices/assets/delete | Удаление ресурса. |
> | Действие | Microsoft.Media/mediaservices/assets/getEncryptionKey/action | Получение ключа шифрования ресурса. |
> | Действие | Microsoft.Media/mediaservices/assets/listContainerSas/action | Вывод списка подписанных URL-адресов контейнера ресурсов. |
> | Действие | Microsoft.Media/mediaservices/assets/listStreamingLocators/action | Вывод списка указателей потоковой передачи для ресурса. |
> | Действие | Microsoft.Media/mediaservices/assets/read | Чтение ресурсов. |
> | Действие | Microsoft.Media/mediaservices/assets/write | Создание или обновление ресурсов. |
> | Действие | Microsoft.Media/mediaservices/contentKeyPolicies/delete | Удаление политики ключей содержимого. |
> | Действие | Microsoft.Media/mediaservices/contentKeyPolicies/getPolicyPropertiesWithSecrets/action | Получение свойств политики с секретами. |
> | Действие | Microsoft.Media/mediaservices/contentKeyPolicies/read | Чтение политики ключей содержимого. |
> | Действие | Microsoft.Media/mediaservices/contentKeyPolicies/write | Создание или обновление политики ключей содержимого. |
> | Действие | Microsoft.Media/mediaservices/delete | Удаление учетной записи служб мультимедиа. |
> | Действие | Microsoft.Media/mediaservices/eventGridFilters/delete | Удаление фильтра сетки событий. |
> | Действие | Microsoft.Media/mediaservices/eventGridFilters/read | Чтение фильтра сетки событий. |
> | Действие | Microsoft.Media/mediaservices/eventGridFilters/write | Создание или обновление фильтра сетки событий. |
> | Действие | Microsoft.Media/mediaservices/liveEventOperations/read | Чтение операции динамического события. |
> | Действие | Microsoft.Media/mediaservices/liveEvents/delete | Удаление динамического события. |
> | Действие | Microsoft.Media/mediaservices/liveEvents/liveOutputs/delete | Удаление выходных данных динамического события. |
> | Действие | Microsoft.Media/mediaservices/liveEvents/liveOutputs/read | Чтение выходных данных динамического события. |
> | Действие | Microsoft.Media/mediaservices/liveEvents/liveOutputs/write | Создание или обновление выходных данных динамического события. |
> | Действие | Microsoft.Media/mediaservices/liveEvents/read | Чтение динамического события. |
> | Действие | Microsoft.Media/mediaservices/liveEvents/reset/action | Сброс операции динамического события. |
> | Действие | Microsoft.Media/mediaservices/liveEvents/start/action | Запуск операции динамического события. |
> | Действие | Microsoft.Media/mediaservices/liveEvents/stop/action | Остановка выполнения операции динамического события. |
> | Действие | Microsoft.Media/mediaservices/liveEvents/write | Создание или обновление динамического события. |
> | Действие | Microsoft.Media/mediaservices/liveOutputOperations/read | Чтение операции с выходными данными динамического события. |
> | Действие | Microsoft.Media/mediaservices/read | Удаление учетной записи служб мультимедиа. |
> | Действие | Microsoft.Media/mediaservices/streamingEndpointOperations/read | Чтение операции конечной точки потоковой передачи. |
> | Действие | Microsoft.Media/mediaservices/streamingEndpoints/delete | Удаление конечной точки потоковой передачи. |
> | Действие | Microsoft.Media/mediaservices/streamingEndpoints/read | Чтение конечной точки потоковой передачи. |
> | Действие | Microsoft.Media/mediaservices/streamingEndpoints/scale/action | Масштабирование операции конечной точки потоковой передачи. |
> | Действие | Microsoft.Media/mediaservices/streamingEndpoints/start/action | Запуск операции конечной точки потоковой передачи. |
> | Действие | Microsoft.Media/mediaservices/streamingEndpoints/stop/action | Остановка выполнения операции конечной точки потоковой передачи. |
> | Действие | Microsoft.Media/mediaservices/streamingEndpoints/write | Создание или обновление конечной точки потоковой передачи. |
> | Действие | Microsoft.Media/mediaservices/streamingLocators/delete | Удаление потокового указателя. |
> | Действие | Microsoft.Media/mediaservices/streamingLocators/listContentKeys/action | Вывод списка ключей содержимого. |
> | Действие | Microsoft.Media/mediaservices/streamingLocators/listPaths/action | Вывод списка путей. |
> | Действие | Microsoft.Media/mediaservices/streamingLocators/read | Считывание потокового указателя. |
> | Действие | Microsoft.Media/mediaservices/streamingLocators/write | Создание или обновление потокового указателя. |
> | Действие | Microsoft.Media/mediaservices/streamingPolicies/delete | Удаление политики потоковой передачи. |
> | Действие | Microsoft.Media/mediaservices/streamingPolicies/read | Чтение политики потоковой передачи. |
> | Действие | Microsoft.Media/mediaservices/streamingPolicies/write | Создание или обновление политики потоковой передачи. |
> | Действие | Microsoft.Media/mediaservices/syncStorageKeys/action | Синхронизация ключей хранилища для присоединенной учетной записи хранения Azure. |
> | Действие | Microsoft.Media/mediaservices/transforms/delete | Удаление преобразования. |
> | Действие | Microsoft.Media/mediaservices/transforms/jobs/cancelJob/action | Отменяет задание. |
> | Действие | Microsoft.Media/mediaservices/transforms/jobs/delete | Удаление задания. |
> | Действие | Microsoft.Media/mediaservices/transforms/jobs/read | Считывание задания. |
> | Действие | Microsoft.Media/mediaservices/transforms/jobs/write | Создание или обновление задания. |
> | Действие | Microsoft.Media/mediaservices/transforms/read | Чтение преобразования. |
> | Действие | Microsoft.Media/mediaservices/transforms/write | Создание или обновление преобразования. |
> | Действие | Microsoft.Media/mediaservices/write | Создание или обновление учетной записи служб мультимедиа. |
> | Действие | Microsoft.Media/operations/read | Возвращает доступные операции. |
> | Действие | Microsoft.Media/register/action | Регистрация подписки для поставщика ресурсов служб мультимедиа и разрешение создания учетных записей служб мультимедиа. |
> | Действие | Microsoft.Media/unregister/action | Отмена регистрации подписки для поставщика ресурсов Media Services. |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Migrate/locations/assessmentOptions/read | Получает параметры оценки, которые можно найти в указанном расположении |
> | Действие | Microsoft.Migrate/locations/checknameavailability/action | Проверка доступности имени ресурса для заданной подписки в заданном расположении |
> | Действие | Microsoft.Migrate/migrateprojects/read | Получает свойства проекта миграции. |
> | Действие | Microsoft.Migrate/migrateprojects/solutions/getconfig/action | Получает конфигурацию решения проекта миграции. |
> | Действие | Microsoft.Migrate/migrateprojects/solutions/read | Получает свойства решения проекта миграции. |
> | Действие | Microsoft.Migrate/Operations/read | Список операций, доступных в поставщике ресурсов Microsoft.Migrate |
> | Действие | Microsoft.Migrate/projects/assessments/read | Выводит список оценок в пределах проекта. |
> | Действие | Microsoft.Migrate/projects/delete | Удаляет проект |
> | Действие | Microsoft.Migrate/projects/groups/assessments/assessedmachines/read | Получение свойств компьютера, прошедшего оценку |
> | Действие | Microsoft.Migrate/projects/groups/assessments/delete | Удаляет оценку |
> | Действие | Microsoft.Migrate/projects/groups/assessments/downloadurl/action | Скачивает отчет оценки по URL-адресу. |
> | Действие | Microsoft.Migrate/projects/groups/assessments/read | Получает свойства оценки. |
> | Действие | Microsoft.Migrate/projects/groups/assessments/write | Создает новую оценку или обновляет существующую |
> | Действие | Microsoft.Migrate/projects/groups/delete | Удаляет группу |
> | Действие | Microsoft.Migrate/projects/groups/read | Получение свойств группы |
> | Действие | Microsoft.Migrate/projects/groups/write | Создает новую группу или обновляет существующую |
> | Действие | Microsoft.Migrate/projects/keys/action | Получает общие ключи проекта |
> | Действие | Microsoft.Migrate/projects/machines/read | Получает свойства компьютера |
> | Действие | Microsoft.Migrate/projects/read | Получает свойства проекта |
> | Действие | Microsoft.Migrate/projects/write | Создает новый проект или обновляет существующий |
> | Действие | Microsoft.Migrate/register/action | Регистрирует подписки в поставщике ресурсов Microsoft.Migrate |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.MixedReality/register/action | Регистрирует подписку для поставщика ресурсов смешанной реальности. |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/create/action | Создавать Пространственные привязки |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/delete | Удаление пространственного привязки |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | Поиск ближайших точек пространственных привязки |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Получение свойств пространственного привязки |
> | Действие | Microsoft.MixedReality/SpatialAnchorsAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметра диагностики для Microsoft.MixedReality/SpatialAnchorsAccounts |
> | Действие | Microsoft.MixedReality/SpatialAnchorsAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Создание или обновление параметра диагностики для Microsoft.MixedReality/SpatialAnchorsAccounts |
> | Действие | Microsoft.MixedReality/SpatialAnchorsAccounts/providers/Microsoft.Insights/metricDefinitions/read | Получение доступных метрик для Microsoft.MixedReality/SpatialAnchorsAccounts |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Найдите пространственных привязки |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Отправка данных диагностики, чтобы помочь улучшить качество обслуживания Azure пространственных привязки |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/write | Обновление свойств пространственного привязки |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.NetApp/locations/operationresults/read | Чтение ресурса результата операции. |
> | Действие | Microsoft.NetApp/locations/read | Чтение ресурса проверки доступности. |
> | Действие | Microsoft.NetApp/netAppAccounts/capacityPools/delete | Удаление пула ресурсов. |
> | Действие | Microsoft.NetApp/netAppAccounts/capacityPools/read | Чтение пула ресурсов. |
> | Действие | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/delete | Удаление ресурса тома. |
> | Действие | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/mountTargets/read | Чтение ресурса цели подключения. |
> | Действие | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/read | Чтение ресурса тома. |
> | Действие | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/delete | Удаление ресурса моментального снимка. |
> | Действие | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/read | Чтение ресурса моментального снимка. |
> | Действие | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/write | Запись ресурса моментального снимка. |
> | Действие | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/write | Запись ресурса тома. |
> | Действие | Microsoft.NetApp/netAppAccounts/capacityPools/write | Запись ресурса пула. |
> | Действие | Microsoft.NetApp/netAppAccounts/delete | Удаляет ресурс учетной записи. |
> | Действие | Microsoft.NetApp/netAppAccounts/read | Чтение ресурса учетной записи. |
> | Действие | Microsoft.NetApp/netAppAccounts/write | Запись ресурса учетной записи. |
> | Действие | Microsoft.NetApp/Operations/read | Чтение ресурсов операций. |

## <a name="microsoftnetwork"></a>Microsoft.Network.

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Network/applicationGatewayAvailableRequestHeaders/read | Получение доступных заголовков запроса шлюза приложений. |
> | Действие | Microsoft.Network/applicationGatewayAvailableResponseHeaders/read | Получение доступного заголовка ответа шлюза приложений. |
> | Действие | Microsoft.Network/applicationGatewayAvailableServerVariables/read | Получение доступных переменных сервера шлюза приложений. |
> | Действие | Microsoft.Network/applicationGatewayAvailableSslOptions/predefinedPolicies/read | Предопределенная политика SSL шлюза приложений. |
> | Действие | Microsoft.Network/applicationGatewayAvailableSslOptions/read | Доступные параметры SSL шлюза приложений. |
> | Действие | Microsoft.Network/applicationGatewayAvailableWafRuleSets/read | Возвращает доступные наборы правил WAF шлюза приложений. |
> | Действие | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Присоединяет пул адресов серверной части приложения шлюза. Не извещающие процедуры. |
> | Действие | Microsoft.Network/applicationGateways/backendhealth/action | Возвращает сведения о работоспособности серверной части шлюза приложений. |
> | Действие | Microsoft.Network/applicationGateways/delete | Удаляет шлюз приложений. |
> | Действие | Microsoft.Network/applicationGateways/read | Возвращает шлюз приложений. |
> | Действие | Microsoft.Network/applicationGateways/start/action | Запускает шлюз приложений. |
> | Действие | Microsoft.Network/applicationGateways/stop/action | Останавливает шлюз приложений. |
> | Действие | Microsoft.Network/applicationGateways/write | Создает новый шлюз приложений или обновляет существующий. |
> | Действие | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/delete | Удаляет политику WAF шлюза приложений |
> | Действие | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/read | Получает политику WAF шлюза приложений |
> | Действие | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/write | Создает политику WAF шлюза приложений или обновляет политику WAF шлюза приложений |
> | Действие | Microsoft.Network/applicationSecurityGroups/delete | Удаление группы безопасности приложений. |
> | Действие | Microsoft.Network/applicationSecurityGroups/joinIpConfiguration/action | Присоединение конфигурации IP-адреса к группам безопасности приложений. Не извещающие процедуры. |
> | Действие | Microsoft.Network/applicationSecurityGroups/joinNetworkSecurityRule/action | Присоединение правила безопасности к группам безопасности приложений. Не извещающие процедуры. |
> | Действие | Microsoft.Network/applicationSecurityGroups/listIpConfigurations/action | Вывод списка конфигураций IP-адресов в ApplicationSecurityGroup. |
> | Действие | Microsoft.Network/applicationSecurityGroups/read | Получение идентификатора группы безопасности приложений. |
> | Действие | Microsoft.Network/applicationSecurityGroups/write | Создание группы безопасности приложений или обновление существующей группы. |
> | Действие | Microsoft.Network/azureFirewallFqdnTags/read | Получает теги полного доменного имени службы "Брандмауэр Azure" |
> | Действие | Microsoft.Network/azurefirewalls/delete | Удаляет службу "Брандмауэр Azure". |
> | Действие | Microsoft.Network/azurefirewalls/read | Получает службу "Брандмауэр Azure". |
> | Действие | Microsoft.Network/azurefirewalls/write | Создает или обновляет службу "Брандмауэр Azure". |
> | Действие | Microsoft.Network/bastionHosts/delete | Удаление узла-бастиона |
> | Действие | Microsoft.Network/bastionHosts/read | Получение узла-бастиона |
> | Действие | Microsoft.Network/bastionHosts/write | Создание или обновление узла-бастиона |
> | Действие | Microsoft.Network/bgpServiceCommunities/read | Возвращает сообщества службы BGP. |
> | Действие | Microsoft.Network/checkFrontDoorNameAvailability/action | Проверка доступности для имени Front Door. |
> | Действие | Microsoft.Network/checkTrafficManagerNameAvailability/action | Проверяет доступность относительного DNS-имени диспетчера трафика. |
> | Действие | Microsoft.Network/connections/delete | Удаляет подключение шлюза виртуальной сети. |
> | Действие | Microsoft.Network/connections/read | Возвращает подключение шлюза виртуальной сети. |
> | Действие | Microsoft.Network/connections/revoke/action | Помечает подключение ExpressRoute как отмененное. |
> | Действие | Microsoft.Network/connections/sharedkey/action | Получение общего ключа для подключения шлюза виртуальной сети. |
> | Действие | Microsoft.Network/connections/sharedKey/read | Возвращает общий ключ для подключения шлюза виртуальной сети. |
> | Действие | Microsoft.Network/connections/sharedKey/write | Создает новый общий ключ для подключения шлюза виртуальной сети или обновляет существующий. |
> | Действие | Microsoft.Network/connections/vpndeviceconfigurationscript/action | Получение конфигурации VPN-устройства для подключения шлюза виртуальной сети. |
> | Действие | Microsoft.Network/connections/write | Создает новое подключение шлюза виртуальной сети или обновляет существующее. |
> | Действие | Microsoft.Network/ddosCustomPolicies/delete | Удаление политики, настроенной с учетом DDoS |
> | Действие | Microsoft.Network/ddosCustomPolicies/read | Получение определения политики, настроенной с учетом DDoS |
> | Действие | Microsoft.Network/ddosCustomPolicies/write | Создание политики, настроенной с учетом DDoS, или обновление такой существующей политики |
> | Действие | Microsoft.Network/ddosProtectionPlans/delete | Удаление плана защиты от атак DDoS. |
> | Действие | Microsoft.Network/ddosProtectionPlans/join/action | Присоединяет план защиты от атак DDoS. Не извещающие процедуры. |
> | Действие | Microsoft.Network/ddosProtectionPlans/read | Получение плана защиты от атак DDoS. |
> | Действие | Microsoft.Network/ddosProtectionPlans/write | Создание или обновление плана защиты от атак DDoS.  |
> | Действие | Microsoft.Network/dnsoperationresults/read | Возвращает результаты операции DNS. |
> | Действие | Microsoft.Network/dnsoperationstatuses/read | Возвращает состояние операции DNS.  |
> | Действие | Microsoft.Network/dnszones/A/delete | Удаляет из зоны DNS набор записей типа A с заданным именем. |
> | Действие | Microsoft.Network/dnszones/A/read | Возвращает набор записей типа A в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Действие | Microsoft.Network/dnszones/A/write | Создает или обновляет набора записей типа A в зоне DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Действие | Microsoft.Network/dnszones/AAAA/delete | Удаляет из зоны DNS набор записей типа AAAA с заданным именем. |
> | Действие | Microsoft.Network/dnszones/AAAA/read | Возвращает набор записей типа AAAA в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Действие | Microsoft.Network/dnszones/AAAA/write | Создает или обновляет набора записей типа AAAA в зоне DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Действие | Microsoft.Network/dnszones/all/read | Возвращает набор записей DNS разного типа. |
> | Действие | Microsoft.Network/dnszones/CAA/delete | Удаление из зоны DNS набора записей типа CAA с заданным именем. |
> | Действие | Microsoft.Network/dnszones/CAA/read | Получение набора записей типа CAA в формате JSON. Набор записей содержит срок жизни, теги и ETag. |
> | Действие | Microsoft.Network/dnszones/CAA/write | Создание или обновление набора записей типа CAA в зоне DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Действие | Microsoft.Network/dnszones/CNAME/delete | Удаляет из зоны DNS набор записей типа CNAME с заданным именем. |
> | Действие | Microsoft.Network/dnszones/CNAME/read | Возвращает набор записей типа CNAME в формате JSON. Набор записей содержит срок жизни, теги и ETag. |
> | Действие | Microsoft.Network/dnszones/CNAME/write | Создает или обновляет набора записей типа CNAME в зоне DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Действие | Microsoft.Network/dnszones/delete | Удаляет зону DNS в формате JSON. К свойствам зоны относятся tags, etag, numberOfRecordSets и maxNumberOfRecordSets. |
> | Действие | Microsoft.Network/dnszones/MX/delete | Удаляет из зоны DNS набор записей типа MX с заданным именем. |
> | Действие | Microsoft.Network/dnszones/MX/read | Возвращает набор записей типа MX в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Действие | Microsoft.Network/dnszones/MX/write | Создает или обновляет набора записей типа MX в зоне DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Действие | Microsoft.Network/dnszones/NS/delete | Удаляет набор записей DNS типа NS. |
> | Действие | Microsoft.Network/dnszones/NS/read | Возвращает набор записей DNS типа NS. |
> | Действие | Microsoft.Network/dnszones/NS/write | Создает или обновляет набор записей DNS типа NS. |
> | Действие | Microsoft.Network/dnszones/PTR/delete | Удаляет из зоны DNS набор записей типа PTR с заданным именем. |
> | Действие | Microsoft.Network/dnszones/PTR/read | Возвращает набор записей типа PTR в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Действие | Microsoft.Network/dnszones/PTR/write | Создает или обновляет набора записей типа PTR в зоне DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Действие | Microsoft.Network/dnszones/read | Возвращает зону DNS в формате JSON. К свойствам зоны относятся tags, etag, numberOfRecordSets и maxNumberOfRecordSets. Обратите внимание, что эта команда не извлекает наборы записей, содержащиеся в зоне. |
> | Действие | Microsoft.Network/dnszones/recordsets/read | Возвращает набор записей DNS разного типа. |
> | Действие | Microsoft.Network/dnszones/SOA/read | Возвращает набор записей DNS типа SOA. |
> | Действие | Microsoft.Network/dnszones/SOA/write | Создает или обновляет набор записей DNS типа SOA. |
> | Действие | Microsoft.Network/dnszones/SRV/delete | Удаляет из зоны DNS набор записей типа SRV с заданным именем. |
> | Действие | Microsoft.Network/dnszones/SRV/read | Возвращает набор записей типа SRV в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Действие | Microsoft.Network/dnszones/SRV/write | Создает или обновляет набор записей типа SRV. |
> | Действие | Microsoft.Network/dnszones/TXT/delete | Удаляет из зоны DNS набор записей типа TXT с заданным именем. |
> | Действие | Microsoft.Network/dnszones/TXT/read | Возвращает набор записей типа TXT в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Действие | Microsoft.Network/dnszones/TXT/write | Создает или обновляет набора записей типа TXT в зоне DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Действие | Microsoft.Network/dnszones/write | Создает или обновляет зону DNS в группе ресурсов.  Используется для обновления тегов в ресурсе зоны DNS. Обратите внимание, что эту команду невозможно использовать для создания или обновления наборов записей в зоне. |
> | Действие | Microsoft.Network/expressRouteCircuits/authorizations/delete | Удаляет данные авторизации канала ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCircuits/authorizations/read | Возвращает данные авторизации канала ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCircuits/authorizations/write | Создает новые данные авторизации канала ExpressRoute или обновляет существующие. |
> | Действие | Microsoft.Network/expressRouteCircuits/delete | Удаляет канал ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCircuits/join/action | Присоединяет канал Expressroute. Не извещающие процедуры. |
> | Действие | Microsoft.Network/expressRouteCircuits/peerings/arpTables/action | Возвращает таблицу ARP для пиринга канала ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCircuits/peerings/connections/delete | Удаление подключения канала ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCircuits/peerings/connections/read | Получение подключения канала ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCircuits/peerings/connections/write | Создание нового ресурса подключения канала ExpressRoute или обновление существующего. |
> | Действие | Microsoft.Network/expressRouteCircuits/peerings/delete | Удаляет пиринг канала ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCircuits/peerings/peerConnections/read | Получает пиринговое подключение канала ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCircuits/peerings/read | Возвращает пиринг канала ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCircuits/peerings/routeTables/action | Возвращает таблицу маршрутов для пиринга канала ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCircuits/peerings/routeTablesSummary/action | Возвращает сводку по таблице маршрутов для пиринга канала ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCircuits/peerings/stats/read | Возвращает статистику пиринга канала ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCircuits/peerings/write | Создает новый пиринг канала ExpressRoute или обновляет существующий. |
> | Действие | Microsoft.Network/expressRouteCircuits/read | Возвращает канал ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCircuits/stats/read | Возвращает статистику канала ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCircuits/write | Создает новый канал ExpressRoute или обновляет существующий. |
> | Действие | Microsoft.Network/expressRouteCrossConnections/join/action | Присоединение Express Route перекрестного подключения. Не извещающие процедуры. |
> | Действие | Microsoft.Network/expressRouteCrossConnections/peerings/arpTables/action | Получение таблицы ARP перекрестного пирингового подключения ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCrossConnections/peerings/delete | Удаление перекрестного пирингового подключения ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCrossConnections/peerings/read | Получение перекрестного пирингового подключения ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCrossConnections/peerings/routeTables/action | Получение таблицы маршрутов перекрестного пирингового подключения ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCrossConnections/peerings/routeTableSummary/action | Получение сводки таблицы маршрутов перекрестного пирингового подключения ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteCrossConnections/peerings/write | Создание перекрестного пирингового подключения ExpressRoute или обновление существующего. |
> | Действие | Microsoft.Network/expressRouteCrossConnections/read | Получение перекрестного подключения ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteGateways/expressRouteConnections/delete | Удаляет подключение Express Route. |
> | Действие | Microsoft.Network/expressRouteGateways/expressRouteConnections/read | Получает подключение Express Route. |
> | Действие | Microsoft.Network/expressRouteGateways/expressRouteConnections/write | Создает подключение Express Route или обновляет имеющееся подключение. |
> | Действие | Microsoft.Network/expressRouteGateways/join/action | Присоединяет шлюз Express Route. Не извещающие процедуры. |
> | Действие | Microsoft.Network/expressRouteGateways/read | Получает шлюз Express Route. |
> | Действие | Microsoft.Network/expressRoutePorts/delete | Удаляет ExpressRoutePorts |
> | Действие | Microsoft.Network/expressRoutePorts/join/action | Присоединяет порты Express Route. Не извещающие процедуры. |
> | Действие | Microsoft.Network/expressRoutePorts/links/read | Получает канал ExpressRoute. |
> | Действие | Microsoft.Network/expressRoutePorts/read | Получает ExpressRoutePorts |
> | Действие | Microsoft.Network/expressRoutePorts/write | Создает или обновляет ExpressRoutePorts |
> | Действие | Microsoft.Network/expressRoutePortsLocations/read | Получает порты расположений ExpressRoute. |
> | Действие | Microsoft.Network/expressRouteServiceProviders/read | Возвращает поставщики службы ExpressRoute. |
> | Действие | Microsoft.Network/frontDoors/backendPools/delete | Удаление внутреннего пула |
> | Действие | Microsoft.Network/frontDoors/backendPools/read | Получение внутреннего пула |
> | Действие | Microsoft.Network/frontDoors/backendPools/write | Создание или обновление внутреннего пула. |
> | Действие | Microsoft.Network/frontDoors/delete | Удаление Front Door |
> | Действие | Microsoft.Network/frontDoors/frontendEndpoints/delete | Удаление интерфейсной конечной точки |
> | Действие | Microsoft.Network/frontDoors/frontendEndpoints/disableHttps/action | Отключение HTTPS для интерфейсной конечной точки. |
> | Действие | Microsoft.Network/frontDoors/frontendEndpoints/enableHttps/action | Отключение протокола HTTPS для конечной точки интерфейса |
> | Действие | Microsoft.Network/frontDoors/frontendEndpoints/read | Получение интерфейсной конечной точки |
> | Действие | Microsoft.Network/frontDoors/frontendEndpoints/write | Создание или обновление интерфейсной конечной точки |
> | Действие | Microsoft.Network/frontDoors/healthProbeSettings/delete | Удаление параметров проверки работоспособности |
> | Действие | Microsoft.Network/frontDoors/healthProbeSettings/read | Получение параметров проверки работоспособности |
> | Действие | Microsoft.Network/frontDoors/healthProbeSettings/write | Создание или обновление параметров проверки работоспособности |
> | Действие | Microsoft.Network/frontDoors/loadBalancingSettings/delete | Создание или обновление параметров балансировки нагрузки |
> | Действие | Microsoft.Network/frontDoors/loadBalancingSettings/read | Получение параметров балансировки нагрузки |
> | Действие | Microsoft.Network/frontDoors/loadBalancingSettings/write | Создание или обновление параметров балансировки нагрузки |
> | Действие | Microsoft.Network/frontDoors/purge/action | Очистка кэшированного содержимого из Front Door |
> | Действие | Microsoft.Network/frontDoors/read | Получение Front Door |
> | Действие | Microsoft.Network/frontDoors/routingRules/delete | Создание правила маршрутизации |
> | Действие | Microsoft.Network/frontDoors/routingRules/read | Получение правила маршрутизации |
> | Действие | Microsoft.Network/frontDoors/routingRules/write | Создание или обновление правила маршрутизации |
> | Действие | Microsoft.Network/frontDoors/validateCustomDomain/action | Проверка конечной точки интерфейса для Front Door |
> | Действие | Microsoft.Network/frontDoors/write | Создание или обновление Front Door |
> | Действие | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/delete | Удаление политики брандмауэра веб-приложения |
> | Действие | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/read | Получение политики брандмауэра веб-приложения |
> | Действие | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/write | Создание или обновление политики брандмауэра веб-приложения |
> | Действие | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Присоединяет пул адресов серверной части подсистемы балансировки нагрузки. Не извещающие процедуры. |
> | Действие | Microsoft.Network/loadBalancers/backendAddressPools/read | Возвращает определение внутреннего пула адресов подсистемы балансировки нагрузки. |
> | Действие | Microsoft.Network/loadBalancers/delete | Удаляет подсистему балансировки нагрузки. |
> | Действие | Microsoft.Network/loadBalancers/frontendIPConfigurations/join/action | Объединяет внешнюю IP-конфигурацию Load Balancer. Не извещающие процедуры. |
> | Действие | Microsoft.Network/loadBalancers/frontendIPConfigurations/read | Возвращает определение внешней IP-конфигурации подсистемы балансировки нагрузки. |
> | Действие | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Присоединяет подсистемы балансировки нагрузки пул NAT входящего трафика. Не извещающие процедуры. |
> | Действие | Microsoft.Network/loadBalancers/inboundNatPools/read | Возвращает определение пула входящих подключений NAT подсистемы балансировки нагрузки. |
> | Действие | Microsoft.Network/loadBalancers/inboundNatRules/delete | Удаляет правило NAT для входящего трафика подсистемы балансировки нагрузки. |
> | Действие | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Присоединяет правило nat для входящего трафика балансировщика нагрузки. Не извещающие процедуры. |
> | Действие | Microsoft.Network/loadBalancers/inboundNatRules/read | Возвращает определение правила NAT для входящего трафика подсистемы балансировки нагрузки. |
> | Действие | Microsoft.Network/loadBalancers/inboundNatRules/write | Создает новое правило NAT для входящего трафика подсистемы балансировки нагрузки или обновляет существующее. |
> | Действие | Microsoft.Network/loadBalancers/loadBalancingRules/read | Возвращает определение правила подсистемы балансировки нагрузки. |
> | Действие | Microsoft.Network/loadBalancers/networkInterfaces/read | Возвращает ссылки на все сетевые интерфейсы в подсистеме балансировки нагрузки. |
> | Действие | Microsoft.Network/loadBalancers/outboundRules/read | Получает определения исходящего правила подсистемы балансировки нагрузки |
> | Действие | Microsoft.Network/loadBalancers/probes/join/action | Позволяет использовать зонды подсистемы балансировки нагрузки. Например, при наличии этого разрешения свойство healthProbe в масштабируемом наборе виртуальных машин может ссылаться на конкретный зонд. Не извещающие процедуры. |
> | Действие | Microsoft.Network/loadBalancers/probes/read | Возвращает пробу подсистемы балансировки нагрузки. |
> | Действие | Microsoft.Network/loadBalancers/read | Возвращает определение подсистемы балансировки нагрузки. |
> | Действие | Microsoft.Network/loadBalancers/virtualMachines/read | Возвращает ссылки на все виртуальные машины в подсистеме балансировки нагрузки. |
> | Действие | Microsoft.Network/loadBalancers/write | Создает новую подсистему балансировки нагрузки или обновляет существующую. |
> | Действие | Microsoft.Network/localnetworkgateways/delete | Удаляет шлюз локальной сети. |
> | Действие | Microsoft.Network/localnetworkgateways/read | Возвращает шлюз локальной сети. |
> | Действие | Microsoft.Network/localnetworkgateways/write | Создает новый шлюз локальной сети или обновляет существующий. |
> | Действие | Microsoft.Network/locations/availableDelegations/read | Получает доступные делегирования. |
> | Действие | Microsoft.Network/locations/availablePrivateEndpointResources/read | Получает доступную конечную точку Private ресурсы |
> | Действие | Microsoft.Network/locations/bareMetalTenants/action | Выделяет или проверяет клиент исходного состояния. |
> | Действие | Microsoft.Network/locations/checkAcceleratedNetworkingSupport/action | Проверяет поддержку ускоренной сети. |
> | Действие | Microsoft.Network/locations/checkDnsNameAvailability/read | Проверяет, доступна ли метка DNS в указанном расположении. |
> | Действие | Microsoft.Network/locations/operationResults/read | Возвращает результат асинхронной операции POST или DELETE. |
> | Действие | Microsoft.Network/locations/operations/read | Возвращает ресурс операции, представляющий состояние асинхронной операции. |
> | Действие | Microsoft.Network/locations/serviceTags/read | Получение тегов службы |
> | Действие | Microsoft.Network/locations/supportedVirtualMachineSizes/read | Возвращает поддерживаемые размеры виртуальных машин. |
> | Действие | Microsoft.Network/locations/usages/read | Возвращает метрики использования ресурсов. |
> | Действие | Microsoft.Network/locations/virtualNetworkAvailableEndpointServices/read | Получение списка доступных служб конечных точек виртуальной сети. |
> | Действие | Microsoft.Network/networkIntentPolicies/delete | Удаляет политику намерений сети |
> | Действие | Microsoft.Network/networkIntentPolicies/read | Получает описание политики намерений сети |
> | Действие | Microsoft.Network/networkIntentPolicies/write | Создает политику намерения сети или обновляет существующую |
> | Действие | Microsoft.Network/networkInterfaces/delete | Удаляет сетевой интерфейс. |
> | Действие | Microsoft.Network/networkInterfaces/effectiveNetworkSecurityGroups/action | Возвращает группы безопасности сети, настроенные для сетевого интерфейса виртуальной машины. |
> | Действие | Microsoft.Network/networkInterfaces/effectiveRouteTable/action | Возвращает таблицу маршрутов, настроенную для сетевого интерфейса виртуальной машины. |
> | Действие | Microsoft.Network/networkInterfaces/ipconfigurations/join/action | Объединяет IP-конфигурацию сетевого интерфейса. Не извещающие процедуры. |
> | Действие | Microsoft.Network/networkInterfaces/ipconfigurations/read | Возвращает определение IP-конфигурации сетевого интерфейса.  |
> | Действие | Microsoft.Network/networkInterfaces/join/action | Подключает виртуальную машину к сетевому интерфейсу. Не извещающие процедуры. |
> | Действие | Microsoft.Network/networkInterfaces/loadBalancers/read | Возвращает все подсистемы балансировки нагрузки, в которых используется сетевой интерфейс. |
> | Действие | Microsoft.Network/networkInterfaces/read | Возвращает определение сетевого интерфейса.  |
> | Действие | Microsoft.Network/networkInterfaces/tapConfigurations/delete | Удаляет конфигурацию TAP сетевого интерфейса. |
> | Действие | Microsoft.Network/networkInterfaces/tapConfigurations/read | Получает конфигурацию TAP сетевого интерфейса. |
> | Действие | Microsoft.Network/networkInterfaces/tapConfigurations/write | Создает конфигурацию TAP сетевого интерфейса или обновляет имеющуюся конфигурацию TAP сетевого интерфейса. |
> | Действие | Microsoft.Network/networkInterfaces/write | Создает новый сетевой интерфейс или обновляет существующий.  |
> | Действие | Microsoft.Network/networkProfiles/delete | Удаляет сетевой профиль |
> | Действие | Microsoft.Network/networkProfiles/read | Получает сетевой профиль |
> | Действие | Microsoft.Network/networkProfiles/removeContainers/action | Удаляет контейнеры |
> | Действие | Microsoft.Network/networkProfiles/setContainers/action | Задает контейнеры |
> | Действие | Microsoft.Network/networkProfiles/setNetworkInterfaces/action | Задает интерфейсы сетевых контейнеров |
> | Действие | Microsoft.Network/networkProfiles/write | Создает или обновляет сетевой профиль |
> | Действие | Microsoft.Network/networkSecurityGroups/defaultSecurityRules/read | Возвращает определение правила безопасности по умолчанию. |
> | Действие | Microsoft.Network/networkSecurityGroups/delete | Удаляет группу безопасности сети. |
> | Действие | Microsoft.Network/networkSecurityGroups/join/action | Присоединяет группу безопасности сети. Не извещающие процедуры. |
> | Действие | Microsoft.Network/networkSecurityGroups/read | Возвращает определение группы безопасности сети. |
> | Действие | Microsoft.Network/networkSecurityGroups/securityRules/delete | Удаляет правило безопасности. |
> | Действие | Microsoft.Network/networkSecurityGroups/securityRules/read | Возвращает определение правила безопасности. |
> | Действие | Microsoft.Network/networkSecurityGroups/securityRules/write | Создает новое правило безопасности или обновляет существующее. |
> | Действие | Microsoft.Network/networkSecurityGroups/write | Создает новую группу безопасности сети или обновляет существующую. |
> | Действие | Microsoft.Network/networkWatchers/availableProvidersList/action | Получение всех доступных поставщиков услуг Интернета в заданном регионе Azure. |
> | Действие | Microsoft.Network/networkWatchers/azureReachabilityReport/action | Получение оценки относительной задержки для поставщиков услуг Интернета из указанного расположения в регионы Azure. |
> | Действие | Microsoft.Network/networkWatchers/configureFlowLog/action | Настраивает ведение журнала потоков для целевого ресурса. |
> | Действие | Microsoft.Network/networkWatchers/connectionMonitors/delete | Удаление монитора подключения. |
> | Действие | Microsoft.Network/networkWatchers/connectionMonitors/query/action | Запрос мониторинга подключения между указанными конечными точками. |
> | Действие | Microsoft.Network/networkWatchers/connectionMonitors/read | Получение сведений о мониторе подключения. |
> | Действие | Microsoft.Network/networkWatchers/connectionMonitors/start/action | Запуск мониторинга подключения между указанными конечными точками. |
> | Действие | Microsoft.Network/networkWatchers/connectionMonitors/stop/action | Остановка или приостановка мониторинга подключения между указанными конечными точками. |
> | Действие | Microsoft.Network/networkWatchers/connectionMonitors/write | Создание монитора подключения. |
> | Действие | Microsoft.Network/networkWatchers/connectivityCheck/action | Проверка возможности прямого подключения TCP между виртуальной машиной и определенной конечной точкой, при этом используется другая виртуальная машина или произвольный удаленный сервер. |
> | Действие | Microsoft.Network/networkWatchers/delete | Удаляет Наблюдатель за сетями. |
> | Действие | Microsoft.Network/networkWatchers/ipFlowVerify/action | Проверяет, разрешена ли передача или прием пакета для определенного места назначения. |
> | Действие | Microsoft.Network/networkWatchers/lenses/delete | Удаление группы связанных элементов. |
> | Действие | Microsoft.Network/networkWatchers/lenses/query/action | Запрос мониторинга трафика в указанной конечной точке. |
> | Действие | Microsoft.Network/networkWatchers/lenses/read | Получение сведений о группе связанных элементов. |
> | Действие | Microsoft.Network/networkWatchers/lenses/start/action | Запуск мониторинга трафика в указанной конечной точке. |
> | Действие | Microsoft.Network/networkWatchers/lenses/stop/action | Остановка или приостановка мониторинга трафика в указанной конечной точке. |
> | Действие | Microsoft.Network/networkWatchers/lenses/write | Создание группы связанных элементов. |
> | Действие | Microsoft.Network/networkWatchers/networkConfigurationDiagnostic/action | Диагностика конфигурации сети. |
> | Действие | Microsoft.Network/networkWatchers/nextHop/action | Для указанного целевого объекта и конечного IP-адреса возвращает тип следующего прыжка и его IP-адрес. |
> | Действие | Microsoft.Network/networkWatchers/packetCaptures/delete | Удаляет запись пакетов. |
> | Действие | Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Возвращает сведения о свойствах и состоянии ресурса записи пакетов. |
> | Действие | Microsoft.Network/networkWatchers/packetCaptures/read | Возвращает определение записи пакетов. |
> | Действие | Microsoft.Network/networkWatchers/packetCaptures/stop/action | Останавливает выполняемый сеанс записи пакетов. |
> | Действие | Microsoft.Network/networkWatchers/packetCaptures/write | Создает запись пакетов. |
> | Действие | Microsoft.Network/networkWatchers/pingMeshes/delete | Удаляет PingMesh. |
> | Действие | Microsoft.Network/networkWatchers/pingMeshes/read | Получает сведения о PingMesh. |
> | Действие | Microsoft.Network/networkWatchers/pingMeshes/start/action | Запускает PingMesh между указанными виртуальными машинами. |
> | Действие | Microsoft.Network/networkWatchers/pingMeshes/stop/action | Останавливает PingMesh между указанными виртуальными машинами. |
> | Действие | Microsoft.Network/networkWatchers/pingMeshes/write | Создает PingMesh. |
> | Действие | Microsoft.Network/networkWatchers/queryConnectionMonitors/action | Пакетный запрос мониторинга подключения между указанными конечными точками |
> | Действие | Microsoft.Network/networkWatchers/queryFlowLogStatus/action | Возвращает состояние ведения журнала потоков для ресурса. |
> | Действие | Microsoft.Network/networkWatchers/queryTroubleshootResult/action | Возвращает результат устранения неполадок предыдущей или текущей операции по устранению неполадок. |
> | Действие | Microsoft.Network/networkWatchers/read | Возвращает определение Наблюдателя за сетями. |
> | Действие | Microsoft.Network/networkWatchers/securityGroupView/action | Отображает настроенные и действующие правила группы безопасности сети для виртуальной машины. |
> | Действие | Microsoft.Network/networkWatchers/topology/action | Возвращает представление уровня сети для ресурсов и их связей в группе ресурсов. |
> | Действие | Microsoft.Network/networkWatchers/troubleshoot/action | Начинает устранение неполадок сетевого ресурса в Azure. |
> | Действие | Microsoft.Network/networkWatchers/write | Создает новый Наблюдатель за сетями или обновляет существующий. |
> | Действие | Microsoft.Network/operations/read | Возвращает доступные операции. |
> | Действие | Microsoft.Network/p2sVpnGateways/delete | Удаляет P2SVpnGateway. |
> | Действие | Microsoft.Network/p2sVpnGateways/generatevpnprofile/action | Создание профиля Vpn для P2SVpnGateway |
> | Действие | Microsoft.Network/p2sVpnGateways/getp2svpnconnectionhealth/action | Получает данные о работоспособности VPN-подключения типа "точка — сеть" для P2SVpnGateway. |
> | Действие | Microsoft.Network/p2sVpnGateways/read | Возвращает P2SVpnGateway. |
> | Действие | Microsoft.Network/p2sVpnGateways/write | Получает P2SVpnGateway. |
> | Действие | Microsoft.Network/privateEndpoints/delete | Удаляет ресурс частной конечной точки. |
> | Действие | Microsoft.Network/privateEndpoints/read | Получает ресурс частной конечной точки. |
> | Действие | Microsoft.Network/privateEndpoints/write | Создает новый частную конечную точку или обновляет существующий частной конечной точки. |
> | Действие | Microsoft.Network/privateLinkServices/delete | Удаляет ресурс службы частной связи. |
> | Действие | Microsoft.Network/privateLinkServices/privateEndpointConnections/delete | Удаление подключения к частной конечной точки. |
> | Действие | Microsoft.Network/privateLinkServices/privateEndpointConnections/read | Возвращает определение подключения частной конечной точки. |
> | Действие | Microsoft.Network/privateLinkServices/privateEndpointConnections/write | Создает новое подключение частной конечной точки или обновляет существующее подключение частной конечной точки. |
> | Действие | Microsoft.Network/privateLinkServices/read | Получает ресурс службы частной связи. |
> | Действие | Microsoft.Network/privateLinkServices/write | Создает новую службу частной связи или обновляет существующую. |
> | Действие | Microsoft.Network/publicIPAddresses/delete | Удаляет общедоступный IP-адрес. |
> | Действие | Microsoft.Network/publicIPAddresses/join/action | Присоединяет общедоступный IP-адрес. Не извещающие процедуры. |
> | Действие | Microsoft.Network/publicIPAddresses/read | Возвращает определение общедоступного IP-адреса. |
> | Действие | Microsoft.Network/publicIPAddresses/write | Создает новый общедоступный IP-адрес или обновляет существующий.  |
> | Действие | Microsoft.Network/publicIPPrefixes/delete | Удаляет префикс общедоступного IP-адреса. |
> | Действие | Microsoft.Network/publicIPPrefixes/join/action | Присоединяет PublicIPPrefix. Не извещающие процедуры. |
> | Действие | Microsoft.Network/publicIPPrefixes/read | Возвращает определение префикса общедоступного IP-адреса. |
> | Действие | Microsoft.Network/publicIPPrefixes/write | Создает новый префикс общедоступного IP-адреса или обновляет существующий. |
> | Действие | Microsoft.Network/register/action | Регистрирует подписку. |
> | Действие | Microsoft.Network/routeFilters/delete | Удаляет определение фильтра маршрутов. |
> | Действие | Microsoft.Network/routeFilters/join/action | Присоединяет фильтр маршрутов. Не извещающие процедуры. |
> | Действие | Microsoft.Network/routeFilters/read | Возвращает определение фильтра маршрутов. |
> | Действие | Microsoft.Network/routeFilters/routeFilterRules/delete | Удаляет определение правила фильтра маршрутов. |
> | Действие | Microsoft.Network/routeFilters/routeFilterRules/read | Возвращает определение правила фильтра маршрутов. |
> | Действие | Microsoft.Network/routeFilters/routeFilterRules/write | Создает новое правило фильтра маршрутов или обновляет существующее. |
> | Действие | Microsoft.Network/routeFilters/write | Создает новый фильтр маршрутов или обновляет существующий. |
> | Действие | Microsoft.Network/routeTables/delete | Удаляет определение таблицы маршрутов. |
> | Действие | Microsoft.Network/routeTables/join/action | Присоединяет таблицу маршрутов. Не извещающие процедуры. |
> | Действие | Microsoft.Network/routeTables/read | Возвращает определение таблицы маршрутов. |
> | Действие | Microsoft.Network/routeTables/routes/delete | Удаляет определение маршрута. |
> | Действие | Microsoft.Network/routeTables/routes/read | Возвращает определение маршрута. |
> | Действие | Microsoft.Network/routeTables/routes/write | Создает новый маршрут или обновляет существующий. |
> | Действие | Microsoft.Network/routeTables/write | Создает новую таблицу маршрутов или обновляет существующую. |
> | Действие | Microsoft.Network/securegateways/delete | Удаление защищенного шлюза. |
> | Действие | Microsoft.Network/securegateways/read | Получение защищенного шлюза. |
> | Действие | Microsoft.Network/securegateways/write | Создание или обновление защищенного шлюза. |
> | Действие | Microsoft.Network/serviceEndpointPolicies/delete | Удаление политики конечной точки службы. |
> | Действие | Microsoft.Network/serviceEndpointPolicies/join/action | Присоединение политики конечной точки службы. Не извещающие процедуры. |
> | Действие | Microsoft.Network/serviceEndpointPolicies/joinSubnet/action | Присоединение подсети к политикам конечной точки службы. Не извещающие процедуры. |
> | Действие | Microsoft.Network/serviceEndpointPolicies/read | Получение описания политики конечной точки службы. |
> | Действие | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/delete | Удаление определения политики конечной точки службы. |
> | Действие | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/read | Получение описания определения политики конечной точки службы. |
> | Действие | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/write | Создание определения политики конечной точки службы или обновление существующего. |
> | Действие | Microsoft.Network/serviceEndpointPolicies/write | Создание политики конечной точки службы или обновление существующей. |
> | Действие | Microsoft.Network/trafficManagerGeographicHierarchies/read | Возвращает географическую иерархию диспетчера трафика, содержащую регионы, которые могут быть использованы для метода географической маршрутизации трафика. |
> | Действие | Microsoft.Network/trafficManagerProfiles/azureEndpoints/delete | Удаление конечной точки Azure из существующего профиля диспетчера трафика. Диспетчер трафика остановит маршрутизацию трафика в удаленную конечную точку Azure. |
> | Действие | Microsoft.Network/trafficManagerProfiles/azureEndpoints/read | Получение конечной точки Azure, которая относится к профилю диспетчера трафика, а также свойств этой конечной точки. |
> | Действие | Microsoft.Network/trafficManagerProfiles/azureEndpoints/write | Добавление новой конечной точки Azure в существующий профиль диспетчера трафика или обновление свойств существующей конечной точки Azure в этом профиле. |
> | Действие | Microsoft.Network/trafficManagerProfiles/delete | Удаляет профиль диспетчера трафика. Будут утрачены все параметры, связанные с профилем диспетчера трафика, и профиль больше не сможет использоваться для маршрутизации трафика. |
> | Действие | Microsoft.Network/trafficManagerProfiles/externalEndpoints/delete | Удаление внешней конечной точки из существующего профиля диспетчера трафика. Диспетчер трафика остановит маршрутизацию трафика в удаленную внешнюю конечную точку. |
> | Действие | Microsoft.Network/trafficManagerProfiles/externalEndpoints/read | Получение внешней конечной точки, которая относится к профилю диспетчера трафика, а также свойств этой конечной точки. |
> | Действие | Microsoft.Network/trafficManagerProfiles/externalEndpoints/write | Добавление новой внешней конечной точки в существующий профиль диспетчера трафика или обновление свойств существующей внешней конечной точки в этом профиле. |
> | Действие | Microsoft.Network/trafficManagerProfiles/heatMaps/read | Получение тепловой карты диспетчера трафика для данного профиля диспетчера трафика, которая содержит количество запросов и данные задержек по исходному IP-адресу и расположению. |
> | Действие | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/delete | Удаление вложенной конечной точки из существующего профиля диспетчера трафика. Диспетчер трафика остановит маршрутизацию трафика в удаленную вложенную конечную точку. |
> | Действие | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/read | Получение вложенной конечной точки, которая относится к профилю диспетчера трафика, а также свойств этой конечной точки. |
> | Действие | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/write | Добавление новой вложенной конечной точки в существующий профиль диспетчера трафика или обновление свойств существующей вложенной конечной точки в этом профиле. |
> | Действие | Microsoft.Network/trafficManagerProfiles/read | Возвращает конфигурацию профиля диспетчера трафика. Он содержит параметры DNS, параметры маршрутизации трафика, параметры отслеживания конечных точек, а также список конечных точек, маршрутизируемых данным профилем диспетчера трафика. |
> | Действие | Microsoft.Network/trafficManagerProfiles/write | Создает профиль диспетчера трафика или изменяет конфигурацию существующего профиля диспетчера трафика.<br>Сюда входит включение или отключение профиля и изменение параметров DNS, параметров маршрутизации трафика или параметров мониторинга конечных точек.<br>Конечные точки, маршрутизируемые с помощью профиля диспетчера трафика, можно добавлять, удалять, включать и отключать. |
> | Действие | Microsoft.Network/trafficManagerUserMetricsKeys/delete | Удаление ключа уровня подписки, используемого для сбора метрик пользователя в реальном времени. |
> | Действие | Microsoft.Network/trafficManagerUserMetricsKeys/read | Получение ключа уровня подписки, используемого для сбора метрик пользователя в реальном времени. |
> | Действие | Microsoft.Network/trafficManagerUserMetricsKeys/write | Создание нового ключа уровня подписки, используемого для сбора метрик пользователя в реальном времени. |
> | Действие | Microsoft.Network/unregister/action | Отменяет регистрацию подписки. |
> | Действие | Microsoft.Network/virtualHubs/delete | Удаление виртуального концентратора. |
> | Действие | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/delete | Удаление подключения к виртуальной сети концентратора. |
> | Действие | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/read | Получение подключения к виртуальной сети концентратора. |
> | Действие | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/write | Создание или обновление подключения к виртуальной сети концентратора. |
> | Действие | Microsoft.Network/virtualHubs/read | Получение виртуального концентратора. |
> | Действие | Microsoft.Network/virtualHubs/write | Создание или обновление виртуального концентратора. |
> | Действие | microsoft.network/virtualnetworkgateways/connections/read | Получение подключения к шлюзу виртуальной сети. |
> | Действие | Microsoft.Network/virtualNetworkGateways/delete | Удаление шлюза виртуальной сети. |
> | Действие | microsoft.network/virtualnetworkgateways/generatevpnclientpackage/action | Создание пакета VPN-клиента для шлюза виртуальной сети. |
> | Действие | microsoft.network/virtualnetworkgateways/generatevpnprofile/action | Создание пакета VPN-профиля для шлюза виртуальной сети. |
> | Действие | microsoft.network/virtualnetworkgateways/getadvertisedroutes/action | Получение объявленных маршрутов шлюза виртуальной сети. |
> | Действие | microsoft.network/virtualnetworkgateways/getbgppeerstatus/action | Получение состояния кэширующего узла BGP шлюза виртуальной сети. |
> | Действие | microsoft.network/virtualnetworkgateways/getlearnedroutes/action | Получение обнаруженных маршрутов шлюза виртуальной сети. |
> | Действие | microsoft.network/virtualnetworkgateways/getvpnclientconnectionhealth/action | Получение работоспособности подключения VPN-клиента для шлюза виртуальной сети. |
> | Действие | microsoft.network/virtualnetworkgateways/getvpnclientipsecparameters/action | Получение параметров IPsec VPN-клиента для клиента P2S шлюза виртуальной сети. |
> | Действие | microsoft.network/virtualnetworkgateways/getvpnprofilepackageurl/action | Получение URL-адреса предварительно созданного пакета профиля VPN-клиента. |
> | Действие | Microsoft.Network/virtualNetworkGateways/read | Получение шлюза виртуальной сети. |
> | Действие | microsoft.network/virtualnetworkgateways/reset/action | Сброс шлюза виртуальной сети. |
> | Действие | microsoft.network/virtualnetworkgateways/resetvpnclientsharedkey/action | Сброс общего ключа Vpnclient клиента P2S VirtualNetworkGateway. |
> | Действие | microsoft.network/virtualnetworkgateways/setvpnclientipsecparameters/action | Настройка параметров IPsec VPN-клиента для клиента P2S шлюза виртуальной сети. |
> | Действие | Microsoft.Network/virtualnetworkgateways/supportedvpndevices/action | Получение списка поддерживаемых VPN-устройств. |
> | Действие | Microsoft.Network/virtualNetworkGateways/write | Создание или обновление шлюза виртуальной сети. |
> | Действие | Microsoft.Network/virtualNetworks/BastionHosts/action | Получение ссылок узла-бастиона в виртуальной сети. |
> | Действие | Microsoft.Network/virtualNetworks/bastionHosts/default/action | Получение ссылок узла-бастиона в виртуальной сети. |
> | Действие | Microsoft.Network/virtualNetworks/checkIpAddressAvailability/read | Проверяет, доступен ли IP-адрес в указанной виртуальной сети. |
> | Действие | Microsoft.Network/virtualNetworks/delete | Удаляет виртуальную сеть. |
> | Действие | Microsoft.Network/virtualNetworks/peer/action | Создает пиринг между виртуальными сетями. |
> | Действие | Microsoft.Network/virtualNetworks/read | Возвращает определение виртуальной сети. |
> | Действие | Microsoft.Network/virtualNetworks/subnets/delete | Удаляет подсеть виртуальной сети. |
> | Действие | Microsoft.Network/virtualNetworks/subnets/join/action | Присоединяет виртуальную сеть. Не извещающие процедуры. |
> | Действие | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Присоединение к подсети ресурса, например учетной записи хранения или базы данных SQL. Не извещающие процедуры. |
> | Действие | Microsoft.Network/virtualNetworks/subnets/prepareNetworkPolicies/action | Подготавливает подсеть, применяя необходимые политики сети. |
> | Действие | Microsoft.Network/virtualNetworks/subnets/read | Возвращает определение подсети виртуальной сети. |
> | Действие | Microsoft.Network/virtualNetworks/subnets/virtualMachines/read | Возвращает ссылки на все виртуальные машины в подсети виртуальной сети. |
> | Действие | Microsoft.Network/virtualNetworks/subnets/write | Создает новую подсеть пиринг виртуальной сети или обновляет существующую. |
> | Действие | Microsoft.Network/virtualNetworks/usages/read | Получение сведения об использовании IP-адресов для каждой подсети виртуальной сети. |
> | Действие | Microsoft.Network/virtualNetworks/virtualMachines/read | Возвращает ссылки на все виртуальные машины в виртуальной сети. |
> | Действие | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/delete | Удаляет пиринг виртуальной сети. |
> | Действие | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/read | Возвращает определение пиринга виртуальной сети. |
> | Действие | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write | Создает новый пиринг виртуальной сети или обновляет существующий. |
> | Действие | Microsoft.Network/virtualNetworks/write | Создает новую виртуальную сеть или обновляет существующую. |
> | Действие | Microsoft.Network/virtualNetworkTaps/delete | Удаление устройства перехватчика трафика (TAP) виртуальной сети. |
> | Действие | Microsoft.Network/virtualNetworkTaps/join/action | Присоединение tap виртуальной сети. Не извещающие процедуры. |
> | Действие | Microsoft.Network/virtualNetworkTaps/read | Получение TAP виртуальной сети. |
> | Действие | Microsoft.Network/virtualNetworkTaps/write | Создание или обновление TAP виртуальной сети. |
> | Действие | Microsoft.Network/virtualWans/delete | Удаление виртуальной глобальной сети. |
> | Действие | Microsoft.network/virtualWans/p2sVpnServerConfigurations/delete | Удаляет конфигурацию P2SVpnServerConfiguration виртуальной глобальной сети |
> | Действие | Microsoft.Network/virtualWans/p2sVpnServerConfigurations/read | Получает конфигурацию P2SVpnServerConfiguration виртуальной глобальной сети. |
> | Действие | Microsoft.network/virtualWans/p2sVpnServerConfigurations/write | Создает конфигурацию P2SVpnServerConfiguration виртуальной глобальной сети или обновляет существующую |
> | Действие | Microsoft.Network/virtualWans/read | Получение виртуальной глобальной сети. |
> | Действие | Microsoft.Network/virtualwans/supportedSecurityProviders/read | Получает поддерживаемых поставщиков безопасности виртуальной глобальной сети. |
> | Действие | Microsoft.Network/virtualWans/virtualHubs/read | Получение всех виртуальных концентраторов, ссылающихся на виртуальную глобальную сеть. |
> | Действие | Microsoft.Network/virtualwans/vpnconfiguration/action | Получение конфигурации VPN. |
> | Действие | Microsoft.Network/virtualWans/vpnSites/read | Получение всех сетей VPN, ссылающихся на виртуальную глобальную сеть. |
> | Действие | Microsoft.Network/virtualWans/write | Создание или обновление виртуальной глобальной сети. |
> | Действие | Microsoft.Network/vpnGateways/delete | Удаляет VPN-шлюз. |
> | Действие | microsoft.network/vpngateways/listvpnconnectionshealth/action | Получает состояние работоспособности подключения для всех или подмножества подключений на шлюзе VpnGateway |
> | Действие | Microsoft.Network/vpnGateways/read | Получение шлюза VPN. |
> | Действие | microsoft.network/vpngateways/reset/action | Выполняет сброс VpnGateway |
> | Действие | microsoft.network/vpnGateways/vpnConnections/delete | Удаляет VpnConnection. |
> | Действие | microsoft.network/vpnGateways/vpnConnections/read | Получение VPN-подключения. |
> | Действие | microsoft.network/vpnGateways/vpnConnections/write | Установка VPN-подключения. |
> | Действие | Microsoft.Network/vpnGateways/write | Установка VPN-шлюза. |
> | Действие | Microsoft.Network/vpnsites/delete | Удаление ресурса сети VPN. |
> | Действие | Microsoft.Network/vpnsites/read | Получение ресурса сети VPN. |
> | Действие | Microsoft.Network/vpnsites/write | Создание или обновление ресурса сети VPN. |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.NotificationHubs/CheckNamespaceAvailability/action | Проверяет, доступно ли имя заданного ресурса пространства имен в службе центра уведомлений. |
> | Действие | Microsoft.NotificationHubs/Namespaces/authorizationRules/action | Возвращает список описаний правил авторизации пространства имен. |
> | Действие | Microsoft.NotificationHubs/Namespaces/authorizationRules/delete | Удаляет правило авторизации для пространства имен. Невозможно удалить правило авторизации пространства имен по умолчанию.  |
> | Действие | Microsoft.NotificationHubs/Namespaces/authorizationRules/listkeys/action | Возвращает строку подключения к пространству имен. |
> | Действие | Microsoft.NotificationHubs/Namespaces/authorizationRules/read | Возвращает список описаний правил авторизации пространства имен. |
> | Действие | Microsoft.NotificationHubs/Namespaces/authorizationRules/regenerateKeys/action | Правило авторизации пространства имен повторно создает первичный или вторичный ключ. Укажите ключ, который должен быть создан повторно. |
> | Действие | Microsoft.NotificationHubs/Namespaces/authorizationRules/write | Создает правила авторизации уровня пространства имен и обновляет его свойства. Можно обновить права доступа, первичный ключ и вторичный ключ для правил авторизации. |
> | Действие | Microsoft.NotificationHubs/Namespaces/CheckNotificationHubAvailability/action | Проверяет, доступно ли заданное имя центра уведомлений в пространстве имен. |
> | Действие | Microsoft.NotificationHubs/Namespaces/Delete | Удаляет ресурс пространства имен. |
> | Действие | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/action | Возвращает список правил авторизации центра уведомлений. |
> | Действие | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/delete | Удаляет правила авторизации центра уведомлений. |
> | Действие | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/listkeys/action | Возвращает строку подключения к центру уведомлений. |
> | Действие | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/read | Возвращает список правил авторизации центра уведомлений. |
> | Действие | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/regenerateKeys/action | Правило авторизации центра уведомлений повторно создает первичный или вторичный ключ. Укажите ключ, который должен быть создан повторно. |
> | Действие | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/write | Создает правила авторизации центра уведомлений и обновляет его свойства. Можно обновить права доступа, первичный ключ и вторичный ключ для правил авторизации. |
> | Действие | Microsoft.NotificationHubs/Namespaces/NotificationHubs/debugSend/action | Отправляет тестовое push-уведомление. |
> | Действие | Microsoft.NotificationHubs/Namespaces/NotificationHubs/Delete | Удаляет ресурс центра уведомлений. |
> | Действие | Microsoft.NotificationHubs/Namespaces/NotificationHubs/metricDefinitions/read | Возвращает список описаний ресурсов метрик пространства имен. |
> | Действие | Microsoft.NotificationHubs/Namespaces/NotificationHubs/pnsCredentials/action | Возвращает все учетные данные PNS центра уведомлений. Сюда входят учетные данные WNS, MPNS, APNS, GCM и Baidu. |
> | Действие | Microsoft.NotificationHubs/Namespaces/NotificationHubs/read | Возвращает список описаний ресурсов центра уведомлений. |
> | Действие | Microsoft.NotificationHubs/Namespaces/NotificationHubs/write | Создает центр уведомлений и обновляет его свойства. К этим свойствам в основном относятся учетные данные PNS. Правила авторизации и срок жизни. |
> | Действие | Microsoft.NotificationHubs/Namespaces/read | Возвращает список описаний ресурсов пространства имен. |
> | Действие | Microsoft.NotificationHubs/Namespaces/write | Создает ресурс пространства имен и обновляет его свойства. К этим свойствам относятся теги и пространство имен. |
> | Действие | Microsoft.NotificationHubs/operationResults/read | Возвращает результаты операции для поставщика концентраторов уведомлений |
> | Действие | Microsoft.NotificationHubs/operations/read | Возвращает список поддерживаемых операций для поставщика Центров уведомлений |
> | Действие | Microsoft.NotificationHubs/register/action | Регистрирует подписку для поставщика ресурсов центров уведомлений и позволяет создавать пространства имен и центры уведомлений. |
> | Действие | Microsoft.NotificationHubs/unregister/action | Отменяет регистрацию подписки для поставщика ресурсов центров уведомлений, позволяющей создавать пространства имен и центры уведомлений. |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.OffAzure/HyperVSites/clusters/read | Получает свойства кластера Hyper-V. |
> | Действие | Microsoft.OffAzure/HyperVSites/clusters/write | Создает или обновляет кластер Hyper-V. |
> | Действие | Microsoft.OffAzure/HyperVSites/delete | Удаляет сайт Hyper-V. |
> | Действие | Microsoft.OffAzure/HyperVSites/hosts/read | Получает свойства узла Hyper-V. |
> | Действие | Microsoft.OffAzure/HyperVSites/hosts/write | Создает или обновляет узел Hyper-V. |
> | Действие | Microsoft.OffAzure/HyperVSites/jobs/read | Получает свойства заданий Hyper-V. |
> | Действие | Microsoft.OffAzure/HyperVSites/machines/read | Получает свойства компьютеров Hyper-V. |
> | Действие | Microsoft.OffAzure/HyperVSites/machines/start/action | Запускает компьютеры Hyper-V. |
> | Действие | Microsoft.OffAzure/HyperVSites/machines/stop/action | Останавливает компьютеры Hyper-V. |
> | Действие | Microsoft.OffAzure/HyperVSites/operationsstatus/read | Получает свойства состояния операции Hyper-V. |
> | Действие | Microsoft.OffAzure/HyperVSites/read | Получает свойства сайта Hyper-V. |
> | Действие | Microsoft.OffAzure/HyperVSites/refresh/action | Обновляет объекты в пределах сайта Hyper-V. |
> | Действие | Microsoft.OffAzure/HyperVSites/runasaccounts/read | Получает свойства учетных записей запуска от имени Hyper-V. |
> | Действие | Microsoft.OffAzure/HyperVSites/usage/read | Получает сведения об использовании сайта Hyper-V. |
> | Действие | Microsoft.OffAzure/HyperVSites/write | Создает или обновляет сайт Hyper-V. |
> | Действие | Microsoft.OffAzure/Operations/read | Чтение предоставленных операций. |
> | Действие | Microsoft.OffAzure/register/action | Регистрирует подписку в поставщике ресурсов Microsoft.OffAzure. |
> | Действие | Microsoft.OffAzure/VMwareSites/delete | Удаляет сайт VMware. |
> | Действие | Microsoft.OffAzure/VMwareSites/jobs/read | Получает свойства заданий VMware. |
> | Действие | Microsoft.OffAzure/VMwareSites/machines/read | Получает свойства компьютеров VMware. |
> | Действие | Microsoft.OffAzure/VMwareSites/machines/start/action | Запускает компьютеры VMware. |
> | Действие | Microsoft.OffAzure/VMwareSites/machines/stop/action | Останавливает компьютеры VMware. |
> | Действие | Microsoft.OffAzure/VMwareSites/operationsstatus/read | Получает свойства состояния операции VMware. |
> | Действие | Microsoft.OffAzure/VMwareSites/read | Получает свойства сайта VMware. |
> | Действие | Microsoft.OffAzure/VMwareSites/refresh/action | Обновляет объекты в пределах сайта VMware. |
> | Действие | Microsoft.OffAzure/VMwareSites/runasaccounts/read | Получает свойства учетных записей запуска от имени VMware. |
> | Действие | Microsoft.OffAzure/VMwareSites/usage/read | Получает сведения об использовании сайта VMware. |
> | Действие | Microsoft.OffAzure/VMwareSites/vcenters/read | Получает свойства VMware vCenter. |
> | Действие | Microsoft.OffAzure/VMwareSites/vcenters/write | Создает или обновляет VMware vCenter. |
> | Действие | Microsoft.OffAzure/VMwareSites/write | Создает или обновляет сайт VMware. |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.OperationalInsights/linkTargets/read | Выводит список существующих учетных записей, которые не связаны с подпиской Azure. Чтобы связать эту подписку Azure с рабочей областью, укажите идентификатор клиента, возвращаемый этой операцией, в свойстве идентификатора клиента в операции создания рабочей области. |
> | Действие | microsoft.operationalinsights/operations/read | Отображение списка всех доступных операций REST API OperationalInsights. |
> | Действие | Microsoft.operationalinsights/Register/Action | Rergisters подписки. |
> | Действие | Microsoft.OperationalInsights/register/action | Регистрирует подписку для поставщика ресурсов. |
> | Действие | Microsoft.operationalinsights/unregister/Action | Отмена регистрации подписки. |
> | Действие | Microsoft.OperationalInsights/workspaces/analytics/query/action | Поиск с помощью нового механизма. |
> | Действие | Microsoft.OperationalInsights/workspaces/analytics/query/schema/read | Получение схемы поиска версии 2. |
> | Действие | Microsoft.OperationalInsights/workspaces/api/query/action | Поиск с помощью нового механизма. |
> | Действие | Microsoft.OperationalInsights/workspaces/api/query/schema/read | Получение схемы поиска версии 2. |
> | Действие | Microsoft.OperationalInsights/workspaces/configurationScopes/delete | Удаляет область конфигурации. |
> | Действие | Microsoft.OperationalInsights/workspaces/configurationScopes/read | Возвращает область конфигурации. |
> | Действие | Microsoft.OperationalInsights/workspaces/configurationScopes/write | Задает область конфигурации. |
> | Действие | Microsoft.OperationalInsights/workspaces/datasources/delete | Удаляет источники данных в рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/datasources/read | Возвращает источники данных в рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/datasources/write | Создает или обновляет источники данных в рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/delete | Удаляет рабочую область. Если при создании рабочая область была связана с существующей рабочей областью, то эта связанная рабочая область не удаляется. |
> | Действие | Microsoft.OperationalInsights/workspaces/gateways/delete | Удаляет шлюз, настроенный для рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/generateregistrationcertificate/action | Создает сертификат регистрации для рабочей области. Этот сертификат используется для подключения Microsoft System Center Operations Manager к рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/intelligencepacks/disable/action | Отключает пакет аналитики для заданной рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/intelligencepacks/enable/action | Включает пакет аналитики для заданной рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/intelligencepacks/read | Выводит список всех пакетов аналитики, которые являются видимыми для заданной рабочей области, а также сведения о том, включен или отключен пакет для этой рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/linkedServices/delete | Удаление связанной службы в заданной рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/linkedServices/read | Получение связанных служб в заданной рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/linkedServices/write | Создание или обновление связанных служб в заданной рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/listKeys/action | Получение списка ключей для рабочей области. Эти ключи используются для подключения агентов Microsoft Operational Insights к рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/listKeys/read | Получение списка ключей для рабочей области. Эти ключи используются для подключения агентов Microsoft Operational Insights к рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/managementGroups/read | Возвращает имена и метаданные для групп управления System Center Operations Manager, подключенных к этой рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/metricDefinitions/read | Получение определений метрик в рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/notificationSettings/delete | Удаление параметров уведомления пользователя для рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/notificationSettings/read | Получение параметров уведомления пользователя для рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/notificationSettings/write | Настройка параметров уведомления пользователя для рабочей области. |
> | Действие | Microsoft.operationalinsights/workspaces/Operations/Read | Возвращает состояние операции OperationalInsights рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/purge/action | Удаление указанных данных из рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesAccountLogon/read | Чтение данных из таблицы AADDomainServicesAccountLogon |
> | Действие | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesLogonLogoff/read | Чтение данных из таблицы AADDomainServicesLogonLogoff |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ADAssessmentRecommendation/read | Чтение данных из таблицы ADAssessmentRecommendation. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ADReplicationResult/read | Чтение данных из таблицы ADReplicationResult. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ADSecurityAssessmentRecommendation/read | Чтение данных из таблицы ADSecurityAssessmentRecommendation. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/Alert/read | Чтение данных из таблицы Alert. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/AlertHistory/read | Чтение данных из таблицы AlertHistory. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/AppCenterError/read | Чтение данных из таблицы AppCenterError |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ApplicationInsights/read | Чтение данных из таблицы ApplicationInsights. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/AuditLogs/read | Чтение данных из таблицы AuditLogs |
> | Действие | Microsoft.OperationalInsights/workspaces/query/AutoscaleEvaluationsLog/read | Чтение данных из таблицы AutoscaleEvaluationsLog |
> | Действие | Microsoft.OperationalInsights/workspaces/query/AutoscaleScaleActionsLog/read | Чтение данных из таблицы AutoscaleScaleActionsLog |
> | Действие | Microsoft.OperationalInsights/workspaces/query/AWSCloudTrail/read | Чтение данных из таблицы AWSCloudTrail |
> | Действие | Microsoft.OperationalInsights/workspaces/query/AzureActivity/read | Чтение данных из таблицы AzureActivity. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/AzureMetrics/read | Чтение данных из таблицы AzureMetrics. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/BoundPort/read | Чтение данных из таблицы BoundPort. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/CommonSecurityLog/read | Чтение данных из таблицы CommonSecurityLog. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ComputerGroup/read | Чтение данных из таблицы ComputerGroup. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ConfigurationChange/read | Чтение данных из таблицы ConfigurationChange. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ConfigurationData/read | Чтение данных из таблицы ConfigurationData. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ContainerImageInventory/read | Чтение данных из таблицы ContainerImageInventory. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ContainerInventory/read | Чтение данных из таблицы ContainerInventory. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ContainerLog/read | Чтение данных из таблицы ContainerLog. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ContainerNodeInventory/read | Чтение данных из таблицы ContainerNodeInventory |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ContainerServiceLog/read | Чтение данных из таблицы ContainerServiceLog. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DatabricksAccounts/read | Чтение данных из таблицы DatabricksAccounts |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DatabricksClusters/read | Чтение данных из таблицы DatabricksClusters |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DatabricksDBFS/read | Чтение данных из таблицы DatabricksDBFS |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DatabricksJobs/read | Чтение данных из таблицы DatabricksJobs |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DatabricksNotebook/read | Чтение данных из таблицы DatabricksNotebook |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DatabricksSecrets/read | Чтение данных из таблицы DatabricksSecrets |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DatabricksSQLPermissions/read | Чтение данных из таблицы DatabricksSQLPermissions |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DatabricksSSH/read | Чтение данных из таблицы DatabricksSSH |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DatabricksTables/read | Чтение данных из таблицы DatabricksTables |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DatabricksWorkspace/read | Чтение данных из таблицы DatabricksWorkspace |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DeviceAppCrash/read | Чтение данных из таблицы DeviceAppCrash. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DeviceAppLaunch/read | Чтение данных из таблицы DeviceAppLaunch. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DeviceCalendar/read | Чтение данных из таблицы DeviceCalendar. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DeviceCleanup/read | Чтение данных из таблицы DeviceCleanup. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DeviceConnectSession/read | Чтение данных из таблицы DeviceConnectSession. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DeviceEtw/read | Чтение данных из таблицы DeviceEtw. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DeviceHardwareHealth/read | Чтение данных из таблицы DeviceHardwareHealth. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DeviceHealth/read | Чтение данных из таблицы DeviceHealth. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DeviceHeartbeat/read | Чтение данных из таблицы DeviceHeartbeat. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DeviceSkypeHeartbeat/read | Чтение данных из таблицы DeviceSkypeHeartbeat. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DeviceSkypeSignIn/read | Чтение данных из таблицы DeviceSkypeSignIn. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DeviceSleepState/read | Чтение данных из таблицы DeviceSleepState. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DHAppFailure/read | Чтение данных из таблицы DHAppFailure. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DHAppReliability/read | Чтение данных из таблицы DHAppReliability. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DHDriverReliability/read | Чтение данных из таблицы DHDriverReliability. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DHLogonFailures/read | Чтение данных из таблицы DHLogonFailures. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DHLogonMetrics/read | Чтение данных из таблицы DHLogonMetrics. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DHOSCrashData/read | Чтение данных из таблицы DHOSCrashData. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DHOSReliability/read | Чтение данных из таблицы DHOSReliability. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DHWipAppLearning/read | Чтение данных из таблицы DHWipAppLearning. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DnsEvents/read | Чтение данных из таблицы DnsEvents. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/DnsInventory/read | Чтение данных из таблицы DnsInventory. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ETWEvent/read | Чтение данных из таблицы ETWEvent. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/Event/read | Чтение данных из таблицы Event. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ExchangeAssessmentRecommendation/read | Чтение данных из таблицы ExchangeAssessmentRecommendation. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ExchangeOnlineAssessmentRecommendation/read | Чтение данных из таблицы ExchangeOnlineAssessmentRecommendation. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/Heartbeat/read | Чтение данных из таблицы Heartbeat. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/HuntingBookmark/read | Чтение данных из таблицы HuntingBookmark |
> | Действие | Microsoft.OperationalInsights/workspaces/query/IISAssessmentRecommendation/read | Чтение данных из таблицы IISAssessmentRecommendation. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/InboundConnection/read | Чтение данных из таблицы InboundConnection. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/InsightsMetrics/read | Чтение данных из таблицы InsightsMetrics |
> | Действие | Microsoft.OperationalInsights/workspaces/query/IntuneAuditLogs/read | Чтение данных из таблицы IntuneAuditLogs. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/IntuneOperationalLogs/read | Чтение данных из таблицы IntuneOperationalLogs. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/KubeEvents/read | Чтение данных из таблицы KubeEvents |
> | Действие | Microsoft.OperationalInsights/workspaces/query/KubeNodeInventory/read | Чтение данных из таблицы KubeNodeInventory. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/KubePodInventory/read | Чтение данных из таблицы KubePodInventory. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/KubeServices/read | Чтение данных из таблицы KubeServices |
> | Действие | Microsoft.OperationalInsights/workspaces/query/LinuxAuditLog/read | Чтение данных из таблицы LinuxAuditLog. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAApplication/read | Чтение данных из таблицы MAApplication. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealth/read | Чтение данных из таблицы MAApplicationHealth. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealthAlternativeVersions/read | Считывает данные из таблицы MAApplicationHealthAlternativeVersions. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealthIssues/read | Считывает данные из таблицы MAApplicationHealthIssues. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAApplicationInstance/read | Чтение данных из таблицы MAApplicationInstance. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAApplicationInstanceReadiness/read | Считывает данные из таблицы MAApplicationInstanceReadiness. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAApplicationReadiness/read | Чтение данных из таблицы MAApplicationReadiness. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MADeploymentPlan/read | Чтение данных из таблицы MADeploymentPlan. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MADevice/read | Чтение данных из таблицы MADevice. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MADeviceNotEnrolled/read | Считывание данных из таблицы MADeviceNotEnrolled |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MADeviceNRT/read | Чтение данных из таблицы MADeviceNRT |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealth/read | Считывает данные из таблицы MADevicePnPHealth. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealthAlternativeVersions/read | Считывает данные из таблицы MADevicePnPHealthAlternativeVersions. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealthIssues/read | Считывает данные из таблицы MADevicePnPHealthIssues. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MADeviceReadiness/read | Чтение данных из таблицы MADeviceReadiness. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MADriverInstanceReadiness/read | Считывает данные из таблицы MADriverInstanceReadiness. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MADriverReadiness/read | Чтение данных из таблицы MADriverReadiness. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddin/read | Чтение данных из таблицы MAOfficeAddin. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinEntityHealth/read | Чтение данных из таблицы MAOfficeAddinEntityHealth |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealth/read | Чтение данных из таблицы MAOfficeAddinHealth. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealthEventNRT/read | Чтение данных из таблицы MAOfficeAddinHealthEventNRT |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealthIssues/read | Считывает данные из таблицы MAOfficeAddinHealthIssues. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinInstance/read | Чтение данных из таблицы MAOfficeAddinInstance. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinInstanceReadiness/read | Считывает данные из таблицы MAOfficeAddinInstanceReadiness. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinReadiness/read | Чтение данных из таблицы MAOfficeAddinReadiness. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeApp/read | Чтение данных из таблицы MAOfficeApp. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppCrashesNRT/read | Чтение данных из таблицы MAOfficeAppCrashesNRT |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppHealth/read | Чтение данных из таблицы MAOfficeAppHealth. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppInstance/read | Чтение данных из таблицы MAOfficeAppInstance. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppInstanceHealth/read | Чтение данных из таблицы MAOfficeAppInstanceHealth |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppReadiness/read | Чтение данных из таблицы MAOfficeAppReadiness. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppSessionsNRT/read | Чтение данных из таблицы MAOfficeAppSessionsNRT |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeBuildInfo/read | Чтение данных из таблицы MAOfficeBuildInfo. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeCurrencyAssessment/read | Чтение данных из таблицы MAOfficeCurrencyAssessment. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeCurrencyAssessmentDailyCounts/read | Чтение данных из таблицы MAOfficeCurrencyAssessmentDailyCounts. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeDeploymentStatus/read | Чтение данных из таблицы MAOfficeDeploymentStatus. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeDeploymentStatusNRT/read | Чтение данных из таблицы MAOfficeDeploymentStatusNRT |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroErrorNRT/read | Чтение данных из таблицы MAOfficeMacroErrorNRT |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroGlobalHealth/read | Чтение данных из таблицы MAOfficeMacroGlobalHealth |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroHealth/read | Считывает данные из таблицы MAOfficeMacroHealth. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroHealthIssues/read | Считывает данные из таблицы MAOfficeMacroHealthIssues. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroIssueInstanceReadiness/read | Считывает данные из таблицы MAOfficeMacroIssueInstanceReadiness. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroIssueReadiness/read | Чтение данных из таблицы MAOfficeMacroIssueReadiness. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroSummary/read | Чтение данных из таблицы MAOfficeMacroSummary. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeSuite/read | Чтение данных из таблицы MAOfficeSuite. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAOfficeSuiteInstance/read | Чтение данных из таблицы MAOfficeSuiteInstance. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAProposedPilotDevices/read | Чтение данных из таблицы MAProposedPilotDevices. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAWindowsBuildInfo/read | Чтение данных из таблицы MAWindowsBuildInfo. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAWindowsCurrencyAssessment/read | Чтение данных из таблицы MAWindowsCurrencyAssessment. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAWindowsCurrencyAssessmentDailyCounts/read | Чтение данных из таблицы MAWindowsCurrencyAssessmentDailyCounts. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAWindowsDeploymentStatus/read | Чтение данных из таблицы MAWindowsDeploymentStatus. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAWindowsDeploymentStatusNRT/read | Чтение данных из таблицы MAWindowsDeploymentStatusNRT |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MAWindowsSysReqInstanceReadiness/read | Считывает данные из таблицы MAWindowsSysReqInstanceReadiness. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/MicrosoftWebW3CLog/read | Чтение данных из таблицы MicrosoftWebW3CLog |
> | Действие | Microsoft.OperationalInsights/workspaces/query/NetworkMonitoring/read | Чтение данных из таблицы NetworkMonitoring. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/OfficeActivity/read | Чтение данных из таблицы OfficeActivity. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/Operation/read | Чтение данных из таблицы Operation. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/OutboundConnection/read | Чтение данных из таблицы OutboundConnection. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/Perf/read | Чтение данных из таблицы Perf. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ProtectionStatus/read | Чтение данных из таблицы ProtectionStatus. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/read | Выполнение запросов к данным в рабочей области |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ReservedAzureCommonFields/read | Чтение данных из таблицы ReservedAzureCommonFields. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ReservedCommonFields/read | Чтение данных из таблицы ReservedCommonFields. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SCCMAssessmentRecommendation/read | Чтение данных из таблицы SCCMAssessmentRecommendation. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SCOMAssessmentRecommendation/read | Чтение данных из таблицы SCOMAssessmentRecommendation. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SecurityAlert/read | Чтение данных из таблицы SecurityAlert. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SecurityBaseline/read | Чтение данных из таблицы SecurityBaseline. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SecurityBaselineSummary/read | Чтение данных из таблицы SecurityBaselineSummary. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SecurityDetection/read | Чтение данных из таблицы SecurityDetection. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SecurityEvent/read | Чтение данных из таблицы SecurityEvent. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SecurityIoTRawEvent/read | Чтение данных из таблицы SecurityIoTRawEvent |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SecurityRecommendation/read | Чтение данных из таблицы SecurityRecommendation |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ServiceFabricOperationalEvent/read | Чтение данных из таблицы ServiceFabricOperationalEvent. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ServiceFabricReliableActorEvent/read | Чтение данных из таблицы ServiceFabricReliableActorEvent. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ServiceFabricReliableServiceEvent/read | Чтение данных из таблицы ServiceFabricReliableServiceEvent. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SfBAssessmentRecommendation/read | Чтение данных из таблицы SfBAssessmentRecommendation. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SfBOnlineAssessmentRecommendation/read | Чтение данных из таблицы SfBOnlineAssessmentRecommendation. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SharePointOnlineAssessmentRecommendation/read | Чтение данных из таблицы SharePointOnlineAssessmentRecommendation. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SigninLogs/read | Чтение данных из таблицы SigninLogs |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SPAssessmentRecommendation/read | Чтение данных из таблицы SPAssessmentRecommendation. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SQLAssessmentRecommendation/read | Чтение данных из таблицы SQLAssessmentRecommendation. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SQLQueryPerformance/read | Чтение данных из таблицы SQLQueryPerformance. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SqlThreatProtectionLoginAudits/read | Чтение данных из таблицы SqlThreatProtectionLoginAudits |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SqlVulnerabilityAssessmentResult/read | Чтение данных из таблицы SqlVulnerabilityAssessmentResult |
> | Действие | Microsoft.OperationalInsights/workspaces/query/Syslog/read | Чтение данных из таблицы Syslog. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/SysmonEvent/read | Чтение данных из таблицы SysmonEvent. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/Tables.Custom/read | Чтение данных из любого пользовательского журнала. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/ThreatIntelligenceIndicator/read | Чтение данных из таблицы ThreatIntelligenceIndicator |
> | Действие | Microsoft.OperationalInsights/workspaces/query/UAApp/read | Чтение данных из таблицы UAApp. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/UAComputer/read | Чтение данных из таблицы UAComputer. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/UAComputerRank/read | Чтение данных из таблицы UAComputerRank. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/UADriver/read | Чтение данных из таблицы UADriver. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/UADriverProblemCodes/read | Чтение данных из таблицы UADriverProblemCodes. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/UAFeedback/read | Чтение данных из таблицы UAFeedback. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/UAHardwareSecurity/read | Чтение данных из таблицы UAHardwareSecurity. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/UAIESiteDiscovery/read | Чтение данных из таблицы UAIESiteDiscovery. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/UAOfficeAddIn/read | Чтение данных из таблицы UAOfficeAddIn. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/UAProposedActionPlan/read | Чтение данных из таблицы UAProposedActionPlan. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/UASysReqIssue/read | Чтение данных из таблицы UASysReqIssue. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/UAUpgradedComputer/read | Чтение данных из таблицы UAUpgradedComputer. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/Update/read | Чтение данных из таблицы Update. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/UpdateRunProgress/read | Чтение данных из таблицы UpdateRunProgress. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/UpdateSummary/read | Чтение данных из таблицы UpdateSummary. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/Usage/read | Чтение данных из таблицы Usage. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/VMBoundPort/read | Считывает данные из таблицы VMBoundPort. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/VMConnection/read | Считывает данные из таблицы VMConnection. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/W3CIISLog/read | Чтение данных из таблицы W3CIISLog. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/WaaSDeploymentStatus/read | Чтение данных из таблицы WaaSDeploymentStatus. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/WaaSInsiderStatus/read | Чтение данных из таблицы WaaSInsiderStatus. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/WaaSUpdateStatus/read | Чтение данных из таблицы WaaSUpdateStatus. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/WDAVStatus/read | Чтение данных из таблицы WDAVStatus. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/WDAVThreat/read | Чтение данных из таблицы WDAVThreat. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/WindowsClientAssessmentRecommendation/read | Чтение данных из таблицы WindowsClientAssessmentRecommendation. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/WindowsEvent/read | Чтение данных из таблицы WindowsEvent |
> | Действие | Microsoft.OperationalInsights/workspaces/query/WindowsFirewall/read | Чтение данных из таблицы WindowsFirewall. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/WindowsServerAssessmentRecommendation/read | Чтение данных из таблицы WindowsServerAssessmentRecommendation. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/WireData/read | Чтение данных из таблицы WireData. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/WorkloadMonitoringPerf/read | Чтение данных из таблицы WorkloadMonitoringPerf. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/WUDOAggregatedStatus/read | Чтение данных из таблицы WUDOAggregatedStatus. |
> | Действие | Microsoft.OperationalInsights/workspaces/query/WUDOStatus/read | Чтение данных из таблицы WUDOStatus. |
> | Действие | Microsoft.OperationalInsights/workspaces/read | Возвращает существующую рабочую область. |
> | Действие | Microsoft.OperationalInsights/workspaces/regeneratesharedkey/action | Повторно создает общий ключ указанной рабочей области |
> | Действие | microsoft.operationalinsights/workspaces/rules/read | Получение всех правил генерации оповещений. |
> | Действие | Microsoft.OperationalInsights/workspaces/savedSearches/delete | Удаляет сохраненный поисковый запрос. |
> | Действие | Microsoft.OperationalInsights/workspaces/savedSearches/read | Возвращает сохраненный поисковый запрос. |
> | Действие | microsoft.operationalinsights/workspaces/savedsearches/results/read | Получение сохраненных результатов поиска. Не рекомендуется |
> | Действие | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/delete | Удаление запланированных действий поиска. |
> | Действие | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/read | Получение запланированных действий поиска. |
> | Действие | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/write | Создание или обновление запланированных действий поиска. |
> | Действие | microsoft.operationalinsights/workspaces/savedsearches/schedules/delete | Удаление запланированного поиска. |
> | Действие | microsoft.operationalinsights/workspaces/savedsearches/schedules/read | Получение запланированного поиска. |
> | Действие | microsoft.operationalinsights/workspaces/savedsearches/schedules/write | Создание или обновление запланированного поиска. |
> | Действие | Microsoft.OperationalInsights/workspaces/savedSearches/write | Создает сохраненный поисковый запрос. |
> | Действие | Microsoft.OperationalInsights/workspaces/schema/read | Возвращает схему поиска для рабочей области.  Схема поиска содержит предоставленные поля и их типы. |
> | Действие | Microsoft.OperationalInsights/workspaces/search/action | Выполняет поисковый запрос. |
> | Действие | microsoft.operationalinsights/workspaces/search/read | Получение результатов поиска. Не рекомендуется. |
> | Действие | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Извлекает открытые ключи для рабочей области. Эти ключи используются для подключения агентов Microsoft Operational Insights к рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Извлекает открытые ключи для рабочей области. Эти ключи используются для подключения агентов Microsoft Operational Insights к рабочей области. |
> | Действие | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/delete | Удаляет конфигурацию хранилища. Это остановит чтение данных из учетной записи хранения, выполняемое Microsoft Operational Insights. |
> | Действие | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/read | Возвращает конфигурацию хранилища. |
> | Действие | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/write | Создает конфигурацию учетной записи хранения. Эти конфигурации используются для извлечения данных из расположения в существующей учетной записи хранения. |
> | Действие | Microsoft.OperationalInsights/workspaces/upgradetranslationfailures/read | Получение журнала ошибок преобразования обновления поиска для рабочей области |
> | Действие | Microsoft.OperationalInsights/workspaces/usages/read | Возвращает данные об использовании для рабочей области, включая объем данных, считываемых рабочей областью. |
> | Действие | Microsoft.OperationalInsights/workspaces/write | Создает новую рабочую область или устанавливает связь с существующей рабочей областью с помощью указанного идентификатора клиента, полученного из нее. |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.OperationsManagement/managementAssociations/delete | Удаление существующей связи управления. |
> | Действие | Microsoft.OperationsManagement/managementAssociations/read | Получение существующей связи управления. |
> | Действие | Microsoft.OperationsManagement/managementAssociations/write | Создание новой связи управления. |
> | Действие | Microsoft.OperationsManagement/managementConfigurations/delete | Удаление существующей конфигурации управления. |
> | Действие | Microsoft.OperationsManagement/managementConfigurations/read | Получение существующей конфигурации управления. |
> | Действие | Microsoft.OperationsManagement/managementConfigurations/write | Создание новой конфигурации управления. |
> | Действие | Microsoft.OperationsManagement/register/action | Регистрирует подписку для поставщика ресурсов. |
> | Действие | Microsoft.OperationsManagement/solutions/delete | Удаляет существующее решение OMS. |
> | Действие | Microsoft.OperationsManagement/solutions/read | Возвращает существующее решение OMS. |
> | Действие | Microsoft.OperationsManagement/solutions/write | Создает решение OMS. |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.PolicyInsights/asyncOperationResults/read | Получает результат асинхронной операции. |
> | Действие | Microsoft.PolicyInsights/operations/read | Получает поддерживаемые операции на пространство имен Microsoft.PolicyInsights |
> | Действие | Microsoft.PolicyInsights/policyEvents/queryResults/action | Запрашивает сведения о событиях политики. |
> | Действие | Microsoft.PolicyInsights/policyEvents/queryResults/read | Запрашивает сведения о событиях политики. |
> | Действие | Microsoft.PolicyInsights/policyStates/queryResults/action | Запрашивает сведения о состояниях политики. |
> | Действие | Microsoft.PolicyInsights/policyStates/queryResults/read | Запрашивает сведения о состояниях политики. |
> | Действие | Microsoft.PolicyInsights/policyStates/summarize/action | Запрашивает сводную информацию о последних состояниях политики. |
> | Действие | Microsoft.PolicyInsights/policyStates/summarize/read | Запрашивает сводную информацию о последних состояниях политики. |
> | Действие | Microsoft.PolicyInsights/policyStates/triggerEvaluation/action | Активирует новую оценку соответствия для выбранной области. |
> | Действие | Microsoft.PolicyInsights/policyTrackedResources/queryResults/read | Запрос сведений о ресурсах, необходимых для политик DeployIfNotExists. |
> | Действие | Microsoft.PolicyInsights/register/action | Регистрирует поставщик ресурсов Policy Insights Майкрософт и позволяет действия в ней. |
> | Действие | Microsoft.PolicyInsights/remediations/cancel/action | Отмена выполняющихся исправления политики Microsoft. |
> | Действие | Microsoft.PolicyInsights/remediations/delete | Удаление исправлений политики. |
> | Действие | Microsoft.PolicyInsights/remediations/listDeployments/read | Перечисление развертываний, необходимых для исправления политики. |
> | Действие | Microsoft.PolicyInsights/remediations/read | Получение исправлений политики. |
> | Действие | Microsoft.PolicyInsights/remediations/write | Создание или обновление политики Microsoft исправления. |
> | Действие | Microsoft.PolicyInsights/unregister/action | Отменяет регистрацию поставщика ресурсов Microsoft Policy Insights. |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Portal/consoles/delete | Удаляет экземпляр Cloud Shell. |
> | Действие | Microsoft.Portal/consoles/read | Считывает экземпляр Cloud Shell. |
> | Действие | Microsoft.Portal/consoles/write | Создание или обновление экземпляра Cloud Shell. |
> | Действие | Microsoft.Portal/dashboards/delete | Удаление панели мониторинга из подписки. |
> | Действие | Microsoft.Portal/dashboards/read | Чтение панели мониторинга для подписки. |
> | Действие | Microsoft.Portal/dashboards/write | Добавление или изменение панели мониторинга для подписки. |
> | Действие | Microsoft.Portal/register/action | Регистрация на портале. |
> | Действие | Microsoft.Portal/usersettings/delete | Удаляет параметры пользователя Cloud Shell. |
> | Действие | Microsoft.Portal/usersettings/read | Считывает параметры пользователя Cloud Shell. |
> | Действие | Microsoft.Portal/usersettings/write | Создание или обновление параметров пользователя Cloud Shell. |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.PowerBIDedicated/capacities/delete | Удаление выделенной емкости Power BI. |
> | Действие | Microsoft.PowerBIDedicated/capacities/read | Извлечение сведений об указанной выделенной емкости Power BI. |
> | Действие | Microsoft.PowerBIDedicated/capacities/resume/action | Возобновляет использование емкости. |
> | Действие | Microsoft.PowerBIDedicated/capacities/skus/read | Получение доступных сведений о номере SKU для емкости. |
> | Действие | Microsoft.PowerBIDedicated/capacities/suspend/action | Приостановка использования емкости. |
> | Действие | Microsoft.PowerBIDedicated/capacities/write | Создание или обновление указанной выделенной емкости Power BI. |
> | Действие | Microsoft.PowerBIDedicated/locations/checkNameAvailability/action | Проверка того, что имя выделенной емкости Power BI является допустимым и не используется. |
> | Действие | Microsoft.PowerBIDedicated/locations/operationresults/read | Получение сведений о результате указанной операции. |
> | Действие | Microsoft.PowerBIDedicated/locations/operationstatuses/read | Извлечение сведений о состоянии указанной операции. |
> | Действие | Microsoft.PowerBIDedicated/operations/read | Извлечение сведений об операциях. |
> | Действие | Microsoft.PowerBIDedicated/register/action | Регистрация выделенного поставщика ресурсов Power BI. |
> | Действие | Microsoft.PowerBIDedicated/skus/read | Извлечение сведений о номерах SKU. |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp является внутренней операцией, используемой службой. |
> | Действие | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocatedStamp является внутренней операцией, используемой службой. |
> | Действие | Microsoft.RecoveryServices/Locations/backupPreValidateProtection/action |  |
> | Действие | Microsoft.RecoveryServices/Locations/backupProtectedItem/write | Создает элемент, защищенный службой архивации. |
> | Действие | Microsoft.RecoveryServices/Locations/backupProtectedItems/read | Возвращает список всех защищенных элементов. |
> | Действие | Microsoft.RecoveryServices/Locations/backupStatus/action | Проверка состояния архивации хранилищ служб восстановления. |
> | Действие | Microsoft.RecoveryServices/Locations/backupValidateFeatures/action | Проверка компонентов. |
> | Действие | Microsoft.RecoveryServices/locations/checkNameAvailability/action | Проверка доступности имени ресурса использует API, позволяющий проверить, доступно ли имя ресурса. |
> | Действие | Microsoft.RecoveryServices/locations/operationStatus/read | Возвращает состояние операции для данной операции |
> | Действие | Microsoft.RecoveryServices/operations/read | Получение списка операций для поставщика ресурсов. |
> | Действие | Microsoft.RecoveryServices/register/action | Регистрация подписки для заданного поставщика ресурсов. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupconfig/read | Возвращает конфигурацию хранилища служб восстановления. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupconfig/write | Обновляет конфигурацию хранилища служб восстановления. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupEngines/read | Возвращение всех серверов управления архивацией, зарегистрированных в хранилище. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/delete | Удаление цели защиты для резервного копирования. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read | Получение цели защиты для резервного копирования. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Создание цели защиты для резервной копии. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Возвращает состояние операции. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Получение всех защищаемых контейнеров. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/delete | Удаление зарегистрированного контейнера. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action | Запрос рабочих нагрузок в контейнере. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Получение всех элементов в контейнере. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Возвращает результат операции, выполненной с контейнером защиты. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Архивирует защищенный элемент. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/delete | Удаляет защищенный элемент. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Возвращает результат операции, выполненной с защищенными элементами. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Возвращает состояние операции, выполненной с защищенными элементами. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Возвращает сведения об объекте для защищенного элемента. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Подготавливает мгновенное восстановление для защищенного элемента. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Возвращает точки восстановления для защищенных элементов. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Восстанавливает точки восстановления для защищенных элементов. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Отменяет мгновенное восстановление для защищенного элемента. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Создает элемент, защищенный службой архивации. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Возвращает все зарегистрированные контейнеры. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write | Создание зарегистрированного контейнера. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Обновляет список контейнеров. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | Отменяет задание. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Возвращает результат операции задания. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupJobs/read | Возвращает все объекты заданий. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Экспорт заданий |
> | Действие | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Возвращает результат операции архивации для хранилища служб восстановления. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupOperations/read | Возвращает состояние операции архивации для хранилища служб восстановления. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupPolicies/delete | Удаляет политику защиты. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Возвращает результаты операции политики. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | Возвращает состояние операции политики. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Возвращает все политики защиты. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Создает политику защиты. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Возвращает список всех защищаемых элементов. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Возвращает список всех защищенных элементов. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Возвращает все контейнеры, принадлежащие подписке. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Вывод списка всех целей защиты для резервного копирования. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/action | Возвращает данные ПИН-кода безопасности для хранилища служб восстановления. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupstorageconfig/read | Возвращает конфигурацию службы хранилища для хранилища служб восстановления. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupstorageconfig/write | Обновляет конфигурацию службы хранилища для хранилища служб восстановления. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Возвращает сводки по защищенным элементам и защищенным серверам для служб восстановления. |
> | Действие | Microsoft.RecoveryServices/Vaults/backupValidateOperation/action | Проверка операций для защищенного элемента |
> | Действие | Microsoft.RecoveryServices/Vaults/certificates/write | Операция обновления сертификата ресурса обновляет сертификат учетных данных для ресурса или хранилища. |
> | Действие | Microsoft.RecoveryServices/Vaults/delete | Операция удаления хранилища удаляет указанный ресурс Azure типа "хранилище". |
> | Действие | Microsoft.RecoveryServices/Vaults/extendedInformation/delete | Операция получения расширенных сведений о хранилище возвращает расширенные сведения об объекте, представляющие ресурс Azure типа "хранилище". |
> | Действие | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Операция получения расширенных сведений о хранилище возвращает расширенные сведения об объекте, представляющие ресурс Azure типа "хранилище". |
> | Действие | Microsoft.RecoveryServices/Vaults/extendedInformation/write | Операция получения расширенных сведений о хранилище возвращает расширенные сведения об объекте, представляющие ресурс Azure типа "хранилище". |
> | Действие | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Возвращает оповещения для хранилища служб восстановления. |
> | Действие | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Разрешение оповещения. |
> | Действие | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/read | Возвращает конфигурацию уведомлений хранилища служб восстановления. |
> | Действие | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/write | Настройка уведомлений по электронной почте в хранилище служб восстановления. |
> | Действие | Microsoft.RecoveryServices/Vaults/read | Операция получения хранилища возвращает объект, представляющий ресурс Azure типа "хранилище". |
> | Действие | Microsoft.RecoveryServices/Vaults/registeredIdentities/delete | Операцию отмены регистрации контейнера можно использовать для отмены регистрации контейнера. |
> | Действие | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Операцию получения результатов операции можно использовать, чтобы получить состояние и результат асинхронно отправленной операции. |
> | Действие | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Операцию получения контейнеров можно использовать для получения контейнеров, зарегистрированных для ресурса. |
> | Действие | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Операцию регистрации контейнера в службе можно использовать для регистрации контейнера в службе восстановления. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Считывание параметров оповещений |
> | Действие | Microsoft.RecoveryServices/vaults/replicationAlertSettings/write | Создание или обновление параметров оповещений |
> | Действие | Microsoft.RecoveryServices/vaults/replicationEvents/read | Считывание события |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Проверяет согласованность структуры. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/delete | Удаление структуры |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/deployProcessServerImage/action | Развертывает образ сервера обработки. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/migratetoaad/action | Перенос Fabric в AAD |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Считывание структуры |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Повторно задает связь шлюза. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/remove/action | Удаляет структуру. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Обновление сертификата для структуры. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationLogicalNetworks/read | Считывание логических сетей |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Считывание сети |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/delete | Удаление сетевых сопоставлений |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Считывание сетевых сопоставлений |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/write | Создание или обновление сетевых сопоставлений |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/discoverProtectableItem/action | Обнаруживает защищаемый элемент. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Считывание контейнеров защиты |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/remove/action | Удаляет контейнер защиты. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/delete | Удаление любых элементов миграции. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/migrate/action | Перенос элемента. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/migrationRecoveryPoints/read | Чтение любых точек восстановления для миграции. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/read | Чтение любых элементов миграции. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/testMigrate/action | Тестирование миграции. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/testMigrateCleanup/action | Очистка тестовой миграции. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/write | Создание или обновление элементов миграции. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Считывает защищаемые элементы. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/addDisks/action | Добавление дисков |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Применяет точку восстановления. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/delete | Удаление защищенных элементов |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Выполняет отработку отказа с фиксацией. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Плановая отработка отказа |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Считывание защищенных элементов |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Считывает точки восстановления для репликации. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/remove/action | Удаляет защищенный элемент. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/removeDisks/action | Удалите диски |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Исправляет репликацию. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Повторно защищает защищенный элемент. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/ResolveHealthErrors/action |  |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/submitFeedback/action | Отправить отзыв |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/targetComputeSizes/read | Считывание целевых объемов вычислительных ресурсов |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Тестовая отработка отказа |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Тестирует очистку отработки отказа. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Отработка отказа |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Обновить службу мобильности |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/write | Создание или обновление защищенных элементов |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/delete | Удаление сопоставлений контейнера защиты |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Считывание сопоставлений контейнера защиты |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/remove/action | Удаляет сопоставление контейнера защиты. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/write | Создание или обновление сопоставлений контейнера защиты |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action | Переключает контейнер защиты. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/write | Создание или обновление контейнеры защиты |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/delete | Удаление поставщиков службы восстановления |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Считывание поставщиков службы восстановления |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Обновляет поставщик. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/remove/action | Удаляет поставщик служб восстановления. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/write | Создание или обновление поставщиков служб восстановления |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Считывание классификации хранилища |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/delete | Удаление сопоставлений классификаций хранилища |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Считывание сопоставлений классификаций хранилища |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/write | Создание или обновление сопоставлений классификаций хранилища |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/delete | Удаление vCenter |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Чтение vCenter |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/write | Создание или обновление vCenter |
> | Действие | Microsoft.RecoveryServices/vaults/replicationFabrics/write | Создание или обновление Fabrics |
> | Действие | Microsoft.RecoveryServices/vaults/replicationJobs/cancel/action | Отменяет задание. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationJobs/read | Считывание задания |
> | Действие | Microsoft.RecoveryServices/vaults/replicationJobs/restart/action | Перезапускает задание. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationJobs/resume/action | Возобновляет выполнение задания. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationMigrationItems/read | Чтение любых элементов миграции. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationNetworkMappings/read | Считывание сетевых сопоставлений |
> | Действие | Microsoft.RecoveryServices/vaults/replicationNetworks/read | Считывание сети |
> | Действие | Microsoft.RecoveryServices/vaults/replicationPolicies/delete | Удаление политики |
> | Действие | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Считывание политики |
> | Действие | Microsoft.RecoveryServices/vaults/replicationPolicies/write | Создание или обновление политики |
> | Действие | Microsoft.RecoveryServices/vaults/replicationProtectedItems/read | Считывание защищенных элементов |
> | Действие | Microsoft.RecoveryServices/vaults/replicationProtectionContainerMappings/read | Считывание сопоставлений контейнера защиты |
> | Действие | Microsoft.RecoveryServices/vaults/replicationProtectionContainers/read | Считывание контейнеров защиты |
> | Действие | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/delete | Удаление планов восстановления |
> | Действие | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Выполняет отработку отказа с фиксацией для плана восстановления. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Выполняет плановую отработку отказа для плана восстановления. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Считывание планов восстановления |
> | Действие | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Повторно защищает план восстановления. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Тестирует отработку отказа для плана восстановления. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Тестирует очистку отработки отказа для плана восстановления. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Выполняет отработку отказа для плана восстановления. |
> | Действие | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/write | Создание или обновление планов восстановления |
> | Действие | Microsoft.RecoveryServices/vaults/replicationRecoveryServicesProviders/read | Считывание поставщиков службы восстановления |
> | Действие | Microsoft.RecoveryServices/vaults/replicationStorageClassificationMappings/read | Считывание сопоставлений классификаций хранилища |
> | Действие | Microsoft.RecoveryServices/vaults/replicationStorageClassifications/read | Считывание классификации хранилища |
> | Действие | Microsoft.RecoveryServices/vaults/replicationSupportedOperatingSystems/read | Чтение любого элемента.  |
> | Действие | Microsoft.RecoveryServices/vaults/replicationUsages/read | Чтение данных об использовании хранилищ репликации |
> | Действие | Microsoft.RecoveryServices/vaults/replicationVaultHealth/read | Считывание всех сведений о работоспособности репликации хранилища |
> | Действие | Microsoft.RecoveryServices/vaults/replicationVaultHealth/refresh/action | Обновление сведений о работоспособности хранилища |
> | Действие | Microsoft.RecoveryServices/vaults/replicationVaultSettings/read | Чтение любого элемента.  |
> | Действие | Microsoft.RecoveryServices/vaults/replicationVaultSettings/write | Создание или обновление любой  |
> | Действие | Microsoft.RecoveryServices/vaults/replicationvCenters/read | Чтение vCenter |
> | Действие | Microsoft.RecoveryServices/vaults/usages/read | Считывание данных об использовании хранилищ |
> | Действие | Microsoft.RecoveryServices/Vaults/usages/read | Возвращает данные об использовании хранилища служб восстановления. |
> | Действие | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Операцию получения маркера хранилища можно использовать, чтобы получить маркер хранилища для операций серверной части уровня хранилища. |
> | Действие | Microsoft.RecoveryServices/Vaults/write | Создает операцию создания хранилища, которая создает ресурс Azure типа "хранилище". |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Relay/checkNameAvailability/action | Проверяет доступность пространства имен в заданной подписке. |
> | Действие | Microsoft.Relay/checkNamespaceAvailability/action | Проверяет доступность пространства имен в заданной подписке. Этот API не рекомендуется. Вместо него используйте CheckNameAvailability. |
> | Действие | Microsoft.Relay/namespaces/authorizationRules/action | Обновление правила авторизации для пространства имен. Это нерекомендуемый API. Чтобы обновить правило авторизации для пространства имен, используйте вызов PUT. Эта операция не поддерживается для API версии 2017-04-01. |
> | Действие | Microsoft.Relay/namespaces/authorizationRules/delete | Удаляет правило авторизации для пространства имен. Невозможно удалить правило авторизации пространства имен по умолчанию.  |
> | Действие | Microsoft.Relay/namespaces/authorizationRules/listkeys/action | Возвращает строку подключения к пространству имен. |
> | Действие | Microsoft.Relay/namespaces/authorizationRules/read | Возвращает список описаний правил авторизации пространства имен. |
> | Действие | Microsoft.Relay/namespaces/authorizationRules/regenerateKeys/action | Повторно создает первичный или вторичный ключ для ресурса. |
> | Действие | Microsoft.Relay/namespaces/authorizationRules/write | Создает правила авторизации уровня пространства имен и обновляет его свойства. Можно обновить права доступа, первичный ключ и вторичный ключ для правил авторизации. |
> | Действие | Microsoft.Relay/namespaces/Delete | Удаляет ресурс пространства имен. |
> | Действие | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Получение ключей правил авторизации основного пространства имен для аварийного восстановления. |
> | Действие | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/read | Получение правил авторизации основного пространства имен для аварийного восстановления. |
> | Действие | Microsoft.Relay/namespaces/disasterRecoveryConfigs/breakPairing/action | Отключение аварийного восстановления и остановка репликации изменений из основного пространства имен в дополнительное. |
> | Действие | Microsoft.Relay/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Проверка доступности псевдонима пространства имен в заданной подписке. |
> | Действие | Microsoft.Relay/namespaces/disasterRecoveryConfigs/delete | Удаление конфигурации аварийного восстановления, связанной с пространством имен. Эта операция может быть вызвана только через основное пространство имен. |
> | Действие | Microsoft.Relay/namespaces/disasterRecoveryConfigs/failover/action | Вызов отработки отказа географического аварийного восстановления и перенастройка псевдонима пространства имен для указания на дополнительное пространство. |
> | Действие | Microsoft.Relay/namespaces/disasterRecoveryConfigs/read | Получение конфигурации аварийного восстановления, связанной с пространством имен. |
> | Действие | Microsoft.Relay/namespaces/disasterRecoveryConfigs/write | Создание или обновление конфигурации аварийного восстановления, связанной с пространством имен. |
> | Действие | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/action | Операция обновления гибридного подключения. Эта операция не поддерживается для API версии 2017-04-01. Правила авторизации. Чтобы обновить правило авторизации, используйте вызов PUT. |
> | Действие | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/delete | Операция удаления правил авторизации гибридного подключения. |
> | Действие | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/listkeys/action | Возвращает строку подключения для гибридного подключения. |
> | Действие | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/read |  Получение списка правил авторизации гибридного подключения. |
> | Действие | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/regeneratekeys/action | Повторно создает первичный или вторичный ключ для ресурса. |
> | Действие | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/write | Создает правила авторизации гибридного подключения и обновляет его свойства. Можно обновить права доступа к правилам авторизации. |
> | Действие | Microsoft.Relay/namespaces/HybridConnections/Delete | Операция удаления ресурса гибридного подключения. |
> | Действие | Microsoft.Relay/namespaces/HybridConnections/read | Возвращает список описаний ресурсов гибридного подключения. |
> | Действие | Microsoft.Relay/namespaces/HybridConnections/write | Создает или обновляет свойства гибридного подключения. |
> | Действие | Microsoft.Relay/namespaces/messagingPlan/read | Получение плана обмена сообщениями для пространства имен.<br>Это нерекомендуемый API.<br>Свойства, предоставляемые через ресурс MessagingPlan, перемещены в ресурс (родительского) пространства имен в более поздних версиях API.<br>Эта операция не поддерживается для API версии 2017-04-01. |
> | Действие | Microsoft.Relay/namespaces/messagingPlan/write | Обновление плана обмена сообщениями для пространства имен.<br>Это нерекомендуемый API.<br>Свойства, предоставляемые через ресурс MessagingPlan, перемещены в ресурс (родительского) пространства имен в более поздних версиях API.<br>Эта операция не поддерживается для API версии 2017-04-01. |
> | Действие | Microsoft.Relay/namespaces/operationresults/read | Получение состояния операции пространства имен. |
> | Действие | Microsoft.Relay/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Возвращает список описаний ресурсов метрик пространства имен. |
> | Действие | Microsoft.Relay/namespaces/read | Возвращает список описаний ресурсов пространства имен. |
> | Действие | Microsoft.Relay/namespaces/removeAcsNamepsace/action | Удаляет пространство имен ACS. |
> | Действие | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/action | Операция обновления ретранслятора WCF. Эта операция не поддерживается для API версии 2017-04-01. Правила авторизации. Чтобы обновить правило авторизации, используйте вызов PUT. |
> | Действие | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/delete | Операция удаления правил авторизации ретранслятора WCF. |
> | Действие | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/listkeys/action | Возвращает строку подключения к ретранслятору WCF. |
> | Действие | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/read |  Получение списка правил авторизации ретранслятора WCF. |
> | Действие | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/regeneratekeys/action | Повторно создает первичный или вторичный ключ для ресурса. |
> | Действие | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/write | Создает правила авторизации ретранслятора WCF и обновляет его свойства. Можно обновить права доступа к правилам авторизации. |
> | Действие | Microsoft.Relay/namespaces/WcfRelays/Delete | Операция удаления ресурса ретранслятора WCF. |
> | Действие | Microsoft.Relay/namespaces/WcfRelays/read | Возвращает список описаний ресурсов ретранслятора WCF. |
> | Действие | Microsoft.Relay/namespaces/WcfRelays/write | Создает или обновляет свойства ретранслятора WCF. |
> | Действие | Microsoft.Relay/namespaces/write | Создает ресурс пространства имен и обновляет его свойства. К этим свойствам относятся теги и пространство имен. |
> | Действие | Microsoft.Relay/operations/read | Получение операций. |
> | Действие | Microsoft.Relay/register/action | Регистрирует подписку для поставщика ресурсов ретранслятора и позволяет создавать ресурсы ретранслятора. |
> | Действие | Microsoft.Relay/unregister/action | Регистрирует подписку для поставщика ресурсов ретранслятора и позволяет создавать ресурсы ретранслятора. |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.ResourceHealth/AvailabilityStatuses/current/read | Возвращает состояние доступности для указанного ресурса. |
> | Действие | Microsoft.ResourceHealth/AvailabilityStatuses/read | Получает состояния доступности для всех ресурсов в указанной области |
> | Действие | Microsoft.ResourceHealth/events/read | Получение событий работоспособности служб для данной подписки. |
> | Действие | Microsoft.Resourcehealth/healthevent/action | Обозначение изменения состояния работоспособности для указанного ресурса. |
> | Действие | Microsoft.Resourcehealth/healthevent/Activated/action | Обозначение изменения состояния работоспособности для указанного ресурса. |
> | Действие | Microsoft.Resourcehealth/healthevent/InProgress/action | Обозначение изменения состояния работоспособности для указанного ресурса. |
> | Действие | Microsoft.Resourcehealth/healthevent/Pending/action | Обозначение изменения состояния работоспособности для указанного ресурса. |
> | Действие | Microsoft.Resourcehealth/healthevent/Resolved/action | Обозначение изменения состояния работоспособности для указанного ресурса. |
> | Действие | Microsoft.Resourcehealth/healthevent/Updated/action | Обозначение изменения состояния работоспособности для указанного ресурса. |
> | Действие | Microsoft.ResourceHealth/impactedResources/read | Получение затронутых ресурсов для данной подписки. |
> | Действие | Microsoft.ResourceHealth/Operations/read | Получение операций, доступных для ResourceHealth (Майкрософт) |
> | Действие | Microsoft.ResourceHealth/register/action | Регистрация подписки на службу "Работоспособность ресурсов Microsoft". |
> | Действие | Microsoft.ResourceHealth/unregister/action | Отмена регистрации подписки для ResourceHealth (Майкрософт) |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Resources/checkPolicyCompliance/action | Проверка состояния соответствия политикам для указанного ресурса. |
> | Действие | Microsoft.Resources/checkResourceName/action | Проверяет допустимость имени ресурса. |
> | Действие | Microsoft.Resources/deployments/cancel/action | Отменяет развертывание. |
> | Действие | Microsoft.Resources/deployments/delete | Удаляет развертывание. |
> | Действие | Microsoft.Resources/deployments/operations/read | Получает или перечисляет операции развертывания. |
> | Действие | Microsoft.Resources/deployments/read | Получает или перечисляет развертывания. |
> | Действие | Microsoft.Resources/deployments/validate/action | Проверяет развертывание. |
> | Действие | Microsoft.Resources/deployments/whatIf/action | Прогнозирует изменения в развертывании шаблона. |
> | Действие | Microsoft.Resources/deployments/write | Создает или обновляет развертывание. |
> | Действие | Microsoft.Resources/links/delete | Удаляет ссылку на ресурс. |
> | Действие | Microsoft.Resources/links/read | Возвращает ссылки на ресурсы или выводит их список. |
> | Действие | Microsoft.Resources/links/write | Создает или обновляет ссылку на ресурс. |
> | Действие | Microsoft.Resources/marketplace/purchase/action | Покупка ресурса на Marketplace. |
> | Действие | Microsoft.Resources/providers/read | Возвращает список поставщиков. |
> | Действие | Microsoft.Resources/resources/read | Возвращает список ресурсов на основе фильтров. |
> | Действие | Microsoft.Resources/subscriptions/locations/read | Возвращает список поддерживаемых расположений. |
> | Действие | Microsoft.Resources/subscriptions/operationresults/read | Получение результатов операции подписки. |
> | Действие | Microsoft.Resources/subscriptions/providers/read | Возвращает поставщики ресурсов или выводит их список. |
> | Действие | Microsoft.Resources/subscriptions/read | Получает список подписок. |
> | Действие | Microsoft.Resources/subscriptions/resourceGroups/delete | Удаляет группу ресурсов со всеми ресурсами. |
> | Действие | Microsoft.Resources/subscriptions/resourcegroups/deployments/operations/read | Получает или перечисляет операции развертывания. |
> | Действие | Microsoft.Resources/subscriptions/resourcegroups/deployments/operationstatuses/read | Возвращает состояния операций развертывания или выводит их список. |
> | Действие | Microsoft.Resources/subscriptions/resourcegroups/deployments/read | Получает или перечисляет развертывания. |
> | Действие | Microsoft.Resources/subscriptions/resourcegroups/deployments/write | Создает или обновляет развертывание. |
> | Действие | Microsoft.Resources/subscriptions/resourceGroups/moveResources/action | Перемещает ресурсы из одной группы ресурсов в другую. |
> | Действие | Microsoft.Resources/subscriptions/resourceGroups/read | Получает или перечисляет группы ресурсов. |
> | Действие | Microsoft.Resources/subscriptions/resourcegroups/resources/read | Получение ресурсов для группы ресурсов. |
> | Действие | Microsoft.Resources/subscriptions/resourceGroups/validateMoveResources/action | Проверяет перемещение ресурсов из одной группы ресурсов в другую. |
> | Действие | Microsoft.Resources/subscriptions/resourceGroups/write | Создает или обновляет группу ресурсов. |
> | Действие | Microsoft.Resources/subscriptions/resources/read | Возвращает ресурсы подписки. |
> | Действие | Microsoft.Resources/subscriptions/tagNames/delete | Удаляет тег подписки. |
> | Действие | Microsoft.Resources/subscriptions/tagNames/read | Возвращает теги подписки или выводит их список. |
> | Действие | Microsoft.Resources/subscriptions/tagNames/tagValues/delete | Удаляет значение тега подписки. |
> | Действие | Microsoft.Resources/subscriptions/tagNames/tagValues/read | Возвращает значения тегов подписки или выводит их список. |
> | Действие | Microsoft.Resources/subscriptions/tagNames/tagValues/write | Добавляет значение тега подписки. |
> | Действие | Microsoft.Resources/subscriptions/tagNames/write | Добавляет тег подписки. |
> | Действие | Microsoft.Resources/tags/delete | Удаляет все теги на ресурсе. |
> | Действие | Microsoft.Resources/tags/read | Получает все теги на ресурсе. |
> | Действие | Microsoft.Resources/tags/write | Обновляет теги на ресурсе, заменив или слияние существующие теги с новым набором тегов или удаление существующих тегов. |
> | Действие | Microsoft.Resources/tenants/read | Возвращает список клиентов. |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Scheduler/jobcollections/delete | Удаляет коллекцию заданий. |
> | Действие | Microsoft.Scheduler/jobcollections/disable/action | Отключает коллекцию заданий. |
> | Действие | Microsoft.Scheduler/jobcollections/enable/action | Включает коллекцию заданий. |
> | Действие | Microsoft.Scheduler/jobcollections/jobs/delete | Удаляет задание. |
> | Действие | Microsoft.Scheduler/jobcollections/jobs/generateLogicAppDefinition/action | Создает определение приложения логики на основе задания планировщика. |
> | Действие | Microsoft.Scheduler/jobcollections/jobs/jobhistories/read | Получает журнал заданий. |
> | Действие | Microsoft.Scheduler/jobcollections/jobs/read | Возвращает задание. |
> | Действие | Microsoft.Scheduler/jobcollections/jobs/run/action | Выполняет задание. |
> | Действие | Microsoft.Scheduler/jobcollections/jobs/write | Создает или обновляет задание. |
> | Действие | Microsoft.Scheduler/jobcollections/read | Возвращает коллекцию заданий. |
> | Действие | Microsoft.Scheduler/jobcollections/write | Создает или обновляет коллекцию заданий. |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Search/checkNameAvailability/action | Проверяет доступность имени службы. |
> | Действие | Microsoft.Search/operations/read | Выводит список всех доступных операций поставщика Microsoft.Search. |
> | Действие | Microsoft.Search/register/action | Регистрирует подписку для поставщика ресурсов службы поиска и позволяет создавать службы поиска. |
> | Действие | Microsoft.Search/searchServices/createQueryKey/action | Создает ключ запроса. |
> | Действие | Microsoft.Search/searchServices/delete | Удаляет службу поиска. |
> | Действие | Microsoft.Search/searchServices/deleteQueryKey/delete | Удаляет ключ запроса. |
> | Действие | Microsoft.Search/searchServices/listAdminKeys/action | Считывает ключи администратора. |
> | Действие | Microsoft.Search/searchServices/listQueryKeys/read | Возвращает список ключей API запроса для данной службы поиска Azure. |
> | Действие | Microsoft.Search/searchServices/read | Считывает службу поиска. |
> | Действие | Microsoft.Search/searchServices/regenerateAdminKey/action | Повторно создает ключ администратора. |
> | Действие | Microsoft.Search/searchServices/start/action | Запускает службу поиска. |
> | Действие | Microsoft.Search/searchServices/stop/action | Останавливает службу поиска. |
> | Действие | Microsoft.Search/searchServices/write | Создает или обновляет службу поиска. |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Security/adaptiveNetworkHardenings/enforce/action | Применяет заданный трафика, ограниченный режим работы правила, создав сопоставления правил безопасности для данной группы безопасности сети |
> | Действие | Microsoft.Security/adaptiveNetworkHardenings/read | Получает адаптивной Усиление защиты сети рекомендации Azure защищенный ресурс |
> | Действие | Microsoft.Security/advancedThreatProtectionSettings/read | Получает параметры расширенной защиты от угроз для ресурса. |
> | Действие | Microsoft.Security/advancedThreatProtectionSettings/write | Обновляет параметры расширенной защиты от угроз для ресурса. |
> | Действие | Microsoft.Security/alerts/read | Получение всех доступных оповещений системы безопасности. |
> | Действие | Microsoft.Security/applicationWhitelistings/read | Возвращает список разрешений приложения. |
> | Действие | Microsoft.Security/applicationWhitelistings/write | Создает новый список разрешений приложения или обновляет существующий. |
> | Действие | Microsoft.Security/complianceResults/read | Получение результатов соответствия для ресурса. |
> | Действие | Microsoft.Security/informationProtectionPolicies/read | Получение сведений о политиках защиты для ресурса |
> | Действие | Microsoft.Security/informationProtectionPolicies/write | Обновление политик защиты сведений для ресурса |
> | Действие | Microsoft.Security/locations/alerts/activate/action | Активация оповещения системы безопасности. |
> | Действие | Microsoft.Security/locations/alerts/dismiss/action | Отклонение оповещения системы безопасности. |
> | Действие | Microsoft.Security/locations/alerts/read | Получение всех доступных оповещений системы безопасности. |
> | Действие | Microsoft.Security/locations/jitNetworkAccessPolicies/delete | Удаляет политики JIT-доступа к сети. |
> | Действие | Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action | Инициирует запрос политики JIT-доступа к сети. |
> | Действие | Microsoft.Security/locations/jitNetworkAccessPolicies/read | Возвращает политики JIT-доступа к сети. |
> | Действие | Microsoft.Security/locations/jitNetworkAccessPolicies/write | Создает новую политику JIT-доступа к сети или обновляет существующую. |
> | Действие | Microsoft.Security/locations/read | Получение расположения данных безопасности. |
> | Действие | Microsoft.Security/locations/tasks/activate/action | Активирует рекомендацию по безопасности. |
> | Действие | Microsoft.Security/locations/tasks/dismiss/action | Закрывает рекомендацию по безопасности. |
> | Действие | Microsoft.Security/locations/tasks/read | Возвращает все доступные рекомендации по безопасности. |
> | Действие | Microsoft.Security/locations/tasks/resolve/action | Разрешение рекомендации по безопасности. |
> | Действие | Microsoft.Security/locations/tasks/start/action | Запуск рекомендации по безопасности. |
> | Действие | Microsoft.Security/policies/read | Возвращает политику безопасности. |
> | Действие | Microsoft.Security/policies/write | Обновляет политику безопасности. |
> | Действие | Microsoft.Security/pricings/delete | Удаление параметров ценообразования для области. |
> | Действие | Microsoft.Security/pricings/read | Получение параметров ценообразования для области. |
> | Действие | Microsoft.Security/pricings/write | Обновление параметров ценообразования для области. |
> | Действие | Microsoft.Security/register/action | Регистрация подписки в центре безопасности Azure. |
> | Действие | Microsoft.Security/securityContacts/delete | Удаление контактного лица по вопросам безопасности. |
> | Действие | Microsoft.Security/securityContacts/read | Получение контактного лица по вопросам безопасности. |
> | Действие | Microsoft.Security/securityContacts/write | Обновление контактного лица по вопросам безопасности. |
> | Действие | Microsoft.Security/securitySolutions/delete | Удаляет решение безопасности. |
> | Действие | Microsoft.Security/securitySolutions/read | Возвращает решения безопасности. |
> | Действие | Microsoft.Security/securitySolutions/write | Создает новое решение безопасности или обновляет существующее. |
> | Действие | Microsoft.Security/securitySolutionsReferenceData/read | Возвращает эталонные данные решений безопасности. |
> | Действие | Microsoft.Security/securityStatuses/read | Возвращает состояния работоспособности ресурсов Azure. |
> | Действие | Microsoft.Security/securityStatusesSummaries/read | Получение сводок состояния безопасности для области. |
> | Действие | Microsoft.Security/settings/read | Получение параметров для области |
> | Действие | Microsoft.Security/settings/write | Обновление параметров для области |
> | Действие | Microsoft.Security/tasks/read | Возвращает все доступные рекомендации по безопасности. |
> | Действие | Microsoft.Security/unregister/action | Отменяет регистрацию подписки в центре безопасности Azure |
> | Действие | Microsoft.Security/webApplicationFirewalls/delete | Удаляет брандмауэр веб-приложения. |
> | Действие | Microsoft.Security/webApplicationFirewalls/read | Возвращает брандмауэры веб-приложения. |
> | Действие | Microsoft.Security/webApplicationFirewalls/write | Создает новый брандмауэр веб-приложения или обновляет существующий. |
> | Действие | Microsoft.Security/workspaceSettings/connect/action | Изменение параметров повторного подключения к рабочей области. |
> | Действие | Microsoft.Security/workspaceSettings/delete | Удаление параметров рабочей области. |
> | Действие | Microsoft.Security/workspaceSettings/read | Получение параметров рабочей области. |
> | Действие | Microsoft.Security/workspaceSettings/write | Обновление параметров рабочей области. |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.SecurityGraph/diagnosticsettings/delete | Удаление параметра диагностики. |
> | Действие | Microsoft.SecurityGraph/diagnosticsettings/read | Чтение параметра диагностики. |
> | Действие | Microsoft.SecurityGraph/diagnosticsettings/write | Запись параметра диагностики. |
> | Действие | Microsoft.SecurityGraph/diagnosticsettingscategories/read | Чтение категорий параметра диагностики. |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.ServiceBus/checkNameAvailability/action | Проверяет доступность пространства имен в заданной подписке. |
> | Действие | Microsoft.ServiceBus/checkNamespaceAvailability/action | Проверяет доступность пространства имен в заданной подписке. Этот API не рекомендуется. Вместо него используйте CheckNameAvailability. |
> | Действие | Microsoft.ServiceBus/locations/deleteVirtualNetworkOrSubnets/action | Удаление правил виртуальной сети в поставщике ресурсов служебной шины для указанной виртуальной сети |
> | Действие | Microsoft.ServiceBus/namespaces/authorizationRules/action | Обновление правила авторизации для пространства имен. Это нерекомендуемый API. Чтобы обновить правило авторизации для пространства имен, используйте вызов PUT. Эта операция не поддерживается для API версии 2017-04-01. |
> | Действие | Microsoft.ServiceBus/namespaces/authorizationRules/delete | Удаляет правило авторизации для пространства имен. Невозможно удалить правило авторизации пространства имен по умолчанию.  |
> | Действие | Microsoft.ServiceBus/namespaces/authorizationRules/listkeys/action | Возвращает строку подключения к пространству имен. |
> | Действие | Microsoft.ServiceBus/namespaces/authorizationRules/read | Возвращает список описаний правил авторизации пространства имен. |
> | Действие | Microsoft.ServiceBus/namespaces/authorizationRules/regenerateKeys/action | Повторно создает первичный или вторичный ключ для ресурса. |
> | Действие | Microsoft.ServiceBus/namespaces/authorizationRules/write | Создает правила авторизации уровня пространства имен и обновляет его свойства. Можно обновить права доступа, первичный ключ и вторичный ключ для правил авторизации. |
> | Действие | Microsoft.ServiceBus/namespaces/Delete | Удаляет ресурс пространства имен. |
> | Действие | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Получение ключей правил авторизации основного пространства имен для аварийного восстановления. |
> | Действие | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/read | Получение правил авторизации основного пространства имен для аварийного восстановления. |
> | Действие | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/breakPairing/action | Отключение аварийного восстановления и остановка репликации изменений из основного пространства имен в дополнительное. |
> | Действие | Microsoft.ServiceBus/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Проверка доступности псевдонима пространства имен в заданной подписке. |
> | Действие | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/delete | Удаление конфигурации аварийного восстановления, связанной с пространством имен. Эта операция может быть вызвана только через основное пространство имен. |
> | Действие | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/failover/action | Вызов отработки отказа географического аварийного восстановления и перенастройка псевдонима пространства имен для указания на дополнительное пространство. |
> | Действие | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/read | Получение конфигурации аварийного восстановления, связанной с пространством имен. |
> | Действие | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/write | Создание или обновление конфигурации аварийного восстановления, связанной с пространством имен. |
> | Действие | Microsoft.ServiceBus/namespaces/eventGridFilters/delete | Удаление фильтра сетки событий, связанного с пространством имен. |
> | Действие | Microsoft.ServiceBus/namespaces/eventGridFilters/read | Получение фильтра сетки событий, связанного с пространством имен. |
> | Действие | Microsoft.ServiceBus/namespaces/eventGridFilters/write | Создание или обновление фильтра сетки событий, связанного с пространством имен. |
> | Действие | Microsoft.ServiceBus/namespaces/eventhubs/read | Возвращает список описаний ресурсов концентратора событий. |
> | Действие | Microsoft.ServiceBus/namespaces/ipFilterRules/delete | Удаление ресурса фильтра IP-адресов |
> | Действие | Microsoft.ServiceBus/namespaces/ipFilterRules/read | Получение ресурса фильтра IP-адресов |
> | Действие | Microsoft.ServiceBus/namespaces/ipFilterRules/write | Создание ресурса фильтра IP-адресов |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/browse/action | Обзор сообщния |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/defer/action | Отложить сообщений |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/receive/action | Получение сообщений |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/schedule/action | Shedule сообщения |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/send/action | Отправка сообщений |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/setstate/action | Установка состояния сеанса |
> | Действие | Microsoft.ServiceBus/namespaces/messagingPlan/read | Получение плана обмена сообщениями для пространства имен.<br>Это нерекомендуемый API.<br>Свойства, предоставляемые через ресурс MessagingPlan, перемещены в ресурс (родительского) пространства имен в более поздних версиях API.<br>Эта операция не поддерживается для API версии 2017-04-01. |
> | Действие | Microsoft.ServiceBus/namespaces/messagingPlan/write | Обновление плана обмена сообщениями для пространства имен.<br>Это нерекомендуемый API.<br>Свойства, предоставляемые через ресурс MessagingPlan, перемещены в ресурс (родительского) пространства имен в более поздних версиях API.<br>Эта операция не поддерживается для API версии 2017-04-01. |
> | Действие | Microsoft.ServiceBus/namespaces/migrate/action | Операция переноса пространства имен. |
> | Действие | Microsoft.ServiceBus/namespaces/migrationConfigurations/delete | Удаляет конфигурацию миграции. |
> | Действие | Microsoft.ServiceBus/namespaces/migrationConfigurations/read | Получает конфигурацию миграции, показывающую ее состояние, и ожидающие операции репликации |
> | Действие | Microsoft.ServiceBus/namespaces/migrationConfigurations/revert/action | Отменяет миграцию из пространства имен категории "Стандартный" в пространство категории "Премиум" |
> | Действие | Microsoft.ServiceBus/namespaces/migrationConfigurations/upgrade/action | Назначает DNS, связанные с пространством имен категории "Стандартный", пространству категории "Премиум". Эта операция завершает миграцию и останавливает синхронизацию ресурсов из одного пространства в другое |
> | Действие | Microsoft.ServiceBus/namespaces/migrationConfigurations/write | Создает или обновляет конфигурации миграции. Эта операция начнет синхронизацию ресурсов из пространства имен категории "Стандартный" в пространство категории "Премиум" |
> | Действие | Microsoft.ServiceBus/namespaces/networkrulesets/delete | Удаление ресурса правила виртуальной сети |
> | Действие | Microsoft.ServiceBus/namespaces/networkrulesets/read | Возвращает ресурс NetworkRuleSet |
> | Действие | Microsoft.ServiceBus/namespaces/networkrulesets/write | Создание ресурса правила виртуальной сети |
> | Действие | Microsoft.ServiceBus/namespaces/operationresults/read | Получение состояния операции пространства имен. |
> | Действие | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Возвращает список описаний параметров диагностики пространства имен. |
> | Действие | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Возвращает список описаний параметров диагностики пространства имен. |
> | Действие | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/logDefinitions/read | Возвращает список описаний ресурсов журналов пространства имен. |
> | Действие | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Возвращает список описаний ресурсов метрик пространства имен. |
> | Действие | Microsoft.ServiceBus/namespaces/queues/authorizationRules/action | Операция обновления очереди. Эта операция не поддерживается для API версии 2017-04-01. Правила авторизации. Чтобы обновить правило авторизации, используйте вызов PUT. |
> | Действие | Microsoft.ServiceBus/namespaces/queues/authorizationRules/delete | Операция удаления правил авторизации очереди. |
> | Действие | Microsoft.ServiceBus/namespaces/queues/authorizationRules/listkeys/action | Возвращает строку подключения к очереди. |
> | Действие | Microsoft.ServiceBus/namespaces/queues/authorizationRules/read |  Возвращает список правил авторизации очереди. |
> | Действие | Microsoft.ServiceBus/namespaces/queues/authorizationRules/regenerateKeys/action | Повторно создает первичный или вторичный ключ для ресурса. |
> | Действие | Microsoft.ServiceBus/namespaces/queues/authorizationRules/write | Создает правила авторизации очереди и обновляет ее свойства. Можно обновить права доступа к правилам авторизации. |
> | Действие | Microsoft.ServiceBus/namespaces/queues/Delete | Операция удаления ресурса очереди. |
> | Действие | Microsoft.ServiceBus/namespaces/queues/read | Возвращает список описаний ресурсов очереди. |
> | Действие | Microsoft.ServiceBus/namespaces/queues/write | Создает или обновляет свойства очереди. |
> | Действие | Microsoft.ServiceBus/namespaces/read | Возвращает список описаний ресурсов пространства имен. |
> | Действие | Microsoft.ServiceBus/namespaces/removeAcsNamepsace/action | Удаляет пространство имен ACS. |
> | Действие | Microsoft.ServiceBus/namespaces/topics/authorizationRules/action | Операция обновления раздела. Эта операция не поддерживается для API версии 2017-04-01. Правила авторизации. Чтобы обновить правило авторизации, используйте вызов PUT. |
> | Действие | Microsoft.ServiceBus/namespaces/topics/authorizationRules/delete | Операция удаления правил авторизации раздела. |
> | Действие | Microsoft.ServiceBus/namespaces/topics/authorizationRules/listkeys/action | Возвращает строку подключения к разделу. |
> | Действие | Microsoft.ServiceBus/namespaces/topics/authorizationRules/read |  Возвращает список правил авторизации раздела. |
> | Действие | Microsoft.ServiceBus/namespaces/topics/authorizationRules/regenerateKeys/action | Повторно создает первичный или вторичный ключ для ресурса. |
> | Действие | Microsoft.ServiceBus/namespaces/topics/authorizationRules/write | Создает правила авторизации раздела и обновляет его свойства. Можно обновить права доступа к правилам авторизации. |
> | Действие | Microsoft.ServiceBus/namespaces/topics/Delete | Операция удаления ресурса раздела. |
> | Действие | Microsoft.ServiceBus/namespaces/topics/read | Возвращает список описаний ресурсов раздела. |
> | Действие | Microsoft.ServiceBus/namespaces/topics/subscriptions/Delete | Операция удаления ресурса подписки на раздел. |
> | Действие | Microsoft.ServiceBus/namespaces/topics/subscriptions/read | Возвращает список описаний ресурсов подписки на раздел. |
> | Действие | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/Delete | Операция удаления ресурса правила. |
> | Действие | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/read | Возвращает список описаний ресурсов правила. |
> | Действие | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/write | Создает или обновляет свойства правила. |
> | Действие | Microsoft.ServiceBus/namespaces/topics/subscriptions/write | Создает или обновляет свойства подписки на раздел. |
> | Действие | Microsoft.ServiceBus/namespaces/topics/write | Создает или обновляет свойства раздела. |
> | Действие | Microsoft.ServiceBus/namespaces/virtualNetworkRules/delete | Удаление ресурса правила виртуальной сети |
> | Действие | Microsoft.ServiceBus/namespaces/virtualNetworkRules/read | Получение ресурса правила виртуальной сети |
> | Действие | Microsoft.ServiceBus/namespaces/virtualNetworkRules/write | Создание ресурса правила виртуальной сети |
> | Действие | Microsoft.ServiceBus/namespaces/write | Создает ресурс пространства имен и обновляет его свойства. К этим свойствам относятся теги и пространство имен. |
> | Действие | Microsoft.ServiceBus/operations/read | Получение операций. |
> | Действие | Microsoft.ServiceBus/register/action | Регистрирует подписку для поставщика ресурсов служебной шины и позволяет создавать ресурсы служебной шины. |
> | Действие | Microsoft.ServiceBus/sku/read | Получение списка описаний ресурсов номеров SKU. |
> | Действие | Microsoft.ServiceBus/sku/regions/read | Получение списка описаний ресурсов регионов для номеров SKU. |
> | Действие | Microsoft.ServiceBus/unregister/action | Регистрирует подписку для поставщика ресурсов служебной шины и позволяет создавать ресурсы служебной шины. |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.ServiceFabric/clusters/applications/delete | Удаление любого приложения. |
> | Действие | Microsoft.ServiceFabric/clusters/applications/read | Чтение любого приложения. |
> | Действие | Microsoft.ServiceFabric/clusters/applications/services/delete | Удаление любой службы. |
> | Действие | Microsoft.ServiceFabric/clusters/applications/services/partitions/read | Чтение любого раздела. |
> | Действие | Microsoft.ServiceFabric/clusters/applications/services/partitions/replicas/read | Чтение любой реплики. |
> | Действие | Microsoft.ServiceFabric/clusters/applications/services/read | Чтение любой службы. |
> | Действие | Microsoft.ServiceFabric/clusters/applications/services/statuses/read | Чтение состояния любой службы. |
> | Действие | Microsoft.ServiceFabric/clusters/applications/services/write | Создание или обновление любой службы. |
> | Действие | Microsoft.ServiceFabric/clusters/applications/write | Создание или обновление любого приложения. |
> | Действие | Microsoft.ServiceFabric/clusters/applicationTypes/delete | Удаление любого типа приложения. |
> | Действие | Microsoft.ServiceFabric/clusters/applicationTypes/read | Чтение любого типа приложения. |
> | Действие | Microsoft.ServiceFabric/clusters/applicationTypes/versions/delete | Удаление любой версии типа приложения. |
> | Действие | Microsoft.ServiceFabric/clusters/applicationTypes/versions/read | Чтение любой версии типа приложения. |
> | Действие | Microsoft.ServiceFabric/clusters/applicationTypes/versions/write | Создание или обновление любой версии типа приложения. |
> | Действие | Microsoft.ServiceFabric/clusters/applicationTypes/write | Создание или обновление любого типа приложения. |
> | Действие | Microsoft.ServiceFabric/clusters/delete | Удаление любого кластера. |
> | Действие | Microsoft.ServiceFabric/clusters/nodes/read | Чтение любого узла. |
> | Действие | Microsoft.ServiceFabric/clusters/read | Чтение любого кластера. |
> | Действие | Microsoft.ServiceFabric/clusters/statuses/read | Чтение состояния любого кластера. |
> | Действие | Microsoft.ServiceFabric/clusters/write | Создание или обновление любого кластера. |
> | Действие | Microsoft.ServiceFabric/locations/clusterVersions/read | Чтение любой версии кластера. |
> | Действие | Microsoft.ServiceFabric/locations/environments/clusterVersions/read | Чтение любой версии кластера для конкретной среды. |
> | Действие | Microsoft.ServiceFabric/locations/operationresults/read | Чтение результатов операций. |
> | Действие | Microsoft.ServiceFabric/locations/operations/read | Чтение операций по расположению. |
> | Действие | Microsoft.ServiceFabric/operations/read | Чтение доступных операций. |
> | Действие | Microsoft.ServiceFabric/register/action | Регистрация любого действия. |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.SignalRService/locations/checknameavailability/action | Проверяет, доступно ли имя для новой службы SignalR. |
> | Действие | Microsoft.SignalRService/locations/operationresults/signalr/read | Запрос состояния асинхронной операции. |
> | Действие | Microsoft.SignalRService/locations/operationstatus/operationId/read |  |
> | Действие | Microsoft.SignalRService/locations/usages/read | Получение сведений об использовании квот для службы Azure SignalR. |
> | Действие | Microsoft.SignalRService/operationresults/read | Запрос состояния асинхронной операции. |
> | Действие | Microsoft.SignalRService/operationstatus/read |  |
> | Действие | Microsoft.SignalRService/register/action | Регистрирует поставщика ресурсов Microsoft.SignalRService в подписке. |
> | Действие | Microsoft.SignalRService/SignalR/delete | Удаление всей службы SignalR. |
> | Действие | Microsoft.SignalRService/SignalR/listFeatures/read | Вывод списка настраиваемых флагов FeatureFlags службы SignalR. Флаги по умолчанию исключаются. |
> | Действие | Microsoft.SignalRService/SignalR/listkeys/action | Просмотр значений ключей доступа SignalR на портале управления или с помощью API. |
> | Действие | Microsoft.SignalRService/SignalR/read | Просмотр параметров и конфигураций SignalR на портале управления или с помощью API. |
> | Действие | Microsoft.SignalRService/SignalR/regeneratekey/action | Изменение значений ключей доступа SignalR на портале управления или с помощью API. |
> | Действие | Microsoft.SignalRService/SignalR/restart/action | Перезапуск службы Azure SignalR на портале управления или с помощью API. Эта операция вызовет определенный период простоя. |
> | Действие | Microsoft.SignalRService/SignalR/switchFeatures/action | Включение или отключение флагов FeatureFlags службы SignalR с поддерживаемыми свойствами с помощью портала управления или API. |
> | Действие | Microsoft.SignalRService/SignalR/write | Изменяет параметры и конфигурации SignalR на портале управления или с помощью API. |
> | Действие | Microsoft.SignalRService/unregister/action | Отменяет регистрацию поставщика ресурсов Microsoft.SignalRService в подписке. |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Solutions/applicationDefinitions/applicationArtifacts/read | Список артефактов приложения определения приложения. |
> | Действие | Microsoft.Solutions/applicationDefinitions/delete | Удаление определения приложения. |
> | Действие | Microsoft.Solutions/applicationDefinitions/read | Извлечение списка определений приложений. |
> | Действие | Microsoft.Solutions/applicationDefinitions/write | Добавление или изменение определения приложения. |
> | Действие | Microsoft.Solutions/applications/applicationArtifacts/read | Список артефактов приложения. |
> | Действие | Microsoft.Solutions/applications/delete | Удаление приложения. |
> | Действие | Microsoft.Solutions/applications/read | Извлекает список приложений. |
> | Действие | Microsoft.Solutions/applications/refreshPermissions/action | Обновление разрешений приложения. |
> | Действие | Microsoft.Solutions/applications/updateAccess/action | Обновление доступа к приложению. |
> | Действие | Microsoft.Solutions/applications/write | Создает приложение. |
> | Действие | Microsoft.Solutions/jitRequests/delete | Удаление запросов Jit |
> | Действие | Microsoft.Solutions/jitRequests/read | Получение списка запросов Jit |
> | Действие | Microsoft.Solutions/jitRequests/write | Создание запроса JIT |
> | Действие | Microsoft.Solutions/locations/operationStatuses/read | Считывает состояние операции ресурса. |
> | Действие | Microsoft.Solutions/register/action | Регистрация в решениях. |
> | Действие | Microsoft.Solutions/unregister/action | Отменяет регистрацию из решения. |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Sql/checkNameAvailability/action | Проверка того, доступно ли данное имя сервера для подготовки по всему миру для данной подписки. |
> | Действие | Microsoft.Sql/instancePools/delete | Удаление пула экземпляров. |
> | Действие | Microsoft.Sql/instancePools/read | Получение пула экземпляров. |
> | Действие | Microsoft.Sql/instancePools/usages/read | Получает сведения об использовании пул экземпляра |
> | Действие | Microsoft.Sql/instancePools/write | Создание или обновление пула экземпляров. |
> | Действие | Microsoft.Sql/locations/auditingSettingsAzureAsyncOperation/read | Извлечение результата операции настройки расширенной политики аудита больших двоичных объектов на сервере. |
> | Действие | Microsoft.Sql/locations/auditingSettingsOperationResults/read | Извлекает результат операции настройки политики аудита больших двоичных объектов на сервере. |
> | Действие | Microsoft.Sql/locations/capabilities/read | Получение сведений о возможностях этой подписки в данном расположении. |
> | Действие | Microsoft.Sql/locations/databaseAzureAsyncOperation/read | Получение состояния операции базы данных. |
> | Действие | Microsoft.Sql/locations/databaseOperationResults/read | Получение состояния операции базы данных. |
> | Действие | Microsoft.Sql/locations/deletedServerAsyncOperation/read | Возвращение выполняемых операций на удаленном сервере. |
> | Действие | Microsoft.Sql/locations/deletedServerOperationResults/read | Возвращение выполняемых операций на удаленном сервере. |
> | Действие | Microsoft.Sql/locations/deletedServers/read | Возвращение списка удаленных серверов или получение свойств для указанного удаленного сервера. |
> | Действие | Microsoft.Sql/locations/deletedServers/recover/action | Восстановление удаленного сервера. |
> | Действие | Microsoft.Sql/locations/elasticPoolAzureAsyncOperation/read | Возвращение асинхронной операции Azure для эластичного пула. |
> | Действие | Microsoft.Sql/locations/elasticPoolOperationResults/read | Возвращение результата операции эластичного пула. |
> | Действие | Microsoft.Sql/locations/encryptionProtectorAzureAsyncOperation/read | Возвращение выполняемых операций шифрования защиты прозрачного шифрования данных |
> | Действие | Microsoft.Sql/locations/encryptionProtectorOperationResults/read | Возвращение выполняемых операций шифрования защиты прозрачного шифрования данных |
> | Действие | Microsoft.Sql/locations/extendedAuditingSettingsAzureAsyncOperation/read | Извлечение результата операции настройки расширенной политики аудита больших двоичных объектов на сервере. |
> | Действие | Microsoft.Sql/locations/extendedAuditingSettingsOperationResults/read | Извлечение результата операции настройки расширенной политики аудита больших двоичных объектов на сервере. |
> | Действие | Microsoft.Sql/locations/firewallRulesAzureAsyncOperation/read | Получение состояния операции правила брандмауэра. |
> | Действие | Microsoft.Sql/locations/firewallRulesOperationResults/read | Получение состояния операции правила брандмауэра. |
> | Действие | Microsoft.Sql/locations/instanceFailoverGroups/delete | Удаление имеющейся группы отработки отказа экземпляра. |
> | Действие | Microsoft.Sql/locations/instanceFailoverGroups/failover/action | Выполнение плановой отработки отказа в имеющейся группе отработки отказа экземпляра. |
> | Действие | Microsoft.Sql/locations/instanceFailoverGroups/forceFailoverAllowDataLoss/action | Выполнение принудительной отработки отказа в имеющейся группе отработки отказа экземпляра. |
> | Действие | Microsoft.Sql/locations/instanceFailoverGroups/read | Возвращение списка групп отработки отказа экземпляра или свойств для указанной группы экземпляра. |
> | Действие | Microsoft.Sql/locations/instanceFailoverGroups/write | Создает группу отработки отказа экземпляра с указанными параметрами или обновление свойств или тегов для указанного экземпляра группы отработки отказа. |
> | Действие | Microsoft.Sql/locations/instancePoolAzureAsyncOperation/read | Возвращает состояние операции экземпляра пула |
> | Действие | Microsoft.Sql/locations/instancePoolOperationResults/read | Возвращает результат операции экземпляра пула |
> | Действие | Microsoft.Sql/locations/interfaceEndpointProfileAzureAsyncOperation/read | Возвращение сведений об асинхронной операции Azure для определенной конечной точки интерфейса |
> | Действие | Microsoft.Sql/locations/interfaceEndpointProfileOperationResults/read | Возвращение сведений об операции с профилем для определенной конечной точки интерфейса |
> | Действие | Microsoft.Sql/locations/jobAgentAzureAsyncOperation/read | Получает состояние операции агента задания. |
> | Действие | Microsoft.Sql/locations/jobAgentOperationResults/read | Получает результат операции агента задания. |
> | Действие | Microsoft.Sql/locations/longTermRetentionBackups/read | Вывод списка резервных копий на долгосрочном хранении для каждой базы данных на каждом сервере в определенном расположении. |
> | Действие | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionBackups/read | Вывод списка резервных копий на долгосрочном хранении для каждой базы данных на сервере. |
> | Действие | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/delete | Удаление резервной копии на долгосрочном хранении. |
> | Действие | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/read | Вывод списка резервных копий на долгосрочном хранении для базы данных. |
> | Действие | Microsoft.Sql/locations/managedDatabaseRestoreAzureAsyncOperation/completeRestore/action | Завершение операции восстановления управляемой базы данных. |
> | Действие | Microsoft.Sql/locations/managedInstanceEncryptionProtectorAzureAsyncOperation/read | Возвращение выполняемых операций шифрования управляемый экземпляр защиты прозрачного шифрования данных |
> | Действие | Microsoft.Sql/locations/managedInstanceEncryptionProtectorOperationResults/read | Возвращение выполняемых операций шифрования управляемый экземпляр защиты прозрачного шифрования данных |
> | Действие | Microsoft.Sql/locations/managedInstanceKeyAzureAsyncOperation/read | Получает выполняемых операций прозрачного шифрования данных управляемых ключей экземпляра |
> | Действие | Microsoft.Sql/locations/managedInstanceKeyOperationResults/read | Получает выполняемых операций прозрачного шифрования данных управляемых ключей экземпляра |
> | Действие | Microsoft.Sql/locations/managedTransparentDataEncryptionAzureAsyncOperation/read | Возвращение выполняемых операций прозрачного шифрования данных в управляемой базе данных. |
> | Действие | Microsoft.Sql/locations/managedTransparentDataEncryptionOperationResults/read | Возвращение выполняемых операций прозрачного шифрования данных в управляемой базе данных. |
> | Действие | Microsoft.Sql/locations/privateEndpointConnectionProxyAzureAsyncOperation/read | Возвращает результат операции частную конечную точку подключения прокси-сервера |
> | Действие | Microsoft.Sql/locations/privateEndpointConnectionProxyOperationResults/read | Возвращает результат операции частную конечную точку подключения прокси-сервера |
> | Действие | Microsoft.Sql/locations/read | Получение расположений, доступных для данной подписки. |
> | Действие | Microsoft.Sql/locations/serverKeyAzureAsyncOperation/read | Возвращение выполняемых операций с ключами прозрачного шифрования данных сервера |
> | Действие | Microsoft.Sql/locations/serverKeyOperationResults/read | Возвращение выполняемых операций с ключами прозрачного шифрования данных сервера |
> | Действие | Microsoft.Sql/locations/syncAgentOperationResults/read | Получение результата операции ресурса агента синхронизации. |
> | Действие | Microsoft.Sql/locations/syncDatabaseIds/read | Получение идентификаторов базы данных синхронизации для конкретного региона и подписки. |
> | Действие | Microsoft.Sql/locations/syncGroupOperationResults/read | Получение результата операции синхронизации группы. |
> | Действие | Microsoft.Sql/locations/syncMemberOperationResults/read | Получение результата операции синхронизации элемента. |
> | Действие | Microsoft.Sql/locations/usages/read | Получение коллекции метрик использования для этой подписки в расположении. |
> | Действие | Microsoft.Sql/locations/virtualNetworkRulesAzureAsyncOperation/read | Возвращение сведений об асинхронной операции Azure для указанных правил виртуальной сети.  |
> | Действие | Microsoft.Sql/locations/virtualNetworkRulesOperationResults/read | Возвращение сведений об операции для указанных правил виртуальной сети.  |
> | Действие | Microsoft.Sql/managedInstances/administrators/delete | Удаление существующего администратора управляемого экземпляра. |
> | Действие | Microsoft.Sql/managedInstances/administrators/read | Получение списка администраторов управляемого экземпляра. |
> | Действие | Microsoft.Sql/managedInstances/administrators/write | Создание или обновление администратора управляемого экземпляра с указанными параметрами. |
> | Действие | Microsoft.Sql/managedInstances/databases/backupShortTermRetentionPolicies/read | Получает политику краткосрочного хранения для управляемой базы данных |
> | Действие | Microsoft.Sql/managedInstances/databases/backupShortTermRetentionPolicies/write | Обновляет политику краткосрочного хранения для управляемой базы данных |
> | Действие | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/read | Получение списка меток конфиденциальности для конкретной базы данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/delete | Удаление существующей управляемой базы данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/diagnosticSettings/read | Возвращает параметр диагностики для ресурса. |
> | Действие | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/diagnosticSettings/write | Создает или обновляет параметр диагностики для ресурса. |
> | Действие | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/logDefinitions/read | Получение доступных журналов для баз данных управляемых экземпляров |
> | Действие | Microsoft.Sql/managedInstances/databases/read | Получение существующей управляемой базы данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/read | Получение списка меток конфиденциальности для конкретной базы данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/schemas/read | Получите схему управляемой базы данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/read | Получить столбец управляемой базы данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/delete | Удаление метки конфиденциальности данного столбца. |
> | Действие | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/disable/action | Отключить чувствительности рекомендации для данного столбца. |
> | Действие | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/enable/action | Включить рекомендации по конфиденциальности для данного столбца. |
> | Действие | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/read | Получение метки конфиденциальности данного столбца. |
> | Действие | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/write | Создание или обновление метки конфиденциальности данного столбца. |
> | Действие | Microsoft.Sql/managedInstances/databases/schemas/tables/read | Получение таблицы управляемой базы данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/read | Получить список политики обнаружения угроз управляемой базы данных, настроенные для заданного сервера. |
> | Действие | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/write | Изменение политики обнаружения угроз для заданной управляемой базы данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/securityEvents/read | Получение событий безопасности управляемой базы данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/sensitivityLabels/read | Получение списка меток конфиденциальности для конкретной базы данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/read | Получение сведений о прозрачном шифровании данных в данной управляемой базе данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/write | Изменение прозрачного шифрования данных в данной управляемой базе данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/delete | Удаление оценки уязвимости указанной базы данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/read | Получение политик оценки уязвимости для конкретной базы данных |
> | Действие | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/delete | Удаление базовых показателей правила оценки уязвимости для указанной базы данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/read | Получение базовых показателей правила оценки уязвимости для указанной базы данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/write | Изменение базовых показателей правила оценки уязвимости для указанной базы данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/export/action | Преобразование существующего результата проверки в удобочитаемый формат. Если он уже существует, ничего не происходит. |
> | Действие | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/initiateScan/action | Выполняет поиск уязвимостей базы данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/read | Возвращение списка записей сканирования оценки уязвимости базы данных или получение записи для указанного идентификатора сканирования. |
> | Действие | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/write | Изменение оценки уязвимости указанной базы данных. |
> | Действие | Microsoft.Sql/managedInstances/databases/write | Создает новую базу данных или обновляет имеющуюся. |
> | Действие | Microsoft.Sql/managedInstances/delete | Удаление существующего управляемого экземпляра. |
> | Действие | Microsoft.Sql/managedInstances/encryptionProtector/read | Возвращение списка предохранителей шифрования сервера или получение свойств для указанного предохранителя. |
> | Действие | Microsoft.Sql/managedInstances/encryptionProtector/write | Обновление свойств указанного предохранителя шифрования сервера. |
> | Действие | Microsoft.Sql/managedInstances/keys/delete | Удаляет имеющийся ключ Управляемого экземпляра SQL Azure. |
> | Действие | Microsoft.Sql/managedInstances/keys/read | Возвращает список ключей управляемых экземпляров или получает свойства указанного ключа управляемого экземпляра. |
> | Действие | Microsoft.Sql/managedInstances/keys/write | Создает ключ с указанными параметрами или обновляет свойства или теги указанного ключа управляемого экземпляра. |
> | Действие | Microsoft.Sql/managedInstances/metricDefinitions/read | Получение определений метрик управляемого экземпляра. |
> | Действие | Microsoft.Sql/managedInstances/metrics/read | Получение метрик управляемого экземпляра. |
> | Действие | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/diagnosticSettings/read | Возвращает параметр диагностики для ресурса. |
> | Действие | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/diagnosticSettings/write | Создает или обновляет параметр диагностики для ресурса. |
> | Действие | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/logDefinitions/read | Получение доступных журналов для управляемых экземпляров |
> | Действие | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/metricDefinitions/read | Возвращение типов метрик, доступных для управляемых экземпляров |
> | Действие | Microsoft.Sql/managedInstances/read | Возвращение списка управляемых экземпляров или получение свойств указанного управляемого экземпляра. |
> | Действие | Microsoft.Sql/managedInstances/recoverableDatabases/read | Возвращает список доступных для восстановления управляемых баз данных. |
> | Действие | Microsoft.Sql/managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies/read | Получает политику краткосрочного хранения для удаленной управляемой базы данных |
> | Действие | Microsoft.Sql/managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies/write | Обновляет политику краткосрочного хранения для удаленной управляемой базы данных |
> | Действие | Microsoft.Sql/managedInstances/restorableDroppedDatabases/read | Возвращает список доступных для восстановления удаленных управляемых баз данных. |
> | Действие | Microsoft.Sql/managedInstances/securityAlertPolicies/read | Получить список политики обнаружения угроз управляемого сервера, настроенные для заданного сервера. |
> | Действие | Microsoft.Sql/managedInstances/securityAlertPolicies/write | Изменение политики обнаружения угроз на данном управляемом сервере. |
> | Действие | Microsoft.Sql/managedInstances/tdeCertificates/action | Создание или обновление сертификата TDE |
> | Действие | Microsoft.Sql/managedInstances/vulnerabilityAssessments/delete | Удаление оценки уязвимости для указанного управляемого экземпляра |
> | Действие | Microsoft.Sql/managedInstances/vulnerabilityAssessments/read | Получение политик оценки уязвимости для конкретного управляемого экземпляра |
> | Действие | Microsoft.Sql/managedInstances/vulnerabilityAssessments/write | Изменение оценки уязвимости для указанного управляемого экземпляра |
> | Действие | Microsoft.Sql/managedInstances/write | Создание управляемого экземпляра с указанными параметрами либо обновление свойств или тегов указанного управляемого экземпляра. |
> | Действие | Microsoft.Sql/operations/read | Получение доступных операций REST. |
> | Действие | Microsoft.Sql/register/action | Регистрация подписки для поставщика ресурсов базы данных SQL Azure и разрешение создания баз данных Microsoft SQL. |
> | Действие | Microsoft.Sql/servers/administratorOperationResults/read | Возвращение выполняемых операций администраторами серверов. |
> | Действие | Microsoft.Sql/servers/administrators/delete | Удаление администратора сервера. |
> | Действие | Microsoft.Sql/servers/administrators/read | Извлекает сведения об администраторе сервера. |
> | Действие | Microsoft.Sql/servers/administrators/write | Создает или обновляет учетные данные администратора сервера. |
> | Действие | Microsoft.Sql/servers/advisors/read | Возвращает список доступных помощников для сервера. |
> | Действие | Microsoft.Sql/servers/advisors/recommendedActions/read | Возвращает список рекомендуемых действий указанного помощника для сервера. |
> | Действие | Microsoft.Sql/servers/advisors/recommendedActions/write | Применяет рекомендуемое действие к серверу. |
> | Действие | Microsoft.Sql/servers/advisors/write | Обновляет состояние автовыполнения Помощника на уровне сервера. |
> | Действие | Microsoft.Sql/servers/auditingPolicies/read | Извлекает сведения о политике аудита таблиц по умолчанию, настроенной на заданном сервере. |
> | Действие | Microsoft.Sql/servers/auditingPolicies/write | Изменяет политику аудита таблиц по умолчанию для заданного сервера. |
> | Действие | Microsoft.Sql/servers/auditingSettings/operationResults/read | Извлекает результат операции настройки политики аудита больших двоичных объектов на сервере. |
> | Действие | Microsoft.Sql/servers/auditingSettings/read | Извлекает сведения о политике аудита больших двоичных объектов, настроенной на заданном сервере. |
> | Действие | Microsoft.Sql/servers/auditingSettings/write | Изменяет политику аудита больших двоичных объектов для заданного сервера. |
> | Действие | Microsoft.Sql/servers/automaticTuning/read | Возвращение параметров автоматической настройки для сервера. |
> | Действие | Microsoft.Sql/servers/automaticTuning/write | Обновление параметров автоматической настройки для сервера и возвращение обновленных параметров. |
> | Действие | Microsoft.Sql/servers/backupLongTermRetentionVaults/delete | Удаление существующего архивного хранилища резервных копий. |
> | Действие | Microsoft.Sql/servers/backupLongTermRetentionVaults/read | С помощью этой операции можно получить хранилище резервных копий долгосрочного хранения. Возвращает сведения о хранилище, зарегистрированном на данном сервере. |
> | Действие | Microsoft.Sql/servers/backupLongTermRetentionVaults/write | С помощью этой операции можно зарегистрировать на сервере хранилище резервных копий долгосрочного хранения. |
> | Действие | Microsoft.Sql/servers/communicationLinks/delete | Удаление существующего подключения сервера. |
> | Действие | Microsoft.Sql/servers/communicationLinks/read | Возвращение списка подключений для указанного сервера. |
> | Действие | Microsoft.Sql/servers/communicationLinks/write | Создание или обновление подключения сервера. |
> | Действие | Microsoft.Sql/servers/connectionPolicies/read | Возвращение списка политик подключений для указанного сервера. |
> | Действие | Microsoft.Sql/servers/connectionPolicies/write | Создание или обновление политики подключения сервера. |
> | Действие | Microsoft.Sql/servers/databases/advisors/read | Возвращает список доступных помощников для базы данных. |
> | Действие | Microsoft.Sql/servers/databases/advisors/recommendedActions/read | Возвращает список рекомендуемых действий указанного помощника для базы данных. |
> | Действие | Microsoft.Sql/servers/databases/advisors/recommendedActions/write | Применяет рекомендуемое действие к базе данных. |
> | Действие | Microsoft.Sql/servers/databases/advisors/write | Обновляет состояние автовыполнения Помощника на уровне базы данных. |
> | Действие | Microsoft.Sql/servers/databases/auditingPolicies/read | Извлекает сведения о политике аудита таблиц, настроенной для заданной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/auditingPolicies/write | Изменяет политику аудита таблиц для заданной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/auditingSettings/read | Извлекает сведения о политике аудита больших двоичных объектов, настроенной для заданной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/auditingSettings/write | Изменяет политику аудита больших двоичных объектов для заданной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/auditRecords/read | Извлекает записи аудита больших двоичных объектов для базы данных. |
> | Действие | Microsoft.Sql/servers/databases/automaticTuning/read | Возвращение параметров автоматической настройки базы данных. |
> | Действие | Microsoft.Sql/servers/databases/automaticTuning/write | Обновление параметров автоматической настройки базы данных и возвращение обновленных параметров. |
> | Действие | Microsoft.Sql/servers/databases/azureAsyncOperation/read | Получение состояния операции базы данных. |
> | Действие | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/read | Возвращение списка политик хранения резервных копий для указанной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/write | Создание или обновление политики хранения резервных копий базы данных. |
> | Действие | Microsoft.Sql/servers/databases/connectionPolicies/read | Извлекает сведения о политике подключений, настроенной для заданной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/connectionPolicies/write | Изменяет политику подключений для заданной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/currentSensitivityLabels/read | Получение списка меток конфиденциальности для конкретной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/dataMaskingPolicies/read | Получение списка политик маскировки данных базы данных. |
> | Действие | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/delete | Удаление правила политики маскировки данных для заданной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/read | Извлекает сведения о правиле политики маскирования данных, настроенной для заданной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/write | Изменяет правило политики маскирования данных для заданной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/dataMaskingPolicies/write | Изменяет политику маскирования данных для заданной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/dataWarehouseQueries/dataWarehouseQuerySteps/read | Возвращает сведения о шаге распределенного запроса к хранилищу данных для выбранного идентификатора шага. |
> | Действие | Microsoft.Sql/servers/databases/dataWarehouseQueries/read | Возвращает данные запроса на распределение к хранилищу данных для выбранного идентификатора запроса. |
> | Действие | Microsoft.Sql/servers/databases/dataWarehouseUserActivities/read | Получение действий пользователя в экземпляре хранилища данных SQL, включая выполняемые и приостановленные запросы. |
> | Действие | Microsoft.Sql/servers/databases/delete | Удаление имеющейся базы данных. |
> | Действие | Microsoft.Sql/servers/databases/export/action | Экспорт базы данных SQL Azure. |
> | Действие | Microsoft.Sql/servers/databases/extendedAuditingSettings/read | Извлечение сведений о расширенной политике аудита больших двоичных объектов, настроенной для заданной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/extendedAuditingSettings/write | Изменение расширенной политики аудита больших двоичных объектов для заданной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/extensions/read | Получение коллекции расширений для базы данных. |
> | Действие | Microsoft.Sql/servers/databases/extensions/write | Изменение расширения для конкретной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/geoBackupPolicies/read | Получение политик геоизбыточного резервного копирования для конкретной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/geoBackupPolicies/write | Создание или обновление политики геоизбыточного резервного копирования базы данных. |
> | Действие | Microsoft.Sql/servers/databases/importExportOperationResults/read | Получение выполняемых операций импорта и экспорта. |
> | Действие | Microsoft.Sql/servers/databases/maintenanceWindowOptions/read | Получает список доступных окон обслуживания для выбранной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/maintenanceWindows/read | Получает параметры окон обслуживания для выбранной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/maintenanceWindows/write | Задает параметры окон обслуживания для выбранной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/metricDefinitions/read | Возвращает типы метрик, доступных для баз данных. |
> | Действие | Microsoft.Sql/servers/databases/metrics/read | Получение метрик для баз данных. |
> | Действие | Microsoft.Sql/servers/databases/move/action | Измените имя существующей базы данных. |
> | Действие | Microsoft.Sql/servers/databases/operationResults/read | Получение состояния операции базы данных. |
> | Действие | Microsoft.Sql/servers/databases/operations/cancel/action | Отмена приостановленной асинхронной операции базы данных SQL Azure, которая еще не завершена. |
> | Действие | Microsoft.Sql/servers/databases/operations/read | Возвращение списка операций, выполняемых в базе данных. |
> | Действие | Microsoft.Sql/servers/databases/pause/action | Приостановка работы базы данных хранилища данных SQL Azure. |
> | Действие | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/read | Возвращает параметр диагностики для ресурса. |
> | Действие | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/write | Создает или обновляет параметр диагностики для ресурса. |
> | Действие | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/logDefinitions/read | Возвращает доступные журналы для баз данных. |
> | Действие | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/metricDefinitions/read | Возвращает типы метрик, доступных для баз данных. |
> | Действие | Microsoft.Sql/servers/databases/queryStore/queryTexts/read | Возвращение коллекции текстов запросов, которые соответствуют заданным параметрам. |
> | Действие | Microsoft.Sql/servers/databases/queryStore/read | Возвращение текущих значений параметров хранилища запросов для базы данных. |
> | Действие | Microsoft.Sql/servers/databases/queryStore/write | Обновляет параметр хранилища запросов для базы данных. |
> | Действие | Microsoft.Sql/servers/databases/read | Возвращение списка баз данных или возвращение свойств указанной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/read | Получение списка меток конфиденциальности для конкретной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/replicationLinks/delete | Принудительно удаляет отношение репликации. При этом возможна потеря данных. |
> | Действие | Microsoft.Sql/servers/databases/replicationLinks/failover/action | Выполнение отработки отказа после синхронизации всех изменений из базы данных-источника, превращая заданную базу данных в базу данных-источник для отношения репликации, а удаленную базу данных-источник — в базу данных-приемник. |
> | Действие | Microsoft.Sql/servers/databases/replicationLinks/forceFailoverAllowDataLoss/action | Выполнение немедленной отработки отказа с потенциальной потерей данных, превращая заданную базу данных в базу данных-источник для отношения репликации, а удаленную базу данных-источник — в базу данных-приемник. |
> | Действие | Microsoft.Sql/servers/databases/replicationLinks/read | Возвращает список связей репликации или получает свойства для указанных связей репликации. |
> | Действие | Microsoft.Sql/servers/databases/replicationLinks/unlink/action | Удаляет отношение репликации в принудительном порядке или после синхронизации с партнером. |
> | Действие | Microsoft.Sql/servers/databases/replicationLinks/updateReplicationMode/action | Обновляет режим репликации для ссылки на синхронный или асинхронный режим. |
> | Действие | Microsoft.Sql/servers/databases/restorePoints/action | Создает точку восстановления. |
> | Действие | Microsoft.Sql/servers/databases/restorePoints/delete | Удаляет точку восстановления для базы данных. |
> | Действие | Microsoft.Sql/servers/databases/restorePoints/read | Возвращение списка точек восстановления для базы данных. |
> | Действие | Microsoft.Sql/servers/databases/resume/action | Возобновление работы базы данных хранилища данных SQL Azure. |
> | Действие | Microsoft.Sql/servers/databases/schemas/read | Получите схему базы данных. |
> | Действие | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Получите столбец базы данных. |
> | Действие | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/delete | Удаление метки конфиденциальности данного столбца. |
> | Действие | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/disable/action | Отключить чувствительности рекомендации для данного столбца. |
> | Действие | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/enable/action | Включить рекомендации по конфиденциальности для данного столбца. |
> | Действие | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/read | Получение метки конфиденциальности данного столбца. |
> | Действие | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/write | Создание или обновление метки конфиденциальности данного столбца. |
> | Действие | Microsoft.Sql/servers/databases/schemas/tables/read | Получите таблицу базы данных. |
> | Действие | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/read | Извлекает список рекомендаций по индексам для базы данных. |
> | Действие | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/write | Применяет рекомендацию по индексу. |
> | Действие | Microsoft.Sql/servers/databases/securityAlertPolicies/read | Получить список политики обнаружения угроз базы данных, настроенные для заданного сервера. |
> | Действие | Microsoft.Sql/servers/databases/securityAlertPolicies/write | Изменение политики обнаружения угроз для конкретной базы данных |
> | Действие | Microsoft.Sql/servers/databases/securityMetrics/read | Возвращение коллекции метрик безопасности базы данных. |
> | Действие | Microsoft.Sql/servers/databases/sensitivityLabels/read | Получение списка меток конфиденциальности для конкретной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/serviceTierAdvisors/read | Возвращает предложение по масштабированию базы данных на основе статистики выполнения запросов для повышения производительности или снижения затрат. |
> | Действие | Microsoft.Sql/servers/databases/skus/read | Возвращение коллекции доступных номеров SKU для базы данных. |
> | Действие | Microsoft.Sql/servers/databases/syncGroups/cancelSync/action | Отмена синхронизации группы синхронизации. |
> | Действие | Microsoft.Sql/servers/databases/syncGroups/delete | Удаление существующей группы синхронизации. |
> | Действие | Microsoft.Sql/servers/databases/syncGroups/hubSchemas/read | Возвращение списка схем баз данных концентратора синхронизации. |
> | Действие | Microsoft.Sql/servers/databases/syncGroups/logs/read | Возвращение списка журналов группы синхронизации. |
> | Действие | Microsoft.Sql/servers/databases/syncGroups/read | Возвращение списка групп синхронизации или свойств для указанной группы. |
> | Действие | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchema/action | Обновление схемы базы данных концентратора синхронизации. |
> | Действие | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchemaOperationResults/read | Получение результата операции обновления схемы концентратора синхронизации. |
> | Действие | Microsoft.Sql/servers/databases/syncGroups/syncMembers/delete | Удаление существующего элемента синхронизации. |
> | Действие | Microsoft.Sql/servers/databases/syncGroups/syncMembers/read | Возвращение списка элементов синхронизации или свойств для указанного элемента. |
> | Действие | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchema/action | Обновление схемы элемента синхронизации. |
> | Действие | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchemaOperationResults/read | Получение результата операции обновления схемы элемента синхронизации. |
> | Действие | Microsoft.Sql/servers/databases/syncGroups/syncMembers/schemas/read | Возвращение списка схем баз данных элемента синхронизации. |
> | Действие | Microsoft.Sql/servers/databases/syncGroups/syncMembers/write | Создание элемента синхронизации с указанными параметрами либо обновление свойств указанного элемента. |
> | Действие | Microsoft.Sql/servers/databases/syncGroups/triggerSync/action | Активация синхронизации группы синхронизации. |
> | Действие | Microsoft.Sql/servers/databases/syncGroups/write | Создание группы синхронизации с указанными параметрами либо обновление свойств указанной группы. |
> | Действие | Microsoft.Sql/servers/databases/topQueries/queryText/action | Возвращает текст Transact-SQL для выбранного идентификатора запроса. |
> | Действие | Microsoft.Sql/servers/databases/topQueries/read | Возвращает статистические данные среды выполнения для выбранного запроса в указанный временной период. |
> | Действие | Microsoft.Sql/servers/databases/topQueries/statistics/read | Возвращает статистические данные среды выполнения для выбранного запроса в указанный временной период. |
> | Действие | Microsoft.Sql/servers/databases/transparentDataEncryption/operationResults/read | Возвращение выполняемых операций прозрачного шифрования данных. |
> | Действие | Microsoft.Sql/servers/databases/transparentDataEncryption/read | Извлекает состояние и сведения о функции безопасности "прозрачное шифрование данных" для заданной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/transparentDataEncryption/write | Изменение состояния прозрачного шифрования данных. |
> | Действие | Microsoft.Sql/servers/databases/upgradeDataWarehouse/action | Обновление работы базы данных хранилища данных SQL Azure. |
> | Действие | Microsoft.Sql/servers/databases/usages/read | Возвращение сведений об использовании базы данных SQL Azure. |
> | Действие | Microsoft.Sql/servers/databases/vulnerabilityAssessments/delete | Удаление оценки уязвимости указанной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/vulnerabilityAssessments/read | Получение политик оценки уязвимости для конкретной базы данных |
> | Действие | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/delete | Удаление базовых показателей правила оценки уязвимости для указанной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/read | Получение базовых показателей правила оценки уязвимости для указанной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/write | Изменение базовых показателей правила оценки уязвимости для указанной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/export/action | Преобразование существующего результата проверки в удобочитаемый формат. Если он уже существует, ничего не происходит. |
> | Действие | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/initiateScan/action | Выполняет поиск уязвимостей базы данных. |
> | Действие | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/read | Возвращение списка записей сканирования оценки уязвимости базы данных или получение записи для указанного идентификатора сканирования. |
> | Действие | Microsoft.Sql/servers/databases/vulnerabilityAssessments/write | Изменение оценки уязвимости указанной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/action | Выполняет поиск уязвимостей базы данных. |
> | Действие | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/operationResults/read | Получение результата выполнения сканирования оценки уязвимости базы данных. |
> | Действие | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/read | Извлечение сведений об оценке уязвимости, настроенной для заданной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/write | Изменение оценки уязвимости указанной базы данных. |
> | Действие | Microsoft.Sql/servers/databases/write | Создание базы данных с указанными параметрами либо обновление свойств или тегов для указанной базы данных. |
> | Действие | Microsoft.Sql/servers/delete | Удаление существующего сервера. |
> | Действие | Microsoft.Sql/servers/disasterRecoveryConfiguration/delete | Удаление существующих конфигураций аварийного восстановления для указанного сервера. |
> | Действие | Microsoft.Sql/servers/disasterRecoveryConfiguration/failover/action | Отработка отказа конфигурации аварийного восстановления. |
> | Действие | Microsoft.Sql/servers/disasterRecoveryConfiguration/forceFailoverAllowDataLoss/action | Принудительная отработка отказа конфигурации аварийного восстановления. |
> | Действие | Microsoft.Sql/servers/disasterRecoveryConfiguration/read | Получение коллекции конфигураций аварийного восстановления, в которых содержится данный сервер. |
> | Действие | Microsoft.Sql/servers/disasterRecoveryConfiguration/write | Изменение конфигурации аварийного восстановления сервера. |
> | Действие | Microsoft.Sql/servers/elasticPoolEstimates/read | Возвращает список оценок эластичных пулов, уже созданных на этом сервере. |
> | Действие | Microsoft.Sql/servers/elasticPoolEstimates/write | Создает оценку эластичного пула для указанных баз данных. |
> | Действие | Microsoft.Sql/servers/elasticPools/advisors/read | Возвращает список доступных помощников для эластичного пула. |
> | Действие | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/read | Возвращает список рекомендуемых действий указанного помощника для эластичного пула. |
> | Действие | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/write | Применяет рекомендуемое действие к эластичному пулу. |
> | Действие | Microsoft.Sql/servers/elasticPools/advisors/write | Обновляет состояние автовыполнения Помощника на уровне эластичного пула. |
> | Действие | Microsoft.Sql/servers/elasticPools/databases/read | Получение списка баз данных в эластичном пуле. |
> | Действие | Microsoft.Sql/servers/elasticPools/delete | Удаление имеющегося эластичного пула. |
> | Действие | Microsoft.Sql/servers/elasticPools/elasticPoolActivity/read | Извлекает действия и сведения о заданном пуле эластичных баз данных. |
> | Действие | Microsoft.Sql/servers/elasticPools/elasticPoolDatabaseActivity/read | Извлекает действия заданной базы данных, входящей в пул эластичных баз данных, а также сведения о ней. |
> | Действие | Microsoft.Sql/servers/elasticPools/metricDefinitions/read | Возвращает типы метрик, доступных для пулов эластичных баз данных. |
> | Действие | Microsoft.Sql/servers/elasticPools/metrics/read | Возвращение метрик для пулов эластичных баз данных. |
> | Действие | Microsoft.Sql/servers/elasticPools/operations/cancel/action | Отмена приостановленной асинхронной операции эластичного пула SQL Azure, которая еще не завершена. |
> | Действие | Microsoft.Sql/servers/elasticPools/operations/read | Возвращение списка операций, выполняемых в эластичном пуле. |
> | Действие | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/read | Возвращает параметр диагностики для ресурса. |
> | Действие | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/write | Создает или обновляет параметр диагностики для ресурса. |
> | Действие | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/metricDefinitions/read | Возвращает типы метрик, доступных для пулов эластичных баз данных. |
> | Действие | Microsoft.Sql/servers/elasticPools/read | Извлечение сведений о пуле эластичных баз данных для заданного сервера. |
> | Действие | Microsoft.Sql/servers/elasticPools/skus/read | Возвращение коллекции доступных номеров SKU для пула эластичных баз данных. |
> | Действие | Microsoft.Sql/servers/elasticPools/write | Создание эластичного пула или изменение свойств существующего. |
> | Действие | Microsoft.Sql/servers/encryptionProtector/read | Возвращение списка предохранителей шифрования сервера или получение свойств для указанного предохранителя. |
> | Действие | Microsoft.Sql/servers/encryptionProtector/write | Обновление свойств указанного предохранителя шифрования сервера. |
> | Действие | Microsoft.Sql/servers/extendedAuditingSettings/read | Извлечение сведений о расширенной политике аудита больших двоичных объектов, настроенной на заданном сервере. |
> | Действие | Microsoft.Sql/servers/extendedAuditingSettings/write | Изменение расширенного аудита больших двоичных объектов для заданного сервера. |
> | Действие | Microsoft.Sql/servers/failoverGroups/delete | Удаление существующей группы отработки отказа. |
> | Действие | Microsoft.Sql/servers/failoverGroups/failover/action | Выполнение плановой отработки отказа в существующей группе отработки отказа. |
> | Действие | Microsoft.Sql/servers/failoverGroups/forceFailoverAllowDataLoss/action | Выполнение принудительной отработки отказа в существующей группе отработки отказа. |
> | Действие | Microsoft.Sql/servers/failoverGroups/read | Возвращение списка групп отработки отказа или свойств для указанной группы. |
> | Действие | Microsoft.Sql/servers/failoverGroups/write | Создание группы отработки отказа с указанными параметрами либо обновление свойств или тегов указанной группы. |
> | Действие | Microsoft.Sql/servers/firewallRules/delete | Удаление существующего правила брандмауэра сервера. |
> | Действие | Microsoft.Sql/servers/firewallRules/read | Возвращение списка правил брандмауэра сервера или получение свойства для указанного правила. |
> | Действие | Microsoft.Sql/servers/firewallRules/write | Создание правила брандмауэра сервера с указанными параметрами, обновление свойств указанного правила или перезапись всех существующих правил новыми правилами брандмауэра сервера. |
> | Действие | Microsoft.Sql/servers/import/action | Создает базу данных на сервере и развертывает схему и данные из пакета DACPAC. |
> | Действие | Microsoft.Sql/servers/importExportOperationResults/read | Получение выполняемых операций импорта и экспорта. |
> | Действие | Microsoft.Sql/servers/interfaceEndpointProfiles/delete | Удаляет профиль определенной конечной точки интерфейса |
> | Действие | Microsoft.Sql/servers/interfaceEndpointProfiles/read | Возвращает свойства для профиля указанной конечной точки интерфейса |
> | Действие | Microsoft.Sql/servers/interfaceEndpointProfiles/write | Создает профиль конечной точки интерфейса с указанными параметрами либо обновляет свойства или теги указанной конечной точки интерфейса |
> | Действие | Microsoft.Sql/servers/jobAgents/delete | Удаляет агент задания базы данных SQL Azure |
> | Действие | Microsoft.Sql/servers/jobAgents/read | Получает агент задания базы данных SQL Azure |
> | Действие | Microsoft.Sql/servers/jobAgents/write | Создает или изменяет агент задания базы данных SQL Azure |
> | Действие | Microsoft.Sql/servers/keys/delete | Удаление существующего ключа сервера. |
> | Действие | Microsoft.Sql/servers/keys/read | Возвращение списка ключей сервера или возвращение свойств указанного ключа сервера. |
> | Действие | Microsoft.Sql/servers/keys/write | Создание ключа с указанными параметрами либо обновление свойств или тегов указанного ключа сервера. |
> | Действие | Microsoft.Sql/servers/operationResults/read | Возвращение выполняемых операций сервера. |
> | Действие | Microsoft.Sql/servers/privateEndpointConnectionProxies/delete | Удаление существующего прокси-сервера частной конечной точки подключения |
> | Действие | Microsoft.Sql/servers/privateEndpointConnectionProxies/read | Возвращает список частную конечную точку подключения прокси-серверов или получение свойств указанного частную конечную точку подключения прокси-сервера. |
> | Действие | Microsoft.Sql/servers/privateEndpointConnectionProxies/validate/action | Закрытый проверяет подключение к конечной точке создания вызова со стороны поставщика сетевых Ресурсов |
> | Действие | Microsoft.Sql/servers/privateEndpointConnectionProxies/write | Создает учетную запись-посредник частную конечную точку подключения с указанными параметрами или обновляет свойства или теги указанного частную конечную точку подключения прокси-сервера. |
> | Действие | Microsoft.Sql/servers/privateEndpointConnections/delete | Удаляет существующее подключение частной конечной точки |
> | Действие | Microsoft.Sql/servers/privateEndpointConnections/read | Возвращает список соединений частную конечную точку или получение свойств для указанного частную конечную точку подключения. |
> | Действие | Microsoft.Sql/servers/providers/Microsoft.Insights/metricDefinitions/read | Возвращение типов метрик, доступных для серверов. |
> | Действие | Microsoft.Sql/servers/read | Возвращение списка серверов или получение свойств для указанного сервера. |
> | Действие | Microsoft.Sql/servers/recommendedElasticPools/databases/read | Извлекает метрики для рекомендуемых пулов эластичных баз данных для заданного сервера. |
> | Действие | Microsoft.Sql/servers/recommendedElasticPools/read | Извлекает рекомендацию для пулов эластичных баз данных, позволяющую снизить затраты или повысить производительность, на основе журнала использования ресурсов. |
> | Действие | Microsoft.Sql/servers/recoverableDatabases/read | Эта операция используется для аварийного восстановления динамической базы данных. Она позволяет восстановить базу данных до последней известной корректной точки. Возвращение сведений о последней корректной резервной копии, но без восстановления базы данных. |
> | Действие | Microsoft.Sql/servers/replicationLinks/read | Возвращает список связей репликации или получает свойства для указанных связей репликации. |
> | Действие | Microsoft.Sql/servers/restorableDroppedDatabases/read | Получение списка баз данных, которые были удалены на заданном сервере, но остались в политике хранения. |
> | Действие | Microsoft.Sql/servers/securityAlertPolicies/operationResults/read | Извлечение результатов операции записи политики обнаружения угроз на сервере. |
> | Действие | Microsoft.Sql/servers/securityAlertPolicies/read | Получить список политики обнаружения угроз сервера, настроенные для заданного сервера. |
> | Действие | Microsoft.Sql/servers/securityAlertPolicies/write | Изменение политики обнаружения угроз для заданного сервера. |
> | Действие | Microsoft.Sql/servers/serviceObjectives/read | Извлекает список целей уровня обслуживания (также известных как уровни производительности), доступных на данном сервере. |
> | Действие | Microsoft.Sql/servers/syncAgents/delete | Удаление существующего агента синхронизации. |
> | Действие | Microsoft.Sql/servers/syncAgents/generateKey/action | Создание ключа регистрации агента синхронизации. |
> | Действие | Microsoft.Sql/servers/syncAgents/linkedDatabases/read | Получение списка баз данных, связанных с агентом синхронизации. |
> | Действие | Microsoft.Sql/servers/syncAgents/read | Возвращение списка агентов синхронизации или свойств для указанного агента. |
> | Действие | Microsoft.Sql/servers/syncAgents/write | Создание агента синхронизации с указанными параметрами либо обновление свойств указанного агента. |
> | Действие | Microsoft.Sql/servers/tdeCertificates/action | Создание или обновление сертификата TDE |
> | Действие | Microsoft.Sql/servers/usages/read | Возвращает квоту DTU сервера и текущие данные об использовании DTU для всех баз данных на сервере. |
> | Действие | Microsoft.Sql/servers/virtualNetworkRules/delete | Удаление существующего правила виртуальной сети. |
> | Действие | Microsoft.Sql/servers/virtualNetworkRules/read | Возвращение списка правил виртуальной сети или свойств указанного правила виртуальной сети. |
> | Действие | Microsoft.Sql/servers/virtualNetworkRules/write | Создание правила виртуальной сети с указанными параметрами либо обновление свойств или тегов указанного правила виртуальной сети. |
> | Действие | Microsoft.Sql/servers/vulnerabilityAssessments/delete | Удаление оценки уязвимости указанного сервера |
> | Действие | Microsoft.Sql/servers/vulnerabilityAssessments/read | Получение политик оценки уязвимости для конкретного сервера |
> | Действие | Microsoft.Sql/servers/vulnerabilityAssessments/write | Изменение оценки уязвимости указанного сервера |
> | Действие | Microsoft.Sql/servers/write | Создание сервера с указанными параметрами либо обновление свойств или тегов указанного сервера. |
> | Действие | Microsoft.Sql/unregister/action | Отмена регистрации подписки для поставщика ресурсов базы данных SQL Azure с возможностью создания баз данных Microsoft SQL. |
> | Действие | Microsoft.Sql/virtualClusters/delete | Удаляет существующий виртуальный кластер. |
> | Действие | Microsoft.Sql/virtualClusters/read | Возвращение списка виртуальных кластеров или свойств указанного виртуального кластера. |
> | Действие | Microsoft.Sql/virtualClusters/write | Обновление тегов виртуального кластера. |

## <a name="microsoftstorage"></a>Microsoft.Storage;

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Storage/checknameavailability/read | Проверяет, является ли имя учетной записи допустимым и неиспользуемым. |
> | Действие | Microsoft.Storage/locations/deleteVirtualNetworkOrSubnets/action | Уведомляет поставщик ресурсов Microsoft.Storage о том, что удаляется виртуальная сеть или подсеть. |
> | Действие | Microsoft.Storage/locations/usages/read | Возвращает предельное и текущее число используемых ресурсов для указанной подписки. |
> | Действие | Microsoft.Storage/operations/read | Опрашивает состояние асинхронной операции. |
> | Действие | Microsoft.Storage/register/action | Регистрирует подписку для поставщика ресурсов службы хранилища и позволяет создавать учетные записи хранения. |
> | Действие | Microsoft.Storage/skus/read | Выводит список номеров SKU, поддерживаемых Microsoft.Storage. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/add/action | Возвращение результата, полученного при добавлении содержимого большого двоичного объекта. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Возвращение результата, полученного при удалении большого двоичного объекта. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/deleteAutomaticSnapshot/action | Возвращает результат, полученный при удалении автоматического моментального снимка |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/filter/action | Возвращает список больших двоичных объектов в учетной записи с соответствующими фильтру тегами |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Возвращение большого двоичного объекта или списка таких объектов. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags/read | Возвращает результат чтения тегов большого двоичного объекта |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags/write | Возвращает результат записи тегов большого двоичного объекта |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Возвращает результат записи BLOB-объекта |
> | Действие | Microsoft.Storage/storageAccounts/blobServices/containers/clearLegalHold/action | Снятие с контейнера больших двоичных объектов удержания по юридическим причинам. |
> | Действие | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Возвращает результат удаления контейнера |
> | Действие | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/delete | Удаление политики неизменяемости контейнера больших двоичных объектов. |
> | Действие | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/extend/action | Расширение политики неизменяемости контейнера больших двоичных объектов. |
> | Действие | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/lock/action | Блокировка политики неизменяемости контейнера больших двоичных объектов. |
> | Действие | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/read | Получение политики неизменяемости контейнера больших двоичных объектов. |
> | Действие | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/write | Размещение политики неизменяемости контейнера больших двоичных объектов. |
> | Действие | Microsoft.Storage/storageAccounts/blobServices/containers/lease/action | Возвращает результат аренды контейнера больших двоичных объектов. |
> | Действие | Microsoft.Storage/storageAccounts/blobServices/containers/read | Возвращает контейнер. |
> | Действие | Microsoft.Storage/storageAccounts/blobServices/containers/read | Возвращает список контейнеров. |
> | Действие | Microsoft.Storage/storageAccounts/blobServices/containers/setLegalHold/action | Постановка контейнера больших двоичных объектов на удержание по юридическим причинам. |
> | Действие | Microsoft.Storage/storageAccounts/blobServices/containers/write | Возвращает результат обновления контейнера больших двоичных объектов. |
> | Действие | Microsoft.Storage/storageAccounts/blobServices/containers/write | Возвращает результат размещения контейнера больших двоичных объектов. |
> | Действие | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Возвращает ключ делегирования пользователя для службы BLOB-объектов |
> | Действие | Microsoft.Storage/storageAccounts/blobServices/read | Возвращение свойств или статистики службы BLOB-объектов. |
> | Действие | Microsoft.Storage/storageAccounts/blobServices/write | Возвращение результата настройки свойств службы BLOB-объектов. |
> | Действие | Microsoft.Storage/storageAccounts/delete | Удаляет существующую учетную запись хранения. |
> | Действие | Microsoft.Storage/storageAccounts/failover/action | Клиент может управлять отработкой отказа в случае проблем доступности |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/actassuperuser/action |  |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete |  |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/modifypermissions/action |  |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read |  |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write |  |
> | Действие | Microsoft.Storage/storageAccounts/fileServices/read | Получить свойства службы файлов |
> | Действие | Microsoft.Storage/storageAccounts/listAccountSas/action | Возвращает маркер SAS для указанной учетной записи хранения. |
> | Действие | Microsoft.Storage/storageAccounts/listkeys/action | Возвращает ключи доступа для указанной учетной записи хранения. |
> | Действие | Microsoft.Storage/storageAccounts/listServiceSas/action | Возвращение токена SAS службы для указанной учетной записи хранения. |
> | Действие | Microsoft.Storage/storageAccounts/managementPolicies/delete | Удаление политик управления учетными записями хранения |
> | Действие | Microsoft.Storage/storageAccounts/managementPolicies/read | Получение политик управления учетными записями хранения |
> | Действие | Microsoft.Storage/storageAccounts/managementPolicies/write | Задание политик управления учетными записями хранения |
> | Действие | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Возвращение результата удаления очереди. |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action | Возвращение результата, полученного при добавлении сообщения. |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete | Возвращение результата, полученного при удалении сообщения. |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action | Возвращение результата, полученного при обработке сообщения. |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Возвращение сообщения. |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/write | Возвращение результата, полученного при записи сообщения. |
> | Действие | Microsoft.Storage/storageAccounts/queueServices/queues/read | Возвращает очередь или список очередей. |
> | Действие | Microsoft.Storage/storageAccounts/queueServices/queues/write | Возвращение результата записи очереди. |
> | Действие | Microsoft.Storage/storageAccounts/queueServices/read | Получение свойств службы очередей |
> | Действие | Microsoft.Storage/storageAccounts/queueServices/read | Возвращение свойств или статистики службы очередей. |
> | Действие | Microsoft.Storage/storageAccounts/queueServices/write | Возвращение результата настройки свойств службы очередей. |
> | Действие | Microsoft.Storage/storageAccounts/read | Возвращает список учетных записей хранения или получает свойства для указанной учетной записи хранения. |
> | Действие | Microsoft.Storage/storageAccounts/regeneratekey/action | Повторно создает ключи доступа для указанной учетной записи хранения. |
> | Действие | Microsoft.Storage/storageAccounts/revokeUserDelegationKeys/action | Отменяет все ключи делегирования пользователей для указанной учетной записи хранения. |
> | Действие | Microsoft.Storage/storageAccounts/services/diagnosticSettings/write | Создает или обновляет параметры диагностики учетной записи хранения. |
> | Действие | Microsoft.Storage/storageAccounts/tableServices/read | Получить свойства службы таблиц |
> | Действие | Microsoft.Storage/storageAccounts/write | Создает новую учетную запись хранения с указанными параметрами, обновляет свойства или теги указанной существующей учетной записи хранения или добавляет в нее личный домен. |
> | Действие | Microsoft.Storage/usages/read | Возвращает предельное и текущее число используемых ресурсов для указанной подписки. |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | microsoft.storagesync/locations/checkNameAvailability/action | Проверяет, является ли имя службы синхронизации хранилища допустимым и неиспользуемым. |
> | Действие | microsoft.storagesync/locations/workflows/operations/read | Возвращает состояние асинхронной операции. |
> | Действие | microsoft.storagesync/storageSyncServices/delete | Удаление служб синхронизации хранилища. |
> | Действие | microsoft.storagesync/storageSyncServices/providers/Microsoft.Insights/metricDefinitions/read | Получение доступных метрик для служб синхронизации хранилища. |
> | Действие | microsoft.storagesync/storageSyncServices/read | Чтение служб синхронизации хранилища. |
> | Действие | microsoft.storagesync/storageSyncServices/registeredServers/delete | Удаление любого зарегистрированного сервера. |
> | Действие | microsoft.storagesync/storageSyncServices/registeredServers/providers/Microsoft.Insights/metricDefinitions/read | Получает доступные метрики для зарегистрированного сервера. |
> | Действие | microsoft.storagesync/storageSyncServices/registeredServers/read | Чтение зарегистрированного сервера. |
> | Действие | microsoft.storagesync/storageSyncServices/registeredServers/write | Создание или обновление любого зарегистрированного сервера. |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/delete | Удаление конечных точек облака. |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/operationresults/read | Получает состояние асинхронной операции резервного копирования или восстановления |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postbackup/action | Это действие следует вызывать после резервного копирования. |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postrestore/action | Это действие следует вызывать после восстановления. |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prebackup/action | Это действие следует вызывать до резервного копирования. |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prerestore/action | Это действие следует вызывать до восстановления. |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/read | Чтение конечных точек облака. |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/restoreheartbeat/action | Восстановление пакетов пульса. |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/write | Создание или обновление конечных точек облака. |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/delete | Удаление любой группы синхронизации. |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/providers/Microsoft.Insights/metricDefinitions/read | Получает доступные метрики для групп синхронизации. |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/read | Чтение всех групп синхронизации. |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/delete | Удаление любой конечной точки сервера. |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/providers/Microsoft.Insights/metricDefinitions/read | Получает доступные метрики для конечных точек сервера. |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/read | Чтение конечных точек сервера. |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/recallAction/action | Действие для отзыва файлов на сервере. |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/write | Создание или обновление конечных точек сервера. |
> | Действие | microsoft.storagesync/storageSyncServices/syncGroups/write | Создание или обновление групп синхронизации. |
> | Действие | microsoft.storagesync/storageSyncServices/workflows/operationresults/read | Возвращает состояние асинхронной операции. |
> | Действие | microsoft.storagesync/storageSyncServices/workflows/operations/read | Возвращает состояние асинхронной операции. |
> | Действие | microsoft.storagesync/storageSyncServices/workflows/read | Чтение рабочих процессов |
> | Действие | microsoft.storagesync/storageSyncServices/write | Создание или обновление служб синхронизации хранилища. |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.StorSimple/managers/accessControlRecords/delete | Удаляет записи контроля доступа. |
> | Действие | Microsoft.StorSimple/managers/accessControlRecords/operationResults/read | Выводит или возвращает результаты операции. |
> | Действие | Microsoft.StorSimple/managers/accessControlRecords/read | Выводит список записей контроля доступа или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/accessControlRecords/write | Создает или обновляет записи контроля доступа. |
> | Действие | Microsoft.StorSimple/managers/alerts/read | Выводит список оповещений или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/backups/read | Возвращает набор архивации или выводит его. |
> | Действие | Microsoft.StorSimple/managers/bandwidthSettings/delete | Удаляет существующие параметры пропускной способности (только для серии 8000). |
> | Действие | Microsoft.StorSimple/managers/bandwidthSettings/operationResults/read | Выводит результаты операции. |
> | Действие | Microsoft.StorSimple/managers/bandwidthSettings/read | Выводит список параметров пропускной способности (только для серии 8000). |
> | Действие | Microsoft.StorSimple/managers/bandwidthSettings/write | Создает или обновляет параметры пропускной способности (только для серии 8000). |
> | Действие | Microsoft.StorSimple/managers/certificates/write | Создает или обновляет сертификаты. |
> | Действие | Microsoft.StorSimple/Managers/certificates/write | Операция обновления сертификата ресурса обновляет сертификат учетных данных для ресурса или хранилища. |
> | Действие | Microsoft.StorSimple/managers/clearAlerts/action | Очищает все оповещения, связанные с диспетчером устройств. |
> | Действие | Microsoft.StorSimple/managers/cloudApplianceConfigurations/read | Выводит список конфигураций, поддерживаемых облачным устройством. |
> | Действие | Microsoft.StorSimple/managers/configureDevice/action | Настраивает устройство. |
> | Действие | Microsoft.StorSimple/managers/delete | Удаляет диспетчеры устройств. |
> | Действие | Microsoft.StorSimple/Managers/delete | Операция удаления хранилища удаляет указанный ресурс Azure типа "хранилище". |
> | Действие | Microsoft.StorSimple/managers/devices/alertSettings/operationResults/read | Выводит или возвращает результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/alertSettings/read | Возвращает параметры оповещения или выводит их список. |
> | Действие | Microsoft.StorSimple/managers/devices/alertSettings/write | Создает или обновляет параметры оповещения. |
> | Действие | Microsoft.StorSimple/managers/devices/authorizeForServiceEncryptionKeyRollover/action | Разрешает смену ключей шифрования службы на устройствах. |
> | Действие | Microsoft.StorSimple/managers/devices/backupPolicies/backup/action | Выполняет ручную архивацию всех томов, защищенных с помощью политики архивации, для создания их резервной копии по запросу. |
> | Действие | Microsoft.StorSimple/managers/devices/backupPolicies/delete | Удаляет существующие политики архивации (только для серии 8000). |
> | Действие | Microsoft.StorSimple/managers/devices/backupPolicies/operationResults/read | Выводит результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/backupPolicies/read | Выводит список политик архивации (только для серии 8000). |
> | Действие | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/delete | Удаляет существующие расписания. |
> | Действие | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/operationResults/read | Выводит результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/read | Выводит список расписаний. |
> | Действие | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/write | Создает или обновляет расписания. |
> | Действие | Microsoft.StorSimple/managers/devices/backupPolicies/write | Создает или обновляет политики архивации (только для серии 8000). |
> | Действие | Microsoft.StorSimple/managers/devices/backups/delete | Удаляет набор архивации. |
> | Действие | Microsoft.StorSimple/managers/devices/backups/elements/clone/action | Клонирует общедоступный ресурс или том с помощью элемента архива. |
> | Действие | Microsoft.StorSimple/managers/devices/backups/elements/operationResults/read | Выводит или возвращает результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/backups/operationResults/read | Выводит или возвращает результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/backups/read | Возвращает набор архивации или выводит его. |
> | Действие | Microsoft.StorSimple/managers/devices/backups/restore/action | Восстанавливает все тома из набора архивации. |
> | Действие | Microsoft.StorSimple/managers/devices/backupScheduleGroups/delete | Удаляет группы расписаний архивации. |
> | Действие | Microsoft.StorSimple/managers/devices/backupScheduleGroups/operationResults/read | Выводит или возвращает результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/backupScheduleGroups/read | Выводит список групп расписаний архивации или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/backupScheduleGroups/write | Создает или обновляет группы расписаний архивации. |
> | Действие | Microsoft.StorSimple/managers/devices/chapSettings/delete | Удаляет параметры CHAP. |
> | Действие | Microsoft.StorSimple/managers/devices/chapSettings/operationResults/read | Выводит или возвращает результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/chapSettings/read | Возвращает параметры CHAP или выводит их список. |
> | Действие | Microsoft.StorSimple/managers/devices/chapSettings/write | Создает или обновляет параметры CHAP. |
> | Действие | Microsoft.StorSimple/managers/devices/deactivate/action | Деактивирует устройство. |
> | Действие | Microsoft.StorSimple/managers/devices/delete | Удаляет устройства. |
> | Действие | Microsoft.StorSimple/managers/devices/disks/read | Выводит список дисков или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/download/action | Скачивает обновления для устройства. |
> | Действие | Microsoft.StorSimple/managers/devices/failover/action | Выполняет отработку отказа устройства. |
> | Действие | Microsoft.StorSimple/managers/devices/failover/operationResults/read | Выводит или возвращает результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/failoverTargets/read | Выводит или возвращает цели отработки отказа для устройств. |
> | Действие | Microsoft.StorSimple/managers/devices/fileservers/backup/action | Архивирует файловый сервер. |
> | Действие | Microsoft.StorSimple/managers/devices/fileservers/delete | Удаляет файловые серверы. |
> | Действие | Microsoft.StorSimple/managers/devices/fileservers/metrics/read | Выводит список метрик или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/fileservers/metricsDefinitions/read | Выводит список определений метрик или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/fileservers/operationResults/read | Выводит или возвращает результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/fileservers/read | Выводит список файловых серверов или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/fileservers/shares/delete | Удаляет общедоступные ресурсы. |
> | Действие | Microsoft.StorSimple/managers/devices/fileservers/shares/metrics/read | Выводит список метрик или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/fileservers/shares/metricsDefinitions/read | Выводит список определений метрик или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/fileservers/shares/operationResults/read | Выводит или возвращает результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/fileservers/shares/read | Выводит список общедоступных ресурсов или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/fileservers/shares/write | Создает или обновляет общедоступные ресурсы. |
> | Действие | Microsoft.StorSimple/managers/devices/fileservers/write | Создает или обновляет файловые серверы. |
> | Действие | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/changeControllerPowerState/action | Изменяет состояние питания контроллера для групп компонентов оборудования. |
> | Действие | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/operationResults/read | Выводит результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/read | Выводит список групп компонентов оборудования. |
> | Действие | Microsoft.StorSimple/managers/devices/install/action | Устанавливает обновления на устройство. |
> | Действие | Microsoft.StorSimple/managers/devices/installUpdates/action | Устанавливает обновления на устройства (только для серии 8000). |
> | Действие | Microsoft.StorSimple/managers/devices/iscsiservers/backup/action | Архивирует сервер iSCSI. |
> | Действие | Microsoft.StorSimple/managers/devices/iscsiservers/delete | Удаляет серверы iSCSI. |
> | Действие | Microsoft.StorSimple/managers/devices/iscsiservers/disks/delete | Удаляет диски. |
> | Действие | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metrics/read | Выводит список метрик или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metricsDefinitions/read | Выводит список определений метрик или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/iscsiservers/disks/operationResults/read | Выводит или возвращает результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/iscsiservers/disks/read | Выводит список дисков или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/iscsiservers/disks/write | Создает или обновляет диски. |
> | Действие | Microsoft.StorSimple/managers/devices/iscsiservers/metrics/read | Выводит список метрик или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/iscsiservers/metricsDefinitions/read | Выводит список определений метрик или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/iscsiservers/operationResults/read | Выводит или возвращает результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/iscsiservers/read | Выводит список серверов iSCSI или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/iscsiservers/write | Создает или обновляет серверы iSCSI. |
> | Действие | Microsoft.StorSimple/managers/devices/jobs/cancel/action | Отменяет выполнение задания. |
> | Действие | Microsoft.StorSimple/managers/devices/jobs/operationResults/read | Выводит результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/jobs/read | Выводит список заданий или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/listFailoverSets/action | Выводит список групп отработки отказа для существующего устройства (только для серии 8000). |
> | Действие | Microsoft.StorSimple/managers/devices/listFailoverTargets/action | Выводит список целей отработки отказа для устройств (только для серии 8000). |
> | Действие | Microsoft.StorSimple/managers/devices/metrics/read | Выводит список метрик или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/metricsDefinitions/read | Выводит список определений метрик или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/confirmMigration/action | Подтверждает успешный перенос и фиксирует его. |
> | Действие | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/confirmMigrationStatus/read | Выводит состояние подтверждения переноса. |
> | Действие | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchConfirmMigrationStatus/action | Извлекает состояние подтверждения переноса. |
> | Действие | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationEstimate/action | Извлекает состояние задания оценки переноса. |
> | Действие | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationStatus/action | Извлекает состояние переноса. |
> | Действие | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/import/action | Импортирует конфигурации источников для переноса. |
> | Действие | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/migrationEstimate/read | Выводит оценку переноса. |
> | Действие | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/migrationStatus/read | Выводит состояние переноса. |
> | Действие | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/operationResults/read | Выводит результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigration/action | Начинает перенос с использованием конфигураций источников. |
> | Действие | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigrationEstimate/action | Запускает задание для оценки длительности процесса переноса. |
> | Действие | Microsoft.StorSimple/managers/devices/networkSettings/operationResults/read | Выводит результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/networkSettings/read | Возвращает параметры сети или выводит их список. |
> | Действие | Microsoft.StorSimple/managers/devices/networkSettings/write | Создает или обновляет параметры сети. |
> | Действие | Microsoft.StorSimple/managers/devices/operationResults/read | Выводит или возвращает результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/publicEncryptionKey/action | Отображает открытый ключ шифрования для диспетчера устройств. |
> | Действие | Microsoft.StorSimple/managers/devices/publishSupportPackage/action | Публикация пакета поддержки для существующего устройства. Пакет поддержки StorSimple — это простой в использовании механизм, который собирает все соответствующие журналы, помогая службе технической поддержки Майкрософт устранять неполадки в работе устройства StorSimple. |
> | Действие | Microsoft.StorSimple/managers/devices/read | Выводит список устройств или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/scanForUpdates/action | Выполняет поиск обновлений в устройстве. |
> | Действие | Microsoft.StorSimple/managers/devices/securitySettings/operationResults/read | Выводит или возвращает результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/securitySettings/read | Выводит список параметров безопасности. |
> | Действие | Microsoft.StorSimple/managers/devices/securitySettings/syncRemoteManagementCertificate/action | Синхронизирует сертификат удаленного управления для устройства. |
> | Действие | Microsoft.StorSimple/managers/devices/securitySettings/update/action | Обновляет параметры безопасности. |
> | Действие | Microsoft.StorSimple/managers/devices/securitySettings/write | Создает или обновляет параметры безопасности. |
> | Действие | Microsoft.StorSimple/managers/devices/sendTestAlertEmail/action | Отправляет тестовое сообщение оповещения настроенным получателям электронной почты. |
> | Действие | Microsoft.StorSimple/managers/devices/shares/read | Выводит список общедоступных ресурсов или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/devices/timeSettings/operationResults/read | Выводит результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/timeSettings/read | Возвращает параметры времени или выводит их список. |
> | Действие | Microsoft.StorSimple/managers/devices/timeSettings/write | Создает или обновляет параметры времени. |
> | Действие | Microsoft.StorSimple/managers/devices/updates/operationResults/read | Выводит или возвращает результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/updateSummary/read | Возвращает сводку обновлений или выводит их список. |
> | Действие | Microsoft.StorSimple/managers/devices/volumeContainers/delete | Удаляет существующие контейнеры томов (только для серии 8000). |
> | Действие | Microsoft.StorSimple/managers/devices/volumeContainers/metrics/read | Выводит список метрик. |
> | Действие | Microsoft.StorSimple/managers/devices/volumeContainers/metricsDefinitions/read | Выводит список определений метрик. |
> | Действие | Microsoft.StorSimple/managers/devices/volumeContainers/operationResults/read | Выводит результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/volumeContainers/read | Выводит список контейнеров томов (только для серии 8000). |
> | Действие | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/delete | Удаляет существующие тома. |
> | Действие | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metrics/read | Выводит список метрик. |
> | Действие | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metricsDefinitions/read | Выводит список определений метрик. |
> | Действие | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/operationResults/read | Выводит результаты операции. |
> | Действие | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/read | Выводит список томов. |
> | Действие | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/write | Создает или обновляет тома. |
> | Действие | Microsoft.StorSimple/managers/devices/volumeContainers/write | Создает или обновляет контейнеры томов (только для серии 8000). |
> | Действие | Microsoft.StorSimple/managers/devices/volumes/read | Выводит список томов. |
> | Действие | Microsoft.StorSimple/managers/devices/write | Создает или обновляет устройства. |
> | Действие | Microsoft.StorSimple/managers/encryptionSettings/read | Возвращает параметры шифрования или выводит их список. |
> | Действие | Microsoft.StorSimple/managers/extendedInformation/delete | Удаляет расширенные сведения о хранилище. |
> | Действие | Microsoft.StorSimple/Managers/extendedInformation/delete | Операция получения расширенных сведений о хранилище возвращает расширенные сведения об объекте, представляющие ресурс Azure типа "хранилище". |
> | Действие | Microsoft.StorSimple/managers/extendedInformation/read | Выводит или получает расширенные сведения о хранилище. |
> | Действие | Microsoft.StorSimple/Managers/extendedInformation/read | Операция получения расширенных сведений о хранилище возвращает расширенные сведения об объекте, представляющие ресурс Azure типа "хранилище". |
> | Действие | Microsoft.StorSimple/managers/extendedInformation/write | Создает или обновляет расширенные сведения о хранилище. |
> | Действие | Microsoft.StorSimple/Managers/extendedInformation/write | Операция получения расширенных сведений о хранилище возвращает расширенные сведения об объекте, представляющие ресурс Azure типа "хранилище". |
> | Действие | Microsoft.StorSimple/managers/features/read | Выводит список функций. |
> | Действие | Microsoft.StorSimple/managers/fileservers/read | Выводит список файловых серверов или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/getEncryptionKey/action | Возвращает ключ шифрования для диспетчера устройств. |
> | Действие | Microsoft.StorSimple/managers/iscsiservers/read | Выводит список серверов iSCSI или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/jobs/read | Выводит список заданий или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/listActivationKey/action | Возвращает ключ активации диспетчера устройств StorSimple. |
> | Действие | Microsoft.StorSimple/managers/listPublicEncryptionKey/action | Выводит список открытых ключей шифрования для диспетчера устройств StorSimple. |
> | Действие | Microsoft.StorSimple/managers/metrics/read | Выводит список метрик или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/metricsDefinitions/read | Выводит список определений метрик или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/migrateClassicToResourceManager/action | Переход с классической модели на модель Resource Manager. |
> | Действие | Microsoft.StorSimple/managers/migrationSourceConfigurations/read | Вывод списка конфигураций источника переноса (только для серии 8000). |
> | Действие | Microsoft.StorSimple/managers/operationResults/read | Выводит или возвращает результаты операции. |
> | Действие | Microsoft.StorSimple/managers/provisionCloudAppliance/action | Создает облачное устройство. |
> | Действие | Microsoft.StorSimple/managers/read | Выводит список диспетчеров устройств или возвращает их. |
> | Действие | Microsoft.StorSimple/Managers/read | Операция получения хранилища возвращает объект, представляющий ресурс Azure типа "хранилище". |
> | Действие | Microsoft.StorSimple/managers/regenerateActivationKey/action | Повторное создание ключа активации для существующего диспетчера устройств StorSimple. |
> | Действие | Microsoft.StorSimple/managers/storageAccountCredentials/delete | Удаляет данные учетной записи хранения. |
> | Действие | Microsoft.StorSimple/managers/storageAccountCredentials/operationResults/read | Выводит или возвращает результаты операции. |
> | Действие | Microsoft.StorSimple/managers/storageAccountCredentials/read | Выводит список данных учетной записи хранения или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/storageAccountCredentials/write | Создает или обновляет данные учетной записи хранения. |
> | Действие | Microsoft.StorSimple/managers/storageDomains/delete | Удаляет домены хранилища. |
> | Действие | Microsoft.StorSimple/managers/storageDomains/operationResults/read | Выводит или возвращает результаты операции. |
> | Действие | Microsoft.StorSimple/managers/storageDomains/read | Выводит список доменов хранилища или возвращает их. |
> | Действие | Microsoft.StorSimple/managers/storageDomains/write | Создает или обновляет домены хранилища. |
> | Действие | Microsoft.StorSimple/managers/write | Создает или обновляет диспетчеры устройств. |
> | Действие | Microsoft.StorSimple/Managers/write | Создает операцию создания хранилища, которая создает ресурс Azure типа "хранилище". |
> | Действие | Microsoft.StorSimple/operations/read | Выводит или возвращает операции. |
> | Действие | Microsoft.StorSimple/register/action | Регистрирует поставщик Microsoft.StorSimple. |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.StreamAnalytics/locations/quotas/Read | Чтение квоты подписки Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/operations/Read | Чтение операций Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/Register/action | Регистрация подписки в поставщике ресурсов Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/Delete | Удаляет задание Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/functions/Delete | Удаление функции задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/functions/operationresults/Read | Чтение результатов операции функции задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/functions/Read | Чтение функции задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/functions/RetrieveDefaultDefinition/action | Получение определения по умолчанию для функции задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/functions/Test/action | Тестирование функции задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/functions/Write | Запись функции задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/inputs/Delete | Удаляет входные данные задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/inputs/operationresults/Read | Чтение результатов операции для получения входных данных задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/inputs/Read | Считывает входные данные задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/inputs/Sample/action | Пример входных данных задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/inputs/Test/action | Тестирование входных данных задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/inputs/Write | Записывает входные данные задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/metricdefinitions/Read | Чтение определений метрик. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/operationresults/Read | Чтение результатов операции задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/outputs/Delete | Удаляет выходные данные задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/outputs/operationresults/Read | Чтение результатов операции для получения выходных данных задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/outputs/Read | Считывает выходные данные задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/outputs/Test/action | Тестирование выходных данных задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/outputs/Write | Записывает выходные данные задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/read | Считывает параметр диагностики. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/write | Записывает параметр диагностики. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/logDefinitions/read | Возвращает доступные журналы для заданий потоковой передачи. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/metricDefinitions/read | Возвращает доступные метрики для заданий потоковой передачи. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/Read | Считывает задание Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/Start/action | Запускает задание Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/Stop/action | Останавливает задание Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/transformations/Delete | Удаляет преобразование задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/transformations/Read | Считывает преобразование задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/transformations/Write | Записывает преобразование задания Stream Analytics. |
> | Действие | Microsoft.StreamAnalytics/streamingjobs/Write | Записывает задание Stream Analytics. |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Subscription/CreateSubscription/action | Создание подписки Azure. |
> | Действие | Microsoft.Subscription/register/action | Регистрирует подписку в поставщике ресурсов Microsoft.Subscription. |
> | Действие | Microsoft.Subscription/SubscriptionDefinitions/read | Получение определения подписки Azure в группе управления. |
> | Действие | Microsoft.Subscription/SubscriptionDefinitions/write | Создание определения подписки Azure. |

## <a name="microsoftsupport"></a>Microsoft.Support

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.Support/register/action | Выполняет регистрацию в поставщике ресурсов поддержки. |
> | Действие | Microsoft.Support/supportTickets/read | Возвращает подробные сведения о запросе в службу поддержки (включая состояние, серьезность, контактную информацию и сообщения) или возвращает список запросов в службу поддержки в подписках. |
> | Действие | Microsoft.Support/supportTickets/write | Создает или обновляет запрос в службу поддержки. Можно создавать запросы в службу поддержки по техническим проблемам, а также проблемам, связанным с выставлением счетов, квотами или управлением подписками. Вы можете изменить уровень серьезности, контактную информацию и сообщения для существующих запросов в службу поддержки. |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.TimeSeriesInsights/environments/accesspolicies/delete | Удаление политики доступа. |
> | Действие | Microsoft.TimeSeriesInsights/environments/accesspolicies/read | Получение свойств политики доступа. |
> | Действие | Microsoft.TimeSeriesInsights/environments/accesspolicies/write | Создание новой политики доступа для среды или обновление существующей политики. |
> | Действие | Microsoft.TimeSeriesInsights/environments/delete | Удаление среды. |
> | Действие | Microsoft.TimeSeriesInsights/environments/eventsources/delete | Удаление источника события. |
> | Действие | Microsoft.TimeSeriesInsights/environments/eventsources/read | Получение свойств источника событий. |
> | Действие | Microsoft.TimeSeriesInsights/environments/eventsources/write | Создание нового источника событий для среды или обновление существующего. |
> | Действие | Microsoft.TimeSeriesInsights/environments/read | Получение свойств среды. |
> | Действие | Microsoft.TimeSeriesInsights/environments/referencedatasets/delete | Удаление эталонного набора данных. |
> | Действие | Microsoft.TimeSeriesInsights/environments/referencedatasets/read | Получение свойств эталонного набора данных. |
> | Действие | Microsoft.TimeSeriesInsights/environments/referencedatasets/write | Создание нового эталонного набора данных для среды или обновление существующего. |
> | Действие | Microsoft.TimeSeriesInsights/environments/status/read | Получение состояния среды и связанных с ней операций, таких как получение входящего трафика. |
> | Действие | Microsoft.TimeSeriesInsights/environments/write | Создание новой среды или обновление существующей. |
> | Действие | Microsoft.TimeSeriesInsights/register/action | Регистрация подписки для поставщика ресурсов службы "Аналитика временных рядов" с возможностью создания сред данной службы. |

## <a name="microsoftvisualstudio"></a>Microsoft.VisualStudio

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.VisualStudio/Account/Delete | Удаление учетной записи. |
> | Действие | Microsoft.VisualStudio/Account/Extension/Read | Считывает учетную запись или расширение. |
> | Действие | Microsoft.VisualStudio/Account/Project/Read | Считывает учетную запись или проект. |
> | Действие | Microsoft.VisualStudio/Account/Project/Write | Настраивает учетную запись или проект. |
> | Действие | Microsoft.VisualStudio/Account/Read | Чтение учетной записи. |
> | Действие | Microsoft.VisualStudio/Account/Write | Настройка учетной записи. |
> | Действие | Microsoft.VisualStudio/Extension/Delete | Удаление расширения. |
> | Действие | Microsoft.VisualStudio/Extension/Read | Чтение расширения. |
> | Действие | Microsoft.VisualStudio/Extension/Write | Установка расширения. |
> | Действие | Microsoft.VisualStudio/Project/Delete | Удаление проекта. |
> | Действие | Microsoft.VisualStudio/Project/Read | Чтение проекта. |
> | Действие | Microsoft.VisualStudio/Project/Write | Настройка проекта. |
> | Действие | Microsoft.VisualStudio/Register/Action | Регистрирует подписку Azure у поставщика Microsoft.VisualStudio. |

## <a name="microsoftweb"></a>microsoft.web

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | microsoft.web/apimanagementaccounts/apiacls/read | Получение списков ACL для учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/apiacls/delete | Удаление списков ACL для API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/apiacls/read | Получение списков ACL для API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/apiacls/write | Обновление списков ACL для API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/connectionacls/read | Получение списков ACL для подключения API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/connections/confirmconsentcode/action | Подтверждение кода согласия на подключение для API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/delete | Удаление списков ACL для подключения API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/read | Получение списков ACL для подключения API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/write | Обновление списков ACL для подключения API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/connections/delete | Удаление подключений API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/connections/getconsentlinks/action | Получение ссылок на согласие для подключений API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/connections/listconnectionkeys/action | Получение ключей подключения для подключений API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/connections/listsecrets/action | Получение списка секретов для подключений API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/connections/read | Получение подключений API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/connections/write | Обновление подключений API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/delete | Удаление API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/delete | Удаление локализованных определений API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/read | Получение локализованных определений API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/write | Обновление локализованных определений API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/read | Получение API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/apis/write | Обновление API-интерфейсов учетных записей управления API. |
> | Действие | microsoft.web/apimanagementaccounts/connectionacls/read | Получение списков ACL для подключения учетных записей управления API. |
> | Действие | microsoft.web/availablestacks/read | Возвращает доступные стеки. |
> | Действие | Microsoft.Web/certificates/Delete | Удаляет существующий сертификат. |
> | Действие | Microsoft.Web/certificates/Read | Возвращает список сертификатов. |
> | Действие | Microsoft.Web/certificates/Write | Добавляет новый сертификат или обновляет существующий. |
> | Действие | microsoft.web/checknameavailability/read | Проверяет, доступно ли имя ресурса. |
> | Действие | microsoft.web/classicmobileservices/read | Возвращает классические мобильные службы. |
> | Действие | Microsoft.Web/connectionGateways/Delete | Удаляет шлюз подключения. |
> | Действие | Microsoft.Web/connectionGateways/Join/Action | Присоединяет шлюз подключения. |
> | Действие | Microsoft.Web/connectionGateways/ListStatus/Action | Получение состояния шлюза подключения. |
> | Действие | Microsoft.Web/connectionGateways/Move/Action | Перемещение шлюза подключения. |
> | Действие | Microsoft.Web/connectionGateways/Read | Возвращает список шлюзов подключений. |
> | Действие | Microsoft.Web/connectionGateways/Write | Создает или обновляет шлюз для подключения. |
> | Действие | microsoft.web/connections/confirmconsentcode/action | Подтверждает код согласия на подключение. |
> | Действие | Microsoft.Web/connections/Delete | Удаляет подключение. |
> | Действие | Microsoft.Web/connections/Join/Action | Присоединяет подключение. |
> | Действие | microsoft.web/connections/listconsentlinks/action | Выводит список ссылок на согласие для подключений. |
> | Действие | Microsoft.Web/connections/Move/Action | Перемещение подключения. |
> | Действие | Microsoft.Web/connections/Read | Возвращает список подключений. |
> | Действие | Microsoft.Web/connections/Write | Создает или обновляет подключение. |
> | Действие | Microsoft.Web/customApis/Delete | Удаление пользовательского API. |
> | Действие | Microsoft.Web/customApis/extractApiDefinitionFromWsdl/Action | Извлечение определения API из формата WSDL. |
> | Действие | Microsoft.Web/customApis/Join/Action | Присоединение пользовательского API. |
> | Действие | Microsoft.Web/customApis/listWsdlInterfaces/Action | Получение списка интерфейсов языка WSDL для пользовательского API. |
> | Действие | Microsoft.Web/customApis/Move/Action | Перемещение пользовательского API. |
> | Действие | Microsoft.Web/customApis/Read | Получение списка пользовательских API. |
> | Действие | Microsoft.Web/customApis/Write | Создание или обновление пользовательского API. |
> | Действие | Microsoft.Web/deletedSites/Read | Возвращение свойств удаленного веб-приложения. |
> | Действие | microsoft.web/deploymentlocations/read | Возвращает расположения развертываний. |
> | Действие | Microsoft.Web/geoRegions/Read | Возвращает список географических регионов. |
> | Действие | microsoft.web/hostingenvironments/capacities/read | Возвращает емкости сред внешнего размещения. |
> | Действие | Microsoft.Web/hostingEnvironments/Delete | Удаляет среду службы приложений. |
> | Действие | microsoft.web/hostingenvironments/detectors/read | Возвращение средств обнаружения для сред внешнего размещения. |
> | Действие | microsoft.web/hostingenvironments/diagnostics/read | Возвращает данные диагностики сред внешнего размещения. |
> | Действие | microsoft.web/hostingenvironments/inboundnetworkdependenciesendpoints/read | Получение конечных точек сети всех входящих зависимостей. |
> | Действие | Microsoft.Web/hostingEnvironments/Join/Action | Присоединяет среды службы приложений |
> | Действие | microsoft.web/hostingenvironments/metricdefinitions/read | Возвращает определения метрик сред внешнего размещения. |
> | Действие | microsoft.web/hostingenvironments/multirolepools/metricdefinitions/read | Возвращает определения метрик пулов множественных ролей в средах внешнего размещения. |
> | Действие | microsoft.web/hostingenvironments/multirolepools/metrics/read | Возвращает метрики пулов множественных ролей в средах внешнего размещения. |
> | Действие | Microsoft.Web/hostingEnvironments/multiRolePools/Read | Возвращает свойства интерфейсного пула в среде службы приложений. |
> | Действие | microsoft.web/hostingenvironments/multirolepools/skus/read | Возвращает номера SKU пулов множественных ролей в средах внешнего размещения. |
> | Действие | microsoft.web/hostingenvironments/multirolepools/usages/read | Возвращает данные об использовании пулов множественных ролей в средах внешнего размещения. |
> | Действие | Microsoft.Web/hostingEnvironments/multiRolePools/Write | Создает новый интерфейсный пул в среде службы приложений или обновляет существующий. |
> | Действие | microsoft.web/hostingenvironments/operations/read | Возвращает операции сред внешнего размещения. |
> | Действие | microsoft.web/hostingenvironments/outboundnetworkdependenciesendpoints/read | Получение конечных точек сети всех исходящих зависимостей. |
> | Действие | Microsoft.Web/hostingEnvironments/PrivateEndpointConnectionsApproval/action | Утвердить подключения Private конечных точек |
> | Действие | Microsoft.Web/hostingEnvironments/Read | Возвращает свойства среды службы приложений. |
> | Действие | Microsoft.Web/hostingEnvironments/reboot/Action | Перезагружает все компьютеры в среде службы приложений. |
> | Действие | microsoft.web/hostingenvironments/resume/action | Возобновляет работу сред внешнего размещения. |
> | Действие | microsoft.web/hostingenvironments/serverfarms/read | Возвращает планы службы приложений в средах внешнего размещения. |
> | Действие | microsoft.web/hostingenvironments/sites/read | Возвращает веб-приложения в средах внешнего размещения. |
> | Действие | microsoft.web/hostingenvironments/suspend/action | Приостанавливает работу сред внешнего размещения. |
> | Действие | microsoft.web/hostingenvironments/usages/read | Возвращает данные об использовании сред внешнего размещения. |
> | Действие | microsoft.web/hostingenvironments/workerpools/metricdefinitions/read | Возвращает определения метрик рабочих пулов в средах внешнего размещения. |
> | Действие | microsoft.web/hostingenvironments/workerpools/metrics/read | Возвращает метрики рабочих пулов в средах внешнего размещения. |
> | Действие | Microsoft.Web/hostingEnvironments/workerPools/Read | Возвращает свойства рабочего пула в среде службы приложений. |
> | Действие | microsoft.web/hostingenvironments/workerpools/skus/read | Возвращает номера SKU рабочих пулов в средах внешнего размещения. |
> | Действие | microsoft.web/hostingenvironments/workerpools/usages/read | Возвращает данные об использовании рабочих пулов в средах внешнего размещения. |
> | Действие | Microsoft.Web/hostingEnvironments/workerPools/Write | Создает новый рабочий пул в среде службы приложений или обновляет существующий. |
> | Действие | Microsoft.Web/hostingEnvironments/Write | Создает новую среду службы приложений или обновляет существующую. |
> | Действие | microsoft.web/ishostingenvironmentnameavailable/read | Проверяет, доступно ли имя среды внешнего размещения. |
> | Действие | microsoft.web/ishostnameavailable/read | Проверяет, доступно ли имя узла. |
> | Действие | microsoft.web/isusernameavailable/read | Проверяет, доступно ли имя пользователя. |
> | Действие | Microsoft.Web/listSitesAssignedToHostName/Read | Возвращает имена сайтов, назначенные имени узла. |
> | Действие | microsoft.web/locations/apioperations/read | Возвращает операции API расположений. |
> | Действие | microsoft.web/locations/connectiongatewayinstallations/read | Возвращает данные об установленных шлюзах подключений в расположениях. |
> | Действие | microsoft.web/locations/deleteVirtualNetworkOrSubnets/action | Уведомление об удалении виртуальной сети или подсети для расположений. |
> | Действие | microsoft.web/locations/extractapidefinitionfromwsdl/action | Извлечение определения API для расположений из синтаксиса WSDL. |
> | Действие | microsoft.web/locations/listwsdlinterfaces/action | Получение списка интерфейсов языка WSDL для расположений. |
> | Действие | microsoft.web/locations/managedapis/apioperations/read | Получение операций с управляемыми API для расположений. |
> | Действие | Microsoft.Web/locations/managedapis/Join/Action | Присоединение управляемого API. |
> | Действие | microsoft.web/locations/managedapis/read | Возвращает расположения управляемых интерфейсов API. |
> | Действие | microsoft.web/operations/read | Возвращает операции. |
> | Действие | microsoft.web/publishingusers/read | Возвращает публикующих пользователей. |
> | Действие | microsoft.web/publishingusers/write | Обновляет публикующих пользователей. |
> | Действие | Microsoft.Web/recommendations/Read | Возвращает список рекомендаций для подписок. |
> | Действие | microsoft.web/register/action | Регистрирует поставщик ресурсов Microsoft.Web для подписки. |
> | Действие | microsoft.web/resourcehealthmetadata/read | Получение метаданных работоспособности ресурса. |
> | Действие | microsoft.web/serverfarms/capabilities/read | Возвращает возможности планов службы приложений. |
> | Действие | Microsoft.Web/serverfarms/Delete | Удаление существующего плана службы приложений |
> | Действие | microsoft.web/serverfarms/firstpartyapps/settings/delete | Удаляет параметры основных приложений в планах службы приложений. |
> | Действие | microsoft.web/serverfarms/firstpartyapps/settings/read | Возвращает параметры основных приложений в планах службы приложений. |
> | Действие | microsoft.web/serverfarms/firstpartyapps/settings/write | Обновляет параметры основных приложений в планах службы приложений. |
> | Действие | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/delete | Удаление ретрансляторов в пространствах имен гибридных подключений для плана службы приложений. |
> | Действие | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/read | Получение ретрансляторов в пространствах имен гибридных подключений для плана службы приложений. |
> | Действие | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/sites/read | Возвращает веб-приложения ретрансляторов в пространствах имен гибридных подключений для плана службы приложений. |
> | Действие | microsoft.web/serverfarms/hybridconnectionplanlimits/read | Возвращает ограничения плана гибридных подключений для плана службы приложений. |
> | Действие | microsoft.web/serverfarms/hybridconnectionrelays/read | Возвращает ретрансляторы гибридных подключений для планов службы приложений. |
> | Действие | microsoft.web/serverfarms/metricdefinitions/read | Возвращает определения метрик планов службы приложений. |
> | Действие | microsoft.web/serverfarms/metrics/read | Возвращает метрики планов службы приложений. |
> | Действие | microsoft.web/serverfarms/operationresults/read | Возвращает результаты операций планов службы приложений. |
> | Действие | Microsoft.Web/serverfarms/Read | Возвращает свойства плана службы приложений. |
> | Действие | Microsoft.Web/serverfarms/restartSites/Action | Перезапустите все веб-приложения в плане службы приложений. |
> | Действие | microsoft.web/serverfarms/sites/read | Возвращает веб-приложения в планах службы приложений. |
> | Действие | microsoft.web/serverfarms/skus/read | Возвращает номера SKU в планах службы приложений. |
> | Действие | microsoft.web/serverfarms/usages/read | Возвращает данные об использовании для планов службы приложений. |
> | Действие | microsoft.web/serverfarms/virtualnetworkconnections/gateways/write | Обновляет шлюзы подключений виртуальной сети для планов службы приложений. |
> | Действие | microsoft.web/serverfarms/virtualnetworkconnections/read | Возвращает подключения виртуальной сети для планов службы приложений. |
> | Действие | microsoft.web/serverfarms/virtualnetworkconnections/routes/delete | Удаляет маршруты подключений виртуальной сети для планов службы приложений. |
> | Действие | microsoft.web/serverfarms/virtualnetworkconnections/routes/read | Возвращает маршруты подключений виртуальной сети для планов службы приложений. |
> | Действие | microsoft.web/serverfarms/virtualnetworkconnections/routes/write | Обновляет маршруты подключений виртуальной сети для планов службы приложений. |
> | Действие | microsoft.web/serverfarms/workers/reboot/action | Перезапускает рабочие роли планов службы приложений. |
> | Действие | Microsoft.Web/serverfarms/Write | Создает новый план службы приложений или обновляет существующий. |
> | Действие | microsoft.web/sites/analyzecustomhostname/read | Анализирует пользовательское имя узла. |
> | Действие | Microsoft.Web/sites/applySlotConfig/Action | Применяет конфигурацию слота веб-приложения из целевого слота к текущему веб-приложению. |
> | Действие | Microsoft.Web/sites/backup/Action | Создает резервную копию веб-приложения. |
> | Действие | microsoft.web/sites/backup/read | Возвращает резервную копию веб-приложений. |
> | Действие | microsoft.web/sites/backup/write | Обновляет резервную копию веб-приложений. |
> | Действие | microsoft.web/sites/backups/action | Обнаружение имеющейся резервной копии приложения, которую можно восстановить из большого двоичного объекта в хранилище Azure. |
> | Действие | microsoft.web/sites/backups/delete | Удаление резервных копий веб-приложений. |
> | Действие | microsoft.web/sites/backups/list/action | Выводит список резервных копий веб-приложений. |
> | Действие | Microsoft.Web/sites/backups/Read | Возвращает свойства резервной копии веб-приложения. |
> | Действие | microsoft.web/sites/backups/restore/action | Восстанавливает резервные копии веб-приложений. |
> | Действие | microsoft.web/sites/backups/write | Обновление резервных копий веб-приложений. |
> | Действие | microsoft.web/sites/config/delete | Удаляет конфигурацию веб-приложений. |
> | Действие | Microsoft.Web/sites/config/list/Action | Отображает влияющие на безопасность параметры веб-приложения, такие как учетные данные для публикации, параметры приложения и строки подключения. |
> | Действие | Microsoft.Web/sites/config/Read | Возвращает параметры конфигурации веб-приложения. |
> | Действие | microsoft.web/sites/config/snapshots/read | Получение моментальных снимков конфигурации веб-приложений. |
> | Действие | Microsoft.Web/sites/config/Write | Обновляет параметры конфигурации веб-приложения. |
> | Действие | microsoft.web/sites/containerlogs/action | Получение сжатых журналов контейнеров для веб-приложений. |
> | Действие | microsoft.web/sites/containerlogs/download/action | Скачивание журналов контейнера веб-приложений. |
> | Действие | microsoft.web/sites/continuouswebjobs/delete | Удаляет непрерывные веб-задания веб-приложений. |
> | Действие | microsoft.web/sites/continuouswebjobs/read | Возвращает непрерывные веб-задания веб-приложений. |
> | Действие | microsoft.web/sites/continuouswebjobs/start/action | Запускает непрерывные веб-задания веб-приложений. |
> | Действие | microsoft.web/sites/continuouswebjobs/stop/action | Останавливает непрерывные веб-задания веб-приложений. |
> | Действие | Microsoft.Web/sites/Delete | Удаление существующего веб-приложения |
> | Действие | microsoft.web/sites/deployments/delete | Удаляет развертывания веб-приложений. |
> | Действие | microsoft.web/sites/deployments/log/read | Возвращает журнал развертываний веб-приложений. |
> | Действие | microsoft.web/sites/deployments/read | Возвращает развертывания веб-приложений. |
> | Действие | microsoft.web/sites/deployments/write | Обновляет развертывания веб-приложений. |
> | Действие | microsoft.web/sites/detectors/read | Получение средств обнаружения для веб-приложений. |
> | Действие | microsoft.web/sites/diagnostics/analyses/execute/Action | Запуск анализа данных диагностики веб-приложений. |
> | Действие | microsoft.web/sites/diagnostics/analyses/read | Получение анализа данных диагностики веб-приложений. |
> | Действие | microsoft.web/sites/diagnostics/aspnetcore/read | Получение данных диагностики веб-приложений для приложения ASP.NET Core. |
> | Действие | microsoft.web/sites/diagnostics/autoheal/read | Получение данных диагностики о функции Autoheal для веб-приложений. |
> | Действие | microsoft.web/sites/diagnostics/deployment/read | Получение данных диагностики о развертывании для веб-приложений. |
> | Действие | microsoft.web/sites/diagnostics/deployments/read | Получение данных диагностики о развертываниях для веб-приложений. |
> | Действие | microsoft.web/sites/diagnostics/detectors/execute/Action | Запуск средства обнаружения диагностики веб-приложений. |
> | Действие | microsoft.web/sites/diagnostics/detectors/read | Получение данных средства обнаружения диагностики веб-приложений. |
> | Действие | microsoft.web/sites/diagnostics/failedrequestsperuri/read | Получение данных диагностики веб-приложений о неудачных запросах для каждого URI. |
> | Действие | microsoft.web/sites/diagnostics/frebanalysis/read | Возвращает данные диагностики об анализе FREB для веб-приложений. |
> | Действие | microsoft.web/sites/diagnostics/loganalyzer/read | Получение данных диагностики веб-приложений об анализаторе журналов. |
> | Действие | microsoft.web/sites/diagnostics/read | Получение категорий данных диагностики веб-приложений. |
> | Действие | microsoft.web/sites/diagnostics/runtimeavailability/read | Возвращает данные диагностики о доступности среды выполнения для веб-приложений. |
> | Действие | microsoft.web/sites/diagnostics/servicehealth/read | Возвращает данные диагностики о работоспособности служб для веб-приложений. |
> | Действие | microsoft.web/sites/diagnostics/sitecpuanalysis/read | Получение данных диагностики веб-приложений об анализе ЦП для сайта. |
> | Действие | microsoft.web/sites/diagnostics/sitecrashes/read | Получение данных диагностики веб-приложений о сбоях сайта. |
> | Действие | microsoft.web/sites/diagnostics/sitelatency/read | Получение данных диагностики веб-приложений о задержках сайта. |
> | Действие | microsoft.web/sites/diagnostics/sitememoryanalysis/read | Получение данных диагностики веб-приложений об анализе памяти для сайта. |
> | Действие | microsoft.web/sites/diagnostics/siterestartsettingupdate/read | Получение данных диагностики веб-приложений об обновлении параметра перезапуска сайта. |
> | Действие | microsoft.web/sites/diagnostics/siterestartuserinitiated/read | Получение данных диагностики веб-приложений об инициированном пользователем перезапуске сайта. |
> | Действие | microsoft.web/sites/diagnostics/siteswap/read | Получение данных диагностики веб-приложений о переключении сайта. |
> | Действие | microsoft.web/sites/diagnostics/threadcount/read | Получение данных диагностики веб-приложений о количестве потоков. |
> | Действие | microsoft.web/sites/diagnostics/workeravailability/read | Возвращает данные диагностики о доступности рабочих процессов для веб-приложений. |
> | Действие | microsoft.web/sites/diagnostics/workerprocessrecycle/read | Возвращает данные диагностики перезапуска рабочих процессов для веб-приложений. |
> | Действие | microsoft.web/sites/domainownershipidentifiers/read | Возвращает идентификаторы владения доменом веб-приложений. |
> | Действие | microsoft.web/sites/domainownershipidentifiers/write | Обновляет идентификаторы владения доменом веб-приложений. |
> | Действие | microsoft.web/sites/extensions/delete | Удаление расширений сайта веб-приложений. |
> | Действие | microsoft.web/sites/extensions/read | Получение расширений сайта веб-приложений. |
> | Действие | microsoft.web/sites/extensions/write | Обновление расширений сайта веб-приложений. |
> | Действие | microsoft.web/sites/functions/action | Выполняет функции веб-приложений. |
> | Действие | microsoft.web/sites/functions/delete | Удаляет функции веб-приложений. |
> | Действие | microsoft.web/sites/functions/listsecrets/action | Выводит список секретов для функций веб-приложений. |
> | Действие | microsoft.web/sites/functions/masterkey/read | Получение главного ключа функций веб-приложений. |
> | Действие | microsoft.web/sites/functions/read | Возвращает функции веб-приложений. |
> | Действие | microsoft.web/sites/functions/token/read | Получение токена функций веб-приложений. |
> | Действие | microsoft.web/sites/functions/write | Обновляет функции веб-приложений. |
> | Действие | microsoft.web/sites/hostnamebindings/delete | Удаляет привязки имен узлов для веб-приложений. |
> | Действие | microsoft.web/sites/hostnamebindings/read | Возвращает привязки имен узлов для веб-приложений. |
> | Действие | microsoft.web/sites/hostnamebindings/write | Обновляет привязки имен узлов для веб-приложений. |
> | Действие | microsoft.web/sites/hostruntime/functions/keys/read | Получение ключей функций среды выполнения узла веб-приложений. |
> | Действие | Microsoft.Web/sites/hostruntime/host/_master/read | Получение главного ключа приложения-функции для операций администрирования. |
> | Действие | Microsoft.Web/sites/hostruntime/host/action | Выполнение действий среды выполнения приложения-функции, таких как синхронизация триггеров, добавление функций, вызов функций, их удаление и т.д. |
> | Действие | microsoft.web/sites/hostruntime/host/read | Получение узла для среды выполнения узла веб-приложений. |
> | Действие | microsoft.web/sites/hybridconnection/delete | Удаляет гибридное подключение для веб-приложений. |
> | Действие | microsoft.web/sites/hybridconnection/read | Возвращает гибридное подключение для веб-приложений. |
> | Действие | microsoft.web/sites/hybridconnection/write | Обновляет гибридное подключение для веб-приложений. |
> | Действие | microsoft.web/sites/hybridconnectionnamespaces/relays/delete | Удаление ретрансляторов в пространствах имен гибридных подключений для веб-приложений. |
> | Действие | microsoft.web/sites/hybridconnectionnamespaces/relays/listkeys/action | Получение списка ключей ретрансляторов в пространствах имен гибридных подключений для веб-приложений. |
> | Действие | microsoft.web/sites/hybridconnectionnamespaces/relays/read | Получение ретрансляторов в пространствах имен гибридных подключений для веб-приложений. |
> | Действие | microsoft.web/sites/hybridconnectionnamespaces/relays/write | Обновление ретрансляторов в пространствах имен гибридных подключений для веб-приложений. |
> | Действие | microsoft.web/sites/hybridconnectionrelays/read | Возвращает ретрансляторы гибридных подключений для веб-приложений. |
> | Действие | microsoft.web/sites/instances/deployments/delete | Удаление развертываний экземпляров веб-приложений. |
> | Действие | microsoft.web/sites/instances/deployments/read | Возвращает развертывания экземпляров веб-приложений. |
> | Действие | microsoft.web/sites/instances/extensions/log/read | Получение журнала расширений экземпляров веб-приложений. |
> | Действие | microsoft.web/sites/instances/extensions/processes/read | Получение процессов расширений экземпляров веб-приложений. |
> | Действие | microsoft.web/sites/instances/extensions/read | Получение расширений экземпляров веб-приложений. |
> | Действие | microsoft.web/sites/instances/processes/delete | Удаляет процессы экземпляров веб-приложений. |
> | Действие | microsoft.web/sites/instances/processes/modules/read | Получение модулей процессов экземпляров веб-приложений. |
> | Действие | microsoft.web/sites/instances/processes/read | Возвращает процессы экземпляров веб-приложений. |
> | Действие | microsoft.web/sites/instances/processes/threads/read | Возвращение потоков процессов экземпляров веб-приложений. |
> | Действие | microsoft.web/sites/instances/read | Возвращает экземпляры веб-приложений. |
> | Действие | microsoft.web/sites/listsyncfunctiontriggerstatus/action | Отображает состояние триггера функции синхронизации для веб-приложений. |
> | Действие | microsoft.web/sites/metricdefinitions/read | Возвращает определения метрик веб-приложений. |
> | Действие | microsoft.web/sites/metrics/read | Возвращает метрики веб-приложений. |
> | Действие | microsoft.web/sites/metricsdefinitions/read | Получение определений метрик веб-приложений. |
> | Действие | microsoft.web/sites/migratemysql/action | Перенос веб-приложений MySQL. |
> | Действие | microsoft.web/sites/migratemysql/read | Получение сведений о переносе веб-приложений MySQL. |
> | Действие | microsoft.web/sites/networktrace/action | Трассирует сеть для веб-приложений. |
> | Действие | microsoft.web/sites/networktraces/operationresults/read | Возвращает результаты веб-приложения сетевой трассировки операции. |
> | Действие | microsoft.web/sites/newpassword/action | Задает новый пароль для веб-приложений. |
> | Действие | microsoft.web/sites/operationresults/read | Возвращает результаты операций веб-приложений. |
> | Действие | microsoft.web/sites/operations/read | Получение операций веб-приложений. |
> | Действие | microsoft.web/sites/perfcounters/read | Возвращает счетчики производительности веб-приложений. |
> | Действие | microsoft.web/sites/premieraddons/delete | Удаляет надстройки Premier для веб-приложений. |
> | Действие | microsoft.web/sites/premieraddons/read | Возвращает надстройки Premier для веб-приложений. |
> | Действие | microsoft.web/sites/premieraddons/write | Обновляет надстройки Premier для веб-приложений. |
> | Действие | microsoft.web/sites/privateaccess/read | Возвращение данных контроля доступа к частным сайтам и авторизованные виртуальные сети, у которых есть доступ к сайту. |
> | Действие | Microsoft.Web/sites/PrivateEndpointConnectionsApproval/action | Утвердить подключения Private конечных точек |
> | Действие | microsoft.web/sites/processes/modules/read | Получение процессов приложений веб-модули. |
> | Действие | microsoft.web/sites/processes/read | Получение процессов веб-приложений. |
> | Действие | microsoft.web/sites/processes/threads/read | Получите потоки процессов Web Apps. |
> | Действие | microsoft.web/sites/publiccertificates/delete | Удаление открытых сертификатов веб-приложений. |
> | Действие | microsoft.web/sites/publiccertificates/read | Получение открытых сертификатов веб-приложений. |
> | Действие | microsoft.web/sites/publiccertificates/write | Обновление открытых сертификатов веб-приложений. |
> | Действие | Microsoft.Web/sites/publish/Action | Публикует веб-приложение. |
> | Действие | Microsoft.Web/sites/publishxml/Action | Возвращает XML-файл профиля публикации для веб-приложения. |
> | Действие | microsoft.web/sites/publishxml/read | Возвращает XML-файл профиля публикации для веб-приложений. |
> | Действие | Microsoft.Web/sites/Read | Возвращает свойства веб-приложения. |
> | Действие | microsoft.web/sites/recommendationhistory/read | Возвращает журнал рекомендаций для веб-приложения. |
> | Действие | microsoft.web/sites/recommendations/disable/action | Отключает рекомендации для веб-приложений. |
> | Действие | Microsoft.Web/sites/recommendations/Read | Возвращает список рекомендаций для веб-приложения. |
> | Действие | microsoft.web/sites/recover/action | Восстановление веб-приложений. |
> | Действие | Microsoft.Web/sites/resetSlotConfig/Action | Сбрасывает конфигурацию веб-приложения. |
> | Действие | microsoft.web/sites/resourcehealthmetadata/read | Получение метаданных работоспособности ресурсов веб-приложений. |
> | Действие | Microsoft.Web/sites/restart/Action | Перезапускает веб-приложение. |
> | Действие | microsoft.web/sites/restore/read | Возвращает данные о восстановлении веб-приложений. |
> | Действие | microsoft.web/sites/restore/write | Восстановление веб-приложений. |
> | Действие | microsoft.web/sites/restorefrombackupblob/action | Восстановление веб-приложения из большого двоичного объекта резервных копий. |
> | Действие | microsoft.web/sites/restorefromdeletedwebapp/action | Восстановление веб-приложений из удаленного приложения. |
> | Действие | microsoft.web/sites/restoresnapshot/action | Восстановление моментальных снимков веб-приложений. |
> | Действие | microsoft.web/sites/siteextensions/delete | Удаление расширений сайта веб-приложений. |
> | Действие | microsoft.web/sites/siteextensions/read | Получение расширений сайта веб-приложений. |
> | Действие | microsoft.web/sites/siteextensions/write | Обновление расширений сайта веб-приложений. |
> | Действие | microsoft.web/sites/slots/analyzecustomhostname/read | Возвращает результаты анализа пользовательского имени узла для слотов веб-приложений. |
> | Действие | Microsoft.Web/sites/slots/applySlotConfig/Action | Применяет конфигурацию слота веб-приложения из целевого слота к текущему слоту. |
> | Действие | Microsoft.Web/sites/slots/backup/Action | Создает резервную копию слота веб-приложения. |
> | Действие | microsoft.web/sites/slots/backup/read | Получение резервной копии слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/backup/write | Обновляет резервную копию слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/backups/action | Обнаружение резервных копий слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/backups/delete | Удаление резервной копии слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/backups/list/action | Выводит список резервных копий слотов веб-приложений. |
> | Действие | Microsoft.Web/sites/slots/backups/Read | Возвращает свойства резервной копии слотов веб-приложения. |
> | Действие | microsoft.web/sites/slots/backups/restore/action | Восстанавливает резервные копии слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/config/delete | Удаляет конфигурацию слотов веб-приложений. |
> | Действие | Microsoft.Web/sites/slots/config/list/Action | Отображает влияющие на безопасность параметры слота веб-приложения, такие как учетные данные для публикации, параметры приложения и строки подключения. |
> | Действие | Microsoft.Web/sites/slots/config/Read | Возвращает параметры конфигурации слота веб-приложения. |
> | Действие | Microsoft.Web/sites/slots/config/Write | Обновляет параметры конфигурации слота веб-приложения. |
> | Действие | microsoft.web/sites/slots/containerlogs/action | Получение сжатых журналов контейнеров для слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/containerlogs/download/action | Скачивание журналов контейнера слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/continuouswebjobs/delete | Удаляет непрерывные веб-задания слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/continuouswebjobs/read | Возвращает непрерывные веб-задания слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/continuouswebjobs/start/action | Запускает непрерывные веб-задания слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/continuouswebjobs/stop/action | Останавливает непрерывные веб-задания слотов веб-приложений. |
> | Действие | Microsoft.Web/sites/slots/Delete | Удаляет существующий слот веб-приложения. |
> | Действие | microsoft.web/sites/slots/deployments/delete | Удаляет развертывания слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/deployments/log/read | Возвращает журнал развертываний слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/deployments/read | Возвращает развертывания слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/deployments/write | Обновляет развертывания слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/detectors/read | Возвращение средств обнаружения слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/diagnostics/analyses/execute/Action | Запуск анализа данных диагностики слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/diagnostics/analyses/read | Получение анализа данных диагностики слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/diagnostics/aspnetcore/read | Получение данных диагностики слотов для веб-приложений для приложения ASP.NET Core. |
> | Действие | microsoft.web/sites/slots/diagnostics/autoheal/read | Получение данных диагностики слотов о функции Autoheal для веб-приложений. |
> | Действие | microsoft.web/sites/slots/diagnostics/deployment/read | Получение данных диагностики слотов о развертывании для веб-приложений. |
> | Действие | microsoft.web/sites/slots/diagnostics/deployments/read | Получение данных диагностики слотов о развертываниях для веб-приложений. |
> | Действие | microsoft.web/sites/slots/diagnostics/detectors/execute/Action | Запуск средства обнаружения данных диагностики слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/diagnostics/detectors/read | Получение сведений средства обнаружения данных диагностики слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/diagnostics/frebanalysis/read | Возвращение данных диагностики об анализе FREB для слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/diagnostics/loganalyzer/read | Получение данных диагностики слотов для веб-приложений об анализаторе журналов. |
> | Действие | microsoft.web/sites/slots/diagnostics/read | Получение данных диагностики слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/diagnostics/runtimeavailability/read | Получение данных диагностики слотов о доступности среды выполнения для веб-приложений. |
> | Действие | microsoft.web/sites/slots/diagnostics/servicehealth/read | Получение данных диагностики слотов о работоспособности служб для веб-приложений. |
> | Действие | microsoft.web/sites/slots/diagnostics/sitecpuanalysis/read | Получение данных диагностики слотов об анализе ЦП сайта для веб-приложений. |
> | Действие | microsoft.web/sites/slots/diagnostics/sitecrashes/read | Получение данных диагностики слотов о сбоях сайта для веб-приложений. |
> | Действие | microsoft.web/sites/slots/diagnostics/sitelatency/read | Получение данных диагностики слотов о задержке сайта для веб-приложений. |
> | Действие | microsoft.web/sites/slots/diagnostics/sitememoryanalysis/read | Получение данных диагностики слотов об анализе памяти сайта для веб-приложений. |
> | Действие | microsoft.web/sites/slots/diagnostics/siterestartsettingupdate/read | Получение данных диагностики слотов для веб-приложений об обновлении параметра перезапуска сайта. |
> | Действие | microsoft.web/sites/slots/diagnostics/siterestartuserinitiated/read | Получение данных диагностики слотов для веб-приложений об инициированном пользователем перезапуске сайта. |
> | Действие | microsoft.web/sites/slots/diagnostics/siteswap/read | Получение данных диагностики слотов для веб-приложений о переключении сайта. |
> | Действие | microsoft.web/sites/slots/diagnostics/threadcount/read | Получение данных диагностики слотов для веб-приложений о количестве потоков. |
> | Действие | microsoft.web/sites/slots/diagnostics/workeravailability/read | Получение данных диагностики слотов для веб-приложений о доступности рабочих процессов. |
> | Действие | microsoft.web/sites/slots/diagnostics/workerprocessrecycle/read | Получение данных диагностики слотов для веб-приложений о перезапуске рабочих процессов. |
> | Действие | microsoft.web/sites/slots/domainownershipidentifiers/read | Получение идентификаторов владения доменом слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/functions/listsecrets/action | Вывод списка секретов для функций слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/functions/read | Возвращение функций слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/hostnamebindings/delete | Удаляет привязки имен узлов для слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/hostnamebindings/read | Возвращает привязки имен узлов для слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/hostnamebindings/write | Обновляет привязки имен узлов для слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/hybridconnection/delete | Удаляет гибридное подключение для слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/hybridconnection/read | Возвращает гибридное подключение для слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/hybridconnection/write | Обновляет гибридное подключение для слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/hybridconnectionnamespaces/relays/delete | Удаление ретрансляторов в пространствах имен гибридных подключений слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/hybridconnectionnamespaces/relays/write | Обновление ретрансляторов в пространствах имен гибридных подключений слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/hybridconnectionrelays/read | Получение ретрансляторов гибридных подключений слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/instances/deployments/read | Возвращает развертывания экземпляры слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/instances/processes/delete | Удаление процессов экземпляров слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/instances/processes/read | Возвращает процессы экземпляры слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/instances/read | Возвращает экземпляры слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/metricdefinitions/read | Возвращает определения метрик слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/metrics/read | Возвращает метрики слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/migratemysql/read | Получение сведений о переносе слотов для веб-приложений MySQL. |
> | Действие | microsoft.web/sites/slots/networktrace/action | Трассировка сети слотов для веб-приложений. |
> | Действие | Microsoft.Web/Sites/slots/networktraces/operationresults/Read | Получите результаты операции трассировки сети слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/newpassword/action | Задает новый пароль для слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/operationresults/read | Возвращает результаты операций слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/operations/read | Получение операций слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/perfcounters/read | Получение счетчиков производительности слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/phplogging/read | Возвращает сведения о ведении журнала PHP для слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/premieraddons/delete | Удаляет надстройки Premier для слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/premieraddons/read | Возвращает надстройки Premier для слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/premieraddons/write | Обновляет надстройки Premier для слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/processes/read | Получение процессов слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/publiccertificates/delete | Удаление открытых сертификатов слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/publiccertificates/read | Получение открытых сертификатов слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/publiccertificates/write | Создание или обновление открытых сертификатов слотов для веб-приложений. |
> | Действие | Microsoft.Web/sites/slots/publish/Action | Публикует слот веб-приложения. |
> | Действие | Microsoft.Web/sites/slots/publishxml/Action | Возвращает XML-файл профиля публикации для слота веб-приложения. |
> | Действие | Microsoft.Web/sites/slots/Read | Возвращает свойства слота развертывания веб-приложения. |
> | Действие | microsoft.web/sites/slots/recover/action | Восстановление слотов веб-приложений. |
> | Действие | Microsoft.Web/sites/slots/resetSlotConfig/Action | Сбрасывает конфигурацию слота веб-приложения. |
> | Действие | microsoft.web/sites/slots/resourcehealthmetadata/read | Получение метаданных работоспособности ресурса слотов для веб-приложений. |
> | Действие | Microsoft.Web/sites/slots/restart/Action | Перезапускает слот веб-приложения. |
> | Действие | microsoft.web/sites/slots/restore/read | Возвращает данные о восстановлении слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/restore/write | Восстановление слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/restorefrombackupblob/action | Восстановление слота веб-приложений из большого двоичного объекта резервных копий. |
> | Действие | microsoft.web/sites/slots/restorefromdeletedwebapp/action | Восстановление слотов веб-приложений из удаленного приложения. |
> | Действие | microsoft.web/sites/slots/restoresnapshot/action | Восстановление моментальных снимков слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/siteextensions/delete | Удаление расширений сайта слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/siteextensions/read | Получение расширений сайта слотов для веб-приложений. |
> | Действие | microsoft.web/sites/slots/siteextensions/write | Обновление расширений сайта слотов для веб-приложений. |
> | Действие | Microsoft.Web/sites/slots/slotsdiffs/Action | Возвращает различия конфигураций веб-приложения и слотов. |
> | Действие | Microsoft.Web/sites/slots/slotsswap/Action | Переключает слоты развертывания веб-приложения. |
> | Действие | microsoft.web/sites/slots/snapshots/read | Получение моментальных снимков слотов для веб-приложений. |
> | Действие | Microsoft.Web/sites/slots/sourcecontrols/Delete | Удаляет параметры конфигурации системы управления версиями для слота веб-приложения. |
> | Действие | Microsoft.Web/sites/slots/sourcecontrols/Read | Возвращает параметры конфигурации системы управления версиями для слота веб-приложения. |
> | Действие | Microsoft.Web/sites/slots/sourcecontrols/Write | Обновляет параметры конфигурации системы управления версиями для слота веб-приложения. |
> | Действие | Microsoft.Web/sites/slots/start/Action | Запускает слот веб-приложения. |
> | Действие | Microsoft.Web/sites/slots/stop/Action | Останавливает слот веб-приложения. |
> | Действие | microsoft.web/sites/slots/sync/action | Синхронизирует слоты веб-приложений. |
> | Действие | microsoft.web/sites/slots/triggeredwebjobs/delete | Удаляет активированные веб-задания слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/triggeredwebjobs/read | Возвращает активированные веб-задания слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/triggeredwebjobs/run/action | Выполняет активированные веб-задания слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/usages/read | Возвращает данные об использовании слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/virtualnetworkconnections/delete | Удаляет подключения виртуальных сетей для слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/virtualnetworkconnections/gateways/write | Обновляет шлюзы подключений виртуальных сетей для слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/virtualnetworkconnections/read | Возвращает подключения виртуальных сетей для слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/virtualnetworkconnections/write | Обновляет подключения виртуальных сетей для слотов веб-приложений. |
> | Действие | microsoft.web/sites/slots/webjobs/read | Возвращает веб-задания слотов веб-приложений. |
> | Действие | Microsoft.Web/sites/slots/Write | Создает новый слот веб-приложения или обновляет существующий. |
> | Действие | Microsoft.Web/sites/slotsdiffs/Action | Возвращает различия конфигураций веб-приложения и слотов. |
> | Действие | Microsoft.Web/sites/slotsswap/Action | Переключает слоты развертывания веб-приложения. |
> | Действие | microsoft.web/sites/snapshots/read | Возвращает моментальные снимки веб-приложений. |
> | Действие | Microsoft.Web/sites/sourcecontrols/Delete | Удаляет параметры конфигурации системы управления версиями веб-приложения. |
> | Действие | Microsoft.Web/sites/sourcecontrols/Read | Возвращает параметры конфигурации системы управления версиями веб-приложения. |
> | Действие | Microsoft.Web/sites/sourcecontrols/Write | Обновляет параметры конфигурации системы управления версиями веб-приложения. |
> | Действие | Microsoft.Web/sites/start/Action | Запускает веб-приложение. |
> | Действие | Microsoft.Web/sites/stop/Action | Останавливает веб-приложение. |
> | Действие | microsoft.web/sites/sync/action | Синхронизирует веб-приложения. |
> | Действие | microsoft.web/sites/syncfunctiontriggers/action | Активация функции синхронизации для веб-приложений. |
> | Действие | microsoft.web/sites/triggeredwebjobs/delete | Удаляет активированные веб-задания веб-приложений. |
> | Действие | microsoft.web/sites/triggeredwebjobs/history/read | Получение истории активированных веб-заданий для веб-приложений. |
> | Действие | microsoft.web/sites/triggeredwebjobs/read | Возвращает активированные веб-задания веб-приложений. |
> | Действие | microsoft.web/sites/triggeredwebjobs/run/action | Выполняет активированные веб-задания веб-приложений. |
> | Действие | microsoft.web/sites/usages/read | Возвращает данные об использовании веб-приложений. |
> | Действие | microsoft.web/sites/virtualnetworkconnections/delete | Удаляет подключения виртуальных сетей веб-приложений. |
> | Действие | microsoft.web/sites/virtualnetworkconnections/gateways/read | Возвращает шлюзы подключений виртуальных сетей веб-приложений. |
> | Действие | microsoft.web/sites/virtualnetworkconnections/gateways/write | Обновляет шлюзы подключений виртуальных сетей веб-приложений. |
> | Действие | microsoft.web/sites/virtualnetworkconnections/read | Возвращает подключения виртуальных сетей веб-приложений. |
> | Действие | microsoft.web/sites/virtualnetworkconnections/write | Обновляет подключения виртуальных сетей веб-приложений. |
> | Действие | microsoft.web/sites/webjobs/read | Возвращает веб-задания веб-приложений. |
> | Действие | Microsoft.Web/sites/Write | Создает новое веб-приложение или обновляет существующее. |
> | Действие | microsoft.web/skus/read | Возвращает номера SKU. |
> | Действие | microsoft.web/sourcecontrols/read | Возвращает систему управления версиями. |
> | Действие | microsoft.web/sourcecontrols/write | Обновляет системы управления версиями. |
> | Действие | microsoft.web/unregister/action | Отменяет регистрацию поставщика ресурсов Microsoft.Web для подписки. |
> | Действие | microsoft.web/validate/action | Выполняет проверку. |
> | Действие | microsoft.web/verifyhostingenvironmentvnet/action | Проверка виртуальной сети среды размещения. |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | ОПИСАНИЕ |
> | --- | --- | --- |
> | Действие | Microsoft.WorkloadMonitor/components/read | Возвращает компоненты ресурса. |
> | Действие | Microsoft.WorkloadMonitor/componentsSummary/read | Возвращает сводки компонентов. |
> | Действие | Microsoft.WorkloadMonitor/monitorInstances/read | Получает экземпляры мониторов ресурса. |
> | Действие | Microsoft.WorkloadMonitor/monitorInstancesSummary/read | Получает сводку экземпляров монитора. |
> | Действие | Microsoft.WorkloadMonitor/monitors/read | Получает мониторы ресурсов. |
> | Действие | Microsoft.WorkloadMonitor/monitors/write | Настойка монитора ресурса. |
> | Действие | Microsoft.WorkloadMonitor/notificationSettings/read | Получает параметры уведомлений для ресурса. |
> | Действие | Microsoft.WorkloadMonitor/notificationSettings/write | Настройка параметров уведомлений для ресурса. |
> | Действие | Microsoft.WorkloadMonitor/operations/read | Получает поддерживаемые операции. |

## <a name="next-steps"></a>Дальнейшие действия

- [Пользовательские роли для ресурсов Azure](custom-roles.md)
- [Встроенные роли управления доступом на основе ролей в Azure](built-in-roles.md)
