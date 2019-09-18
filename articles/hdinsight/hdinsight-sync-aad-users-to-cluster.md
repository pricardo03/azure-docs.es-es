---
title: 'Sincronización de los usuarios de Azure Active Directory en un clúster: Azure HDInsight'
description: Sincronice los usuarios autenticados de Azure Active Directory en un clúster de HDInsight.
ms.service: hdinsight
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: f58c847f512f2db72fdca823637192c3b638b1ae
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70879346"
---
# <a name="synchronize-azure-active-directory-users-to-an-hdinsight-cluster"></a>Sincronización de los usuarios de Azure Active Directory en un clúster de HDInsight

[Los clústeres de HDInsight con Enterprise Security Package (ESP)](hdinsight-domain-joined-introduction.md) pueden usar una autenticación segura con los usuarios de Azure Active Directory (Azure AD) y las directivas de *control de acceso basado en rol* (RBAC). A medida que agrega usuarios y grupos a Azure AD, puede sincronizar los usuarios que necesitan tener acceso al clúster.

## <a name="prerequisites"></a>Requisitos previos

Si aún no lo ha hecho, [cree un clúster de HDInsight con Enterprise Security Package](hdinsight-domain-joined-configure.md).

## <a name="add-new-azure-ad-users"></a>Incorporación de nuevos usuarios de Azure AD

Para ver los hosts, abra la interfaz de usuario web de Ambari. Cada nodo se actualizará con la nueva configuración de actualización desatendida.

1. En [Azure Portal](https://portal.azure.com), vaya al directorio de Azure AD asociado al clúster de ESP.

2. Seleccione **Todos los usuarios** en el menú de la izquierda y, a continuación, seleccione **Nuevo usuario**.

    ![Panel Todos los usuarios](./media/hdinsight-sync-aad-users-to-cluster/aad-users.png)

3. Rellene el formulario del nuevo usuario. Seleccione los grupos que creó para asignar permisos basados en el clúster. En este ejemplo, cree un grupo denominado "HiveUsers", al que puede asignar los nuevos usuarios. Las [instrucciones de ejemplo](hdinsight-domain-joined-configure.md) para crear un clúster de ESP incluyen la adición de dos grupos: `HiveUsers` y `AAD DC Administrators`.

    ![Panel Nuevo usuario](./media/hdinsight-sync-aad-users-to-cluster/aad-new-user.png)

4. Seleccione **Crear**.

## <a name="use-the-apache-ambari-rest-api-to-synchronize-users"></a>Uso de la API REST de Apache Ambari para sincronizar los usuarios

Los grupos de usuarios especificados durante el proceso de creación del clúster se sincronizan en ese momento. La sincronización de usuario se produce automáticamente una vez cada hora. Para sincronizar los usuarios inmediatamente o para sincronizar un grupo distinto de los especificados durante la creación del clúster, use la API de REST de Ambari.

El método siguiente utiliza POST con la API de REST de Ambari. Para obtener más información, consulte [Administración de clústeres de HDInsight con la API REST de Apache Ambari](hdinsight-hadoop-manage-ambari-rest-api.md).

1. [Conecte el clúster con SSH](hdinsight-hadoop-linux-use-ssh-unix.md). En el panel de información general para el clúster en Azure Portal, seleccione el botón **Secure Shell (SSH)** .

    ![Secure Shell (SSH)](./media/hdinsight-sync-aad-users-to-cluster/hdinsight-secure-shell.png)

2. Copie el comando `ssh` mostrado y péguelo en el cliente SSH. Escriba la contraseña de usuario SSH cuando se le pida.

3. Tras la autenticación, escriba el siguiente comando:

    ```bash
    curl -u admin:<YOUR PASSWORD> -sS -H "X-Requested-By: ambari" \
    -X POST -d '{"Event": {"specs": [{"principal_type": "groups", "sync_type": "existing"}]}}' \
    "https://<YOUR CLUSTER NAME>.azurehdinsight.net/api/v1/ldap_sync_events"
    ```
    
    La respuesta debería tener este aspecto:

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

4. Para ver el estado de sincronización, ejecute un nuevo comando `curl`:

    ```bash
    curl -u admin:<YOUR PASSWORD> https://<YOUR CLUSTER NAME>.azurehdinsight.net/api/v1/ldap_sync_events/1
    ```
    
    La respuesta debería tener este aspecto:
    
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

5. Este resultado muestra que el estado es **COMPLETO**, una vez creado un nuevo usuario, y se asigna al usuario a un grupo. En este ejemplo, el usuario se asigna al grupo LDAP sincronizado "HiveUsers", puesto que se agregó a ese mismo grupo en Azure AD.

> [!NOTE]  
> El método anterior solo sincroniza los grupos de Azure AD especificados en la propiedad **Access user group** (Grupo de usuarios con acceso) de la configuración del dominio durante la creación del clúster. Para más información, consulte [Create an HDInsight cluster](domain-joined/apache-domain-joined-configure.md) (Creación de un clúster de HDInsight).

## <a name="verify-the-newly-added-azure-ad-user"></a>Verificación del usuario de Azure AD recién agregado

Abra la [interfaz de usuario web de Apache Ambari](hdinsight-hadoop-manage-ambari.md) para verificar que el nuevo usuario de Azure AD se agregó. Acceda a la interfaz de usuario web de Ambari en **`https://<YOUR CLUSTER NAME>.azurehdinsight.net`** . Escriba el nombre de usuario y la contraseña del administrador del clúster.

1. En el panel de Ambari, seleccione **Manage Ambari** (Administrar Ambari) en el menú **admin** (Administrador).

    ![Administración de Ambari](./media/hdinsight-sync-aad-users-to-cluster/manage-ambari.png)

2. Seleccione **Usuarios** en el grupo de menús **Usuario + Administración del grupo**, en el lado izquierdo de la página.

    ![Elemento del menú Usuarios](./media/hdinsight-sync-aad-users-to-cluster/users-link.png)

3. El nuevo usuario debe aparecer dentro de la tabla de usuarios. El tipo está establecido en `LDAP` en lugar de en `Local`.

    ![Página Usuarios](./media/hdinsight-sync-aad-users-to-cluster/hdinsight-users-page.png)

## <a name="log-in-to-ambari-as-the-new-user"></a>Inicio de sesión en Ambari como el nuevo usuario

Cuando el nuevo usuario (o cualquier otro usuario del dominio) inicia sesión en Ambari, se usan sus credenciales del dominio y el nombre de usuario completo de Azure AD.  Ambari muestra un alias de usuario, que es el nombre para mostrar del usuario en Azure AD. El nuevo usuario de ejemplo tiene el nombre de usuario `hiveuser3@contoso.com`. En Ambari, este nuevo usuario aparece como `hiveuser3`, pero el usuario inicia sesión en Ambari como `hiveuser3@contoso.com`.

## <a name="see-also"></a>Otras referencias

* [Configuración de directivas de Apache Hive en HDInsight con ESP](hdinsight-domain-joined-run-hive.md)
* [Administración de clústeres de HDInsight con Enterprise Security Package](hdinsight-domain-joined-manage.md)
* [Autorización de usuarios en Apache Ambari](hdinsight-authorize-users-to-ambari.md)
