---
title: Руководство разработчика хранилища ключей Azure
description: Разработчики могут использовать хранилище ключей Azure для управления криптографическими ключами в среде Microsoft Azure.
services: key-vault
author: msmbaldwin
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: mbaldwin
ms.openlocfilehash: a8cb0ea9fb3c6e8388271c4274baf4ecc7282cda
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "80247237"
---
# <a name="azure-key-vault-developers-guide"></a>Руководство разработчика хранилища ключей Azure

Хранилище Key Vault обеспечивает безопасный доступ к конфиденциальной информации из приложений:

- Ключи и секреты защищены без необходимости написания кода. Вы сможете легко использовать их в своих приложениях.
- Ваши клиенты смогут управлять собственными ключами, поэтому вы можете сосредоточиться на предоставлении базовых функций программного обеспечения. Таким образом, ваши приложения не будут владеть ответственностью или потенциальной ответственностью за ключи и секреты арендаторов ваших клиентов.
- Ваше приложение может использовать ключи для подписывания и шифрования, но осуществляет управление ключами во внешней среде, чтобы ваше решение могло работать в качестве географически распределенного приложения.
- Управление сертификатами Key Vault. См. дополнительные сведения о [ключах, секретах и сертификатах](about-keys-secrets-and-certificates.md).

Дополнительные сведения о хранилище ключей Azure см. в статье [Что такое хранилище ключей Azure?](key-vault-overview.md).

## <a name="public-previews"></a>Общедоступные предварительные версии

Периодически мы выпускаем общедоступные предварительные версии нового компонента Key Vault. Предлагаем вам протестировать их и отправить отзыв по нашему адресу электронной почты для обратной связи: azurekeyvault@microsoft.com.

## <a name="creating-and-managing-key-vaults"></a>Создание хранилищ ключей и управление ими

Azure Key Vault позволяет обеспечить безопасное хранение учетных данных, а также других ключей и секретов, но для их получения код должен выполнять аутентификацию в Key Vault. Управляемые удостоверения для ресурсов Azure упрощают решение этой задачи, предоставляя службам Azure автоматически управляемое удостоверение в Azure Active Directory (Azure AD). Это удостоверение можно использовать для аутентификации в любой службе, которая поддерживает аутентификацию Azure AD, включая Key Vault, не храня какие-либо учетные данные в коде. 

Дополнительные сведения об управляемых удостоверениях для ресурсов Azure см. в [обзоре управляемых удостоверений](../active-directory/managed-identities-azure-resources/overview.md). Дополнительные сведения о работе с AAD см. в статье [Интеграция приложений с Azure Active Directory](../active-directory/develop/active-directory-integrating-applications.md).

Прежде чем приступить к работе с ключами, секретами и сертификатами в хранилище ключей, нужно создать его. Вы можете создать хранилище ключей и управлять им с помощью интерфейса командной строки (CLI), PowerShell, шаблонов Resource Manager или REST, как описано в следующих статьях:

- [Создание хранилищ ключей и управление ими с помощью CLI](quick-create-cli.md).
- [Создание хранилищ ключей и управление ими с помощью PowerShell](quick-create-powershell.md).
- [Создавайте и управляйте ключами Vaults с помощью порта Azure](quick-create-portal.md)
- [Создавайте и управляйте ключами Vaults с помощью Python](quick-create-python.md)
- [Создавайте и управляйте ключами Vaults с помощью Java](quick-create-java.md)
- [Создавайте и управляйте ключами Vaults с помощью Node.js](quick-create-node.md)
- [Создавайте и управляйте ключами Vaults с помощью .NET (v4 SDK)](quick-create-net.md)
- [Create a key vault and add a secret via an Azure Resource Manager template](quick-create-template.md) (Создание хранилища ключей и добавление секрета с помощью шаблона Azure Resource Manager)
- [Создание хранилищ ключей и управление ими с помощью REST](/rest/api/keyvault/).


## <a name="coding-with-key-vault"></a>Программирование с помощью хранилища ключей

Система управления Key Vault для программистов включает несколько интерфейсов. В этом разделе содержится несколько примеров кода и ссылки на все языки. 

### <a name="supported-programming-and-scripting-languages"></a>Поддерживаемые языки программирования и написания сценариев

