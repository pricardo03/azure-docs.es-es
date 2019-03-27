---
title: Copia de datos de Azure Data Lake Storage Gen1 en Gen2 con Azure Data Factory
description: Uso de Azure Data Factory para copiar datos de Azure Data Lake Storage Gen1 en Gen2
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: jingwang
ms.openlocfilehash: b975b34c638db812d3809bf50d1eccbb651028b5
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58448927"
---
# <a name="copy-data-from-azure-data-lake-storage-gen1-to-gen2-with-azure-data-factory"></a>Copia de datos de Azure Data Lake Storage Gen1 en Gen2 con Azure Data Factory

‎Azure Data Lake Storage Gen2 es un conjunto de funcionalidades dedicadas al análisis de macrodatos basado en [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md). Permite establecer una conexión con los datos usando tanto el sistema de archivos como el almacenamiento de objetos.

Si actualmente usa Azure Data Lake Storage Gen1, puede evaluar la nueva funcionalidad de Gen2 mediante copiando datos de Data Lake Storage Gen1 en Gen2 mediante Azure Data Factory.

Azure Data Factory es un servicio de integración de datos en la nube totalmente administrado. Puede utilizar el servicio para rellenar el lago con datos de un amplio conjunto de almacenes de datos locales y basados en la nube y ahorrar tiempo al crear las soluciones de análisis. Para una lista detallada de conectores admitidos, consulte la tabla de [Almacenes de datos admitidos](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory ofrece una solución de movimiento de datos administrados y de escalabilidad horizontal. Debido a la arquitectura con escalabilidad horizontal de ADF, puede ingerir datos con un alto rendimiento. Para más información, consulte el [rendimiento de la actividad de copia](copy-activity-performance.md).

En este artículo se muestra cómo utilizar la herramienta Copiar datos de Data Factory para copiar datos de _Azure Data Lake Storage Gen1_ en _Azure Data Lake Storage Gen2_. Puede seguir los mismos pasos para copiar datos de otros tipos de almacenes de datos.

## <a name="prerequisites"></a>Requisitos previos

* Suscripción de Azure: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* Cuenta de Azure Data Lake Storage Gen1 con datos en ella.
* Cuenta de Azure Storage con Data Lake Storage Gen2 habilitado: Si no tiene una cuenta de Storage, haga clic [en este vínculo](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) para crear una.

## <a name="create-a-data-factory"></a>Crear una factoría de datos

1. En el menú izquierdo, seleccione **crear un recurso** > **datos y análisis** > **Data Factory**:
   
   ![Selección de la factoría de datos en el panel Nuevo](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. En la página **Nueva factoría de datos**, proporcione los valores de los campos que se muestran en la imagen siguiente: 
      
   ![Página New data factory (Nueva factoría de datos)](./media/load-azure-data-lake-storage-gen2-from-gen1/new-azure-data-factory.png)
 
    * **Nombre**: escriba un nombre único global para la factoría de datos de Azure. Si recibe el error "El nombre de la factoría de datos \"LoadADLSDemo\" no está disponible", escriba uno diferente. Por ejemplo, podría utilizar el nombre _**suNombre**_**ADFTutorialDataFactory**. Intente crear de nuevo la factoría de datos. Para conocer las reglas de nomenclatura de los artefactos de Data Factory, consulte [Data Factory: reglas de nomenclatura](naming-rules.md).
    * **Suscripción**: seleccione la suscripción de Azure donde desea crear la factoría de datos. 
    * **Grupo de recursos**: seleccione un grupo de recursos existente en la lista desplegable o seleccione la opción **Crear nuevo** y escriba el nombre de un grupo de recursos. Para obtener más información sobre los grupos de recursos, consulte [Uso de grupos de recursos para administrar los recursos de Azure](../azure-resource-manager/resource-group-overview.md).  
    * **Versión**: seleccione **V2**.
    * **Ubicación**: Seleccione la ubicación de la factoría de datos. Solo las ubicaciones admitidas se muestran en la lista desplegable. Los almacenes de datos que las factorías de datos usan pueden estar en otras ubicaciones y regiones. 

3. Seleccione **Crear**.
4. Una vez completada la creación, vaya a la factoría de datos. Verá la página principal de **Factoría de datos**, tal y como se muestra en la siguiente imagen: 
   
   ![Página principal Factoría de datos](./media/load-azure-data-lake-storage-gen2-from-gen1/data-factory-home-page.png)

   Seleccione el icono **Author & Monitor** (Creación y supervisión) para iniciar la aplicación de integración de datos en una pestaña independiente.

## <a name="load-data-into-azure-data-lake-storage-gen2"></a>Carga de datos en Azure Data Lake Storage Gen2

1. En la página **Get started** (Introducción), seleccione el icono **Copy Data** (Copiar datos) para iniciar la herramienta para copiar datos: 

   ![Icono de la herramienta Copy Data](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-data-tool-tile.png)
2. En la página **Properties** (Propiedades), especifique **CopyFromADLSGen1ToGen2** en el campo **Task name** (Nombre de la tarea) y seleccione **Next** (Siguiente):

    ![Página de propiedades](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-data-tool-properties-page.png)
3. En la página **Almacén de datos de origen**, haga clic en **+ Crear nueva conexión**:

    ![Página Source data store (Almacén de datos de origen)](./media/load-azure-data-lake-storage-gen2-from-gen1/source-data-store-page.png)
    
    Seleccione **Azure Data Lake Storage Gen1** en la galería de conectores y seleccione **Continue** (Continuar).
    
    ![Página de Azure Data Lake Storage Gen1 de almacén de datos de origen](./media/load-azure-data-lake-storage-gen2-from-gen1/source-data-store-page-adls-gen1.png)
    
4. En la página **Specify Azure Data Lake Storage Gen1 connection** (Especificar conexión de Azure Data Lake Storage Gen1), siga estos pasos:
   1. Seleccione su instancia de Data Lake Storage Gen1 para el nombre de cuenta y especifique o valide el **inquilino**.
   2. Haga clic en **Test connection** (Prueba de conexión) para validar la configuración y, después, seleccione **Finish** (Finalizar).
   3. Podrá ver una nueva conexión creada. Seleccione **Next** (Siguiente).
   
   > [!IMPORTANT]
   > En este tutorial, utilizará una identidad administrada para recursos de Azure para autenticar la cuenta de Data Lake Storage Gen1. Asegúrese de conceder a MSI los permisos adecuados en Azure Data Lake Storage Gen1 con [estas instrucciones](connector-azure-data-lake-store.md#managed-identity).
   
   ![Especificación de una cuenta de Azure Data Lake Storage Gen1](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen1-account.png)
      
5. En la página **Choose the input file or folder** (Elegir archivo o carpeta de entrada), vaya a la carpeta y el archivo que desea copiar. Seleccione la carpeta o el archivo y seleccione **Choose** (Elegir):

    ![Elegir archivo o carpeta de entrada](./media/load-azure-data-lake-storage-gen2-from-gen1/choose-input-folder.png)

6. Elija el comportamiento de copia seleccionando las opciones **Copy files recursively** (Copiar archivos de forma recursiva) y **Binary copy** (Copia binaria). Seleccione **Next** (Siguiente):

    ![Especificación de carpeta de salida](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-binary-copy.png)
    
7. En la página **Almacén de datos de destino**, haga clic en **+Crear nueva conexión** y después seleccione **Azure Data Lake Storage Gen2** y **Continuar**:

    ![Página Destination data store (Almacén de datos de destino)](./media/load-azure-data-lake-storage-gen2-from-gen1/destination-data-storage-page.png)

8. En la página **Specify Azure Data Lake Storage Gen2 connection** (Especificar conexión de Azure Data Lake Storage Gen2), siga estos pasos:

   1. Seleccione la cuenta habilitada para Data Lake Storage Gen2 en la lista desplegable "Storage account name" (Nombre de la cuenta de almacenamiento).
   2. Seleccione **Finalizar** para crear la conexión. Luego, seleccione **Siguiente**.
   
   ![Especificación de una cuenta de Azure Data Lake Storage Gen2](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen2-account.png)

9. En la página **Choose the output file or folder** (Elegir archivo o carpeta de salida) escriba **copyfromadlsgen1** como nombre de la carpeta de salida y seleccione **Next** (Siguiente): 

    ![Especificación de carpeta de salida](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen2-path.png)

10. En la página **Settings** (Configuración), seleccione **Next** (Siguiente) para usar las configuraciones predeterminadas.

11. En la página **Summary** (Resumen), revise la configuración y seleccione **Next** (Siguiente):

    ![Página de resumen](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-summary.png)
12. En la página **Deployment** (Implementación), seleccione **Monitor** (Supervisión) para supervisar la canalización:

    ![Página Deployment (Implementación)](./media/load-azure-data-lake-storage-gen2-from-gen1/deployment-page.png)
13. Observe que la pestaña **Monitor** (Supervisión) de la izquierda se selecciona automáticamente. La columna **Actions** (Acciones) incluye los vínculos para ver los detalles de la ejecución de actividad y volver a ejecutar la canalización:

    ![La supervisión de la canalización se ejecuta](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-pipeline-runs.png)

14. Para ver las ejecuciones de actividad asociadas con la ejecución de canalización, seleccione el vínculo **View Activity Runs** (Ver ejecuciones de actividad) en la columna **Actions** (Acciones). Como solo hay una actividad (actividad de copia) en la canalización, solo verá una entrada. Para volver a la vista de ejecuciones de canalización, seleccione el vínculo **Pipelines** (Canalizaciones) de la parte superior. Seleccione **Refresh** (Actualizar) para actualizar la lista. 

    ![Supervisión de las ejecuciones de actividad](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-activity-runs.png)

15. Para supervisar los detalles de la ejecución de cada actividad de copia, seleccione el vínculo con un icono de gafas **Details** (Detalles) en **Actions** (Acciones) en la vista de supervisión de la actividad. Puede supervisar detalles como el volumen de datos copiados desde el origen al receptor, el rendimiento de los datos, los pasos de ejecución con su duración correspondiente y las configuraciones que se utilizan:

    ![Detalles de la supervisión de la ejecución de actividad](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-activity-run-details.png)

16. Verifique que los datos se copian en la cuenta de Data Lake Storage Gen2.

## <a name="best-practices"></a>Procedimientos recomendados

Al copiar grandes volúmenes de datos desde un almacén de datos basado en archivos, se recomienda:

- Particione los archivos en conjuntos de archivos de 10 TB a 30 TB.
- No desencadenar demasiadas ejecuciones de copia simultáneas para evitar limitaciones en los almacenes de datos de origen y de destino. Puede comenzar con una ejecución de copia, supervisar el rendimiento y, gradualmente, agregar más según sea necesario.

## <a name="next-steps"></a>Pasos siguientes

* [Información general de la actividad de copia](copy-activity-overview.md)
* [Conector de Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md)