---
title: Запросы на увеличение квоты виртуальных ЦП Azure Resource Manager | Документация Майкрософт
description: Запросы на увеличение квоты виртуальных ЦП Azure Resource Manager
author: ganganarayanan
ms.author: gangan
ms.date: 6/13/2018
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 204deaf3a67984c0dd5eca5352686719e7767885
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60649355"
---
# <a name="resource-manager-vcpu-quota-increase-requests"></a>Запросы на увеличение квоты виртуальных ЦП Resource Manager

Квоты виртуальных ЦП Resource Manager применяются на уровне региона и семейства SKU.
Дополнительные сведения о применении квот доступны на странице [Подписка Azure, границы, квоты и ограничения службы](https://aka.ms/quotalimits).
Чтобы узнать больше о семействах SKU, вы можете сравнить затраты и производительность на странице [Цены на виртуальные машины Windows](https://aka.ms/pricingcompute).

Чтобы запросить увеличение квоты, следуйте инструкциям ниже, используя для создания запроса на обслуживание колонку Azure "Использование + квота", доступную на портале Azure. 

## <a name="request-quota-increase-at-subscription-level"></a>Запрос увеличения квоты на уровне подписки

1. В https://portal.azure.com выберите **Подписки**.

   ![Подписки](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Выберите подписку, которая требует увеличенную квоту.

   ![Выберите подписку.](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Выберите **Использование и квоты**.

   ![Использование и квоты](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. В правом верхнем углу выберите **Запросить увеличение**.

   ![Запросить увеличение](./media/resource-manager-core-quotas-request/request-increase.png)

5. Шаг 1. В качестве типа квоты выберите **Ядра**. 

   ![Заполнение формы](./media/resource-manager-core-quotas-request/forms.png)
   
6. Шаг 2. Выберите модель развертывания "Resource Manager" и укажите расположение.

    ![Колонка "Проблема" для квоты](./media/resource-manager-core-quotas-request/Problem-step.png)

3. Выберите семейства SKU, которые требуют увеличения квоты.

    ![Выбранные семейства SKU](./media/resource-manager-core-quotas-request/SKU-selected.png)

4. Введите желаемые ограничения для подписки.

    ![Новый запрос квоты для SKU](./media/resource-manager-core-quotas-request/SKU-new-quota.png)

- Чтобы удалить строку, снимите флажок SKU в раскрывающемся списке семейства SKU или щелкните значок отмены "x".
После ввода требуемой квоты для каждого семейства SKU нажмите кнопку "Далее" на странице "Проблема", чтобы продолжить создание запроса на поддержку.

