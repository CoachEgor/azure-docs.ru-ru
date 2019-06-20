---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/03/2016
ms.author: estfan
ms.openlocfilehash: 7cfce34cb2d6002dba5ec570bf859ec47e894c65
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185543"
---
#### <a name="prerequisites"></a>Технические условия
* Учетная запись Azure. Вы можете создать [бесплатную учетную запись](https://azure.microsoft.com/free).
* Учетная запись [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3). 

Прежде чем использовать свою учетную запись OneDrive в приложении логики, необходимо авторизовать приложение логики для подключения к этой учетной записи.  Это можно легко сделать из приложения логики на портале Azure. 

Авторизуйте приложение логики для подключения к учетной записи OneDrive, выполнив приведенные ниже действия.

1. Создайте приложение логики. В конструкторе приложений логики в раскрывающемся списке выберите параметр **Показать API, управляемые Майкрософт**, а затем введите в поле поиска "onedrive". Выберите один из триггеров или одно из действий.  
   ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Если вы ранее не создавали подключения к OneDrive, то вам будет предложено ввести учетные данные OneDrive.  
   ![](./media/connectors-create-api-onedrive/onedrive-2.png)
3. Выберите **Войти** и введите имя пользователя и пароль. Выберите **Войти**.  
   ![](./media/connectors-create-api-onedrive/onedrive-3.png)   
   
    Эти учетные данные используются для авторизации приложения логики, чтобы оно могло подключиться и получить доступ к данным в вашей учетной записи OneDrive. 
4. Выберите **Да**, чтобы авторизовать приложение логики для использования учетной записи OneDrive.  
   ![](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. Обратите внимание, что было создано подключение. Теперь перейдите к другим действиям в приложении логики.  
   ![](./media/connectors-create-api-onedrive/onedrive-5.png)

