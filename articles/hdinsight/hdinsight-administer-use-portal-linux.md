---
title: Administración de clústeres de Apache Hadoop en HDInsight mediante Azure Portal
description: Aprenda a crear y administrar clústeres de HDInsight mediante el portal de Azure.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: hrasheed
ms.openlocfilehash: 80c27d588bb8b4c60ae8432fbd3a2ac3d2db1394
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58802667"
---
# <a name="manage-apache-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Administración de clústeres de Apache Hadoop en HDInsight mediante Azure Portal

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

En [Azure Portal][azure-portal], puede administrar clústeres de [Apache Hadoop](https://hadoop.apache.org/) en Azure HDInsight. Use el selector de pestañas anterior para más información sobre cómo administrar clústeres de Hadoop en HDInsight con otras herramientas.

## <a name="prerequisites"></a>Requisitos previos
- Una suscripción de Azure. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Un clúster existente de Apache Hadoop en HDInsight.  Consulte [Crear clústeres basados en Linux en HDInsight con Azure Portal](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="getting-started"></a>Introducción
Inicie sesión en [https://portal.azure.com](https://portal.azure.com).

## <a name="showClusters"></a> Enumeración y visualización de clústeres
En la página de **clústeres de HDInsight** se muestran los clústeres existentes.  En el portal:
1. En el menú izquierdo, seleccione **Todos los servicios**.
2. Seleccione **Clústeres de HDInsight** en **ANALYTICS**.

## <a name="homePage"></a> Página principal del clúster 
Seleccione el nombre del clúster desde el [ **clústeres de HDInsight** ](#showClusters) página.  Se abrirá la vista de **Información general**, que se parece a la siguiente imagen:

![Azure Portal: aspectos básicos del clúster de HDInsight](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials2.png)

**Menú superior:**  

| item| DESCRIPCIÓN |
|---|---|
|Move|mueve el clúster a otro grupo de recursos o a otra suscripción.|
|Eliminar|elimina el clúster. |
|Actualizar|actualiza la vista.|

**Menú de la izquierda:**  
  - **Menú de la parte superior izquierda**

    | item| DESCRIPCIÓN |
    |---|---|
    |Información general|proporciona información general del clúster.|
    |Registro de actividades|muestra y consulta los registros de actividad.|
    |Control de acceso (IAM)|usa asignaciones de roles.  Vea [Uso de asignaciones de roles para administrar el acceso a los recursos de la suscripción de Azure](../role-based-access-control/role-assignments-portal.md).|
    |Etiquetas|las etiquetas permiten establecer pares clave-valor para definir una taxonomía personalizada de Cloud Services. Por ejemplo, puede crear una clave denominada **proyecto**y luego usar un valor común para todos los servicios asociados a un proyecto específico.|
    |Diagnosticar y solucionar problemas|muestra información de solución de problemas.|
    |Guía de inicio rápido|muestra información que lo ayuda a empezar a usar HDInsight.|
    |Herramientas|información de ayuda para herramientas relacionadas con HDInsight.|

  - **Menú Configuración**  

    | item| DESCRIPCIÓN |
    |---|---|
    |Tamaño del clúster|comprueba, aumenta o reduce el número de nodos de trabajo del clúster. Consulte [Escalado de clústeres](hdinsight-administer-use-portal-linux.md#scale-clusters).|
    |Límites de cuota|muestra los núcleos utilizados y disponibles de la suscripción.|
    |SSH e inicio de sesión del clúster|muestra las instrucciones para conectarse al clúster mediante la conexión de Secure Shell (SSH). Para más información, consulte [Uso SSH con HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).|
    |Data Lake Storage Gen1|configura el acceso a Data Lake Storage Gen1.  Consulte [Quickstart: Configuración de clústeres en HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).|
    |Cuentas de almacenamiento|muestra las cuentas de almacenamiento y las claves. Las cuentas de almacenamiento se configuran durante el proceso de creación del clúster.|
    |APLICACIONES|agrega o quita aplicaciones de HDInsight.  Vea [Instalación de aplicaciones de HDInsight personalizadas](hdinsight-apps-install-custom-applications.md).|
    |Acciones de script|ejecuta scripts de Bash en el clúster. Consulte [Personalización de clústeres de HDInsight mediante la acción de scripts (Linux)](hdinsight-hadoop-customize-cluster-linux.md).|
    |Tiendas de metadatos externas|consulte las tiendas de metadatos de [Apache Hive](https://hive.apache.org/) y [Apache Oozie](https://oozie.apache.org/). Las tiendas de metadatos solo pueden configurarse durante el proceso de creación del clúster.|
    |Asociado de HDInsight|agrega o quita el asociado de HDInsight actual.|
    |Properties (Propiedades)|vea las [propiedades del clúster](#properties).|
    |Bloqueos|agrega bloqueos para evitar la modificación o eliminación del clúster.|
    |Script de Automation|muestra y exporta la plantilla de Azure Resource Manager para el clúster. Actualmente, solo se puede exportar la cuenta de Azure Storage dependiente. Consulte [Creación de clústeres de Apache Hadoop basados en Linux en HDInsight con plantillas de Azure Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md).|

  - **Menú Supervisión**

    | item| DESCRIPCIÓN |
    |---|---|
    |Alertas|administra las alertas y acciones.|
    |Métricas|Supervisar las métricas de clúster en los registros de Azure Monitor.|
    |Configuración de diagnóstico|configuración donde almacenar las métricas de diagnóstico.|
    |Operations Management Suite|Supervisar el clúster en Azure Operations Management Suite (OMS) y los registros de Azure Monitor.|

  - **Menú Soporte y solución de problemas**

    | item| DESCRIPCIÓN |
    |---|---|
    |Estado de los recursos|consulte [Introducción a Azure Resource Health](../service-health/resource-health-overview.md).|
    |Nueva solicitud de soporte|permite crear una incidencia de soporte técnico con el soporte técnico de Microsoft.|

## <a name="properties"></a> Propiedades del clúster

En la [página principal del clúster](#homePage), en **Configuración**, seleccione **Propiedades**.

|item | DESCRIPCIÓN |
|---|---|
|Nombre de host.|Nombre del clúster.|
|Dirección URL del clúster|La dirección URL para la interfaz web de Ambari.|
|Secure shell (SSH)|el nombre de usuario y el nombre de host que se usarán para acceder al clúster mediante SSH.|
|Status|El valor es uno de los siguientes: Aborted, Accepted, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, Operational, Running, Error, Deleting, Deleted, Timedout, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued o ClusterCustomization.|
|Region|ubicación de Azure. Para ver una lista de ubicaciones de Azure admitidas, consulte el cuadro de lista desplegable **Región** en [Precios de HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).|
|Fecha de creación|la fecha en que se implementó el clúster.|
|Sistema operativo|**Windows** o **Linux**.|
|Type|Hadoop, HBase, Storm o Spark.|
|Versión|Consulte [Versiones de HDInsight](hdinsight-component-versioning.md).|
|Subscription|el nombre de la suscripción.|
|Origen de datos predeterminado|el sistema de archivos predeterminado del clúster.|
|Tamaño de los nodos de trabajo|el tamaño de máquina virtual seleccionado de los nodos de trabajo.|
|Tamaño del nodo principal|el tamaño de máquina virtual seleccionado de los nodos principales.|
|Virtual network|El nombre de la red Virtual, que se implementa el clúster, si se seleccionó una durante la implementación.|

## <a name="move-clusters"></a>Mover clústeres

Puede mover un clúster de HDInsight a otro grupo de recursos de Azure u otra suscripción.

En la [página principal del clúster](#homePage):

1. Seleccione **Mover** en el menú superior.
2. Seleccione **Mover a otro grupo de recursos** o **Mover a otra suscripción**.
3. Siga las instrucciones de la nueva página.

## <a name="delete-clusters"></a>Eliminación de clústeres
Al eliminar un clúster, no se elimina la cuenta de almacenamiento predeterminada ni otras cuentas de almacenamiento vinculadas. Puede volver a crear el clúster con las mismas cuentas de almacenamiento y las mismas tiendas de metadatos. Se recomienda usar un nuevo contenedor de blobs predeterminado cuando vuelva a crear el clúster.

En la [página principal del clúster](#homePage):

1. Seleccione **Eliminar** en el menú.
2. Siga las instrucciones de la nueva página.

Vea también [Pausa o apagado de clústeres](#pauseshut-down-clusters).

## <a name="add-additional-storage-accounts"></a>Adición de cuentas de almacenamiento adicionales

Después de crear el clúster, puede agregar más cuentas de Azure Storage y cuentas de Azure Data Lake Storage. Para más información, consulte [Adición de más cuentas de almacenamiento a HDInsight](./hdinsight-hadoop-add-storage.md).

## <a name="scale-clusters"></a>Escalado de clústeres
La característica de escalado de clústeres permite cambiar el número de nodos de trabajo que usa un clúster de Azure HDInsight sin necesidad de volver a crear el clúster.

> [!NOTE]  
> Solo son compatibles los clústeres con la versión 3.1.3 de HDInsight, o superior. Si no está seguro de la versión del clúster, puede comprobar la página de propiedades.  Consulte Enumeración y visualización de clústeres.

En la [página principal del clúster](#homePage):

1. En **Configuración**, seleccione **Tamaño del clúster**.
2. Escriba el **número de nodos de trabajo** en el cuadro de texto numérico. El límite del número de nodos del clúster varía según las suscripciones de Azure. Puede ponerse en contacto con el servicio de soporte relacionado con la facturación para aumentar el límite.  La información del costo refleja los cambios realizados en el número de nodos.
3. Seleccione **Guardar**.

    ![HDInsight hadoop hbase storm spark escalar](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster2.png)

A continuación se muestra cómo el efecto de cambiar el número de nodos de datos varía para cada tipo de clúster compatible con HDInsight:

* Apache Hadoop

    Puede aumentar sin ningún problema la cantidad de nodos de trabajo en un clúster de Hadoop que se encuentre en ejecución, sin que afecte a ningún trabajo pendiente o en ejecución. También se pueden enviar trabajos nuevos mientras la operación está en curso. Los errores que puedan surgir en una operación de escalado se enfrentan oportunamente, por lo que el clúster siempre queda en estado funcional.

    Cuando se realiza la reducción vertical de un clúster de Hadoop al disminuir la cantidad de nodos de datos, se reinician algunos de los servicios del clúster. Este comportamiento hace que todos los trabajos pendientes y en ejecución fallen al completarse la operación de escalado. Sin embargo, puede volver a enviar los trabajos una vez finalizada la operación.
* HBase Apache

    Puede agregar nodos sin problemas al clúster de HBase mientras se encuentra en ejecución, así como eliminarlos. Los servidores regionales se equilibran automáticamente en unos pocos minutos tras completar la operación de escalado. Sin embargo, puede equilibrar manualmente los servidores regionales iniciando sesión en el nodo principal del clúster y ejecutando los comandos siguientes desde una ventana del símbolo del sistema:

    ```bash
    pushd %HBASE_HOME%\bin
    hbase shell
    balancer
    ```

    Para más información sobre el uso del shell de HBase, vea [Introducción a un ejemplo de Apache HBase en HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md).

* Apache Storm

    Puede agregar o quitar sin problemas nodos de datos de su clúster de Storm mientras se encuentra en ejecución. Sin embargo, después de finalizar correctamente la operación de escalado, deberá volver a equilibrar la topología.

    Esto se puede realizar de dos formas:

  * La interfaz de usuario web de Storm
  * La herramienta de la interfaz de línea de comandos (CLI)

    Consulte la [documentación de Apache Storm](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) para obtener más detalles.

    La interfaz de usuario web de Storm se encuentra disponible en el clúster de HDInsight:

    ![Reequilibrio de escalado de HDInsight Storm](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster-storm-rebalance.png)

    El siguiente es un comando de la CLI de ejemplo para volver a equilibrar la topología de Storm:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```


## <a name="pauseshut-down-clusters"></a>Pausa o apagado de clústeres

La mayoría de los trabajos de Hadoop son trabajos por lotes que se ejecutan solo ocasionalmente. En la mayoría de los clústeres de Hadoop, hay grandes períodos de tiempo en los que el clúster no se usa para el procesamiento. Con HDInsight, los datos se almacenan en Azure Storage, por lo que puede eliminar un clúster de forma segura cuando no está en uso.
También se le cobrará por un clúster de HDInsight aunque no se esté usando. Como en muchas ocasiones los cargos por el clúster son mucho más elevados que los cargos por el almacenamiento, desde el punto de vista económico tiene sentido eliminar clústeres cuando no se estén usando.

Hay muchas maneras de programar el proceso:

* Usar Azure Data Factory. Consulte [Creación de clústeres de Apache Hadoop basados en Linux en HDInsight a petición con Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) para crear servicios vinculados a HDInsight a petición.
* Usar Azure PowerShell.  Vea [Análisis de datos de retrasos de vuelos](hdinsight-analyze-flight-delay-data-linux.md).
* Use la CLI de Azure clásica. Consulte [Administración de clústeres de HDInsight con la CLI de Azure clásica](hdinsight-administer-use-command-line.md).
* Usar .NET SDK de HDInsight. Consulte [Envío de trabajos de Apache Hadoop](hadoop/submit-apache-hadoop-jobs-programmatically.md).

Para información sobre precios, vea [Precios de HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/). Para eliminar un clúster desde el portal, vea [Eliminación de clústeres](#delete-clusters)



## <a name="upgrade-clusters"></a>Actualización de clústeres

Vea [Actualización del clúster de HDInsight a una versión más reciente](./hdinsight-upgrade-cluster.md).

## <a name="open-the-apache-ambari-web-ui"></a>Apertura de la interfaz de usuario web de Apache Ambari

Ambari proporciona una intuitiva y sencilla interfaz de usuario web de administración de Hadoop respaldada por sus API RESTful. Ambari permite a los administradores de sistema administrar y supervisar clústeres de Hadoop.

En la [página principal del clúster](#homePage):

1. Seleccione **Paneles de clúster**.

    ![Menú de clúster de Hadoop de HDInsight](./media/hdinsight-administer-use-portal-linux/hdinsight-azure-portal-cluster-menu2.png)

1. Seleccione **Inicio de Ambari** en la nueva página.
2. Escriba el nombre de usuario y la contraseña del clúster.  El nombre de usuario predeterminado del clúster es _admin_. La interfaz de usuario web de Ambari presentará el aspecto siguiente:

Para obtener más información, consulte [Administración de clústeres de HDInsight con la interfaz de usuario web de Apache Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="change-passwords"></a>Cambio de contraseñas
Un clúster de HDInsight puede tener dos cuentas de usuario. La cuenta de usuario del clúster de HDInsight (cuenta de usuario HTTP) y la cuenta de usuario SSH se crean durante el proceso de creación. Puede usar el portal para cambiar la contraseña de la cuenta de usuario del clúster, y las acciones de script para cambiar la cuenta de usuario de SSH.

### <a name="change-the-cluster-user-password"></a>Cambio de la contraseña de usuario del clúster

> [!NOTE]  
> El cambio de la contraseña de usuario (admin) del clúster puede provocar que las acciones de script que se ejecutan en este clúster no lo hagan correctamente. Si tiene cualquier acción de script persistente cuyo destino son nodos de trabajo, estos scripts pueden producir un error al agregar nodos al clúster a través de operaciones de cambio de tamaño. Para más información sobre acciones de script, consulte [Personalización de clústeres de HDInsight mediante la acción de scripts (Linux)](hdinsight-hadoop-customize-cluster-linux.md).

En la [página principal del clúster](#homePage):
1. Seleccione **SSH + Cluster login** (SSH e inicio de sesión del clúster) en **Configuración**.
2. Seleccione **Restablecer credenciales**.
3. Escriba y confirme la nueva contraseña en los cuadros de texto.
4. Seleccione **Aceptar**.

La contraseña se cambia en todos los nodos del clúster.

### <a name="change-the-ssh-user-password"></a>Cambio de la contraseña de usuario de SSH
1. Con un editor de texto, guarde el texto siguiente como un archivo llamado **changepassword.sh**.

    > [!IMPORTANT]  
    > Debe utilizar un editor que use LF como final de líneas. Si el editor utiliza CRLF, el script no funcionará.

    ```bash
    #! /bin/bash
    USER=$1
    PASS=$2
    usermod --password $(echo $PASS | openssl passwd -1 -stdin) $USER
    ```

2. Cargue el archivo en una ubicación de almacenamiento a la que pueda accederse desde HDInsight con una dirección HTTP o HTTPS. Por ejemplo, un almacén de archivos público como OneDrive o Almacenamiento de blobs de Azure. Guarde el identificador URI (dirección HTTP o HTTPS) en el archivo, ya que este URI se necesitará en el paso siguiente.
3. Desde el [página principal del clúster](#homePage), seleccione **acciones de Script** en **configuración**.
4. En la hoja **Acciones de script**, seleccione **Enviar nuevo**. 
5. En la hoja **Enviar acción de script**, escriba la siguiente información:

   | Campo | Valor |
   | --- | --- |
   | Tipo de script | Seleccione **- Personalizado** en la lista desplegable.|
   | NOMBRE |"Cambio de contraseña de SSH" |
   | URI de script de Bash |El identificador URI del archivo changepassword.sh |
   | Tipos de nodo: (principal, de trabajo, nimbus, supervisor, Zookeeper, etc.) |✓ para todos los tipos de nodo indicados |
   | Parámetros |Escriba el nombre de usuario de SSH y la contraseña nueva. Debe haber un espacio entre el nombre de usuario y la contraseña. |
   | Conservar esta acción de script... |Deje este campo en sin activar. |

6. Seleccione **Crear** para aplicar el script. Una vez que finalice el script, puede conectarse al clúster mediante SSH con la nueva contraseña.

## <a name="grantrevoke-access"></a>Concesión o revocación del acceso
Los clústeres de HDInsight tienen los siguientes servicios web HTTP (todos estos servicios tienen extremos RESTful):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

De manera predeterminada, estos servicios se conceden para el acceso. Puede revocar o conceder el acceso mediante la [CLI de Azure clásica](hdinsight-administer-use-command-line.md#enabledisable-http-access-for-a-cluster) y [Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access).

## <a name="find-the-subscription-id"></a>Búsqueda del identificador de la suscripción
Cada clúster está asociado a una suscripción de Azure.  El identificador de la suscripción de Azure es visible desde la [página principal del clúster](#homePage).

## <a name="find-the-resource-group"></a>Búsqueda del grupo de recursos
En el modo de Azure Resource Manager, cada clúster de HDInsight se crea con un grupo de Azure Resource Manager. El grupo de Resource Manager es visible desde la [página principal del clúster](#homePage).

## <a name="find-the-storage-accounts"></a>Búsqueda de las cuentas de almacenamiento
Los clústeres de HDInsight usan una cuenta de Azure Storage o Azure Data Lake Storage para almacenar los datos. Cada clúster de HDInsight puede tener una cuenta de almacenamiento predeterminada y una serie de cuentas de almacenamiento vinculadas. Para enumerar las cuentas de almacenamiento, en la [página principal del clúster](#homePage), en **Configuración**, seleccione **Cuentas de almacenamiento**.


## <a name="monitor-jobs"></a>Supervisión de trabajos
Consulte [Administración de clústeres de HDInsight con la interfaz de usuario web de Apache Ambari](hdinsight-hadoop-manage-ambari.md#monitoring).


## <a name="monitor-cluster-usage"></a>Supervisión del uso de los clústeres
La sección **Uso** de la hoja del clúster de HDInsight muestra información sobre el número de núcleos disponibles con la suscripción para su uso con HDInsight, así como el número de núcleos asignados al clúster y cómo se asignan a los nodos de este clúster. Consulte Enumeración y visualización de clústeres.

> [!IMPORTANT]  
> Para supervisar los servicios que proporciona el clúster de HDInsight, debe usar la web de Ambari o la API de REST de Ambari. Para más información sobre el uso de Ambari, consulte [Administración de clústeres de HDInsight con Apache Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="connect-to-a-cluster"></a>Conectarse a un clúster

* [Uso de Apache Hive con HDInsight](hadoop/apache-hadoop-use-hive-ambari-view.md)
* [Uso de SSH con HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha aprendido algunas funciones administrativas básicas. Para obtener más información, consulte los artículos siguientes:

* [Administración de HDInsight con PowerShell de Azure](hdinsight-administer-use-powershell.md)
* [Administración de HDInsight con la CLI de Azure clásica](hdinsight-administer-use-command-line.md)
* [Creación de clústeres de HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
* [Lea más sobre el uso de la interfaz de usuario web de Apache Ambari](hdinsight-hadoop-manage-ambari.md)
* [Detalles sobre el uso de la API REST de Apache Ambari](hdinsight-hadoop-manage-ambari-rest-api.md)
* [Uso de Apache Hive en HDInsight](hadoop/hdinsight-use-hive.md)
* [Uso de Apache Pig en HDInsight](hadoop/hdinsight-use-pig.md)
* [Uso de Apache Sqoop en HDInsight](hadoop/hdinsight-use-sqoop.md)
* [Introducción a HDInsight de Azure](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [¿Qué versión de Apache Hadoop tiene en Azure HDInsight?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-command-line.png "Línea de comandos de Hadoop"
