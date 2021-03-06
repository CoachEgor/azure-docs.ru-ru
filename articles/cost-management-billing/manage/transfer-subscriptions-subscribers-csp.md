---
title: Перенос подписок Azure между подписчиками и CSP
description: Сведения о переносе подписок Azure между подписчиками и CSP.
author: bandersmsft
ms.reviewer: dhgandhi
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 11/17/2020
ms.author: banders
ms.openlocfilehash: 4cc213db19a567f18c5ff483b64a75e3bbeef892
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2020
ms.locfileid: "94962116"
---
# <a name="transfer-azure-subscriptions-between-subscribers-and-csps"></a>Перенос подписок Azure между подписчиками и CSP

Эта статья содержит общие действия, которые используются для передачи подписок Azure между партнерами по программе "Поставщик облачных решений" (CSP) и их клиентами. Приведенные здесь сведения призваны упростить подписчикам Azure координацию с партнером. Сведения, используемые партнерами Майкрософт в ходе перехода, задокументированы с статье [Передача подписок Azure клиента другому партнеру](/partner-center/switch-azure-subscriptions-to-a-different-partner).

Перед созданием запроса на передачу скачайте или экспортируйте все сведения о расходах и выставлении счетов, которые вы хотите сохранить. Данные о выставлении счетов и использовании не передаются вместе с подпиской. Дополнительные сведения об экспорте данных об управлении расходами см. в статье [Руководство. Создание задачи для экспорта данных и управление экспортированными данными](../costs/tutorial-export-acm-data.md). Дополнительные сведения о скачивании данных о счетах и использовании см. в статье [Скачивание или просмотр счета на оплату и данных о ежедневном использовании в Azure](download-azure-invoice-daily-usage-date.md).

Если у вас есть резервирования, они перестанут действовать после передачи подписки. Обязательно [отмените все резервирования и верните средства за них](../reservations/exchange-and-refund-azure-reservations.md) до передачи подписки.

## <a name="transfer-ea-subscriptions-to-a-csp-partner"></a>Передача подписок с Соглашением Enterprise партнерам по программе CSP

