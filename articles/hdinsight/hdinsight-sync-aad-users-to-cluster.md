---
title: Синхронизация пользователей Azure Active Directory с кластером Azure HDInsight
description: Синхронизация пользователей, прошедших проверку подлинности, от Azure Active Directory к кластеру HDInsight.
ms.service: hdinsight
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 9a7008ca7967135209b8fc2c341998570a2679be
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2019
ms.locfileid: "70960836"
---
# <a name="synchronize-azure-active-directory-users-to-an-hdinsight-cluster"></a>Синхронизация пользователей Azure Active Directory с кластером HDInsight

[Кластеры HDInsight с Корпоративным пакетом безопасности (ESP)](hdinsight-domain-joined-introduction.md) могут использовать строгую аутентификацию пользователей Azure Active Directory (Azure AD), а также политики *управления доступом на основе ролей* (RBAC). При добавлении пользователей и групп в Azure AD вы можете синхронизировать пользователей, которым необходим доступ к кластеру.

## <a name="prerequisites"></a>Предварительные требования

Если вы еще этого не сделали, [создайте кластер HDInsight с Корпоративным пакетом безопасности](hdinsight-domain-joined-configure.md).

## <a name="add-new-azure-ad-users"></a>Добавление новых пользователей Azure AD

Чтобы просмотреть узлы, откройте веб-интерфейс Ambari. Для каждого узла будут заданы новые параметры автоматического обновления.

