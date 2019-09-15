---
title: Настройка Application Insights Azure для мониторинга моделей машинного обучения
titleSuffix: Azure Machine Learning
description: Мониторинг веб-служб, развернутых с помощью Машинное обучение Azure Azure Application Insights
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 07/12/2019
ms.custom: seoapril2019
ms.openlocfilehash: 785507e9ae12d8da564a223c8cdf544a98b8de61
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/15/2019
ms.locfileid: "71002869"
---
# <a name="monitor-your-azure-machine-learning-models-with-application-insights"></a>Мониторинг моделей машинного обучения в Azure с помощью Application Insights

Из этой статьи вы узнаете, как настроить Azure Application Insights для Машинное обучение Azure. Application Insights можно использовать для отслеживания следующих параметров:
* частоты запросов, времени отклика и частоты сбоев;
* частоты зависимостей, времени отклика и частоты сбоев;
* Исключения.

[Дополнительные сведения об Application Insights](../../azure-monitor/app/app-insights-overview.md). 


## <a name="prerequisites"></a>Предварительные требования

* Если у вас еще нет подписки Azure, создайте бесплатную учетную запись Azure, прежде чем начинать работу. Опробуйте [бесплатную или платную версию машинное обучение Azure](https://aka.ms/AMLFree) уже сегодня.

* Должны быть установлены рабочая область машинного обучения Azure, локальный каталог со скриптами и пакет SDK машинного обучения Azure для Python. В руководстве по [настройке среды разработки](how-to-configure-environment.md) описано, как получить эти обязательные компоненты.
* Обученная модель машинного обучения для развертывания в службе Azure Kubernetes (AKS) или в экземпляре контейнера Azure (ACI). Если у вас ее нет, см. руководство по [обучению модели классификации изображений](tutorial-train-models-with-aml.md).


## <a name="use-sdk-to-configure"></a>Использование пакета SDK для настройки 

### <a name="update-a-deployed-service"></a>Обновление развернутой службы
1. Найдите службу в рабочей области. Значение `ws` обозначает имя рабочей области.

    ```python
    from azureml.core.webservice import Webservice
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Обновите службу и включите Application Insights. 

    ```python
    aks_service.update(enable_app_insights=True)
    ```

### <a name="log-custom-traces-in-your-service"></a>Трассировка пользовательских журналов в службе
Если требуется вести журнал пользовательских трассировок, выполните инструкции из руководства по стандартному процессу развертывания для AKS или ACI, представленные в документе [Deploy models with the Azure Machine Learning service](how-to-deploy-and-where.md) (Развертывание моделей с помощью Службы машинного обучения Azure). Затем выполните следующие действия:

1. измените файл оценки, добавив выражения print;
    
    ```python
    print ("model initialized" + time.strftime("%H:%M:%S"))
    ```

2. Обновите конфигурацию службы.
    
    ```python
    config = Webservice.deploy_configuration(enable_app_insights=True)
    ```

3. Создайте образ и разверните его в [AKS](how-to-deploy-to-aks.md) или [ACI](how-to-deploy-to-aci.md).  

### <a name="disable-tracking-in-python"></a>Отключение наблюдения в Python

Чтобы отключить Application Insights, используйте следующий код:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```
    
## <a name="use-portal-to-configure"></a>Настройка с помощью портала

Вы можете включить или отключить Azure Application Insights на портале Azure.

1. Откройте рабочую область на [портале Azure](https://portal.azure.com).

1. На вкладке **Развертывания** выберите службу, для которой требуется включить Application Insights.

   [![Список служб на вкладке "Развертывания"](media/how-to-enable-app-insights/Deployments.PNG)](./media/how-to-enable-app-insights/Deployments.PNG#lightbox)

3. Выберите **Изменить**

   [![Кнопка "Изменить"](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

4. В разделе **Дополнительные параметры** установите флажок **Включить диагностику AppInsights**.

   [![Установленный флажок для включения диагностики](media/how-to-enable-app-insights/AdvancedSettings.png)](./media/how-to-enable-app-insights/AdvancedSettings.png#lightbox)

1. Чтобы применить изменение, в верхней части экрана выберите **Изменить**. 

### <a name="disable"></a>Отключение
1. Откройте рабочую область на [портале Azure](https://portal.azure.com).
1. Откройте **Развертывания**, затем выберите службу и щелкните **Изменить**.

   [![Нажмите кнопку "Изменить"](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox).

1. В разделе **Дополнительные параметры** снимите флажок **Включить диагностику AppInsights**. 

   [![Снятый флажок включения диагностики](media/how-to-enable-app-insights/uncheck.png)](./media/how-to-enable-app-insights/uncheck.png#lightbox)

1. Чтобы применить изменение, в верхней части экрана выберите **Изменить**. 
 

## <a name="evaluate-data"></a>Данные оценки
Данные службы хранятся в учетной записи Application Insights в той же группе ресурсов, что и Машинное обучение Azure.
Чтобы их просмотреть:
1. Откройте рабочее пространство Службы машинного обучения на [портале Azure](https://portal.azure.com) и щелкните ссылку Application Insights.

    [![AppInsightsLoc](media/how-to-enable-app-insights/AppInsightsLoc.png)](./media/how-to-enable-app-insights/AppInsightsLoc.png#lightbox)

1. Выберите вкладку **Обзор**, чтобы увидеть базовый набор метрик для своей службы.

   [![Обзор](media/how-to-enable-app-insights/overview.png)](./media/how-to-enable-app-insights/overview.png#lightbox)

3. Чтобы выполнить поиск в пользовательских трассировках, выберите **Аналитика**.
4. В разделе схемы выберите **Трассировки**. Затем выберите **Запуск**, чтобы выполнить запрос. Данные будут отображаться в формате таблицы и сопоставляться с пользовательскими вызовами в файле оценки. 

   [![Пользовательские трассировки](media/how-to-enable-app-insights/logs.png)](./media/how-to-enable-app-insights/logs.png#lightbox)

Дополнительные сведения об использовании службы Application Insights см. в статье [Что такое Azure Application Insights?](../../azure-monitor/app/app-insights-overview.md)


## <a name="example-notebook"></a>Пример записной книжки

В записной книжке [Enable-App-Insights-in-Production-Service. ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/enable-app-insights-in-production-service/enable-app-insights-in-production-service.ipynb) описываются концепции, описанные в этой статье. 
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Следующие шаги
Также данные можно собирать с моделей в рабочей среде. Ознакомьтесь со статьей [Сбор данных для моделей в рабочей среде](how-to-enable-data-collection.md). 

Также читайте [Azure Monitor для контейнеров](https://docs.microsoft.com/azure/monitoring/monitoring-container-insights-overview?toc=%2fazure%2fmonitoring%2ftoc.json).
