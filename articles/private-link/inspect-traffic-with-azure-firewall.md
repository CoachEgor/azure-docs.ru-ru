---
title: Использование брандмауэра Azure для проверки трафика, предназначенного для частной конечной точки
titleSuffix: Azure Private Link
description: Узнайте, как проверить трафик, предназначенный для частной конечной точки, с помощью брандмауэра Azure.
services: private-link
author: jocortems
ms.service: private-link
ms.topic: how-to
ms.date: 09/02/2020
ms.author: allensu
ms.openlocfilehash: 5cbfd90ca65a1fb75c9cbe5602ac2a69741e378f
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "96017242"
---
# <a name="use-azure-firewall-to-inspect-traffic-destined-to-a-private-endpoint"></a>Использование брандмауэра Azure для проверки трафика, предназначенного для частной конечной точки

Частная конечная точка Azure является фундаментальным стандартным блоком для частной связи Azure. Частные конечные точки позволяют ресурсам Azure, развернутым в виртуальной сети, обмениваться данными с частными ресурсами.

Частные конечные точки позволяют ресурсам обращаться к службе закрытых ссылок, развернутой в виртуальной сети. Доступ к частной конечной точке через пиринг виртуальных сетей и локальные сетевые подключения расширяют возможности подключения.

Может потребоваться проверка или блокировка трафика от клиентов к службам, предоставляемым через частные конечные точки. Пройдите эту проверку с помощью [брандмауэра Azure](../firewall/overview.md) или виртуального сетевого модуля стороннего производителя.

Действительны следующие ограничения.

* Группы безопасности сети (группы безопасности сети) не применяются к частным конечным точкам
* Определяемые пользователем маршруты (UDR) не применяются к частным конечным точкам
* Одна таблица маршрутов может быть присоединена к подсети.
* Таблица маршрутов поддерживает до 400 маршрутов.

Брандмауэр Azure фильтрует трафик одним из следующих:

