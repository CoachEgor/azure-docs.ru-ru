---
title: включить файл
description: включить файл
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/05/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: bb6f2281cc115904db6babcf31f1af387b254d48
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94426574"
---
## <a name="trusted-microsoft-services"></a>Доверенные службы Майкрософт
При включении параметра **разрешить доверенным службам Майкрософт обходить этот параметр брандмауэра** доступ к ресурсам служебной шины предоставляется следующим службам.

| Доверенная служба | Поддерживаемые сценарии использования | 
| --------------- | ------------------------- | 
| Сетка событий Azure | Позволяет службе "Сетка событий Azure" отсылать события в очереди или разделы в пространстве имен служебной шины. Также необходимо выполнить следующие действия. <ul><li>Включение назначенного системой удостоверения для раздела или домена</li><li>Добавление удостоверения в роль отправителя данных служебной шины Azure в пространстве имен служебной шины</li><li>Затем настройте подписку на события, использующую очередь или раздел служебной шины в качестве конечной точки для использования назначенного системой удостоверения.</li></ul> <p>Дополнительные сведения см. в статье о [доставке событий с помощью управляемого удостоверения](../articles/event-grid/managed-service-identity.md) .</p>|
