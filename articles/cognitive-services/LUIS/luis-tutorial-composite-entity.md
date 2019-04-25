---
title: Учебник составной сущности
titleSuffix: Azure Cognitive Services
description: Добавление составной сущности для объединения извлеченных данных различных типов в содержащую сущность. Клиентское приложение может легко извлекать связанные данные разных типов путем их объединения.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 02/19/2019
ms.author: diberry
ms.openlocfilehash: c49090a5563a6d63c90b29cc7442c1e4ed9886e0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60495884"
---
# <a name="tutorial-group-and-extract-related-data"></a>Руководство по Группирование и извлечение связанных данных
В этом руководстве описывается добавление составной сущности для объединения извлеченных данных различных типов в содержащую сущность. Клиентское приложение может легко извлекать связанные данные разных типов путем их объединения.

Целью сущности является группировка связанных сущностей в сущность родительской категории. До создания составной сущности информация существует в виде отдельных сущностей. Она похожа на иерархическую сущность, но может содержать разные типы сущностей. 

Составная сущность является оптимальным выбором для этого типа данных, так как:

* данные связаны между собой; 
* для них используются различные типы сущностей;
* они должны быть сгруппированы и обработаны клиентским приложением в качестве единицы информации.

**В этом руководстве рассмотрено, как выполнять следующие задачи.**

<!-- green checkmark -->
> [!div class="checklist"]
> * Импортировать пример приложения
> * Создавать намерение.
> * Добавление составной сущности 
> * Train
> * Опубликовать
> * Получать намерения и сущности из конечной точки.

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Импорт примера приложения

1.  Скачайте и сохраните [JSON-файл приложения](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/build-app/tutorial_list.json) из учебника по сущностям списка.

2. Импортируйте JSON-файл в новое приложение.

3. Из раздела **Управление** на вкладке **Версии** скопируйте версию и назовите ее `composite`. Клонирование — это отличный способ поэкспериментировать с различными функциями LUIS без влияния на исходную версию. Так как имя версии используется в маршруте URL-адреса, оно не может содержать символы, которые недопустимы в URL-адресе.

## <a name="composite-entity"></a>Составная сущность

В этом приложении название отдела определяется в сущности списка **Отдел** и включает в себя синонимы. 

Намерение **TransferEmployeeToDepartment** имеет примеры высказываний для запроса перевода сотрудника в другой отдел. 

К примерам высказываний для этого намерения относятся следующие.

|Примеры высказываний|
|--|
|Перевести Джона Смита в бухгалтерию.|
|Перевести Джил Джонс в отдел исследований.|
 
Запрос на перевод должен включать названия отдела и имя сотрудника. 

## <a name="add-the-personname-prebuilt-entity-to-help-with-common-data-type-extraction"></a>Добавьте предварительно созданную сущность PersonName, чтобы помочь с извлечением распространенных типов данных

LUIS предоставляет несколько предварительно созданных сущностей для извлечения общих данных. 

1. В верхней панели навигации выберите **Сборка**, а затем выберите **Сущности** в левом меню навигации.

1. Нажмите кнопку **Manage prebuilt entities** (Управление предварительно созданными сущностями).

1. Выберите **[PersonName](luis-reference-prebuilt-person.md)** из списка предварительно созданных сущностей, а затем щелкните **Готово**.

    ![Снимок экрана выбора сущности number в диалоговом окне предварительно созданных сущностей](./media/luis-tutorial-composite-entity/add-personname-prebuilt-entity.png)

    Эта сущность помогает добавлять распознавание имени в клиентское приложение.

## <a name="create-composite-entity-from-example-utterances"></a>Создание составной сущности из примеров высказываний

1. В левой области навигации выберите **Intents** (Намерения).

1. Выберите **TransferEmployeeToDepartment** из списка намерений.

1. В utterance `place John Jackson in engineering`, выберите сущность personName, `John Jackson`, а затем выберите **обернуть в составной сущности** в списке ниже utterance всплывающего меню. 

    ![Снимок экрана выбора составного переноса по словам в раскрывающемся списке диалогового окна](./media/luis-tutorial-composite-entity/hr-create-composite-entity-1.png)

