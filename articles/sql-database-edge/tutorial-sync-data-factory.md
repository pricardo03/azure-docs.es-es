---
title: Sincronización de datos desde Azure SQL Database Edge mediante Azure Data Factory | Microsoft Docs
description: Más información sobre la sincronización de datos entre Azure SQL Database Edge y Azure Blob Storage
keywords: sql database edge,sync data from sql database edge, sql database edge data factory
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: tutorial
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: 0e75da9516303bb4250b6847a4b381d07b3d7dad
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73509209"
---
# <a name="tutorial-sync-data-from-sql-database-edge-to-azure-blob-storage-using-azure-data-factory"></a>Tutorial: Sincronización de datos entre SQL Database Edge y Azure Blob Storage con Azure Data Factory

En este tutorial, usará Azure Data Factory para sincronizar de forma incremental datos de una tabla de una instancia de Azure SQL Database Edge en Azure Blob Storage.

## <a name="before-you-begin"></a>Antes de empezar

Si aún no ha creado una base de datos o una tabla en la implementación de Azure SQL Database Edge, use uno de los métodos siguientes para crear una:

* Use [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms/) o [Azure Data Studio](/sql/azure-data-studio/download/) para conectarse a SQL Database Edge y ejecutar un script de SQL a fin de crear la base de datos y la tabla.
* Para crear una base de datos y una tabla de SQL mediante [SQLCMD](/sql/tools/sqlcmd-utility/), conéctese directamente al módulo SQL Database Edge. Para más información, consulte [Conectarse al motor de base de datos mediante sqlcmd](/sql/ssms/scripting/sqlcmd-connect-to-the-database-engine/).
* Use SQLPackage.exe para implementar un archivo dacpac en el contenedor de SQL Database Edge. Esto se puede automatizar especificando el URI del archivo SQLPackage como parte de la configuración de las propiedades deseadas de los módulos, o bien utilizando directamente la herramienta cliente SqlPackage.exe para implementar un dacpac en SQL Database Edge.

    Para descargar sqlpackage, consulte [Descarga e instalación de sqlpackage](/sql/tools/sqlpackage-download/). Se proporcionan los siguientes comandos de ejemplo para SqlPackage.exe, pero consulte la documentación de SqlPackage para más información.

    **Creación de dacpac**:

    ```cmd
    sqlpackage /Action:Extract /SourceConnectionString:"Data Source=<Server_Name>,<port>;Initial Catalog=<DB_name>;User ID=<user>;Password=<password>" /TargetFile:<dacpac_file_name> 
    ```

    **Aplicación de dacpac**:

    ```cmd
    sqlpackage /Action:Publish /Sourcefile:<dacpac_file_name> /TargetServerName:<Server_Name>,<port> /TargetDatabaseName:<DB_Name> /TargetUser:<user> /TargetPassword:<password>
    ```

## <a name="create-a-sql-table-and-procedure-to-store-and-update-the-watermark-levels"></a>Creación de una tabla y un procedimiento SQL para almacenar y actualizar los niveles de marca de agua

La tabla de marcas de agua se usa para almacenar la última marca de tiempo hasta la que los datos ya se han sincronizado con el almacenamiento de Azure. El procedimiento almacenado de Transact-SQL (T-SQL) se utiliza para actualizar la tabla de marcas de agua después de cada sincronización. 

Ejecute los siguientes comandos en la instancia de SQL Database Edge:

```sql
    Create table [dbo].[watermarktable]
    (
    TableName varchar(255),
    WatermarkValue datetime,
    )
    GO

    CREATE PROCEDURE usp_write_watermark @timestamp datetime, @TableName varchar(50)  
    AS  
    BEGIN
    UPDATE [dbo].[watermarktable]
    SET [WatermarkValue] = @timestamp WHERE [TableName] = @TableName
    END
    Go
```

## <a name="create-a-data-factory-workflow"></a>Creación de un flujo de datos de Data Factory

En esta sección, creará una canalización de Azure Data Factory para sincronizar los datos de una tabla en Azure SQL Database Edge en Azure Blob Storage.

### <a name="create-data-factory-using-the-data-factory-ui"></a>Creación de una factoría de datos con la interfaz de usuario de Data Factory

