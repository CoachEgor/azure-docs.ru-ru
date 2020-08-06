---
title: Краткое руководство. Создание зоны и записи Azure DNS — Azure CLI
titleSuffix: Azure DNS
description: Краткое руководство. Создание зоны и записи DNS в Azure DNS. В этом пошаговом руководстве описано, как создать зону и запись DNS, а также управлять ими с помощью Azure CLI.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: quickstart
ms.date: 3/11/2019
ms.author: rohink
ms.custom: devx-track-azurecli
ms.openlocfilehash: a15209351f95e44c7cf10993513e03ba35b53d27
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/31/2020
ms.locfileid: "87494372"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-azure-cli"></a>Краткое руководство. Создание зоны и записи Azure DNS с помощью Azure CLI

В этой статье описано, как создать зону и запись DNS, используя Azure CLI, доступный для Windows, Mac и Linux. Эти действия также можно выполнить с помощью [портала Azure](dns-getstarted-portal.md) или [Azure PowerShell](dns-getstarted-powershell.md).

Зона DNS используется для размещения DNS-записей определенного домена. Чтобы разместить свой домен в Azure DNS, необходимо создать зону DNS для этого доменного имени. Каждая запись DNS для вашего домена создается внутри этой зоны DNS. Наконец, чтобы опубликовать зону DNS в Интернете, необходимо настроить серверы доменных имен для домена. Каждый из этих шагов описан ниже.

Azure DNS также поддерживает частные зоны DNS. Дополнительные сведения об использовании частных зон DNS см. в статье [Using Azure DNS for private domains](private-dns-overview.md) (Использование Azure DNS для частных доменов). Дополнительные сведения о создании частной зоны DNS см. в руководстве по [началу работы с частными зонами Azure DNS с использованием CLI](./private-dns-getstarted-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="create-the-resource-group"></a>Создание группы ресурсов

Перед созданием зоны DNS создайте группу ресурсов, которая будет включать эту зону DNS:

```azurecli
az group create --name MyResourceGroup --location "East US"
```

## <a name="create-a-dns-zone"></a>Создание зоны DNS

Зона DNS создается с помощью команды `az network dns zone create`. Чтобы просмотреть справку для этой команды, введите `az network dns zone create -h`.

В следующем примере будет создана зона DNS *contoso.xyz* в группе ресурсов *MyResourceGroup*. Пример можно использовать для создания зоны DNS, заменив соответствующие значения собственными.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.xyz
```

## <a name="create-a-dns-record"></a>Создание записи DNS

Чтобы создать запись DNS, используйте команду `az network dns record-set [record type] add-record`. Справочные сведения о записях А см. здесь: `azure network dns record-set A add-record -h`.

В следующем примере создается запись с относительным именем www в зоне DNS contoso.xyz в группе ресурсов MyResourceGroup. Полное доменное имя набора записей — "www.contoso.xyz". Используется тип записи A с IP-адресом 10.10.10.10 и сроком жизни по умолчанию 3600 секунд (1 час).

```azurecli
az network dns record-set a add-record -g MyResourceGroup -z contoso.xyz -n www -a 10.10.10.10
```

## <a name="view-records"></a>Просмотр записей

Чтобы просмотреть список записей DNS в зоне, используйте следующую команду:

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.xyz
```

## <a name="test-the-name-resolution"></a>Тестирование разрешения имен

Теперь, когда у вас есть тестовая зона DNS с тестовой записью "A", можно протестировать разрешение имен с помощью инструмента *nslookup*. 

**Тестирование разрешения DNS-имени.**

1. Выполните следующий командлет, чтобы получить список серверов имен для вашей зоны.

   ```azurecli
   az network dns record-set ns show --resource-group MyResourceGroup --zone-name contoso.xyz --name @
   ```

1. Скопируйте имя одного из серверов доменных имен из выходных данных, полученных на предыдущем этапе.

1. Откройте командную строку и выполните приведенную ниже команду.

   ```
   nslookup www.contoso.xyz <name server name>
   ```

   Пример:

   ```
   nslookup www.contoso.xyz ns1-08.azure-dns.com.
   ```

   Результат будет подобным приведенному на следующем снимке экрана.

   ![nslookup](media/dns-getstarted-portal/nslookup.PNG)

Имя узла **www\.contoso.xyz** разрешается как **10.10.10.10**, как и было настроено. Такой результат подтверждает, что разрешение имен работает правильно.

## <a name="delete-all-resources"></a>Удаление всех ресурсов

Чтобы очистить ненужные ресурсы, созданные при работе с этим кратким руководством, удалите группу ресурсов:

```azurecli
az group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы создали свою первую зону и запись DNS с помощью Azure CLI, можно создать записи для веб-приложения в личном домене.

> [!div class="nextstepaction"]
> [Создание записей DNS для веб-приложения в пользовательском домене](./dns-web-sites-custom-domain.md)
