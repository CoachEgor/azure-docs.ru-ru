---
title: Просмотр состояния задания инструмента импорта и экспорта Azure версии 1 | Документация Майкрософт
description: Узнайте, как использовать файлы журнала, созданные при запуске задания импорта или экспорта, для просмотра состояния задания импорта и экспорта.
author: twooley
services: storage
ms.service: storage
ms.topic: how-to
ms.date: 01/26/2017
ms.author: twooley
ms.subservice: common
ms.openlocfilehash: 5ab0dd7f0e0ed90c205c37ddbb7ea3a9fca74ae9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "85512267"
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>Просмотр состояния задания импорта и экспорта Azure с помощью файлов журнала копирования
При обработке дисков, связанных с заданием импорта или экспорта, служба импорта и экспорта Microsoft Azure записывает файлы журнала копирования в учетную запись хранения, в которую импортируются большие двоичные объекты или из которой они экспортируются. Файл журнала содержит подробные сведения о состоянии каждого импортируемого или экспортируемого файла. При запросе состояния завершенного задания возвращается URL-адрес для каждого файла журнала копирования. Дополнительные сведения см. [здесь](https://docs.microsoft.com/rest/api/storageimportexport/Jobs/Get).  

## <a name="example-urls"></a>Примеры URL-адресов

Ниже приведены примеры URL-адресов файлов журнала копирования для задания импорта с двумя дисками.  

 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  

 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  

 Дополнительные сведения о формате журналов копирования и полный список кодов состояний см. в статье [Формат файла журнала службы импорта и экспорта Azure](../storage-import-export-file-format-log.md).  

## <a name="next-steps"></a>Дальнейшие шаги

 * [Настройка средства импорта и экспорта Azure](storage-import-export-tool-setup-v1.md)   
 * [Подготовка жестких дисков для задания импорта](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [Подготовка задания импорта](../storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [Подготовка задания экспорта](../storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [Устранение неполадок со средством импорта и экспорта Azure](storage-import-export-tool-troubleshooting-v1.md)
