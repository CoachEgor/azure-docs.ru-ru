---
ms.openlocfilehash: 9a5a2d92f70c411c46ebb4efb35e17e9b0c477ca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60542353"
---
1. Установите dapl, rdmacm, ibverbs и mlx4.

   ```bash
   sudo apt-get update

   sudo apt-get install libdapl2 libmlx4-1
   ```

2. В /etc/waagent.conf включите RDMA, раскомментировав следующие строки конфигурации. Для изменения этого файла требуется доступ с правами root.

   ```
   OS.EnableRDMA=y

   OS.UpdateRdmaDriver=y
   ```

3. Добавьте или измените следующие параметры памяти в КБ в файле /etc/security/limits.conf. Для изменения этого файла требуется доступ с правами root. В целях тестирования можно также задать неограниченное значение для параметра memlock. Например, `<User or group name>   hard    memlock   unlimited`.

   ```
   <User or group name> hard    memlock <memory required for your application in KB>

   <User or group name> soft    memlock <memory required for your application in KB>
   ```

4. Установите библиотеку Intel MPI [Купите и скачайте](https://software.intel.com/intel-mpi-library/) библиотеку с сайта Intel или скачайте [бесплатную пробную версию](https://registrationcenter.intel.com/en/forms/?productid=1740).

   ```bash
   wget http://registrationcenter-download.intel.com/akdlm/irc_nas/tec/9278/l_mpi_p_5.1.3.223.tgz
   ```

   Поддерживаются только среды выполнения Intel MPI 5.x.

   Шаги установки см. в [руководстве по установке библиотеки Intel MPI](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html).

5. Включите ptrace для некорневых процессов без отладчика (требуется для последних версий Intel MPI).

   ```bash
   echo 0 | sudo tee /proc/sys/kernel/yama/ptrace_scope
   ```
   <!--ms.date: 07/30/2018-->