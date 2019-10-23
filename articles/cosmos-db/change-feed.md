---
title: Compatibilidad con la fuente de cambios en Azure Cosmos DB
description: Use la compatibilidad con la fuente de cambios de Azure Cosmos DB para controlar los cambios en documentos y realizar el procesamiento basado en eventos tales como desencadenadores y mantener actualizados las cachés y los sistemas de análisis.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.reviewer: sngun
ms.custom: seodec18
ms.openlocfilehash: 8e6bd3dadd636127f212db0ea0c0755a6b52a087
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2019
ms.locfileid: "72757024"
---
# <a name="change-feed-in-azure-cosmos-db---overview"></a>Fuente de cambios en Azure Cosmos DB: introducción

Para proporcionar compatibilidad con la fuente de cambios en Azure Cosmos DB, se observa si hay algún cambio en un contenedor de Azure Cosmos. A continuación, muestra la lista ordenada de los documentos que han cambiado en el orden en el que se modificaron. Los cambios se conservan y se pueden procesar de manera asincrónica. El resultado puede distribuirse entre uno o varios clientes para procesarse en paralelo. 

Azure Cosmos DB es adecuado para IoT, juegos y aplicaciones de registro de operaciones. Un modelo de diseño común en estas aplicaciones es usar los cambios en los datos para desencadenar acciones adicionales. Ejemplos de acciones adicionales:

* Desencadenamiento de una notificación o una llamada a una API cuando se inserta o actualiza un elemento.
* Procesamiento de secuencias en tiempo real para IoT o procesamiento de estadísticas sobre datos operativos en tiempo real.
* Movimiento de datos adicionales mediante la sincronización con una memoria caché, un motor de búsqueda o un almacén de datos, o mediante el archivo de datos en un almacenamiento en frío.

La fuente de cambios en Azure Cosmos DB le permite crear soluciones eficientes y escalables para cada uno de estos patrones, como se muestra en la imagen siguiente:

![Uso de la fuente de cambios de Azure Cosmos DB para aumentar la eficacia de los escenarios de informática orientada a eventos y análisis en tiempo real](./media/change-feed/changefeedoverview.png)

## <a name="supported-apis-and-client-sdks"></a>SDK de cliente y API compatibles

La característica actualmente es compatible con los siguientes SDK de cliente y API de Cosmos DB.

| **Controladores cliente** | **CLI de Azure** | **SQL API** | **Cassandra API** | **API de Azure Cosmos DB para MongoDB** | **Gremlin API**|**Table API** |
| --- | --- | --- | --- | --- | --- | --- |
| .NET | N/D | Sí | No | No | Sí | Sin |
|Java|N/D|Sí|No|No|Sí|Sin|
|Python|N/D|Sí|No|No|Sí|Sin|
|Node/JS|N/D|Sí|No|No|Sí|Sin|

## <a name="change-feed-and-different-operations"></a>Fuente de cambios y operaciones diferentes

Hoy en día, verá todas las operaciones en la fuente de cambios. La funcionalidad con la que puede controlar la fuente de cambios para operaciones específicas, tales como solo actualizaciones y no inserciones (las cuales aún no están disponibles). Puede agregar un "marcador temporal" en el elemento para las actualizaciones y filtrar los elementos basándose en dicho marcador durante el procesamiento en la fuente de cambios. Actualmente, la fuente de cambios no registra las eliminaciones. Al igual que en el ejemplo anterior, puede agregar un marcador temporal a los elementos que va a eliminar; por ejemplo, puede agregar un atributo "eliminado" al elemento y establecerlo en "true" para, posteriormente, agregar un período de vida al elemento, de forma que se elimine automáticamente. Puede leer la fuente de cambios para los elementos históricos (el cambio más reciente que corresponde al elemento no incluye los cambios intermedios), por ejemplo, los que se agregaron hace cinco años. Si no se eliminó el elemento, puede leer la fuente de cambios hasta el origen del contenedor.

