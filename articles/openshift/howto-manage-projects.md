---
title: Управление ресурсами в Azure Red Hat OpenShift | Документация Майкрософт
description: Управление проектами, шаблонами, потоками изображений в кластере Azure Red Hat OpenShift
services: openshift
keywords: проекты Red Hat openshift запрашивают самоподготовку
author: mjudeikis
ms.author: gwallace
ms.date: 07/19/2019
ms.topic: conceptual
ms.service: container-service
ms.openlocfilehash: d4f53238951784a74e6e3fc8a73d1f112ce75608
ms.sourcegitcommit: d322d0a9d9479dbd473eae239c43707ac2c77a77
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2020
ms.locfileid: "79139119"
---
# <a name="manage-projects-templates-image-streams-in-an-azure-red-hat-openshift-cluster"></a>Управление проектами, шаблонами, потоками изображений в кластере Azure Red Hat OpenShift 

В платформе контейнера OpenShift проекты используются для группирования и изоляции связанных объектов. Администратор может предоставить разработчикам доступ к конкретным проектам, позволить им создавать собственные проекты и предоставлять им административные права для отдельных проектов.

## <a name="self-provisioning-projects"></a>Самостоятельная подготовка проектов

Вы можете позволить разработчикам создавать собственные проекты. Конечная точка API отвечает за подготовку проекта в соответствии с шаблоном, который называется Project-Request. Веб-консоль и команда `oc new-project` используют эту конечную точку, когда разработчик создает новый проект.

При отправке запроса проекта API заменяет следующие параметры в шаблоне:

| Параметр               | Description                                    |
| ----------------------- | ---------------------------------------------- |
| PROJECT_NAME            | Имя проекта. Обязательный элемент.             |
| PROJECT_DISPLAYNAME     | Отображаемое имя проекта. Может быть пустым. |
| PROJECT_DESCRIPTION     | Описание проекта. Может быть пустым.  |
| PROJECT_ADMIN_USER      | Имя пользователя для администрирования.       |
| PROJECT_REQUESTING_USER | Имя пользователя запрашивающего пользователя.           |

Доступ к API предоставляется разработчикам с помощью привязки роли кластера самоподготовки. По умолчанию эта функция доступна для всех разработчиков, прошедших проверку подлинности.

## <a name="modify-the-template-for-a-new-project"></a>Изменение шаблона для нового проекта 

1. Войдите в систему как пользователь с правами `customer-admin`.

2. Измените шаблон запроса проекта по умолчанию.

   ```
   oc edit template project-request -n openshift
   ```

3. Удалите шаблон проекта по умолчанию из процесса обновления Azure Red Hat OpenShift (АТО), добавив следующую аннотацию: `openshift.io/reconcile-protect: "true"`

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

   Шаблон запроса проекта не будет обновлен процессом обновления АТО. Это позволяет клиентам настраивать шаблон и сохранять эти настройки при обновлении кластера.

## <a name="disable-the-self-provisioning-role"></a>Отключение роли самоподготовки

Вы можете предотвратить самостоятельную подготовку новых проектов для группы пользователей, прошедших проверку подлинности.

1. Войдите в систему как пользователь с правами `customer-admin`.

2. Измените привязку роли кластера самоподготовки.

   ```
   oc edit clusterrolebinding.rbac.authorization.k8s.io self-provisioners
   ```

3. Удалите роль из процесса обновления АТО, добавив следующую аннотацию: `openshift.io/reconcile-protect: "true"`.

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

4. Измените привязку роли кластера, чтобы предотвратить `system:authenticated:oauth` создания проектов:

   ```
   apiVersion: rbac.authorization.k8s.io/v1
   groupNames:
   - osa-customer-admins
   kind: ClusterRoleBinding
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
     labels:
       azure.openshift.io/owned-by-sync-pod: "true"
     name: self-provisioners
   roleRef:
     name: self-provisioner
   subjects:
   - kind: SystemGroup
     name: osa-customer-admins
   ```

## <a name="manage-default-templates-and-imagestreams"></a>Управление шаблонами по умолчанию и Имажестреамс

В Azure Red Hat OpenShift можно отключить обновления для любых шаблонов по умолчанию и потоков изображений в `openshift` пространстве имен.
Чтобы отключить обновления для всех `Templates` и `ImageStreams` в пространстве имен `openshift`:

1. Войдите в систему как пользователь с правами `customer-admin`.

2. Изменить `openshift`ое пространство имен:

   ```
   oc edit namespace openshift
   ```

3. Удалите `openshift` пространство имен из процесса обновления АТО, добавив следующую аннотацию: `openshift.io/reconcile-protect: "true"`

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

   Любой отдельный объект в пространстве имен `openshift` можно удалить из процесса обновления, добавив в него заметку `openshift.io/reconcile-protect: "true"`.

## <a name="next-steps"></a>Дальнейшие действия

Воспользуйтесь руководством.
> [!div class="nextstepaction"]
> [Создание кластера Azure Red Hat OpenShift](tutorial-create-cluster.md)
