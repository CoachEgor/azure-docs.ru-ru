---
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/28/2020
ms.author: normesta
ms.openlocfilehash: ac91ef5a56fe234cba47b430b78e74ce81c9d504
ms.sourcegitcommit: df66dff4e34a0b7780cba503bb141d6b72335a96
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2020
ms.locfileid: "96536993"
---
Служба хранилища Azure предоставляет следующие метрики транзакций в Azure Monitor.

| Метрика | Описание |
| ------------------- | ----------------- |
| Transactions | Количество запросов к службе хранения или указанной операции API. Это число включает успешные и неудачные запросы, а также запросы, вызвавшие ошибку. <br/><br/> Единица измерения: Count <br/> Тип агрегирования: Итог <br/> Применимые измерения. ResponseType, GeoType, ApiName и Authentication ([определение](#metrics-dimensions))<br/> Пример значения. 1024 |
| Входящий трафик | Объем входящих данных. Это количество включает входящий трафик из внешнего клиента в службу хранилища Azure, а также входящий трафик в пределах Azure. <br/><br/> Единица измерения: Байты <br/> Тип агрегирования: Итог <br/> Применимые измерения. GeoType, ApiName и Authentication ([определение](#metrics-dimensions)) <br/> Пример значения. 1024 |
| Исходящие | Объем исходящих данных. Это количество включает исходящий трафик из внешнего клиента в службу хранилища Azure, а также исходящий трафик в пределах Azure. В результате это количество не отражает исходящий трафик для тарификации. <br/><br/> Единица измерения: Байты <br/> Тип агрегирования: Итог <br/> Применимые измерения. GeoType, ApiName и Authentication ([определение](#metrics-dimensions)) <br/> Пример значения. 1024 |
| SuccessServerLatency | Среднее время, используемое для обработки успешного запроса службы хранилища Azure. Это значение не включает в себя сетевую задержку, указанную в метрике SuccessE2ELatency. <br/><br/> Единица измерения: Миллисекунды <br/> Тип агрегирования: Среднее <br/> Применимые измерения. GeoType, ApiName и Authentication ([определение](#metrics-dimensions)) <br/> Пример значения. 1024 |
| SuccessE2ELatency | Средняя совокупная задержка успешных запросов к службе хранения или указанной операции API. Это значение включает необходимое время обработки в пределах службы хранилища Azure для считывания запроса, отправки ответа и получения подтверждения ответа. Разница между значениями SuccessE2ELatency и Сукцесссерверлатенци заключается в задержке, которая может быть вызвана сетью и клиентом.<br/><br/> Единица измерения: Миллисекунды <br/> Тип агрегирования: Среднее <br/> Применимые измерения. GeoType, ApiName и Authentication ([определение](#metrics-dimensions)) <br/> Пример значения. 1024 |
| Доступность | Процент доступности службы хранения или указанной операции API. Доступность рассчитывается следующим образом: общее значение запросов для тарификации делится на количество применимых запросов, включая запросы, вызвавшие непредвиденные ошибки. Все непредвиденные ошибки приводят к снижению уровня доступности службы хранения или указанной операции API. <br/><br/> Единица измерения: Процент <br/> Тип агрегирования: Среднее <br/> Применимые измерения. GeoType, ApiName и Authentication ([определение](#metrics-dimensions)) <br/> Пример значения. 99,99 |
