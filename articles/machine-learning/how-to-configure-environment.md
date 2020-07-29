---
title: Настройка среды разработки Python
titleSuffix: Azure Machine Learning
description: Узнайте, как настроить среду разработки для Машинное обучение Azure. Используйте среды Conda, Создавайте файлы конфигурации и настраивайте собственный облачный сервер записных книжек, записные книжки Jupyter, Azure Databricks, IDE, редакторы кода и виртуальную машину для обработки и анализа данных.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.date: 12/27/2019
ms.topic: conceptual
ms.custom: how-to, tracking-python
ms.openlocfilehash: f0ac64fa184382f3939530565caa93f87da0fbf6
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/28/2020
ms.locfileid: "87321485"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Настройка среды разработки для Машинного обучения Azure
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Из этой статьи вы узнаете, как настроить среду разработки для работы с Машинное обучение Azure. Машинное обучение Azure не зависит от платформы. Единственным жестким требованием для среды разработки является Python 3. Также рекомендуется использовать изолированную среду, например Anaconda или Virtualenv.

В следующей таблице показаны все среды разработки, описываемые в этой статье, а также достоинства и недостатки.

| Среда | Плюсы | Минусы |
| --- | --- | --- |
| [Облачный Машинное обучение Azure вычислительный экземпляр (Предварительная версия)](#compute-instance) | Самый простой способ начать работу. В виртуальной машине рабочей области уже установлен весь пакет SDK, а учебные материалы для записных книжек предварительно клонированы и готовы к выполнению. | Отсутствие контроля над средой разработки и зависимостями. Дополнительные затраты на виртуальную машину Linux (виртуальную машину можно остановить, когда она не используется, чтобы избежать расходов). См. сведения о [ценах](https://azure.microsoft.com/pricing/details/virtual-machines/linux/). |
| [Локальная среда](#local) | Полный контроль над средой разработки и зависимостями. Запустите с любым инструментом сборки, средой или интегрированной средой разработки. | Приступая к работе, занимает больше времени. Необходимо установить необходимые пакеты SDK, а также установить среду, если у вас ее еще нет. |
| [Azure Databricks](#aml-databricks) | Идеально подходит для выполнения крупномасштабных рабочих процессов машинного обучения на масштабируемой Apache Sparkной платформе. | Избыточное для экспериментального машинного обучения, а также экспериментов и рабочих процессов с меньшим масштабом. Дополнительные затраты, вызванные Azure Databricks. См. сведения о [ценах](https://azure.microsoft.com/pricing/details/databricks/). |
| [Виртуальная машина для обработки и анализа данных (DSVM)](#dsvm) | Аналогично облачному вычислительному экземпляру (Python и пакет SDK предварительно установлены), но с предварительно установленными дополнительными популярными средствами обработки и анализа данных и машинного обучения. Простота масштабирования и объединения с другими пользовательскими инструментами и рабочими процессами. | Более медленный режим работы по сравнению с облачным вычислительным экземпляром. |

В этой статье также приводятся дополнительные советы по использованию следующих средств:

* [Записные книжки Jupyter](#jupyter). Если вы уже используете Jupyter Notebook, пакет SDK будет иметь несколько дополнительных средств, которые следует установить.

* [Visual Studio Code](#vscode). при использовании Visual Studio Code [расширение машинное обучение Azure](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai) включает расширенную языковую поддержку для Python, а также функции, позволяющие работать с машинное обучение Azure гораздо более удобной и продуктивной.

## <a name="prerequisites"></a>Предварительные требования

Рабочая область машинного обучения Azure. Дополнительные сведения см. в [инструкциях по созданию рабочей области Машинного обучения Azure](how-to-manage-workspace.md). Рабочая область — это все, что нужно для начала работы с вашим собственным [облачным сервером записных книжек](#compute-instance), [DSVM](#dsvm)или [Azure Databricks](#aml-databricks).

Чтобы установить среду пакета SDK для [локального компьютера](#local), необходимо также [Jupyter Notebook сервер](#jupyter) или [Visual Studio Code](#vscode) :

- Диспетчер пакетов [Anaconda](https://www.anaconda.com/download/) или [Miniconda](https://conda.io/miniconda.html) .

- В Linux или macOS вам понадобится оболочка bash.

    > [!TIP]
    > Если в Linux или macOS вы используете оболочку, отличающуюся от bash (например, zsh), при выполнении некоторых команд могут возникнуть ошибки. Чтобы решить эту проблему, используйте команду `bash`, чтобы запустить bash и выполните там нужные команды.

- В Windows вам понадобится командная строка по умолчанию или командная строка Anaconda (устанавливается вместе с Anaconda и Miniconda).

## <a name="your-own-cloud-based-compute-instance"></a><a id="compute-instance"></a>Собственный облачный вычислительный экземпляр

Машинное обучение Azure [вычислительный экземпляр (Предварительная версия)](concept-compute-instance.md) — это безопасная облачная Рабочая станция Azure, которая предоставляет специалистам по обработке и анализу данных Jupyter портативный компьютер, JupyterLab и полностью подготовленную среду машинного обучения.

Нет ничего для установки или настройки для вычислительного экземпляра.  Создайте его в рабочей области Машинное обучение Azure в любое время. Укажите только имя и укажите тип виртуальной машины Azure. Попробуйте сейчас с помощью этого [учебника: Настройка среды и рабочей области](tutorial-1st-experiment-sdk-setup.md).

Дополнительные сведения об экземплярах вычислений, включая установку пакетов, см. в разделе [COMPUTE Instances](concept-compute-instance.md).

Чтобы приостанавливаться к выплатам за вычислительные ресурсы, необходимо [Отключить вычислительный экземпляр](tutorial-1st-experiment-sdk-train.md#clean-up-resources).

## <a name="data-science-virtual-machine"></a><a id="dsvm"></a>Виртуальная машина для обработки и анализа данных

DSVM — это настраиваемый образ виртуальной машины. Он предназначен для обработки и анализа данных, и на нем предварительно настроены:

  - пакеты TensorFlow, PyTorch, Scikit-learn, XGBoost и пакет SDK для Машинного обучения Azure;
  - популярные инструменты для обработки и анализа данных, в том числе изолированная среда Spark и Drill;
  - инструменты Azure, такие как интерфейс командной строки Azure, AzCopy и Обозреватель службы хранилища;
  - интегрированные среды разработки, например Visual Studio Code и PyCharm;
  - Сервер Jupyter Notebook.

Пакет SDK для Машинного обучения Azure работает с версией DSVM для Ubuntu или Windows. Но если вы также планируете использовать Виртуальную машину для обработки и анализа данных в качестве цели для вычислений, это возможно только на Ubuntu.

Чтобы использовать DSVM в качестве среды разработки:

1. Создайте DSVM в любой из следующих сред:

    * На портале Azure:

        * [Создание виртуальной машины для обработки и анализа данных на платформе Ubuntu](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)

        * [Создание виртуальной машины Windows для обработки и анализа данных](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm)

    * Интерфейс командной строки Azure:

        > [!IMPORTANT]
        > * Чтобы использовать Azure CLI, нужно сначала войти в подписку Azure с помощью команды `az login`.
        >
        > * При использовании команд на этом шаге необходимо указать имя группы ресурсов, имя виртуальной машины, имя пользователя и пароль.

        * Чтобы создать DSVM на платформе Ubuntu, воспользуйтесь приведенной ниже командой.

            ```azurecli-interactive
            # create a Ubuntu DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            # If you need to create a new resource group use: "az group create --name YOUR-RESOURCE-GROUP-NAME --location YOUR-REGION (For example: westus2)"
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:linux-data-science-vm-ubuntu:linuxdsvmubuntu:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --generate-ssh-keys --authentication-type password
            ```

        * Чтобы создать DSVM на платформе Windows, воспользуйтесь приведенной ниже командой.

            ```azurecli-interactive
            # create a Windows Server 2016 DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:dsvm-windows:server-2016:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --authentication-type password
            ```

2. Пакет SDK для Машинного обучения Azure уже установлен в DSVM. Чтобы использовать среду Conda, которая содержит пакет SDK, используйте одну из следующих команд:

    * Виртуальная машина для обработки и анализа данных под управлением Ubuntu:

        ```bash
        conda activate py36
        ```

    * Виртуальная машина для обработки и анализа данных под управлением Windows:

        ```bash
        conda activate AzureML
        ```

1. Чтобы убедиться, у вас есть доступ к пакету SDK и проверить версию, используйте следующий код Python:

    ```python
    import azureml.core
    print(azureml.core.VERSION)
    ```

1. Сведения о настройке DSVM для использования рабочей области Машинное обучение Azure см. в разделе [Создание файла конфигурации рабочей области](#workspace) .

Дополнительные сведения о Виртуальных машинах для обработки и анализа данных см. [здесь](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a name="local-computer"></a><a id="local"></a>Локальный компьютер

Если вы используете локальный компьютер (который может также быть удаленной виртуальной машиной), создайте среду Anaconda и установите пакет SDK. Приведем пример:

1. Скачайте и установите [Anaconda](https://www.anaconda.com/distribution/#download-section) (версия Python 3,7), если у вас ее еще нет.

1. Откройте командную строку Anaconda и создайте среду с помощью следующих команд:

    Выполните следующую команду, чтобы создать среду.

    ```bash
    conda create -n myenv python=3.7.7
    ```

    Затем активируйте среду.

    ```bash
    conda activate myenv
    ```

    В этом примере создается среда с помощью Python 3.7.7, но можно выбрать любые конкретные версии. Совместимость с пакетом SDK может быть не гарантирована с определенными основными версиями (рекомендуется 3.5). при возникновении ошибок рекомендуется попробовать другую версию или подверсию в среде Anaconda. Скачивание компонентов и пакетов и последующее создание среды займет несколько минут.

1. Выполните следующие команды в новой среде, чтобы включить зависящие от среды ядра IPython Notebook. Это обеспечит ожидаемое поведение при импорте ядра и пакета при работе с записными книжками Jupyter в средах Anaconda:

    ```bash
    conda install notebook ipykernel
    ```

    Затем выполните следующую команду, чтобы создать ядро:

    ```bash
    ipython kernel install --user --name myenv --display-name "Python (myenv)"
    ```

1. Для установки пакетов используйте следующие команды:

    Эта команда устанавливает базовый пакет SDK Машинное обучение Azure с записной книжкой и `automl` дополнительными данными. Это `automl` очень большая установка, и ее можно удалить из квадратных скобок, если вы не планируете выполнять автоматические эксперименты машинного обучения. `automl`Дополнительный пакет также включает в себя машинное обучение Azure пакета SDK для подготовки данных по умолчанию в качестве зависимости.

    ```bash
    pip install azureml-sdk[notebooks,automl]
    ```

   > [!NOTE]
   > * Если появится сообщение о том, что не удается удалить PyYAML, попробуйте использовать другую команду:
   >
   >   `pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML`
   >
   > * Начиная с версии macOS Catalina, в качестве стандартной оболочки входа и интерактивной оболочки используется zsh (Z shell). В zsh выполните следующую команду, в которой используется escape-последовательность с символом "\\" (обратная косая черта).
   >
   >   `pip install --upgrade azureml-sdk\[notebooks,automl\]`

   Установка пакета SDK займет несколько минут. Дополнительные сведения о параметрах установки см. в разделе [руководства по установке](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

1. Установите другие пакеты для экспериментов машинного обучения.

    Используйте одну из следующих команд и замените *\<new package>* пакетом, который необходимо установить. Установка пакетов `conda install` с помощью требует, чтобы пакет был частью текущих каналов (новые каналы можно добавить в Anaconda Cloud).

    ```bash
    conda install <new package>
    ```

    Кроме того, пакеты можно устанавливать с помощью `pip` .

    ```bash
    pip install <new package>
    ```

### <a name="jupyter-notebooks"></a><a id="jupyter"></a>Jupyter Notebook

Приложения Jupyter Notebook предоставляются компанией [Project Jupyter](https://jupyter.org/). Это решение предназначено для реализации интерактивного процесса кодирования, в котором создаются документы с динамическим кодом, описательным текстом и графикой. Приложения Jupyter Notebook также позволяют использовать результаты вашей работы совместно с другими пользователями, так как вы можете сохранять выходные данные разделов кода в документе. Jupyter Notebook можно устанавливать на различных платформах.

Процедура, описанная в разделе [локальный компьютер](#local) , устанавливает необходимые компоненты для запуска записных книжек Jupyter в среде Anaconda.

Чтобы включить эти компоненты в среде Jupyter Notebook, выполните следующие действия.

1. Откройте запрос Anaconda и активируйте среду.

    ```bash
    conda activate myenv
    ```

1. Клонирование [репозитория GitHub](https://aka.ms/aml-notebooks) для набора примеров записных книжек.

    ```bash
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Запустите сервер Jupyter Notebook с помощью следующей команды:

    ```bash
    jupyter notebook
    ```

1. Чтобы убедиться, что Jupyter Notebook может использовать пакет SDK, создайте **новую** записную книжку, выберите **Python 3** в качестве ядра, а затем выполните следующую команду в ячейке записной книжки:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Если возникли проблемы с импортом модулей и получением `ModuleNotFoundError` , убедитесь, что ядро Jupyter подключено к правильному пути для вашей среды, выполнив следующий код в ячейке записной книжки.

    ```python
    import sys
    sys.path
    ```

1. Чтобы настроить Jupyter Notebook для использования рабочей области Машинное обучение Azure, перейдите к разделу [Создание файла конфигурации рабочей области](#workspace) .

### <a name="visual-studio-code"></a><a id="vscode"></a>Visual Studio Code

Visual Studio Code — очень популярный редактор межплатформенного кода, поддерживающий обширный набор языков программирования и средств с помощью расширений, доступных в [Visual Studio Marketplace](https://marketplace.visualstudio.com/vscode). [Расширение машинное обучение Azure](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai) устанавливает [расширение Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) для кодирования во всех типах окружений Python (Virtual, Anaconda и т. д.). Кроме того, он предоставляет удобные возможности для работы с Машинное обучение Azure ресурсами и выполнения Машинное обучение Azure экспериментов, не открывая Visual Studio Code.

Использование Visual Studio Code для разработки:

1. Установка расширения Машинное обучение Azure для Visual Studio Code см. в разделе [машинное обучение Azure](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).

    Дополнительные сведения см. в статье [Приступая к работе с Azure Machine Learning for Visual Studio Code](tutorial-setup-vscode-extension.md).

1. Сведения об использовании Visual Studio Code для любого типа разработки на Python см. в статье Начало [работы с Python в VSCode](https://code.visualstudio.com/docs/python/python-tutorial).

    - Чтобы выбрать среду пакета SDK Python, содержащую пакет SDK, откройте VS Code, а затем нажмите клавиши CTRL + SHIFT + P (Linux и Windows) или Command + Shift + P (Mac).
        - Откроется __Палитра команд__ .

    - Введите __Python: выберите интерпретатор__, а затем выберите соответствующую среду.

1. Чтобы проверить, можно использовать пакет SDK, создайте новый файл Python (. Копировать), содержащий следующий код:

    ```python
    #%%
    import azureml.core
    azureml.core.VERSION
    ```
    Запустите этот код, щелкнув CodeLens "выполнить ячейку" или просто нажмите клавиши SHIFT + ВВОД.
<a name="aml-databricks"></a>

## <a name="azure-databricks"></a>Azure Databricks
Azure Databricks — это среда на основе Apache Spark в облаке Azure. Она предоставляет среду для совместной работы на основе записных книжек с вычислительным кластером на основе ЦП или GPU.

Как Azure Databricks работает с Машинное обучение Azure:
+ Вы можете обучить модель с помощью Spark MLlib и развернуть модель в ACI/AKS из Azure Databricks.
+ Вы также можете использовать [автоматизированные возможности машинного обучения](concept-automated-ml.md) в специальном пакете SDK для машинного обучения Azure с Azure Databricks.
+ Azure Databricks можно использовать в качестве целевого объекта вычислений из [конвейера машинное обучение Azure](concept-ml-pipelines.md).

### <a name="set-up-your-databricks-cluster"></a>Настройка кластера кирпичей данных

Создание [кластера кирпичей](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal). Некоторые параметры применяются только в том случае, если пакет SDK устанавливается для автоматического машинного обучения в модулях.
**Создание кластера занимает несколько минут.**

Используйте следующие параметры.

| Параметр |Область применения| Значение |
|----|---|---|
| Имя кластера |always| yourclustername |
| Среда выполнения Databricks |always|Среда выполнения, не поддерживающая ML 6,5 (Scala 2,11, Spark 2.4.3) |
| Версия Python |always| 3 |
| Рабочие роли |always| 2 или больше |
| Типы виртуальных машин узла рабочей роли <br>(определяет максимальное число одновременных итераций) |Автоматическое Машинное обучение<br>только| Предпочитается виртуальная машина, оптимизированная для операций в памяти |
| Включение автомасштабирования |Автоматическое Машинное обучение<br>только| Снять пометку |

Дождитесь запуска кластера, прежде чем продолжать.

### <a name="install-the-correct-sdk-into-a-databricks-library"></a>Установка правильного пакета SDK в библиотеке кирпичей
После запуска кластера [Создайте библиотеку](https://docs.databricks.com/user-guide/libraries.html#create-a-library) , чтобы присоединить соответствующий пакет SDK машинное обучение Azure к кластеру.

1. Щелкните правой кнопкой мыши текущую папку рабочей области, в которой нужно сохранить библиотеку. Выберите **создать**  >  **библиотеку**.

1. Выберите **только один** вариант (другие установки пакета SDK не поддерживаются).

   |&nbsp;Дополнительные возможности пакета SDK &nbsp;|Источник|&nbsp;Имя PyPi&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
   |----|---|---|
   |Для кирпичей| Отправка Python Egg или PyPI | azureml-sdk[databricks]|
   |Для кирпичей с-<br> возможности автоматического ML| Отправка Python Egg или PyPI | azureml-SDK [аутомл]|

   > [!Warning]
   > Другие дополнительные компоненты SDK устанавливаться не могут. Выберите только один из указанных выше параметров [кирпичы данных] или [аутомл].

   * Не выбирайте **Автоматическое присоединение ко всем кластерам**.
   * Выберите **присоединить** рядом с именем кластера.

1. Отслеживать ошибки до тех пор, пока состояние не изменится на " **подключено**", что может занять несколько минут.  Если этот шаг завершается ошибкой:

   Попробуйте перезапустить кластер, выполнив следующие действия.
   1. В левой области щелкните **Кластеры**.
   1. Выберите имя кластера в таблице.
   1. На вкладке **Библиотеки** щелкните **Перезапустить**.

   Также учитывайте следующее.
   + В Аутомл config при использовании Azure Databricks добавьте следующие параметры:
       1. ```max_concurrent_iterations```основано на количестве рабочих узлов в кластере.
        2. ```spark_context=sc```основывается на контексте Spark по умолчанию.
   + Если у вас старая версия пакета SDK, отмените ее выбор из установленных библиотек кластера и перейдите в корзину. Установите новую версию пакета SDK и перезапустите кластер. Если после перезагрузки возникла ошибка, отключите и повторно подключите кластер.

Если установка прошла успешно, Импортированная библиотека должна выглядеть следующим образом:

Пакет SDK для блоков с **_автоматическим_** ![ пакетом SDK машинное обучение Azure для кирпичей](./media/how-to-configure-environment/amlsdk-withoutautoml.jpg)

Пакет SDK для кирпичей **с** автоматическим пакетом SDK для машинного обучения ![ с автоматическим машинным обучением, установленным на модулях](./media/how-to-configure-environment/automlonadb.png)

### <a name="start-exploring"></a>Начало знакомства

Попробуйте продукт:
+ Несмотря на то что доступны многие примеры записных книжек, **с Azure Databricks работают только [эти образцы записных книжек](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) .**

+ Импортируйте эти примеры непосредственно из рабочей области. См. ниже: ![ выберите Импорт ](./media/how-to-configure-environment/azure-db-screenshot.png)
 ![ панели импорта](./media/how-to-configure-environment/azure-db-import.png)

+ Узнайте, как [создать конвейер с модулями обработки в качестве обучающего вычислений](how-to-create-your-first-pipeline.md).

## <a name="create-a-workspace-configuration-file"></a><a id="workspace"></a>Создание файла конфигурации рабочей области

Файл конфигурации рабочей области — это JSON-файл, который сообщает пакету SDK, как взаимодействовать с рабочей областью Машинное обучение Azure. В этого файла *config.json* следующий формат:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Этот JSON-файл должен находиться в структуре каталогов, которые содержат сценарии Python или приложения Jupyter Notebook. Он может находиться в том же каталоге, подкаталоге с именем *.azureml* или родительском каталоге.

Чтобы использовать этот файл в коде, выполните `ws=Workspace.from_config()`. Этот код загружает данные из файла и подключается к рабочей области.

Существует три способа создать файл конфигурации.

* **Используйте [ws. write_config](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)**: для записи *config.jsв* файле. Файл содержит сведения о конфигурации вашей рабочей области. Вы можете скачать или скопировать файл *config.json* в другие среды разработки.

* **Скачайте файл**: в [портал Azure](https://ms.portal.azure.com)выберите **скачать config.jsв** разделе **Обзор** рабочей области.

     ![Портал Azure](./media/how-to-configure-environment/configure.png)

* **Создайте файл программно**: в следующем фрагменте кода подключитесь к рабочей области, указав идентификатор подписки, группу ресурсов и имя рабочей области. Затем конфигурация рабочей области будет сохранена в файле.

    ```python
    from azureml.core import Workspace

    subscription_id = '<subscription-id>'
    resource_group  = '<resource-group>'
    workspace_name  = '<workspace-name>'

    try:
        ws = Workspace(subscription_id = subscription_id, resource_group = resource_group, workspace_name = workspace_name)
        ws.write_config()
        print('Library configuration succeeded')
    except:
        print('Workspace not found')
    ```

    Этот код записывает файл конфигурации в файл *. azureml/config.jsв* файле.

## <a name="next-steps"></a>Дальнейшие действия

- [Обучение модели](tutorial-train-models-with-aml.md) на машинное обучение Azure с помощью набора данных MNIST
- Просмотрите статью [What is the Azure Machine Learning SDK for Python?](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) (Что такое пакет SDK для Машинного обучения Azure для Python?)
