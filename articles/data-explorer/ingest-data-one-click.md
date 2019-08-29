---
title: Uso de ingesta con un solo clic para ingerir datos en Azure Data Explorer
description: Obtenga información sobre cómo ingerir (cargar) datos en Azure Data Explorer mediante la ingesta con un solo clic.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/14/2019
ms.openlocfilehash: b1ce2d9efe44021b4e3191739bd2f922e34c44cb
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69519831"
---
# <a name="use-one-click-ingestion-to-ingest-data-into-azure-data-explorer"></a>Uso de ingesta con un solo clic para ingerir datos en Azure Data Explorer

En este artículo se muestra cómo usar la ingesta con un solo clic para la ingesta rápida de una nueva tabla en formato JSON o CSV desde el almacenamiento en Azure Data Explorer. Una vez que se ingieren los datos, puede editar la tabla y ejecutar consultas mediante la interfaz de usuario web.

## <a name="prerequisites"></a>Requisitos previos

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
* Inicie sesión en [la aplicación](https://dataexplorer.azure.com/).
* Cree un [clúster y una base de datos de Azure Data Explorer](create-cluster-database-portal.md).
* Origen de datos en Azure Storage.

## <a name="ingest-new-data"></a>Ingesta de nuevos datos

1. Haga clic con el botón derecho en *Database name* (Nombre de la base de datos) y seleccione **Ingest new data (Preview)** [Ingerir nuevos datos (versión preliminar)]

    ![selección de ingesta con un solo clic en la interfaz de usuario web](media/ingest-data-one-click/one-click-ingestion-in-webui.png)   
 
1. En la ventana **Data Ingestion (Preview)** [Ingesta de datos (versión preliminar)], en la pestaña **Source** (Origen), complete los **detalles del proyecto**:

    * Introduzca el nuevo nombre en **Table name** (Nombre de tabla). 
    * Seleccione **Ingestion type** > **from storage** (Tipo de ingesta > De almacenamiento).
    * Escriba la dirección URL al almacenamiento en **Link to storage** (Vínculo al almacenamiento). Use una dirección URL de SAS de blob para las cuentas de almacenamiento privadas. 
    * Seleccione **Editar esquema**.
 
    ![detalles del origen de la ingesta con un solo clic](media/ingest-data-one-click/one-click-ingestion-source.png) 

1. En la pestaña **Schema** (Esquema), seleccione **Data format** (Formato de datos) en la lista desplegable > **JSON** o **CSV**. 
   
   Si selecciona **CSV**:
    * Active la casilla **Ignore headline** (Omitir encabezado) para omitir la fila de encabezado del archivo CSV.    
    * **Mapping name** (Nombre de asignación) se establece automáticamente, pero se puede editar.

    ![esquema de formato csv de ingesta con un solo clic.png](media/ingest-data-one-click/one-click-csv-format.png)

   Si selecciona **JSON**:
    * Seleccione **JSON levels** (Niveles de JSON): 1 a 10 en la lista desplegable. Los niveles del archivo JSON se muestran en la tabla de la parte inferior derecha. 
    * **Mapping name** (Nombre de asignación) se establece automáticamente, pero se puede editar.

    ![esquema de formato json de ingesta con un solo clic](media/ingest-data-one-click/one-click-json-format.png)  

1. En **Editor**, seleccione **V** a la derecha para abrirlo. En el editor, puede ver y copiar las consultas automáticas generadas a partir de los valores que ha introducido. 

1.  En la tabla de la parte inferior derecha: 
    * Seleccione **V** a la derecha de la columna para **cambiar el nombre de la columna**, **eliminar la columna**, **orden ascendente** u **orden descendente**.
    * Haga doble clic en el nombre de columna para editarlo.
    * Seleccione el icono situado a la izquierda del nombre de la columna para cambiar el tipo de datos. 

1. Seleccione **Start ingestion** (Iniciar ingesta) para crear tablas, crear asignaciones e ingerir datos.
 
## <a name="query-data"></a>Datos de consulta

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
