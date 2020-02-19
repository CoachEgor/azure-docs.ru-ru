---
title: Создание Cognitive Services ресурса с помощью Azure CLI
titleSuffix: Azure Cognitive Services
description: Приступая к работе с Azure Cognitive Services, создав и подпишитесь на ресурс с помощью интерфейса командной строки Azure.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 10/04/2019
ms.author: aahi
ms.openlocfilehash: 3e5fd2315a5d352ed5abee9253b5c84eea3b73a9
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2020
ms.locfileid: "77462281"
---
# <a name="create-a-cognitive-services-resource-using-the-azure-command-line-interfacecli"></a>Создание Cognitive Services ресурса с помощью интерфейс командной строки Azure (CLI)

Используйте это краткое руководство для начала работы с Azure Cognitive Services с помощью [интерфейса командной строки Azure (CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Cognitive Services представлены [ресурсами](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) Azure, созданными в подписке Azure. После создания ресурса используйте ключи и конечную точку, созданные для проверки подлинности приложений. 


В этом кратком руководстве вы узнаете, как зарегистрироваться в Azure Cognitive Services и создать учетную запись с одной или несколькими службами, используя [интерфейс командной строки Azure (CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Эти службы представлены [ресурсами](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal)Azure, которые позволяют подключиться к одному или нескольким API-интерфейсы Cognitive Services Azure.

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="prerequisites"></a>Предварительные требования

* Допустимая подписка Azure — [создайте ее](https://azure.microsoft.com/free/) бесплатно.
* [Интерфейс командной строки Azure (CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)

## <a name="install-the-azure-cli-and-sign-in"></a>Установка Azure CLI и вход в систему 

Установка [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Чтобы войти в локальную установку интерфейса командной строки, выполните команду [AZ login](https://docs.microsoft.com/cli/azure/reference-index#az-login) :

```console
az login
```

Для выполнения этих команд в браузере можно также использовать зеленую кнопку **попробовать** .
 
## <a name="create-a-new-azure-cognitive-services-resource-group"></a>Создание новой группы ресурсов Azure Cognitive Services

Перед созданием ресурса Cognitive Services необходимо иметь группу ресурсов Azure, в которой будет содержаться ресурс. При создании нового ресурса можно либо создать новую группу ресурсов, либо использовать существующую. В этой статье показано, как создать новую группу ресурсов.

### <a name="choose-your-resource-group-location"></a>Выберите расположение группы ресурсов

Чтобы создать ресурс, вам потребуется одно из расположений Azure, доступных для вашей подписки. Список доступных расположений можно получить с помощью команды [AZ Account List-Locations](/cli/azure/account#az-account-list-locations) . Большинство Cognitive Services доступны из нескольких расположений. Выберите ближайший вариант или посмотрите, какие расположения доступны для службы.

> [!IMPORTANT]
> * Запомните расположение Azure, так как оно понадобится вам при вызове Cognitive Services Azure.
> * Доступность некоторых Cognitive Services может варьироваться в зависимости от региона. Дополнительные сведения см. в статье [продукты Azure по регионам](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services).  

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

Создав расположение Azure, создайте новую группу ресурсов в Azure CLI с помощью команды [AZ Group Create](/cli/azure/group#az-group-create) .

В приведенном ниже примере замените расположение Azure `westus2` одним из расположений Azure, доступных для вашей подписки.

```azurecli-interactive
az group create \
    --name cognitive-services-resource-group \
    --location westus2
```

## <a name="create-a-cognitive-services-resource"></a>Создание ресурса Cognitive Services

### <a name="choose-a-cognitive-service-and-pricing-tier"></a>Выбор службы и ценовой категории

При создании нового ресурса необходимо знать тип службы, которую вы хотите использовать, а также [ценовую категорию](https://azure.microsoft.com/pricing/details/cognitive-services/) (или номер SKU). Эта и другие сведения будут использоваться в качестве параметров при создании ресурса.

### <a name="multi-service"></a>Несколько служб

| Service                    | Вид                      |
|----------------------------|---------------------------|
| Несколько служб. Дополнительные сведения см. на странице с [ценами](https://azure.microsoft.com/pricing/details/cognitive-services/) .            | `CognitiveServices`     |


> [!NOTE]
> Многие из Cognitive Services ниже имеют бесплатный уровень, который можно использовать для пробного использования службы. Чтобы использовать уровень "бесплатный", используйте `F0` в качестве номера SKU для ресурса.

### <a name="vision"></a>Зрение

| Service                    | Вид                      |
|----------------------------|---------------------------|
| API Компьютерного зрения            | `ComputerVision`          |
| Прогнозирование Пользовательское визуальное распознавание | `CustomVision.Prediction` |
| Пользовательское визуальное распознавание обучение   | `CustomVision.Training`   |
| Распознавание лиц                       | `Face`                    |
| Распознаватель документов            | `FormRecognizer`          |
| Распознаватель рукописного текста             | `InkRecognizer`           |

### <a name="search"></a>Поиск

| Service            | Вид                  |
|--------------------|-----------------------|
| API Автозаполнения Bing   | `Bing.Autosuggest.v7` |
| Пользовательский поиск Bing | `Bing.CustomSearch`   |
| API Поиска сущностей Bing | `Bing.EntitySearch`   |
| Поиск Bing        | `Bing.Search.v7`      |
| API Проверки орфографии Bing   | `Bing.SpellCheck.v7`  |

### <a name="speech"></a>Речь

| Service            | Вид                 |
|--------------------|----------------------|
| Службы "Речь"    | `SpeechServices`     |
| Распознавание речи | `SpeakerRecognition` |

### <a name="language"></a>Язык

| Service            | Вид                |
|--------------------|---------------------|
| Основные сведения о форме | `FormUnderstanding` |
| LUIS               | `LUIS`              |
| QnA Maker          | `QnAMaker`          |
| Анализ текста     | `TextAnalytics`     |
| Перевод текста   | `TextTranslation`   |

### <a name="decision"></a>Решение

| Service           | Вид               |
|-------------------|--------------------|
| Детектор аномалий  | `AnomalyDetector`  |
| Content Moderator | `ContentModerator` |
| Персонализатор      | `Personalizer`     |

Список доступных типов "виды" можно найти с помощью команды [AZ cognitiveservices Account List-видах](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-list-kinds) :

```azurecli-interactive
az cognitiveservices account list-kinds
```

### <a name="add-a-new-resource-to-your-resource-group"></a>Добавление нового ресурса в группу ресурсов

Чтобы создать новый ресурс Cognitive Services и подписываться на него, используйте команду [AZ cognitiveservices Account Create](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create) . Эта команда добавляет новый оплачиваемый ресурс в группу ресурсов, созданную ранее. При создании нового ресурса необходимо знать тип службы, которую вы хотите использовать, а также ценовую категорию (или SKU) и расположение Azure:

Вы можете создать ресурс F0 (Free) для детектора аномалий с именем `anomaly-detector-resource` с помощью следующей команды.

```azurecli-interactive
az cognitiveservices account create \
    --name anomaly-detector-resource \
    --resource-group cognitive-services-resource-group \
    --kind AnomalyDetector \
    --sku F0 \
    --location westus2 \
    --yes
```

## <a name="get-the-keys-for-your-resource"></a>Получение ключей для ресурса

Чтобы войти в локальную установку интерфейса командной строки (CLI), используйте команду [AZ login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) .

```console
az login
```

Используйте команду [AZ cognitiveservices Account Keys List](https://docs.microsoft.com/cli/azure/cognitiveservices/account/keys?view=azure-cli-latest#az-cognitiveservices-account-keys-list) , чтобы получить ключи для своего ресурса службы.

```azurecli-interactive
    az cognitiveservices account keys list \
    --name anomaly-detector-resource \
    --resource-group cognitive-services-resource-group
```

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="pricing-tiers-and-billing"></a>Ценовые категории и выставление счетов

Ценовые категории (и сумма, за которую взимается плата) основываются на количестве отправленных вами транзакций с использованием данных проверки подлинности. Каждая ценовая категория определяет:
* Максимальное число разрешенных транзакций в секунду (TPS).
* функции службы, включенные в ценовой категории.
* Стоимость предопределенного количества транзакций. При превышении этого объема взимается плата за дополнительную плату, указанную в [сведениях о ценах](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) для вашей службы.

## <a name="get-current-quota-usage-for-your-resource"></a>Получение сведений об используемой квоте для ресурса

Используйте команду [AZ cognitiveservices Account List-Usage](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-list-usage) , чтобы получить сведения об использовании для вашего ресурса службы "неприятный".

```azurecli-interactive
az cognitiveservices account list-usage \
    --name anomaly-detector-resource \
    --resource-group cognitive-services-resource-group \
    --subscription subscription-name
```

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы хотите очистить и удалить Cognitive Services ресурс, можно удалить его или группу ресурсов. При удалении группы ресурсов также удаляются все другие ресурсы, содержащиеся в этой группе.

Чтобы удалить группу ресурсов и связанные с ней ресурсы, используйте команду AZ Group DELETE.

```azurecli-interactive
az group delete --name cognitive-services-resource-group
```

## <a name="see-also"></a>См. также:

* [Проверка подлинности запросов к Azure Cognitive Services](authentication.md)
* [Что такое Azure Cognitive Services?](Welcome.md)
* [Поддержка естественного языка](language-support.md)
* [Поддержка контейнеров DOCKER](cognitive-services-container-support.md)
