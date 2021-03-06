---
title: Сведения о VPN-шлюзе Azure
description: Узнайте, что представляет собой VPN-шлюз и как его использовать для подключения к виртуальным сетям IPsec IKE "сеть — сеть", "виртуальная сеть — виртуальная сеть" и "точка — сеть".
services: vpn-gateway
author: cherylmc
Customer intent: As someone with a basic network background, but is new to Azure, I want to understand the capabilities of Azure VPN Gateway so that I can securely connect to my Azure virtual networks.
ms.service: vpn-gateway
ms.topic: overview
ms.date: 08/27/2020
ms.author: cherylmc
ms.custom: contperf-fy21q1
ms.openlocfilehash: c19e1156f897873defaba4cebaabb904d034ead3
ms.sourcegitcommit: 3ea45bbda81be0a869274353e7f6a99e4b83afe2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2020
ms.locfileid: "97032751"
---
# <a name="what-is-vpn-gateway"></a>Сведения о VPN-шлюзе

VPN-шлюз — это особый тип шлюза виртуальной сети, используемый для отправки зашифрованного трафика между виртуальной сетью Azure и локальным расположением через общедоступный Интернет. Его также можно использовать для обмена зашифрованным трафиком между виртуальными сетями Azure через сеть Майкрософт. Каждая виртуальная сеть может иметь только один VPN-шлюз. Однако к одному VPN-шлюзу можно создать несколько подключений. При создании нескольких подключений к одному VPN-шлюзу все VPN-туннели совместно используют доступную для этого шлюза пропускную способность.

## <a name="what-is-a-virtual-network-gateway"></a><a name="whatis"></a>Что такое шлюз виртуальной сети

Шлюз виртуальной сети состоит из двух или более виртуальных машин, развернутых в определенной создаваемой подсети, которая называется *подсетью шлюза*. Виртуальные машины шлюза виртуальной сети содержат таблицы маршрутизации и запускают определенные службы шлюза. Эти виртуальные машины создаются при создании шлюза виртуальной сети. Вы не можете напрямую настраивать виртуальные машины, которые являются частью шлюза виртуальной сети.

