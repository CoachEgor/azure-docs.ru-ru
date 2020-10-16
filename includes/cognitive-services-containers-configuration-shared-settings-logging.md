---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 873fd8cbc211f098c93b8fb3fbe701e4a34d8487
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "68320526"
---
Параметры `Logging` управляют поддержкой ведения журнала ASP.NET Core для контейнера. Вы можете использовать для контейнера те же параметры конфигурации и значения, что и для приложения ASP.NET Core. 

Контейнер поддерживает указанных ниже поставщиков ведения журналов.

|Поставщик|Назначение|
|--|--|
|[Консоль](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)|Поставщик ведения журнала `Console` для ASP.NET Core. Для этого поставщика ведения журнала поддерживаются все параметры конфигурации ASP.NET Core и значения по умолчанию.|
|[Отладка](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)|Поставщик ведения журнала `Debug` для ASP.NET Core. Для этого поставщика ведения журнала поддерживаются все параметры конфигурации ASP.NET Core и значения по умолчанию.|
|[Диск](#disk-logging)|Поставщик ведения журнала JSON. Поставщик ведения журнала записывает данные журнала в выходное подключение.|

В этой команде для контейнера хранятся сведения о ведении журнала в формате JSON для выходного подключения:

```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<endpoint> \
ApiKey=<api-key> \
Logging:Disk:Format=json
```

Во время выполнения контейнера в этой команде для контейнера отображается отладочная информация с префиксом `dbug`:

```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
<registry-location>/<image-name> \
Eula=accept \
Billing=<endpoint> \
ApiKey=<api-key> \
Logging:Console:LogLevel:Default=Debug
```

### <a name="disk-logging"></a>Ведение журнала диска

Поставщик ведения журнала `Disk` поддерживает перечисленные ниже параметры конфигурации:

| name | Тип данных | Описание |
|------|-----------|-------------|
| `Format` | Строка | Выходной формат файлов журналов.<br/> **Примечание.** Чтобы включить поставщика ведения журнала, для этого параметра необходимо задать значение `json`. Если это значение задано без указания выходного подключения, при создании экземпляра контейнера возникает ошибка. |
| `MaxFileSize` | Целое число | Максимальный размер файла журнала в мегабайтах (МБ). Когда размер текущего файла журнала достигает этого значения или превышает его, поставщик ведения журнала создает файл журнала. Если задано значение –1, то размер файла журнала ограничивается только максимальным размером файла (если он задан) для выходного подключения. Значение по умолчанию — 1. |

Дополнительные сведения о настройке поддержки ведения журналов для ASP.NET Core см. в разделе [Настройка файла параметров](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1).

