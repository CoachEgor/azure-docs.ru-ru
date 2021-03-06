---
title: Общие сведения о Компьютерное зрение пространственном анализе
titleSuffix: Azure Cognitive Services
description: В этом документе объясняются основные понятия и функции контейнера Компьютерное зрение пространственного анализа.
services: cognitive-services
author: tchristiani
manager: nitinme
ms.author: terrychr
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 12/14/2020
ms.openlocfilehash: e017fac551e3122cc6586b32423ff166462ccad8
ms.sourcegitcommit: 63d0621404375d4ac64055f1df4177dfad3d6de6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/15/2020
ms.locfileid: "97513399"
---
# <a name="introduction-to-computer-vision-spatial-analysis"></a>Общие сведения о Компьютерное зрение пространственном анализе

Компьютерное зрение пространственного анализа — это новая функция Azure Cognitive Services Компьютерное зрение, которая помогает организациям максимально увеличить ценность своих физических пространств путем понимания движений и присутствия людей в определенной области. Он позволяет принимать видео от ККТВ или биообъектических камер, запускать навыки искусственного интеллекта для извлечения ценных сведений из видеопотоков и создавать события, которые будут использоваться в других системах. С помощью входных данных из потока камеры навык искусственного интеллекта может выполнять такие задачи, как подсчет числа людей, входящих в место, или Оценка соответствия требованиям социальных дистанЦинг.

## <a name="the-basics-of-spatial-analysis"></a>Основы пространственного анализа

В настоящее время основные навыки пространственного анализа основаны на конвейере, который принимает видео, обнаруживает людей в видео, отслеживает людей по мере их перемещения и создает события по мере того, как люди взаимодействуют с интересующими регионами.

## <a name="spatial-analysis-terms"></a>Термины пространственного анализа

| Термин | Определение |
|------|------------|
| Обнаружение людей | Этот компонент отвечает на вопрос «где находятся люди в этом образе»? Он находит людей в образе и передает ограничивающее поле, указывающее расположение каждого пользователя в компонент отслеживания людей. |
| Отслеживание людей | Этот компонент подключается к обнаружению людей с течением времени, когда люди переходят перед камерой. В нем используется логическая логика, описывающая, как люди обычно перемещают и изменяют основные сведения об общем внешнем представлении людей. Он не может отслеживать людей на нескольких камерах или переназначить кого-либо, которые исчезают более приблизительно за одну минуту. Отслеживание людей не использует никакие биометрические маркеры, такие как распознавание лиц или отслеживание гаит. |
| Интересующая область | Это зона или строка, определенные во входном видео как часть конфигурации. Когда человек взаимодействует с регионом видео, система создает событие. Например, для навыка Персонкроссинглине линия определяется в видео. Когда пользователь пересекает эту строку, создается событие. |
| Событие | Событие — это основные выходные данные пространственного анализа. Каждый из навыков периодически создает определенное событие (например, раз в минуту) или при возникновении определенного триггера. Это событие включает сведения о том, что произошло во входном видео, но не содержит изображений или видео. Например, навык Пеоплекаунт может создать событие, содержащее обновленное число при каждом изменении числа человек (триггер) или каждую минуту (периодически). |

## <a name="example-use-cases-for-spatial-analysis"></a>Примеры вариантов использования для пространственного анализа

Ниже приведены примеры использования, которые мы имели в виду при разработке и тестировании пространственного анализа.

**Соответствие социальных дистанЦинг** . в офисе есть несколько камер, использующих пространственный анализ для мониторинга соответствия требованиям социального дистанЦинг, измеряя расстояние между людьми. Диспетчер помещений может использовать тепловые карты, отображающий статистические данные о соответствии социального дистанЦинг по времени для корректировки рабочей области и упрощения социальных дистанЦинг.

**Анализ покупателей** . в магазине по продуктам используются камеры, на которые указывает продукт, чтобы измерять влияние изменений в продажах на трафик магазина. Система позволяет менеджеру магазина обнаруживать, какие новые продукты применяют наибольшее изменение в отношении задействования.

**Управление очередями** — камеры, на которые указывает очередь извлечения, предоставляют предупреждения руководителям, когда время ожидания становится слишком длинным, что позволяет им открывать больше строк. Исторические данные по отказаться в очередях дают ценные сведения о поведении потребителей.

