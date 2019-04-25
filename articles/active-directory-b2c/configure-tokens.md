---
title: Настройка маркеров — Azure Active Directory B2C | Документация Майкрософт
description: Узнайте, как настроить параметры токена время существования и совместимости в Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 2455b8c12ed042af3a06a158a4e5c60a0aee748b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60384040"
---
# <a name="configure-tokens-in-azure-active-directory-b2c"></a>Настройка маркеров в Azure Active Directory B2C

В этой статье вы узнаете, как настроить [время существования и совместимости маркера](active-directory-b2c-reference-tokens.md) в Azure Active Directory (Azure AD) B2C.

## <a name="prerequisites"></a>Технические условия

[Создайте поток пользователя](tutorial-create-user-flows.md), чтобы пользователи могли зарегистрироваться и войти в ваше приложение.

## <a name="configure-token-lifetime"></a>Настройка времени жизни маркера

Вы можете настроить время существования маркера на любой поток пользователя.

1. Войдите на [портале Azure](https://portal.azure.com).
2. Убедитесь, что вы используете каталог, содержащий клиент Azure AD B2C. Выберите **фильтр каталога и подписки** в верхнем меню и выберите каталог, содержащий клиент Azure AD B2C.
3. Выберите **Все службы** в левом верхнем углу окна портала Azure, а затем найдите и выберите **Azure AD B2C**.
4. Выберите **маршруты пользователей (политики)**.
5. Откройте ранее созданный поток пользователя. 
6. Выберите **Свойства**.
7. В разделе **время существования маркера**, измените следующие свойства в соответствии с потребностями вашего приложения:

    ![Настройка времени жизни маркера](./media/configure-tokens/token-lifetime.png)

8. Выберите команду **Сохранить**.

## <a name="configure-token-compatibility"></a>Настройка совместимости токенов

1. Выберите **маршруты пользователей (политики)**.
2. Откройте ранее созданный поток пользователя. 
3. Выберите **Свойства**.
4. В разделе **маркеров параметров совместимости**, измените следующие свойства в соответствии с потребностями вашего приложения:

    ![Настройка совместимости токенов](./media/configure-tokens/token-compatibility.png)

5. Выберите команду **Сохранить**.

## <a name="next-steps"></a>Дальнейшие действия

Узнайте больше о том, как [использовать маркеры доступа](active-directory-b2c-access-tokens.md).



