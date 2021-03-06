---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 95bd83575809f6ecda716ff751b47b7bb499cae3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "85073222"
---
## <a name="configure-an-environment-variable-for-authentication"></a>Настройка переменной среды для проверки подлинности

Приложения должны проверять подлинность доступа к Cognitive Services, которые они используют. Для проверки подлинности рекомендуется создать переменную среды для хранения ключей ресурсов Azure. 

После создания ключа запишите его в новую переменную среды на локальном компьютере, на котором выполняется приложение. Чтобы задать переменную среды, откройте окно консоли и следуйте инструкциям для используемой операционной системы. Замените `your-key` на один из ключей для ресурса.

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx COGNITIVE_SERVICE_KEY "your-key"
```

После добавления переменной среды может потребоваться перезапустить все запущенные приложения, которым может понадобиться считать переменную среды, в том числе окно консоли. Например, если вы используете Visual Studio в качестве редактора, перезапустите Visual Studio перед запуском примера.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export COGNITIVE_SERVICE_KEY=your-key
```

После добавления переменной среды запустите `source ~/.bashrc` из окна консоли, чтобы применить изменения.

#### <a name="macos"></a>[macOS](#tab/unix)

Измените .bash_profile и добавьте переменную среды.

```bash
export COGNITIVE_SERVICE_KEY=your-key
```

После добавления переменной среды запустите `source ~/.bash_profile` из окна консоли, чтобы применить изменения.

***
