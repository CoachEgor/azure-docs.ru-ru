---
title: Общие сведения о Когнитивном поиске Azure
titleSuffix: Azure Cognitive Search
description: Когнитивный поиск Azure — это полностью управляемая облачная служба поиска Майкрософт. Описание вариантов использования, рабочего процесса разработки, сравнение с другими продуктами Майкрософт для поиска и инструкции по началу работы.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 11/24/2020
ms.custom: contperf-fy21q1
ms.openlocfilehash: 01f61eac31c388cd0bf8a857beb7a10944187c6b
ms.sourcegitcommit: 3ea45bbda81be0a869274353e7f6a99e4b83afe2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2020
ms.locfileid: "97027906"
---
# <a name="what-is-azure-cognitive-search"></a>Что собой представляет Когнитивный поиск Azure?

Когнитивный поиск Azure ([ранее известный как "Поиск Azure"](whats-new.md#new-service-name)) — это облачная служба поиска, которая предоставляет разработчикам API и средства для создания расширенных возможностей при работе с конфиденциальным и разнородным содержимым веб-приложений, а также мобильных и корпоративных приложений. 

При создании службы "Когнитивный поиск" вы получите:

+ поисковую систему, выполняющую индексирование и выполнение запросов;
+ постоянное хранилище создаваемых и администрируемых индексов поиска;
+ язык запросов для создания простых и сложных запросов;
+ ориентирование на искусственный интеллект при анализе, в результате которого создается пригодное для поиска содержимое на основе изображений, необработанного текста и файлов приложений;
+ интеграция с источниками данных Azure с помощью индексаторов поиска, что обеспечивает автоматизацию импорта и обновления данных.

Архитектурно служба поиска находится между внешними хранилищами данных, которые содержат неиндексированные данные, и клиентским приложением, которое отправляет запросы в индекс поиска и обрабатывает ответ.

![Архитектура Когнитивного поиска Azure](media/search-what-is-azure-search/azure-search-diagram.svg "Архитектура Когнитивного поиска Azure")

Внешне служба поиска интегрируется с другими службами Azure в форме *индексаторов*, которые автоматизируют прием и получение данных из источников данных Azure, а также *набора навыков* на основе ИИ из Cognitive Services, например анализа изображений и текста, или настраиваемого ИИ, созданного в Машинном обучении Azure или импортированного из Функций Azure.

В самой службе поиска находятся две основные рабочие нагрузки: *индексирование* и *запросы*. 

+ Индексирование переносит текст в службу поиска и подготавливает его для поиска. На внутреннем уровне входящий текст обрабатывается в маркерах безопасности и хранится в инвертированных индексах для быстрой проверки. 

  Во время индексирования можно добавить *обогащение с помощью ИИ* за счет [ когнитивных навыков](cognitive-search-working-with-skillsets.md), предварительно определенных из Майкрософт, либо настраиваемых и созданных вами навыков. Последующие анализ и преобразования могут привести к созданию новых сведений и структур, которые ранее не существовали, обеспечивая значительное удобство для многих сценариев поиска и анализа знаний.

+ После заполнения индекса поисковыми данными клиентское приложение отправляет запросы в службу поиска и обрабатывает ответы. Запрос обрабатывается по поисковому индексу, который вы создаете, которым владеете и который сохраняете в своей службе. В клиентском приложении интерфейс поиска определяется с помощью API-интерфейсов Когнитивного поиска Azure и может предусматривать настройку релевантности, автозаполнение, сопоставление синонимов, нечеткое сопоставление, сопоставление шаблонов, фильтрацию и сортировку.

Эта служба предоставляется через простой [REST API](/rest/api/searchservice/) или [пакет SDK для .NET](search-howto-dotnet-sdk.md), которые скрывают естественную сложность извлечения информации. Вы также можете использовать портал Azure для администрирования служб и управления содержимым с помощью средств для создания прототипов и запросов к индексам и навыкам. Поскольку служба работает в облаке, инфраструктурой и доступностью управляет корпорация Майкрософт.

## <a name="why-use-cognitive-search"></a>Зачем использовать Когнитивный поиск Azure

Когнитивный поиск Azure хорошо подходит для следующих сценариев приложений:

+ Консолидация разнородного содержимого в закрытый, определяемый пользователем индекс поиска. Индекс поиска можно заполнить потоками документов JSON из любого источника. В случае поддерживаемых источников в Azure для автоматизации индексирования используйте *индексатор*. Управление схемой индекса и расписанием обновления является ключевой причиной использования Когнитивного поиска.

+ Простая реализация функций, связанных с поиском. API службы поиска Azure упрощают создание запросов, фасетную навигацию, фильтры (включая геопространственный поиск), сопоставление синонимов, автодополнение и настройку релевантности. Встроенные функции позволяют удовлетворить все требования пользователей к поиску, предоставляя возможности на уровне коммерческих систем поиска в Интернете.

+ Необработанное содержимое — это большие единообразные текстовые файлы или изображения или файлы приложений, хранящиеся в хранилище BLOB-объектов Azure или Cosmos DB. Вы можете применять [когнитивные навыки](cognitive-search-concept-intro.md) во время индексирования, чтобы находить и извлекать текст, создавать структуру или создавать новые данные, например переведенный текст или сущности.

+ Для содержимого требуется лингвистический или пользовательский анализ текста. При работе с содержимым на языке, отличном от английского, Когнитивный поиск Azure поддерживает анализаторы Lucene и процессоры естественного языка Майкрософт. Можно также настроить анализаторы для выполнения специализированной обработки необработанного содержимого, например для фильтрации диакритических знаков или распознавания и сохранения шаблонов в строках.

Дополнительные сведения о конкретных функциональных возможностях см. в статье [Возможности Когнитивного поиска Azure](search-features-list.md)

## <a name="how-to-get-started"></a>Начало работы

Полное изучение основных функций поиска можно выполнить в четыре этапа:

1. [**Создание службы поиска**](search-create-service-portal.md) на уровне "Бесплатный", который используется совместно с другими подписчиками, или [на платном уровне](https://azure.microsoft.com/pricing/details/search/) для выделенных ресурсов, используемых только вашей службой. Все руководства можно выполнить в бесплатной версии службы.

1. [**Создание индекса поиска**](search-what-is-an-index.md) с помощью портала и [REST API](/rest/api/searchservice/create-index). [Пакет SDK для .NET](search-howto-dotnet-sdk.md) или другой пакет SDK. Схема индекса определяет структуру содержимого, доступного для поиска.

1. [**Передача содержимого**](search-what-is-data-import.md) в индекс. Используйте [модель "принудительная отправка"](tutorial-optimize-indexing-push-api.md) для отправки документов JSON из любого источника или используйте [ модель "извлечение данных" (индексаторы)](search-indexer-overview.md), если исходные данные находятся в Azure.

1. [**Запрос индекса**](search-query-overview.md) с помощью [обозревателя поиска](search-explorer.md) на портале, [REST API](search-get-started-rest.md), [пакета SDK для .NET](/dotnet/api/azure.search.documents.searchclient.search) или другого пакета SDK.

> [!TIP]
> Ускорьте процесс, начав с [**мастера импорта данных**](search-get-started-portal.md), а также источника данных Azure для создания, загрузки и запроса индекса за считаные минуты.

## <a name="how-it-compares"></a>Сопоставление

Пользователи часто спрашивают, чем служба "Когнитивный поиск Azure" отличается от других решений для поиска. В следующей таблице представлены основные отличия.

| По сравнению с | Основные отличия |
|-------------|-----------------|
| Поиск (Майкрософт) | [Поиск (Майкрософт)](/microsoftsearch/overview-microsoft-search) предназначен для прошедших проверку подлинности пользователей Microsoft 365, которым необходимо выполнять запросы к содержимому в SharePoint. Он предлагается как готовая к использованию функция поиска, включенная и настроенная администраторами, с возможностью принимать внешнее содержимое через соединители от корпорации Майкрософт и из других источников. Если это подходит под описание вашего сценария, Поиск (Майкрософт) с Microsoft 365 является привлекательным вариантом для изучения.<br/><br/>В отличие от него служба "Когнитивный поиск Azure" выполняет запросы к определенному индексу, заполненному вашими данными и документами, часто из различных источников. Служба "Когнитивный поиск Azure" предоставляет возможности программы-обходчика для некоторых источников данных Azure через [индексаторы](search-indexer-overview.md), но вы можете также отправить любой документ JSON, который соответствует схеме индекса, в отдельный объединенный ресурс с поддержкой поиска. Кроме того, можно настроить конвейер индексирования так, чтобы он включал машинное обучение и лексические анализаторы. Так как служба "Когнитивный поиск" представляет собой подключаемый компонент в более крупных решениях, ее можно интегрировать практически в любое приложение на любой платформе.|
|Bing; | [API Bing для поиска в Интернете](../cognitive-services/bing-web-search/index.yml) ищет совпадение с введенными словами в индексах на сайте Bing.com. Индексы создаются на основе содержимого HTML, XML и другого веб-содержимого с общедоступных сайтов. Служба [Пользовательский поиск Bing](/azure/cognitive-services/bing-custom-search/) основана на той же платформе и предлагает ту же технологию поискового модуля для типов веб-содержимого, но предназначена для отдельных веб-сайтов.<br/><br/>В службе "Когнитивный поиск" можно определить и заполнить индекс. С помощью [индексаторов](search-indexer-overview.md) можно сканировать данные в источниках данных Azure, а также отправлять в службу поиска любой документ JSON, соответствующий индексу. |
|Поиск по базе данных | Многие платформы баз данных имеют встроенный поисковый интерфейс. SQL Server поддерживает [полнотекстовый поиск](/sql/relational-databases/search/full-text-search). Cosmos DB и аналогичные технологии содержат индексы с поддержкой запросов. При оценке систем, сочетающих возможности поиска и хранения, бывает сложно выбрать оптимальный вариант. Во многих решениях они используются одновременно: СУБД — для хранения, а служба "Когнитивный поиск Azure" — для специализированных возможностей поиска.<br/><br/>В отличие от поиска в СУБД, служба "Когнитивный поиск Azure" хранит содержимое из разнородных источников и поддерживает специализированные функции обработки текста, например возможность лингвистической обработки (выделение основы, лемматизация, словоформы) на [56 языках](/rest/api/searchservice/language-support). Также эта служба поддерживает автоматическое исправление слов с ошибками, [синонимы](/rest/api/searchservice/synonym-map-operations), [предложения](/rest/api/searchservice/suggestions), [управление оценкой](/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [аспекты](./search-filters-facets.md) и [пользовательскую разметку](/rest/api/searchservice/custom-analyzers-in-azure-search). [Полнотекстовая поисковая система](search-lucene-query-architecture.md) в службе "Когнитивный поиск Azure" основана на Apache Lucene, отраслевом стандарте в области поиска информации. Тем не менее, хотя Когнитивный поиск Azure сохраняет данные в виде инвертированного индекса, он не заменяет настоящее хранилище данных, и мы не рекомендуем использовать его в таком качестве. Дополнительные сведения см. в [этой теме форума](https://stackoverflow.com/questions/40101159/can-azure-search-be-used-as-a-primary-database-for-some-data). <br/><br/>Другим важным отличием в этой категории является использование ресурсов. Для индексирования и некоторых запросов часто требуются большие объемы вычислительных ресурсов. Перенос заданий поиска из СУБД в специализированное облачное решение позволяет сберечь ресурсы для обработки транзакций. Более того, внешнее решение для поиска упрощает масштабирование при изменении объема запросов.|
|Специализированное решение для поиска | Предположим, что вы решили использовать выделенное решение поиска с полный спектром функций. Теперь вам важно сравнить возможности локальных и облачных служб в этой категории. Многие технологии поиска предоставляют контроль над конвейерами индексирования и запросов, расширенный синтаксис запросов и фильтров, контроль над ранжированием и релевантностью, а также функции автоматического и интеллектуального поиска. <br/><br/>Облачная служба является правильным выбором, если вам необходимо готовое масштабируемое решение, которое почти не нуждается в обслуживании и дополнительных затратах. <br/><br/>В рамках облачной модели некоторые поставщики предоставляют сопоставимые базовые функции с полнотекстовым и геопространственным поиском и возможность обработки определенного уровня неоднозначности во входных данных для поиска. Обычно выбор подходящего варианта зависит от наличия [специализированных функций](search-features-list.md), простых и удобных API-интерфейсов, а также нужных инструментов и средств управления. |

Среди поставщиков облачных услуг Когнитивный поиск Azure наиболее эффективен для рабочих нагрузок полнотекстового поиска в хранилищах содержимого и базах данных Azure для приложений, которые в первую очередь полагаются на поиск для получения сведений и навигации по содержимому. 

Ниже перечислены ключевые преимущества.

+ Интеграция данных Azure (с использованием поисковых модулей) на уровне индексирования
+ Портал Azure для централизованного управления
+ Масштабирование, надежность и доступность мирового уровня в Azure
+ Обработка необработанных данных с использованием искусственного интеллекта делает данные более доступными для поиска. Сюда относится текст с изображений и поиск закономерностей в неструктурированном содержимом.
+ Лингвистический и пользовательский анализ с анализаторами для надежного полнотекстового поиска, доступными на 56 языках
+ [Основные функции, общие для поисковых приложений:](search-features-list.md) система оценок, фасетная навигация, предложения, синонимы, геопространственный поиск и многое другое.

Наши клиенты могут использовать множество функций службы Когнитивного поиска Azure, в том числе интернет-каталоги, бизнес-программы и приложения обнаружения документов.

## <a name="watch-this-video"></a>Рекомендуемый видеоролик

В этом 15-минутном видео руководитель программы Луис Кабрера (Luis Cabrera) рассказывает о Когнитивном поиске Azure.

>[!VIDEO https://www.youtube.com/embed/kOJU0YZodVk?version=3]