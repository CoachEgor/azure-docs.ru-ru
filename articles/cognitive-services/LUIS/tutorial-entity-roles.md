---
title: Руководство по Использование контекстно-зависимых данных с ролями — LUIS
titleSuffix: Azure Cognitive Services
description: Поиск связанных данных на основе контекста. Например, места исходного расположения и назначения при перемещении из одного здания и офиса в другое связаны между собой.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/17/2019
ms.author: diberry
ms.openlocfilehash: cd646ef061a0be06a9b1a56b72a4f35d9796aa63
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "75447866"
---
# <a name="tutorial-extract-contextually-related-data-from-an-utterance"></a>Руководство по Извлечение из речевого фрагмента данных, связанных с контекстом

Из данного руководства можно узнать, как выполнять поиск связанных частей данных на основе контекста. Например, исходное и конечное расположения для перемещения из одного города в другой. Могут потребоваться фрагменты данных, которые связаны друг с другом.

Роли можно использовать с любым предварительно созданным или настраиваемым типом сущности. Они используются в примерах речевых фрагментов и шаблонов.

**В этом руководстве рассмотрено, как выполнять следующие задачи.**

> [!div class="checklist"]
> * Создание приложения
> * Добавление намерения
> * Получение сведений об источнике и назначении с помощью ролей.
> * Обучение
> * Публикация
> * Получение намерений и ролей сущностей из конечной точки.

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="related-data"></a>Связанные данные

Это приложение определяет, в каких случаях сотрудник должен перемещаться из исходного города в целевой. Оно использует предварительно созданную сущность geographyV2 для определения названий городов. Роли используются, чтобы определить тип расположения (исходное или конечное) в речевом фрагменте.

Роль должна использоваться, когда извлекаемые данные сущности:

* связаны между собой в контексте речевого фрагмента;
* содержат конкретные слова, обозначающие каждую роль. Примеры этих слов включают в себя: из/в, выход/переход, от/к.
* Обе эти роли часто находятся в одном речевом фрагменте, позволяя LUIS учиться на частом использовании в контексте.
* Должны быть сгруппированы и обработаны клиентским приложением в качестве единицы информации.

## <a name="create-a-new-app"></a>Создание нового приложения

