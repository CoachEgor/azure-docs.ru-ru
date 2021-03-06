---
title: Общие сведения о сходстве и оценке
titleSuffix: Azure Cognitive Search
description: Описываются основные понятия сходства и оценки, а также действия, которые разработчик может выполнить для настройки результатов оценки.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/08/2020
ms.openlocfilehash: 5bd1a9111528146224561995feaecf54612a1c78
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91535667"
---
# <a name="similarity-and-scoring-in-azure-cognitive-search"></a>Сходство и оценка в Когнитивном поиске Azure

Оценка — это вычисление оценки поиска для всех элементов, возвращенных в результатах поиска, для запросов полнотекстового поиска. Это показатель релевантности элемента в контексте текущей операции поиска. Чем выше оценка, тем более релевантен элемент. Элементы в результатах поиска упорядочиваются по рангу (от наивысшего до наинизшего) на основе оценок поиска, вычисленных для каждого элемента. 

По умолчанию в ответе возвращаются первые 50 элементов, но можно использовать параметр **$top**, чтобы вернуть меньше или больше элементов (до 1000 в одном ответе) и параметр **$skip**, чтобы получить следующий набор результатов.

Оценка поиска вычисляется на основе статистических свойств данных и запроса. Когнитивный поиск Azure находит документы, соответствующие условиям поиска (некоторые или все в зависимости от [searchMode](/rest/api/searchservice/search-documents#searchmodeany--all-optional)), отдавая предпочтение документам, которые содержат несколько экземпляров условия поиска. Оценка поиска возрастает, если условие поиска редко встречается в индексе данных, но часто — внутри документа. Основу для такого подхода к вычислению релевантности называют *TF-IDF* или частотой условия — инверсная частота в документе.

Значения оценки поиска в результирующем наборе могут повторяться. Если у нескольких совпадений одинаковая оценка поиска, порядок сортировки элементов не определен и не является стабильным. Выполните запрос еще раз, и, возможно, позиции элементов изменятся, особенно если вы используете бесплатную службу или оплачиваемую службу с несколькими репликами. Если есть два элемента с одинаковой оценкой, невозможно определить, какой из них отобразится первым.

Если вы хотите разорвать связь между повторяющимися оценками, вы можете добавить предложение **$orderby**, чтобы упорядочить элементы по оценке, а затем упорядочить по другому сортируемому полю (например, `$orderby=search.score() desc,Rating desc`). Дополнительные сведения см. в статье [Синтаксис $orderby OData в Когнитивном поиске Azure](./search-query-odata-orderby.md).

> [!NOTE]
> `@search.score = 1.00` указывает на результирующий набор без оценивания или без ранжирования. Оценка одинакова для всех результатов. Не оцениваемые результаты появляются, когда в форме запроса используется поиск нечетких соответствий, запросы с подстановочными знаками или регулярными выражениями либо выражение **$filter**. 

## <a name="scoring-profiles"></a>Профили оценки

Вы можете настроить способ ранжирования различных полей, определив пользовательский *профиль повышения*. Профили оценки позволяют управлять приоритетом элементов в результатах поиска. Например, может потребоваться повысить приоритет элементов на основе потенциального дохода, повысить уровень более новых элементов или, возможно, элементов, которые слишком долго находились на складе. 

Профиль повышения — часть определения индекса, состоящая из взвешенных полей, функций и параметров. Дополнительные сведения о таком определении см. в статье [Добавление профилей повышения в индекс службы Когнитивного поиска Azure](index-add-scoring-profiles.md).

<a name="scoring-statistics"></a>

## <a name="scoring-statistics-and-sticky-sessions"></a>Статистика оценки и закрепленные сеансы

Для масштабируемости Azure Когнитивный поиск распределяет каждый индекс по горизонтали через процесс сегментирования, что означает [физическое разделение частей индекса](search-capacity-planning.md#concepts-search-units-replicas-partitions-shards).

По умолчанию оценка документа вычисляется на основе статистических свойств данных *в сегменте*. Этот подход, как правило, не является проблемой для большого объема данных. Он обеспечивает лучшую производительность, чем вычисление оценки на основе информации по всем сегментам. Тем не менее, использование этой оптимизации производительности может привести к тому, что два очень похожих (или даже идентичных) документа будут иметь разные оценки релевантности, если они окажутся в разных сегментах.

Если вы предпочитаете вычислять оценку на основе статистических свойств всех сегментов, вы можете сделать это, добавив *scoringStatistics=global* в качестве [параметра запроса](/rest/api/searchservice/search-documents) (или добавив *"scoringStatistics": "global"* в качестве параметра текста [запроса](/rest/api/searchservice/search-documents)).

```http
GET https://[service name].search.windows.net/indexes/[index name]/docs?scoringStatistics=global&api-version=2020-06-30&search=[search term]
  Content-Type: application/json
  api-key: [admin or query key]  
```
Использование scoringStatistics гарантирует, что все сегменты в одной реплике обеспечивают одинаковые результаты. Тем не менее, разные реплики могут немного отличаться друг от друга, так как они всегда обновляются с последними изменениями в индексе. В некоторых случаях вам может потребоваться, чтобы пользователи во время "сеанса запроса" получали более согласованные результаты. В таких случаях вы можете предоставить `sessionId` как часть запросов. `sessionId` является уникальной строкой, которую вы создаете для ссылки на уникальный сеанс пользователя.

```http
GET https://[service name].search.windows.net/indexes/[index name]/docs?sessionId=[string]&api-version=2020-06-30&search=[search term]
  Content-Type: application/json
  api-key: [admin or query key]  
```
Пока используется та же самая строка `sessionId`, будет предпринята максимальная попытка нацеливания на одну и ту же реплику, что повысит согласованность результатов, которые увидят пользователи. 

> [!NOTE]
> Повторное использование одних и тех же значений `sessionId` может повлиять на балансировку нагрузки запросов по репликам, а также негативно повлиять на производительность службы поиска. Значение, используемое в качестве sessionId, не может начинаться с символа "_".

## <a name="similarity-ranking-algorithms"></a>Алгоритмы ранжирования сходства

Когнитивный поиск Azure поддерживает два различных алгоритма ранжирования сходства: *классический алгоритм сходства* и официальную реализацию алгоритма *Okapi BM25* (в настоящее время доступного в предварительной версии). Классический алгоритм сходства является алгоритмом по умолчанию. Однако начиная с 15 июля, все новые службы, созданные после этой даты, используют новый алгоритм BM25. Это единственный алгоритм, который доступен для новых служб.

Сейчас вы можете указать, какой алгоритм ранжирования сходства вы хотели бы использовать. Дополнительные сведения см. в статье [Алгоритм ранжирования в Когнитивном поиске Azure](index-ranking-similarity.md).

В следующем фрагменте видео приведено краткое описание алгоритмов ранжирования, которые используются в Когнитивном поиске Azure. Дополнительные сведения можно получить, просмотрев полное видео.

> [!VIDEO https://www.youtube.com/embed/Y_X6USgvB1g?version=3&start=322&end=643]

<a name="featuresMode-param"></a>

## <a name="featuresmode-parameter-preview"></a>параметр Феатуресмоде (Предварительная версия)

В запросах [поиска документов](/rest/api/searchservice/preview-api/search-documents) имеется новый параметр [феатуресмоде](/rest/api/searchservice/preview-api/search-documents#featuresmode) , который может предоставить дополнительные сведения о релевантности на уровне полей. В то время как `@searchScore` вычисляется для всего документа (то, насколько это относится к этому документу в контексте этого запроса), через феатуресмоде можно получить сведения об отдельных полях, как указано в `@search.features` структуре. Структура содержит все поля, используемые в запросе (определенные поля с помощью **searchFields** в запросе или все поля с атрибутами, поддерживающими **Поиск** в индексе). Для каждого поля можно получить следующие значения:

+ Число уникальных токенов, найденных в поле
+ Оценка подобия или мера аналогичного содержимого поля относительно термина запроса
+ Частота терминов или количество раз, когда термин запроса был найден в поле

Для запроса, предназначенного для полей "Description" и "Title", ответ, который содержит, `@search.features` может выглядеть следующим образом:

```json
"value": [
 {
    "@search.score": 5.1958685,
    "@search.features": {
        "description": {
            "uniqueTokenMatches": 1.0,
            "similarityScore": 0.29541412,
            "termFrequency" : 2
        },
        "title": {
            "uniqueTokenMatches": 3.0,
            "similarityScore": 1.75451557,
            "termFrequency" : 6
        }
```

Вы можете использовать эти точки данных в [пользовательских решениях оценки](https://github.com/Azure-Samples/search-ranking-tutorial) или воспользоваться информацией для отладки проблем, связанных с поиском.


## <a name="see-also"></a>См. также

 [Профили оценки](index-add-scoring-profiles.md) [REST API ссылки на](/rest/api/searchservice/) [поисковые документы API](/rest/api/searchservice/search-documents) [Azure когнитивный Поиск .NET SDK](/dotnet/api/overview/azure/search)