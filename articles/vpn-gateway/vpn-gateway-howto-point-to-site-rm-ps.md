---
title: 'Подключение к виртуальной сети из компьютера — P2S VPN и собственная аутентификация Azure Certificate: PowerShell'
description: Установка подключения между клиентом Windows или Mac OS X и виртуальной сетью Azure с помощью P2S и самозаверяющих сертификатов или сертификатов, выданных ЦС. В этой статье используется PowerShell.
titleSuffix: Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 01/15/2020
ms.author: cherylmc
ms.openlocfilehash: 3a17b56d3abed30ccb495fd9111ff1299165175c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "84988079"
---
# <a name="configure-a-point-to-site-vpn-connection-to-a-vnet-using-native-azure-certificate-authentication-powershell"></a>Настройка VPN-подключения типа "точка — сеть" к виртуальной сети с помощью собственной аутентификации Azure на основе сертификата: PowerShell

Эта статья поможет вам установить безопасное подключение отдельных клиентов под управлением Windows, Mac OS X или Linux к виртуальной сети Azure. VPN-подключения типа "точка — сеть" (P2S) эффективны для подключения к виртуальной сети из удаленного расположения, например, если вы дома или на конференции. Вы также можете использовать подключение "точка — сеть" вместо VPN-подключения "сеть — сеть" при наличии небольшого количества клиентов, которым требуется подключение к виртуальной сети. Для подключения типа "точка — сеть" не требуется VPN-устройство или общедоступный IP-адрес. Подключение "точка — сеть" — это VPN-подключение по протоколу SSTP (Secure Socket Tunneling Protocol) или IKEv2. См. дополнительные сведения о [VPN-подключениях "точка — сеть"](point-to-site-about.md).

![Схема соединения компьютера с виртуальной сетью Azure через подключение типа "точка — сеть"](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/p2snativeportal.png)

## <a name="architecture"></a>Architecture

Для собственной аутентификации Azure на основе сертификата при подключениях "точка — сеть" необходимы следующие компоненты, которые можно настроить в этом руководстве:

* VPN-шлюз с маршрутизацией на основе маршрутов.
* Открытый ключ (CER-файл) для корневого сертификата, импортированный в Azure. Сразу после передачи сертификат считается доверенным сертификатом и используется для проверки подлинности.
* Сертификат клиента, созданный на основе корневого сертификата. Сертификат клиента, установленный на каждом клиентском компьютере, который будет подключен к виртуальной сети. Этот сертификат используется для проверки подлинности клиента.
* Конфигурация VPN-клиента. Файлы конфигурации VPN-клиента содержат информацию, необходимую для подключения клиента к виртуальной сети. Эти файлы позволяют настроить существующий VPN-клиент, предоставляемый в операционной системе. Перед подключением каждый клиент нужно настроить, используя параметры в файлах конфигурации.

## <a name="before-you-begin"></a>Перед началом