* [Полное доменное имя в правилах сети](../firewall/fqdn-filtering-network-rules.md) для протоколов TCP и UDP
* [Полное доменное имя в правилах приложений](../firewall/features.md#application-fqdn-filtering-rules) для HTTP, HTTPS и MSSQL. 

Большинство служб, предоставляемых закрытыми конечными точками, используют протокол HTTPS. При использовании Azure SQL рекомендуется использовать правила приложений по правилам сети.

> [!NOTE]
> Фильтрация полных доменных имен SQL поддерживается только в [прокси-режиме](../azure-sql/database/connectivity-architecture.md#connection-policy) (порт 1433). Режим **прокси-сервера** может привести к увеличению задержки по сравнению с *перенаправлением*. Если вы хотите продолжить использовать режим перенаправления, который используется по умолчанию для клиентов, подключающихся в Azure, можно отфильтровать доступ с помощью полного доменного имени в правилах сети брандмауэра.

## <a name="scenario-1-hub-and-spoke-architecture---dedicated-virtual-network-for-private-endpoints"></a>Сценарий 1. Выделенная виртуальная сеть для частных конечных точек в архитектуре "звезда"

:::image type="content" source="./media/inspect-traffic-using-azure-firewall/hub-and-spoke.png" alt-text="Выделенная виртуальная сеть для частных конечных точек" border="true":::

Этот сценарий является самой расширяемой архитектурой для частного подключения к нескольким службам Azure с помощью частных конечных точек. Маршрут, указывающий на адресное пространство сети, в котором развернуты частные конечные точки. Такая конфигурация сокращает затраты на администрирование и предотвращает ограничение в 400 маршрутов.

Если виртуальные сети являются равноправными, то при подключении из виртуальной сети клиента к брандмауэру Azure в виртуальной сети будет взиматься плата.

Дополнительные сведения о расходах, связанных с подключениями к одноранговым виртуальным сетям, см. в разделе часто задаваемых вопросов на странице [цен](https://azure.microsoft.com/pricing/details/private-link/) .

>[!NOTE]
> Этот сценарий можно реализовать с помощью любых правил сети сторонних NVA или брандмауэра Azure вместо правил приложений.

## <a name="scenario-2-hub-and-spoke-architecture---shared-virtual-network-for-private-endpoints-and-virtual-machines"></a>Сценарий 2. Архитектура концентратора и звезды — общая виртуальная сеть для частных конечных точек и виртуальных машин

:::image type="content" source="./media/inspect-traffic-using-azure-firewall/shared-spoke.png" alt-text="Частные конечные точки и виртуальные машины в одной виртуальной сети" border="true":::

Этот сценарий реализуется в следующих случаях:

* Для частных конечных точек нельзя использовать выделенную виртуальную сеть.

* Когда в виртуальной сети отображаются только несколько служб с помощью частных конечных точек

Виртуальные машины будут содержать и 32 системных маршрутов, указывающих на каждую закрытую конечную точку. Один маршрут на частные конечные точки настроен для маршрутизации трафика через брандмауэр Azure. 

Административные издержки на поддержание таблицы маршрутов увеличиваются по мере предоставления служб в виртуальной сети. Возможность попадания в пределы маршрута также увеличивается.

В зависимости от общей архитектуры можно использовать ограничение 400 маршрутов. При возможности рекомендуется использовать сценарий 1.

Если виртуальные сети являются равноправными, то при подключении из виртуальной сети клиента к брандмауэру Azure в виртуальной сети будет взиматься плата.

Дополнительные сведения о расходах, связанных с подключениями к одноранговым виртуальным сетям, см. в разделе часто задаваемых вопросов на странице [цен](https://azure.microsoft.com/pricing/details/private-link/) .

>[!NOTE]
> Этот сценарий можно реализовать с помощью любых правил сети сторонних NVA или брандмауэра Azure вместо правил приложений.

## <a name="scenario-3-single-virtual-network"></a>Сценарий 3. одна виртуальная сеть

:::image type="content" source="./media/inspect-traffic-using-azure-firewall/single-vnet.png" alt-text="Одна виртуальная сеть" border="true":::

Есть некоторые ограничения на реализацию: миграция на центральную и лучевую архитектуру невозможна. Те же вопросы, что и в сценарии 2. В этом сценарии плата за пиринг виртуальных сетей не применяется.

>[!NOTE]
> Если вы хотите реализовать этот сценарий с помощью стороннего NVA или брандмауэра Azure, сетевые правила вместо правил приложений необходимы для трафика SNAT, предназначенного для частных конечных точек. В противном случае связь между виртуальными машинами и частными конечными точками завершится ошибкой.

## <a name="scenario-4-on-premises-traffic-to-private-endpoints"></a>Сценарий 4. локальный трафик к частным конечным точкам

:::image type="content" source="./media/inspect-traffic-using-azure-firewall/on-premises.png" alt-text="Локальный трафик в частные конечные точки" border="true":::

Эта архитектура может быть реализована, если вы настроили подключение к локальной сети с помощью следующих средств: 

* [ExpressRoute](..\expressroute\expressroute-introduction.md)
* [VPN типа "сеть — сеть"](..\vpn-gateway\vpn-gateway-howto-site-to-site-resource-manager-portal.md) 

Если требования к безопасности потребовали передачи клиентского трафика к службам, предоставляемым через частные конечные точки через устройство безопасности, разверните этот сценарий.

Те же вопросы, что и в сценарии 2, применяются выше. В этом сценарии не взимается плата за пиринг виртуальных сетей. Дополнительные сведения о том, как настроить DNS-серверы, чтобы разрешить локальным рабочим нагрузкам доступ к частным конечным точкам, см. [в статье локальные рабочие нагрузки с использованием DNS-сервера пересылки](./private-endpoint-dns.md#on-premises-workloads-using-a-dns-forwarder).

>[!NOTE]
> Если вы хотите реализовать этот сценарий с помощью стороннего NVA или брандмауэра Azure, сетевые правила вместо правил приложений необходимы для трафика SNAT, предназначенного для частных конечных точек. В противном случае связь между виртуальными машинами и частными конечными точками завершится ошибкой.

## <a name="prerequisites"></a>Предварительные требования

* Подписка Azure.
* Рабочая область Log Analytics.  

В разделе [Создайте рабочую область log Analytics в портал Azure](../azure-monitor/learn/quick-create-workspace.md) , чтобы создать рабочую область, если у вас ее нет в вашей подписке.


## <a name="sign-in-to-azure"></a>Вход в Azure

Войдите на портал Azure по адресу https://portal.azure.com.

## <a name="create-a-vm"></a>Создание виртуальной машины

В этом разделе вы создадите виртуальную сеть и подсеть для размещения виртуальной машины, используемой для доступа к ресурсу частной ссылки. В качестве примера службы используется база данных SQL Azure.

## <a name="virtual-networks-and-parameters"></a>Виртуальные сети и параметры

Создайте три виртуальные сети и соответствующие подсети для:

* Содержит брандмауэр Azure, используемый для ограничения обмена данными между виртуальной машиной и частной конечной точкой.
* Разместите виртуальную машину, которая используется для доступа к ресурсу частной ссылки.
* Разместите закрытую конечную точку.

Замените следующие параметры в шагах приведенными ниже сведениями.

### <a name="azure-firewall-network"></a>Сеть брандмауэра Azure
| Параметр                   | Значение                 |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroup |
| **\<virtual-network-name>** | мязфввнет          |
| **\<region-name>**          | Центрально-южная часть США      |
| **\<IPv4-address-space>**   | 10.0.0.0/16          |
| **\<subnet-name>**          | азурефиреваллсубнет        |
| **\<subnet-address-range>** | 10.0.0.0/24          |

### <a name="virtual-machine-network"></a>Сетевые ресурсы виртуальных машин
| Параметр                   | Значение                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroup |
| **\<virtual-network-name>** | myVMVNet          |
| **\<region-name>**          | Центрально-южная часть США      |
| **\<IPv4-address-space>**   | 10.1.0.0/16          |
| **\<subnet-name>**          | VMSubnet      |
| **\<subnet-address-range>** | 10.1.0.0/24          |

### <a name="private-endpoint-network"></a>Сеть частной конечной точки
| Параметр                   | Значение                 |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroup |
| **\<virtual-network-name>** | мипевнет         |
| **\<region-name>**          | Центрально-южная часть США      |
| **\<IPv4-address-space>**   | 10.2.0.0/16          |
| **\<subnet-name>**          | приватиндпоинтсубнет    |        |
| **\<subnet-address-range>** | 10.2.0.0/24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

10. Повторите шаги 1 – 9, чтобы создать виртуальные сети для размещения ресурсов виртуальной машины и частной конечной точки.

### <a name="create-virtual-machine"></a>Создание виртуальной машины

1. В верхней левой части экрана портал Azure выберите **создать ресурс**  >  **вычислений**  >  **Виртуальная машина**.

2. В окне **Создание виртуальной машины — Основы** введите или выберите следующую информацию:

    | Параметр | Значение |
    | ------- | ----- |
    | **Сведения о проекте** | |
    | Подписка | Выберите свою подписку. |
    | Группа ресурсов | Выберите **myResourceGroup**. Вы создали эту группу ресурсов в рамках предыдущего раздела.  |
    | **Сведения об экземпляре** |  |
    | Имя виртуальной машины | Введите **myVM**. |
    | Region | Выберите **Юго-Центральный регион США**. |
    | Параметры доступности | Оставьте значение по умолчанию **No infrastructure redundancy required** (Избыточность инфраструктуры не требуется). |
    | Образ — | Выберите **Ubuntu Server 18,04 LTS-Gen1**. |
    | Размер | Выберите **Standard_B2s**. |
    | **Учетная запись администратора** |  |
    | Authentication type (Тип проверки подлинности) | выберите **Пароль**. |
    | Имя пользователя | Введите выбранное имя пользователя. |
    | Пароль | Введите выбранный пароль. Пароль должен включать минимум 12 символов и соответствовать [определенным требованиям к сложности](../virtual-machines/linux/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    | Подтверждение пароля | Введите пароль еще раз. |
    | **Правила входящего порта** |  |
    | Общедоступные входящие порты | Выберите **Отсутствует**. |
    |||

3. По завершении выберите **Next: Диски**.

4. В окне **Создание виртуальной машины — Диски** оставьте значения по умолчанию и выберите **Далее: Сети**.

5. В окне **Создание виртуальной машины — Сети** выберите такую информацию:

    | Параметр | Значение |
    | ------- | ----- |
    | Виртуальная сеть | Выберите **myVMVNet**.  |
    | Подсеть | Выберите **подсеть VMSubnet (10.1.0.0/24)**.|
    | Общедоступный IP-адрес | Оставьте значение по умолчанию **(new) myVm-ip**. |
    | Общедоступные входящие порты | Выберите **Разрешить выбранные порты**. |
    | Выбрать входящие порты | Выберите **SSH**.|
    ||

6. Выберите **Review + create** (Просмотреть и создать). Вы будете перенаправлены на страницу **Просмотр и создание**, где Azure проверит вашу конфигурацию.

7. При появлении сообщения **Проверка пройдена** нажмите кнопку **Создать**.

## <a name="deploy-the-firewall"></a>Развертывание брандмауэра

1. На **домашней странице** или в меню портала Azure выберите **Создать ресурс**.

2. Введите **брандмауэр** в поле поиска и нажмите клавишу **ВВОД**.

3. Выберите **Брандмауэр**, а затем щелкните **Создать**.

4. Используйте сведения в следующей таблице, чтобы настроить брандмауэр на странице **Создание брандмауэра**:

    | Параметр | Значение |
    | ------- | ----- |
    | **Сведения о проекте** | |
    | Подписка | Выберите свою подписку. |
    | Группа ресурсов | Выберите **myResourceGroup**.  |
    | **Сведения об экземпляре** |  |
    | Имя | Введите **мязурефиревалл**. |
    | Region | Выберите **Юго-Центральный регион США**. |
    | Зона доступности | Оставьте значение по умолчанию **Отсутствует**. |
    | Выберите виртуальную сеть    |    Выберите **использовать существующий**.    |
    | Виртуальная сеть    |    Выберите **мязфввнет**.    |
    | Общедоступный IP-адрес    |    Выберите **Добавить новые** и в списке Имя введите **мифиревалл-IP**.    |
    | Принудительное туннелирование    | Оставьте значение по умолчанию **отключено**.    |
    |||
5. Выберите **Review + create** (Просмотреть и создать). Вы будете перенаправлены на страницу **Просмотр и создание**, где Azure проверит вашу конфигурацию.

6. При появлении сообщения **Проверка пройдена** нажмите кнопку **Создать**.

## <a name="enable-firewall-logs"></a>Включение журналов брандмауэра

В этом разделе вы включите журналы в брандмауэре.

1. В портал Azure выберите **все ресурсы** в меню слева.

2. Выберите брандмауэр **мязурефиревалл** в списке ресурсов.

3. В разделе **наблюдение** в параметрах брандмауэра выберите **параметры диагностики** .

4. Выберите **+ Добавить параметр диагностики** в параметрах диагностики.

5. В **параметре диагностики** введите или выберите следующие сведения:

    | Параметр | Значение |
    | ------- | ----- |
    | Имя параметра диагностики | Введите **мидиагсеттинг**. |
    | Сведения о категории | |
    | log | Выберите **азурефиреваллаппликатионруле** и **азурефиреваллнетворкруле**. |
    | Сведения о назначении | Установите флажок **Отправить в Log Analytics**. |
    | Подписка | Выберите свою подписку. |
    | Рабочая область Log Analytics | Выберите рабочую область Log Analytics. |

6. Нажмите кнопку **Сохранить**.

## <a name="create-azure-sql-database"></a>Создание базы данных SQL Azure

В этом разделе вы создадите закрытую базу данных SQL.

1. В верхней левой части экрана портал Azure выберите **создать ресурс**  >  **базы**  >  **данных SQL**.

2. В статье **Создание базы данных SQL — основные** сведения введите или выберите следующую информацию:

    | Параметр | Значение |
    | ------- | ----- |
    | **Сведения о проекте** | |
    | Подписка | Выберите свою подписку. |
    | Группа ресурсов | Выберите **myResourceGroup**. Вы создали эту группу ресурсов в рамках предыдущего раздела.|
    | **Сведения о базе данных** |  |
    | Имя базы данных  | Введите **mydatabase**.  |
    | Сервер | Выберите **создать** и введите приведенные ниже сведения.    |
    | Имя сервера | Введите **MyDBServer**. Если это имя используется, введите уникальное имя.   |
    | учетные данные администратора сервера для входа; | Введите имя, которое вы выбрали. |
    | Пароль    |    Введите выбранный пароль.    |
    | Подтверждение пароля | Введите пароль еще раз.    |
    | Расположение    | Выберите **Юго-Центральный регион США**.    |
    | Хотите использовать эластичный пул SQL    | Оставьте значение по умолчанию **Нет**. |
    | Вычисления и хранилище | Оставьте хранилище по умолчанию **общего назначения го поколения, 2 виртуальных ядер, 32 ГБ**. |
    |||

3. Выберите **Review + create** (Просмотреть и создать). Вы будете перенаправлены на страницу **Просмотр и создание**, где Azure проверит вашу конфигурацию.

4. При появлении сообщения **Проверка пройдена** нажмите кнопку **Создать**.

## <a name="create-private-endpoint"></a>Создание частной конечной точки

В этом разделе вы создадите закрытую конечную точку для базы данных SQL Azure в предыдущем разделе.

1. В портал Azure выберите **все ресурсы** в меню слева.

2. Выберите **MyDBServer** Azure SQL Server в списке служб.  Если вы использовали другое имя сервера, выберите это имя.

3. В параметрах сервера в разделе **Безопасность** выберите **частные конечные подключения** .

4. Щелкните **+ Добавить частную конечную точку**.

5. В окне **Создание частной конечной точки** введите или выберите эти сведения на вкладке " **основы** ":

    | Параметр | Значение |
    | ------- | ----- |
    | **Сведения о проекте** | |
    | Подписка | Выберите свою подписку. |
    | Группа ресурсов | Выберите **myResourceGroup**. |
    | **Сведения об экземпляре** | |
    | Имя | Введите **склприватиндпоинт**. |
    | Region | Выберите **Юго-Центральный регион США.** |

6. Перейдите на вкладку **ресурс** или выберите **Далее: ресурс** в нижней части страницы.

7. На вкладке **ресурс** введите или выберите следующие сведения:

    | Параметр | Значение |
    | ------- | ----- |
    | Метод подключения | Выберите **Подключиться к ресурсу Azure в моем каталоге**. |
    | Подписка | Выберите свою подписку. |
    | Тип ресурса | Выберите **Microsoft.Sql/servers**. |
    | Ресурс | Выберите **MyDBServer** или имя сервера, созданного на предыдущем шаге.
    | Целевой подресурс | Выберите **sqlServer**. |

8. Перейдите на вкладку **Конфигурация** или нажмите кнопку **Далее: Конфигурация** в нижней части страницы.

9. На вкладке **Конфигурация** введите или выберите следующие сведения:

    | Параметр | Значение |
    | ------- | ----- |
    | **Сеть** | |
    | Виртуальная сеть | Выберите **мипевнет**. |
    | Подсеть | Выберите **приватиндпоинтсубнет**. |
    | **Интеграция с частной зоной DNS** | |
    | Интеграция с частной зоной DNS | Выберите **Да**. |
    | Подписка | Выберите свою подписку. |
    | Частные зоны DNS | Оставьте **privatelink.Database.Windows.NET** по умолчанию. |

10. Перейдите на вкладку **Просмотр и создание** или выберите в нижней части страницы пункт Просмотр и **Создание** .

11. Выберите **Создать**.

12. После создания конечной точки в разделе **Безопасность** выберите **брандмауэры и виртуальные сети** .

13. В окне **брандмауэры и виртуальные сети** нажмите кнопку **Да** рядом, чтобы **Разрешить службам Azure и ресурсам доступ к этому серверу**.

14. Нажмите кнопку **Сохранить**.

## <a name="connect-the-virtual-networks-using-virtual-network-peering"></a>Подключение виртуальных сетей с помощью пиринга виртуальных сетей

В этом разделе мы подключимся к виртуальным сетям **myVMVNet** и **мипевнет** , **мязфввнет** с помощью пиринга. Между **myVMVNet** и **мипевнет** не будет осуществляться прямое подключение.

1. На панели поиска портала введите **мязфввнет**.

2. Выберите **пиринг** в меню **Параметры** и щелкните **+ добавить**.

3. В области **Добавление пиринга** введите или выберите следующие сведения.

    | Параметр | Значение |
    | ------- | ----- |
    | Имя пиринга из Мязфввнет в удаленную виртуальную сеть | Введите **мязфввнет-to-myVMVNet**. |
    | **Сведения о одноранговом узле** |  |
    | Модель развертывания виртуальной сети  | Оставьте **Диспетчер ресурсов** по умолчанию.  |
    | Я знал идентификатор ресурса | не устанавливайте флажок.    |
    | Подписка | Выберите свою подписку.    |
    | Виртуальная сеть | Выберите **myVMVNet**. |
    | Имя пиринга из удаленной виртуальной сети в Мязфввнет    |    Введите **myVMVNet-to-мязфввнет**.    |
    | **Конфигурация** | |
    | **Настройка параметров доступа к виртуальной сети** | |
    | Разрешить доступ к виртуальной сети из Мязфввнет в удаленную виртуальную сеть | Оставьте значение по умолчанию **Включено**.    |
    | Разрешить доступ к виртуальной сети из удаленной виртуальной сети в Мязфввнет    | Оставьте значение по умолчанию **Включено**.    |
    | **Настройка параметров перенаправленного трафика** | |
    | Разрешить перенаправленный трафик из удаленной виртуальной сети в Мязфввнет    | Выберите значение **Включено**. |
    | Разрешить перенаправленный трафик из Мязфввнет в удаленную виртуальную сеть | Выберите значение **Включено**. |
    | **Настройка параметров транзитного шлюза** | |
    | Разрешить транзит шлюзов | Не устанавливайте флажок |
    |||

4. Щелкните **ОК**.

5. Щелкните **+ Добавить**.

6. В области **Добавление пиринга** введите или выберите следующие сведения.

    | Параметр | Значение |
    | ------- | ----- |
    | Имя пиринга из Мязфввнет в удаленную виртуальную сеть | Введите **мязфввнет-to-мипевнет**. |
    | **Сведения о одноранговом узле** |  |
    | Модель развертывания виртуальной сети  | Оставьте **Диспетчер ресурсов** по умолчанию.  |
    | Я знал идентификатор ресурса | не устанавливайте флажок.    |
    | Подписка | Выберите свою подписку.    |
    | Виртуальная сеть | Выберите **мипевнет**. |
    | Имя пиринга из удаленной виртуальной сети в Мязфввнет    |    Введите **мипевнет-to-мязфввнет**.    |
    | **Конфигурация** | |
    | **Настройка параметров доступа к виртуальной сети** | |
    | Разрешить доступ к виртуальной сети из Мязфввнет в удаленную виртуальную сеть | Оставьте значение по умолчанию **Включено**.    |
    | Разрешить доступ к виртуальной сети из удаленной виртуальной сети в Мязфввнет    | Оставьте значение по умолчанию **Включено**.    |
    | **Настройка параметров перенаправленного трафика** | |
    | Разрешить перенаправленный трафик из удаленной виртуальной сети в Мязфввнет    | Выберите значение **Включено**. |
    | Разрешить перенаправленный трафик из Мязфввнет в удаленную виртуальную сеть | Выберите значение **Включено**. |
    | **Настройка параметров транзитного шлюза** | |
    | Разрешить транзит шлюзов | Не устанавливайте флажок |

7. Щелкните **ОК**.

## <a name="link-the-virtual-networks-to-the-private-dns-zone"></a>Связывание виртуальных сетей с частной зоной DNS

В этом разделе мы будем связывать виртуальные сети **myVMVNet** и **мязфввнет** с частной зоной DNS **privatelink.Database.Windows.NET** . Эта зона была создана при создании закрытой конечной точки. 

Эта ссылка необходима для того, чтобы виртуальная машина и брандмауэр разрешили полное доменное имя базы данных на адрес своей частной конечной точки. Виртуальная сеть **мипевнет** была автоматически связана при создании частной конечной точки.

>[!NOTE]
>Если вы не свяжете виртуальную машину и виртуальные сети брандмауэра с частной зоной DNS, как виртуальная машина, так и брандмауэр по-прежнему смогут разрешить SQL Server полное доменное имя. Они будут разрешаться в общедоступный IP-адрес.

1. На панели поиска портала введите **привателинк. Database**.

2. В результатах поиска выберите **privatelink.Database.Windows.NET** .

3. В разделе **Параметры** выберите **ссылки на виртуальную сеть** .

4. Выберите **+ Добавить**.

5. В поле **Добавить ссылку на виртуальную сеть** введите или выберите следующие сведения.

    | Параметр | Значение |
    | ------- | ----- |
    | Имя ссылки | Введите **Link-to-myVMVNet**. |
    | **Сведения о виртуальной сети** |  |
    | Я знал, что идентификатор ресурса виртуальной сети  | не устанавливайте флажок.  |
    | Подписка | Выберите свою подписку.    |
    | Виртуальная сеть | Выберите **myVMVNet**. |
    | **CONFIGURATION** | |
    | Включить автоматическую регистрацию | не устанавливайте флажок.    |


6. Щелкните **ОК**.

## <a name="configure-an-application-rule-with-sql-fqdn-in-azure-firewall"></a>Настройка правила приложения с полным доменным именем SQL в брандмауэре Azure

В этом разделе вы настроите правило приложения, разрешающее обмен данными между **myVM** и закрытой конечной точкой для SQL Server **MyDBServer.Database.Windows.NET**. 

Это правило разрешает обмен данными через брандмауэр, созданный на предыдущих шагах.

1. На панели поиска портала введите **мязурефиревалл**.

2. В результатах поиска выберите **мязурефиревалл** .

3. Выберите **правила** в разделе **Параметры** в обзоре **мязурефиревалл** .

4. Откройте вкладку **Коллекция правил приложения**.

5. Выберите **+ Добавить коллекцию правил приложения**.

6. В окне **Добавление коллекции правил приложения** введите или выберите следующие сведения:

    | Параметр | Значение |
    | ------- | ----- |
    | Имя | Введите **склприватиндпоинт**. |
    | Priority | Введите **100**. |
    | Действие | Введите **allow**. |
    | **Правила** |  |
    | **Теги FQDN** | |
    | Имя  | Не указывайте.  |
    | Тип исходного значения | Оставьте **IP-адрес** по умолчанию.    |
    | Источник | Не указывайте. |
    | Теги FQDN | Оставьте значение по умолчанию **0**. |
    | **Целевые полные доменные имена** | |
    | Имя | Введите **склприватиндпоинт**.    |
    | Тип исходного значения | Оставьте **IP-адрес** по умолчанию. |
    | Источник | Введите **10.1.0.0/16**. |
    | Протокол: порт | Введите **MSSQL: 1433**. |
    | Целевые полные доменные имена | Введите **MyDBServer.Database.Windows.NET**. |
    |||

7. Выберите **Добавить**.

## <a name="route-traffic-between-the-virtual-machine-and-private-endpoint-through-azure-firewall"></a>Маршрутизация трафика между виртуальной машиной и частной конечной точкой через брандмауэр Azure

Мы не создали пиринг виртуальных сетей напрямую между виртуальными сетями **myVMVNet** и **мипевнет**. Виртуальная машина **myVM** не имеет маршрута к созданной закрытой конечной точке. 

В этом разделе мы создадим таблицу маршрутов с настраиваемым маршрутом. 

Маршрут отправляет трафик из подсети **myVM** в адресное пространство **мипевнет** виртуальной сети через брандмауэр Azure.

1. В меню портала Azure или на странице **Домашняя** выберите **Создать ресурс**.

2. Введите **таблицу маршрутов** в поле поиска и нажмите клавишу **Ввод**.

3. Выберите **Таблица маршрутов** и нажмите кнопку **создать**.

4. На странице **Создание таблицы маршрутов** используйте следующую таблицу для настройки таблицы маршрутов.

    | Параметр | Значение |
    | ------- | ----- |
    | **Сведения о проекте** | |
    | Подписка | Выберите свою подписку. |
    | Группа ресурсов | Выберите **myResourceGroup**.  |
    | **Сведения об экземпляре** |  |
    | Region | Выберите **Юго-Центральный регион США**. |
    | Имя | Введите **подсеть VMsubnet-to-азурефиревалл**. |
    | Распространение маршрутов шлюза | Выберите вариант **Нет**. |

5. Выберите **Review + create** (Просмотреть и создать). Вы будете перенаправлены на страницу **Просмотр и создание**, где Azure проверит вашу конфигурацию.

6. При появлении сообщения **Проверка пройдена** нажмите кнопку **Создать**.

7. После завершения развертывания выберите **Переход к ресурсу**.

8. В разделе **Параметры** выберите **маршруты** .

9. Щелкните **+ Добавить**.

10. На странице **Добавление маршрута** введите или выберите следующие сведения:

    | Параметр | Значение |
    | ------- | ----- |
    | Имя маршрута | Введите **мивмсубнет-to-приватиндпоинт**. |
    | Префикс адреса | Введите **10.2.0.0/16**.  |
    | Тип следующего прыжка | Выберите **Виртуальный модуль**. |
    | Адрес следующего прыжка | Введите **10.0.0.4**. |

11. Щелкните **ОК**.

12. Выберите **подсети** в разделе **Параметры**.

13. Выберите **+ Связать**.

14. На странице **Сопоставление подсети** введите или выберите следующие сведения:

    | Параметр | Значение |
    | ------- | ----- |
    | Виртуальная сеть | Выберите **myVMVNet**. |
    | Подсеть | Выберите **подсеть VMSubnet**.  |

15. Щелкните **ОК**.

## <a name="connect-to-the-virtual-machine-from-your-client-computer"></a>Подключение к виртуальной машине с клиентского компьютера

Подключитесь к виртуальной машине **myVm** из Интернета, выполнив следующие действия.

1. На панели поиска портала введите **myVm-IP**.

2. В результатах поиска выберите **myVM-IP** .

3. Скопируйте или запишите значение в поле **IP-адрес**.

4. Если вы используете Windows 10, выполните следующую команду с помощью PowerShell. Для других версий клиента Windows используйте SSH-клиент [, например,](https://www.putty.org/)выводимые сведения:

* Замените **username** именем пользователя администратора, введенным при создании виртуальной машины.

* Замените **IPAddress** IP-адресом из предыдущего шага.

    ```bash
    ssh username@IPaddress
    ```

5. Введите пароль, определенный при создании **myVm**

## <a name="access-sql-server-privately-from-the-virtual-machine"></a>Доступ к SQL Server в частном порядке с виртуальной машины

В этом разделе вы будете подключаться к базе данных SQL в частном порядке с помощью частной конечной точки.

1. Введите `nslookup mydbserver.database.windows.net`
    
    Вы получите примерно следующее сообщение:

    ```bash
    Server:         127.0.0.53
    Address:        127.0.0.53#53

    Non-authoritative answer:
    mydbserver.database.windows.net       canonical name = mydbserver.privatelink.database.windows.net.
    Name:   mydbserver.privatelink.database.windows.net
    Address: 10.2.0.4
    ```

2. Установите [SQL Server программ командной строки](/sql/linux/quickstart-install-connect-ubuntu?view=sql-server-ver15#tools).

3. Выполните следующую команду, чтобы подключиться к SQL Server. Используйте администратора и пароль сервера, определенные при создании SQL Server на предыдущих шагах.

* Замените **\<ServerAdmin>** именем администратора, введенным при создании SQL Server.

* Замените **\<YourPassword>** паролем администратора, введенным при создании SQL Server.

    ```bash
    sqlcmd -S mydbserver.database.windows.net -U '<ServerAdmin>' -P '<YourPassword>'
    ```
4. При успешном входе в систему будет отображаться Командная строка SQL. Введите **Exit** , чтобы выйти из средства **sqlcmd** .

5. Закройте подключение к **myVM** , введя **команду Exit**.

## <a name="validate-the-traffic-in-azure-firewall-logs"></a>Проверка трафика в журналах брандмауэра Azure

1. В портал Azure выберите **все ресурсы** и выберите свою рабочую область log Analytics.

2. На странице Log Analytics Рабочая область выберите **журналы** в разделе **Общие** .

3. Нажмите синюю кнопку « **Начало работы** ».

4. В окне **примеры запросов** выберите **брандмауэры** во **всех запросах**.

5. Нажмите кнопку **выполнить** в разделе **данные журнала правил приложений**.

6. В выходных данных запроса журнала убедитесь, что **MyDBServer.Database.Windows.NET** указан в разделе **полное доменное имя** , а **склприватиндпоинт** — в разделе **рулеколлектион**.

## <a name="clean-up-resources"></a>Очистка ресурсов

По завершении использования ресурсов удалите группу ресурсов и все содержащиеся в ней ресурсы.

1. Введите **myResourceGroup** в поле **Поиск** в верхней части портала и выберите **myResourceGroup** в результатах поиска.

1. Выберите **Удалить группу ресурсов**.

1. Введите **myResourceGroup** в поле **Введите имя группы ресурсов** и нажмите кнопку **Удалить**.

## <a name="next-steps"></a>Дальнейшие действия

В этой статье вы изучили различные сценарии, которые можно использовать для ограничения трафика между виртуальной машиной и частной конечной точкой с помощью брандмауэра Azure. 

Вы подключились к виртуальной машине и безопасно взаимодействовали с базой данных через брандмауэр Azure с помощью частной ссылки.

Дополнительные сведения о частных конечных точках см. в статье [что такое частная конечная точка Azure?](private-endpoint-overview.md).