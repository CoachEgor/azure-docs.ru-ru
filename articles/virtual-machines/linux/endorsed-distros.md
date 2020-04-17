---
title: Дистрибутивы Linux, рекомендованные для использования в Azure
description: Узнайте о рекомендованных дистрибутивах Linux в Azure, включая рекомендации по Ubuntu, CentOS, Oracle и SUSE.
services: virtual-machines-linux
documentationcenter: ''
author: gbowerman
manager: gwallace
tags: azure-service-management,azure-resource-manager
ms.assetid: 2777a526-c260-4cb9-a31a-bdfe1a55fffc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 11/04/2019
ms.author: guybo
ms.openlocfilehash: aebb31c825725e8b748fe52bfdf74a79074d6a52
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2020
ms.locfileid: "81460500"
---
# <a name="endorsed-linux-distributions-on-azure"></a>Одобренные дистрибутивы Linux на Azure
Партнеры предоставляют образы Linux в Azure Marketplace. Мы активно сотрудничаем с разными сообществами Linux, чтобы расширить список рекомендованных дистрибутивов. Если дистрибутив Linux недоступен в Marketplace, всегда можно использовать собственный, следуя инструкциям в статье [Информация о нерекомендованных дистрибутивах](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic).

## <a name="supported-distributions-and-versions"></a>Поддерживаемые дистрибутивы и версии
В следующей таблице перечислены дистрибутивы и версии Linux, поддерживаемые в Azure. Для получения более подробной информации о поддержке Linux и технологии с открытым исходным кодом в Azure обратитесь к [службе поддержки изображений Linux в Microsoft Azure.](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure)

