---
title: Cargar datos desde SAP Business Warehouse mediante Azure Data Factory | Microsoft Docs
description: Uso de Azure Data Factory para copiar datos desde SAP Business Warehouse (BW)
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: jingwang
ms.openlocfilehash: 4cd61db3ec0e8d88c9b1c6d6ba427b120b3f1af1
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66152415"
---
# <a name="copy-data-from-sap-business-warehouse-by-using-azure-data-factory"></a>Copiar datos desde SAP Business Warehouse mediante Azure Data Factory

En este artículo se muestra cómo usar Azure Data Factory para copiar datos desde SAP Business Warehouse (BW) a través de concentrador abierto a Azure Data Lake Storage Gen2. Puede usar un proceso similar para copiar datos a otro [almacenes de datos receptores admitidos](copy-activity-overview.md#supported-data-stores-and-formats).

> [!TIP]
> Para obtener información general sobre cómo copiar datos desde SAP BW, incluida la integración de concentrador abierto de SAP BW y flujo de extracción de delta, vea [copiar datos desde SAP Business Warehouse a través de concentrador abierto mediante Azure Data Factory](connector-sap-business-warehouse-open-hub.md).

## <a name="prerequisites"></a>Requisitos previos

- **Azure Data Factory**: Si no tiene uno, siga los pasos necesarios para [crear una factoría de datos](quickstart-create-data-factory-portal.md#create-a-data-factory).

- **SAP BW abrir Centro de destino (GI) con el tipo de destino "Tabla de base de datos"**: Para crear un GI o para comprobar que su GI está configurado correctamente para la integración de Data Factory, consulte el [las configuraciones de destino de concentrador abierto de SAP BW](#sap-bw-open-hub-destination-configurations) sección de este artículo.

- **El usuario de SAP BW tiene los permisos siguientes**:

  - Autorización para las llamadas a funciones remotas (RFC) y SAP BW.
  - Permisos para la actividad "Ejecutar" de la **S_SDSAUTH** objeto de autorización.

- **Un [integration runtime (IR) autohospedado](concepts-integration-runtime.md#self-hosted-integration-runtime) con el conector SAP .NET 3.0**. Siga estos pasos de configuración:

  1. Instale y registre el entorno integration runtime autohospedado, versión 3,13 o posterior. (Este proceso se describe más adelante en este artículo).

  2. Descargue el [conector de SAP de 64 bits para Microsoft .NET 3.0](https://support.sap.com/en/product/connectors/msnet.html) desde el sitio Web de SAP e instalarlo en el mismo equipo que el Integration Runtime autohospedado. Durante la instalación, asegúrese de que selecciona **instalar ensamblados en GAC** en el **pasos de configuración opcionales** cuadro de diálogo, como se muestra en la siguiente imagen:

     ![Configurar el cuadro de diálogo del conector SAP para .NET](media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

## <a name="do-a-full-copy-from-sap-bw-open-hub"></a>Hacer una copia completa de concentrador abierto de SAP BW

En el portal de Azure, vaya a la factoría de datos. Seleccione **Author & Monitor** para abrir la interfaz de usuario de la factoría de datos en una pestaña independiente.

1. En el **Comencemos** página, seleccione **Copy Data** para abrir la herramienta Copy Data.

2. En el **propiedades** , especifique un **nombre de la tarea**y, a continuación, seleccione **siguiente**.

3. En el **almacén de datos de origen** página, seleccione **+ crear nueva conexión**. Seleccione **concentrador abierto de SAP BW** desde la Galería de conector y, a continuación, seleccione **continuar**. Para filtrar los conectores, puede escribir **SAP** en el cuadro de búsqueda.

4. En el **conexión de concentrador abierto de especificar SAP BW** página, siga estos pasos para crear una nueva conexión.

   ![Página Crear servicio vinculado de concentrador abierto de SAP BW](media/load-sap-bw-data/create-sap-bw-open-hub-linked-service.png)

   1. Desde el **conectarse mediante una instancia de integration runtime** lista, seleccione una existente autohospedados. O bien, optar por crear uno si aún no tiene uno.

      Para crear un nuevo Integration Runtime autohospedado, seleccione **+ nuevo**y, a continuación, seleccione **autohospedado**. Escriba un **nombre**y, a continuación, seleccione **siguiente**. Seleccione **configuración rápida** para instalar en el equipo actual, o seguir la **instalación Manual** pasos que se proporcionan.

      Como se mencionó en [requisitos previos](#prerequisites), asegúrese de que tiene el conector de SAP para Microsoft .NET 3.0 instalado en el mismo equipo donde se ejecuta Integration Runtime autohospedado.

   2. Rellene el SAP BW **nombre del servidor**, **número del sistema**, **Id. de cliente,** **lenguaje** (if distinto **EN**) , **Nombre de usuario**, y **contraseña**.

   3. Seleccione **Probar conexión** para validar la configuración y, a continuación, seleccione **finalizar**.

   4. Se crea una nueva conexión. Seleccione **Next** (Siguiente).

5. En el **seleccionar destinos de concentrador abierto** página, busque los destinos de concentrador abierto que están disponibles en su SAP BW. Seleccione el GI para copiar datos desde y, a continuación, seleccione **siguiente**.

   ![Seleccione la tabla de destino de concentrador abierto de SAP BW](media/load-sap-bw-data/select-sap-bw-open-hub-table.png)

6. Especifique un filtro, si la necesita. Si su GI solo contiene datos de la ejecución de un proceso (DTP) de transferencia de datos única con un identificador de solicitud única o está seguro de que finalice la DTP y van a copiar los datos, desactive la **excluir la última solicitud** casilla de verificación.

   Más información sobre estos valores en el [las configuraciones de destino de concentrador abierto de SAP BW](#sap-bw-open-hub-destination-configurations) sección de este artículo. Seleccione **validar** comprobar qué datos se devolverán. Luego, seleccione **Siguiente**.

   ![Configurar filtro de concentrador abierto de SAP BW](media/load-sap-bw-data/configure-sap-bw-open-hub-filter.png)

7. En el **almacén de datos de destino** página, seleccione **+ crear nueva conexión** > **Azure Data Lake Storage Gen2**  >   **Continuar**.

8. En el **conexión especificar Azure Data Lake Storage** página, siga estos pasos para crear una conexión.

   ![Crear una página de servicio vinculado de ADLS Gen2](media/load-sap-bw-data/create-adls-gen2-linked-service.png)

   1. Seleccione su cuenta de Data Lake Storage compatible con Gen2 desde el **nombre** lista desplegable.
   2. Seleccione **Finalizar** para crear la conexión. Luego, seleccione **Siguiente**.

9. En el **elegir el archivo de salida o la carpeta** , escriba **copyfromopenhub** como el nombre de la carpeta de salida. Luego, seleccione **Siguiente**.

   ![Elija la página de la carpeta de salida](media/load-sap-bw-data/choose-output-folder.png)

10. En el **configuración de formato de archivo** página, seleccione **siguiente** para usar la configuración predeterminada.

    ![Especificar la página de formato de receptor](media/load-sap-bw-data/specify-sink-format.png)

11. En el **configuración** , expanda **configuración de rendimiento**. Escriba un valor para **grado de paralelismo copia** como 5 para cargar desde SAP BW en paralelo. Luego, seleccione **Siguiente**.

    ![Configurar opciones de copia](media/load-sap-bw-data/configure-copy-settings.png)

12. En el **resumen** página, revise la configuración. Luego, seleccione **Siguiente**.

13. En el **implementación** página, seleccione **Monitor** para supervisar la canalización.

    ![Página Deployment (Implementación)](media/load-sap-bw-data/deployment.png)

14. Tenga en cuenta que el **Monitor** ficha en el lado izquierdo de la página se selecciona automáticamente. El **acciones** columna incluye vínculos para ver los detalles de ejecución de actividad y volver a ejecutar la canalización.

    ![Vista de supervisión de canalización](media/load-sap-bw-data/pipeline-monitoring.png)

15. Para ver las ejecuciones de actividad asociadas con la ejecución de canalización, seleccione **View Activity Runs** en el **acciones** columna. Como solo hay una actividad (actividad de copia) en la canalización, solo verá una entrada. Para volver a la vista de ejecuciones de canalización, seleccione la **canalizaciones** vínculo en la parte superior. Seleccione **Refresh** (Actualizar) para actualizar la lista.

    ![Pantalla supervisión de actividades](media/load-sap-bw-data/activity-monitoring.png)

16. Para supervisar los detalles de ejecución para cada actividad de copia, seleccione el **detalles** vínculo, que es un icono de gafas siguiente **acciones** en la vista de supervisión de actividades. Detalles disponibles incluyen el volumen de datos copiados del origen al receptor, el rendimiento de los datos, los pasos de ejecución y duración y configuraciones utilizadas.

    ![Detalles de supervisión de actividad](media/load-sap-bw-data/activity-monitoring-details.png)

17. Para ver el **Id. de solicitud máximo**, vuelva a la vista y seleccione la supervisión de actividades **salida** en **acciones**.

    ![Pantalla de salida de actividad](media/load-sap-bw-data/activity-output.png)

    ![Vista de detalles de salida de actividad](media/load-sap-bw-data/activity-output-details.png)

## <a name="do-an-incremental-copy-from-sap-bw-open-hub"></a>Hacer una copia incremental de concentrador abierto de SAP BW

> [!TIP]
> Consulte [flujo de extracción de concentrador abierto de SAP BW conector delta](connector-sap-business-warehouse-open-hub.md#delta-extraction-flow) para obtener información sobre cómo el conector de SAP BW Open Hub en Data Factory copia los datos incrementales desde SAP BW. En este artículo también puede ayudarle a entender la configuración básica del conector.

Ahora, vamos a seguir configurar la copia incremental de concentrador abierto de SAP BW.

Copia incremental utiliza un mecanismo de "límite máximo" que se basa en el **Id. de solicitud**. Ese identificador se genera automáticamente en el destino de concentrador abierto de SAP BW el DTP. El diagrama siguiente muestra este flujo de trabajo:

![Diagrama de flujo de flujo de trabajo de copia incremental](media/load-sap-bw-data/incremental-copy-workflow.png)

En la factoría de datos **Comencemos** página, seleccione **creación de una canalización de plantilla** para usar la plantilla integrada.

1. Busque **SAP BW** para buscar y seleccionar la **Incremental copiar desde SAP BW en Azure Data Lake Storage Gen2** plantilla. Esta plantilla copia los datos en Azure Data Lake Storage Gen2. Puede usar un flujo de trabajo similar para copiar a otros tipos de receptor.

2. En la página principal de la plantilla, seleccione o cree las siguientes tres conexiones y, a continuación, seleccione **usar esta plantilla** en la esquina inferior derecha de la ventana.

   - **Almacenamiento de blobs de Azure**: En este tutorial, usamos almacenamiento de blobs de Azure para almacenar el límite máximo, que es el *max Copiar identificador de solicitud*.
   - **SAP BW Open Hub**: Este es el origen para copiar datos de. Consulte el tutorial anterior de copia completa de la configuración detallada.
   - **Azure Data Lake Storage Gen2**: Esto es que el receptor desea copiar los datos. Consulte el tutorial anterior de copia completa de la configuración detallada.

   ![Copia incremental de la plantilla de SAP BW](media/load-sap-bw-data/incremental-copy-from-sap-bw-template.png)

3. Esta plantilla genera una canalización con las siguientes tres actividades y hace que sean encadenada en caso de éxito: *Búsqueda*, *copiar datos*, y *Web*.

   Vaya a la canalización **parámetros** ficha. Vea todas las configuraciones que debe proporcionar.

   ![Copia incremental de la configuración de SAP BW](media/load-sap-bw-data/incremental-copy-from-sap-bw-pipeline-config.png)

   - **SAPOpenHubDestinationName**: Especifique el nombre de tabla de concentrador abierto para copiar datos de.

   - **ADLSGen2SinkPath**: Especifique la ruta de acceso de Azure Data Lake Storage Gen2 de destino para copiar los datos. Si no existe la ruta de acceso, la actividad de copia de Data Factory crea una ruta de acceso durante la ejecución.

   - **HighWatermarkBlobPath**: Especifique la ruta de acceso para almacenar el valor de límite máximo, tales como `container/path`.

   - **HighWatermarkBlobName**: Especifique el nombre del blob para almacenar el valor de límite máximo, tales como `requestIdCache.txt`. En el almacenamiento de blobs, vaya a la ruta de acceso correspondiente de HighWatermarkBlobPath + HighWatermarkBlobName, como *container/path/requestIdCache.txt*. Creación de un blob con contenido 0.

      ![Contenido del blob](media/load-sap-bw-data/blob.png)

   - **LogicAppURL**: En esta plantilla, usamos WebActivity para llamar a Azure Logic Apps para establecer el valor de límite máximo de Blob storage. O bien, puede usar Azure SQL Database para almacenarla. Usar una actividad de procedimiento almacenado para actualizar el valor.

      En primer lugar debe crear una aplicación lógica, como se muestra en la siguiente imagen. A continuación, pegue el **URL de HTTP POST**.

      ![Configuración de la aplicación lógica](media/load-sap-bw-data/logic-app-config.png)

      1. Vaya a Azure Portal. Seleccione un nuevo **Logic Apps** service. Seleccione **+ aplicación lógica en blanco** para ir a **Diseñador de Logic Apps**.

      2. Crear un desencadenador de **se recibe cuando una solicitud HTTP**. Especifique el cuerpo de solicitud HTTP como sigue:

         ```json
         {
            "properties": {
               "sapOpenHubMaxRequestId": {
                  "type": "string"
               }
            },
            "type": "object"
         }
         ```

      3. Agregar un **Create blob** acción. Para **ruta de acceso de carpeta** y **nombre del Blob**, utilice los mismos valores que ha configurado previamente en **HighWatermarkBlobPath** y **HighWatermarkBlobName**.

      4. Seleccione **Guardar**. A continuación, copie el valor de **URL de HTTP POST** a usar en la canalización de Data Factory.

4. Después de proporcionar los parámetros de canalización de Data Factory, seleccione **depurar** > **finalizar** para invocar una ejecución para validar la configuración. O bien, seleccione **publicar todo** para publicar los cambios y, a continuación, seleccione **desencadenador** para realizar una ejecución.

## <a name="sap-bw-open-hub-destination-configurations"></a>Configuraciones de destino de concentrador abierto de SAP BW

Esta sección presenta la configuración del lado de SAP BW para usar el conector de SAP BW Open Hub en Data Factory para copiar los datos.

### <a name="configure-delta-extraction-in-sap-bw"></a>Configuración de extracción delta en SAP BW

Si es necesario que las copias del historial y copia incremental o sólo copia incremental, configurar extracción delta en SAP BW.

1. Crear el destino de concentrador abierto. Puede crear el GI en SAP transacción RSA1, que crea automáticamente la transformación necesaria y el proceso de transferencia de datos. Use la configuración siguiente:

   - **ObjectType**: Puede usar cualquier tipo de objeto. En este caso, usamos **InfoCube** como ejemplo.
   - **Tipo de destino**: Seleccione **tabla de base de datos**.
   - **Clave de la tabla**: Seleccione **técnico clave**.
   - **Extracción**: Seleccione **mantener los datos y registros de insertar en tabla**.

   ![Crear el cuadro de diálogo de SAP BW GI delta extracción](media/load-sap-bw-data/create-sap-bw-ohd-delta.png)

   ![Crear el cuadro de diálogo de SAP BW GI delta2 extracción](media/load-sap-bw-data/create-sap-bw-ohd-delta2.png)

   Puede aumentar el número de paralelo que se ejecutan los procesos de trabajo SAP para el DTP que HABÍA:

   ![create-sap-bw-ohd-delta3](media/load-sap-bw-data/create-sap-bw-ohd-delta3.png)

2. Programar el DTP que HABÍA en cadenas de procesos.

   Una diferencia DTP para un cubo solo funciona si aún no se han comprimido las filas necesarias. Asegúrese de que no se está ejecutando compresión del cubo BW antes el DTP que HABÍA en la tabla de concentrador abierto. La manera más fácil de hacerlo es integrar el DTP que HABÍA en sus cadenas de procesos existentes. En el ejemplo siguiente, se inserta el DTP que HABÍA (para el GI) en la cadena de procesos entre el *ajustar* (de acumulación agregado) y *contraer* pasos (con compresión de cubo).

   ![Crear el diagrama de flujo de cadena de procesos de SAP BW](media/load-sap-bw-data/create-sap-bw-process-chain.png)

### <a name="configure-full-extraction-in-sap-bw"></a>Configuración de extracción completa en SAP BW

Además de la extracción de delta, puede ser conveniente una extracción completa de la misma InfoProvider BW de SAP. Normalmente, esto se aplica si desea realizar la copia completa, pero no es incremental, o desea [resync extracción delta](#resync-delta-extraction).

No puede tener el DTP que HABÍA más de una para el mismo GI. Por lo tanto, debe crear un GI adicional antes de la extracción de delta.

![Creación de SAP BW GI completa](media/load-sap-bw-data/create-sap-bw-ohd-full.png)

Para una carga completa GI, elija opciones diferentes a para la extracción de delta:

- En GI: Establecer el **extracción** opción **registros de insertar y eliminar datos**. En caso contrario, se extraerán datos muchas veces cuando se repita el DTP que HABÍA en una cadena de procesos BW.

- En el DTP que HABÍA: Establecer **modo de extracción** a **completa**. Debe cambiar el DTP que HABÍA creado automáticamente desde **Delta** a **completa** inmediatamente después de crea el GI, como se muestra en esta imagen:

   ![Creación de SAP BW GI configurado para la extracción "Completa" de cuadro de diálogo](media/load-sap-bw-data/create-sap-bw-ohd-full2.png)

- En el conector de concentrador abierto BW de la factoría de datos: Desactivar **última solicitud de exclusión**. En caso contrario, nada se va a extraer.

Normalmente, ejecute manualmente el DTP que HABÍA completo. O bien, puede crear una cadena de procesos para el DTP que HABÍA completo. Suele ser una cadena independiente que es independiente de las cadenas de proceso existente. En cualquier caso, *Asegúrese de que ha finalizado el DTP que HABÍA antes de iniciar la extracción mediante Data Factory copia*. En caso contrario, se copiará solo datos parciales.

### <a name="run-delta-extraction-the-first-time"></a>Ejecuta delta extracción por primera vez

La primera extracción delta es técnicamente un *completa extracción*. De forma predeterminada, el conector de SAP BW Open Hub excluye la última solicitud, cuando copia los datos. Para la primera extracción delta, no se extraen datos mediante la actividad de copia de la factoría de datos hasta que un DTP posteriores genera diferencias de datos en la tabla con un identificador de solicitud independiente. Hay dos maneras de evitar esta situación:

- Desactivar el **última solicitud de exclusión** opción de la primera extracción delta. Asegúrese de que la primera diferencia DTP ha finalizado antes de iniciar la extracción de delta por primera vez.
-  Utilice el procedimiento para resincronizar la extracción de delta, tal como se describe en la sección siguiente.

### <a name="resync-delta-extraction"></a>Resincronizar la extracción de delta

Los siguientes escenarios de cambian los datos en cubos de SAP BW, pero no tiene en cuenta la diferencia DTP:

- Eliminación selectiva de SAP BW (de filas mediante el uso de cualquier condición de filtro)
- Eliminación de la solicitud de SAP BW (de solicitudes defectuosa)

Un destino de concentrador abierto de SAP no es un destino de datos data mart controlados (en todos los paquetes de soporte técnico de SAP BW desde 2015). Por lo tanto, puede eliminar datos de un cubo sin cambiar los datos en el GI. A continuación, debe volver a sincronizar los datos del cubo con Data Factory:

1. Ejecutar una extracción completa en Data Factory (con una completa DTP de SAP).
2. Eliminar todas las filas de la tabla de concentrador abierto para las diferencias DTP.
3. Establecer el estado de las diferencias DTP para **Fetched**.

Una vez hecho esto, todas las extracciones de delta de factoría de datos y delta subsiguiente DTPs funcionan según lo previsto.

Para establecer el estado de las diferencias DTP para **Fetched**, puede usar la siguiente opción para ejecutar manualmente el DTP delta:

    *No Data Transfer; Delta Status in Source: Fetched*

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre la compatibilidad del conector de concentrador abierto de SAP BW:

> [!div class="nextstepaction"]
>[Conector SAP Business Warehouse Open Hub](connector-sap-business-warehouse-open-hub.md)