### <a name="sort-order-of-items-in-change-feed"></a>Criterio de ordenación de los elementos en la fuente de cambios

Los elementos de la fuente de cambios están ordenados según su hora de modificación. Este criterio de ordenación solo se garantiza por clave de partición lógica.

### <a name="change-feed-in-multi-region-azure-cosmos-accounts"></a>Fuente de cambios en las cuentas de Azure Cosmos de varias regiones

En una cuenta de Azure Cosmos de varias regiones, si se produce un error en una región de escritura, la fuente de cambios funcionará durante la operación de conmutación por error manual y será contigua.

### <a name="change-feed-and-time-to-live-ttl"></a>Fuente de cambios y el período de vida (TTL)

Si la propiedad TTL (período de vida) se establece en -1 en un elemento, la fuente de cambios se conservará para siempre. Si los datos no se eliminan, se mantendrán en la fuente de cambios.  

### <a name="change-feed-and-_etag-_lsn-or-_ts"></a>Fuente de cambios y _etag, _lsn o _ts

El formato _etag es interno y no debe depender de él, porque puede cambiar en cualquier momento. _ts es una modificación o una marca de tiempo de creación. Puede usar _ts para la comparación cronológica. _lsn es un identificador de lote que se agrega solo para la fuente de cambios y representa el identificador de transacción. Muchos elementos pueden tener el mismo _lsn. ETag en FeedResponse es diferente del formato _etag que se ve en un elemento. _etag es un identificador interno que se usa para el control de simultaneidad e indica la versión del elemento, mientras que ETag se usa para secuenciar la fuente.

## <a name="change-feed-use-cases-and-scenarios"></a>Casos de uso y escenarios de la fuente de cambios

La fuente de cambios permite un procesamiento eficaz de grandes conjuntos de datos con un elevado volumen de escritura. La fuente de cambios también ofrece una alternativa a las consultas de conjuntos de datos completos para identificar qué ha cambiado.

### <a name="use-cases"></a>Casos de uso

Por ejemplo, con una fuente de cambios puede realizar las siguientes tareas de manera eficaz:

* Actualizar una caché, un índice de búsqueda o un almacenamiento de datos con los datos almacenados en Azure Cosmos DB.

* Implementar archivos y capas de datos en el nivel de aplicación; por ejemplo, almacenar "datos activos" en Azure Cosmos DB y "jubilar datos inactivos" en [Azure Blob Storage](../storage/common/storage-introduction.md).

* Realizar migraciones sin tiempos de inactividad a otra cuenta de Cosmos Azure o a otro contenedor de Azure Cosmos con una clave de partición lógica diferente.

* Implementar [arquitectura lambda](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) con Azure Cosmos DB, donde Azure Cosmos DB es compatible con las capas de servicio en tiempo real, por lotes y de consulta, lo que habilita la arquitectura lambda con un bajo TCO.

* Recibir y almacenar datos de eventos de dispositivos, sensores, infraestructura y aplicaciones, además de procesar dichos eventos en tiempo real, por ejemplo, con [Spark](../hdinsight/spark/apache-spark-overview.md).  En la siguiente imagen se muestra cómo puede implementar la arquitectura lambda mediante Azure Cosmos DB a través de la fuente de cambios:

![Canalización Lambda basada en Azure Cosmos DB para ingesta y consulta](./media/change-feed/lambda.png)

### <a name="scenarios"></a>Escenarios

Los siguientes son algunos de los escenarios en los que se puede implementar fácilmente la fuente de cambios:

