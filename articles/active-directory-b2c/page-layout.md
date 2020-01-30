---
title: Выберите макет страницы — Azure Active Directory B2C
description: Узнайте, как выбрать макет страницы в Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 452687f3886a85bea796e3899410667ee1d592fa
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76840321"
---
# <a name="select-a-page-layout-in-azure-active-directory-b2c-using-custom-policies"></a>Выбор макета страницы в Azure Active Directory B2C с помощью настраиваемых политик

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Вы можете включить клиентский код JavaScript в политиках Azure Active Directory B2C (Azure AD B2C) независимо от того, используете ли вы пользовательские потоки или пользовательские политики. Чтобы включить JavaScript для приложений, необходимо добавить элемент в [настраиваемую политику](custom-policy-overview.md), выбрать макет страницы и использовать [b2clogin.com](b2clogin.md) в запросах.

Макет страницы — это ассоциация элементов, которые Azure AD B2C предоставляет и предоставленное вами содержимое.

В этой статье описано, как выбрать макет страницы в Azure AD B2C, настроив его в пользовательской политике.

> [!NOTE]
> Если вы хотите включить JavaScript для потоков пользователей, см. раздел [версии JavaScript и страницы в Azure Active Directory B2C](user-flow-javascript-overview.md).

## <a name="replace-datauri-values"></a>Замена значений DataUri

В настраиваемых политиках могут быть элементы [ContentDefinitions](contentdefinitions.md), которые определяют шаблоны HTML, используемые в пути взаимодействия пользователя. **ContentDefinition** содержит **DataUri**, ссылающийся на элементы страницы, предоставляемые Azure AD B2C. **LoadUri** — это относительный путь к содержимому HTML и CSS, которое вы предоставляете.

```XML
<ContentDefinition Id="api.idpselections">
  <LoadUri>~/tenant/default/idpSelector.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Idp selection page</Item>
    <Item Key="language.intro">Sign in</Item>
  </Metadata>
</ContentDefinition>
```

Чтобы выбрать макет страницы, измените значения **DataUri** в [ContentDefinitions](contentdefinitions.md) в политиках. Перейдя от старых значений **DataUri** к новым значениям, вы выбираете неизменяемый пакет. Преимущество использования этого пакета заключается в том, что вы понимаете, что он не изменяется и вызывает непредвиденное поведение на странице.

Чтобы указать макет страницы в пользовательских политиках, использующих старое значение **DataUri** , вставьте `contract` между `elements` и типом страницы (например, `selfasserted`) и укажите номер версии. Пример.

| Старое значение DataUri | Новое значение DataUri |
| ----------------- | ----------------- |
| `urn:com:microsoft:aad:b2c:elements:claimsconsent:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:claimsconsent:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:idpselection:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssd:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssd:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.1.0` |

## <a name="version-change-log"></a>Журнал изменений версий

Пакеты макета страницы периодически обновляются для включения исправлений и улучшений в элементы страницы. В следующем журнале изменений указаны изменения, появившиеся в каждой версии.

### <a name="200"></a>2.0.0

- Страница с автоматическим подтверждением (`selfasserted`)
  - Добавлена поддержка [элементов управления отображением](display-controls.md) в пользовательских политиках.

### <a name="120"></a>1.2.0

- Все страницы
  - Исправления специальных возможностей
  - Теперь можно добавить атрибут `data-preload="true"` в теги HTML для управления порядком загрузки CSS и JavaScript. Сценарии включают:
    - Используйте его для ссылки CSS, чтобы загрузить CSS в то же время, что и код HTML, чтобы избежать мерцания при загрузке файлов.
    - Этот атрибут позволяет управлять порядком выборки и выполнения тегов скрипта до загрузки страницы.
  - Поле электронной почты теперь `type=email`, а мобильные клавиатуры будут предоставлять правильные варианты
  - Поддержка преобразования Chrome
- Унифицированная и самостоятельно подтвержденная страница
  - Поля имя пользователя и адрес электронной почты и пароль теперь используют HTML-элемент Form.  Теперь пограничным и IE будет разрешено правильно сохранять эти сведения

### <a name="110"></a>1.1.0

- Страница "исключения" (глобалексцептион)
  - Исправление специальных возможностей
  - Удалено сообщение по умолчанию, если в политике нет контакта
  - CSS по умолчанию удален
- Страница MFA (многофакторная)
  - Кнопка "подтвердить код" удалена
  - Поле ввода для кода теперь принимает только до шести (6) символов.
  - Страница будет автоматически пытаться проверить код, указанный при входе в 6-значный код, без нажатия кнопки
  - Если код неправильный, поле ввода автоматически снимается
  - После трех (3) попыток с неверным кодом B2C отправляет ошибку проверяющей стороне.
  - Исправления специальных возможностей
  - CSS по умолчанию удален
- Страница с автоматическим подтверждением (selfasserted)
  - Удалено оповещение об отмене
  - Класс CSS для элементов error
  - Улучшена функция отображения или скрытия логики ошибок
  - CSS по умолчанию удален
- Унифицированный поставщик общих служб (унифиедссп)
  - Добавлен элемент управления "оставаться в системе" (функции "оставаться)

### <a name="100"></a>1.0.0

- Начальный выпуск

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о настройке пользовательского интерфейса приложений см. в статье [Настройка пользовательского интерфейса приложения с помощью настраиваемой политики в Azure Active Directory B2C](custom-policy-ui-customization.md).
