---
ms.openlocfilehash: c2b1e1d87600e83733eb8bdbf5bc6e2e63376cf4
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "95993019"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Поиск облачной пространственной привязки

Одна из основных причин использования Пространственных привязок Azure — возможность искать ранее сохраненную облачную пространственную привязку. Существует несколько разных способов поиска облачной пространственной привязки. За один раз можно использовать одну стратегию для наблюдателя.
- Нахождение привязок по идентификатору.
- Нахождение привязок, подключенных к ранее найденной привязке. Больше узнать о связях привязок можно [здесь](../articles/spatial-anchors/concepts/anchor-relationships-way-finding.md).
- Нахождение привязки, используя [Простое уточнение расположения](../articles/spatial-anchors/concepts/coarse-reloc.md).

> [!NOTE]
> Каждый раз при обнаружении привязки Пространственные привязки Azure попытаются использовать собранные данные среды, чтобы расширить визуальные сведения о привязке. Если у вас возникли проблемы с нахождением привязки, можно создать привязку, а затем определить ее несколько раз, используя разные углы и условия освещения.

При нахождении облачных пространственных привязок по идентификатору необходимо сохранить идентификатор облачной пространственной привязки в серверной службе приложения и сделать его доступным для всех устройств, которые могут правильно пройти в ней проверку подлинности. Пример см. в [Руководстве по совместному использованию службы "Пространственные привязки" на разных устройствах](../articles/spatial-anchors/tutorials/tutorial-share-anchors-across-devices.md).

Создайте экземпляр объекта `AnchorLocateCriteria`, задайте идентификаторы, которые вы ищете, и в сеансе вызовите метод `CreateWatcher`, предоставив `AnchorLocateCriteria`.