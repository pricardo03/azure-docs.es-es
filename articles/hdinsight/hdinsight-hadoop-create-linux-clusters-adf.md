---
title: 'Tutorial: Clústeres de Apache Hadoop a petición en Azure HDInsight: Data Factory'
description: 'Tutorial: Aprenda a crear clústeres de Apache Hadoop a petición en HDInsight mediante Azure Data Factory.'
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.date: 04/18/2019
ms.openlocfilehash: 0b3062e14873ec971163c125fccd6852d8662663
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71098721"
---
# <a name="tutorial-create-on-demand-apache-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Tutorial: Creación de clústeres de Apache Hadoop a petición en HDInsight mediante Azure Data Factory
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

En este tutorial, aprenderá a crear un clúster de [Apache Hadoop](https://hadoop.apache.org/), a petición, en Azure HDInsight mediante Azure Data Factory. A continuación, puede usar canalizaciones en Azure Data Factory para ejecutar trabajos de Hive y eliminar el clúster. Al final de este tutorial, aprenderá a poner en marcha una ejecución de trabajo de Big Data donde la creación del clúster, la ejecución de trabajo y la eliminación del clúster tienen lugar a tiempo.

En este tutorial se describen las tareas siguientes: 

> [!div class="checklist"]
> * Creación de una cuenta de Azure Storage
> * Conocer la actividad de Azure Data Factory
> * Crear una factoría de datos mediante Azure Portal
> * Crear servicios vinculados
> * Crear una canalización
> * Desencadenar una canalización
> * Supervisión de una canalización
> * Comprobación del resultado

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* Instalación del [módulo Az](https://docs.microsoft.com/powershell/azure/overview) de PowerShell.

* Una entidad de servicio de Azure Active Directory. Una vez que haya creado la entidad de servicio, asegúrese de recuperar el **identificador de la aplicación** y la **clave de autenticación** mediante las instrucciones en el artículo vinculado. Necesitará estos valores más adelante en el tutorial. Asimismo, asegúrese de que la entidad de servicio es miembro del rol de *colaborador* de la suscripción o del grupo de recursos en el que se crea el clúster. A fin de obtener instrucciones sobre cómo recuperar los valores necesarios y asignar los roles adecuados, consulte [Creación de una entidad de servicio de Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md).

## <a name="create-preliminary-azure-objects"></a>Creación de objetos de Azure previos

En esta sección, puede crear diversos objetos que se usarán para el clúster de HDInsight que cree a petición. La cuenta de almacenamiento creada contendrá el ejemplo de script de [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) (`partitionweblogs.hql`) que usa para simular un ejemplo de trabajo de [Apache Hive](https://hive.apache.org/) que se ejecuta en el clúster.

En esta sección se usa un script de Azure PowerShell para crear la cuenta de almacenamiento y copiar los archivos necesarios en ella. El script de ejemplo de Azure PowerShell de esta sección realiza las siguientes tareas:

1. Inicia sesión en Azure.
2. Crea un grupo de recursos de Azure.
3. Crea una cuenta de Azure Storage.
4. Crea un contenedor de blobs en la cuenta de almacenamiento.
5. Copia el ejemplo de script de HiveQL (**partitionweblogs.hql**) en el contenedor de blobs. El script está disponible en [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql). El ejemplo de script ya está disponible en otro contenedor de blobs público. El script de PowerShell siguiente realiza una copia de estos archivos en la cuenta de Azure Storage que crea.

> [!WARNING]  
> El tipo de cuenta de almacenamiento `BlobStorage` no se puede usar para los clústeres de HDInsight.

**Para crear una cuenta de almacenamiento y copiar los archivos mediante Azure PowerShell:**

> [!IMPORTANT]  
> Especifique los nombres del grupo de recursos de Azure y la cuenta de Azure Storage que creará el script.
> Anote el **nombre del grupo de recursos**, el **nombre de la cuenta de almacenamiento** y la **clave de la cuenta de almacenamiento** que genere el script. Los necesitará en la siguiente sección.

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfv2hiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}
#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzResourceGroup `
    -Name $resourceGroupName `
    -Location $location

New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -Kind StorageV2 `
    -Location $location `
    -SkuName Standard_LRS `
    -EnableHttpsTrafficOnly 1

$destStorageAccountKey = (Get-AzStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous

$destContext = New-AzStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzStorageContainer `
    -Name $destContainerName `
    -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName

$blobs|Start-AzStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzStorageBlob `
    -Context $destContext `
    -Container $destContainerName
#endregion

Write-host "`nYou will use the following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

**Para comprobar la creación de la cuenta de almacenamiento**

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Seleccione **Grupos de recursos** en el panel izquierdo.
3. Seleccione el nombre del grupo de recursos que creó en el script de PowerShell. Utilice el filtro si se muestran demasiados grupos de recursos.
4. En el icono **Recursos**, podrá ver un recurso a menos que comparta el grupo de recursos con otros proyectos. Ese recurso es la cuenta de almacenamiento con el nombre que especificó anteriormente. Escriba el nombre de la cuenta de almacenamiento.
5. Seleccione los iconos **Blobs**.
6. Seleccione el contenedor **adfgetstarted**. Puede ver una carpeta llamada **hivescripts**.
7. Abra la carpeta y asegúrese de que contiene el ejemplo de archivo de script, **partitionweblogs.hql**.

## <a name="understand-the-azure-data-factory-activity"></a>Conocer la actividad de Azure Data Factory

[Azure Data Factory](../data-factory/introduction.md) organiza y automatiza el movimiento y la transformación de datos. Azure Data Factory puede crear un clúster de Hadoop de HDInsight Just-In-Time para procesar un segmento de datos de entrada y eliminar el clúster cuando el procesamiento está completo. 

En Azure Data Factory, una factoría de datos puede tener una o varias canalizaciones de datos. Una canalización de datos consta de una o varias actividades. Existen dos tipos de actividades:

- [Actividades de movimiento de datos](../data-factory/copy-activity-overview.md): las actividades de movimiento de datos se usan para mover datos de un almacén de datos de origen a un almacén de datos de destino.
- [Actividades de transformación de datos](../data-factory/transform-data.md). Los actividades de transformación de datos se usan para procesar o transformar datos. La actividad de Hive de HDInsight es una de las actividades de transformación de datos compatibles con Data Factory. En este tutorial se usa la actividad de transformación de Hive.

En este artículo, se configura la actividad de Hive para crear un clúster Hadoop de HDInsight a petición. Cuando se ejecuta la actividad para procesar datos, esto es lo que ocurre:

1. Se crea automáticamente un clúster de Hadoop de HDInsight para que just-in-time procese el segmento. 

2. Los datos de entrada se procesan mediante la ejecución de un script de HiveQL en el clúster. En este tutorial, el script de HiveQL asociado con la actividad de Hive realiza las siguientes acciones:

    - Usa la tabla existente (*hivesampletable*) para crear otra tabla **HiveSampleOut**.
    - Rellena la tabla **HiveSampleOut** solo con columnas específicas de la *hivesampletable* original.
    
3. El clúster de Hadoop de HDInsight se elimina cuando se ha completado el procesamiento y el clúster está inactivo durante el período configurado (valor timeToLive). Si el siguiente segmento de datos está disponible para su procesamiento con en este tiempo de inactividad timeToLive, el mismo clúster se usa para procesar el segmento.  

## <a name="create-a-data-factory"></a>Crear una factoría de datos

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

2. En el menú de la izquierda, vaya a la opción **+ Crear un recurso** > **Analytics** > **Data Factory**.

    ![Azure Data Factory en el portal](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-azure-portal.png "Azure Data Factory en el portal")

3. Escriba o seleccione los siguientes valores del icono **Nueva factoría de datos**:

    |Propiedad  |Valor  |
    |---------|---------|
    |Name | Escriba un nombre para la factoría de datos. Este nombre debe ser único globalmente.|
    |Subscription | Seleccione su suscripción a Azure. |
    |Resource group | Seleccione **Usar existente** y, a continuación, seleccione el grupo de recursos que ha creado mediante el script de PowerShell. |
    |Versión | Déjela en **V2**. |
    |Location | La ubicación se establece automáticamente en la ubicación que ha especificado anteriormente durante la creación del grupo de recursos. En este tutorial, la ubicación se establece en **Este de EE. UU.** |

    ![Creación de Azure Data Factory mediante Azure Portal](./media/hdinsight-hadoop-create-linux-clusters-adf/create-data-factory-portal.png "Creación de Azure Data Factory mediante Azure Portal")

4. Seleccione **Crear**. La creación de una factoría de datos podría tardar entre dos y cuatro minutos.

5. Una vez que se cree la factoría de datos, recibirá una notificación de **implementación correcta** con el botón **Ir al recurso**.  Seleccione **Ir al recurso** para abrir la vista predeterminada de Data Factory.

6. Seleccione **Crear y supervisar** para iniciar el portal de creación y supervisión de Azure Data Factory.

    ![Información general del portal de Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-portal-overview.png "Información general de Azure Data Factory")

## <a name="create-linked-services"></a>Crear servicios vinculados

En esta sección, puede crear dos servicios vinculados dentro de su factoría de datos.

- Un **servicio vinculado a Azure Storage** que vincule una cuenta de Azure Storage con la factoría de datos. Este almacenamiento lo usa el clúster HDInsight a petición. También contiene el script de Hive que se ejecuta en el clúster.
- Un **servicio vinculado de HDInsight a petición**. Azure Data Factory permite crear automáticamente un clúster de HDInsight y ejecutar el script de Hive. A continuación, elimina el clúster de HDInsight si el clúster está inactivo durante un tiempo configurado previamente.

### <a name="create-an-azure-storage-linked-service"></a>Creación de un servicio vinculado de Azure Storage

1. En el panel izquierdo de la página de **introducción**, seleccione el icono **Autor**.

    ![Creación de un servicio vinculado de Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-edit-tab.png "Creación de un servicio vinculado de Azure Data Factory")

2. Seleccione **Conexiones** en la esquina inferior izquierda de la ventana y seleccione **+ Nuevo**.

    ![Creación de conexiones en Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-create-new-connection.png "Creación de conexiones en Azure Data Factory")

3. En el cuadro de diálogo **Nuevo servicio vinculado**, seleccione **Azure Blob Storage** y después **Continuar**.

    ![Creación de un servicio vinculado de Azure Storage para Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service.png "Creación de un servicio vinculado de Azure Storage para Data Factory")

4. Proporcione los siguientes valores del servicio vinculado de almacenamiento:

    |Propiedad |Valor |
    |---|---|
    |Name |Escriba `HDIStorageLinkedService`.|
    |Suscripción de Azure |Seleccione la suscripción en la lista desplegable.|
    |Nombre de la cuenta de almacenamiento |Seleccione la cuenta de Azure Storage que creó como parte del script de PowerShell.|

    A continuación, seleccione **Finish** (Finalizar).

    ![Asignación de un nombre para el servicio vinculado de Azure Storage](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service-details.png "Asignación de un nombre para el servicio vinculado de Azure Storage")

### <a name="create-an-on-demand-hdinsight-linked-service"></a>Creación de un servicio vinculado de HDInsight a petición

1. Seleccione el botón **+ New** (+ Nuevo) una vez más para crear otro servicio vinculado.

2. En la ventana **Nuevo servicio vinculado**, seleccione la pestaña **Proceso**.

3. Seleccione **Azure HDInsight** y, a continuación, **Continuar**.

    ![Creación de un servicio vinculado de HDInsight para Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service.png "Creación de un servicio vinculado de HDInsight para Azure Data Factory")

4. En la ventana **Nuevo servicio vinculado**, escriba los siguientes valores y deje el resto como valor predeterminado:

    | Propiedad | Valor |
    | --- | --- |
    | Name | Escriba `HDInsightLinkedService`.|
    | type | Seleccione **HDInsight a petición**. |
    | Servicio vinculado de Azure Storage | Seleccione `HDIStorageLinkedService`. |
    | Tipo de clúster | Seleccione **hadoop** |
    | Período de vida | Proporcione la duración que desea que tenga la disponibilidad del clúster de HDInsight antes de eliminarse automáticamente|
    | Id. de entidad del servicio | Proporcione el identificador de aplicación de la entidad de servicio de Azure Active Directory que ha creado como parte de los requisitos previos. |
    | Clave de entidad de servicio | Proporcione la clave de autenticación para la entidad de servicio de Azure Active Directory. |
    | Prefijo del nombre del clúster | Proporcione un valor que se agregará como prefijo a todos los tipos de clúster creados por la factoría de datos. |
    |Subscription |Seleccione la suscripción en la lista desplegable.|
    | Selección de un grupo de recursos | Seleccione el grupo de recursos que ha creado como parte del script de PowerShell que ha usado anteriormente.|
    |Selección de una región | Seleccione una región en la lista desplegable.|
    | Nombre de usuario de SSH del clúster o tipo de SO | Especifique un nombre de usuario de SSH, habitualmente `sshuser`. |
    | Contraseña de SSH del clúster o tipo de SO | Proporcione una contraseña para el usuario de SSH |
    | Nombre de usuario de clúster o tipo de SO | Escriba un nombre de usuario de clúster, habitualmente `admin`. |
    | Contraseña de usuario del clúster o tipo de SO | Proporcione una contraseña para el usuario del clúster. |

    A continuación, seleccione **Finish** (Finalizar).

    ![Asignación de valores para el servicio vinculado de HDInsight](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service-details.png "Asignación de valores para el servicio vinculado de HDInsight")

## <a name="create-a-pipeline"></a>Crear una canalización

1. Haga clic en el botón **+** (Más) y seleccione **Pipeline** (Canalización).

    ![Creación de una canalización en Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-create-pipeline.png "Creación de una canalización en Azure Data Factory")

2. En el cuadro de herramientas **Actividades**, expanda **HDInsight** y arrastre la actividad **Hive** en la superficie del diseñador de canalizaciones. En la pestaña **General**, proporcione un nombre para la actividad.

    ![Adición de actividades a la canalización de Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-add-hive-pipeline.png "Adición de actividades a la canalización de Data Factory")

3. Asegúrese de que tiene seleccionada la actividad de Hive, seleccione la pestaña **Clúster de HDI** y, en la lista desplegable **Servicio vinculado de HDInsight**, seleccione el servicio vinculado que ha creado anteriormente, **HDinightLinkedService**, para HDInsight.

    ![Facilitación de detalles del clúster de HDInsight para la canalización](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-hive-activity-select-hdinsight-linked-service.png "Facilitación de detalles del clúster de HDInsight para la canalización")

4. Seleccione la pestaña **Script** y complete los siguientes pasos:

    1. En **Servicio vinculado de script**, seleccione **HDIStorageLinkedService** en la lista desplegable. Este valor es el servicio vinculado de almacenamiento que ha creado anteriormente.

    1. En **Ruta de acceso del archivo**, seleccione **Examinar almacenamiento** y vaya a la ubicación donde el ejemplo de script de Hive se encuentra disponible. Si ha ejecutado el script de PowerShell anteriormente, esta ubicación debe ser `adfgetstarted/hivescripts/partitionweblogs.hql`.

        ![Facilitación de detalles del script de Hive para la canalización](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-path.png "Facilitación de detalles del script de Hive para la canalización")

    1. En **Avanzado** > **Parámetros**, seleccione **Rellenado automático a partir de script**. Esta opción busca cualquier parámetro en el script de Hive que necesite valores en tiempo de ejecución. El script que usa (**partitionweblogs.hql**) tiene un parámetro **Output**. Proporcione el **valor** en el formato `wasbs://adfgetstarted@<StorageAccount>.blob.core.windows.net/outputfolder/` para que apunte a una carpeta existente de su instancia de Azure Storage. La ruta de acceso distingue mayúsculas de minúsculas. Esta es la ruta de acceso donde se almacenará la salida del script. El esquema `wasbs` es necesario porque ahora las cuentas de almacenamiento tienen habilitada de forma predeterminada la transferencia segura.
    
        ![Facilitación de parámetros para el script de Hive](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-parameters.png "Facilitación de parámetros para el script de Hive")

1. Seleccione **Validar** para validar la canalización. Seleccione el botón **>>** (flecha derecha) para cerrar la ventana de comprobación.

    ![Validación de la canalización de Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-validate-all.png "Validación de la canalización de Azure Data Factory")

1. Por último, seleccione **Publicar todo** para publicar los artefactos en Azure Data Factory.

    ![Publicación de la canalización de Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-publish-pipeline.png "Publicación de la canalización de Azure Data Factory")

## <a name="trigger-a-pipeline"></a>Desencadenar una canalización

1. En la barra de herramientas de la superficie del diseñador, seleccione **Agregar desencadenador** > **Desencadenar ahora**.

    ![Desencadenamiento de la canalización de Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-trigger-pipeline.png "Desencadenamiento de la canalización de Azure Data Factory")

2. Seleccione **Finalizar** en la barra lateral emergente.

## <a name="monitor-a-pipeline"></a>Supervisión de una canalización

1. Cambie a la pestaña **Monitor** (Supervisar) de la izquierda. Puede ver una ejecución de canalización en la lista **Pipeline Runs** (Ejecuciones de canalización). Tenga en cuenta el estado de la ejecución en la columna **Estado**.

    ![Supervisión de la canalización de Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline.png "Supervisión de la canalización de Azure Data Factory")

1. Seleccione **Refresh** (Actualizar) para actualizar el estado.

1. También puede seleccionar el icono **Ver ejecuciones de actividad** para ver la ejecución de actividad asociada a la canalización. En la siguiente captura de pantalla, verá solo una ejecución de actividad ya que solo hay una actividad en la canalización que ha creado. Para cambiar de nuevo a la vista anterior, seleccione **Canalizaciones** hacia la parte superior de la página.

    ![Supervisión de la actividad de canalización de Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline-activity.png "Supervisión de la actividad de canalización de Azure Data Factory")

## <a name="verify-the-output"></a>Comprobación del resultado

1. Para comprobar el resultado, en Azure Portal, vaya a la cuenta de almacenamiento que ha usado para este tutorial. Debe ver las siguientes carpetas o contenedores:

    - Verá un **adfgerstarted/outputfolder** que contiene el resultado del script de Hive que se ha ejecutado como parte de la canalización.

    - Verá un contenedor **adfhdidatafactory-\<linked-service-name>-\<timestamp>** . Este contenedor es la ubicación de almacenamiento predeterminada del clúster de HDInsight que se ha creado como parte de la ejecución de la canalización.

    - Verá un contenedor **adfjobs** que tiene los registros de trabajo de Azure Data Factory.  

        ![Comprobación de la salida de la canalización de Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-verify-output.png "Comprobación de la salida de la canalización de Azure Data Factory")

## <a name="clean-up-resources"></a>Limpieza de recursos

Con la creación del clúster de HDInsight a petición, no es necesario que elimine de forma explícita el clúster de HDInsight. El clúster se elimina en función de la configuración que ha proporcionado durante la creación de la canalización. Sin embargo, incluso tras la eliminación del clúster, las cuentas de almacenamiento asociadas al clúster continúan existiendo. Este comportamiento es por diseño para que pueda mantener intactos sus datos. Sin embargo, si no desea persistir los datos, puede eliminar la cuenta de almacenamiento que ha creado.

De forma alternativa, puede eliminar todo el grupo de recursos que ha creado para este tutorial. Esto elimina la cuenta de almacenamiento y la instancia de Azure Data Factory que ha creado.

### <a name="delete-the-resource-group"></a>Eliminar el grupo de recursos

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione **Grupos de recursos** en el panel izquierdo.
1. Seleccione el nombre del grupo de recursos que creó en el script de PowerShell. Utilice el filtro si se muestran demasiados grupos de recursos. Se abrirá el grupo de recursos.
1. En el icono **Recursos** , aparecerá la cuenta de almacenamiento predeterminada y Data Factory a menos que comparta el grupo de recursos con otros proyectos.
1. Seleccione **Eliminar grupo de recursos**. Así se eliminan también la cuenta de almacenamiento y los datos almacenados en ella.

    ![Eliminación del grupo de recursos desde Azure Portal](./media/hdinsight-hadoop-create-linux-clusters-adf/delete-resource-group.png "Eliminación del grupo de recursos")

1. Escriba el nombre del grupo de recursos para confirmar la eliminación y, después, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido cómo utilizar Azure Data Factory para crear el clúster de HDInsight a petición y ejecutar los trabajos de [Apache Hive](https://hive.apache.org/). Adelántese al siguiente artículo para aprender a crear clústeres de HDInsight con una configuración personalizada.

> [!div class="nextstepaction"]
>[Creación de clústeres de HDInsight de Azure con una configuración personalizada](hdinsight-hadoop-provision-linux-clusters.md)
