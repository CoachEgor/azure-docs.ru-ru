---
title: Региональное аварийное восстановление для Azure Databricks
description: В этой статье описывается подход к аварийному восстановлению в Azure Databricks.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 03/13/2019
ms.openlocfilehash: 800b51c8f900d2ea99900ea147b33010452348f5
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/03/2020
ms.locfileid: "75639877"
---
# <a name="regional-disaster-recovery-for-azure-databricks-clusters"></a>Региональное аварийное восстановление кластеров Azure Databricks

В этой статье описывается архитектура аварийного восстановления, которую удобно использовать для кластеров Azure Databricks, а также действия, необходимые для реализации этой архитектуры.

## <a name="azure-databricks-architecture"></a>Архитектура Azure Databricks

На высоком уровне при создании рабочей области Azure Databricks с помощью портала Azure [управляемое устройство](../azure-resource-manager/managed-applications/overview.md) развертывается как ресурс Azure в вашей подписке, в выбранном регионе Azure (например, "Западная часть США"). Это устройство развертывается в [виртуальной сети Azure](../virtual-network/virtual-networks-overview.md) с [группой безопасности сети](../virtual-network/manage-network-security-group.md) и учетной записью службы хранилища Azure, доступной в вашей подписке. Виртуальная сеть обеспечивает защиту на уровне периметра для рабочей области Databricks и защищается с помощью группы безопасности сети. В этой рабочей области можно создавать кластеры Databricks, указывая тип виртуальной машины работника и драйвера, а также версию среды выполнения Databricks. Сохраненные данные доступны в вашей учетной записи хранения, которая может быть хранилищем BLOB-объектов Azure или Azure Data Lake Storage. После создания кластера можно выполнять задания через записные книжки, интерфейсы REST API, а также конечные точки ODBC и JDBC, присоединяя их к определенному кластеру.

Плоскость управления Databricks контролирует и отслеживает среду рабочей области Databricks. Все операции управления, например создание кластера, инициируются с плоскости управления. Все метаданные, например запланированные задания, хранятся в Базе данных Azure с георепликацией для обеспечения отказоустойчивости.

![Архитектура Databricks](media/howto-regional-disaster-recovery/databricks-architecture.png)

Одно из преимуществ этой архитектуры заключается в том, что пользователи могут подключить Azure Databricks к любому ресурсу хранилища в своей учетной записи. Главное преимущество состоит в том, что вычислительную среду (Azure Databricks) и хранилище можно масштабировать независимо друг от друга.

## <a name="how-to-create-a-regional-disaster-recovery-topology"></a>Как создать топологию регионального аварийного восстановления

Как вы могли заметить в представленном выше описании архитектуры, для конвейера больших данных с Azure Databricks используется ряд компонентов: служба хранилища Azure, База данных Azure и другие источники данных. Azure Databricks — это *вычислительная среда* для конвейера больших данных. По своей природе она *временна*, то есть несмотря на то что ваши данные по-прежнему доступны в службе хранилища Azure, работу *вычислительной среды* (кластера Azure Databricks) можно завершить, чтобы вам не пришлось платить за ее использование, когда в этом нет необходимости. *Вычислительная среда* (Azure Databricks) и источники хранения должны находиться в одном регионе во избежание высокой задержки при выполнении заданий.  

Создавая собственную топологию регионального аварийного восстановления, соблюдайте следующие требования:

   1. Подготавливайте несколько рабочих областей Azure Databricks в отдельных регионах Azure. Например, создайте основную рабочую область Azure Databricks в регионе "Восточная часть США 2". Создайте дополнительную рабочую область Azure Databricks для аварийного восстановления в отдельном регионе, например "Западная часть США".

   2. Используйте [геоизбыточное хранилище](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage). Данные, связанные с Azure Databricks, по умолчанию хранятся в службе хранилища Azure. Результаты выполнения заданий Databricks также размещаются в хранилище BLOB-объектов Azure, чтобы обеспечить их устойчивость и высокую доступность послу завершения работы кластера. Так как служба хранилища и кластер Databricks находятся в одном регионе, следует использовать геоизбыточное хранилище, чтобы можно было получить доступ к данным в дополнительном регионе, если основной регион недоступен.

   3. После создания дополнительного региона необходимо перенести пользователей, их папки, записные книжки, конфигурацию кластеров и заданий, библиотеки, хранилище и скрипты инициализации, а также заново настроить управление доступом. Дополнительные сведения представлены в следующем разделе.

