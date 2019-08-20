---
title: Доменные службы Azure Active Directory. Присоединение к виртуальной машине CoreOS Linux | Документация Майкрософт
description: Присоединение виртуальной машины CoreOS Linux к доменным службам Azure Active Directory
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 5db65f30-bf69-4ea3-9ea5-add1db83fdb8
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: c1f3d1ec7bb9e9f449cea3f9aa36ca8f80348c6e
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/19/2019
ms.locfileid: "69612819"
---
# <a name="join-a-coreos-linux-virtual-machine-to-a-managed-domain"></a>Присоединение виртуальной машины CoreOS Linux к управляемому домену
Из этой статьи вы узнаете, как присоединить виртуальную машину CoreOS Linux в Azure к управляемому домену доменных служб Azure AD.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Перед началом работы
Чтобы выполнить задачи, описанные в этой статье, вам потребуется следующее:
1. Действующая **подписка Azure**.
2. **Каталог Azure AD** — синхронизированный с локальным каталогом или каталогом только для облака.
3. **Доменные службы Azure AD** должны быть включены для каталога Azure AD. Если это еще не сделано, выполните все задачи, описанные в [руководстве по началу работы](tutorial-create-instance.md).
4. Обязательно укажите IP-адреса управляемого домена в качестве DNS-серверов для виртуальной сети. Дополнительные сведения см. в статье об [изменении настроек DNS виртуальной сети Azure](tutorial-create-instance.md#update-dns-settings-for-the-azure-virtual-network).
5. Выполните шаги, необходимые для [синхронизации паролей с управляемым доменом доменных служб Azure AD](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds).


## <a name="provision-a-coreos-linux-virtual-machine"></a>Подготовка виртуальной машины CoreOS Linux
Подготовьте виртуальную машину CoreOS в Azure любым из следующих способов:
* [портал Azure](../virtual-machines/linux/quick-create-portal.md)
* [Интерфейс командной строки Azure](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

В этой статье используется образ виртуальной машины с **CoreOS Linux (стабильная версия)** в Azure.

> [!IMPORTANT]
> * Разверните виртуальную машину в **виртуальной сети, для которой включены доменные службы Azure AD**.
> * Выберите **другую подсеть** (не ту, для которой включены доменные службы Azure AD).
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Удаленное подключение к только что подготовленной виртуальной машине Linux
Вы подготовили виртуальную машину CoreOS в Azure. Следующая задача — установить удаленное подключение к виртуальной машине, используя учетную запись локального администратора, созданную при подготовке виртуальной машины.

Следуйте инструкциям в статье [как войти в виртуальную машину под управлением Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Настройка файла hosts на виртуальной машине Linux
В окне терминала SSH откройте файл /etc/hosts для редактирования, чтобы изменить в нем IP-адрес и имя узла вашего компьютера.

```console
sudo vi /etc/hosts
```

Добавьте следующее значение в файл hosts:

```console
127.0.0.1 contoso-coreos.contoso.com contoso-coreos
```

Здесь "contoso.com" — это доменное DNS-имя управляемого домена. contoso-coreos — это имя узла виртуальной машины CoreOS, присоединяемой к управляемому домену.


## <a name="configure-the-sssd-service-on-the-linux-virtual-machine"></a>Настройка службы SSSD на виртуальной машине Linux
Обновите файл конфигурации SSSD (/etc/sssd/sssd.conf) в соответствии со следующим примером:

```console
[sssd]
config_file_version = 2
services = nss, pam
domains = contoso.COM

[domain/contoso.COM]
id_provider = ad
auth_provider = ad
chpass_provider = ad

ldap_uri = ldap://contoso.com
ldap_search_base = dc=contoso,dc=com
ldap_schema = rfc2307bis
ldap_sasl_mech = GSSAPI
ldap_user_object_class = user
ldap_group_object_class = group
ldap_user_home_directory = unixHomeDirectory
ldap_user_principal = userPrincipalName
ldap_account_expire_policy = ad
ldap_force_upper_case_realm = true
fallback_homedir = /home/%d/%u

krb5_server = contoso.com
krb5_realm = contoso.COM
```

Замените contoso. COM "с доменным именем DNS управляемого домена. В файле конфигураций имя домена должно быть указано прописными буквами.


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Присоединение виртуальной машины Linux к управляемому домену
Теперь, когда все требуемые пакеты установлены на виртуальной машине Linux, мы готовы присоединить виртуальную машину к управляемому домену.

```console
sudo adcli join -D contoso.COM -U bob@contoso.COM -K /etc/krb5.keytab -H contoso-coreos.contoso.com -N coreos
```


> [!NOTE]
> **Устранение неполадок**. Если команда *adcli* не может найти управляемый домен, сделайте следующее:
>   * Проверьте подключение между доменом и виртуальной машиной (с помощью команды ping).
>   * Убедитесь, что виртуальная машина развернута в одной виртуальной сети с управляемым доменом.
>   * Проверьте, обновлены ли параметры DNS-сервера для виртуальной сети — должны быть указаны контроллеры управляемого домена.

Запустите службу SSSD. В окне терминала SSH введите следующую команду:
  
```console
sudo systemctl start sssd.service
```


## <a name="verify-domain-join"></a>Проверка присоединения к домену
Проверьте, присоединена ли виртуальная машина к управляемому домену. Подключитесь к виртуальной машине CoreOS, присоединенной к домену, используя другое SSH-подключение. Используйте учетную запись пользователя домена и проверьте, правильно ли разрешится эта учетная запись.

1. В окне терминала SSH введите следующую команду, чтобы подключиться по протоколу SSH к виртуальной машине CoreOS, присоединенной к домену. Используйте учетную запись домена, которая принадлежит к управляемому домену (в нашем примере — bob@contoso.COM).
    
    ```console
    ssh -l bob@contoso.COM contoso-coreos.contoso.com
    ```

2. Чтобы проверить, правильно ли инициализирован корневой каталог, в окне терминала SSH введите следующую команду:
    
    ```console
    pwd
    ```

3. Чтобы проверить, правильно ли определено членство в группе, в окне терминала SSH введите следующую команду:
   
    ```console
    id
    ```


## <a name="troubleshooting-domain-join"></a>Устранение неполадок при присоединении к домену
См. статью, посвященную [устранению неполадок при присоединении к домену](join-windows-vm.md#troubleshoot-domain-join-issues).

## <a name="related-content"></a>См. также
* [Приступая к работе с доменными службами Azure AD](tutorial-create-instance.md)
* [Присоединение виртуальной машины Windows Server к управляемому домену](active-directory-ds-admin-guide-join-windows-vm.md)
* [Как войти в виртуальную машину под управлением Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
