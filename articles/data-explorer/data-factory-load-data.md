---
title: Copiar datos de Azure Data Factory en el Explorador de datos de Azure
description: En este tema, aprenderá a introducir datos de (cargar) en el Explorador de datos de Azure mediante el uso de la herramienta de copia de Azure Data Factory
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: 64856d53168a7676cf279da2d8675ce81e1985f7
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60005373"
---
# <a name="copy-data-to-azure-data-explorer-using-azure-data-factory"></a>Copiar datos en el Explorador de datos de Azure mediante Azure Data Factory 

El Explorador de datos de Azure es un servicio de análisis de datos rápido, totalmente administrado para análisis en tiempo real en grandes volúmenes de datos de transmisión por secuencias desde diversos orígenes, como aplicaciones, sitios Web y dispositivos de IoT. Explorar los datos de forma iterativa e identificar patrones y anomalías para mejorar los productos, mejorar las experiencias de cliente, supervisar los dispositivos y mejorar las operaciones. Explore nuevas preguntas y obtenga respuestas en solo unos minutos. Azure Data Factory es un servicio de integración de datos en la nube totalmente administrado. Puede usar el servicio para rellenar la base de datos del explorador de datos de Azure con los datos del sistema actual y ahorrar tiempo al compilar las soluciones de análisis.

Azure Data Factory ofrece las siguientes ventajas para cargar datos en el Explorador de datos de Azure:

* **Fácil de configurar**: Un asistente intuitivo de cinco pasos sin necesidad de scripting.
* **Almacén de datos enriquecidos admiten**: Compatibilidad integrada para un amplio conjunto de servidores locales y almacenes de datos basado en la nube. Para una lista detallada, consulte la tabla de [almacenes de datos admitidos](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats).
* **Seguro y conforme**: Se transfieran a través de HTTPS o ExpressRoute. La presencia del servicio global garantiza que los datos nunca abandonan el límite geográfico.
* **Alto rendimiento**: Hasta la velocidad de carga de datos de 1 GB/s en el Explorador de datos de Azure. Para más información, consulte el [rendimiento de la actividad de copia](/azure/data-factory/copy-activity-performance).

Este artículo muestra cómo usar la herramienta Copy Data de Data Factory para cargar datos de Amazon S3 en el Explorador de datos de Azure. Puede seguir pasos similares para copiar datos desde otros almacenes de datos, como [Azure Blob Storage](/azure/data-factory/connector-azure-blob-storage), [Azure SQL Database](/azure/data-factory/connector-azure-sql-database), [Azure SQL Data Warehouse](/azure/data-factory/connector-azure-sql-data-warehouse), [Google BigQuery](/azure/data-factory/connector-google-bigquery),[Oracle](/azure/data-factory/connector-oracle), y [sistema de archivos](/azure/data-factory/connector-file-system).

## <a name="prerequisites"></a>Requisitos previos

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
* [Un clúster de explorador de datos de Azure y la base de datos](create-cluster-database-portal.md)
* Origen de datos.

## <a name="create-a-data-factory"></a>Crear una factoría de datos

1. Seleccione el **crear un recurso** botón (+) en la esquina superior izquierda del portal > **Analytics** > **Data Factory**.

   ![Creación de una factoría de datos](media/data-factory-load-data/create-adf.png)

1. En el **nueva factoría de datos** página, proporcione valores para los campos siguientes y, a continuación, seleccione **crear**.

    ![Página New data factory (Nueva factoría de datos)](media/data-factory-load-data/my-new-data-factory.png)

    **Configuración**  | **Descripción del campo**
    |---|---|
    | **Nombre** | Escriba un nombre único global para la factoría de datos. Si recibe el error *"nombre de la factoría de datos \"LoadADXDemo\" no está disponible"*, escriba un nombre diferente para la factoría de datos. Para las reglas de nomenclatura de artefactos de Data Factory, consulte [las reglas de nomenclatura de Data Factory](/azure/data-factory/naming-rules).|
    | **Suscripción** | seleccione la suscripción de Azure donde desea crear la factoría de datos. |
    | **Grupo de recursos** | Seleccione **crear nuevo** y escriba el nombre de un grupo de recursos. Seleccione **usar existente**, si tiene un grupo de recursos existente. |
    | **Versión** | Seleccione **V2** |
    | **Ubicación** | Seleccione la ubicación de la factoría de datos. Solo las ubicaciones admitidas se muestran en la lista desplegable. Los almacenes de datos utilizados por la factoría de datos pueden ser en otras ubicaciones o regiones. |
    | | |

