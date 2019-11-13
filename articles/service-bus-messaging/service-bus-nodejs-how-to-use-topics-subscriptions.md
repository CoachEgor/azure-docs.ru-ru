---
title: Краткое руководство. Использование разделов и подписок служебной шины Azure с Node.js
description: Краткое руководство. Узнайте, как использовать разделы и подписки служебной шины в Azure в приложении Node.js.
services: service-bus-messaging
documentationcenter: nodejs
author: axisc
manager: timlt
editor: spelluru
ms.assetid: b9f5db85-7b6c-4cc7-bd2c-bd3087c99875
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 11/05/2019
ms.author: aschhab
ms.openlocfilehash: 40543f55dc0cb56f6bc575f926456faf2d0ae5a3
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2019
ms.locfileid: "73719208"
---
# <a name="quickstart-how-to-use-service-bus-topics-and-subscriptions-with-nodejs-and-the-azure-sb-package"></a>Краткое руководство. Использование разделов и подписок служебной шины с Node.js и пакетом azure/sb
> [!div class="op_multi_selector" title1="Язык программирования" title2="Пакет Node.js"]
> - [(Node.js | azure-sb)](service-bus-nodejs-how-to-use-topics-subscriptions.md)
> - [(Node.js | @azure/service-bus)](service-bus-nodejs-how-to-use-topics-subscriptions-new-package.md)

