---
title: 'Inicio rápido: Apache Hadoop, Apache Hive y portal de Azure HDInsight'
description: En este inicio rápido, usará Azure Portal para crear un clúster de Hadoop de HDInsight.
keywords: introducción a hadoop, hadoop linux, inicio rápido de hadoop, introducción a hive, inicio rápido en hive
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017,mvc,seodec18
ms.topic: quickstart
ms.date: 09/25/2019
ms.openlocfilehash: 35dc4938760ca83a6781d5791c746ee2f74310ab
ms.sourcegitcommit: f0f73c51441aeb04a5c21a6e3205b7f520f8b0e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77031600"
---
# <a name="quickstart-create-apache-hadoop-cluster-in-azure-hdinsight-using-azure-portal"></a>Inicio rápido: Creación de un clúster de Apache Hadoop en Azure HDInsight mediante Azure Portal

En este artículo, aprenderá a crear clústeres de [Apache Hadoop](https://hadoop.apache.org/) en HDInsight con Azure Portal y a ejecutar trabajos de Apache Hive en HDInsight. La mayoría de los trabajos de Hadoop son por lotes. Se crea un clúster, se ejecutan algunos trabajos y luego se elimina el clúster. En este artículo, realizará las tres tareas.

En este inicio rápido, usará Azure Portal para crear un clúster de Hadoop de HDInsight. También puede crear un clúster con la [plantilla de Azure Resource Manager](apache-hadoop-linux-tutorial-get-started.md).

Actualmente HDInsight tiene [siete tipos diferentes de clúster](../hdinsight-overview.md#cluster-types-in-hdinsight). Cada uno de estos tipos de clúster es compatible con un conjunto de componentes diferente. Todos los tipos de clúster son compatibles con Hive. Para ver una lista de los componentes compatibles con HDInsight, consulte [Novedades en las versiones de clústeres de Apache Hadoop proporcionadas por HDInsight](../hdinsight-component-versioning.md).  

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="create-an-apache-hadoop-cluster"></a>Creación de un clúster de Apache Hadoop

En esta sección, crea un clúster de Hadoop en HDInsight con Azure Portal.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. En Azure Portal, vaya a la opción **Crear un recurso** > **Analytics** > **HDInsight**.

    ![Crear un clúster HDInsight del recurso](./media/apache-hadoop-linux-create-cluster-get-started-portal/create-hdinsight-cluster.png "Crear un clúster HDInsight del recurso")

1. En **Aspectos básicos**, escriba o seleccione los siguientes valores:

    |Propiedad  |Descripción  |
    |---------|---------|
    |Subscription    |  Seleccione su suscripción a Azure. |
    |Resource group     | Cree un grupo de recursos o seleccione uno existente.  Un grupo de recursos es un contenedor de componentes de Azure.  En este caso, el grupo de recursos contiene el clúster de HDInsight y la cuenta de Azure Storage dependiente. |
    |Nombre del clúster   | Escriba el nombre del clúster de Hadoop. Dado que todos los clústeres de HDInsight comparten el mismo espacio de nombres de DNS, este nombre debe ser único. El nombre puede tener un máximo de 59 caracteres, letras, números y guiones incluidos. El primer y el último carácter del nombre no pueden ser guiones. |
    |Location    | Seleccione una ubicación de Azure en la que quiera crear el clúster.  Elija una ubicación más cercana a usted para mejorar el rendimiento. |
    |Tipo de clúster| Seleccione **Seleccionar tipo de clúster**. A continuación, seleccione **Hadoop** como tipo de clúster.|
    |Versión|Se especificará la versión predeterminada para el tipo de clúster. Seleccione en la lista desplegable si desea especificar una versión diferente.|
    |Nombre de usuario y contraseña de inicio de sesión del clúster    | El nombre de inicio de sesión predeterminado es **admin**. La contraseña debe tener un mínimo de 10 caracteres y contener al menos un dígito, una letra mayúscula y una letra minúscula, y un carácter no alfanumérico (excepto los caracteres ' " y `\). Asegúrese de **no proporcionar** contraseñas comunes, como "Pass@word1".|
    |Nombre de usuario de Secure Shell (SSH) | El nombre de usuario predeterminado es **sshuser**.  Puede proporcionar otro nombre para el nombre de usuario de SSH. |
    |Uso de la contraseña de inicio de sesión del clúster para SSH| Seleccione esta casilla para que el usuario de SSH tenga la misma contraseña que la proporcionada para el usuario de inicio de sesión del clúster.|

    ![HDInsight Linux: introducción a la especificación de valores de clúster básicos](./media/apache-hadoop-linux-create-cluster-get-started-portal/azure-portal-cluster-basics-blank.png "Especificación de valores básicos para crear un clúster de HDInsight")

    Seleccione el botón **Siguiente: Almacenamiento >>**  para avanzar a la configuración de almacenamiento.

1. En la pestaña **Almacenamiento**, proporcione los valores siguientes:

    |Propiedad  |Descripción  |
    |---------|---------|
    |Tipo de almacenamiento principal|Use el valor predeterminado **Azure Storage**.|
    |Método de selección|Use el valor predeterminado **Seleccionar de la lista**.|
    |Cuenta de almacenamiento principal|Utilice la lista desplegable para seleccionar una cuenta de almacenamiento existente o bien elija **Crear nuevo**. Si crea una cuenta nueva, el nombre debe tener una longitud de entre 3 y 24 caracteres y solo puede contener números y letras minúsculas.|
    |Contenedor|Use el valor que se rellena automáticamente.|

    ![HDInsight Linux: introducción a la especificación de valores de almacenamiento en clúster](./media/apache-hadoop-linux-create-cluster-get-started-portal/azure-portal-cluster-storage.png "Especificación de valores de almacenamiento para crear un clúster de HDInsight")

    Seleccione la pestaña **Revisar y crear**.

1. En la pestaña **Revisar y crear**, compruebe los valores seleccionados en los pasos anteriores.

    ![HDInsight Linux: introducción a los resúmenes de clústeres](./media/apache-hadoop-linux-create-cluster-get-started-portal/azure-portal-cluster-review-create-hadoop.png "HDInsight Linux: introducción a los resúmenes de clústeres")

1. Seleccione **Crear**. Se tarda aproximadamente 20 minutos en crear un clúster.

Una vez creado el clúster, verá la página de información general del clúster en Azure Portal.

![HDInsight Linux: introducción a la configuración de clústeres](./media/apache-hadoop-linux-create-cluster-get-started-portal/cluster-settings-overview.png "Propiedades del clúster de HDInsight")

Cada clúster depende de una [cuenta de Azure Storage](../hdinsight-hadoop-use-blob-storage.md) o de una [cuenta de Azure Data Lake](../hdinsight-hadoop-use-data-lake-store.md). Se conoce como cuenta de almacenamiento predeterminada. El clúster de HDInsight y su cuenta de almacenamiento predeterminada deben estar en la misma región de Azure. Al eliminar los clústeres no se elimina la cuenta de almacenamiento.

> [!NOTE]  
> Para conocer otros métodos de creación de clústeres y las propiedades que se usan en este inicio rápido, vea [Creación de clústeres en HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="run-apache-hive-queries"></a>Ejecución de consultas de Apache Hive

[Apache Hive](hdinsight-use-hive.md) es el componente más popular de los que se usan en HDInsight. Hay muchas maneras de ejecutar trabajos de Hive en HDInsight. En este inicio rápido se usa la vista de Hive de Ambari desde el portal. Para conocer otros métodos para enviar trabajos de Hive, consulte [Uso de Hive en HDInsight](hdinsight-use-hive.md).

1. Para abrir Ambari, desde la captura de pantalla anterior, seleccione **Panel de clúster**.  También puede ir a `https://ClusterName.azurehdinsight.net`, donde `ClusterName` es el clúster que creó en la sección anterior.

    ![HDInsight Linux: introducción al panel de clústeres](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-linux-get-started-open-cluster-dashboard.png "HDInsight Linux: introducción al panel de clústeres")

2. Escriba el nombre de usuario de Hadoop y la contraseña que especificó al crear el clúster. El nombre de usuario predeterminado es **admin**.

3. Abra la **vista de Hive** como se muestra en la siguiente captura de pantalla:

    ![Selección de la vista de Hive de Ambari](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdi-select-hive-view.png "Menú de vista de Hive de HDInsight")

4. En la pestaña **CONSULTA**, pegue las instrucciones HiveQL siguientes en la hoja de cálculo:

    ```sql
    SHOW TABLES;
    ```

    ![Editor de consultas de vistas de Hive de HDInsight](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdi-apache-hive-view1.png "Editor de consultas de la vista de Hive de HDInsight")

5. Seleccione **Execute**(Ejecutar). Aparecerá una pestaña **RESULTADOS** en la pestaña **CONSULTA** que mostrará información sobre el trabajo. 

    Cuando haya finalizado la consulta, la pestaña **CONSULTA** muestra los resultados de la operación. Verá una tabla denominada **hivesampletable**. Esta es una tabla de Hive de ejemplo que viene integrada en todos los clústeres de HDInsight.

    ![Resultados de la vista de Apache Hive en HDInsight](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-hive-views.png "Resultados de la vista de Apache Hive en HDInsight")

6. Repita los pasos 4 y 5 para ejecutar la consulta siguiente:

    ```sql
    SELECT * FROM hivesampletable;
    ```

7. También puede guardar los resultados de la consulta. Seleccione el botón de menú de la derecha y especifique si quiere descargar los resultados como archivo CSV o almacenarlos en la cuenta de almacenamiento asociada al clúster.

    ![Guardar el resultado de la consulta de Apache Hive](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-linux-hive-view-save-results.png "Guardar el resultado de la consulta de Apache Hive")

Después de completar un trabajo de Hive, puede [exportar los resultados a una base de datos de Azure SQL o SQL Server](apache-hadoop-use-sqoop-mac-linux.md), y también puede [visualizar los resultados con Excel](apache-hadoop-connect-excel-power-query.md). Para más información sobre el uso de Hive en HDInsight, consulte [Uso de Apache Hive y HiveQL con Apache Hadoop en HDInsight para analizar un archivo log4j de Apache de muestra](hdinsight-use-hive.md).

## <a name="clean-up-resources"></a>Limpieza de recursos

Después de completar el inicio rápido, puede ser conveniente eliminar el clúster. Con HDInsight, los datos se almacenan en Azure Storage, por lo que puede eliminar un clúster de forma segura cuando no se esté usando. También se le cobrará por un clúster de HDInsight aunque no se esté usando. Como en muchas ocasiones los cargos por el clúster son mucho más elevados que los cargos por el almacenamiento, desde el punto de vista económico tiene sentido eliminar clústeres cuando no se estén usando.

> [!NOTE]  
> Si avanza de *inmediato* al siguiente artículo para aprender a ejecutar operaciones de ETL con Hadoop en HDInsight, es posible que quiera mantener el clúster en ejecución. Esto es porque en el tutorial tendrá que crear un clúster de Hadoop de nuevo. Sin embargo, si no va a continuar con el próximo artículo de inmediato, debe eliminar el clúster ahora.

### <a name="to-delete-the-cluster-andor-the-default-storage-account"></a>Para eliminar el clúster o la cuenta de almacenamiento predeterminada

1. Vuelva a la pestaña de explorador en la que tenga Azure Portal. Estará en la página de información general del clúster. Si solo quiere eliminar el clúster, pero desea seguir conservando la cuenta de almacenamiento predeterminada, seleccione **Eliminar**.

    ![Azure HDInsight: eliminación de clústeres](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-delete-cluster.png "Eliminar clúster de Azure HDInsight")

2. Si quiere eliminar el clúster y la cuenta de almacenamiento predeterminada, seleccione el nombre del grupo de recursos (resaltado en la captura de pantalla anterior) para abrir la página del grupo de recursos.

3. Seleccione **Eliminar grupo de recursos** para eliminar el grupo de recursos, que contiene el clúster y la cuenta de almacenamiento predeterminada. Tenga en cuenta que, al eliminar el grupo de recursos, se elimina también la cuenta de almacenamiento. Si desea mantener la cuenta de almacenamiento, elija eliminar solo el clúster.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha aprendido a crear un clúster de HDInsight basado en Linux mediante una plantilla de Resource Manager y a realizar consultas básicas de Hive. En el siguiente artículo, aprenderá a realizar una operación ETL (extraer, transformar y cargar) con Hadoop en HDInsight.

> [!div class="nextstepaction"]
> [Extracción, transformación y carga de datos mediante Interactive Query en HDInsight](../interactive-query/interactive-query-tutorial-analyze-flight-data.md)
