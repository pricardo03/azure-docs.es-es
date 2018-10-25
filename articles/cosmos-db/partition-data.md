---
title: Creación de particiones y escalado horizontal en Azure Cosmos DB | Microsoft Docs
description: Obtenga información sobre cómo funciona la creación de particiones en Azure Cosmos DB, cómo configurar la creación de particiones y las claves de partición y cómo seleccionar la clave de partición correcta para su aplicación.
services: cosmos-db
author: aliuy
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/26/2018
ms.author: andrl
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 968651e2bd06d54c8b735bf2418e0d84b94f315d
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "49078575"
---
# <a name="partition-and-scale-in-azure-cosmos-db"></a>Partición y escalado en Azure Cosmos DB

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) es un servicio de bases de datos multimodelo, con distribución global, diseñado para ayudarle a lograr un rendimiento rápido y predecible. Se escala sin problemas junto con la aplicación. Este artículo proporciona información general sobre cómo funcionan las particiones para todos los modelos de datos de Azure Cosmos DB. También describe cómo puede configurar los contenedores de Azure Cosmos DB para escalar de forma eficaz las aplicaciones.

Azure Cosmos DB admite los siguientes tipos de contenedores a través de todas las API:

- **Contenedor fijo**: estos contenedores pueden almacenar una base de datos de gráficos de hasta 10 GB de tamaño con un máximo de 10 000 unidades de solicitud por segundo asignadas. Para crear un contenedor fijo no es necesario especificar una propiedad de clave de partición en los datos.

