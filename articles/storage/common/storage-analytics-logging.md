---
title: Registro de análisis de almacenamiento de Azure
description: Obtenga información sobre cómo registrar los detalles acerca de las solicitudes realizadas en el almacenamiento de Azure.
services: storage
author: fhryo-msft
ms.service: storage
ms.topic: article
ms.date: 03/11/2019
ms.author: fryu
ms.subservice: common
ms.openlocfilehash: 09a5a6d823240b724e6ec88de38df068a58982d9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61483509"
---
# <a name="azure-storage-analytics-logging"></a>Registro de Azure Storage analytics

Storage Analytics registra información detallada sobre las solicitudes correctas y erróneas realizadas a un servicio de almacenamiento. Esta información se puede utilizar para supervisar solicitudes concretas y para diagnosticar problemas con un servicio de almacenamiento. Las solicitudes se registran en función de la mejor opción.

 Registro de Storage Analytics no está habilitado de forma predeterminada para la cuenta de almacenamiento. Puede habilitarlo en [Azure Portal](https://portal.azure.com/); para más información, consulte [Supervisión de una cuenta de almacenamiento en Azure Portal](/azure/storage/storage-monitor-storage-account). También puede habilitar Storage Analytics mediante programación a través de la API de REST o la biblioteca de cliente. Use la [Get Blob Service Properties](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API), [Get Queue Service Properties](https://docs.microsoft.com/rest/api/storageservices/Get-Queue-Service-Properties), y [Get Table Service Properties](https://docs.microsoft.com/rest/api/storageservices/Get-Table-Service-Properties) operaciones para habilitar Storage Analytics para cada servicio.

 Las entradas del registro se crean solo si se presentan solicitudes al punto de conexión de servicio. Por ejemplo, si una cuenta de almacenamiento tiene actividad en su punto de conexión de Blob pero no en sus puntos de conexión de tabla o cola, se creará solo los registros que pertenecen a Blob service.

> [!NOTE]
>  Registro de Storage Analytics actualmente solo está disponible para los servicios Blob, Queue y Table. Sin embargo, no se admite la cuenta de almacenamiento premium.

## <a name="requests-logged-in-logging"></a>Solicitudes registradas en el registro
### <a name="logging-authenticated-requests"></a>Registrar solicitudes de autenticación

 Se registran los siguientes tipos de solicitudes autenticadas:

- Solicitudes correctas
- Solicitudes erróneas, incluidos errores de tiempo de espera, de limitación, de red, de autorización y de otro tipo
- Solicitudes mediante una firma de acceso compartido (SAS) o OAuth, incluidas las solicitudes correctas y con errores
- Solicitudes de datos de análisis

  Las solicitudes realizadas por el propio Storage Analytics, como la creación o eliminación del registro, no se registran. Puede encontrar una lista completa de los datos registrados en los temas [Operaciones y mensajes de estado registrados por Storage Analytics](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) y [Formato del registro de Storage Analytics](/rest/api/storageservices/storage-analytics-log-format).

### <a name="logging-anonymous-requests"></a>Registrar solicitudes anónimas

 Se registran los siguientes tipos de solicitudes anónimas:

- Solicitudes correctas
- Errores del servidor
- Errores de tiempo de espera de cliente y servidor
- Solicitudes GET erróneas con el código de error 304 (No modificado)

  El resto de solicitudes anónimas erróneas no se registran. Puede encontrar una lista completa de los datos registrados en los temas [Operaciones y mensajes de estado registrados por Storage Analytics](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) y [Formato del registro de Storage Analytics](/rest/api/storageservices/storage-analytics-log-format).

## <a name="how-logs-are-stored"></a>Cómo se almacenan los registros

Todos los registros se almacenan en blobs en bloques en un contenedor denominado `$logs`, que se crea automáticamente cuando se habilita Storage Analytics para una cuenta de almacenamiento. El `$logs` contenedor se encuentra en el espacio de nombres de blob de la cuenta de almacenamiento, por ejemplo: `http://<accountname>.blob.core.windows.net/$logs`. Este contenedor no se puede eliminar una vez habilitado Storage Analytics, aunque su contenido sí se puede eliminar. Si usa la herramienta de exploración de almacenamiento para navegar directamente al contenedor, verá todos los blobs que contienen los datos de registro.

> [!NOTE]
>  El `$logs` contenedor no se muestra cuando se realizan una operación de lista de contenedores, por ejemplo, la operación de lista de contenedores. Se debe tener acceso a él directamente. Por ejemplo, puede usar la operación de enumeración de Blobs para tener acceso a los blobs en el `$logs` contenedor.

A medida que se registran las solicitudes, Storage Analytics cargará los resultados intermedios como bloques. Periódicamente, Storage Analytics confirmará estos bloques para que estén disponibles como blobs. Puede tardar hasta una hora para los datos de registro a aparezcan en los blobs en el **$logs** contenedor porque la frecuencia a la que el servicio de almacenamiento vacía los escritores de registro. Es posible que haya entradas duplicadas en los registros creados durante la misma hora. Puede determinar si un registro es un duplicado comprobando el número **RequestId** y **Operation**.

Si tiene un gran volumen de datos de registro con varios archivos para cada hora, puede usar los metadatos del blob para determinar los datos que contiene el registro mediante el examen de los campos de metadatos del blob. Esto también es útil porque puede a veces haber un retraso mientras se escriben datos en los archivos de registro: los metadatos de blob proporciona una indicación más precisa del contenido del blob que el nombre del blob.

La mayoría de las herramientas de exploración almacenamiento le permiten ver los metadatos de blobs; También puede leer esta información mediante PowerShell o mediante programación. El siguiente fragmento de PowerShell es un ejemplo de filtrado de la lista de blobs de registro por nombre para especificar una hora y por metadatos para identificar solo los registros que contienen **escribir** operaciones.  

 ```powershell
 Get-AzureStorageBlob -Container '$logs' |  
 Where-Object {  
     $_.Name -match 'table/2014/05/21/05' -and   
     $_.ICloudBlob.Metadata.LogType -match 'write'  
 } |  
 ForEach-Object {  
     "{0}  {1}  {2}  {3}" –f $_.Name,   
     $_.ICloudBlob.Metadata.StartTime,   
     $_.ICloudBlob.Metadata.EndTime,   
     $_.ICloudBlob.Metadata.LogType  
 }  
 ```  

Para obtener información sobre cómo mostrar los blobs mediante programación, vea [enumerar recursos de Blob](http://msdn.microsoft.com/library/azure/hh452233.aspx) y [configuración y recuperar propiedades y metadatos para los recursos de Blob](http://msdn.microsoft.com/library/azure/dd179404.aspx).  

### <a name="log-naming-conventions"></a>Convenciones de nomenclatura de los registros

 Cada registro se escribirá en el formato siguiente:

 `<service-name>/YYYY/MM/DD/hhmm/<counter>.log`

 La tabla siguiente describe cada atributo en el nombre del registro:

|Atributo|DESCRIPCIÓN|
|---------------|-----------------|
|`<service-name>`|El nombre del servicio de almacenamiento. Por ejemplo: `blob`, `table`, o `queue`|
|`YYYY`|El año de cuatro dígitos para el registro. Por ejemplo: `2011`|
|`MM`|El mes de dos dígitos para el registro. Por ejemplo: `07`|
|`DD`|El día de dos dígitos para el registro. Por ejemplo: `31`|
|`hh`|La hora de dos dígitos que indica la hora inicial para los registros, en UTC de 24 horas formato. Por ejemplo: `18`|
|`mm`|El número de dos dígitos que indica el minuto inicial para los registros. **Nota:**  Este valor no es compatible con la versión actual del análisis de almacenamiento y su valor siempre será `00`.|
|`<counter>`|Un contador basado en cero con seis dígitos que indica el número de blobs del registro generados para el servicio de almacenamiento en un período de tiempo de una hora. Este contador se inicia en `000000`. Por ejemplo: `000001`|

 Este es un nombre de registro de ejemplo completo que combina los ejemplos anteriores:

 `blob/2011/07/31/1800/000001.log`

 Este es un ejemplo de URI que puede utilizarse para tener acceso al registro anterior:

 `https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log`

 Cuando se registra una solicitud de almacenamiento, el nombre del registro resultante depende de la hora en la que se completó la operación solicitada. Por ejemplo, si una solicitud GetBlob se completó a las 6:30 P.M. del 31/7/2011, el registro se escribirá con el prefijo siguiente: `blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Metadatos del registro

 Todos los blobs del registro se almacenan con metadatos que se pueden utilizar para identificar los datos de registro que contiene el blob. La tabla siguiente describe cada atributo de metadatos:

|Atributo|DESCRIPCIÓN|
|---------------|-----------------|
|`LogType`|Describe si el registro contiene información relacionada con las operaciones de lectura, escritura o eliminación. Este valor puede incluir un tipo o una combinación de los tres, separados por comas.<br /><br /> Ejemplo 1: `write`<br /><br /> Ejemplo 2: `read,write`<br /><br /> Ejemplo 3: `read,write,delete`|
|`StartTime`|La primera hora de una entrada en el registro, en forma de `YYYY-MM-DDThh:mm:ssZ`. Por ejemplo: `2011-07-31T18:21:46Z`|
|`EndTime`|La última hora de una entrada en el registro, en forma de `YYYY-MM-DDThh:mm:ssZ`. Por ejemplo: `2011-07-31T18:22:09Z`|
|`LogVersion`|La versión del formato del registro.|

 En la lista siguiente muestra metadatos completos de ejemplo utilizando los ejemplos anteriores:

-   `LogType=write`
-   `StartTime=2011-07-31T18:21:46Z`
-   `EndTime=2011-07-31T18:22:09Z`
-   `LogVersion=1.0`

## <a name="enable-storage-logging"></a>Habilitar el registro de almacenamiento

Puede habilitar el registro de almacenamiento con Azure portal, PowerShell y SDK de Storage.

### <a name="enable-storage-logging-using-the-azure-portal"></a>Habilitar el registro de almacenamiento mediante Azure portal  

En el portal de Azure, use el **configuración de diagnóstico (clásica)** hoja para controlar el registro de Storage, accesible desde el **supervisión (clásica)** sección de una cuenta de almacenamiento **hoja de menú** .

Puede especificar los servicios de almacenamiento que desea iniciar sesión y el período de retención (en días) para los datos registrados.  

### <a name="enable-storage-logging-using-powershell"></a>Habilitar el registro de almacenamiento mediante PowerShell  

 Puede usar PowerShell en el equipo local para configurar el registro de almacenamiento en la cuenta de almacenamiento mediante el cmdlet de PowerShell de Azure **Get-AzureStorageServiceLoggingProperty** para recuperar la configuración actual y el cmdlet  **Set-AzureStorageServiceLoggingProperty** para cambiar la configuración actual.  

 Los cmdlets que controlan el registro de almacenamiento usan un **LoggingOperations** parámetro que es una cadena que contiene una lista separada por comas de los tipos de solicitud para iniciar sesión. Los tres tipos de solicitud posibles son **leer**, **escribir**, y **eliminar**. Para desactivar el registro, use el valor **ninguno** para el **LoggingOperations** parámetro.  

 El siguiente comando activa el registro para leer, escribir y eliminar las solicitudes en el servicio de cola en su cuenta de almacenamiento predeterminada con la retención establecida en cinco días:  

```powershell
Set-AzureStorageServiceLoggingProperty -ServiceType Queue -LoggingOperations read,write,delete -RetentionDays 5  
```  

 El siguiente comando desactiva el registro para el servicio de tabla en la cuenta de almacenamiento predeterminada:  

```powershell
Set-AzureStorageServiceLoggingProperty -ServiceType Table -LoggingOperations none  
```  

 Para más información sobre cómo configurar los cmdlets de Azure PowerShell para que funcionen con su suscripción de Azure y cómo seleccionar la cuenta de almacenamiento predeterminada que quiere usar, vea: [Cómo instalar y configurar Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/).  

### <a name="enable-storage-logging-programmatically"></a>Habilitar el almacenamiento de registros mediante programación  

 Además de usar el portal de Azure o los cmdlets de PowerShell de Azure para controlar el registro de almacenamiento, también puede usar una de las API de almacenamiento de Azure. Por ejemplo, si está utilizando un lenguaje .NET puede usar la biblioteca de cliente de almacenamiento.  

 Las clases **CloudBlobClient**, **CloudQueueClient**, y **CloudTableClient** tienen métodos como **SetServiceProperties** y **SetServicePropertiesAsync** que toman un **ServiceProperties** objeto como parámetro. Puede usar el **ServiceProperties** objeto para configurar el registro de almacenamiento. Por ejemplo, la siguiente C# fragmento de código muestra cómo cambiar el período de retención para el registro de cola y lo que se registra:  

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);  
var queueClient = storageAccount.CreateCloudQueueClient();  
var serviceProperties = queueClient.GetServiceProperties();  

serviceProperties.Logging.LoggingOperations = LoggingOperations.All;  
serviceProperties.Logging.RetentionDays = 2;  

queueClient.SetServiceProperties(serviceProperties);  
```  

 Para obtener más información sobre el uso de un lenguaje .NET para configurar el registro de almacenamiento, consulte [referencia de biblioteca de cliente de almacenamiento](https://msdn.microsoft.com/library/azure/dn261237.aspx).  

 Para obtener información general acerca de cómo configurar el registro de almacenamiento con la API de REST, consulte [habilitar y configurar el análisis de almacenamiento](https://msdn.microsoft.com/library/azure/hh360996.aspx).  

## <a name="download-storage-logging-log-data"></a>Descargar el registro de los datos de registro de Storage

 Para ver y analizar los datos de registro, debe descargar los blobs que contienen los datos de registro que está interesado en una máquina local. Muchas herramientas de exploración de almacenamiento permiten descargar blobs de la cuenta de almacenamiento; También puede usar el equipo de Azure Storage proporcionado la herramienta de línea de comandos de copia de Azure (**AzCopy**) para descargar los datos de registro.  

 Para asegurarse de que descargue los datos del registro que está interesado y evitar la descarga de más de una vez los mismos datos de registro:  

-   Utilice la fecha y la convención de nomenclatura de tiempo para blobs que contienen datos de registro para realizar un seguimiento de los blobs que ya ha descargado para que análisis y evitar volver a descargar los mismos datos de más de una vez.  

-   Use los metadatos de los blobs que contienen datos de registro para identificar el período específico para el que el blob contiene datos de registro para identificar el blob exacto que necesita para descargar.  

> [!NOTE]
>  AzCopy forma parte del SDK de Azure, pero siempre puede descargar la versión más reciente de [ https://aka.ms/AzCopy ](https://aka.ms/AzCopy). De forma predeterminada, AzCopy se instala en la carpeta **C:\Program Files (x86) sdks\windows Azure\AzCopy**, y debe añadir esta carpeta a la ruta de acceso antes de intentar ejecutar la herramienta en un símbolo del sistema o ventana de PowerShell.  

 El ejemplo siguiente muestra cómo puede descargar los datos de registro para el servicio de cola para las horas a partir de 09 AM, 10 AM y 11 AM de 20 de mayo de 2014. El **/S** parámetro hace que AzCopy para crear una estructura de carpetas local en función de las fechas y horas en los nombres de archivo de registro; el **/V** parámetro hace que AzCopy generar la salida detallada; el **/Y** parámetro hace que AzCopy sobrescriba los archivos locales. Reemplace **< yourstorageaccount\>**  con el nombre de la cuenta de almacenamiento y reemplace **< yourstoragekey\>**  con la clave de cuenta de almacenamiento.  

```
AzCopy 'http://<yourstorageaccount>.blob.core.windows.net/$logs/queue'  'C:\Logs\Storage' '2014/05/20/09' '2014/05/20/10' '2014/05/20/11' /sourceKey:<yourstoragekey> /S /V /Y  
```  

 AzCopy también tiene algún parámetro útil que controlan cómo establece la hora última modificación de los archivos que descarga y, si intentará descargar los archivos que son más antiguos o más recientes que los archivos que ya existen en el equipo local. También puede ejecutar en modo reiniciable. ¿Para obtener más información, vea la ayuda ejecutando el **AzCopy /?** comando.  

 Para obtener un ejemplo de cómo descargar los datos de registro mediante programación, vea la entrada de blog [registro de Windows Azure Storage: Usar registros para seguir las solicitudes de almacenamiento](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/08/03/windows-azure-storage-logging-using-logs-to-track-storage-requests.aspx) y busque la palabra "DumpLogs" en la página.  

 Cuando haya descargado los datos de registro, puede ver las entradas del registro en los archivos. Estos archivos de registro usan un formato de texto delimitado registro que muchas herramientas de lectura pueden analizar, incluye Microsoft Message Analyzer (para obtener más información, consulte la guía [Monitoring, Diagnosing and Troubleshooting Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md)). Diferentes herramientas tienen diferentes recursos para formatear, filtrar, ordenar y buscar el contenido de los archivos de registro. Para obtener más información sobre el formato de archivo de registro de registro de almacenamiento y el contenido, consulte [formato del registro de Storage Analytics](/rest/api/storageservices/storage-analytics-log-format) y [Storage Analytics Logged Operations y mensajes de estado](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages).

## <a name="next-steps"></a>Pasos siguientes

* [Formato del registro de Storage Analytics](/rest/api/storageservices/storage-analytics-log-format)
* [Operaciones y mensajes de estado registrados por Storage Analytics](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)
* [Métricas del análisis de almacenamiento (clásico)](storage-analytics-metrics.md)