* En las aplicaciones web o móviles [sin servidor](https://azure.microsoft.com/solutions/serverless/), puede realizar el seguimiento de eventos tales como los cambios realizados en el perfil, preferencias o ubicación del cliente para desencadenar determinadas acciones como, por ejemplo, enviar notificaciones push a los dispositivos mediante [Azure Functions](change-feed-functions.md).

* Si usa Azure Cosmos DB para compilar un juego, puede usar la fuente de cambios para, por ejemplo, implementar marcadores en tiempo real basados en las puntuaciones de los juegos completados.


## <a name="working-with-change-feed"></a>Trabajo con la fuente de cambios

Puede trabajar con la fuente de cambios mediante las siguientes opciones:

* [Using change feed with Azure Functions](change-feed-functions.md) (Usar la fuente de cambios con Azure Functions)
* [Usar la fuente de cambios con el procesador de fuente de cambios](change-feed-processor.md) 

La fuente de cambios está disponible para cada clave de partición lógica dentro del contenedor y, por tanto, se puede distribuir entre uno o varios consumidores para el procesamiento en paralelo, tal y como se muestra en la imagen a continuación.

![Procesamiento distribuido de la fuente de cambios de Azure Cosmos DB](./media/change-feed/changefeedvisual.png)

## <a name="features-of-change-feed"></a>Características de la fuente de cambios

* La fuente de cambios está habilitada de forma predeterminada para todas las cuentas de Azure Cosmos.

* Puede usar el [procesamiento aprovisionado](request-units.md) para leer la fuente de cambios, igual que con cualquier otra operación de Azure Cosmos DB, en cualquiera de las regiones asociadas con la base de datos de Azure Cosmos.

* La fuente de cambios incluye inserciones y operaciones de actualización realizadas en los elementos dentro del contenedor. Puede capturar las eliminaciones si establece un marcador de "eliminación temporal" dentro de los elementos (tales como documentos) en lugar de eliminaciones. También pude establecer un período finito de expiración para los elementos con la [funcionalidad de período de vida](time-to-live.md). Por ejemplo, puede elegir 24 horas y usar el valor de esa propiedad para capturar las eliminaciones. Con esta solución, tiene que procesar los cambios dentro de un intervalo de tiempo más corto que el período de expiración de TTL. 

* Cada cambio realizado en un elemento aparece exactamente una vez en la fuente de cambios y los clientes deben administrar la lógica de puntos de comprobación. Si quiere evitarse la complejidad de administrar los puntos de comprobación, el procesador de fuentes de cambio proporciona puntos de comprobación automáticos y semántica de "al menos una vez". Consulte [Usar la fuente de cambios con el procesador de fuente de cambios](change-feed-processor.md).

* Solo el cambio más reciente en un elemento determinado se incluye en el registro de cambios. Puede que los cambios intermedios no estén disponibles.

* La fuente de cambios está organizada según el orden de modificación dentro de cada valor de clave de partición lógica. No hay ningún orden garantizado entre valores de clave de partición.

* Los cambios se pueden sincronizar desde cualquier punto en el tiempo, es decir, no hay ningún período fijo de retención de datos durante el que los cambios estén disponibles.

* Los cambios están disponibles en paralelo para todas las claves de partición lógica de un contenedor de Azure Cosmos. Esta funcionalidad permite que los cambios de contenedores grandes se procesen en paralelo por medio de varios consumidores.

* Las aplicaciones pueden solicitar varias fuentes de cambios en el mismo contenedor de forma simultánea. ChangeFeedOptions.StartTime se puede usar para proporcionar un punto inicial. Por ejemplo, para buscar el token de continuación correspondiente a una hora especificada. ContinuationToken, si se especifica, tiene prioridad sobre los valores StartTime y StartFromBeginning. La precisión de ChangeFeedOptions.StartTime es aproximadamente de 5 segundos. 

## <a name="next-steps"></a>Pasos siguientes

Ahora, puede obtener más información acerca de las fuentes de cambios en los siguientes artículos:

* [Options to read change feed](read-change-feed.md) (Opciones para leer la fuente de cambios)
* [Using change feed with Azure Functions](change-feed-functions.md) (Usar la fuente de cambios con Azure Functions)
* [Usar el procesador de fuente de cambios](change-feed-processor.md)