Драйверы Linux Integration Services (LIS) для Hyper-V и Azure представляют из себя модули ядра, которые Майкрософт встраивает непосредственно в основное ядро Linux.  Некоторые драйверы LIS встроены в ядро дистрибутива по умолчанию. Более старые дистрибутивы на основе Red Hat Enterprise (RHEL) и CentOS можно отдельно скачать на странице [служб интеграции Linux (LIS) 4.2 для Hyper-V и Azure](https://www.microsoft.com/download/details.aspx?id=55106). Дополнительные сведения о драйверах LIS см. в разделе [Рекомендации по ядру Linux](create-upload-generic.md#linux-kernel-requirements).

Агент Linux для Azure уже предварительно установлен в образах Azure Marketplace и обычно доступен из репозитория пакета дистрибутива. Исходный код можно найти на сайте [GitHub](https://github.com/azure/walinuxagent).


| Distribution | Версия | Драйверы | Агент |
| --- | --- | --- | --- |
| CentOS |CentOS 6,3 ", 7,0", 8,0 " |CentOS 6.3: [LIS скачать](https://www.microsoft.com/download/details.aspx?id=55106)<p>CentOS 6.4 и более поздних версий: в ядре |Пакет: В [репо](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) под "WALinuxAgent" <br/>Исходный код: [GitHub](https://github.com/Azure/WALinuxAgent) |
| [CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) |494.4.0 + |В ядре |Исходный код: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent) |
| Debian |Debian 7,9 ", 8,2 ", 9, 10 |В ядре |Пакет: в репозитории в разделе WAAgent  <br/>Исходный код: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Oracle Linux |6.4+, 7.0+ |В ядре |Пакет: в репозитории в разделе WALinuxAgent  <br/>Исходный код: [GitHub](https://go.microsoft.com/fwlink/p/?LinkID=250998) |
| Red Hat Enterprise Linux |RHEL 6.7 ", 7.1 ", 8.0" |В ядре |Пакет: в репозитории в разделе WALinuxAgent  <br/>Исходный код: [GitHub](https://github.com/Azure/WALinuxAgent) |
| SUSE Linux Enterprise |SLES или SLES for SAP<br>11 SP4<br>12 SP1 или более поздняя версия<br>15|В ядре |Пакет:<p> для версии 11: в репозитории [Cloud:Tools](https://build.opensuse.org/project/show/Cloud:Tools);<br>для версии 12: входит в состав модуля Public Cloud в python-azure-agent.<br/>Исходный код: [GitHub](https://go.microsoft.com/fwlink/p/?LinkID=250998) |
| openSUSE |openSUSE LEAP 42.2+ |В ядре |Пакет: в репозитории [Cloud:Tools](https://build.opensuse.org/project/show/Cloud:Tools) под именем python-azure-agent <br/>Исходный код: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Ubuntu |Ubuntu 12.04+ **<sup>1</sup>** |В ядре |Пакет: в репозитории в разделе WALinuxAgent  <br/>Исходный код: [GitHub](https://github.com/Azure/WALinuxAgent) |

  - **<sup>1</sup>** Информацию о расширенной поддержке Ubuntu 12.04 и 14.04 можно найти здесь: [Ubuntu Расширенное техническое обслуживание безопасности](https://www.ubuntu.com/esm).


## <a name="image-update-cadence"></a>Каденция обновления изображения
Azure требует, чтобы издатели одобренных дистрибутивов Linux регулярно обновляли свои изображения в Azure Marketplace с последними исправлениями и исправлениями безопасности, ежеквартально или быстрее. Обновленные изображения в Azure Marketplace автоматически доступны клиентам в качестве новых версий изображения SKU. Более подробная информация о том, как найти изображения Linux: [Найти Linux VM изображения в Azure Marketplace](https://docs.microsoft.com/azure/virtual-machines/linux/cli-ps-findimage).

### <a name="additional-links"></a>Дополнительные ссылки
 - [SUSE Общедоступный облачный жизненный цикл](https://www.suse.com/c/suse-public-cloud-image-life-cycle/)

## <a name="azure-tuned-kernels"></a>Ядра, настроенные лазурное,

Azure тесно сотрудничает с различными одобренными дистрибутивами Linux для оптимизации изображений, которые они публикуют на Azure Marketplace. Одним из аспектов этого сотрудничества является разработка «настроенных» ядер Linux, которые оптимизированы для платформы Azure и поставляются в качестве полностью поддерживаемых компонентов дистрибутива Linux. Ядра Azure-Tuned включают новые функции и улучшения производительности, а также более быструю (обычно ежеквартальную) каденцию по сравнению с ядрами по умолчанию или общими ядрами, которые доступны из дистрибутива.

В большинстве случаев эти ядра, предварительно установленные на изображениях по умолчанию, находятся в Azure Marketplace, и поэтому клиенты Azure сразу же получат выгоду от этих оптимизированных ядер. Более подробную информацию об этих ядрах Azure-Tuned можно найти в следующих ссылках:

 - Ядро CentOS Azure-Tuned - Доступно через SIG виртуализации CentOS - [Подробнее](https://wiki.centos.org/SpecialInterestGroup/Virtualization)
 - Ядро облака Debian - Доступно с изображением Debian 10 и Debian 9 на Azure - [Подробнее](https://wiki.debian.org/Cloud/MicrosoftAzure)
 - SLES Лазурно-настроенное ядро - [Подробнее](https://www.suse.com/c/a-different-builtin-kernel-for-azure-on-demand-images/)
 - Ubuntu Azure-Настроенное ядро - [Подробнее](https://blog.ubuntu.com/2017/09/21/microsoft-and-canonical-increase-velocity-with-azure-tailored-kernel)


## <a name="partners"></a>Участники

### <a name="coreos"></a>CoreOS
[https://coreos.com/docs/running-coreos/cloud-providers/azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

На веб-сайте CoreOS.

*Операционная система CoreOS обеспечивает безопасность, согласованность и надежность при работе. В CoreOS пакеты не устанавливаются через yum или apt. Для управления службами на более высоком уровне абстракции используются контейнеры Linux. Единый код службы и все зависимости упакованы в контейнер, который может выполняться на одном или нескольких компьютерах под управлением CoreOS.*

### <a name="credativ"></a>Credativ
[https://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure](https://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ представляет собой независимую компанию, оказывающую консалтинговые и прочие услуги, которая специализируется на разработке и реализации профессиональных решений с помощью бесплатного программного обеспечения. Эксперты Credative получили международное признание многих использующих наши услуги ИТ-отделов как ведущие специалисты по открытому коду. Сейчас Credativ совместно с Майкрософт готовит соответствующие образы Debian для Debian 8 (Jessie) и версий, предшествующих Debian 7 (Wheezy). Они специально разработаны для работы в Azure, и ими можно легко управлять с помощью этой платформы. Credativ также поддерживает долгосрочное обслуживание и обновление образов Debian для Azure с помощью своих центров поддержки открытого кода.

### <a name="oracle"></a>Oracle;
[https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html](https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Oracle предлагает широкий набор решений для общедоступных и частных облаков. Это дает клиентам свободу выбора и гибкость при развертывании программного обеспечения Oracle в облаках Oracle, а также в других облаках. Партнерство Oracle с корпорацией Майкрософт позволяет клиентам развертывать программное обеспечение Oracle в общедоступных и частных облаках Майкрософт при обеспечении сертификации и поддержки от Oracle.  Заинтересованность компании Oracle и ее инвестиции в решения для общедоступных и частных облаков Oracle остаются неизменными.

### <a name="red-hat"></a>Red Hat
[https://www.redhat.com/en/partners/strategic-alliance/microsoft](https://www.redhat.com/en/partners/strategic-alliance/microsoft)

Как ведущий мировой поставщик решений с открытым исходным кодом, Red Hat помогает более чем 90 % компаний, входящих в список Fortune 500, решать их бизнес-задачи, оптимизировать ИТ- и бизнес-стратегии и готовиться к дальнейшему развитию технологий. Для этого Red Hat предлагает защищенные решения, используя открытую бизнес-модель и доступную, прогнозируемую модель подписки.

### <a name="suse"></a>SUSE
[https://www.suse.com/suse-linux-enterprise-server-on-azure](https://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server в Azure — проверенная платформа, предоставляющая высокую степень надежности и безопасности для работы в облаке. Универсальная платформа SUSE Linux легко интегрируется с облачными службами Azure, создавая облачную среду с простым управлением. Свыше 9200 сертифицированных приложений для SUSE Linux Enterprise Server от более чем 1800 независимых поставщиков программного обеспечения гарантируют, что рабочие нагрузки, поддерживаемые в центре обработки данных, могут быть беспрепятственно развернуты в Azure.

### <a name="canonical"></a>Canonical
[https://www.ubuntu.com/cloud/azure](https://www.ubuntu.com/cloud/azure)

Инженерно-техническое открытое сообщество Canonical способствует успешному распространению Ubuntu в клиентских, серверных и облачных средах, включая персональные облачные службы для потребителей. Canonical представляет Ubuntu как единую бесплатную платформу, используемую в различных средах, от телефона до облака, с семейством согласованных интерфейсов для телефонов, планшетов, телевизоров и настольных компьютеров. Это делает Ubuntu лучшим вариантом для различных учреждений, от поставщиков общедоступных облаков до создателей бытовой электроники, и фаворитом среди частных технологических решений.

Благодаря центрам разработки и проектирования по всему миру Canonical имеет уникальные возможности для партнерства с производителями оборудования, поставщиками содержимого и разработчиками программного обеспечения для вывода на рынок решений Ubuntu для разных сред, от компьютеров до серверов и портативных устройств.