Из этого учебника вы узнаете, как создавать приложения Node.js для отправки сообщений в раздел служебной шины и получения сообщений из подписки служебной шины с помощью пакета [azure-sb](https://www.npmjs.com/package/azure-sb). Эти примеры написаны на JavaScript и используют [модуль Node.js для Azure](https://www.npmjs.com/package/azure) с интегрированным пакетом `azure-sb`.

Пакет [azure-sb](https://www.npmjs.com/package/azure-sb) работает с [интерфейсами REST API служебной шины](/rest/api/servicebus/service-bus-runtime-rest). Вы можете ускорить работу с помощью нового пакета [@azure/service-bus](https://www.npmjs.com/package/@azure/service-bus), который использует более быстрый [протокол AMQP 1.0](service-bus-amqp-overview.md). Дополнительные сведения о новом пакете см. в статьях [об использовании разделов и подписок служебной шины с Node. js и пакетом @azure/service-bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-nodejs-how-to-use-topics-subscriptions-new-package). Далее в этой статье описывается работа с пакетом [azure](https://www.npmjs.com/package/azure).

Здесь рассматриваются следующие сценарии:

- создание разделов и подписок; 
- создание фильтров подписки; 
- отправка сообщений в раздел; 
- получение сообщений из подписки;
- удаление разделов и подписок. 

Дополнительные сведения о разделах и подписках см. в разделе [Дальнейшие действия](#next-steps).

## <a name="prerequisites"></a>Предварительные требования
- Подписка Azure. Для работы с этим учебником требуется учетная запись Azure. Вы можете [активировать преимущества подписчика Visual Studio или MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) или [зарегистрироваться для получения бесплатной учетной записи](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
- Выполните шаги из [краткого руководства по созданию раздела Служебной шины и подписок на него с помощью портала Azure](service-bus-quickstart-topics-subscriptions-portal.md), чтобы создать **пространство имен** Служебной шины и получить **строку подключения**.

    > [!NOTE]
    > В рамках этого краткого руководства вы создадите **раздел** и **подписку** на этот раздел с помощью **Node.js**. 

## <a name="create-a-nodejs-application"></a>Создание приложения Node.js
Создайте пустое приложение Node.js. Указания по созданию приложения Node.js см. в статьях [Создание и развертывание приложения Node.js на веб-сайте Azure], [Облачная служба Node.js][Node.js Cloud Service] (с помощью Windows PowerShell) или веб-сайт с WebMatrix.

## <a name="configure-your-application-to-use-service-bus"></a>Настройка приложения для использования служебной шины
Для использования служебной шины скачайте пакет Node.js для Azure. Пакет содержит набор библиотек, взаимодействующих со службами REST Service Bus.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Использование диспетчера пакета Node (NPM) для получения пакета
1. Откройте интерфейс командной строки, например **PowerShell** (Windows), **Terminal** (Mac) или **Bash** (Unix).
2. Перейдите в папку, где создан пример приложения.
3. В командной строке введите команду **npm install azure**. Должен получиться вот такой результат:

   ```
       azure@0.7.5 node_modules\azure
   ├── dateformat@1.0.2-1.2.3
   ├── xmlbuilder@0.4.2
   ├── node-uuid@1.2.0
   ├── mime@1.2.9
   ├── underscore@1.4.4
   ├── validator@1.1.1
   ├── tunnel@0.0.2
   ├── wns@0.5.3
   ├── xml2js@0.2.7 (sax@0.5.2)
   └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
   ```
3. Выполнив команду **ls** вручную, можно убедиться, что папка **node\_modules** создана. Найдите в папке пакет **azure**, который содержит библиотеки, необходимые для доступа к разделам служебной шины.

### <a name="import-the-module"></a>Импорт модуля
С помощью Блокнота или другого текстового редактора добавьте в начало файла **server.js** приложения следующее:

```javascript
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Настройка подключения к Service Bus
Модуль Azure считывает из переменной среды `AZURE_SERVICEBUS_CONNECTION_STRING` строку подключения, полученную на предыдущем шаге "Извлечение учетных данных". Если эта переменная среды не задана, при вызове `createServiceBusService` необходимо указать сведения об учетной записи.

Пример настройки переменных среды для облачной службы Azure см. в разделе [Пример для Azure CLI](../container-instances/container-instances-environment-variables.md#azure-cli-example).



## <a name="create-a-topic"></a>Создание раздела
Объект **ServiceBusService** позволяет работать с разделами. Следующий код создает объект **ServiceBusService**. Добавьте его в начало файла **server.js** после оператора импорта модуля Аzure.

```javascript
var serviceBusService = azure.createServiceBusService();
```

Если вызвать `createTopicIfNotExists` для объекта **ServiceBusService**, возвращается указанный раздел (при его наличии) или создается новый раздел с указанным именем. В следующем коде используется метод `createTopicIfNotExists`, чтобы создать раздел `MyTopic` или подключиться к нему.

```javascript
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

Метод `createTopicIfNotExists` также поддерживает дополнительные параметры, позволяющие переопределить настройки раздела по умолчанию, такие как срок жизни сообщения или максимальный размер раздела. 

В следующем примере показано, как установить максимальный размер раздела 5 ГБ и определить срок жизни в 1 минуту.

```javascript
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Фильтры
Используя **ServiceBusService**, к выполняемым операциям можно применить дополнительные операции фильтрации. К операциям фильтрации могут относиться ведение журнала, автоматический повтор и т. д. Фильтры являются объектами, реализующими метод со следующей сигнатурой:

```javascript
function handle (requestOptions, next)
```

Выполнив предварительную обработку параметров запроса, метод вызывает `next` и передает обратный вызов со следующей сигнатурой.

```javascript
function (returnObject, finalCallback, next)
```

В этом обратном вызове после обработки `returnObject` (ответа на запрос к серверу) функция обратного вызова должна вызвать функцию next (если она существует), чтобы продолжить обработку других фильтров, или вызвать `finalCallback`, чтобы завершить вызов службы.

В пакет SDK Azure для Node.js включены два фильтра, реализующие логику повторных попыток: **ExponentialRetryPolicyFilter** и **LinearRetryPolicyFilter**. Следующий код создает объект **ServiceBusService**, использующий **ExponentialRetryPolicyFilter**:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="create-subscriptions"></a>Создание подписок
Подписки на разделы также создаются с помощью объекта **ServiceBusService**. Подписки имеют имена и могут использовать дополнительный фильтр, который ограничивает набор сообщений, доставляемых в виртуальную очередь подписки.

> [!NOTE]
> По умолчанию подписки являются постоянными и существуют либо до их удаления, либо до удаления раздела, с которым они связаны. Если приложение содержит логику для создания подписки, она сначала должна проверить, существует ли подписка, используя метод `getSubscription`.
>
> Вы можете удалять подписки автоматически, задав [свойство AutoDeleteOnIdle](https://docs.microsoft.com/javascript/api/azure-arm-sb/sbsubscription?view=azure-node-latest#autodeleteonidle).

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Создание подписки с фильтром по умолчанию (MatchAll)
Фильтр **MatchAll** является фильтром по умолчанию при создании подписки. Если используется фильтр **MatchAll**, все сообщения, опубликованные в разделе, помещаются в виртуальную очередь подписки. В следующем примере создается подписка AllMessages и используется фильтр по умолчанию **MatchAll**.

```javascript
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Создание подписок с фильтрами
Вы также можете создать фильтры, позволяющие определять, какие сообщения, отправленные в раздел, будут отображаться в той или иной подписке раздела.

Самый гибкий тип фильтра, который поддерживается подписками, — это **SqlFilter**, реализующий подмножество SQL92. Фильтры SQL работают со свойствами сообщений, которые опубликованы в разделе. Дополнительные сведения о выражениях, которые можно использовать с фильтром SQL, см. в описании синтаксиса [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

Добавить фильтры в подписку можно с помощью метода `createRule` объекта **ServiceBusService**. Этот метод позволяет добавлять новые фильтры в существующую подписку.

> [!NOTE]
> Так как ко всем новым подпискам автоматически применяется фильтр по умолчанию, сначала необходимо удалить фильтр по умолчанию, иначе фильтр **MatchAll** переопределит поведение всех остальных заданных фильтров. Вы можете удалить правило по умолчанию с помощью метода `deleteRule` объекта **ServiceBusService**.
>
>

В следующем примере создается подписка `HighMessages`, содержащая фильтр **SqlFilter**, который выбирает только те сообщения, значение настраиваемого свойства `messagenumber` которых больше 3.

```javascript
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'HighMessages',
            'HighMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Аналогичным образом в следующем примере создается подписка `LowMessages` с фильтром **SqlFilter**, который выбирает только те сообщения, значение настраиваемого свойства `messagenumber` которых меньше или равно 3.

```javascript
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'LowMessages',
            'LowMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Если сообщение отправляется в раздел `MyTopic`, оно будет доставлено получателям, подписанным на подписку раздела `AllMessages`, и выборочно доставляться получателям, подписанным на подписки разделов `HighMessages` и `LowMessages` (в зависимости от содержимого сообщения).

## <a name="how-to-send-messages-to-a-topic"></a>Как отправлять сообщения в раздел
Чтобы отправить сообщение в раздел служебной шины, приложение должно использовать метод `sendTopicMessage` объекта **ServiceBusService**.
Сообщения, отправленные в разделы служебной шины, являются объектами **BrokeredMessage**.
Объекты **BrokeredMessage** содержат набор стандартных свойств (например, `Label` и `TimeToLive`), словарь, используемый для хранения настраиваемых свойств приложения, и текст из строковых данных. Приложение может задавать текст сообщения, передавая строковое значение в `sendTopicMessage`, и все обязательные стандартные свойства будут заполнены значениями по умолчанию.

В следующем примере показано, как отправить пять тестовых сообщений в `MyTopic`. Значение свойства `messagenumber` каждого сообщения зависит от итерации цикла (свойство определяет, какие подписки его получают).

```javascript
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Разделы служебной шины поддерживают максимальный размер сообщения 256 КБ для [уровня "Стандартный"](service-bus-premium-messaging.md) и 1 МБ для [уровня Premium](service-bus-premium-messaging.md). Максимальный размер заголовка, который содержит стандартные и настраиваемые свойства приложения, — 64 КБ. Ограничения количества сообщений в разделе нет, но существует ограничение на общий размер сообщений, содержащихся в разделе. Этот размер раздела определяется при создании с верхним пределом 5 ГБ.

## <a name="receive-messages-from-a-subscription"></a>Получение сообщений из подписки
Сообщения извлекаются из подписки с помощью метода `receiveSubscriptionMessage` для объекта **ServiceBusService**. По умолчанию прочитанные сообщения удаляются из подписки. Тем не менее можно присвоить необязательному параметру `isPeekLock` значение **true**, чтобы прочитать (просмотреть) и заблокировать сообщение, не удаляя его из подписки.

Поведение по умолчанию — чтение и удаление сообщения как часть операции получения — является простейшей моделью, оптимальной для сценариев, в которых приложение может не обрабатывать сообщение в случае сбоя. Чтобы понять это поведение, рассмотрим сценарий, в котором объект-получатель выдает запрос на получение и выходит из строя до его обработки. Так как служебная шина отмечает сообщение как использованное, перезапущенное приложение, начав снова использовать сообщения, пропустит сообщение, использованное до аварийного завершения работы.

Если параметр `isPeekLock` имеет значение **true**, получение становится двухэтапной операцией, что позволяет поддерживать приложения, не допускающие пропущенных сообщений. Получив запрос, служебная шина находит следующее сообщение, блокирует его, чтобы другие получатели не могли его принять, и возвращает его приложению.
Обработав сообщение (или сохранив его в надежном месте для последующей обработки), приложение вызывает метод **deleteMessage** и передает сообщение для удаления в качестве параметра, таким образом завершая второй этап процесса получения. Метод **deleteMessage** помечает сообщение как использованное и удаляет его из подписки.

В следующем примере показано, как получать и обрабатывать сообщения с помощью метода `receiveSubscriptionMessage`. В этом примере сначала поступает и удаляется сообщение из подписки LowMessages, затем поступает сообщение из подписки HighMessages при значении true, заданном для параметра `isPeekLock`. Затем сообщение удаляется с помощью метода `deleteMessage`.

```javascript
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        })
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Как обрабатывать сбои приложения и нечитаемые сообщения
служебная шина предоставляет функции, помогающие корректно выполнить восстановление после ошибок в приложении или трудностей, возникших при обработке сообщения. Если по какой-либо причине приложению-получателю не удается обработать сообщение, оно вызывает метод `unlockMessage` для объекта **ServiceBusService**. Применяя этот метод, служебная шина разблокирует сообщение в подписке и снова сделает его доступным для получения. В данном случае имеется в виду то же самое или другое потребляющее приложение.

Также есть время ожидания, связанное с заблокированным сообщением в пределах подписки. Если приложение не сможет обработать сообщение в течение времени ожидания (например, при сбое приложения), служебная шина автоматически разблокирует сообщение и снова сделает его доступным для получения.

Если в приложении происходит сбой после обработки сообщения, но перед вызовом метода `deleteMessage`, сообщение будет повторно доставлено в приложение после его перезапуска. Такая реакция на событие часто называется *по крайней мере одна обработка*. Это значит, что каждое приложение обрабатывается по крайней мере один раз, но в некоторых случаях одно и то же сообщение может быть доставлено повторно. Если повторная обработка недопустима, необходимо добавить логику для обработки повторной доставки сообщения. Для этого можно использовать свойство сообщения **MessageId**, которое остается постоянным в ходе разных попыток доставки.

## <a name="delete-topics-and-subscriptions"></a>Удаление разделов и подписок
Если не задано [свойство AutoDeleteOnIdle](https://docs.microsoft.com/javascript/api/azure-arm-sb/sbsubscription?view=azure-node-latest#autodeleteonidle), разделы и подписки хранятся постоянно, и их нужно удалять явным образом на [портале Azure][Azure portal] или с помощью программных средств.
В следующем примере показано, как удалить раздел с именем `MyTopic`.

```javascript
serviceBusService.deleteTopic('MyTopic', function (error) {
    if (error) {
        console.log(error);
    }
});
```

При удалении раздела также удаляются все подписки, зарегистрированные в этом разделе. Подписки также можно удалять по отдельности. В следующем примере показано, как удалить подписку с именем `HighMessages` из раздела `MyTopic`.

```javascript
serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
    if(error) {
        console.log(error);
    }
});
```

> [!NOTE]
> Вы можете управлять ресурсами служебной шины с помощью [обозревателя служебной шины](https://github.com/paolosalvatori/ServiceBusExplorer/). Обозреватель служебной шины позволяет без труда подключаться к пространству имен служебной шины и управлять сущностями обмена сообщениями. Средство предоставляет дополнительные возможности, например функции импорта и экспорта или возможность проверять разделы, очереди, подписки, службы ретрансляции, центры уведомлений и концентраторы событий. 

## <a name="next-steps"></a>Дополнительная информация
Вы узнали основные сведения о разделах служебной шины. Для получения дополнительных сведений используйте следующие ссылки.

* См. статью [Очереди, разделы и подписки][Queues, topics, and subscriptions].
* Справочник API для [SqlFilter][SqlFilter].
* Посетите репозиторий [Azure SDK for Node][Azure SDK for Node] на веб-сайте GitHub.

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Создание и развертывание приложения Node.js на веб-сайте Azure]: ../app-service/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md

