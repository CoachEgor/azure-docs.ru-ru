---
title: Настройка гибридного присоединения к Azure Active Directory для управляемых доменов | Документация Майкрософт
description: Сведения о том, как настроить гибридное присоединение к Azure Active Directory для управляемых доменов.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: tutorial
ms.date: 03/06/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 303f02e0c6b72b7061a996b3ce8e70799954b435
ms.sourcegitcommit: 21c3363797fb4d008fbd54f25ea0d6b24f88af9c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/08/2020
ms.locfileid: "96861057"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-managed-domains"></a>Руководство по Настройка гибридного присоединения к Azure Active Directory для управляемых доменов

Из этого учебника вы узнаете, как настроить гибридное присоединение к Azure Active Directory (Azure AD) для устройств, подключенных к домену Active Directory. Этот метод поддерживает управляемую среду, которая включает в себя локальную службу Active Directory и Azure AD.

Подобно пользователю в вашей организации, устройство — это еще одно основное удостоверение, которое необходимо защитить. Удостоверение устройства можно использовать для защиты ресурсов в любое время и из любого расположения. Этой цели можно достичь, используя управление удостоверениями устройств в Azure AD. Используйте один из следующих методов.

- присоединение к Azure AD;
- гибридное присоединение к Azure AD;
- регистрация в Azure AD.

В этой статье рассматривается гибридное присоединение к Azure AD.

Размещение устройств в Azure AD позволяет увеличить эффективность работы пользователей благодаря выполнению единого входа для облачных и локальных ресурсов. В то же время вы можете защитить доступ к облачным и локальным ресурсам с помощью [условного доступа](../conditional-access/howto-conditional-access-policy-compliant-device.md).

Вы можете развернуть управляемую среду, используя [синхронизацию хэша паролей](../hybrid/whatis-phs.md) или [сквозную аутентификацию](../hybrid/how-to-connect-pta.md) с [простым единым входом](../hybrid/how-to-connect-sso.md). В этих сценариях не требуется настраивать сервер федерации для проверки подлинности.

В этом руководстве описано следующее:

> [!div class="checklist"]
> * Настройка гибридного присоединения к Azure AD.
> * Включение устройств Windows нижнего уровня.
> * Проверка присоединенных устройств.
> * Диагностика

## <a name="prerequisites"></a>Предварительные требования

- [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) (1.1.819.0 или более поздних версий);
- учетные данные глобального администратора для клиента Azure AD;
- учетные данные администратора предприятия для каждого леса;

Ознакомьтесь со следующими статьями:

- [Что такое удостоверение устройства?](overview.md)
- [Руководство. Планирование реализации гибридного присоединения к Azure Active Directory](hybrid-azuread-join-plan.md).
- [Контролируемая проверка гибридного присоединения к Azure AD](hybrid-azuread-join-control.md).

> [!NOTE]
> Azure AD не поддерживает смарт-карты и сертификаты в управляемых доменах.

Проверьте, синхронизированы ли в Azure AD Connect объекты-компьютеры гибридных устройств, которые нужно присоединить к Azure AD. Если объекты-компьютеры принадлежат конкретным подразделениям, для этих подразделений нужно настроить синхронизацию в Azure AD Connect. Дополнительные сведения о том, как синхронизировать объекты-компьютеры с помощью Azure AD Connect, см. в разделе [Фильтрация по подразделениям](../hybrid/how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering).

Начиная с версии 1.1.819.0, с Azure AD Connect предоставляется мастер для настройки гибридного присоединения к Azure AD. Этот мастер значительно упрощает настройку. Мастер настраивает точки подключения службы (SCP) для регистрации устройств.

Действия по настройке в этой статье основаны на использовании мастера в Azure AD Connect.

Для гибридного присоединения к Azure AD требуется, чтобы у устройств был доступ к следующим ресурсам Майкрософт из сети организации:  

