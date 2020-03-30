---
title: Устранение распространенных неполадок
description: Узнайте, как устранить общие проблемы при развертывании, запуске или управлении экземплярами контейнеров Azure
ms.topic: article
ms.date: 09/25/2019
ms.custom: mvc
ms.openlocfilehash: 07cdbfb27aaf9076e726ebda861ed24996e10135
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "74533398"
---
# <a name="troubleshoot-common-issues-in-azure-container-instances"></a>Устранение распространенных неполадок с помощью службы "Экземпляры контейнеров Azure"

В этой статье показано, как устранять распространенные неполадки при развертывании контейнеров и управлении ими в службе "Экземпляры контейнеров Azure". Смотрите также [Часто задаваемые вопросы](container-instances-faq.md).

Если вам нужна дополнительная поддержка, ознакомьтесь с доступными опциями **поддержки Справки и поддержки** на [портале Azure.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)

## <a name="issues-during-container-group-deployment"></a>Проблемы при развертывании Контейнерной группы
### <a name="naming-conventions"></a>Соглашения об именовании.

При определении спецификации контейнера некоторые параметры требуют соблюдения ограничений на именование. Ниже приведена таблица особых требований для свойств группы контейнеров. Дополнительные сведения о соглашениях об именовании Azure см. в разделе [Правила именования и ограничения][azure-name-restrictions] в центре архитектуры Azure.

| Область | Длина | Регистр | Допустимые знаки | Рекомендуемый шаблон | Пример |
| --- | --- | --- | --- | --- | --- |
| Имя группы контейнеров | От 1 до 64 |Без учета регистра |Буквы, цифры, дефис в любом месте, за исключением первого знака |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Имя контейнера | От 1 до 64 |Без учета регистра |Буквы, цифры, дефис в любом месте, за исключением первого знака |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Порты контейнера | От 1 до 65 535 |Целое число |Целое число от 1 до 65 535. |`<port-number>` |`443` |
| Метка имени DNS | 5–63 |Без учета регистра |Буквы, цифры, дефис в любом месте, за исключением первого знака |`<name>` |`frontend-site1` |
| Переменная среды | 1–63 |Без учета регистра |Буквы, цифры и подчеркивание (_) в любом месте, за исключением первого знака |`<name>` |`MY_VARIABLE` |
| Имя тома | 5–63 |Без учета регистра |Буквы в нижнем регистре, цифры и дефисы в любом месте, кроме первого или последнего знака. Не может содержать два дефиса подряд. |`<name>` |`batch-output-volume` |

### <a name="os-version-of-image-not-supported"></a>Версия ОС образа не поддерживается

Если вы укажете образ, не поддерживаемый службой "Экземпляры контейнеров Azure", будет возвращена ошибка `OsVersionNotSupported`. Ошибка похожа на следующую, где `{0}` — это имя образа, который вы пытались развернуть:

```json
{
  "error": {
    "code": "OsVersionNotSupported",
    "message": "The OS version of image '{0}' is not supported."
  }
}
```

