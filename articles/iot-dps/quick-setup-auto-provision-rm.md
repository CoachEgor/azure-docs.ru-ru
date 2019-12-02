---
title: Настройка Службы подготовки устройств к добавлению в Центр Интернета вещей с помощью шаблона Azure Resource Manager
description: Краткое руководство Azure. Настройка Службы подготовки устройств к добавлению в Центр Интернета вещей с помощью шаблона
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: b40e126ca23190fbe50a717016b18719be6950e2
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/21/2019
ms.locfileid: "74276382"
---
# <a name="quickstart-set-up-the-iot-hub-device-provisioning-service-with-an-azure-resource-manager-template"></a>Краткое руководство. Настройка службы "Подготовка устройств к добавлению в Центр Интернета вещей" c помощью шаблона Azure Resource Manager

[Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) можно использовать, чтобы программно настраивать облачные ресурсы Azure, необходимые для подготовки устройств. В этих шагах показано, как создать центр Интернета вещей, Службу подготовки устройств к добавлению в Центр Интернета вещей и связать обе службы с помощью шаблона Azure Resource Manager. В этом кратком руководстве для выполнения программных шагов, необходимых для создания группы ресурсов и развертывания шаблона, используется [Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli). Однако для развертывания шаблона также можно использовать [портал Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal), [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy), .NET, Ruby или другие языки программирования. 


## <a name="prerequisites"></a>Предварительные требования

- Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.
- Для этого краткого руководства требуется запустить Azure CLI локально. Требуется Azure CLI версии 2.0 или более поздней. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо установить или обновить интерфейс командной строки, см. статью [Установка Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).


## <a name="sign-in-to-azure-and-create-a-resource-group"></a>Вход в Azure и создание группы ресурсов

Войдите в учетную запись Azure и выберите подписку.

1. В командной строке запустите [команду login][lnk-login-command]:
    
    ```azurecli
    az login
    ```

    Следуйте инструкциям, чтобы выполнить аутентификацию с использованием кода и войти в учетную запись Azure через веб-браузер.

2. Если у вас есть несколько подписок Azure, то при выполнении входа в Azure вы получаете доступ ко всем учетным записям Azure, связанным с вашими учетными данными. Используйте следующую [команду для вывода учетных записей Azure][lnk-az-account-command], доступных для использования:
    
    ```azurecli
    az account list 
    ```

    Используйте следующую команду, чтобы выбрать подписку, которая будет использоваться для выполнения команд для создания Центра Интернета вещей. Вы можете использовать имя подписки или идентификатор из выходных данных предыдущей команды:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

3. Такие облачные ресурсы Azure, как Центры Интернета вещей и службы подготовки, создаются в группе ресурсов. Используйте имеющуюся группу ресурсов либо выполните следующую [команду для создания группы ресурсов][lnk-az-resource-command]:
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > В предыдущем примере создается группа ресурсов в расположении западной части США. Список доступных расположений можно просмотреть, выполнив команду `az account list-locations -o table`.
    >
    >

## <a name="create-a-resource-manager-template"></a>Создание шаблона Resource Manager

Используйте шаблон JSON для создания службы подготовки и связанного Центра Интернета вещей в группе ресурсов. Вы также можете использовать шаблон Azure Resource Manager для изменения имеющейся службы подготовки или Центра Интернета вещей.

