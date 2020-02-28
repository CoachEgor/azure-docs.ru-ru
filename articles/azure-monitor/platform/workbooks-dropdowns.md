---
title: Azure Monitor параметры раскрывающегося списка книги
description: Упрощение сложных отчетов с помощью предварительно созданных и настраиваемых параметризованных книг, содержащих параметры раскрывающегося списка
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: f3220a363025d80fd7636dbfc3af3d2d9d7bc040
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2020
ms.locfileid: "77658291"
---
# <a name="workbook-drop-down-parameters"></a>Параметры раскрывающегося списка книги

Раскрывающиеся списка позволяют пользователю получить одно или несколько входных значений из известного набора (например, выбрать один из запросов приложения). Раскрывающиеся списка предоставляют понятный пользователю способ сбора произвольных входных данных от пользователей. Раскрывающиеся списка особенно полезны при включении фильтрации в интерактивных отчетах. 

Самый простой способ указать раскрывающийся список — это сделать так, чтобы в параметре параметра был указан статический набор. Более интересный способ — получить список динамически через запрос ККЛ. Параметры параметров также позволяют указать, является ли он одиночным или множественным выбором, и если это множественный выбор, то как результирующий набор должен быть отформатирован (разделитель, цитата и т. д.).

## <a name="creating-a-static-drop-down-parameter"></a>Создание статического раскрывающегося параметра

1. Начните с пустой книги в режиме редактирования.
2. Выберите _Добавить параметры_ из ссылок в книге.
3. Нажмите синюю кнопку _Добавить параметр_ .
4. В новой области параметров, которая появляется на экране, введите:
    1. Имя параметра: `Environment`
    2. Тип параметра: `Drop down`
    3. Обязательный: `checked`
    4. Разрешить `multiple selection`: `unchecked`
    5. Получить данные из: `JSON`
5. Вставьте следующий фрагмент JSON в блок входных текстовых данных JSON:
    ```json
    [
        { "value":"dev", "label":"Development" },
        { "value":"ppe", "label":"Pre-production" },
        { "value":"prod", "label":"Production", "selected":true }
    ]
    ```
6. Нажмите синюю кнопку `Update`.
7. Нажмите кнопку "Сохранить" на панели инструментов, чтобы создать параметр.
8. Параметр среды будет раскрывающимся списком с тремя значениями.

    ![Изображение, показывающее создание статического Drown](./media/workbook-dropdowns/dropdown-create.png)

## <a name="creating-a-static-dropdown-with-groups-of-items"></a>Создание статического раскрывающегося списка с группами элементов
Если результат запроса/JSON содержит поле "Группа", в раскрывающемся списке будут отображаться группы значений. Выполните приведенный выше пример, но вместо этого используйте следующий код JSON:
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


## <a name="creating-a-dynamic-drop-down-parameter"></a>Создание динамического раскрывающегося параметра
1. Начните с пустой книги в режиме редактирования.
2. Выберите _Добавить параметры_ из ссылок в книге.
3. Нажмите синюю кнопку _Добавить параметр_ .
4. В новой области параметров, которая появляется на экране, введите:
    1. Имя параметра: `RequestName`
    2. Тип параметра: `Drop down`
    3. Обязательный: `checked`
    4. Разрешить `multiple selection`: `unchecked`
    5. Получить данные из: `Query`
5. Вставьте следующий фрагмент JSON в блок входных текстовых данных JSON:

    ```kusto
        requests
        | summarize by name
        | order by name asc
    ```
1. Нажмите синюю кнопку `Run Query`.
2. Нажмите кнопку "Сохранить" на панели инструментов, чтобы создать параметр.
3. Параметр Рекуестнаме будет содержать раскрывающиеся имена всех запросов в приложении.

    ![Изображение, показывающее создание динамического раскрывающегося списка](./media/workbook-dropdowns/dropdown-dynamic.png)

## <a name="referencing-drop-down-parameter"></a>Ссылка на раскрывающийся параметр
### <a name="in-kql"></a>В ККЛ
1. Добавьте в книгу элемент управления запроса и выберите ресурс Application Insights.
2. В редакторе ККЛ введите этот фрагмент кода.

    ```kusto
        requests
        | where name == '{RequestName}'
        | summarize Requests = count() by bin(timestamp, 1h)

    ```
3. Это увеличивает время оценки запроса до:

    ```kusto
        requests
        | where name == 'GET Home/Index'
        | summarize Requests = count() by bin(timestamp, 1h)
    ```

4. Выполните запрос, чтобы просмотреть результаты. При необходимости выводите его в виде диаграммы.

    ![Изображение, показывающее раскрывающийся список, на который ссылается ККЛ](./media/workbook-dropdowns/dropdown-reference.png)


## <a name="parameter-value-label-selection-and-group"></a>Значение параметра, метка, выбор и группа
Запрос, используемый в динамическом раскрывающемся списке выше, просто возвращает список значений, которые точно отображаются в раскрывающемся списке. Но что делать, если вы хотите выбрать другое отображаемое имя или один из них? Параметры раскрывающегося списка позволяют сделать это с помощью столбцов значение, метка, выбор и группировка.

В примере ниже показано, как получить список зависимостей Application Insights, отображаемые имена которых имеют формат эмодзи, первая выбрана и сгруппирована по именам операций.

```kusto
dependencies
| summarize by operation_Name, name
| where name !contains ('.')
| order by name asc
| serialize Rank = row_number()
| project value = name, label = strcat('🌐 ', name), selected = iff(Rank == 1, true, false), group = operation_Name
```
    ![Image showing a drop-down parameter using value, label, selection and group options](./media/workbook-dropdowns/dropdown-more-options.png)


## <a name="drop-down-parameter-options"></a>Параметры раскрывающегося списка параметров
| Параметр | Пояснение | Пример |
| ------------- |:-------------|:-------------|
| `{DependencyName}` | Выбранное значение | ПОЛУЧИТЬ фабрикамаккаунт |
| `{DependencyName:label}` | Выбранная метка | 🌐 GET фабрикамаккаунт |
| `{DependencyName:value}` | Выбранное значение | ПОЛУЧИТЬ фабрикамаккаунт |

## <a name="multiple-selection"></a>Выбор нескольких элементов
В примерах до сих пор явно задан параметр для выбора только одного значения в раскрывающемся списке. Параметры раскрывающегося списка также поддерживают `multiple selection` — это так же просто, как проверка параметра `Allow multiple selection`. 

Пользователь также имеет возможность указать формат результирующего набора с помощью параметров `delimiter` и `quote with`. Значение по умолчанию просто возвращает значения как коллекцию в этой форме: "a", "b", "c". Они также имеют возможность ограничить количество вариантов выбора.

ККЛ, ссылающийся на параметр, потребуется изменить для работы с форматом результата. Самый распространенный способ включить его — с помощью оператора `in`.

```kusto
dependencies
| where name in ({DependencyName})
| summarize Requests = count() by bin(timestamp, 1h), name
```

Ниже приведен пример раскрывающегося списка множественного выбора на работе.

![Изображение, отображающее раскрывающийся список множественного выбора](./media/workbook-dropdowns/dropdown-multiselect.png)

## <a name="next-steps"></a>Следующие шаги

* [Узнайте](workbooks-visualizations.md) больше о книгах с множеством разнообразных вариантов визуализации.
* [Управление](workbooks-access-control.md) доступом к ресурсам книги и предоставление общего доступа к ним.
