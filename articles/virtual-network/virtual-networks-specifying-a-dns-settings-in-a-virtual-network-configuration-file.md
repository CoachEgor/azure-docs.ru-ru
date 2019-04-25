---
title: Задание параметров DNS в файле конфигурации виртуальной сети | Документация Майкрософт
description: Как изменить параметры DNS-сервера в виртуальной сети с помощью файла конфигурации виртуальной сети в классической модели развертывания.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
tags: azure-service-management
ms.assetid: a8905927-92ac-42b5-8c33-8e42c000692c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: genli
ms.openlocfilehash: 36f7ed9b02b66718327c1a05a6cf29eedf39e7a5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60232860"
---
# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>Задание параметров DNS в файле конфигурации виртуальной сети
Файл конфигурации сети состоит из двух элементов, которые можно использовать для задания параметров системы доменных имен (DNS): **DnsServers** и **DnsServerRef**. Список DNS-серверов можно добавить, задав их IP-адреса и ссылочные имена в элементе **DnsServers** . После этого в элементе **DnsServerRef** можно указать записи DNS-серверов из элемента DnsServers, которые будут применяться для различных сетевых сайтов в виртуальной сети.

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

В этой статье рассматривается классическая модель развертывания.

Файл конфигурации сети может содержать следующие элементы. Заголовок каждого элемента связан со страницей, содержащей дополнительную информацию о параметрах значений элемента.

> [!IMPORTANT]
> Дополнительные сведения о настройке файла конфигурации сети см. в статье [Настройка виртуальной сети с помощью файла конфигурации сети](virtual-networks-using-network-configuration-file.md). Сведения о каждом элементе, содержащемся в файле конфигурации сети, см. в статье [Схема настройки виртуальной сети Azure](https://msdn.microsoft.com/library/azure/jj157100.aspx).
> 
> 

[Элемент DNS](https://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

> [!WARNING]
> Атрибут **name** в элементе **DnsServer** используется только в качестве ссылки на элемент **DnsServerRef**. Он не представляет имя узла для DNS-сервера. Каждое значение атрибута **DnsServer** должно быть уникальным во всей подписке Microsoft Azure.
> 
> 

[Элемент Virtual Network Sites](https://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

> [!NOTE]
> Чтобы задать этот параметр для элемента Virtual Network Sites, его необходимо определить ранее в элементе DNS. *Имя* DnsServerRef в элементе Virtual Network Sites должно ссылаться на значение имени, указанное в элементе DNS для *имени* DnsServer.
> 
> 

## <a name="next-steps"></a>Дальнейшие действия
* Ознакомьтесь со [схемой конфигурации виртуальной сети Azure](https://go.microsoft.com/fwlink/?LinkId=248093).
* Ознакомьтесь со [схемой конфигурации службы Azure](https://msdn.microsoft.com/library/windowsazure/ee758710).
* [Настройка виртуальной сети с помощью файлов конфигурации сети](virtual-networks-using-network-configuration-file.md).

