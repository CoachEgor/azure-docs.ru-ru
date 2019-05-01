---
title: Взаимодействие с отчетами с помощью интерфейса API JavaScript | Документация Майкрософт
description: Интерфейс API JavaScript службы Power BI позволяет с легкостью внедрять отчеты Power BI в приложения.
services: power-bi-workspace-collections
ms.service: power-bi-workspace-collections
author: rkarlin
ms.author: rkarlin
ms.topic: conceptual
ms.workload: powerbi
ms.date: 09/20/2017
ms.openlocfilehash: 252296af8b2065ae22bed8b421d4d00718b78287
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64705516"
---
# <a name="interact-with-power-bi-reports-using-the-javascript-api"></a>Взаимодействие с отчетами Power BI с помощью интерфейса API JavaScript

Интерфейс API JavaScript службы Power BI позволяет с легкостью внедрять отчеты Power BI в приложения. С помощью API ваши приложения могут программно взаимодействовать с разными элементами отчетов, например со страницами и фильтрами. Эти интерактивные возможности обеспечивают более полную интеграцию отчетов Power BI с приложениями.

> [!IMPORTANT]
> Использовать службу "Коллекции рабочих областей Power BI" не рекомендуется. Она будет доступна до июня 2018 года или до даты, указанной в договоре. Мы советуем организовать перенос приложения в Power BI Embedded, чтобы избежать прерываний в его работе. Сведения о том, как перенести данные из коллекций рабочих областей Power BI в Power BI Embedded, см. в [этой статье](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/).

Чтобы внедрить отчет Power BI в приложение, воспользуйтесь плавающим фреймом, размещенным как часть приложения. Как показано на рисунке ниже, плавающий фрейм выполняет роль границы между приложением и отчетом.

![Плавающий фрейм коллекции рабочих областей Power BI без API Javascript](media/interact-with-reports/iframe-without-javacript.png)

Плавающий фрейм значительно упрощает внедрение, но без интерфейса API JavaScript отчет и приложение не смогут взаимодействовать друг с другом. Из-за этого отчет не будет восприниматься как часть приложения. Между отчетом и приложением должен происходить обмен данными (см. рисунок ниже).

![Плавающий фрейм коллекции рабочих областей Power BI с API Javascript](media/interact-with-reports/iframe-with-javascript.png)

Интерфейс API JavaScript службы Power BI позволяет написать код, который может безопасно передаваться через границу плавающего фрейма. Это позволяет приложению программно выполнить действие в отчете и прослушать события, возникающие в результате действий пользователя в отчете.

## <a name="what-can-you-do-with-the-power-bi-javascript-api"></a>Что можно сделать с помощью интерфейса API JavaScript службы Power BI?

С помощью интерфейса API JavaScript вы можете управлять отчетами, переходить на те или иные страницы отчетов, фильтровать отчеты и управлять событиями внедрения. Структура интерфейса API показана на схеме ниже.

![Схема интерфейса API JavaScript службы Power BI](media/interact-with-reports/javascript-api-diagram.png)

### <a name="manage-reports"></a>Управление отчетами
Интерфейс API JavaScript дает возможность управлять поведением на уровне отчета и страницы.

* Безопасное внедрение отчетов Power BI в приложение (попробуйте воспользоваться [демонстрационным приложением внедрения](https://azure-samples.github.io/powerbi-angular-client/#/scenario1)
  * Задание маркера доступа.
* Настройка отчета.
  * Включение и отключение панели фильтров и панели навигации по страницам (попробуйте воспользоваться [демонстрационным приложением обновления параметров](https://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  * Установка значений по умолчанию для страниц и фильтров (попробуйте воспользоваться [демонстрацией задания значений по умолчанию](https://azure-samples.github.io/powerbi-angular-client/#/scenario5)
* Включение и выключение полноэкранного режима.

[Дополнительные сведения о внедрении отчетов.](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)

### <a name="navigate-to-pages-in-a-report"></a>Навигация по страницам отчета
Интерфейс API JavaScript позволяет обнаруживать все страницы в отчете и задавать текущую страницу. Попробуйте воспользоваться [демонстрационным приложением навигации](https://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[Дополнительные сведения о навигации по страницам.](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a>Фильтрация данных в отчете
Интерфейс API JavaScript предоставляет базовые и расширенные возможности фильтрации для внедренных отчетов и страниц отчетов. Попробуйте [демонстрационное приложение фильтрации](https://azure-samples.github.io/powerbi-angular-client/#/scenario4)и ознакомьтесь с базовым кодом.

#### <a name="basic-filters"></a>Базовые фильтры
Базовый фильтр размещается на уровне столбца или иерархии и содержит ряд значений, которые нужно добавить или исключить.

```typescript
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "https://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```

#### <a name="advanced-filters"></a>Расширенные фильтры
Расширенные фильтры используют логические операторы И и ИЛИ и могут принимать одно или два условия, в каждом из которых есть свой оператор и значение. Поддерживаются такие условия:

* Нет
* LessThan;
* LessThanOrEqual;
* GreaterThan
* GreaterThanOrEqual;
* Содержит
* DoesNotContain;
* StartsWith;
* DoesNotStartWith;
* Is;
* IsNot;
* IsBlank;
* IsNotBlank.

```typescript
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "https://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```

[Дополнительные сведения о фильтрации.](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)

### <a name="handling-events"></a>Обработка событий

Приложение может не только отправлять информацию в плавающий фрейм, но и получать от него информацию о следующих событиях:

* внедрение;
  * загрузка;
  * error
* Отчеты
  * изменение страницы;
  * выбор данных (ожидается в ближайшее время).

[Дополнительные сведения об обработке событий.](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об интерфейсе API JavaScript службы Power BI доступны по таким ссылкам:

* [Вики-сайт по интерфейсу API JavaScript](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
* [Справочник по объектным моделям](https://microsoft.github.io/powerbi-models/modules/_models_.html)
* [Демонстрация в реальном времени](https://microsoft.github.io/PowerBI-JavaScript/demo/)