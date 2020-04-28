---
title: Azure Relay запросы гибридные подключения-HTTP в .NET
description: 'Написание консольного приложения #C для HTTP-запросов гибридных подключений Azure Relay в .NET.'
services: service-bus-relay
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: conceptual
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/01/2018
ms.author: spelluru
ms.openlocfilehash: 7c984876c4338b4f6802ba55752c8f612c390e94
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75355167"
---
# <a name="get-started-with-relay-hybrid-connections-http-requests-in-net"></a>Начало работы с HTTP-запросами гибридных подключений Azure Relay в .NET
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

В этом кратком руководстве описано, как создать приложения .NET, которые отправляют и получают сообщения по протоколу HTTP. Приложения используют функцию гибридных подключений службы Azure Relay. См. дополнительные сведения о [службе Azure Relay](relay-what-is-it.md). 

При работе с этим кратким руководством вы выполните следующие задачи:

1. Создайте пространство имен ретранслятора с помощью портала Azure.
2. Создайте гибридное подключение в этом пространстве имен с помощью портала Azure.
3. Создайте серверное консольное приложение (прослушиватель) для получения сообщений.
4. Создайте клиентское консольное приложение (отправитель) для отправки сообщений.
5. Запустите приложения. 

## <a name="prerequisites"></a>Предварительные требования

Для работы с данным руководством вам потребуется:

* [Visual Studio 2015 или более поздней версии](https://www.visualstudio.com). В описанных в этом руководстве примерах используется Visual Studio 2017.
* Подписка Azure. Если у вас ее нет, [Создайте бесплатную учетную запись](https://azure.microsoft.com/free/) , прежде чем начинать работу.

## <a name="create-a-namespace"></a>Создание пространства имен
[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="create-a-hybrid-connection"></a>Создание гибридного подключения
[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="create-a-server-application-listener"></a>Создание серверного приложения (прослушивателя)
Создайте в Visual Studio консольное приложение C# для прослушивания и получения сообщений, отправленных ретранслятором.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-server](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-server.md)]

## <a name="create-a-client-application-sender"></a>Создание клиентского приложения (отправителя)
Создайте в Visual Studio консольное приложение C# для отправки сообщений в ретранслятор.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-client](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-client.md)]

## <a name="run-the-applications"></a>Запуск приложений
1. Запустите серверное приложение. В консоли отображается пример выходных данных.

    ```
    Online
    Server listening
    ```
1. Выполните клиентское приложение В окне клиента вы увидите `hello!`. Клиент отправил HTTP-запрос к серверу и получил ответ `hello!`. 
3. Теперь, чтобы закрыть окна консоли, в обоих ее окнах нажмите клавишу **ВВОД**. 

Поздравляем, вы создали законченное приложение гибридные подключения!

## <a name="next-steps"></a>Дальнейшие действия

В этом кратком руководстве описано, как создать клиентские и серверные приложения .NET, которые отправляют и получают сообщения по протоколу HTTP. Функция гибридных подключений службы Azure Relay также поддерживает использование протокола WebSocket для отправки и получения сообщений. См. дополнительные сведения об [использовании протокола WebSocket с гибридными подключениями Azure Relay](relay-hybrid-connections-dotnet-get-started.md).

В этом кратком руководстве описано, как использовать .NET Framework для создания клиентских и серверных приложений. Сведения о том, как создавать клиентские и серверные приложения с помощью Node.js, см. в руководствах по использованию [Node.js и WebSocket](relay-hybrid-connections-node-get-started.md) или [Node.js и HTTP](relay-hybrid-connections-http-requests-dotnet-get-started.md).
