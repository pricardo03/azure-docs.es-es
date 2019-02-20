---
title: Copia diferencial desde base de datos con tabla de control con Azure Data Factory | Microsoft Docs
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
ms.openlocfilehash: 23e1255013cd5e52166fe0e59a8931dd9ecd81a0
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2019
ms.locfileid: "55966921"
---
# <a name="delta-copy-from-database-with-control-table"></a>Copia diferencial de base de datos con tabla de control

Si desea cargar incrementalmente los cambios (filas nuevas o actualizadas) solo desde una tabla de una base de datos en Azure con una tabla de control externa que almacena el valor de límite máximo.  La plantilla presente está diseñada para ese caso. 

Esta plantilla requiere que el esquema de la base de datos de origen contenga OBLIGATORIAMENTE una columna de marca de tiempo o una clave de incremento para identificar las filas nuevas o actualizadas.

Si tiene una columna de marca de tiempo en la base de datos de origen para identificar las filas nuevas o actualizadas, pero no desea crear una tabla externa de control para habilitar la copia diferencial, puede usar la herramienta Copiar datos para obtener una canalización, que usa una hora programada por el desencadenador como variable para leer las nuevas filas solo desde la base de datos de origen.

## <a name="about-this-solution-template"></a>Acerca de esta plantilla de solución

Esta plantilla siempre recupera primero el valor límite antiguo y luego lo compara con el valor de marca de agua actual. Después, solo copia los cambios de la base de datos de origen según la comparación entre dos valores de marca de agua.  Cuando termina, almacena el nuevo valor de límite máximo en una tabla de control externa para la carga de datos diferencial de la próxima vez.

La plantilla contiene cuatro actividades:
-   Una actividad de **búsqueda** para recuperar el valor de límite máximo antiguo almacenado en una tabla de control externa.
-   Una actividad de **búsqueda** para recuperar el valor de límite máximo actual de la base de datos de origen.
-   Una actividad de **copia** para copiar los cambios solo de la base de datos de origen en el almacén de destino. La consulta utilizada para identificar los cambios de la base de datos de origen en la actividad de copia es similar a SELECT * FROM Data_Source_Table WHERE TIMESTAMP_Column > “last high-watermark” and TIMESTAMP_Column <= “current high-watermark”.
-   Una actividad de **SqlServerStoredProcedure** para escribir el valor de límite máximo actual en una tabla de control externa para la copia diferencial de la próxima vez.

La plantilla define cinco parámetros:
-   El parámetro *Data_Source_Table_Name* es el nombre de tabla de la base de datos de origen de donde desea cargar los datos.
-   El parámetro *Data_Source_WaterMarkColumn* es el nombre de columna en la tabla de origen que se puede usar para identificar las filas nuevas o actualizadas. Normalmente, el tipo de esta columna puede ser datetime o INT, etc.
-   El parámetro *Data_Destination_Folder_Path* o *Data_Destination_Table_Name* es el lugar donde los datos se copian en el almacén de destino.
-   El parámetro *Control_Table_Table_Name* es el nombre de tabla de control externa para almacenar el valor de límite máximo.
-   El parámetro *Control_Table_Column_Name* es el nombre de columna en la tabla de control externa para almacenar el valor de límite máximo.

## <a name="how-to-use-this-solution-template"></a>Uso de esta plantilla de solución

1. Explore la tabla de origen que desea cargar y defina la columna de límite máximo que puede usarse para segmentar las filas nuevas o actualizadas. Normalmente, el tipo de esta columna puede ser datetime o INT, etc. y sus datos siguen creciendo cuando se agregan nuevas filas.  En la tabla de origen del ejemplo (nombre de tabla: data_source_table) a continuación, podemos usar la columna *LastModifytime* como la columna de límite máximo.

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
    
2. Cree una tabla de control en un servidor SQL Server o SQL Azure para almacenar el valor de límite máximo para cargar los datos diferenciales. En el ejemplo siguiente, puede ver que el nombre de la tabla de control es *watermarktable*. Dentro de ella, el nombre de columna para almacenar el valor de límite máximo es *WatermarkValue* y su tipo es *datetime*.

    ```sql
            create table watermarktable
            (
            WatermarkValue datetime,
            );
            INSERT INTO watermarktable
            VALUES ('1/1/2010 12:00:00 AM')
    ```
    
3. Cree un procedimiento almacenado en el mismo servidor de SQL Server o SQL Azure utilizado para crear la tabla de control. El procedimiento almacenado se utiliza para escribir el nuevo valor de límite máximo en una tabla de control externa para la carga de datos diferencial de la próxima vez.

    ```sql
            CREATE PROCEDURE update_watermark @LastModifiedtime datetime
            AS

            BEGIN

                UPDATE watermarktable
                SET [WatermarkValue] = @LastModifiedtime 

            END
    ```
    
4. Vaya a la plantilla de **copia diferencial desde base de datos** y cree una **nueva conexión** a su base de datos de origen desde donde se copiarán los datos.

    ![Creación de una nueva conexión con la tabla de origen](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable4.png)

5. Cree una **nueva conexión** al almacén de datos de destino donde se copiarán los datos.

    ![Creación de una nueva conexión a la tabla de destino](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable5.png)

6. Cree una **nueva conexión** a la tabla de control externa y el procedimiento almacenado.  Se conecta a la base de datos donde había creado la tabla de control y procedimiento almacenado en el paso 2 y 3.

    ![Creación de una nueva conexión al almacén de datos de la tabla de control](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable6.png)

7. Haga clic en **Usar esta plantilla**.

     ![Uso de esta plantilla](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable7.png)
    
8. Ve la canalización disponible en el panel, como se muestra en el ejemplo siguiente:

     ![Revisión de canalización](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable8.png)

9. Haga clic en la actividad de procedimiento almacenado, seleccione el **nombre del procedimiento almacenado**, haga clic en **Import parameter** (Importar parámetro) y haga clic en **Agregar contenido dinámico**.  

     ![Establecimiento de actividad de procedimiento almacenado](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable9.png) 

10. Escriba el contenido **@{activity('LookupCurrentWaterMark').output.firstRow.NewWatermarkValue}** y haga clic en **Finalizar**.  

     ![Escritura del contenido para el parámetro del procedimiento almacenado](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable10.png)      
     
11. Haga clic en **Depurar**, escriba los parámetros y haga clic en **Finalizar**.

    ![Clic en Depurar](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable11.png)

12. Ve el resultado disponible en el panel, como se muestra en el ejemplo siguiente:

    ![Revisión del resultado](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable12.png)

13. Puede crear nuevas filas en la tabla de origen.  El sql de ejemplo para crear nuevas filas puede ser así:

    ```sql
            INSERT INTO data_source_table
            VALUES (10, 'newdata','9/10/2017 2:23:00 AM')

            INSERT INTO data_source_table
            VALUES (11, 'newdata','9/11/2017 9:01:00 AM')
    ```
13. Ejecute la canalización de nuevo: haga clic en **Depurar**, escriba los parámetros de entrada y haga clic en **Finalizar**.

    ![Clic en Depurar](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable11.png)

14. Verá solo las filas nuevas que se copiaron en el destino.

15. (Opcional) Si selecciona SQL Data Warehouse como destino de datos, también necesita escribir la conexión de una instancia de Azure Blob Storage como almacenamiento provisional, porque así lo requiere SQL Data Warehouse Polybase.  Asegúrese de que ya ha creado el contenedor en Blob Storage.  
    
    ![Configuración de PolyBase](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable15.png)
    
## <a name="next-steps"></a>Pasos siguientes

- [Introducción al servicio Azure Data Factory](introduction.md)