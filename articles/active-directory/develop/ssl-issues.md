---
title: Устранение проблем с SSL (MSAL iOS/macOS) | Службы
titleSuffix: Microsoft identity platform
description: Сведения о различных проблемах, связанных с использованием SSL-сертификатов с MSAL. Библиотека цели-C.
services: active-directory
documentationcenter: ''
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/28/2019
ms.author: twhitney
ms.reviewer: ''
ms.custom: aaddev
ms.openlocfilehash: fef1e1df15fed8452066f06a351452a83f73d89b
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2020
ms.locfileid: "76701354"
---
# <a name="how-to-troubleshoot-msal-for-ios-and-macos-ssl-issues"></a>Как устранять неполадки MSAL для устройств iOS и macOS SSL

В этой статье содержатся сведения, помогающие устранить проблемы, которые могут возникнуть при использовании [библиотеки проверки подлинности Майкрософт (MSAL) для iOS и macOS](reference-v2-libraries.md)

## <a name="network-issues"></a>Проблемы с сетью

**Ошибка-1200**: "произошла ошибка SSL, не удается установить безопасное соединение с сервером".

Эта ошибка означает, что соединение не защищено. Это происходит, когда сертификат недействителен. Дополнительные сведения, в том числе о том, какой сервер не проходит проверку SSL, см. в `NSURLErrorFailingURLErrorKey` в словаре `userInfo` объекта ошибки.

Эта ошибка связана с сетевой библиотекой Apple. Полный список кодов ошибок NSURL находится в Нсурлеррор. h в пакетах SDK для macOS и iOS. Дополнительные сведения об этой ошибке см. в разделе [URL-Загрузка кодов системных ошибок](https://developer.apple.com/documentation/foundation/1508628-url_loading_system_error_codes?language=objc).

## <a name="certificate-issues"></a>Проблемы с сертификатом

Если URL-адрес, предоставляющий недопустимый сертификат, подключается к серверу, который планируется использовать в процессе проверки подлинности, хороший Запуск диагностики проблемы заключается в проверке URL-адреса с помощью службы проверки SSL, такой как [Qualys SSL Labs Analyzer](https://www.ssllabs.com/ssltest/analyze.html). Он тестирует сервер на основе широкого спектра сценариев и браузеров и проверяет наличие многих известных уязвимостей.

По умолчанию новая функция [безопасности транспорта приложений Apple (ATS)](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW35) применяет более строгие политики безопасности к приложениям, использующим SSL-сертификаты. Некоторые операционные системы и веб-браузеры приступили к применению некоторых из этих политик по умолчанию. По соображениям безопасности рекомендуется не отключать ATS.

Сертификаты, использующие хэши SHA-1, имеют известные уязвимости. Большинство современных веб-браузеров не разрешают сертификаты с хэш SHA-1.

## <a name="captive-portals"></a>Порталы для пере(пере)

Портал перезаписи предоставляет пользователю веб-страницу при первом доступе к сети Wi-Fi и еще не предоставил доступ к этой сети. Он перехватывает трафик Интернета до тех пор, пока пользователь не будет удовлетворять требованиям портала. Сетевые ошибки, так как пользователь не может подключиться к сетевым ресурсам, пока пользователь не подключится через портал.

## <a name="next-steps"></a>Дальнейшие действия

Сведения о [порталах](https://en.wikipedia.org/wiki/Captive_portal) с пределом и новой функции [безопасности транспорта приложений Apple (ATS)](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW35) .