#### <a name="rest"></a>REST

Интерфейс REST предоставляет доступ ко всем ресурсам Key Vault: хранилищам, ключам, секретам и т. д. 

[Справочник по REST API для Key Vault](/rest/api/keyvault/)

#### <a name="net"></a>.NET

[Справочник по API .NET для Key Vault](/dotnet/api/overview/azure/key-vault?view=azure-dotnet).

Дополнительные сведения о версии 2.x пакета SDK для .NET см. в [заметках о выпуске](key-vault-dotnet2api-release-notes.md).

#### <a name="java"></a>Java

[Пакет Java SDK для Key Vault](/java/api/overview/azure/keyvault)

#### <a name="nodejs"></a>Node.js

В Node.js API управления Key Vault не включен в API объекта Key Vault. В следующей обзорной статье объясняется, как получить доступ к этим API. 

[Модули Azure Key Vault для Node.js](/javascript/api/overview/azure/key-vault?view=azure-node-latest)

#### <a name="python"></a>Python

[Библиотеки Azure Key Vault для Python](/python/api/overview/azure/key-vault?view=azure-python)

#### <a name="azure-cli"></a>Azure CLI

[Azure CLI для Key Vault](/cli/azure/keyvault?view=azure-cli-latest)

#### <a name="azure-powershell"></a>Azure PowerShell 

