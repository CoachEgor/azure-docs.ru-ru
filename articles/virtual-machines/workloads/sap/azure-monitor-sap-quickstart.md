---
title: Развертывание Azure Monitor для решений SAP с помощью портал Azure
description: Развертывание Azure Monitor для решений SAP с помощью портал Azure
author: sameeksha91
ms.author: sakhare
ms.topic: how-to
ms.service: virtual-machines
ms.subservice: workloads
ms.date: 08/17/2020
ms.reviewer: cynthn
ms.openlocfilehash: c3b3848e4e4f7b0445f882265dbe66bb10b48833
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2020
ms.locfileid: "94968593"
---
# <a name="deploy-azure-monitor-for-sap-solutions-with-azure-portal"></a>Развертывание Azure Monitor для решений SAP с помощью портал Azure

Azure Monitor для ресурсов решений SAP можно создать с помощью [портал Azure](https://azure.microsoft.com/features/azure-portal). Этот метод предоставляет пользовательский интерфейс на основе браузера для развертывания Azure Monitor для решений SAP и настройки поставщиков.

## <a name="sign-in-to-azure-portal"></a>Вход на портал Azure

Войдите на портал Azure по адресу https://portal.azure.com.

## <a name="create-monitoring-resource"></a>Создание ресурса мониторинга

1. Выберите **Azure Monitor для решений SAP** в **Azure Marketplace**.

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-1.png" alt-text="На рисунке показано, как выбрать Azure Monitor для решений SAP в Azure Marketplace." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-1.png":::

2. На вкладке **Основные сведения** укажите необходимые значения. Если применимо, можно использовать существующую рабочую область Log Analytics.

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-2.png" alt-text="Отображение параметров конфигурации портал Azure." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-2.png":::

3. При выборе виртуальной сети убедитесь, что системы, которые вы хотите отслеживать, доступны в пределах этой виртуальной сети. 

   > [!IMPORTANT]
   > Выбор **общей папки** для совместного использования данных корпорацией Майкрософт позволяет нашим группам поддержки предоставлять дополнительную поддержку.

## <a name="configure-providers"></a>Настройка поставщиков

### <a name="sap-hana-provider"></a>Поставщик SAP HANA 

1. Перейдите на вкладку **поставщик** , чтобы добавить поставщиков, которые нужно настроить. Вы можете добавить нескольких поставщиков один за другим или добавить их после развертывания ресурса мониторинга. 

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-3.png" alt-text="Отображение вкладки поставщик для добавления дополнительных поставщиков в Azure Monitor для решений SAP." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-3.png":::

2. Щелкните **Добавить поставщика** и выберите **SAP HANA** в раскрывающемся списке. 

   > [!IMPORTANT]
   > Убедитесь, что SAP HANA поставщик настроен для SAP HANA главного узла.

3. Введите частный IP-адрес для сервера HANA.

4. Введите имя клиента базы данных, который вы хотите использовать. Вы можете выбрать любой клиент, но мы рекомендуем использовать **системдб** , так как он обеспечивает широкий спектр областей наблюдения. 

5. Введите номер порта SQL, связанный с базой данных HANA. Номер порта должен быть в формате **[3]**  +  **[instance #]**  +  **[13]**. Например, 30013. 

6. Введите имя пользователя базы данных, которое вы хотите использовать. Убедитесь, что пользователь базы данных имеет назначенные роли **мониторинга** и **чтения каталога** . 

7. По завершении нажмите кнопку **Добавить поставщика**. Продолжайте добавлять дополнительные поставщики по мере необходимости или выберите **проверить и создать** , чтобы завершить развертывание.

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-4.png" alt-text="Изображение параметров конфигурации при добавлении сведений о поставщике." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-4.png":::

### <a name="high-availability-cluster-pacemaker-provider"></a>Поставщик кластера высокой доступности (Pacemaker)

1. В раскрывающемся списке выберите **кластер высокой доступности (Pacemaker)** . 

   > [!IMPORTANT]
   > Чтобы настроить поставщик кластера высокой доступности (Pacemaker), убедитесь, что ha_cluster_provider установлен на каждом узле. Дополнительные сведения см. в разделе средство [экспорта кластеров Ha](https://github.com/ClusterLabs/ha_cluster_exporter#installation)

2. Введите конечную точку Prometheus в формате http://IP:9664/metrics . 
 
3. Введите идентификатор системы (SID), имя узла и название кластера.

4. По завершении нажмите кнопку **Добавить поставщика**. Продолжайте добавлять дополнительные поставщики по мере необходимости или выберите **проверить и создать** , чтобы завершить развертывание.

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-5.png" alt-text="На рисунке показаны параметры, относящиеся к поставщику Pacemaker кластера высокой доступности." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-5.png":::


### <a name="microsoft-sql-server-provider"></a>Поставщик Microsoft SQL Server

1. Перед добавлением поставщика Microsoft SQL Server необходимо выполнить следующий скрипт в SQL Server Management Studio, чтобы создать пользователя с соответствующими разрешениями, необходимыми для настройки поставщика.

   ```sql
   USE [<Database to monitor>]
   DROP USER [AMS]
   GO
   USE [master]
   DROP USER [AMS]
   DROP LOGIN [AMS]
   GO
   CREATE LOGIN [AMS] WITH PASSWORD=N'<password>', DEFAULT_DATABASE=[<Database to monitor>], DEFAULT_LANGUAGE=[us_english], CHECK_EXPIRATION=OFF, CHECK_POLICY=OFF
   CREATE USER AMS FOR LOGIN AMS
   ALTER ROLE [db_datareader] ADD MEMBER [AMS]
   ALTER ROLE [db_denydatawriter] ADD MEMBER [AMS]
   GRANT CONNECT TO AMS
   GRANT VIEW SERVER STATE TO AMS
   GRANT VIEW SERVER STATE TO AMS
   GRANT VIEW ANY DEFINITION TO AMS
   GRANT EXEC ON xp_readerrorlog TO AMS
   GO
   USE [<Database to monitor>]
   CREATE USER [AMS] FOR LOGIN [AMS]
   ALTER ROLE [db_datareader] ADD MEMBER [AMS]
   ALTER ROLE [db_denydatawriter] ADD MEMBER [AMS]
   GO
   ``` 

2. Щелкните **Добавить поставщика** и выберите **Microsoft SQL Server** в раскрывающемся списке. 

3. Заполните поля, используя сведения, связанные с Microsoft SQL Server. 

4. По завершении нажмите кнопку **Добавить поставщика**. Продолжайте добавлять дополнительные поставщики по мере необходимости или выберите **проверить и создать** , чтобы завершить развертывание.

     :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-6.png" alt-text="На рисунке показаны сведения о добавлении поставщика Microsoft SQL Server." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-6.png":::

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о [Azure Monitor для решений SAP](azure-monitor-overview.md)
