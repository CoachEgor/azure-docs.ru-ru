---
title: включение файла
description: включение файла
services: app-service
author: ggailey777
ms.service: app-service
ms.topic: include
ms.date: 02/19/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 14f2e663e3db81684a73c4ea093ed0403cbb09ed
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185445"
---
> [!NOTE]
> Время ожидания веб-приложения может истечь после 20 минут бездействия. Запросы к фактическому сайту сбрасывают таймер. Просмотр конфигурации приложения на портале Azure или выполнения запросов к сайту расширенные средства (`https://<app_name>.scm.azurewebsites.net`) не сбрасывают таймер. Если приложение выполняет непрерывные веб-задания или веб-задания по расписанию, включите **Всегда включено** для обеспечения надежного выполнения веб-заданий. Эта функция доступна только в [ценовых категориях](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) "Базовый", "Стандартный" и "Премиум".
