---
title: Uso de Azure Data Factory para copiar archivos nuevos de forma incremental solo por el nombre de archivo con particiones de tiempo | Microsoft Docs
description: Cree una instancia de Azure Data Factory y, luego, use la herramienta Copiar datos para cargar incrementalmente archivos nuevos solo por el nombre de archivo con particiones de tiempo.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 1/24/2019
ms.openlocfilehash: c89764d746f07e6100b1f250d4c107bb700fe014
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61099079"
---
# <a name="incrementally-copy-new-files-based-on-time-partitioned-file-name-by-using-the-copy-data-tool"></a>Copia incremental de nuevos archivos por el nombre de archivo con particiones de tiempo mediante la herramienta Copiar datos

En este tutorial, usará Azure Portal para crear una factoría de datos. Luego, use la herramienta Copiar datos para crear una canalización que copie incrementalmente los archivos nuevos por el nombre de archivo con particiones de tiempo de una instancia de Azure Blob Storage a otra. 

> [!NOTE]
> Si no está familiarizado con Azure Data Factory, consulte [Introducción a Azure Data Factory](introduction.md).

En este tutorial, realizará los siguientes pasos:

> [!div class="checklist"]
> * Creación de una factoría de datos.
> * Uso de la herramienta Copy Data para crear una canalización.
> * Supervisión de las ejecuciones de canalización y actividad.

## <a name="prerequisites"></a>Requisitos previos

* **Suscripción de Azure**: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* **Cuenta de Azure Storage**: Use Blob Storage como almacén de datos de _origen_ y _receptor_. Si no dispone de una cuenta de almacenamiento de Azure, consulte las instrucciones de [Creación de una cuenta de almacenamiento](../storage/common/storage-quickstart-create-account.md).

### <a name="create-two-containers-in-blob-storage"></a>Crear dos contenedores en Azure Blob Storage

Haga lo siguiente para preparar su instancia de Blob Storage para el tutorial.

