---
title: Получение подробных сведений о базе данных Azure Blockchain Workbench
description: Узнайте, как получить сведения о базе данных и сервере базы данных Azure Блокчейн Workbench Preview.
ms.date: 09/05/2019
ms.topic: article
ms.reviewer: mmercuri
ms.openlocfilehash: 2b3190a9d042be8ead1ff3d5ef48d4a2a19e8963
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "74324693"
---
# <a name="get-information-about-your-azure-blockchain-workbench-database"></a>Получение сведений о базе данных Azure Blockchain Workbench

В этой статье показано, как получить подробные сведения о базе данных Azure Блокчейн Workbench Preview.

## <a name="overview"></a>Обзор

Сведения о приложениях, рабочих процессах и выполнении смарт-контракта предоставляются с помощью представлений базы данных в базе данных SQL Workbench Blockchain. Разработчики могут использовать эти сведения, когда применяют такие средства, как Microsoft Excel, Power BI, Visual Studio и SQL Server Management Studio.

Прежде чем разработчик может подключиться к базе данных, ему необходимо следующее.

* Доступ к внешнему клиенту, разрешенный в брандмауэре базы данных. В этой статье о настройке брандмауэра базы данных объясняется, как разрешить доступ.
* Имя сервера базы данных и имя базы данных.

## <a name="connect-to-the-blockchain-workbench-database"></a>Подключение к базе данных Blockchain Workbench

Для подключения к базе данных:

1. Войдите в портал Azure с учетной записью, имеющей разрешения **владельца** для ресурсов Azure блокчейн Workbench.
2. В левой области навигации выберите **Группа ресурсов**.
3. Выберите имя группы ресурсов для развертывания Blockchain Workbench.
4. Выберите **Тип** для сортировки списка ресурсов, а затем выберите **SQL Server**. В отсортированном списке на следующем снимке экрана показаны две базы данных SQL, master и использующая lhgn в качестве **префикса ресурса**.

   ![Отсортированный список ресурсов Blockchain Workbench](./media/getdb-details/sorted-workbench-resource-list.png)

5. Чтобы просмотреть подробные сведения о базе данных Blockchain Workbench, перейдите по ссылке базы данных с **префиксом ресурса**, заданным для развертывания Blockchain Workbench.

   ![Сведения о базе данных](./media/getdb-details/workbench-db-details.png)

Имя сервера базы данных и имя базы данных позволяют подключиться к базе данных Blockchain Workbench с помощью средства разработки или отчетности.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Database views in Azure Blockchain Workbench](database-views.md) (Представления базы данных в Azure Blockchain Workbench)