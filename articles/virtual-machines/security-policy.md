---
title: Политики безопасности и использования
description: Сведения о безопасности и политиках виртуальных машин в Azure.
author: cynthn
ms.service: virtual-machines
ms.subservice: security
ms.workload: infrastructure-services
ms.date: 11/27/2018
ms.author: cynthn
ms.topic: conceptual
ms.openlocfilehash: b7f6d30ffa849f78b0f7baa2d84e4fb01f3bf192
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2020
ms.locfileid: "96500348"
---
# <a name="secure-and-use-policies-on-virtual-machines-in-azure"></a>Обеспечение безопасности и использование политик на виртуальных машинах в Azure

Важно обеспечить безопасность виртуальной машины (ВМ) для приложений, которые вы запускаете. Для защиты виртуальных машин можно применить одну или несколько служб или функций Azure, которые обеспечивают безопасный доступ к виртуальным машинам и безопасное хранение данных. Из этой статьи вы узнаете, как обеспечить защиту своих виртуальных машин и приложений.

## <a name="antimalware"></a>Защита от вредоносных программ;

Спектр современных угроз, с которыми сталкиваются пользователи облачных сред, расширяется быстро, что вынуждает искать средства эффективной защиты, обеспечивающие соответствие требованиям к безопасности и нормативным требованиям. [Антивредоносное ПО Майкрософт для Azure](../security/fundamentals/antimalware.md) предоставляет бесплатную защиту в реальном времени, которая помогает обнаруживать и устранять вирусы, шпионское ПО и другие вредоносные программы. Вы можете настроить предупреждения, уведомляющие о попытках установки или запуска известных вредоносных или нежелательных программ на виртуальной машине. Она не поддерживается на виртуальных машинах под управлением Linux или Windows Server 2008.

## <a name="azure-security-center"></a>Центр безопасности Azure

[Центр безопасности Azure](../security-center/security-center-introduction.md) позволяет предотвращать и обнаруживать угрозы на виртуальных машинах, а также реагировать на эти угрозы. Центр безопасности предоставляет встроенные функции мониторинга безопасности и управления политиками для подписок Azure, помогает выявлять угрозы, которые в противном случае могли бы оказаться незамеченными, и взаимодействует с широким комплексом решений по обеспечению безопасности.

JIT-доступ к центру безопасности можно применить в рамках развертывания виртуальной машины, чтобы заблокировать входящий трафик для виртуальных машин Azure, уменьшая вероятность атак и обеспечивая легкий доступ к виртуальным машинам при необходимости. Если включен режим JIT и пользователь запрашивает доступ к виртуальной машине, Центр безопасности проверяет, какие разрешения есть у пользователя для нее. Если у пользователя есть правильные разрешения, запрос разрешается и Центр безопасности автоматически настраивает группу безопасности сети, которая разрешает передачу входящего трафика через выбранные порты на ограниченный период времени. После истечения этого времени центр безопасности восстанавливает прежнее состояние групп безопасности сети. 

## <a name="encryption"></a>Шифрование

Для управляемых дисков предлагается два метода шифрования. Шифрование на уровне операционной системы, которое является шифрованием дисков Azure, и шифрование на уровне платформы, которое является шифрованием на стороне сервера.

### <a name="server-side-encryption"></a>Шифрование на стороне сервера

Управляемые диски Azure по умолчанию автоматически шифруют данные при сохранении их в облаке. Шифрование на стороне сервера защищает ваши данные и помогает удовлетворить ваши обязательства по обеспечению безопасности и соответствия требованиям Организации. Данные в управляемых дисках Azure шифруются прозрачно с использованием 256-разрядного [шифрования AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), одного из наиболее сильных блочных шифров, который совместим со стандартом FIPS 140-2.

Шифрование не влияет на производительность управляемых дисков. Дополнительные затраты на шифрование не взимается.

Вы можете использовать ключи, управляемые платформой, для шифрования управляемого диска или управлять шифрованием с помощью собственных ключей. Если вы решили управлять шифрованием с помощью собственных ключей, вы можете указать *управляемый клиентом ключ*, который будет использоваться для шифрования и расшифровки всех данных на управляемых дисках. 

Дополнительные сведения о шифровании на стороне сервера см. в статьях, посвященных [Windows](./disk-encryption.md) или [Linux](./disk-encryption.md).

### <a name="azure-disk-encryption"></a>Шифрование дисков Azure

Для повышения уровня безопасности и соответствия требованиям [виртуальных машин Windows](windows/disk-encryption-overview.md) и [Linux](linux/disk-encryption-overview.md) содержимое виртуальных дисков в Azure можно зашифровать. Виртуальные диски на виртуальных машинах Windows шифруются в неактивном состоянии с помощью Bitlocker. А виртуальные диски на виртуальных машинах Linux шифруются в неактивном состоянии с помощью dm-crypt. 