[Azure PowerShell для Key Vault](/powershell/module/az.keyvault/?view=azps-3.6.1#key_vault)

### <a name="code-examples"></a>Примеры кода

Полные примеры использования хранилища ключей с приложениями см. в следующих документах:

- [Примеры кода Azure Key Vault](https://azure.microsoft.com/resources/samples/?service=key-vault) — примеры кода для Azure Key Vault. 
- [Использование Azure Key Vault из Web-приложений](quick-create-net.md) — руководство по использованию Azure Key Vault из веб-приложений Azure. 

## <a name="how-tos"></a>Инструкции

В следующих статьях приводятся рекомендации по решению конкретных задач при работе с Azure Key Vault:

- [Изменение идентификатора клиента хранилища ключей после перемещения подписки](key-vault-subscription-move-fix.md). При перемещении подписки Azure из клиента A в клиент Б имеющиеся хранилища ключей недоступны для субъектов (пользователей и приложений) в клиенте Б. Устраните эту проблему, следуя указаниям в этом руководстве.
- [Доступ к хранилищу ключей под защитой брандмауэра](key-vault-access-behind-firewall.md). Для доступа к хранилищу ключей необходимо, чтобы клиентское приложение имело доступ к нескольким конечным точкам, требуемым для различных функций.
- [Создание ключей, защищенных аппаратным модулем безопасности, и их передача в хранилище ключей Azure](key-vault-hsm-protected-keys.md) — Данная статья поможет вам подготовить и создать ваши собственные ключи, защищенные аппаратным модулем безопасности, а затем передать их в хранилище ключей Azure.
- [Передача безопасных значений (например, паролей) во время развертывания](../azure-resource-manager/templates/key-vault-parameter.md) — Если в процессе развертывания в качестве параметра необходимо передать безопасное значение (например, пароль), его можно сохранить как секретный код в хранилище ключей Azure и вставить ссылку на это значение в другие шаблоны Resource Manager.
- [Использование хранилища ключей для расширенного управления ключами с помощью SQL Server](https://msdn.microsoft.com/library/dn198405.aspx) —Соединитель SQL Server для хранилища ключей Azure позволяет SQL Server и SQL в виртуальных машинах использовать службу хранилища ключей Azure в качестве поставщика расширенного управления ключами (EKM) для защиты ключей шифрования для связи приложений, а также предоставляет прозрачное шифрование данных, шифрование резервной копии и шифрование на уровне столбцов.
- [Развертывание сертификатов на виртуальных машинах из хранилища ключей](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) : облачное приложение, которое выполняется на виртуальной машине в Azure, требует сертификата. Как получить этот сертификат для виртуальной машины?
- [Настройка полной смены ключей и аудита в Key Vault](key-vault-key-rotation-log-monitoring.md) — руководство по настройке смены ключей и аудита журналов с помощью Azure Key Vault.
- [Deploying Azure Web App Certificate through Key Vault]( https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/) (Развертывание сертификата службы веб-приложения Azure с помощью Key Vault) — пошаговое руководство по развертыванию сертификатов, хранящихся в Key Vault, в рамках предложения [Сертификаты службы приложений](https://azure.microsoft.com/blog/internals-of-app-service-certificate/).
- [Предоставление разрешения на доступ к Key Vault нескольким приложениям](key-vault-group-permissions-for-apps.md). Политика контроля доступа Key Vault поддерживает не более 1024 приложений. Однако вы можете создать группу безопасности Azure Active Directory. Добавьте все связанные субъекты-службы в эту группу безопасности, а затем предоставьте доступ данной группе безопасности к Key Vault.
- Руководство по интеграции и использованию хранилищ ключей в Azure см. в этой статье [с примерами шаблонов Azure Resource Manager для хранилища ключей от Райана Джонса (Ryan Jones)](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
- В разделе [Как использовать обратимое удаление в Key Vault с помощью интерфейса командной строки](key-vault-soft-delete-cli.md) описывается использование и жизненный цикл хранилища ключей, а также различных объектов хранилища ключей с возможностью обратимого удаления.
- В разделе [Как использовать обратимое удаление в Key Vault с помощью PowerShell](key-vault-soft-delete-powershell.md) описывается использование и жизненный цикл хранилища ключей, а также различных объектов хранилища ключей, поддерживающих обратимое удаление.

## <a name="integrated-with-key-vault"></a>Интеграция с хранилищем ключей

Эти статьи посвящены другим сценариям и службам, использующим Key Vault или интегрирующимся с ним.

- [Лазурное шифрование дисков](../security/fundamentals/encryption-overview.md) использует стандартную функцию [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) в отрасли Windows и функцию [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux для обеспечения громкости шифрования для ОС и дисков данных. Это решение интегрировано с хранилищем ключей Azure. Решение позволяет управлять ключами и секретами дискового шифрования через подписку хранилища ключей. Шифрование выполняется для всех данных на дисках виртуальных машин в хранилище Azure.
- В [Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md) можно включить шифрование данных, хранящихся в учетной записи. Data Lake Store предоставляет два режима для управления главными ключами шифрования (MEKs), необходимыми для расшифровки любых данных, хранящихся в Data Lake Store. Вы можете позволить Data Lake Store управлять главными ключами шифрования или управлять ими самостоятельно с помощью учетной записи хранилища ключей Azure. Способ управления ключами можно задать во время создания учетной записи Data Lake Store.
- [Azure Information Protection](/azure/information-protection/plan-implement-tenant-key) позволяет управлять собственным ключом клиента. Например, вместо того, чтобы вашим ключом клиента управляла корпорация Майкрософт (по умолчанию), вы можете сами управлять им в соответствии с определенными нормами своей организации. Сценарий с использованием собственного ключа клиента называется BYOK.

## <a name="key-vault-overviews-and-concepts"></a>Основные сведения о Key Vault

- [Общие сведения о функции обратимого удаления в Azure Key Vault](key-vault-ovw-soft-delete.md). Описание функции, которая позволяет восстанавливать случайно или намеренно удаленные объекты.
- [Руководство по регулированию Azure Key Vault](key-vault-ovw-throttling.md). Описание основных механизмов регулирования числа запросов и подходов к созданию приложения.
- [Key Vault storage account keys overview](key-vault-ovw-storage-keys.md) (Общие сведения об учетной записи хранения Key Vault). Описание интеграции Key Vault с ключами учетных записей службы хранилища Azure.
- [Системы безопасности и географические ограничения Azure Key Vault](key-vault-ovw-security-worlds.md). Описание взаимосвязей между регионами и областями безопасности.

## <a name="social"></a>Социальные сети

- [Блог хранилища ключей](https://aka.ms/kvblog)
- [Форум хранилища ключей](https://aka.ms/kvforum)

## <a name="supporting-libraries"></a>Поддержка библиотек

- [Библиотека ядра Key Vault Microsoft Azure](https://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) предоставляет интерфейсы **IKey** и **IKeyResolver** для поиска ключей по идентификаторам и выполнения операций с ключами.
- [Расширения хранилища ключей Microsoft Azure](https://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) предоставляют расширенные возможности для хранилища ключей Azure.
