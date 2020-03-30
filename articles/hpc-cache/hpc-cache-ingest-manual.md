---
title: Лазурный HPC Кэш данных глотать - ручная копия
description: Как использовать команды cp для перемещения данных в цель хранения Blob в кэше HPC Azure
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: fc397088e46f0d2b623080f3deed24c386e7d8b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "74168481"
---
# <a name="azure-hpc-cache-data-ingest---manual-copy-method"></a>Лазурный HPC кэш данных глотать - ручной метод копирования

В этой статье приводятся подробные инструкции по ручному копированию данных в контейнер для хранения данных Blob для использования с Azure HPC Cache. Для оптимизации скорости копирования используется многопоточные параллельные операции.

Подробнее о перемещении данных в хранилище Blob для кэша Azure HPC читайте в [материале «Перемещение данных в хранилище Azure Blob».](hpc-cache-ingest.md)

## <a name="simple-copy-example"></a>Простой пример копирования

Можно вручную создать многопоточные операции копирования на клиентском компьютере, запустив более одной команды копирования одновременно в фоновом режиме для предопределенных наборов файлов или путей.

Команда Linux/UNIX ``cp`` содержит аргумент ``-p`` для сохранения прав владения и метаданных времени. Добавление этого аргумента в приведенные ниже команды является необязательным. (Добавление аргумента увеличивает количество вызовов файловой системы, отправляемых от клиента к файловой системе назначения для модификации метаданных.)

В этом простом примере два файла копируются в параллельном режиме:

```bash
cp /mnt/source/file1 /mnt/destination1/ & cp /mnt/source/file2 /mnt/destination1/ &
```

После запуска этой команды команда `jobs` покажет, что выполняются два потока.

## <a name="copy-data-with-predictable-file-names"></a>Копирование данных с предсказуемыми именами файлов

Если имена файлов предсказуемы, можно использовать выражения для создания параллельных потоков копирования. 

Например, если в каталоге содержится 1000 файлов, пронумерованных последовательно от `0001` до `1000`, можно использовать следующие выражения для создания десяти параллельных потоков, каждый из которых копирует 100 файлов:

```bash
cp /mnt/source/file0* /mnt/destination1/ & \
cp /mnt/source/file1* /mnt/destination1/ & \
cp /mnt/source/file2* /mnt/destination1/ & \
cp /mnt/source/file3* /mnt/destination1/ & \
cp /mnt/source/file4* /mnt/destination1/ & \
cp /mnt/source/file5* /mnt/destination1/ & \
cp /mnt/source/file6* /mnt/destination1/ & \
cp /mnt/source/file7* /mnt/destination1/ & \
cp /mnt/source/file8* /mnt/destination1/ & \
cp /mnt/source/file9* /mnt/destination1/
```

## <a name="copy-data-with-unstructured-file-names"></a>Копирование данных с неструктурированными именами файлов

Если структура именования файлов не предсказуема, можно сгруппировать файлы по именам каталогов. 

В этом примере осуществляется сбор целых каталогов для отправки в команды ``cp``, выполняемые в качестве фоновых задач.

```bash
/root
|-/dir1
| |-/dir1a
| |-/dir1b
| |-/dir1c
   |-/dir1c1
|-/dir1d
```

После сбора файлов можно выполнить команды параллельного копирования, чтобы рекурсивно скопировать подкаталоги и все их содержимое:

```bash
cp /mnt/source/* /mnt/destination/
mkdir -p /mnt/destination/dir1 && cp /mnt/source/dir1/* mnt/destination/dir1/ & 
cp -R /mnt/source/dir1/dir1a /mnt/destination/dir1/ & 
cp -R /mnt/source/dir1/dir1b /mnt/destination/dir1/ & 
cp -R /mnt/source/dir1/dir1c /mnt/destination/dir1/ & # this command copies dir1c1 via recursion
cp -R /mnt/source/dir1/dir1d /mnt/destination/dir1/ &
```

## <a name="when-to-add-mount-points"></a>Когда добавлять точки подключения

После того, как у вас есть достаточно параллельных потоков происходит против одной точки крепления файловой системы назначения, будет точка, где добавление большего количества потоков не дает больше пропускной воды. (Пропуск будет измеряться в файлах/секундах или байтах в секунду, в зависимости от типа данных.) Или, что еще хуже, чрезмерное резьба иногда может привести к деградации пропускной связи.  

Когда это происходит, можно добавить точки крепления на стороне клиента к другим адресам крепления Azure HPC, используя тот же путь крепления удаленной файловой системы:

