---
title: Предварительные требования для шифровании дисков Azure для виртуальных машин IaaS | Документация Майкрософт
description: Эта статья содержит предварительные требования для использования шифрования дисков Microsoft Azure с виртуальными машинами IaaS.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/25/2019
ms.custom: seodec18
ms.openlocfilehash: 16a556264cda3ed4eb93e8fb738765ddcb379f69
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67068575"
---
# <a name="azure-disk-encryption-prerequisites"></a>Предварительные требования для шифрования дисков Azure

В этой статье объясняются компоненты, которые должны быть установлены до того, как вы сможете использовать шифрование дисков Azure. Шифрование дисков Azure интегрируется с [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/), обеспечивая управление ключами шифрования. Для настройки шифрования дисков Azure можно использовать [Azure PowerShell](/powershell/azure/overview), [Azure CLI](/cli/azure/) или [портал Azure](https://portal.azure.com).

Ниже перечислены предварительные требования для включения шифрования дисков Azure на виртуальных машинах IaaS Azure в сценариях, которые обсуждались в статье [Azure Disk Encryption for IaaS VMs](azure-security-disk-encryption-overview.md) (Шифрование дисков Azure для виртуальных машин IaaS). Эти требования обязательно должны быть выполнены. 

> [!WARNING]
> - Если вы уже использовали [шифрование дисков Azure с помощью приложения Azure AD](azure-security-disk-encryption-prerequisites-aad.md) для шифрования этой виртуальной машины, этот способ нужно применять и далее для шифрования виртуальной машины. На этой зашифрованной виртуальной машине нельзя использовать [шифрование дисков Azure](azure-security-disk-encryption-prerequisites.md), так как этот сценарий не работает — переключение из приложения AAD для данной зашифрованной виртуальной машины сейчас не поддерживается.
> - Выполнение некоторых приведенных рекомендаций может привести к более интенсивному использованию данных, сети или вычислительных ресурсов, а следовательно к дополнительным затратам на лицензии или подписки. Необходима действующая подписка Azure, которая позволяет создавать ресурсы Azure в поддерживаемых регионах.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="bkmk_OSs"></a> Поддерживаемые операционные системы
Шифрование дисков Azure поддерживается в приведенных ниже операционных системах.

- Версии Windows Server: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2012 R2 Server Core и Windows Server 2016 Server core.
Для Windows Server 2008 R2 перед включением шифрования в Azure требуется установить .NET Framework 4.5. Установите его из центра обновления Windows с помощью необязательное обновление Microsoft .NET Framework 4.5.2 для Windows Server 2008 R2 x64-разрядных систем (KB2901983).
- Ядро Windows Server 2012 R2 и Windows Server 2016 Core поддерживаются шифрование дисков Azure, когда компонент bdehdcfg установлено на виртуальной Машине.
- Версии клиентов Windows: клиент Windows 8 и клиент Windows 10.
- Шифрование дисков Azure поддерживается только для определенных дистрибутивов и версий серверов Linux из коллекции Azure. Список поддерживаемых сейчас версий см. в статье [Шифрование дисков Azure: часто задаваемые вопросы](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport). Ссылаться на [дистрибутивов Linux, рекомендованные для Azure](../virtual-machines/linux/endorsed-distros.md) список образов, поддерживаемые корпорацией Майкрософт и [какие дистрибутивы Linux поддерживает шифрование дисков Azure?](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) в [Azure Часто задаваемые вопросы о шифрования на диске](azure-security-disk-encryption-faq.md) список поддерживаемых в настоящее время версии, основанные на дистрибутивы подтвержденных образа.
- Для шифрования дисков Azure требуется, чтобы хранилище ключей и виртуальные машины находились в одном регионе и подписке Azure. Настройка ресурсов в отдельных регионах приведет к сбою при включении функции шифрования дисков Azure.

## <a name="bkmk_LinuxPrereq"></a> Дополнительные требования для виртуальных машин Linux IaaS 

- Службе шифрования дисков Azure на виртуальной машине Linux требуется 7 ГБ ОЗУ, чтобы выполнять шифрование диска ОС на [поддерживаемых образах](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport). Как только процесс шифрования диска ОС будет завершен, виртуальную машину можно настроить для работы с меньшим объемом памяти.
- Шифрование дисков Azure требуется dm-crypt и vfat модулей в представление в системе. Удаление или отключение vfat из образа по умолчанию помешает системы чтения ключа тома и получение ключа, необходимого для разблокировки дисков при последующих перезагрузках. Действия усиления безопасности системы, удалите модуль vfat из системы не совместимы с шифрованием дисков Azure. 
- Перед включением шифрования диски данных, которые нужно зашифровать, должны быть правильно указаны в /etc/fstab. Используйте постоянное имя устройства для этой записи, так как имена устройств в формате "/dev/sdX" необязательно будут с одним и тем же диском в случае перезагрузок, особенно после применения шифрования. Дополнительные сведения об этом поведении см. в статье [Устранение неполадок при изменении имени устройства виртуальной машины Linux](../virtual-machines/linux/troubleshoot-device-names-problems.md)
- Убедитесь, что параметры /etc/fstab для подключения настроены правильно. Чтобы настроить эти параметры, запустите команду mount -a или перезагрузите виртуальную машину и подключите ее заново таким образом. Как только это будет завершено, проверьте выходные данные команды lsblk, чтобы убедиться, что нужный диск все еще подключен. 
  - Если файл /etc/fstab не подключает диск должным образом до включения шифрования, служба шифрования дисков Azure не сможет правильно подключить его.
  - Служба шифрования дисков Azure переместит информацию о подключении из /etc/fstab в собственный файл конфигурации как часть процесса шифрования. Не волнуйтесь, если увидите, что запись отсутствует в /etc/fstab после завершения шифрования диска данных.
  -  После перезагрузки процессу шифрования дисков Azure потребуется некоторое время для подключения только что зашифрованных дисков. После перезагрузки они не будут доступны сразу. Прежде чем зашифрованные диски станут доступными для других процессов, потребуется некоторое время для их запуска, разблокировки и подключения. Этот процесс может занять более минуты после перезагрузки в зависимости от характеристик системы.

Примеры команд, которые можно использовать для подключения дисков данных и создания необходимых записей /etc/fstab, можно найти в [строках 244–248 этого файла сценария](https://github.com/ejarvi/ade-cli-getting-started/blob/master/validate.sh#L244-L248). 


## <a name="bkmk_GPO"></a> Сетевые подключения и групповая политика

**Чтобы использовать функцию шифрования дисков Azure, конфигурация конечной точки сети виртуальной машины IaaS должна соответствовать приведенным ниже требованиям:**
  - Виртуальная машина IaaS должна иметь возможность подключиться к конечной точке Azure Active Directory \[login.microsoftonline.com\], чтобы получить маркер для подключения к хранилищу ключей.
  - Для записи ключей шифрования в ваше хранилище ключей виртуальная машина IaaS должна иметь возможность подключиться к конечной точке хранилища ключей.
  - Виртуальная машина IaaS должна иметь возможность подключиться к конечной точке службы хранилища Azure, в которой размещен репозиторий расширений Azure, и к учетной записи хранения Azure, в которой размещены VHD-файлы.
  -  Если ваша политика безопасности ограничивает доступ к Интернету с виртуальных машин Azure, можно разрешить указанный выше универсальный код ресурса (URI) и настроить определенное правило, чтобы разрешить исходящие подключения к данным IP-адресам. Дополнительные сведения см. в статье [Доступ к Azure Key Vault из-за брандмауэра](../key-vault/key-vault-access-behind-firewall.md).    


**Групповая политика:**
 - Решение шифрования дисков Azure использует внешний предохранитель ключа BitLocker для виртуальных машин IaaS под управлением Windows. Если виртуальные машины присоединены к домену, не применяйте групповые политики, требующие использования предохранителей TPM. Сведения о групповой политике "Разрешить использование BitLocker без совместимого TPM" см. в [справке по групповым политикам BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#a-href-idbkmk-unlockpol1arequire-additional-authentication-at-startup).

-  Политика BitLocker на присоединенных к домену виртуальных машинах с настраиваемой групповой политикой должна включать следующий параметр: [Настройка хранилища пользователя BitLocker, сведения о восстановлении, "->" Разрешить 256-разрядный ключ восстановления](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings). Шифрование дисков Azure завершится ошибкой, если параметры настраиваемой групповой политики для BitLocker несовместимы. На компьютерах без соответствующего параметра политики может потребоваться применить новую политику, принудительно обновить ее (gpupdate.exe /force) и перезагрузить компьютер.

- Шифрование дисков Azure завершится ошибкой, если алгоритм AES-CBC, который используется с помощью BitLocker блокирует уровня групповой политики домена.


## <a name="bkmk_PSH"></a> Azure PowerShell
В [Azure PowerShell](/powershell/azure/overview) доступен набор командлетов, которые используют модель [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) для управления ресурсами Azure. Его можно использовать в браузере с [Azure Cloud Shell](../cloud-shell/overview.md), а также установить на локальном компьютере, следуя инструкциям ниже, и применять в любом сеансе PowerShell. Если вы уже установили его локально, убедитесь, что вы используете последнюю версию версии пакета SDK для Azure PowerShell для настройки шифрования дисков Azure. Скачайте последнюю версию [Azure PowerShell](https://github.com/Azure/azure-powershell/releases).

### <a name="install-azure-powershell-for-use-on-your-local-machine-optional"></a>Установите Azure PowerShell для использования на вашем локальном компьютере (необязательно): 
1. Следуйте инструкциям по ссылкам для вашей операционной системы, а затем перейдите к остальным шагам.      
   - [Установите и настройте Azure PowerShell](/powershell/azure/install-az-ps). 
     - Установите Azure PowerShell, PowerShellGet и загрузить модуль Az. 

2. Проверьте установленные версии модуля Az. При необходимости [обновите модуль Azure PowerShell](/powershell/azure/install-az-ps#update-the-azure-powershell-module).
    Рекомендуется использовать последнюю версию модуля Az.

     ```powershell
     Get-Module Az -ListAvailable | Select-Object -Property Name,Version,Path
     ```

3. Войдите Azure с помощью [Connect AzAccount](/powershell/module/az.accounts/connect-azaccount) командлета.
     
     ```azurepowershell-interactive
     Connect-AzAccount
     # For specific instances of Azure, use the -Environment parameter.
     Connect-AzAccount –Environment (Get-AzEnvironment –Name AzureUSGovernment)
    
     <# If you have multiple subscriptions and want to specify a specific one, 
     get your subscription list with Get-AzSubscription and 
     specify it with Set-AzContext.  #>
     Get-AzSubscription
     Set-AzContext -SubscriptionId "xxxx-xxxx-xxxx-xxxx"
     ```

4.  При необходимости ознакомьтесь со статьей [Начало работы с Azure PowerShell](/powershell/azure/get-started-azureps).

## <a name="bkmk_CLI"></a>Установка Azure CLI для использования на вашем локальном компьютере (необязательно)

[Azure CLI 2.0](/cli/azure) — это интерфейс командной строки для управления ресурсами Azure. Этот интерфейс обеспечивает гибкие функции подачи запросов, выполнение длительных операций без блокировки и простое создание скриптов. Его можно использовать в браузере с [Azure Cloud Shell](../cloud-shell/overview.md), а также установить на локальном компьютере и использовать в любом сеансе PowerShell.

1. [Установите Azure CLI](/cli/azure/install-azure-cli) для использования на вашем локальном компьютере (необязательно):

2. Проверьте номер установленной версии.
     
     ```azurecli-interactive
     az --version
     ``` 

3. Затем войдите в Azure с помощью команды [az login](/cli/azure/authenticate-azure-cli).
     
     ```azurecli
     az login
     
     # If you would like to select a tenant, use: 
     az login --tenant "<tenant>"

     # If you have multiple subscriptions, get your subscription list with az account list and specify with az account set.
     az account list
     az account set --subscription "<subscription name or ID>"
     ```

4. При необходимости ознакомьтесь со статьей [Начало работы с Azure CLI 2.0](/cli/azure/get-started-with-azure-cli). 


## <a name="prerequisite-workflow-for-key-vault"></a>Необходимый рабочий процесс для Key Vault
Если вы уже знакомы с предварительными требованиями Key Vault и Azure AD для шифрования дисков Azure, можно использовать [скрипт PowerShell предварительных требований к шифрованию дисков Azure](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 ). Дополнительные сведения об использовании скрипта предварительных требований см. в [кратком руководстве по шифрованию виртуальных машин](quick-encrypt-vm-powershell.md) и [приложении шифрования дисков Azure](azure-security-disk-encryption-appendix.md#bkmk_prereq-script). 

1. При необходимости создайте группу ресурсов.
2. Создать хранилище ключей. 
3. Установите политики расширенного доступа к хранилищу ключей.

>[!WARNING]
>Перед удалением хранилища ключа убедитесь, что никакие из существующих виртуальных машин не зашифрованы этим ключом. Для защиты хранилища от случайного удаления включите для него параметры [Включить обратимое удаление](../key-vault/key-vault-soft-delete-powershell.md#enabling-soft-delete) и [Блокировка ресурса](../azure-resource-manager/resource-group-lock-resources.md). 
 
## <a name="bkmk_KeyVault"></a> Создание хранилища ключей 
Шифрование дисков Azure интегрировано с [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/), что позволяет управлять секретами и ключами шифрования дисков в подписке Key Vault и контролировать их. Вы можете создать хранилище ключей или использовать существующее для шифрования дисков Azure. Дополнительные сведения о хранилищах ключей см. в статье [Приступая к работе с Azure Key Vault](../key-vault/key-vault-get-started.md) и [Защита хранилища ключей](../key-vault/key-vault-secure-your-key-vault.md). Для создания хранилища ключей можно использовать шаблон Resource Manager, Azure PowerShell или Azure CLI. 


>[!WARNING]
>Чтобы убедиться, что секреты шифрования не пересекают региональные границы, шифрованию дисков Azure требуется, чтобы хранилище ключей и виртуальные машины были расположены в одном регионе. Создайте и используйте хранилище ключей, которое находится в том же регионе, что и виртуальная машина для шифрования. 


### <a name="bkmk_KVPSH"></a> Создание хранилища ключей с помощью PowerShell

Можно создать хранилище ключей с помощью Azure PowerShell [New AzKeyVault](/powershell/module/az.keyvault/New-azKeyVault) командлета. Дополнительные командлеты для хранилища ключей см. в разделе [Az.KeyVault](/powershell/module/az.keyvault/). 

1. При необходимости [подключитесь к подписке Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH). 
2. Создать новую группу ресурсов, при необходимости с [New AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup).  Для вывода списка расположений центров обработки данных используйте [Get-AzLocation](/powershell/module/az.resources/get-azlocation). 
     
     ```azurepowershell-interactive
     # Get-AzLocation 
     New-AzResourceGroup –Name 'MyKeyVaultResourceGroup' –Location 'East US'
     ```

3. Создать новое хранилище ключей с помощью [New AzKeyVault](/powershell/module/az.keyvault/New-azKeyVault)
    
      ```azurepowershell-interactive
     New-AzKeyVault -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -Location 'East US'
     ```

4. Запишите возвращенные значения **имени хранилища**, **имени группы ресурсов**, **идентификатора ресурса**, **кода URI хранилища** и **идентификатора объекта** для последующего использования при шифровании дисков. 


### <a name="bkmk_KVCLI"></a> Создание хранилища ключей с помощью Azure CLI
Вы можете управлять вашим хранилищем ключей в Azure CLI с помощью команд [az keyvault](/cli/azure/keyvault#commands). Чтобы создать хранилище ключей, используйте команду [az keyvault create](/cli/azure/keyvault#az-keyvault-create).

1. При необходимости [подключитесь к подписке Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Чтобы создать группу ресурсов, используйте команду [az group create](/cli/azure/group#az-group-create). Для перечисления расположений используйте команду [az account list-locations](/cli/azure/account#az-account-list). 
     
     ```azurecli-interactive
     # To list locations: az account list-locations --output table
     az group create -n "MyKeyVaultResourceGroup" -l "East US"
     ```

3. Чтобы создать новое хранилище ключей, используйте команду[az keyvault create](/cli/azure/keyvault#az-keyvault-create).
    
     ```azurecli-interactive
     az keyvault create --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --location "East US"
     ```

4. Запишите возвращенные значения **имени хранилища**, **имени группы ресурсов**, **идентификатора ресурса**, **кода URI хранилища** и **идентификатора объекта** для последующего использования. 

### <a name="bkmk_KVRM"></a> Создание хранилища ключей с помощью шаблона Resource Manager

Можно создать хранилище ключей с помощью [шаблона Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create).

1. В шаблоне быстрого запуска Azure щелкните **Deploy to Azure** (Развернуть в Azure).
2. Выберите подписку, группу ресурсов, расположение группы ресурсов, имя хранилища ключей, идентификатор объекта, юридические условия и соглашение, а затем щелкните **Приобрести**. 


## <a name="bkmk_KVper"></a> Установка политики расширенного доступа к хранилищу ключей
Платформа Azure должна иметь доступ к ключам шифрования или секретам, расположенным в вашем хранилище ключей, чтобы предоставлять их виртуальной машине при ее загрузке для расшифровки томов. Включите шифрование диска в хранилище ключей, иначе развертывание завершится сбоем.  

### <a name="bkmk_KVperPSH"></a> Установка политик расширенного доступа для хранилища ключей с помощью Azure PowerShell
 Используйте командлет PowerShell хранилища ключей [AzKeyVaultAccessPolicy набора](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) включить шифрование дисков для хранилища ключей.

  - **Включение хранилища ключей для шифрования дисков:** для шифрования дисков Azure требуется использовать параметр EnabledForDiskEncryption.
      
     ```azurepowershell-interactive 
     Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForDiskEncryption
     ```

  - **Включение хранилища ключей для развертывания (если необходимо):** позволяет поставщику ресурсов Microsoft.Compute извлекать секреты из этого хранилища ключей в случае использования ссылки на него при создании ресурса, например виртуальной машины.

     ```azurepowershell-interactive
      Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForDeployment
     ```

  - **Включение хранилища ключей для развертывания шаблона (если необходимо):** позволяет Azure Resource Manager получать секреты из этого хранилища ключей в случае использования ссылки на него при развертывании шаблона.

     ```azurepowershell-interactive             
     Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForTemplateDeployment
     ```

### <a name="bkmk_KVperCLI"></a> Установка политик расширенного доступа для хранилища ключей с помощью Azure CLI
Включите шифрование дисков для хранилища ключей с помощью команды [az keyvault update](/cli/azure/keyvault#az-keyvault-update). 

 - **Включение хранилища ключей для шифрования дисков:** параметр Enabled-for-disk-encryption является обязательным. 

     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-disk-encryption "true"
     ```  

 - **Включение хранилища ключей для развертывания (если необходимо):** позволяет поставщику ресурсов Microsoft.Compute извлекать секреты из этого хранилища ключей в случае использования ссылки на него при создании ресурса, например виртуальной машины.

     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-deployment "true"
     ``` 

 - **Включение хранилища ключей для развертывания шаблона (если необходимо):** позволяет Resource Manager получать секреты из хранилища.
     ```azurecli-interactive  
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-template-deployment "true"
     ```


### <a name="bkmk_KVperrm"></a> Установка политики расширенного доступа для хранилища ключей с помощью портала Azure

1. Выберите хранилище ключей, перейдите на вкладку **Политики доступа** и выберите **Щелкните, чтобы показать политики расширенного доступа**.
2. Установите флажок **Включить доступ к шифрованию дисков Azure для шифрования томов**.
3. Выберите **Включить доступ к виртуальным машинам Azure для развертывания** или **Включить доступ к Azure Resource Manager для развертывания шаблонов**, если это необходимо. 
4. Выберите команду **Сохранить**.

    ![Установка политики расширенного доступа к хранилищу Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)


## <a name="bkmk_KEK"></a> Настройка ключа шифрования ключей (необязательно)
Если вы хотите использовать ключ шифрования ключей (KEK), чтобы добавить уровень безопасности ключей шифрования, поместите KEK в хранилище ключей. Используйте [AzKeyVaultKey добавить](/powershell/module/az.keyvault/add-azkeyvaultkey) командлет, чтобы создать ключ шифрования ключей в хранилище ключей. Кроме того, KEK можно импортировать из своего локального модуля HSM службы управления ключами. Дополнительные сведения см. в документации по [Key Vault](../key-vault/key-vault-hsm-protected-keys.md). Когда ключ шифрования ключей указан, шифрование дисков Azure использует его для упаковки секретов шифрования перед записью в Key Vault.

* При создании ключей, используйте тип ключа RSA. Шифрование дисков Azure пока не поддерживает использование ключей эллиптической кривой.

* Для URL-адресов секрета и ключа шифрования ключей (KEK) хранилища ключей необходимо включить управление версиями. Это требование Azure. Ниже приведены примеры действительных URL-адресов секрета и ключа шифрования ключей.

  * Пример допустимого URL-адреса секрета: *https://contosovault.vault.azure.net/secrets/EncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx* .
  * Пример допустимого URL-адреса ключа шифрования ключей: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx* .

* Шифрование дисков Azure не поддерживает указание номеров портов в URL-адресах секрета и ключа шифрования ключей для хранилища ключей. Ниже приведены примеры недопустимых и допустимых URL-адресов хранилища ключей.

  * Неприемлемый URL-адрес хранилища ключей: *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx* .
  * Приемлемый URL-адрес хранилища ключей: *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx* .


### <a name="bkmk_KEKPSH"></a> Настройка ключа шифрования ключей с помощью Azure PowerShell 
Перед использованием скрипта PowerShell нужно ознакомиться с предварительными требованиями к шифрованию дисков Azure, чтобы понять шаги. Для примера скрипта могут потребоваться изменения в вашей среде. Этот скрипт создает все необходимые компоненты для шифрования дисков Azure и шифрует существующую виртуальную машину IaaS, упаковывая ключ шифрования диска с помощью ключа шифрования ключей. 

 ```powershell
 # Step 1: Create a new resource group and key vault in the same location.
     # Fill in 'MyLocation', 'MyKeyVaultResourceGroup', and 'MySecureVault' with your values.
     # Use Get-AzLocation to get available locations and use the DisplayName.
     # To use an existing resource group, comment out the line for New-AzResourceGroup
     
     $Loc = 'MyLocation';
     $KVRGname = 'MyKeyVaultResourceGroup';
     $KeyVaultName = 'MySecureVault'; 
     New-AzResourceGroup –Name $KVRGname –Location $Loc;
     New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname -Location $Loc;
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $KeyVaultResourceId = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname).ResourceId;
     $diskEncryptionKeyVaultUrl = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname).VaultUri;
     
 #Step 2: Enable the vault for disk encryption.
     Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDiskEncryption;
      
 #Step 3: Create a new key in the key vault with the Add-AzKeyVaultKey cmdlet.
     # Fill in 'MyKeyEncryptionKey' with your value.
     
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     Add-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName -Destination 'HSM';
     $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     
 #Step 4: Encrypt the disks of an existing IaaS VM
     # Fill in 'MySecureVM' and 'MyVirtualMachineResourceGroup' with your values. 
     
     $VMName = 'MySecureVM';
     $VMRGName = 'MyVirtualMachineResourceGroup';
     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
```


 
## <a name="next-steps"></a>Дальнейшие действия
> [!div class="nextstepaction"]
> [Включение шифрования дисков Azure для виртуальных машин Windows IaaS](azure-security-disk-encryption-windows.md)

> [!div class="nextstepaction"]
> [Включение шифрование дисков Azure для Linux](azure-security-disk-encryption-linux.md)