## <a name="detailed-migration-steps"></a>Подробное описание этапов миграции

1. **Настройка интерфейса командной строки Databricks на компьютере**

   В этой статье представлен ряд примеров кода, в которых для выполнения большинства автоматизированных задач используется интерфейс командной строки, так как эту оболочку удобнее использовать, чем REST API Azure Databricks.

   Прежде чем выполнять какие-либо действия по миграции, установите databricks-cli на настольном компьютере или виртуальной машине, которые планируется использовать для работы. Дополнительные сведения см. в разделе [Установка интерфейса командной строки Databricks](/azure/databricks/dev-tools/databricks-cli).

   ```bash
   pip install databricks-cli
   ```

   > [!NOTE]
   > Все представленные в этой статье скрипты python рассчитаны на работу с Python версий от 2.7, но ниже 3.x.

2. **Настройка двух профилей**

   Настройте один профиль для основной рабочей области и еще один для дополнительной:

   ```bash
   databricks configure --profile primary
   databricks configure --profile secondary
   ```

   Представленные в этой статье блоки кода переключаются между профилями на каждом последующем этапе с помощью соответствующей команды рабочей области. Обязательно замените имена в каждом блоке кода на названия созданных вами профилей.

   ```python
   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"
   ```

   При необходимости вы можете вручную переключать профили в командной строке:

   ```bash
   databricks workspace ls --profile primary
   databricks workspace ls --profile secondary
   ```

3. **Перенос пользователей Azure Active Directory**

   Вручную добавьте в дополнительную рабочую область тех же пользователей Azure Active Directory, которые существуют в основной рабочей области.

4. **Перенос записных книжек и папок пользователей**

   Используйте приведенный ниже код python, чтобы перенести изолированные пользовательские среды, включающие вложенную структуру папок и записные книжки каждого пользователя.

   > [!NOTE]
   > На этом этапе не копируются библиотеки, так как базовый API не поддерживает их.

   Скопируйте и сохраните приведенный ниже скрипт python в файл и запустите его в командной строке Databricks. Например, `python scriptname.py`.

   ```python
   from subprocess import call, check_output

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Get a list of all users
   user_list_out = check_output(["databricks", "workspace", "ls", "/Users", "--profile", EXPORT_PROFILE])
   user_list = user_list_out.splitlines()

   # Export sandboxed environment (folders, notebooks) for each user and import into new workspace.
   # Libraries are not included with these APIs / commands.

   for user in user_list:
     print "Trying to migrate workspace for user " + user

     call("mkdir -p " + user, shell=True)
     export_exit_status = call("databricks workspace export_dir /Users/" + user + " ./" + user + " --profile " + EXPORT_PROFILE, shell=True)

     if export_exit_status==0:
       print "Export Success"
       import_exit_status = call("databricks workspace import_dir ./" + user + " /Users/" + user + " --profile " + IMPORT_PROFILE, shell=True)
       if import_exit_status==0:
         print "Import Success"
       else:
         print "Import Failure"
     else:
       print "Export Failure"

   print "All done"
   ```