1. На [портале Azure](https://portal.azure.com) перейдите в каталог Azure AD, связанный с кластером ESP.

2. Выберите **All users** (Все пользователи) в левом меню, а затем щелкните **New user** (Новый пользователь).

    ![Панель All users (Все пользователи)](./media/hdinsight-sync-aad-users-to-cluster/users-and-groups-new.png)

3. Заполните форму нового пользователя. Выберите группы, созданные для назначения разрешений на основе кластеров. В этом примере создайте группу с именем HiveUsers, которой можно назначить новых пользователей. [Примеры инструкций](hdinsight-domain-joined-configure.md) для создания кластера ESP включают в себя добавление двух групп: `HiveUsers` и `AAD DC Administrators`.

    ![Панель New user (Новый пользователь)](./media/hdinsight-sync-aad-users-to-cluster/hdinsight-new-user-form.png)

4. Нажмите кнопку **Создать**.

## <a name="use-the-apache-ambari-rest-api-to-synchronize-users"></a>Синхронизация пользователей с помощью REST API Apache Ambari

Группы пользователей, указанные во время создания кластера, уже синхронизированы. Синхронизация пользователей выполняется автоматически один раз в час. Чтобы немедленно синхронизировать пользователей или группы, которые не указаны при создании кластера, используйте REST API Ambari.

Следующий метод использует POST с REST API Ambari. Дополнительные сведения см. в статье [Управление кластерами HDInsight с помощью REST API Ambari](hdinsight-hadoop-manage-ambari-rest-api.md).

1. [Подключитесь к кластеру, используя протокол SSH](hdinsight-hadoop-linux-use-ssh-unix.md). В области обзора для кластера на портале Azure нажмите кнопку **Безопасная оболочка (SSH)** .

    ![Безопасная оболочка (SSH)](./media/hdinsight-sync-aad-users-to-cluster/hdinsight-secure-shell.png)

2. Скопируйте отображаемую команду `ssh` и вставьте ее в клиент SSH. При появлении запроса введите пользовательский пароль SSH.

3. После аутентификации введите следующую команду:

    ```bash
    curl -u admin:<YOUR PASSWORD> -sS -H "X-Requested-By: ambari" \
    -X POST -d '{"Event": {"specs": [{"principal_type": "groups", "sync_type": "existing"}]}}' \
    "https://<YOUR CLUSTER NAME>.azurehdinsight.net/api/v1/ldap_sync_events"
    ```
    
    Результат должен выглядеть следующим образом.

    ```json
    {
      "resources" : [
        {
          "href" : "http://hn0-hadoop.<YOUR DOMAIN>.com:8080/api/v1/ldap_sync_events/1",
          "Event" : {
            "id" : 1
          }
        }
      ]
    }
    ```

4. Чтобы просмотреть состояние синхронизации, выполните новую команду `curl`:

    ```bash
    curl -u admin:<YOUR PASSWORD> https://<YOUR CLUSTER NAME>.azurehdinsight.net/api/v1/ldap_sync_events/1
    ```
    
    Результат должен выглядеть следующим образом.
    
    ```json
    {
      "href" : "http://hn0-hadoop.YOURDOMAIN.com:8080/api/v1/ldap_sync_events/1",
      "Event" : {
        "id" : 1,
        "specs" : [
          {
            "sync_type" : "existing",
            "principal_type" : "groups"
          }
        ],
        "status" : "COMPLETE",
        "status_detail" : "Completed LDAP sync.",
        "summary" : {
          "groups" : {
            "created" : 0,
            "removed" : 0,
            "updated" : 0
          },
          "memberships" : {
            "created" : 1,
            "removed" : 0
          },
          "users" : {
            "created" : 1,
            "removed" : 0,
            "skipped" : 0,
            "updated" : 0
          }
        },
        "sync_time" : {
          "end" : 1497994072182,
          "start" : 1497994071100
        }
      }
    }
    ```

5. В этом результате показано, что состояние — **COMPLETE** (Завершено) (был создан один пользователь, и ему было назначено членство). В этом примере пользователь назначен для группы HiveUsers, синхронизированной по протоколу LDAP, так как он был добавлен в ту же группу в Azure AD.

> [!NOTE]  
> В предыдущем методе выполняется синхронизация только тех групп Azure AD, которые указаны в свойстве **Access user group** (Доступ к группе пользователей) параметров домена при создании кластера. Дополнительные сведения см. в статье [Настройка среды с присоединенной к домену песочницей HDInsight](domain-joined/apache-domain-joined-configure.md).

## <a name="verify-the-newly-added-azure-ad-user"></a>Проверка только что добавленного пользователя Azure AD

Откройте [веб-интерфейс Apache Ambari](hdinsight-hadoop-manage-ambari.md) и убедитесь, что новый пользователь Azure AD добавлен. Получите доступ к веб-интерфейсу Ambari, перейдя к **`https://<YOUR CLUSTER NAME>.azurehdinsight.net`** . Укажите имя администратора и пароль кластера.

1. На панели мониторинга Ambari выберите **Manage Ambari** (Управление Ambari) в меню **admin**.

    ![Управление Ambari](./media/hdinsight-sync-aad-users-to-cluster/manage-apache-ambari.png)

2. Выберите **Пользователи** в группе меню **User + Group Management** (Управление пользователями и группами) в левой части страницы.

    ![Пункт меню "Пользователи"](./media/hdinsight-sync-aad-users-to-cluster/hdinsight-users-menu-item.png)

3. Новый пользователь должен быть указан в таблице "Пользователи". Для типа необходимо задать значение `LDAP`, а не `Local`.

    ![Страница "Пользователи"](./media/hdinsight-sync-aad-users-to-cluster/hdinsight-users-page.png)

## <a name="log-in-to-ambari-as-the-new-user"></a>Вход в Ambari в качестве нового пользователя

Когда новый пользователь (или любой другой пользователь домена) входит в Ambari, он использует полное имя пользователя и учетные данные домена Azure AD.  В Ambari показан псевдоним пользователя, являющийся отображаемым именем пользователя в Azure AD. В новом примере имя пользователя — `hiveuser3@contoso.com`. В Ambari этот пользователь отображается как `hiveuser3`, но он входит в Ambari как `hiveuser3@contoso.com`.

## <a name="see-also"></a>См. также

* [Настройка политик Apache Hive в кластере HDInsight с ESP](hdinsight-domain-joined-run-hive.md)
* [Управление кластерами HDInsight с помощью корпоративного пакета безопасности](hdinsight-domain-joined-manage.md)
* [Предоставление пользователям доступа к представлениям Apache Ambari](hdinsight-authorize-users-to-ambari.md)