Настраивая шлюз виртуальной сети, вы настраиваете параметр, указывающий тип шлюза. Тип шлюза определяет, как будет использоваться шлюз виртуальной сети и какие действия он будет предпринимать. VPN-шлюз — это созданный шлюз виртуальной сети типа VPN. Этим он отличается от шлюза ExpressRoute, который использует другой тип шлюза. Виртуальная сеть может иметь два шлюза виртуальной сети: один VPN-шлюз и один шлюз ExpressRoute. Дополнительные сведения см. в разделе [Типы шлюзов](vpn-gateway-about-vpn-gateway-settings.md#gwtype).

Создание шлюза виртуальной сети может длиться до 45 минут. При создании шлюза виртуальной сети связанные виртуальные машины развертываются в подсети шлюза и на них настраиваются необходимые указанные параметры. Создав VPN-шлюз, вы можете создать подключение VPN-туннеля IPsec/IKE между этим и другим VPN-шлюзом (подключение "виртуальная сеть — виртуальная сеть") или создать распределенное подключение VPN-туннеля IPsec/IKE между VPN-шлюзом и локальным VPN-устройством (подключение "сеть — сеть"). Вы также можете создать VPN-подключение типа "точка — сеть" (VPN-подключение по протоколу OpenVPN, IKEv2 или SSTP), которое позволяет подключиться к виртуальной сети из удаленного расположения, например во время конференции или из дома.

## <a name="configuring-a-vpn-gateway"></a><a name="configuring"></a>Настройка VPN-шлюза

При подключении через VPN-шлюз используется ряд ресурсов, настроенных с определенными параметрами. Большинство этих ресурсов можно настроить по отдельности, однако некоторые из них следует настраивать в определенном порядке.

### <a name="design"></a><a name="diagrams"></a>Конструирование

Важно знать, что существуют различные конфигурации подключения к VPN-шлюзу. Вам нужно определить, какая из конфигураций наилучшим образом соответствует вашим требованиям. Например, для подключений "точка — сеть", "сеть — сеть" и параллельных подключений ExpressRoute и "сеть — сеть" используются различные инструкции и требования к конфигурации. Сведения о макетах и схемах топологии подключения см.в разделе [Конструктор](design.md).

### <a name="planning-table"></a><a name="planningtable"></a>Таблица планирования

Приведенная ниже таблица поможет вам подобрать наилучший вариант подключения для решения.

[!INCLUDE [cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]

### <a name="settings"></a><a name="settings"></a>Параметры

Правильный выбор параметров каждого ресурса критически важен для успешного создания подключения. Сведения об отдельных ресурсах и параметрах для VPN-шлюза см. в статье [Сведения о параметрах VPN-шлюза](vpn-gateway-about-vpn-gateway-settings.md). Эта статья содержит сведения о типах шлюзов, SKU шлюзов, типах VPN, типах подключения, подсетях шлюзов, локальных сетевых шлюзах и других интересующих параметрах ресурсов.

### <a name="deployment-tools"></a><a name="tools"></a>Средства развертывания

Создать и настроить ресурсы можно с помощью одного средства настройки, например портала Azure. Затем с помощью другого средства, например PowerShell, можно настроить дополнительные ресурсы или при необходимости внести изменения в имеющееся ресурсы. Сейчас на портале Azure можно настроить не все ресурсы и их параметры. В статьях с инструкциями по топологии каждого подключения указывается, нужно ли использовать определенное средство настройки.

## <a name="gateway-skus"></a><a name="gwsku"></a>SKU шлюзов

Во время создания шлюза виртуальной сети укажите номер SKU шлюза, который вы хотите использовать. Выберите номер SKU, который соответствует требованиям, в зависимости от типов рабочих нагрузок, пропускной способности, функций и соглашений об уровне обслуживания.

* Подробные сведения о номерах SKU шлюзов, в том числе поддерживаемые возможности, инструкции по разработке и тестированию в рабочей среде, а также инструкции по настройке см. в разделе [Gateway SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku) (SKU шлюзов).
* Сведения об устаревших SKU см. в статье [Working with virtual network gateway SKUs (legacy SKUs)](vpn-gateway-about-skus-legacy.md) (Работа с SKU шлюзов виртуальных сетей (устаревшие SKU)).

### <a name="gateway-skus-by-tunnel-connection-and-throughput"></a><a name="benchmark"></a>Номера SKU шлюзов в зависимости от количества туннелей и подключений, и пропускной способности шлюзов

[!INCLUDE [Aggregated throughput by SKU](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

## <a name="availability-zones"></a><a name="availability"></a>Зоны доступности

VPN-шлюзы можно развернуть в Зонах доступности Azure. В результате шлюзы виртуальной сети смогут работать на более высоких уровнях отказоустойчивости, масштабируемости и доступности. При развертывании в зонах доступности Azure происходит физическое и логическое разделение шлюзов в пределах региона с одновременной защитой локального сетевого подключения к Azure от сбоев на уровне зоны. Ознакомьтесь с разделом [Избыточные между зонами шлюзы виртуальной сети в Зонах доступности Azure](about-zone-redundant-vnet-gateways.md).

## <a name="pricing"></a><a name="pricing"></a>Цены

[!INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)]

См. дополнительные сведения о [номерах SKU для VPN-шлюзов](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

## <a name="faq"></a><a name="faq"></a>Часто задаваемые вопросы

См. [вопросы и ответы, связанные с использованием VPN-шлюза](vpn-gateway-vpn-faq.md).

## <a name="whats-new"></a><a name="new"></a>Новые возможности

Подпишитесь на RSS-канал и просматривайте последние обновления компонентов для VPN-шлюза на странице [Обновления Azure](https://azure.microsoft.com/updates/?category=networking&query=VPN%20Gateway).

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения см. в статье [VPN-шлюз: вопросы и ответы](vpn-gateway-vpn-faq.md).
- См. дополнительные сведения об [ограничениях подписки и службы](../azure-resource-manager/management/azure-subscription-service-limits.md#networking-limits).
- Дополнительные сведения о некоторых других ключевых [сетевых возможностях](../networking/networking-overview.md) Azure.
