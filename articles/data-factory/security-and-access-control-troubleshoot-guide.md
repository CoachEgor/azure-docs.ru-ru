---
title: Устранение проблем безопасности и контроля доступа
description: Узнайте, как устранять проблемы безопасности и контроля доступа в фабрике данных Azure.
services: data-factory
author: lrtoyou1223
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 11/19/2020
ms.author: lle
ms.reviewer: craigg
ms.openlocfilehash: 51cb1a1a8151748fc9c6cd4c81da967424b52868
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/15/2020
ms.locfileid: "97505160"
---
# <a name="troubleshoot-azure-data-factory-security-and-access-control-issues"></a>Устранение неполадок, связанных с управлением безопасностью и доступом в фабрике данных Azure

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

В этой статье рассматриваются распространенные способы устранения неполадок безопасности и контроля доступа в фабрике данных Azure.

## <a name="common-errors-and-messages"></a>Распространенные ошибки и сообщения

### <a name="connectivity-issue-in-the-copy-activity-of-the-cloud-datastore"></a>Проблемы с подключением в действии копирования облачного хранилища данных

#### <a name="symptoms"></a>Симптомы

При возникновении проблем с подключением в хранилище данных источника или приемника могут возвращаться различные сообщения об ошибках.

#### <a name="cause"></a>Причина 

Проблема обычно вызвана одним из следующих факторов:

* Параметр прокси-сервера в узле среды выполнения интеграции (IR), если вы используете локальную среду IR.

* Параметр брандмауэра на автономном узле IR, если используется локальная среда IR.

* Параметр брандмауэра в облачном хранилище данных.

#### <a name="resolution"></a>Решение

* Чтобы убедиться в наличии проблемы с подключением, проверьте следующие моменты.

   - Ошибка возникает из источников или соединителей приемника.
   - Ошибка происходит в начале действия копирования.
   - Этот сбой согласуется с Azure IR или локальным IR с одним узлом, так как он может быть случайным сбоем в многоузловой среде IR, если проблема возникает только на некоторых узлах.

