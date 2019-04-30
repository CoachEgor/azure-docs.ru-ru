---
title: Проверка задания Azure Stream Analytics с демонстрационными данными
description: В этой статье описывается тестирование задания Azure Stream Analytics, примера входных данных и отправка демонстрационных данных на портале Azure.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: fca76b632e9bcc27ed762886eaea696a5696ad3f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60761946"
---
# <a name="test-a-stream-analytics-query-with-sample-data"></a>Проверка запроса Stream Analytics с демонстрационными данными

С помощью Azure Stream Analytics можно отправлять демонстрационные данные и проверять выполнение запросов на портале Azure без запуска или остановки заданий.

## <a name="upload-sample-data-and-test-the-query"></a>Отправка примера данных и проверка запроса

1. Войдите на портал Azure. 

2. Найдите имеющееся задание Stream Analytics и выберите его.

3. На странице задания Stream Analytics в разделе **Топологии задания** установите флажок **Запрос**, чтобы открыть окно редактора запросов. 

4. Чтобы проверить запрос с использованием примера входных данных, щелкните любые входные данные правой кнопкой мыши.  Затем выберите **Отправить образец данных из файла**. Данные должны быть сериализованы в JSON, CSV или AVRO. Пример входных данных должен быть в кодировке UTF-8 и не сжат. При тестировании входных данных в формате CSV в качестве разделителя поддерживается только запятая (,).

    ![Проверка запроса редактора запросов Stream Analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

5. После отправки нажмите кнопку **Тест** для проверки этого запроса на основе указанных демонстрационных данных.

    ![Проверка демонстрационных данных редактора запросов Stream Analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

6. Если выходные данные проверки нужны для последующего использования, в браузере отображается ссылка для скачивания результатов по выходным данным запроса. 

7. Итеративно измените запрос и проверьте его еще раз, чтобы увидеть, каким образом изменяются выходные данные.

   ![Пример выходных данных редактора запросов Stream Analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

   При использовании в запросе нескольких наборов выходных данных результаты отображаются на отдельных вкладках, между которыми можно легко переключаться.

8. После проверки результатов, показанных в браузере, **сохраните** запрос. Затем **запустите** задание и позвольте ему обрабатывать входящие события.

## <a name="next-steps"></a>Дальнейшие действия
> [!div class="nextstepaction"]
> [Справочник по языку запросов Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
