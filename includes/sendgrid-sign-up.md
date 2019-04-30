---
author: erikre
ms.service: multiple
ms.topic: include
ms.date: 11/25/2018
ms.author: erikre
ms.openlocfilehash: 96c4da8465a87fee4c00bfc6177515c94910704a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60444870"
---
Клиенты Azure могут разблокировать 25 000 бесплатных сообщений электронной почты каждый месяц. Эти 25 000 сообщений предоставят вам доступ к расширенным отчетам и функциям аналитики, а также ко [всем интерфейсам API][all APIs] (Web, SMTP, Event, Parse и многим другим). Сведения о дополнительных службах, предоставляемых SendGrid, см. на странице [решений SendGrid][SendGrid Solutions].

### <a name="to-sign-up-for-a-sendgrid-account"></a>Регистрация для получения учетной записи SendGrid
1. Войдите на [портал Azure][Azure portal].
2. В меню слева вверху щелкните **Создать ресурс**.

    ![command-bar-new][command-bar-new]
3. Щелкните **Надстройки** > **SendGrid Email Delivery** (Доставка электронной почты SendGrid).

    ![sendgrid-store][sendgrid-store]
4. Заполните форму регистрации и щелкните **Создать**.

    ![sendgrid-create][sendgrid-create]
5. Введите **имя**, идентифицирующее службу SendGrid, в параметрах настройки Azure. Имена должны иметь длину в диапазоне от 1 до 100 знаков и содержать только алфавитно-цифровые символы, дефисы, точки и подчеркивания. Имя должно быть уникальным в списке элементов Магазина Azure, на которые вы подписались.
6. Введите и подтвердите **пароль**.
7. Выберите свою **подписку**.
8. Создайте новую **группу ресурсов** или выберите существующую.
9. В разделе **Ценовая категория** выберите план SendGrid для регистрации.

    ![sendgrid-pricing][sendgrid-pricing]
10. Введите **промокод** (при наличии).
11. Введите **контактные данные**.
12. Прочтите и примите **юридические условия**.
13. После подтверждения покупки вы увидите всплывающее окно **Развертывание прошло успешно**, а ваша учетная запись появится в разделе **Все ресурсы**.

    ![all-resources][all-resources]

    После того, как вы завершите покупку и нажмете кнопку **Управление**, чтобы запустить процесс проверки по электронной почте, вы получите электронное сообщение с предложением подтвердить учетную запись SendGrid. Если вы не получили это сообщение или столкнулись с проблемами при подтверждении учетной записи, ознакомьтесь с разделом часто задаваемых вопросов.

    ![управление][manage]

    **Пока вы не подтвердили свою учетную запись, вы можете отправлять до 100 электронных сообщений в день.**

    Для изменения плана подписки или просмотра параметров контактов SendGrid выберите имя службы SendGrid, чтобы открыть панель мониторинга SendGrid Marketplace.

    ![параметры][settings]

    Чтобы отправить электронное сообщение с помощью SendGrid, необходимо указать ключ API.

### <a name="to-find-your-sendgrid-api-key"></a>Как найти ключ API SendGrid
1. Нажмите кнопку **Управление**.

    ![управление][manage]
2. На панели мониторинга SendGrid в меню слева выберите **Параметры** > **Ключи API**.

    ![api-keys][api-keys]

3. Щелкните **Создать ключ API**.

    ![general-api-key][general-api-key]
4. Как минимум укажите **имя ключа** и предоставьте полный доступ для **отправки почты**. Затем щелкните **Сохранить**.

    ![access][access]
5. На этом этапе API будет отображен один раз. Обязательно сохраните его в надежном месте.

### <a name="to-find-your-sendgrid-credentials"></a>Поиск учетных данных SendGrid
1. Щелкните значок ключа, чтобы найти свое **имя пользователя**.

    ![key][key]
2. Используется пароль, который вы выбрали при установке. Можно выбрать **Смена пароля** или **Сброс пароля**, чтобы внести изменения.

Чтобы настроить параметры доставки электронной почты, нажмите кнопку **Управление**. Откроется панель мониторинга SendGrid.

![управление][manage]

Дополнительные сведения об отправке по электронной почте посредством SendGrid см. в [обзоре API электронной почты][Email API Overview].

<!--images-->

[command-bar-new]: ./media/sendgrid-sign-up/new-addon.png
[sendgrid-store]: ./media/sendgrid-sign-up/sendgrid-store.png
[sendgrid-create]: ./media/sendgrid-sign-up/sendgrid-create.png
[sendgrid-pricing]: ./media/sendgrid-sign-up/sendgrid-pricing.png
[all-resources]: ./media/sendgrid-sign-up/all-resources.png
[manage]: ./media/sendgrid-sign-up/manage.png
[settings]: ./media/sendgrid-sign-up/settings.png
[api-keys]: ./media/sendgrid-sign-up/api-keys.png
[general-api-key]: ./media/sendgrid-sign-up/general-api-key.png
[access]: ./media/sendgrid-sign-up/access.png
[key]: ./media/sendgrid-sign-up/key.png

<!--Links-->

[SendGrid Solutions]: https://sendgrid.com/solutions
[Azure portal]: https://portal.azure.com
[SendGrid Getting Started]: http://sendgrid.com/docs
[SendGrid Provisioning Process]: https://support.sendgrid.com/hc/articles/200181628-Why-is-my-account-being-provisioned-
[all APIs]: https://sendgrid.com/docs/API_Reference/index.html
[Email API Overview]: https://sendgrid.com/docs/API_Reference/Web_API_v3/Mail/index.html
