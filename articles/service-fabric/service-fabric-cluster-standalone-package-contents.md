---
title: Автономный пакет Azure Service Fabric для Windows Server
description: Описание изолированного пакета Azure Service Fabric для Windows Server и его содержимого.
author: maburlik
ms.topic: conceptual
ms.date: 8/10/2017
ms.author: maburlik
ms.openlocfilehash: 1bb73fa69717f067139067f127a0d50af4878d6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451847"
---
# <a name="contents-of-service-fabric-standalone-package-for-windows-server"></a>Содержимое изолированного пакета Service Fabric для Windows Server
В [скачанном](https://go.microsoft.com/fwlink/?LinkId=730690) изолированном пакете Service Fabric для Windows Server вы найдете приведенные ниже файлы.

| **Имя файла** | **Краткое описание** |
| --- | --- |
| CreateServiceFabricCluster.ps1 |Сценарий PowerShell, который создает кластер, используя параметры из файла ClusterConfig.json. |
| RemoveServiceFabricCluster.ps1 |Сценарий PowerShell, который удаляет кластер, используя параметры из файла ClusterConfig.json. |
| AddNode.ps1 |Сценарий PowerShell, который добавляет узел в имеющийся развернутый кластер на текущий компьютер. |
| RemoveNode.ps1 |Сценарий PowerShell, который удаляет узел имеющегося развернутого кластера с текущего компьютера. |
| CleanFabric.ps1 |Сценарий PowerShell для удаления изолированной установки Service Fabric с текущего компьютера. Предыдущие установки MSI должны быть удалены с помощью собственной связанной программы удаления. |
| TestConfiguration.ps1 |Сценарий PowerShell для анализа инфраструктуры в соответствии с указаниями в cluster.JSON. |
| DownloadServiceFabricRuntimePackage.ps1 |Сценарий PowerShell, используемый для скачивания последней версии внешнего пакета среды выполнения при развертывании на компьютер без подключения к Интернету. |
| DeploymentComponentsAutoextractor.exe |Самораспаковывающийся архив, содержащий компоненты развертывания для сценариев использования изолированного пакета. |
| EULA_ENU.txt |Условия лицензии на использование изолированного пакета Windows Server Microsoft Azure Service Fabric. Вы можете [загрузить копию лицензионного соглашения](https://go.microsoft.com/fwlink/?LinkID=733084). |
| Readme.txt |Ссылка на заметки о выпуске и основные инструкции по установке. В этом документе находится сокращенный набор инструкций. |
| ThirdPartyNotice.rtf |Примечание о включенном в пакет программном обеспечении сторонних производителей. |
| Tools\Microsoft.Azure.ServiceFabric.WindowsServer.SupportPackage.zip |StandaloneLogCollector.exe — запускается по требованию для сбора и отправки журналов трассировки в корпорацию Майкрософт для поддержки. |
| Tools\ServiceFabricUpdateService.zip |Средство, используемое для включения автоматического обновления кода для кластеров, которые не имеют доступа к Интернету. Дополнительные сведения см. [здесь](service-fabric-cluster-upgrade-windows-server.md).|

**Шаблоны** 

| **Имя файла** | **Краткое описание** |
| --- | --- |
| ClusterConfig.Unsecure.DevCluster.json |Пример файла конфигурации кластера, содержащего параметры незащищенного кластера разработки с тремя узлами и одной виртуальной машиной или компьютером, в том числе сведения о каждом узле, который является частью кластера. |
| ClusterConfig.Unsecure.MultiMachine.json |Пример файла конфигурации кластера, содержащего параметры незащищенного кластера с несколькими виртуальными машинами или компьютерами, в том числе сведения о каждом компьютере, который находится в кластере. |
| ClusterConfig.Windows.DevCluster.json |Пример файла конфигурации кластера, содержащего параметры защищенного кластера разработки с тремя узлами и одной виртуальной машиной или компьютером, в том числе сведения о каждом узле, который является частью кластера. Этот кластер защищен с помощью [удостоверений Windows](https://msdn.microsoft.com/library/ff649396.aspx). |
| ClusterConfig.Windows.MultiMachine.json |Пример файла конфигурации кластера, который содержит все параметры кластера, защищенного с помощью системы безопасности Windows, с несколькими виртуальными машинами или компьютерами, в том числе сведения о каждом компьютере, который находится в защищенном кластере. Этот кластер защищен с помощью [удостоверений Windows](https://msdn.microsoft.com/library/ff649396.aspx). |
| ClusterConfig.x509.DevCluster.json |Пример файла конфигурации кластера, содержащего параметры защищенного кластера разработки с тремя узлами и одной виртуальной машиной или компьютером, в том числе сведения о каждом узле, который является частью кластера. Этот кластер защищен с помощью сертификатов x.509. |
| ClusterConfig.x509.MultiMachine.json |Пример файла конфигурации кластера, содержащего все параметры защищенного кластера разработки с несколькими виртуальными машинами или компьютерами, в том числе сведения о каждом узле, который находится в защищенном кластере. Этот кластер защищен с помощью сертификатов x.509. |
| ClusterConfig.gMSA.Windows.MultiMachine.json |Пример файла конфигурации кластера, содержащего все параметры защищенного кластера разработки с несколькими виртуальными машинами или компьютерами, в том числе сведения о каждом узле, который находится в защищенном кластере. Кластер защищается с помощью [групповых управляемых учетных записей служб](https://technet.microsoft.com/library/jj128431(v=ws.11).aspx). |

## <a name="cluster-configuration-samples"></a>Примеры конфигурации кластера
Последние версии шаблонов конфигурации кластеров можно найти на странице GitHub: [Автономные образцы конфигурации кластера.](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

## <a name="independent-runtime-package"></a>Независимый пакет среды выполнения
Последняя версия пакета среды выполнения автоматически загружается во время развертывания кластера по [ссылке для скачивания среды выполнения Service Fabric для Windows Server](https://go.microsoft.com/fwlink/?linkid=839354).

## <a name="related"></a>Сопутствующие материалы
* [Создание изолированного кластера Azure Service Fabric](service-fabric-cluster-creation-for-windows-server.md)
* [Сценарии защиты кластера Service Fabric](service-fabric-windows-cluster-windows-security.md)
