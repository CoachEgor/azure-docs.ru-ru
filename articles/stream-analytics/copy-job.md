---
title: Копирование или архивация Azure Stream Analytics заданий
description: В этой статье описывается копирование или резервное копирование задания Azure Stream Analytics.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/11/2019
ms.openlocfilehash: 258fc30f3e4a4ba374ada89c03b824ff5e27718e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "82612271"
---
# <a name="copy-or-back-up-azure-stream-analytics-jobs"></a>Копирование или архивация Azure Stream Analytics заданий

Вы можете копировать или архивировать развернутые задания Azure Stream Analytics с помощью Visual Studio Code или Visual Studio. При копировании задания в другой регион время последнего вывода не копируется. Поэтому при запуске скопированного задания нельзя использовать параметр [**при последней остановке**](https://docs.microsoft.com/azure/stream-analytics/start-job#start-options) .

## <a name="before-you-begin"></a>Перед началом
* Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись Azure](https://azure.microsoft.com/free/).

* Войдите на [портал Azure](https://portal.azure.com/).

* Установите [расширение Azure Stream Analytics для средств Visual Studio Code](quick-create-vs-code.md#install-the-azure-stream-analytics-tools-extension) или [Azure Stream Analytics для Visual Studio](quick-create-vs-code.md#install-the-azure-stream-analytics-tools-extension).  

## <a name="visual-studio-code"></a>Visual Studio Code

1. Щелкните значок **Azure** на панели действий Visual Studio Code, а затем разверните узел **Stream Analytics** . Задания должны отображаться в подписках.

   ![Открыть обозреватель Stream Analytics](./media/vscode-explore-jobs/open-explorer.png)

2. Чтобы экспортировать задание в локальный проект, в Visual Studio Code в **обозревателе Stream Analytics** выберите задание, которое необходимо экспортировать. Затем выберите папку для проекта.

    ![Экспорт задания ASA в Visual Studio Code](./media/vscode-explore-jobs/export-job.png)

    Проект будет экспортирован в выбранную папку и добавлен в текущую рабочую область.

    ![Экспорт задания ASA в Visual Studio Code](./media/stream-analytics-manage-job/copy-backup-stream-analytics-jobs.png)

3. Чтобы опубликовать задание в другом регионе или резервной копии с другим именем, выберите **выбрать из подписок для публикации** в редакторе запросов ( \* . asaql) и следуйте инструкциям.

    ![Публикация в Azure в Visual Studio Code](./media/quick-create-vs-code/submit-job.png)

## <a name="visual-studio"></a>Visual Studio

1. Следуйте [инструкциям по экспорту развернутого Azure Stream Analytics задания в инструкции проекта](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-vs-tools#export-jobs-to-a-project).

2. Откройте \* файл. asaql в редакторе запросов, выберите **отправить в Azure** в редакторе скриптов и следуйте инструкциям, чтобы опубликовать задание в другом регионе или резервной копии с новым именем.

## <a name="next-steps"></a>Дальнейшие действия

* [Краткое руководство. Создание Stream Analytics задания с помощью Visual Studio Code](quick-create-vs-code.md)
* [Краткое руководство. Создание задания Stream Analytics с помощью Visual Studio](stream-analytics-quick-create-vs.md)
* [Развертывание заданий Azure Stream Analytics с помощью CI/CD и Azure Pipelines](stream-analytics-tools-visual-studio-cicd-vsts.md)
