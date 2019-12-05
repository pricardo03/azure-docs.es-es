---
title: 'Creación de clústeres de Apache Hadoop con un explorador web: Azure HDInsight'
description: Aprenda a crear clústeres de Apache Hadoop, Apache HBase, Apache Storm o Apache Spark en Linux para HDInsight con un explorador web y Azure Portal.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/13/2019
ms.openlocfilehash: 820ddb8d06cfd2aac2b053305f23ad330e4fd7c3
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74215910"
---
# <a name="create-linux-based-clusters-in-hdinsight-by-using-the-azure-portal"></a>Creación de clústeres basados en Linux en HDInsight mediante Azure Portal

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

El Portal de Azure es una herramienta de administración basada en web para servicios y recursos hospedados en la nube de Microsoft Azure. En este artículo aprenderá a crear clústeres de Azure HDInsight basados en Linux mediante el portal.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Requisitos previos

* **Una suscripción de Azure**. Consulte [cómo obtener la evaluación gratuita de Azure para probar Hadoop en HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Un explorador web moderno**. Azure Portal usa HTML5 y JavaScript. Puede que no funcione correctamente en exploradores web anteriores.

## <a name="create-clusters"></a>Creación de clústeres

El Portal de Azure expone la mayor parte de las propiedades del clúster. Mediante las plantillas de Azure Resource Manager puede ocultar una gran cantidad de detalles. Para más información, consulte [Creación de clústeres de Apache Hadoop en HDInsight con plantillas de Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. En el menú de la izquierda, vaya a **+ Crear un recurso** >  **Analytics** > **Azure HDInsight**.

    ![Creación de un clúster en Azure Portal](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster.png "Creación de un clúster en Azure Portal")

1. En la página **Create HDInsight cluster** (Crear clúster de HDInsight), seleccione **Go to classic create experience** (Ir a la experiencia de creación clásica).

    ![Vaya a la experiencia de creación clásica](./media/hdinsight-hadoop-create-linux-clusters-portal/azure-portal-cluster-create-classic.png)

1. En la página **HDInsight**, seleccione **Personalizado (tamaño, configuración, aplicaciones)** .

1. Seleccione **1 Datos básicos**. A continuación, escriba la siguiente información:

    |Propiedad |DESCRIPCIÓN |
    |---|---|
    |Nombre del clúster|Este nombre debe ser único globalmente.|
    |Subscription|En la lista desplegable, seleccione la suscripción de Azure que se usa para el clúster.|
    |Tipo de clúster|Seleccione el tipo de clúster que desea crear. Hadoop y Apache Spark son algunos ejemplos. El **sistema operativo** será **Linux**. A continuación, seleccione una versión del tipo de clúster. Use la versión predeterminada si no sabe qué opción elegir. Para obtener más información, consulte [Versiones de clústeres de HDInsight](hdinsight-component-versioning.md).|
    |Nombre de usuario de inicio de sesión del clúster|Proporcione el nombre de usuario; el valor predeterminado es **admin**.|
    |Contraseña de inicio de sesión de clúster|Proporcione la contraseña.|
    |Nombre de usuario de Secure Shell (SSH)|El valor predeterminado es **sshuser**. Si desea la misma contraseña de SSH que la contraseña de administrador que especificó anteriormente, active la casilla **Usar la contraseña de inicio de sesión del clúster para SSH**. De no ser así, especifique una **CONTRASEÑA** o **CLAVE PÚBLICA** para autenticar al usuario de SSH. Es recomendable usar una clave pública. Elija **Seleccionar** en la parte inferior para guardar la configuración de las credenciales.  Para más información, consulte [Connect to HDInsight (Apache Hadoop) by using SSH](hdinsight-hadoop-linux-use-ssh-unix.md) (Conexión a través de SSH con HDInsight [Apache Hadoop]).|
    |Resource group|Especifique si desea crear un nuevo grupo de recursos o utilizar uno existente.|
    |Location|Especifique un centro de datos en el que crear el clúster.|

    ![Conceptos básicos de creación de clústeres de HDInsight](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-basics.png "Creación de un clúster en Azure Portal")

    > [!IMPORTANT]  
    > Los clústeres de HDInsight se presentan como distintos tipos. Estos corresponden a la carga de trabajo o la tecnología para los que el clúster está optimizado. No hay ningún método compatible para crear un clúster que combine varios tipos. Por ejemplo, Storm y HBase en un clúster.

    Seleccione **Siguiente** para ir a la página siguiente.

1. En **2 Seguridad y redes**, puede conectar el clúster a una red virtual mediante el menú desplegable proporcionado. Seleccione una red virtual de Azure y la subred si quiere colocar el clúster en una red virtual. Para más información acerca del uso de HDInsight con una red virtual, consulte [Planeamiento de la implementación de una red virtual para clústeres de Azure HDInsight](hdinsight-plan-virtual-network-deployment.md). El artículo incluye los requisitos de configuración específicos de la red virtual.

    Si quiere usar **Enterprise Security Package**, siga estas instrucciones: [Configuración de un clúster de HDInsight con Enterprise Security Package mediante Azure Active Directory Domain Services](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds).

    Seleccione **Siguiente** para ir a la página siguiente.

1. En **3 Almacenamiento**, para **Configuración de la cuenta de almacenamiento**, especifique si desea Azure Storage o Azure Data Lake Storage como almacenamiento predeterminado. Para más información, vea la tabla siguiente.

    | Tipo de almacenamiento principal | DESCRIPCIÓN |
    |------------------|-------------|
    | Azure Storage   |  * Para **Método de selección**, elija **Mis suscripciones** si desea especificar una cuenta de almacenamiento que forma parte de su suscripción de Azure. Después, seleccione la cuenta de almacenamiento. De lo contrario, seleccione **Clave de acceso**. A continuación, proporcione la información de la cuenta de almacenamiento que desee elegir fuera de la suscripción de Azure.</br></br> * Para **Contenedor predeterminado**, elija el nombre del contenedor predeterminado sugerido por el portal o especifique el suyo propio.</br></br> * Si Azure Blob Storage es el almacenamiento predeterminado, también puede seleccionar **Cuentas de almacenamiento adicionales** para especificar cuentas de almacenamiento adicionales para asociar con el clúster. Para **Claves de Azure Storage**, seleccione **Add a storage key** (Agregar una clave de almacenamiento). A continuación, puede proporcionar una cuenta de almacenamiento de las suscripciones de Azure o de otras. Proporcione la clave de acceso de la cuenta de almacenamiento.</br></br> * Si Blob Storage es el almacenamiento predeterminado, también puede seleccionar **Acceso a Data Lake Storage** para especificar Azure Data Lake Storage como almacenamiento adicional. Para más información, consulte [Inicio rápido: Configuración de clústeres en HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).</li></ul> |
    | Azure Data Lake Storage | Seleccione **Azure Data Lake Storage Gen1** o **Azure Data Lake Storage Gen2**. A continuación, consulte el artículo [Inicio rápido: Creación de un clúster de HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md) para más información. |

    **Configuración de MetaStore (opcional)**

    De manera opcional, especifique una base de datos SQL para guardar los metadatos de Apache Hive y Apache Oozie asociados con el clúster. Para **Seleccionar una base de datos SQL para Hive**, seleccione una base de datos SQL. Después, proporcione el nombre de usuario y la contraseña de la base de datos. Repita estos pasos para los metadatos de Oozie.

    Estas son algunas consideraciones sobre el uso de Azure SQL Database para las Metastore:
    * La base de datos de Azure SQL usada para Metastore debe permitir la conectividad con otros servicios de Azure, incluido Azure HDInsight. En el lado derecho del panel de Azure SQL Database, seleccione el nombre de servidor. Este servidor donde se ejecuta la instancia de SQL Database. Una vez ya en la vista del servidor, seleccione **Configurar**. A continuación, para **Servicios de Azure**, seleccione **Sí**. Después, seleccione **Guardar**.
    * Al crear una instancia de Metastore, no asigne nombre a una base de datos con guiones. Estos caracteres podrían provocar que el proceso de creación del clúster produzca errores.

    ![Almacenamiento en creación de clústeres de HDInsight](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-storage.png "Creación de un clúster en Azure Portal")

    > [!WARNING]  
    > No se admite el uso de una cuenta de almacenamiento adicional en una ubicación diferente a la del clúster de HDInsight.

    Seleccione **Siguiente** para ir a la página siguiente.

1. En **4 Aplicaciones (opcional)** , seleccione las aplicaciones que desee. De Microsoft, de proveedores de software independientes (ISV) o también puede desarrollarlas. Para más información, consulte [Instalación de aplicaciones durante la creación del clúster](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation).

    Seleccione **Siguiente** para ir a la página siguiente.

1. Haga clic en **5 Tamaño del clúster** para mostrar información sobre los nodos que se usarán para este clúster. Establezca el número de nodos de trabajo que necesita para el clúster. También se muestra el costo estimado de la ejecución del clúster.

    ![Nodos de creación de clústeres de HDInsight](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-nodes.png "Especificación del número de nodos del clúster")

   > [!IMPORTANT]  
   > Si planea crear más de 32 nodos de trabajo, seleccione un tamaño de nodo principal con al menos ocho núcleos y 14 GB de RAM. Planee los nodos al crear el clúster o mediante el escalado del clúster después de la creación.
   >
   > Para más información sobre los tamaños de nodo y los costos asociados, consulte [Precios de Azure HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

    Seleccione **Siguiente** para ir a la página siguiente.

1. En **6 Acciones de script**, puede personalizar un clúster para instalar componentes personalizados. Esta opción funciona si quiere usar un script personalizado para personalizar un clúster, conforme se crea el clúster. Para más información sobre las acciones de script, consulte [Personalización de clústeres de HDInsight basados en Linux mediante acciones de script](hdinsight-hadoop-customize-cluster-linux.md).

   Seleccione **Siguiente** para ir a la página siguiente.

1. En **7 Resumen**, verifique la información proporcionada anteriormente. Seleccione **Crear**.

    ![Resumen de creación de clústeres de HDInsight](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-summary.png "Especificación del número de nodos del clúster")

    > [!NOTE]  
    > El clúster tarda algo de tiempo en crearse, normalmente unos 20 minutos. Supervise las **notificaciones** para comprobar el proceso de aprovisionamiento.

1. Una vez completado el proceso de creación, seleccione **Ir al recurso** en la notificación **Implementación correcta**. La ventana del clúster ofrece la siguiente información.

    ![Información general de HDI en Azure Portal](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-completed.png "Propiedades de clúster")

    Algunos de los iconos de la ventana se explican de la siguiente manera:

    |Propiedad | DESCRIPCIÓN |
    |---|---|
    |Información general|Proporciona toda la información esencial sobre el clúster. Algunos ejemplos son el nombre, el grupo de recursos al que pertenece, la ubicación, el sistema operativo y la dirección URL del panel del clúster.|
    |Paneles de clúster|Le dirige al portal de Ambari asociado al clúster.|
    |SSH e inicio de sesión del clúster|Proporciona la información necesaria para acceder al clúster mediante SSH.|
    |Eliminar|Elimina el clúster de HDInsight.|

## <a name="customize-clusters"></a>Personalización de los clústeres

* [Personalización de los clústeres de HDInsight con Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md)
* [Personalización de clústeres de HDInsight basados en Linux mediante acciones de script](hdinsight-hadoop-customize-cluster-linux.md)

## <a name="delete-the-cluster"></a>Eliminación del clúster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Solución de problemas

Si experimenta problemas con la creación de clústeres de HDInsight, consulte los [requisitos de control de acceso](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="next-steps"></a>Pasos siguientes

Ha creado correctamente un clúster de HDInsight. Ahora puede aprender a trabajar con el clúster.

### <a name="apache-hadoop-clusters"></a>Clústeres de Apache Hadoop

* [Uso de Apache Hive con HDInsight](hadoop/hdinsight-use-hive.md)
* [Uso de MapReduce con HDInsight](hadoop/hdinsight-use-mapreduce.md)

### <a name="apache-hbase-clusters"></a>Clústeres de Apache HBase

* [Introducción a Apache HBase en HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md)
* [Desarrollo de aplicaciones de Java para Apache HBase en HDInsight](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="apache-storm-clusters"></a>Clústeres de Apache Storm

* [Desarrollo de topologías de Java para Apache Storm en HDInsight](storm/apache-storm-develop-java-topology.md)
* [Uso de componentes de Python en Apache Storm en HDInsight](storm/apache-storm-develop-python-topology.md)
* [Implementación y supervisión de topologías con Apache Storm en HDInsight](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="apache-spark-clusters"></a>Clústeres de Apache Spark

* [Creación de una aplicación independiente con Scala](spark/apache-spark-create-standalone-application.md)
* [Ejecución de trabajos de forma remota en un clúster de Apache Spark mediante Apache Livy](spark/apache-spark-livy-rest-interface.md)
* [Apache Spark con BI: Análisis de datos interactivos con Spark en HDInsight con las herramientas de BI](spark/apache-spark-use-bi-tools.md)
* [Apache Spark con Machine Learning: uso de Spark en HDInsight para predecir los resultados de la inspección de alimentos](spark/apache-spark-machine-learning-mllib-ipython.md)