1. Seleccione las notificaciones en la barra de herramientas para supervisar el proceso de creación. Una vez completada la creación, vaya a la factoría de datos que creó. El **Data Factory** se abrirá la página principal.

   ![Página principal Factoría de datos](media/data-factory-load-data/data-factory-home-page.png)

1. Seleccione el **Author & Monitor** icono para iniciar la aplicación en una pestaña independiente.

## <a name="load-data-into-azure-data-explorer"></a>Cargar datos en el Explorador de datos de Azure

Se pueden cargar datos desde muchos tipos de [almacenes de datos](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats) en el Explorador de datos de Azure. En este tema se detalla al cargar datos de Amazon S3.

Hay dos maneras de cargar datos en el Explorador de datos de Azure mediante Azure Data Factory:

* Interfaz de usuario de Azure Data Factory - [ **autor** ficha](/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory)
* [Azure Data Factory **Copy Data** herramienta](/azure/data-factory/quickstart-create-data-factory-copy-data-tool) usados en este artículo.

### <a name="copy-data-from-amazon-s3-source"></a>Copiar datos desde Amazon S3 (origen)

1. En el **Comencemos** página, seleccione el **Copy Data** para iniciar la herramienta Copy Data.

   ![Icono de la herramienta Copy data](media/data-factory-load-data/copy-data-tool-tile.png)

1. En el **propiedades** , especifique **nombre de la tarea** y seleccione **siguiente**.

    ![Copia de las propiedades del origen](media/data-factory-load-data/copy-from-source.png)

1. En el **almacén de datos de origen** página, haga clic en **+ crear nueva conexión**.

    ![Creación de conexión de datos de origen](media/data-factory-load-data/source-create-connection.png)

1. Seleccione **Amazon S3**y, a continuación, seleccione **continuar**

    ![Nuevo servicio vinculado](media/data-factory-load-data/amazons3-select-new-linked-service.png)

1. En el **nuevo servicio vinculado (Amazon S3)** página, realice los pasos siguientes:

    ![Especificar servicio vinculado de Amazon S3](media/data-factory-load-data/amazons3-new-linked-service-properties.png)

    * Especificar **nombre** de su nuevo servicio vinculado.
    * Seleccione **conectarse mediante una instancia de integration runtime** valor en la lista desplegable.
    * Especifique el valor de **Access Key ID** (Identificador de clave de acceso).
    * Especifique el valor de **Secret Access Key** (Clave de acceso secreta).
    * Seleccione **Probar conexión** para probar la conexión de servicio vinculado que creó.
    * Seleccione **Finalizar**.

1. En el **almacén de datos de origen** página, verá la nueva conexión AmazonS31. Seleccione **Next** (Siguiente).

   ![Conexión creada del almacén de datos de origen](media/data-factory-load-data/source-data-store-created-connection.png)

1. En el **elegir el archivo de entrada o la carpeta** página:

    1. Vaya a la carpeta o el archivo que se va a copiar. Seleccione la carpeta o el archivo.
    1. Seleccione el comportamiento de copia según sea necesario. Mantener **copia binaria** desactivada.
    1. Seleccione **Next** (Siguiente).

    ![Elegir archivo o carpeta de entrada](media/data-factory-load-data/source-choose-input-file.png)

1. En el **formatos de archivo de configuración** página Seleccione la configuración pertinente para el archivo y haga clic en **siguiente**.

   ![Elegir archivo o carpeta de entrada](media/data-factory-load-data/source-file-format-settings.png)

### <a name="copy-data-into-azure-data-explorer-destination"></a>Copiar datos en el Explorador de datos de Azure (destino)

Azure Explorador de datos nuevo servicio vinculado se crea para copiar los datos en la tabla de destino del explorador de datos de Azure (receptor) especificados a continuación.

1. En el **almacén de datos de destino** página, puede usar conexión del almacén de datos existentes o especificar un nuevo almacén de datos, haga clic en **+ crear nueva conexión**.

    ![Página Destination data store (Almacén de datos de destino)](media/data-factory-load-data/destination-create-connection.png)

1. En el **nuevo servicio vinculado** página, seleccione **Explorador de Azure Data**y, a continuación, seleccione **continuar**

    ![Seleccione el Explorador de datos de Azure: nuevo servicio vinculado](media/data-factory-load-data/adx-select-new-linked-service.png)

