---
title: Copia masiva de una base de datos mediante el uso de una tabla de control con Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo usar una plantilla de solución para copiar datos de forma masiva desde una base de datos mediante el uso de una tabla de control externo para almacenar una lista de particiones de tablas de origen mediante el uso de Azure Data Factory.
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
ms.openlocfilehash: c4224693642e8c9f76deedc0c8ad8586e122cc23
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57530576"
---
# <a name="bulk-copy-from-a-database-with-a-control-table"></a>Copia masiva de una base de datos con una tabla de control

Para copiar datos desde un almacén de datos en el servidor de Oracle, Teradata, Netezza o SQL Server a Azure SQL Data Warehouse, tendrá que cargar grandes cantidades de datos de varias tablas. Normalmente, los datos tienen que tener particiones en cada tabla para que pueda cargar filas con varios subprocesos en paralelo de una sola tabla. En este artículo se describe una plantilla para utilizarla en estos escenarios.

 >! Tenga en cuenta si desea copiar datos de un número reducido de tablas con el volumen de datos relativamente pequeñas en SQL Data Warehouse, es más eficaz utilizar la [herramienta Copy Data de Azure Data Factory](copy-data-tool.md). La plantilla que se describe en este artículo es más que necesita para ese escenario.

## <a name="about-this-solution-template"></a>Acerca de esta plantilla de solución

Esta plantilla recupera una lista de particiones de base de datos de origen para copiar de una tabla de control externo. A continuación, recorre en iteración cada partición en la base de datos de origen y copia los datos en el destino.

La plantilla contiene tres actividades:
- **Búsqueda** recupera la lista de particiones de base de datos está seguro de una tabla de control externo.
- **ForEach** Obtiene la lista de particiones de la actividad de búsqueda y recorre en iteración cada partición para la actividad de copia.
- **Copia** copia cada partición desde el almacén de base de datos de origen en el almacén de destino.

La plantilla define cinco parámetros:
- *Control_Table_Name* es la tabla de control externo, que almacena la lista de partición para la base de datos de origen.
- *Control_Table_Schema_PartitionID* es el nombre del nombre de columna en la tabla de control externo que almacena el identificador de cada partición. Asegúrese de que el Id. de partición es único para cada partición en la base de datos de origen.
- *Control_Table_Schema_SourceTableName* es la tabla de control externo que almacena cada nombre de tabla de la base de datos de origen.
- *Control_Table_Schema_FilterQuery* es el nombre de la columna en la tabla de control externo que almacena la consulta de filtro para obtener los datos de cada partición en la base de datos de origen. Por ejemplo, si la partición de los datos por año, la consulta que se almacena en cada fila podría ser similar a ' Seleccionar * desde el origen de datos donde LastModifytime > = '' 2015-01-01 00:00:00 '' y LastModifytime < = '' 23:59:59.999 2015-12-31'' '.
- *Data_Destination_Folder_Path* es la ruta de acceso donde se copian los datos en el almacén de destino. Este parámetro solo está visible si el destino que elija es almacenamiento basado en archivos. Si elige SQL Data Warehouse como almacén de destino, este parámetro no es necesario. Pero los nombres de tabla y el esquema en el almacén de datos de SQL deben ser igual que las de la base de datos de origen.

## <a name="how-to-use-this-solution-template"></a>Uso de esta plantilla de solución

1. Crear una tabla de control en SQL Server o Azure SQL Database para almacenar la lista de particiones de base de datos de origen para la copia masiva. En el ejemplo siguiente, hay cinco particiones en la base de datos de origen. Son tres particiones para la *datasource_table*, y dos para el *project_table*. La columna *LastModifytime* se utiliza para dividir los datos de tabla *datasource_table* desde la base de datos de origen. La consulta que se utiliza para leer la primera partición es ' Seleccionar * desde datasource_table donde LastModifytime > = '' 2015-01-01 00:00:00 '' y LastModifytime < = '' 23:59:59.999 2015-12-31'' '. Puede usar una consulta similar para leer datos de otras particiones.

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

2. Vaya a la **de base de datos de copia masiva** plantilla. Crear un **New** conexión a la tabla de control externo que ha creado en el paso 1.

    ![Creación de una nueva conexión a la tabla de control](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable2.png)

3. Crear un **New** conexión a la base de datos de origen que se va a copiar datos desde.

     ![Creación de una nueva conexión a la base de datos de origen](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable3.png)
    
4. Crear un **New** almacenar de conexión a los datos de destino que va a copiar los datos a.

    ![Creación de una nueva conexión al almacén de destino](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable4.png)

5. Seleccione **usar esta plantilla**.

    ![Uso de esta plantilla](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable5.png)
    
6. Verá la canalización, como se muestra en el ejemplo siguiente:

    ![Revisión de la canalización](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable6.png)

7. Seleccione **depurar**, escriba el **parámetros**y, a continuación, seleccione **finalizar**.

    ![Haga clic en ** ** de depuración](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable7.png)

8. Debería ver resultados similares al ejemplo siguiente:

    ![Revisión del resultado](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable8.png)

9. (Opcional) Si eligió SQL Data Warehouse como destino de los datos, debe especificar una conexión a Azure Blob storage para el almacenamiento provisional, según sea necesario por Polybase en SQL Data Warehouse. Asegúrese de que se ha creado el contenedor de Blob storage.
    
    ![Configuración de PolyBase](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable9.png)
       
## <a name="next-steps"></a>Pasos siguientes

- [Introducción al servicio Azure Data Factory](introduction.md)