1. Cree un contenedor denominado **source**.  Cree una ruta de acceso de carpeta como **2019/02/26/14** en el contenedor. Cree un archivo de texto vacío y asígnele el nombre **file1.txt**. Cargue el archivo file1.txt en la ruta de acceso de carpeta **source/2019/02/26/14** en su cuenta de almacenamiento.  Puede usar varias herramientas para realizar estas tareas, como el [Explorador de Azure Storage](https://storageexplorer.com/).
    
    ![cargar archivos](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/upload-file.png)
    
    > [!NOTE]
    > Ajuste el nombre de carpeta con la hora UTC correspondiente.  Por ejemplo, si la hora UTC actual es 2:03 p. m. del 26 de febrero de 2019, puede crear la ruta de acceso de carpeta como **source/2019/02/26/14/** por la regla de **source/{Year}/{Month}/{Day}/{Hour}/** .

2. Cree un contenedor denominado **destination**. Puede usar varias herramientas para realizar estas tareas, como el [Explorador de Azure Storage](https://storageexplorer.com/).

## <a name="create-a-data-factory"></a>Crear una factoría de datos

1. En el menú de la izquierda, seleccione **Crear un recurso** > **Datos y análisis** > **Data Factory**: 
   
   ![Selección de la factoría de datos en el panel Nuevo](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. En la página **Nueva factoría de datos**, en **Nombre**, escriba **ADFTutorialDataFactory**. 
      
    ![Nueva factoría de datos](./media/tutorial-copy-data-tool/new-azure-data-factory.png)
    
    El nombre de la factoría de datos debe ser _globalmente único_. Puede aparecer el siguiente mensaje de error:
   
   ![Mensaje de error de nueva factoría de datos](./media/tutorial-copy-data-tool/name-not-available-error.png)
   
   Si recibe un mensaje de error sobre el valor de nombre, escriba un nombre diferente para la factoría de datos. Por ejemplo, utilice _**suNombre**_ **ADFTutorialDataFactory**. Para conocer las reglas de nomenclatura de los artefactos de Data Factory, consulte [Data Factory: reglas de nomenclatura](naming-rules.md).
3. Seleccione la **suscripción** de Azure en la que quiere crear la nueva factoría de datos. 
4. Para **Grupo de recursos**, realice uno de los siguientes pasos:
     
    a. Seleccione en primer lugar **Usar existente**y después un grupo de recursos de la lista desplegable.

    b. Seleccione **Crear nuevo**y escriba el nombre de un grupo de recursos. 
         
    Para más información sobre los grupos de recursos, consulte [Uso de grupos de recursos para administrar los recursos de Azure](../azure-resource-manager/resource-group-overview.md).

5. En **Versión**, seleccione **V2** como versión.
6. En **Ubicación**, seleccione la ubicación de la factoría de datos. Solo las ubicaciones admitidas se muestran en la lista desplegable. Los almacenes de datos (por ejemplo, Azure Storage y SQL Database) y los procesos (por ejemplo, Azure HDInsight) que usa la factoría de datos pueden estar en otras ubicaciones o regiones.
7. Seleccione **Anclar al panel**. 
8. Seleccione **Crear**.
9. En el panel, el icono **Deploying Data Factory** (Implementando Data Factory) muestra el estado del proceso.

    ![Icono de implementación de una factoría de datos](media/tutorial-copy-data-tool/deploying-data-factory.png)
10. Una vez finalizada la creación, se muestra la página principal de **Data Factory**.
   
    ![Página principal Factoría de datos](./media/tutorial-copy-data-tool/data-factory-home-page.png)
11. Para abrir la interfaz de usuario de Azure Data Factory en otra pestaña, seleccione el icono **Author & Monitor** (Creación y supervisión). 

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Uso de la herramienta Copy Data para crear una canalización

1. En la página **Comencemos**, seleccione el título **Copiar datos** para iniciar la herramienta Copiar datos. 

   ![Icono de la herramienta Copy Data](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
   
2. En la página **Propiedades**, realice los pasos siguientes:

    a. En **Nombre de la tarea**, escriba **DeltaCopyFromBlobPipeline**.

    b. En **Task cadence o Task schedule** (Cadencia de tareas o Programación de tareas), seleccione **Run regularly on schedule** (Ejecutar periódicamente según programación).

    c. En **Tipo de desencadenador**, seleccione **Tumbling Window** (Ventana de saltos de tamaño constante).
    
    d. En **Periodicidad**, establezca **1 hora**. 
    
    e. Seleccione **Next** (Siguiente). 
    
    La interfaz de usuario de Data Factory crea una canalización con el nombre de la tarea especificado. 

    ![Página de propiedades](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/copy-data-tool-properties-page.png)
3. En la página **Almacén de datos de origen**, realice los pasos siguientes:

    a. Haga clic en **+ Crear nueva conexión** para agregar una conexión.

    ![Página Source data store (Almacén de datos de origen)](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page.png)
    
    b. Seleccione **Azure Blob Storage** en la galería y, a continuación, haga clic en **Continuar**.

    ![Página Source data store (Almacén de datos de origen)](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page-select-blob.png)
    
    c. En la página **New Linked Service** (Nuevo servicio vinculado), seleccione la cuenta de almacenamiento de la lista **Nombre de la cuenta de almacenamiento** y, después, haga clic en **Finalizar**.
    
    ![Página Source data store (Almacén de datos de origen)](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page-linkedservice.png)
    
    d. Seleccione el servicio vinculado recién creado y, a continuación, haga clic en **Siguiente**. 
    
   ![Página Source data store (Almacén de datos de origen)](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page-select-linkedservice.png)
4. En la página **Choose the input file or folder** (Elegir el archivo o la carpeta de entrada), lleve a cabo los siguientes pasos:
    
    a. Busque y seleccione el contenedor **source** y, después, seleccione **Elegir**.
    
    ![Seleccione el archivo o la carpeta de entrada.](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/choose-input-file-folder.png)
    
    b. En **File loading behavior** (Comportamiento de carga de archivos), seleccione **Incremental load: time-partitioned folder/file names** (Carga Incremental: nombres de archivo/carpeta con particiones de tiempo).
    
    ![Seleccione el archivo o la carpeta de entrada.](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/choose-loading-behavior.png)
    
    c. Escriba la ruta de acceso de carpeta dinámica como **source/{year}/{month}/{day}/{hour}/** y cambie el formato de la siguiente manera:
    
    ![Seleccione el archivo o la carpeta de entrada.](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/input-file-name.png)
    
    d. Seleccione **Binary copy** (Copia binaria) y haga clic en **Siguiente**.
    
    ![Seleccione el archivo o la carpeta de entrada.](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/check-binary-copy.png)     
5. En la página **Destination data store** (Almacén de datos de destino), seleccione el elemento **AzureBlobStorage**, que es la misma cuenta de almacenamiento que el almacén de origen de datos y, a continuación, haga clic en **Siguiente**.

    ![Página Destination data store (Almacén de datos de destino)](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/destination-data-store-page-select-linkedservice.png) 
6. En la página **Choose the output file or folder** (Elegir el archivo o la carpeta de salida), realice los pasos siguientes:
    
    a. Busque y seleccione la carpeta **destination** y, después, seleccione **Elegir**.
    
    ![Elección del archivo o la carpeta de salida](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/choose-output-file-folder.png)   
    
    b. Escriba la ruta de acceso de carpeta dinámica como **source/{year}/{month}/{day}/{hour}/** y cambie el formato de la siguiente manera:
    
    ![Elección del archivo o la carpeta de salida](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/input-file-name2.png)    
    
    c. Haga clic en **Next**.
    
    ![Elección del archivo o la carpeta de salida](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/click-next-after-output-folder.png)  
7. En la página **Settings** (Configuración), seleccione **Next** (Siguiente). 

    ![Página Configuración](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/settings-page.png)  
8. En la página **Summary** (Resumen), revise la configuración y seleccione **Next** (Siguiente).

    ![Página de resumen](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/summary-page.png)
    
9. En la página **Deployment** (Implementación), seleccione **Monitor** (Supervisión) para supervisar la canalización (tarea).
    ![Página de implementación](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/deployment-page.png)
    
10. Observe que la pestaña **Monitor** (Supervisión) de la izquierda se selecciona automáticamente.  Debe esperar a que la canalización se ejecute cuando se desencadene automáticamente (después de una hora aproximadamente).  Cuando se ejecuta, la columna **Acciones** incluye los vínculos para ver los detalles de la ejecución de actividad y volver a ejecutar la canalización. Seleccione **Refresh** (Actualizar) para actualizar la lista y, luego, seleccione el vínculo **View Activity Runs** (Ver ejecuciones de actividad) en la columna **Actions** (Acciones). 

    ![La supervisión de la canalización se ejecuta](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs1.png)
11. Como solo hay una actividad (actividad de copia) en la canalización, solo verá una entrada. Puede ver que el archivo de origen (file1.txt) se ha copiado de **source/2019/02/26/14/** a **destination/2019/02/26/14/** con el mismo nombre de archivo.  

    ![La supervisión de la canalización se ejecuta](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs2.png)
    
    Esto también se puede confirmar usando el Explorador de Azure Storage(https://storageexplorer.com/) ) para examinar los archivos.
    
    ![La supervisión de la canalización se ejecuta](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs3.png)
12. Cree otro archivo de texto vacío con el nuevo nombre **file2.txt**. Cargue el archivo file2.txt en la ruta de acceso de carpeta **source/2019/02/26/15** en su cuenta de almacenamiento.   Puede usar varias herramientas para realizar estas tareas, como el [Explorador de Azure Storage](https://storageexplorer.com/).   
    
    ![La supervisión de la canalización se ejecuta](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs4.png)
    
    > [!NOTE]
    > Es posible que ya sepa que debe crear una nueva ruta de acceso de carpeta. Ajuste el nombre de carpeta con la hora UTC correspondiente.  Por ejemplo, si la hora UTC actual es 3:20 p. m. del 26 de febrero de 2019, puede crear la ruta de acceso de carpeta como **source/2019/02/26/15/** por la regla de **source/{Year}/{Month}/{Day}/{Hour}/** .
    
13. Para volver a la vista **Pipeline Runs** (Ejecuciones de canalizaciones), seleccione **All Pipeline Runs** (Todas las ejecuciones de canalizaciones) y espere a que la misma canalización se desencadene de nuevo automáticamente al cabo de una hora.  

    ![La supervisión de la canalización se ejecuta](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs5.png)

14. Seleccione **View Activity Run** (Ver ejecución de actividad) para la segunda ejecución de canalización cuando se realice y repita el proceso para revisar los detalles.  

    ![La supervisión de la canalización se ejecuta](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs6.png)
    
    Puede ver que el archivo de origen (file2.txt) se ha copiado de **source/2019/02/26/15/** a **destination/2019/02/26/15/** con el mismo nombre de archivo.
    
    ![La supervisión de la canalización se ejecuta](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs7.png) 
    
    Esto también se puede confirmar usando el Explorador de Azure Storage (https://storageexplorer.com/) ) para examinar los archivos en el contenedor **destination**.
    
    ![La supervisión de la canalización se ejecuta](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs8.png)

    
## <a name="next-steps"></a>Pasos siguientes
Pase al tutorial siguiente para obtener información acerca de la transformación de datos mediante el uso de un clúster de Spark en Azure:

> [!div class="nextstepaction"]
>[Transformar datos con un clúster de Spark en la nube](tutorial-transform-data-spark-portal.md)