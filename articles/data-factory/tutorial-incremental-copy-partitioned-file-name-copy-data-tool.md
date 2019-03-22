---
title: Solo mediante Azure Data Factory para copiar archivos nuevos de forma incremental basado en nombre de archivo con particiones de tiempo | Microsoft Docs
description: Cree una factoría de datos de Azure y, a continuación, utilice la herramienta Copy Data para cargar incrementalmente los nuevos archivos de solo según el nombre de archivo con particiones de tiempo.
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
ms.openlocfilehash: df1542d6d20120a9b1e087fadf3743479ecebf07
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57533840"
---
# <a name="incrementally-copy-new-files-based-on-time-partitioned-file-name-by-using-the-copy-data-tool"></a>Copia incremental de archivos nuevo basados en nombre de archivo con particiones de tiempo mediante la herramienta Copy Data

En este tutorial, usará Azure Portal para crear una factoría de datos. Después, usar la herramienta Copy Data para crear una canalización que copia nuevos archivos según el nombre de archivo con particiones de tiempo desde Azure Blob storage a Azure Blob storage de forma incremental. 

> [!NOTE]
> Si no está familiarizado con Azure Data Factory, consulte [Introducción a Azure Data Factory](introduction.md).

En este tutorial, realizará los siguientes pasos:

> [!div class="checklist"]
> * Creación de una factoría de datos.
> * Uso de la herramienta Copy Data para crear una canalización.
> * Supervisión de las ejecuciones de canalización y actividad.

## <a name="prerequisites"></a>Requisitos previos

* **Suscripción de Azure**: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* **Cuenta de Azure Storage**: Uso de Blob storage como el _origen_ y _receptor_ almacén de datos. Si no dispone de una cuenta de almacenamiento de Azure, consulte las instrucciones de [Creación de una cuenta de almacenamiento](../storage/common/storage-quickstart-create-account.md).

### <a name="create-two-containers-in-blob-storage"></a>Cree dos contenedores en Blob storage

Preparar el almacenamiento de blobs para el tutorial mediante los pasos siguientes.

