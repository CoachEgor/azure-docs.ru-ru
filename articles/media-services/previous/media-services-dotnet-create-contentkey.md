---
title: Создание ContentKey с использованием .NET
description: В этой статье показано, как создавать ключи содержимого, обеспечивающие безопасный доступ к ресурсам.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 225b05e5-7d30-409c-b5b7-3ef0634310c7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: aebd6dee9314d6e5641988767c024790b6b721f4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79251158"
---
# <a name="create-contentkeys-with-net"></a>Создание ContentKey с использованием .NET 
> [!div class="op_single_selector"]
> * [REST](media-services-rest-create-contentkey.md)
> * [.NET](media-services-dotnet-create-contentkey.md)
> 
> 

Службы мультимедиа позволяют создавать и доставлять зашифрованные ресурсы. **ContentKey** обеспечивает безопасный доступ к вашим **ресурсам-контейнерам**. 

При создании нового ресурса-контейнера (например, перед [передачей файлов](media-services-dotnet-upload-files.md)) можно указать следующие параметры шифрования: **StorageEncrypted**, **CommonEncryptionProtected** или **EnvelopeEncryptionProtected**. 

При доставке ресурсов-контейнеров в клиенты можно [настроить динамическое шифрование таких ресурсов](media-services-dotnet-configure-asset-delivery-policy.md), используя один из двух следующих типов шифрования: **DynamicEnvelopeEncryption** или **DynamicCommonEncryption**.

Зашифрованные ресурсы-контейнеры должны быть связаны с сущностями **ContentKey**. В этой статье описано, как создать ключ содержимого.

> [!NOTE]
> Когда ресурс-контейнер **StorageEncrypted** создается с помощью пакета SDK служб мультимедиа для .NET, ключ содержимого (**ContentKey**) создается и связывается с ресурсом-контейнером автоматически.
> 
> 

## <a name="contentkeytype"></a>ContentKeyType
Одно из значений, которые необходимо задать при создания ключа содержимого — тип ключа содержимого. Выберите одно из следующих значений. 

```csharp
    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }
```

## <a name="create-envelope-type-contentkey"></a><a id="envelope_contentkey"></a>Создание ContentKey конвертного типа
В следующем фрагменте кода создается ключ содержимого конвертного типа шифрования. Затем ключ связывается с указанным ресурсом.

```csharp
    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

call

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);
```


## <a name="create-common-type-contentkey"></a><a id="common_contentkey"></a>Создание ContentKey общего типа
В следующем фрагменте кода создается ключ содержимого общего типа шифрования. Затем ключ связывается с указанным ресурсом.

```csharp
    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.CommonEncryption);

        // Associate the key with the asset.
        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int length)
    {
        var returnValue = new byte[length];

        using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
        {
            rng.GetBytes(returnValue);
        }

        return returnValue;
    }
call

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 
```

## <a name="media-services-learning-paths"></a>Схемы обучения работе со службами мультимедиа
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Предоставление отзыва
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

