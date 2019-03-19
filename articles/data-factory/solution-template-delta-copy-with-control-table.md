---
title: La copia delta desde una base de datos mediante el uso de una tabla de control con Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo usar una plantilla de solución para copiar incrementalmente filas nuevas o actualizadas solo desde una base de datos con Azure Data Factory.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/24/2018
ms.openlocfilehash: c32592ce539eeb2dec71792e4a6eb31e7d904eff
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57771164"
---
# <a name="delta-copy-from-a-database-with-a-control-table"></a>Copia delta desde una base de datos con una tabla de control

En este artículo se describe una plantilla que está disponible para la carga incremental de filas nuevas o actualizadas de una tabla de base de datos en Azure mediante el uso de una tabla de control externo que almacena un valor de límite máximo.

Esta plantilla requiere que el esquema de la base de datos de origen contiene una clave de incremento o columna de marca de tiempo para identificar filas nuevas o actualizadas.

>[!NOTE]
> Si tiene una columna de marca de tiempo en la base de datos de origen para identificar filas nuevas o actualizadas, pero no desea crear una tabla de control externo que se usará para la copia delta, en su lugar, puede usar el [herramienta Copy Data de Azure Data Factory](copy-data-tool.md) para obtener una canalización. Esa herramienta usa un tiempo programado por el desencadenador como una variable para leer las nuevas filas de la base de datos de origen.

## <a name="about-this-solution-template"></a>Acerca de esta plantilla de solución

Esta plantilla primero recupera el valor de marca de agua antiguo y lo compara con el valor actual de la marca de agua. Después de eso, copian solo los cambios de la base de datos de origen según una comparación entre los valores de marca de agua de dos. Por último, almacena el nuevo valor de límite máximo a una tabla de control externo para cargar la próxima vez que los datos diferenciales.

La plantilla contiene cuatro actividades:
- **Búsqueda** recupera el valor de límite máximo anterior, que se almacena en una tabla de control externo.
- Otro **búsqueda** actividad recupera el valor de límite máximo actual de la base de datos de origen.
- **Copia** copia solo los cambios de la base de datos de origen en el almacén de destino. Es similar a la consulta que identifica los cambios en la base de datos de origen ' Seleccionar * desde Data_Source_Table donde TIMESTAMP_Column > "último máximos" y TIMESTAMP_Column < = "límite máximo actual" '.
- **SqlServerStoredProcedure** escribe el valor de límite máximo actual en una tabla de control externo para la copia delta próxima vez.

La plantilla define cinco parámetros:
- *Data_Source_Table_Name* es la tabla en la base de datos de origen que desea cargar los datos de.
- *Data_Source_WaterMarkColumn* es el nombre de la columna en la tabla de origen que se usa para identificar nuevas o las filas actualizadas. El tipo de esta columna es normalmente *datetime*, *INT*, o similar.
- *Data_Destination_Folder_Path* o *Data_Destination_Table_Name* es el lugar donde los datos se copian en el almacén de destino.
- *Control_Table_Table_Name* es la tabla de control externo que almacena el valor de límite máximo.
- *Control_Table_Column_Name* es la columna en la tabla de control externo que almacena el valor de límite máximo.

## <a name="how-to-use-this-solution-template"></a>Uso de esta plantilla de solución

1. Explore el origen de tabla que quiere cargar y definir la columna de límite máximo que puede usarse para identificar filas nuevas o actualizadas. El tipo de esta columna podría ser *datetime*, *INT*, o similar. Valor de esta columna aumenta a medida que se agregan nuevas filas. En la tabla de origen de ejemplo siguiente (data_source_table), podemos usar el *LastModifytime* columna como columna de marca de agua de alto.

    ```sql
            PersonID    Name    LastModifytime
            1   aaaa    2017-09-01 00:56:00.000
            2   bbbb    2017-09-02 05:23:00.000
            3   cccc    2017-09-03 02:36:00.000
            4   dddd    2017-09-04 03:21:00.000
            5   eeee    2017-09-05 08:06:00.000
            6   fffffff 2017-09-06 02:23:00.000
            7   gggg    2017-09-07 09:01:00.000
            8   hhhh    2017-09-08 09:01:00.000
            9   iiiiiiiii   2017-09-09 09:01:00.000
    ```
    