Убедитесь в том, что у вас уже есть подписка Azure. Если у вас еще нет подписки Azure, вы можете активировать преимущества для [подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) или зарегистрироваться для использования [бесплатной учетной записи](https://azure.microsoft.com/pricing/free-trial).

### <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

>[!NOTE]
> Большинство действий, описанных в этой статье, могут использовать Azure Cloud Shell. Тем не менее, чтобы отправить открытый ключ корневого сертификата необходимо использовать PowerShell или портал Azure локально.
>

### <a name="example-values"></a><a name="example"></a>Примеры значений

Эти примеры значений можно использовать для создания тестовой среды или анализа примеров из этой стать. Переменные настраиваются в части [1](#declare) этой статьи. Вы можете использовать эти пошаговые инструкции, используя указанные в них значения, или же изменить значения в соответствии со своей средой.

* **Имя: VNet1**.
* **Адресное пространство: 192.168.0.0/16** и **10.254.0.0/16**.<br>Чтобы продемонстрировать, что эта конфигурация будет работать с несколькими адресными пространствами, в этом примере мы используем несколько адресных пространств. Однако это необязательно для этой конфигурации.
* **Имя подсети: интерфейсная часть**
  * **Диапазон адресов подсети: 192.168.1.0/24**.
* **Имя подсети: BackEnd.**
  * **Диапазон адресов подсети: 10.254.1.0/24.**
* **Имя подсети: GatewaySubnet.**<br>Имя подсети *GatewaySubnet* обязательно для работы VPN-шлюза.
  * **Диапазон адресов подсети шлюза: 192.168.200.0/24.** 
* **Пул адресов VPN-клиента: 172.16.201.0/24.**<br>VPN-клиенты, подключающиеся к виртуальной сети с помощью этого подключения типа "точка — сеть", получают IP-адреса из пула адресов VPN-клиента.
* **Подписка**. Если у вас есть несколько подписок, убедитесь, что используется правильная.
* **Группа ресурсов: TestRG**
* **Расположение: Восточная часть США**
* **DNS-сервер: IP-адрес** DNS-сервера, который нужно использовать для разрешения имен. (необязательно).
* **Имя шлюза: Vnet1GW**.
* **Имя общедоступного IP-адреса: VNet1GWPIP**.
* **Тип VPN: RouteBased.** 

## <a name="1-sign-in-and-set-variables"></a><a name="declare"></a>1. вход и задание переменных

В этом разделе мы выполним вход и объявим значения для этой конфигурации. Объявленные значения используются в примерах скриптов. Измените значения в соответствии со своей средой. Также можно использовать объявленные значения и выполнить эти шаги в качестве упражнения.

### <a name="sign-in"></a>Войти

[!INCLUDE [sign in](../../includes/vpn-gateway-cloud-shell-ps-login.md)]

### <a name="declare-variables"></a>Объявление переменных

Объявите переменные, которые вы хотите использовать. Используйте следующий пример, подставив собственные значения в соответствующих параметрах. Если необходимо закрыть сеанс PowerShell / Cloud Shell во время выполнения упражнения, просто скопируйте и вставьте значения в повторно объявленные переменные.

  ```azurepowershell-interactive
  $VNetName  = "VNet1"
  $FESubName = "FrontEnd"
  $BESubName = "Backend"
  $GWSubName = "GatewaySubnet"
  $VNetPrefix1 = "192.168.0.0/16"
  $VNetPrefix2 = "10.254.0.0/16"
  $FESubPrefix = "192.168.1.0/24"
  $BESubPrefix = "10.254.1.0/24"
  $GWSubPrefix = "192.168.200.0/26"
  $VPNClientAddressPool = "172.16.201.0/24"
  $RG = "TestRG"
  $Location = "East US"
  $GWName = "VNet1GW"
  $GWIPName = "VNet1GWPIP"
  $GWIPconfName = "gwipconf"
  ```

## <a name="2-configure-a-vnet"></a><a name="ConfigureVNet"></a>2. Настройка виртуальной сети

1. Создайте группу ресурсов.

   ```azurepowershell-interactive
   New-AzResourceGroup -Name $RG -Location $Location
   ```
2. Создайте конфигурации подсети для виртуальной сети, присвоив им имена *FrontEnd*, *BackEnd* и *GatewaySubnet*. Эти префиксы должны быть частью объявленного адресного пространства виртуальной сети.

   ```azurepowershell-interactive
   $fesub = New-AzVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
   $besub = New-AzVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
   $gwsub = New-AzVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
   ```
3. Создание виртуальной сети.

   В этом примере параметр сервера -DnsServer является необязательным. Если указать значение, DNS-сервер не создается. Необходимо указать IP-адрес DNS-сервера, который может разрешать имена для ресурсов, к которым вы подключаетесь из своей виртуальной сети. В этом примере мы использовали частный IP-адрес, но это не IP-адрес DNS-сервера. Подставьте собственные значения. Указанное значение используется для ресурсов, развертываемых в виртуальной сети, а не для подключений "точка — сеть" или VPN-клиентов.

   ```azurepowershell-interactive
   New-AzVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer 10.2.1.3
   ```
4. Укажите переменные для созданной виртуальной сети.

   ```azurepowershell-interactive
   $vnet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $RG
   $subnet = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
   ```
5. VPN-шлюз должен иметь общедоступный IP-адрес. Сначала запросите ресурс IP-адреса, а затем укажите его при создании шлюза виртуальной сети. IP-адрес динамически назначается ресурсу при создании VPN-шлюза. В настоящее время VPN-шлюз поддерживает выделение *динамических* общедоступных IP-адресов. Вы не можете запросить назначение статического общедоступного IP-адреса. Однако это не означает, что IP-адрес изменяется после назначения VPN-шлюзу. Общедоступный IP-адрес изменяется только после удаления и повторного создания шлюза. При изменении размера, сбросе или других внутренних операциях обслуживания или обновления IP-адрес VPN-шлюза не изменяется.

   Запросите динамически назначенный общедоступный IP-адрес.

   ```azurepowershell-interactive
   $pip = New-AzPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
   $ipconf = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
   ```

## <a name="3-create-the-vpn-gateway"></a><a name="creategateway"></a>3. Создание VPN-шлюза

Настройте и создайте шлюз для своей виртуальной сети.

* У параметра -GatewayType должно быть значение **Vpn**, а у параметра -VpnType — **RouteBased**.
* Параметр -VpnClientProtocol используется, чтобы указать типы туннелей, которые необходимо включить. Параметры туннеля: **опенвпн, SSTP** и **IKEv2**. Вы можете включить одну из них или любую поддерживаемую комбинацию. Если требуется включить несколько типов, укажите имена, разделенные запятыми. Опенвпн и SSTP нельзя включить одновременно. Для подключения клиент strongSwan в Android и Linux и собственный VPN-клиент IKEv2 в iOS и OSX используют только туннель IKEv2. Клиенты Windows сначала пытаются подключиться через IKEv2, и если им это не удается, возвращаются к SSTP. Клиент Опенвпн можно использовать для подключения к типу туннеля Опенвпн.
* SKU "базовый" шлюза виртуальной сети не поддерживает проверку подлинности IKEv2, Опенвпн или RADIUS. Если вы планируете подключение к своей виртуальной сети клиентов Mac, не используйте SKU "Базовый".
* Создание VPN-шлюза может занять до 45 минут в зависимости от выбранного [номера SKU шлюза](vpn-gateway-about-vpn-gateway-settings.md). В этом примере используется IKEv2.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
-Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
-VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 -VpnClientProtocol "IKEv2"
```

## <a name="4-add-the-vpn-client-address-pool"></a><a name="addresspool"></a>4. Добавление пула адресов VPN-клиента

По завершении создания сетевого VPN-шлюза можно добавить пул адресов клиента VPN. Пул адресов VPN-клиента представляет собой диапазон, из которого VPN-клиенты будут получать IP-адреса при подключении. Используйте диапазон частных IP-адресов, который не пересекается с локальным расположением, из которого выполнено подключение, или с виртуальной сетью, к которой вы хотите подключиться. В этом примере пул адресов VPN-клиента объявляется как [переменная](#declare) на шаге 1.

```azurepowershell-interactive
$Gateway = Get-AzVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
Set-AzVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
```

## <a name="5-generate-certificates"></a><a name="Certificates"></a>5. Создание сертификатов

Сертификаты используются в Azure для проверки подлинности VPN-клиентов в VPN-подключениях типа "точка — сеть". Необходимо отправить сведения об открытом ключе корневого сертификата в Azure. После этого открытый ключ считается доверенным. Сертификаты клиентов должны создаваться из доверенного корневого сертификата, а затем устанавливаться на каждом клиентском компьютере в хранилище сертификатов Certificates-Current User/Personal. Сертификат используется для проверки подлинности клиента, когда он инициирует подключение к виртуальной сети. 

Используемые самозаверяющие сертификаты должны быть созданы с помощью определенных параметров. Чтобы создать самозаверяющий сертификат, см. инструкции по [PowerShell и Windows 10](vpn-gateway-certificates-point-to-site.md). Если у вас нет Windows 10, вы можете использовать [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Во время создания самозаверяющих корневых сертификатов и сертификатов клиента обязательно следуйте инструкциям. В противном случае создаваемые сертификаты не будут совместимы с подключениями "точка — сеть", и вы получите сообщение об ошибке подключения.

### <a name="1-obtain-the-cer-file-for-the-root-certificate"></a><a name="cer"></a>1. получите CER файл для корневого сертификата.

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]


### <a name="2-generate-a-client-certificate"></a><a name="generate"></a>2. Создание сертификата клиента

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="6-upload-the-root-certificate-public-key-information"></a><a name="upload"></a>6. Отправка сведений о открытом ключе корневого сертификата

Убедитесь, что создание VPN-шлюза завершено. После создания шлюза вы можете передать CER-файл (который содержит сведения об открытом ключе) для доверенного корневого сертификата в Azure. После отправки CER-файла Azure сможет использовать его для проверки подлинности клиентов, на которых установлен клиентский сертификат, созданный из доверенного корневого сертификата. При необходимости позже можно отправить дополнительные файлы доверенных корневых сертификатов (не более 20).

>[!NOTE]
> Невозможно передать CER файл с помощью Azure Cloud Shell. Можно либо использовать PowerShell локально на компьютере, либо выполнить [портал Azure действия](vpn-gateway-howto-point-to-site-resource-manager-portal.md#uploadfile).
>

1. Объявите переменную для имени сертификата, заменив существующее значение собственным.

   ```azurepowershell
   $P2SRootCertName = "P2SRootCert.cer"
   ```
2. Добавьте собственный путь к файлу, а затем выполните командлеты.

   ```azurepowershell
   $filePathForCert = "C:\cert\P2SRootCert.cer"
   $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
   $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
   $p2srootcert = New-AzVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
   ```
3. Отправьте сведения об открытом ключе в Azure. После отправки сведений о сертификате Azure рассматривает его как доверенный корневой сертификат. При отправке убедитесь, что вы используете PowerShell локально на компьютере или вместо этого можете использовать [портал Azure действия](vpn-gateway-howto-point-to-site-resource-manager-portal.md#uploadfile). Невозможно отправить с помощью Azure Cloud Shell.

   ```azurepowershell
   Add-AzVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
   ```

## <a name="7-install-an-exported-client-certificate"></a><a name="clientcertificate"></a>7. Установка экспортированного сертификата клиента

Если вы хотите создать подключение типа "точка — сеть" на клиентском компьютере, отличном от того, который использовался для создания сертификатов клиентов, необходимо установить сертификат клиента. При установке сертификата клиента потребуется пароль, созданный при экспорте сертификата клиента.

Убедитесь, что сертификат клиента был экспортирован как PFX-файл вместе со всей цепочкой сертификатов (это значение по умолчанию). В противном случае данные корневого сертификата будут отсутствовать на клиентском компьютере и клиент не сможет пройти проверку должным образом. 

Дополнительные сведения см. в руководстве по [установке сертификата клиента](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="8-configure-the-native-vpn-client"></a><a name="clientconfig"></a>8. Настройка собственного VPN-клиента

Файлы конфигурации VPN-клиента содержат параметры для настройки устройств, которые подключаются к виртуальной сети с использованием подключения "точка — сеть". Дополнительные сведения см. в инструкциях по [созданию и установке файлов конфигурации VPN-клиента для собственной аутентификации Azure на основе сертификата при подключениях "точка — сеть"](point-to-site-vpn-client-configuration-azure-cert.md).

## <a name="9-connect-to-azure"></a><a name="connect"></a>9. подключение к Azure

### <a name="to-connect-from-a-windows-vpn-client"></a>Подключение из VPN-клиента для Windows

>[!NOTE]
>Вам потребуются права администратора для клиентского компьютера Windows, с которого устанавливается подключение.
>
>

1. Чтобы подключиться к виртуальной сети, откройте VPN-подключения на клиентском компьютере и найдите созданное VPN-подключение. Его имя совпадает с названием вашей виртуальной сети. Нажмите кнопку **Соединить**. Может появиться всплывающее сообщение об использовании сертификата. Щелкните **Продолжить** для повышения прав доступа. 
2. На странице состояния **подключения** щелкните **Подключить**. При отображении экрана **Выбор сертификата** убедитесь, что отображается именно тот сертификат клиента, который предполагается использовать для соединения. Если окно не появится, выберите нужный сертификат в раскрывающемся списке и нажмите кнопку **ОК**.

   ![Подключение VPN-клиента к Azure](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png)
3. Теперь подключение установлено.

   ![Подключение установлено](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png)

#### <a name="troubleshooting-windows-client-p2s-connections"></a>Устранение неполадок при подключении "точка — сеть" через клиент Windows

[!INCLUDE [client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

### <a name="to-connect-from-a-mac-vpn-client"></a>Подключение из VPN-клиента для Mac

В диалоговом окне Network (Сеть) найдите необходимый клиентский профиль и нажмите кнопку **Connect** (Подключиться).
Ознакомьтесь с подробными инструкциями в разделе [Установка Mac (OS X)](https://docs.microsoft.com/azure/vpn-gateway/point-to-site-vpn-client-configuration-azure-cert#installmac). Если у вас возникают неполадки подключения, убедитесь, что шлюз виртуальной сети не использует SKU "Базовый". SKU "Базовый" не поддерживается для клиентов Mac.

  ![Подключение для Mac](./media/vpn-gateway-howto-point-to-site-rm-ps/applyconnect.png)

## <a name="to-verify-your-connection"></a><a name="verify"></a>Проверка подключения

Эти инструкции применимы к клиентам Windows.

1. Чтобы проверить, активно ли VPN-подключение, откройте окно командной строки от имени администратора и выполните команду *ipconfig/all*.
2. Просмотрите результаты. Обратите внимание, что полученный вами IP-адрес — это один из адресов в пуле адресов VPN-клиента подключения "точка–cеть", указанном в конфигурации. Вы должны увидеть результат, аналогичный приведенному ниже.

   ```
   PPP adapter VNet1:
      Connection-specific DNS Suffix .:
      Description.....................: VNet1
      Physical Address................:
      DHCP Enabled....................: No
      Autoconfiguration Enabled.......: Yes
      IPv4 Address....................: 172.16.201.3(Preferred)
      Subnet Mask.....................: 255.255.255.255
      Default Gateway.................:
      NetBIOS over Tcpip..............: Enabled
   ```

## <a name="to-connect-to-a-virtual-machine"></a><a name="connectVM"></a>Подключение к виртуальной машине

Эти инструкции применимы к клиентам Windows.

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="to-add-or-remove-a-root-certificate"></a><a name="addremovecert"></a>Добавление и удаление корневого сертификата

Вы можете добавлять доверенные корневые сертификаты в Azure, а также удалять их из Azure. При удалении корневого сертификата клиенты, использующие цифровой сертификат, созданный из этого корневого сертификата, не смогут пройти проверку подлинности и поэтому не смогут подключиться. Чтобы клиенты могли проходить аутентификацию и подключаться, необходимо установить новый сертификат клиента, созданный на основе корневого сертификата, который является доверенным для Azure (то есть он передан в Azure).

### <a name="to-add-a-trusted-root-certificate"></a><a name="addtrustedroot"></a>Добавление доверенного корневого сертификата

В Azure можно добавить до 20 CER-файлов корневых сертификатов. Ниже описано, как добавить корневой сертификат:

#### <a name="method-1"></a><a name="certmethod1"></a>Метод 1


Это метод является наиболее эффективным способом отправки корневого сертификата. Для его использования необходимо иметь на своем компьютере установленный командлет Azure PowerShell (а не Azure Cloud Shell).

1. Подготовьте CER-файл для отправки:

   ```azurepowershell
   $filePathForCert = "C:\cert\P2SRootCert3.cer"
   $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
   $CertBase64_3 = [system.convert]::ToBase64String($cert.RawData)
   $p2srootcert = New-AzVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64_3
   ```
2. Отправьте файл. Вы можете отправить только один файл за раз.

   ```azurepowershell
   Add-AzVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64_3
   ```

3. Убедитесь, что файл сертификата загружен:

   ```azurepowershell
   Get-AzVpnClientRootCertificate -ResourceGroupName "TestRG" `
   -VirtualNetworkGatewayName "VNet1GW"
   ```

#### <a name="method-2---azure-portal"></a><a name="certmethod2"></a>Метод 2. Портал Azure

В этом методе больше шагов, чем в методе 1, но результат тот же. Он включен при необходимости просмотра данных сертификата. Для его использования необходимо иметь на своем компьютере установленный командлет Azure PowerShell (а не Azure Cloud Shell).

1. Создайте корневой сертификат и подготовьте его к добавлению в Azure. Экспортируйте открытый ключ как CER-файл X.509 в кодировке Base-64 и откройте его в текстовом редакторе. Скопируйте значения, как показано в следующем примере.

   ![сертификат](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png)

   > [!NOTE]
   > При копировании данных сертификата обязательно скопируйте текст как одну непрерывную строку без символов возврата каретки и перевода строки. Может потребоваться изменить параметры представления в текстовом редакторе, чтобы показать символы или показать все знаки и просмотреть символы возврата каретки и перевода строки.
   >
   >

2. Укажите имя сертификата и сведения о ключе как значения переменных. Подставьте собственные значения, как показано в следующем примере.

   ```azurepowershell
   $P2SRootCertName2 = "ARMP2SRootCert2.cer"
   $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
   ```
3. Добавьте новый корневой сертификат. Можно добавлять только один сертификат за раз.

   ```azurepowershell
   Add-AzVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2
   ```
4. Чтобы проверить, добавлен ли новый сертификат должным образом, воспользуйтесь приведенным ниже примером.

   ```azurepowershell
   Get-AzVpnClientRootCertificate -ResourceGroupName "TestRG" `
   -VirtualNetworkGatewayName "VNet1GW"
   ```

### <a name="to-remove-a-root-certificate"></a><a name="removerootcert"></a>Удалите корневой сертификат

1. Объявите переменные.

   ```azurepowershell-interactive
   $GWName = "Name_of_virtual_network_gateway"
   $RG = "Name_of_resource_group"
   $P2SRootCertName2 = "ARMP2SRootCert2.cer"
   $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
   ```
2. Удалите сертификат.

   ```azurepowershell-interactive
   Remove-AzVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
   ```
3. Используйте приведенный ниже пример, чтобы убедиться, что сертификат успешно удален.

   ```azurepowershell-interactive
   Get-AzVpnClientRootCertificate -ResourceGroupName "TestRG" `
   -VirtualNetworkGatewayName "VNet1GW"
   ```

## <a name="to-revoke-a-client-certificate"></a><a name="revoke"></a>Отзыв сертификата клиента

Можно отозвать сертификаты клиента. Список отзыва сертификатов позволяет выборочно запрещать подключение типа "точка-сеть" на основе отдельных сертификатов клиента. Эта процедура отличается от удаления доверенного корневого сертификата. При удалении доверенного корневого сертификата (CER-файл) из Azure будет запрещен доступ для всех сертификатов клиента, созданных на основе отозванного корневого сертификата или подписанных им. Отзыв сертификата клиента, а не корневого сертификата, позволяет по-прежнему использовать другие сертификаты, созданные на основе корневого сертификата, для проверки подлинности.

Обычно корневой сертификат используется для управления доступом на уровнях группы или организации, а отозванный сертификат клиента — для точного контроля доступа для отдельных пользователей.

### <a name="revoke-a-client-certificate"></a><a name="revokeclientcert"></a>Отзыв сертификата клиента

1. Получите отпечаток сертификата клиента. Дополнительные сведения см. в разделе [Получение отпечатка сертификата](https://msdn.microsoft.com/library/ms734695.aspx).
2. Скопируйте данные в текстовый редактор и удалите все пробелы, чтобы предоставить отпечаток в виде непрерывной строки. Далее эта строка будет объявлена в качестве переменной.
3. Объявите переменные. Обязательно объявите отпечаток, полученный на предыдущем шаге.

   ```azurepowershell-interactive
   $RevokedClientCert1 = "NameofCertificate"
   $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
   $GWName = "Name_of_virtual_network_gateway"
   $RG = "Name_of_resource_group"
   ```
4. Добавьте отпечаток в список отозванных сертификатов. После добавления отпечатка отобразится Succeeded.

   ```azurepowershell-interactive
   Add-AzVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
   -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG `
   -Thumbprint $RevokedThumbprint1
   ```
5. Убедитесь, что отпечаток добавлен в список отзыва сертификатов.

   ```azurepowershell-interactive
   Get-AzVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
   ```
6. Теперь сертификат нельзя использовать для подключения. Клиенты, пытающиеся подключиться с помощью этого сертификата, получат сообщение, что он недействителен.

### <a name="to-reinstate-a-client-certificate"></a><a name="reinstateclientcert"></a>Возобновление сертификата клиента

Можно возобновить использование сертификата клиента, удалив отпечаток из списка отозванных сертификатов клиента.

1. Объявите переменные. Обязательно объявите правильный отпечаток сертификата, который требуется возобновить.

   ```azurepowershell-interactive
   $RevokedClientCert1 = "NameofCertificate"
   $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
   $GWName = "Name_of_virtual_network_gateway"
   $RG = "Name_of_resource_group"
   ```
2. Удалите отпечаток сертификата из списка отзыва сертификатов.

   ```azurepowershell-interactive
   Remove-AzVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
   -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
   ```
3. Проверьте, удален ли отпечаток из списка отозванных отпечатков.

   ```azurepowershell-interactive
   Get-AzVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
   ```

## <a name="point-to-site-faq"></a><a name="faq"></a>Часто задаваемые вопросы о подключениях типа "точка — сеть"

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="next-steps"></a>Дальнейшие шаги
Установив подключение, можно добавить виртуальные машины в виртуальные сети. Дополнительные сведения о виртуальных машинах см. [здесь](https://docs.microsoft.com/azure/). Дополнительные сведения о сетях и виртуальных машинах см. в статье [Azure и Linux: обзор сетей виртуальных машин](../virtual-machines/linux/azure-vm-network-overview.md).

Дополнительные сведения об устранении неполадок подключения "точка — сеть" см. в руководстве [Устранение неполадок подключения типа "точка — сеть" Azure](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
