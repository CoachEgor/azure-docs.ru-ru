---
title: Совместное использование данных и их получение из Хранилища BLOB-объектов Azure и Azure Data Lake Storage
description: Сведения о совместном использовании и получении данных из хранилища BLOB-объектов Azure и Azure Data Lake Storage
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 12/16/2020
ms.openlocfilehash: 9dfc8be54fc55842440e376916b2eb9bb04a4610
ms.sourcegitcommit: 86acfdc2020e44d121d498f0b1013c4c3903d3f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97617091"
---
# <a name="share-and-receive-data-from-azure-blob-storage-and-azure-data-lake-storage"></a>Совместное использование данных и их получение из Хранилища BLOB-объектов Azure и Azure Data Lake Storage

[!INCLUDE[appliesto-storage](includes/appliesto-storage.md)]

Общий ресурс данных Azure поддерживает общий доступ на основе моментальных снимков из учетной записи хранения. В этой статье объясняется, как предоставлять общий доступ и принимать данные из следующих источников: хранилище BLOB-объектов Azure, Azure Data Lake Storage 1-го поколения Azure Data Lake Storage 2-го поколения.

Общая папка данных Azure поддерживает общий доступ к файлам, папкам и файловым системам Azure Data Lake Gen1 и Azure Data Lake Gen2. Он также поддерживает общий доступ к BLOB-объектам, папкам и контейнерам из хранилища BLOB-объектов Azure. В настоящее время поддерживается только блочный BLOB-объект. Данные, общие для этих источников, можно получить в Azure Data Lake Gen2 или хранилище BLOB-объектов Azure.

Если файловые системы, контейнеры или папки используются совместно с общим доступом на основе моментальных снимков, потребитель данных может создать полную копию общих данных или использовать функцию добавочного моментального снимка для копирования только новых или обновленных файлов. Добавочный моментальный снимок основан на времени последнего изменения файлов. Существующие файлы с тем же именем будут перезаписаны во время создания моментального снимка. Файл, удаленный из источника, не удален на целевом объекте. Пустые вложенные папки в источнике не копируются на целевой объект. 

## <a name="share-data"></a>Совместное использование данных

### <a name="prerequisites-to-share-data"></a>Необходимые условия для совместного использования данных

* Подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/), прежде чем начинать работу.
* Адрес электронной почты для входа получателей в Azure (псевдонимы электронной почты не поддерживаются).
* Если исходное хранилище данных Azure находится в подписке Azure, отличной от той, которая будет использоваться для создания ресурса Data Share, зарегистрируйте [поставщик ресурсов Microsoft.DataShare](concepts-roles-permissions.md#resource-provider-registration) в подписке, в которой находится хранилище данных Azure. 

### <a name="prerequisites-for-source-storage-account"></a>Необходимые условия для исходной учетной записи хранения

* Учетная запись хранения Azure. Если у вас ее еще нет, вы можете создать [Учетную запись службі хранилища Azure](../storage/common/storage-account-create.md)
* Разрешение на запись в учетную запись хранения, которая имеется в *Microsoft.Storage/storageAccounts/write*. Это разрешение назначено роли участника.
* Разрешение на добавление назначения ролей в учетную запись хранения, имеющуюся в *Microsoft.Authorization/role assignments/write*. Это разрешение назначено роли владельца. 

### <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портал Azure](https://portal.azure.com/).

### <a name="create-a-data-share-account"></a>Создание учетной записи для общего доступа к данным

Создайте ресурс Azure Data Share в группе ресурсов Azure.

1. Нажмите кнопку меню в левом верхнем углу окна портала и выберите элемент **Создать ресурс** (+).

1. Найдите *Data Share*.

1. Выберите Data Share и команду **Создать**.

1. Укажите основные сведения о ресурсе Azure Data Share, указав следующую информацию. 

     **Параметр** | **Рекомендуемое значение** | **Описание поля**
    |---|---|---|
    | Подписка | Ваша подписка | Выберите подписку Azure, которую нужно использовать для своей учетной записи Data Share.|
    | Группа ресурсов | *test-resource-group* | Воспользуйтесь существующей группой ресурсов или создайте новую. |
    | Расположение | *Восточная часть США 2* | Выберите регион для учетной записи Data Share.
    | Имя | *datashareaccount* | Укажите имя вашей учетной записи Data Share. |
    | | |

1. Выберите **Проверить и создать**, а затем **Создать**, чтобы подготовить учетную запись для общего ресурса данных. Подготовка новой учетной записи для общего доступа к данным обычно занимает около 2 минут или меньше. 

1. Когда развертывание будет завершено, выберите **Перейти ресурсу**.

### <a name="create-a-share"></a>Создание общей папки

1. Перейдите на страницу "Обзор Data Share".

    ![Общий доступ к данным](./media/share-receive-data.png "Общий доступ к данным") 

1. Выберите команду **Начать совместное использование данных**.

1. Нажмите кнопку **Создать**.   

1. Введите сведения о совместном использовании. Укажите имя, тип совместного использования, описание содержимого и условия использования (необязательно). 

    ![EnterShareDetails](./media/enter-share-details.png "Введите сведения об общем ресурсе") 

1. Выберите **Continue** (Продолжить).

1. Чтобы добавить наборы данных в общий ресурс, выберите **Добавить наборы данных**. 

    ![Добавление наборов данных в общий ресурс](./media/datasets.png "Наборы данных")

1. Выберите тип набора данных, который хотите добавить. Вы увидите другой список типов наборов данных в зависимости от типа ресурса (моментальный снимок или на странице), который вы выбрали на предыдущем этапе. 

    ![AddDatasets](./media/add-datasets.png "Добавление наборов данных")    

1. Перейдите к объекту, доступ к которому нужно предоставить, и выберите "Добавить наборы данных". 

    ![SelectDatasets](./media/select-datasets.png "Выбор наборов данных")    

1. На вкладке "Получатели" введите адреса электронной почты получателей данных, выбрав команду "+Добавить получателя". 

    ![AddRecipients](./media/add-recipient.png "Добавление получателей") 

1. Выберите **Continue** (Продолжить).

1. Если выбран тип общего доступа к моментальным снимкам, вы можете настроить расписание моментальных снимков для предоставления обновлений данных потребителям. 

    ![EnableSnapshots](./media/enable-snapshots.png "Включение моментальных снимков") 

1. Выберите время начала и интервал повторения. 

1. Выберите **Continue** (Продолжить).

1. На вкладке "Просмотр и создание" просмотрите содержимое пакета, параметры, получателей и параметры синхронизации. Нажмите кнопку **Создать**.

Azure Data Share создан, и получатель Data Share готов принять ваше приглашение. 

## <a name="receive-data"></a>Получение данных

### <a name="prerequisites-to-receive-data"></a>Необходимые условия для получения данных
Чтобы принять приглашение в общий ресурс данных, вам необходимо подготовить к работе ряд ресурсов Azure, которые перечислены ниже. 

Прежде чем принять приглашение в общий ресурс данных, выполните все предварительные требования. 

* Подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/), прежде чем начинать работу.
* Приглашение Data Share. Приглашение от Microsoft Azure с темой "Приглашение Azure Data Share от **<yourdataprovider@domain.com>** ".
* Зарегистрируйте [поставщик ресурсов Microsoft.DataShare](concepts-roles-permissions.md#resource-provider-registration) в подписке Azure, где вы создадите ресурс Data Share и подписку Azure, в которой находятся целевые хранилища данных Azure.

### <a name="prerequisites-for-target-storage-account"></a>Необходимые условия для целевой учетной записи хранения

* Учетная запись хранения Azure. Если у вас еще нет учетной записи хранения Azure, вы можете [создать ее](../storage/common/storage-account-create.md). 
* Разрешение на запись в учетную запись хранения, которая имеется в *Microsoft.Storage/storageAccounts/write*. Это разрешение назначено роли участника. 
* Разрешение на добавление назначения ролей в учетную запись хранения, имеющуюся в *Microsoft.Authorization/role assignments/write*. Это разрешение назначено роли владельца.  

### <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портал Azure](https://portal.azure.com/).

### <a name="open-invitation"></a>Открытие приглашения

1. Вы можете открыть приглашение из электронной почты или непосредственно из портала Azure. 

   Чтобы открыть приглашение из электронной почты, проверьте папку "Входящие" на наличие приглашения от поставщика данных. Это должно быть приглашение от Microsoft Azure с названием **Приглашение Azure Data Share от <yourdataprovider@domain.com>** . Выберите **Просмотреть приглашение**, чтобы увидеть ваше приглашение в Azure. 

   Чтобы открыть приглашение непосредственно из портала Azure, найдите на портале параметр **Приглашения Data Share**. Вы перейдете к списку приглашений Data Share.

   ![Список приглашений](./media/invitations.png "Список приглашений") 

1. Выберите общий ресурс для просмотра. 

### <a name="accept-invitation"></a>Принятие приглашения
1. Просмотрите все поля, в том числе **Условия использования**. Если вы согласны с условиями использования, установите флажок для подтверждения своего согласия. 

   ![Условия использования](./media/terms-of-use.png "Условия использования") 

1. В разделе *Target Data Share Account* (Целевая учетная запись Data Share) выберите подписку и группу ресурсов, куда будете развертывать общий ресурс данных. 

   В поле **учетной записи Data Share** выберите **Создать**, если у вас нет существующей учетной записи Data Share. В противном случае выберите учетную запись Data Share, где бы вы хотели принимать данные. 

   В поле **Received Share Name** (Имя полученного общего ресурса) можно оставить значение по умолчанию, заданное поставщиком данных, или указать для полученного общего ресурса новое имя. 

   Согласившись с условиями использования и указав учетную запись Data Share для управления полученным общим ресурсом, выберите **Accept and Configure** (Принять и настроить). Будет создана подписка на общий ресурс. 

   ![Принятие параметров](./media/accept-options.png "Принятие параметров") 

   В результате вы перейдете к полученному общему ресурсу в учетной записи Data Share. 

   Если вы не хотите принимать приглашение, выберите *Отклонить*. 

### <a name="configure-received-share"></a>Настройка полученного общего ресурса
Выполните следующие действия, чтобы настроить место для получения данных.

1. Перейдите на вкладку **Наборы данных**. Установите флажок рядом с набором данных, которому следует назначить место назначение. Нажмите **+ Сопоставить с целевым объектом**, чтобы выбрать целевое хранилище данных. 

   ![Кнопка "Сопоставить с целевым объектом"](./media/dataset-map-target.png "Сопоставление с целевым объектом") 

1. Выберите целевое хранилище данных, в котором должны находиться данные. Все файлы данных в целевом хранилище данных с одинаковым путем и именем будут перезаписаны. 

   ![Целевая учетная запись хранения](./media/map-target.png "Целевое хранилище") 

1. Для совместного использования на основе моментальных снимков можно также включить на вкладе **Расписание моментальных снимков** функцию расписания моментальных снимков при условии, что поставщик данных создал такое расписание для регулярного обновления данных. Установите флажок рядом с расписанием моментальных снимков и выберите **+ Включить**.

   ![Включение расписания моментальных снимков](./media/enable-snapshot-schedule.png "Включение расписания моментальных снимков")

### <a name="trigger-a-snapshot"></a>Активация создания моментальных снимков
Эти действия применимы только к общему доступу на основе моментальных снимков.

1. Вы можете активировать создание моментальных снимков, нажав на вкладке **Подробности** кнопку **Активировать моментальный снимок**. Здесь вы сможете активировать полные или добавочные моментальные снимки ваших данных. Если вы осуществляете прием данных от поставщика в первый раз, выберите полное копирование. 

   ![Активация создания моментального снимка](./media/trigger-snapshot.png "Активация создания моментального снимка") 

1. Если состояние последнего запуска является *успешным*, перейдите к целевому хранилищу данных для просмотра полученных данных. Выберите **Наборы данных** и щелкните ссылку в целевом пути. 

   ![Наборы данных объекта-получателя](./media/consumer-datasets.png "Сопоставление набора данных объекта-получателя") 

### <a name="view-history"></a>Просмотр истории
Этот шаг применим только для общего доступа на основе моментальных снимков. Чтобы просмотреть журнал моментальных снимков, выберите вкладку **Журнал**. Здесь вы найдете журнал всех моментальных снимков за последние 30 дней. 

## <a name="next-steps"></a>Дальнейшие действия
Вы узнали, как предоставлять общий доступ и принимать данные из учетной записи хранения с помощью службы общего доступа к данным Azure. Чтобы получить дополнительные сведения о совместном использовании из других источников данных, перейдите к [поддерживаемым хранилищам данных](supported-data-stores.md).