---
title: Значение терминов в данные об использовании Azure и затратах с CSV-ФАЙЛ для клиента соглашение Microsoft | Документация Майкрософт
description: Узнайте, как читать и интерпретировать разделы Azure об использовании и затратах с CSV-ФАЙЛ для профиля выставления счетов
services: ''
documentationcenter: ''
author: bandersmsft
manager: alherz
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: banders
ms.openlocfilehash: 8f71f42386ce49d4d7178cb03d28d74edacd7e39
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60371314"
---
# <a name="understand-terms-on-your-azure-usage-and-charges-csv-for-a-microsoft-customer-agreement"></a>Значение терминов в данные об использовании Azure и затратах с CSV-ФАЙЛ для клиента соглашение Microsoft

Эта статья относится к учетной записи выставления счетов для клиента соглашение Microsoft. [Проверьте наличие доступа к соглашению клиента Microsoft](#check-access-to-a-microsoft-customer-agreement).

Azure об использовании и затратах CSV-файл содержит ежедневно и индикатор уровня плата за использование за текущий расчетный период.

Чтобы получить файл Azure об использовании и затратах, см. в разделе [Просмотр и загрузка данных об использовании Azure и плата за соглашение с клиентом Microsoft](billing-download-azure-daily-usage.md).
Эти сведения доступны в формате файла данных с разделителями-запятыми (CSV), который можно открыть в приложении для электронных таблиц.

Плата за использование — это общая сумма за использование подписки **за месяц**. Она не учитывает какие-либо поступления на счет или скидки.

## <a name="changes-in-the-enterprise-agreement-azure-usage-and-charges-csv"></a>Изменения использования соглашения Enterprise Azure и сумм к оплате CSV

Если вы являетесь клиентом EA, можно заметить, условия профиль выставления счетов за использование Azure CSV-файла отличаются от условия в CSV-файле использование EA Azure. Ниже приведен сопоставление EA условия использования для выставления счетов условия использования профиля.

| Использование EA Azure CSV | Microsoft Customer соглашение Azure об использовании и затратах CSV |
| --- | --- |
| Дата | date |
| Месяц| date |
| День | date |
| Год | date |
| Продукт | product |
| Значение MeterId | meterID |
| Категория единицы измерения | meterCategory |
| Подкатегория единицы измерения | meterSubCategory |
| Регион единицы измерения | meterRegion |
| Имя единицы измерения | meterName |
| ConsumedQuantity | quantity |
| Частота ресурса | effectivePrice | <!-- this was highlighted -->
| Расширенные затраты | cost |
| Расположение ресурса | resourceLocation |
| Использованная служба | consumedService |
| InstanceId | instanceId |
| Служебная информация 1 | serviceInfo1 |
| Служебная информация 2 | serviceInfo2 |
| Дополнительные сведения | additionalInfo |
| Теги | tags |
| ИД службы хранилища | Н/Д |
| DepartmentName | invoiceSection | <!-- this was highlighted -->
| CostCenter | costCenter |
| Единица измерения | unitofMeasure |
| ResourceGroup | resourceGroup |
| ChargesBilledSeparately | isAzureCreditEligible | <!-- this was highlighted -->

<!-- TO DO: Marketplace CSV? -->

## <a name="detailed-terms-and-descriptions-in-your-azure-usage-and-charges-file"></a>Подробные условия и описания в файле Azure об использовании и затратах

В следующем разделе описаны важные термины, встречающиеся в файле Azure об использовании и затратах.

Термин | ОПИСАНИЕ
--- | ---
invoiceId | Документа уникальный идентификатор, указанный в счете PDF
previousInvoiceId | Ссылка на исходный счет, если этот элемент имеет возмещение
billingAccountName | Имя учетной записи выставления счетов
billingAccountId | Уникальный идентификатор для корня платежный счет
billingProfileId | Имя профиля выставления счетов, накапливаемый накладные расходы, выставляются
billingProfileName | Уникальный идентификатор для профиля выставления счетов, накапливаемый накладные расходы, выставляются
invoiceSectionId | Уникальный идентификатор для раздела счета
invoiceSectionName | Имя раздела счета
costCenter | Центр затрат, определенные в подписке для отслеживания затрат (доступно только в открытых периодов выставления счетов)
billingPeriodStartDate | Дата начала расчетного периода, для которого создается счета
billingPeriodEndDate | Дата окончания расчетного периода, для которого создается счета
servicePeriodStartDate | Дата начала периода, оценка которого определила и заблокирован цен на службу потребляемого или приобретенных
servicePeriodEndDate | Дата окончания периода, оценка которого определила и заблокирован цен на службу потребляемого или приобретенных
date | Для Azure и Marketplace основано на использовании расходов это дата оценки. Для разового покупки (резервирования, Marketplace) или основных регулярные платежи (поддержка предложений) это с даты покупки.
serviceFamily | Семейство службы, к которой принадлежит служба
productOrderId | Уникальный идентификатор для заказа продукта
productOrderName | Уникальное имя для заказа продукта
consumedService | Имя используемой службы
meterId | Уникальный идентификатор для единицы измерения
meterName | Имя счетчика
meterCategory | Имя категории, классификации для единицы измерения. Например *облачные службы*, *сети*и т. д.
meterSubCategory | Имя категории счетчика дочернюю классификацию
meterRegion | Имя региона, где доступны метрики для службы. Расположение центра обработки данных для определенных служб, которые оцениваются в зависимости от расположения центра обработки данных.
offer | Название предложения, которые приобрели
productId | Уникальный идентификатор для продукта, выставление счетов, связанные
product | Имя продукта, выставление счетов, связанные
Идентификатор подписки | Уникальный идентификатор для подписки, выставление счетов, связанные
subscriptionName | Имя подписки, выставление счетов, связанные
ReservationId | Уникальный идентификатор для экземпляра приобретенное резервирование
reservationName | Имя экземпляра приобретенное резервирование
publisherType | Тип добавляемого издателя (значения: firstParty, thirdPartyReseller, thirdPartyAgency)
publisherName | Издатель для служб Marketplace
resourceGroupId | Уникальный идентификатор для группы ресурсов, связанной с ресурсом
имя_группы_ресурсов | Имя группы ресурсов, связанные с ресурсом
resourceId | Уникальный идентификатор для экземпляра ресурса
тип_ресурса | Тип экземпляра ресурса
resourceLocation | Расположение центра обработки данных, где выполняется ресурс.
location | Нормализованный путь к ресурсу, если расположения различных ресурсов настроены для тех же регионах
quantity | Число единиц, приобретенных или обрабатываются
unitOfMeasure | Единица измерения для выставления счетов для службы. Например службы вычислений действует почасовая оплата.
chargeType | Тип оплаты. Значения: <ul><li>AsCharged использование: Платежи, вычисленные на основе использования службы Azure. Это включает использование на виртуальных машинах, которые не взимается, из-за зарезервированные экземпляры.</li><li>AsCharged-PurchaseMarketplace: Фиксированный или одноразовый регулярные платежи от покупки в Marketplace</li><li>AsCharged-UsageMarketplace: Плата за службы Marketplace, оплачиваемых в соответствии с единиц потребления</li></ul>
isAzureCreditEligible | Флаг, указывающий, подходит ли плата в службе к оплате за использование кредиты Azure (значения: True, False)
serviceInfo1 | Метаданные о службе
serviceInfo2 | Устаревшее поле, в которое записываются необязательные метаданные службы
additionalInfo | Дополнительные метаданные для конкретной службы.
tags | Теги, присваиваемый ресурсу

### <a name="how-do-i-make-sure-that-the-charges-in-my-azure-usage-and-charges-file-are-correct"></a>Как сделать правильность расходов в Azure файл об использовании и затратах?

Если в вашем файле подробные сведения об использовании, которое вы хотите узнать подробнее о взимается плата, см. в разделе [основная информация о расходах на профиль выставления счетов](billing-mca-understand-your-bill.md)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Проверьте доступ к соглашению клиента Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Требуется помощь? Свяжитесь с нами.

Если у вас есть вопросы или нужна помощь, [создать запрос в службу поддержки](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Дальнейшие действия

- [Просмотр и загрузка счете Microsoft Azure](billing-download-azure-invoice.md)
- [Просматривать и скачивать данные об использовании Microsoft Azure и затратах](billing-download-azure-daily-usage.md)
