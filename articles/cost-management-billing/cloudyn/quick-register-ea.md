---
title: Регистрация Соглашения Enterprise Azure в Cloudyn
description: В этом кратком руководстве описывается процесс регистрации, который необходимо пройти для создания пробной подписки Cloudyn и входа на портал Cloudyn.
author: bandersmsft
ms.author: banders
ms.date: 03/12/2020
ms.topic: quickstart
ms.custom: seodec18
ms.service: cost-management-billing
ms.reviewer: benshy
ROBOTS: NOINDEX
ms.openlocfilehash: 3c54328630d4e349af25c5f28a53a66117bf6a39
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "79474617"
---
# <a name="register-an-azure-enterprise-agreement-and-view-cost-data"></a>Регистрация соглашения Azure Enterprise и просмотр данных о затратах

Используйте соглашение Enterprise Azure для регистрации в службе Cloudyn. Такая регистрация предоставляет доступ к порталу Cloudyn. В этом кратком руководстве описывается процесс регистрации, который необходимо пройти для создания пробной подписки Cloudyn и входа на портал Cloudyn. Здесь также показано, как начать просматривать данные о затратах.

Служба "Управление затратами Azure" предоставляет те же возможности, что и Cloudyn. Служба "Управление затратами Azure" является собственным решением Azure по управлению затратами. Она позволяет анализировать затраты, создавать бюджеты и управлять ими, экспортировать данные, а также просматривать рекомендации и реагировать на них с целью экономии средств. Дополнительные сведения см. в статье [Управление затратами Azure](../cost-management-billing-overview.md).

[!INCLUDE [cloudyn-note](../../../includes/cloudyn-note.md)]

## <a name="sign-in-to-azure"></a>Вход в Azure

- Войдите на портал Azure по адресу [https://portal.azure.com](https://portal.azure.com).

## <a name="register-with-cloudyn"></a>Регистрация в Cloudyn

1. На портале Azure в списке служб выберите **Cost Management + Billing** (Управление затратами + Выставление счетов).
2. В разделе **Обзор** щелкните **Cloudyn**.  
    ![Страница Cloudyn, показанная на портале Azure](./media/quick-register-ea/cost-mgt-billing-service.png)
3. На странице **Cloudyn** нажмите кнопку **Go to Cloudyn** (Перейти к Cloudyn), чтобы открыть страницу регистрации Cloudyn в новом окне.
4. На странице пробной регистрации на портале Cloudyn введите название вашей компании, а затем выберите **Azure Enterprise Enrollment Administrator** (Администратор регистрации Azure Enterprise).  
5. Введите свой ключ API для регистрации на портале Enterprise Portal. Если у вас нет под рукой ключа, то щелкните ссылку [Enterprise Portal](https://ea.azure.com) и выполните описанные ниже действия.  
    ![Вставка ключа API на вкладке "Выставление счетов"](./media/quick-register-ea/trial-reg.png)
   1. Войдите на веб-сайт Azure Enterprise и щелкните **Отчеты**. Затем выберите **API Access Key** (Ключ доступа к API) и скопируйте первичный ключ.  
    ![Пример ключа API EA на портале EA](./media/quick-register-ea/ea-key.png)
   3. Вернитесь на страницу регистрации и вставьте эти ключ API.
6. Примите условия использования, а затем проверьте свой ключ. Нажмите кнопку **Next** (Далее), чтобы авторизовать Cloudyn для сбора данных ресурсов Azure. Собранные данные включают в себя сведения об использовании, производительности, выставлении счетов, а также данные тегов из подписок.  
    ![Пример успешной проверки ключа API EA](./media/quick-register-ea/ea-key-validated.png)
7. В разделе **Invite other stakeholders** (Пригласить других заинтересованных лиц) можно добавить пользователей, введя их адреса электронной почты. После завершения нажмите кнопку **Далее**. На добавление платежных данных в Cloudyn в зависимости от размера регистрации в Azure может потребоваться до 24 часов.
8. Щелкните **Go to Cloudyn** (Перейти к Cloudyn), чтобы открыть портал Cloudyn. Затем на странице **Cloud Accounts Management** (Управление облачными учетными записями) должны отобразиться сведения вашей зарегистрированной учетной записи EA.

Руководство по регистрации соглашения Enterprise см. в видео [How to Find Your EA Enrollment ID and API Key for use in Cloudyn](https://youtu.be/u_phLs_udig) (Как найти свой идентификатор регистрации EA и ключ API для использования в службе Cloudyn).

[!INCLUDE [cost-management-create-account-view-data](../../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Дальнейшие действия

В этом кратком руководстве вы использовали данные соглашения Azure Enterprise для регистрации в Cloudyn. Вы также вошли на портал Cloudyn и начали просматривать данные о затратах. Дополнительные сведения о Cloudyn см. в руководстве для Cloudyn.

> [!div class="nextstepaction"]
> [Просмотр сведений об использовании и затратах](tutorial-review-usage.md)
