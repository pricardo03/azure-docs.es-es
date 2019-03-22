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
ms.date: 03/19/2019
ms.author: jingwang
ms.openlocfilehash: 9458903378576a50db9be92b9377987829e1ba41
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58200164"
---
# <a name="load-data-from-sap-business-warehouse-bw-by-using-azure-data-factory"></a>Cargar datos desde SAP Business Warehouse (BW) mediante Azure Data Factory

En este artículo se muestra un tutorial sobre cómo usar la factoría de datos _cargar datos desde SAP Business Warehouse (BW) a través de concentrador abierto en Azure Data Lake Storage Gen2_. Puede seguir pasos similares para copiar datos a otro [almacenes de datos receptores admitidos](copy-activity-overview.md#supported-data-stores-and-formats). 

> [!TIP]
> Consulte [artículo del conector SAP BW Open Hub](connector-sap-business-warehouse-open-hub.md) acerca de cómo copiar datos desde SAP BW en general, incluida la introducción sobre el flujo de extracción de integración y delta de concentrador abierto de SAP BW.

## <a name="prerequisites"></a>Requisitos previos

- **Azure Data Factory (ADF):** Si no dispone de una factoría de datos, siga el "[crear una factoría de datos](quickstart-create-data-factory-portal.md#create-a-data-factory)" sección para crear uno. 

- **SAP BW abrir Centro de destino (GI) con el tipo de destino como "Tabla de base de datos".** Siga [las configuraciones de destino de concentrador abierto de SAP BW](#sap-bw-open-hub-destination-configurations) sección para crear uno o para confirmar si su GI existente está correctamente configurado para integrarse con ADF.

- **Usuario de SAP BW que se usa debe tener los siguientes permisos:**

  - Autorización para RFC y SAP BW.
  - Permisos para el "**ejecutar**"Actividad del objeto de autorización"**S_SDSAUTH**".

- **[Autohospedaje de Integration Runtime](concepts-integration-runtime.md#self-hosted-integration-runtime) con el conector SAP .NET 3.0 son necesarios**. A continuación se muestran los preparativos detallados que necesitan hacer:

  1. Instale y registre Integration Runtime autohospedado con la versión > = 3.13 (que se describe en el tutorial siguiente). 

  2. Descargue el [64-bit conector SAP .NET 3.0](https://support.sap.com/en/product/connectors/msnet.html) desde el sitio Web de SAP e instalarlo en la máquina de Integration Runtime Autohospedado.  Al instalar, en la ventana de "pasos de configuración opcionales", asegúrese de seleccionar el "**instalar ensamblados en GAC**" opción tal como se muestra en la siguiente imagen.

     ![Configurar el conector SAP para .NET](media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

## <a name="full-copy-from-sap-bw-open-hub"></a>Copia completa de concentrador abierto de SAP BW

En Azure portal, vaya a la factoría de datos -> seleccione **Author & Monitor** para iniciar la UI de ADF en una pestaña independiente. 

1. En la página **Let's get started** (Introducción), seleccione **Copy Data** (Copiar datos) para iniciar la herramienta Copy Data. 

2. En el **propiedades** , especifique un nombre para el **nombre de la tarea** campo y, a continuación, seleccione **siguiente**.

3. En el **almacén de datos de origen** página, haga clic en **+ crear nueva conexión** -> seleccione **concentrador abierto de SAP BW** desde la Galería de conector -> seleccione **continuar**. Puede escribir "SAP" en el cuadro de búsqueda para filtrar los conectores.

4. En el **conexión de concentrador abierto de especificar SAP BW** página, 

   ![Crear servicio de concentrador abierto de SAP BW vinculado](media/load-sap-bw-data/create-sap-bw-open-hub-linked-service.png)

   1. Elija la **conectarse mediante una instancia de Integration Runtime**: haga clic en la lista desplegable para seleccionar una instancia de IR autohospedado existente o cree uno si no tiene configurado aún Integration Runtime Autohospedado. 

      Para crear nuevo, haga clic en **+ nuevo** en la lista desplegable-> seleccione el tipo de **autohospedado** -> especifique un **nombre** y haga clic en **siguiente** -> elija **Configuración rápida** para instalar en el equipo actual o siga el **instalación Manual** los pasos allí.

      Como se mencionó en [requisitos previos](#prerequisites), asegúrese de que también tiene la **conector SAP .NET 3.0** instalado en el mismo equipo donde se ejecuta Integration Runtime Autohospedado.

   2. Especifique el SAP BW **nombre del servidor**, **número del sistema**, **Id. de cliente,** **lenguaje** (si es distinta de EN), **denombredeusuario**, y **contraseña**.

   3. Haga clic en **Test connection** (Prueba de conexión) para validar la configuración y, después, seleccione **Finish** (Finalizar).

   4. Podrá ver una nueva conexión creada. Seleccione **Next** (Siguiente).

5. En el **destinos de concentrador abierto seleccione** página, busque los destinos de concentrador abierto disponible en su SAP BW y seleccione la que desea copiar los datos de, a continuación, haga clic en **siguiente**.

   ![Seleccione la tabla de concentrador abierto de SAP BW](media/load-sap-bw-data/select-sap-bw-open-hub-table.png)

6. Especifique el filtro si es necesario. Si el destino de concentrador abierto solo contiene datos de ejecución única de proceso de transferencia de datos (DTP) con el Id. de solicitud única, o está seguro de su DTP ha finalizado y desea copiar todos los datos, desactive la **excluir la última solicitud**. Puede aprender más sobre cómo estas opciones se relacionan con la configuración de SAP BW en [las configuraciones de destino de concentrador abierto de SAP BW](#sap-bw-open-hub-destination-configurations) sección. Haga clic en **validar** volver a comprobar los datos devueltos, a continuación, seleccione **siguiente**.

   ![Configurar filtro de concentrador abierto de SAP BW](media/load-sap-bw-data/configure-sap-bw-open-hub-filter.png)

7. En el **almacén de datos de destino** página, haga clic en **+ crear nueva conexión**y, a continuación, seleccione **Azure Data Lake Storage Gen2**y seleccione **continuar**.

8. En el **conexión especificar Azure Data Lake Storage** página, 

   ![Crear el servicio vinculado de Gen2 de ADLS](media/load-sap-bw-data/create-adls-gen2-linked-service.png)

   1. Seleccione su cuenta de Data Lake Storage Gen2 capaz de la lista desplegable de "Nombre de cuenta de almacenamiento".
   2. Seleccione **Finalizar** para crear la conexión. Luego, seleccione **Siguiente**.

9. En el **elegir el archivo de salida o la carpeta** página, escriba "copyfromopenhub" como nombre de carpeta de salida y seleccione **siguiente**.

   ![Elija la carpeta de salida](media/load-sap-bw-data/choose-output-folder.png)

10. En el **configuración de formato de archivo** página, seleccione **siguiente** para usar la configuración predeterminada.

    ![Especificar el formato de receptor](media/load-sap-bw-data/specify-sink-format.png)

11. En el **configuración** , expanda el **configuración de rendimiento**y establezca **grado de paralelismo copia** como 5 con el fin de cargar desde SAP BW en paralelo. Haga clic en **Next**.

    ![Configurar opciones de copia](media/load-sap-bw-data/configure-copy-settings.png)

12. En el **resumen** página, revise la configuración y seleccione **siguiente**.

13. En el **implementación** página, seleccione **Monitor** para supervisar la canalización.

    ![Página Deployment (Implementación)](media/load-sap-bw-data/deployment.png)

14. Observe que la pestaña **Monitor** (Supervisión) de la izquierda se selecciona automáticamente. La columna **Actions** (Acciones) incluye los vínculos para ver los detalles de la ejecución de actividad y volver a ejecutar la canalización:

    ![Supervisión de la canalización](media/load-sap-bw-data/pipeline-monitoring.png)

15. Para ver las ejecuciones de actividad asociadas con la ejecución de canalización, seleccione el vínculo **View Activity Runs** (Ver ejecuciones de actividad) en la columna **Actions** (Acciones). Como solo hay una actividad (actividad de copia) en la canalización, solo verá una entrada. Para volver a la vista de ejecuciones de canalización, seleccione el vínculo **Pipelines** (Canalizaciones) de la parte superior. Seleccione **Refresh** (Actualizar) para actualizar la lista.

    ![Supervisión de la actividad](media/load-sap-bw-data/activity-monitoring.png)

16. Para supervisar los detalles de la ejecución de cada actividad de copia, seleccione el vínculo con un icono de gafas **Details** (Detalles) en **Actions** (Acciones) en la vista de supervisión de la actividad. Puede supervisar detalles como el volumen de datos copiados desde el origen al receptor, el rendimiento de los datos, los pasos de ejecución con su duración correspondiente y las configuraciones que se utilizan:

    ![Detalles de supervisión de actividad](media/load-sap-bw-data/activity-monitoring-details.png)

17. Revise el **Id. de solicitud máximo** que se copia. Vuelva a la vista de supervisión de actividades, haga clic en el **salida** en **acciones**.

    ![Salida de la actividad](media/load-sap-bw-data/activity-output.png)

    ![Detalles de salida de actividad](media/load-sap-bw-data/activity-output-details.png)

## <a name="incremental-copy-from-sap-bw-open-hub"></a>Copia incremental de concentrador abierto de SAP BW

> [!TIP]
>
> Hacer referencia a [flujo de extracción de concentrador abierto de SAP BW conector delta](connector-sap-business-warehouse-open-hub.md#delta-extraction-flow) para obtener más información sobre cómo funciona el conector de concentrador abierto de ADF SAP BW para copiar los datos incrementales desde SAP BW y lea este artículo desde el principio para comprender los conceptos básicos de relacionados con el conector configuraciones.

Ahora, vamos a seguir configurar la copia incremental de concentrador abierto de SAP BW. 

La copia incremental está usando el mecanismo de límite máximo en función de **Id. de solicitud** generado automáticamente en el destino de concentrador abierto de SAP BW DTP. En el siguiente diagrama se representa el flujo de trabajo de este enfoque:

![Flujo de trabajo de copia incremental](media/load-sap-bw-data/incremental-copy-workflow.png)

En la UI de ADF **Comencemos** página, seleccione **creación de una canalización de plantilla** para aprovechar la plantilla integrada. 

1. Buscar "SAP BW" para buscar y seleccionar la plantilla denominada **Incremental copiar desde SAP BW en Azure Data Lake Storage Gen2**. Esta plantilla copia los datos en ADLS Gen2, más adelante puede seguir el flujo similar para copiar a otros tipos de receptor.

2. En la página principal de la plantilla, seleccione o cree las siguientes tres conexiones, haga clic **usar esta plantilla** en la esquina inferior derecha.

   - **Azure Blob**: en este tutorial, usamos el Blob de Azure para almacenar el límite máximo, que es el identificador de solicitud copiado max.
   - **Concentrador de SAP BW abierto**: el origen para copiar datos de. Consulte el tutorial anterior de copia completa de los detalles de configuración.
   - **ADLS Gen2**: su receptor desea copiar los datos. Consulte el tutorial anterior de copia completa de los detalles de configuración.

   ![Copia incremental de la plantilla de SAP BW](media/load-sap-bw-data/incremental-copy-from-sap-bw-template.png)

3. Esta plantilla genera una canalización con tres actividades - **búsquedas, la copia de datos y Web** - y hace que estén encadenada en caso de éxito. Vaya a la canalización **parámetros** pestaña, verá todas las configuraciones que se debe proporcionar.

   ![Copia incremental desde la configuración de SAP BW](media/load-sap-bw-data/incremental-copy-from-sap-bw-pipeline-config.png)

   - **SAPOpenHubDestinationName**: especifique el nombre de tabla de concentrador abierto para copiar datos de.

   - **ADLSGen2SinkPath**: especificar la ruta de acceso de destino Gen2 ADLS para copiar los datos. Si no existe la ruta de acceso, la actividad de copia de ADF creará uno durante la ejecución.

   - **HighWatermarkBlobPath**: especifique la ruta de acceso para almacenar el valor de límite máximo, por ejemplo, `container/path`. 

   - **HighWatermarkBlobName**: especifique el nombre del blob para almacenar el valor de límite máximo, por ejemplo, `requestIdCache.txt`. En el almacenamiento de blobs, en la ruta de acceso correspondiente de HighWatermarkBlobPath + HighWatermarkBlobName, por ejemplo, "*container/path/requestIdCache.txt*", cree un blob con contenido 0. 

      ![Contenido del blob](media/load-sap-bw-data/blob.png)

   - **LogicAppURL**: en esta plantilla, se usa la actividad Web para llamar a Logic Apps para establecer el valor de límite máximo de Blob storage. Como alternativa, también puede usar la base de datos SQL para almacenarlos y usar la actividad de procedimiento almacenado para actualizar el valor. 

      En este caso, deberá crear en primer lugar una aplicación lógica como los siguientes valores, a continuación, copie el **URL de HTTP POST** a este campo. 

      ![Configuración de la aplicación lógica](media/load-sap-bw-data/logic-app-config.png)

      1. Vaya a Azure portal -> nuevo un **Logic Apps** servicio -> haga clic en **+ aplicación lógica en blanco** para ir a **Diseñador de Logic Apps**.

      2. Crear un desencadenador de **cuando recibe una solicitud HTTP es**. Especifique el cuerpo de solicitud HTTP como sigue:

         ```json
         {
            "properties": {
               "sapOpenHubMaxRequestId": {
                  "type": "string"
               },
               "type": "object"
            }
         }
         ```

      3. Agregar una acción de **Create blob**. Para "Ruta de acceso de carpeta" y "Nombre de Blob", use el mismo valor configurado en el anterior HighWatermarkBlobPath y HighWatermarkBlobName.

      4. Haga clic en **guardar**y, a continuación, copie el valor de **URL de HTTP POST** para usar en la canalización de ADF.

4. Después de proporcionar todos los valores para parámetros de la canalización ADF, haga clic en **depurar** -> **finalizar** para invocar una ejecución para validar la configuración. O bien, puede seleccionar **publicar todo** para publicar todos los cambios, a continuación, haga clic en **desencadenador** para realizar una ejecución.

## <a name="sap-bw-open-hub-destination-configurations"></a>Configuraciones de destino de concentrador abierto de SAP BW

Esta sección presenta la configuración necesaria en el lado de SAP BW para poder usar el conector de concentrador abierto de SAP BW en ADF para copiar los datos.

### <a name="configure-delta-extraction-in-sap-bw"></a>Configuración de extracción delta en SAP BW

Si es necesario que tanto las copias del historial y copia incremental o sólo copia incremental, configurar extracción delta en SAP BW.

1. Crear el destino de concentrador abierto (GI)

   Puede crear el GI en SAP transacción RSA1, que crea automáticamente la transformación necesaria y el proceso de transferencia de datos (DTP). Use la configuración siguiente:

   - Tipo de objeto puede ser cualquiera. Aquí usamos InfoCube como ejemplo.
   - **Tipo de destino:** *Tabla de base de datos*
   - **Clave de la tabla:** *Técnica clave*
   - **Extracción:** *Mantener los datos y registros de insertar en tabla*

   ![Creación de extracción de SAP BW GI delta](media/load-sap-bw-data/create-sap-bw-ohd-delta.png)

   ![create-sap-bw-ohd-delta2](media/load-sap-bw-data/create-sap-bw-ohd-delta2.png)

   Puede aumentar el número de paralelo que se ejecutan los procesos de trabajo SAP para el DTP que HABÍA:

   ![create-sap-bw-ohd-delta3](media/load-sap-bw-data/create-sap-bw-ohd-delta3.png)

2. Programar el DTP que HABÍA en cadenas de procesos

   Un DTP diferencial para un cubo solo funciona cuando la necesitan filas no se han comprimido aún. Por lo tanto, debe asegurarse de que la compresión de cubo BW no se está ejecutando el DTP que HABÍA antes de a la tabla de concentrador abierto. La manera más fácil para esto es integrar este DTP en sus cadenas de procesos existentes. En el ejemplo siguiente, se inserta el DTP que HABÍA (para el GI) en la cadena de procesos entre el paso de ajuste (de acumulación agregado) y contraer (con compresión de cubo).

   ![create-sap-bw-process-chain](media/load-sap-bw-data/create-sap-bw-process-chain.png)

### <a name="configure-full-extraction-in-sap-bw"></a>Configuración de extracción completa en SAP BW

Además de la extracción de delta, puede tener una extracción de la misma InfoProvider completa. Normalmente, se aplica si desea que se copia sin necesidad de incremental completa o desea [volver a sincronizar la extracción de delta](#re-sync-delta-extraction).

No debe tener el DTP que HABÍA más de uno para el mismo GI. Por lo tanto, deberá crear una extracción GI, a continuación, delta adicional.

![create-sap-bw-ohd-full](media/load-sap-bw-data/create-sap-bw-ohd-full.png)

Para una carga completa GI, elija opciones diferentes a la extracción de delta:

- En GI: establecer la opción de "Extracción" como "*registros de insertar y eliminar datos*". En caso contrario, podrían extraer datos varias veces al repetir el DTP que HABÍA en una cadena de procesos BW.

- En DTP: establecer "Modo de extracción" como "*completa*". Debe cambiar el DTP que HABÍA creado automáticamente desde Delta en Full después de que se ha creado el GI:

   ![create-sap-bw-ohd-full2](media/load-sap-bw-data/create-sap-bw-ohd-full2.png)

- En el conector de concentrador abierto de ADF SAP BW: desactivar la opción "*última solicitud de exclusión*". En caso contrario, no hay nada que se extraerán. 

Normalmente, ejecutar manualmente el DTP que HABÍA completa. O también podría crear una cadena de procesos para el DTP que HABÍA completa: normalmente sería una cadena de procesos independiente independiente de las cadenas de proceso existente. En cualquier caso, debe **Asegúrese de que ha finalizado el DTP que HABÍA antes de iniciar la extracción mediante la copia de ADF**, de lo contrario, se copiarán los datos parciales.

### <a name="run-delta-extraction-the-first-time"></a>Ejecuta delta extracción por primera vez

La primera extracción Delta es técnicamente un **extracción completa**. Tenga en cuenta por el conector de concentrador abierto de ADF SAP BW predeterminado excluye la última solicitud cuando se copian los datos. En el caso de extracción delta por primera vez, en la actividad de copia ADF, no se extraerá ningún dato hasta que hay posteriores DTP genera diferencias de datos en la tabla con Id. de solicitud independiente. Aunque hay dos maneras posibles de evitar esta situación:

1. Desactive la opción "Excluir de última solicitud" para el primer Delta extracción de este caso que debe asegurarse de que ha terminado la primera DTP Delta antes de iniciar la extracción de Delta de la primera vez
2. Utilice el procedimiento para volver a sincronizar la extracción de Delta tal como se describe a continuación.

### <a name="re-sync-delta-extraction"></a>Volver a sincronizar la extracción de delta

Hay algunos escenarios que cambian los datos de SAP BW cubos pero no tiene en cuenta el DTP que HABÍA Delta:

- Eliminación selectiva de SAP BW (de las filas usando cualquier condición de filtro)
- Eliminación de solicitud de SAP BW (de solicitud errónea)

Un destino de concentrador abierto de SAP no es un destino de datos data mart controlados (en todos los SAP BW compatibilidad con paquetes desde el año 2015). Por lo tanto, es posible eliminar los datos de un cubo sin cambiar los datos en el GI. En este caso, debe volver a sincronizar los datos del cubo con los datos de ADF realizando los pasos siguientes:

1. Ejecuta una extracción completa en ADF (mediante el uso de una completa DTP en SAP)
2. Eliminar todas las filas de la tabla de concentrador abierto para el DTP que HABÍA Delta
3. Establezca el estado de la DTP Delta en recuperadas

Una vez hecho esto, todas las DTPs Delta y extracciones de Delta de ADF se funcionarán bien según lo previsto.

Puede establecer el estado de la DTP Delta en recuperadas ejecutando el DTP que HABÍA Delta manualmente mediante la siguiente opción: "*Ninguna transferencia de datos; Estado de delta de origen: Capturan*".

## <a name="next-steps"></a>Pasos siguientes

Pase el siguiente artículo para obtener información sobre la compatibilidad del conector de concentrador abierto de SAP BW: 

> [!div class="nextstepaction"]
>[Conector SAP Business Warehouse Open Hub](connector-sap-business-warehouse-open-hub.md)
