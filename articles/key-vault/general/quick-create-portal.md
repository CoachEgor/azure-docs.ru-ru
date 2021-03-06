---
title: Краткое руководство. Создание Azure Key Vault с помощью портала Azure
description: Краткое руководство по созданию Azure Key Vault с помощью портала Azure.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: quickstart
ms.custom: mvc
ms.date: 12/08/2020
ms.author: mbaldwin
ms.openlocfilehash: b5f4f3d2b3eda9f00049cee26ae95850d65257d6
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/07/2020
ms.locfileid: "96778875"
---
# <a name="quickstart-create-a-key-vault-using-the-azure-portal"></a>Краткое руководство. Создание хранилища ключей с помощью портала Azure

Azure Key Vault — это облачная служба, которая предоставляет защищенное хранилище для [ключей](../keys/index.yml), [секретов](../secrets/index.yml) и [сертификатов](../certificates/index.yml). Дополнительные сведения о Key Vault см. в [этой статье](overview.md), а сведения о том, что можно хранить в хранилище ключей, см. [здесь](about-keys-secrets-certificates.md).

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Из этого краткого руководства вы узнаете, как создать хранилище ключей с помощью [портала Azure](https://portal.azure.com). 

## <a name="sign-in-to-azure"></a>Вход в Azure

Войдите на портал Azure по адресу https://portal.azure.com.

## <a name="create-a-vault"></a>Создание хранилища

1. На **домашней странице** или в меню портала Azure выберите команду **Создать ресурс**.
2. В поле поиска введите **Key Vault**.
3. В списке результатов выберите **Key Vault**.
4. В разделе Key Vault выберите **Создать**.
5. В разделе **Создать Key Vault** введите приведенные ниже сведения.
    - **Name** (Имя). Укажите уникальное имя. В этом кратком руководстве мы используем **Contoso-vault2**. 
    - **Подписка**: Выберите подписку.
    - В разделе **Группа ресурсов** выберите **Создать** и введите имя группы ресурсов.
    - В раскрывающемся меню **Расположение** выберите расположение.
    - Для других параметров оставьте значения по умолчанию.
6. Указав приведенные выше сведения, выберите **Создать**.

Запишите значения двух указанных ниже свойств.

* **Имя хранилища.** В нашем примере это **Contoso-Vault2**. Вы будете использовать это имя для выполнения других этапов.
* **URI хранилища**. В нашем примере это https://contoso-vault2.vault.azure.net/. Необходимо, чтобы приложения, использующие ваше хранилище через REST API, использовали этот URI.

На этом этапе операции в новом хранилище ключей может выполнять только учетная запись Azure.

![Выходные данные после создания Key Vault](../media/quick-create-portal/vault-properties.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

Другие руководства о Key Vault созданы на основе этого документа. Если вы планируете продолжить работу с последующими краткими руководствами и статьями, эти ресурсы можно не удалять.
Удалите ненужную группу ресурсов. Key Vault и связанные ресурсы будут также удалены. Чтобы удалить группу ресурсов на портале, сделайте следующее:

1. В поле поиска в верхней части портала введите имя группы ресурсов. Если в результатах поиска отображается группа ресурсов, используемая в этом кратком руководстве, выберите ее.
2. Выберите **Удалить группу ресурсов**.
3. В поле **Введите имя группы ресурсов:** введите имя группы ресурсов и выберите **Удалить**.


## <a name="next-steps"></a>Дальнейшие действия

С помощью этого краткого руководства вы создали хранилище ключей на портале Azure. Дополнительные сведения о Key Vault и его интеграции в приложения см. в следующих статьях.

- [Обзор Azure Key Vault](overview.md)
- [Руководство разработчика Azure Key Vault](developers-guide.md)
- [Рекомендации по Azure Key Vault](best-practices.md)