* Если используется локальная **Среда IR**, проверьте параметры прокси-сервера, брандмауэра и сети, так как подключение к тому же хранилищу данных может быть продолжено, если вы используете Azure IR. Сведения об устранении этой ситуации см. в следующих статьях:

   * [Локальные ИК-порты и брандмауэры](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#ports-and-firewalls)
   * [Соединитель Azure Data Lake Storage](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-store)
  
* Если вы используете **Azure IR**, попробуйте отключить параметр брандмауэра хранилища данных. Этот подход может устранить проблемы в следующих двух ситуациях.
  
   * [Azure IR IP-адреса](https://docs.microsoft.com/azure/data-factory/azure-integration-runtime-ip-addresses) отсутствуют в списке разрешений.
   * Функция *разрешить доверенным службам Майкрософт доступ к этой учетной записи хранения* отключена для [хранилища BLOB-объектов Azure](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#supported-capabilities) и [Azure Data Lake Storage Gen 2](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#supported-capabilities).
   * Параметр " *Разрешить доступ к службам Azure* " не включен для Azure Data Lake Storage 1-го поколения.

Если ни один из вышеперечисленных методов не работает, обратитесь в корпорацию Майкрософт за помощью.


### <a name="invalid-or-empty-authentication-key-issue-after-public-network-access-is-disabled"></a>Недействительная или пустая ошибка ключа проверки подлинности после отключения доступа к общедоступной сети

#### <a name="symptoms"></a>Симптомы

После отключения доступа к общедоступной сети для фабрики данных локальная среда выполнения интеграции выдает следующую ошибку: "ключ проверки подлинности недопустим или пуст".

#### <a name="cause"></a>Причина

Проблема, скорее всего, вызвана проблемой с разрешением системы доменных имен (DNS), так как отключение общего подключения и установка частной конечной точки не допускают повторного подключения.

Чтобы проверить, разрешено ли полное доменное имя (FQDN) фабрики данных на общедоступный IP-адрес, выполните следующие действия.

1. Убедитесь, что виртуальная машина Azure создана в той же виртуальной сети, что и частная конечная точка фабрики данных.

2. Выполните PsPing и выполните команду ping из виртуальной машины Azure в полное доменное имя фабрики данных:

   `psping.exe <dataFactoryName>.<region>.datafactory.azure.net:443`
   `ping <dataFactoryName>.<region>.datafactory.azure.net`

   > [!Note]
   > Необходимо указать порт для команды PsPing. Порт 443 показан здесь, но не является обязательным.

3. Проверьте, разрешены ли обе команды в общедоступный IP-адрес фабрики данных Azure, основанный на указанном регионе. IP-адрес должен иметь следующий формат: `xxx.xxx.xxx.0`

#### <a name="resolution"></a>Решение

Чтобы устранить эту проблему, выполните следующие действия.
- См. статью [Частная ссылка Azure для фабрики данных Azure](https://docs.microsoft.com/azure/data-factory/data-factory-private-link#dns-changes-for-private-endpoints) . Инструкция предназначена для настройки частной зоны DNS или сервера для разрешения полного доменного имени фабрики данных в частный IP-адрес.

- В качестве долгосрочного решения рекомендуется использовать пользовательский DNS-сервер. Однако если вы не хотите настраивать частную зону или сервер DNS, попробуйте следующее временное решение:

  1. Измените файл узла в Windows и сопоставьте частный IP-адрес (закрытую конечную точку фабрики данных Azure) с полным доменным именем фабрики данных Azure.
  
     На виртуальной машине Azure перейдите в `C:\Windows\System32\drivers\etc` , а затем откройте файл *узла* в блокноте. Добавьте строку, которая сопоставляет частный IP-адрес с полным доменным именем в конце файла, и сохраните изменения.
     
     ![Снимок экрана: сопоставление частного IP-адреса с узлом.](media/self-hosted-integration-runtime-troubleshoot-guide/add-mapping-to-host.png)

  1. Повторно выполните те же команды, что и в предыдущих шагах проверки, чтобы проверить ответ, который должен содержать частный IP-адрес.

  1. Повторно зарегистрируйте локальную среду выполнения интеграции, и проблема должна быть решена.

### <a name="unable-to-register-ir-authentication-key-on-self-hosted-vms-due-to-private-link"></a>Не удалось зарегистрировать ключ проверки подлинности IR на самостоятельно размещенных виртуальных машинах из-за частной ссылки

#### <a name="symptoms"></a>Симптомы

Не удается зарегистрировать ключ проверки подлинности IR на виртуальной машине, размещенной на собственном сервере, так как включена частная ссылка. Отображается следующее сообщение об ошибке:

"Не удалось получить токен службы из службы ADF с ключом * * * * * * * * * * * * * * * * и стоимость времени: 0,1250079 секунд код ошибки: Инвалидгатевайкэй, activityId: XXXXXXX и подробное сообщение об ошибке — IP-адрес клиента является недопустимым частный IP причина: фабрика данных не смогла получить доступ к общедоступной сети, поэтому она не может подключиться к облаку для успешного подключения."

#### <a name="cause"></a>Причина

Эта ошибка может быть вызвана виртуальной машиной, в которой вы пытаетесь установить локальную среду IR. Чтобы подключиться к облаку, убедитесь, что включен доступ к общедоступной сети.

#### <a name="resolution"></a>Решение

**Решение 1**
 
Чтобы устранить эту проблему, выполните следующие действия.

1. Перейдите на страницу [фабрики — обновление](https://docs.microsoft.com/rest/api/datafactory/Factories/Update) .

1. В правом верхнем углу нажмите кнопку **попробовать** .
1. В разделе **Параметры** заполните необходимые сведения. 
1. В разделе **текст** вставьте следующее свойство:

    ```
    { "tags": { "publicNetworkAccess":"Enabled" } }
    ```
1. Выберите **выполнить** , чтобы запустить функцию. 

1. В разделе **Параметры** заполните необходимые сведения. 

1. В разделе **текст** вставьте следующее свойство:
    ```
    { "tags": { "publicNetworkAccess":"Enabled" } }
    ``` 

1. Выберите **выполнить** , чтобы запустить функцию. 
1. Убедитесь, что отображается **код отклика: 200** . Вставленное свойство должно также отображаться в определении JSON.

1. Добавьте ключ проверки подлинности IR еще раз в среду выполнения интеграции.


**Решение 2**

Чтобы устранить эту проблему, перейдите в раздел " [Частная связь Azure" для фабрики данных Azure](https://docs.microsoft.com/azure/data-factory/data-factory-private-link).

Попробуйте включить доступ к общедоступной сети в пользовательском интерфейсе, как показано на следующем снимке экрана:

![Снимок экрана элемента управления "включено" для "разрешить доступ к общедоступной сети" в области "Сетевые подключения".](media/self-hosted-integration-runtime-troubleshoot-guide/enable-public-network-access.png)

## <a name="next-steps"></a>Дальнейшие действия

Для получения дополнительных сведений об устранении неполадок воспользуйтесь следующими ресурсами:

*  [Частная ссылка для фабрики данных](data-factory-private-link.md)
*  [Блог о Фабрике данных](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Запросы на добавление функции в Фабрику данных](https://feedback.azure.com/forums/270578-data-factory)
*  [Видео по Azure](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Microsoft Q&странице](/answers/topics/azure-data-factory.html)
*  [Форум о переполнении стека для фабрики данных](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Сведения о Фабрике данных в Twitter](https://twitter.com/hashtag/DataFactory)