1. Используйте текстовый редактор, чтобы создать шаблон Azure Resource Manager с именем **template.json** со следующим основным содержимым. 

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {},
       "variables": {},
       "resources": []
   }
   ```

2. Замените раздел **parameters** следующим содержимым. Раздел parameters определяет параметры, значения которых можно передать из другого файла. В этом разделе определяется имя центра Интернета вещей и службы подготовки, которые будут созданы, а также их расположение. Значения будут ограничены регионами Azure, поддерживающими центры Интернета вещей и службы подготовки. Чтобы получить список поддерживаемых расположений для службы подготовки устройств, можно выполнить следующую команду: `az provider show --namespace Microsoft.Devices --query "resourceTypes[?resourceType=='ProvisioningServices'].locations | [0]" --out table` или перейти на страницу [состояния Azure](https://azure.microsoft.com/status/) и выполнить поиск по фразе "Служба подготовки устройств".

   ```json
    "parameters": {
        "iotHubName": {
            "type": "string"
        },
        "provisioningServiceName": {
            "type": "string"
        },
        "hubLocation": {
            "type": "string",
            "allowedValues": [
                "eastus",
                "westus",
                "westeurope",
                "northeurope",
                "southeastasia",
                "eastasia"
            ]
        }
    },

   ```

3. Замените раздел **variables** следующим содержимым. В этом разделе определяются значения, используемые позже для создания строки подключения Центра Интернета вещей, необходимой для связи службы подготовки и Центра Интернета вещей. 
 
   ```json
    "variables": {        
        "iotHubResourceId": "[resourceId('Microsoft.Devices/Iothubs', parameters('iotHubName'))]",
        "iotHubKeyName": "iothubowner",
        "iotHubKeyResource": "[resourceId('Microsoft.Devices/Iothubs/Iothubkeys', parameters('iotHubName'), variables('iotHubKeyName'))]"
    },

   ```

4. Чтобы создать Центр Интернета вещей, добавьте следующие строки в коллекцию **resources**. JSON определяет минимальные свойства, необходимые для создания центра Интернета вещей. Значения **name** и **location** будут передаваться в качестве параметров из другого файла. Дополнительные сведения о свойствах, которые можно указать для центра Интернета вещей в шаблоне, см. в статье [Microsoft.Devices IotHubs template reference](https://docs.microsoft.com/azure/templates/microsoft.devices/iothubs) (Справочник по шаблону Microsoft.Devices IotHubs).

   ```json
        {
            "apiVersion": "2017-07-01",
            "type": "Microsoft.Devices/IotHubs",
            "name": "[parameters('iotHubName')]",
            "location": "[parameters('hubLocation')]",
            "sku": {
                "name": "S1",
                "capacity": 1
            },
            "tags": {
            },
            "properties": {
            }            
        },

   ``` 

5. Чтобы создать службу подготовки, добавьте следующие строки после указания Центра Интернета вещей в коллекции **resources**. Свойства службы подготовки **name** и **location** будут передаваться в качестве параметров. В коллекции **iotHubs** определяются центры Интернета вещей, которые будут связаны со службой подготовки. Как минимум необходимо указать свойства **connectionString** и **location** для всех связанных Центров Интернета вещей. Кроме того, можно задать свойства **allocWeight** и **applyAllocationPolicy** в каждом Центре Интернета вещей, а также свойства **allocPolicy** и **authorizationPolicies** в самой службе подготовки. Дополнительные сведения см. в статье [Microsoft.Devices/IotHubs template reference](https://docs.microsoft.com/azure/templates/microsoft.devices/provisioningservices) (Справка по шаблону Microsoft.Devices/provisioningServices).

   Свойство **dependsOn** используется, чтобы убедиться, что диспетчер ресурсов создает Центр Интернета вещей до службы подготовки. Для шаблона требуется указать строку подключения к Центру Интернета вещей, чтобы задать его связь со службой подготовки устройств, а также чтобы сначала создавались центр и его ключи. Шаблон использует такие функции, как **concat** и **listKeys**, чтобы создать строку подключения из параметризованных переменных. Дополнительные сведения см. в статье [Функции шаблона Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions).

   ```json
        {
            "type": "Microsoft.Devices/provisioningServices",
            "sku": {
                "name": "S1",
                "capacity": 1
            },
            "name": "[parameters('provisioningServiceName')]",
            "apiVersion": "2017-11-15",
            "location": "[parameters('hubLocation')]",
            "tags": {},
            "properties": {
                "iotHubs": [
                    {
                        "connectionString": "[concat('HostName=', reference(variables('iotHubResourceId')).hostName, ';SharedAccessKeyName=', variables('iotHubKeyName'), ';SharedAccessKey=', listkeys(variables('iotHubKeyResource'), '2017-07-01').primaryKey)]",
                        "location": "[parameters('hubLocation')]",
                        "name": "[concat(parameters('iotHubName'),'.azure-devices.net')]"
                    }
                ]
            },
            "dependsOn": ["[parameters('iotHubName')]"]
        }

   ```

6. Сохраните файл шаблона. По завершении шаблон должен выглядеть следующим образом:

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
           "iotHubName": {
               "type": "string"
           },
           "provisioningServiceName": {
               "type": "string"
           },
           "hubLocation": {
               "type": "string",
               "allowedValues": [
                   "eastus",
                   "westus",
                   "westeurope",
                   "northeurope",
                   "southeastasia",
                   "eastasia"
               ]
           }
       },
       "variables": {        
           "iotHubResourceId": "[resourceId('Microsoft.Devices/Iothubs', parameters('iotHubName'))]",
           "iotHubKeyName": "iothubowner",
           "iotHubKeyResource": "[resourceId('Microsoft.Devices/Iothubs/Iothubkeys', parameters('iotHubName'), variables('iotHubKeyName'))]"
       },
       "resources": [
           {
               "apiVersion": "2017-07-01",
               "type": "Microsoft.Devices/IotHubs",
               "name": "[parameters('iotHubName')]",
               "location": "[parameters('hubLocation')]",
               "sku": {
                   "name": "S1",
                   "capacity": 1
               },
               "tags": {
               },
               "properties": {
               }            
           },
           {
               "type": "Microsoft.Devices/provisioningServices",
               "sku": {
                   "name": "S1",
                   "capacity": 1
               },
               "name": "[parameters('provisioningServiceName')]",
               "apiVersion": "2017-11-15",
               "location": "[parameters('hubLocation')]",
               "tags": {},
               "properties": {
                   "iotHubs": [
                       {
                           "connectionString": "[concat('HostName=', reference(variables('iotHubResourceId')).hostName, ';SharedAccessKeyName=', variables('iotHubKeyName'), ';SharedAccessKey=', listkeys(variables('iotHubKeyResource'), '2017-07-01').primaryKey)]",
                           "location": "[parameters('hubLocation')]",
                           "name": "[concat(parameters('iotHubName'),'.azure-devices.net')]"
                       }
                   ]
               },
               "dependsOn": ["[parameters('iotHubName')]"]
           }
       ]
   }
   ```

