---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: ff7ba04271c150018f2c55b62e40542a686608cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "67185797"
---
## <a name="create-a-client-connection"></a><a name="create-client"></a>Создание клиентского соединения
Создайте подключение клиента, создав объект `WindowsAzure.MobileServiceClient` .  Замените `appUrl` URL-адресом в своем мобильном приложении.

```javascript
var client = WindowsAzure.MobileServiceClient(appUrl);
```

## <a name="work-with-tables"></a><a name="table-reference"></a>Работа со столами
Для доступа к данным или их обновления создайте ссылку на таблицу серверной части. Замените `tableName` на имя таблицы.

```javascript
var table = client.getTable(tableName);
```

Имея ссылку на таблицу, можно работать с этой таблицей:

* [Запрос таблицы](#querying)
  * [Фильтрация данных](#table-filter)
  * [Разбиение данных по страницам](#table-paging)
  * [Сортировка данных](#sorting-data)
* [Вставка данных](#inserting)
* [Изменение данных](#modifying)
* [Удалять данные](#deleting)

### <a name="how-to-query-a-table-reference"></a><a name="querying"></a>Как: Запрос таблицы ссылки
Ссылку на таблицу можно использовать для запроса данных на сервере.  Запросы осуществляются на языке типа LINQ.
Чтобы вернуть все данные из таблицы, используйте следующий код:

```javascript
/**
 * Process the results that are received by a call to table.read()
 *
 * @param {Object} results the results as a pseudo-array
 * @param {int} results.length the length of the results array
 * @param {Object} results[] the individual results
 */
function success(results) {
   var numItemsRead = results.length;

   for (var i = 0 ; i < results.length ; i++) {
       var row = results[i];
       // Each row is an object - the properties are the columns
   }
}

function failure(error) {
    throw new Error('Error loading data: ', error);
}

table
    .read()
    .then(success, failure);
```

Выполненная функция вызывается с результатами.  Не используйте в этой функции свойство `for (var i in results)` — оно вызывает перебор данных, включаемых в результаты применения других функций запросов (таких как `.includeTotalCount()`).

Дополнительные сведения о синтаксисе запроса см. в [документации по объектам запросов].

#### <a name="filtering-data-on-the-server"></a><a name="table-filter"></a>Фильтрация данных на сервере
Используйте предложение `where` с ссылкой на таблицу.

```javascript
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

Также можно использовать функцию фильтрации объектов.  В этом случае переменная `this` присваивается текущему объекту фильтрации.  Представленный ниже код функционально эквивалентен предыдущему примеру:

```javascript
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

#### <a name="paging-through-data"></a><a name="table-paging"></a>Прогон через данные
Используйте методы `take()` и `skip()`.  Например, если вы хотите разбить на строки таблицу, содержащую 100 записей:

```javascript
var totalCount = 0, pages = 0;

// Step 1 - get the total number of records
table.includeTotalCount().take(0).read(function (results) {
    totalCount = results.totalCount;
    pages = Math.floor(totalCount/100) + 1;
    loadPage(0);
}, failure);

function loadPage(pageNum) {
    let skip = pageNum * 100;
    table.skip(skip).take(100).read(function (results) {
        for (var i = 0 ; i < results.length ; i++) {
            var row = results[i];
            // Process each row
        }
    }
}
```

Метод `.includeTotalCount()` добавляет в объект результатов поле totalCount.  В этом поле указывается общее количество записей, которое будет возвращено, если данные не разбиты на страницы.

После этого можно настроить список страниц, используя переменную pages и определенные кнопки пользовательского интерфейса, и загрузить новые данные на каждой странице с помощью метода `loadPage()`.  Чтобы ускорить доступ к уже загруженным записям, добавьте кэширование.

#### <a name="how-to-return-sorted-data"></a><a name="sorting-data"></a>Практическое руководство. Возврат отсортированных данных
Используйте методы запроса `.orderBy()` или `.orderByDescending()`:

```javascript
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

Дополнительные сведения об объекте запроса см. в [документации по объектам запросов].

### <a name="how-to-insert-data"></a><a name="inserting"></a>Как вставить данные
Создайте объект JavaScript с соответствующей датой и асинхронно вызовите метод `table.insert()`:

```javascript
var newItem = {
    name: 'My Name',
    signupDate: new Date()
};

table
    .insert(newItem)
    .done(function (insertedItem) {
        var id = insertedItem.id;
    }, failure);
```

Вставленный элемент возвращается с дополнительными полями, необходимыми для операций синхронизации.  Обновите свой кэш, используя эти данные для последующих обновлений.

Серверный пакет SDK мобильных приложений Azure для Node.js поддерживает динамическую схему для целей разработки.  Динамическая схема позволяет добавлять столбцы в таблицу, указывая их в операциях вставки или обновления.  Перед переносом приложения в рабочую среду динамическую схему рекомендуется отключить.

### <a name="how-to-modify-data"></a><a name="modifying"></a>Практическое руководство. Изменение данных
Как и при использовании метода `.insert()`, в этом случае следует создать объект обновления и вызвать метод `.update()`.  Объект обновления должен содержать идентификатор обновляемой записи, получаемый при ее чтении или при вызове метода `.insert()`.

```javascript
var updateItem = {
    id: '7163bc7a-70b2-4dde-98e9-8818969611bd',
    name: 'My New Name'
};

table
    .update(updateItem)
    .done(function (updatedItem) {
        // You can now update your cached copy
    }, failure);
```

### <a name="how-to-delete-data"></a><a name="deleting"></a>Как удалить данные
Для удаления записи вызовите метод `.del()`.  В ссылке на объект укажите идентификатор:

```javascript
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
