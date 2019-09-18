---
title: Настройка параметров DNS в файле конфигурации службы | Документация Майкрософт
description: Настройка пользовательских параметров DNS с помощью файла конфигурации службы для виртуальной сети
services: virtual-network
documentationcenter: na
author: genlin
manager: dcscontentpm
editor: tysonn
ms.assetid: 467a4b99-8691-40b3-b640-e25e49675c71
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2016
ms.author: genli
ms.openlocfilehash: f27802d76a8b94a0d5f1eb0c35fd55c93712e557
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/17/2019
ms.locfileid: "71059078"
---
# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>Настройка параметров DNS в файле конфигурации службы
## <a name="dns-elements"></a>Элементы DNS
Файл конфигурации службы может содержать элемент DnsServers со списком IPv4-адресов для серверов доменных имен (DNS-серверов), которые будет использовать служба. Параметры файла конфигурации службы имеют приоритет над параметрами файла конфигурации сети. Дополнительные сведения см. в статье, посвященной [схеме конфигурации службы Azure (файл CSCFG)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**Элемент NetworkConfiguration**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> Атрибут **name** в элементе **DnsServer** используется только в качестве ссылочного имени. Он не представляет имя узла для DNS-сервера. Каждое значение атрибута **DnsServer** должно быть уникальным во всей подписке Microsoft Azure.
> 
> 

## <a name="see-also"></a>См. также
[Схема конфигурации службы Azure (CSCFG-файл)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Схема настройки виртуальной сети Azure](https://go.microsoft.com/fwlink/?LinkId=248093)

[Настройка виртуальной сети с помощью файлов конфигурации сети](https://go.microsoft.com/fwlink/?LinkId=248094)

[Параметры виртуальной сети на портале управления](https://go.microsoft.com/fwlink/?LinkId=248092)

