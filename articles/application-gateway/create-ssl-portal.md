---
title: Руководство. Настройка шлюза приложений с завершением SSL-запросов с помощью портала Azure
description: В этом руководстве вы узнаете, как с помощью портала Azure настроить шлюз приложений и добавить сертификат для завершения SSL-запросов.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 4/17/2019
ms.author: victorh
ms.openlocfilehash: f3ba3eb12dc85a72c4e49c374e62209b83400d33
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2019
ms.locfileid: "59677856"
---
# <a name="tutorial-configure-an-application-gateway-with-ssl-termination-using-the-azure-portal"></a>Руководство по Настройка шлюза приложений с завершением SSL-запросов с помощью портала Azure

С помощью портала Azure можно настроить [шлюз приложений](overview.md) с сертификатом для завершения SSL-запросов, в котором используются виртуальные машины для внутренних серверов.

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Создание самозаверяющего сертификата
> * создание шлюза приложений с сертификатом;
> * создание виртуальных машин, используемых в качестве внутренних серверов.
> * Тестирование шлюза приложений

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Вход в Azure

Войдите на портал Azure по адресу [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-self-signed-certificate"></a>Создание самозаверяющего сертификата

В этом разделе вы создадите самозаверяющий сертификат с помощью командлета [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate). Передайте этот сертификат на портал Azure при создании прослушивателя для шлюза приложений.

На локальном компьютере откройте окно Windows PowerShell от имени администратора. Выполните следующую команду для создания сертификата.

```powershell
New-SelfSignedCertificate \
  -certstorelocation cert:\localmachine\my \
  -dnsname www.contoso.com
```

Вы увидите примерно такой ответ:

```
PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\my

Thumbprint                                Subject
----------                                -------
E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630  CN=www.contoso.com

Use [Export-PfxCertificate](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate) with the Thumbprint that was returned to export a pfx file from the certificate:
```

```powershell
$pwd = ConvertTo-SecureString -String "Azure123456!" -Force -AsPlainText
Export-PfxCertificate \
  -cert cert:\localMachine\my\E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630 \
  -FilePath c:\appgwcert.pfx \
  -Password $pwd
```

## <a name="create-an-application-gateway"></a>Создание шлюза приложений

Виртуальная сеть необходима для обмена данными между создаваемыми ресурсами. В этом примере создаются две подсети: одна для шлюза приложений, а другая — для внутренних серверов. Вы можете создать виртуальную сеть во время создания шлюза приложений.

1. Нажмите кнопку **Создать** в верхнем левом углу портала Azure.
2. Щелкните **Сети**, а затем в списке "Рекомендованные" выберите **Шлюз приложений**.
3. Для имени шлюза приложений введите *myAppGateway*, а для новой группы ресурсов — *myResourceGroupAG*.
4. Оставьте значения по умолчанию для других параметров, а затем нажмите кнопку **ОК**.
5. Щелкните **Выбрать виртуальную сеть**, выберите **Создать**, а затем введите следующие значения для виртуальной сети:

   - *myVNet* — имя виртуальной сети;
   - *10.0.0.0/16* — диапазон адресов виртуальной сети;
   - *myAGSubnet* — имя подсети;
   - *10.0.0.0/24* — диапазон адресов подсети.

     ![Создание виртуальной сети](./media/create-ssl-portal/application-gateway-vnet.png)

6. Нажмите кнопку **ОК**, чтобы создать виртуальную сеть и подсеть.
7. Щелкните **Выберите общедоступный IP-адрес**, выберите **Создать**, а затем введите имя общедоступного IP-адреса. В этом примере общедоступный IP-адрес — *myAGPublicIPAddress*. Оставьте значения по умолчанию для других параметров, а затем нажмите кнопку **ОК**.
8. Выберите **HTTPS** в качестве протокола прослушивателя и убедитесь в том, что указан порт **443**.
9. Щелкните значок папки и перейдите к созданному ранее сертификату *appgwcert.pfx*, чтобы передать его.
10. Введите *mycert1* для имени сертификата и *Azure123456!* — для пароля, а затем нажмите кнопку **OK**.

    ![Создание шлюза приложений](./media/create-ssl-portal/application-gateway-create.png)

11. Просмотрите параметры на странице сводки и выберите **ОК**, чтобы создать сетевые ресурсы и шлюз приложений. Процесс создания шлюза приложений может занять несколько минут. Дождитесь успешного завершения развертывания, прежде чем переходить к следующему разделу.

### <a name="add-a-subnet"></a>Добавление подсети

1. Выберите **Все ресурсы** в меню слева, а затем в списке ресурсов щелкните **myVNet**.
2. Выберите **Подсети**, а затем — **Подсеть**.

    ![Создание подсети](./media/create-ssl-portal/application-gateway-subnet.png)

3. Введите *myBackendSubnet* в качестве имени подсети и нажмите кнопку **ОК**.

## <a name="create-backend-servers"></a>Создание внутренних серверов

В этом примере вы создадите две виртуальные машины, которые будут использоваться как внутренние серверы для шлюза приложений. Вы также установите службы IIS на виртуальных машинах, чтобы убедиться, что шлюз приложений успешно создан.

### <a name="create-a-virtual-machine"></a>Создание виртуальной машины

1. Нажмите кнопку **Создать**.
2. Выберите **Вычисления**, а затем в списке "Рекомендованные" выберите **Windows Server 2016 Datacenter**.
3. Введите следующие значения для виртуальной машины:

    - *myVM* — имя виртуальной машины;
    - *azureuser* — имя пользователя учетной записи администратора;
    - *Azure123456!* — пароль.
    - Щелкните **Use existing** (Использовать существующую), а затем выберите *myResourceGroupAG*.

4. Нажмите кнопку **ОК**.
5. Выберите значение **DS1_V2** в качестве размера виртуальной машины и нажмите кнопку **Выбрать**.
6. Убедитесь, что выбрана виртуальная сеть **myVNet** и подсеть **myBackendSubnet**. 
7. Выберите **Отключено**, чтобы отключить диагностику загрузки.
8. Нажмите кнопку **ОК**, просмотрите параметры на странице сводки и щелкните **Создать**.

### <a name="install-iis"></a>Установка служб IIS

1. Откройте интерактивную оболочку и убедитесь, что для нее задано значение **PowerShell**.

    ![Установка пользовательского расширения](./media/create-ssl-portal/application-gateway-extension.png)

2. Чтобы установить службы IIS, выполните на виртуальной машине следующие команды: 

    ```azurepowershell-interactive
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -ExtensionName IIS `
      -VMName myVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
      -Location EastUS
    ```

3. Создайте вторую виртуальную машину и установите службы IIS, следуя только что выполненным инструкциям. Введите *myVM2* в качестве имени виртуальной машины и значения параметра VMName в команде Set-AzVMExtension.

### <a name="add-backend-servers"></a>Добавление внутренних серверов

1. Выберите **Все ресурсы**, а затем — **myAppGateway**.
1. Выберите **Серверные пулы**. Пул по умолчанию был создан автоматически с помощью шлюза приложений. Выберите **appGatewayBackendPool**.
1. Нажмите кнопку **Добавить целевой объект**, чтобы добавить в серверный пул каждую созданную виртуальную машину.

    ![Добавление внутренних серверов](./media/create-ssl-portal/application-gateway-backend.png)

1. Щелкните **Сохранить**.

## <a name="test-the-application-gateway"></a>Тестирование шлюза приложений

1. Выберите **Все ресурсы**, а затем щелкните **myAGPublicIPAddress**.

    ![Запись общедоступного IP-адреса шлюза приложений](./media/create-ssl-portal/application-gateway-ag-address.png)

2. Скопируйте общедоступный IP-адрес и вставьте его в адресную строку браузера. Чтобы принять предупреждение системы безопасности, если используется самозаверяющий сертификат безопасности, выберите "Сведения", а затем — "Перейти на веб-страницу":

    ![Предупреждение системы безопасности](./media/create-ssl-portal/application-gateway-secure.png)

    На экране отобразится защищенный веб-сайт IIS, как в показано следующем примере:

    ![Тестирование базового URL-адреса в шлюзе приложений](./media/create-ssl-portal/application-gateway-iistest.png)

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Узнайте больше о возможностях Шлюза приложений Azure](application-gateway-introduction.md).