1. Затем немедленно выберите последнюю сущность `engineering` в высказывании. Зеленая линия под выбранными словами указывает на составную сущность. В контекстном меню введите имя `TransferEmployeeInfo` и нажмите клавишу ВВОД. 

    ![Снимок экрана ввода составного имени в раскрывающемся списке диалогового окна](./media/luis-tutorial-composite-entity/hr-create-composite-entity-2.png)

1. В диалоговом окне **Какой тип сущности необходимо создать?** в списке содержатся все необходимые поля: `personName` и `Department`. Нажмите кнопку **Готово**. Обратите внимание, что в составную сущность была добавлена предварительно созданная сущность personName. Если предварительно созданные сущности отображаются между начальным и конечным маркером составной сущности, составная сущность должна содержать такие предварительно созданные сущности. Если предварительно созданные сущности не включены, верно спрогнозировать поведение составной сущности невозможно, однако это можно сделать для каждого отдельного элемента.

    ![Снимок экрана ввода составного имени в раскрывающемся списке диалогового окна](./media/luis-tutorial-composite-entity/hr-create-composite-entity-3.png)

## <a name="label-example-utterances-with-composite-entity"></a>Помеченный пример высказываний с составной сущностью

1. В каждом примере высказывания выберите самую левую сущность, которая должна быть в составной сущности. Затем выберите **Wrap in composite entity** (Заключить в составную сущность).

1. Выберите последнее слово в составной сущности, а затем во всплывающем меню выберите **TransferEmployeeInfo**. 

1. Убедитесь, что все высказывания в намерении помечены составной сущностью. 

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Обучите приложение для проверки изменений намерения 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Публикация приложения, чтобы в обученную модель можно было отправлять запросы из конечной точки

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Получение намерения и прогнозирование сущности из конечной точки 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Перейдите в конец URL-адреса и введите `Move Jill Jones to DevOps`. Последний параметр строки запроса — `q`. Это запрос фразы. 

    Поскольку этот тест был создан для проверки правильности извлечения составной сущности, то он может включать либо существующие образцы высказываний, либо новое высказывание. Хорошей проверкой является включение всех дочерних сущностей в составную сущность.

    ```json
    {
      "query": "Move Jill Jones to DevOps",
      "topScoringIntent": {
        "intent": "TransferEmployeeToDepartment",
        "score": 0.9882747
      },
      "intents": [
        {
          "intent": "TransferEmployeeToDepartment",
          "score": 0.9882747
        },
        {
          "intent": "None",
          "score": 0.00925369747
        }
      ],
      "entities": [
        {
          "entity": "jill jones",
          "type": "builtin.personName",
          "startIndex": 5,
          "endIndex": 14
        },
        {
          "entity": "devops",
          "type": "Department",
          "startIndex": 19,
          "endIndex": 24,
          "resolution": {
            "values": [
              "Development Operations"
            ]
          }
        },
        {
          "entity": "jill jones to devops",
          "type": "TransferEmployeeInfo",
          "startIndex": 5,
          "endIndex": 24,
          "score": 0.9607566
        }
      ],
      "compositeEntities": [
        {
          "parentType": "TransferEmployeeInfo",
          "value": "jill jones to devops",
          "children": [
            {
              "type": "builtin.personName",
              "value": "jill jones"
            },
            {
              "type": "Department",
              "value": "devops"
            }
          ]
        }
      ]
    }
    ```

   Это высказывание возвращает массив составных сущностей. Каждой сущности присваивается тип и значение. Чтобы достичь большей точности для каждой дочерней сущности, используйте комбинацию типа и значения из элемента составного массива, это позволит найти соответствующий элемент в массиве сущностей.  

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Связанные сведения

* [Учебник по сущности списка](luis-quickstart-intents-only.md)
* Сведения о понятии [составной сущности](luis-concept-entity-types.md)
* [Обучение](luis-how-to-train.md)
* [Как опубликовать предложение](luis-how-to-publish-app.md)
* [Тестирование приложения LUIS на портале LUIS](luis-interactive-test.md)


## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как создать составную сущность, предназначенную для инкапсуляции существующих сущностей. Это позволяет клиентскому приложению находить группы связанных данных разных типов и продолжать разговор. Клиентское приложение для приложения по управлению персоналом может спросить о дате и времени начала и завершения перемещения. Также оно может спросить о других логистических аспектах, например о номере телефона. 

> [!div class="nextstepaction"] 
> [Руководство: 7. Добавление простой сущности и списка фраз](luis-quickstart-primary-and-secondary-data.md)  
