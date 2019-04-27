---
title: Использование Conversation Learner с другими технологиями построения ботов в Microsoft Cognitive Services | Документация Майкрософт
titleSuffix: Azure
description: Узнайте, как использовать Conversation Learner с другими технологиями построения ботов.
services: cognitive-services
author: mattm
manager: larsliden
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 07/13/2018
ms.author: v-jaswel
ms.openlocfilehash: b90fc42aa56bfc813ec464670336dea75cff0f0e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60640317"
---
# <a name="how-to-use-conversation-learner-with-other-bot-building-technologies"></a>Использование Conversation Learner с другими технологиями построения ботов

В этом руководстве описано, как использовать Conversation Learner с другими технологиями построения ботов и разделение памяти (или состояния) между этими технологиями. 

## <a name="video"></a>Видео

[![Предварительная версия учебника по гибридным ботам](https://aka.ms/cl_Tutorial_v3_Hybrid_Applications_Preview)](https://aka.ms/cl_Tutorial_v3_Hybrid_Applications)

## <a name="requirements"></a>Требования
Для этого руководства требуется эмулятор бота для создания записанных диалогов, а не пользовательский веб-интерфейс записи диалогов. Дополнительные сведения о Bot Framework Emulator доступны [здесь](https://docs.microsoft.com/azure/bot-service/bot-service-debug-emulator?view=azure-bot-service-4.0). 

Для работы с этим руководством требуется запустить гибридный учебный бот.

    npm run tutorial-hybrid

## <a name="details"></a>Сведения

Пока Conversation Learner находится под контролем, все состояние, относящееся к сеансу Conversation Learner, должно храниться в диспетчере памяти Conversation Learner. Это необходимо, так как машинное обучение использует состояние, чтобы определить, как вести разговор. Внешнее состояние может быть передано в Conversation Learner в параметр OnSessionStartCallback, который вызывается в начале сеанса. Внутреннее состояние может быть возвращено в параметре OnSessionEndCallback при завершении сеанса.

Conversation Learner можно считать вызовом функции, которая принимает некоторое начальное состояние и возвращает значения.

В этом примере будет создан гибридный бот с использованием двух различных систем.
1. Модель Conversation Learner <br/>
    Модель Conversation Learner используется для определения следующего действия бота на основе текущего сеанса. Эта часть бота принимает одну часть начального состояния `isOpen` (которая указывает, открыт или закрыт магазин) и возвращает другую часть состояния `purchaseItem` (имя предмета, который пользователь покупает).

2. Согласование текста <br />
    Просто изучает входящий текст на определенные строки и выдает ответы. Эта часть бота управляет другими механизмами хранения бота и отвечает за запуск сеанса Conversation Learner. В частности, он управляет тремя переменными: `usingConversationLearner`, `storeIsOpen` и `purchaseItem`.

Начнем с рассмотрения модели, используемой в этой демонстрации.

### <a name="open-the-demo"></a>Запуск демонстрационного примера

В веб-интерфейсе щелкните "Импорт учебников" и выберите модель Tutorial-16-HybridBot.

## <a name="entities"></a>Сущности

Откройте страницу сущностей и обратите внимание на две сущности: `isOpen` и `purchaseItem`

Чтобы понять, как используются эти сущности, откройте файл: `C:\<installedpath>\src\demos\tutorialHybrid.ts`, чтобы посмотреть обратные вызовы.

Обратите внимание, что код `OnSessionStartCallback` копирует значение `storeIsOpen` из хранилища разговоров BotBuilder в качестве значения сущности `isOpen`, чтобы оно было доступно для Conversation Learner. См. следующий код.

![](../media/tutorial17_sessionstart.PNG)

Аналогично код в `OnSessionEndCallback` (если сеанс был завершен из-за выученного действия, а не только тайм-аута) копирует значение объекта `purchaseItem` в хранилище BotBuilder `purchaseItem`. См. следующий код.

![](../media/tutorial17_sessionend.PNG)

Теперь давайте рассмотрим действия.

## <a name="actions"></a>Действия

Обратите внимание, что в модели имеются четыре действия.

Далее приведены предполагаемые правила для действий.

- Если сущность `isOpen` задана, бот спросит: "Что вы хотите приобрести?" и сохранит это в слоте `puchaseItem`.
- Если `isOpen` не задана, бот скажет: "I’m sorry we’re closed" (Извините, но мы закрыты).
- Два других действия относятся к типу `END_SESSION`.
- Действие END_SESSION указывает приложению ConversationLearner, что диалог завершен.

### <a name="overall-bot-logic"></a>Общая логика ботов

Сначала вы видите, что, если флаг `usingConversationLearner` состояния бота был установлен, управление передается Conversation Learner. В противном случае управление передается чему-то другому.  В этом примере отображается простое сопоставление текста, но это могут быть любые другие технологии ботов, включая LUIS, QnA maker и даже другой экземпляр Conversation Learner.

Нам нужен способ для открытия и закрытия магазина, поэтому мы сравниваем строки с "открытым магазином" и "закрытым магазином" и устанавливаем флаг "storeIsOpen"(магазин открыт).

Теперь нужен способ для активации передачи управления моделью Conversation Learner. При сопоставление со строкой "магазин" необходимо выполнить следующие действия.
- Установите флаг `usingConversationLearner` в память бота.
- Вызовите метод "StartSession" в модели Conversation Learner.  Она активирует параметр onSessionStartCallback, который инициализирует значение сущности `isOpen`.

См. ниже.

![](../media/tutorial17_useConversationLearner.PNG)

Также выполняется текстовое совпадение с "журналом", который отображает последний элемент покупки.
Наконец, если напечатано что-то еще, будут отображены доступные пользовательские команды.

## <a name="train-dialog"></a>Обучение диалогу

В этом руководстве модель уже предварительно обучена.  Мы полностью проверим бот, чтобы увидеть эффект обратного вызова начала и окончание сеанса на практике.

## <a name="testing-the-bot"></a>Тестирование бота

В отличие от одиночных моделей ботов Conversation Leaner его невозможно проверить в пользовательском интерфейсе Conversation Learner, поскольку он может показать только то, что обрабатывается моделью Conversation Learner.

### <a name="install-the-bot-framework-emulator"></a>Установка эмулятора Bot Framework

- Перейдите к [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator).
- Скачайте и установите эмулятор.

### <a name="configure-the-emulator"></a>Настройка эмулятора

- Откройте эмулятор и убедитесь, что URL-адрес предназначен для того же порта, на котором работает бот. Например: `http://localhost:3978/api/messages`

### <a name="test"></a>Тест 

#### <a name="scenario-1-store-is-closed"></a>Сценарий 1. Магазин закрыт
1. Введите 'shop' (магазин). Он выполняется путем сопоставления текста и предоставляет управление моделью Conversation Learner.
2. Введите "hello" (Здравствуйте).  Так как значение `isOpen` не задано, бот скажет "Извините, но мы закрыты" и завершит сеанс.

#### <a name="scenario-2-store-is-open"></a>Сценарий 2. Магазин открыт
1. Введите 'open store' (открыть магазин).  Это задаст параметру `isOpen` значение true.
1. Введите 'shop' (магазин).
1. Введите "hello" (Здравствуйте).  Так как `isOpen` имеет значение true, бот спросит "Что вы хотите приобрести?"
1. Введите 'chair' (стул). 'chair' будет сохранено в памяти Conversation Learner как сущность `purchaseItem`. Вызывается обратный вызов конечного сеанса, который копирует это значение в хранилище разговора.
1. Введите 'history' (журнал).  Бот скажет 'Вы приобрели стул', так как `purchaseItem` был вашей последней покупкой.

## <a name="conclusion"></a>Заключение

С помощью вышеупомянутых инструкций можно объединить Conversation Learner с любой другой технологией построения ботов.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Создание ветвей и откат](./17-branch-undo.md)
