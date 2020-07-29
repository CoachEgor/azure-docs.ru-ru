---
title: Локальный шлюз данных для Azure Analysis Services | Документация Майкрософт
description: Локальный шлюз данных необходим тогда, когда сервер служб Analysis Services в Azure будет подключен к локальным источникам данных.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 648646b6f973762245c344cd2629a874a219b170
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "76310158"
---
# <a name="connecting-to-on-premises-data-sources-with-on-premises-data-gateway"></a>Подключение к локальным источникам данных с помощью локального шлюза данных

Локальный шлюз данных обеспечивает защищенную передачу данных между локальными источниками данных и серверами служб Azure Analysis Services в облаке. Помимо работы с несколькими серверами Azure Analysis Services в одном регионе, последняя версия шлюза также работает с Azure Logic Apps, Power BI, Power Apps и Power автоматизируется. Хотя устанавливаемый шлюз одинаков во всех этих службах, Azure Analysis Services и Logic Apps имеют некоторые дополнительные действия.

Приведенные здесь сведения относятся к работе Azure Analysis Services с локальным шлюзом данных. Дополнительные сведения о шлюзе в целом и о том, как он работает с другими службами, см. в статье [что такое локальный шлюз данных?](/data-integration/gateway/service-gateway-onprem).

Для Azure Analysis Services установка с помощью шлюза в первый раз состоит из четырех этапов:

- **Скачивание и запуск программы установки.** На этом шаге устанавливается служба шлюза на компьютер организации. Войдите в Azure с помощью учетной записи своего [клиента](/previous-versions/azure/azure-services/jj573650(v=azure.100)#what-is-an-azure-ad-tenant) Azure AD. Учетные записи Azure B2B (гостевые) не поддерживаются.

- **Регистрация шлюза.** На этом шаге следует указать имя и ключ восстановления для шлюза и выбрать регион. Таким образом шлюз регистрируется в облачной службе шлюза. Ресурс шлюза можно зарегистрировать в любом регионе, но рекомендуется находиться в том же регионе, что и серверы Analysis Services. 

- **Создание ресурса шлюза в Azure** . на этом шаге вы создадите ресурс шлюза в Azure.

- **Подключите серверы к ресурсу шлюза** . После получения ресурса шлюза можно приступить к подключению к нему серверов. Можно подключить несколько серверов и другие ресурсы, если они находятся в одном регионе.



## <a name="how-it-works"></a><a name="how-it-works"> </a>Принцип работы
Установленный на компьютере в вашей организации шлюз работает как служба Windows, **локальный шлюз данных**. Эта локальная служба зарегистрирована в облачной службе шлюза с помощью служебной шины Azure. Затем вы создадите локальный ресурс шлюза данных для подписки Azure. Затем ваши серверы Azure Analysis Services будут подключены к ресурсу шлюза Azure. Если моделям вашего сервера необходимо подключиться к локальным источникам данных для запросов или обработки, запрос и поток данных проходит через ресурс шлюза, служебную шину Azure, локальную службу локального шлюза данных и источники данных. 

![Принцип работы](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Запросы и поток данных

1. Облачная служба создает запрос с зашифрованными учетными данными для локального источника. Затем запрос отправляется в очередь, чтобы его обработал шлюз.
2. Облачная служба шлюза анализирует запрос и отправляет его в [служебную шину Azure](https://azure.microsoft.com/documentation/services/service-bus/).
3. Локальный шлюз данных опрашивает служебную шину Azure, чтобы проверить наличие ожидающих запросов.
4. Шлюз получает запрос, расшифровывает учетные данные и подключается к источникам данных, используя эти учетные данные.
5. Затем шлюз отправляет запрос к источнику данных для выполнения.
6. Результаты отправляются из источника данных обратно в шлюз, а затем в облачную службу и на сервер.

## <a name="installing"></a>Установка

При установке для Azure Analysis Servicesной среды важно выполнить действия, описанные в [статье Установка и настройка локального шлюза данных для Azure Analysis Services](analysis-services-gateway-install.md). Эта статья относится к Azure Analysis Services. Он включает дополнительные шаги, необходимые для настройки локального ресурса шлюза данных в Azure и подключения сервера Azure Analysis Services к ресурсу.

## <a name="ports-and-communication-settings"></a>Порты и параметры связи

Шлюз создает исходящее подключение к служебной шине Azure. Он обменивается данными через исходящие порты: TCP 443 (по умолчанию), а также 5671, 5672 и 9350–9354.  Шлюзу не требуются входящие порты.

Может потребоваться включить IP-адреса для вашей области данных в брандмауэре. Вы можете скачать [список IP-адресов центра обработки данных Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=56519). Список обновляется раз в неделю. IP-адреса, перечисленные в списке центра обработки данных Azure, находятся в нотации CIDR. Дополнительные сведения о CIDR см. [здесь](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).

Ниже приведены полные доменные имена, используемые шлюзом.

| Имена доменов | Исходящие порты | Описание: |
| --- | --- | --- |
| *.powerbi.com |80 |HTTP для скачивания установщика. |
| *.powerbi.com |443 |HTTPS |
| *.analysis.windows.net |443 |HTTPS |
| *. login.windows.net, login.live.com, aadcdn.msauth.net |443 |HTTPS |
| *.servicebus.windows.net |5671–5672 |Протокол AMQP |
| *.servicebus.windows.net |443, 9350–9354 |Прослушиватели ретрансляции служебной шины по протоколу TCP (порт 443 требуется для получения маркера контроля доступа). |
| *.frontend.clouddatahub.net |443 |HTTPS |
| *.core.windows.net |443 |HTTPS |
| login.microsoftonline.com |443 |HTTPS |
| *.msftncsi.com |443 |Используется для проверки подключения к Интернету, если шлюз недоступен для службы Power BI. |
| *.microsoftonline-p.com |443 |Используется для проверки подлинности в зависимости от конфигурации. |
| dc.services.visualstudio.com  |443 |Используется AppInsights для сбора данных телеметрии. |

### <a name="forcing-https-communication-with-azure-service-bus"></a><a name="force-https"></a>Принудительное использование протокола HTTPS для связи со служебной шиной Azure

Можно настроить шлюз для принудительного использования протокола HTTPS для связи со служебной шиной Azure вместо прямого подключения TCP. Однако это может значительно снизить производительность. Для этого можно отредактировать файл *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config*, изменив значение `AutoDetect` на `Https`. Обычно этот файл находится в расположении *C:\Program Files\On-premises data gateway*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <a name="next-steps"></a>Дальнейшие шаги 

Следующие статьи включены в общее содержимое локального шлюза данных, которое применяется ко всем службам, которые поддерживает шлюз:

* [Вопросы и ответы о локальном шлюзе данных](https://docs.microsoft.com/data-integration/gateway/service-gateway-onprem-faq)   
* [Использование приложения локального шлюза данных](https://docs.microsoft.com/data-integration/gateway/service-gateway-app)   
* [Администрирование на уровне клиента](https://docs.microsoft.com/data-integration/gateway/service-gateway-tenant-level-admin)
* [Настройка параметров прокси-сервера](https://docs.microsoft.com/data-integration/gateway/service-gateway-proxy)   
* [Настройка параметров связи](https://docs.microsoft.com/data-integration/gateway/service-gateway-communication)   
* [Настройка файлов журналов](https://docs.microsoft.com/data-integration/gateway/service-gateway-log-files)   
* [Устранение проблем](https://docs.microsoft.com/data-integration/gateway/service-gateway-tshoot)
* [Мониторинг и оптимизация производительности шлюза](https://docs.microsoft.com/data-integration/gateway/service-gateway-performance)