- `https://enterpriseregistration.windows.net`
- `https://login.microsoftonline.com`
- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com` (если вы используете или планируете использовать простой единый вход).

> [!WARNING]
> Если ваша организация использует прокси-серверы, которые перехватывают трафик SSL для таких сценариев, как защита от потери данных или ограничения арендатора Azure AD, убедитесь, что трафик к https://device.login.microsoftonline.com исключается из процесса приостановки и изучения TLS-трафика. Если адрес https://device.login.microsoftonline.com не будет исключен, это может вызвать ошибки при аутентификации с помощью сертификата клиента, что приведет к проблемам с регистрацией устройств и условным доступом на основе устройств.

Если вашей организации требуется доступ к Интернету через исходящий прокси-сервер, вы можете [реализовать автоматическое обнаружение веб-прокси (WPAD)](/previous-versions/tn-archive/cc995261(v=technet.10)), чтобы обеспечить регистрацию устройств в Azure AD на компьютерах Windows 10. Если возникли проблемы при настройке и управлении WPAD, см. статью об устранении неполадок с автоматическим обнаружением [здесь](/previous-versions/tn-archive/cc302643(v=technet.10)). На устройствах с ОС Windows 10 версии ниже 1709 WPAD является единственным доступным вариантом, позволяющим настроить прокси-сервер для работы с гибридным присоединением к Azure AD. 

Если вы не используете WPAD, параметры прокси-сервера WinHTTP можно настроить на компьютере с Windows 10 версии не ниже 1709. Дополнительные сведения см. в статье [Развертывание параметров прокси-сервера WinHTTP с помощью объекта групповой политики](/archive/blogs/netgeeks/winhttp-proxy-settings-deployed-by-gpo).

> [!NOTE]
> Если вы настроите параметры прокси-сервера на компьютере с помощью параметров WinHTTP, любые компьютеры, которым не удается подключиться к настроенному прокси-серверу, не смогут подключиться к Интернету.

Если вашей организации требуется доступ в Интернет через исходящий прокси-сервер с аутентификацией, необходимо убедиться, что ваши компьютеры с Windows 10 могут успешно пройти проверку подлинности на этом прокси-сервере. Так как компьютеры с Windows 10 выполняют регистрацию устройства с использованием контекста компьютера, необходимо настроить проверку подлинности для исходящего прокси-сервера с использованием контекста компьютера. Обратитесь к поставщику исходящего прокси-сервера, чтобы узнать требования к конфигурации.

Проверить, есть ли у устройства доступ к указанным выше ресурсам Майкрософт под системной учетной записью, можно с помощью скрипта [проверки подключения при регистрации устройств](https://gallery.technet.microsoft.com/Test-Device-Registration-3dc944c0).

## <a name="configure-hybrid-azure-ad-join"></a>Настройка гибридного присоединения к Azure AD.

Чтобы настроить гибридное присоединение к Azure AD с помощью Azure AD Connect, сделайте следующее:

1. Запустите Azure AD Connect и выберите **Настройка**.

   ![Экран приветствия](./media/hybrid-azuread-join-managed-domains/welcome-azure-ad-connect.png)

1. На странице **Дополнительные задачи** выберите раздел **Настройка параметров устройств**, а затем нажмите кнопку **Далее**.

   ![Дополнительные задачи](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-additional-tasks.png)

1. В разделе **Обзор** нажмите кнопку **Далее**.

   ![Обзор](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-overview.png)

1. На странице **Подключение к Azure AD** укажите учетные данные глобального администратора для клиента Azure AD.  

   ![Подключение к Azure AD](./media/hybrid-azuread-join-managed-domains/connect-to-azure-ad-username-password.png)

1. На странице **Device options** (Параметры устройств) установите параметр **Настроить гибридное присоединение к Azure AD** и нажмите кнопку **Далее**.

   ![Параметры устройств](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-device-options.png)

1. На странице **Конфигурация точки подключения службы** выполните приведенные ниже действия с каждым лесом, для которого требуется настроить точку подключения службы с помощью Azure AD Connect, а затем нажмите кнопку **Далее**.

   1. Щелкните **Лес**.
   1. Щелкните **Служба проверки подлинности**.
   1. Выберите **Добавить**, чтобы ввести учетные данные администратора предприятия.

   ![SCP](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-scp-configuration.png)

1. На странице **Операционные системы устройств** выберите операционные системы, используемые устройствами в вашей среде Active Directory, и нажмите кнопку **Далее**.

   ![Операционная система устройства](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-device-operating-systems.png)

1. На странице **Готово к настройке** нажмите кнопку **Настроить**.

   ![Готово к настройке](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-ready-to-configure.png)

1. На странице **Конфигурация завершена** нажмите кнопку **Выйти**.

   ![Завершение настройки](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-configuration-complete.png)

## <a name="enable-windows-down-level-devices"></a>Включение устройств Windows нижнего уровня.

Если некоторые из присоединенных к домену устройств являются устройствами Windows нижнего уровня, выполните указанные ниже действия.

- Настройка параметров локальной интрасети для регистрации устройств.
- Настройка простого единого входа
- Установка Microsoft Workplace Join для компьютеров Windows нижнего уровня

> [!NOTE]
> Поддержка Windows 7 закончилась 14 января 2020 г. Дополнительные сведения см. в статье [Поддержка Windows 7 закончилась 14 января 2020 г.](https://support.microsoft.com/help/4057281/windows-7-support-ended-on-january-14-2020)

### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Настройка параметров локальной интрасети для регистрации устройств.

Чтобы успешно выполнить гибридное присоединение устройств Windows нижнего уровня к Azure AD и избежать запросов на предоставление сертификатов при аутентификации этих устройств в Azure AD, можно ввести политику для присоединенных к домену устройств, предусматривающую добавление следующих URL-адресов в зону локальной интрасети в Internet Explorer:

- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com`