5. **Перенос конфигураций кластеров**

   После переноса записных книжек при желании вы можете перенести конфигурации кластеров в новую рабочую область. Этот этап почти полностью автоматизирован с помощью databricks-cli, если только вам не нужно перенести конфигурации кластеров выборочно.

   > [!NOTE]
   > К сожалению, конечной точки для создания конфигураций кластеров не существует, а этот скрипт сразу пытается создать каждый кластер. Если в вашей подписке недостаточно ядер, создание кластера может завершиться неудачей. Сбой можно игнорировать, если конфигурация перенесена успешно.

   Приведенный ниже скрипт выводит таблицу соответствия старых идентификаторов кластеров новым. Ее можно использовать позже для переноса заданий (настроенных на использование имеющихся кластеров).

   Скопируйте и сохраните приведенный ниже скрипт python в файл и запустите его в командной строке Databricks. Например, `python scriptname.py`.

   ```python
   from subprocess import call, check_output
   import json, os

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Get all clusters info from old workspace
   clusters_out = check_output(["databricks", "clusters", "list", "--profile", EXPORT_PROFILE])
   clusters_info_list = clusters_out.splitlines()

   # Create a list of all cluster ids
   clusters_list = []
   ##for cluster_info in clusters_info_list: clusters_list.append(cluster_info.split(None, 1)[0])

   for cluster_info in clusters_info_list: 
      if cluster_info != '':
         clusters_list.append(cluster_info.split(None, 1)[0])

   # Optionally filter cluster ids out manually, so as to create only required ones in new workspace

   # Create a list of mandatory / optional create request elements
   cluster_req_elems = ["num_workers","autoscale","cluster_name","spark_version","spark_conf","node_type_id","driver_node_type_id","custom_tags","cluster_log_conf","spark_env_vars","autotermination_minutes","enable_elastic_disk"]

   print(str(len(clusters_list)) + " clusters found in the primary site" )

   print ("---------------------------------------------------------")
   # Try creating all / selected clusters in new workspace with same config as in old one.
   cluster_old_new_mappings = {}
   i = 0
   for cluster in clusters_list:
      i += 1
      print("Checking cluster " + str(i) + "/" + str(len(clusters_list)) + " : " + cluster)
      cluster_get_out = check_output(["databricks", "clusters", "get", "--cluster-id", cluster, "--profile", EXPORT_PROFILE])
      print ("Got cluster config from old workspace")

       # Remove extra content from the config, as we need to build create request with allowed elements only
      cluster_req_json = json.loads(cluster_get_out)
      cluster_json_keys = cluster_req_json.keys()

      #Don't migrate Job clusters
      if cluster_req_json['cluster_source'] == u'JOB' : 
         print ("Skipping this cluster as it is a Job cluster : " + cluster_req_json['cluster_id'] )
         print ("---------------------------------------------------------")
         continue

      for key in cluster_json_keys:
         if key not in cluster_req_elems:
            cluster_req_json.pop(key, None)

      # Create the cluster, and store the mapping from old to new cluster ids

      #Create a temp file to store the current cluster info as JSON
      strCurrentClusterFile = "tmp_cluster_info.json" 

      #delete the temp file if exists
      if os.path.exists(strCurrentClusterFile) : 
         os.remove(strCurrentClusterFile)

      fClusterJSONtmp = open(strCurrentClusterFile,"w+")
      fClusterJSONtmp.write(json.dumps(cluster_req_json))
      fClusterJSONtmp.close()

      #cluster_create_out = check_output(["databricks", "clusters", "create", "--json", json.dumps(cluster_req_json), "--profile", IMPORT_PROFILE])
      cluster_create_out = check_output(["databricks", "clusters", "create", "--json-file", strCurrentClusterFile , "--profile", IMPORT_PROFILE])
      cluster_create_out_json = json.loads(cluster_create_out)
      cluster_old_new_mappings[cluster] = cluster_create_out_json['cluster_id']

      print ("Cluster create request sent to secondary site workspace successfully")
      print ("---------------------------------------------------------")

      #delete the temp file if exists
      if os.path.exists(strCurrentClusterFile) : 
         os.remove(strCurrentClusterFile)

   print ("Cluster mappings: " + json.dumps(cluster_old_new_mappings))
   print ("All done")
   print ("P.S. : Please note that all the new clusters in your secondary site are being started now!")
   print ("       If you won't use those new clusters at the moment, please don't forget terminating your new clusters to avoid charges")
   ```

