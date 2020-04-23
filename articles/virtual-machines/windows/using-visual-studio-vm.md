---
title: Использование Visual Studio на виртуальной машине Azure
description: Использование Visual Studio на виртуальной машине Azure.
author: cathysull
manager: cathys
ms.service: virtual-machines-windows
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/04/2019
ms.author: cathys
keywords: visualstudio
ms.openlocfilehash: 15de6a9aa7e49f680dcd953937d6b12cb9e16d4a
ms.sourcegitcommit: 086d7c0cf812de709f6848a645edaf97a7324360
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2020
ms.locfileid: "82101609"
---
# <a name="visual-studio-images-on-azure"></a>Образы Visual Studio в Azure
Запуск Visual Studio на предварительно настроенной виртуальной машине Azure — простой и быстрый способ получить "с нуля" готовую к работе среду разработки. Образы системы с различными конфигурациями Visual Studio доступны в [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/compute?filters=virtual-machine-images%3Bmicrosoft%3Bwindows&page=1&subcategories=application-infrastructure).

Впервые работаете с Azure? [Создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free).

> [!NOTE]
> Не все подписки подходят для развертывания образов Windows 10. Дополнительные сведения см. [в статье Использование клиента Windows в Azure для сценариев разработки и тестирования](https://docs.microsoft.com/azure/virtual-machines/windows/client-images) .

## <a name="what-configurations-and-versions-are-available"></a>Какие конфигурации и версии доступны?
Образы большинства последних основных версий (Visual Studio 2019, Visual Studio 2017 и Visual Studio 2015) можно найти в Azure Marketplace.  Для каждой выпущенной основной версии доступна исходная версия "выпущено для веб" (RTW) и последние обновленные версии.  Для каждой из этих версий доступны выпуски Visual Studio Enterprise и Visual Studio Community.  Эти образы обновляются по крайней мере раз в месяц для включения последних обновлений Visual Studio и Windows.  Хотя имена образов не меняются, описание каждого образа содержит версию установленного продукта и дату выпуска образа.

| Версия выпуска                                                                                                                                                | Выпуски              | Версия продукта   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------:|:---------------------:|:-----------------:|
| [Visual Studio 2019: Последняя (версия 16,4)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftvisualstudio.visualstudio2019latest?tab=Overview) | Enterprise, Community | Версия 16.4.0    |
| [Visual Studio 2019: RTW](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftvisualstudio.visualstudio2019?tab=Overview)                         | Enterprise, Community | Версия 16.0.9    |
| [Visual Studio 2017: последняя версия (15.9)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftvisualstudio.visualstudio?tab=Overview)           | Enterprise, Community | Версия 15.9.17   |
| [Visual Studio 2017 RTW](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftvisualstudio.visualstudio?tab=Overview)                             | Enterprise, Community | Версия 15.0.27  |
| [Последняя версия Visual Studio 2015 (обновление 3)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftvisualstudio.visualstudio?tab=Overview)               | Enterprise, Community | Версия 14.0.25431.01 |

> [!NOTE]
> В соответствии с политикой обслуживания корпорации Майкрософт истек срок обслуживания изначально выпущенной версии (RTW) Visual Studio 2015. Visual Studio 2015 с обновлением 3 — единственная предлагаемая версия в линейке продуктов Visual Studio 2015.

Дополнительные сведения см. в разделе [Обслуживание продуктов Visual Studio и Team Foundation Server](https://www.visualstudio.com/productinfo/vs-servicing-vs).

## <a name="what-features-are-installed"></a>Какие компоненты устанавливаются?
Каждый образ содержит рекомендуемый набор компонентов для данного выпуска Visual Studio. Как правило, установка включает в себя:

* все доступные рабочие нагрузки, включая рекомендуемые дополнительные компоненты для них;
* пакеты SDK для .NET 4.6.2 и .NET 4.7, пакеты нацеливания и средства для разработчиков;
* Visual F#;
* расширение GitHub для Visual Studio;
* инструменты LINQ to SQL.

Командная строка, используемая для установки Visual Studio при создании образов:

```
    vs_enterprise.exe --allWorkloads --includeRecommended --passive ^
       add Microsoft.Net.Component.4.7.SDK ^
       add Microsoft.Net.Component.4.7.TargetingPack ^ 
       add Microsoft.Net.Component.4.6.2.SDK ^
       add Microsoft.Net.Component.4.6.2.TargetingPack ^
       add Microsoft.Net.ComponentGroup.4.7.DeveloperTools ^
       add Microsoft.VisualStudio.Component.FSharp ^
       add Component.GitHub.VisualStudio ^
       add Microsoft.VisualStudio.Component.LinqToSql
```

Если образ не содержит требуемый компонент Visual Studio, сообщите об этом с помощью инструмента обратной связи в правом верхнем углу страницы.

## <a name="what-size-vm-should-i-choose"></a>Какой размер виртуальной машины следует выбрать?
Azure предлагает широкий диапазон размеров виртуальных машин. Так как Visual Studio является мощным многопоточным приложением, вам нужен размер виртуальной машины, в которой предусмотрено по крайней мере два процессора и 7 ГБ памяти. Ниже приведены рекомендуемые размеры виртуальных машин для образов Visual Studio:

   * Standard_D2_v3
   * Standard_D2s_v3
   * Standard_D4_v3
   * Standard_D4s_v3
   * Standard_D2_v2
   * Standard_D2S_v2
   * Standard_D3_v2
    
Дополнительные сведения о последних размерах виртуальных машин см. в статье [Размеры виртуальных машин Windows в Azure](/azure/virtual-machines/windows/sizes).

Благодаря Azure первый выбранный вариант всегда можно заново сбалансировать, изменив размер виртуальной машины. Можно подготовить новую виртуальную машину, размер которой лучше подходит для ваших задач, или изменить размер существующей виртуальной машины, выбрав другое базовое оборудование. Дополнительные сведения см. в статье [Изменение размера виртуальной машины Windows](/azure/virtual-machines/windows/resize-vm).

## <a name="after-the-vm-is-running-whats-next"></a>Что делать после запуска виртуальной машины?
Visual Studio реализует модель использования собственной лицензии в Azure. Как и при установке закрытого оборудования, одним из первых шагов является лицензирование установленного экземпляра Visual Studio. Чтобы разблокировать Visual Studio, выполните одно из следующих действий:
- выполните вход с учетной записью Майкрософт, связанной с подпиской Visual Studio; 
- введите ключ продукта, полученный при покупке Visual Studio.

Дополнительные сведения см. в разделах [Выполните вход в Visual Studio](/visualstudio/ide/signing-in-to-visual-studio) и [Практическое руководство. Разблокирование Visual Studio](/visualstudio/ide/how-to-unlock-visual-studio).

## <a name="how-do-i-save-the-development-vm-for-future-or-team-use"></a>Как сохранить виртуальную машину разработки для использования в будущем или использования командой?

Разнообразие сред разработки очень велико, и создание более сложных сред требует реальных затрат. Независимо от конфигурации вашей среды, вы можете сохранить или записать настроенную виртуальную машину в качестве "базового образа" для использования в будущем или для использования другими членами вашей команды. Затем при загрузке новой виртуальной машины ее можно будет подготовить из базового образа, а не из образа Azure Marketplace.

Вкратце, используйте инструмент Sysprep и завершите работу запущенной виртуальной машины. Затем запишите *(рисунок 1)* ее как образ с помощью пользовательского интерфейса портала Azure. Azure сохранит файл с расширением `.vhd`, содержащий образ, в учетной записи хранения на ваш выбор. Затем новый образ появится в виде ресурса образа в списке ресурсов вашей подписки.

<img src="media/using-visual-studio-vm/capture-vm.png" alt="Capture an image through the Azure portal UI" style="border:3px solid Silver; display: block; margin: auto;"><center>*Рисунок 1. Запись образа с помощью пользовательского интерфейса портала Azure*</center>

Дополнительные сведения см. в статье [Создание управляемого образа универсальной виртуальной машины в Azure](/azure/virtual-machines/windows/capture-image-resource).

> [!IMPORTANT]
> Не забудьте подготовить виртуальную машину с помощью Sysprep. Если пропустить этот шаг, то Azure не удастся подготовить виртуальную машину из образа.

> [!NOTE]
> За хранение этих образов будет взиматься плата, но эти дополнительные затраты будут незначительными по сравнению с дополнительными затратами, необходимыми для повторной сборки виртуальной машины "с нуля" для каждого пользователя в группе, которому она нужна. Например, создание и хранение в течение месяца образа размером 127 ГБ, который может повторно использовать вся ваша команда, обойдется всего в несколько долларов. Однако эти затраты незначительны по сравнению с часами, затрачиваемыми каждым сотрудником на сборку и проверку правильно настроенной среды разработки, предназначенной для индивидуального использования.

Кроме того, для ваших задач разработки или технологий может понадобиться больший масштаб. Например, это могут быть разнообразные конфигурации разработки и конфигурации с несколькими виртуальными машинами. Вы можете использовать Azure DevTest Labs, чтобы создать _файлы инструкций_, автоматизирующие создание вашего "золотого образа", а также управлять политиками для работающих виртуальных машин вашей команды. Статья [Использование Azure DevTest Labs для разработчиков](/azure/devtest-lab/devtest-lab-developer-lab) — лучший источник дополнительных сведений о DevTest Labs.

## <a name="next-steps"></a>Дальнейшие действия
Теперь, когда вы знаете о предварительно настроенных образах Visual Studio, следующим шагом является создание виртуальной машины.

* [Создание виртуальной машины с помощью портала Azure](quick-create-portal.md)
* [Обзор виртуальных машин Windows](overview.md)
