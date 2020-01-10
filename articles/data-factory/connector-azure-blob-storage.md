---
title: Копирование и преобразование данных в хранилище BLOB-объектов Azure
description: Узнайте, как копировать данные в хранилище BLOB-объектов и из него, а затем преобразовывать данные в хранилище BLOB-объектов с помощью фабрики данных.
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/12/2019
ms.openlocfilehash: be7491bb141330b04c2384038e8ef34a727d1317
ms.sourcegitcommit: 8b37091efe8c575467e56ece4d3f805ea2707a64
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75830298"
---
# <a name="copy-and-transform-data-in-azure-blob-storage-by-using-azure-data-factory"></a>Копирование и преобразование данных в хранилище BLOB-объектов Azure с помощью фабрики данных Azure

> [!div class="op_single_selector" title1="Выберите используемую версию службы "Фабрика данных":"]
> * [Версия 1](v1/data-factory-azure-blob-connector.md)
> * [Текущая версия](connector-azure-blob-storage.md)

В этой статье описывается, как с помощью действия копирования в фабрике данных Azure копировать данные в хранилище BLOB-объектов Azure и в него, а затем использовать поток данных для преобразования данных в хранилище BLOB-объектов Azure. Дополнительные сведения о Фабрике данных Azure см. во [вводной статье](introduction.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="supported-capabilities"></a>Поддерживаемые возможности

Этот соединитель больших двоичных объектов Azure поддерживается для следующих действий:

- [Действие копирования](copy-activity-overview.md) с [поддерживаемой матрицей источника и приемника](copy-activity-overview.md)
- [Поток данных сопоставления](concepts-data-flow-overview.md)
- [Действие поиска](control-flow-lookup-activity.md)
- [Действие получения метаданных в Фабрике данных Azure](control-flow-get-metadata-activity.md)
- [Удалить действие](delete-activity.md)

Для действия копирования этот соединитель хранилища BLOB-объектов поддерживает:

- Копирование больших двоичных объектов в учетные записи хранения Azure общего назначения, в хранилище горячих и холодных BLOB-объектов и из них. 
- Копирование BLOB-объектов с помощью ключа учетной записи, проверка подлинности ресурсов Azure на основе подписанного URL-адреса, субъект-службы или управляемых удостоверений.
- Копирование больших двоичных объектов из блочных, добавочных или страничных BLOB-объектов, а также копирование данных только в блочные BLOB-объекты.
- Копирование больших двоичных объектов "как есть", анализ или создание больших двоичных объектов путем использования [поддерживаемых форматов файлов и кодеков сжатия](supported-file-formats-and-compression-codecs.md).
- [Сохранять метаданные файла во время копирования](#preserve-metadata-during-copy).

>[!IMPORTANT]
>Если включить параметр **разрешить доверенным службам Майкрософт доступ к этой учетной записи хранения** в параметрах брандмауэра службы хранилища Azure и вы хотите использовать среду выполнения интеграции Azure для подключения к хранилищу BLOB-объектов, необходимо использовать [проверку подлинности управляемого удостоверения](#managed-identity).

## <a name="get-started"></a>Начать

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Следующие разделы содержат сведения о свойствах, которые используются для определения сущностей фабрики данных, характерных для хранилища BLOB-объектов.

## <a name="linked-service-properties"></a>Свойства связанной службы

Дополнительные сведения о типах аутентификации, поддерживаемых соединителем больших двоичных объектов Azure, см. в следующих разделах.

- [Проверка подлинности на основе ключа учетной записи](#account-key-authentication)
- [Аутентификация SAS](#shared-access-signature-authentication)
- [Проверка подлинности субъекта-службы](#service-principal-authentication)
- [Проверка подлинности на основе управляемого удостоверения службы](#managed-identity)

>[!NOTE]
>При использовании Polybase для загрузки данных в хранилище данных SQL, если в исходном или промежуточном хранилище BLOB-объектов настроена конечная точка виртуальной сети, необходимо использовать управляемую проверку подлинности идентификации в соответствии с требованиями Polybase и использовать локально размещенную Integration Runtime с версией 3,18 или более поздней версии. Дополнительные предварительные требования см. в разделе [Проверка подлинности управляемого удостоверения](#managed-identity) .

>[!NOTE]
>Операции HDInsight и Машинное обучение Azure поддерживают только проверку подлинности с помощью ключа учетной записи хранилища BLOB-объектов Azure.

### <a name="account-key-authentication"></a>Проверка подлинности на основе ключа учетной записи

При использовании проверки подлинности на основе ключа учетной записи поддерживаются следующие свойства.

| Свойство | Description | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойству type должно быть присвоено значение **AzureBlobStorage** (рекомендуется) или **AzureStorage** (см. примечания ниже). |Да |
| connectionString | В свойстве connectionString указываются сведения, необходимые для подключения к службе хранилища. <br/> Вы можете также поместить ключ учетной записи в Azure Key Vault и извлечь конфигурацию `accountKey` из строки подключения. Ознакомьтесь с приведенными ниже примерами и подробными сведениями в статье [Хранение учетных данных в Azure Key Vault](store-credentials-in-key-vault.md). |Да |
| connectVia | [Среда выполнения интеграции](concepts-integration-runtime.md), используемая для подключения к хранилищу данных. Вы можете использовать среду выполнения интеграции Azure или локальную среду IR (если хранилище данных расположено в частной сети). Если не указано другое, по умолчанию используется интегрированная среда выполнения Azure. |Нет |

>[!NOTE]
>Дополнительная конечная точка службы BLOB-объектов не поддерживается при использовании проверки подлинности ключа учетной записи. Можно использовать другие типы проверки подлинности.

>[!NOTE]
>При использовании связанной службы тип AzureStorage по-прежнему поддерживается как есть, в то время как вам предлагается использовать новый тип связанной службы — AzureBlobStorage.

**Пример**.

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Пример: ключ учетной записи хранения в Azure Key Vault**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;",
            "accountKey": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }            
    }
}
```

### <a name="shared-access-signature-authentication"></a>Аутентификация SAS

Подпись общего доступа обеспечивает делегированный доступ к ресурсам в вашей учетной записи хранения. Вы можете использовать подписанный URL-адрес, чтобы предоставить клиенту ограниченные разрешения на работу с объектами в вашей учетной записи хранения на определенный период. Не нужно предоставлять совместный доступ к ключам доступа для учетной записи. Подписанный URL-адрес — это универсальный код ресурса (URI), который в своих параметрах запроса содержит все сведения, необходимые для доступа к ресурсу хранилища с прохождением аутентификации. Для доступа к ресурсам хранилища с помощью подписанного URL-адреса клиенту достаточно передать SAS в соответствующий конструктор или метод. Дополнительные сведения о подписанном URL-адресе см. в статье [Использование подписанных URL-адресов (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
>- Сейчас Фабрика данных поддерживает как **подписанные URL-адреса уровня службы**, так и **подписанные URL-адреса уровня учетной записи**. Дополнительные сведения о подписанных URL-адресах см. [в статье предоставление ограниченного доступа к ресурсам службы хранилища Azure с помощью подписанных URL (SAS)](../storage/common/storage-sas-overview.md).
>- В более поздней конфигурации набора данных путь к папке — это полный путь, начинающийся с уровня контейнера. Вам нужно настроить такой путь, соответствующий пути в вашем URI SAS.

> [!TIP]
> Чтобы создать подписанный URL-адрес уровня службы для учетной записи хранения, можно выполнить следующие команды PowerShell. Замените заполнители и предоставьте необходимое разрешение.
> `$context = New-AzStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

При использовании аутентификации SAS поддерживаются следующие свойства.

| Свойство | Description | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойству type должно быть присвоено значение **AzureBlobStorage** (рекомендуется) или **AzureStorage** (см. примечания ниже). |Да |
| sasUri | Укажите URI подписанного URL-адреса для ресурсов службы хранилища, например для большого двоичного объекта или контейнера. <br/>Пометьте это поле как SecureString, чтобы безопасно хранить его в Фабрике данных. Также можно разместить маркер SAS в Azure Key Vault, чтобы использовать автоматический поворот и удалить часть токена. Ознакомьтесь с приведенными ниже примерами и подробными сведениями в статье [Хранение учетных данных в Azure Key Vault](store-credentials-in-key-vault.md). |Да |
| connectVia | [Среда выполнения интеграции](concepts-integration-runtime.md), используемая для подключения к хранилищу данных. Вы можете использовать среду выполнения интеграции Azure или локальную среду IR (если хранилище данных расположено в частной сети). Если не указано другое, по умолчанию используется интегрированная среда выполнения Azure. |Нет |

>[!NOTE]
>При использовании связанной службы тип AzureStorage по-прежнему поддерживается как есть, в то время как вам предлагается использовать новый тип связанной службы — AzureBlobStorage.

**Пример**.

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource e.g. https://<container>.blob.core.windows.net/?sv=<storage version>&amp;st=<start time>&amp;se=<expire time>&amp;sr=<resource>&amp;sp=<permissions>&amp;sip=<ip range>&amp;spr=<protocol>&amp;sig=<signature>>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Пример: ключ учетной записи хранения в Azure Key Vault**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource without token e.g. https://<container>.blob.core.windows.net/>"
            },
            "sasToken": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

При создании URI подписанного URL-адреса необходимо учитывать следующее.

- Задайте для объектов соответствующие разрешения на чтение или запись. Они устанавливаются с учетом назначения связанной службы (чтение, запись, чтение и запись) в фабрике данных.
- Задайте **время окончания срока действия** соответствующим образом. Убедитесь, что срок действия доступа к объектам хранилища не истекает в период активности конвейера.
- В зависимости от потребности URI следует создать для нужного контейнера или большого двоичного объекта. URI подписанного URL-адреса для большого двоичного объекта позволяет фабрике данных получить доступ к определенному большому двоичному объекту. URI подписанного URL-адреса для контейнера хранилища BLOB-объектов позволяет Фабрике данных выполнить итерацию по большим двоичным объектам в этом контейнере. Чтобы предоставить доступ к большему или меньшему количеству объектов позднее или обновить URI подписанного URL-адреса, не забудьте обновить связанную службу с помощью нового URI.

### <a name="service-principal-authentication"></a>Проверка подлинности субъекта-службы

Дополнительные сведения о проверке подлинности субъекта-службы хранилища Azure см. в статье [Аутентификация доступа к службе хранилища Azure с помощью Azure Active Directory (предварительная версия)](../storage/common/storage-auth-aad.md).

Чтобы использовать проверку подлинности субъекта-службы, выполните следующие действия.

1. Чтобы зарегистрировать сущность приложения в Azure Active Directory (Azure AD), необходимо следовать указаниям из раздела [Регистрация приложения в клиенте Azure AD](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Запишите следующие значения, которые используются для определения связанной службы:

    - Идентификатор приложения
    - Ключ приложения
    - Tenant ID

2. Предоставьте правильное разрешение субъекта-службы в Хранилище BLOB-объектов Azure. Дополнительные сведения о ролях см. в статье [Manage access rights to Azure Storage data with RBAC](../storage/common/storage-auth-aad-rbac.md) (Управление ролями доступа к данным службы хранилища Azure с помощью RBAC).

    - **Для источника** в Системе управления идентификацией и доступом (IAM) предоставьте по крайней мере роль **читателя данных Storage Blob**.
    - В Системе управления идентификацией и доступом (IAM) **приемнику** необходимо выделить роль не ниже **участника для данных больших двоичных объектов хранилища**.

Для связанной службы хранилища BLOB-объектов Azure поддерживаются следующие свойства.

| Свойство | Description | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Для свойства type необходимо задать значение **AzureBlobStorage**. |Да |
| serviceEndpoint | Укажите конечную точку хранилища больших двоичных объектов Azure с шаблоном `https://<accountName>.blob.core.windows.net/`. |Да |
| servicePrincipalId | Укажите идентификатора клиента приложения. | Да |
| servicePrincipalKey | Укажите ключ приложения. Пометьте это поле как **SecureString**, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). | Да |
| tenant | Укажите сведения о клиенте (доменное имя или идентификатор клиента), в котором находится приложение. Его можно получить, наведя указатель мыши на правый верхний угол страницы портала Azure. | Да |
| connectVia | [Среда выполнения интеграции](concepts-integration-runtime.md), используемая для подключения к хранилищу данных. Вы можете использовать среду выполнения интеграции Azure или локальную среду IR (если хранилище данных расположено в частной сети). Если не указано другое, по умолчанию используется интегрированная среда выполнения Azure. |Нет |

>[!NOTE]
>Проверка подлинности субъекта-службы поддерживается только таким типом связанной службы, как AzureBlobStorage, а не предыдущим типом связанной службы AzureStorage.

**Пример**.

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {            
            "serviceEndpoint": "https://<accountName>.blob.core.windows.net/",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>" 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Управляемые удостоверения для аутентификации ресурсов Azure

Фабрика данных может быть связана с [управляемым удостоверением для ресурсов Azure](data-factory-service-identity.md), которое представляет эту фабрику данных. Вы можете напрямую использовать это управляемое удостоверение для проверки подлинности хранилища BLOB-объектов, как при использовании собственного субъекта-службы. Оно разрешает назначенной фабрике данных обращаться к данным и копировать их из службы хранилища BLOB-объектов и в нее.

Общие сведения о проверке подлинности хранилища Azure см. в статье [Authenticate access to Azure Storage using Azure Active Directory](../storage/common/storage-auth-aad.md) (Проверка подлинности при доступе к службе хранилища Azure с помощью Azure Active Directory). Чтобы использовать управляемые удостоверения для проверки подлинности ресурсов Azure, выполните следующие действия.

1. [Получите управляемые сведения об удостоверении фабрики данных](data-factory-service-identity.md#retrieve-managed-identity) , скопировав значение "идентификатор приложения удостоверений службы", созданное вместе с фабрикой.

2. Предоставьте правильное разрешение управляемому удостоверению в Хранилище BLOB-объектов Azure. Дополнительные сведения о ролях см. в статье [Manage access rights to Azure Storage data with RBAC](../storage/common/storage-auth-aad-rbac.md) (Управление ролями доступа к данным службы хранилища Azure с помощью RBAC).

    - **Для источника** в Системе управления идентификацией и доступом (IAM) предоставьте по крайней мере роль **читателя данных Storage Blob**.
    - В Системе управления идентификацией и доступом (IAM) **приемнику** необходимо выделить роль не ниже **участника для данных больших двоичных объектов хранилища**.

>[!IMPORTANT]
>Если вы используете Polybase для загрузки данных из большого двоичного объекта (в качестве источника или промежуточного хранения) в хранилище данных SQL, то при использовании проверки подлинности управляемого удостоверения для большого двоичного объекта убедитесь, что вы также выполняйте шаги 1 и 2 в [этом руководстве](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage) , чтобы 1) зарегистрировать сервер базы данных SQL с помощью Azure Active Directory (Azure AD) и 2) назначить роль участника данных BLOB- остальные обрабатываются фабрикой данных. Если хранилище BLOB-объектов настроено с конечной точкой виртуальной сети Azure, чтобы использовать Polybase для загрузки данных из нее, необходимо использовать управляемую проверку подлинности с идентификацией в соответствии с требованиями Polybase.

Для связанной службы хранилища BLOB-объектов Azure поддерживаются следующие свойства.

| Свойство | Description | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Для свойства type необходимо задать значение **AzureBlobStorage**. |Да |
| serviceEndpoint | Укажите конечную точку хранилища больших двоичных объектов Azure с шаблоном `https://<accountName>.blob.core.windows.net/`. |Да |
| connectVia | [Среда выполнения интеграции](concepts-integration-runtime.md), используемая для подключения к хранилищу данных. Вы можете использовать среду выполнения интеграции Azure или локальную среду IR (если хранилище данных расположено в частной сети). Если не указано другое, по умолчанию используется интегрированная среда выполнения Azure. |Нет |

> [!NOTE]
> Проверка подлинности ресурсов Azure с помощью управляемых удостоверений поддерживается только таким типом связанной службы как AzureBlobStorage, но не предыдущим типом связанной службы AzureStorage. 

**Пример**.

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {            
            "serviceEndpoint": "https://<accountName>.blob.core.windows.net/"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Свойства набора данных

Полный список разделов и свойств, доступных для определения наборов данных, см. в статье о [наборах данных](concepts-datasets-linked-services.md). 

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Для большого двоичного объекта Azure поддерживаются следующие свойства в параметрах `location` в наборе данных на основе формата:

| Свойство   | Description                                                  | Обязательно для заполнения |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Свойство Type расположения в наборе данных должно иметь значение **азуреблобсторажелокатион**. | Да      |
| контейнер  | Контейнер больших двоичных объектов.                                          | Да      |
| folderPath | Путь к папке в заданном контейнере. Если вы хотите использовать подстановочный знак для фильтрации папки, пропустите этот параметр и укажите в параметрах источника действия. | Нет       |
| fileName   | Имя файла в заданном контейнере + folderPath. Если вы хотите использовать подстановочные знаки для фильтрации файлов, пропустите этот параметр и укажите в параметрах источника действия. | Нет       |

**Пример**.

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Свойства действия копирования

Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure](concepts-pipelines-activities.md). В этом разделе содержится список свойств, поддерживаемых источником и приемником хранилища BLOB-объектов.

### <a name="blob-storage-as-a-source-type"></a>Хранилище BLOB-объектов в качестве типа источника

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Следующие свойства поддерживаются для большого двоичного объекта Azure в параметрах `storeSettings` в источнике копирования на основе формата:

| Свойство                 | Description                                                  | Обязательно для заполнения                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | Свойство Type в разделе `storeSettings` должно иметь значение **азуреблобсторажереадсеттингс**. | Да                                           |
| recursive                | Указывает, следует ли читать данные рекурсивно из вложенных папок или только из указанной папки. Обратите внимание, что если для свойства recursive задано значение true, а приемником является файловое хранилище, пустые папки и вложенные папки не создаются в приемнике. Допустимые значения: **true** (по умолчанию) и **false**. | Нет                                            |
| вилдкардфолдерпас       | Путь к папке с подстановочными знаками в заданном контейнере, настроенном в наборе данных для фильтрации исходных папок. <br>Допустимые подстановочные знаки: `*` (соответствует нулю или большему количеству знаков) и `?` (соответствует нулю или одному знаку). Для экранирования используйте `^`, если фактическое имя папки содержит подстановочный знак или escape-символ. <br>Дополнительные примеры приведены в разделе [Примеры фильтров папок и файлов](#folder-and-file-filter-examples). | Нет                                            |
| вилдкардфиленаме         | Имя файла с подстановочными знаками в заданном контейнере + folderPath/Вилдкардфолдерпас для фильтрации исходных файлов. <br>Допустимые подстановочные знаки: `*` (соответствует нулю или большему количеству знаков) и `?` (соответствует нулю или одному знаку). Для экранирования используйте `^`, если фактическое имя папки содержит подстановочный знак или escape-символ.  Дополнительные примеры приведены в разделе [Примеры фильтров папок и файлов](#folder-and-file-filter-examples). | Да, если `fileName` не указан в наборе данных |
| modifiedDatetimeStart    | Фильтр файлов на основе атрибута: Последнее изменение. Файлы будут выбраны, если время их последнего изменения находится в диапазоне времени `modifiedDatetimeStart` и `modifiedDatetimeEnd`. Время представлено часовым поясом UTC в формате "2018-12-01T05:00:00Z". <br> Свойства могут иметь значение NULL. Это означает, что фильтры атрибута файла не будут применяться к набору данных.  Если для параметра `modifiedDatetimeStart` задано значение даты и времени, но параметр `modifiedDatetimeEnd` имеет значение NULL, то будут выбраны файлы, чей атрибут последнего изменения больше указанного значения даты и времени или равен ему.  Если для параметра `modifiedDatetimeEnd` задано значение даты и времени, но параметр `modifiedDatetimeStart` имеет значение NULL, то будут выбраны все файлы, чей атрибут последнего изменения меньше указанного значения даты и времени. | Нет                                            |
| modifiedDatetimeEnd      | То же, что и выше.                                               | Нет                                            |
| maxConcurrentConnections | Число подключений для одновременного подключения к хранилищу хранилища. Укажите, только если требуется ограничить одновременный подключение к хранилищу данных. | Нет                                            |

> [!NOTE]
> Для формата текста Parquet/с разделителями **BlobSource** источник действия копирования типа, упомянутый в следующем разделе, по-прежнему поддерживается "как есть" для обеспечения обратной совместимости. Рекомендуется использовать эту новую модель для перемотки, и пользовательский интерфейс создания ADF переключился на создание этих новых типов.

**Пример**.

```json
"activities":[
    {
        "name": "CopyFromBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSettings",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AzureBlobStorageReadSettings",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="blob-storage-as-a-sink-type"></a>Хранилище BLOB-объектов в качестве типа приемника

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Следующие свойства поддерживаются для большого двоичного объекта Azure в параметрах `storeSettings` в приемнике копирования на основе формата:

| Свойство                 | Description                                                  | Обязательно для заполнения |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | Свойство Type в разделе `storeSettings` должно иметь значение **азуреблобсторажевритесеттингс**. | Да      |
| copyBehavior             | Определяет поведение копирования, когда источником являются файлы из файлового хранилища данных.<br/><br/>Допустимые значения:<br/><b>— PreserveHierarchy (по умолчанию)</b>. Сохраняет иерархию файлов в целевой папке. Относительный путь исходного файла в исходной папке идентичен относительному пути целевого файла в целевой папке.<br/><b>— FlattenHierarchy</b>. Все файлы из исходной папки размещаются на первом уровне в целевой папке. Целевые файлы имеют автоматически сформированные имена. <br/><b>— MergeFiles</b>. Объединяет все файлы из исходной папки в один файл. Если указано имя Blob-объекта или имя файла, то оно присваивается объединенному файлу. В противном случае присваивается автоматически созданное имя файла. | Нет       |
| maxConcurrentConnections | Число подключений для одновременного подключения к хранилищу хранилища. Укажите, только если требуется ограничить одновременный подключение к хранилищу данных. | Нет       |

**Пример**.

```json
"activities":[
    {
        "name": "CopyFromBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Parquet output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "ParquetSink",
                "storeSettings":{
                    "type": "AzureBlobStorageWriteSettings",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Примеры фильтров папок и файлов

В этом разделе описываются результаты применения фильтров с подстановочными знаками к пути папки и имени файла.

| folderPath | fileName | recursive | Структура исходной папки и результат фильтрации (извлекаются файлы, выделенные **полужирным** шрифтом)|
|:--- |:--- |:--- |:--- |
| `container/Folder*` | (пусто, используйте по умолчанию) | false | контейнер<br/>&nbsp;&nbsp;&nbsp;&nbsp;ПапкаA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Вложенная_папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Другая_папкаB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл6.csv |
| `container/Folder*` | (пусто, используйте по умолчанию) | true | контейнер<br/>&nbsp;&nbsp;&nbsp;&nbsp;ПапкаA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Вложенная_папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Другая_папкаB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл6.csv |
| `container/Folder*` | `*.csv` | false | контейнер<br/>&nbsp;&nbsp;&nbsp;&nbsp;ПапкаA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Вложенная_папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Другая_папкаB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл6.csv |
| `container/Folder*` | `*.csv` | true | контейнер<br/>&nbsp;&nbsp;&nbsp;&nbsp;ПапкаA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Вложенная_папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Другая_папкаB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл6.csv |

### <a name="some-recursive-and-copybehavior-examples"></a>Некоторые примеры recursive и copyBehavior

В данном разделе описываются результаты выполнения операции копирования при использовании различных сочетаний значений recursive и copyBehavior.

| recursive | copyBehavior | Структура папок источника | Результаты цели |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Файл1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Файл2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Вложенная_папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл5 | Целевая папка "Папка1" создается с такой же структурой, как и исходная папка:<br/><br/>Папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Файл1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Файл2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Вложенная_папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл5 |
| true |flattenHierarchy | Папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Файл1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Файл2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Вложенная_папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл5 | Целевая папка "Папка1" создается со следующей структурой: <br/><br/>Папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;автоматически созданное имя для "Файл1"<br/>&nbsp;&nbsp;&nbsp;&nbsp;автоматически созданное имя для "Файл2"<br/>&nbsp;&nbsp;&nbsp;&nbsp;автоматически созданное имя для "Файл3"<br/>&nbsp;&nbsp;&nbsp;&nbsp;автоматически созданное имя для "Файл4"<br/>&nbsp;&nbsp;&nbsp;&nbsp;автоматически созданное имя для "Файл5" |
| true |mergeFiles | Папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Файл1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Файл2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Вложенная_папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл5 | Целевая папка "Папка1" создается со следующей структурой: <br/><br/>Папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Содержимое файлов "Файл1", "Файл2", "Файл3", "Файл4" и "Файл5" объединяется в один файл с автоматически созданным именем. |
| false |preserveHierarchy | Папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Файл1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Файл2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Вложенная_папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл5 | Целевая папка "Папка1" создается со следующей структурой. <br/><br/>Папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Файл1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Файл2<br/><br/>Папка "Вложенная_папка1" с файлами "Файл3", "Файл4" и "Файл5" не будет включена в эту папку. |
| false |flattenHierarchy | Папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Файл1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Файл2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Вложенная_папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл5 | Целевая папка "Папка1" создается со следующей структурой. <br/><br/>Папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;автоматически созданное имя для "Файл1"<br/>&nbsp;&nbsp;&nbsp;&nbsp;автоматически созданное имя для "Файл2"<br/><br/>Папка "Вложенная_папка1" с файлами "Файл3", "Файл4" и "Файл5" не будет включена в эту папку. |
| false |mergeFiles | Папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Файл1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Файл2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Вложенная_папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл5 | Целевая папка "Папка1" создается со следующей структурой<br/><br/>Папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Содержимое файлов "Файл1" и "Файл2" объединяется в один файл с автоматически созданным именем. автоматически созданное имя для "Файл1"<br/><br/>Папка "Вложенная_папка1" с файлами "Файл3", "Файл4" и "Файл5" не будет включена в эту папку. |

## <a name="preserve-metadata-during-copy"></a>Сохранять метаданные во время копирования

При копировании файлов из Amazon S3/BLOB-объекта Azure или Azure Data Lake Storage 2-го поколения в Azure Data Lake Storage 2-го поколения или BLOB-объект Azure можно сохранить метаданные файла вместе с данными. Дополнительные сведения см. в статье [Сохранение метаданных](copy-activity-preserve-metadata.md#preserve-metadata).

## <a name="mapping-data-flow-properties"></a>Сопоставление свойств потока данных

При преобразовании данных в потоке сопоставления данных можно считывать и записывать файлы из хранилища BLOB-объектов Azure в формате JSON, Avro, в тексте с разделителями или Parquet. Дополнительные сведения см. в разделе Преобразование [источника](data-flow-source.md) и [Преобразование приемника](data-flow-sink.md) в функции потока данных сопоставления.

### <a name="source-transformation"></a>Преобразование источника

В преобразовании «источник» можно выполнять чтение из контейнера, папки или отдельного файла в хранилище BLOB-объектов Azure. Вкладка **Параметры источника** позволяет управлять чтением файлов. 

![Параметры источника](media/data-flow/sourceOptions1.png "Параметры источника")

**Путь с подстановочными знаками:** Использование шаблона с подстановочными знаками даст указание потоку ADF перебрать каждую соответствующую папку и файл в одном преобразовании источника. Это эффективный способ обработки нескольких файлов в одном потоке. Добавьте несколько шаблонов сопоставления с подстановочными знаками с символом +, который появляется при наведении указателя мыши на существующий шаблон шаблона.

В исходном контейнере выберите ряд файлов, соответствующих шаблону. В наборе данных можно указать только контейнер. Путь к шаблону должен также включать путь к папке из корневой папки.

Примеры подстановочных знаков:

* ```*``` представляет любой набор символов
* ```**``` представляет рекурсивную вложенность каталога
* ```?``` заменяет один символ
* ```[]``` соответствует одному из символов в квадратных скобках

* ```/data/sales/**/*.csv``` получает все CSV-файлы в/Дата/Салес
* ```/data/sales/20??/**``` получает все файлы в 20 века
* ```/data/sales/2004/*/12/[XY]1?.csv``` получает все CSV-файлы в 2004 в декабре, начиная с префикса X или Y и заканчивая двузначным числом

**Корневой путь к разделу:** Если в источнике файлов имеются секционированные папки с форматом ```key=value``` (например, Year = 2019), то верхний уровень этого дерева папок секционирования можно назначить имени столбца в потоке данных потока.

Во-первых, задайте подстановочный знак, чтобы включить все пути, которые являются секционированными папками, а также конечные файлы, которые вы хотите прочитать.

![Параметры исходного файла секции](media/data-flow/partfile2.png "Параметр файла секционирования")

Для определения верхнего уровня структуры папок используйте параметр путь к корневому каталогу секции. При просмотре содержимого данных с помощью предварительного просмотра данных вы увидите, что ADF добавит разрешенные секции, найденные на каждом уровне папок.

![Корневой путь к разделу](media/data-flow/partfile1.png "Предварительный просмотр корневого пути к разделу")

**Список файлов:** Это набор файлов. Создайте текстовый файл, содержащий список файлов относительных путей для обработки. Наведите указатель на этот текстовый файл.

**Столбец для хранения имени файла:** Сохраните имя исходного файла в столбце данных. Введите имя нового столбца, чтобы сохранить строку имени файла.

**После завершения:** Выберите не выполнять никаких действий с исходным файлом после выполнения потока данных, удалите исходный файл или переместите исходный файл. Пути для перемещения являются относительными.

Чтобы переместить исходные файлы в другое расположение после обработки, сначала выберите "Переместить" для операции с файлом. Затем задайте каталог "from". Если вы не используете подстановочные знаки для своего пути, параметр "от" будет иметь ту же папку, что и исходная папка.

Если у вас есть исходный путь с подстановочным знаком, синтаксис будет выглядеть следующим образом:

```/data/sales/20??/**/*.csv```

Можно указать "от" в качестве

```/data/sales```

И "to" как

```/backup/priorSales```

В этом случае все файлы, источником которых является/дата/Салес, перемещаются в/Баккуп/приорсалес.

> [!NOTE]
> Операции с файлами выполняются только при запуске потока данных из запуска конвейера (Отладка или выполнение конвейера), в котором используется действие «выполнение потока данных» в конвейере. Операции с файлами *не* выполняются в режиме отладки потока данных.

**Фильтровать по дате последнего изменения:** Вы можете отфильтровать обрабатываемые файлы, указав диапазон дат последнего изменения. Все значения даты и времени указаны в формате UTC. 

### <a name="sink-properties"></a>Свойства приемника

В преобразовании приемника можно выполнять запись в контейнер или папку в хранилище BLOB-объектов Azure. Вкладка **Параметры** позволяет управлять записью файлов.

![параметры приемника](media/data-flow/file-sink-settings.png "параметры приемника")

**Очистите папку:** Определяет, удаляется ли конечная папка перед записью данных.

**Параметр имени файла:** Определяет, как имена целевых файлов именуются в конечной папке. Параметры имени файла:
   * **По умолчанию**: разрешить Spark использовать имена файлов на основе параметров по умолчанию.
   * **Шаблон**: введите шаблон, который перечисляет выходные файлы для каждой секции. Например, **займы [n]. csv** будут создавать loans1. csv, loans2. csv и т. д.
   * **Для каждой секции**: введите одно имя файла для каждой секции.
   * **Как данные в столбце**: задайте для выходного файла значение столбца. Путь задается относительно контейнера набора данных, а не конечной папки.
   * **Вывод в один файл**: Объединение секционированных выходных файлов в один именованный файл. Путь задается относительно папки набора данных. Имейте в виду, что операция слияния с TE может завершиться сбоем в зависимости от размера узла. Этот параметр не рекомендуется для больших наборов данных.

**Цитата:** Определяет, следует ли заключать все значения в кавычки

## <a name="lookup-activity-properties"></a>Свойства действия поиска

Чтобы получить сведения о свойствах, проверьте [действие поиска](control-flow-lookup-activity.md).

## <a name="getmetadata-activity-properties"></a>Свойства действия с метаданными

Дополнительные сведения о свойствах см. в статье [действие с операциями](control-flow-get-metadata-activity.md) с помощью метаданных. 

## <a name="delete-activity-properties"></a>Свойства действия удаления

Чтобы получить сведения о свойствах, проверьте [действие Удалить](delete-activity.md) .

## <a name="legacy-models"></a>Устаревшие модели

>[!NOTE]
>Следующие модели по-прежнему поддерживаются "как есть" для обеспечения обратной совместимости. Рекомендуется использовать новую модель, упомянутую в разделах выше, и пользовательский интерфейс создания ADF переключился на создание новой модели.

### <a name="legacy-dataset-model"></a>Устаревшая модель набора данных

| Свойство | Description | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойство type для набора данных должно иметь значение **AzureBlob**. |Да |
| folderPath | Путь контейнеру и папке в хранилище BLOB-объектов. <br/><br/>Фильтр с подстановочными знаками поддерживается для пути без имени контейнера. Допустимые подстановочные знаки: `*` (соответствует нулю или большему количеству знаков) и `?` (соответствует нулю или одному знаку). Для экранирования используйте `^`, если фактическое имя папки содержит подстановочный знак или escape-символ. <br/><br/>Например: myblobcontainer/myblobfolder//. Дополнительные примере см. в разделе [Примеры фильтров папок и файлов](#folder-and-file-filter-examples). |Yes для действия Copy/Lookup, No для действия GetMetadata |
| fileName | **Имя или фильтр постановочных знаков** для файлов по указанному folderPath. Если этому свойству не присвоить значение, набор данных будет указывать на все большие двоичные объекты в папке. <br/><br/>Допустимые знаки подстановки для фильтра: `*` (соответствует нулю или нескольким символам) и `?` (соответствует нулю или одному символу).<br/>Пример 1. `"fileName": "*.csv"`<br/>Пример 2. `"fileName": "???20180427.txt"`<br/>Используйте `^` для экранирования символов, если фактическое имя файла содержит подстановочный знак или этот escape-символ.<br/><br/>Если параметр fileName не указан для выходного набора данных, а **preserveHierarchy** не указан в приемнике действия, действие копирования автоматически создает имя большого двоичного объекта в следующем формате: "*Data. [ Идентификатор запуска действия GUID]. [GUID if FlattenHierarchy]. [Format, если настроено]. [compression, если он настроен]* ", например" Data. 0a405f8a-93ff-4c6f-b3be-f69616f1df7a. txt. gz "; При копировании из табличного источника с использованием имени таблицы вместо запроса используется шаблон имени " *[имя таблицы]. [ format]. [compression, если настроено]* ", например" MyTable. csv ". |Нет |
| modifiedDatetimeStart | Фильтр файлов на основе атрибута: Последнее изменение. Файлы будут выбраны, если время их последнего изменения находится в диапазоне времени `modifiedDatetimeStart` и `modifiedDatetimeEnd`. Время представлено часовым поясом UTC в формате "2018-12-01T05:00:00Z". <br/><br/> Учитывайте общую производительность перемещения данных, включив этот параметр, если требуется использовать фильтр файлов из огромных объемов файлов. <br/><br/> Свойства могут иметь значение NULL, что означает, что фильтр атрибутов файла не будет применен к набору данных.  Если для параметра `modifiedDatetimeStart` задано значение даты и времени, но параметр `modifiedDatetimeEnd` имеет значение NULL, то будут выбраны файлы, чей атрибут последнего изменения больше указанного значения даты и времени или равен ему.  Если для параметра `modifiedDatetimeEnd` задано значение даты и времени, но параметр `modifiedDatetimeStart` имеет значение NULL, то будут выбраны все файлы, чей атрибут последнего изменения меньше указанного значения даты и времени.| Нет |
| modifiedDatetimeEnd | Фильтр файлов на основе атрибута: Последнее изменение. Файлы будут выбраны, если время их последнего изменения находится в диапазоне времени `modifiedDatetimeStart` и `modifiedDatetimeEnd`. Время представлено часовым поясом UTC в формате "2018-12-01T05:00:00Z". <br/><br/> Учитывайте общую производительность перемещения данных, включив этот параметр, если требуется использовать фильтр файлов из огромных объемов файлов. <br/><br/> Свойства могут иметь значение NULL, что означает, что фильтр атрибутов файла не будет применен к набору данных.  Если для параметра `modifiedDatetimeStart` задано значение даты и времени, но параметр `modifiedDatetimeEnd` имеет значение NULL, то будут выбраны файлы, чей атрибут последнего изменения больше указанного значения даты и времени или равен ему.  Если для параметра `modifiedDatetimeEnd` задано значение даты и времени, но параметр `modifiedDatetimeStart` имеет значение NULL, то будут выбраны все файлы, чей атрибут последнего изменения меньше указанного значения даты и времени.| Нет |
| format | Если требуется скопировать файлы между файловыми хранилищами "как есть" (двоичное копирование), можно пропустить раздел форматирования в определениях входного и выходного наборов данных.<br/><br/>Если нужно проанализировать или создать файлы определенного формата, поддерживаются следующие типы форматов файлов: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat** и **ParquetFormat**. Свойству **type** в разделе **format** необходимо присвоить одно из этих значений. Дополнительные сведения см. в разделах о [текстовом формате](supported-file-formats-and-compression-codecs-legacy.md#text-format), [формате JSON](supported-file-formats-and-compression-codecs-legacy.md#json-format), [формате Avro](supported-file-formats-and-compression-codecs-legacy.md#avro-format), [формате Orc](supported-file-formats-and-compression-codecs-legacy.md#orc-format) и [формате Parquet](supported-file-formats-and-compression-codecs-legacy.md#parquet-format). |Нет (только для сценария двоичного копирования) |
| compression | Укажите тип и уровень сжатия данных. Дополнительные сведения см. в разделе [Поддержка сжатия](supported-file-formats-and-compression-codecs-legacy.md#compression-support).<br/>Поддерживаемые типы: **GZip**, **Deflate**, **BZip2** и **ZipDeflate**.<br/>Поддерживаемые уровни: **Optimal** и **Fastest**. |Нет |

>[!TIP]
>Чтобы скопировать все большие двоичные объекты в папке, укажите только **folderPath**.<br>Чтобы скопировать один большой двоичный объект с заданным именем, укажите **folderPath** с частью папки и **fileName** с именем файла.<br>Чтобы скопировать подмножество больших двоичных объектов в папке, укажите **folderPath** с частью папки и **fileName**, используя фильтр подстановочных знаков. 

**Пример**.

```json
{
    "name": "AzureBlobDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
            "referenceName": "<Azure Blob storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

### <a name="legacy-copy-activity-source-model"></a>Исходная модель действия копирования прежних версий

| Свойство | Description | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойство type источника действия копирования должно иметь значение **BlobSource**. |Да |
| recursive | Указывает, следует ли читать данные рекурсивно из вложенных папок или только из указанной папки. Обратите внимание, что если для свойства recursive задано значение true, а приемником является файловое хранилище, пустые папки и вложенные папки не создаются в приемнике.<br/>Допустимые значения: **true** (по умолчанию) и **false**. | Нет |
| maxConcurrentConnections | Число подключений для одновременного подключения к хранилищу хранилища. Укажите, только если требуется ограничить одновременный подключение к хранилищу данных. | Нет |

**Пример**.

```json
"activities":[
    {
        "name": "CopyFromBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure Blob input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="legacy-copy-activity-sink-model"></a>Модель приемника действия копирования прежних версий

| Свойство | Description | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойство типа приемника действия копирования должно иметь значение **BlobSink**. |Да |
| copyBehavior | Определяет поведение копирования, когда источником являются файлы из файлового хранилища данных.<br/><br/>Допустимые значения:<br/><b>— PreserveHierarchy (по умолчанию)</b>. Сохраняет иерархию файлов в целевой папке. Относительный путь исходного файла в исходной папке идентичен относительному пути целевого файла в целевой папке.<br/><b>— FlattenHierarchy</b>. Все файлы из исходной папки размещаются на первом уровне в целевой папке. Целевые файлы имеют автоматически сформированные имена. <br/><b>— MergeFiles</b>. Объединяет все файлы из исходной папки в один файл. Если указано имя Blob-объекта или имя файла, то оно присваивается объединенному файлу. В противном случае присваивается автоматически созданное имя файла. | Нет |
| maxConcurrentConnections | Число подключений для одновременного подключения к хранилищу хранилища. Укажите, только если требуется ограничить одновременный подключение к хранилищу данных. | Нет |

**Пример**.

```json
"activities":[
    {
        "name": "CopyToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Blob output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "BlobSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

## <a name="next-steps"></a>Дальнейшие действия

В таблице [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md##supported-data-stores-and-formats) приведен список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования в фабрике данных.