```bash
10.1.0.100:/nfs on /mnt/sourcetype nfs (rw,vers=3,proto=tcp,addr=10.1.0.100)
10.1.1.101:/nfs on /mnt/destination1type nfs (rw,vers=3,proto=tcp,addr=10.1.1.101)
10.1.1.102:/nfs on /mnt/destination2type nfs (rw,vers=3,proto=tcp,addr=10.1.1.102)
10.1.1.103:/nfs on /mnt/destination3type nfs (rw,vers=3,proto=tcp,addr=10.1.1.103)
```

Добавление точек подключения на стороне клиента позволяет разделить дополнительные команды копирования на дополнительные точки подключения `/mnt/destination[1-3]`, что позволяет повысить уровень параллелизма.  

Например, если файлы очень велики, можно определить для команд копирования использование различных путей назначения, параллельно отправив дополнительные команды из клиента, выполняющего копирование.

```bash
cp /mnt/source/file0* /mnt/destination1/ & \
cp /mnt/source/file1* /mnt/destination2/ & \
cp /mnt/source/file2* /mnt/destination3/ & \
cp /mnt/source/file3* /mnt/destination1/ & \
cp /mnt/source/file4* /mnt/destination2/ & \
cp /mnt/source/file5* /mnt/destination3/ & \
cp /mnt/source/file6* /mnt/destination1/ & \
cp /mnt/source/file7* /mnt/destination2/ & \
cp /mnt/source/file8* /mnt/destination3/ & \
```

В приведенном выше примере процессы копирования файлов клиента направлены на все три целевые точки подключения.

## <a name="when-to-add-clients"></a>Когда следует добавлять клиенты

Наконец, когда вы достигли предела возможностей клиента, добавление потоков копирования или точек подключения не даст дополнительного увеличения числа файлов в секунду или байт в секунду. В этой ситуации можно развернуть другой клиент с тем же набором точек подключения, для которых будут выполняться собственные наборы процессов копирования файлов. 

Пример

```bash
Client1: cp -R /mnt/source/dir1/dir1a /mnt/destination/dir1/ &
Client1: cp -R /mnt/source/dir2/dir2a /mnt/destination/dir2/ &
Client1: cp -R /mnt/source/dir3/dir3a /mnt/destination/dir3/ &

Client2: cp -R /mnt/source/dir1/dir1b /mnt/destination/dir1/ &
Client2: cp -R /mnt/source/dir2/dir2b /mnt/destination/dir2/ &
Client2: cp -R /mnt/source/dir3/dir3b /mnt/destination/dir3/ &

Client3: cp -R /mnt/source/dir1/dir1c /mnt/destination/dir1/ &
Client3: cp -R /mnt/source/dir2/dir2c /mnt/destination/dir2/ &
Client3: cp -R /mnt/source/dir3/dir3c /mnt/destination/dir3/ &

Client4: cp -R /mnt/source/dir1/dir1d /mnt/destination/dir1/ &
Client4: cp -R /mnt/source/dir2/dir2d /mnt/destination/dir2/ &
Client4: cp -R /mnt/source/dir3/dir3d /mnt/destination/dir3/ &
```

## <a name="create-file-manifests"></a>Создание манифестов файлов

После понимания подходов выше (несколько копий потоков на один пункт назначения, несколько направлений на клиента, несколько клиентов на сеть доступных исходных файлов системы), рассмотреть эту рекомендацию: Сборка файла манифестов, а затем использовать их с копией команды между несколькими клиентами.

В этом сценарии для создания манифестов файлов или каталогов используется команда UNIX ``find``:

```bash
user@build:/mnt/source > find . -mindepth 4 -maxdepth 4 -type d
./atj5b55c53be6-01/support/gsi/2018-07-22T21:12:06EDT
./atj5b55c53be6-01/support/pcap/2018-07-23T01:34:57UTC
./atj5b55c53be6-01/support/trace/rolling
./atj5b55c53be6-03/support/gsi/2018-07-22T21:12:06EDT
./atj5b55c53be6-03/support/pcap/2018-07-23T01:34:57UTC
./atj5b55c53be6-03/support/trace/rolling
./atj5b55c53be6-02/support/gsi/2018-07-22T21:12:06EDT
./atj5b55c53be6-02/support/pcap/2018-07-23T01:34:57UTC
./atj5b55c53be6-02/support/trace/rolling
```

