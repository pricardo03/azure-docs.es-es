---
title: Copia de datos de Azure Data Factory a Azure Data Explorer
description: En este tema, aprenderá a introducir (cargar) datos en Azure Data Explorer mediante el uso de la herramienta de copia de Azure Data Factory.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: 2142fbf03daa6667b20db43f9212a2b5e6d7dd44
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657532"
---
# <a name="copy-data-to-azure-data-explorer-using-azure-data-factory"></a>Copia de datos a Azure Data Explorer mediante Azure Data Factory 

Azure Data Explorer es un servicio de análisis de datos rápido y totalmente administrado para analizar en tiempo real grandes volúmenes de datos de que se transmiten desde muchos orígenes, como aplicaciones, sitios web y dispositivos IoT. Explore los datos de forma interactiva e identifique los patrones y anomalías para mejorar los productos y la experiencia del cliente, supervisar los dispositivos y aumentar las operaciones. Explore nuevas preguntas y obtenga respuestas en solo unos minutos. Azure Data Factory es un servicio de integración de datos en la nube totalmente administrado. Puede utilizar el servicio para rellenar la base de datos de Azure Data Explorer con los datos del sistema actual y ahorrar tiempo al crear las soluciones de análisis.

Azure Data Factory ofrece las siguientes ventajas para cargar datos en Azure Data Explorer:

* **Fácil de configurar**: un asistente intuitivo en 5 pasos sin necesidad de scripting.
* **Amplia compatibilidad para el almacenamiento de datos**: compatibilidad integrada para un amplio conjunto de almacenes de datos tanto locales como en la nube. Para una lista detallada, consulte la tabla de [almacenes de datos admitidos](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats).
* **Seguro y compatible**: los datos se transfieren a través de HTTPS o ExpressRoute. La presencia del servicio global garantiza que los datos nunca abandonan el límite geográfico.
* **Alto rendimiento**: hasta 1 GB/s de velocidad de carga de datos en Azure Data Explorer. Para más información, consulte el [rendimiento de la actividad de copia](/azure/data-factory/copy-activity-performance).

En este artículo se muestra cómo utilizar la herramienta Copy Data de Data Factory para cargar datos de Amazon S3 en Azure Data Explorer. Puede seguir pasos similares para copiar datos de otros tipos de almacenes de datos, como [Azure Blob Storage](/azure/data-factory/connector-azure-blob-storage), [Azure SQL Database](/azure/data-factory/connector-azure-sql-database), [Azure SQL Data Warehouse](/azure/data-factory/connector-azure-sql-data-warehouse), [Google BigQuery](/azure/data-factory/connector-google-bigquery),[Oracle](/azure/data-factory/connector-oracle) y [File System](/azure/data-factory/connector-file-system).

## <a name="prerequisites"></a>Requisitos previos

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
* [Un clúster y la base de datos de Azure Data Explorer](create-cluster-database-portal.md).
* Origen de datos.

## <a name="create-a-data-factory"></a>Crear una factoría de datos

1. Seleccione el botón **Crear un recurso** (+) de la esquina superior izquierda del portal > **Análisis** > **Factoría de datos**.

   ![Creación de una factoría de datos](media/data-factory-load-data/create-adf.png)

1. En la página **Nueva factoría de datos**, proporcione los valores para los siguientes campos y, a continuación, seleccione **Crear**.

    ![Página New data factory (Nueva factoría de datos)](media/data-factory-load-data/my-new-data-factory.png)

    **Configuración**  | **Descripción del campo**
    |---|---|
    | **Nombre** | Escriba un nombre único global para la factoría de datos. Si recibe el error *"El nombre de la factoría de datos \"LoadADXDemo\" no está disponible"* , escriba uno diferente. Para conocer las reglas de nomenclatura de los artefactos de Data Factory, consulte [Data Factory: reglas de nomenclatura](/azure/data-factory/naming-rules).|
    | **Suscripción** | seleccione la suscripción de Azure donde desea crear la factoría de datos. |
    | **Grupo de recursos** | Seleccione **Crear nuevo**y escriba el nombre de un nuevo grupo de recursos. Seleccione **Usar existente**, si dispone de un grupo de recursos existente. |
    | **Versión** | Seleccione **V2** |
    | **Ubicación** | Seleccione la ubicación de la factoría de datos. Solo las ubicaciones admitidas se muestran en la lista desplegable. Los almacenes de datos que las factorías de datos usan pueden estar en otras ubicaciones o regiones. |
    | | |

1. Para supervisar el proceso de creación, seleccione Notificaciones en la barra de herramientas. Una vez completada la creación, vaya a la factoría de datos que ha creado. Se abrirá la página principal de **Data Factory**.

   ![Página principal Factoría de datos](media/data-factory-load-data/data-factory-home-page.png)

