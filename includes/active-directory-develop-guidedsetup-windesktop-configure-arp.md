---
title: включение файла
description: включение файла
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 09/17/2018
ms.date: 11/05/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 5c2bce5635dfe488c1725efdd2954ecd81cf8e79
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60300628"
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Добавление в приложение сведений о его регистрации
На этом шаге вам нужно добавить идентификатор приложения в свой проект.

1. Откройте файл `App.xaml.cs` и замените строку, содержащую `ClientId`, на:

    ```csharp
    private static string ClientId = "[Enter the application Id here]";
    ```

<!-- ms.date: 11/05/2018 -->