---
title: Активация подписок и учетных записей Azure | Документация Майкрософт
description: Сведения о предоставлении доступа к новым и имеющимся учетным записям с использованием API Azure Resource Manager и устранении распространенных проблем с учетными записями.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/19/2019
ms.topic: quickstart
ms.service: cost-management-billing
manager: vitavor
ms.custom: secdec18
ms.openlocfilehash: 77ea61d6aafad556a90b934d77569b8166aeafef
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74230176"
---
# <a name="activate-azure-subscriptions-and-accounts-with-cloudyn"></a>Активация подписок и учетных записей Azure с помощью Cloudyn

Добавляя или обновляя учетные данные Azure Resource Manager, вы позволяете службе Cloudyn обнаруживать все учетные записи и подписки в клиенте Azure. Если на виртуальных машинах включено расширение системы диагностики Azure, служба Cloudyn может собирать расширенные метрики, такие как показатели загрузки ЦП и памяти. В этой статье показано, как разрешить доступ к новым и имеющимся учетным записям с использованием API Azure Resource Manager. Здесь также рассматривается, как устранять распространенные проблемы с учетными записями.

Служба Cloudyn не имеет доступа к большинству данных подписки Azure, если подписка _не активирована_. Необходимо изменить _неактивированные_ учетные записи, чтобы служба Cloudyn могла получить к ним доступ.

## <a name="required-azure-permissions"></a>Необходимые разрешения Azure

Для выполнения процедур, описанных в этой статье, необходимы определенные разрешения. У вас или администратора клиента должны быть оба следующих разрешения:

- разрешение на регистрацию приложения CloudynCollector в клиенте Azure AD;
- возможность назначать приложения ролям в подписках Azure.