Партнеры по прямым счетам CSP, сертифицированные как [Поставщик управляемых служб Azure Expert](https://partner.microsoft.com/membership/azure-expert-msp), могут запрашивать перенос подписок Azure для клиентов с прямым Соглашением Enterprise (EA). Перенос подписок разрешен только для клиентов, которые приняли Клиентское соглашение Майкрософт (MCA) и приобрели план Azure в рамках программы CSP.

После утверждения запроса CSP может предоставить своим клиентам объединенный счет. Дополнительные сведения о передаче подписок с помощью CSP см. в разделе [Get billing ownership of Azure subscriptions to your MPA account](mpa-request-ownership.md) (Получение права владения на выставление счетов в подписках Azure).

>[!IMPORTANT]
> После передачи подписки EA партнеру CSP все увеличения квоты, ранее сделанные в подписке EA, будут приведены к значениям по умолчанию. Если после передачи подписки требуется увеличить квоту, вашему поставщику CSP нужно отправить запрос на [увеличение квоты](../../azure-portal/supportability/regional-quota-requests.md). 

## <a name="other-subscription-transfers-to-a-csp-partner"></a>Передача другой подписки партнеру CSP

Чтобы перенести любые другие подписки Azure партнеру CSP, подписчику необходимо переместить ресурсы из исходных подписок в подписки CSP. Используйте следующие рекомендации для перемещения ресурсов между подписками.

1. Обратитесь к своему партнеру CSP, чтобы создать целевые подписки Azure CSP.
1. Убедитесь, что исходная и целевая подписки CSP находятся в одном арендаторе Azure Active Directory (Azure AD).  
    Арендатор Azure AD для подписки Azure CSP изменить нельзя. Вместо этого необходимо добавить исходную подписку или связать ее с клиентом Azure AD CSP. Дополнительные сведения см. в статье [Связывание или добавление подписки Azure в клиент Azure Active Directory](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).
    > [!IMPORTANT]
    > - При связывании подписки с другим каталогом Azure AD пользователи, которым назначены роли с помощью [управления доступом Azure на основе ролей (Azure RBAC)](../../role-based-access-control/role-assignments-portal.md) теряют доступ. Администраторы классической подписки, включая администратора служб и соадминистраторов, также теряют доступ.
    > - Назначения политик также удаляются из подписки, если подписка связана с другим каталогом.
1. Учетная запись пользователя, используемая для перемещения, должна иметь доступ владельца в [Azure RBAC](add-change-subscription-administrator.md) для обеих подписок.
1. Прежде чем начать, [проверьте](/rest/api/resources/resources/validatemoveresources), могут ли перемещаться ресурсы Azure из исходной подписки в целевую.  
    Некоторые ресурсы Azure не могут перемещаться между подписками. Полный список ресурсов Azure, которые можно переместить, см. в статье [Move operation support for resources](../../azure-resource-manager/management/move-support-resources.md) (Поддержка операции перемещения для ресурсов).
    > [!IMPORTANT]
    >  - Azure CSP поддерживает только ресурсы Azure Resource Manager. Если какие бы то ни было ресурсы Azure в исходной подписке созданы с помощью классической модели развертывания Azure, перед миграцией их необходимо перенести в [Azure Resource Manager](/azure/cloud-solution-provider/migration/ea-payg-to-azure-csp/ea-open-direct-asm-to-arm). Для просмотра веб-страницы необходимо быть партнером.

1. Убедитесь, что все службы исходной подписки используют модель Azure Resource Manager. Затем перенесите ресурсы из исходной подписки в целевую с помощью [Перемещения ресурсов Azure](../../azure-resource-manager/management/move-resource-group-and-subscription.md).
    > [!IMPORTANT]
    >  - Перемещение ресурсов Azure между подписками, в зависимости от ресурсов в подписках, может привести к простою службы.

## <a name="transfer-csp-subscription-to-other-offer"></a>Перенос подписки из предложения "Партнер CSP" в другое предложение

Чтобы перенести любые другие подписки из предложения "Партнер CSP" в любое другое предложение Azure, подписчику необходимо переместить ресурсы между исходными подписками CSP и целевыми подписками.

1. Создайте целевые подписки Azure.
1. Убедитесь, что исходная и целевая подписки находятся в одном арендаторе Azure Active Directory (Azure AD). Дополнительные сведения об изменении клиента Azure AD см. в разделе [Associate or add an Azure subscription to your Azure Active Directory tenant](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md) (Связывание или добавление подписки Azure в клиент Azure Active Directory).
    Обратите внимание, что для подписки CSP не поддерживается изменение имени каталога. Например, если вы переносите данные из CSP в подписку с оплатой по мере использования, вам потребуется изменить каталог подписки с оплатой по мере использования, чтобы она соответствовала каталогу.

    > [!IMPORTANT]
    >  - При связывании подписки с другим каталогом, пользователи, которым назначены роли с помощью [Azure RBAC](../../role-based-access-control/role-assignments-portal.md), теряют доступ. Администраторы классической подписки, включая администратора служб и соадминистраторов, также теряют доступ.
    >  - Назначения политик также удаляются из подписки, если подписка связана с другим каталогом.

1. Учетная запись пользователя, используемая для перемещения, должна иметь доступ владельца в [Azure RBAC](add-change-subscription-administrator.md) для обеих подписок.
1. Прежде чем начать, [проверьте](/rest/api/resources/resources/validatemoveresources), могут ли перемещаться ресурсы Azure из исходной подписки в целевую.
    > [!IMPORTANT]
    >  - Некоторые ресурсы Azure не могут перемещаться между подписками. Полный список ресурсов Azure, которые можно переместить, см. в статье [Move operation support for resources](../../azure-resource-manager/management/move-support-resources.md) (Поддержка операции перемещения для ресурсов).

1. Перенесите ресурсы из исходной подписки в целевую подписку с помощью [Перемещения ресурсов Azure](../../azure-resource-manager/management/move-resource-group-and-subscription.md).
    > [!IMPORTANT]
    >  - Перемещение ресурсов Azure между подписками, в зависимости от ресурсов в подписках, может привести к простою службы.

## <a name="next-steps"></a>Дальнейшие действия
- [Get billing ownership of Azure subscriptions for your MPA account](mpa-request-ownership.md) (Запрос прав владения подписками Azure для учетной записи MPA).
- Дополнительные сведения см. [Управление учетными записями и подписками с помощью API выставления счетов Azure](../index.yml).
