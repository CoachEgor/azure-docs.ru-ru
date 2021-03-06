---
title: Среда службы приложений сети
description: Среда службы приложений сведения о сети
author: ccompy
ms.assetid: 6f262f63-aef5-4598-88d2-2f2c2f2bfc24
ms.topic: article
ms.date: 11/16/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 61059c3e0f9737df6ace338f4252a338ea1f200c
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "94663847"
---
# <a name="app-service-environment-networking"></a>Среда службы приложений сети

> [!NOTE]
> Эта статья относится к Среда службы приложений v3 (Предварительная версия)
> 

Среда службы приложений (ASE) — это одно развертывание клиента службы приложений Azure, в которой размещаются веб-приложения, приложения API и приложения функций. При установке ASE вы выбираете виртуальную сеть Azure, в которой она должна быть развернута. Все входящие и исходящие трафикные приложения будут находиться в указанной виртуальной сети.  

ASEv3 использует две подсети.  Одна подсеть используется для частной конечной точки, которая обрабатывает входящий трафик. Это может быть уже существующая подсеть или новая.  Входящая подсеть может использоваться для других целей, чем частная конечная точка. Исходящая подсеть может использоваться только для исходящего трафика из ASE. В исходящей подсети ASE ничего не может попасть.

## <a name="addresses"></a>Адреса 
ASE имеет следующие адреса при создании:

| Тип адреса | description; |
|--------------|-------------|
| Входящий адрес | Входящий адрес — это адрес частной конечной точки, используемый вашим ASE. |
| Исходящая подсеть | Исходящая подсеть также является подсетью ASE. Во время просмотра эта подсеть используется только для исходящего трафика. |
| Исходящий адрес Windows | Приложения Windows в этом ASE будут использовать этот адрес по умолчанию при выполнении исходящих вызовов через Интернет. |
| Исходящий адрес Linux | Приложения Linux в этом ASE будут использовать этот адрес по умолчанию при выполнении исходящих вызовов через Интернет. |

Если удалить закрытую конечную точку, используемую ASE, вы не сможете связаться с приложениями в ASE. Не удаляйте Azure DNS закрытую зону, связанную с вашим ASE.  

ASE использует адреса в исходящей подсети для поддержки инфраструктуры, используемой ASE. При масштабировании планов службы приложений в ASE вы будете использовать больше адресов. Приложения в ASE не имеют выделенных адресов в исходящей подсети. Адреса, используемые приложением в исходящей подсети, будут изменяться со временем.

## <a name="ports"></a>Порты

ASE получает трафик приложения на портах 80 и 443.  Отсутствуют требования к входящему или исходящему порту для ASE. 

## <a name="extra-configurations"></a>Дополнительные конфигурации

В отличие от ASEv2, при использовании ASEv3 можно задать группы безопасности сети (группы безопасности сети) и таблицы маршрутов (определяемые пользователем маршруты), которые будут отображаться без ограничений. Если вы хотите принудительно создать туннель для всего исходящего трафика от ASE на устройство NVA. Устройства WAF можно разместить перед всем входящим трафиком в ASE. 

## <a name="dns"></a>DNS

Приложения в ASE будут использовать DNS, с которым настроена виртуальная сеть. Если требуется, чтобы некоторые приложения использовали другой DNS-сервер, можно вручную задать его для каждого приложения с помощью параметров приложения WEBSITE_DNS_SERVER и WEBSITE_DNS_ALT_SERVER. Параметр приложения WEBSITE_DNS_ALT_SERVER настраивает дополнительный DNS-сервер. Дополнительный DNS-сервер используется только при отсутствии ответа от основного DNS-сервера. 

## <a name="preview-limitation"></a>Ограничение предварительной версии

Существует несколько сетевых функций, которые недоступны в ASEv3.  В ASEv3 отсутствуют следующие вещи:

• FTP • удаленная отладка • развертывание внешнего балансировщика нагрузки • возможность доступа к закрытому реестру контейнеров для развертываний контейнеров • возможность выполнять вызовы глобально-одноранговой виртуальных сетей • возможность резервного копирования и восстановления с помощью конечной точки службы или защищенного хранилища частной конечной точки. учетная запись • возможность иметь параметры приложения, keyvault ссылки на конечные точки службы или закрытые учетные записи keyvault. • возможность использовать BYOS в конечной точке службы или защищенной учетной записи хранения в частной конечной точке • использование наблюдателя за сетями или NSG потоком исходящего трафика.
    
    

