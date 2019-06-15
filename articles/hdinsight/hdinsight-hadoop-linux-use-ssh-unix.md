---
title: Использование SSH c Hadoop в Azure HDInsight
description: Вы можете получить доступ к HDInsight с помощью Secure Shell (SSH). Этот документ содержит сведения о подключении к HDInsight из клиентов Windows, Linux, Unix или macOS с помощью команд SSH и SCP.
author: hrasheed-msft
ms.reviewer: jasonh
keywords: команды hadoop в linux, команды linux hadoop, hadoop на платформе macos, ssh hadoop, кластер ssh hadoop
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: cc60a8ca0e0b04a5fcec5300fbeb1a0f34d1c7b4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64686382"
---
# <a name="connect-to-hdinsight-apache-hadoop-using-ssh"></a>Подключение к HDInsight (Apache Hadoop) с помощью SSH

Сведения о выполнении безопасного подключения к Apache Hadoop в Azure HDInsight с помощью [Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell). Дополнительные сведения о подключении через виртуальную сеть, см. в разделе [архитектуру виртуальной сети Azure HDInsight](./hdinsight-virtual-network-architecture.md) и [расширить HDInsight Azure, с помощью виртуальной сети Azure](./hdinsight-extend-hadoop-virtual-network.md).

Следующая таблица содержит адрес и порт сведения, необходимые при подключении к HDInsight с помощью клиента SSH:

| Адрес | Порт | Подключается к... |
| ----- | ----- | ----- |
| `<clustername>-ssh.azurehdinsight.net` | 22 | Основной головной узел |
| `<clustername>-ssh.azurehdinsight.net` | 23 | Дополнительный головной узел |
| `<clustername>-ed-ssh.azurehdinsight.net` | 22 | Граничный узел (службы машинного обучения в HDInsight) |
| `<edgenodename>.<clustername>-ssh.azurehdinsight.net` | 22 | Граничный узел (любой другой тип кластера, если существует граничный узел) |

Замените `<clustername>` именем кластера. Замените `<edgenodename>` на имя граничного узла. 