1. Войдите в предварительную версию портала LUIS по URL-адресу [https://preview.luis.ai](https://preview.luis.ai).

1. Выберите **Создание приложения**, введите имя `HumanResources` и сохраните язык и региональные параметры по умолчанию (на **английском языке**). Описание оставьте пустым.

1. Нажмите кнопку **Готово**.

## <a name="create-an-intent-to-move-employees-between-cities"></a>Создание намерения для перемещения сотрудников между городами

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. Выберите **Create new intent**. (Создать намерение).

1. Введите `MoveEmployeeToCity` во всплывающем диалоговом окне и нажмите кнопку **Done** (Готово).

    > [!div class="mx-imgBorder"]
    > ![Снимок экрана диалогового окна создания намерения с](./media/tutorial-entity-roles/create-new-intent-move-employee-to-city.png)

1. Добавьте примеры фраз в намерение.

    |Примеры высказываний|
    |--|
    |Переместить Джона В. Смита из Сиэтла в Орландо.|
    |Переместить Джил Джонс из Сиэтла в Каир.|
    |Перевести Джона Джексона из Тампа в Атланту. |
    |Перевести Дебру Дотери в Талсы из Чикаго.|
    |Перем. Джил Джонс из Каира в Тампа.|
    |Перевести Елис Андерсон в Окленд из Редмонда.|
    |Направить Карла Чамерлина из Сан-Франциско в Редмонд.|
    |Отправить Стива Стандиша из Сан-Диего в Бельвю. |
    |Перебросить Таннера Томпсона из Канзас-Сити в Чикаго.|

    > [!div class="mx-imgBorder"]
    > ![Снимок экрана приложения LUIS с новыми фразами в намерении MoveEmployee](./media/tutorial-entity-roles/hr-enter-utterances.png)

## <a name="add-prebuilt-entity-geographyv2"></a>Добавление предварительно созданной сущности geographyV2

Предварительно созданная сущность, geographyV2, извлекает сведения о расположении, включая названия городов. Так как речевые фрагменты содержат два названия городов, связанные друг с другом в контексте, роли можно использовать для извлечения этого контекста.

1. Выберите **Entities** (Сущности) в меню навигации слева.

1. Выберите **Add prebuilt entity** (Добавить предварительно созданную сущность), а затем выберите `geo` в строке поиска, чтобы отфильтровать предварительно созданные сущности.

    > [!div class="mx-imgBorder"]
    > ![Добавление предварительно созданной сущности geographyV2 в приложение](media/tutorial-entity-roles/add-geographyV2-prebuilt-entity.png)

1. Установите флажок и выберите **Done** (Готово).
1. В списке **Entities** (Сущности) выберите **geographyV2**, чтобы открыть новую сущность.
1. Добавьте две роли, `Origin` и `Destination`.

    > [!div class="mx-imgBorder"]
    > ![Добавление ролей в предварительно созданную сущность](media/tutorial-entity-roles/add-roles-to-prebuilt-entity.png)

1. Выберите **Intents** (Намерения) в области навигации слева, затем выберите намерение **MoveEmployeeToCity**. Обратите внимание на то, что названия городов помечены с использованием предварительно созданной сущности **geographyV2**.
1. На контекстной панели инструментов выберите **Entity palette** (Палитра сущностей).

    > [!div class="mx-imgBorder"]
    > ![Выбор палитры сущностей на контекстной панели инструментов](media/tutorial-entity-roles/intent-detail-context-toolbar-select-entity-palette.png)

1. Выберите созданную сущность **geographyV2**, а затем щелкните **Entity inspector** (Инспектор сущностей).
1. В разделе **Entity inspector** (Инспектор сущностей) выберите одну роль — **Назначение**. Это приведет к изменению курсора мыши. С помощью курсора пометьте текст во всех речевых фрагментах, которые являются конечным расположением.

    > [!div class="mx-imgBorder"]
    > ![Выбор роли на палитре сущностей](media/tutorial-entity-roles/entity-palette-select-entity-role.png)


1. Вернитесь к разделу **Entity inspector** (Инспектор сущностей) и выберите роль **Источник**. С помощью курсора пометьте текст во всех речевых фрагментах, которые являются исходным расположением.

## <a name="add-example-utterances-to-the-none-intent"></a>Добавление примеров речевых фрагментов в намерение None

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Обучение приложения для проверки изменений намерения

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Публикация приложения, чтобы в обученную модель можно было отправлять запросы из конечной точки

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Получение намерения и прогнозирование сущности из конечной точки

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


1. Перейдите в конец URL-адреса в адресной строке и введите `Please move Carl Chamerlin from Tampa to Portland`. Последний параметр строки запроса — `q`. Это **запрос** фразы. Эта фраза не совпадает ни с какими помеченными фразами, поэтому она является хорошим тестом и должна возвращать намерение `MoveEmployee` с извлеченной сущностью.

    ```json
    {
      "query": "Please move Carl Chamerlin from Tampa to Portland",
      "topScoringIntent": {
        "intent": "MoveEmployeeToCity",
        "score": 0.9706451
      },
      "intents": [
        {
          "intent": "MoveEmployeeToCity",
          "score": 0.9706451
        },
        {
          "intent": "None",
          "score": 0.0307451729
        }
      ],
      "entities": [
        {
          "entity": "tampa",
          "type": "builtin.geographyV2.city",
          "startIndex": 32,
          "endIndex": 36,
          "role": "Origin"
        },
        {
          "entity": "portland",
          "type": "builtin.geographyV2.city",
          "startIndex": 41,
          "endIndex": 48,
          "role": "Destination"
        }
      ]
    }
    ```

    При этом прогнозируется правильное намерение, а для массива сущностей в соответствующем свойстве **entities** существует как целевая, так и исходная роли.

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Дополнительные сведения

* [Основные понятия сущностей](luis-concept-entity-types.md)
* [Основные понятия ролей](luis-concept-roles.md)
* [Список предварительно созданных сущностей](luis-reference-prebuilt-entities.md)
* [Обучение активной версии приложения LUIS](luis-how-to-train.md)
* [Как опубликовать предложение](luis-how-to-publish-app.md)
* [Тестирование приложения LUIS на портале LUIS](luis-interactive-test.md)
* [Роли](luis-concept-roles.md)

## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как создать намерение и добавить пример с речевыми фрагментами для данных об исходном и конечном расположении, полученных с учетом контекста. После обучения и публикации приложения клиентское приложение может использовать данную информацию для создания и перемещения билета с соответствующей информацией.

> [!div class="nextstepaction"]
> [Сведения о добавлении составной сущности](luis-tutorial-composite-entity.md)
