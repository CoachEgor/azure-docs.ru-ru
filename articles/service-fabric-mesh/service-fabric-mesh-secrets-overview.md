---
title: Хранение и использование секретов приложения сети Service Fabric Azure
description: Служба "Сетка Service Fabric" поддерживает секреты в качестве ресурсов Azure. Вот как можно хранить секреты и управлять ими с помощью приложений Service Fabricной сетки.
author: erikadoyle
ms.author: edoyle
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: da4eaa34840f14714616b85e24fd62cf65602b84
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "76277637"
---
# <a name="service-fabric-mesh-application-secrets"></a>Секреты приложения в службе "Сетка Service Fabric"
Служба "Сетка Service Fabric" поддерживает секреты в качестве ресурсов Azure. Секретом в службе "Сетка Service Fabric" может быть любая конфиденциальная текстовая информация, например строки подключения к хранилищу, пароли или другие значения, которые должны хранится и передаваться защищенными.

![Общие сведения секретах в Сетке][sf-mesh-secrets-overview]

## <a name="mesh-secrets-resources"></a>Ресурсы секретов Сетки
Секрет приложения в Сетке состоит из следующих компонентов:
* Ресурс **Секреты** — это контейнер, в котором хранятся секреты в текстовом формате. Секреты, находящиеся в пределах ресурса **Секреты**, хранятся и передаются в защищенном виде.
* Один или несколько ресурсов **Секреты** (или Значения), которые хранятся в контейнере ресурса **Секреты**. Каждый ресурс **Секреты** (или Значения) отличается номером версии.

## <a name="next-steps"></a>Дальнейшие действия 
Дополнительные сведения о секретах в службе "Сетка Service Fabric".
- [Управление секретами приложения "Сетка Service Fabric"](service-fabric-mesh-howto-manage-secrets.md)
- [Общие сведения о модели ресурсов Service Fabric](service-fabric-mesh-service-fabric-resources.md)

<!-- pics -->
[sf-mesh-secrets-overview]: ./media/service-fabric-mesh-secrets-overview/MeshAppSecretsOverview.png
