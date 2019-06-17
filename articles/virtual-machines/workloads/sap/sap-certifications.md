---
title: Сертификация Microsoft Azure для SAP | Документация Майкрософт
description: Обновленный список текущих конфигураций и сертификатов SAP на платформе Azure.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/15/2019
ms.author: rclaus
ms.custom: ''
ms.openlocfilehash: a38d5b44aaca0aa28e7ecd32543d2b0563964b3d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65797445"
---
# <a name="sap-certifications-and-configurations-running-on-microsoft-azure"></a>Сертификаты и конфигурации SAP на платформе Microsoft Azure

Компания SAP и корпорация Майкрософт имеют долгую историю плодотворного сотрудничества, которое обеспечивает взаимные преимущества для их клиентов. Корпорация Майкрософт регулярно обновляет свою платформу и отправляет компании SAP сведения о новых сертификатах. Тем самым гарантируется, что Microsoft Azure остается лучшей платформой для выполнения рабочих нагрузок SAP. В следующих таблицах представлены поддерживаемые конфигурации Azure и список сертификатов SAP, который регулярно пополняется. 

## <a name="sap-hana-certifications"></a>Сертификаты SAP HANA
Справочные материалы:

- Сведения о поддержке SAP HANA для собственных виртуальных машин Azure и SAP HANA (крупные экземпляры) приведены в [списке сертифицированных платформ IaaS для SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

| Продукт SAP | Поддерживаемая ОС | Предложения Azure |
| --- | --- | --- |
| SAP HANA, версия для разработчиков (включая клиентское ПО HANA с драйверами SQLODBC, ODBO (только для ОС Windows), ODBC и JDBC, среду разработки HANA Studio и базу данных HANA) | Red Hat Enterprise Linux, SUSE Linux Enterprise | Семейство виртуальных машин серии D |
| Business One в HANA | SUSE Linux Enterprise | DS14_v2, M32ts, M32ls, M64ls, M64s <br /> [Платформы IaaS, сертифицированные для SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure%23SAP%20Business%20One) |
| SAP S/4 HANA | Red Hat Enterprise Linux, SUSE Linux Enterprise | Управляемая доступность для GS5. Полная поддержка M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2, <br /> SAP HANA (крупные экземпляры) в Azure. [Платформы IaaS, сертифицированные для SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) |
| Suite on HANA, OLTP | Red Hat Enterprise Linux, SUSE Linux Enterprise | От M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2, SAP HANA в Azure (крупные экземпляры) <br /> [Платформы IaaS, сертифицированные для SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) |
| HANA Enterprise для BW, OLAP | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5, от M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2, <br /> SAP HANA (крупные экземпляры) в Azure. [Платформы IaaS, сертифицированные для SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) |
| SAP BW/4 HANA | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5, от M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2, <br /> SAP HANA в Azure (крупные экземпляры) <br /> [Платформы IaaS, сертифицированные для SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) |

Обратите внимание, что термин "кластеризация" на странице с [платформами IaaS, сертифицированными для SAP HAN](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure), используется как синоним слова "горизонтальное масштабирование". Здесь не имеется в виду кластеризация с высоким уровнем доступности.

## <a name="sap-netweaver-certifications"></a>Сертификация SAP NetWeaver
Платформа Microsoft Azure сертифицирована для следующих продуктов SAP при полной поддержке Майкрософт и SAP.
Справочные материалы:

- [1928533 - приложения SAP в Azure: Поддерживаемые продукты и типы виртуальных Машин Azure](https://launchpad.support.sap.com/#/notes/1928533) для всех приложений на основе SAP NetWeaver, включая SAP TREX, SAP LiveCache и сервер содержимого SAP. Здесь также содержится список всех баз данных, кроме SAP HANA.


| Продукт SAP | Гостевая ОС | Реляционная СУБД | Типы виртуальных машин |
| --- | --- | --- | --- |
| ПО SAP Business Suite | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (только Windows и Oracle Linux), DB2, SAP ASE |От a5 до A11, D11 – D14, DS11 – DS14, от DS11_v2 до DS15_v2, GS1 – GS5, от D2s_v3 до D64s_v3, от E2s_v3 до E64s_v3, от M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2 |
| SAP Business All-in-One | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (только Windows и Oracle Linux), DB2, SAP ASE |От a5 до A11, D11 – D14, DS11 – DS14, от DS11_v2 до DS15_v2, GS1 – GS5, от D2s_v3 до D64s_v3, от E2s_v3 до E64s_v3, от M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2 |
| Бизнес-аналитика SAP BusinessObjects | Windows |Н/Д |От a5 до A11, D11 – D14, DS11 – DS14, от DS11_v2 до DS15_v2, GS1 – GS5, от D2s_v3 до D64s_v3, от E2s_v3 до E64s_v3, от M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2 |
| SAP NetWeaver | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (только Windows и Oracle Linux), DB2, SAP ASE |От a5 до A11, D11 – D14, DS11 – DS14, от DS11_v2 до DS15_v2, GS1 – GS5, от D2s_v3 до D64s_v3, от E2s_v3 до E64s_v3, от M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2 |

## <a name="other-sap-workload-supported-on-azure"></a>Другие рабочие нагрузки SAP, поддерживаемые в Azure

| Продукт SAP | Гостевая ОС | Реляционная СУБД | Типы виртуальных машин |
| --- | --- | --- | --- |
| SAP Business One на SQL Server | Windows  | SQL Server; | Все типы виртуальных машин, сертифицированные NetWeaver<br /> [Примечание к SAP № 928839](https://launchpad.support.sap.com/#/notes/928839) |
| SAP BPC 10.01 MS SP08 | Windows и Linux | | Все типы виртуальных машин, сертифицированные NetWeaver<br /> Примечание к SAP № 2451795 |
| Платформа SAP BusinessObjects BI | Windows и Linux | | Примечание к SAP № 2145537 |
| SAP Data Services 4.2 | | | Примечание к SAP № 2288344 |
| SAP Hybris Commerce Platform 5.x и 6.x | Windows | SQL Server, Oracle | Все типы виртуальных машин, сертифицированные NetWeaver<br /> [Вики-сайт Hybris](https://wiki.hybris.com/display/SUP/Using+the+hybris+Platform+with+the+Cloud) |
