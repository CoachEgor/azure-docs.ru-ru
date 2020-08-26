---
title: Подключение учетной записи Google Cloud Platform к Cloudyn в Azure
description: Подключение учетной записи Google Cloud Platform для просмотра затрат и сведений об использовании данных в отчетах Cloudyn.
author: bandersmsft
ms.author: banders
ms.date: 03/12/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.subservice: cloudyn
ms.reviewer: benshy
ms.custom: seodec18
ROBOTS: NOINDEX
ms.openlocfilehash: 8d549a8fd7c8b03f98fe2b11c94531ed323e0e3a
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/20/2020
ms.locfileid: "88690074"
---
# <a name="connect-a-google-cloud-platform-account"></a>Подключение учетной записи Google Cloud Platform

Имеющуюся учетную запись Google Cloud Platform можно подключить к Cloudyn. После этого сведения о затратах и использовании станут доступны для просмотра в отчетах Cloudyn. Информация в этой статье поможет вам настроить учетную запись Google и подключить ее к Cloudyn.

[!INCLUDE [cloudyn-note](../../../includes/cloudyn-note.md)]

## <a name="collect-project-information"></a>Сбор сведений о проекте

Начните сбор сведений о проекте.

1. Войдите в консоль Google Cloud Platform по адресу[https://console.cloud.google.com](https://console.cloud.google.com).
2. Просмотрите сведения о проекте, который вы хотите реализовать в Cloudyn, и найдите **имя проекта** и **идентификатор проекта**. Сохраните данные для использования на следующих шагах.  
    ![Имя и идентификатор проекта в консоли Google Cloud Platform](./media/connect-google-account/gcp-console01.png)
3. Если выставление счетов не включено и не связано с проектом, создайте учетную запись выставления счетов. Дополнительные сведения см. в разделе [Создание учетной записи выставления счетов](https://cloud.google.com/billing/docs/how-to/manage-billing-account#create/_a/_new/_billing/_account).

## <a name="enable-storage-bucket-billing-export"></a>Включение экспорта данных выставления счетов контейнеров хранилища

Cloudyn извлекает данные выставления счетов Google из контейнеров хранилища. Сохраните сведения об **имени контейнера** и **префиксе отчета** для последующего использования во время регистрации Cloudyn.

При использовании Google Cloud Storage для хранения отчетов об использовании взимается минимальная плата. Дополнительные сведения см. на странице [цен на службу облачного хранилища](https://cloud.google.com/storage/pricing).

1. Если экспорт выставленных счетов в файл не включен, следуйте инструкциям в разделе [Как включить экспорт выставленных счетов в файл](https://cloud.google.com/billing/docs/how-to/export-data-file#how_to_enable_billing_export_to_a_file). Для этого можно использовать формат экспорта выставленных счетов CSV или JSON.
2. В противном случае в консоли Google Cloud Platform перейдите к разделу **Выставление счета** > **Billing export** (Экспорт выставленного счета). Запишите **имя контейнера** и **префикс отчета** выставления счетов.  
    ![Сведения об экспорте выставленных счетов на соответствующей странице](./media/connect-google-account/billing-export.png)

## <a name="enable-google-cloud-platform-apis"></a>Включение API-интерфейсов Google Cloud Platform

Для сбора информации о потреблении и ресурсах Cloudyn требуется включить следующие API Google Cloud Platform:

- API BigQuery;
- Google Cloud SQL;
- API Google Cloud Datastore;
- Google Cloud Storage
- API Google Cloud Storage JSON;
- API Google Compute Engine.

### <a name="enable-or-verify-apis"></a>Включение и проверка API

1. В консоли Google Cloud Platform выберите проект, который вы хотите зарегистрировать в Cloudyn.
2. Перейдите к разделу **APIs & Services** (API и службы)  > **Библиотека**.
3. Используйте поиск, чтобы найти все перечисленные ранее API.
4. Убедитесь, что для каждого API отображается состояние **API enabled** (API включен). В противном случае выберите **Включить**.

## <a name="add-a-google-cloud-account-to-cloudyn"></a>Добавление учетной записи Google Cloud в Cloudyn

1. Откройте портал Cloudyn с портала Azure или перейдите по адресу [https://azure.cloudyn.com](https://azure.cloudyn.com/) и войдите.
2. Щелкните **Параметры** (значок шестеренки), а затем выберите **Cloud Accounts** (Облачные учетные записи).
3. На странице **управления учетными записями** выберите вкладку **Google Accounts** (Учетные записи Google), а затем щелкните **Add new +** (Добавить новый элемент +).
4. В поле **Google Account Name** (Имя учетной записи Google) введите адрес электронной почты для учетной записи выставления счетов, а затем щелкните **Далее**.
5. В диалоговом окне аутентификации Google выберите или введите учетную запись Google, а затем **предоставьте** cloudyn.com доступ к учетной записи.
6. Добавьте записанную ранее информацию о проекте запроса. Введите **идентификатор проекта**, имя **проекта**, имя контейнера **выставления счетов** и префикс отчета **файла выставления счетов**, а затем щелкните **Сохранить**.  
    ![Добавление проекта Google в учетную запись Cloudyn](./media/connect-google-account/add-project.png)

Учетная запись Google отобразится в списке учетных записей с указанием **Проверка подлинности выполнена**. Под ним должно отображаться имя и идентификатор проекта Google с символом зеленой галочки. Состояние учетной записи должно быть **Завершено**.

В течение нескольких часов в отчетах Cloudyn появятся сведения об издержках и использовании Google.

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о Cloudyn см. в руководстве для Cloudyn [Просмотр сведений об использовании и затратах](tutorial-review-usage.md).
