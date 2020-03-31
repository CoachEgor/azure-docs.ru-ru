---
title: В рабочей книге Azure Monitor снижаются параметры
description: Упрощение сложной отчетности с помощью заранее построенных и пользовательских параметризированных трудовых книжек, содержащих параметры выпадения
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: f3220a363025d80fd7636dbfc3af3d2d9d7bc040
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658291"
---
# <a name="workbook-drop-down-parameters"></a>Workbook упасть параметры

Падение вниз позволяет пользователю собирать одно или несколько значений ввода из известного набора (например, выберите один из запросов приложения). Падение падений обеспечивает удобный способ сбора произвольных входов от пользователей. Падение особенно полезно для включения фильтрации в интерактивных отчетах. 

Самый простой способ указать выпадение — это предоставить статический список в параметре. Более интересный способ заключается в том, чтобы получить список динамически через запрос КЗЛ. Параметры также позволяют указать, является ли он одномандатным или многоизбранным, и если он многовыбирает, как следует отформатировать набор результатов (делимитер, цитата и т.д.).

## <a name="creating-a-static-drop-down-parameter"></a>Создание статического параметра выпадения

1. Начните с пустой трудовой книжки в режиме рерит.
2. Выберите _Параметры добавления_ из ссылок в трудовой книжке.
3. Нажмите на синюю кнопку _Добавить параметр._
4. В новом параметре панели, которая всплывает ввести:
    1. Название параметра:`Environment`
    2. Тип параметра:`Drop down`
    3. Обязательно:`checked`
    4. Разрешить: `multiple selection``unchecked`
    5. Получить данные от:`JSON`
5. В текстовом блоке Ввода JSON вставьте фрагмент Json:
    ```json
    [
        { "value":"dev", "label":"Development" },
        { "value":"ppe", "label":"Pre-production" },
        { "value":"prod", "label":"Production", "selected":true }
    ]
    ```
6. Нажми на `Update` синюю кнопку.
7. Выберите "Сохранить" из панели инструментов для создания параметра.
8. Параметр среды будет выпадением вниз с тремя значениями.

    ![Изображение, показывающее создание статического утопления](./media/workbook-dropdowns/dropdown-create.png)

## <a name="creating-a-static-dropdown-with-groups-of-items"></a>Создание статического выпадения с группами элементов
Если результат запроса/json содержит поле "группы", в отсечке будут отображаться группы значений. Следуйте приведенной выше примеру, но вместо этого используйте следующий Json:
```json
[
    { "value":"dev", "label":"Development", "group":"Development" },
    { "value":"dev-cloud", "label":"Development (Cloud)", "group":"Development" },
    { "value":"ppe", "label":"Pre-production", "group":"Test" },
    { "value":"ppe-test", "label":"Pre-production (Test)", "group":"Test" },
    { "value":"prod1", "label":"Prod 1", "selected":true, "group":"Production" },
    { "value":"prod2", "label":"Prod 2", "group":"Production" }
]
```
    ![Image showing an example of a grouped dropdown](./media/workbook-dropdowns/grouped-dropDown.png)


## <a name="creating-a-dynamic-drop-down-parameter"></a>Создание динамического параметра выпадения
1. Начните с пустой трудовой книжки в режиме рерит.
2. Выберите _Параметры добавления_ из ссылок в трудовой книжке.
3. Нажмите на синюю кнопку _Добавить параметр._
4. В новом параметре панели, которая всплывает ввести:
    1. Название параметра:`RequestName`
    2. Тип параметра:`Drop down`
    3. Обязательно:`checked`
    4. Разрешить: `multiple selection``unchecked`
    5. Получить данные от:`Query`
5. В текстовом блоке Ввода JSON вставьте фрагмент Json:

    ```kusto
        requests
        | summarize by name
        | order by name asc
    ```
1. Нажми на `Run Query` синюю кнопку.
2. Выберите "Сохранить" из панели инструментов для создания параметра.
3. Параметр RequestName будет выпадением имен всех запросов в приложении.

    ![Изображение, показывающее создание динамического падения](./media/workbook-dropdowns/dropdown-dynamic.png)

## <a name="referencing-drop-down-parameter"></a>Ссылки падают параметр
### <a name="in-kql"></a>В КЗЛ
1. Добавьте элемент управления запросами в трудовую книгу и выберите ресурс Application Insights.
2. В редакторе КЗЛ введите этот фрагмент

    ```kusto
        requests
        | where name == '{RequestName}'
        | summarize Requests = count() by bin(timestamp, 1h)

    ```
3. Это расширяет время оценки запросов на:

    ```kusto
        requests
        | where name == 'GET Home/Index'
        | summarize Requests = count() by bin(timestamp, 1h)
    ```

4. Выполнить запрос, чтобы увидеть результаты. Дополнительно, отобразить его в виде диаграммы.

    ![Изображение, показывающее падение вниз, на который ссылаются в КЗЛ](./media/workbook-dropdowns/dropdown-reference.png)


## <a name="parameter-value-label-selection-and-group"></a>Значение параметра, этикетка, выбор и группа
Запрос, используемый в динамическом параметре выпадения выше, возвращает список значений, которые добросовестно отображаются в выпадении. Но что делать, если вы хотите другое имя дисплея, или один из них будет выбран? Параметры падения позволяют это спомощьей через столбцы значения, метки, выделения и группы.

Приведенный ниже пример показывает, как получить список зависимостей Application Insights, имена отображений которых стилизованы под эмодзи, имеет первый выбранный и сгруппирован по именам операций.

```kusto
dependencies
| summarize by operation_Name, name
| where name !contains ('.')
| order by name asc
| serialize Rank = row_number()
| project value = name, label = strcat('🌐 ', name), selected = iff(Rank == 1, true, false), group = operation_Name
```
    ![Image showing a drop-down parameter using value, label, selection and group options](./media/workbook-dropdowns/dropdown-more-options.png)


## <a name="drop-down-parameter-options"></a>Удаление параметров параметра
| Параметр | Объяснение | Пример |
| ------------- |:-------------|:-------------|
| `{DependencyName}` | Выбранное значение | GET fabrikamaccount |
| `{DependencyName:label}` | Выбранная метка | 🌐 GET fabrikamaccount |
| `{DependencyName:value}` | Выбранное значение | GET fabrikamaccount |

## <a name="multiple-selection"></a>Выбор нескольких элементов
Примеры до сих пор явно установить параметр, чтобы выбрать только одно значение в падение вниз. Падение параметров также `multiple selection` поддержку - включение это `Allow multiple selection` так же просто, как проверка опции. 

Пользователь также имеет возможность указать формат набора результатов `delimiter` `quote with` через настройки и настройки. По умолчанию просто возвращает значения в виде коллекции в этой форме: 'a', 'b', 'c'. Они также имеют возможность ограничить количество выборов.

Ссылки на параметр необходимо будет изменить для работы с форматом результата. Наиболее `in` распространенный способ включить его через оператора.

```kusto
dependencies
| where name in ({DependencyName})
| summarize Requests = count() by bin(timestamp, 1h), name
```

Вот пример для нескольких выбора выпадения вниз на работе:

![Изображение, показывающее многовыбираетьец параметра выпадения](./media/workbook-dropdowns/dropdown-multiselect.png)

## <a name="next-steps"></a>Дальнейшие действия

* [Начать](workbooks-visualizations.md) узнавать больше о рабочих книгах много богатых вариантов визуализации.
* [Контролируйте](workbooks-access-control.md) и делитесь доступом к ресурсам вашей трудовой книжки.
