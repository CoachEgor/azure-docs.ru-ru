---
title: Теги службы группы безопасности сети (NSG) для Azure HDInsight
description: Используйте теги службы HDInsight, чтобы разрешить входящий трафик в кластер из узлов служб работоспособности и управления, не добавляя IP-адреса в группы безопасности сети.
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 08/11/2020
ms.openlocfilehash: 1a90bc6636dcb3aa81f09b0489850c1a95b3256d
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96022757"
---
# <a name="nsg-service-tags-for-azure-hdinsight"></a>Теги службы NSG для Azure HDInsight

Теги службы Azure HDInsight для групп безопасности сети (группы безопасности сети) представляют собой группы IP-адресов для служб работоспособности и управления. Эти группы помогают сократить сложность создания правил безопасности. [Теги службы](../virtual-network/network-security-groups-overview.md#service-tags) разрешают входящий трафик от конкретных IP [-адресов](hdinsight-management-ip-addresses.md) , не вводя в группы безопасности сети каждый адрес управления.

Служба HDInsight управляет этими тегами службы. Нельзя создать собственный тег службы или изменить существующий тег. Корпорация Майкрософт управляет префиксами адресов, которые соответствуют тегу службы, и автоматически обновляет тег службы при изменении адресов.

Если вы хотите использовать определенный регион и тег службы еще не документирован на этой странице, можно использовать [API обнаружения тегов службы](../virtual-network/service-tags-overview.md#use-the-service-tag-discovery-api-public-preview) для поиска тега службы. Можно также скачать [JSON тег службы](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files) и найти нужный регион.

## <a name="get-started-with-service-tags"></a>Начало работы с тегами службы

Существует два варианта использования тегов службы в группах безопасности сети.

- **Использовать один глобальный тег службы hdinsight**. Этот параметр открывает виртуальную сеть для всех IP-адресов, которые служба HDInsight использует для наблюдения за кластерами во всех регионах. Этот вариант является простейшим, но может быть не подходящим, если имеются требования к безопасности.

- **Использовать несколько региональных тегов служб**. Этот параметр открывает виртуальную сеть только для IP-адресов, используемых HDInsight в определенном регионе. Однако если вы используете несколько регионов, необходимо добавить несколько тегов службы в виртуальную сеть.

## <a name="use-a-single-global-hdinsight-service-tag"></a>Использовать один глобальный тег службы HDInsight

Самый простой способ начать использовать теги службы с кластером HDInsight — добавить глобальный тег `HDInsight` к правилу NSG.

1. В [портал Azure](https://portal.azure.com/)выберите группу безопасности сети.

1. В разделе **Параметры** выберите **правила безопасности для входящего трафика**, а затем щелкните **+ добавить**.

1. В раскрывающемся списке **источник** выберите **тег службы**.

1. В раскрывающемся списке **тег службы источника** выберите **HDInsight**.

    ![Добавление тега службы из портал Azure](./media/hdinsight-service-tags/azure-portal-add-service-tag.png)

Этот тег содержит IP-адреса служб работоспособности и управления для всех регионов, в которых доступна служба HDInsight. Тег обеспечит взаимодействие кластера с необходимыми службами работоспособности и управления независимо от того, где она создана.

## <a name="use-regional-hdinsight-service-tags"></a>Использование региональных тегов службы HDInsight

Если параметр Global TAG не будет работать из-за необходимости более узких разрешений, можно разрешить только теги службы, применимые к региону. В зависимости от региона, в котором создан кластер, может существовать несколько тегов служб.

Чтобы узнать, какие теги службы следует добавить в ваш регион, ознакомьтесь со следующими разделами статьи.

### <a name="use-a-single-regional-service-tag"></a>Использование одного регионального тега службы

Если кластер находится в регионе, указанном в этой таблице, необходимо добавить в NSG только один региональный тег службы.

| Country | Region | Тег службы |
| ---- | ---- | ---- |
| Австралия | Восточная Австралия | HDInsight. AustraliaEast |
| &nbsp; | Australia Southeast | HDInsight. AustraliaSoutheast |
| &nbsp; | Центральная Австралия | HDInsight. Аустралиацентрал |
| Китай | Восточный Китай 2 | HDInsight. ChinaEast2 |
| &nbsp; | Северный Китай 2 | HDInsight. ChinaNorth2 |
| США | Центрально-северная часть США | HDInsight. NorthCentralUS |
| &nbsp; | Западная часть США 2 | HDInsight. WestUS2 |
| &nbsp; | центрально-западная часть США | HDInsight. WestCentralUS |
| Canada | Восточная Канада | HDInsight. Канадаеаст |
| Бразилия | Южная Бразилия | HDInsight. Бразилсаус |
| Корея | Республика Корея, центральный регион | HDInsight. Кореацентрал |
| &nbsp; | Республика Корея, южный регион | HDInsight. Кореасаус |
| Индия | Центральная Индия | HDInsight. Централиндиа |
| &nbsp; | Южная Индия | HDInsight. Саусиндиа |
| Япония | Западная Япония | HDInsight. Жапанвест |
| Франция | Центральная Франция| HDInsight. Францецентрал |
| Соединенное Королевство | южная часть Соединенного Королевства | HDInsight. UKSouth |
| Azure для государственных организаций | Центральная часть США (DoD) | HDInsight. Усдодцентрал |
| &nbsp; | US Gov (Техас) | HDInsight. Усговтексас |
| &nbsp; | Восточная Восток | HDInsight. Усдодеаст |
| &nbsp; | US Gov (Аризона) | HDInsight. Усговаризона |

### <a name="use-multiple-regional-service-tags"></a>Использование нескольких региональных тегов служб

Если регион, в котором создан кластер, отсутствует в приведенной выше таблице, необходимо разрешить несколько региональных тегов служб. Необходимость в использовании более одного из них заключается в том, что для различных регионов существуют различия в упорядочении поставщиков ресурсов.

Остальные регионы делятся на группы в зависимости от используемых тегов региональных служб.

#### <a name="group-1"></a>Группа 1

Если кластер создается в одном из регионов в следующей таблице, разрешите Теги службы `HDInsight.WestUS` и `HDInsight.EastUS` . Кроме того, в списке указан тег регионального сервиса. Для регионов в этом разделе требуется три тега службы.

Например, если кластер создается в `East US 2` регионе, необходимо добавить следующие теги службы в группу безопасности сети:

- `HDInsight.EastUS2`
- `HDInsight.WestUS`
- `HDInsight.EastUS`

| Country | Region | Тег службы |
| ---- | ---- | ---- |
| США | восточная часть США 2 | HDInsight. EastUS2 |
| &nbsp; | Центральная часть США | HDInsight. CentralUS |
| &nbsp; | Норсцентрал США | HDInsight. NorthCentralUS |
| &nbsp; | Центрально-южная часть США | HDInsight. SouthCentralUS |
| &nbsp; | Восточная часть США | HDInsight. EastUS |
| &nbsp; | западная часть США | HDInsight. WestUS |
| Япония | Восточная Япония | HDInsight. Жапанеаст |
| Европа | Северная Европа | HDInsight. Норсеуропе |
| &nbsp; | Западная Европа| HDInsight. WestEurope |
| Азия | Восточная Азия | HDInsight. EastAsia |
| &nbsp; | Southeast Asia | HDInsight. SoutheastAsia |
| Австралия | Восточная Австралия | HDInsight. AustraliaEast |

#### <a name="group-2"></a>Группа 2

Кластеры в регионах *Северный Китай* и *Восточный Китай* должны разрешать два тега службы: `HDInsight.ChinaNorth` и `HDInsight.ChinaEast` .

#### <a name="group-3"></a>Группа 3

Кластеры в регионах *US gov (Айова)* и *US gov (Вирджиния)* должны разрешать два тега службы: `HDInsight.USGovIowa` и `HDInsight.USGovVirginia` .

#### <a name="group-4"></a>Группа 4

Для кластеров в регионах *центрального региона Германии* и *северо в Германии* необходимо разрешить два тега службы: `HDInsight.GermanyCentral` и `HDInsight.GermanyNortheast` .

## <a name="next-steps"></a>Следующие шаги

- [Группы безопасности сети: Теги служб](../virtual-network/network-security-groups-overview.md#security-rules)
- [Создание виртуальных сетей для кластеров Azure HDInsight](hdinsight-create-virtual-network.md)