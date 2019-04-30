---
title: Использование разделов служебной шины (Ruby) | Документация Майкрософт
description: Узнайте, как использовать разделы и подписки служебной шины в Azure. Примеры кода написаны для приложений Ruby.
services: service-bus-messaging
documentationcenter: ruby
author: axisc
manager: timlt
editor: ''
ms.assetid: 3ef2295e-7c5f-4c54-a13b-a69c8045d4b6
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 04/15/2019
ms.author: aschhab
ms.openlocfilehash: c2862c3521c05ba81f7a567f7951d22a9ab95c44
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61472341"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-ruby"></a>Как использовать разделы и подписки служебной шины с Ruby
 
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

В этой статье показано, как использовать разделы и подписки служебной шины в приложениях Ruby. К рассмотренным сценариям относятся:

- создание разделов и подписок; 
- создание фильтров подписки; 
- отправка сообщений в раздел; 
- получение сообщений из подписки;
- удаление разделов и подписок.


## <a name="prerequisites"></a>Технические условия
1. Подписка Azure. Для работы с этим учебником требуется учетная запись Azure. Вы можете активировать ваши [преимущества для подписчиков Visual Studio или MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) или зарегистрируйте [бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
2. Выполните шаги [краткое руководство: Чтобы создать раздел служебной шины и подписок для раздела с помощью портала Azure](service-bus-quickstart-topics-subscriptions-portal.md) для создания служебной шины **пространства имен** и получить **строку подключения**. 

    > [!NOTE]
    > Вы создадите **разделе** и **подписки** в раздел с помощью **Ruby** в этом кратком руководстве. 

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="create-a-topic"></a>Создать раздел
Объект **Azure::ServiceBusService** позволяет работать с разделами. Следующий код создает объект **Azure::ServiceBusService**. Чтобы создать раздел, используйте метод `create_topic()`. В следующем примере создается раздел или выводится ошибка, если она возникла.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  topic = azure_service_bus_service.create_topic("test-topic")
rescue
  puts $!
end
```

Также можно передать объект **Azure::ServiceBus::Topic** с дополнительными параметрами, которые позволяют переопределить параметры раздела по умолчанию, например срок жизни сообщения или максимальный размер очереди. В следующем примере показано, как установить максимальный размер очереди 5 ГБ и срок жизни в 1 минуту.

```ruby
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Создание подписок
Подписки разделов также создаются с помощью объекта **Azure::ServiceBusService**. Подписки имеют имена и могут использовать дополнительный фильтр, который ограничивает набор сообщений, доставляемых в виртуальную очередь подписки.

Подписки являются постоянными. Они существуют либо до их удаления, либо до удаления раздела, с которым они связаны. Если приложение содержит логику для создания подписки, оно сначала должно проверить, существует ли подписка, используя метод getSubscription.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Создание подписки с фильтром по умолчанию (MatchAll)
Если при создании подписки не указан фильтр, то используется фильтр по умолчанию — **MatchAll**. Если используется фильтр **MatchAll**, то все сообщения, опубликованные в разделе, помещаются в виртуальную очередь подписки. В следующем примере создается подписка с именем "all-messages" и используется фильтр по умолчанию **MatchAll**.

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Создание подписок с фильтрами
Можно также задавать фильтры, позволяющие определять, какие посылаемые в раздел сообщения должны отображаться в рамках конкретной подписки.

Самый гибкий тип фильтра, который поддерживается подписками, — это **Azure::ServiceBus::SqlFilter**, реализующий подмножество SQL92. Фильтры SQL работают со свойствами сообщений, которые опубликованы в разделе. Дополнительные сведения о выражениях, которые можно использовать с фильтром SQL, см. в описании синтаксиса [SqlFilter](service-bus-messaging-sql-filter.md).

Фильтры можно добавить в подписку с помощью метода `create_rule()` объекта **Azure::ServiceBusService**. Этот метод позволяет добавлять новые фильтры в существующую подписку.

Так как ко всем новым подпискам автоматически применяется фильтр по умолчанию, сначала необходимо удалить фильтр по умолчанию, иначе **MatchAll** будет действовать вместо любых других заданных фильтров. Вы можете удалить правило по умолчанию с помощью метода `delete_rule()` объекта **Azure::ServiceBusService**.

В приведенном ниже примере создается подписка high-messages с фильтром **Azure::ServiceBus::SqlFilter**, который выбирает только те сообщения, значение настраиваемого свойства `message_number` которых больше 3.

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Аналогичным образом в следующем примере создается подписка `low-messages` с фильтром **Azure::ServiceBus::SqlFilter**, который выбирает только те сообщения, значение свойства `message_number` которых меньше или равно 3.

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Если сообщение отправляется в раздел `test-topic`, оно всегда будет доставляться в приемники, подписанные на подписку раздела `all-messages`, и в отдельные приемники, подписанные на подписки разделов `high-messages` и `low-messages` (в зависимости от содержимого сообщения).

## <a name="send-messages-to-a-topic"></a>Отправка сообщений в раздел
Чтобы отправить сообщение в раздел служебной шины, приложение должно использовать метод `send_topic_message()` объекта **Azure::ServiceBusService**. Сообщения, отправленные в разделы служебной шины, представляют собой экземпляры объектов **Azure::ServiceBus::BrokeredMessage**. Объекты **Azure::ServiceBus::BrokeredMessage** содержат набор стандартных свойств (например, `label` и `time_to_live`), словарь, используемый для хранения настраиваемых свойств приложения, и текст из строковых данных. Приложение может задавать текст сообщения, передавая строковое значение в метод `send_topic_message()`, и все обязательные стандартные свойства будут заполнены значениями по умолчанию.

В следующем примере показано, как отправить пять тестовых сообщений в `test-topic`. Значение настраиваемого свойства `message_number` каждого сообщения зависит от итерации цикла (определяет подписку, которая получит это сообщение).

```ruby
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Разделы служебной шины поддерживают максимальный размер сообщения 256 КБ для [уровня "Стандартный"](service-bus-premium-messaging.md) и 1 МБ для [уровня Premium](service-bus-premium-messaging.md). Максимальный размер заголовка, который содержит стандартные и настраиваемые свойства приложения, — 64 КБ. Ограничения на количество сообщений в разделе нет, но есть максимальный общий размер сообщений, содержащихся в разделе. Этот размер раздела определяется при создании с верхним пределом 5 ГБ.

## <a name="receive-messages-from-a-subscription"></a>Получение сообщений из подписки
Сообщения извлекаются из подписки с помощью метода `receive_subscription_message()` объекта **Azure::ServiceBusService**. По умолчанию сообщения считываются (просматриваются) и блокируются без удаления из подписки. Сообщения можно считывать и удалять из подписки, установив для параметра `peek_lock` значение **false**.

По умолчанию чтение и удаление выполняются в два этапа, что позволяет поддерживать приложения, не способные обрабатывать отсутствующие сообщения. Получив запрос, служебная шина находит следующее сообщение, блокирует его, чтобы предотвратить его получение другими получателями, и возвращает его приложению. Обработав сообщение (или сохранив его в надежном месте для последующей обработки), приложение вызывает метод `delete_subscription_message()` и указывает сообщение, которое будет удалено как параметр, таким образом завершая второй этап процесса получения. Метод `delete_subscription_message()` помечает сообщение как использованное и удаляет его из подписки.

Если параметр `:peek_lock` имеет значение **false**, чтение и удаление сообщения осуществляется очень просто. Этот метод оптимально подходит для сценариев, в которых приложение может пропустить обработку сообщения без последствий в случае сбоя. Рассмотрим сценарий, в котором объект-получатель выдает запрос на получение и аварийно завершается до его обработки. Так как служебная шина отмечает сообщение как использованное, перезапущенное приложение, начав снова использовать сообщения, пропустит сообщение, использованное до аварийного завершения работы.

В следующем примере показано, как получать и обрабатывать сообщения с помощью метода `receive_subscription_message()`. В примере сначала поступает и удаляется сообщение из подписки `low-messages` с помощью метода `:peek_lock`, для которого задано значение **false**, затем поступает другое сообщение из подписки `high-messages` и удаляется с помощью метода `delete_subscription_message()`.

```ruby
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Как обрабатывать сбои приложения и нечитаемые сообщения
служебная шина предоставляет функции, помогающие корректно выполнить восстановление после ошибок в приложении или трудностей, возникших при обработке сообщения. Если по какой-либо причине приложению-получателю не удается обработать сообщение, оно вызывает метод `unlock_subscription_message()` для объекта **Azure::ServiceBusService**. После этого служебная шина разблокирует сообщение в подписке и снова сделает его доступным для получения тем же или другим приложением.

Кроме того, при блокировке сообщения в подписке предусмотрено определенное время ожидания. Если приложение не может обработать сообщение до истечения времени блокировки (например, аварийно завершит работу), то служебная шина автоматически разблокирует сообщение и оно станет доступным для повторного получения.

Если в приложении происходит сбой после обработки сообщения, но перед вызовом метода `delete_subscription_message()`, сообщение будет повторно доставлено в приложение после его перезапуска. Часто такой подход называют *Обработать хотя бы один раз*, т. е. каждое сообщение будет обрабатываться по крайней мере один раз, но в некоторых случаях это же сообщение может доставляться повторно. Если повторная обработка недопустима, разработчики приложения должны добавить дополнительную логику для обработки повторной доставки сообщений. Как правило, эта логика достигается с помощью свойства `message_id` сообщения, которое остается постоянным для различных попыток доставки.

## <a name="delete-topics-and-subscriptions"></a>Удаление разделов и подписок
Разделы и подписки хранятся постоянно, и их нужно удалять явным образом на [портале Azure][Azure portal] или с помощью программных средств. В следующем примере показано, как удалить раздел с именем `test-topic`.

```ruby
azure_service_bus_service.delete_topic("test-topic")
```

При удалении раздела также удаляются все подписки, зарегистрированные в этом разделе. Подписки также можно удалять по отдельности. В следующем примере кода показано, как удалить подписку с именем `high-messages` из раздела `test-topic`.

```ruby
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Дальнейшие действия
Вы узнали основные сведения о разделах служебной шины. Для получения дополнительных сведений используйте следующие ссылки.

* См. статью [Очереди, разделы и подписки](service-bus-queues-topics-subscriptions.md).
* Справочник API для [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter).
* Посетите репозиторий [Azure SDK для Ruby](https://github.com/Azure/azure-sdk-for-ruby) на веб-сайте GitHub.

[Azure portal]: https://portal.azure.com
