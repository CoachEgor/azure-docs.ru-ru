---
title: Создание приложения-функции на портале Azure
description: Создание нового приложения-функции в Azure на портале.
ms.topic: how-to
ms.date: 08/29/2019
ms.custom: mvc
ms.openlocfilehash: 8d19a269903de309bf219c2546fa70c3abe7be10
ms.sourcegitcommit: 5db975ced62cd095be587d99da01949222fc69a3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2020
ms.locfileid: "97093595"
---
# <a name="create-a-function-app-from-the-azure-portal"></a>Создание приложения-функции на портале Azure

В этом разделе показано, как использовать функции Azure для создания приложения-функции в портал Azure. Приложение-функция — это контейнер, в котором выполняются отдельные функции. 

## <a name="create-a-function-app"></a>Создание приложения-функции

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

После создания приложения-функции можно создать отдельные функции на одном или нескольких языках программирования. Создайте функции [с помощью портала](functions-create-first-azure-function.md#create-function), [непрерывного развертывания](functions-continuous-deployment.md) или [передачи по FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).

## <a name="service-plans"></a>Планы обслуживания

Функции Azure имеют три разных плана обслуживания: план потребления, план Premium и выделенный план (служба приложений). При создании приложения-функции необходимо выбрать свой план обслуживания, который впоследствии невозможно изменить. Дополнительные сведения см. в разделе [Выбор правильного плана обслуживания для Функций Azure](functions-scale.md).

Если вы планируете выполнять функции JavaScript для выделенного плана (службы приложений), следует выбрать план с меньшим числом ядер. Дополнительные сведения см. в разделе [обзора функций для JavaScript](functions-reference-node.md#choose-single-vcpu-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Требования к учетной записи хранения

При создании приложения-функции необходимо создать или привязать учетную запись хранения Azure общего назначения, поддерживающую хранилища BLOB-объектов, очередей и таблиц. Для внутренних операций, таких как управление триггерами и ведение журнала выполнения функций, Функции Azure используют службу хранилища. Некоторые учетные записи хранения не поддерживают очереди и таблицы. Например, это относится к учетным записям хранения только для больших двоичных объектов, службе хранилища Azure уровня "Премиум" и учетным записям хранения общего назначения с репликацией ZRS. 

Учетные записи неподдерживаемого типа отфильтровываются при создании приложения-функции в портал Azure. Портал также позволяет использовать существующую учетную запись хранения, только если эта учетная запись находится в том же регионе, что и создаваемое приложение-функция. Если по какой бы то ни было причине нужно нарушать производительность учетной записи хранения, используемой приложением функции в том же регионе, необходимо создать приложение функции за пределами портала. 

>[!NOTE]
>При использовании плана потребления файлы кода и конфигурации привязок приложения-функции хранятся в хранилище файлов Azure в основной учетной записи хранения. При удалении основной учетной записи хранения это содержимое удаляется без возможности восстановления. 

Дополнительные сведения о типах учетных записей хранения см. в разделе [Введение в службы хранилища Azure](../storage/common/storage-introduction.md#core-storage-services). 

## <a name="next-steps"></a>Дальнейшие действия

Хотя портал Azure позволяет легко создать и опробовать функции, мы рекомендуем [локальную разработку](functions-develop-local.md). После создания приложения-функции на портале необходимо еще добавить функцию. 

> [!div class="nextstepaction"]
> [Добавление функции, активируемой HTTP](functions-create-first-azure-function.md#create-function)
