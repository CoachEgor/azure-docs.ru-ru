---
title: Примеры шаблонов Azure Resource Manager для Azure Front Door
description: Примеры шаблонов Azure Resource Manager для службы Azure Front Door
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/13/2018
ms.author: sharadag
ms.openlocfilehash: de3b13162e7828542331e7e7a34d0101f7feaf71
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "79471664"
---
# <a name="azure-resource-manager-deployment-model-templates-for-front-door"></a>Шаблоны модели развертывания Azure Resource Manager для Front Door

В приведенной ниже таблице представлены ссылки на шаблоны модели развертывания с помощью Azure Resource Manager для службы Azure Front Door. 

| | |
| ---| ---|
| [Создание базовой конфигурации Front Door](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-create-basic)| Создает базовую конфигурацию Front Door с одной серверной системой. |
| [Создание Front Door с несколькими серверными системами, серверных пулами и маршрутизацией на основе URL-адресов](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-create-multiple-backends)| Создает Front Door с балансировкой нагрузки, настроенный для нескольких серверных систем в одном или нескольких серверных пулах на основе URL-пути. |
| [Подключение пользовательского домена с HTTPS (управляемый Front Door сертификат) с помощью Front Door](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-custom-domain)| Добавьте пользовательский домен в конфигурацию Front Door и включите для него HTTPS-трафик с помощью управляемого Front Door сертификата, который был создан через DigiCert. |
| [Создание Front Door с геофильтрацией](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-geo-filtering)| Создайте конфигурацию Front Door, которая разрешает и блокирует трафик из определенных стран и регионов. |
| [Управление пробами работоспособности для серверных систем в конфигурации Front Door](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-health-probes)| Обновите Front Door, чтобы изменить параметры проб работоспособности, обновив путь к пробам, а также периодичность отправки проб. |
| [Создание Front Door с конфигурацией серверной системы "активный/резервный"](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-priority-lb)| Создает Front Door, который демонстрирует маршрутизацию на основе приоритетов для топологии приложений "активный/резервный", т. е. по умолчанию отправляет весь трафик в основную (наиболее приоритетную) серверную систему, пока она не станет недоступной. |
| [Создание Front Door с кэшированием определенных маршрутов](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-create-caching)| Создает Front Door с кэшированием определенной конфигурации маршрутизации, что позволяет кэшировать все статические активы для вашей рабочей нагрузки. |
| [Настройка сходства сеансов для имен узлов Front Door](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-session-affinity) | Обновляет Front Door, включая сходство сеанса для интерфейсного узла и, таким образом, отправляя последующий трафик из одного и того же сеанса пользователя в одну и ту же серверную часть. |
| [Настройка Front Door для включения IP-адресов клиентов в разрешенный список или список блокировок](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-waf-clientip)| Настраивает Front Door на ограничение трафика с определенных IP-адресов, используя настраиваемое управление доступом и IP-адреса клиентов. |
| [Настройка Front Door на работу с определенными параметрами HTTP](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-waf-http-params)| Настраивает Front Door на разрешение или блокировку определенного трафика согласно параметрам HTTP во входящих запросах с использованием пользовательских правил для управления доступом на основе параметров HTTP. |
| [Настройка ограничения скорости для Front Door](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-rate-limiting)| Настраивает Front Door на ограничение скорости входящего трафика для заданного интерфейсного узла. |
| | |

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [создании Front Door](quickstart-create-front-door.md).
- Дополнительные сведения о том, [как работает Front Door](front-door-routing-architecture.md).