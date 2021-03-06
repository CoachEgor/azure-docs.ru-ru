---
title: Создание отчетов о расширении файлов на основе данных с помощью зрения Insights
description: В этом пошаговом руководство описано, как просмотреть и использовать расширение файла зрения Insights для данных.
author: batamig
ms.author: bagol
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/24/2020
ms.openlocfilehash: 222106f5838a1eb237987afaa93924f1efe4a1f4
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2020
ms.locfileid: "96746290"
---
# <a name="file-extension-insights-about-your-data-from-azure-purview"></a>Расширение файла сведения о данных из Azure зрения 

В этом пошаговом руководство описывается, как получить доступ к файлам, а также просмотреть и отфильтровать аналитические сведения о расширениях файлов или типах файлов, найденных в данных.

Поддерживаются следующие источники данных: хранилище BLOB-объектов Azure, Azure Data Lake Storage (ADLS) GEN 1, Azure Data Lake Storage (ADLS) GEN 2

В этом пошаговом руководство вы узнаете, как:
> [!div class="checklist"]
> * Запустите учетную запись зрения из Azure. 
> - Просмотр сведений о расширении файла в данных
> - Детализация дополнительных сведений о расширении файла для данных

## <a name="prerequisites"></a>Предварительные требования 

Прежде чем приступить к работе с зрения Insights, убедитесь, что выполнены следующие действия.

- Настройка ресурсов Azure и заполнение соответствующих учетных записей тестовыми данными

- Настройка и завершение проверки тестовых данных в каждом источнике данных

Дополнительные сведения см. [в статье Управление источниками данных в Azure зрения (Предварительная версия)](manage-data-sources.md).

## <a name="use-purview-file-extension-insights"></a>Использование расширения файлов зрения

При сканировании ресурсов Azure зрения может определить типы файлов, находящиеся в области данных, и предоставить вам дополнительные сведения о каждом типе файлов. Сведения включают количество файлов каждого типа, где находятся эти файлы, и возможность их пропросмотра для конфиденциальных данных.

> [!NOTE]
> После того как вы проверили исходные типы, дайте **расширенной информации о расширении файла** несколько часов, чтобы отразить новые активы.

**Просмотр расширенной аналитики файлов:**

1. Перейдите на экран экземпляра **Azure зрения** [в портал Azure](https://aka.ms/purviewportal) и выберите свою учетную запись зрения.

1. На странице **Обзор** в разделе Начало **работы** выберите плитку **Запуск учетной записи зрения** .

1. В зрения выберите пункт меню **Insights** :::image type="icon" source="media/insights/ico-insights.png" border="false"::: слева, чтобы получить доступ к области **Insights** .
    
1. В поле " **аналитика**" выберите вкладку **расширения файлов** .

    Отчет содержит сводку по количеству уникальных расширений файлов, а также диаграмму основных 10 расширений в течение выбранного периода времени (по умолчанию — 30 дней).

    :::image type="content" source="media/file-extension-insights/file-extension-overview-small.png" alt-text="Отчет о расширении файла — обзор" lightbox="media/file-extension-insights/file-extension-overview.png":::

    Чтобы узнать больше, выполните одно из следующих действий.

    - Выберите селектор **времени** в верхней части отчета, чтобы изменить временной интервал, для которого были найдены расширения файлов.
    
    - Выберите **Просмотреть больше** под диаграммой, чтобы просмотреть полный список найденных расширений файлов. Дополнительные сведения см. в разделе [Углубленная детализация расширенной аналитики файлов](#file-extension-insights-drilldown). 

### <a name="file-extension-insights-drilldown"></a>Углубленная детализация расширенной аналитики файлов

После просмотра подробных сведений о типах файлов, найденных в области данных, выполните детализацию для получения дополнительных сведений о расположении, где они расположены, а также о том, можно ли проверить их на наличие конфиденциальных данных.

Пример:

:::image type="content" source="media/file-extension-insights/file-extension-drilldown-small.png" alt-text="Отчет о расширении файла — Углубленная детализация" lightbox="media/file-extension-insights/file-extension-drilldown.png":::

В сетке отображаются сведения о каждом обнаруженном расширении файла, включая:

- **Число файлов**. Число файлов с указанным расширением.
- **Сканирование содержимого**. Поддерживается ли расширение файла для проверки конфиденциальных данных.
- **Подписки**. Число подписок, в которых было найдено указанное расширение.
- **Источники**. Число обнаруженных источников с указанным расширением файла.



Используйте фильтры, расположенные над сеткой, чтобы отфильтровать отображаемые данные:

|Параметр  |Описание  |
|---------|---------|
|**Фильтровать по ключевому слову**     |    Введите текст в поле **Фильтровать по ключевому слову**  , чтобы просмотреть фильтрацию типов файлов по имени. Например, чтобы просмотреть только документы PDF, введите `PDF` .     |
|**Time**        | Выберите для фильтрации по определенному интервалу времени, в течение которого были созданы данные. <br>**По умолчанию:** 30 дней  |
|**Расширение файла**     |Выберите, чтобы отфильтровать сетку по одному или нескольким типам файлов.        |
|**Источники**    |Выберите, чтобы отфильтровать сетку по конкретным источникам данных. |
|**Сканирование содержимого**     |Выберите вариант **поддерживается** или **не поддерживается**, чтобы отображать только типы файлов, которые можно просканировать для конфиденциальных данных, или данные, которые не могут быть просмотрены, такие как **CERT** или **JPG** . |
| | |

Над фильтрами выберите **изменить столбцы** , :::image type="icon" source="media/insights/ico-columns.png" border="false"::: чтобы отобразить больше или меньше столбцов в сетке или изменить порядок. 

Чтобы отсортировать сетку, выберите заголовок столбца для сортировки по этому столбцу.
## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об отчетах Azure зрения Insights
> [!div class="nextstepaction"]
> [Глоссарий](glossary-insights.md)

> [!div class="nextstepaction"]
> [Просмотр аналитических сведений](scan-insights.md)

> [!div class="nextstepaction"]
> [Аналитика классификации](./classification-insights.md)

> [!div class="nextstepaction"]
> [Метка конфиденциальности — аналитика](sensitivity-insights.md)