Para crear una factoría de datos, siga las instrucciones de este [tutorial](../data-factory/quickstart-create-data-factory-portal.md#create-a-data-factory).

### <a name="create-a-data-factory-pipeline"></a>Creación de una canalización de Data Factory

1. En la página de **introducción** de la interfaz de usuario de Data Factory, seleccione el icono **Create Pipeline** (Crear canalización).

    ![Data Factory: creación de una canalización](media/tutorial-sync-data-factory/data-factory-get-started.png)

2. En la página **General** (General) de la ventana **Properties** (Propiedades) de la canalización, escriba el nombre **PeriodicSync**.

3. Agreguemos la actividad **Lookup** (Búsqueda) para recuperar el valor de marca de agua anterior. En el cuadro de herramientas **Activities** (Actividades), expanda **General** (General), arrastre la actividad **Lookup** (Búsqueda) y colóquela en la superficie del diseñador de canalizaciones. Cambie el nombre de la actividad a *OldWatermark*.

    ![antigua búsqueda de marcas de agua](media/tutorial-sync-data-factory/create-old-watermark-lookup.png)

4. Cambie a la pestaña **Settings** (Configuración) y seleccione **+ New** (+ Nuevo) para **Source Dataset** (Conjunto de datos de origen). En este paso, creará un conjunto de datos que representará los datos de watermarktable. Esta tabla contiene la marca de agua que se utilizó anteriormente en la operación de copia anterior.

5. En la ventana **New Dataset** (Nuevo conjunto de datos), seleccione **Azure SQL Server** y **Continue** (Continuar).  

6. En la ventana **Set properties** (Establecer propiedades) para el conjunto de datos, escriba *WatermarkDataset* como nombre.

7. En **Linked Service** (Servicio vinculado), seleccione **New** (Nuevo) y siga estos pasos:

    1. Escriba *SQLDBEdgeLinkedService* en **Name** (Nombre).

    2. Escriba los detalles del servidor SQL Database Edge para **Server name** (Nombre del servidor).

    3. Especifique el **nombre de la base de datos** en la lista desplegable.

    4. Escriba su **User name** (Nombre de usuario) y **Password** (Contraseña).

    5. Para probar la conexión a la instancia de SQL Database Edge, seleccione **Test connection** (Probar conexión).

    6. Seleccione **Crear**.

    ![creación de un servicio vinculado](media/tutorial-sync-data-factory/create-linked-service.png)

    7. Seleccione **OK** (Aceptar).

8. En la pestaña **Settings** (Configuración), seleccione **Edit** (Edición).

9. En la pestaña **Connection** (Conexión), seleccione *[dbo].[watermarktable]* en **Table** (Tabla). Si desea una vista previa de los datos de la tabla, seleccione **Preview data** (Vista previa de los datos).

10. Cambie al editor de canalización; para ello, seleccione la pestaña de la canalización de la parte superior o el nombre de esta de la vista de árbol de la izquierda. En la ventana de propiedades de la actividad **Lookup** (Búsqueda), confirme que **WatermarkDataset** está seleccionado en el campo **Source Dataset** (Conjunto de datos de origen).

11. En el cuadro de herramientas **Activities** (Actividades), expanda **General** (General), y arrastre otra actividad **Lookup** (Búsqueda) y colóquela en la superficie del diseñador de canalizaciones; después, establezca el nombre en **NewWatermark** en la pestaña **General** (General) de la ventana de propiedades. Esta actividad de búsqueda obtiene el nuevo valor de marca de agua de la tabla y copia los datos de origen en el destino.

12. En la ventana de propiedades de la segunda actividad **Lookup** (Búsqueda), cambie a la pestaña **Settings** (Configuración) y seleccione **New** (Nuevo) para crear un conjunto de datos que apunte a la tabla de origen que contenga el nuevo valor de marca de agua.

13. En la ventana **New Dataset** (Nuevo conjunto de datos), seleccione la instancia de SQL Database Edge y **Continue** (Continuar).

    1. En la ventana **Set Properties** (Establecer propiedades), escriba **SourceDataset** (Conjunto de datos de origen) en **Name** (Nombre). Seleccione *SQLDBEdgeLinkedService* como servicio vinculado.

    2. Seleccione ***la tabla que desea sincronizar*** en Table (Tabla). También puede especificar una consulta para este conjunto de datos, como se menciona más adelante en el tutorial. La consulta tiene prioridad sobre la tabla que se especifica en este paso.

    3. Seleccione **Aceptar**.

14. Cambie al editor de canalización; para ello, seleccione la pestaña de la canalización de la parte superior o el nombre de esta de la vista de árbol de la izquierda. En la ventana de propiedades de la actividad de **búsqueda**, confirme que **SourceDataset** está seleccionado en el campo **Source Dataset** (Conjunto de datos de origen).

15. Seleccione **Query** (Consulta) en el campo **Use Query** (Usar consulta) y escriba la siguiente consulta tras actualizar el nombre de tabla en la consulta: solo se selecciona el valor máximo de la marca de tiempo en la tabla. Asegúrese de haber activado también **First row only** (Solo la primera fila).

    ```sql
    select MAX(timestamp) as NewWatermarkvalue from [TableName]
    ```

    ![seleccionar consulta](media/tutorial-sync-data-factory/select-query-data-factory.png)

16. En el cuadro de herramientas **Activities** (Actividades), expanda **Move & Transform** (Mover y transformar) y arrastre la actividad **Copy** (Copiar) del cuadro de herramientas Activities (Actividades); después, establezca el nombre en **IncrementalCopy**.

17. Conecte las dos actividades **Lookup** (Búsqueda) a la actividad **Copy** (Copiar); para ello, arrastre el **botón verde** de las actividades **Lookup** (Búsqueda) a la actividad **Copy** (Copia). Suelte el botón del mouse cuando vea el color del borde de la actividad de copia cambiar a azul.

18. Seleccione la actividad **Copy** (Copia) y confirme que ve sus propiedades en la ventana **Properties** (Propiedades).

19. Cambie a la pestaña **Source** (Origen) de la ventana **Properties** (Propiedades) y realice los pasos siguientes:

    1. Seleccione **SourceDataset** en el campo **Source Dataset** (Conjunto de datos de origen).

    2. Seleccione **Query** (Consulta) en el campo **Use Query** (Usar consulta).

    3. Escriba la consulta SQL en el campo **Query** (Consulta). Consulta de ejemplo a continuación

    4. Consulta SQL:

    ```sql
    select * from TemperatureSensor where timestamp > '@{activity('OldWaterMark').output.firstRow.WatermarkValue}' and timestamp <= '@{activity('NewWaterMark').output.firstRow.NewWatermarkvalue}'
    ```

20. Cambie a la pestaña **Sink** (Receptor) y seleccione **+ New** (+ Nuevo) en el campo **Sink Dataset** (Conjunto de datos receptor).

21. En este tutorial, el almacén de datos receptor es de tipo **Azure Blob Storage**. Seleccione **Azure Blob Storage** y **Continue** (Continuar) en la ventana **New Dataset** (Nuevo conjunto de datos).

22. En la página **Select Format** (Seleccionar formato), elija el tipo de formato de los datos y, después, seleccione **Continue** (Continuar).

23. En la ventana **Set Properties** (Establecer propiedades), escriba **SinkDataset** (Conjunto de datos de receptor) como nombre. En Linked Service (Servicio vinculado), seleccione **+ New** (+ Nuevo). En este paso se crea una conexión (servicio vinculado) y su instancia de **Azure Blob Storage**.

24. En la ventana **New Linked Service (Azure Blob Storage)** [Nuevo servicio vinculado (Azure Blob Storage)], realice los siguientes pasos:

    1. Escriba *AzureStorageLinkedService* como nombre.

    2. Seleccione su cuenta de Azure Storage como **Storage account name** (Nombre de cuenta de Storage) con su suscripción a Azure.

    3. Pruebe la **conexión** y, después, seleccione **Finish** (Finalizar).

25. En la ventana **Set Properties** (Establecer propiedades), confirme que *AzureStorageLinkedService* está seleccionado para **Linked service** (Servicio vinculado). A continuación, seleccione **Create** (Crear) y **OK** (Aceptar).

26. En la pestaña **Sink** (Receptor), seleccione **Edit** (Edición).

27. Vaya a la pestaña **Connection** (Conexión) de *SinkDataset* y realice los pasos siguientes:

    1. En el campo **Ruta de acceso de archivo**, escriba *asdedatasync/incrementalcopy*, donde **adftutorial** es el nombre del contenedor de blobs e **incrementalcopy** es el nombre de la carpeta. Cree el contenedor si no existe o asígnele el nombre de uno existente. Si no existe, Azure Data Factory crea automáticamente la carpeta de salida *incrementalcopy*. También puede usar el botón **Browse** (Examinar) para ir a una carpeta del contenedor de blobs mediante la **ruta de acceso del archivo**.

    2. En la parte **File** (Archivo) del campo **File path** (Ruta de acceso de archivo), seleccione **Add dynamic content [ALT+P]** y, después, *escriba @CONCAT('Incremental-', pipeline().RunId, '.txt')* en la ventana abierta. A continuación, seleccione **Finish** (Finalizar). El nombre de archivo se genera dinámicamente mediante la expresión. Cada ejecución de canalización tiene un identificador único. La actividad de copia usa el identificador de ejecución para generar el nombre de archivo.

28. Cambie al editor de **canalizaciones**; para ello, seleccione la pestaña de la canalización de la parte superior o su nombre en la vista de árbol de la izquierda.

29. En el cuadro de herramientas **Activities** (Actividades), expanda **General** (General), arrastre la actividad **Stored Procedure** (Procedimiento almacenado) del cuadro de herramientas **Actividades** para colocarla en la superficie del diseñador de canalizaciones. **Conecte** el resultado verde (correcto) de la actividad **Copy** (Copiar) con la actividad **Stored Procedure** (Procedimiento almacenado).

30. Seleccione **Stored Procedure Activity** (Actividad Procedimiento almacenado) en el diseñador de canalizaciones y cambie el nombre a *SPtoUpdateWatermarkActivity*.

31. Cambie a la pestaña **SQL Account** (Cuenta de SQL) y seleccione *SQLDBEdgeLinkedService* en **Linked service** (Servicio vinculado).

32. Cambie a la pestaña **Stored Procedure** (Procedimiento almacenado) y realice los pasos siguientes:

    1. En **Stored procedure name** (Nombre del procedimiento almacenado), seleccione *[dbo].[usp_write_watermark]* .

    2. Para especificar valores para los parámetros del procedimiento almacenado, seleccione Import parameter (Importar parámetro) y escriba los valores siguientes para él:

    |NOMBRE|type|Valor|
    |-----|----|-----|
    |LastModifiedtime|DateTime|@{activity('NewWaterMark').output.firstRow.NewWatermarkvalue}|
    |TableName|Cadena|@{activity('OldWaterMark').output.firstRow.TableName}|

33. Para comprobar la configuración de la canalización, seleccione **Validate** (Comprobar) en la barra de herramientas. Confirme que no haya errores de comprobación. Para cerrar la ventana **Pipeline Validation Report** (Informe de comprobación de la canalización), seleccione **>>** .

34. Para publicar entidades (servicios vinculados, conjuntos de datos y canalizaciones) en el servicio Azure Data Factory, seleccione el botón **Publish All** (Publicar todo). Espere hasta que vea un mensaje de que la publicación se completó correctamente.

## <a name="trigger-a-pipeline-on-schedule"></a>Desencadenar una canalización según una programación

1. Vaya a la barra desencadenador de la canalización, seleccione **Add Trigger** (Agregar desencadenador), después, seleccione **New/Edit** (Nuevo/Editar) y seleccione **+ New** (+ Nuevo).

2. Asigne al desencadenador el nombre *HourlySync*, elija **Type** (Tipo) como programación y establezca cada hora como **Recurrence** (Periodicidad).

3. Seleccione **Aceptar**.

4. Seleccione **Publish All** (Publicar todo).

5. Seleccione **Trigger Now** (Desencadenar ahora).

6. Cambie a la pestaña **Monitor** (Supervisar) de la izquierda. Puede ver el estado de ejecución de la canalización activado por el desencadenador manual. Seleccione el botón **Refresh** (Actualizar) para actualizar la lista.

## <a name="next-steps"></a>Pasos siguientes

La canalización de Azure Data Factory de este tutorial copia los datos de una tabla en la instancia de SQL Database Edge a una ubicación de Azure Blob Storage cada hora. Para más información sobre el uso de Data Factory en otros escenarios, consulte los siguientes [tutoriales](../data-factory/tutorial-copy-data-portal.md).
