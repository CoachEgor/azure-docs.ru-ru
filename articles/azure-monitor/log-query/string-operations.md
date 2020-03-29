---
title: Работа со строками в запросах журнала Azure Monitor | Документация Майкрософт
description: Описано редактирование, сравнение, выполнение поиска и многие другие операции со строками в запросах журнала Azure Monitor.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: a394fee7178b2e3e167c8bd905ab175b25d1d813
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "75397469"
---
# <a name="work-with-strings-in-azure-monitor-log-queries"></a>Работа со строками в запросах журнала Azure Monitor


> [!NOTE]
> Перед завершением этого учебника необходимо завершить [работу с аналитикой журналов Azure Monitor Log Analytics](get-started-portal.md) и начать работу с [запросами журналов Azure Monitor.](get-started-queries.md)

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

В этой статье описывается, как редактировать, сравнивать, выполнять поиск и многие другие операции со строками.

Каждый символ в строке имеет номер индекса в соответствии с его расположением. Первый символ находится на индексе 0, следующий символ 1, и так далее. Различные строковые функции используют номера индексов по-разному, как показано в следующих разделах. Во многих приведенных примерах используется команда **print** для демонстрации работы со строками без использования определенного источника данных.


## <a name="strings-and-escaping-them"></a>Строки и их экранирование
Строковые значения заключаются в одинарные или двойные кавычки. Backslash\\( ) используется для избежания символов к персонажу, следующему за ним, \" например, к t для вкладки, n для newline и самому символу цитаты.

```Kusto
print "this is a 'string' literal in double \" quotes"
```

```Kusto
print 'this is a "string" literal in single \' quotes'
```

Чтобы символ \\ не интерпретировался как escape-символ, добавьте \@ в качестве префикса к строке:

```Kusto
print @"C:\backslash\not\escaped\with @ prefix"
```


## <a name="string-comparisons"></a>Сравнения строк

Оператор       |Описание                         |С учетом регистра|Пример (при `true`)
---------------|------------------------------------|--------------|-----------------------
`==`           |Равно                              |Да           |`"aBc" == "aBc"`
`!=`           |Не равно                          |Да           |`"abc" != "ABC"`
`=~`           |Равно                              |нет            |`"abc" =~ "ABC"`
`!~`           |Не равно                          |нет            |`"aBc" !~ "xyz"`
`has`          |Правая часть представляет собой все слово в левой части |нет|`"North America" has "america"`
`!has`         |Правая часть не является всем словом в левой части       |нет            |`"North America" !has "amer"` 
`has_cs`       |Правая часть представляет собой все слово в левой части |Да|`"North America" has_cs "America"`
`!has_cs`      |Правая часть не является всем словом в левой части       |Да            |`"North America" !has_cs "amer"` 
`hasprefix`    |Правая часть представляет собой префикс слова в левой части         |нет            |`"North America" hasprefix "ame"`
`!hasprefix`   |Правая часть не является префиксом слова в левой части     |нет            |`"North America" !hasprefix "mer"` 
`hasprefix_cs`    |Правая часть представляет собой префикс слова в левой части         |Да            |`"North America" hasprefix_cs "Ame"`
`!hasprefix_cs`   |Правая часть не является префиксом слова в левой части     |Да            |`"North America" !hasprefix_cs "CA"` 
`hassuffix`    |Правая часть представляет собой суффикс слова в левой части         |нет            |`"North America" hassuffix "ica"`
`!hassuffix`   |Правая часть не является суффиксом слова в левой части     |нет            |`"North America" !hassuffix "americ"`
`hassuffix_cs`    |Правая часть представляет собой суффикс слова в левой части         |Да            |`"North America" hassuffix_cs "ica"`
`!hassuffix_cs`   |Правая часть не является суффиксом слова в левой части     |Да            |`"North America" !hassuffix_cs "icA"`
`contains`     |Правая часть является вхождением в левую часть  |нет            |`"FabriKam" contains "BRik"`
`!contains`    |Правая часть не входит в левую часть           |нет            |`"Fabrikam" !contains "xyz"`
`contains_cs`   |Правая часть является вхождением в левую часть  |Да           |`"FabriKam" contains_cs "Kam"`
`!contains_cs`  |Правая часть не входит в левую часть           |Да           |`"Fabrikam" !contains_cs "Kam"`
`startswith`   |Правая часть является начальным вхождением в левую часть|нет            |`"Fabrikam" startswith "fab"`
`!startswith`  |Правая часть не является начальным вхождением в левую часть|нет        |`"Fabrikam" !startswith "kam"`
`startswith_cs`   |Правая часть является начальным вхождением в левую часть|Да            |`"Fabrikam" startswith_cs "Fab"`
`!startswith_cs`  |Правая часть не является начальным вхождением в левую часть|Да        |`"Fabrikam" !startswith_cs "fab"`
`endswith`     |Правая часть является конечным вхождением в левую часть|нет             |`"Fabrikam" endswith "Kam"`
`!endswith`    |Правая часть не является конечным вхождением в левую часть|нет         |`"Fabrikam" !endswith "brik"`
`endswith_cs`     |Правая часть является конечным вхождением в левую часть|Да             |`"Fabrikam" endswith "Kam"`
`!endswith_cs`    |Правая часть не является конечным вхождением в левую часть|Да         |`"Fabrikam" !endswith "brik"`
`matches regex`|Левая часть содержит соответствие для правой части        |Да           |`"Fabrikam" matches regex "b.*k"`
`in`           |Соответствует одному из элементов       |Да           |`"abc" in ("123", "345", "abc")`
`!in`          |Не соответствует одному из элементов   |Да           |`"bca" !in ("123", "345", "abc")`