**Создание & анализа** — здание Office использует камеры, которые обращаются к разделам, чтобы измерять футфалл и как люди используют рабочую область. Insights позволяет руководителю здания настроить службу и макет для лучшего обслуживания оккупантс.

**Минимальное распознавание персонала** — в центре обработки данных камеры отслеживают действия вокруг серверов. Когда сотрудники физически исправляют конфиденциальное оборудование, два человека всегда должны присутствовать во время восстановления по соображениям безопасности. Камеры используются для проверки соблюдения этого правила.

**Оптимизация рабочей области** . в быстром ресторанном ресторане камеры на кухни используются для создания статистических сведений о рабочем процессе сотрудников. Они используются менеджерами для улучшения процессов и обучения для группы.

## <a name="considerations-when-choosing-a-use-case"></a>Рекомендации по выбору варианта использования

**Предотвращение критических оповещений безопасности** — пространственный анализ не предназначен для обеспечения критической безопасности оповещений в реальном времени. Не следует полагаться на случаи, когда требуются оповещения в режиме реального времени, чтобы активировать вмешательство для предотвращения травм, например отключив интенсивность оборудования при наличии человека. Его можно использовать для уменьшения рисков с помощью статистики и вмешательства, чтобы уменьшить рискованное поведение, например, если пользователи входят в ограниченную или запрещенную область.

**Избегайте использования для принятия решений по найму** — пространственный анализ предоставляет вероятностная метрики, касающиеся расположения и перемещения людей в пространстве. Хотя эти данные могут оказаться полезными при статистическом усовершенствовании процесса, данные не являются хорошим показателем производительности отдельных рабочих ролей и не должны использоваться для принятия решений, связанных с приемом на работу.

**Старайтесь не использовать решения, связанные с** обделом здравоохранения. пространственный анализ предоставляет вероятностная и частичные данные, связанные с передвижениями людей. Данные не подходят для принятия решений, связанных с работоспособностью.

**Избегайте использования в защищенных пространствах** — защитите конфиденциальность отдельных пользователей, оценивая расположения и положения камеры, изменяя углы и регион, чтобы они не проводили никаких защищенных областей, таких как реструмс.

**Тщательно рассмотрите возможность использования в школах или елдерли** . пространственный анализ не тестировался с данными, содержащими небольшие значения в возрасте 18 или взрослых свыше возраста 65. Мы рекомендуем клиентам тщательно оценивать частоту ошибок для своего сценария в средах, в которых эти предоминате возрастают.

**Тщательно рассмотрите возможность использования в общедоступных пространствах** — Оцените расположение и положение камеры, настройте углы и регион для сворачивания коллекции из общедоступных пространств. Освещение и Погода в общедоступных пространствах, таких как улиц и парки, значительно повлияют на производительность системы пространственного анализа, и очень трудно обеспечить эффективную раскрытие в общедоступных пространствах.

## <a name="spatial-analysis-gating-for-public-preview"></a>Ограничение пространственного анализа для общедоступной предварительной версии

Чтобы обеспечить использование пространственного анализа в тех сценариях, для которых он был разработан, мы предоставляем клиентам доступ к этой технологии через процесс приложения. Чтобы получить доступ к пространственному анализу, необходимо начать с заполнения интерактивной формы принимать. [Начните работу с приложением](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyQZ7B8Cg2FEjpibPziwPcZUNlQ4SEVORFVLTjlBSzNLRlo0UzRRVVNPVy4u).

Доступ к общедоступной предварительной версии пространственного анализа регулируется исключительно по усмотрению корпорации Майкрософт в зависимости от критериев, заглянув процесса и доступности для поддержки ограниченного числа клиентов во время этой предварительной версии. В общедоступной предварительной версии мы ищем клиентов, имеющих значительную связь с корпорацией Майкрософт, заинтересованы в работе с нами в рекомендуемых вариантах использования, а также в дополнительных сценариях, которые связаны с нашими ответственными обязательствами искусственного интеллекта.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Характеристики и ограничения для пространственного анализа](https://docs.microsoft.com/legal/cognitive-services/computer-vision/accuracy-and-limitations?context=%2fazure%2fcognitive-services%2fComputer-vision%2fcontext%2fcontext)