У учетных записей в подписках Azure должен быть доступ `Microsoft.Authorization/*/Write` для назначения приложения CloudynCollector. Это разрешение предоставляется ролью [владельца](../role-based-access-control/built-in-roles.md#owner) или [администратора доступа пользователей](../role-based-access-control/built-in-roles.md#user-access-administrator).

Если вашей учетной записи назначена роль **участник**, значит у вас нет соответствующего разрешения для назначения приложения. При попытке присвоить приложение CloudynCollector подписке Azure появится сообщение об ошибке.

### <a name="check-azure-active-directory-permissions"></a>Проверка разрешений в Azure Active Directory

1. Войдите на [портал Azure](https://portal.azure.com).
2. На портале Azure выберите **Azure Active Directory**.
3. В Azure Active Directory выберите **Параметры пользователя**.
4. Проверьте параметр **Регистрация приложений**.
    - Если задано значение **Да**, тогда пользователи без прав администратора могут регистрировать приложения AD. Это означает, что приложение в клиенте Azure AD может зарегистрировать любой пользователь.  
    ![В параметрах пользователя выберите регистрацию приложений](./media/activate-subs-accounts/app-register.png)
    - Если для параметра **Регистрация приложений** задано значение **Нет**, тогда только пользователи с правами администратора в клиенте могут регистрировать приложения Azure Active Directory. Администратор клиента должен зарегистрировать приложение CloudynCollector.


## <a name="add-an-account-or-update-a-subscription"></a>Добавление учетной записи или обновление подписки

При добавлении учетной записи или обновлении подписки вы предоставляете службе Cloudyn доступ к данным Azure.

### <a name="add-a-new-account-subscription"></a>Добавление новой учетной записи (подписки)

1. На портале Cloudyn щелкните символ шестеренки в верхнем углу справа и выберите **Cloud Accounts** (Облачные учетные записи).
2. Щелкните **Добавить новую учетную запись**, и появится поле **добавления новой учетной записи**. Введите необходимые сведения.  
    ![Введите необходимые сведения в поле "Добавление новой учетной записи"](./media/activate-subs-accounts/add-new-account.png)

### <a name="update-a-subscription"></a>Обновление подписки

1. Если вы хотите обновить подписку, которая _не активирована_ и уже существует в службе Cloudyn в разделе управления учетными записями, щелкните символ редактирования справа от родительского _GUID клиента_. Подписки группируются в родительском клиенте. Поэтому не активируйте их по отдельности.
    ![Выберите свой идентификатор клиента в поле повторного обнаружения подписок](./media/activate-subs-accounts/existing-sub.png)
2. При необходимости введите идентификатор клиента. Если вы не знаете идентификатор клиента, выполните следующие действия, чтобы найти его.
    1. Войдите на [портале Azure](https://portal.azure.com).
    2. На портале Azure выберите **Azure Active Directory**.
    3. Чтобы получить идентификатор клиента, щелкните **Свойства** для клиента Azure AD.
    4. Скопируйте идентификатор каталога (GUID). Это и есть ваш идентификатор клиента.
    Дополнительные сведения см. в разделе [Получение идентификатора клиента](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in).
3. При необходимости выберите идентификатор тарифа. Если вы не знаете свой идентификатор тарифа, выполните следующие действия, чтобы найти его.
    1. В верхнем правом углу на портале Azure щелкните информацию о пользователе, а затем выберите **Показать мой счет**.
    2. В разделе **Учетная запись выставления счетов** щелкните **Подписки**.
    3. В разделе **Мои подписки** выберите подписку.
    4. Идентификатор тарифа отображается в разделе **Идентификатор предложения**. Скопируйте идентификатор предложения для подписки.
4. В поле "Добавить новую учетную запись" (или "Изменить подписку") нажмите кнопку **Сохранить** (или **Далее**). Вы будете перенаправлены на портал Azure.
5. Войдите на портал. Нажмите кнопку **Принять**, чтобы авторизовать сборщик Cloudyn для получения доступа к учетной записи Azure.

    Вы перейдете на страницу управления учетными записями Cloudyn, и состояние учетной записи в подписке сменится на **active** (активна). В столбце Resource Manager для нее должен отображаться символ зеленой галочки.

    Если этот символ не отображается для одной или нескольких подписок, значит у вас нет разрешений на создание приложения для чтения (CloudynCollector) для подписки. Пользователю с разрешениями более высокого уровня для подписки необходимо повторить этот процесс.

Просмотрите видео [Подключение к Azure Resource Manager с помощью службы Cloudyn](https://youtu.be/oCIwvfBB6kk), в котором описан пошаговый процесс.

>[!VIDEO https://www.youtube.com/embed/oCIwvfBB6kk?ecver=1]

## <a name="resolve-common-indirect-enterprise-set-up-problems"></a>Устранение ошибок, связанных с корпоративной настройкой через посредника

Если вы работаете на условиях соглашения Enterprise или участвуете в программе для поставщиков облачных решений (CSP), при первом использовании портала Cloudyn может появиться следующее сообщение:

- *The specified API key is not a top level enrollment key* (Указанный ключ API не является ключом регистрации верхнего уровня). Эта ошибка отображается в мастере **настройки службы Cloudyn**.
- *Direct Enrollment – No* (Прямая регистрация — нет). Эта ошибка отображается на портале соглашения Enterprise.
- *Нет данных об использовании за последние 30 дней. Обратитесь к распространителю, чтобы убедиться, что разметка была включена для учетной записи Azure.* Эта ошибка отображается на портале Cloudyn.

Предыдущие сообщения об ошибках указывают на то, что вы приобрели Соглашение Azure Enterprise через торгового посредника или CSP. Торговый посредник или CSP должен включать _исправления_ для учетной записи Azure, чтобы вы могли просматривать данные в Cloudyn.

Вот как можно исправить ошибки:

1. Торговый посредник должен включить _исправления_ для вашей учетной записи. Инструкции см. в [руководстве по непрямому переносу клиентов в облачную среду](https://ea.azure.com/api/v3Help/v2IndirectCustomerOnboardingGuide).
2. Создайте ключ соглашения Azure Enterprise для использования с Cloudyn. Инструкции см. в статье [Регистрация соглашения Azure Enterprise и просмотр данных о затратах](https://docs.microsoft.com/azure/cost-management/quick-register-ea).

Прежде чем создавать ключ API соглашения Azure Enterprise, необходимый для установки Cloudyn, необходимо включить API выставления счетов Azure, следуя указаниям в статьях:

- [Обзор API-интерфейсов отчетов для корпоративных клиентов](../billing/billing-enterprise-api.md)
- [API отчетов Microsoft Azure Enterprise Portal](https://ea.azure.com/helpdocs/reportingAPI) в разделе **Включение доступа к данным API**.

Необходимо предоставить администраторам отдела, владельцам учетных записей и администраторам предприятия разрешения на _просмотр затрат_ с помощью API выставления счетов.

Только администратор служб Azure может включить службу Cloudyn. Разрешений соадминистратора недостаточно. Но вы можете обойти требование, касающееся разрешений администратора. Можно запросить у администратора Azure Active Directory разрешение на авторизацию **CloudynAzureCollector** с помощью скрипта PowerShell. Приведенный ниже скрипт предоставляет разрешение на регистрацию субъекта-службы Azure Active Directory **CloudynAzureCollector**.


```powershell
#THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

#Tenant - enter your tenant ID or Name
$tenant = "<ReplaceWithYourTenantID>"

#Cloudyn Collector application ID
$appId = "83e638ef-7885-479f-bbe8-9150acccdb3d"

#URL to activate the consent screen
$url = "https://login.windows.net/"+$tenant+"/oauth2/authorize?api-version=1&response_type=code&client_id="+$appId+"&redirect_uri=http%3A%2F%2Flocalhost%3A8080%2FCloudynJava&prompt=consent"

#Choose your browser, the default is Internet Explorer

#Chrome
#[System.Diagnostics.Process]::Start("chrome.exe", "--incognito $url")

#Firefox
#[System.Diagnostics.Process]::Start("firefox.exe","-private-window $url" )

#IExplorer
[System.Diagnostics.Process]::Start("iexplore.exe","$url -private" )

```

## <a name="next-steps"></a>Дополнительная информация

- Если вы еще не завершили первое руководство по службе Cloudyn, ознакомьтесь с ним в статье [Просмотр сведений об использовании и затратах](tutorial-review-usage.md).
