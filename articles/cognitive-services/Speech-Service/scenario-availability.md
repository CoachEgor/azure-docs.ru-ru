---
title: Доступность сценария — служба речи
titleSuffix: Azure Cognitive Services
description: В пакете SDK для речевых функций реализовано множество сценариев в различных языках программирования и средах. Не все сценарии доступны во всех языках программирования или во всех средах. Ниже приведена доступность каждого сценария.
services: cognitive-services
author: chrisbasoglu
manager: xdh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: cbasoglu
ms.openlocfilehash: 6f8f892e7ca81881b0cc00e1708e3f05052c573c
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/17/2020
ms.locfileid: "76264209"
---
# <a name="scenario-availability"></a>Доступность сценариев

В пакете SDK для речевых функций реализовано множество сценариев в различных языках программирования и средах. Не все сценарии доступны во всех языках программирования или во всех средах. Ниже приведена доступность каждого сценария.

- **Распознавание речи (SR), список фраз, намерения, перевод и локальные контейнеры**
  - Все языки программирования и среды, в которых есть ссылка со стрелкой <img src="media/index/link.jpg" height="15" width="15"></img> в таблице краткого руководства [здесь](https://aka.ms/csspeech).
- **Преобразование текста в речь (TTS)**
  - C++/Виндовс & Linux
  - C#/Виндовс & UWP & Unity
  - Java (JRE и Android)
  - Python
  - Swift
  - Objective-C
  - REST API TTS можно использовать в любой другой ситуации.
- **Заданное ключевое слово (КВС)**
  - C++/Виндовс & Linux
  - C#/Виндовс & Linux
  - Python и Windows & Linux
  - Java и Windows & Linux & Android (пакет SDK для речевых устройств)
  - Функция поиска ключевых слов (КВС) может работать с любым типом микрофона, официальная поддержка КВС, однако, в настоящее время ограничена только массивами микрофонов, находящихся на оборудовании Azure Kinect DK или в пакете SDK для речевых устройств.
- **Голосовые помощники**
  - C++/Виндовс & Linux & macOS
  - C#/виндовс
  - Java/Windows & Linux & macOS & Android (пакет SDK для речевых устройств)
- **Транскрибирование бесед**
  - C++/Виндовс & Linux
  - C#(Платформа & .NET Core)/Виндовс & UWP & Linux
  - Java и Windows & Linux & Android (пакет SDK для речевых устройств)
- **Беседа с несколькими устройствами**
  - C++/виндовс
  - C#(Платформа & .NET Core)/Виндовс
- **Транскрипция центра обработки вызовов**
  - REST API и могут использоваться в любой ситуации.
- **Звуковые входные данные, сжатые кодеком**
  - C++/линукс
  - C#/линукс
  - Java, Linux, Android и iOS