Мы рекомендуем __всегда подключаться к граничному узлу__ (при наличии его в кластере) с помощью SSH. Службы, размещенные на головных узлах, критически важны для работоспособности Hadoop. Граничный узел выполняет только размещаемые на нем службы. Дополнительные сведения об использовании граничных узлов см. в разделе [Доступ к граничному узлу](hdinsight-apps-use-edge-node.md#access-an-edge-node).

> [!TIP]  
> При первом подключении к HDInsight клиент SSH может отобразить предупреждение о том, что не удается установить подлинность узла. При появлении запроса выберите "Да", чтобы добавить узел в список доверенных серверов клиента SSH.
>
> Если вы ранее подключались к серверу с тем же именем, вы можете получить предупреждение о том, что сохраненный ключ узла не совпадает с ключом узла сервера. Обратитесь к документации для вашего клиента SSH, чтобы узнать, как удалить имеющуюся запись для имени сервера.

## <a name="ssh-clients"></a>SSH-клиенты

Системы Linux, Unix и macOS предоставляют команды `ssh` и `scp`. Клиент `ssh` обычно используется для создания удаленного сеанса командной строки с помощью системы на основе Unix или Linux. С помощью `scp` можно безопасно копировать файлы между клиентской и удаленной системой.

Microsoft Windows не устанавливает клиенты SSH по умолчанию. Клиенты `ssh` и `scp` для Windows доступны в следующих пакетах:

* [Клиент OpenSSH](https://docs.microsoft.com/windows-server/administration/openssh/openssh_install_firstuse). Необязательный компонент, представленный в версии Windows 10 Fall Creators Update.

* [Bash на Ubuntu в Windows 10](https://docs.microsoft.com/windows/wsl/about).

* [Azure Cloud Shell](../cloud-shell/quickstart.md). Cloud Shell позволяет использовать среду Bash в браузере.

* [Git](https://git-scm.com/).

Существуют также несколько графический SSH-клиентов, такие как [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/) и [MobaXterm](https://mobaxterm.mobatek.net/). Хотя эти клиенты можно использовать для подключения к HDInsight, процесс подключения отличается от использования служебной программы `ssh`. Дополнительные сведения см. в документации по используемому графическому клиенту.

## <a id="sshkey"></a>Аутентификация. Ключи SSH

Ключи SSH применяют [шифрование с открытым ключом](https://en.wikipedia.org/wiki/Public-key_cryptography) для проверки подлинности сеансов SSH. В отличие от паролей, они являются более безопасными и предоставляют простой способ защиты доступа к кластеру Hadoop.

Если учетная запись SSH защищена ключом, при подключении клиент должен предоставить соответствующий закрытый ключ.

* В большинстве клиентов можно настроить использование __ключа по умолчанию__. Например, клиент `ssh` будет искать закрытый ключ в `~/.ssh/id_rsa` в средах Linux и Unix.

* Вы можете указать __путь к закрытому ключу__. В клиенте `ssh` это можно сделать с помощью параметра `-i`. Например, `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.

* Если у вас есть __несколько закрытых ключей__ для использования на разных серверах, рекомендуем использовать служебные программы, например [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent). Служебную программу `ssh-agent` можно использовать для автоматического выбора подходящего ключа при установке сеанса SSH.

> [!IMPORTANT]  
> Если закрытый ключ защищен парольной фразой, при использовании ключа ее необходимо будет ввести. Существуют также служебные программы (например, `ssh-agent`), позволяющие кэшировать пароль.

### <a name="create-an-ssh-key-pair"></a>Создание пары ключей SSH

Чтобы создать файлы открытого и закрытого ключей, используйте команду `ssh-keygen`. Приведенная ниже команда создает пару 2048-разрядных ключей RSA, которую можно использовать с HDInsight.

    ssh-keygen -t rsa -b 2048

Во время создания ключа появится запрос на ввод сведений, например о том, где хранятся ключи и следует ли использовать парольную фразу. После завершения этой процедуры будут созданы два файла: файл открытого ключа и файл закрытого ключа.

* __Открытый ключ__ используется для создания кластера HDInsight. Этот файл имеет расширение `.pub`.

* __Закрытый ключ__ используется для проверки подлинности клиента в кластере HDInsight.

> [!IMPORTANT]  
> Эти ключи можно защитить с помощью парольной фразы. Парольная фраза — это, по сути, пароль закрытого ключа. Даже если кто-то получит ваш закрытый ключ, он не сможет использовать его, не зная парольную фразу.

### <a name="create-hdinsight-using-the-public-key"></a>Создание кластера HDInsight с помощью открытого ключа

| Метод создания | Использование открытого ключа |
| ------- | ------- |
| Портал Azure | Снимите флажок __Использовать тот же пароль в учетных данных кластера__, а затем выберите в качестве типа проверки подлинности SSH значение __Открытый ключ__. Наконец, выберите файл открытого ключа или вставьте текстовое содержимое файла в поле __Открытый ключ SSH__.</br>![Диалоговое окно открытого ключа SSH при создании кластера HDInsight](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png) |
| Azure PowerShell | Используйте `-SshPublicKey` параметр [New AzHdinsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) командлета, передайте содержимое открытого ключа в виде строки.|
| Инфраструктура CLI Azure | Используйте `--sshPublicKey` параметр [создания az hdinsight](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create) команды и передайте содержимое открытого ключа в виде строки. |
| Шаблон Resource Manager | Пример использования ключей SSH с помощью шаблона см. на странице [Deploy HDInsight on Linux (w/ Azure Storage, SSH key)](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/) (Развертывание HDInsight в Linux с использованием службы хранилища Azure и ключа SSH). Элемент `publicKeys` в файле [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) используется для передачи ключей в Azure при создании кластера. |

## <a id="sshpassword"></a>Аутентификация. Пароль

Учетные записи SSH можно защитить с помощью пароля. При подключении к HDInsight через SSH вам будет предложено ввести пароль.

> [!WARNING]  
> Мы не советуем использовать такой метод аутентификации для SSH. Пароли можно угадать. К тому же они подвержены атакам методом подбора. Вместо этого рекомендуется использовать [проверку подлинности с помощью ключей SSH](#sshkey).

> [!IMPORTANT]  
> Срок действия пароля учетной записи SSH истекает через 70 дней после создания кластера HDInsight. Если срок действия пароля истек, его можно изменить с помощью инструкций в документе по [управлению HDInsight](hdinsight-administer-use-portal-linux.md#change-passwords).

### <a name="create-hdinsight-using-a-password"></a>Создание кластера HDInsight с использованием пароля

| Метод создания | Указание пароля |
| --------------- | ---------------- |
| Портал Azure | По умолчанию учетная запись пользователя SSH содержит тот же пароль, что и учетная запись входа кластера. Чтобы использовать другой пароль, снимите флажок __Использовать тот же пароль в учетных данных кластера__, а затем введите пароль в поле __Пароль SSH__.</br>![Диалоговое окно пароля SSH при создании кластера HDInsight](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)|
| Azure PowerShell | Используйте `--SshCredential` параметр [New AzHdinsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) командлета и передать `PSCredential` , содержащий имя учетной записи пользователя SSH и пароль. |
| Инфраструктура CLI Azure | Используйте `--sshPassword` параметр [создания az hdinsight](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create) и укажите значение пароля. |
| Шаблон Resource Manager | Пример использования пароля с помощью шаблона см. на странице [Deploy HDInsight cluster with Storage and SSH password](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/) (Развертывание HDInsight с использованием службы хранилища и пароля SSH). Элемент `linuxOperatingSystemProfile` в файле [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) используется для передачи имени учетной записи SSH и пароля в Azure при создании кластера.|

### <a name="change-the-ssh-password"></a>Изменение пароля SSH

Дополнительные сведения об изменении пароля учетной записи пользователя SSH см. в разделе __Изменение паролей__ статьи [Управление кластерами Hadoop в HDInsight с помощью портала Azure](hdinsight-administer-use-portal-linux.md#change-passwords).

## <a id="domainjoined"></a>Аутентификация. Присоединенный к домену кластер HDInsight

Если вы используете __присоединенный к домену кластер HDInsight__, после подключения по протоколу SSH к локальному пользователю необходимо выполнить команду `kinit`. Эта команда запрашивает имя и пароль пользователя домена и проверяет подлинность сеанса с помощью домена Azure Active Directory, связанного с кластером.

Можно также включить проверку подлинности Kerberos на каждом домене соединенных узле (например, головного узла, граничного узла) в ssh с помощью учетной записи домена. Для этого измените файл конфигурации sshd:
```bash
sudo vi /etc/ssh/sshd_config
```
Раскомментируйте и замените `KerberosAuthentication` на `yes`

```bash
sudo service sshd restart
```

Вы можете в любое время проверить успешность проверки подлинности Kerberos с помощью команды `klist`.

Дополнительные сведения см. в статье [Настройка присоединенных к домену кластеров HDInsight (предварительная версия)](./domain-joined/apache-domain-joined-configure.md).

## <a name="connect-to-nodes"></a>Подключение к узлам

К головному узлу и граничному узлу (если он есть) можно подключиться по Интернету через порты 22 и 23.

* При подключении к __головному узлу__ используйте порт __22__, чтобы подключиться к первичному головному узлу, и порт __23__, чтобы подключиться к вторичному головному узлу. Используйте полное доменное имя `clustername-ssh.azurehdinsight.net`, где `clustername` — имя кластера.

    ```bash
    # Connect to primary head node
    # port not specified since 22 is the default
    ssh sshuser@clustername-ssh.azurehdinsight.net

    # Connect to secondary head node
    ssh -p 23 sshuser@clustername-ssh.azurehdinsight.net
    ```
    
* Чтобы подключиться к __граничному узлу__, используйте порт 22. Полное доменное имя — `edgenodename.clustername-ssh.azurehdinsight.net`, где `edgenodename` — это имя, которое вы указали при создании граничного узла. `clustername` — это имя кластера.

    ```bash
    # Connect to edge node
    ssh sshuser@edgnodename.clustername-ssh.azurehdinsight.net
    ```

> [!IMPORTANT]  
> В предыдущих примерах предполагается, что вы используете аутентификацию на основе пароля или что аутентификация на основе сертификата выполняется автоматически. Если вы используете пару ключей SSH для проверки подлинности и сертификат не используется автоматически, укажите закрытый ключ с помощью параметра `-i`. Например, `ssh -i ~/.ssh/mykey sshuser@clustername-ssh.azurehdinsight.net`.

После подключения командная строка изменится, и в ней отобразится имя пользователя SSH и узел, к которому вы подключены. Например, если к первичному головному узлу подключается пользователь `sshuser`, командная строка выглядит так: `sshuser@hn0-clustername:~$`.

### <a name="connect-to-worker-and-apache-zookeeper-nodes"></a>Подключение к рабочим узлам и узлам Apache Zookeeper

Рабочие узлы и узлы Zookeeper недоступны напрямую через Интернет, но к ним можно получить доступ с головного или граничного узла кластера. Шаги ниже описывают общую процедуру подключения к другим узлам.

1. Используйте SSH, чтобы подключиться к головному или граничному узлу:

        ssh sshuser@myedge.mycluster-ssh.azurehdinsight.net

2. После подключения по протоколу SSH используйте команду `ssh`, чтобы соединиться с рабочим узлом в кластере:

        ssh sshuser@wn0-myhdi

    Чтобы получить список имен узлов, обратитесь к разделу [Пример. Получение полного доменного имени для узлов кластера](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes).

Если учетная запись SSH защищена __паролем__, при подключении введите пароль.

Если она защищена __ключами SSH__, убедитесь, что на клиентском компьютере включено перенаправление SSH.

> [!NOTE]  
> Другой способ получить прямой доступ ко всем узлам кластера — установить HDInsight в виртуальной сети Azure. Затем к той же виртуальной сети можно присоединить удаленный компьютер и получить прямой доступ ко всем узлам кластера.
>
> Дополнительные сведения см. в статье [Расширение возможностей HDInsight с помощью виртуальной сети Azure](hdinsight-extend-hadoop-virtual-network.md).

### <a name="configure-ssh-agent-forwarding"></a>Настройка перенаправления SSH-агента

> [!IMPORTANT]  
> При описании шагов ниже предполагается использование системы под управлением Linux или UNIX, а также Bash Windows 10. Если выполнение этих шагов в вашей системе не дает результатов, см. документацию по соответствующему SSH-клиенту.

1. Откройте в текстовом редакторе файл `~/.ssh/config`. Если этот файл отсутствует, его можно создать. Для этого в командной строке введите команду `touch ~/.ssh/config`.

2. Добавьте следующий текст в файл `config`.

        Host <edgenodename>.<clustername>-ssh.azurehdinsight.net
          ForwardAgent yes

    Замените сведения об __узле__ на адрес узла, к которому вы подключаетесь с помощью SSH. В предыдущем примере используется граничный узел. Эта запись позволяет настроить перенаправление агента SSH для указанного узла.

3. Чтобы проверить перенаправление агента SSH, выполните следующую команду в терминале:

        echo "$SSH_AUTH_SOCK"

    Эта команда возвращает сведения аналогичные следующим:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Если сведения не возвращаются, то это значит, что `ssh-agent` не работает. Чтобы получить сведения о скриптах запуска агентов, см. документ по [использованию агента SSH с SSH (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) или документацию по соответствующему SSH-клиенту.

4. Убедившись в том, что **агент SSH** запущен, используйте следующую команду, чтобы добавить закрытый ключ SSH для агента:

        ssh-add ~/.ssh/id_rsa

    Если закрытый ключ хранится в другом файле, замените `~/.ssh/id_rsa` на путь к файлу.

5. Подключитесь к граничному или головному узлу кластера с помощью SSH. Затем воспользуйтесь командой SSH для подключения к рабочему узлу или узлу Zookeeper. Подключение устанавливается с использованием перенаправленного ключа.

## <a name="copy-files"></a>Копирование файлов

Программу `scp` можно использовать для копирования файлов в отдельные узлы в кластере и обратно. Например, следующая команда копирует каталог `test.txt` из локальной системы в первичный головной узел:

```bash
scp test.txt sshuser@clustername-ssh.azurehdinsight.net:
```

Так как после символа `:` не указан путь, файл помещается в домашний каталог `sshuser`.

Например, следующая команда копирует файл `test.txt` из домашнего каталога `sshuser` в первичный головной узел в локальной системе:

```bash
scp sshuser@clustername-ssh.azurehdinsight.net:test.txt .
```

> [!IMPORTANT]  
> Команда `scp` может получать доступ только к файловой системе отдельных узлов в кластере. Ее нельзя использовать для доступа к данным в HDFS-совместимом хранилище кластера.
>
> Используйте команду `scp`, если вам нужно отправить ресурс, чтобы использовать его из сеанса SSH. Например, отправьте скрипт Python, а затем запустите его из сеанса SSH.
>
> Сведения о непосредственной загрузке данных в HDFS-совместимое хранилище см. в следующих документах:
>
> * [Использование службы хранилища Azure с кластерами Azure HDInsight](hdinsight-hadoop-use-blob-storage.md).
>
> * [Использование HDInsight и Azure Data Lake Storage](hdinsight-hadoop-use-data-lake-store.md).

## <a name="next-steps"></a>Дальнейшие действия

* [Использование туннелирования SSH для доступа к веб-интерфейсу Ambari, JobHistory, NameNode, Oozie и другим веб-интерфейсам](hdinsight-linux-ambari-ssh-tunnel.md)
* [Расширение возможностей HDInsight с помощью виртуальной сети Azure](hdinsight-extend-hadoop-virtual-network.md)
* [Доступ к граничному узлу](hdinsight-apps-use-edge-node.md#access-an-edge-node)
