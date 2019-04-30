---
title: Метрики для службы Azure NetApp Files | Документация Майкрософт
description: В этой статье описываются метрики для Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: b-juche
ms.openlocfilehash: 1563b60ef26ac5e4d40f45095d0109dd9dd71570
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61084945"
---
# <a name="metrics-for-azure-netapp-files"></a>Метрики для Azure NetApp Files

Служба Azure NetApp Files предоставляет метрики для выделенного хранилища, фактического использования емкости хранилища, пропускной способности тома, операций ввода-вывода в секунду и задержки. Проанализировав эти метрики, вы сможете узнать больше о производительности томов и шаблонах использования учетных записей NetApp.  

## <a name="capacity_pools"></a>Использование метрик для пулов емкости

- *Volume pool allocated size* (Выделенный размер пула для тома)  
    Размер (ГиБ) выделенного пула емкости.  
- *Volume pool allocated used* (Используемый размер выделенного пула для тома)  
    Общая квота тома (ГиБ) в полученном пуле емкости (общее количество предоставленных размеров томов в пуле емкости). Размер, выбранный в процессе создания тома.  
- *Volume pool total logical size* (Общий логический размер пула для тома)  
    Общее логическое пространство (ГиБ), применяемое к томам в пуле емкости.  
- *Volume pool total snapshot size* (Общий размер моментальных снимков пула для тома)  
    Общее добавочное логическое пространство используемое в моментальных снимках.  

## <a name="volumes"></a>Показатели использования для томов

- *Volume allocated size*  (Выделенный объем тома)  
    Размер тома (квота) предоставляемый в ГиБ.  
- *Volume logical size*  (Логический размер тома)  
    Общее логическое пространство, используемое в томе (ГиБ). Сюда можно отнести логическое пространство используемое активными файлами системы и моментальными снимками.  
- *Volume snapshot size*  (Размер моментальных снимков в томе)  
    Добавочное логическое пространство, используемое моментальными снимками в томе.  

## <a name="next-steps"></a>Дальнейшие действия

* [Общие сведения об иерархии хранилища Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Настройка пула емкости](azure-netapp-files-set-up-capacity-pool.md)
* [Создание тома для Azure NetApp Files](azure-netapp-files-create-volumes.md)