1. En el **nuevo servicio vinculado (Explorador de datos de Azure)** página, realice los pasos siguientes:

    ![Nuevo servicio vinculado de ADX](media/data-factory-load-data/adx-new-linked-service.png)

   * Seleccione **nombre** datos del explorador de Azure de servicio vinculado.
   * En **método de selección de cuenta**: 
        * Seleccione el **suscripción de Azure** botón de radio y seleccione su **suscripción de Azure** cuenta. A continuación, seleccione su **clúster**. Tenga en cuenta la lista desplegable tendrá solo los clústeres de lista que pertenecen al usuario.
        * Como alternativa, seleccione **escribir manualmente** botón de radio y escriba su **extremo**.
    * Especifique el **inquilino**.
    * Escriba **Id. de entidad de servicio**.
    * Seleccione **clave principal del servicio** botón y escriba **clave de entidad de servicio**.
    * Seleccione su **base de datos** en el menú desplegable. Como alternativa, seleccione **editar** casilla y escriba el nombre de la base de datos.
    * Seleccione **Probar conexión** para probar la conexión de servicio vinculado que creó. Si puede conectarse a la instalación, una marca de verificación verde **conexión correcta** aparecerá.
    * Seleccione **finalizar** para completar la creación del servicio vinculado.

    > [!NOTE]
    > La entidad de servicio está usando Azure Data Factory para obtener acceso al servicio de explorador de datos de Azure. Para la entidad de servicio, [crear un Azure Active Directory (Azure AD) de entidad de servicio](/azure/azure-stack/azure-stack-create-service-principals#manage-service-principal-for-azure-ad). No utilice el **Azure Key Vault** método.

1. El **almacén de datos de destino** se abre. La conexión de datos del explorador de datos de Azure que creó está disponible para su uso. Seleccione **siguiente** para configurar la conexión.

    ![Almacén de datos de destino ADX](media/data-factory-load-data/destination-data-store.png)

1. En **asignación de tabla**, establezca el nombre de la tabla de destino y seleccione **siguiente**.

    ![Asignación de tabla del conjunto de datos de destino](media/data-factory-load-data/destination-dataset-table-mapping.png)

1. En el **asignación de columnas** página:
    * Se realiza la primera asignación ADF según [asignación de esquemas ADF](/azure/data-factory/copy-activity-schema-and-type-mapping)
        * Establecer el **asignaciones de columnas** para la tabla de destino de Azure Data Factory. Se muestra la asignación predeterminada de origen a la tabla de destino de ADF.
        * Anule la selección de las columnas que no es necesario definir la asignación de columna.
    * La segunda asignación se produce cuando los datos tabulares se ingieren en el Explorador de datos de Azure. La asignación se realiza conforme a [las reglas de asignación de CSV](/azure/kusto/management/mappings#csv-mapping). Tenga en cuenta que incluso si los datos de origen no estaban en formato CSV, ADF ha convertido los datos en formato tabular, por lo tanto, asignación de CSV es la asignación solo pertinente en esta fase.
        * En **propiedades del receptor de explorador de datos de Azure (Kusto)** agregar la correspondiente **nombre de la asignación de ingesta** (opcional) hasta esa asignación de columna se puede usar.
        * Si **nombre de la asignación de ingesta** no se especifica, orden de asignación "por nombre" definido en **asignaciones de columnas** se producirá la sección. Si se produce un error en la asignación de "por nombre", el Explorador de datos de Azure intentará introducir los datos en un orden "posición de columna por" (mapas por posición como valor predeterminado).
    * Seleccione **siguiente**

    ![Asignación de columnas del conjunto de datos de destino](media/data-factory-load-data/destination-dataset-column-mapping.png)

1. En la página **Configuración** :
    * Establecer la correspondiente **la configuración de tolerancia a errores**.
    * **Configuración de rendimiento**: Enable staging no es aplicable. **Configuración avanzada** incluir consideraciones de costos. Déjelo tal cual si se necesita ningún específico.
    * Seleccione **Next** (Siguiente).

    ![Configuración de copia de datos](media/data-factory-load-data/copy-data-settings.png)

1. En **resumen**, revise la configuración y seleccione **siguiente**.

    ![Copiar datos de resumen](media/data-factory-load-data/copy-data-summary.png)

1. En el **página implementación**:
    * Seleccione **Monitor** para cambiar a la **Monitor** pestaña y ver el estado de la canalización (flujo de datos, errores y progreso).
    * Seleccione **editar canalización** para editar los servicios vinculados, conjuntos de datos y canalizaciones.
    * Seleccione **finalizar** a la tarea de copia completa de datos

    ![Página Deployment (Implementación)](media/data-factory-load-data/deployment.png)

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre la [conector del explorador de Azure Data](/azure/data-factory/connector-azure-data-explorer) en Azure Data Factory.

* Más información acerca de cómo editar los servicios vinculados, conjuntos de datos y canalizaciones en la [interfaz de usuario de Data Factory](/azure/data-factory/quickstart-create-data-factory-portal).

* Obtenga información sobre [consultas del explorador de Azure Data](/azure/data-explorer/web-query-data) para consultar datos.
