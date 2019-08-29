---
title: Использование разделов служебной шины Azure с Python | Документация Майкрософт
description: Узнайте, как использовать разделы и подписки служебной шины Azure в Python.
services: service-bus-messaging
documentationcenter: python
author: axisc
manager: timlt
editor: spelluru
ms.assetid: c4f1d76c-7567-4b33-9193-3788f82934e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 04/15/2019
ms.author: aschhab
ms.openlocfilehash: ef0237b38c8f640c0fc4b1b1788215c8804a5cd4
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2019
ms.locfileid: "70141900"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-python"></a>Как использовать разделы и подписки служебной шины с Python

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

В этой статье описывается использование разделов и подписок служебной шины. Примеры написаны на Python и используют [пакет SDK для Azure Python][Azure Python package]. К рассмотренным сценариям относятся:

- создание разделов и подписок; 
- создание фильтров подписки; 
- отправка сообщений в раздел; 
- получение сообщений из подписки;
- удаление разделов и подписок.

## <a name="prerequisites"></a>Предварительные требования
1. Подписка Azure. Для работы с этим учебником требуется учетная запись Azure. Вы можете активировать [преимущества Visual Studio или подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) или зарегистрироваться для использования [бесплатной учетной записи](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
2. Выполните действия, [описанные в кратком руководстве: Используйте портал Azure для создания раздела служебной шины и подписки на раздел](service-bus-quickstart-topics-subscriptions-portal.md) , чтобы создать **пространство имен** служебной шины и получить **строку подключения**.

    > [!NOTE]
    > Вы создадите **раздел** и подписку на раздел с помощью **Python** в этом кратком руководстве. 
3. Установите [пакет Azure Python][Azure Python package]. См. раздел [руководства по установке Python](/azure/python/python-sdk-azure-install).

## <a name="create-a-topic"></a>Создать раздел

Объект **ServiceBusService** позволяет работать с разделами. Добавьте следующий код в начало любого файла Python, из которого планируется получать доступ к служебной шине программным способом.

```python
from azure.servicebus.control_client import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

Следующий код создает объект **ServiceBusService**. Замените `mynamespace`, `sharedaccesskeyname` и `sharedaccesskey` своим пространством имен, именем и значением ключа подписанного URL-адреса (SAS).

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Получить значения для имени ключа SAS и значение можно на [портале Azure][Azure portal].

```python
bus_service.create_topic('mytopic')
```

Метод `create_topic` также поддерживает дополнительные параметры, позволяющие переопределить настройки раздела по умолчанию, такие как срок жизни сообщения или максимальный размер раздела. В следующем примере показано, как установить максимальный размер раздела 5 ГБ и значение срока жизни в одну минуту:

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Создание подписок

Подписки на разделы также создаются с помощью объекта **ServiceBusService**. Подписки имеют имена и могут использовать дополнительный фильтр, который ограничивает набор сообщений, доставляемых в виртуальную очередь подписки.

> [!NOTE]
> По умолчанию подписки остаются постоянными и будут существовать до тех пор, пока не будет удалена подписка или раздел, на который они подписаны.
> 
> Вы можете автоматически удалить подписки, задав [свойство auto_delete_on_idle](https://docs.microsoft.com/python/api/azure-mgmt-servicebus/azure.mgmt.servicebus.models.sbsubscription?view=azure-python).

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Создание подписки с фильтром по умолчанию (MatchAll)

Если при создании подписки не указан фильтр, то используется фильтр по умолчанию — **MatchAll**. Если используется фильтр **MatchAll**, то все сообщения, опубликованные в разделе, помещаются в виртуальную очередь подписки. В следующем примере создается подписка с именем `AllMessages` и используется фильтр по умолчанию **MatchAll**.

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Создание подписок с фильтрами

Вы также можете задать фильтры, позволяющие определять, какие отправляемые в раздел сообщения должны отображаться в рамках той или иной подписки.

Самый гибкий тип фильтра, который поддерживается подписками, — это **SqlFilter**, реализующий подмножество SQL92. Фильтры SQL работают со свойствами сообщений, которые опубликованы в разделе. Дополнительные сведения о выражениях, которые можно использовать с SQL-фильтром, см. в описании синтаксиса [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

Фильтры можно добавить в подписку с помощью метода **create\_rule** объекта **ServiceBusService**. Этот метод позволяет добавлять новые фильтры в существующую подписку.

> [!NOTE]
> Так как ко всем новым подпискам автоматически применяется фильтр по умолчанию, сначала необходимо удалить фильтр по умолчанию, иначе фильтр **MatchAll** переопределит поведение всех остальных заданных фильтров. Вы можете удалить правило по умолчанию с помощью метода `delete_rule` объекта **ServiceBusService**.
> 
> 

В следующем примере создается подписка `HighMessages`, содержащая фильтр **SqlFilter**, который выбирает только те сообщения, значение настраиваемого свойства `messagenumber` которых больше 3.

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Аналогичным образом в следующем примере создается подписка `LowMessages` с фильтром **SqlFilter**, который выбирает только те сообщения, значение настраиваемого свойства `messagenumber` которых меньше или равно 3.

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Теперь, когда в раздел `mytopic` отправляется сообщение, оно будет всегда доставляться получателям, подписанным на раздел **AllMessages**, и выборочно доставляться получателям, подписанным на разделы **HighMessages** и **LowMessages** в зависимости от содержания сообщения.

## <a name="send-messages-to-a-topic"></a>Отправка сообщений в раздел

Чтобы отправить сообщение в раздел служебной шины, приложение должно использовать метод `send_topic_message` объекта **ServiceBusService**.

В следующем примере показано, как отправить пять тестовых сообщений в `mytopic`. Значение свойства `messagenumber` каждого сообщения зависит от итерации цикла (определяет, какие подписки его получают).

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'),
                  custom_properties={'messagenumber': i})
    bus_service.send_topic_message('mytopic', msg)
```

Разделы служебной шины поддерживают максимальный размер сообщения 256 КБ для [уровня "Стандартный"](service-bus-premium-messaging.md) и 1 МБ для [уровня Premium](service-bus-premium-messaging.md). Максимальный размер заголовка, который содержит стандартные и настраиваемые свойства приложения, — 64 КБ. Ограничения на количество сообщений в разделе нет, но есть максимальный общий размер сообщений, содержащихся в разделе. Этот размер раздела определяется при создании с верхним пределом 5 ГБ. Дополнительные сведения о квотах см. в статье [Квоты на служебную шину][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Получение сообщений из подписки

Сообщения извлекаются из подписки с помощью метода `receive_subscription_message` для объекта **ServiceBusService**.

```python
msg = bus_service.receive_subscription_message(
    'mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Прочитанные сообщения удаляются из подписки, если для параметра `peek_lock` установлено значение **False**. Вы можете прочитать (извлечь) и заблокировать сообщение, не удаляя его из очереди, присвоив параметру `peek_lock` значение **True**.

Поведение по умолчанию — чтение и удаление сообщения как часть операции получения — является простейшей моделью, оптимальной для сценариев, в которых приложение может не обрабатывать сообщение в случае сбоя. Чтобы понять это поведение, рассмотрим сценарий, в котором объект-получатель выдает запрос на получение и выходит из строя до его обработки. Так как служебная шина отмечает сообщение как использованное, перезапущенное приложение, начав снова использовать сообщения, пропустит сообщение, использованное до аварийного завершения работы.

Если параметр `peek_lock` имеет значение **true**, получение становится операцией из двух этапов, что позволяет поддерживать приложения, неустойчивые к пропуску сообщений. Получив запрос, служебная шина находит следующее сообщение, блокирует его, чтобы предотвратить его получение другими получателями, и возвращает его приложению. Обработав сообщение (или сохранив его в надежном месте для последующей обработки), приложение завершает второй этап процесса получения, вызывая метод `delete` объекта **Message**. Метод `delete` помечает сообщение как использованное и удаляет его из подписки.

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
if msg.body is not None:
print(msg.body)
msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Как обрабатывать сбои приложения и нечитаемые сообщения

служебная шина предоставляет функции, помогающие корректно выполнить восстановление после ошибок в приложении или трудностей, возникших при обработке сообщения. Если по какой-либо причине приложению-получателю не удается обработать сообщение, оно вызывает метод `unlock` для объекта **Message**. После применения этого метода служебная шина разблокирует сообщение в подписке и снова сделает его доступным для получения тем же или другим приложением.

Кроме того, при блокировке сообщения в подписке предусмотрено определенное время ожидания. Если приложение не может обработать сообщение до истечения времени блокировки (например, аварийно завершит работу), то служебная шина автоматически разблокирует сообщение и оно станет доступным для повторного получения.

Если в приложении происходит сбой после обработки сообщения, но перед вызовом метода `delete`, сообщение будет повторно доставлено в приложение после его перезапуска. Часто такой подход называют По крайней мере\*один раз обработка каждого сообщения обрабатывается по крайней мере один раз, но в некоторых ситуациях такое же сообщение может быть доставлено снова. Если повторная обработка недопустима, разработчики приложения должны добавить дополнительную логику для обработки повторной доставки сообщений. Для этого вы можете использовать свойство сообщения **MessageId**, которое остается постоянным в ходе разных попыток доставки.

## <a name="delete-topics-and-subscriptions"></a>Удаление разделов и подписок

Разделы и подписки являются постоянными, если не задано [свойство auto_delete_on_idle](https://docs.microsoft.com/python/api/azure-mgmt-servicebus/azure.mgmt.servicebus.models.sbsubscription?view=azure-python) . Их можно удалить либо с помощью [портал Azure][Azure portal] , либо программным способом. В следующем примере показано, как удалить раздел с именем `mytopic`

```python
bus_service.delete_topic('mytopic')
```

При удалении раздела также удаляются все подписки, зарегистрированные в этом разделе. Подписки также можно удалять по отдельности. В следующем примере кода показано, как удалить подписку с именем `HighMessages` из раздела `mytopic`.

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

> [!NOTE]
> Вы можете управлять ресурсами служебной шины с помощью [обозревателя служебной шины](https://github.com/paolosalvatori/ServiceBusExplorer/). Обозреватель служебной шины позволяет без труда подключаться к пространству имен служебной шины и управлять сущностями обмена сообщениями. Средство предоставляет дополнительные возможности, например функции импорта и экспорта или возможность проверять разделы, очереди, подписки, службы ретрансляции, центры уведомлений и концентраторы событий. 

## <a name="next-steps"></a>Следующие шаги

Вы узнали основные сведения о разделах служебной шины. Для получения дополнительных сведений используйте следующие ссылки.

* См. статью [Очереди, разделы и подписки][Queues, topics, and subscriptions].
* Справочник по [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
