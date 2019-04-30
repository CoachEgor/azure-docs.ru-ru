---
title: Функции добавления емкости и расширения функций в Службе приложений Azure| Документация Майкрософт
description: Узнайте, как увеличить масштаб приложения в службе приложений Azure для добавления емкости и расширения функций.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: f7091b25-b2b6-48da-8d4a-dcf9b7baccab
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 98d3d1f6fc0f2f30196f360811808579dfbab312
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60832154"
---
# <a name="scale-up-an-app-in-azure"></a>Увеличение масштаба приложения в Azure

> [!NOTE]
> Новая ценовая категория **PremiumV2** предоставляет более быстрые процессоры, хранилище типа SSD и удвоенное соотношение "память — ядро" по сравнению с существующими ценовыми категориями. В дополнение к более высокой производительности вы можете сэкономить свои деньги, запуская приложения на меньшем числе экземпляров. Для масштабирования до ценовой категории **PremiumV2** см. раздел [Настройка ценовой категории PremiumV2 для службы приложений](app-service-configure-premium-tier.md).
>

В этой статье показано, как масштабировать приложение в службе приложений Azure. Существует два рабочих процесса масштабирования (увеличение масштаба и развертывание), и в этой статье рассматривается первый из них.

* [Увеличение масштаба](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling). Получение дополнительных ресурсов, в частности ЦП, памяти и места на диске, и дополнительных возможностей, таких как выделенные виртуальные машины, личные домены и сертификаты, промежуточные слоты, автоматическое масштабирование и т. д. Чтобы увеличить масштаб приложения, следует изменить ценовую категорию плана службы приложений, к которому относится ваше приложение.
* [Развертывание](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling). Увеличение количества экземпляров виртуальных машин, на которых работает приложение.
  В зависимости от ценовой категории вы можете развернуть приложение на виртуальных машинах в количестве до 20 экземпляров. Использование [сред службы приложений](environment/intro.md) с ценовой категорией **Изолированная** позволит увеличить количество экземпляров до 100. Дополнительные сведения см. в статье [Масштабирование числа экземпляров вручную или автоматически](../monitoring-and-diagnostics/insights-how-to-scale.md). Из нее вы узнаете, как использовать автомасштабирование, которое позволяет масштабировать число экземпляров автоматически на основе предварительно определенных правил и расписаний.

Применение этих параметров масштаба занимает всего несколько секунд, но они влияют на все приложения в вашем [плане службы приложений](../app-service/overview-hosting-plans.md).
Для этого не требуется вносить изменения в код или повторно развертывать приложение.

Сведения о ценах и функциях отдельных планов службы приложений см. [здесь](https://azure.microsoft.com/pricing/details/web-sites/).  

> [!NOTE]
> Прежде чем сменить уровень **Бесплатный** для плана службы приложений, следует снять имеющиеся [ограничения на расходы](https://azure.microsoft.com/pricing/spending-limits/), установленные для вашей подписки Azure. Просмотреть или изменить параметры подписки на службу приложений Microsoft Azure можно на странице [подписок Microsoft Azure][azuresubscriptions].
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a>Увеличение масштаба ценовой категории
1. В браузере откройте [портал Azure][portal].
2. На странице приложения службы приложений щелкните **Все параметры**, а затем — **Увеличить масштаб**.
   
    ![Переход для увеличения масштаба приложения Azure][ChooseWHP]
3. Выберите нужную категорию, а затем щелкните **Применить**.
   
    После завершения операции на вкладке **Уведомления** появится зеленая надпись **Выполнено**.

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a>Масштабирование связанных ресурсов
Если приложение зависит от других служб, таких как база данных SQL Azure или служба хранилища Azure, их можно масштабировать отдельно. Эти ресурсы не находятся под управлением плана службы приложений.

1. В разделе **Основное** щелкните ссылку **Группа ресурсов**.
   
    ![Увеличение масштаба связанных ресурсов приложения Azure](./media/web-sites-scale/RGEssentialsLink.png)
2. В области **Сводка** на странице **Группа ресурсов** щелкните ресурс, который нужно масштабировать. На приведенном ниже снимке экрана показаны ресурс базы данных SQL и ресурс службы хранилища Azure.
   
    ![Переход на страницу группы ресурсов для увеличения масштаба приложения Azure](./media/web-sites-scale/ResourceGroup.png)
3. Для ресурса базы данных SQL щелкните **Параметры** > **Ценовая категория**, чтобы изменить ценовую категорию.
   
    ![Увеличение масштаба внутреннего сервера базы данных SQL для приложения Azure](./media/web-sites-scale/ScaleDatabase.png)
   
    Можно также включить [георепликацию](../sql-database/sql-database-geo-replication-overview.md) для экземпляра базы данных SQL.
   
    Для ресурса службы хранилища Azure щелкните **Параметры** > **Конфигурация**, чтобы изменить возможности хранилища.
   
    ![Увеличение масштаба учетной записи хранения Azure, используемой приложением Azure](./media/web-sites-scale/ScaleStorage.png)

<a name="OtherFeatures"></a>
<a name="devfeatures"></a>

## <a name="compare-pricing-tiers"></a>Сравнение ценовых категорий
Дополнительные сведения, в том числе размеры виртуальных машин для различных ценовых категорий, см. в разделе [Сведения о ценах для службы приложений](https://azure.microsoft.com/pricing/details/web-sites/).
Таблицу лимитов, квот, ограничений и поддерживаемых функций для каждой ценовой категории см. в разделе [Ограничения службы приложений](../azure-subscription-service-limits.md#app-service-limits).

<a name="Next Steps"></a>

## <a name="next-steps"></a>Дальнейшие действия
* Сведения о ценах, поддержке и соглашения об уровне обслуживания см. в следующих разделах:
  
    [Сведения о ценах — передача данных](https://azure.microsoft.com/pricing/details/data-transfers/)
  
    [Планы поддержки Microsoft Azure](https://azure.microsoft.com/support/plans/)
  
    [Соглашения об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/)
  
    [Сведения о ценах — база данных SQL](https://azure.microsoft.com/pricing/details/sql-database/)
  
    [Размеры виртуальных машин и облачных служб для Microsoft Azure][vmsizes]
  
* Рекомендации по использованию Службы приложений Azure, в том числе по созданию масштабируемой и устойчивой архитектуры, см. в записи блога о [рекомендациях по веб-приложениям службы приложений Azure](https://azure.microsoft.com/blog/best-practices-windows-azure-websites-waws/).
* Видео о масштабировании приложений службы приложений см. на следующих ресурсах:
  
  * [Стефан Шаков (Stefan Schackow). Когда следует масштабировать веб-сайты Azure](https://azure.microsoft.com/resources/videos/azure-web-sites-free-vs-standard-scaling/)
  * [Стефан Шаков (Stefan Schackow). Автоматическое масштабирование веб-сайтов Azure, ЦП или по расписанию](https://azure.microsoft.com/resources/videos/auto-scaling-azure-web-sites/)
  * [Стефан Шаков (Stefan Schackow). Как масштабируются веб-сайты Azure](https://azure.microsoft.com/resources/videos/how-azure-web-sites-scale/)

<!-- LINKS -->
[vmsizes]:https://azure.microsoft.com/pricing/details/app-service/
[SQLaccountsbilling]:https://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:https://account.windowsazure.com/subscriptions
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png
