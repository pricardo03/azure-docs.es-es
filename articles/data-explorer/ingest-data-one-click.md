---
title: Uso de ingesta con un solo clic para ingerir datos en Azure Data Explorer
description: Obtenga información sobre cómo ingerir (cargar) datos en Azure Data Explorer mediante la ingesta con un solo clic.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/31/2019
ms.openlocfilehash: c4ee4ed81cd4cc443a8f412462a5a7f204c91898
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2019
ms.locfileid: "74688187"
---
# <a name="use-one-click-ingestion-to-ingest-data-into-azure-data-explorer"></a>Uso de ingesta con un solo clic para ingerir datos en Azure Data Explorer

En este artículo se muestra cómo usar la ingesta con un solo clic para la ingesta rápida de una nueva tabla en formato JSON o CSV desde el almacenamiento o un archivo local en una tabla ya existente o en otra nueva de Azure Data Explorer. Use el intuitivo asistente y, en unos minutos, se habrán ingerido los datos y se podrá editar la tabla y ejecutar consultas mediante la interfaz de usuario web.

## <a name="prerequisites"></a>Requisitos previos

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
* Inicie sesión en [la aplicación](https://dataexplorer.azure.com/).
* Cree un [clúster y una base de datos de Azure Data Explorer](create-cluster-database-portal.md).
* Inicie sesión en la [interfaz de usuario web](https://dataexplorer.azure.com/) y [agregue una conexión al clúster](/azure/data-explorer/web-query-data#add-clusters)
* Origen de datos en Azure Storage.

## <a name="ingest-new-data"></a>Ingesta de nuevos datos

1. Haga clic con el botón derecho en la *base de datos* o fila de *tabla* en el menú izquierdo de la interfaz de usuario web y seleccione **Ingest new data (Preview)** [Ingerir datos nuevos (versión preliminar)].

    ![selección de ingesta con un solo clic en la interfaz de usuario web](media/ingest-data-one-click/one-click-ingestion-in-webui.png)   
 
1. En la ventana **Ingest new data (preview)** [Ingerir datos nuevos (versión preliminar)], en la pestaña **Source** (Origen), complete los **detalles del proyecto**:

    * **Tabla**: Seleccione el nombre de tabla existente en la lista desplegable o seleccione **Crear nuevo** para crear una nueva tabla.
    * Seleccione **Ingestion type** > **from storage** or **from file** (Tipo de ingesta > De almacenamiento o desde archivo).
        * Si seleccionó **De almacenamiento**, escriba **Vínculo a almacenamiento** para agregar la dirección URL al almacenamiento. Use una [dirección URL de SAS de blob](/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) para las cuentas de almacenamiento privadas. 
        * Si seleccionó **Desde archivo**, seleccione **Examinar** y arrastre el archivo al cuadro.
    * Seleccione **Editar esquema** para ver y editar la configuración de la columna de la tabla.
 
    ![detalles del origen de la ingesta con un solo clic](media/ingest-data-one-click/one-click-ingestion-source.png) 

    > [!TIP]
    > Si selecciona **Ingest new data (Preview)** [Ingerir datos nuevos (versión preliminar)], en una fila de *tabla*, el nombre de la tabla seleccionada aparecerá en los **Detalles del proyecto**.

1. Si seleccionó una tabla existente, se abrirá la ventana **Map columns** (Asignar columnas) para asignar columnas de datos de origen a las columnas de la tabla de destino. 
    * Use **Omit column** (Omitir columna) para quitar una columna de destino de la tabla. 
    * Use **New column** (Nueva columna) para agregar una nueva columna a la tabla. 

    ![Ventana Asignar columnas](media/ingest-data-one-click/one-click-map-columns-window.png)

1. En la pestaña **Esquema**:

    * Seleccione **Compression type** (Tipo de compresión) en la lista desplegable > **Uncompressed** (Sin comprimir) o **GZip**.
    * Seleccione **Formato de datos** en la lista desplegable > **JSON**, **CSV**, **TSV**, **SCSV**, **SOHSV**, **TSVE**o **PSV**. 
        * Al seleccionar el formato **JSON**, seleccione los **niveles de JSON**: 1-10. Los niveles afectan a la representación de datos de la columna de tabla. 
        * Si selecciona un formato distinto de JSON, active la casilla **Include column names** (Incluir los nombres de columna) para omitir la fila de encabezado del archivo.    
    * **Mapping name** (Nombre de asignación) se establece automáticamente, pero se puede editar.
    * Si seleccionó una tabla existente, puede seleccionar el botón **Asignar columnas** para abrir la ventana **Asignar columnas**.

    ![esquema de formato csv de ingesta con un solo clic.png](media/ingest-data-one-click/one-click-csv-format.png)

1. En **Editor**, seleccione **V** a la derecha para abrirlo. En el editor, puede ver y copiar las consultas automáticas generadas a partir de los valores que ha introducido. 

1.  En la tabla: 
    * Haga clic con el botón derecho en los encabezados de columna nuevos para **cambiar el tipo de datos**, **cambiar el nombre de la columna**, **eliminar la columna**, **filtrar de menor a mayor**o **filtrar de mayor a menor**. En las columnas existentes solo está disponible la ordenación de datos. 
    * Haga doble clic en el nuevo nombre de columna para editarlo.

1. Seleccione **Start ingestion** (Iniciar ingesta) para crear tablas, crear asignaciones e ingerir datos.

    ![esquema de formato json de ingesta con un solo clic](media/ingest-data-one-click/one-click-json-format.png) 
 
## <a name="query-data"></a>Consultar datos

1. En la ventana **Data ingestion completed** (Ingesta de datos completada), los tres pasos se marcarán con marcas de verificación verdes si la ingesta de datos se ha completado correctamente. 
 
    ![ingesta de datos con un solo clic completada](media/ingest-data-one-click/one-click-data-ingestion-complete.png)

1. Seleccione **V** para abrir la consulta. Copie en la interfaz de usuario web para editar la consulta.

1. El menú de la derecha contiene **Quick queries** (Consultas rápidas) y **Tools** (Herramientas). 

    * **Quick queries** (Consultas rápidas) incluye vínculos a la interfaz de usuario web con consultas de ejemplo.
    * **Tools** (Herramientas) incluye un vínculo a la interfaz de usuario web con **comandos de eliminación** que le permiten solucionar problemas ejecutando el comando `.drop` correspondiente.

    > [!TIP]
    > Pueden perderse datos al usar comandos `.drop`. Úselos con precaución.

## <a name="next-steps"></a>Pasos siguientes

* [Consulta de datos en la interfaz de usuario web de Azure Data Explorer](web-query-data.md)
* [Escritura de consultas para Azure Data Explorer mediante el lenguaje de consulta Kusto](write-queries.md)