Эта ошибка чаще всего встречается при развертывании изображений Windows, которые основаны на полугодовом выпуске канала 1709 или 1803, которые не поддерживаются. Для поддерживаемых изображений Windows в экземплярах контейнеров Azure [см.](container-instances-faq.md#what-windows-base-os-images-are-supported)

### <a name="unable-to-pull-image"></a>Сбой получения образа

Если службе "Экземпляры контейнеров Azure" не удалось изначально получить образ, в течение некоторого периода она будет повторять эту операцию. Если операция по извлечению образа продолжает завершаться сбоем, ACI в конечном итоге не выполняет развертывание, и вы можете увидеть ошибку `Failed to pull image`.

Чтобы устранить эту проблему, удалите экземпляр контейнера и повторите развертывание. Убедитесь, что образ существует в реестре и что имя образа введено верно.

Если образ невозможно получить, в выходных данных команды [az container show][az-container-show] отображаются следующие события:

```bash
"events": [
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "pulling image \"mcr.microsoft.com/azuredocs/aci-hellowrld\"",
    "name": "Pulling",
    "type": "Normal"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "Failed to pull image \"mcr.microsoft.com/azuredocs/aci-hellowrld\": rpc error: code 2 desc Error: image t/aci-hellowrld:latest not found",
    "name": "Failed",
    "type": "Warning"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:20+00:00",
    "lastTimestamp": "2017-12-21T22:57:16+00:00",
    "message": "Back-off pulling image \"mcr.microsoft.com/azuredocs/aci-hellowrld\"",
    "name": "BackOff",
    "type": "Normal"
  }
],
```
### <a name="resource-not-available-error"></a>Ошибка при недоступном ресурсе

Из-за изменения нагрузки региональных ресурсов в Azure при попытке развернуть экземпляр контейнера может появляться следующая ошибка:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Эта ошибка означает, что из-за высокой нагрузки в регионе, в котором вы пытаетесь выполнить развертывание, сейчас не удается выделить ресурсы, указанные для контейнера. Чтобы устранить эту проблему, используйте один или несколько способов, указанных ниже.

* Убедитесь, что параметры развертывания контейнера соответствуют параметрам, определенным в статье [Доступность службы "Экземпляры контейнеров Azure" в регионах](container-instances-region-availability.md).
* Укажите для контейнера более низкие значения ЦП и памяти.
* Выполните развертывание в другом регионе Azure.
* Выполните развертывание позднее.

## <a name="issues-during-container-group-runtime"></a>Проблемы во время выполнения контейнерной группы
### <a name="container-continually-exits-and-restarts-no-long-running-process"></a>Контейнер постоянно завершает работу и перезагружается (не очень долгий процесс)

Контейнерные группы по умолчанию выполняют [политику перезапуска](container-instances-restart-policy.md)**Always**, поэтому контейнеры в группе контейнеров всегда перезапускаются после завершения. Может потребоваться изменить ее на **OnFailure** или **Never**, если планируется запуск контейнеров на основе задач. Если вы настроили **OnFailure**, но контейнер по-прежнему перезагружается, возможно, существует проблема в выполняемом в контейнере приложении или скрипте.

При запуске групп контейнеров без длительных процессов можно видеть повторные выходы и перезапуски образов, такие как Ubuntu или Alpine. Подключение через [EXEC](container-instances-exec.md) не будет работать, так как контейнер не имеет процесса, поддерживающего его. Чтобы решить эту проблему, включите команду запуска, как следующее с развертыванием группы контейнеров, чтобы сохранить контейнер работает.

```azurecli-interactive
## Deploying a Linux container
az container create -g MyResourceGroup --name myapp --image ubuntu --command-line "tail -f /dev/null"
```

```azurecli-interactive 
## Deploying a Windows container
az container create -g myResourceGroup --name mywindowsapp --os-type Windows --image mcr.microsoft.com/windows/servercore:ltsc2019
 --command-line "ping -t localhost"
```

API Экземпляров контейнеров и портала Azure включают свойство `restartCount`. Чтобы узнать количество перезагрузок контейнера, выполните в Azure CLI команду [az container show][az-container-show]. Ниже приведен пример выходных данных (сокращены для удобства), в конце которого можно увидеть свойство `restartCount`.

```json
...
 "events": [
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:06+00:00",
     "lastTimestamp": "2017-11-13T21:20:06+00:00",
     "message": "Pulling: pulling image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Pulled: Successfully pulled image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Created: Created container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Started: Started container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   }
 ],
 "previousState": null,
 "restartCount": 0
...
}
```

> [!NOTE]
> Большинство образов контейнера для дистрибутивов Linux настраивают оболочку, например bash, в качестве команды по умолчанию. Оболочка сама по себе не является долго работающей службой. Поэтому, если для этих контейнеров настроена политика перезагрузки **Always**, они сразу же завершают работу и начинают циклически перезагружаться.

### <a name="container-takes-a-long-time-to-start"></a>Контейнер долго запускается

Три основных фактора, которые способствуют времени запуска контейнеров в Azure Container Instances:

* [Размер образа](#image-size)
* [Расположение образа](#image-location)
* [Кэшированные изображения](#cached-images)

Для образов Windows действуют [дополнительные рекомендации](#cached-images).

#### <a name="image-size"></a>Размер образа

Если контейнер запускается долго, но в конечном итоге успешно, обратите внимание на размер образа контейнера. Так как служба "Экземпляры контейнеров Azure" извлекает образ контейнера по требованию, время запуска непосредственно связано с его размером.

Размер образа контейнера можно просмотреть с помощью команды `docker images` в интерфейсе командной строки Docker:

```console
$ docker images
REPOSITORY                                    TAG       IMAGE ID        CREATED          SIZE
mcr.microsoft.com/azuredocs/aci-helloworld    latest    7367f3256b41    15 months ago    67.6MB
```

Чтобы обеспечить маленький размер образа, нужно добавить в него только то, что требуется во время выполнения. Один из способов — это [многоэтапная сборка][docker-multi-stage-builds]. За счет многоэтапной сборки можно очень просто гарантировать, что итоговый образ содержит артефакты, необходимые для приложения, и в нем отсутствует дополнительное содержимое, которое требовалось во время сборки.

#### <a name="image-location"></a>Расположение образа

Чтобы получение образа меньше влияло на время запуска контейнера, вы также можете разместить образ контейнера в [реестре контейнеров Azure](/azure/container-registry/) того же региона, в котором будут развернуты экземпляры контейнеров. Таким образом сокращается сетевой путь, который должен пройти образ контейнера, а также значительно сокращается время загрузки.

#### <a name="cached-images"></a>Кэшированные изображения

Azure Container Instances использует механизм кэширования, чтобы помочь ускорить время запуска `nanoserver:1809`контейнера `servercore:1809`для изображений, построенных на общих [базовых изображениях Windows,](container-instances-faq.md#what-windows-base-os-images-are-supported)включая `servercore:ltsc2019`и . Обычно используемые изображения `ubuntu:1604` `alpine:3.6` Linux, такие как и также кэшируются. Для обновленного списка кэшированных изображений и тегов используйте API [кэшированных изображений.][list-cached-images]

> [!NOTE]
> Возможность использования образов на основе Windows Server 2019 в службе "Экземпляры контейнеров Azure" предоставляется в режиме предварительной версии.

#### <a name="windows-containers-slow-network-readiness"></a>Контейнеры Windows замедляют период готовности сети

В контейнерах Windows при первоначальном создании в течение 30 секунд (в редких случаях и дольше) может отсутствовать входящее или исходящее подключение. Если приложению-контейнеру требуется подключение к Интернету, добавьте логику задержки и повторного выполнения, позволяющую через 30 секунд установить подключение к Интернету. После начальной установки работа с контейнерами в сети будет восстановлена соответствующим образом.

### <a name="cannot-connect-to-underlying-docker-api-or-run-privileged-containers"></a>Не удается подключиться к основному API Docker или запустить привилегированные контейнеры.

Служба "Экземпляры контейнеров Azure" не предоставляет прямой доступ к базовой инфраструктуре, в которой размещены группы контейнеров. Это включает в себя доступ к API Docker, запущенному на узле контейнера, и выполнение привилегированных контейнеров. Если требуется взаимодействие с Docker, обратитесь к [справочной документации по REST](https://aka.ms/aci/rest), чтобы узнать о поддерживаемых API в службе "Экземпляры контейнеров Azure". Также можно отправить запрос на [форумах обратной связи ACI](https://aka.ms/aci/feedback).

### <a name="container-group-ip-address-may-not-be-accessible-due-to-mismatched-ports"></a>IP-адрес контейнерной группы может быть недоступен из-за несоответствия портов

Azure Container Instances пока не поддерживает картирование портов, как при обычной конфигурации докеров. Если вы обнаружите, что IP-адрес контейнерной группы недоступен, если вы считаете, что это должно быть, `ports` убедитесь, что вы настроили изображение контейнера для прослушивания тех же портов, которые вы предоставляете в вашей контейнерной группе с свойством.

Если вы хотите подтвердить, что экземпляры контейнеров Azure могут слушать в настраиваемом `aci-helloworld` в контейнере порту, проверьте развертывание изображения, которое предоставляет порт. Также запустите `aci-helloworld` приложение так, чтобы оно слушало в порту. `aci-helloworld`принимает дополнительную переменную `PORT` среды для переопределения порта по умолчанию 80, который он слушает. Например, для тестирования порта 9000 установите [переменную среды](container-instances-environment-variables.md) при создании группы контейнеров:

1. Настройка группы контейнеров для разоблачения порта 9000 и передачи номера порта в качестве значения переменной среды. Пример отформатирован для оболочки Bash. Если вы предпочитаете другую оболочку, такую как PowerShell или Command Prompt, вам необходимо соответствующим образом отрегулировать переменное назначение.
    ```azurecli
    az container create --resource-group myResourceGroup \
    --name mycontainer --image mcr.microsoft.com/azuredocs/aci-helloworld \
    --ip-address Public --ports 9000 \
    --environment-variables 'PORT'='9000'
    ```
1. Найдите IP-адрес контейнерной группы `az container create`в командном выпуске . Ищите значение **ip**. 
1. После успешного подготовки контейнера просмотрите IP-адрес и порт контейнерного приложения `192.0.2.0:9000`в браузере, например: . 

    Вы должны увидеть "Добро пожаловать в Azure Контейнер Instances!" сообщение отображается веб-приложением.
1. Когда вы закончите с контейнером, `az container delete` удалите его с помощью команды:

    ```azurecli
    az container delete --resource-group myResourceGroup --name mycontainer
    ```

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как [получить журналы контейнеров и события,](container-instances-get-logs.md) чтобы помочь отладить контейнеры.

<!-- LINKS - External -->
[azure-name-restrictions]: https://docs.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#naming-and-tagging-resources
[windows-sac-overview]: https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview
[docker-multi-stage-builds]: https://docs.docker.com/engine/userguide/eng-image/multistage-build/
[docker-hub-windows-core]: https://hub.docker.com/_/microsoft-windows-servercore
[docker-hub-windows-nano]: https://hub.docker.com/_/microsoft-windows-nanoserver

<!-- LINKS - Internal -->
[az-container-show]: /cli/azure/container#az-container-show
[list-cached-images]: /rest/api/container-instances/listcachedimages