## <a name="countof"></a>countof

Подсчитывает вхождения подстроки в строку. Можно сопоставить текстовые строки или использовать регулярное выражение. Совпадения для текстовых строк могут перекрывать друг друга, при этом для регулярных выражений — не могут.

### <a name="syntax"></a>Синтаксис
```
countof(text, search [, kind])
```

### <a name="arguments"></a>Аргументы:
- `text` — строка входных данных. 
- `search` — текстовая строка или регулярное выражение, которое проверяется на вхождение в text.
- `kind` - _нормальный_ | _регекс_ (по умолчанию: нормальный).

### <a name="returns"></a>Результаты

Количество совпадений для строки поиска в контейнере. Совпадения для текстовых строк могут перекрывать друг друга, при этом для регулярных выражений — не могут.

### <a name="examples"></a>Примеры

#### <a name="plain-string-matches"></a>Совпадения текстовой строки

```Kusto
print countof("The cat sat on the mat", "at");  //result: 3
print countof("aaa", "a");  //result: 3
print countof("aaaa", "aa");  //result: 3 (not 2!)
print countof("ababa", "ab", "normal");  //result: 2
print countof("ababa", "aba");  //result: 2
```

#### <a name="regex-matches"></a>Совпадения регулярного выражения

```Kusto
print countof("The cat sat on the mat", @"\b.at\b", "regex");  //result: 3
print countof("ababa", "aba", "regex");  //result: 1
print countof("abcabc", "a.c", "regex");  // result: 2
```


## <a name="extract"></a>extract

Получает соответствие регулярного выражения из указанной строки. Дополнительно также преобразует извлеченную подстроку в указанный тип.

### <a name="syntax"></a>Синтаксис

```Kusto
extract(regex, captureGroup, text [, typeLiteral])
```

### <a name="arguments"></a>Аргументы

