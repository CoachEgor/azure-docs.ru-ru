---
title: Использование редактора Azure Cloud Shell | Документация Майкрософт
description: Общие сведения о том, как использовать редактор Azure Cloud Shell.
services: azure
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: damaerte
ms.openlocfilehash: 7f597bb5cba1a12bdb93325fe2b877ffc644e3e4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "60199234"
---
# <a name="using-the-azure-cloud-shell-editor"></a>Использование редактора Azure Cloud Shell

Azure Cloud Shell включает в себя встроенный редактор файлов, созданный на основе [редактора Monaco](https://github.com/Microsoft/monaco-editor) с открытым исходным кодом. Редактор Cloud Shell поддерживает такие функции, как выделение языков, палитра команд и проводник файлов.

![Редактор Cloud Shell](media/using-cloud-shell-editor/open-editor.png)

## <a name="opening-the-editor"></a>Открытие редактора

Для простого создания и редактирования файлов запустите редактор, выполнив команду `code .` в терминале Cloud Shell. Это действие открывает редактор с активным рабочим каталогом, установленным в терминале.

Чтобы открыть файл для быстрого редактирования напрямую, выполните команду `code <filename>` и откройте редактор без проводника.

Чтобы открыть редактор с помощью кнопки в пользовательском интерфейсе, щелкните значок редактора `{}` на панели инструментов. Откроется редактор, и в обозревателе файлов по умолчанию будет установлен каталог `/home/<user>`.

## <a name="closing-the-editor"></a>Закрытие редактора

Чтобы закрыть редактор, откройте панель действий `...` в верхней правой части окна редактора и выберите `Close editor`.

![Закрытие редактора](media/using-cloud-shell-editor/close-editor.png)

## <a name="command-palette"></a>Палитра команд

Чтобы запустить палитру команду, используйте ключ `F1`, когда фокус установлен на редакторе. Открыть палитру команд также можно на панели действий.

![Палитра команд](media/using-cloud-shell-editor/cmd-palette.png)

## <a name="contributing-to-the-monaco-editor"></a>Участие в развитии редактора Monaco

Поддержка выделения кода в редакторе Cloud Shell реализуется с помощью функций верхнего уровня при использовании определений синтаксиса Monarch в [редакторе Monaco](https://github.com/Microsoft/monaco-editor). Чтобы узнать, как внести свой вклад, перейдите к этому [руководству по Monaco](https://github.com/Microsoft/monaco-editor/blob/master/CONTRIBUTING.md).

## <a name="next-steps"></a>Дальнейшие действия
[Попробуйте краткое руководство по bash в Cloud Shell](quickstart.md) 
 [Просмотрите полный список интегрированных средств Cloud Shell](features.md)