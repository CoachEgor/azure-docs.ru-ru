---
author: clemensv
ms.service: service-bus-relay
ms.topic: include
ms.date: 11/09/2018
ms.author: clemensv
ms.openlocfilehash: bbcf38ab54632144920b729f433c222f426d96dc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66111543"
---
1. Войдите на [портал Azure][Azure portal].
2. В меню слева выберите **+ Создать ресурс**. Выберите **Интеграция** > **Ретрансляция**. Если пункт **Ретрансляция** в списке отсутствует, щелкните **Просмотреть все** в правом верхнем углу. 
3. В разделе **Создать пространство имен** укажите имя пространства имен. Система немедленно проверяет, доступно ли оно.
4. В поле **Подписка** выберите подписку Azure, в рамках которой будет создано пространство имен.
5. В поле [Группа ресурсов](../articles/azure-resource-manager/manage-resource-groups-portal.md) выберите существующую группу ресурсов, в которую будет включено это пространство имен, или создайте новую.  
6. В поле **Расположение** выберите страну или регион для размещения пространства имен.
   
    ![Создание пространства имен][create-namespace]
7. Нажмите кнопку **Создать**. Система создаст пространство имен и включит его. Через несколько минут система подготовит к работе ресурсы для вашей учетной записи.

### <a name="get-management-credentials"></a>Получение учетных данных управления

1. Нажмите **All resources** (Все ресурсы) и выберите созданное имя пространства имен.
2. В окне передачи пространства имен выберите **Политики общего доступа**.  
3. В окне **Политики общего доступа** выберите **RootManageSharedAccessKey**.
   
    ![Сведения о подключении][connection-info]
4. В разделе **Policy: RootManageSharedAccessKey** (Политика: RootManageSharedAccessKey) нажмите кнопку **Копировать** рядом с элементом **Строка подключения — первичный ключ**. Строка подключения будет скопирована в буфер обмена для дальнейшего использования. Вставьте на время эти значения в Блокноте или любом другом месте.
   
    ![Строка подключения][connection-string]

5. Повторите предыдущий шаг, скопировав и вставив значение **первичного ключа** во временное расположение для последующего использования.  

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace.png
[connection-info]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
