---
title: Включить автономную синхронизацию (Android)
description: Узнайте, как использовать мобильные приложения службы приложений для кэширования и синхронизации автономных данных в приложении Android.
ms.assetid: 32a8a079-9b3c-4faf-8588-ccff02097224
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: c215105af5fe1ef8056b0d816cf2c2a6b96f2038
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2020
ms.locfileid: "77461629"
---
# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Включение автономной синхронизации для мобильного приложения Android
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Обзор
В этом учебнике рассматривается функция автономной синхронизации мобильных приложений Azure для Android. Автономная синхронизация позволяет пользователям взаимодействовать с мобильным приложением &mdash;просматривать, добавлять или изменять данные &mdash; даже при отсутствии подключения к сети. Изменения сохраняются в локальной базе данных. Как только устройство возвращается в режим подключения к сети, эти изменения синхронизируются с удаленной внутренним сервером.

Если вы впервые работаете с мобильными приложениями Azure, сначала ознакомьтесь с руководством [Создание приложения Android]. Если вы не используете скачанный проект быстрого запуска сервера, в проект необходимо добавить пакет расширений доступа к данным. в статье [Работа с пакетом SDK для внутреннего сервера .NET для мобильных приложений Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Дополнительные сведения о функции автономной синхронизации см. в статье [Автономная синхронизация данных в мобильных приложениях Azure].

## <a name="update-the-app-to-support-offline-sync"></a>Обновление приложения для поддержки синхронизации автономных данных
При включенной синхронизации автономных данных операции чтения и записи осуществляются с помощью *таблицы синхронизации* (с помощью интерфейса *IMobileServiceSyncTable*), которая является частью базы данных **SQLite** на вашем устройстве.

Для принудительной отправки изменений на устройстве в мобильные службы Azure и получения изменений из мобильных служб используйте *контекст синхронизации* (*MobileServiceClient.SyncContext*), который инициализируется с помощью локальной базы данных.

1. В `TodoActivity.java` закомментируйте существующее определение `mToDoTable` и раскомментируйте версию таблицы синхронизации.
   
        private MobileServiceSyncTable<ToDoItem> mToDoTable;
2. В методе `onCreate` закомментируйте существующую инициализацию `mToDoTable` и раскомментируйте следующее определение.
   
        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
3. В `refreshItemsFromTable` закомментируйте определение `results` и раскомментируйте следующее определение.
   
        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();
4. Закомментируйте определение `refreshItemsFromMobileServiceTable`.
5. Раскомментируйте определение `refreshItemsFromMobileServiceTableSyncTable`.
   
        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }
6. Раскомментируйте определение `sync`.
   
        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-the-app"></a>Тестирование приложения
В этом разделе мы протестируем поведение приложения при доступной сети Wi-Fi, а затем отключим Wi-Fi и рассмотрим автономный сценарий.

Добавляемые элементы данных будут находиться в локальном хранилище SQLight, но не будут синхронизированы с мобильной службой, пока вы не нажмете кнопку **Обновить** . У разных приложений могут быть разные требования к синхронизации данных, но для демонстрационных целей этого учебника мы будем считать, что пользователь явно запрашивает ее.

При нажатии этой кнопки запускается новая фоновая задача. Сначала она отправляет все изменения, внесенные в локальное хранилище, используя контекст синхронизации, а затем переносит все измененные данные из службы Azure в локальную таблицу.

### <a name="offline-testing"></a>Тестирование в автономном режиме
1. Переведите устройство или эмулятор в режим *В самолете*. Это позволит смоделировать автономный сценарий.
2. Добавьте несколько элементов *ToDo* или отметьте некоторые элементы как завершенные. Выйдите из эмулятора (или принудительно закройте приложение) и перезапустите его. Убедитесь, что изменения на устройстве по прежнему отображаются (так как они хранятся в локальном хранилище SQLight).
3. Просмотрите содержимое таблицы Azure *TodoItem* с помощью инструмента SQL, например *SQL Server Management Studio* или с помощью клиента REST, например *Fiddler* или *Postman*. Убедитесь, что новые элементы *не* были синхронизированы с сервером.
   
       + Для серверной части Node.js перейдите на [портал Azure](https://portal.azure.com/), затем на странице серверной части своего мобильного приложения щелкните **Easy Tables** > **TodoItem**, чтобы просмотреть содержимое таблицы `TodoItem`.
       + Для серверной части .NET просмотрите содержимое таблицы с помощью средства SQL, например *SQL Server Management Studio*, или с помощью клиента REST, например *Fiddler* или *Postman*.
4. Включите Wi-Fi в эмуляторе или на устройстве. Затем нажмите кнопку **Обновить** .
5. Снова просмотрите данные TodoItem на портале Azure. Должны отображаться как новые, так и измененные TodoItem.

## <a name="additional-resources"></a>Дополнительные ресурсы
* [Автономная Синхронизация данных в мобильных приложениях Azure]
* [Облачное покрытие: автономная синхронизация в мобильных службах Azure] \(Примечание. видео находится в мобильных службах, но автономная синхронизация работает аналогичным образом в мобильных приложениях Azure\)

<!-- URLs. -->

[Автономная Синхронизация данных в мобильных приложениях Azure]: app-service-mobile-offline-data-sync.md

[Создание приложения Android]: app-service-mobile-android-get-started.md

[Облачное покрытие: автономная синхронизация в мобильных службах Azure]: https://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: https://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