- **Contenedor ilimitado**: estos contenedores se pueden escalar de forma automática para almacenar un gráfico de más de 10 GB mediante particionamiento horizontal. Cada partición almacenará 10 GB y los datos se equilibrarán automáticamente según la **clave de partición especificada**, que será un parámetro obligatorio cuando se usa un contenedor ilimitado. Este tipo de contenedor puede almacenar un tamaño de datos prácticamente ilimitado y permite hasta 100 000 unidades de solicitud por segundo, o más ([póngase en contacto con soporte técnico](https://aka.ms/cosmosdbfeedback?subject=Cosmos%20DB%20More%20Throughput%20Request)).

## <a name="partitioning-in-azure-cosmos-db"></a>Creación de particiones en Azure Cosmos DB
Azure Cosmos DB proporciona contenedores para el almacenamiento de datos llamados colecciones (para el documento), grafos o tablas. Los contenedores son recursos lógicos y pueden abarcar uno o varios servidores o particiones físicos. Azure Cosmos DB determina el número de particiones en función del tamaño de almacenamiento y del rendimiento aprovisionado en un contenedor o un conjunto de contenedores. 

### <a name="physical-partition"></a>Partición física

Una partición *física* es una cantidad fija de almacenamiento reservado respaldado por SSD y combinado con una cantidad variable de recursos de proceso (CPU y memoria). Todas las particiones físicas se replican para lograr una alta disponibilidad. Cada conjunto de contenedores puede compartir una o varias particiones físicas. Azure Cosmos DB se encarga de administrar las particiones físicas de principio a fin, por lo que no tendrá que escribir ningún código complejo ni administrar ninguna partición. Los contenedores de Azure Cosmos DB son ilimitados en términos de almacenamiento y rendimiento. Las particiones físicas son un concepto interno de Azure Cosmos DB y son transitorias. Azure Cosmos DB escalará automáticamente el número de particiones físicas según la carga de trabajo. Por lo tanto, no debe correlacionar el diseño de la base de datos en función del número de particiones físicas. Por el contrario, debería asegurarse de elegir la clave de partición correcta que determina las particiones lógicas. 

### <a name="logical-partition"></a>Partición lógica

Una partición *lógica* es aquella dentro de una partición física que almacena todos los datos asociados a un valor de clave de partición única. Varias particiones lógicas pueden terminar en la misma partición física. En el siguiente diagrama, un único contenedor tiene tres particiones lógicas. Cada partición lógica almacena los datos para una clave de partición, LAX, AMS y MEL, respectivamente. Cada una de las particiones lógicas LAX, AMS y MEL no puede crecer más allá del límite máximo de las particiones lógicas de 10 GB. 

![Partición de los recursos](./media/introduction/azure-cosmos-db-partitioning.png) 

Cuando un contenedor cumple los [requisitos previos de la creación de particiones](#prerequisites), esta creación es completamente transparente para la aplicación. Azure Cosmos DB controla la distribución de los datos en las distintas particiones físicas y lógicas, así como el enrutamiento de las solicitudes de consulta a la partición correcta. 

## <a name="how-does-partitioning-work"></a>¿Cómo funciona la creación de particiones?

Cada documento debe tener una *clave de partición* y una *clave de fila* que lo identifiquen de forma única. La clave de partición ejerce de partición lógica para los datos y proporciona a Azure Cosmos DB un límite natural para distribuir datos entre las particiones físicas. **Los datos de una única partición lógica deben residir dentro de una misma partición física, y la administración de particiones físicas está administrada por Azure Cosmos DB**. 

En resumen, la creación de particiones funciona en Azure Cosmos DB de la siguiente manera:

* Va a aprovisionar un conjunto de contenedores de Azure Cosmos DB con un rendimiento de **T** RU/s (solicitudes por segundo).  
* En un segundo plano, Azure Cosmos DB aprovisiona las particiones físicas necesarias para atender **T** solicitudes por segundo. Si **T** es superior al rendimiento máximo por partición física **t**, Azure Cosmos DB aprovisiona **N = T/t** particiones físicas. El valor de rendimiento máximo por partición(t) se configura mediante Azure Cosmos DB, y dicho valor se asigna en función del rendimiento aprovisionado total y de la configuración de hardware que se utiliza.  
* Azure Cosmos DB asigna el espacio de claves de los hash de claves de partición uniformemente entre las **N** particiones físicas. Por lo tanto, el número de particiones lógicas que cada partición física hospeda es **1/N** * valores de número de claves de partición.  
* Si una partición física **p** alcanza su límite de almacenamiento, Azure Cosmos DB divide **p** en dos nuevas particiones físicas, **p1** y **p2**. Lo que hace es distribuir los valores correspondientes a la mitad de las claves, aproximadamente, a cada una de las nuevas particiones. Esta operación de división es completamente invisible para la aplicación. Si una partición física alcanza su límite de almacenamiento y todos los datos en la partición física pertenecen a la misma clave de partición lógica, no tiene lugar la operación de división. Esto se debe a que todos los datos de una clave de partición lógica única deben residir en la misma partición física. En este caso, se debe emplear una estrategia de claves de partición diferente.  
* Si el rendimiento que aprovisiona es mayor que **t*N**, Azure Cosmos DB divide una o varias de sus particiones físicas para respaldar el rendimiento más alto.

La semántica para claves de partición es ligeramente diferente a fin de coincidir con la semántica de cada API, como se muestra en la siguiente tabla:

| API | Clave de partición | Clave de fila |
| --- | --- | --- |
| SQL | Ruta de acceso de clave de partición personalizada | `id` fija | 
| MongoDB | Clave de partición personalizada  | `_id` fija | 
| Gremlin | Propiedad de clave de partición personalizada | `id` fija | 
| Tabla | `PartitionKey` fija | `RowKey` fija | 

Azure Cosmos DB usa la creación de particiones basada en hash. Al escribir un elemento, Azure Cosmos DB aplica un algoritmo hash al valor de clave de partición y usa el resultado al que se ha aplicado un algoritmo hash para determinar en qué partición se va a almacenar el elemento. 

> [!NOTE]
> Azure Cosmos DB almacena todos los elementos con la misma clave de partición en la misma partición física. 

## <a name="best-practices-when-choosing-a-partition-key"></a>Procedimientos recomendados al elegir una clave de partición

La elección de la clave de partición es una decisión importante que debe realizarse en el tiempo de diseño. Elija un nombre de propiedad que tenga una amplia gama de valores e incluso patrones de acceso. Se recomienda tener una clave de partición con un gran número de valores distintos (por ejemplo, centenares o miles). Le permite distribuir la carga de trabajo uniformemente entre estos valores. Una clave de partición idónea es la que aparece con frecuencia como filtro en sus consultas y que tiene suficiente cardinalidad para asegurar que la solución sea escalable.

Si una partición física alcanza su límite de almacenamiento y los datos de la partición tienen la misma clave de partición, Azure Cosmos DB devuelve el mensaje que indica que *la clave de partición alcanzó el tamaño máximo de 10 GB* y la partición no se divide; por tanto, elegir una clave de partición es una decisión muy importante. 

Elija una clave de partición con que se cumpla lo siguiente:

* La distribución de almacenamiento es uniforme en todas las claves.  
* Elija una clave de partición que distribuya los datos uniformemente por las particiones.

  Es una buena idea comprobar cómo se distribuyen los datos en las particiones. Para comprobar la distribución de datos en el portal, vaya a la cuenta de Azure Cosmos DB, haga clic en **Métrica**, en la sección **Supervisión** y luego haga clic en la pestaña **Almacenamiento** para ver cómo se distribuyen los datos en las diferentes particiones físicas.

  ![Partición de los recursos](./media/partition-data/partitionkey-example.png)

  La imagen de la izquierda anterior muestra el resultado de una clave de partición incorrecta y la imagen de la derecha anterior muestra el resultado cuando se elige una clave de partición correcta. En la imagen de la izquierda, puede ver que los datos no se distribuyen uniformemente en las particiones. Debe procurar elegir una clave de partición que distribuya los datos como se ve en la imagen de la derecha.

* Optimice las consultas para obtener los datos dentro de los límites de una partición siempre que sea posible. Una estrategia de partición óptima se alinea con los patrones de consulta. Las consultas que obtienen datos de una sola partición proporcionan el mejor rendimiento. Las consultas que se invocan con alta simultaneidad se pueden enrutar de manera eficaz si se incluye la clave de partición en el predicador de filtro.  

* Por lo general, se recomienda elegir una clave de partición con una mayor cardinalidad, porque habitualmente genera una mejor distribución y escalabilidad. Por ejemplo, se puede formar una clave sintética si se concatenan valores de varias propiedades para aumentar la cardinalidad.  

Cuando elige una clave con las consideraciones anteriores, no es necesario que se preocupe por el número de particiones ni por cuánto rendimiento se asigna por partición física, porque Azure Cosmos DB escala de manera horizontal el número de particiones físicas y también puede escalar las particiones individuales en caso de ser necesario.

## <a name="prerequisites"></a>Requisitos previos para la creación de particiones

Los contenedores de Azure Cosmos DB se pueden crear como fijos o ilimitados. Los contenedores de tamaño fijo tienen un límite máximo de 10 GB y un rendimiento de 10 000 RU/s. Para crear un contenedor como ilimitado, debe especificar una clave de partición y una capacidad de proceso mínima de 1000 RU/s. También puede crear contenedores de Azure Cosmos DB que compartan el rendimiento. En tales casos, cada contenedor debe especificar una clave de partición y pueden crecer de forma ilimitada. 

A continuación se detallan los requisitos previos a tener en cuenta para la creación de particiones y el escalado:

* Al crear un contenedor (por ejemplo, una colección, un grafo o una tabla) en Azure Portal, seleccione la opción **Ilimitada** para la capacidad de almacenamiento a fin de aprovechar las ventajas del escalado ilimitado. Para que las particiones físicas se dividan automáticamente en **p1** y **p2**, tal y como se describe en el artículo [Cómo funciona la creación de particiones](#how-does-partitioning-work), el contenedor debe crearse con un rendimiento de 1000 RU/s o más (o compartirlo entre un conjunto de contenedores) y se debe proporcionar una clave de partición. 

* Si crea un contenedor con el rendimiento inicial mayor o igual que 1000 RU/s y proporciona una clave de partición, puede beneficiarse del escalado ilimitado sin realizar ningún cambio en el contenedor. Lo que significa que, aunque cree un contenedor **fijo**, si el contenedor inicial se crea con un rendimiento de al menos 1000 RU/s y se especifica una clave de partición, el contenedor actúa como un contenedor ilimitado.

* Todos los contenedores configurados para compartir el rendimiento como parte de un conjunto de contenedores se tratan como contenedores **ilimitados**.

Si ha creado un contenedor de tamaño **Fijo** sin ninguna clave de partición o con una capacidad de procesamiento menor que 1000 RU/s, el contenedor no se podrá escalar automáticamente. Para migrar los datos de un contenedor fijo a un contenedor ilimitado, debe usar la [herramienta de migración de datos](import-data.md) o la [biblioteca de fuente de cambios](change-feed.md). 

## <a name="designing-for-partitioning"></a> Creación de una tabla de particiones 
Puede usar Azure Portal o la CLI de Azure para crear contenedores y escalarlos en cualquier momento. En esta sección se muestra cómo crear contenedores y cómo especificar la capacidad de proceso aprovisionada y la clave de partición que utiliza cada API.


### <a name="sql-api"></a>API DE SQL
En el ejemplo siguiente se muestra cómo crear un contenedor (una colección) mediante SQL API. 

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

Puede leer un elemento (documento) mediante el método `GET` de la API de REST o mediante `ReadDocumentAsync` en uno de los SDK.

```csharp
// Read document. Needs the partition key and the ID to be specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

Para más información, consulte [Creación de particiones en Azure Cosmos DB con SQL API](sql-api-partition-data.md).

### <a name="mongodb-api"></a>MongoDB API
Con la API de MongoDB, puede crear una colección particionada mediante la herramienta, el controlador o el SDK que prefiera. En este ejemplo, usamos el shell de Mongo para crear la colección.

En el shell de Mongo:

```
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
Resultados:

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

### <a name="table-api"></a>Table API

Para crear una tabla con Table API, utilice el método `CreateIfNotExists`. 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists(throughput: 800);
```

La capacidad de proceso aprovisionada se establece como un argumento de `CreateIfNotExists`. La clave de partición se crea implícitamente como el valor `PartitionKey`. 

Puede recuperar una sola entidad con el código siguiente:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
Para más información, consulte [Desarrollo con Table API](tutorial-develop-table-dotnet.md).

### <a name="gremlin-api"></a>API de Gremlin

Con la API de Gremlin, puede usar Azure Portal o la CLI de Azure para crear un contenedor que representa un grafo. Como Azure Cosmos DB tiene varios modelos, también puede usar una de las otras API para crear y escalar el contenedor de grafos.

> [!NOTE]
> `/id` y `/label` no se admiten como claves de partición para un contenedor en la API de Gremlin.

Puede leer cualquier vértice o borde mediante el id. y la clave de partición en Gremlin. Por ejemplo, para un grafo con la región ("EE. UU.") como clave de partición y "Seattle" como clave de fila, puede encontrar un vértice mediante la siguiente sintaxis:

```
g.V(['USA', 'Seattle'])
```

Puede hacer referencia a un borde mediante la clave de partición y la clave de fila.

```
g.E(['USA', 'I5'])
```

Para más información, consulte [Using a partitioned graph in Azure Cosmos DB](graph-partitioning.md) (Uso de un grafo con particiones en Azure Cosmos DB).

## <a name="form-partition-key-by-concatenating-multiple-fields"></a>Clave de partición del formulario mediante la concatenación de varios campos

También puede formar una clave de partición mediante la concatenación y relleno de múltiples valores de propiedades en una única propiedad artificial "partitionKey" del elemento. Estas claves se conocen como claves sintéticas.

Por ejemplo, tiene un documento similar al siguiente:

```json
{
"deviceId": "abc-123",
"date": 2018
}
```

Una opción es establecer partitionKey en /deviceId o /date. Si desea formar una clave de partición en el identificador del dispositivo y la fecha. Concatene estos dos valores en una propiedad "partitionKey" artificial y establezca la clave de partición en /partitionKey.

```json
{
"deviceId": "abc-123",
"date": 2018,
"partitionKey": "abc-123-2018"
}
```

En escenarios en tiempo real puede tener miles de documentos, por lo que debe definir la lógica del lado cliente para concatenar los valores en una clave sintética, insertar la clave sintética en los documentos y, a continuación, utilizarla para especificar la clave de partición.

## <a name="designing-for-scale"></a> Diseño para el escalado
Para escalar de forma eficaz con Azure Cosmos DB, debe elegir una buena clave de partición al crear el contenedor. Existen dos consideraciones principales cuando se trata de elegir una buena clave de partición:

* **Límites y transacciones de consultas**. La elección de una clave de partición debe equilibrar la necesidad de usar transacciones frente al requisito de distribuir las entidades en varias claves de partición para garantizar que la solución sea escalable. Por una parte, se puede establecer la misma clave de partición para todos los elementos, pero esta opción puede limitar la escalabilidad de su solución. Por otra parte, puede asignar una clave de partición única para cada elemento. Esta opción es muy escalable, pero no le permitirá utilizar transacciones entre documentos mediante los procedimientos y desencadenadores almacenados. Una clave de partición idónea le permite usar consultas eficaces y tiene suficiente cardinalidad para asegurar que la solución es escalable. 
* **Sin cuellos de botella de rendimiento y almacenamiento**. Es importante elegir una propiedad que permita la distribución de las escrituras en diversos valores definidos. Las solicitudes a la misma clave de partición no pueden exceder la capacidad de proceso aprovisionada asignada a una partición, y tendrán una velocidad limitada. Por tanto, es importante elegir una clave de partición que no dé como resultado "zonas activas" dentro de la aplicación. Puesto que todos los datos de una clave de una sola partición deben almacenarse en una partición, también se recomienda evitar claves de partición que tengan grandes volúmenes de datos para el mismo valor. 

Echemos un vistazo a algunos escenarios en el mundo real, así como a las buenas claves de partición de cada uno de ellos:
* Si implementa un back-end de perfil de usuario, el *identificador de usuario* es una buena elección como clave de partición.
* Si almacena datos de IoT, como el estado del dispositivo, un *identificador de dispositivo* es una buena elección como clave de partición.
* Si usa Azure Cosmos DB para registrar datos de series temporales, el *nombre de host* o el *identificador de proceso* son una buena elección como clave de partición.
* Si tiene una arquitectura multiempresa, el *identificador del inquilino* es una buena elección como clave de partición.

En algunos casos de uso, como IoT y los perfiles de usuario, la clave de partición podría ser igual que su *identificador* (clave de documento). En otros, como en los datos de series temporales, la clave de partición será diferente del *identificador*.

### <a name="partitioning-and-loggingtime-series-data"></a>Creación de particiones, registro y datos de serie temporal
Uno de los casos de uso más comunes de Azure Cosmos DB es el registro y la telemetría. Es importante elegir una buena clave de partición en este escenario, ya que es posible que necesite leer y escribir grandes volúmenes de datos. La elección de una clave de partición depende de las tasas de lectura y escritura y de los tipos de consultas que espera ejecutar. Estas son algunas sugerencias sobre cómo elegir una buena clave de partición:

* Si su caso implica una pequeña tasa de escrituras que se acumulan durante un largo período de tiempo, y necesita consultar por intervalos de marcas de tiempo con otros filtros, use un resumen de la marca de tiempo. Por ejemplo, un buen enfoque es usar la fecha como una clave de partición. Esto le permite consultar todos los datos para una fecha dada desde una sola partición. 
* Si la carga de trabajo tiene muchas escrituras, lo que es muy común en este escenario, utilice una clave de partición que no esté basada en la marca de tiempo. Como tal, Azure Cosmos DB puede distribuir y escalar escrituras uniformemente entre varias particiones. En este caso, el *nombre de host*, el *identificador de proceso*, el *identificador de actividad* u otra propiedad con cardinalidad elevada, son una buena elección. 
* Otro enfoque es un enfoque híbrido, en el que tiene varios contenedores, uno para cada día o mes, y la clave de partición es una propiedad pormenorizada como *nombre de host*. Este enfoque tiene la ventaja de que puede establecer un rendimiento diferente para cada contenedor o conjunto de contenedores en función de la franja de tiempo, la escala y las necesidades de rendimiento. Por ejemplo, un contenedor para el mes actual se puede aprovisionar con un mayor rendimiento ya que sirve lecturas y escrituras. Los meses anteriores se pueden aprovisionar con menor rendimiento ya que solo atienden lecturas.

### <a name="partitioning-and-multitenancy"></a>Particiones y aplicaciones multiinquilino
Si implementa una aplicación multiinquilino mediante Azure Cosmos DB, hay dos diseños populares que se deben considerar: *una clave de partición por inquilino* y *un contenedor por inquilino*. Estas son las ventajas y desventajas de cada uno:

* **Una clave de partición por inquilino**. En este modelo, los inquilinos se colocan en un solo contenedor. Las consultas e inserciones de un único inquilino pueden realizarse en una sola partición. También se puede implementar la lógica transaccional en todos los elementos que pertenecen a un inquilino. Puesto que varios inquilinos comparten un contenedor, se puede utilizar mejor el almacenamiento y el rendimiento aprovisionado mediante la agrupación de los recursos para los inquilinos en un solo contenedor, en lugar de aprovisionar para cada inquilino. El inconveniente es que no es posible aislar el procesamiento por inquilino. El aumento del rendimiento para garantizarlo se aplicará a todo el contenedor con todos los inquilinos frente a los aumentos previstos para un inquilino individual.
* **Un contenedor por inquilino**. En este modelo, cada inquilino tiene su propio contenedor y puede reservar una capacidad de proceso garantizada por inquilino. Este modelo es más rentable para las aplicaciones multiinquilino con un número pequeño de inquilinos.

También puede usar un enfoque de híbrido que coloca juntos los inquilinos pequeños y aísla los inquilinos grandes en su propio contenedor.

## <a name="next-steps"></a>Pasos siguientes
En este artículo, hemos proporcionado información general sobre los conceptos y procedimientos recomendados para la creación de particiones y el escalado con Azure Cosmos DB. 

* Información sobre el [procesamiento aprovisionado en Azure Cosmos DB](request-units.md).
* Información sobre la [distribución global en Azure Cosmos DB](distribute-data-globally.md).



