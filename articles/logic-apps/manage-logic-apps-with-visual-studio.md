---
title: Управление приложениями логики в Visual Studio — Azure Logic Apps
description: Узнайте, как управлять приложениями логики и другими ресурсами Azure с помощью Visual Studio с помощью Visual Studio Cloud Explorer.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.custom: mvc
ms.date: 05/07/2019
ms.openlocfilehash: 6a36770ff623e35c56a972a482e24ebb24849051
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2019
ms.locfileid: "65467431"
---
# <a name="manage-logic-apps-with-visual-studio"></a>Управление приложениями логики в Visual Studio

Несмотря на то, что можно создать, изменить, управление и развертывание приложений логики в [портала Azure](https://portal.azure.com), можно также использовать Visual Studio, если вы хотите добавить приложений логики, чтобы система управления версиями, опубликовать разные версии и создать [Azure Диспетчер ресурсов](../azure-resource-manager/resource-group-overview.md) шаблоны для различных сред развертывания. С помощью Visual Studio Cloud Explorer вы можете найти приложения логики и другие ресурсы Azure и управлять ими. Например, вы можете открывать, загружать, редактировать, запускать, просматривать журнал запусков, а также отключать и включать приложения логики, которые уже развернуты на портале Azure. Если вы ранее не работали с Azure Logic Apps в Visual Studio, [узнайте о создании приложений логики с помощью Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

> [!IMPORTANT]
> Развертывание или публикация приложения логики из Visual Studio перезаписывает версию этого приложения на портале Azure. Поэтому, если вы вносите изменения на портале Azure, которые нужно сохранить, [обновите приложение логики в Visual Studio](#refresh) с портала Azure перед следующим развертыванием или публикацией из Visual Studio.

<a name="requirements"></a>

## <a name="prerequisites"></a>Технические условия

* Подписка Azure. Если у вас еще нет подписки Azure, [зарегистрируйтесь для получения бесплатной учетной записи Azure](https://azure.microsoft.com/free/).

* Скачайте и установите эти средства, если вы еще этого не сделали: 

  * [Visual Studio 2019, 2017 или 2015 — выпуски Community или выше](https://aka.ms/download-visual-studio). 
  В этом кратком руководстве используется бесплатная версия Visual Studio Community 2017.

    > [!IMPORTANT]
    > При установке Visual Studio 2019 или 2017 обязательно выберите рабочую нагрузку **разработки Azure**. Если используется Visual Studio 2019, Cloud Explorer может открыть конструктор приложений логики на портале Azure. Открытие встроенного конструктора приложений логики пока не поддерживается. Дополнительные сведения см. в разделе [управления ресурсами, связанными с учетными записями Azure в Visual Studio Cloud Explorer](https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-resources-managing-with-cloud-explorer?view).

    Чтобы установить Cloud Explorer для Visual Studio 2015, [скачать Cloud Explorer в Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015). 
    Дополнительные сведения см. в разделе [управления ресурсами, связанными с учетными записями Azure в Visual Studio Cloud Explorer (2015)](https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-resources-managing-with-cloud-explorer?view=vs-2015).

  * [Пакет SDK для Azure (2.9.1 или более поздней версии)](https://azure.microsoft.com/downloads/) 

  * [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)

  * Средства Azure Logic Apps для необходимой версии Visual Studio:

    * [Visual Studio 2019](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2019)

    * [Visual Studio 2017](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2017)

    * [Visual Studio 2015](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2015)

    Вы можете скачать и установить средства Azure Logic Apps напрямую из Visual Studio Marketplace или узнать, [как установить это расширение из Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions). 
    После завершения установки перезагрузите Visual Studio.

* Доступ к Интернету при использовании встроенного конструктора Logic Apps

  Конструктору требуется подключение к Интернету, чтобы создать ресурсы в Azure и считать свойства и данные из соединителей в приложении логики. 
  Например, если вы используете соединитель Dynamics CRM Online, конструктор проверяет экземпляр CRM и получает информацию для отображения доступных свойств по умолчанию и пользовательских свойств.

<a name="find-logic-apps-vs"></a>

## <a name="find-your-logic-apps"></a>Поиск приложений логики

В Visual Studio с помощью Cloud Explorer можно найти все приложения логики, которые связаны с подпиской Azure и развернуты на портале Azure.

1. Откройте Visual Studio. В меню **Представление** выберите **Cloud Explorer**.

1. В Cloud Explorer выберите **Управление учетными записями**. Выберите подписку Azure, связанную с приложениями логики, а затем нажмите кнопку **Применить**. Например:

   ![Выбор параметра "Управление учетными записями"](./media/manage-logic-apps-with-visual-studio/account-management-select-Azure-subscription.png)

1. В зависимости от выполняемого поиска (по **группам ресурсов** или **их типам**), выполните действия ниже.

   * **Группы ресурсов**: Cloud Explorer отображает все группы ресурсов, связанные с выбранной подпиской Azure. 
   Разверните группу ресурсов, которая содержит ваше приложение логики, а затем выберите его.

   * **Типы ресурсов**: после выбора своей подписки Azure разверните **Приложения логики**. Когда Cloud Explorer отобразит все развернутые приложения логики, связанные с подпиской, выберите свое приложение логики.

<a name="open-designer"></a>

## <a name="open-in-visual-studio"></a>Открытие в Visual Studio

Visual Studio позволяет открыть приложения логики, которые были созданы и развернуты напрямую через портал Azure или в виде проектов Azure Resource Manager с помощью Visual Studio.

1. Откройте Cloud Explorer и найдите свое приложение логики. 

1. В контекстном меню приложения логики выберите **Открыть в редакторе приложений логики**.

   В этом примере показаны приложения логики по типу ресурсов, поэтому они будут отображаться в разделе **Приложения логики**.

   ![Открытие развернутого приложения логики с портала Azure](./media/manage-logic-apps-with-visual-studio/open-logic-app-in-editor.png)

   Когда приложение логики откроется в конструкторе Logic Apps, в нижней части конструктора можно выбрать **Представление кода**, чтобы просмотреть базовую структуру определения приложения логики. 
   Если вы хотите создать шаблон развертывания для приложения логики, узнайте о [загрузке шаблона Azure Resource Manager](#download-logic-app) для этого приложения. Узнайте больше о [шаблонах Resource Manager](../azure-resource-manager/resource-group-overview.md#template-deployment).

<a name="download-logic-app"></a>

## <a name="download-from-azure"></a>Скачивание из Azure

Вы можете загрузить приложения логики с [портала Azure](https://portal.azure.com) и сохранить их в виде шаблонов [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). Затем вы можете локально изменить шаблоны в Visual Studio и настроить приложения логики для разных сред развертывания. При загрузке приложений логики автоматически *параметризуются* их определения внутри [шаблонов Resource Manager](../azure-resource-manager/resource-group-overview.md#template-deployment), которые также используют формат JSON.

1. В Visual Studio откройте Cloud Explorer, затем найдите и выберите приложение логики, которое нужно загрузить с портала Azure.

   > [!NOTE]
   > Для Visual Studio 2019 г. Cloud Explorer можно открыть конструктор приложений логики на портале Azure, но еще не удается открыть встроенного конструктора приложений логики для загрузки приложения логики.

2. В контекстном меню этого приложения логики выберите **Открыть в редакторе приложений логики**.

   Откроется конструктор приложений логики, отображая соответствующее приложение. 
   Чтобы просмотреть базовое определение и структуру приложения логики, в нижней части конструктора выберите **Представление кода**. 

3. На панели инструментов конструктора щелкните **Загрузить**.

   ![Выбор команды "Загрузить"](./media/manage-logic-apps-with-visual-studio/download-logic-app.png)

4. Когда появится запрос указать расположение, перейдите к нему и сохраните шаблон Resource Manager для определения этого приложения логики в виде JSON-файла. 

Определение вашего приложения логики появится в подразделе `resources` внутри шаблона Resource Manager. Теперь вы можете изменить определение приложения логики и шаблон Resource Manager с помощью Visual Studio. Вы также можете добавить шаблон как проект Azure Resource Manager в решение Visual Studio. Ознакомьтесь с [проектами Resource Manager для приложений логики в Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md). 

<a name="refresh"></a>

## <a name="refresh-from-azure"></a>Обновление из Azure

Если вы измените приложение логики на портале Azure, чтобы сохранить эти изменения, необходимо обновить версию этого приложения с изменениями в Visual Studio. 

* В Visual Studio на панели инструментов конструктора приложений логики выберите **Обновить**.

  -или-

* В Cloud Explorer Visual Studio откройте контекстное меню приложения логики и выберите **Обновить**.

![Обновление приложения логики с изменениями](./media/manage-logic-apps-with-visual-studio/refresh-logic-app.png)

## <a name="publish-logic-app-updates"></a>Публикация обновлений приложения логики

Когда вы будете готовы развернуть обновления приложения логики из Visual Studio в Azure, на панели инструментов конструктора приложений логики выберите **Опубликовать**.

![Публикация обновленного приложения логики](./media/manage-logic-apps-with-visual-studio/publish-logic-app.png)

## <a name="manually-run-your-logic-app"></a>Запуск приложения логики вручную

В Visual Studio вы можете вручную запустить приложение логики, развернутое в Azure. На панели инструментов конструктора приложений логики выберите **Запустить триггер**.

![Запуск приложения логики вручную](./media/manage-logic-apps-with-visual-studio/manually-run-logic-app.png)

## <a name="review-run-history"></a>Просмотр журнала выполнения

Чтобы проверить состояние приложения логики и диагностировать проблемы, связанные с запуском этого приложения, можно просмотреть подробные сведения для запусков в Visual Studio, например входные и выходные данные.

1. В Cloud Explorer откройте контекстное меню приложения логики и выберите **Открыть журнал запусков**.

   ![Выбор параметра "Открыть журнал запусков"](./media/manage-logic-apps-with-visual-studio/view-run-history.png)

1. Чтобы просмотреть сведения об определенном запуске, дважды щелкните его. Например:

   ![Подробный журнал запусков](./media/manage-logic-apps-with-visual-studio/view-run-history-details.png)
  
   > [!TIP]
   > Чтобы отсортировать таблицу по свойству, выберите заголовок столбца для этого свойства. 

1. Разверните шаги, входные и выходные данные которых нужно просмотреть. Например:

   ![Просмотр входных и выходных данных для каждого шага](./media/manage-logic-apps-with-visual-studio/run-inputs-outputs.png)

## <a name="disable-or-enable-logic-app"></a>Отключение и включение приложения логики

Остановить запуск триггера в следующий раз, когда будет выполнено его условие, можно без удаления приложения логики. Отключение приложения логики предотвращает создание и запуск обработчиком Logic Apps будущих экземпляров рабочего процесса для этого приложения.
В Cloud Explorer откройте контекстное меню приложения логики и выберите **Отключить**.

![Отключение приложения логики](./media/manage-logic-apps-with-visual-studio/disable-logic-app.png)

> [!NOTE]
> При отключении приложения логики новые экземпляры запуска не создаются. Работа всех выполняющихся и ожидающих выполнения запусков будет продолжаться до завершения, что может занять некоторое время. 

Когда вы будете готовы возобновить работу приложения логики, вы можете повторно активировать его. В Cloud Explorer откройте контекстное меню приложения логики и выберите **Включить**.

![Включение приложения логики](./media/manage-logic-apps-with-visual-studio/enable-logic-app.png)

## <a name="delete-your-logic-app"></a>Удаление приложения логики

Чтобы удалить приложение логики с портала Azure, в Cloud Explorer откройте его контекстное меню и выберите **Удалить**.

![Удаление приложения логики](./media/manage-logic-apps-with-visual-studio/delete-logic-app.png)

> [!NOTE]
> При удалении приложения логики новые экземпляры запуска не создаются. Всех выполняющиеся и ожидающие выполнения запуски будут отменены. При наличии тысяч операций запуска отмена может занять значительное время. 

## <a name="troubleshooting"></a>Устранение неполадок

При открытии проекта приложения логики в конструкторе Logic Apps может не отображаться параметр для выбора подписки Azure. Вместо этого приложение логики откроется не с той подпиской Azure, которую вы хотите использовать. Это происходит, потому что после открытия JSON-файла для приложения логики Visual Studio кэширует первую выбранную подписку для использования в будущем. Чтобы устранить эту проблему, попробуйте одно из этих действий:

* Переименуйте JSON-файл приложения логики. Кэш подписки зависит от имени файла.

* Чтобы удалить ранее выбранных подписок для *все* приложений логики в решении, удалите скрытую папку Параметры Visual Studio (.vs) в каталоге решения. Это расположение хранит сведения о подписке.

## <a name="next-steps"></a>Дальнейшие действия

Из этой статьи вы узнали, как управлять развернутыми приложениями логики в Visual Studio. Теперь ознакомьтесь с настройкой определений приложения логики для развертывания:

> [!div class="nextstepaction"]
> [Создание определений для приложений логики с помощью JSON](../logic-apps/logic-apps-author-definitions.md)