Необходимо также включить параметр **Разрешить обновление строки состояния в сценарии** в локальной зоне интрасети пользователя.

### <a name="configure-seamless-sso"></a>Настройка простого единого входа

Для успешного завершения гибридного присоединения к Azure AD устройств Windows нижнего уровня в управляемом домене, использующем [синхронизацию хэша паролей](../hybrid/whatis-phs.md) или [сквозную проверку подлинности](../hybrid/how-to-connect-pta.md) для проверки подлинности в Azure AD, необходимо также [настроить простой единый вход](../hybrid/how-to-connect-sso-quick-start.md#step-2-enable-the-feature).

### <a name="install-microsoft-workplace-join-for-windows-down-level-computers"></a>Установка Microsoft Workplace Join для компьютеров Windows нижнего уровня

Чтобы зарегистрировать устройства Windows нижнего уровня, необходимо установить в организации [Microsoft Workplace Join для компьютеров, на которых не используется Windows 10](https://www.microsoft.com/download/details.aspx?id=53554). Microsoft Workplace Join для компьютеров не на базе Windows 10 доступна в центре загрузки Майкрософт.

Развернуть пакет можно с помощью системы распространения программного обеспечения, например  [Microsoft Endpoint Configuration Manager](/configmgr/). Этот пакет поддерживает параметры стандартной автоматической установки с использованием параметра `quiet`. В текущей версии Configuration Manager доступны дополнительные преимущества предыдущих версий, такие как возможность отслеживать ход регистрации.

Установщик создает в системе запланированную задачу, которая выполняется в контексте пользователя. Задача запускается в момент входа пользователя в систему Windows. Эта задача автоматически присоединяет устройство к Azure AD, используя учетные данные пользователя, после аутентификации с помощью AD.

## <a name="verify-the-registration"></a>Проверка регистрации

Ниже приведены три способа определения и проверки состояния устройства.

### <a name="locally-on-the-device"></a>Локально на устройстве

1. Откройте Windows PowerShell.
2. Введите `dsregcmd /status`.
3. Убедитесь, что для **AzureAdJoined** и **DomainJoined** установлено значение **YES** (Да).
4. Вы можете использовать **DeviceId** и сравнить состояние службы с помощью портала Azure или PowerShell.

### <a name="using-the-azure-portal"></a>Использование портала Azure

1. Перейдите на страницу устройства по [прямой ссылке](https://portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/Devices).
2. См. сведения о том, как найти устройство, в статье [Управление удостоверениями устройств с помощью портала Azure](./device-management-azure-portal.md).
3. Если для столбца **Зарегистрировано** отображается состояние **Ожидается**, это значит, что гибридное присоединение к Azure AD не завершено.
4. Если для столбца **Зарегистрировано** отображается **дата и время**, это значит, что гибридное присоединение к Azure AD завершено.

### <a name="using-powershell"></a>Использование PowerShell

Проверьте состояние регистрации устройства в клиенте Azure с помощью **[Get-MsolDevice](/powershell/module/msonline/get-msoldevice)** . Этот командлет применяется в [модуле PowerShell для Azure Active Directory](/powershell/azure/active-directory/install-msonlinev1).

Когда вы используете командлет **Get-MSolDevice**, чтобы получить сведения о службе:

- Должен существовать объект с **идентификатором устройства**, который совпадает с идентификатором в клиенте Windows.
- У параметра **DeviceTrustType** должно быть значение **Domain Joined**. Этот параметр является эквивалентным состоянию **Гибридные устройства, присоединенные к Azure AD** на странице **Устройства** на портале Azure AD.
- Для устройств, которые используются при условном доступе, у параметра **Enabled** должно быть значение **True**, а у **DeviceTrustLevel** — **Managed**.

1. Откройте Windows PowerShell от имени администратора.
2. Введите `Connect-MsolService`, чтобы подключится к своему клиенту Azure.

#### <a name="count-all-hybrid-azure-ad-joined-devices-excluding-pending-state"></a>Число всех устройств с гибридным присоединением к Azure AD, кроме тех, для кого отображается состояние **Ожидается**

```azurepowershell
(Get-MsolDevice -All -IncludeSystemManagedDevices | where {($_.DeviceTrustType -eq 'Domain Joined') -and (([string]($_.AlternativeSecurityIds)).StartsWith("X509:"))}).count
```

#### <a name="count-all-hybrid-azure-ad-joined-devices-with-pending-state"></a>Число всех устройств с гибридным присоединением к Azure AD с отображаемым состоянием **Ожидается**

```azurepowershell
(Get-MsolDevice -All -IncludeSystemManagedDevices | where {($_.DeviceTrustType -eq 'Domain Joined') -and (-not([string]($_.AlternativeSecurityIds)).StartsWith("X509:"))}).count
```

#### <a name="list-all-hybrid-azure-ad-joined-devices"></a>Перечисление всех устройств с гибридным присоединением к Azure AD

```azurepowershell
Get-MsolDevice -All -IncludeSystemManagedDevices | where {($_.DeviceTrustType -eq 'Domain Joined') -and (([string]($_.AlternativeSecurityIds)).StartsWith("X509:"))}
```

#### <a name="list-all-hybrid-azure-ad-joined-devices-with-pending-state"></a>Перечисление всех гибридных устройств с гибридным присоединением к Azure AD с отображаемым состоянием **Ожидается**

```azurepowershell
Get-MsolDevice -All -IncludeSystemManagedDevices | where {($_.DeviceTrustType -eq 'Domain Joined') -and (-not([string]($_.AlternativeSecurityIds)).StartsWith("X509:"))}
```

#### <a name="list-details-of-a-single-device"></a>Вывод сведений об одном устройстве.

1. Введите `get-msoldevice -deviceId <deviceId>` (это идентификатор **DeviceId**, полученный локально на устройстве).
2. Убедитесь, что параметр **Включено** имеет значение **True**.

## <a name="troubleshoot-your-implementation"></a>Устранение неполадок реализации

Если возникают проблемы с настройкой гибридного присоединения к Azure AD для устройств Windows, присоединенных к домену, ознакомьтесь со следующими статьями:

- [Устранение неполадок с устройствами с помощью команды dsregcmd](./troubleshoot-device-dsregcmd.md)
- [Устранение неполадок на устройствах с гибридным присоединением к Azure Active Directory](troubleshoot-hybrid-join-windows-current.md)
- [Устранение неполадок на устройствах нижнего уровня с гибридным присоединением к Azure Active Directory](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Дальнейшие действия

Перейдите к следующей статье, чтобы узнать, как управлять удостоверениями устройств с помощью портала Azure.
> [!div class="nextstepaction"]
> [Управление удостоверениями устройств](device-management-azure-portal.md)
