---
title: Copia masiva desde base de datos con tabla de control con Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo usar una plantilla de solución para copiar completamente los datos de forma masiva desde una base de datos con una tabla de control externa a fin de almacenar la lista de particiones de las tablas de origen con Azure Data Factory.
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
ms.date: 12/14/2018
ms.openlocfilehash: b267da18f2537e462ecda0ac265eac07a069c293
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967009"
---
# <a name="bulk-copy-from-database-with-control-table"></a>Copia masiva desde base de datos con tabla de control

Cuando desea copiar datos desde un almacenamiento de datos como el servidor de Oracle, el servidor de Netezza, el servidor de Teradata o SQL Server en Azure, tiene que cargar la enorme cantidad de datos de varias tablas en orígenes de datos. En la mayoría de los casos, los datos se tienen que dividir aún más en cada tabla para poder cargar filas con varios subprocesos en paralelo desde una única tabla. La plantilla presente está diseñada para ese caso. 

Si desea copiar los datos de un número reducido de tablas con un tamaño pequeño de datos, resulta más eficaz ir a la "herramienta Copiar datos" para tener una sola actividad de copia, o la actividad foreach + actividad de copia en su canalización. Esta plantilla es más de lo que necesita para este sencillo caso de uso.

## <a name="about-this-solution-template"></a>Acerca de esta plantilla de solución

Esta plantilla recupera la lista de particiones de la base de datos de origen desde una tabla de control externa que necesita copiarse en el almacén de destino y, a continuación, recorre en iteración cada partición en la base de datos de origen y realiza la operación de copia de datos.

La plantilla contiene tres actividades:
-   Una actividad de **búsqueda** para recuperar la lista de particiones de la base de datos de origen desde una tabla de control externa.
-   Una actividad **ForEach** para obtener la lista de particiones de la actividad de búsqueda y, a continuación, recorrer en iteración cada una de ellas para la actividad de copia.
-   Una actividad de **copia** para copiar cada partición del almacén de base de datos de origen en el almacén de destino.

La plantilla define cinco parámetros:
-   El parámetro *Control_Table_Name* es el nombre de tabla para la tabla de control externo. La tabla de control se utiliza para almacenar la lista de particiones de la base de datos de origen.
-   El parámetro *Control_Table_Schema_PartitionID* es el nombre de columna en la tabla de control externa para almacenar cada identificador de partición. Asegúrese de que el identificador de partición es único para cada partición en la base de datos de origen.
-   El parámetro *Control_Table_Schema_SourceTableName* es el nombre de columna en la tabla de control externa para almacenar cada nombre de tabla desde la base de datos de origen.
-   El parámetro *Control_Table_Schema_FilterQuery* es el nombre de columna en la tabla de control externa para almacenar la consulta de filtro para obtener los datos de cada partición de la base de datos de origen. Por ejemplo, si realizó una partición en los datos por cada año, la consulta almacenada en cada fila debe ser similar a select * from datasource where LastModifytime >= ''2015-01-01 00:00:00'' and LastModifytime <= ''2015-12-31 23:59:59.999''.
-   El parámetro *Data_Destination_Folder_Path* es la ruta de acceso de la carpeta donde se copian los datos en el almacén de destino.  Este parámetro solo está visible cuando el destino que elija es un almacenamiento basado en archivo.  Si elige SQL Data Warehouse como almacén de destino, no es necesario especificar aquí ningún parámetro. Pero los nombres de tabla y el esquema en SQL Data Warehouse deben ser iguales que los de la base de datos de origen.

## <a name="how-to-use-this-solution-template"></a>Uso de esta plantilla de solución

1. Cree una tabla de control en un servidor SQL Server o SQL Azure para almacenar la lista de particiones de la base de datos de origen para la copia masiva.  En el ejemplo siguiente, puede ver que hay cinco particiones en la base de datos de origen, donde tres particiones son para una tabla:*datasource_table* y dos particiones son para otra tabla:*project_table*. La columna *LastModifytime* se utiliza para dividir los datos de la tabla *datasource_table* desde la base de datos de origen. La consulta que se usa para leer la primera partición es select * from datasource_table where LastModifytime >= ''2015-01-01 00:00:00'' and LastModifytime <= ''2015-12-31 23:59:59.999''.  También puede ver la consulta similar para leer datos de otras particiones. 

     ```sql
            Create table ControlTableForTemplate
            (
            PartitionID int,
            SourceTableName  varchar(255),
            FilterQuery varchar(255)
            );

            INSERT INTO ControlTableForTemplate
            (PartitionID, SourceTableName, FilterQuery)
            VALUES
            (1, 'datasource_table','select * from datasource_table where LastModifytime >= ''2015-01-01 00:00:00'' and LastModifytime <= ''2015-12-31 23:59:59.999'''),
            (2, 'datasource_table','select * from datasource_table where LastModifytime >= ''2016-01-01 00:00:00'' and LastModifytime <= ''2016-12-31 23:59:59.999'''),
            (3, 'datasource_table','select * from datasource_table where LastModifytime >= ''2017-01-01 00:00:00'' and LastModifytime <= ''2017-12-31 23:59:59.999'''),
            (4, 'project_table','select * from project_table where ID >= 0 and ID < 1000'),
            (5, 'project_table','select * from project_table where ID >= 1000 and ID < 2000');
    ```

2. Vaya a la plantilla de **copia masiva desde base de datos** y cree una **nueva conexión** a la tabla de control externa.  Esta se conecta a la base de datos donde había creado la tabla de control en el paso 1.

    ![Creación de una nueva conexión a la tabla de control](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable2.png)

3. Cree una **nueva conexión** a la base de datos de origen desde donde se copian los datos.

     ![Creación de una nueva conexión a la base de datos de origen](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable3.png)
    
4. Cree una **nueva conexión** al almacén de datos de destino donde se copiarán los datos.

    ![Creación de una nueva conexión al almacén de destino](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable4.png)

5. Haga clic en **Usar esta plantilla**.

    ![Uso de esta plantilla](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable5.png)
    
6. Ve la canalización disponible en el panel, como se muestra en el ejemplo siguiente:

    ![Revisión de la canalización](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable6.png)

7. Haga clic en **Depurar**, escriba los parámetros y haga clic en **Finalizar**.

    ![Clic en Depurar](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable7.png)

8. Ve el resultado disponible en el panel, como se muestra en el ejemplo siguiente:

    ![Revisión del resultado](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable8.png)

9. (Opcional) Si selecciona SQL Data Warehouse como destino de datos, también necesita escribir la conexión de una instancia de Azure Blob Storage como almacenamiento provisional, porque así lo requiere SQL Data Warehouse Polybase.  Asegúrese de que ya ha creado el contenedor en Blob Storage.  
    
    ![Configuración de PolyBase](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable9.png)
       
## <a name="next-steps"></a>Pasos siguientes

- [Introducción al servicio Azure Data Factory](introduction.md)