---
title: Элементы управления примеров схем UK OFFICIAL и UK NHS
description: Сопоставление элементов управления примеров схем UK OFFICIAL и UK NHS. Каждый элемент управления сопоставляется с определениями одной или нескольких Политик Azure, которые помогают выполнять оценку.
ms.date: 11/05/2020
ms.topic: sample
ms.openlocfilehash: 352ba30a21c638c68401e2f8e471096a777fbde9
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96009387"
---
# <a name="control-mapping-of-the-uk-official-and-uk-nhs-blueprint-samples"></a>Сопоставление элементов управления примеров схем UK OFFICIAL и UK NHS

В этой статье подробно описано, как примеры схем UK OFFICIAL и UK NHS сопоставляются с элементами управления UK OFFICIAL и UK NHS. Дополнительные сведения об элементах управления UK OFFICIAL см. [здесь](https://www.gov.uk/government/publications/government-security-classifications).

Приведенные здесь сопоставления относятся к элементам управления **UK OFFICIAL** и **UK NHS**. Используйте панель навигации справа для перехода непосредственно к сопоставлению конкретных элементов управления. Многие сопоставленные элементы управления реализуются с помощью инициативы [Политика Azure](../../../policy/overview.md). Чтобы просмотреть полную инициативу, откройте раздел **Политика** на портале Azure и перейдите на страницу **Определения**. Затем найдите и выберите встроенную инициативу политики **\[Предварительная версия\]: аудит элементов управления UK OFFICIAL и UK NHS и развертывание указанных расширений виртуальных машин для выполнения требований аудита**.

> [!IMPORTANT]
> Каждый элемент управления ниже связан с одним или несколькими определениями [Политики Azure](../../../policy/overview.md). Такие политики помогут вам в [оценке соответствия](../../../policy/how-to/get-compliance-data.md) с помощью элементов управления, но часто полное или точное соответствие между элементом управления и одной или несколькими политиками отсутствует. Поэтому состояние **Соответствует** в Политике Azure применимо только к самим политикам и не означает полное соответствие всем требованиям элемента управления. Кроме того, стандарт соответствия включает элементы управления, на которые сейчас не распространяются определения Политики Azure. Следовательно, сведения о соответствии в Политике Azure — это только частичное представление общего состояния соответствия. Связи между элементами управления и определениями Политики Azure для этого примера схемы соответствия могут измениться в будущем. Историю изменений можно просмотреть на странице [журнала фиксаций в GitHub](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/ukofficial/control-mapping.md).

## <a name="1-data-in-transit-protection"></a>Защита данных в процессе передачи (1)

Эта схема помогает обеспечить безопасность передачи информации с помощью служб Azure, назначая определения [Политики Azure](../../../policy/overview.md) для аудита небезопасных подключений к учетным записям хранения и Кэшу Redis.

- К кэшу Redis должны выполняться только безопасные подключения.
- Должно выполняться безопасное перемещение в учетные записи хранения.
- Отображение результатов аудита веб-серверов Windows без безопасных протоколов связи.
- Веб-приложение должно быть доступно только по HTTPS
- Приложение-функция должно быть доступно только по HTTPS
- Приложение API должно быть доступно только по HTTPS.

## <a name="23-data-at-rest-protection"></a>Защита неактивных данных (2.3)

Эта схема помогает принудительно применить политику по использованию элементов управления шифрованием, назначая определения [Политики Azure](../../../policy/overview.md) для принудительного применения определенных элементов управления шифрованием и аудита использования слабых параметров шифрования. Определение областей, в которых ресурсы Azure могут иметь неоптимальные криптографические конфигурации, поможет вам выполнить корректирующие действия, чтобы убедиться, что ресурсы настроены в соответствии с вашей информационной политикой безопасности. В частности, присвоенные в рамках этой схемы политики отвечают за следующее: требуют шифрование для учетных записей хранения озера данных; требуют прозрачное шифрование данных для баз данных SQL; ведут аудит отсутствия шифрования для учетных записей хранения, баз данных SQL, дисков виртуальной машины и переменных учетной записи службы автоматизации; ведут аудит небезопасных подключений для учетных записей хранения и Кэша Redis; ведут аудит слабого шифрования для паролей виртуальной машины; ведут аудит незашифрованного обмена данными с Service Fabric.

- Шифрование дисков должно применяться к виртуальным машинам.
- Необходимо включить шифрование для переменных учетной записи службы автоматизации.
- Свойство ClusterProtectionLevel в кластерах Service Fabric должно иметь значение EncryptAndSign.
- Прозрачное шифрование данных должно быть включено в базах данных SQL.
- Развертывание прозрачного шифрования данных для SQL DB
- Требование шифрования для учетных записей Data Lake Store.
- Разрешенные расположения (строго ограничены регионами "Южная часть Соединенного Королевства" и "Западная часть Соединенного Королевства").
- Разрешенные расположения для групп ресурсов (строго ограничены регионами "Южная часть Соединенного Королевства" и "Западная часть Соединенного Королевства").

## <a name="52-vulnerability-management"></a>Управление уязвимостями (5.2)

Эта схема помогает управлять уязвимостями информационной системы, назначая определения [Политики Azure](../../../policy/overview.md) для мониторинга отсутствующей защиты конечных точек, отсутствующих обновлений системы, уязвимостей операционной системы, уязвимостей SQL и уязвимостей виртуальной машины. Эти полезные сведения предоставляют информацию в реальном времени о состоянии безопасности развернутых ресурсов и помогают вам приоритезировать действия по исправлению.

- Мониторинг отсутствия Endpoint Protection в Центре безопасности Azure
- На ваших компьютерах должны быть установлены обновления системы
- Обновления системы должны быть установлены в масштабируемых наборах виртуальных машин.
- Уязвимости в настройках безопасности на ваших компьютерах должны быть устранены
- Уязвимости в базах данных SQL должны быть устранены.
- Уязвимости должны быть устранены с помощью решения для оценки уязвимостей.
- На серверах SQL Server должна быть включена оценка уязвимости.
- В управляемых экземплярах SQL должна быть включена оценка уязвимостей.
- Уязвимости в конфигурации безопасности в масштабируемом наборе виртуальных машин должны быть устранены.
- Необходимо включить Расширенную защиту данных на управляемых экземплярах SQL.
- Расширенная защита данных должна быть включена на серверах SQL Server.

## <a name="53-protective-monitoring"></a>Защитный мониторинг (5.3)

Эта схема помогает защитить ресурсы информационной системы путем назначения определений [Политики Azure](../../../policy/overview.md), обеспечивающих защитный мониторинг неограниченного доступа, действий списка разрешений и угроз.

- Учетные записи хранения должны ограничивать доступ к сети.
- На компьютерах должны быть включены адаптивные элементы управления приложениями для определения безопасных приложений.
- Аудит виртуальных машин, для которых не настроено аварийное восстановление
- Защита от атак DDoS Azure уровня "Стандартный" должна быть включена
- Для типов "Расширенной защиты от угроз" должно быть задано значение "Все" в настройках "Расширенной защиты данных" управляемого экземпляра SQL
- Для типов "Расширенной защиты от угроз" должно быть задано значение "Все" в настройках "Расширенной защиты данных" сервера SQL
- Развертывание обнаружения угроз на серверах SQL Server
- Развертывание стандартного расширения IaaSAntimalware корпорации Майкрософт для Windows

## <a name="9-secure-user-management"></a>Безопасное управление пользователями (9) 

Управление доступом на основе ролей Azure (Azure RBAC) упрощает управление доступом к ресурсам в Azure.
С помощью портала Azure вы можете просмотреть, у кого есть доступ к ресурсам Azure и какие им назначены разрешения. Эта схема помогает ограничивать и контролировать права доступа, назначая определения [Политики Azure](../../../policy/overview.md) для аудита внешних учетных записей с правами владельца и (или) чтения и записи, учетных записей с правами владельца, чтения и (или) записи, которые не используют многофакторную проверку подлинности.

- MFA должна быть включена для учетных записей с разрешениями владельца в вашей подписке.
- MFA должна быть включена для учетных записей с разрешениями на запись в вашей подписке.
- MFA должна быть включена для учетных записей с разрешениями на чтение в вашей подписке.
- Внешние учетные записи с разрешениями владельца следует удалять из подписки.
- Внешние учетные записи с разрешениями на запись следует удалять из подписки.
- Внешние учетные записи с разрешениями на чтение следует удалять из подписки.

## <a name="10-identity-and-authentication"></a>Идентификация и проверка подлинности (10)

Эта схема помогает ограничивать и контролировать права доступа, назначая определения [Политики Azure](../../../policy/overview.md) для аудита внешних учетных записей с правами владельца и (или) чтения и записи, учетных записей с правами владельца, чтения и (или) записи, которые не используют многофакторную проверку подлинности.

- MFA должна быть включена для учетных записей с разрешениями владельца в вашей подписке.
- MFA должна быть включена для учетных записей с разрешениями на запись в вашей подписке.
- MFA должна быть включена для учетных записей с разрешениями на чтение в вашей подписке.
- Внешние учетные записи с разрешениями владельца следует удалять из подписки.
- Внешние учетные записи с разрешениями на запись следует удалять из подписки.
- Внешние учетные записи с разрешениями на чтение следует удалять из подписки.

Эта схема назначает определения Политики Azure для аудита использования проверки подлинности Azure Active Directory для серверов SQL Server и Service Fabric. Использование проверки подлинности Azure Active Directory упрощает контроль разрешений и обеспечивает централизованное управление удостоверениями для пользователей баз данных и других служб Майкрософт.

- Администратор Azure Active Directory должен быть подготовлен для серверов SQL Server.
- Кластеры Service Fabric должны использовать для проверки подлинности клиента только Azure Active Directory.

Эта схема также назначает определения Политики Azure для аудита учетных записей, которые нужно проверять в первую очередь, например устаревшие учетные записи и внешние учетные записи. При необходимости можно запретить вход для учетных записей (или удалить их), что немедленно отменит все права доступа к ресурсам Azure.
Эта схема назначает два определения Политики Azure для аудита устаревших учетных записей, которые, возможно, требуется удалить.

- Устаревшие учетные записи следует удалять из подписки.
- Устаревшие учетные записи с разрешениями владельца следует удалять из подписки.
- Внешние учетные записи с разрешениями владельца следует удалять из подписки.
- Внешние учетные записи с разрешениями на запись следует удалять из подписки.

Эта схема также назначает определение Политики Azure для аудита разрешений в файле паролей виртуальной машины Linux и создания оповещений при их неправильной настройке. Этот механизм позволяет предпринять корректирующее действие, чтобы избежать компрометации структур проверки подлинности.

- Отображение результатов аудита виртуальных машин Linux без разрешений 0644 для файла passwd.

Эта схема помогает принудительно требовать надежные пароли, назначая определения Политики Azure для аудита виртуальных машин Windows, в которых не применяется минимальная надежность и другие требования к паролю. Определение виртуальных машин, которые нарушают политику надежности паролей, помогает выполнить корректирующие действия и обеспечить соблюдение требований политики для паролей всех учетных записей пользователей на виртуальных машинах.

- Отображение результатов аудита виртуальных машин Windows без параметра сложности пароля.
- Отображение результатов аудита виртуальных машин Windows с максимальным сроком действия пароля более 70 дней.
- Отображение результатов аудита виртуальных машин Windows с минимальным сроком действия пароля менее одного дня.
- Отображение результатов аудита виртуальных машин Windows с минимальной длиной пароля менее 14 символов.
- Отображение результатов аудита виртуальных машин Windows с возможностью повторного использования предыдущих 24 паролей.

Эта схема помогает управлять доступом к ресурсам Azure, назначая определения Политики Azure. для аудита использования типов и конфигураций ресурсов, которые предоставляют повышенные права доступа к ресурсам. Определение ресурсов, которые нарушают эти политики, помогает применить корректирующие действия и ограничить круг доступа к ресурсам Azure только авторизованными пользователями.

- Отображение результатов аудита виртуальных машин Linux с учетными записями без паролей.
- Отображение результатов аудита виртуальных машин Linux с разрешенным удаленным подключением учетных записей без паролей
- Необходимо перенести учетные записи хранения в новые ресурсы Azure Resource Manager.
- Необходимо перенести виртуальные машины в новые ресурсы Azure Resource Manager.
- Аудит виртуальных машин, которые не используют управляемые диски

## <a name="11-external-interface-protection"></a>Защита внешнего интерфейса (11)

Кроме использования более 25 политик для обеспечения надежного управления пользователями, эта схема также помогает защитить интерфейсы служб от несанкционированного доступа путем назначения определения [Политики Azure](../../../policy/overview.md), которое выполняет мониторинг учетных записей хранения с неограниченным доступом.
Использование учетных записей хранения с неограниченным доступом может привести к нежелательному доступу к данным, содержащимся в информационной системе. Эта схема также назначает политику, которая включает адаптивные элементы управления приложениями на виртуальных машинах.

- Учетные записи хранения должны ограничивать доступ к сети.
- На компьютерах должны быть включены адаптивные элементы управления приложениями для определения безопасных приложений.
- Доступ через подключенную к Интернету конечную точку должен быть ограничен.
- На виртуальных машинах с выходом в Интернет должны применяться рекомендации Адаптивной защиты сети.
- Решение защиты конечных точек должно быть установлено в масштабируемых наборах виртуальных машин.
- Порты управления виртуальных машин должны быть защищены с помощью JIT-управления доступом к сети
- Удаленная отладка должна быть отключена для приложений-функций.
- Удаленная отладка должна быть отключена для веб-приложений.
- Удаленная отладка должна быть отключена для приложений API.

## <a name="13-audit-information-for-users"></a>Данные аудита для пользователей (13)

Эта схема помогает обеспечить ведение журнала событий системы, назначая определения [Политики Azure](../../../policy/overview.md) для аудита параметров журналов для ресурсов Azure.
Также эта политика ведет аудит отправки виртуальными машинами журналов в указанную рабочую область Log Analytics.

- Расширенная защита данных должна быть включена на серверах SQL Server.
- Аудит параметров диагностики
- \[Предварительная версия.\] Развертывание агента Log Analytics для виртуальных машин Linux
- \[Предварительная версия.\] Развертывание агента Log Analytics для виртуальных машин Windows
- Развертывание наблюдателя сети при создании виртуальных сетей


## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы ознакомились с сопоставлением элементов управления схемы UK OFFICIAL и UK NHS, просмотрите общие сведения и сведения о развертывании этого примера по следующим ссылкам:

> [!div class="nextstepaction"]
> [Общие сведения о схемах UK OFFICIAL и UK NHS](./index.md)
> [Развертывание схем UK OFFICIAL и UK NHS](./deploy.md)

Дополнительные статьи о схемах и способах их использования:

- Ознакомьтесь со сведениями о [жизненном цикле схем](../../concepts/lifecycle.md).
- Узнайте, как использовать [статические и динамические параметры](../../concepts/parameters.md).
- Научитесь настраивать [последовательность схемы](../../concepts/sequencing-order.md).
- Узнайте, как применять [блокировку ресурсов схемы](../../concepts/resource-locking.md).
- Узнайте, как [обновлять существующие назначения](../../how-to/update-existing-assignments.md).
