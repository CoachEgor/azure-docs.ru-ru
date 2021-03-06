---
title: Настройка сохраняемости данных — кэш Azure уровня Premium для Redis
description: Узнайте, как настроить постоянное хранение и управлять им для экземпляров кэша Redis для Azure ценовой категории "Премиум"
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 10/09/2020
ms.openlocfilehash: 8ae76ca27c8c6f8fed5692b9a2376fff53a52bb6
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2020
ms.locfileid: "92536578"
---
# <a name="how-to-configure-data-persistence-for-a-premium-azure-cache-for-redis"></a>Настройка постоянного хранения данных для кэша Redis для Azure ценовой категории "Премиум"
Из этой статьи вы узнаете, как настроить сохраняемость в кэше Azure уровня "Премиум" для Redis с помощью портал Azure. Кэш Azure для Redis предлагает разные варианты кэша, которые позволяют выбирать размер и функции кэша, включая функции ценовой категории "Премиум", такие как кластеризация, постоянное хранение данных и поддержка виртуальной сети. 

## <a name="what-is-data-persistence"></a>Что такое постоянное хранение?
Функция [Сохраняемость Redis](https://redis.io/topics/persistence) позволяет постоянно хранить данные, размещенные в Redis. Также можно создавать моментальные снимки и архивировать данные, которые можно будет восстановить в случае сбоя оборудования. Это огромное преимущество по сравнению с уровнями Базовый и Стандартный, в которых все данные хранятся в памяти, и при сбое, когда узлы кэша становятся недоступными, может произойти потеря данных. 

Кэш Redis для Azure обеспечивает сохраняемость Redis на основе следующих моделей:

* **Сохраняемость RDB.** Если настроена сохраняемость RDB (база данных Redis), кэш Redis для Azure сохраняет моментальный снимок кэша Redis для Azure на диске в двоичном формате Redis в соответствии с настроенной частотой резервного копирования. В случае аварии, при которой становятся недоступными как основной экземпляр кэша, так и реплика кэша, кэш воссоздается на основе последнего моментального снимка. Узнайте больше о [преимуществах](https://redis.io/topics/persistence#rdb-advantages) и [недостатках](https://redis.io/topics/persistence#rdb-disadvantages) сохраняемости RDB.
* **Сохраняемость AOF.** Если настроена сохраняемость AOF (файл только для добавления), кэш Redis для Azure сохраняет каждую операцию записи в журнал, который сохраняется по крайней мере один раз в секунду в учетную запись хранения Azure. В случае аварии, при которой становятся недоступными основной экземпляр и реплика кэша, кэш воссоздается на основе сохраненных операций записи. Узнайте больше о [преимуществах](https://redis.io/topics/persistence#aof-advantages) и [недостатках](https://redis.io/topics/persistence#aof-disadvantages) сохраняемости AOF.

Сохраняемость записывает данные Redis в учетную запись хранения Azure, которой вы владеете и управляете. Вы можете настроить из **новой колонки кэш Azure для Redis** во время создания кэша и в **меню ресурсов** для существующих кэшей уровня "Премиум".

> [!NOTE]
> 
> Служба хранилища Azure автоматически шифрует данные при их сохранении. Для шифрования можно использовать собственные ключи. Дополнительные сведения см. [в разделе ключи, управляемые клиентом, с помощью Azure Key Vault](../storage/common/storage-service-encryption.md).
> 
> 

1. Чтобы создать кэш уровня "Премиум", войдите в [портал Azure](https://portal.azure.com) и выберите **создать ресурс**. Кэши можно создавать не только на портале Azure, но и с помощью шаблонов Resource Manager, PowerShell или интерфейса командной строки Azure. Дополнительные сведения о создании кэша Azure для Redis см. в разделе [Создание кэша](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

    :::image type="content" source="media/cache-private-link/1-create-resource.png" alt-text="Создать ресурс.":::
   
2. На странице **Создание** выберите **Базы данных** , а затем **Кэш Azure для Redis**.

    :::image type="content" source="media/cache-private-link/2-select-cache.png" alt-text="Выбор элемента &quot;Кэш Azure для Redis&quot;.":::

3. На странице **новый кэш Redis** настройте параметры для нового кэша Premium.
   
   | Параметр      | Рекомендуемое значение  | Description |
   | ------------ |  ------- | -------------------------------------------------- |
   | **DNS-имя** | Введите глобально уникальное имя | Имя кэша должно быть строкой от 1 до 63 символов, содержащих только цифры, буквы или дефисы. Имя должно начинаться и заканчиваться цифрой или буквой и не может содержать более одного дефиса подряд. *Имя узла* для экземпляра кэша получит значение *\<DNS name>.redis.cache.windows.net*. | 
   | **подписка** | Раскрывающийся список и выберите свою подписку. | В этой подписке будет создан новый экземпляр кэша Redis для Azure. | 
   | **Группа ресурсов** | Выберите группу ресурсов или щелкните **создать** и введите новое имя группы ресурсов. | Имя группы ресурсов, в которой будут созданы кэш и другие ресурсы. Поместив все ресурсы приложения в одну группу ресурсов, вы сможете легко управлять ими и/или удалить их вместе. | 
   | **Местоположение** | Раскрывающийся список и выберите расположение. | Выберите оптимальный [регион](https://azure.microsoft.com/regions/) для других служб, которые будут использовать кэш. |
   | **Тип кэша** | И выберите кэш уровня "Премиум", чтобы настроить функции уровня "Премиум". Дополнительные сведения см. в статье о [ценах на кэш Azure для Redis](https://azure.microsoft.com/pricing/details/cache/). |  Ценовая категория определяет размер, производительность и функции, доступные для кэша. Дополнительные сведения см. в [обзоре предложения "Кэш Redis для Azure"](cache-overview.md). |

4. Выберите вкладку **Сети** или нажмите кнопку **Сети** в нижней части страницы.

5. На вкладке **Сети** выберите способ подключения. Для экземпляров кэша уровня "Премиум" можно подключаться к публичному, через общедоступные IP-адреса или конечные точки службы или в частном порядке с помощью частной конечной точки.

6. Нажмите кнопку **Далее: дополнительно** в нижней части страницы или выберите вкладку **Далее: дополнительно**.

7. На вкладке **Дополнительно** для экземпляра кэша уровня "Премиум" настройте параметры для неtls-порта, кластеризации и сохраняемости данных. Для сохраняемости данных можно выбрать сохраняемость или **RDB** **AOF** . 

8. Чтобы включить сохранение RDB, щелкните **RDB** и настройте параметры. 
   
   | Параметр      | Рекомендуемое значение  | Описание |
   | ------------ |  ------- | -------------------------------------------------- |
   | **Периодичность резервного копирования** | Раскрывающийся список и выберите интервал резервного копирования, включая **15 минут** , **30 минут** , **60 минут** , **6 часов** , **12 часов** и **24 часа**. | Отсчет этого интервала начинается после успешного завершения предыдущей операции резервного копирования. По истечении этого интервала запускается новое резервное копирование. | 
   | **Учетная запись хранения** | Раскрывающийся список и выберите свою учетную запись хранения. | Необходимо выбрать учетную запись хранения в том же регионе и подписке, что и кэш, и рекомендуется использовать учетную запись **хранения класса Premium** , так как хранилище класса Premium имеет более высокую пропускную способность.  | 
   | **Storage Key (Ключ хранилища)** | Раскрывающийся список и выберите **первичный** или **вторичный ключ** для использования. | Если ключ к хранилищу данных для учетной записи сохраняемости создается заново, то необходимо повторно настроить желаемый ключ в раскрывающемся списке **Ключ к хранилищу данных**. | 

    Первая резервная копия инициируется по истечении интервала периодичности резервного копирования.

9. Чтобы включить сохраняемость AOF, щелкните **AOF** и настройте параметры. 
   
   | Параметр      | Рекомендуемое значение  | Описание |
   | ------------ |  ------- | -------------------------------------------------- |
   | **Первая учетная запись хранения** | Раскрывающийся список и выберите свою учетную запись хранения. | Эта учетная запись хранения должна находиться в том же регионе и подписке, что и кэш, и рекомендуется использовать учетную запись **хранения класса Premium** , так как хранилище класса Premium имеет более высокую пропускную способность | 
   | **Первый ключ к хранилищу данных** | Раскрывающийся список и выберите **первичный** или **вторичный ключ** для использования. | Если ключ к хранилищу данных для учетной записи сохраняемости создается заново, то необходимо повторно настроить желаемый ключ в раскрывающемся списке **Ключ к хранилищу данных**. | 
   | **Вторая учетная запись хранения** | Используемых И выберите дополнительную учетную запись хранения. | При необходимости можно настроить дополнительную учетную запись хранения. Если настроена вторичная учетная запись хранения, операции записи в кэш реплики сохраняются в нее. | 
   | **Второй ключ хранилища** | Используемых Раскрывающийся список и выберите **первичный** или **вторичный ключ** для использования. | Если ключ к хранилищу данных для учетной записи сохраняемости создается заново, то необходимо повторно настроить желаемый ключ в раскрывающемся списке **Ключ к хранилищу данных**. | 

    Если включена сохраняемость AOF, операции записи в кэш сохраняются в указанную учетную запись хранения (или учетные записи, если вы настроили вторичную учетную запись хранения). В случае масштабного сбоя, в результате которого выходят из строя основной экземпляр и реплика кэша, кэш восстанавливается на основе сохраненного журнала AOF.

10. Нажмите кнопку **Далее: теги** или нажмите кнопку **Далее: теги** в нижней части страницы.

11. При необходимости на вкладке **Теги** введите имя и значение, чтобы классифицировать ресурс. 

12. Выберите **Проверить и создать**. Вы будете перенаправлены на вкладку "Просмотр и создание", где Azure проверит вашу конфигурацию.

13. Когда отобразится сообщение "Проверка пройдена" зеленого цвета, выберите **Создать**.

На создание кэша требуется некоторое время. Вы можете отслеживать ход выполнения на странице **обзорных сведений** кэша Azure для Redis. Когда **Состояние** примет значение **Running** (Выполняется), кэш будет готов к использованию. 

## <a name="persistence-faq"></a>Часто задаваемые вопросы о постоянном хранении
Следующий список содержит ответы на часто задаваемые вопросы о постоянном хранении данных для кэша Redis для Azure.

* [Можно ли включить постоянное хранение для ранее созданного кэша?](#can-i-enable-persistence-on-a-previously-created-cache)
* [Можно ли одновременно активировать сохраняемость AOF и RDB?](#can-i-enable-aof-and-rdb-persistence-at-the-same-time)
* [Какую модель сохраняемости следует выбрать?](#which-persistence-model-should-i-choose)
* [Что произойдет, если выполнено масштабирование до другого размера, а резервная копия была создана до этой операции?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)
* [Можно ли использовать одну и ту же учетную запись хранения для сохранения данных в двух разных кэшах?](#can-i-use-the-same-storage-account-for-persistence-across-two-different-caches)


### <a name="rdb-persistence"></a>Сохраняемость RDB
* [Можно ли изменить частоту резервного копирования RDB после создания кэша?](#can-i-change-the-rdb-backup-frequency-after-i-create-the-cache)
* [Почему при установленной частоте резервного копирования RDB в 60 минут между созданием резервных копий проходит больше времени?](#why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
* [Что происходит со старыми резервными копиями RDB при создании другой?](#what-happens-to-the-old-rdb-backups-when-a-new-backup-is-made)

### <a name="aof-persistence"></a>Сохраняемость AOF
* [Когда следует использовать вторичную учетную запись хранения?](#when-should-i-use-a-second-storage-account)
* [Влияет ли сохраняемость AOF на пропускную способность, задержку или производительность кэша?](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)
* [Как удалить вторичную учетную запись хранения?](#how-can-i-remove-the-second-storage-account)
* [Что такое перезапись и как она влияет на кэш?](#what-is-a-rewrite-and-how-does-it-affect-my-cache)
* [Что следует ожидать при масштабировании кэша с включенной сохраняемостью AOF?](#what-should-i-expect-when-scaling-a-cache-with-aof-enabled)
* [Как организованы данные AOF в хранилище?](#how-is-my-aof-data-organized-in-storage)


### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Можно ли включить постоянное хранение для ранее созданного кэша?
Да, вы можете настроить сохраняемость Redis как при создании кэша, так и в уже существующем кэше Премиум.

### <a name="can-i-enable-aof-and-rdb-persistence-at-the-same-time"></a>Можно ли одновременно активировать сохраняемость AOF и RDB?

Нет. Вы можете включить только сохраняемость RDB или AOF.

### <a name="which-persistence-model-should-i-choose"></a>Какую модель сохраняемости следует выбрать?

Сохраняемость AOF сохраняет каждую запись в журнал, что влияет на пропускную способность, а сохраняемость RDB сохраняет резервные копии на основе настроенного интервала резервного копирования, что оказывает минимальное влияние на производительность. Выберите сохраняемость AOF, если основная цель заключается в минимизации потери данных, а уменьшение пропускной способности кэша не является проблемой. Используйте сохраняемость RDB, если вы хотите поддерживать оптимальную пропускную способность в кэше, но при этом необходим механизм восстановления данных.

* Узнайте больше о [преимуществах](https://redis.io/topics/persistence#rdb-advantages) и [недостатках](https://redis.io/topics/persistence#rdb-disadvantages) сохраняемости RDB.
* Узнайте больше о [преимуществах](https://redis.io/topics/persistence#aof-advantages) и [недостатках](https://redis.io/topics/persistence#aof-disadvantages) сохраняемости AOF.

Дополнительные сведения о производительности при использовании сохраняемости AOF см. в разделе [Влияет ли сохраняемость AOF на пропускную способность, задержку или производительность кэша?](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)

### <a name="what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation"></a>Что произойдет, если выполнено масштабирование до другого размера, а резервная копия была создана до этой операции?

Сохраняемость RDB и AOF:

* Если выполнено масштабирование до большего размера, то ничего не произойдет.
* Если выполнено масштабирование до меньшего размера, а значение существующего пользовательского параметра [databases](cache-configure.md#databases) превышает значение [databases limit](cache-configure.md#databases) для нового размера, то данные в этих базах данных не будут восстановлены. Дополнительные сведения см. в разделе [Что происходит с пользовательским параметром databases при масштабировании?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
* Если выполнено масштабирование до меньшего размера, и вам не хватает места для хранения всех данных из последнего архива, то при восстановлении ключи будут исключены. Обычно для этого используется политика вытеснения [allkeys-lru](https://redis.io/topics/lru-cache).

### <a name="can-i-use-the-same-storage-account-for-persistence-across-two-different-caches"></a>Можно ли использовать одну и ту же учетную запись хранения для сохранения данных в двух разных кэшах?
Да, вы можете использовать одну и ту же учетную запись хранения для сохранения данных в двух разных кэшах.

### <a name="can-i-change-the-rdb-backup-frequency-after-i-create-the-cache"></a>Можно ли изменить частоту резервного копирования RDB после создания кэша?
Да, вы можете изменить частоту резервного копирования для сохранения RDB в колонке " **сохраняемость данных** ". Инструкции см. в разделе "Настройка сохраняемости Redis".

### <a name="why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Почему при установленной частоте резервного копирования RDB в 60 минут между созданием резервных копий проходит больше времени?
Интервал резервного копирования сохраняемости RDB не начинается, пока не завершится процесс предыдущего резервного копирования. Если интервал резервного копирования 60 минут и на процесс резервного копирования уходит 15 минут, следующее резервное копирование начнется не ранее чем через 75 минут после начала предыдущего резервного копирования.

### <a name="what-happens-to-the-old-rdb-backups-when-a-new-backup-is-made"></a>Что происходит со старыми резервными копиями RDB при создании другой?
Все резервные копии сохраняемости RDB, за исключением последней, автоматически удаляются. Это удаление может происходить не сразу, но старые резервные копии не хранятся в течение неограниченного периода времени.


### <a name="when-should-i-use-a-second-storage-account"></a>Когда следует использовать вторичную учетную запись хранения?

Вторичную учетную запись хранения для сохраняемости AOF следует использовать, если вы считаете, что в кэше выполняется больше операций над множеством, чем ожидалось.  Это гарантирует, что ваш кеш не достигнет ограничений пропускной способности.

### <a name="does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache"></a>Влияет ли сохраняемость AOF на пропускную способность, задержку или производительность кэша?

Сохраняемость AOF влияет на пропускную способность примерно на 15–20% при нагрузке кэша ниже максимальной (при нагрузке ЦП и сервера менее 90%). Если кэш остается в рамках этих ограничений, проблемы с задержками не должны возникать. Однако если включена сохраняемость AOF, кэш достигнет этих ограничений быстрее.

### <a name="how-can-i-remove-the-second-storage-account"></a>Как удалить вторичную учетную запись хранения?

Чтобы удалить вторичную учетную запись хранения сохраняемости AOF, установите одну учетную запись хранения в качестве основной и вторичной. Для существующих кэшей доступ к колонке **сохраняемости данных** осуществляется из **меню ресурсов** для кэша. Чтобы отключить сохраняемость AOF, щелкните **отключено**.

### <a name="what-is-a-rewrite-and-how-does-it-affect-my-cache"></a>Что такое перезапись и как она влияет на кэш?

Если файл AOF достигает достаточно большого размера, перезапись автоматически ставится в очередь кэша. Этот процесс изменяет размер файла AOF с минимальным набором операций, необходимых для создания текущего набора данных. Во время операций перезаписи следует ожидать быстрого достижения ограничения производительности, особенно при работе с большими наборами данных. Операции перезаписи возникают реже по мере увеличения файла AOF, но при этом занимают значительное количество времени.

### <a name="what-should-i-expect-when-scaling-a-cache-with-aof-enabled"></a>Что следует ожидать при масштабировании кэша с включенной сохраняемостью AOF?

Если файл AOF во время масштабирования значительно увеличивается в размере, следует ожидать, что операция масштабирования займет больше времени, чем ожидалось, так как после завершения масштабирования файл перезагружается.

Дополнительные сведения см. в разделе [Что произойдет, если выполнено масштабирование до другого размера, а резервная копия была создана до этой операции?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="how-is-my-aof-data-organized-in-storage"></a>Как организованы данные AOF в хранилище?

Данные, хранящиеся в файлах AOF, делятся на несколько страничных BLOB-объектов на узел. Это позволяет увеличить производительность сохранения данных в хранилище. В таблице ниже показано, сколько страничных BLOB-объектов используются в каждой ценовой категории.

| Уровень Премиум | BLOB-объекты |
|--------------|-------|
| P1           | 4 на сегмент    |
| P2           | 8 на сегмент    |
| P3           | 16 на сегмент   |
| P4           | 20 на сегмент   |

Если включена кластеризация, каждый сегмент в кэше имеет свой собственный набор страничных BLOB-объектов, как показано в предыдущей таблице. Например, кэш P2 с тремя сегментами распределяет файл AOF по 24 страничным BLOB-объектам (8 BLOB-объектов на сегмент, 3 сегменты).

После перезаписи в хранилище создается два набора файлов AOF. Операции перезаписи выполняются в фоновом режиме и добавляются к первому набору файлов, а операции над множеством, которые отправляются в кэш во время перезаписи, добавляются ко второму набору. Во время перезаписи на случай сбоя временно сохраняется резервная копия, которая сразу же удаляется после завершения перезаписи.


## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о кэше Azure для функций Redis.

* [Кэш Azure для уровней службы Redis Premium](cache-overview.md#service-tiers)

<!-- IMAGES -->

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-rdb-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-rdb-persistence.png

[redis-cache-aof-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-aof-persistence.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png