Перенаправьте этот результат в файл: `find . -mindepth 4 -maxdepth 4 -type d > /tmp/foo`

Затем можно выполнить итерацию по манифесту, используя команды BASH для подсчета файлов и определения размеров подкаталогов:

```bash
ben@xlcycl1:/sps/internal/atj5b5ab44b7f > for i in $(cat /tmp/foo); do echo " `find ${i} |wc -l`    `du -sh ${i}`"; done
244    3.5M    ./atj5b5ab44b7f-02/support/gsi/2018-07-18T00:07:03EDT
9      172K    ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-18T05:01:00UTC
124    5.8M    ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-19T01:01:01UTC
152    15M     ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-20T01:01:00UTC
131    13M     ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-20T21:59:41UTC_partial
789    6.2M    ./atj5b5ab44b7f-02/support/gsi/2018-07-20T21:59:41UTC
134    12M     ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-20T22:22:55UTC_hpccache_catchup
7      16K     ./atj5b5ab44b7f-02/support/pcap/2018-07-18T17:12:19UTC
8      83K     ./atj5b5ab44b7f-02/support/pcap/2018-07-18T17:17:17UTC
575    7.7M    ./atj5b5ab44b7f-02/support/cores/armada_main.2000.1531980253.gsi
33     4.4G    ./atj5b5ab44b7f-02/support/trace/rolling
281    6.6M    ./atj5b5ab44b7f-01/support/gsi/2018-07-18T00:07:03EDT
15     182K    ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-18T05:01:00UTC
244    17M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-19T01:01:01UTC
299    31M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-20T01:01:00UTC
256    29M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-20T21:59:41UTC_partial
889    7.7M    ./atj5b5ab44b7f-01/support/gsi/2018-07-20T21:59:41UTC
262    29M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-20T22:22:55UTC_hpccache_catchup
11     248K    ./atj5b5ab44b7f-01/support/pcap/2018-07-18T17:12:19UTC
11     88K     ./atj5b5ab44b7f-01/support/pcap/2018-07-18T17:17:17UTC
645    11M     ./atj5b5ab44b7f-01/support/cores/armada_main.2019.1531980253.gsi
33     4.0G    ./atj5b5ab44b7f-01/support/trace/rolling
244    2.1M    ./atj5b5ab44b7f-03/support/gsi/2018-07-18T00:07:03EDT
9      158K    ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-18T05:01:00UTC
124    5.3M    ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-19T01:01:01UTC
152    15M     ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-20T01:01:00UTC
131    12M     ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-20T21:59:41UTC_partial
789    8.4M    ./atj5b5ab44b7f-03/support/gsi/2018-07-20T21:59:41UTC
134    14M     ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-20T22:25:58UTC_hpccache_catchup
7      159K    ./atj5b5ab44b7f-03/support/pcap/2018-07-18T17:12:19UTC
7      157K    ./atj5b5ab44b7f-03/support/pcap/2018-07-18T17:17:17UTC
576    12M     ./atj5b5ab44b7f-03/support/cores/armada_main.2013.1531980253.gsi
33     2.8G    ./atj5b5ab44b7f-03/support/trace/rolling
```

Наконец, вы должны создавать фактические команды копирования файлов для клиентов.  

При наличии четырех клиентов используйте следующую команду:

```bash
for i in 1 2 3 4 ; do sed -n ${i}~4p /tmp/foo > /tmp/client${i}; done
```

Если у вас есть пять клиентов, используйте примерно следующее:

```bash
for i in 1 2 3 4 5; do sed -n ${i}~5p /tmp/foo > /tmp/client${i}; done
```

И за шесть .... Экстраполировать по мере необходимости.

```bash
for i in 1 2 3 4 5 6; do sed -n ${i}~6p /tmp/foo > /tmp/client${i}; done
```

Вы получите *N* файлов (по одному для каждого из ваших *N* клиентов), у которых есть пути к каталогам 4-го уровня, полученные как часть выходных данных команды `find`. 

Используйте каждый файл для создания команды копирования:

```bash
for i in 1 2 3 4 5 6; do for j in $(cat /tmp/client${i}); do echo "cp -p -R /mnt/source/${j} /mnt/destination/${j}" >> /tmp/client${i}_copy_commands ; done; done
```

Вы получите *N* файлов (каждый с командой копирования в строке), которые могут быть запущены в качестве сценария BASH в клиенте. 

Целью является параллельное выполнение нескольких потоков этих сценариев в каждом клиенте (одновременно в нескольких клиентах).
