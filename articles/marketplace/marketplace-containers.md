---
title: Руководство по публикации предложения "Контейнеры" в Azure Marketplace
description: В этой статье описываются требования к публикации предложения "Контейнеры" в Marketplace.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: ellacroi
manager: nunoc
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 07/09/2018
ms.author: ellacroi
ms.openlocfilehash: ffdc6b22209c1d334ac7b75c7079e755a7405154
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/08/2019
ms.locfileid: "73822994"
---
# <a name="containers-offer-publishing-guide"></a>Руководство по публикации предложения "Контейнеры"

Предложение "Контейнеры" поможет опубликовать образ контейнера в Azure Marketplace. Сведения в этом руководстве помогут понять требования к этому предложению. 

Ниже приведены предложения транзакций с развертыванием и выставлением счетов через Marketplace. Пользователь видит призыв к действию — "Получить сейчас".

Используйте тип предложения "Контейнер", если ваше решение представляет собой образ контейнера Docker, подготовленный в виде службы контейнеров Azure на основе Kubernetes.

>[!NOTE]
>Например, это может быть "Служба Azure Kubernetes" или "Экземпляры контейнеров Azure" (самая популярная среди пользователей Azure в случае среды выполнения контейнеров на основе Kubernetes).  

В настоящее время корпорация Майкрософт поддерживает свободное лицензирование и модель с использованием собственной лицензии (BYOL).

## <a name="containers-offer"></a>Предложение "Контейнеры"

| Требование | Сведения |  
|:--- |:--- |  
| Выставление счетов и ценообразование | Поддерживается либо бесплатная, либо модель выставления счетов BYOL. |  
| Образ, созданный из файла Dockerfile | Образы контейнеров должны соответствовать спецификации образов Docker и создаваться из файла Dockerfile.<ul> <li>Дополнительные сведения о создании образов Docker см. в разделе об использовании по адресу [docs.docker.com/engine/reference/builder/#usage](https://docs.docker.com/engine/reference/builder/#usage).</li> </ul> |  
| Размещение в ACR | Образы контейнеров должны размещаться в репозитории в Реестре контейнеров Azure (ACR).<ul> <li>Дополнительные сведения о работе с ACR см. на странице "Краткое руководство. Создание реестра контейнеров на портале Azure" по адресу [docs.microsoft.com/azure/container-registry/container-registry-get-started-portal](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-portal).</li> </ul> |  
| Добавление тегов к образам | У образа контейнера должен быть хотя бы один тег (но не более 16).<ul> <li>Дополнительные сведения о пометке образа тегами см. на странице тегов Docker по адресу [docs.docker.com/engine/reference/commandline/tag](https://docs.docker.com/engine/reference/commandline/tag).</li> </ul> |  

## <a name="next-steps"></a>Дальнейшие действия

Если это еще не сделано, 

- [зарегистрируйтесь](https://azuremarketplace.microsoft.com/sell) в Marketplace.

Если вы зарегистрированы и создаете предложение или работаете над существующим,

- [войдите на Портал Cloud Partner](https://cloudpartner.azure.com), чтобы создать или завершить предложение.
- Дополнительные сведения см. в статье [Контейнеры](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/containers/cpp-containers-offer).
