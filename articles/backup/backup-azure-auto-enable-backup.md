---
title: Автоматическое включение резервного копирования при создании виртуальной машины с использованием Политики Azure
description: Статья, в которой описывается, как использовать политику Azure для автоматического включения резервного копирования для всех виртуальных машин, созданных в данной области.
ms.topic: conceptual
ms.date: 11/08/2019
ms.openlocfilehash: 78fe0ccdbf6f1cc3498d14530d7492a86e8bf730
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/19/2020
ms.locfileid: "92174067"
---
# <a name="auto-enable-backup-on-vm-creation-using-azure-policy"></a>Автоматическое включение резервного копирования при создании виртуальной машины с использованием Политики Azure

Одной из ключевых обязанностей администратора по резервному копированию или соответствию в Организации является обеспечение резервного копирования всех критически важных для бизнеса компьютеров с учетом соответствующего срока хранения.

Сейчас Azure Backup предоставляет встроенную политику (с помощью политики Azure), которую можно назначить **всем виртуальным машинам Azure в указанном расположении в рамках подписки или группы ресурсов**. Когда эта политика назначается данной области, все новые виртуальные машины, созданные в этой области, автоматически настраиваются для резервного копирования в **существующее хранилище в том же расположении и подписке**. Пользователь может указать хранилище и политику хранения, с которой должны быть связаны резервные виртуальные машины.

## <a name="supported-scenarios"></a>Поддерживаемые сценарии

* В настоящее время встроенная политика поддерживается только для виртуальных машин Azure. Пользователи должны соблюдать осторожность, чтобы политика хранения, указанная во время назначения, была политикой хранения виртуальной машины. Сведения о всех SKU виртуальных машин, поддерживаемых этой политикой, см. в [этом](./backup-azure-policy-supported-skus.md) документе.

* Политика может быть назначена одному расположению и подписке за раз. Чтобы включить резервное копирование для виртуальных машин в разных местах и подписках, необходимо создать несколько экземпляров назначения политики, по одному для каждого сочетания расположения и подписки.

* Указанное хранилище и виртуальные машины, для которых настроено резервное копирование, могут находиться в разных группах ресурсов.

* Область группы управления в настоящее время не поддерживается.

* Встроенная политика в настоящее время недоступна в национальных облаках.

[!INCLUDE [backup-center.md](../../includes/backup-center.md)]

## <a name="using-the-built-in-policy"></a>Использование встроенной политики

Чтобы назначить политику для требуемой области, выполните следующие действия:

1. Войдите в портал Azure и перейдите на панель мониторинга **политики** .
1. В меню слева выберите **определения** , чтобы получить список всех встроенных политик в ресурсах Azure.
1. Отфильтруйте список **категорий = резервное копирование**. Вы увидите список, отфильтрованный по одной политике с именем "Настройка резервного копирования на виртуальных машинах расположения в существующее центральное хранилище в том же расположении".
![Панель мониторинга политик](./media/backup-azure-auto-enable-backup/policy-dashboard.png)
1. Выберите имя политики. Вы будете перенаправлены к подробному определению этой политики.
![Панель определения политики](./media/backup-azure-auto-enable-backup/policy-definition-blade.png)
1. Нажмите кнопку **назначить** в верхней части панели. Будет перенаправлен на область **Назначение политики** .
1. В разделе **Основные сведения**выберите три точки рядом с полем **область** . Откроется правая область контекста, где можно выбрать подписку для применения политики. При необходимости можно также выбрать группу ресурсов, чтобы политика была применена только для виртуальных машин в определенной группе ресурсов.
![Основы назначения политик](./media/backup-azure-auto-enable-backup/policy-assignment-basics.png)
1. На вкладке **Параметры** выберите расположение из раскрывающегося списка и выберите хранилище и политику резервного копирования, с которой должны быть связаны виртуальные машины в области.
![Параметры назначения политики](./media/backup-azure-auto-enable-backup/policy-assignment-parameters.png)
1. Убедитесь, что для свойства **Effect** задано значение deployIfNotExists.
1. Перейдите к разделу **Просмотр и создание** и выберите **создать**.

> [!NOTE]
>
> Политику Azure также можно использовать на существующих виртуальных машинах, используя [исправление](../governance/policy/how-to/remediate-resources.md).

> [!NOTE]
>
> Рекомендуется, чтобы эта политика не была назначена более чем 200 виртуальным машинам за раз. Если политика назначена более чем 200 виртуальным машинам, это может привести к запуску резервного копирования через несколько часов позже, чем указано в расписании.

## <a name="next-steps"></a>Next Steps

[Дополнительные сведения о политике Azure](../governance/policy/overview.md)
