---
title: Прокси приложения Azure AD и Qlik Sense | Документация Майкрософт
description: Сведения о включении прокси приложения на портале Azure и установке соединителей для обратного прокси-сервера.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: article
ms.date: 09/06/2018
ms.author: celested
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 12e36c49d8c4db60302d4b72dfa81ea4aceec77b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60441335"
---
# <a name="application-proxy-and-qlik-sense"></a>Прокси приложений и Qlik Sense 
Совместное использование Azure Active Directory Application Proxy и Qlik Sense упрощает использование прокси приложения для предоставления удаленного доступа к развертыванию Qlik Sense.  

## <a name="prerequisites"></a>Технические условия 
В оставшейся части этого сценария предполагается, что вы сделали следующее:
 
- Настроили [Qlik Sense](https://community.qlik.com/docs/DOC-19822). 
- [Установили соединитель прокси приложения.](application-proxy-add-on-premises-application.md#install-and-register-a-connector) 
 
## <a name="publish-your-applications-in-azure"></a>Публикация приложений в Azure 
Для публикации QlikSense необходимо опубликовать два приложения в Azure.  

### <a name="application-1"></a>Приложение № 1 
Выполните следующие действия, чтобы опубликовать приложение. Более подробное пошаговое руководство по этапам 1–8 приведено в разделе [Публикация приложений с помощью прокси приложения Azure AD](application-proxy-add-on-premises-application.md). 


1. Войдите на портал Azure как глобальный администратор. 
2. Выберите **Azure Active Directory** > **Корпоративные приложения**. 
3. Щелкните **Добавить** в верхней части колонки. 
4. Выберите **Локальное приложение**. 
5. Введите в обязательные поля сведения о новом приложении. Вам нужно настроить следующие параметры. 
   - **Внутренний URL-адрес**. Это приложение должно использовать внутренний URL-адрес, которым является URL-адрес Qlik Sense. Пример: **https&#58;//demo.qlikemm.com:4244**. 
   - **Метод предварительной аутентификации**. Azure Active Directory (рекомендуется, но не обязательно). 
1. Щелкните **Добавить** в нижней части колонки. Когда приложение будет добавлено, откроется меню быстрого запуска. 
2. В меню быстрого запуска выберите **Назначить пользователя для тестирования**, а затем назначьте для приложения хотя бы одного пользователя. Убедитесь, что у тестовой учетной записи есть доступ к локальному приложению. 
3. Щелкните **Назначить**, чтобы сохранить назначение тестового пользователя. 
4. (Необязательно.) В колонке управления приложением щелкните "Единый вход". Выберите **Ограниченное делегирование Kerberos** в раскрывающемся меню и заполните обязательные поля на основе конфигурации Qlik. Щелкните **Сохранить**. 

### <a name="application-2"></a>Приложение № 2 
Выполните те же действия, что и для приложения № 1, за исключением следующих моментов: 

**Шаг 5**. Внутренний URL-адрес теперь должен быть URL-адресом Qlik Sense с портом аутентификации, который используется приложением. Значение по умолчанию — **4244** для HTTPS и 4248 для HTTP. Пример: **https&#58;//demo.qlik.com:4244**.</br></br> 
**Шаг 10**. Не настраивайте единый вход и оставьте параметр **Единый вход** отключенным.
 
 
## <a name="testing"></a>Тестирование 
Теперь приложение готово к тестированию. Перейдите по внешнему URL-адресу, который использовался для публикации QlikSense в приложении № 1, и войдите в систему как пользователь, назначенный для обоих приложений.  

## <a name="additional-references"></a>Дополнительные ссылки
Дополнительные сведения о публикации Qlik Sense с помощью прокси приложения см. в статье сообщества Qlik: [Azure AD with Integrated Windows Authentication using a Kerberos Constrained Delegation with Qlik Sense](https://community.qlik.com/docs/DOC-20183) (Использование Azure AD со встроенной проверкой подлинности Windows с помощью ограниченного делегирования Kerberos посредством Qlik Sense).

## <a name="next-steps"></a>Дальнейшие действия

- [Опубликуйте приложения с помощью прокси-сервера приложений.](application-proxy-add-on-premises-application.md)
- [Работа с соединителями прокси приложения](application-proxy-connector-groups.md)