В Azure за шифрование виртуальных дисков плата не взимается. Криптографические ключи хранятся в хранилище ключей Azure с применением защиты программного обеспечения. В качестве альтернативы можно импортировать или создать ключи аппаратных модулей безопасности (HSM), сертифицированных по стандартам уровня 2 FIPS 140-2. Эти криптографические ключи используются для шифрования и расшифровки виртуальных дисков, подключенных к виртуальной машине. Вы сохраняете контроль над этими криптографическими ключами и можете проводить аудит их использования. Субъект-служба Azure Active Directory предоставляет безопасный механизм для выдачи этих криптографических ключей при включении и отключении виртуальных машин.

## <a name="key-vault-and-ssh-keys"></a>Хранилище ключей и ключи SSH

Секреты и сертификаты могут моделироваться как ресурсы и предоставляться решением [Key Vault](../key-vault/general/basic-concepts.md). Создать хранилища ключей для [виртуальных машин Windows](windows/key-vault-setup.md) можно с помощью Azure PowerShell, а для [виртуальных машин Linux](linux/key-vault-setup.md) — с помощью Azure CLI. Вы также можете создать ключи для шифрования.

Политики доступа к хранилищу ключей предоставляют доступ отдельно к ключам, секретам и сертификатам. Например, можно предоставить пользователю доступ только к ключам, но не предоставить разрешения на секреты. Тем не менее разрешения на доступ к ключам и секретам или сертификатам находятся на уровне хранилища. Другими словами, [политика доступа к хранилищу ключей](../key-vault/general/secure-your-key-vault.md) не поддерживает разрешения на уровне объектов.

При подключении к виртуальным машинам необходимо использовать шифрование с открытым ключом, чтобы обеспечить более высокий уровень безопасности при входе на виртуальные машины. Этот процесс предполагает обмен открытыми и закрытыми ключами с использованием команды Secure Shell (SSH), что позволяет аутентифицировать себя, не вводя имя пользователя и пароль. Пароли подвержены атакам методом подбора, особенно на виртуальных машинах с подключением к Интернету, таких как веб-серверы. С помощью пары ключей Secure Shell (SSH) можно создавать [виртуальные машины Linux](linux/mac-create-ssh-keys.md), использующие ключи SSH для проверки подлинности, что позволяет обойтись без использования паролей для входа. Кроме того, с помощью ключей SSH можно подключаться с [виртуальной машины Windows](linux/ssh-from-windows.md) к виртуальной машине Linux.

## <a name="managed-identities-for-azure-resources"></a>Управляемые удостоверения для ресурсов Azure

Распространенная проблема при создании облачных приложений — управление учетными данными в коде для проверки подлинности в облачных службах. Обеспечение безопасности учетных данных является важной задачей. В идеальном случае учетные данные никогда не передаются на рабочие станции разработчиков и не проверяются после изменения в системе управления версиями. Azure Key Vault позволяет обеспечить безопасное хранение учетных данных, секретов, а также других ключей, но для их получения код должен выполнять проверку подлинности в Key Vault. 

Функция управляемых удостоверений для ресурсов Azure в Azure Active Directory решает эту проблему. Функция предоставляет службам Azure автоматически управляемое удостоверение в Azure AD. Удостоверение можно использовать для проверки подлинности в любой службе, которая поддерживает проверку подлинности Azure AD, включая Key Vault, при этом не сохраняя каких-либо учетных данных в коде.  Код, выполняющийся на виртуальной машине, может выполнить запрос на получение маркера из двух конечных точек, доступных только на виртуальной машине. Дополнительные подробные сведения об этой службе см. в статье c общими сведениями об [управляемых удостоверениях для ресурсов Azure](../active-directory/managed-identities-azure-resources/overview.md).   

## <a name="policies"></a>Политики

С помощью [политик Azure](../governance/policy/overview.md) можно определить требуемое поведение [виртуальных машин Windows](./windows/policy.md) и [Linux](./linux/policy.md) вашей организации. С помощью политик организация может обеспечить выполнение различных норм и правил во всей организации. Обязательные для выполнения стандарты поведения помогают снизить риск, что способствует успешной деятельности организации.

## <a name="azure-role-based-access-control"></a>Управление доступом на основе ролей в Azure

Используя [Управление доступом на основе ролей Azure (Azure RBAC)](../role-based-access-control/overview.md), вы можете разделить обязанности внутри группы и предоставить только доступ к пользователям на виртуальной машине, которые им необходимы для выполнения своих задач. Вместо того чтобы предоставлять всем неограниченные разрешения для виртуальной машины, можно разрешить только определенные действия. Вы можете настроить управление доступом для виртуальной машины на [портале Azure](../role-based-access-control/role-assignments-portal.md) с помощью [Azure CLI](/cli/azure/role) или [Azure PowerShell](../role-based-access-control/role-assignments-powershell.md).


## <a name="next-steps"></a>Дальнейшие действия
- Выполните действия для мониторинга безопасности виртуальной машины в центре безопасности Azure для [Linux](../security/fundamentals/overview.md) или [Windows](./tutorial-azure-security.md).