6. **Перенос конфигурации заданий**

   Если вы перенесли конфигурации кластеров на предыдущем этапе, вы можете перенести конфигурации заданий. Этот этап полностью автоматизирован с помощью databricks-cli, если только вам не нужно перенести конфигурации заданий выборочно.

   > [!NOTE]
   > Конфигурация запланированного задания также содержит сведения о расписании, поэтому по умолчанию эта конфигурация начнет работу в назначенное время после миграции. С помощью приведенного ниже блока кода можно удалить все сведения о расписании во время миграции (во избежание повторного выполнения в старых и новых рабочих областях). Настраивайте расписание для таких заданий, когда вы будете готовы к прямой миграции.

   Конфигурация заданий должна включать параметры для нового или имеющегося кластера. Если используется имеющийся кластер, приведенный ниже скрипт попытается заменить старый идентификатор кластера на новый.

   Скопируйте и сохраните приведенный ниже скрипт python в файл. Замените значения параметров `old_cluster_id` и `new_cluster_id` на выходные данные, полученные в результате переноса кластеров на предыдущем этапе. Запустите его из командной строки databricks-cli. Пример: `python scriptname.py`.

   ```python
   from subprocess import call, check_output
   import json

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Please replace the old to new cluster id mappings from cluster migration output
   cluster_old_new_mappings = {"old_cluster_id": "new_cluster_id"}

   # Get all jobs info from old workspace
   try:
     jobs_out = check_output(["databricks", "jobs", "list", "--profile", EXPORT_PROFILE])
     jobs_info_list = jobs_out.splitlines()
   except:
     print "No jobs to migrate"
     sys.exit(0)

   # Create a list of all job ids
   jobs_list = []
   for jobs_info in jobs_info_list:
     jobs_list.append(jobs_info.split(None, 1)[0])

   # Optionally filter job ids out manually, so as to create only required ones in new workspace

   # Create each job in the new workspace based on corresponding settings in the old workspace

   for job in jobs_list:
     print "Trying to migrate " + job

     job_get_out = check_output(["databricks", "jobs", "get", "--job-id", job, "--profile", EXPORT_PROFILE])
     print "Got job config from old workspace"

     job_req_json = json.loads(job_get_out)  
     job_req_settings_json = job_req_json['settings']

     # Remove schedule information so job doesn't start before proper cutover
     job_req_settings_json.pop('schedule', None)

     # Replace old cluster id with new cluster id, if job configured to run against an existing cluster
     if 'existing_cluster_id' in job_req_settings_json:
       if job_req_settings_json['existing_cluster_id'] in cluster_old_new_mappings:
         job_req_settings_json['existing_cluster_id'] = cluster_old_new_mappings[job_req_settings_json['existing_cluster_id']]
       else:
         print "Mapping not available for old cluster id " + job_req_settings_json['existing_cluster_id']
         continue

     call(["databricks", "jobs", "create", "--json", json.dumps(job_req_settings_json), "--profile", IMPORT_PROFILE])
     print "Sent job create request to new workspace successfully"

   print "All done"
   ```

7. **Перенос библиотек**

   В настоящее время не существует простого способа перенести библиотеки из одной рабочей области в другую. Вместо этого библиотеки следует устанавливать в новой рабочей области вручную. Эту операцию можно автоматизировать, используя [интерфейс командной строки DBFS](https://github.com/databricks/databricks-cli#dbfs-cli-examples) (для отправки пользовательских библиотек в рабочую область) и [интерфейс командной строки библиотек](https://github.com/databricks/databricks-cli#libraries-cli).

8. **Миграция хранилища BLOB-объектов Azure и Azure Data Lake Storage подключений**

   Вручную Подключите все точки подключения [хранилища BLOB-объектов Azure](/azure/databricks/data/data-sources/azure/azure-storage) и [Azure Data Lake Storage (Gen 2)](/azure/databricks/data/data-sources/azure/azure-datalake-gen2) с помощью решения на основе записной книжки. Ресурсы хранилища были бы подключены в основной рабочей области, и это следует повторить в дополнительной рабочей области. Внешних API для подключений не существует.

9. **Перенос скриптов инициализации кластеров**

   Все скрипты инициализации кластеров можно перенести из старой рабочей области в новую с помощью [интерфейса командной строки DBFS](https://github.com/databricks/databricks-cli#dbfs-cli-examples). Для начала скопируйте необходимые скрипты из каталога `dbfs:/dat abricks/init/..` на локальный компьютер или виртуальную машину. Затем скопируйте эти скрипты в новую рабочую область, находящуюся по тому же пути.

   ```bash
   // Primary to local
   dbfs cp -r dbfs:/databricks/init ./old-ws-init-scripts --profile primary

   // Local to Secondary workspace
   dbfs cp -r old-ws-init-scripts dbfs:/databricks/init --profile secondary
   ```

10. **Повторная настройка и применение управления доступом вручную**

    Если имеющаяся основная рабочая область настроена на использование уровня (SKU) "Премиум", то наверняка используется [функция управления доступом](/azure/databricks/administration-guide/access-control/index).

    Если функция управления доступом не используется, вручную примените управление доступом к ресурсам (записным книжкам, кластерам, заданиям, таблицам).

## <a name="disaster-recovery-for-your-azure-ecosystem"></a>Аварийное восстановление для экосистемы Azure

Если вы используете другие службы Azure, обязательно реализуйте рекомендации по аварийному восстановлению для этих служб. Например, если вы решили использовать внешний экземпляр хранилище метаданных Hive, следует рассмотреть возможность аварийного восстановления для [SQL Server Azure](../sql-database/sql-database-disaster-recovery.md), [Azure HDInsight](../hdinsight/hdinsight-high-availability-linux.md)и (или) [базы данных Azure для MySQL](../mysql/concepts-business-continuity.md). Общие сведения об аварийном восстановлении см. в статье [Аварийное восстановление для приложений Azure](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в [документации по Azure Databricks](index.yml).