1. Seleccione el icono **Author & Monitor** (Creación y supervisión) para iniciar la aplicación en una pestaña independiente.

## <a name="load-data-into-azure-data-explorer"></a>Carga de datos en Azure Data Explorer

Se pueden cargar datos desde muchos tipos de [almacenes de datos](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats) en Azure Data Explorer. En este tema se trata la carga de datos desde Amazon S3.

Hay dos formas de cargar datos en Azure Data Explorer mediante Azure Data Factory:

* Interfaz de usuario de Azure Data Factory: pestaña [**Author** (Creación)](/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory)
* Herramienta Copy Data de [Azure Data Factory **** ](/azure/data-factory/quickstart-create-data-factory-copy-data-tool) usada en este artículo.

### <a name="copy-data-from-amazon-s3-source"></a>Copia de datos desde Amazon S3 (origen)

1. En la página **Let's get started** (Introducción), seleccione el icono **Copy Data** para iniciar la herramienta Copy Data.

   ![Icono de la herramienta Copy Data](media/data-factory-load-data/copy-data-tool-tile.png)

1. En la página **Propiedades**, especifique **Nombre de tarea** y seleccione **Siguiente**.

    ![Copia de las propiedades del origen](media/data-factory-load-data/copy-from-source.png)

1. En la página **Almacén de datos de origen**, haga clic en **+ Crear nueva conexión**.

    ![Conexión creada mediante datos de origen](media/data-factory-load-data/source-create-connection.png)

1. Seleccione **Amazon S3** y, a continuación, seleccione **Continuar**.

    ![Nuevo servicio vinculado](media/data-factory-load-data/amazons3-select-new-linked-service.png)

1. En la página **New Linked Service (Amazon S3)** (Nuevo servicio vinculado [Amazon S3]), realice los pasos siguientes:

    ![Especificación del servicio vinculado de Amazon S3](media/data-factory-load-data/amazons3-new-linked-service-properties.png)

    * Especifique el **nombre** de su nuevo servicio vinculado.
    * Seleccione **Connect via integration runtime** (Conectar a través del entorno de ejecución de integración) desde el menú desplegable.
    * Especifique el valor de **Access Key ID** (Identificador de clave de acceso).
    * Especifique el valor de **Secret Access Key** (Clave de acceso secreta).
    * Seleccione **Probar conexión** para probar la conexión de servicio vinculado que creó.
    * Seleccione **Finalizar**.

1. En la página **Almacén de datos de origen**, verá la nueva conexión AmazonS31. Seleccione **Next** (Siguiente).

   ![Conexión creada mediante almacén de datos de origen](media/data-factory-load-data/source-data-store-created-connection.png)

1. En la página **Choose the input file or folder** (Elegir el archivo o la carpeta de entrada):

    1. Busque el archivo o la carpeta que desee copiar. Seleccione la carpeta o el archivo.
    1. Seleccione el comportamiento de la opción de copia según sea necesario. Conserve la opción **Binary copy** (Copia binaria) sin activar.
    1. Seleccione **Next** (Siguiente).

    ![Elegir archivo o carpeta de entrada](media/data-factory-load-data/source-choose-input-file.png)

1. En la página **file formats settings** (Configuración de formato de archivo), seleccione la configuración relevante para su archivo y haga clic en **Siguiente**.

   ![Elegir archivo o carpeta de entrada](media/data-factory-load-data/source-file-format-settings.png)

### <a name="copy-data-into-azure-data-explorer-destination"></a>Copia de datos en Azure Data Explorer (destino)

Se ha creado el nuevo servicio vinculado de Azure Data Explorer para copiar los datos en la tabla de destino de Azure Data Explorer (receptor) especificada a continuación.

1. En la página **Destination data store** (Almacén de datos de destino), puede usar conexión del almacén de datos existente o especificar un nuevo almacén de datos haciendo clic en **+ Crear nueva conexión**.

    ![Página Destination data store (Almacén de datos de destino)](media/data-factory-load-data/destination-create-connection.png)

1. En la página **New Linked Service** (Nuevo servicio vinculado), seleccione **Azure Data Explorer** y después **Continuar**.

    ![Selección de Azure Data Explorer: nuevo servicio vinculado](media/data-factory-load-data/adx-select-new-linked-service.png)

