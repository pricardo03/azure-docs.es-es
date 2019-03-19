---
title: Métricas de análisis de Azure Storage (clásico)
description: Obtenga información sobre cómo usar las métricas en Azure Storage.
services: storage
author: fhryo-msft
ms.service: storage
ms.topic: article
ms.date: 03/11/2019
ms.author: fryu
ms.subservice: common
ms.openlocfilehash: b35d3d22ce154420c9099143894688389e8af420
ms.sourcegitcommit: f596d88d776a3699f8c8cf98415eb874187e2a48
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2019
ms.locfileid: "58078047"
---
# <a name="azure-storage-analytics-metrics-classic"></a>Métricas de análisis de Azure Storage (clásico)

Storage Analytics puede almacenar métricas que incluyen estadísticas acumuladas de las transacciones y datos de capacidad sobre las solicitudes realizadas a un servicio de almacenamiento. Las transacciones se notifican tanto en el nivel de operación de API como en el nivel de servicio de almacenamiento, y la capacidad se notifica en el nivel de servicio de almacenamiento. Los datos de las métricas se pueden utilizar para analizar el uso del servicio de almacenamiento, diagnosticar problemas con las solicitudes realizadas en el mismo y mejorar el rendimiento de las aplicaciones que utilizan un servicio.  

 Las métricas de Storage Analytics se habilitan de forma predeterminada para las nuevas cuentas de almacenamiento. Puede configurar las métricas en el [portal Azure](https://portal.azure.com/); para obtener más información, consulte [supervisar una cuenta de almacenamiento en Azure portal](/azure/storage/storage-monitor-storage-account). También puede habilitar Storage Analytics mediante programación a través de la API de REST o la biblioteca de cliente. Utilice las operaciones Set Service Properties para habilitar Storage Analytics para cada servicio.  

> [!NOTE]
> Métricas de Storage Analytics están disponibles para los servicios Blob, cola, tabla y archivo.
> Métricas de Storage Analytics ahora son las métricas del modelo clásico. Microsoft recomienda usar [las métricas de almacenamiento en Azure Monitor](/azure/storage/common/storage-metrics-in-azure-monitor.md) en lugar de las métricas del análisis de almacenamiento.

## <a name="transaction-metrics"></a>Métricas de transacciones  
 Se registra un conjunto sólido de datos a intervalos de cada hora o de cada minuto para cada servicio de almacenamiento y operación de API que se ha solicitado, incluidos entradas/salidas, disponibilidad, errores y porcentajes de solicitudes por categorías. Puede ver una lista completa de los detalles de transacción en el tema [Esquema de las tablas de métricas de Storage Analytics](/rest/api/storageservices/storage-analytics-metrics-table-schema.md).  

 Los datos de las transacciones se registran en dos niveles: el nivel de servicio y el nivel de operación de API. En el nivel de servicio, las estadísticas que resumen todas las operaciones de API solicitadas se escriben en una entidad de la tabla cada hora, aunque no se haya realizado ninguna solicitud al servicio. En el nivel de operación de API, las estadísticas se escriben únicamente en una entidad si la operación se solicitó durante esa hora.  

 Por ejemplo, si realiza una operación **GetBlob** en Blob service, las métricas de análisis de Storage registrarán la solicitud y la incluirán en los datos agregados tanto para Blob service como para la operación **GetBlob**. Sin embargo, si no hay ningún **GetBlob** se solicita una operación durante la hora, una entidad no escribirá en el *$MetricsTransactionsBlob* para esa operación.  

 Las métricas de transacciones se registran para las solicitudes del usuario y para las solicitudes realizadas por Storage Analytics. Por ejemplo, se registran las solicitudes realizadas por Storage Analytics para escribir registros y entidades de tabla.

## <a name="capacity-metrics"></a>Métricas de capacidad  

> [!NOTE]
>  Actualmente, las métricas de capacidad solo están disponibles para Blob service.

 Los datos de capacidad se registran diariamente para Blob service de una cuenta de almacenamiento, y se escriben dos entidades de tabla. Una entidad proporciona estadísticas para los datos de usuario y la otra proporciona estadísticas sobre el contenedor de blob `$logs` utilizado por Storage Analytics. El *$MetricsCapacityBlob* tabla incluye las estadísticas siguientes:  

- **Capacity**: La cantidad de almacenamiento utilizado por Blob service de la cuenta de almacenamiento, en bytes.  
- **ContainerCount**: El número de contenedores de blob en Blob service de la cuenta de almacenamiento.  
- **ObjectCount**: El número de blobs de bloque o en páginas confirmados y sin confirmar en Blob service de la cuenta de almacenamiento.  

  Para obtener más información acerca de las métricas de capacidad, vea el [Esquema de las tablas de métricas de Storage Analytics](/rest/api/storageservices/storage-analytics-metrics-table-schema.md).  

## <a name="how-metrics-are-stored"></a>Cómo se almacenan las métricas  

 Todos los datos de métricas para cada uno de los servicios de almacenamiento se almacenan en tres tablas reservadas para ese servicio: una tabla para la información sobre transacciones, otra tabla para la información sobre las transacciones por minuto y una última tabla para la información sobre capacidad. La información sobre transacciones y transacciones por minuto consta de datos de solicitudes y respuestas, y la información sobre capacidad consta de datos de uso del almacenamiento. Las métricas por hora, las métricas por minuto y capacidad para un almacenamiento de blobs de la cuenta es el servicio pueden tener acceso en tablas cuyos nombres como se describe en la tabla siguiente.  

|Nivel de métricas|Nombres de tabla|Compatible con versiones|  
|-------------------|-----------------|----------------------------|  
|Métricas por horas, ubicación principal|-   $MetricsTransactionsBlob<br />-   $MetricsTransactionsTable<br />-   $MetricsTransactionsQueue|Versiones anteriores a 2013-08-15 solamente. Si bien estos nombres todavía se admiten, se recomienda que empiece a usar las tablas que se muestran a continuación.|  
|Métricas por horas, ubicación principal|-   $MetricsHourPrimaryTransactionsBlob<br />-   $MetricsHourPrimaryTransactionsTable<br />-   $MetricsHourPrimaryTransactionsQueue<br />-   $MetricsHourPrimaryTransactionsFile|Todas las versiones. Compatibilidad con métricas de servicios de archivo está disponible únicamente en la versión 2015-04-05 y versiones posteriores.|  
|Métricas por minutos, ubicación principal|-   $MetricsMinutePrimaryTransactionsBlob<br />-   $MetricsMinutePrimaryTransactionsTable<br />-   $MetricsMinutePrimaryTransactionsQueue<br />-   $MetricsMinutePrimaryTransactionsFile|Todas las versiones. Compatibilidad con métricas de servicios de archivo está disponible únicamente en la versión 2015-04-05 y versiones posteriores.|  
|Métricas por horas, ubicación secundaria|-   $MetricsHourSecondaryTransactionsBlob<br />-   $MetricsHourSecondaryTransactionsTable<br />-   $MetricsHourSecondaryTransactionsQueue|Todas las versiones. Debe estar habilitada la replicación con redundancia geográfica con acceso de lectura.|  
|Métricas por minutos, ubicación secundaria|-   $MetricsMinuteSecondaryTransactionsBlob<br />-   $MetricsMinuteSecondaryTransactionsTable<br />-   $MetricsMinuteSecondaryTransactionsQueue|Todas las versiones. Debe estar habilitada la replicación con redundancia geográfica con acceso de lectura.|  
|Capacidad (solo Blob service)|$MetricsCapacityBlob|Todas las versiones.|  

 Estas tablas se crean automáticamente cuando se habilita Storage Analytics para un punto de conexión de servicio de almacenamiento. Se tiene acceso a través del espacio de nombres de la cuenta de almacenamiento, por ejemplo: `https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`. Las tablas de métricas no aparecen en una operación de lista y deben tener acceso directamente a través del nombre de la tabla.  

## <a name="enable-metrics-using-the-azure-portal"></a>Habilitar las métricas mediante el portal de Azure
Siga estos pasos para habilitar las métricas en [Azure Portal](https://portal.azure.com):

1. Vaya a la cuenta de almacenamiento.
1. Seleccione **configuración de diagnóstico (clásica)** desde el **menú** panel.
1. Asegúrese de que la opción **Estado** está establecida en **Activada**.
1. Seleccione las métricas para los servicios que desea supervisar.
1. Especifique una directiva de retención para indicar cuánto tiempo se conservarán las métricas y los datos de registro.
1. Seleccione **Guardar**.

[Azure Portal](https://portal.azure.com) no permite actualmente configurar métricas por minuto en su cuenta de almacenamiento; debe habilitar las métricas por minuto con PowerShell o mediante programación.

> [!NOTE]
>  Tenga en cuenta que el portal de Azure no permite actualmente configurar métricas por minuto en su cuenta de almacenamiento. Debe habilitar las métricas por minuto con PowerShell o mediante programación.  

## <a name="enable-storage-metrics-using-powershell"></a>Habilitar las métricas de almacenamiento con PowerShell  
Puede usar PowerShell en el equipo local para configurar las métricas de almacenamiento en la cuenta de almacenamiento mediante el cmdlet de PowerShell de Azure **Get-AzureStorageServiceMetricsProperty** para recuperar la configuración actual y el cmdlet  **Set-AzureStorageServiceMetricsProperty** para cambiar la configuración actual.  

Los cmdlets que controlan las Métricas de almacenamiento usan los siguientes parámetros:  

* **ServiceType**, los valores posibles son **Blob**, **cola**, **tabla**, y **archivo**.
* **MetricsType**, los valores posibles son **hora** y **minuto**.  
* **MetricsLevel**, los valores posibles son:
* **Ninguna**: Desactiva la supervisión.
* **Servicio**: Recopila métricas como entrada/salida, disponibilidad, latencia y porcentajes de éxito, que se agregan para el blob, cola, tabla y servicios de archivo.
* **ServiceAndApi**: Además de las métricas de servicio, recopila el mismo conjunto de métricas para cada operación de almacenamiento en la API de servicios de Azure Storage.

Por ejemplo, el siguiente comando activa las métricas por minuto para el servicio BLOB en su cuenta de almacenamiento predeterminada con el período de retención establecido en cinco días:  

```  
Set-AzureStorageServiceMetricsProperty -MetricsType Minute   
-ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5  
```  

El comando siguiente recupera el nivel de métricas por hora actual y los días de retención para el servicio BLOB en su cuenta de almacenamiento predeterminada:  

```  
Get-AzureStorageServiceMetricsProperty -MetricsType Hour   
-ServiceType Blob  
```  

Para más información sobre cómo configurar los cmdlets de Azure PowerShell para que funcionen con su suscripción de Azure y cómo seleccionar la cuenta de almacenamiento predeterminada que quiere usar, vea: [Cómo instalar y configurar Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/).  

## <a name="enable-storage-metrics-programmatically"></a>Habilitar las métricas de almacenamiento mediante programación  
Además de usar el portal de Azure o los cmdlets de PowerShell de Azure para controlar las métricas de almacenamiento, también puede usar una de las API de almacenamiento de Azure. Por ejemplo, si está utilizando un lenguaje .NET puede usar la biblioteca de cliente de almacenamiento.  

Las clases **CloudBlobClient**, **CloudQueueClient**, **CloudTableClient**, y **CloudFileClient** todos tienen métodos como  **SetServiceProperties** y **SetServicePropertiesAsync** que toman un **ServiceProperties** objeto como parámetro. Puede usar el **ServiceProperties** objeto para la configuración de las métricas de almacenamiento. Por ejemplo, la siguiente C# fragmento de código muestra cómo cambiar los días de retención y el nivel de métricas para las métricas por hora de la cola:  

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);  
var queueClient = storageAccount.CreateCloudQueueClient();  
var serviceProperties = queueClient.GetServiceProperties();  

serviceProperties.HourMetrics.MetricsLevel = MetricsLevel.Service;  
serviceProperties.HourMetrics.RetentionDays = 10;  

queueClient.SetServiceProperties(serviceProperties);  
```  

Para obtener más información sobre el uso de un lenguaje .NET para configurar las métricas de almacenamiento, consulte [biblioteca de cliente de almacenamiento para .NET](https://msdn.microsoft.com/library/azure/mt347887.aspx).  

Para obtener información general acerca de cómo configurar las métricas de almacenamiento mediante la API de REST, consulte [habilitar y configurar el análisis de almacenamiento](/rest/api/storageservices/Enabling-and-Configuring-Storage-Analytics.md).  

##  <a name="viewing-storage-metrics"></a>Mostrar Métricas de almacenamiento  
Cuando haya configurado las métricas de Storage Analytics para supervisar su cuenta de almacenamiento, Storage Analytics se encargará de registrar las métricas en un conjunto de tablas conocidas en su cuenta de almacenamiento. Puede configurar gráficos para ver las métricas por hora en [Azure Portal](https://portal.azure.com):

1. Vaya a la cuenta de almacenamiento en [Azure Portal](https://portal.azure.com).
1. Seleccione **métricas (clásico)** en el **menú** hoja para el servicio cuyas métricas desea ver.
1. Haga clic en el gráfico que desea configurar.
1. En la hoja **Editar gráfico**, seleccione el **intervalo de tiempo**, el **tipo de gráfico** y las métricas que quiere mostrar en el gráfico.

En el **supervisión (clásica)** sección de la hoja del menú de su cuenta de almacenamiento en Azure portal, puede configurar [reglas de alerta](#metrics-alerts), como el envío de correo electrónico de alertas para avisarle cuando una métrica específica alcanza un valor determinado.

Si desea descargar las métricas de almacenamiento a largo plazo o para analizarlas localmente, debe usar una herramienta o escribir código para leer las tablas. Debe descargar las métricas por minuto para el análisis. Las tablas no aparecen si lista todas las tablas en su cuenta de almacenamiento, pero puede acceder a ellas directamente por su nombre. Muchas herramientas de exploración de almacenamiento son conscientes de estas tablas y permiten que las vea directamente (consulte [herramientas de cliente de Azure Storage](/azure/storage/storage-explorers) para obtener una lista de herramientas disponibles).

||||  
|-|-|-|  
|**Métricas**|**Nombres de tabla**|**Notas**|  
|Métricas por hora|$MetricsHourPrimaryTransactionsBlob<br /><br /> $MetricsHourPrimaryTransactionsTable<br /><br /> $MetricsHourPrimaryTransactionsQueue<br /><br /> $MetricsHourPrimaryTransactionsFile|En versiones anteriores a 2013-08-15, estas tablas se denominaban:<br /><br /> $MetricsTransactionsBlob <br /><br /> $MetricsTransactionsTable<br /><br />  $MetricsTransactionsQueue<br /><br /> Las métricas para el servicio de archivos están disponible desde la versión 2015-04-05.|  
|Métricas por minuto|$MetricsMinutePrimaryTransactionsBlob<br /><br /> $MetricsMinutePrimaryTransactionsTable<br /><br /> $MetricsMinutePrimaryTransactionsQueue<br /><br /> $MetricsMinutePrimaryTransactionsFile|Solo puede habilitarse mediante PowerShell o mediante programación.<br /><br /> Las métricas para el servicio de archivos están disponible desde la versión 2015-04-05.|  
|Capacity|$MetricsCapacityBlob|Solo el servicio BLOB.|  

Puede encontrar detalles completos de los esquemas para estas tablas en [Esquema de las tablas de métricas de Storage Analytics](/rest/api/storageservices/storage-analytics-metrics-table-schema.md). Las filas de ejemplo que tiene a continuación muestran solo un subconjunto de columnas disponibles, pero ilustran algunas características importantes acerca de la manera en que las Métricas de almacenamiento guardan estas métricas:  

||||||||||||  
|-|-|-|-|-|-|-|-|-|-|-|  
|**PartitionKey**|**RowKey**|**Timestamp**|**TotalRequests**|**TotalBillableRequests**|**TotalIngress**|**TotalEgress**|**Disponibilidad**|**AverageE2ELatency**|**AverageServerLatency**|**PercentSuccess**|  
|20140522T1100|user;All|2014-05-22T11:01:16.7650250Z|7|7|4003|46801|100|104.4286|6.857143|100|  
|20140522T1100|user;QueryEntities|2014-05-22T11:01:16.7640250Z|5|5|2694|45951|100|143.8|7.8|100|  
|20140522T1100|user;QueryEntity|2014-05-22T11:01:16.7650250Z|1|1|538|633|100|3|3|100|  
|20140522T1100|user;UpdateEntity|2014-05-22T11:01:16.7650250Z|1|1|771|217|100|9|6|100|  

En este ejemplo de datos de métricas por minuto, la clave de partición usa el tiempo en la resolución de minutos. La clave de fila identifica el tipo de información que se almacena en la fila y se compone de dos fragmentos de información, el tipo de acceso y el tipo de solicitud:  

-   El tipo de acceso es **usuario** o **sistema**, donde **usuario** hace referencia a todas las solicitudes de usuario para el servicio de almacenamiento y **sistema** hace referencia a solicitudes realizadas por Storage Analytics.  

-   El tipo de solicitud es **todas** en cuyo caso es una línea de resumen, o lo identifica como una API específica **QueryEntity** o **UpdateEntity**.  

Los datos de ejemplo anteriores muestran todos los registros para un único minuto (empezando a las 11:00 A.M.), por lo que el número de **QueryEntities** solicita más el número de **QueryEntity** solicita más el número de  **UpdateEntity** solicitudes agregar hasta siete, que es el total se muestra en el **user: All** fila. De forma similar, puede derivar la latencia media de extremo a otro 104,4286 en la **user: All** fila calculando ((143.8 * 5) + 3 + 9) / 7.  

## <a name="metrics-alerts"></a>Alertas de métricas
Debería considerar la configuración de alertas en el [portal Azure](https://portal.azure.com) por lo que se le notificará automáticamente los cambios importantes en el comportamiento de los servicios de almacenamiento. Si usa una herramienta de exploración de almacenamiento para descargar estos datos de métricas en un formato delimitado, puede usar Microsoft Excel para analizar los datos. Consulte [Herramientas de cliente de Azure Storage](/azure/storage/storage-explorers) para ver una lista de las herramientas del explorador de almacenamiento disponible. Puede configurar alertas en el **alerta (clásica)** hoja, accesible mediante **supervisión (clásica)** en la hoja de menú de la cuenta de almacenamiento.

> [!IMPORTANT]
> Puede producirse un retraso entre un evento de almacenamiento y cuándo se registran los datos de métricas por hora o minuto correspondientes. En el caso de las métricas por minuto, es posible que se escriban varios minutos de datos de una vez. Esto puede conllevar que las transacciones de minutos anteriores se sumen a la transacción del minuto actual. Cuando esto sucede, es posible que el servicio de alerta carezca de todos los datos de métricas disponibles para el intervalo de alerta configurado, lo que podría provocar que se activaran alertas de forma inesperada.
>

## <a name="accessing-metrics-data-programmatically"></a>Acceso a los datos de métricas mediante programación  
El listado siguiente muestra código C# de ejemplo que accede a las métricas por minuto para un intervalo de minutos y muestra los resultados en una ventana de la consola. El ejemplo de código usa la versión de la biblioteca de cliente de Azure Storage 4.x o posterior, que incluye el **CloudAnalyticsClient** clase que simplifica el acceso a las tablas de métricas de storage.  

```csharp
private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)  
{  
 // Convert the dates to the format used in the PartitionKey  
 var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");  
 var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");  

 var services = Enum.GetValues(typeof(StorageService));  
 foreach (StorageService service in services)  
 {  
     Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);  
     var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);  
     var t = analyticsClient.GetMinuteMetricsTable(service);  
     var opContext = new OperationContext();  
     var query =  
             from entity in metricsQuery  
             // Note, you can't filter using the entity properties Time, AccessType, or TransactionType  
             // because they are calculated fields in the MetricsEntity class.  
             // The PartitionKey identifies the DataTime of the metrics.  
             where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0   
             select entity;  

     // Filter on "user" transactions after fetching the metrics from Table Storage.  
     // (StartsWith is not supported using LINQ with Azure Table storage)  
     var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));  
     var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();  
     Console.WriteLine(resultString);  
 }  
}  

