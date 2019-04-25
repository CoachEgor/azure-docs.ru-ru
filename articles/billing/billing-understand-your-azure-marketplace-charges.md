---
title: Основная информация о расходах на внешние службы в Azure | Документация Майкрософт
description: Узнайте больше о выставлении счетов за использование внешних служб, ранее известных как Marketplace, в Azure.
services: ''
documentationcenter: ''
author: adpick
manager: adpick
editor: ''
tags: billing
ms.assetid: 5e0e2a3c-d111-4054-8508-0c111c1b749b
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ae9c2c975bf49725be1858ad02a1c4b90ef58a7f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60370586"
---
# <a name="understand-your-azure-billing-for-external-service-charges"></a>Основные сведения о выставлении счетов за использование внешних служб в Azure
Внешние службы публикуются сторонними поставщиками программного обеспечения в Azure Marketplace. Например, SendGrid — это внешние службы, которые можно приобрести в Azure, но которые не опубликованы корпорацией Майкрософт.

При подготовке новой внешней службы или ресурса отображается предупреждение.

![Предупреждение о покупке Marketplace](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

> [!NOTE]
> Внешние службы публикуются компаниями, не принадлежащими корпорации Майкрософт, но иногда продукты Майкрософт также относят к внешним службам.
> 
> 

## <a name="how-external-services-are-billed"></a>Как выставляются счета за внешние службы
- Счета за внешние службы выставляются отдельно. Они обрабатываются как отдельные заказы в подписке Azure. Расчетный период для каждой службы задается при ее покупке. Его не следует путать с расчетным периодом подписки, в которой вы приобрели службу. Вы будете получать отдельные счета, плата за которые будет отдельно взиматься с кредитной карты.
- У каждой внешней службы своя модель выставления счетов. Одни службы используют модель с оплатой по мере использования, другие — модель с ежемесячными платежами. Для оплаты внешних служб Azure нужна кредитная карта, их нельзя приобрести посредством оплаты счета.
- Для оплаты внешних служб невозможно использовать бесплатные кредитные средства. Если вы используете подписку Azure, которая включает в себя [бесплатные деньги на счете](https://azure.microsoft.com/pricing/spending-limits/), то ими невозможно оплачивать счета за внешние службы. Для покупки внешних служб используйте кредитную карту.

## <a name="view-external-service-spending-and-history-in-the-azure-portal"></a>Просмотр расходов на внешние службы и журнала на портале Azure
Можно просмотреть список внешних служб для каждой подписки на [портале Azure](https://portal.azure.com/). 

1. Войдите на [портал Azure](https://portal.azure.com/) в качестве администратора учетной записи.
2. В главном меню выберите **Подписки**.
   
    ![Выбор пункта "Подписки" в главном меню](./media/billing-understand-your-azure-marketplace-charges/sub-button.png) 
3. В колонке **Подписки** выберите подписку, которую нужно просмотреть, а затем щелкните **Внешние службы**.
   
    ![Выберите подписку в колонке "Выставление счетов".](./media/billing-understand-your-azure-marketplace-charges/select-sub-external-services.png)
4. Должны отобразиться все заказы внешних служб, включая имя издателя, приобретенный уровень служб, имя, присвоенное ресурсу, и текущее состояние заказа. Чтобы просмотреть предыдущие счета, выберите внешнюю службу.
   
    ![Выберите внешнюю службу.](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)
5. Здесь можно просмотреть прошлые суммы счетов, включая разделение налога.
   
    ![Просмотрите журнал выставленных счетов за внешние службы.](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="view-external-service-spending-for-enterprise-agreement-ea-customers"></a>Просмотр расходов на внешние службы для клиентов с соглашением Enterprise (EA)
Клиенты с соглашением Enterprise могут просматривать данные о расходах на внешние службы и скачивать отчеты на портале EA. Чтобы приступить к работе, см. статью [Azure Marketplace for EA Customers](https://ea.azure.com/helpdocs/azureMarketplace) (Azure Marketplace для клиентов с соглашением Enterprise).

## <a name="manage-payment-methods-for-external-service-orders"></a>Управление способами оплаты заказов внешних служб
Измените метод оплаты заказов внешних служб в [Центре управления учетной записью](https://account.windowsazure.com/).

> [!NOTE]
> Если вы приобрели подписку с рабочей или учебной учетной записью, то [обратитесь в службу поддержки](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), чтобы изменить метод оплаты.
> 
> 

1. Войдите в [Центр управления учетной записью](https://account.windowsazure.com/) и [перейдите на вкладку **Marketplace**](https://account.windowsazure.com/Store).
   
    ![Выберите вкладку "Marketplace" в Центре управления учетной записью.](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)
2. Выберите внешнюю службу для управления.
   
    ![Выберите внешнюю службу для управления.](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)
3. В правой части страницы щелкните **Изменить метод оплаты**. Эта ссылка позволяет открыть другой портал для управления методом оплаты.
   
    ![Сводка заказов](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)
4. Щелкните **Изменить информацию** и следуйте инструкциям, чтобы обновить платежную информацию.
   
    ![Выберите "Изменить информацию".](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)

## <a name="cancel-an-external-service-order"></a>Отмена заказа внешней службы
Если вы хотите отменить заказ внешней службы, то удалите соответствующий ресурс на [портале Azure](https://portal.azure.com).

![Удалите ресурс.](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-us"></a>Требуется помощь? Свяжитесь с нами.

Если у вас есть вопросы или нужна помощь, [создать запрос в службу поддержки](https://go.microsoft.com/fwlink/?linkid=2083458).

