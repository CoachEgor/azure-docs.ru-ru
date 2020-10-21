---
title: Дальнейшие действия по созданию проекта Service Fabric
description: Узнайте о проекте приложения, который вы только что создали в Visual Studio,  о том, как создать службы с помощью руководств, а также ознакомьтесь с дополнительными сведениями о разработке служб для Service Fabric.
ms.topic: conceptual
ms.date: 12/07/2017
ms.openlocfilehash: 01a69016e0c299fba0365fab5332b572fd2ca87a
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2020
ms.locfileid: "92314476"
---
# <a name="your-service-fabric-application-and-next-steps"></a>Ваше приложение Service Fabric и дальнейшие действия
Ваше приложение Azure Service Fabric создано. В этой статье описываются некоторые руководства, с которыми вам необходимо ознакомиться, состав проекта, а также приведены некоторые дополнительные сведения, которые будут вам интересны, и возможные дальнейшие действия.

## <a name="get-started-with-tutorials-walk-throughs-and-samples"></a>Приступая к работе со статьями, пошаговыми руководствами и примерами
Вы готовы начать работу?  

Ознакомьтесь с руководством по работе с приложением .NET. Узнайте, как [создать приложение](service-fabric-tutorial-create-dotnet-app.md) с интерфейсной частью ASP.NET Core и серверной службой с отслеживанием состояния, [развернуть его](service-fabric-tutorial-deploy-app-to-party-cluster.md) в кластер, [настроить непрерывную интеграцию и поставку](service-fabric-tutorial-deploy-app-with-cicd-vsts.md), а также [мониторинг и диагностику](service-fabric-tutorial-monitoring-aspnet.md).

Вы также можете ознакомиться с пошаговыми руководствами ниже и создать свою первую
- [службу Reliable Services на C# в Windows](service-fabric-reliable-services-quick-start.md); 
- [службу Reliable Actors на C# в Windows](service-fabric-reliable-actors-get-started.md); 
- [гостевую исполняемую службу в Windows](quickstart-guest-app.md); 
- [Приложение-контейнер Windows](service-fabric-get-started-containers.md) 

Возможно, вас также заинтересуют [примеры приложений](/samples/browse/?products=azure).

## <a name="have-questions-or-feedback--need-to-report-an-issue"></a>Есть вопросы или предложения?  Необходимо сообщить о проблеме?
Ознакомьтесь с [часто задаваемыми вопросами](service-fabric-common-questions.md), найдите ответы о возможностях Service Fabric и узнайте об использовании этой платформы.

В качестве [вариантов поддержки](service-fabric-support.md) доступны форумы StackOverflow и MSDN, где можно задать вопросы, а также ресурсы для сообщения о проблемах, получения поддержки и отправки отзыва о продукте.

## <a name="the-application-project"></a>Проект приложения
Каждое новое приложение включает проект приложения. В зависимости от типа выбранной службы также могут быть один или два дополнительных проекта.

Проект приложения включает:

* Набор ссылок на службы, входящие в состав приложения.
* Три профиля публикации (локальный 1-узловой, локальный 5-узловой и облачный), которые можно использовать для хранения настроек для работы в различных средах, например параметров конечной точки кластера и параметра, определяющего, следует ли разворачивать обновления по умолчанию.
* Три файла с параметрами приложения (те же, что и выше), которые можно использовать для хранения конфигураций приложения для разных сред, включая число разделов, которые создаются для службы. Узнайте, как [настроить приложение для нескольких сред](service-fabric-manage-multiple-environment-app-configuration.md).
* Сценарий развертывания, который можно использовать для развертывания приложения из командной строки или в составе автоматизированного конвейера непрерывной интеграции и развертывания. Узнайте больше о [развертывании приложений с помощью PowerShell](service-fabric-deploy-remove-applications.md).
* Манифест приложения, описывающий приложение. Манифест можно найти в папке ApplicationPackageRoot. Узнайте больше о [манифестах приложений и служб](service-fabric-application-model.md).



## <a name="learn-more-about-the-programming-models"></a>Узнайте больше о моделях программирования
Service Fabric предлагает несколько способов записи и управления службами.  По этим ссылкам можно получить общие и основные сведения о [службах Reliable Services c отслеживанием и без отслеживания состояния](service-fabric-reliable-services-introduction.md), службах [Reliable Actors](service-fabric-reliable-actors-introduction.md), [контейнерах](service-fabric-containers-overview.md), [гостевых исполняемых службах](service-fabric-guest-executables-introduction.md) и [службах ASP.NET Core c отслеживанием и без отслеживания состояния](service-fabric-reliable-services-communication-aspnetcore.md).

## <a name="learn-about-service-communication"></a>Узнайте больше о взаимодействии служб
Приложение Service Fabric состоит из множества разных служб, каждая из которых выполняет специализированную задачу. Эти службы могут обмениваться данными друг с другом. Кроме того, вне кластера могут присутствовать клиентские приложения, которые подключаются к службам и взаимодействуют с ними. Узнайте, как в Service Fabric [настроить обмен данными со службами и между ними](service-fabric-connect-and-communicate-with-services.md). 

## <a name="learn-about-configuring-application-security"></a>Узнайте о настройке безопасности приложения
Вы можете защитить приложения, работающие в кластере под разными учетными записями. Кроме того, платформа помогает защищать ресурсы, используемые приложениями при развертывании с помощью учетных записей, например файлы, каталоги и сертификаты. Это позволяет изолировать друг от друга выполняемые приложения, даже если они запущены в общей среде.  Узнайте, как [настроить политики безопасности для приложения](service-fabric-application-runas-security.md).

В приложении может храниться конфиденциальная информация, например строки подключения к хранилищу, пароли или другие значения, которые не должны обрабатываться в виде обычного текста. Узнайте, как [управлять конфиденциальными сведениями в приложении](service-fabric-application-secret-management.md).

## <a name="learn-about-the-application-lifecycle"></a>Узнайте больше о жизненном цикле приложения
Как и в случае с другими платформами, приложение в Service Fabric обычно проходит следующие фазы: проектирование, разработка, тестирование, развертывание, обновление, техническое обслуживание и удаление. В [этой статье](service-fabric-application-lifecycle.md) представлен обзор интерфейсов API и их использования различными ролями на всех этапах жизненного цикла приложения Service Fabric.

## <a name="next-steps"></a>Дальнейшие действия
- [Развертывание кластера Service Fabric на платформе Windows в виртуальной сети Azure](service-fabric-tutorial-create-vnet-and-windows-cluster.md).
- Визуализируйте кластер, включая развернутые приложения и физическую структуру, с помощью [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
- [Версии и обновление служб](service-fabric-application-upgrade-tutorial.md)