1. En la página **New Linked Service (Azure Data Explorer)** [Nuevo servicio vinculado (Azure Data Explorer)], siga estos pasos:

    ![ADX: nuevo servicio vinculado](media/data-factory-load-data/adx-new-linked-service.png)

   * Seleccione **Nombre** para el servicio vinculado de Azure Data Explorer.
   * En **Método de selección de cuenta**: 
        * Seleccione el botón de radio **Desde suscripción de Azure** y seleccione su cuenta de **suscripción de Azure**. A continuación, seleccione el **clúster**. Tenga en cuenta que la lista desplegable mostrará solo los clústeres que pertenecen al usuario.
        * También puede seleccionar el botón de radio de **introducción manual** y especificar su **punto de conexión**.
    * Especifique el **inquilino**.
    * Escriba el **Id. de entidad de servicio**.
    * Seleccione el botón **Clave de entidad de servicio** y especifique **Clave de entidad de servicio**.
    * Seleccione la **base de datos** en el menú desplegable. También puede activar la casilla **Editar** y especificar el nombre de su base de datos.
    * Seleccione **Probar conexión** para probar la conexión de servicio vinculado que creó. Si puede conectarse a la configuración, aparecerá una marca verde de **conexión correcta**.
    * Seleccione **Finalizar** para completar la creación del servicio vinculado.

    > [!NOTE]
    > Azure Data Factory usa la entidad de servicio para acceder al servicio de Azure Data Explorer. Para la entidad de servicio, [cree una entidad de servicio de Azure Active Directory (Azure AD)](/azure-stack/operator/azure-stack-create-service-principals#manage-an-azure-ad-service-principal). No utilice el método de **Azure Key Vault**.

1. Se abrirá el **almacén de datos de destino**. La conexión de datos de Azure Data Explorer que creó está disponible para su uso. Seleccione **Siguiente** para configurar la conexión.

    ![Almacén de datos de destino de ADX](media/data-factory-load-data/destination-data-store.png)

1. En **Asignación de tabla**, establezca el nombre de tabla de destino y seleccione **Siguiente**.

    ![Asignación de tabla de conjunto de datos de destino](media/data-factory-load-data/destination-dataset-table-mapping.png)

1. En la página **Asignación de columnas**:
    * ADF realiza la primera asignación según la [asignación de esquemas de ADF](/azure/data-factory/copy-activity-schema-and-type-mapping).
        * Establezca las **asignaciones de columna** para la tabla de destino de Azure Data Factory. La asignación predeterminada se muestra desde el origen hasta la tabla de destino de ADF.
        * Anule la selección de las columnas que no necesite para definir su asignación de columnas.
    * La segunda asignación se produce cuando estos datos tabulares se insertan en Azure Data Explorer. La asignación se realiza según las [reglas de asignación de CSV](/azure/kusto/management/mappings#csv-mapping). Tenga en cuenta que incluso si los datos de origen no estaban en formato CSV, ADF convierte los datos en formato tabular. Por lo tanto, la asignación de CSV es la única asignación relevante en esta fase.
        * En las **propiedades del receptor de Azure Data Explorer (Kusto)** , agregue el **nombre de asignación de ingesta** (opcional) de manera que se pueda usar la asignación de columnas.
        * Si no se especifica el **nombre de asignación de ingesta**, se define el orden de asignación por nombre en la sección **Asignaciones de columnas**. Si se produce un error en la asignación por nombre, Azure Data Explorer intentará introducir los datos en el orden de posición por columna (se asigna por posición de forma predeterminada).
    * Seleccione **Siguiente**.

    ![Asignación de columnas de conjunto de datos de destino](media/data-factory-load-data/destination-dataset-column-mapping.png)

1. En la página **Configuración** :
    * Establezca la **configuración de tolerancia a errores** relevante.
    * **Configuración de rendimiento**: el permiso de almacenamiento provisional no es aplicable. **Configuración avanzada** incluye las consideraciones sobre el coste. Déjelo tal cual si no hay necesidades específicas.
    * Seleccione **Next** (Siguiente).

    ![Copia de configuración de datos](media/data-factory-load-data/copy-data-settings.png)

1. En **Resumen**, revise la configuración y seleccione **Siguiente**.

    ![Resumen de copia de datos](media/data-factory-load-data/copy-data-summary.png)

1. En la página **Implementación**:
    * Seleccione **Monitor** para cambiar a la pestaña **Monitor** y ver el estado de la canalización (progreso, errores y flujo de datos).
    * Seleccione **Editar canalización** para poder editar los servicios vinculados, los conjuntos de datos y las canalizaciones.
    * Seleccione **Finalizar** para completar la tarea de datos de copia.

    ![Página Deployment (Implementación)](media/data-factory-load-data/deployment.png)

## <a name="next-steps"></a>Pasos siguientes

* Puede obtener más información sobre [Azure Data Explorer](/azure/data-factory/connector-azure-data-explorer) en Azure Data Factory.

* Puede obtener más información sobre la edición de servicios vinculados, conjuntos de datos y canalizaciones en la [interfaz de usuario de Data Factory](/azure/data-factory/quickstart-create-data-factory-portal).

* Obtenga más información sobre las [consultas de Azure Data Explorer](/azure/data-explorer/web-query-data) para las consultas de datos.
