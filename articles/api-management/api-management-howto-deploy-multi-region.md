---
title: Развертывание служб управления API Azure в нескольких регионах Azure | Документация Майкрософт
description: Дополнительные сведения о развертывании экземпляра службы управления Azure API в различных регионах Azure.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2019
ms.author: apimpm
ms.openlocfilehash: d22da92355616c208c7616b4b0e8c26b7f9e7006
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60658197"
---
# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Развертывание экземпляра службы управления Azure API в различных регионах Azure

Служба управления API Azure поддерживает развертывание в нескольких регионах, что позволяет издателям API распространять единую службу управления API Azure в любом количестве требуемых регионов Azure. Это сокращает задержки, связанные с географической удаленностью потребителей API, а также повышает доступность службы, когда какой-либо из регионов переходит в автономный режим.

Новая служба управления API Azure изначально содержит только одну [единицу][unit] в одном регионе Azure, который считается основным. Однако через портал Azure можно легко добавить дополнительные регионы. В каждом регионе развертывается сервер шлюза управления API и весь трафик вызовов направляется на ближайший из таких шлюзов с точки зрения задержки. Если регион переходит в автономный режим, трафик автоматически перенаправляется к другому ближайшему шлюзу.

> [!NOTE]
> Служба управления API Azure в разных регионах может реплицировать только компонент шлюза API. Компонент службы управления размещается только в основном регионе. В случае сбоя в основном регионе невозможно применить изменения конфигураций экземпляра службы управления API Azure (включая обновления настроек или политик).

[!INCLUDE [premium.md](../../includes/api-management-availability-premium.md)]

## <a name="add-region"> </a>Создание экземпляра службы управления API в новом регионе

> [!NOTE]
> Если экземпляр службы управления API еще не создан, выполните инструкции в статье [Создание экземпляра службы управления API Azure][Create an API Management service instance].

На портале Azure перейдите на страницу **Scale and pricing** (Масштаб и цены) для своего экземпляра службы управления API. 

![Вкладка "Масштаб"][api-management-scale-service]

Чтобы развернуть службу в новом регионе, щелкните **+ Add region** (+ Добавить регион) на панели инструментов.

![Добавить регион][api-management-add-region]

Выберите расположение из раскрывающегося списка и задайте число единиц с помощью ползунка.

![Указание единиц][api-management-select-location-units]

Щелкните **Добавить**, чтобы разместить выбранные ресурсы в таблице "Расположения". 

Повторите этот процесс, пока не будут настроены все расположения, затем щелкните **Сохранить** на панели инструментов, чтобы начать процесс развертывания.

## <a name="remove-region"> </a>Удаление экземпляра службы управления API из расположения

На портале Azure перейдите на страницу **Scale and pricing** (Масштаб и цены) для своего экземпляра службы управления API. 

![Вкладка "Масштаб"][api-management-scale-service]

Для расположения, которое нужно удалить, откройте контекстное меню с помощью кнопки **…** в правой части таблицы. Щелкните **Удалить**.

Подтвердите удаление и нажмите кнопку **Сохранить**, чтобы применить изменения.

## <a name="route-backend"> </a>Маршрутизация вызовов API в региональных серверных службах

Служба управления API Azure поддерживает только один URL-адрес внутренней службы. Несмотря на то что экземпляры службы управления API Azure существуют в разных регионах, шлюз API будет по-прежнему перенаправлять запросы к одной и той же внутренней службе, которая развертывается только в один регион. В этом случае выигрыш в производительности будет поступать только из ответов, кэшированных в службе управления API Azure в регионе, указанном для запроса, но обращение к серверной части по всему миру может по-прежнему вызывать большую задержку.

Чтобы полностью использовать географическое распределение системы, необходимо обладать серверными службами, развернутыми в тех же регионах, что и экземпляры службы управления API Azure. Затем с помощью политик и свойства `@(context.Deployment.Region)` можно перенаправлять трафик в локальные экземпляры серверной части.

1. Перейдите к экземпляру службы управления API Azure и в меню слева щелкните **Интерфейсы API**.
2. Выберите нужный API.
3. Щелкните **Редактор кода** в раскрывающемся меню со стрелкой в **Обработке входящих запросов**.

    ![Редактор кода API](./media/api-management-howto-deploy-multi-region/api-management-api-code-editor.png)

4. Используйте условие `set-backend` в сочетании с условными политиками `choose`, чтобы создать подходящую политику маршрутизации в разделе `<inbound> </inbound>` файла.

    Например, следующий файл XML будет работать как для региона "Западная часть США", так и для региона "Восточная Азия".

    ```xml
    <policies>
        <inbound>
            <base />
            <choose>
                <when condition="@("West US".Equals(context.Deployment.Region, StringComparison.OrdinalIgnoreCase))">
                    <set-backend-service base-url="http://contoso-us.com/" />
                </when>
                <when condition="@("East Asia".Equals(context.Deployment.Region, StringComparison.OrdinalIgnoreCase))">
                    <set-backend-service base-url="http://contoso-asia.com/" />
                </when>
                <otherwise>
                    <set-backend-service base-url="http://contoso-other.com/" />
                </otherwise>
            </choose>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
        <on-error>
            <base />
        </on-error>
    </policies>
    ```

> [!TIP]
> Может также передней серверных службах с [диспетчера трафика Azure](https://azure.microsoft.com/services/traffic-manager/)прямых вызовов API для диспетчера трафика и позвольте ей устранить маршрутизации автоматически.

## <a name="custom-routing"> </a>Использовать пользовательскую маршрутизацию к шлюзам региональной службе управления API

API управления перенаправлять запросы на региональном *шлюза* на основе [наименьшую задержку](../traffic-manager/traffic-manager-routing-methods.md#performance). Несмотря на то, что он уже не сможете переопределить этот параметр в службе управления API, можно использовать диспетчер трафика с помощью настраиваемых правил маршрутизации.

1. Создать свой собственный [диспетчера трафика Azure](https://azure.microsoft.com/services/traffic-manager/).
1. Если вы используете личный домен, [использовать его с помощью диспетчера трафика](../traffic-manager/traffic-manager-point-internet-domain.md) вместо службы управления API.
1. [Настройка в диспетчере трафика конечные точки API управления региональной](../traffic-manager/traffic-manager-manage-endpoints.md). Региональные конечные точки соответствовать шаблону URL-адрес `https://<service-name>-<region>-01.regional.azure-api.net`, например `https://contoso-westus2-01.regional.azure-api.net`.
1. [Настроить конечные точки API управления региональной состояние в диспетчере трафика](../traffic-manager/traffic-manager-monitoring.md). Конечные точки в региональных состоянии соответствовать шаблону URL-адрес `https://<service-name>-<region>-01.regional.azure-api.net/status-0123456789abcdef`, например `https://contoso-westus2-01.regional.azure-api.net/status-0123456789abcdef`.
1. Укажите [метода маршрутизации](../traffic-manager/traffic-manager-routing-methods.md) диспетчера трафика.


[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-location-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-location-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Create an API Management service instance]: get-started-create-service-instance.md
[Get started with Azure API Management]: get-started-create-service-instance.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[unit]: https://azure.microsoft.com/pricing/details/api-management/
[Premium]: https://azure.microsoft.com/pricing/details/api-management/
