---
title: Оценка большого количества физических серверов для миграции в Azure с помощью службы "миграция Azure" | Документация Майкрософт
description: В этой статье описывается, как оценить большое число физических серверов для миграции в Azure с помощью службы "миграция Azure".
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: how-to
ms.date: 01/19/2020
ms.openlocfilehash: 45954a42613fb1716e3b66305c0485a3966b8e37
ms.sourcegitcommit: ea551dad8d870ddcc0fee4423026f51bf4532e19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/07/2020
ms.locfileid: "96753694"
---
# <a name="assess-large-numbers-of-physical-servers-for-migration-to-azure"></a>Оценка большого количества физических серверов для миграции в Azure

В этой статье описывается, как оценить большое число локальных физических серверов для миграции в Azure с помощью средства Azure Migration Server для оценки.

Служба [Миграция Azure](migrate-services-overview.md) объединяет в себе инструменты, которые используются для поиска, оценки и переноса приложений, инфраструктуры и рабочих нагрузок в Microsoft Azure. Она включает в себя инструменты Миграции Azure и предложения независимых поставщиков программного обеспечения. 


Вы узнаете, как выполнять следующие задачи:
> [!div class="checklist"]
> * Планируйте оценку в масштабе.
> * Настройте разрешения Azure и подготовьте физические серверы для оценки.
> * Создайте проект службы "миграция Azure" и создайте оценку.
> * Изучите оценку при планировании миграции.


> [!NOTE]
> Если вы хотите испытать эксперимент для оценки нескольких серверов перед оценкой масштаба, следуйте нашим [сериям руководств](./tutorial-discover-physical.md).

## <a name="plan-for-assessment"></a>Планирование оценки

При планировании оценки большого количества физических серверов необходимо обратить внимание на несколько моментов:

- **Планирование проектов для миграции Azure**. Узнайте, как развертывать проекты службы "миграция Azure". Например, если центры обработки данных находятся в разных географических регионах или необходимо хранить метаданные обнаружения, оценки или миграции в разных регионах, может потребоваться несколько проектов.
- **Планирование устройств**. Служба "миграция Azure" использует локальное устройство миграции Azure, развернутое на компьютере Windows, чтобы непрерывно обнаруживать серверы для оценки и миграции. Устройство отслеживает изменения среды, такие как добавление виртуальных машин, дисков или сетевых адаптеров. Он также отправляет метаданные и данные о производительности в Azure. Необходимо определить, сколько устройств нужно развернуть.


## <a name="planning-limits"></a>Ограничения планирования
 
Используйте приведенные в этой таблице ограничения для планирования.

**Planning** | **Ограничения**
--- | --- 
**Проекты службы "Миграция Azure"** | Оценка до 35 000 серверов в проекте.
**Устройство службы "Миграция Azure"** | Устройство может обнаружить до 1000 серверов.<br/> Устройство может быть связано только с одним проектом службы "миграция Azure".<br/> С одним проектом службы "миграция Azure" можно связать любое количество устройств. <br/><br/> 
**Группа** | В одну группу можно добавить до 35 000 серверов.
**Оценка службы "миграция Azure"** | Вы можете оценить до 35 000 серверов за одну оценку.


## <a name="other-planning-considerations"></a>Другие вопросы планирования

- Чтобы запустить обнаружение с устройства, необходимо выбрать каждый физический сервер. 

## <a name="prepare-for-assessment"></a>Подготовка к оценке

Подготовка Azure и физических серверов для оценки серверов. 

1. Проверьте [требования к поддержке физических серверов и ограничения](migrate-support-matrix-physical.md).
2. Настройте разрешения для взаимодействия учетной записи Azure с миграцией Azure.
3. Подготовьте физические серверы.

Чтобы настроить эти параметры, следуйте инструкциям, приведенным в [этом руководстве](./tutorial-discover-physical.md) .

## <a name="create-a-project"></a>Создание проекта

В соответствии с требованиями к планированию выполните следующие действия.

1. Создайте проект службы "Миграция Azure".
2. Добавьте средство Azure Migrate Server для оценки в проекты.

[Подробнее](./create-manage-projects.md)

## <a name="create-and-review-an-assessment"></a>Создание и проверка оценки

1. Создание оценок для физических серверов.
1. Ознакомьтесь с оценками подготовки к планированию миграции.

Дополнительные [сведения](tutorial-assess-physical.md) о создании и просмотре оценок.
    

## <a name="next-steps"></a>Дальнейшие шаги

Работая с этой статьей, вы выполните следующие задачи:
 
> [!div class="checklist"] 
> * Запланировано масштабирование оценки миграции Azure для физических серверов.
> * Подготовка Azure и физических серверов для оценки.
> * Вы создали проект службы "миграция Azure" и выполнили оценку.
> * Проверенные оценки при подготовке к миграции.

Теперь [вы узнаете, как](concepts-assessment-calculation.md) рассчитываются оценки и как [изменять оценки](how-to-modify-assessment.md).