## <a name="create-a-resource-manager-parameter-file"></a>Создание файла параметров Resource Manager

Шаблон, определенный на предыдущем шаге, использует параметры, чтобы указать имя центра Интернета вещей, имя службы подготовки и расположение (регион Azure) для их создания. Эти параметры передаются в шаблон из отдельного файла. Это позволяет повторно использовать один и тот же шаблон для нескольких развертываний. Чтобы создать файл параметров, сделайте следующее:

1. Используйте текстовый редактор, чтобы создать файл параметров Azure Resource Manager с именем **parameters.json** со следующим основным содержимым: 

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {}
       }
   }
   ```

2. Добавьте значение **iotHubName** в раздел параметра.  Имя центра Интернета вещей должно быть глобально уникальным в Azure. Для этого можно добавить уникальный префикс или суффикс к имени примера или же вообще выбрать новое имя. Убедитесь, что это имя соответствует соглашению об именовании: оно должно быть длиной от 3 до 50 символов и может содержать только буквы верхнего или нижнего регистра, цифры или дефисы (-). 

   ```json
    "parameters": {
        "iotHubName": {
            "value": "my-sample-iot-hub"
        },
    }
   
   ```

3. Добавьте значение **provisioningServiceName** в раздел параметра. Кроме того, нужно будет выбрать глобально уникальное имя для службы подготовки. Убедитесь, что это имя соответствует соглашению об именовании для Службы подготовки устройств к добавлению в Центр Интернета вещей: оно должно быть длиной от 3 до 64 символов и может содержать только буквы верхнего или нижнего регистра, цифры или дефисы.

   ```json
    "parameters": {
        "iotHubName": {
            "value": "my-sample-iot-hub"
        },
        "provisioningServiceName": {
            "value": "my-sample-provisioning-service"
        },
    }

   ```

4. Добавьте значение **hubLocation** в раздел параметра. Это значение указывает расположения Центра Интернета вещей и службы подготовки. Значение должно соответствовать одному из расположений, указанных в коллекции **allowedValues** в определении параметра в файле шаблона. Эта коллекция ограничивает значения расположениями Azure, поддерживающими Центры Интернета вещей и службы подготовки. Чтобы получить список поддерживаемых расположений для службы подготовки устройств, можно выполнить команду `az provider show --namespace Microsoft.Devices --query "resourceTypes[?resourceType=='ProvisioningServices'].locations | [0]" --out table` или перейти на страницу [состояния Azure](https://azure.microsoft.com/status/) и выполнить поиск по фразе "Служба подготовки устройств".

   ```json
    "parameters": {
        "iotHubName": {
            "value": "my-sample-iot-hub"
        },
        "provisioningServiceName": {
            "value": "my-sample-provisioning-service"
        },
        "hubLocation": {
            "value": "westus"
        }
    }

   ```

5. Сохраните файл. 


> [!IMPORTANT]
> И Центр Интернета вещей, и служба подготовки будут общедоступны в качестве конечных точек DNS, поэтому не используйте конфиденциальную информацию, когда присваиваете им имена.
>

## <a name="deploy-the-template"></a>Развертывание шаблона

Используйте следующие команды Azure CLI для развертывания шаблонов и подтверждения развертывания.

1. Чтобы развернуть шаблон, перейдите в папку, содержащую шаблон и файлы параметров. Затем выполните приведенную ниже [команду, чтобы начать развертывание](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create):
    
    ```azurecli
     az group deployment create -g {your resource group name} --template-file template.json --parameters @parameters.json
    ```

   Выполнение этой операции займет несколько минут. После этого найдите свойство **provisioningState**, успешно завершившееся в выходных данных. 

   ![Выходные данные подготовки](./media/quick-setup-auto-provision-rm/output.png) 


2. Чтобы проверить развертывание, выполните следующую [команду для перечисления ресурсов](https://docs.microsoft.com/cli/azure/resource?view=azure-cli-latest#az-resource-list), после чего найдите новую службу подготовки и Центр Интернета вещей в выходных данных:

    ```azurecli
     az resource list -g {your resource group name}
    ```


## <a name="clean-up-resources"></a>Очистка ресурсов

Другие краткие руководства в этой коллекции созданы на основе этого документа. Если вы планируете продолжать работу с этими руководствами по быстрому запуску или обычными руководствами, не удаляйте созданные ресурсы. Если вы планируете продолжить, можно использовать Azure CLI для [удаления отдельного ресурса][lnk-az-resource-command], например центра Интернета вещей или службы подготовки, или группы ресурсов со всеми ее ресурсами.

Чтобы удалить службу подготовки, выполните следующую команду:

```azurecli
az iot hub delete --name {your provisioning service name} --resource-group {your resource group name}
```
Чтобы удалить Центр Интернета вещей, выполните следующую команду:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

Чтобы удалить группу ресурсов и все ее ресурсы, выполните следующую команду:

```azurecli
az group delete --name {your resource group name}
```

Группы ресурсов и отдельные ресурсы также можно удалять с помощью портала Azure, PowerShell, REST API и поддерживаемых пакетов SDK платформ, опубликованных для Azure Resource Manager или Службы подготовки устройств к добавлению в Центр Интернета вещей.

## <a name="next-steps"></a>Дополнительная информация

Вы развернули центр Интернета вещей и экземпляр службы подготовки устройств, а затем связали эти два ресурса. Чтобы узнать, как подготовить виртуальное устройство, см. краткое руководство по созданию имитированного устройства.

> [!div class="nextstepaction"]
> [Краткое руководство. Подготовка имитированного устройства TPM с помощью пакета SDK Интернета вещей Azure для C](./quick-create-simulated-device.md)


<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-devguide]: iot-hub-devguide.md
[lnk-portal]: iot-hub-create-through-portal.md 
