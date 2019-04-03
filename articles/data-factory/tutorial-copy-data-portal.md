---
title: Uso de Azure Portal para crear una canalización de factoría de datos | Microsoft Docs
description: En este tutorial se proporcionan instrucciones paso a paso para usar Azure Portal para crear una factoría de datos con una canalización. La canalización usa la actividad de copia para copiar datos de Azure Blob Storage a SQL Database.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 06/21/2018
ms.author: jingwang
ms.openlocfilehash: 38c9c97af0be77bf9ad4bea2d24676c7448b3aea
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58447561"
---
# <a name="copy-data-from-azure-blob-storage-to-a-sql-database-by-using-azure-data-factory"></a>Copia de datos de Azure Blob Storage a SQL Database mediante Azure Data Factory
En este tutorial, creará una factoría de datos mediante la interfaz de usuario (UI) de Azure Data Factory. La canalización de esta factoría de datos copia los datos de Azure Blob Storage a Azure SQL Database. El patrón de configuración de este tutorial se aplica a la copia de un almacén de datos basado en archivos a un almacén de datos relacional. Para obtener una lista de los almacenes de datos que se admiten como orígenes y receptores, consulte la tabla de [almacenes de datos admitidos](copy-activity-overview.md#supported-data-stores-and-formats).

> [!NOTE]
> - Si no está familiarizado con Data Factory, consulte [Introducción a Azure Data Factory](introduction.md).

En este tutorial, realizará los siguientes pasos:

> [!div class="checklist"]
> * Creación de una factoría de datos.
> * Creación de una canalización con una actividad de copia.
> * Realización de la serie de pruebas de la canalización.
> * Desencadenamiento manual de la canalización.
> * Desencadenamiento de la canalización de forma programada.
> * Supervisión de las ejecuciones de canalización y actividad.

## <a name="prerequisites"></a>Requisitos previos
* **Suscripción de Azure**. Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
* **Cuenta de Azure Storage**. Usará Blob Storage como almacén de datos de *origen*. Si no tiene una cuenta de almacenamiento, consulte [Crear una cuenta de almacenamiento](../storage/common/storage-quickstart-create-account.md) para crear una.
* **Azure SQL Database**. Usará la base de datos como un almacén de datos *receptor*. Si no tiene una base de datos SQL, consulte [Creación de una instancia de Azure SQL Database](../sql-database/sql-database-get-started-portal.md) para crear una.

### <a name="create-a-blob-and-a-sql-table"></a>Creación de un blob y una tabla SQL

Ahora, prepare su almacenamiento de blobs y su base de datos SQL para el tutorial mediante los pasos siguientes:

#### <a name="create-a-source-blob"></a>Creación de un blob de origen

1. Inicie el Bloc de notas. Copie el texto siguiente y guárdelo como un archivo **emp.txt** en el disco:

    ```
    John,Doe
    Jane,Doe
    ```

1. Cree un contenedor denominado **adftutorial** en su instancia de Blob Storage. Cree una carpeta denominada **input** en este contenedor. A continuación, cargue el archivo **emp.txt** en la carpeta **input**. Use Azure Portal o herramientas como [Explorador de Azure Storage](https://storageexplorer.com/) para realizar estas tareas.

#### <a name="create-a-sink-sql-table"></a>Creación de una tabla SQL receptora

1. Use el siguiente script de SQL para crear la tabla **dbo.emp** en la base de datos SQL:

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

1. Permita que los servicios de Azure accedan a SQL Server. Asegúrese de que **Permitir el acceso a servicios de Azure** esté **Activado** para SQL Server de forma que Data Factory pueda escribir datos en su instancia de SQL Server. Para comprobar y activar esta configuración, realice los siguientes pasos:

     a. En el lado izquierdo, seleccione **Más servicios** > **Servidores SQL Server**.

    b. Seleccione el servidor y, en **Configuración**, seleccione **Firewall**.

    c. En la página **Configuración de firewall**, seleccione **Activado** en **Permitir el acceso a servicios de Azure**.

## <a name="create-a-data-factory"></a>Crear una factoría de datos
En este paso, creará una factoría de datos e iniciará la interfaz de usuario de Data Factory para crear una canalización en la factoría de datos. 

1. Abra **Microsoft Edge** o **Google Chrome**. Actualmente, la interfaz de usuario de Data Factory solo se admite en los exploradores web Microsoft Edge y Google Chrome.
2. En el menú de la izquierda, seleccione **Crear un recurso** > **Datos y análisis** > **Data Factory**: 
  
   ![Selección de la factoría de datos en el panel Nuevo](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

3. En la página **Nueva factoría de datos**, en **Nombre**, escriba **ADFTutorialDataFactory**. 
      
     ![Nueva factoría de datos](./media/tutorial-copy-data-portal/new-azure-data-factory.png)
 
   El nombre de la instancia de Azure Data Factory debe ser *único de forma global*. Si ve el siguiente mensaje de error en el campo de nombre, cambie el nombre de la factoría de datos (por ejemplo, suNombreADFTutorialDataFactory). Para conocer las reglas de nomenclatura de los artefactos de Data Factory, consulte [Azure Data Factory: reglas de nomenclatura](naming-rules.md).
  
   ![Mensaje de error](./media/tutorial-copy-data-portal/name-not-available-error.png)
4. Seleccione la **suscripción** de Azure en la que quiere crear la factoría de datos. 
5. Para **Grupo de recursos**, realice uno de los siguientes pasos:
     
     a. Seleccione en primer lugar **Usar existente**y después un grupo de recursos de la lista desplegable.

    b. Seleccione **Crear nuevo**y escriba el nombre de un grupo de recursos. 
         
    Para más información sobre los grupos de recursos, consulte [Uso de grupos de recursos para administrar los recursos de Azure](../azure-resource-manager/resource-group-overview.md). 
6. En **Versión**, seleccione **V2**.
7. En **Ubicación**, seleccione la ubicación de la factoría de datos. En la lista desplegable solo se muestran las ubicaciones que se admiten. Los almacenes de datos (por ejemplo, Azure Storage y SQL Database) y los procesos (por ejemplo, Azure HDInsight) que usa la factoría de datos pueden estar en otras regiones.
8. Seleccione **Anclar al panel**. 
9. Seleccione **Crear**. 
10. En el panel, verá el icono siguiente con el estado **Deploying Data Factory** (Implementando Data Factory): 

    ![Icono de implementación de una factoría de datos](media/tutorial-copy-data-portal/deploying-data-factory.png)
1. Una vez finalizada la creación, verá la página **Data Factory** tal como se muestra en la imagen.
   
    ![Página principal Factoría de datos](./media/tutorial-copy-data-portal/data-factory-home-page.png)
1. Haga clic en **Author & Monitor** (Creación y supervisión) para iniciar la interfaz de usuario de Data Factory en una pestaña independiente.

## <a name="create-a-pipeline"></a>Crear una canalización
En este paso, creará una canalización con una actividad de copia en la factoría de datos. La actividad de copia realiza la copia de los datos de Blob Storage a SQL Database. En el [tutorial de inicio rápido](quickstart-create-data-factory-portal.md),creó una canalización mediante estos pasos:

1. Creación del servicio vinculado. 
1. Creación del conjunto de datos de entrada y salida.
1. Creación de una canalización

En este tutorial, comenzará a crear la canalización. A continuación, creará servicios vinculados y conjuntos de datos cuando los necesite para configurar la canalización. 

1. En la página **Let's get started** (Introducción) seleccione **Create pipeline** (Crear canalización). 

   ![Creación de una canalización](./media/tutorial-copy-data-portal/create-pipeline-tile.png)
1. En la pestaña **General** de la canalización, escriba **CopyPipeline** como **nombre** de la canalización.

1. En el cuadro de herramientas **Activities** (Actividades), expanda la categoría **Move and Transform** (Mover y transformar) y arrastre y suelte la actividad **Copy Data** (Copiar datos) desde el cuadro de herramientas hasta la superficie de diseño de la canalización. Especifique **CopyFromBlobToSql** en **Name** (Nombre).

    ![Actividad de copia](./media/tutorial-copy-data-portal/drag-drop-copy-activity.png)

### <a name="configure-source"></a>Configuración del origen

1. Vaya a la pestaña **Source** (Origen). Haga clic en **+ New** (+ Nuevo) para crear un conjunto de datos de origen. 

1. En la ventana **New Dataset** (Nuevo conjunto de datos), seleccione **Azure Blob Storage** y después **Finish** (Finalizar). Los datos de origen están en Blob Storage, así que seleccionará **Azure Blob Storage** como conjunto de datos de origen. 

    ![Selección de almacenamiento](./media/tutorial-copy-data-portal/select-azure-blob-dataset.png)

1. Verá que se abre una nueva pestaña para el conjunto de datos de blob. En la pestaña **General** de la parte inferior de la ventana **Properties** (Propiedades), en **Name** (Nombre), escriba **SourceBlobDataset**.

    ![Nombre del conjunto de datos](./media/tutorial-copy-data-portal/dataset-name.png)

1. Vaya a la pestaña **Connection** (Conexión) de la ventana **Properties** (Propiedades). Junto al cuadro de texto **Linked service** (Servicio vinculado), seleccione **+ New** (+Nuevo). 

    ![Botón New linked service (Nuevo servicio vinculado)](./media/tutorial-copy-data-portal/source-dataset-new-linked-service-button.png)

1. En la ventana **New Linked Service** (Nuevo servicio vinculado), escriba **AzureStorageLinkedService** como nombre, seleccione la cuenta de almacenamiento en la lista **Nombre de la cuenta de almacenamiento** y, finalmente, seleccione **Guardar** para implementar el servicio vinculado.

    ![Nuevo servicio vinculado](./media/tutorial-copy-data-portal/new-azure-storage-linked-service.png)

1. Una vez que se ha creado el servicio vinculado, volverá a la configuración del conjunto de datos. Junto a **File path** (Ruta de acceso del archivo), seleccione **Browse** (Examinar).

    ![Botón Browse (Examinar) de la ruta de acceso del archivo](./media/tutorial-copy-data-portal/file-browse-button.png)

1. Vaya a la carpeta **adftutorial/input**, seleccione el archivo **emp.txt** y, luego, **Finish** (Finalizar). 

    ![Seleccionar archivo de entrada](./media/tutorial-copy-data-portal/select-input-file.png)

1. Confirme que el **formato de archivo** está establecido en **Text format** (Formato de texto) y que el **delimitador de columna** está establecido en **Comma (Coma) (`,`)**. Si el archivo de origen usa diferentes delimitadores de fila y columna, puede seleccionar **Detect Text Format** (Detectar formato de texto) en **File format** (Formato de archivo). La herramienta Copy Data detecta automáticamente el formato de archivo y los delimitadores. Todavía puede invalidar estos valores. Para obtener una vista previa de los datos de esta página, seleccione **Preview data** (Vista previa de los datos).

    ![Detectar el formato de texto](./media/tutorial-copy-data-portal/detect-text-format.png)

1. Vaya a la pestaña **Schema** (Esquema) de la ventana **Properties** (Propiedades) y seleccione **Import Schema**. (Importar esquema). Tenga en cuenta que la aplicación detectó dos columnas en el archivo de origen. Aquí importará el esquema, así que puede asignar columnas del almacén de datos de origen al almacén de datos receptor. Si no es necesario asignar columnas, puede omitir este paso. En este tutorial, importará el esquema.

    ![Detectar el esquema de origen](./media/tutorial-copy-data-portal/detect-source-schema.png)  

1. Ahora, vaya a la canalización -> pestaña **Origen** y confirme que **SourceBlobDataset** está seleccionado. Para obtener una vista previa de los datos de esta página, seleccione **Preview data** (Vista previa de los datos). 
    
    ![Conjunto de datos de origen](./media/tutorial-copy-data-portal/source-dataset-selected.png)

### <a name="configure-sink"></a>Configuración del receptor

1. Vaya a la pestaña **Sink** (Receptor) y seleccione **+ New** (+Nuevo) para crear un conjunto de datos del receptor. 

    ![Conjunto de datos del receptor](./media/tutorial-copy-data-portal/new-sink-dataset-button.png)
1. En la ventana **Nuevo conjunto de datos**, escriba "SQL" en el cuadro de búsqueda para filtrar los conectores, seleccione **Azure SQL Database** y, después, seleccione **Finalizar**. En este tutorial, copiará los datos en una base de datos SQL. 

    ![Selección de base de datos SQL](./media/tutorial-copy-data-portal/select-azure-sql-dataset.png)
1. En la pestaña **General** de la ventana **Properties** (Propiedades), en **Name** (Nombre), escriba **OutputSqlDataset**. 
    
    ![Nombre del conjunto de datos de salida](./media/tutorial-copy-data-portal/output-dataset-name.png)
1. Vaya a la pestaña **Connection** (Conexión) y, junto a **Linked service** (Servicio vinculado), seleccione **+ New** (+Nuevo). Un conjunto de datos debe estar asociado con un servicio vinculado. El servicio vinculado tiene la cadena de conexión que usa Data Factory para conectarse a la base de datos SQL en tiempo de ejecución. El conjunto de datos especifica el contenedor, la carpeta y el archivo (opcional) donde se copian los datos. 
    
    ![Servicio vinculado](./media/tutorial-copy-data-portal/new-azure-sql-database-linked-service-button.png)       
1. En la ventana **New Linked Service** (Nuevo servicio vinculado), realice los pasos siguientes: 

     a. En **Name** (Nombre), escriba **AzureSqlDatabaseLinkedService**.

    b. En **Server name** (Nombre del servidor), seleccione su instancia de SQL Server.

    c. En **Database name** (Nombre de la base de datos), seleccione la base de datos SQL.

    d. En **User name** (Nombre de usuario), escriba el nombre del usuario.

    e. En **Password** (Contraseña), escriba la contraseña del usuario.

    f. Seleccione **Test connection** (Prueba de conexión) para probar la conexión.

    g. Seleccione **Save** (Guardar) para guardar el servicio vinculado. 
    
    ![Guardar nuevo servicio vinculado](./media/tutorial-copy-data-portal/new-azure-sql-linked-service-window.png)

1. En **Table** (Tabla), seleccione **[dbo].[emp]**. 

    ![Tabla](./media/tutorial-copy-data-portal/select-emp-table.png)
1. Vaya a la pestaña **Schema** (Esquema) y seleccione **Import Schema** (Importar esquema). 

    ![Seleccionar Import schema (Importar esquema)](./media/tutorial-copy-data-portal/import-destination-schema.png)
1. Seleccione la columna **ID** (Id.) y luego seleccione **Delete** (Eliminar). La columna **ID** (Id.) es una columna de identidad de la base de datos SQL, por lo que no es necesario insertar datos en esta columna durante la actividad de copia.

    ![Eliminar columna ID (Identificador)](./media/tutorial-copy-data-portal/delete-id-column.png)
1. Vaya a la pestaña con la canalización y, en **Sink Dataset** (Conjunto de datos del receptor), confirme que se ha seleccionado **OutputSqlDataset**.

    ![Pestaña Pipeline (Canalización)](./media/tutorial-copy-data-portal/pipeline-tab-2.png)        

### <a name="configure-mapping"></a>Configuración de la asignación

Vaya a la pestaña **Mapping** (Asignación) en la parte inferior de la ventana **Properties** (Propiedades) y seleccione **Import Schemas** (Importar esquemas). Tenga en cuenta que las columnas primera y segunda del archivo de origen se asignan a **FirstName** y **LastName** de la base de datos SQL.

![Esquemas de asignación](./media/tutorial-copy-data-portal/map-schemas.png)

## <a name="validate-the-pipeline"></a>Comprobación de la canalización
Para validar la canalización, seleccione **Validar** en la barra de herramientas.
 
También puede ver el código JSON asociado a la canalización. Para ello, haga clic en **Código** en la parte superior derecha.

## <a name="debug-and-publish-the-pipeline"></a>Depuración y publicación de la canalización
Puede depurar una canalización antes de publicar artefactos (servicios vinculados, conjuntos de datos y canalizaciones) en Data Factory o en su propio repositorio Git de Azure Repos. 

1. Para depurar la canalización, seleccione **Depurar** en la barra de herramientas. Verá el estado de ejecución de la canalización en la pestaña **Output** (Salida) en la parte inferior de la ventana. 

1. Una vez que la canalización se pueda ejecutar correctamente, en la barra de herramientas superior, seleccione **Publicar todo**. Esta acción publica las entidades (conjuntos de datos y canalizaciones) que creó para Data Factory.

    ![Publicar](./media/tutorial-copy-data-portal/publish-button.png)

1. Espere a que aparezca el mensaje **Successfully published** (Publicado correctamente). Para ver los mensajes de notificación, haga clic en **Mostrar notificaciones** en la parte superior derecha (botón de campana). 

## <a name="trigger-the-pipeline-manually"></a>Desencadenamiento manual de la canalización
En este paso, desencadenará manualmente la canalización que publicó en el paso anterior. 

1. Seleccione **Trigger** (Desencadenar) en la barra de herramientas y, después, seleccione **Trigger Now** (Desencadenar ahora). En la página **Pipeline Run** (Ejecución de canalización), seleccione **Finish** (Finalizar).  

1. Vaya a la pestaña **Monitor** (Supervisar) de la izquierda. Verá una ejecución de canalización que se desencadena de forma manual. Puede usar los vínculos de la columna **Actions** (Acciones) para ver los detalles de la actividad y volver a ejecutar la canalización.

    ![La supervisión de la canalización se ejecuta](./media/tutorial-copy-data-portal/monitor-pipeline.png)
1. Para ver las ejecuciones de actividad asociadas con la ejecución de canalización, seleccione el vínculo **View Activity Runs** (Ver ejecuciones de actividad) en la columna **Actions** (Acciones). En este ejemplo, solo hay una actividad, así que solo verá una entrada en la lista. Para ver detalles sobre la operación de copia, seleccione el vínculo **Details** (Detalles) (icono de gafas) en la columna **Actions** (Acciones). Seleccione **Pipelines** (Canalizaciones) en la parte superior para volver a la vista **Pipeline Runs** (Ejecuciones de canalización). Para actualizar la vista, seleccione **Refresh** (Actualizar).

    ![Supervisión de las ejecuciones de actividad](./media/tutorial-copy-data-portal/view-activity-runs.png)
1. Compruebe que se agregan dos filas más a la tabla **emp** de la base de datos SQL. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>Desencadenamiento de la canalización de forma programada
En esta programación, creará un desencadenador de programación para la canalización. El desencadenador ejecuta la canalización de acuerdo con la programación especificada, como diariamente o cada hora. En este ejemplo, establecerá el desencadenador para que se ejecute cada minuto hasta la fecha y hora de finalización especificadas. 

1. Vaya a la pestaña **Author** (Creador) a la izquierda, por encima de la pestaña Monitor (Supervisar). 

1. Vaya a la canalización, haga clic en **Desencadenar** en la barra de herramientas y seleccione **New/Edit** (Nuevo/Editar). 

1. En la página **Add Triggers** (Agregar desencadenadores), seleccione **Choose trigger** (Elegir desencadenador) y, después, seleccione **+New** (+Nuevo). 

    ![New button](./media/tutorial-copy-data-portal/add-trigger-new-button.png)
1. En la ventana **New Trigger** (Nuevo desencadenador), lleve a cabo los siguientes pasos: 

     a. En **Name** (Nombre), escriba **RunEveryMinute**.

    b. En **End** (Fin), seleccione **On Date** (El día).

    c. En **End On** (Finaliza el), seleccione la lista desplegable.

    d. Seleccione la opción **current day** (día actual). De forma predeterminada, el día de finalización se establece en el día siguiente.

    e. Actualice la sección de **minutos** con unos cuantos minutos que pasen de la fecha y hora actuales. El desencadenador se activa únicamente después de publicar los cambios. Si lo establece solo en un par de minutos de diferencia y no lo publica para entonces, no verá una ejecución de desencadenador.

    f. Seleccione **Aplicar**. 

    ![Propiedades de desencadenador](./media/tutorial-copy-data-portal/set-trigger-properties.png)

    g. Seleccione la opción **Activated** (Activado). Puede desactivarla y activarla más adelante mediante esta casilla.

    h. Seleccione **Next** (Siguiente).

    ![Botón Activated (Activado)](./media/tutorial-copy-data-portal/trigger-activiated-next.png)

    > [!IMPORTANT]
    > Con cada ejecución de canalización se asocia un costo, así que establezca la fecha de finalización correctamente. 
1. En la página **Trigger Run Parameters** (Parámetros de ejecución de desencadenador), revise la advertencia y luego seleccione **Finish** (Finalizar). La canalización de este ejemplo no toma ningún parámetro. 

    ![Parámetros de ejecución de desencadenador](./media/tutorial-copy-data-portal/trigger-pipeline-parameters.png)

1. Haga clic en **Publicar todo** para publicar el cambio. 

1. Vaya a la pestaña **Monitor** (Supervisión) de la izquierda para ver las ejecuciones de canalización desencadenadas. 

    ![Ejecuciones de canalización desencadenadas](./media/tutorial-copy-data-portal/triggered-pipeline-runs.png)    
1. Para cambiar de la vista **Pipeline Runs** (Ejecuciones de canalización) a la vista **Trigger Runs** (Ejecuciones de desencadenador), seleccione **Pipeline Runs** (Ejecuciones de canalización) y seleccione **Trigger Runs** (Ejecuciones de desencadenador).
    
    ![Ejecuciones de desencadenador](./media/tutorial-copy-data-portal/trigger-runs-menu.png)
1. Verá las ejecuciones de desencadenador en una lista. 

    ![Lista de ejecuciones de desencadenador](./media/tutorial-copy-data-portal/trigger-runs-list.png)
1. Compruebe que se insertan dos filas por minuto (para cada ejecución de canalización) en la tabla **emp** hasta la hora de finalización especificada. 

## <a name="next-steps"></a>Pasos siguientes
La canalización de este ejemplo copia los datos de una ubicación a otra de Blob Storage. Ha aprendido a: 

> [!div class="checklist"]
> * Creación de una factoría de datos.
> * Creación de una canalización con una actividad de copia.
> * Realización de la serie de pruebas de la canalización.
> * Desencadenamiento manual de la canalización.
> * Desencadenamiento de la canalización de forma programada.
> * Supervisión de las ejecuciones de canalización y actividad.


Para aprender a copiar datos desde el entorno local a la nube, avance al tutorial siguiente: 

> [!div class="nextstepaction"]
>[Copia de datos del entorno local a la nube](tutorial-hybrid-copy-portal.md)