1. Crear un contenedor denominado **origen**.  Crear una ruta de acceso de carpeta como **2019/02/26/14** en el contenedor. Cree un archivo de texto vacío y asígnele el nombre **file1.txt**. Cargue el file1.txt en la ruta de acceso de carpeta **origen/2019/02/26/14** en su cuenta de almacenamiento.  Puede usar varias herramientas para realizar estas tareas, como el [Explorador de Azure Storage](https://storageexplorer.com/).
    
    ![cargar archivos](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/upload-file.png)
    
    > [!NOTE]
    > Ajuste el nombre de carpeta con la hora UTC.  Por ejemplo, si la hora UTC actual es 2:03 P.M. el 26 de febrero de 2019, puede crear la ruta de acceso de carpeta como **origen/2019/02/26/14/** por la regla de **origen / {Year} / {Month} / {Day} / {Hour} /**.

2. Crear un contenedor denominado **destino**. Puede usar varias herramientas para realizar estas tareas, como el [Explorador de Azure Storage](https://storageexplorer.com/).

## <a name="create-a-data-factory"></a>Crear una factoría de datos

1. En el menú izquierdo, seleccione **+ Nuevo** > **Data + Analytics** > **Data Factory**: 
   
   ![Creación de nueva factoría de datos](./media/tutorial-copy-data-tool/new-azure-data-factory-menu.png)
2. En la página **Nueva factoría de datos**, en **Nombre**, escriba **ADFTutorialDataFactory**. 
      
    ![Nueva factoría de datos](./media/tutorial-copy-data-tool/new-azure-data-factory.png)
    
    El nombre de la factoría de datos debe ser _globalmente único_. Puede aparecer el siguiente mensaje de error:
   
   ![Mensaje de error de nueva factoría de datos](./media/tutorial-copy-data-tool/name-not-available-error.png)
   
   Si recibe un mensaje de error sobre el valor de nombre, escriba un nombre diferente para la factoría de datos. Por ejemplo, utilice _**suNombre**_**ADFTutorialDataFactory**. Para conocer las reglas de nomenclatura de los artefactos de Data Factory, consulte [Data Factory: reglas de nomenclatura](naming-rules.md).
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

1. En el **Comencemos** página, seleccione el **Copy Data** title para iniciar la herramienta Copy Data. 

   ![Icono de la herramienta Copy Data](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
   
2. En el **propiedades** página, realice los pasos siguientes:

     a. En **nombre de la tarea**, escriba **DeltaCopyFromBlobPipeline**.

    b. En **cadencia de tareas o programación de tareas**, seleccione **ejecutarse periódicamente según programación**.

    c. En **el tipo de desencadenador**, seleccione **Tumblingwindow**.
    
    d. En **periodicidad**, escriba **1 hora (s)**. 
    
    e. Seleccione **Next** (Siguiente). 
    
    La interfaz de usuario de Data Factory crea una canalización con el nombre de la tarea especificado. 

    ![Página de propiedades](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/copy-data-tool-properties-page.png)
3. En la página **Almacén de datos de origen**, realice los pasos siguientes:

     a. Haga clic en **+ crear nueva conexión**, para agregar una conexión.

    ![Página Source data store (Almacén de datos de origen)](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page.png)
    
    b. Seleccione **Azure Blob Storage** desde la galería y, a continuación, haga clic en **continuar**.

    ![Página Source data store (Almacén de datos de origen)](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page-select-blob.png)
    
    c. En el **nuevo servicio vinculado** , seleccione la cuenta de almacenamiento de la **nombredecuentadealmacenamiento** lista y, a continuación, haga clic en **finalizar**.
    
    ![Página Source data store (Almacén de datos de origen)](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page-linkedservice.png)
    
    d. Seleccione el servicio vinculado recién creado, a continuación, haga clic en **siguiente**. 
    
   ![Página Source data store (Almacén de datos de origen)](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page-select-linkedservice.png)
4. En la página **Choose the input file or folder** (Elegir el archivo o la carpeta de entrada), lleve a cabo los siguientes pasos:
    
     a. Busque y seleccione el **origen** contenedor, a continuación, seleccione **elegir**.
    
    ![Seleccione el archivo o la carpeta de entrada.](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/choose-input-file-folder.png)
    
    b. En **comportamiento de carga de archivo**, seleccione **carga Incremental: los nombres de archivo/carpeta particiones tiempo**.
    
    ![Seleccione el archivo o la carpeta de entrada.](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/choose-loading-behavior.png)
    
    c. Escribir la ruta de acceso dinámico como **origen / {year} / {month} / {day} / {hour} /** y cambiar el formato como lo siguiente:
    
    ![Seleccione el archivo o la carpeta de entrada.](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/input-file-name.png)
    
    d. Comprobar **copia binaria** y haga clic en **siguiente**.
    
    ![Seleccione el archivo o la carpeta de entrada.](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/check-binary-copy.png)     
5. En el **almacén de datos de destino** página, seleccione el **AzureBlobStorage**, que es el mismo almacenamiento de la cuenta como almacén de datos de origen y, a continuación, haga clic en **siguiente**.

    ![Página Destination data store (Almacén de datos de destino)](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/destination-data-store-page-select-linkedservice.png) 
6. En el **elegir el archivo de salida o la carpeta** página, realice los pasos siguientes:
    
     a. Busque y seleccione el **destino** carpeta, a continuación, haga clic en **elegir**.
    
    ![Elección del archivo o la carpeta de salida](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/choose-output-file-folder.png)   
    
    b. Escribir la ruta de acceso dinámico como **origen / {year} / {month} / {day} / {hour} /** y cambiar el formato como lo siguiente:
    
    ![Elección del archivo o la carpeta de salida](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/input-file-name2.png)    
    
    c. Haga clic en **Next**.
    
    ![Elección del archivo o la carpeta de salida](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/click-next-after-output-folder.png)  
7. En la página **Settings** (Configuración), seleccione **Next** (Siguiente). 

    ![Página Configuración](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/settings-page.png)  
8. En la página **Summary** (Resumen), revise la configuración y seleccione **Next** (Siguiente).

    ![Página de resumen](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/summary-page.png)
    
9. En la página **Deployment** (Implementación), seleccione **Monitor** (Supervisión) para supervisar la canalización (tarea).
    ![Página de implementación](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/deployment-page.png)
    
10. Observe que la pestaña **Monitor** (Supervisión) de la izquierda se selecciona automáticamente.  Debe esperar para la ejecución cuando se desencadena automáticamente de canalización (sobre después de una hora).  Cuando se ejecuta, el **acciones** columna incluye vínculos para ver los detalles de la ejecución de actividad y volver a ejecutar la canalización. Seleccione **actualizar** para actualizar la lista y seleccione el **ver ejecuciones de actividad** vincular en el **acciones** columna. 

    ![La supervisión de la canalización se ejecuta](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs1.png)
11. Como solo hay una actividad (actividad de copia) en la canalización, solo verá una entrada. Puede ver el archivo de origen (file1.txt) se han copiado desde **origen/2019/02/26/14/** a **destino/2019/02/26/14/** con el mismo nombre de archivo.  

    ![La supervisión de la canalización se ejecuta](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs2.png)
    
    También puede comprobar el mismo mediante el Explorador de almacenamiento de Azure (https://storageexplorer.com/) a analizar los archivos.
    
    ![La supervisión de la canalización se ejecuta](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs3.png)
12. Cree otro archivo de texto vacío con el nuevo nombre como **file2.txt**. Cargue el archivo de file2.txt en la ruta de acceso de carpeta **origen/2019/02/26/15** en su cuenta de almacenamiento.   Puede usar varias herramientas para realizar estas tareas, como el [Explorador de Azure Storage](https://storageexplorer.com/).   
    
    ![La supervisión de la canalización se ejecuta](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs4.png)
    
    > [!NOTE]
    > Es posible que tenga en cuenta que es necesaria crear una nueva ruta de carpeta. Ajuste el nombre de carpeta con la hora UTC.  Por ejemplo, si la hora UTC actual es 3:20 P.M. el 26 de febrero de 2019, puede crear la ruta de acceso de carpeta como **origen/2019/02/26/15/** por la regla de **{Year} / {Month} / {Day} / {Hour} /**.
    
13. Para volver a la **ejecuciones de canalización** visualizarla, seleccione **todas las ejecuciones de canalizaciones**y espere a que la misma canalización que se va a desencadenar de nuevo automáticamente después de una hora otra.  

    ![La supervisión de la canalización se ejecuta](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs5.png)

14. Seleccione **vista actividad ejecutar** para la segunda canalización que se ejecutan cuando viene y hacer lo mismo para revisar los detalles.  

    ![La supervisión de la canalización se ejecuta](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs6.png)
    
    Puede ver el archivo de origen (file2.txt) se han copiado desde **origen/2019/02/26/15/** a **destino/2019/02/26/15/** con el mismo nombre de archivo.
    
    ![La supervisión de la canalización se ejecuta](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs7.png) 
    
    También puede comprobar el mismo mediante el Explorador de almacenamiento de Azure (https://storageexplorer.com/) para examinar los archivos de **destino** contenedor
    
    ![La supervisión de la canalización se ejecuta](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs8.png)

    
## <a name="next-steps"></a>Pasos siguientes
Pase al tutorial siguiente para obtener información acerca de la transformación de datos mediante el uso de un clúster de Spark en Azure:

> [!div class="nextstepaction"]
>[Transformar datos con un clúster de Spark en la nube](tutorial-transform-data-spark-portal.md)