- `regex` — регулярное выражение.
- `captureGroup` — положительная целочисленная константа, указывающая извлекаемую группу записи. 0 для полного совпадения, 1 — значение, соответствующее первой "("круглой скобке")" в регулярном выражении, 2 или более — последующим круглым скобкам.
- `text` — строка для поиска.
- `typeLiteral` — необязательный литерал типа (например, typeof(long)). Если указан, то извлеченная подстрока преобразуется в этот тип.

### <a name="returns"></a>Результаты
Возвращается подстрока, которая сравнивалась с указанной в группе записи captureGroup, при необходимости преобразованная в тип typeLiteral.
Если соответствия нет или не удается выполнить преобразование типа, возвращается NULL.

### <a name="examples"></a>Примеры

Следующий пример извлекает последний октет *ComputerIP* из записи пакета пульса:
```Kusto
Heartbeat
| where ComputerIP != "" 
| take 1
| project ComputerIP, last_octet=extract("([0-9]*$)", 1, ComputerIP) 
```

Следующий пример извлекает последний октет, приводит его к типу (числа) *real* и вычисляет следующее значение IP-адреса:
```Kusto
Heartbeat
| where ComputerIP != "" 
| take 1
| extend last_octet=extract("([0-9]*$)", 1, ComputerIP, typeof(real)) 
| extend next_ip=(last_octet+1)%255
| project ComputerIP, last_octet, next_ip
```

Ниже в примере строки *Trace* производится поиск определения "Duration". Соответствие приводится к типу *real* и умножается на константу времени (1s), *которая приводит Duration к типу timespan*.
```Kusto
let Trace="A=12, B=34, Duration=567, ...";
print Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real));  //result: 567
print Duration_seconds =  extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s);  //result: 00:09:27
```


## <a name="isempty-isnotempty-notempty"></a>isempty, isnotempty, notempty

- *isempty* возвращает значение true, если аргумент является пустой строкой или NULL (см. также *isnull*).
- *isnotempty* возвращает значение true, если аргумент не является пустой строкой или NULL (см. также *isnotnull*), Псевдоним: *notempty*.

### <a name="syntax"></a>Синтаксис

```Kusto
isempty(value)
isnotempty(value)
```

### <a name="examples"></a>Примеры

```Kusto
print isempty("");  // result: true

print isempty("0");  // result: false

print isempty(0);  // result: false

print isempty(5);  // result: false

Heartbeat | where isnotempty(ComputerIP) | take 1  // return 1 Heartbeat record in which ComputerIP isn't empty
```


## <a name="parseurl"></a>parseurl

Разделяет URL-адрес на части (протокол, узел, порт и т. д.) и возвращает объект словаря, содержащий элементы в виде строк.

### <a name="syntax"></a>Синтаксис

```
parseurl(urlstring)
```

### <a name="examples"></a>Примеры

```Kusto
print parseurl("http://user:pass@contoso.com/icecream/buy.aspx?a=1&b=2#tag")
```

Результат будет таким:
```
{
    "Scheme" : "http",
    "Host" : "contoso.com",
    "Port" : "80",
    "Path" : "/icecream/buy.aspx",
    "Username" : "user",
    "Password" : "pass",
    "Query Parameters" : {"a":"1","b":"2"},
    "Fragment" : "tag"
}
```


## <a name="replace"></a>replace

Заменяет все найденные совпадения регулярного выражения другой строкой. 

### <a name="syntax"></a>Синтаксис

```
replace(regex, rewrite, input_text)
```

### <a name="arguments"></a>Аргументы

- `regex` — регулярное выражение для сравнения. Оно может содержать группы захвата в '('круглых скобках')'.
- `rewrite` — регулярное выражение, результат которого заменит все соответствия, найденные сопоставлениями регулярного выражения. Используйте \0 для указания полного соответствия, \1 для первой группы записи, \2 и так далее — для следующих групп записи.
- `input_text` — строка входных данных для поиска.

### <a name="returns"></a>Результаты
Текст после замены всех вхождений регулярного выражения результатами вычислений rewrite. Совпадения не перекрываются.

