---
title: включить файл
description: включить файл
services: networking
author: anzaman
ms.service: VPN Gateway
ms.topic: include
ms.date: 08/25/2020
ms.author: alzam
ms.custom: include file
ms.openlocfilehash: 9fe9ef5549ced3b73d18d553fa0b62ec019684fe
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95555096"
---
| Ресурс                                | Ограничение        |
|-----------------------------------------|------------------------------|
| Префиксы адресов виртуальной сети                   | 600 на VPN-шлюз          |
| Совокупные маршруты BGP                    | 4 000 на VPN-шлюз        |
| Префиксы адресов шлюза локальной сети  | 1000 на шлюз локальной сети               |
| Подключения типа "сеть — сеть"                         | [Зависит от SKU шлюза](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku)|
| Подключения типа "точка — сеть"                         | [Зависит от SKU шлюза](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku) |
| Ограничение маршрута P2S — IKEv2                 | 256 для Windows, отличных от Windows **/** 25           |
| Ограничение маршрута P2S — Опенвпн               | 1000                         |
| Макс. flows                              | 100 КБ для VpnGw1/AZ  **/**  512 КБ для VpnGw2-4/AZ|