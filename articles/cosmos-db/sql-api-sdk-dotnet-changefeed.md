---
title: Recursos, API y SDK para los procesadores de fuente de cambios .NET para Azure Cosmos DB | Microsoft Docs
description: Aprenda todo lo necesario sobre el SDK y la API para los procesadores de fuente de cambios como, por ejemplo, fechas de lanzamiento, fechas de retirada y cambios realizados de una versión a otra del SDK para los procesadores de fuente de cambios de .NET.
services: cosmos-db
author: ealsur
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 10/24/2018
ms.author: maquaran
ms.openlocfilehash: f09430aeb38e6762729167494a23096c7bc5ca85
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50023958"
---
# <a name="net-change-feed-processor-sdk-download-and-release-notes"></a>SDK para los procesadores de fuente de cambios de .NET: descarga y notas de la versión
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [Fuente de cambios de .NET](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [Proveedor de recursos de REST](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [BulkExecutor: .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor: Java](sql-api-sdk-bulk-executor-java.md)

|   |   |
|---|---|
|**Descarga del SDK**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)|
|**Documentación de la API**|[Documentación de referencia de la API de biblioteca de procesadores de fuente de cambios](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)|
|**Primeros pasos**|[Primeros pasos con el SDL para los procesadores de fuente de cambios de .NET](change-feed.md)|
|**Plataforma admitida actualmente**| [Microsoft .NET 4.5 Framework](https://www.microsoft.com/download/details.aspx?id=30653)</br> [Microsoft .NET Core](https://www.microsoft.com/net/download/core) |

## <a name="release-notes"></a>Notas de la versión

### <a name="v2-builds"></a>compilaciones v2

### <a name="a-name221221"></a><a name="2.2.1"/>2.2.1
* Cálculo del estimador fijo para las cuentas Multi Master y formato de token de sesión nuevo.

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Compatibilidad agregada para las colecciones de concesión con particiones. La clave de partición debe definirse como /id.
* Cambios importantes menores: los métodos de la interfaz IChangeFeedDocumentClient y la clase ChangeFeedDocumentClient se cambiaron para incluir los parámetros RequestOptions y CancellationToken. IChangeFeedDocumentClient es un punto de extensibilidad avanzado que le permite proporcionar una implementación personalizada del cliente de documentos para usarla con procesadores de fuente de cambios; por ejemplo, decore DocumentClient e intercepte todas las llamadas a él para realizar un seguimiento adicional, un control de errores, etc. Con esta actualización, el código que implementa IChangeFeedDocumentClient debe cambiarse para incluir nuevos parámetros en la implementación.
* Mejoras de diagnósticos menores.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Se agregó una nueva API, Task&lt;IReadOnlyList&lt;RemainingPartitionWork&gt;&gt; IRemainingWorkEstimator.GetEstimatedRemainingWorkPerPartitionAsync(). Esto puede usarse para obtener el trabajo estimado para cada partición.
* Admite el SDK de Microsoft.Azure.DocumentDB 2.0. Requiere Microsoft.Azure.DocumentDB 2.0 o posterior.

### <a name="a-name206206"></a><a name="2.0.6"/>2.0.6
* Se ha agregado una propiedad ChangeFeedEventHost.HostName pública para la compatibilidad con v1.

### <a name="a-name205205"></a><a name="2.0.5"/>2.0.5
* Se ha corregido una condición de carrera que se produce durante la división de la partición. La condición de carrera puede dar lugar a la adquisición de la concesión y perderla inmediatamente durante la división de la partición y causar contención. Se ha corregido el problema de la condición de carrera con esta versión.

### <a name="a-name204204"></a><a name="2.0.4"/>2.0.4
* SDK de GA

### <a name="a-name203-prerelease203-prerelease"></a><a name="2.0.3-prerelease"/>Versión preliminar 2.0.3
* Se han corregido los siguientes problemas:
  * Al producirse la división de particiones, podrían modificarse procesamientos de documentos duplicados antes de la partición.
  * GetEstimatedRemainingWork API devolvió 0 cuando no había concesiones en la colección de concesiones.

* Se han hecho públicas las siguientes excepciones. Las extensiones que implementan IPartitionProcessor pueden iniciar estas excepciones.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.LeaseLostException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionNotFoundException.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionSplitException. 

### <a name="a-name202-prerelease202-prerelease"></a><a name="2.0.2-prerelease"/>2.0.2-prerelease
* Cambios menores en la API:
  * Se quitó ChangeFeedProcessorOptions.IsAutoCheckpointEnabled, que estaba marcado como obsoleto.

### <a name="a-name201-prerelease201-prerelease"></a><a name="2.0.1-prerelease"/>2.0.1-prerelease
* Mejoras de estabilidad:
  * Mejor administración de la inicialización del almacén de concesiones. Cuando el almacén de concesiones está vacío, solo lo puede iniciar una instancia de procesador. Las otras esperarán.
  * Liberación o renovación de concesión más estable y eficaz. Renovar o liberar una concesión de una partición es independiente de la renovación de otras. En v1, se hacia de manera secuencial en todas las particiones.
* Nueva API v2:
  * Patrón de generador para la construcción flexible del procesador: la clase ChangeFeedProcessorBuilder.
    * Puede tomar cualquier combinación de parámetros.
    * Puede tomar la instancia DocumentClient para supervisar o conceder colecciones (no disponible en v1).
  * IChangeFeedObserver.ProcessChangesAsync ahora toma CancellationToken.
  * IRemainingWorkEstimator: el estimador de trabajo restante se puede usar de forma independiente del procesador.
  * Nuevos puntos de extensibilidad:
    * IParitionLoadBalancingStrategy: para el equilibrio de carga personalizado de particiones entre instancias del procesador.
    * ILease, ILeaseManager: para la administración personalizada de concesiones.
    * IPartitionProcessor: para cambios de procesamiento personalizados en una partición.
* Logging: usa la biblioteca [LibLog](https://github.com/damianh/LibLog).
* Compatible 100 % con la API v1.
* Base de código nueva.
* Compatible con el [SDK de .NET para SQL](sql-api-sdk-dotnet.md), versiones 1.21.1 y superior.

### <a name="v1-builds"></a>compilaciones v1

### <a name="a-name133133"></a><a name="1.3.3"/>1.3.3
* Se han agregado más registros.
* Se ha corregido la pérdida DocumentClient al llamar varias veces a la estimación de trabajo pendiente.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2
* Correcciones de la estimación de trabajo pendiente.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1
* Mejoras de estabilidad.
  * Corrección para administrar el problema de las tareas canceladas que puede detener a los observadores en algunas particiones.
* Compatibilidad con puntos de control manuales.
* Compatible con el [SDK de .NET para SQL](sql-api-sdk-dotnet.md), versiones 1.21 y posteriores.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Agrega compatibilidad con .NET Standard 2.0. El paquete admite ahora los monikers de plataforma `netstandard2.0` y `net451`.
* Compatible con el [SDK de .NET para SQL](sql-api-sdk-dotnet.md), versiones 1.17.0 y posteriores.
* Compatible con el [SDK de .NET Core para SQL](sql-api-sdk-dotnet-core.md), versiones 1.5.1 y posteriores.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1
* Corrige un problema con el cálculo de la estimación de trabajo restante cuando la fuente de cambios estaba vacía o no había trabajo pendiente.
* Compatible con el [SDK de .NET para SQL](sql-api-sdk-dotnet.md), versiones 1.13.2 y posteriores.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Agregue un método para obtener una estimación de trabajo restante que se va a procesar en la fuente de cambio.
* Compatible con el [SDK de .NET para SQL](sql-api-sdk-dotnet.md), versiones 1.13.2 y posteriores.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* SDK de GA
* Compatible con el [SDK de .NET para SQL](sql-api-sdk-dotnet.md), versiones 1.14.1 y anteriores.


## <a name="release--retirement-dates"></a>Fechas de lanzamiento y de retirada
Microsoft notificará la retirada de un SDK con al menos **12 meses** de antelación para facilitar la transición a una versión compatible o más reciente.

Solo se agregan nuevas características, funcionalidad y optimizaciones al SDK actual, por lo que se recomienda actualizar siempre a la última versión del SDK tan pronto como sea posible. 

El servicio rechazará cualquier solicitud realizada a Cosmos DB mediante un SDK retirado.

<br/>

| Versión | Fecha de lanzamiento | Fecha de retirada |
| --- | --- | --- |
| [2.2.1](#2.2.1) |24 de octubre de 2018 |--- |
| [1.3.3](#1.3.3) |8 de mayo de 2018 |--- |
| [1.3.2](#1.3.2) |18 de abril de 2018 |--- |
| [1.3.1](#1.3.1) |13 de marzo de 2018 |--- |
| [1.2.0](#1.2.0) |31 de octubre de 2017 |--- |
| [1.1.1](#1.1.1) |29 de agosto de 2017 |--- |
| [1.1.0](#1.1.0) |13 de agosto de 2017 |--- |
| [1.0.0](#1.0.0) |7 de julio de 2017 |--- |


## <a name="faq"></a>Preguntas más frecuentes
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Otras referencias
Para más información sobre Cosmos DB, consulte la página del servicio [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). 

