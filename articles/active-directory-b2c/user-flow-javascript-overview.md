---
title: JavaScript и страница "Версии контракта" для потоков пользователя в Azure Active Directory B2C | Документация Майкрософт
description: Узнайте, как включить JavaScript и использовать страницу "Версии контракта", чтобы настроить поток пользователя в Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 5102755c9e830f43fa92e8546e5125960e0a2f9a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60360254"
---
# <a name="about-using-javascript-and-page-contract-versions-in-a-user-flow"></a>Использование JavaScript и страницы "Версии контракта" в потоке пользователя

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure AD B2C предоставляет набор упакованного содержимого, который содержит HTML, CSS и JavaScript, для элементов пользовательского интерфейса в ваших потоках пользователя. Если вы планируете включить код, исполняемый на клиенте [JavaScript](javascript-samples.md) в своих пользовательских потоках, необходимо убедиться, что элементы, на которых основан JavaScript, являются неизменными. В противном случае любые изменения могут вызвать непредвиденное поведение на страницах потока пользователя. Чтобы избежать этого, вы можете применить использование контракта на странице для потока пользователя и указать страницу "Версии контракта". Это гарантирует, что все определения содержимого, на которых основан JavaScript, останутся неизменяемыми. Даже если у вас нет намерения включить JavaScript для потока пользователя, вы можете указать страницу "Версии контракта" для страниц потока пользователя.

> [!NOTE]
> В этой статье рассматривается JavaScript для потоков пользователя, но вы также можете использовать JavaScript и выбирать страницу "Версии контракта" во время использования [пользовательских политик](page-contract.md).

## <a name="enable-javascript"></a>Включение JavaScript

В свойствах потока пользователя вы можете включить JavaScript, который также применит использование контракта на странице. Затем вы можете задать страницу "Версии контракта", как описано в следующем разделе.

![Включение параметра JavaScript](media/user-flow-javascript-overview/javascript-settings.PNG)

## <a name="specify-a-page-contract-version"></a>Указание страницы "Версии контракта"

Даже если вы не включили JavaScript в свойствах потока пользователя, вы можете указать страницу "Версии контракта" для страниц потока пользователя. Откройте поток пользователя и выберите **Макеты страниц**. В разделе **Имя макета** выберите страницу потока пользователя и **страницу "Версии контракта"**.

![Включение параметра JavaScript](media/user-flow-javascript-overview/page-contract-version.PNG)

## <a name="next-steps"></a>Дальнейшие действия
См. статью [Примеры JavaScript для Azure Active Directory B2C](javascript-samples.md).
