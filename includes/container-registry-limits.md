---
title: включение файла
description: включение файла
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 04/29/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: d20e266d1331fc15e65b2d119468483ff53a4c06
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2019
ms.locfileid: "64951563"
---
| Resource | базовая; | Стандартная | Премиум |
|---|---|---|---|
| Хранилище<sup>1</sup> | 10 ГБ | 100 ГБ| 500 ГБ |
| Максимальный уровень размер изображения | 200 ГБ | 200 ГБ | 200 ГБ |
| Операции чтения за минуту<sup>2, 3</sup> | 1000 | 3000 | 10 000 |
| Операции записи за минуту<sup>2, 4</sup> | 100 | 500 | 2 000 |
| Пропускная способность скачивания в Мбит/с<sup>2</sup> | 30 | 60 | 100 |
| Пропускная способность передачи в Мбит/с<sup>2</sup> | 10 | 20 | 50 |
| Объекты Webhook | 2 | 10 | 100 |
| Георепликация | Н/Д | Н/Д | [Поддерживается][geo-replication] |
| Доверие к содержимому (предварительная версия) | Н/Д | Н/Д | [Поддерживается][content-trust] |

<sup>1</sup>указанное хранилище обусловлены объем *включены* хранилища для каждого уровня. Дополнительная ежедневная плата взымается за ГиБ для хранилища изображений выше этих ограничений. Сведения о курсах, см. в разделе [цен на реестр контейнеров Azure][pricing].

<sup>2</sup>*операции чтения*, *операции записи*, и *пропускной способности* представляют собой минимальные расчеты. Реестр контейнеров Azure стремится повысить производительность по мере использования.

<sup>3</sup>объект [Вытягивание из docker](https://docs.docker.com/registry/spec/api/#pulling-an-image) преобразуется в несколько операций чтения на основе числа слоев в образе, а также получении манифеста.

<sup>4</sup>объект [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) преобразуется в несколько операций записи, в зависимости от числа слоев, которые должны быть переданы. Команда `docker push` включает *операции чтения* для получения манифеста для имеющегося образа.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md