### <a name="examples"></a>Примеры

```Kusto
SecurityEvent
| take 1
| project Activity 
| extend replaced = replace(@"(\d+) -", @"Activity ID \1: ", Activity) 
```

Можно получить следующие результаты:

Действие                                        |заменена
------------------------------------------------|----------------------------------------------------------
4663 — была предпринята попытка доступа к объекту.  |Идентификатор действия 4663 — была предпринята попытка доступа к объекту.


## <a name="split"></a>split

Разделяет указанную строку в соответствии с указанным разделителем и возвращает массив результирующих подстрок.

### <a name="syntax"></a>Синтаксис
```
split(source, delimiter [, requestedIndex])
```

### <a name="arguments"></a>Аргументы:

- `source` — строка для разделения в соответствии с указанным разделителем.
- `delimiter` — разделитель, который будет использоваться для разделения исходной строки.
- `requestedIndex` — необязательный индекс, начинающийся с нуля. Если указан, то возвращаемый массив строк будет содержать только этот элемент (если существует).


### <a name="examples"></a>Примеры

```Kusto
print split("aaa_bbb_ccc", "_");    // result: ["aaa","bbb","ccc"]
print split("aa_bb", "_");          // result: ["aa","bb"]
print split("aaa_bbb_ccc", "_", 1); // result: ["bbb"]
print split("", "_");               // result: [""]
print split("a__b", "_");           // result: ["a","","b"]
print split("aabbcc", "bb");        // result: ["aa","cc"]
```

## <a name="strcat"></a>strcat

Сцепляет строковые аргументы (поддерживает от 1 до 16 аргументов).

### <a name="syntax"></a>Синтаксис
```
strcat("string1", "string2", "string3")
```

### <a name="examples"></a>Примеры
```Kusto
print strcat("hello", " ", "world") // result: "hello world"
```


## <a name="strlen"></a>strlen

Возвращает длину строки.

### <a name="syntax"></a>Синтаксис
```
strlen("text_to_evaluate")
```

### <a name="examples"></a>Примеры
```Kusto
print strlen("hello")   // result: 5
```


## <a name="substring"></a>substring

Извлекает подстроку из заданной исходной строки, начиная с указанного индекса. При необходимости можно указать длину запрашиваемой подстроки.

### <a name="syntax"></a>Синтаксис
```
substring(source, startingIndex [, length])
```

### <a name="arguments"></a>Аргументы:

- `source` — исходная строка, из которой будет извлечена подстрока.
- `startingIndex` — отсчитываемая от нуля позиция первого знака запрашиваемой подстроки.
- `length` — необязательный параметр, который указывает требуемую длину возвращаемой подстроки.

### <a name="examples"></a>Примеры
```Kusto
print substring("abcdefg", 1, 2);   // result: "bc"
print substring("123456", 1);       // result: "23456"
print substring("123456", 2, 2);    // result: "34"
print substring("ABCD", 0, 2);  // result: "AB"
```


## <a name="tolower-toupper"></a>tolower, toupper

Преобразует все буквы заданной строки в нижний или верхний регистр.

### <a name="syntax"></a>Синтаксис
```
tolower("value")
toupper("value")
```

### <a name="examples"></a>Примеры
```Kusto
print tolower("HELLO"); // result: "hello"
print toupper("hello"); // result: "HELLO"
```



## <a name="next-steps"></a>Дальнейшие действия
Ознакомьтесь с дополнительными руководствами по следующим ссылкам:
* [Функции агрегирования](aggregations.md)
* [Расширенные статистические функции в запросах Azure Log Analytics](advanced-aggregations.md)
* [Создание графиков и диаграмм](charts.md)
* [Работа c JSON и структурами данных](json-data-structures.md)
* [Составление расширенных запросов](advanced-query-writing.md)
* [Соединения в анализе данных из нескольких источников](joins.md)
