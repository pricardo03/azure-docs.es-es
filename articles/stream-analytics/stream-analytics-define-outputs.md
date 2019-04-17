---
title: Información sobre las salidas desde Azure Stream Analytics
description: En este artículo se describen las opciones de salida de datos disponibles en Azure Stream Analytics, incluido Power BI para los resultados del análisis.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 3/25/2019
ms.custom: seodec18
ms.openlocfilehash: 03871c3f3627e85cc2af2f05a5fba38bd8069a15
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59609496"
---
# <a name="understand-outputs-from-azure-stream-analytics"></a>Información sobre las salidas desde Azure Stream Analytics
En este artículo se describe los tipos de salidas disponibles para un trabajo de Azure Stream Analytics. Las salidas le permiten almacenar y guardar los resultados de los trabajos de Stream Analytics. Mediante el uso de los datos de salida, puede realizar más análisis de negocios y el almacenamiento de datos de los datos.

Al diseñar la consulta de Stream Analytics, hacer referencia al nombre de la salida mediante el uso de la [cláusula INTO](https://msdn.microsoft.com/azure/stream-analytics/reference/into-azure-stream-analytics). Puede usar una única salida por trabajo, o varias salidas por trabajo de streaming (si las necesita) proporcionando varias cláusulas INTO en la consulta.

Para crear, editar y probar el trabajo de Stream Analytics genera, puede usar el [portal Azure](stream-analytics-quick-create-portal.md#configure-job-output), [Azure PowerShell](stream-analytics-quick-create-powershell.md#configure-output-to-the-job), [.NET API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.ioutputsoperations?view=azure-dotnet), [API de REST](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-output), y [Visual Studio](stream-analytics-quick-create-vs.md).

Algunos tipos de salida admiten [particiones](#partitioning). [Tamaños de lote de salida](#output-batch-size) varían para optimizar el rendimiento.


## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Stream Analytics admite el [Almacén de Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/). Azure Data Lake Store es un repositorio de hiperescala de toda la empresa para cargas de trabajo de análisis de macrodatos. Puede usar Data Lake Store para almacenar los datos de cualquier tamaño, tipo y velocidad de ingesta para realizar análisis exploratorios y operativos. Stream Analytics debe estar autorizada para acceder a Data Lake Store.

La salida de Azure Data Lake Store desde Stream Analytics no está disponible actualmente en las regiones de Azure China (21Vianet) y Azure Alemania (T-Systems International).

### <a name="authorize-an-azure-data-lake-store-account"></a>Autorización de una cuenta de Azure Data Lake Store

1. Al seleccionar Data Lake Store como una salida en el portal de Azure, se le pedirá que autorice una conexión a una instancia existente de Data Lake Store.

   ![Autorice una conexión a Data Lake Store](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)

2. Si ya tiene acceso a Data Lake Store, seleccione **autorizar ahora**. Una página Emerge e indica **redirigiendo a la autorización**. Después de autorización se realiza correctamente, se le presentará la página que le permite configurar la salida de Data Lake Store.

3. Una vez que la cuenta de Data Lake Store autenticado, puede configurar las propiedades de la salida de Data Lake Store.

   ![Definición de Data Lake Store como salida de Stream Analytics](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)

En la tabla siguiente se enumera los nombres de propiedad y sus descripciones para configurar la salida de Data Lake Store.   

| Nombre de propiedad | DESCRIPCIÓN |
| --- | --- |
| Alias de salida | Un nombre descriptivo usado en las consultas para dirigir la salida de la consulta a Data Lake Store. |
| Nombre de cuenta | El nombre de la cuenta de Data Lake Store donde está enviando la salida. Se le presentará una lista desplegable de cuentas de Data Lake Store que están disponibles en su suscripción. |
| Patrón del prefijo de la ruta de acceso | La ruta de acceso de archivo que se usa para escribir los archivos en la cuenta de Data Lake Store especificada. Puede especificar una o varias instancias de la {date} y {time} variables:<br /><ul><li>Ejemplo 1: carpeta1/registros/{date}/{time}</li><li>Ejemplo 2: carpeta1/registros/{date}</li></ul><br />La marca de tiempo de la estructura de carpetas creada sigue UTC y no en hora local.<br /><br />Si el patrón de ruta de acceso de archivo no contiene una barra diagonal (/), el patrón final de la ruta de acceso de archivo se trata como un prefijo de nombre de archivo. <br /><br />Se crean nuevos archivos en las circunstancias siguientes:<ul><li>Cambio en el esquema de salida</li><li>Reinicio externo o interno de un trabajo</li></ul> |
| Formato de fecha | Opcional. Si el token de fecha se usa en la ruta de acceso de prefijo, puede seleccionar el formato de fecha en el que se organizan los archivos. Ejemplo: AAAA/MM/DD |
|Formato de hora | Opcional. Si el token de hora se usa en la ruta de acceso de prefijo, puede seleccionar el formato de hora en el que se organizan los archivos. Actualmente, el único valor admitido es HH. |
| Formato de serialización de eventos | El formato de serialización para los datos de salida. Se admiten JSON, CSV y Avro.|
| Encoding | Si usa el formato CSV o JSON, debe especificarse una codificación. Por el momento, UTF-8 es el único formato de codificación compatible.|
| Delimitador | Se aplica sólo para la serialización de CSV. Stream Analytics admite un número de delimitadores comunes para la serialización de datos CSV. Los valores admitidos son la coma, punto y coma, espacio, tabulador y barra vertical.|
| Formato | Se aplica sólo para la serialización de JSON. **Separado por líneas** especifica que el resultado tiene el formato haciendo que cada objeto JSON separado por una línea nueva. **Matriz** especifica que la salida se formatea como una matriz de objetos JSON. Esta matriz se cierra cuando el trabajo se detenga o Stream Analytics haya pasado a la siguiente ventana de tiempo. En general, es preferible utilizar JSON separados por líneas, ya que no requiere ningún control especial mientras todavía se escribe para el archivo de salida.|

### <a name="renew-data-lake-store-authorization"></a>Renovación de la autorización de Data Lake Store
Tiene que volver a autenticar la cuenta de Data Lake Store si su contraseña ha cambiado desde que se creó o autenticó por última vez su trabajo. Si no vuelve a autenticar, el trabajo no genera resultados de salida y muestra un error que indica la necesidad de volver a autorizarse en los registros de operaciones. 

Actualmente, el token de autenticación debe actualizarse manualmente cada 90 días para todos los trabajos con salida de Data Lake Store. Puede superar esta limitación por [autentican a través administra las identidades (versión preliminar)](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-managed-identities-adls).

Para renovar la autorización:

1. Seleccione **detener** para detener el trabajo.
1. Vaya a su Store lago de datos de salida y seleccione el **renovar la autorización** vínculo.

   Durante un tiempo breve, indica una página emergente **redirigiendo a la autorización**. Si la autorización sea correcta, la página indica **autorización se ha renovado correctamente** y, a continuación, se cierra automáticamente. 
   
1. Seleccione **guardar** en la parte inferior de la página. A continuación, puede reiniciar el trabajo desde el **última hora de detención** para evitar la pérdida de datos.

![Renovación de la autorización de Data Lake Store en la salida](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)

## <a name="sql-database"></a>SQL Database
Puede usar [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) como salida de datos que son relacionales por naturaleza o de las aplicaciones que dependen del contenido que se hospeda en una base de datos relacional. Los trabajos de Stream Analytics se escriben en una tabla existente en la base de datos SQL. El esquema de tabla debe coincidir exactamente con los campos y sus tipos en la salida del trabajo. También puede especificar [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) opción de salida como una salida a través de la base de datos de SQL. Para obtener información sobre cómo mejorar el rendimiento de escritura, consulte el [Stream Analytics con Azure SQL Database como salida](stream-analytics-sql-output-perf.md) artículo. 

En la tabla siguiente se enumera los nombres de propiedad y su descripción para crear una salida de la base de datos SQL.

| Nombre de propiedad | DESCRIPCIÓN |
| --- | --- |
| Alias de salida |Un nombre descriptivo usado en las consultas para dirigir la salida de la consulta a esta base de datos. |
| Base de datos | El nombre de la base de datos donde está enviando la salida. |
| Nombre de servidor | El nombre del servidor SQL Database. |
| Nombre de usuario | El nombre de usuario que tiene acceso de escritura a la base de datos. Stream Analytics admite solo la autenticación de SQL. |
| Contraseña | La contraseña para conectarse a la base de datos. |
| Tabla | El nombre de la tabla donde se escribe la salida. El nombre de tabla distingue mayúsculas de minúsculas. El esquema de esta tabla debe coincidir exactamente con el número de campos y los tipos que genera la salida del trabajo. |
|Heredación del esquema de partición| Una opción para heredar el esquema de partición del paso de consulta anterior, para habilitar la topología totalmente paralela con múltiples escritores en la tabla. Para obtener más información, vea [Salida de Azure Stream Analytics a Azure SQL Database](stream-analytics-sql-output-perf.md).|
|Número de lotes coincidente| El límite recomendado en el número de registros que se envían con cada masiva Insertar transacción.|

> [!NOTE]
> Actualmente, la oferta de Azure SQL Database se admite para una salida de trabajo de Stream Analytics. No se admite una máquina virtual de Azure que ejecuta SQL Server con una base de datos conectado. Esto está sujeto a cambios en versiones futuras.
>

## <a name="blob-storage"></a>Almacenamiento de blobs
Azure Blob storage ofrece una solución rentable y escalable para almacenar grandes cantidades de datos no estructurados en la nube. Para obtener una introducción en Blob storage y su uso, consulte [cómo usar blobs](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

En la tabla siguiente se enumera los nombres de propiedad y sus descripciones para crear una salida de blob.

| Nombre de propiedad       | DESCRIPCIÓN                                                                      |
| ------------------- | ---------------------------------------------------------------------------------|
| Alias de salida        | Un nombre descriptivo usado en las consultas para dirigir la salida de la consulta a este almacenamiento de blobs. |
| Cuenta de almacenamiento     | El nombre de la cuenta de almacenamiento donde está enviando la salida.               |
| Clave de cuenta de almacenamiento | La clave secreta asociada con la cuenta de almacenamiento.                              |
| Contenedor de almacenamiento   | Una agrupación lógica de los blobs almacenados en Azure Blob service. Cuando carga un blob a Blob service, debe especificar un contenedor para ese blob. |
| Patrón de la ruta de acceso | Opcional. El patrón de ruta de acceso de archivo que se usa para escribir los blobs dentro del contenedor especificado. <br /><br /> En el patrón de ruta de acceso, puede elegir usar una o varias instancias de las variables de fecha y hora para especificar la frecuencia con que se escriben los blobs: <br /> {date}, {time} <br /><br />Puede usar la creación de particiones de blobs personalizada para especificar un nombre {field} personalizado de los datos de eventos para crear particiones de los blobs. El nombre del campo es alfanumérico y puede incluir espacios, guiones y caracteres de subrayado. Entre las restricciones en los campos personalizados se incluyen las siguientes: <ul><li>Los nombres de campo no distingue mayúsculas de minúsculas. Por ejemplo, el servicio no puede distinguir entre la columna "ID" y la columna "id".</li><li>No se permiten los campos anidados. En su lugar, utilice un alias en la consulta del trabajo para "aplanar" el campo.</li><li>No se puede utilizar expresiones como un nombre de campo.</li></ul> <br />Esta característica también permite usar configuraciones del especificador de un formato de fecha y hora personalizado en la ruta de acceso. Los formatos de fecha y hora personalizados se deben especificar de uno en uno, delimitado por la palabra clave {datetime:\<especificador >}. Entradas permitidas para \<especificador > son yyyy, MM, M, dd, d, HH, H, mm, m, ss o s. El {datetime:\<especificador >} palabra clave puede utilizarse varias veces en la ruta de acceso para formar las configuraciones de fecha y hora personalizado. <br /><br />Ejemplos: <ul><li>Ejemplo 1: cluster1/logs/{date}/{time}</li><li>Ejemplo 2: cluster1/logs/{date}</li><li>Ejemplo 3: cluster1/{client_id}/{date}/{time}</li><li>Ejemplo 4: cluster1/{datetime:ss}/{myField}, donde la consulta es: SELECT data.myField AS myField FROM Input;</li><li>Ejemplo 5: cluster1/year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}</ul><br />La marca de tiempo de la estructura de carpetas creada sigue UTC y no en hora local.<br /><br />Nomenclatura de los archivos, utiliza la convención siguiente: <br /><br />{Patrón del prefijo de la ruta de acceso}/schemaHashcode_Guid_Number.extension<br /><br />Archivos de salida de ejemplo:<ul><li>Myoutput/20170901/00/45434_gguid_1.csv</li>  <li>Myoutput/20170901/01/45434_gguid_1.csv</li></ul> <br />Para obtener más información sobre esta característica, consulte [las particiones de salida de blob de Azure Stream Analytics personalizado](stream-analytics-custom-path-patterns-blob-storage-output.md). |
| Formato de fecha | Opcional. Si el token de fecha se usa en la ruta de acceso de prefijo, puede seleccionar el formato de fecha en el que se organizan los archivos. Ejemplo: AAAA/MM/DD |
| Formato de hora | Opcional. Si el token de hora se usa en la ruta de acceso de prefijo, puede seleccionar el formato de hora en el que se organizan los archivos. Actualmente, el único valor admitido es HH. |
| Formato de serialización de eventos | Formato de serialización para los datos de salida. Se admiten JSON, CSV y Avro. |
| Encoding    | Si usa el formato CSV o JSON, debe especificarse una codificación. Por el momento, UTF-8 es el único formato de codificación compatible. |
| Delimitador   | Se aplica sólo para la serialización de CSV. Stream Analytics admite un número de delimitadores comunes para la serialización de datos CSV. Los valores admitidos son la coma, punto y coma, espacio, tabulador y barra vertical. |
| Formato      | Se aplica sólo para la serialización de JSON. **Separado por líneas** especifica que el resultado tiene el formato haciendo que cada objeto JSON separado por una línea nueva. **Matriz** especifica que la salida se formatea como una matriz de objetos JSON. Esta matriz se cierra cuando el trabajo se detenga o Stream Analytics haya pasado a la siguiente ventana de tiempo. En general, es preferible utilizar JSON separados por líneas, ya que no requiere ningún control especial mientras todavía se escribe para el archivo de salida. |

Cuando usa el almacenamiento de blobs como salida, se crea un nuevo archivo en el blob en los casos siguientes:

* Si el archivo excede el número máximo de bloques permitidos (actualmente, 50 000). Es posible que alcance el número máximo permitido de bloques sin alcanzar el tamaño máximo permitido de blobs. Por ejemplo, si la velocidad de salida es alta, puede ver más bytes por bloque y el tamaño de archivo es mayor. Si la velocidad de salida es baja, cada bloque tiene menos datos y el tamaño de archivo es menor.
* Si hay un cambio de esquema en la salida y el formato de salida requiere un esquema fijo (CSV y Avro).
* Si se reinicia un trabajo, ya sea externamente por un usuario que lo detiene e inicia, o internamente para el mantenimiento del sistema o la recuperación tras un error.
* Si la consulta está completamente particionada y un nuevo archivo se crea para cada partición de salida.
* Si el usuario elimina un archivo o un contenedor de la cuenta de almacenamiento.
* Si el resultado es el momento con particiones realizada mediante el patrón del prefijo de ruta de acceso, y se usa un nuevo blob cuando la consulta se pasa a la hora siguiente.
* Si la salida tiene particiones por un campo personalizado, y se crea un nuevo blob por clave si de partición no existe.
* Si la salida tiene particiones por un campo personalizado donde cardinalidad de clave de la partición supera los 8000, y se crea un nuevo blob por clave de partición.

## <a name="event-hubs"></a>Event Hubs
El servicio [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) es un agente de ingesta de eventos de publicación-suscripción altamente escalable. Puede recopilar millones de eventos por segundo. Un uso de un centro de eventos como salida es cuando la salida de un trabajo de Stream Analytics se convierte en la entrada de otro trabajo de streaming.

Necesita unos cuantos parámetros para configurar los flujos de datos desde event hubs como salida.

| Nombre de propiedad | DESCRIPCIÓN |
| --- | --- |
| Alias de salida | Un nombre descriptivo usado en las consultas para dirigir la salida de la consulta a este centro de eventos. |
| Espacio de nombres del centro de eventos |Un contenedor para un conjunto de entidades de mensajería. Cuando crea un nuevo centro de eventos, también se crea un espacio de nombres del centro de eventos. |
| Nombre del centro de eventos | El nombre de la salida del centro de eventos. |
| Nombre de la directiva del centro de eventos | La directiva de acceso compartido, que puede crear en el centro de eventos **configurar** ficha. Cada directiva de acceso compartido tiene un nombre, los permisos establecidos y las claves de acceso. |
| Clave de la directiva de centro de eventos | La clave de acceso compartido que se usa para autenticar el acceso al espacio de nombres del centro de eventos. |
| Columna de clave de partición | Opcional. Una columna que contiene la clave de partición para la salida de event hubs. |
| Formato de serialización de eventos | El formato de serialización para los datos de salida. Se admiten JSON, CSV y Avro. |
| Encoding | Por el momento, UTF-8 es el único formato de codificación compatible para CSV y JSON. |
| Delimitador | Se aplica sólo para la serialización de CSV. Stream Analytics admite un número de delimitadores comunes para la serialización de datos en formato CSV. Los valores admitidos son la coma, punto y coma, espacio, tabulador y barra vertical. |
| Formato | Se aplica sólo para la serialización de JSON. **Separado por líneas** especifica que el resultado tiene el formato haciendo que cada objeto JSON separado por una línea nueva. **Matriz** especifica que la salida se formatea como una matriz de objetos JSON. Esta matriz se cierra cuando el trabajo se detenga o Stream Analytics haya pasado a la siguiente ventana de tiempo. En general, es preferible utilizar JSON separados por líneas, ya que no requiere ningún control especial mientras todavía se escribe para el archivo de salida. |
| Columnas de propiedades | Opcional. Columnas separadas por comas que necesitan estar conectados como propiedades de usuario del mensaje saliente, en lugar de la carga. Obtener más información sobre esta característica está en la sección [propiedades de metadatos personalizados para la salida](#custom-metadata-properties-for-output). |

## <a name="power-bi"></a>Power BI
Puede usar [Power BI](https://powerbi.microsoft.com/) como salida para un trabajo de Stream Analytics proporcionar una experiencia de visualización enriquecida de los resultados del análisis. Puede utilizar esta capacidad con paneles operativos, generación de informes e informes controlados por métrica.

La salida de Power BI desde Stream Analytics no está disponible actualmente en las regiones de Azure China (21Vianet) y Azure Alemania (T-Systems International).

### <a name="authorize-a-power-bi-account"></a>Autorización de una cuenta de Power BI
1. Cuando se selecciona Power BI como salida en Azure portal, se le pedirá que autorice a un usuario de Power BI existente o para crear una nueva cuenta de Power BI.
   
   ![Autorizar a un usuario de Power BI para configurar la salida](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)

2. Cree una cuenta nueva si aún no tiene uno y, a continuación, seleccione **autorizar ahora**. Aparece la página siguiente:
   
   ![Autenticación en Power BI desde la cuenta de Azure](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)

3. Proporcione la cuenta profesional o educativa para autorizar la salida de Power BI. Si aún no está registrado en Power BI, seleccione **Suscríbase ahora**. Se pueden diferir de la cuenta de suscripción de Azure que ya ha iniciado sesión con la cuenta profesional o educativa que usó para Power BI.

### <a name="configure-the-power-bi-output-properties"></a>Configuración de las propiedades de salida de Power BI
Una vez que la cuenta de Power BI autenticado, puede configurar las propiedades para la salida de Power BI. En la tabla siguiente se enumera los nombres de propiedad y sus descripciones para configurar la salida de Power BI.

| Nombre de propiedad | DESCRIPCIÓN |
| --- | --- |
| Alias de salida |Proporcione un nombre descriptivo que se usa en consultas para dirigir la salida de consulta a esta salida de Power BI. |
| Área de trabajo de grupo |Para habilitar el uso compartido de datos con otros usuarios de Power BI, puede seleccionar grupos dentro de su cuenta de Power BI o elegir **mi área de trabajo** si no desea escribir en un grupo. Actualizar un grupo existente requiere renovar la autenticación de Power BI. |
| Nombre del conjunto de datos |Proporcione un nombre de conjunto de datos que desea utilizar la salida de Power BI. |
| Nombre de tabla |Proporcione un nombre de tabla en el conjunto de datos de la salida de Power BI. Actualmente, la salida de Power BI de trabajos de Stream Analytics solo puede tener una tabla en un conjunto de datos. |

Para ver un tutorial de la configuración de una salida de Power BI y un panel, vea el [Azure Stream Analytics y Power BI](stream-analytics-power-bi-dashboard.md) artículo.

> [!NOTE]
> No cree explícitamente el conjunto de datos y la tabla en el panel de Power BI. El conjunto de datos y la tabla se rellenan automáticamente cuando se inicia el trabajo y el trabajo empiece a enviar salida a Power BI. Si la consulta del trabajo no genera ningún resultado, no se crean el conjunto de datos y la tabla. Si Power BI ya cuenta con un conjunto de datos y una tabla con el mismo nombre que el proporcionado en este trabajo de Stream Analytics, se sobrescriben los datos existentes.
>

### <a name="create-a-schema"></a>Creación de un esquema
Azure Stream Analytics crea un esquema de conjunto de datos y tabla de Power BI para el usuario si aún no existen. En todos los demás casos, la tabla se actualiza con los nuevos valores. Actualmente, solo una tabla puede existir dentro de un conjunto de datos. 

Power BI usa la directiva de retención, primero en salir (FIFO). Se recopilan los datos en una tabla hasta que llegue a 200 000 filas.

### <a name="convert-a-data-type-from-stream-analytics-to-power-bi"></a>Convertir a un tipo de datos de Stream Analytics a Power BI
Azure Stream Analytics actualiza el modelo de datos dinámicamente en tiempo de ejecución si cambia el esquema de salida. Se realiza un seguimiento de los cambios de nombre de columna, los cambios de tipo de columna y la adición o eliminación de columnas.

Esta tabla muestran las conversiones de tipos de datos de [tipos de datos de Stream Analytics](https://msdn.microsoft.com/library/azure/dn835065.aspx) a Power BI [tipos de Entity Data Model (EDM)](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/entity-data-model), si no existen un conjunto de datos de Power BI y una tabla.

De Stream Analytics | A Power BI
-----|-----
bigint | Int64
nvarchar(max) | string
Datetime | DateTime
float | Double
Matriz de registro | Cadena de tipo, valor constante "IRecord" o "IArray"

### <a name="update-the-schema"></a>Revisión del esquema
Stream Analytics deduce el esquema de modelo de datos basándose en el primer conjunto de eventos en la salida. Más adelante, si es necesario, se actualiza el esquema de modelo de datos para dar cabida a los eventos de entrada que podrían no caber en el esquema original.

Evitar la `SELECT *` consulta para evitar que la actualización de esquema dinámico en las filas. Además de posibles implicaciones de rendimiento, podría dar como resultado la incertidumbre del tiempo necesario para los resultados. Seleccione los campos exactos que necesitan mostrarse en el panel de Power BI. Además, los valores de datos deben ser compatibles con el tipo de datos elegido.


Anterior o actual | Int64 | string | DateTime | Double
-----------------|-------|--------|----------|-------
Int64 | Int64 | string | string | Double
Double | Double | string | string | Double
string | string | string | string | string 
DateTime | string | string |  DateTime | string


### <a name="renew-power-bi-authorization"></a>Renovación de la autorización de Power BI
Si la contraseña de su cuenta de Power BI cambia después de que se creó o autenticó por última vez su trabajo de Stream Analytics, debe volver a autenticar Stream Analytics. Si Azure Multi-Factor Authentication se configura en el inquilino de Azure Active Directory (Azure AD), también debe renovar la autorización de Power BI cada dos semanas. Un síntoma de este problema es la ausencia de salidas de trabajos y un "error de autenticación de usuario" en los registros de operaciones:

  ![Error de autenticación de usuario de Power BI](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)

Para resolver este problema, detenga su trabajo en ejecución y vaya a la salida de Power BI. Seleccione el vínculo **Renovar autorización** y reinicie el trabajo desde **Hora de la última detención** para evitar la pérdida de datos.

  ![Renovación de la autorización de Power BI para la salida](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)

## <a name="table-storage"></a>Almacenamiento de tablas
[almacenamiento de tablas de Azure](../storage/common/storage-introduction.md) ofrece un tipo de almacenamiento de alta disponibilidad y escalabilidad masiva, de forma que las aplicaciones pueden escalarse automáticamente para ajustarse a la demanda de los usuarios. Table storage es el almacén de claves/atributos NoSQL de Microsoft, que puede utilizar para datos estructurados con menos restricciones en el esquema. El almacenamiento de tablas de Azure puede usarse para almacenar datos con de persistencia y recuperación eficaz.

En la tabla siguiente se enumera los nombres de propiedad y sus descripciones para crear una tabla de salida.

| Nombre de propiedad | DESCRIPCIÓN |
| --- | --- |
| Alias de salida |Un nombre descriptivo usado en las consultas para dirigir la salida de la consulta a este almacenamiento de tablas. |
| Cuenta de almacenamiento |El nombre de la cuenta de almacenamiento donde está enviando la salida. |
| Clave de cuenta de almacenamiento |La clave de acceso asociada con la cuenta de almacenamiento. |
| Nombre de tabla |El nombre de la tabla. Si no existe, se crea la tabla. |
| Clave de partición |El nombre de la columna de salida que contiene la clave de partición. La clave de partición es un identificador único para la partición dentro de una tabla que constituye la primera parte de la clave principal de la entidad. Es un valor de cadena que puede ser de hasta 1 KB de tamaño. |
| Clave de fila |El nombre de la columna de salida que contiene la clave de fila. La clave de fila es un identificador único para una entidad dentro de una partición. Forma la segunda parte de la clave principal de la entidad. La clave de fila es un valor de cadena que puede ser de hasta 1 KB de tamaño. |
| Tamaño de lote |El número de registros para una operación por lotes. El valor predeterminado (100) es suficiente para la mayoría de los trabajos. Consulte la [especificación de operación por lotes de tabla](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) para obtener más información acerca de cómo modificar esta configuración. |

## <a name="service-bus-queues"></a>Colas de Service Bus
[Colas de Service Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) ofrecen una entrega de mensajes FIFO a uno o varios consumidores en competencia. Normalmente, los mensajes se reciban y procesen los receptores en el orden temporal en el que se agregaron a la cola. Cada mensaje se recibe y procesa un solo consumidor de mensaje.

En la tabla siguiente se enumera los nombres de propiedad y sus descripciones para crear una cola de salida.

| Nombre de propiedad | DESCRIPCIÓN |
| --- | --- |
| Alias de salida |Un nombre descriptivo usado en las consultas para dirigir la salida de la consulta a esta cola de Service Bus. |
| Espacio de nombres de Service Bus |Un contenedor para un conjunto de entidades de mensajería. |
| Nombre de cola |El nombre de la cola de Service Bus. |
| Nombre de la directiva de colas |Cuando se crea una cola, también puede crear directivas de acceso compartido en la cola **configurar** ficha. Cada directiva de acceso compartido tiene un nombre, los permisos establecidos y las claves de acceso. |
| Clave de la directiva de colas |La clave de acceso compartido que se usa para autenticar el acceso al espacio de nombres de Service Bus. |
| Formato de serialización de eventos |El formato de serialización para los datos de salida. Se admiten JSON, CSV y Avro. |
| Encoding |Por el momento, UTF-8 es el único formato de codificación compatible para CSV y JSON. |
| Delimitador |Se aplica sólo para la serialización de CSV. Stream Analytics admite un número de delimitadores comunes para la serialización de datos en formato CSV. Los valores admitidos son la coma, punto y coma, espacio, tabulador y barra vertical. |
| Formato |Solo es aplicable a tipo JSON. **Separado por líneas** especifica que el resultado tiene el formato haciendo que cada objeto JSON separado por una línea nueva. **Matriz** especifica que la salida se formatea como una matriz de objetos JSON. |
| Columnas de propiedades | Opcional. Columnas separadas por comas que necesitan estar conectados como propiedades de usuario del mensaje saliente, en lugar de la carga. Obtener más información sobre esta característica está en la sección [propiedades de metadatos personalizados para la salida](#custom-metadata-properties-for-output). |

El número de particiones se [basa en la SKU y el tamaño de Service Bus](../service-bus-messaging/service-bus-partitioning.md). La clave de partición es un valor entero único para cada partición.

## <a name="service-bus-topics"></a>Temas de Service Bus
Colas de Service Bus proporcionan un método de comunicación uno a uno del remitente al receptor. [Temas de Service Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) proporcionan una forma de uno a varios de comunicación.

En la tabla siguiente se enumera los nombres de propiedad y sus descripciones para crear una salida de tema.

| Nombre de propiedad | DESCRIPCIÓN |
| --- | --- |
| Alias de salida |Un nombre descriptivo usado en las consultas para dirigir la salida de la consulta a este tema de Service Bus. |
| Espacio de nombres de Service Bus |Un contenedor para un conjunto de entidades de mensajería. Al crear un nuevo centro de eventos, también se crea un espacio de nombres de Service Bus. |
| Nombre del tema |Los temas son entidades de mensajería, similares a los centros de eventos y las colas. Están diseñados para recopilar flujos de eventos desde dispositivos y servicios. Cuando se crea un tema, también ha proporcionado un nombre específico. Los mensajes enviados a un tema no están disponibles a menos que se crea una suscripción, así que asegúrese hay uno o más suscripciones en el tema. |
| Nombre de la directiva de temas |Cuando se crea un tema, también puede crear directivas de acceso compartido en el tema **configurar** ficha. Cada directiva de acceso compartido tiene un nombre, los permisos establecidos y las claves de acceso. |
| Clave de la directiva de temas |La clave de acceso compartido que se usa para autenticar el acceso al espacio de nombres de Service Bus. |
| Formato de serialización de eventos |El formato de serialización para los datos de salida. Se admiten JSON, CSV y Avro. |
| Encoding |Si usa el formato CSV o JSON, debe especificarse una codificación. Por el momento, UTF-8 es el único formato de codificación compatible. |
| Delimitador |Se aplica sólo para la serialización de CSV. Stream Analytics admite un número de delimitadores comunes para la serialización de datos en formato CSV. Los valores admitidos son la coma, punto y coma, espacio, tabulador y barra vertical. |
| Columnas de propiedades | Opcional. Columnas separadas por comas que necesitan estar conectados como propiedades de usuario del mensaje saliente, en lugar de la carga. Obtener más información sobre esta característica está en la sección [propiedades de metadatos personalizados para la salida](#custom-metadata-properties-for-output). |

El número de particiones se [basa en la SKU y el tamaño de Service Bus](../service-bus-messaging/service-bus-partitioning.md). La clave de partición es un valor entero único para cada partición.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) es un servicio de base de datos distribuida globalmente que ofrece escalado elástico ilimitado en el mundo, consultas enriquecidas y la indexación automática a través de los modelos de datos independiente del esquema. Para obtener información sobre las opciones de la colección de Azure Cosmos DB para Stream Analytics, consulte el [Stream Analytics con Azure Cosmos DB como salida](stream-analytics-documentdb-output.md) artículo.

La salida de Azure Cosmos DB desde Stream Analytics no está disponible actualmente en las regiones de Azure China (21Vianet) y Azure Alemania (T-Systems International).

> [!Note]
> En este momento, Azure Stream Analytics solo admite la conexión a Azure Cosmos DB mediante la API de SQL.
> Aún no se admiten otras API de Azure Cosmos DB. Si apunta Azure Stream Analytics a las cuentas de Azure Cosmos DB creadas con otras API, puede que los datos no se almacenen correctamente.

En la tabla siguiente se describen las propiedades para crear una salida de Azure Cosmos DB.

| Nombre de propiedad | DESCRIPCIÓN |
| --- | --- |
| Alias de salida | Un alias para hacer referencia a esta salida en la consulta de Stream Analytics. |
| Receptor | Azure Cosmos DB. |
| Opción de importación | Elija **seleccionar Cosmos DB desde su suscripción** o **configuración proporcionan Cosmos DB manualmente**.
| Identificador de cuenta | El nombre o el URI del punto de conexión de la cuenta de Azure Cosmos DB. |
| Clave de cuenta | La clave de acceso compartido para la cuenta de Azure Cosmos DB. |
| Base de datos | El nombre de la base de datos de Azure Cosmos DB. |
| Patrón de nombre de colección | El nombre de la colección o el patrón para las colecciones que se usará. <br />Puede construir el formato de nombre de la colección mediante el token {partition} opcional, donde las particiones comienzan desde 0. Dos ejemplos:  <br /><ul><li> _MyCollection_: Debe existir una colección denominada "MyCollection".</li>  <li> _MyCollection{partition}_: En función de la columna de partición.</li></ul> Deben existir las colecciones de columnas de partición: "MyCollection0", "MyCollection1", "MyCollection2", y así sucesivamente. |
| Clave de partición | Opcional. Se necesita solo si usa un token {partition} en el patrón de nombre de la colección.<br /> La clave de partición es el nombre del campo en los eventos de salida que se usa para especificar la clave de partición de salida entre colecciones.<br /> Para la salida de colección sencilla, puede utilizar cualquier columna de salida arbitraria. Un ejemplo es PartitionId. |
| Id. de documento |Opcional. El nombre del campo en los eventos de salida que se usa para especificar la clave principal en qué insert o update se basan las operaciones.

## <a name="azure-functions"></a>Azure Functions
Azure Functions es un servicio de proceso sin servidor que puede usar para ejecutar código a petición sin tener que aprovisionar ni administrar infraestructura explícitamente. Le permite implementar código desencadenado por eventos que ocurren en los servicios de Azure o asociado. Esta capacidad de Azure Functions para responder a los desencadenadores hace una salida natural para Azure Stream Analytics. Este adaptador de salida permite a los usuarios conectar Stream Analytics con funciones de Azure y ejecutar un script o un fragmento de código en respuesta a una variedad de eventos.

La salida de Azure Functions desde Stream Analytics no está disponible actualmente en las regiones de Azure China (21Vianet) y Azure Alemania (T-Systems International).

Azure Stream Analytics invoca a Azure Functions a través de desencadenadores HTTP. El adaptador de salida de Azure Functions está disponible con las propiedades configurables siguientes:

| Nombre de propiedad | DESCRIPCIÓN |
| --- | --- |
| Aplicación de función |El nombre de la aplicación de Azure Functions. |
| Función |El nombre de la función de la aplicación de Azure Functions. |
| Clave |Si desea utilizar una función de Azure de otra suscripción, puede hacerlo mediante la clave para acceder a la función. |
| Tamaño máximo de lote |Una propiedad que le permite establece el tamaño máximo de cada lote de salida que se envía a la función de Azure. La unidad de entrada se muestra en bytes. De forma predeterminada, este valor es 262 144 bytes (256 KB). |
| Número máximo de lotes  |Una propiedad que permite especificar el número máximo de eventos en cada lote que se envía a Azure Functions. El valor predeterminado es 100. |

Cuando Azure Stream Analytics recibe un 413 ("http entidad solicitada demasiado grande") excepción desde una función de Azure, reduce el tamaño de los lotes que envía a Azure Functions. En el código de función de Azure, use esta excepción para asegurarse de que Azure Stream Analytics no envíe lotes demasiado grandes. Además, asegúrese de que los valores de recuento y tamaño de lote máximo utilizados en la función sean coherentes con los valores especificados en el portal de Stream Analytics.

Además, en una situación donde no hay ningún evento en un período de tiempo de inicio, se genera ningún resultado. Como resultado, el **computeResult** no se llama a la función. Este comportamiento es coherente con las funciones integradas de agregado en ventanas.

## <a name="custom-metadata-properties-for-output"></a>Propiedades de metadatos personalizados para la salida 

Puede asociar las columnas de la consulta como propiedades de usuario a los mensajes salientes. Estas columnas no van en la carga. Las propiedades están presentes en el formulario de un diccionario en el mensaje de salida. *Clave* es el nombre de columna y *valor* es el valor de columna en el diccionario de propiedades. Se admiten todos los tipos de datos de Stream Analytics, excepto el registro y la matriz.  

Salidas compatibles son: 
* Cola de Service Bus 
* Tema del bus de servicio 
* Centro de eventos 

En el ejemplo siguiente, agregamos los dos campos `DeviceId` y `DeviceStatus` a los metadatos. 
* Consulta: `select *, DeviceId, DeviceStatus from iotHubInput`
* Configuración de salida: `DeviceId,DeviceStatus`

![Columnas de propiedades](./media/stream-analytics-define-outputs/10-stream-analytics-property-columns.png)

Inspeccionado en el centro de eventos a través de propiedades de mensaje de salida de la siguiente captura de pantalla [Explorador de Service Bus](https://github.com/paolosalvatori/ServiceBusExplorer).

![Propiedades personalizadas del evento](./media/stream-analytics-define-outputs/09-stream-analytics-custom-properties.png)

## <a name="partitioning"></a>Creación de particiones

En la tabla siguiente se resume la asistencia de la partición y el número de redactores de salida para cada tipo de salida:

| Tipo de salida | Compatibilidad con particiones | Clave de partición  | Número de redactores de salida |
| --- | --- | --- | --- |
| Azure Data Lake Store | Sí | Use {date} {time} los tokens y en el patrón del prefijo de ruta de acceso. Elija el formato de fecha como aaaa/MM/DD, DD/MM/AAAA o MM-DD-AAAA. HH se usa para el formato de hora. | Sigue las particiones de entrada para [consultas que se pueden paralelizar totalmente](stream-analytics-scale-jobs.md). |
| Azure SQL Database | Sí | Según la cláusula PARTITION BY en la consulta. | Sigue las particiones de entrada para [consultas que se pueden paralelizar totalmente](stream-analytics-scale-jobs.md). Para obtener más información acerca de lograr escribir mejor rendimiento al cargar datos en Azure SQL Database, consulte [salida de Azure Stream Analytics a Azure SQL Database](stream-analytics-sql-output-perf.md). |
| Azure Blob Storage | Sí | Use {date} y tokens de {time} desde los campos de evento en el patrón de ruta de acceso. Elija el formato de fecha como aaaa/MM/DD, DD/MM/AAAA o MM-DD-AAAA. HH se usa para el formato de hora. La salida de los blobs puede particionarse con un solo atributo de evento personalizado {fieldname} o {datetime:\<especificador>}. | Sigue las particiones de entrada para [consultas que se pueden paralelizar totalmente](stream-analytics-scale-jobs.md). |
| Azure Event Hubs | Sí | Sí | Varía según alineación de particiones.<br /> Cuando la clave de partición para la salida de event hubs está igualmente alineada con el paso de consulta ascendente (anterior), el número de sistemas de escritura es el mismo que el número de particiones de salida de event hubs. Cada sistema de escritura usa el [clase EventHubSender](/dotnet/api/microsoft.servicebus.messaging.eventhubsender?view=azure-dotnet) para enviar eventos a la partición específica. <br /> Cuando la clave de partición para la salida de event hubs no está alineada con el paso de consulta ascendente (anterior), el número de sistemas de escritura es el mismo que el número de particiones en el paso anterior. Cada sistema de escritura usa el [clase SendBatchAsync](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.sendasync?view=azure-dotnet) en **EventHubClient** para enviar eventos a todas las particiones de salida. |
| Power BI | Sin  | None | No aplicable. |
| Almacenamiento de tablas de Azure | Sí | Cualquier columna de resultados.  | Sigue las particiones de entrada para [consultas totalmente paralelizadas](stream-analytics-scale-jobs.md). |
| Tema de Azure Service Bus | Sí | Se elige automáticamente. El número de particiones se basa en [la SKU y el tamaño de Service Bus](../service-bus-messaging/service-bus-partitioning.md). La clave de partición es un valor entero único para cada partición.| Igual que el número de particiones en el tema de salida.  |
| Cola de Azure Service Bus | Sí | Se elige automáticamente. El número de particiones se basa en [la SKU y el tamaño de Service Bus](../service-bus-messaging/service-bus-partitioning.md). La clave de partición es un valor entero único para cada partición.| Igual que el número de particiones en la cola de salida. |
| Azure Cosmos DB | Sí | Usar el token {partition} en el patrón de nombre de la colección. El valor de {partition} se basa en la cláusula PARTITION BY en la consulta. | Sigue las particiones de entrada para [consultas totalmente paralelizadas](stream-analytics-scale-jobs.md). |
| Azure Functions | Sin  | None | No aplicable. |

Si el adaptador de salida no tiene particiones, la falta de datos de una partición de entrada provocará un retraso hasta la cantidad de llegada tardía de tiempo. En tales casos, la salida se combina en un único escritor, lo que podría producir cuellos de botella en la canalización. Para más información acerca de la directiva de llegada tardía, consulte [consideraciones acerca del orden de eventos de Azure Stream Analytics](stream-analytics-out-of-order-and-late-events.md).

## <a name="output-batch-size"></a>Tamaño de lote de salida
Azure Stream Analytics utiliza lotes de tamaño variable para procesar eventos y escribir en las salidas. Normalmente, el motor de Stream Analytics no escribe un mensaje a la vez y utiliza lotes para mejorar la eficacia. Cuando la velocidad de los eventos entrantes y salientes es alta, Stream Analytics utiliza lotes más grandes. Cuando la tasa de salida es baja, utiliza lotes más pequeños para mantener baja la latencia.

La siguiente tabla explica algunas de las consideraciones para el procesamiento por lotes de salida:

| Tipo de salida | Tamaño máximo de mensaje | Optimización de tamaño de lote |
| :--- | :--- | :--- |
| Azure Data Lake Store | Consulte [límites de Data Lake Storage](../azure-subscription-service-limits.md#data-lake-store-limits). | Usar hasta 4 MB por operación de escritura. |
| Azure SQL Database | 10 000 número máximo de filas por inserción masiva única.<br />100 número mínimo de filas por inserción masiva única. <br />Consulte [SQL Azure limita](../sql-database/sql-database-resource-limits.md). |  Inicialmente, todos los lotes es insertado con el tamaño máximo del lote de forma masiva. Puede dividir el lote en la mitad (hasta alcanzar el tamaño mínimo del lote) según los errores con posibilidad de reintento de SQL. |
| Azure Blob Storage | Consulte [límites de Azure Storage](../azure-subscription-service-limits.md#storage-limits). | El tamaño de bloque máximo de blob es 4 MB.<br />El recuento de bock máximo de blob es de 50.000. |
| Azure Event Hubs  | 256 KB por mensaje. <br />Consulte [límites de Event Hubs](../event-hubs/event-hubs-quotas.md). |  Cuando no está alineado a la creación de particiones de entrada/salida, cada evento se empaqueta individualmente en **EventData** y envían en un lote de hasta el tamaño máximo del mensaje (1 MB para la SKU Premium). <br /><br />  Si las particiones de entrada/salida es aligned, varios eventos se empaquetan en un único **EventData** de instancia, hasta alcanzar el tamaño máximo del mensaje y envían.  |
| Power BI | Consulte [limita la API de Rest de Power BI](https://msdn.microsoft.com/library/dn950053.aspx). |
| Almacenamiento de tablas de Azure | Consulte [límites de Azure Storage](../azure-subscription-service-limits.md#storage-limits). | El valor predeterminado es 100 entidades por transacción única. Se puede configurar con un valor menor según sea necesario. |
| Cola de Azure Service Bus   | 256 KB por mensaje.<br /> Consulte [límites de Service Bus](../service-bus-messaging/service-bus-quotas.md). | Usar un evento único por mensaje. |
| Tema de Azure Service Bus | 256 KB por mensaje.<br /> Consulte [límites de Service Bus](../service-bus-messaging/service-bus-quotas.md). | Usar un evento único por mensaje. |
| Azure Cosmos DB   | Consulte [límites de Azure Cosmos DB](../azure-subscription-service-limits.md#azure-cosmos-db-limits). | Tamaño del lote y escribir la frecuencia se ajustan dinámicamente en función de las respuestas de Azure Cosmos DB. <br /> No hay ningún limitaciones predeterminadas Stream Analytics. |
| Azure Functions   | | El tamaño del lote predeterminado es 262 144 bytes (256 KB). <br /> El recuento de eventos predeterminado por lote es 100. <br /> El tamaño del lote es configurable y puede aumentar o disminuir en las [opciones de salida](#azure-functions) de Stream Analytics.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> 
> [Inicio rápido: Creación de un trabajo de Stream Analytics mediante Azure Portal](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
