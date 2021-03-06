---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/04/2019
ms.author: alkohli
ms.openlocfilehash: 10d6ef2c90390f08e38726363416493f937de8f2
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2020
ms.locfileid: "96467603"
---
Ниже приведены размеры объектов Azure, которые могут быть записаны. Убедитесь, что все передаваемые файлы соответствуют этим ограничениям.

| Тип объекта Azure | Ограничение передачи                                             |
|-------------------|-----------------------------------------------------------|
| Блочный BLOB-объект        | 4,75 ТБ                                                 |
| Страничный BLOB-объект         | 1 TБ <br> Каждый файл, отправляемый в формате страничного BLOB-объекта, должен быть выровнен по размеру 512 байт (целое кратное). В противном случае произойдет сбой передачи. <br> VHD и VHDX выровнены по размеру 512 байт. |
| Файлы Azure         | 1 TБ <br> Каждый файл, отправляемый в формате страничного BLOB-объекта, должен быть выровнен по размеру 512 байт (целое кратное). В противном случае произойдет сбой передачи. <br> VHD и VHDX выровнены по размеру 512 байт. |

> [!IMPORTANT]
> Разрешается создавать файлы (вне зависимости от типа хранилища) размером до 5 ТБ. Тем не менее, если создан файл, размер которого превышает лимит на отправку, определенный в приведенной таблице, он не будет отправлен. Чтобы высвободить пространство, файл необходимо удалить вручную.
