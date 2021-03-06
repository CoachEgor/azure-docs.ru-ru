---
title: Решение VMware для Azure, Клаудсимпле — Настройка DNS и DHCP для частного облака
description: Описание настройки DNS и DHCP для приложений и рабочих нагрузок, работающих в среде частного облака Клаудсимпле.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d7531a22d5b0f44fdaae837a75d17ea18852acf0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87077298"
---
# <a name="set-up-dns-and-dhcp-applications-and-workloads-in-your-cloudsimple-private-cloud"></a>Настройка приложений и рабочих нагрузок DNS и DHCP в частном облаке Клаудсимпле

Приложениям и рабочим нагрузкам, выполняемым в среде частного облака, требуется разрешение имен и службы DHCP для назначения адресов и поиска по IP-адресам.  Для предоставления этих служб требуется определенная инфраструктура DHCP и DNS.  Вы можете настроить виртуальную машину для предоставления этих служб в среде частного облака.  

## <a name="prerequisites"></a>Предварительные требования

* Группа распределенных портов с настроенной виртуальной ЛС
* Настройка маршрута к локальным или DNS-серверам в Интернете
* Шаблон виртуальной машины или ISO-образ для создания виртуальной машины

## <a name="linux-based-dns-server-setup"></a>Установка DNS-сервера на основе Linux

Linux предлагает различные пакеты для настройки DNS-серверов.  Ниже приведен [Пример настройки из дигиталоцеан](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-ubuntu-18-04) с инструкциями по настройке DNS-сервера BIND с открытым исходным кодом.

## <a name="windows-based-setup"></a>Установка на основе Windows

В этих разделах Майкрософт описывается настройка сервера Windows Server в качестве DNS-сервера и в качестве DHCP-сервера.

* [Windows Server как DNS-сервер](/windows-server/networking/dns/dns-top)
* [Windows Server как DHCP-сервер](/windows-server/networking/technologies/dhcp/dhcp-top)
