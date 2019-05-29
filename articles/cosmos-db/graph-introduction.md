---
title: Introducción a Gremlin API de Azure Cosmos DB
description: Aprenda a usar Azure Cosmos DB para almacenar, realizar consultas y recorrer grafos enormes con latencia baja mediante el lenguaje de consulta de grafos Gremlin de Apache TinkerPop.
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 05/20/2019
ms.author: lbosq
ms.openlocfilehash: 6f5d90f8b825b7076a1a5122dbef3c8b2990e216
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65954264"
---
# <a name="introduction-to-azure-cosmos-db-gremlin-api"></a>Introducción a Azure Cosmos DB: API de Gremlin

[Azure Cosmos DB](introduction.md) es el servicio de base de datos con varios modelos y distribución global de Microsoft para aplicaciones críticas. Es una base de datos que sigue varios modelos y es compatible con los modelos de datos de documento, de clave-valor, de grafo y de columnas. Gremlin API de Azure Cosmos DB se usa para almacenar y operar en los datos del grafo. Gremlin API admite el modelado de datos de Graph y proporciona API para atravesar los datos del grafo.

En este artículo se proporciona una introducción a Gremlin API de Azure Cosmos DB y se explica cómo se puede usar para almacenar grafos grandes con miles de millones de vértices y bordes. Puede consultar los gráficos con una latencia de milisegundos y evolucionar la estructura y el esquema de los gráficos con facilidad. Para consultar Azure Cosmos DB, puede usar el lenguaje de recorrido de grafos [Apache TinkerPop](https://tinkerpop.apache.org) o [Gremlin](https://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps).

## <a name="what-is-a-graph-database"></a>¿Qué es una base de datos de grafo?
Los datos, tal y como aparecen en el mundo real, están conectados naturalmente. El modelado de datos tradicional se centra en las entidades. En el caso de muchas aplicaciones, también existe la necesidad de modelar las entidades y las relaciones de manera natural.

Un [gráfico](http://mathworld.wolfram.com/Graph.html) es una estructura que está formada por [vértices](http://mathworld.wolfram.com/GraphVertex.html) y [bordes](http://mathworld.wolfram.com/GraphEdge.html). Tanto los vértices como los bordes pueden tener un número arbitrario de propiedades. 

* **Vértices**: denotan objetos discretos, como una persona, un lugar o un evento. 

* **Bordes**: representan las relaciones entre los vértices. Por ejemplo, es posible que una persona conozca a otra, haya participado en un evento y recientemente haya estado en una ubicación. 

* **Propiedades**: expresan información acerca de los bordes y los vértices. Las propiedades de ejemplo incluyen un vértice con nombre y edad. Los bordes tienen una marca de tiempo o un peso. En términos más formales, este modelo se conoce como [grafo de propiedades](https://tinkerpop.apache.org/docs/current/reference/#intro). Azure Cosmos DB es compatible con el modelo de grafo de propiedades.

Por ejemplo, en el siguiente grafo de ejemplo, se muestran las relaciones entre personas, dispositivos móviles, intereses y sistemas operativos:

![Base de datos de ejemplo que muestra personas, dispositivos e intereses](./media/graph-introduction/sample-graph.png)

Las bases de datos de gráficos permiten modelar y almacenar gráficos de forma natural y eficaz, lo que las hace útiles para muchos escenarios. Suelen ser bases de datos NoSQL, ya que estos casos de uso también necesitan a menudo flexibilidad en el esquema y una iteración rápida.

Puede combinar los cruces seguros rápidos que proporcionan las bases de datos de grafos con algoritmos de grafo (como búsqueda en profundidad, búsqueda en amplitud y el algoritmo de Dijkstra) para resolver problemas en diversos dominios, como las redes sociales, la administración de contenido, el geoespacial y las recomendaciones.

## <a name="features-of-azure-cosmos-db-graph-database"></a>Características de la base de datos de grafos de Azure Cosmos DB
 
Azure Cosmos DB es una base de datos de gráficos totalmente administrada que ofrece una distribución global, escalado elástico de almacenamiento y rendimiento, indexación y consulta automáticas, niveles de coherencia ajustables y compatibilidad con el estándar TinkerPop.

![Arquitectura de grafo de Azure Cosmos DB](./media/graph-introduction/cosmosdb-graph-architecture.png)

Azure Cosmos DB ofrece las siguientes funcionalidades diferenciadas en comparación con otras bases de datos de gráficos del mercado:

* Rendimiento y almacenamiento escalables de manera elástica

  Los gráficos en el mundo real necesitan escalarse más allá de la capacidad de un único servidor. Con Azure Cosmos DB, puede escalar sus grafos sin problemas en varios servidores. También puede escalar el rendimiento de su gráfico de forma independiente en función de los patrones de acceso. Azure Cosmos DB admite bases de datos de grafos que pueden escalarse a tamaños de almacenamiento prácticamente ilimitado y rendimiento aprovisionado.

* Replicación de varias regiones

  Azure Cosmos DB replica con transparencia los datos de gráfico en todas las regiones que se han asociado a la cuenta. La replicación permite desarrollar aplicaciones que requieren acceso global a los datos. Hay equilibrios en los ámbitos de la coherencia, disponibilidad y rendimiento, y las garantías correspondientes. Azure Cosmos DB proporciona conmutación por error regional transparente con las API de hospedaje múltiple. Puede escalar el rendimiento y el almacenamiento de forma elástica en todo el mundo.

* Consultas rápidas y recorridos con la sintaxis conocida de Gremlin

  Almacene vértices heterogéneos y bordes, y consulte estos documentos por medio de la sintaxis conocida de Gremlin. Azure Cosmos DB usa una tecnología de indexación estructurada mediante registros sin bloqueos y sumamente simultánea para indexar automáticamente todo el contenido. Esta capacidad permite realizar consultas y recorridos enriquecidos en tiempo real sin necesidad de especificar sugerencias de esquemas, índices secundarios ni vistas. Aprenda más en [Compatibilidad de Azure Cosmos DB con grafos Gremlin](gremlin-support.md).

* Completamente administrada

  Azure Cosmos DB elimina la necesidad de administrar recursos de bases de datos y máquinas. Al ser un servicio de Microsoft Azure completamente administrado, no es preciso que administre las máquinas virtuales, que implemente y configure software, que administre el escalado ni que realice complejas actualizaciones en la capa de datos. Se hacen copias de seguridad de todos los grafos, además de protegerlos ante errores regionales. Puede agregar fácilmente una cuenta de Azure Cosmos DB y aprovisionar capacidad a medida que la necesite, para poder concentrarse en su aplicación en lugar de en la operación y administración de la base de datos.

* Indexación automática

  De manera predeterminada, Azure Cosmos DB indexa automáticamente todas las propiedades en los nodos y bordes del grafo, y no espera ni requiere ningún esquema ni la creación de índices secundarios.

* Compatibilidad con Apache TinkerPop

  Azure Cosmos DB es compatible de forma nativa con el estándar Apache TinkerPop de código abierto y se puede integrar con otros sistemas de gráficos habilitados para TinkerPop. Por tanto, puede realizar fácilmente la migración desde una base de datos de grafos diferente, como Titan o Neo4j, o usar Azure Cosmos DB con marcos de análisis de grafos, como Apache Spark GraphX.

* Niveles de coherencia ajustables

  Seleccione entre cinco niveles de coherencia bien definidos para lograr un equilibrio óptimo entre coherencia y rendimiento. Para las consultas y las operaciones de lectura, Azure Cosmos DB ofrece cinco niveles de coherencia diferentes: segura, obsolescencia limitada, sesión, prefijo coherente y posible. Estos niveles de coherencia bien definidos y pormenorizados le permiten realizar equilibrios razonables entre la coherencia, la disponibilidad y la latencia. Encontrará más información en [Niveles de coherencia de datos optimizables en Azure Cosmos DB](consistency-levels.md).

Además, Azure Cosmos DB puede usar varios modelos, como documento y gráfico, en los mismos contenedores o bases de datos. Puede usar un contenedor de documentos para almacenar los datos de gráficos en paralelo con los documentos. Puede usar las consultas SQL sobre JSON y las de Gremlin para consultar los mismos datos como un gráfico.

## <a name="get-started"></a>Introducción

Puede usar la interfaz de la línea de comandos (CLI) de Azure, Azure PowerShell o Azure Portal para crear cuentas de Gremlin API de Azure Cosmos DB y acceder a ella. Después de crear una cuenta, puede acceder a las bases de datos de grafos dentro de esa cuenta con el punto de conexión de servicio de Gremlin API `https://<youraccount>.gremlin.cosmosdb.azure.com`, que proporciona un front-end de WebSocket para Gremlin. Puede configurar las herramientas compatibles con TinkerPop, como [Gremlin Console](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console), para que se conecten a este punto de conexión y compilar aplicaciones en Java, Node.js o cualquier controlador cliente de Gremlin.

En la siguiente tabla se muestran los controladores Gremlin populares que puede usar en Azure Cosmos DB:

| Descargar | Documentación | Introducción | Versión del conector admitida |
| --- | --- | --- | --- |
| [.NET](https://tinkerpop.apache.org/docs/3.3.1/reference/#gremlin-DotNet) | [Gremlin.NET en GitHub](https://github.com/apache/tinkerpop/tree/master/gremlin-dotnet) | [Crear gráfico con .NET](create-graph-dotnet.md) | 3.4.0-RC2 |
| [Java](https://mvnrepository.com/artifact/com.tinkerpop.gremlin/gremlin-java) | [Gremlin JavaDoc](https://tinkerpop.apache.org/javadocs/current/full/) | [Crear gráfico con Java](create-graph-java.md) | 3.2.0+ |
| [Node.js](https://www.npmjs.com/package/gremlin) | [Gremlin-JavaScript en GitHub](https://github.com/jbmusso/gremlin-javascript) | [Crear gráfico con Node.js](create-graph-nodejs.md) | 2.6.0|
| [Python](https://tinkerpop.apache.org/docs/3.3.1/reference/#gremlin-python) | [Gremlin-Python en GitHub](https://github.com/apache/tinkerpop/tree/master/gremlin-python) | [Crear gráfico con Python](create-graph-python.md) | 3.2.7 |
| [PHP](https://packagist.org/packages/brightzone/gremlin-php) | [Gremlin-PHP en GitHub](https://github.com/PommeVerte/gremlin-php) | [Crear gráfico con PHP](create-graph-php.md) | 3.1.0 |
| [Gremlin Console](https://tinkerpop.apache.org/downloads.html) | [Documentación de TinkerPop](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) |  [Crear gráfico con la consola de Gremlin](create-graph-gremlin-console.md) | 3.2.0 + |

## <a name="graph-database-design-considerations"></a>Consideraciones acerca del diseño de la base de datos de grafos

Durante el diseño del grafo, las decisiones de modelar una entidad como un vértice en sí mismo, en lugar de una propiedad de otras entidades de vértice, repercute en el rendimiento y el costo. El principal factor para tomar esta decisión es cómo se van a consultar los datos, así como la escalabilidad del propio modelo.

Tenga en cuenta las siguientes preguntas antes de planear el modelado de la entidad:

* ¿Cuáles son las entidades que deben recuperarse como vértices en la mayoría de mis consultas?

* ¿Cuál es la información que incluyo en el grafo que se agrega para fines de filtrado de datos?

* ¿Qué entidades son meras conexiones a otras entidades, cuyos valores se recuperan después?

* ¿Qué información necesita mi consulta recuperar y cuál es el cargo de la unidad de solicitud que se va a generar?

Por ejemplo, supongamos el siguiente diseño de grafo:

![Ejemplo de consideraciones acerca del diseño del grafo](./media/graph-introduction/graph-design-considerations-example.png)

* Dependiendo de las consultas, es posible que la relación distrito -> tienda se use de forma exclusiva para el filtrado de los vértices de la tienda. Por ejemplo, si las consultas tienen el formato tipo "obtener todas las tiendas que pertenecen a un distrito específico". Si este es el caso, merece la pena considerar la contracción de la entidad de distrito de un vértice propio en una propiedad del vértice de la tienda. 

* Este enfoque tiene la ventaja de reducir el costo de recuperar cada vértice de tienda al obtener tres objetos de grafo simultáneamente (distrito, distrito->tienda, tienda) en un solo vértice de tienda. Esto puede suponer mejoras de rendimiento, así como un costo reducido por consulta,

* dado que el vértice de tienda vincula dos entidades diferentes: empleados y producto. Hace que la tienda sea un vértice necesario. ya que proporciona más posibilidades para atravesar.  



## <a name="scenarios-that-can-use-gremlin-api"></a>Escenarios que pueden usar Gremlin API
Estos son algunos escenarios donde se puede usar la compatibilidad con gráficos de Azure Cosmos DB:

* Redes sociales

  Al combinar datos sobre los clientes y sus interacciones con otras personas, puede desarrollar experiencias personalizadas, predecir el comportamiento de los clientes o conectar a personas con otras que tengan intereses similares. Azure Cosmos DB puede utilizarse para administrar redes sociales y realizar un seguimiento de los datos y las preferencias de los usuarios.

* Motores de recomendaciones

  Este escenario se usa con frecuencia en el sector minorista. Al combinar información sobre productos, usuarios e interacciones de usuario, como comprar, examinar o clasificar un artículo, puede generar recomendaciones personalizadas. Azure Cosmos DB, con su latencia baja, escalado elástico y compatibilidad nativa con grafos, es ideal para el modelado de estas interacciones.

* Geoespaciales

  Muchas aplicaciones en telecomunicaciones, logística y programación de viajes necesitan encontrar una ubicación de interés dentro de un área o localizar la mejor ruta entre dos ubicaciones o la más corta. Azure Cosmos DB es una opción natural para estos problemas.

* Internet de las cosas

  Con la red y las conexiones entre dispositivos IoT modeladas como un grafo, se puede conocer mejor el estado de los dispositivos y recursos. También se puede ver de qué manera los cambios en una parte de la red pueden afectar a otra.

## <a name="next-steps"></a>Pasos siguientes
Para aprender más sobre la compatibilidad con grafos en Azure Cosmos DB:

* Empiece con el [tutorial de gráficos en Azure Cosmos DB](create-graph-dotnet.md).
* Obtenga más información sobre cómo [consultar grafos en Azure Cosmos DB mediante Gremlin](gremlin-support.md).
