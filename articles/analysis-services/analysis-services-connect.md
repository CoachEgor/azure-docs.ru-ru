---
title: Подключение к Azure Analysis Services | Документация Майкрософт
description: Узнайте, как подключиться к серверу служб Analysis Services в Azure и получить от него данные.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 03/29/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 9a8863189ee9cb63d86b157c0bbebb6fd16116b0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61027989"
---
# <a name="connecting-to-servers"></a>Подключение к серверам

В этой статье описывается подключение к серверу с помощью приложений для моделирования данных и управления, таких как SQL Server Management Studio (SSMS) или SQL Server Data Tools (SSDT). Для этого также можно использовать клиентские приложения создания отчетов, такие как Microsoft Excel, Power BI Desktop или пользовательские приложения. Для подключений к службам Azure Analysis Services используется протокол HTTPS.

## <a name="client-libraries"></a>Клиентские библиотеки

[Получение последних версий клиентских библиотек](analysis-services-data-providers.md)

Все подключения к серверу независимо от типа требуют обновленных клиентских библиотек AMO, ADOMD.NET и OLEDB для взаимодействия с сервером Analysis Services. Для SSMS, SSDT, Excel 2016 и более поздней версии, а также Power BI последние версии клиентских библиотек устанавливаются вместе с ежемесячными выпусками. Однако в некоторых случаях последние версии могут отсутствовать в приложении. Например, такое возможно, если обновление откладывается политикой либо если обновление Office 365 производится по отложенному каналу.

## <a name="server-name"></a>Имя сервера

При создании сервера служб Analysis Services в Azure вам нужно указать уникальное имя сервера и регион, в котором он будет создан. При указании имени сервера в подключении используется следующая схема именования:

```
<protocol>://<region>/<servername>
```
 Здесь protocol — это строка **asazure**, region — код URI региона, в котором был создан сервер (например, westus.asazure.windows.net), а servername — это имя сервера, уникальное в пределах указанного региона.

### <a name="get-the-server-name"></a>Получение имени сервера

На **портале Azure** выберите сервер, а затем щелкните **Обзор** > **Имя сервера** и скопируйте имя сервера. Если к этому серверу подключаются и другие пользователи в организации, вы можете сообщить им это имя сервера. Указывая имя сервера, необходимо использовать полный путь.

![Получение имени сервера в Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

> [!NOTE]
> Протокол для Восточной части США 2 — **aspaaseastus2**.

## <a name="connection-string"></a>Строка подключения

При подключении к службам Azure Analysis Services с помощью модели табличного объекта используйте следующие форматы строки подключения:

###### <a name="integrated-azure-active-directory-authentication"></a>Встроенная проверка подлинности Azure Active Directory

Служба встроенной проверки подлинности извлекает данные из кэша учетных данных Azure Active Directory, если он доступен. В противном случае отобразится окно входа в Azure.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Проверка подлинности Azure Active Directory с использованием имени пользователя и пароля

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a>Проверка подлинности Windows (встроенный механизм безопасности)

Используйте учетную запись Windows, с которой выполняется текущий процесс.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```

## <a name="connect-using-an-odc-file"></a>Подключение с помощью ODC-файла

Пользователи предыдущих версий Excel могут подключаться к серверу Azure Analysis Services с помощью файла подключения к данным Office (ODC). Дополнительные сведения см. в разделе [Создание файла подключения к данным Office (ODC-файла)](analysis-services-odc.md).


## <a name="next-steps"></a>Дальнейшие действия

[Подключение с помощью Excel](analysis-services-connect-excel.md)    
[Подключение с помощью Power BI](analysis-services-connect-pbi.md)   
[Управление службами Analysis Services](analysis-services-manage.md)   

