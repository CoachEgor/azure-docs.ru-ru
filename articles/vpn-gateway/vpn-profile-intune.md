---
title: Создание профиля Intune для VPN-клиентов Azure
titleSuffix: Azure VPN Gateway
description: Узнайте, как создать настраиваемый профиль Intune для развертывания профилей клиента VPN Azure.
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/15/2020
ms.author: cherylmc
ms.openlocfilehash: 63b1d7f95711017feba52cad97f05128d83734da
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "94655176"
---
# <a name="create-an-intune-profile-to-deploy-vpn-client-profiles"></a>Создание профиля Intune для развертывания профилей VPN-клиента

Профили для VPN-клиентов Azure (Windows 10) можно развернуть с помощью Microsoft Intune. Эта статья поможет вам создать профиль Intune с помощью пользовательских параметров.

> [!NOTE]
> Этот метод будет работать только для развертывания профилей, использующих Azure Active Directory или общий сертификат для проверки подлинности клиента. Если используются уникальные сертификаты клиента, каждый пользователь должен вручную выбрать правильный сертификат в VPN-клиенте Azure.
>

## <a name="prerequisites"></a>Предварительные требования

* Устройства уже зарегистрированы в Intune MDM.
* VPN-клиент Azure для Windows 10 уже развернут на клиентском компьютере.
* Поддерживается только версия Windows 19H2 или более поздняя.

## <a name="modify-xml"></a><a name="xml"></a>Изменить XML

В следующих шагах мы используем пример XML для настраиваемого профиля OMA-URI для Intune со следующими параметрами:

* Автоматическое подключение
* Обнаружение доверенной сети включено.

Другие поддерживаемые параметры см. в статье [Поддержка VPNV2 CSP](/windows/client-management/mdm/vpnv2-csp) .

1. Скачайте профиль VPN из портал Azure и извлеките файл *azurevpnconfig.xml* из пакета.
1. Скопируйте приведенный ниже текст и вставьте его в новый файл текстового редактора.

   ```xml-interactive
    <VPNProfile>
      <!--<EdpModeId>corp.contoso.com</EdpModeId>-->
      <RememberCredentials>true</RememberCredentials>
      <AlwaysOn>true</AlwaysOn>
      <TrustedNetworkDetection>contoso.com,test.corp.contoso.com</TrustedNetworkDetection>
      <DeviceTunnel>false</DeviceTunnel>
      <RegisterDNS>false</RegisterDNS>
      <PluginProfile>
        <ServerUrlList>azuregateway-7cee0077-d553-4323-87df-069c331f58cb-053dd0f6af02.vpn.azure.com</ServerUrlList> 
        <CustomConfiguration>

        </CustomConfiguration>
        <PluginPackageFamilyName>Microsoft.AzureVpn_8wekyb3d8bbwe</PluginPackageFamilyName>
      </PluginProfile>
    </VPNProfile>
   ```
1. Измените запись между ```<ServerUrlList>``` и ```</ServerUrlList>``` с записью из скачанного профиля (azurevpnconfig.xml). Измените полное доменное имя "Трустеднетворкдетектион" в соответствии с вашей средой.
1. Откройте скачанный профиль Azure (azurevpnconfig.xml) и скопируйте все содержимое в буфер обмена, выделив текст и нажав клавиши (Ctrl) + C. 
1. Вставьте скопированный текст из предыдущего шага в файл, созданный на шаге 2 между ```<CustomConfiguration>  </CustomConfiguration>``` тегами. Сохраните файл с расширением XML.
1. Запишите значение в ```<name>  </name>``` тегах. Это имя профиля. Это имя потребуется при создании профиля в Intune. Закройте файл и запомните расположение, в котором он сохранен.

## <a name="create-intune-profile"></a>Создание профиля Intune

В этом разделе вы создадите профиль Microsoft Intune с пользовательскими параметрами.

1. Войдите в Intune и перейдите к элементу **устройства-> профили конфигурации**. Выберите **+ создать профиль**.

   :::image type="content" source="./media/create-profile-intune/configuration-profile.png" alt-text="профили конфигурации.":::
1. В поле **Платформа** выберите **Windows 10 и более поздние версии**. В качестве **профиля** выберите **Custom (настраиваемый**). Затем выберите **Создать**.
1. Присвойте профилю имя и описание, а затем нажмите кнопку **Далее**.
1. На вкладке **Параметры конфигурации** нажмите кнопку **Добавить**.

    * **Имя:** Введите имя конфигурации.
    * **Описание:** Необязательное описание.
    * **OMA-URI:** ```./User/Vendor/MSFT/VPNv2/<name of your connection>/ProfileXML``` (эти сведения можно найти в файле azurevpnconfig.xml в <name></name> теге).
    * **Тип данных:** Строка (XML-файл).

   Щелкните значок папки и выберите файл, сохраненный на шаге 6, в разделе [XML-](#xml) действия. Нажмите **Добавить**.

   :::image type="content" source="./media/create-profile-intune/configuration-settings.png" alt-text="Параметры конфигурации" lightbox="./media/create-profile-intune/configuration-settings.png":::
1. Выберите **Далее**.
1. В разделе **назначения** выберите группу, в которую необходимо принудительно отправить конфигурацию. Затем нажмите кнопку **Далее**.
1. Правила применимости являются необязательными. При необходимости определите любые правила, а затем нажмите кнопку **Далее**.
1. На странице " **Проверка и создание** " выберите **создать**.

    :::image type="content" source="./media/create-profile-intune/create-profile.png" alt-text="Создание профиля":::
1. Теперь ваш настраиваемый профиль создан. Дополнительные Microsoft Intune действиях по развертыванию этого профиля см. в разделе [Назначение профилей пользователей и устройств](/mem/intune/configuration/device-profile-assign).
 
## <a name="next-steps"></a>Дальнейшие действия

Узнайте больше о [подключениях "точка — сеть"](point-to-site-about.md).