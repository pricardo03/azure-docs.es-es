---
title: Uso de una plantilla de Azure Data Factory para la copia masiva de la base de datos a Azure Data Explorer
description: En este tema, aprenderá a usar una plantilla de Azure Data Factory para la copia masiva de la base de datos a Azure Data Explorer
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/08/2019
ms.openlocfilehash: 5a6aebd276ef8658da9ca763be7da5c38a9c772a
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70873418"
---
# <a name="use-azure-data-factory-template-for-bulk-copy-from-database-to-azure-data-explorer"></a>Uso de una plantilla de Azure Data Factory para la copia masiva de la base de datos a Azure Data Explorer

Azure Data Explorer es un servicio de análisis de datos rápido y totalmente administrado para analizar en tiempo real grandes volúmenes de datos de que se transmiten desde muchos orígenes, como aplicaciones, sitios web y dispositivos IoT. Azure Data Factory es un servicio de integración de datos en la nube totalmente administrado. Puede usar el servicio de Azure Data Factory para rellenar la base de datos de Azure Data Explorer con los datos del sistema actual y ahorrar tiempo al crear las soluciones de análisis. 

Las [plantillas de Azure Data Factory](/azure/data-factory/solution-templates-introduction) son canalizaciones de Azure Data Factory predefinidas que le permiten empezar a trabajar rápidamente con Data Factory y reducir el tiempo de desarrollo para crear proyectos de integración de datos. La plantilla de **copia masiva de la base de datos a Azure Data Explorer** se crea mediante las actividades de **búsqueda** y **ForEach**. Puede usar la plantilla para crear muchas canalizaciones por base de datos o tabla para una copia más rápida de los datos. 

> [!IMPORTANT]
> * Use la plantilla de **copia masiva desde la base de datos a Azure Data Explorer** para copiar grandes cantidades de datos desde una bases de datos como SQL Server y Google BigQuery a Azure Data Explorer. 
> * Use la [herramienta de copia](data-factory-load-data.md) para copiar algunas tablas con cantidades pequeñas o moderadas de datos a Azure Data Explorer. 

## <a name="prerequisites"></a>Requisitos previos

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
* [Un clúster y la base de datos de Azure Data Explorer](create-cluster-database-portal.md).
* [Creación de una factoría de datos](data-factory-load-data.md#create-a-data-factory)
* Origen de datos en la base de datos

## <a name="create-controltabledataset"></a>Creación de ControlTableDataset

**ControlTableDataset** indica qué datos se copiarán del origen al destino en la canalización. El número de filas indica el número total de canalizaciones necesarias para copiar los datos. **ControlTableDataset** debe definirse como parte de la base de datos de origen.

Ejemplo de formato para una tabla de origen de SQL Server:
    
```sql   
CREATE TABLE control_table (
PartitionId int,
SourceQuery varchar(255),
ADXTableName varchar(255)
);
```
    
|Propiedad  |DESCRIPCIÓN  | Ejemplo
|---------|---------| ---------|
|PartitionId   |   orden de copia | 1  |  
|SourceQuery   |   consulta que indica qué datos se copiarán durante el tiempo de ejecución de la canalización | <br>`select * from table where lastmodifiedtime  LastModifytime >= ''2015-01-01 00:00:00''>` </br>    
|ADXTableName  |  nombre de la tabla de destino | MyAdxTable       |  

Si **ControlTableDataset** está en un formato diferente, cree un elemento **ControlTableDataset** comparable para el formato.

## <a name="use-bulk-copy-from-database-to-azure-data-explorer-template"></a>Uso de la copia masiva de la base de datos a la plantilla de Azure Data Explorer

1. En la página **Comencemos**, seleccione el icono **Create pipeline from template** (Crear canalización a partir de una plantilla) o seleccione el icono de lápiz (pestaña **Autor**) a la derecha > **+**  > **Pipeline from template** (Canalización a partir de plantilla) para abrir la galería de plantillas.

    ![Página Comencemos de Azure Data Factory](media/data-factory-template/adf-get-started.png)

1. Seleccione la plantilla **Copia masiva de la base de datos a Azure Data Explorer**.
 
    ![Seleccionar la canalización de la plantilla](media/data-factory-template/pipeline-from-template.png)

1.  En la ventana de **copia masiva desde la base de datos a Azure Data Explorer**, seleccione los conjuntos de datos existentes en la lista desplegable. 

    * **ControlTableDataset**: servicio vinculado a la tabla de control que indica qué datos se copian del origen al destino y dónde se colocarán en el destino. 
    * **SourceDataset**: servicio vinculado a la base de datos de origen. 
    * **AzureDataExplorerTable**: tabla de Azure Data Explorer. Si el conjunto de datos no existe, [cree el servicio vinculado de Azure Data Explorer](data-factory-load-data.md#create-the-azure-data-explorer-linked-service) para agregar el conjunto de datos.
    * Seleccione **Usar esta plantilla**.

    ![configuración de la plantilla de copia masiva de Azure Data Explorer](media/data-factory-template/configure-bulk-copy-adx-template.png)

1. Seleccione un área del lienzo (fuera de las actividades) para acceder a la canalización de la plantilla. Seleccione **Parámetros** para especificar los parámetros de la tabla, incluido el **Nombre** (nombre de la tabla de control) y el **Valor predeterminado** (nombres de columna).

    ![Parámetros de canalización](media/data-factory-template/pipeline-parameters.png)

1.  Seleccione la actividad de búsqueda **GetPartitionList** para ver la configuración predeterminada. Automáticamente se crea una consulta.
1.  Seleccione la actividad de comando **ForEachPartition** y seleccione **Configuración**
    * **Número de lotes**: seleccione un número entre 1 y 50. Esta selección determina el número de canalizaciones que se ejecutan en paralelo hasta que se alcanza el número de filas de **ControlTableDataset**. 
    * No active la casilla **Secuencial** para que los lotes de canalización se ejecuten en paralelo.

    ![Configuración de ForEachPartition](media/data-factory-template/foreach-partition-settings.png)

    > [!TIP]
    > El procedimiento recomendado consiste en ejecutar muchas canalizaciones en paralelo, para que los datos se puedan copiar más rápidamente. Particione los datos en la tabla de origen y asigne una partición por canalización, según la fecha y la tabla, para aumentar la eficacia.

1. Seleccione **Validar todo** para validar la canalización de ADF. Consulte **Salida de comprobación de canalización**.

    ![Validar las canalizaciones de la plantilla](media/data-factory-template/validate-template-pipelines.png)

1. Seleccione **Depurar** si es necesario y, a continuación, **Agregar desencadenador** para ejecutar la canalización.

    ![Ejecutar la canalización](media/data-factory-template/trigger-run-of-pipeline.png)    


Ahora puede usar la plantilla de **copia masiva desde la base de datos a Azure Data Explorer** para copiar eficazmente grandes cantidades de datos de las bases de datos y las tablas.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información acerca del procedimiento para [copiar datos a Azure Data Explorer mediante Azure Data Factory](data-factory-load-data.md).

* Puede obtener más información sobre [Azure Data Explorer](/azure/data-factory/connector-azure-data-explorer) en Azure Data Factory.

* Obtenga más información sobre las [consultas de Azure Data Explorer](/azure/data-explorer/web-query-data) para las consultas de datos.