private static string MetricsString(MetricsEntity entity, OperationContext opContext)  
{  
 var entityProperties = entity.WriteEntity(opContext);  
 var entityString =  
         string.Format("Time: {0}, ", entity.Time) +  
         string.Format("AccessType: {0}, ", entity.AccessType) +  
         string.Format("TransactionType: {0}, ", entity.TransactionType) +  
         string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));  
 return entityString;  
}  
```  

## <a name="billing-on-storage-metrics"></a>Facturación de las métricas de almacenamiento  
Las solicitudes por escrito para crear entidades de tabla para métricas se cobran con las tarifas estándar aplicables a todas las operaciones de Azure Storage.  

Las solicitudes de lectura y eliminación de datos de métricas de un cliente de también son facturables tarifas estándar. Si ha configurado una directiva de retención de datos, no se le cobrará cuando Azure Storage elimine datos de métricas antiguos. Sin embargo, si elimina datos de análisis, se cobrará a su cuenta las operaciones de eliminación.  

La capacidad usada por las tablas de métricas también es facturable. Puede usar lo siguiente para calcular la cantidad de capacidad que se utiliza para almacenar datos de métricas:  

-   Si cada hora un servicio utiliza todas las API en todos los servicios, aproximadamente 148KB de datos se almacena cada hora en las tablas de transacciones de métricas si ha habilitado tanto y API de nivel de servicio resumen.  
-   Si dentro de cada hora, un servicio utiliza todas las API en el servicio, a continuación, aproximadamente 12KB de datos se almacena cada hora en las tablas de transacciones de métricas si ha habilitado el resumen de nivel de servicio simplemente.  
-   La tabla de capacidad para blobs tiene agregan dos filas cada día, siempre que haya optado por sesión para los registros. Esto implica que todos los días, el tamaño de esta tabla aumenta en 300 bytes aproximadamente.

## <a name="next-steps"></a>Pasos siguientes
* [Cómo supervisar una cuenta de almacenamiento](https://www.windowsazure.com/manage/services/storage/how-to-monitor-a-storage-account/)   
* [Esquema de las tablas de métricas de Storage Analytics](/rest/api/storageservices/storage-analytics-metrics-table-schema.md)   
* [Operaciones y mensajes de estado registrados por Storage Analytics](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages.md)   
* [Registro de Storage Analytics](storage-analytics-logging.md)