2. Crear una tabla de control en SQL Server o Azure SQL Database para almacenar el valor de límite máximo para cargar los datos diferenciales. En el ejemplo siguiente, el nombre de la tabla de control es *watermarktable*. En esta tabla, *WatermarkValue* es la columna que almacena el valor de límite máximo y su tipo es *datetime*.

    ```sql
            create table watermarktable
            (
            WatermarkValue datetime,
            );
            INSERT INTO watermarktable
            VALUES ('1/1/2010 12:00:00 AM')
    ```
    
3. Crear un procedimiento almacenado en la misma instancia de SQL Server o Azure SQL Database que usó para crear la tabla de control. El procedimiento almacenado se utiliza para escribir el nuevo valor de límite máximo en la tabla de control externo para cargar la próxima vez que los datos diferenciales.

    ```sql
            CREATE PROCEDURE update_watermark @LastModifiedtime datetime
            AS

            BEGIN

                UPDATE watermarktable
                SET [WatermarkValue] = @LastModifiedtime 

            END
    ```
    
4. Vaya a la **copia diferencial de base de datos** plantilla. Crear un **New** conexión a la base de datos de origen que desea copiar datos desde.

    ![Creación de una nueva conexión con la tabla de origen](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable4.png)

5. Crear un **New** conexión al almacén de datos de destino que desea copiar los datos.

    ![Creación de una nueva conexión a la tabla de destino](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable5.png)

6. Crear un **New** conexión a la tabla de control externo y un procedimiento almacenado que creó en los pasos 2 y 3.

    ![Creación de una nueva conexión al almacén de datos de la tabla de control](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable6.png)

7. Seleccione **usar esta plantilla**.

     ![Uso de esta plantilla](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable7.png)
    
8. Verá la canalización disponible, tal como se muestra en el ejemplo siguiente:

     ![Revisión de la canalización](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable8.png)

9. Seleccione **procedimiento almacenado**. Para **nombre del procedimiento almacenado**, elija **[update_watermark]**. Seleccione **parámetro de importación**y, a continuación, seleccione **agregar contenido dinámico**.  

     ![Establezca la actividad de procedimiento almacenado](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable9.png) 

10. Escribe el contenido  **\@{activity('LookupCurrentWaterMark').output.firstRow.NewWatermarkValue}** y, a continuación, seleccione **finalizar**.  

     ![Escribir el contenido de los parámetros del procedimiento almacenado](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable10.png)      
     
11. Seleccione **depurar**, escriba el **parámetros**y, a continuación, seleccione **finalizar**.

    ![Seleccione ** ** de depuración](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable11.png)

12. Se muestran resultados similares al ejemplo siguiente:

    ![Revisión del resultado](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable12.png)

13. Puede crear nuevas filas en la tabla de origen. Este es el lenguaje SQL de ejemplo para crear nuevas filas:

    ```sql
            INSERT INTO data_source_table
            VALUES (10, 'newdata','9/10/2017 2:23:00 AM')

            INSERT INTO data_source_table
            VALUES (11, 'newdata','9/11/2017 9:01:00 AM')
    ```
14. Para volver a ejecutar la canalización, seleccione **depurar**, escriba el **parámetros**y, a continuación, seleccione **finalizar**.

    ![Seleccione ** ** de depuración](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable11.png)

    Verá que sólo nuevas filas se han copiado al destino.

15. (Opcional): Si ha seleccionado SQL Data Warehouse como destino de los datos, también debe proporcionar una conexión a Azure Blob storage para el almacenamiento provisional, que requiere Polybase en SQL Data Warehouse. Asegúrese de que el contenedor ya se ha creado en el almacenamiento de blobs.
    
    ![Configuración de PolyBase](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable15.png)
    
## <a name="next-steps"></a>Pasos siguientes

- [Copia masiva de una base de datos mediante el uso de una tabla de control con Azure Data Factory](solution-template-bulk-copy-with-control-table.md)
- [Copiar archivos de varios contenedores con Azure Data Factory](solution-template-copy-files-multiple-containers.md)
