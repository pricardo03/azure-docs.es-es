---
title: 'Azure Cosmos DB: API, SDK y recursos de Java asincrónico para SQL'
description: Obtenga toda la información sobre la API y el SDK de Java asincrónico para SQL, incluidas la fechas de lanzamiento, fechas de retirada y cambios realizados entre las versiones del SDK de Java asincrónico para SQL de Azure Cosmos DB.
author: moderakh
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 2/4/2019
ms.author: moderakh
ms.openlocfilehash: 31683159f96dd87fe4e7ceea050d5e1d213decba
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55770357"
---
# <a name="azure-cosmos-db-async-java-sdk-for-sql-api-release-notes-and-resources"></a>SDK de Java asincrónico de Azure Cosmos DB para API de SQL: Notas de la versión y recursos
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
> * [SQL](sql-api-query-reference.md)
> * [BulkExecutor: .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor: Java](sql-api-sdk-bulk-executor-java.md)

El SDK de Java asincrónico de API de SQL es distinto al SDK de Java de API de SQL al proporcionar a las operaciones asincrónicas compatibilidad con la [biblioteca de Netty](https://netty.io/). El [SDK de Java de API de SQL](sql-api-sdk-java.md) no admite las operaciones asincrónicas. 

| |  |
|---|---|
| **Descarga del SDK** | [Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) |
|**Documentación de la API** |[Documentación de referencia de API](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient?view=azure-java-stable) | 
|**Contribuya al SDK** | [GitHub](https://github.com/Azure/azure-cosmosdb-java) | 
|**Primeros pasos** | [Introducción al SDK de Java asincrónico](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started) | 
|**Código de ejemplo** | [GitHub](https://github.com/Azure/azure-cosmosdb-java#usage-code-sample)| 
| **Consejos de rendimiento**| [Léame de GitHub](https://github.com/Azure/azure-cosmosdb-java#guide-for-prod)| 
| **Tiempo de ejecución mínimo admitido**|[JDK 8](https://aka.ms/azure-jdks) | 

## <a name="release-notes"></a>Notas de la versión

### <a name="a-name240-beta-1240-beta-1"></a><a name="2.4.0-beta-1"/>2.4.0-beta-1
* Se ha agregado compatibilidad con Direct.
* Se han cambiado las API que aceptan java.util.Collection para las que el orden es importante para aceptar java.util.List en su lugar.
  Ahora ConnectionPolicy#getPreferredLocations() JsonSerialization y PartitionKey(.) aceptan List.
* Se ha corregido un error de sesión en la consulta de documentos en modo de puerta de enlace.
* Dependencias actualizadas (netty 0.4.20 [github #79](https://github.com/Azure/azure-cosmosdb-java/issues/79), RxJava 1.3.8).

### <a name="a-name231231"></a><a name="2.3.1"/>2.3.1
* Corrige la manipulación de respuestas de consultas muy extensas.
* Corrige el control de tokens de recursos al crear una instancia de cliente ([github #78](https://github.com/Azure/azure-cosmosdb-java/issues/78)).
* Actualiza la dependencia vulnerable en jackson-databind ([github #77](https://github.com/Azure/azure-cosmosdb-java/pull/77)).

### <a name="a-name230230"></a><a name="2.3.0"/>2.3.0
* Se ha corregido un error de pérdida de recursos.
* Se ha agregado compatibilidad con MultiPolygon.
* Se ha agregado compatibilidad con los encabezados personalizados en RequestOptions.

### <a name="a-name222222"></a><a name="2.2.2"/>2.2.2
* Se ha corregido un error de empaquetado.

### <a name="a-name221221"></a><a name="2.2.1"/>2.2.1
* Se corrigió un error NPE en la ruta de acceso de reintento de escritura.
* Se corrigió un error NPE en la administración de puntos de conexión.
* Se han actualizado dependencias vulnerables ([GitHub #68](https://github.com/Azure/azure-cosmosdb-java/issues/68)).
* Se agregó compatibilidad para el registro de red de Netty para solucionar problemas.

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Se agregó compatibilidad para escrituras en varias regiones.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Se agregó compatibilidad con proxy.
* Se agregó compatibilidad con la autorización de tokens de recursos.
* Se ha corregido un error en el tratamiento de claves de partición de gran tamaño ([GitHub 63](https://github.com/Azure/azure-cosmosdb-java/issues/63)).
* Documentación mejorada.
* SDK reestructurados en módulos más granulares.

### <a name="a-name201201"></a><a name="2.0.1"/>2.0.1
* Se ha corregido un error de las configuraciones regionales distintas del inglés ([GitHub 51](https://github.com/Azure/azure-cosmosdb-java/issues/51)).
* Se han agregado métodos auxiliares en conflicto de recursos.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Se ha reemplazado la dependencia de org.json por jackson por motivos de rendimiento y licencias ([GitHub 29](https://github.com/Azure/azure-cosmosdb-java/issues/29)).
* La clase OfferV2 en desuso se quitó.
* Se agregó el método de descriptor de acceso a la clase Offer para el contenido de rendimiento.
* Todos los métodos de Document/Resource que devuelven tipos org.json cambiaron para devolver un tipo de objeto jackson.
* El método getObject(.) de clases de ampliación de JsonSerializable cambió para devolver un tipo jackson ObjectNode.
* El método getCollection(.) cambió para devolver un tipo Collection ObjectNode.
* Se eliminaron los constructores de subclases JsonSerializable con el argumento org.json.JSONObject.
* JsonSerializable.toJson (SerializationFormattingPolicy.Indented) usa ahora dos espacios para la sangría.
  
### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2
* Se agregó compatibilidad con la directiva de índice único.
* Se agregó compatibilidad para limitar la respuesta del tamaño del token de continuación en opciones de fuente.
* Se agregó compatibilidad para división de particiones en la consulta entre particiones.
* Se ha corregido un error en la serialización de marcas de tiempo de Json ([GitHub 32](https://github.com/Azure/azure-cosmosdb-java/issues/32)).
* Se ha corregido un error en la serialización de enumeración.
* Se ha corregido un error en la administración de documentos con un tamaño de 2 MB ([GitHub 33](https://github.com/Azure/azure-cosmosdb-java/issues/33)).
* Se ha actualizado la dependencia com.fasterxml.jackson.core:jackson-databind a la versión 2.9.5 debido a un error ([jackson-databind: GitHub #1599](https://github.com/FasterXML/jackson-databind/issues/1599))
* Se ha actualizado la dependencia en rxjava-extras a la versión 0.8.0.17 debido a un error ([rxjava-extras: GitHub #30](https://github.com/davidmoten/rxjava-extras/issues/30)).
* Se ha actualizado la descripción de metadatos en el archivo pom para que esté alineado con el resto de la documentación.
* Se ha mejorado la sintaxis ([GitHub 41](https://github.com/Azure/azure-cosmosdb-java/issues/41)), ([GitHub 40](https://github.com/Azure/azure-cosmosdb-java/issues/40)).

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Se agregó compatibilidad con la contrapresión en las consultas.
* Se agregó compatibilidad con el identificador de intervalo de claves de partición en la consulta.
* Corrección que permite un token de mayor continuación en el encabezado de solicitud (corrección de errores GitHub 24).
* Se actualizó la dependencia de Netty a 4.1.22.Final para garantizar que la JVM se apaga después de que finaliza el subproceso principal.
* Corrección para evitar pasar el token de sesión cuando se leen los recursos maestros.
* Se agregaron más ejemplos.
* Se agregaron más escenarios de pruebas comparativas.
* Se corrigieron los archivos de encabezado de Java para la generación correcta de documentos de Java.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* El SDK de disponibilidad general con compatibilidad total para E/S sin bloqueo mediante la [biblioteca de Netty](https://netty.io/) en el modo de puerta de enlace. 

## <a name="release-and-retirement-dates"></a>Fechas de lanzamiento y de retirada
Microsoft notificará la retirada de un SDK con al menos **12 meses** de antelación para facilitar la transición a una versión compatible o más reciente.

Solo se han agregado nuevas características, funcionalidades y optimizaciones al SDK actual. Por lo tanto, se recomienda actualizar siempre a la última versión del SDK lo antes posible.

El servicio rechazará cualquier solicitud realizada a Cosmos DB mediante un SDK retirado.

<br/>

| Versión | Fecha de lanzamiento | Fecha de retirada |
| --- | --- | --- |
| [2.4.0-beta-1](#2.4.0-beta-1) |4 de febrero de 2019|--- |
| [2.3.1](#2.3.1) |15 de enero de 2019|--- |
| [2.3.0](#2.3.0) |29 de noviembre de 2018|--- |
| [2.2.2](#2.2.2) |8 de noviembre de 2018|--- |
| [2.2.1](#2.2.1) |2 de noviembre de 2018|--- |
| [2.2.0](#2.2.0) |22 de septiembre de 2018|--- |
| [2.1.0](#2.1.0) |5 de septiembre de 2018|--- |
| [2.0.1](#2.0.1) |16 de agosto de 2018|--- |
| [2.0.0](#2.0.0) |20 de junio de 2018|--- |
| [1.0.2](#1.0.2) |18 de mayo de 2018|--- |
| [1.0.1](#1.0.1) |20 de abril de 2018|--- |
| [1.0.0](#1.0.0) |27 de febrero de 2018|--- |

## <a name="faq"></a>Preguntas más frecuentes
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Otras referencias
Para más información sobre Cosmos DB, consulte la página del servicio [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

