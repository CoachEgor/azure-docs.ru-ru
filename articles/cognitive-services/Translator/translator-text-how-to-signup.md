---
title: Начало работы с API перевода текстов
titleSuffix: Azure Cognitive Services
description: В этой статье будет уотена возможность подписаться на API текста переводчика когнитивных служб Azure и получить ключ подписки.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.custom: seodec18
ms.openlocfilehash: cfae5318213e8af164c8d0cc9ed378705a5644b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "73835695"
---
# <a name="how-to-sign-up-for-the-translator-text-api"></a>Регистрация для использования API перевода текстов

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

- Нет учетной записи? Чтобы поэкспериментировать, вы можете создать [бесплатную учетную запись](https://azure.microsoft.com/free/).
- Уже есть учетная запись? [Войти](https://ms.portal.azure.com/)

## <a name="create-a-subscription-to-the-translator-text-api"></a>Создание подписки на API перевода текстов

После входа на портал создайте подписку на API перевода текстов, выполнив следующие действия.

1. Выберите **и создайте ресурс.**
1. В поле поиска **Search the Marketplace** (Поиск в Marketplace) введите **Перевод текстов** и выберите его из списка результатов.
1. Нажмите кнопку **Создать**, чтобы определить сведения для подписки.
1. В списке **Ценовая категория** выберите ценовую категорию, которая лучше всего соответствует вашим потребностям.
    1. У всех подписок есть уровень "Бесплатный". Уровень "Бесплатный" поддерживает те же компоненты и функциональные возможности, что и платные планы, и является бессрочным.
    1. Для учетной записи может быть только одна бесплатная подписка.
1. Чтобы завершить создание подписки, нажмите кнопку **Создать**.

## <a name="authentication-key"></a>ключ проверки подлинности;

При регистрации для использования перевода текстов вы получаете личный ключ доступа, уникальный для вашей подписки. Этот ключ требуется при каждом вызове API перевода текстов.

1. Получите ключ проверки подлинности, выбрав соответствующую подписку.
1. Выберите **Ключи** в разделе **Управление ресурсами** сведений о подписке.
1. Скопируйте один из ключей, указанных для вашей подписки.

## <a name="learn-test-and-get-support"></a>Обучение, тестирование и получение поддержки

- [Примеры кода в GitHub](https://github.com/MicrosoftTranslator)
- [Форум технической поддержки Microsoft Translator](https://www.aka.ms/TranslatorForum)

Microsoft Translator обычно выполняет первые несколько запросов, прежде чем проверить состояние подписки учетной записи. Если первые несколько вызовов API Microsoft Translator были успешными, а потом произошел сбой, причину сбоя можно выяснить из сообщения об ошибке. Зарегистрируйте отклик API, чтобы узнать причину сбоя.

## <a name="pricing-options"></a>Варианты оплаты

- [API перевода текстов](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)

## <a name="customization"></a>Настройка

Используйте пользовательский переводчик, чтобы настроить переводы и создать систему переводов, оптимизированную для собственной терминологии и стиля, начав с универсальных систем нейронного машинного перевода Microsoft Translator. [Дополнительные сведения](customization.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Приступая к работе с Azure (трехминутное видео)](https://azure.microsoft.com/get-started/?b=16.24)
- [Способ оплаты счета](https://azure.microsoft.com/pricing/invoicing/)
