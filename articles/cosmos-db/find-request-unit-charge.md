---
title: Búsqueda del cargo de unidad de solicitud (RU) en Azure Cosmos DB
description: Sepa cómo encontrar el cargo de unidad de solicitud de cualquier operación que se ejecuta en un contenedor de Azure Cosmos.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: sample
ms.date: 03/21/2019
ms.author: thweiss
ms.openlocfilehash: e3175ee136057c695ceef3cd1976b447a529c803
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2019
ms.locfileid: "59053045"
---
# <a name="find-the-request-unit-ru-charge-in-azure-cosmos-db"></a>Búsqueda del cargo de unidad de solicitud (RU) en Azure Cosmos DB

En este artículo se presentan las distintas formas de encontrar el consumo de [unidades de solicitud](request-units.md) de cualquier operación que se ejecuta en un contenedor de Azure Cosmos. Actualmente, es posible medir este consumo con Azure Portal o por medio de la inspección de la respuesta enviada desde Azure Cosmos DB mediante uno de los SDK.

## <a name="core-api"></a>Core API

### <a name="use-the-azure-portal"></a>Uso de Azure Portal

Azure Portal permite actualmente buscar el cargo de solicitud de una única consulta SQL.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. [Cree una cuenta de Azure Cosmos DB](create-sql-api-dotnet.md#create-account) y suminístrele datos, o seleccione una cuenta existente que ya contenga datos.

1. Abra el panel **Data Explorer** y seleccione el contenedor en el que quiere trabajar.

1. Haga clic en **Nueva consulta SQL**.

1. Escriba una consulta válida y, luego, haga clic en **Ejecutar consulta**.

1. Haga clic en **Query Stats** (Estadísticas de consulta) para mostrar el cargo de solicitud real correspondiente a la solicitud que acaba de ejecutar.

![Captura de pantalla del cargo de solicitud de la consulta SQL en Azure Portal](./media/find-request-unit-charge/portal-sql-query.png)

### <a name="use-the-net-sdk-v2"></a>Uso de .NET SDK V2

Los objetos devueltos por [.NET SDK v2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/) (consulte [este inicio rápido](create-sql-api-dotnet.md) respecto a su uso) exponen una propiedad `RequestCharge`.

```csharp
ResourceResponse<Document> fetchDocumentResponse = await client.ReadDocumentAsync(
    UriFactory.CreateDocumentUri("database", "container", "itemId"),
    new RequestOptions
    {
        PartitionKey = new PartitionKey("partitionKey")
    });
var requestCharge = fetchDocumentResponse.RequestCharge;

StoredProcedureResponse<string> storedProcedureCallResponse = await client.ExecuteStoredProcedureAsync<string>(
    UriFactory.CreateStoredProcedureUri("database", "container", "storedProcedureId"),
    new RequestOptions
    {
        PartitionKey = new PartitionKey("partitionKey")
    });
requestCharge = storedProcedureCallResponse.RequestCharge;

IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri("database", "container"),
    "SELECT * FROM c",
    new FeedOptions
    {
        PartitionKey = new PartitionKey("partitionKey")
    }).AsDocumentQuery();
while (query.HasMoreResults)
{
    FeedResponse<dynamic> queryResponse = await query.ExecuteNextAsync<dynamic>();
    requestCharge = queryResponse.RequestCharge;
}
```

### <a name="use-the-java-sdk"></a>Uso del SDK de Java

Los objetos devueltos por el [SDK de Java](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) (consulte [este inicio rápido](create-sql-api-java.md) respecto a su uso) exponen un método `getRequestCharge()`.

```java
RequestOptions requestOptions = new RequestOptions();
requestOptions.setPartitionKey(new PartitionKey("partitionKey"));

Observable<ResourceResponse<Document>> readDocumentResponse = client.readDocument(String.format("/dbs/%s/colls/%s/docs/%s", "database", "container", "itemId"), requestOptions);
readDocumentResponse.subscribe(result -> {
    double requestCharge = result.getRequestCharge();
});

Observable<StoredProcedureResponse> storedProcedureResponse = client.executeStoredProcedure(String.format("/dbs/%s/colls/%s/sprocs/%s", "database", "container", "storedProcedureId"), requestOptions, null);
storedProcedureResponse.subscribe(result -> {
    double requestCharge = result.getRequestCharge();
});

FeedOptions feedOptions = new FeedOptions();
feedOptions.setPartitionKey(new PartitionKey("partitionKey"));

Observable<FeedResponse<Document>> feedResponse = client
    .queryDocuments(String.format("/dbs/%s/colls/%s", "database", "container"), "SELECT * FROM c", feedOptions);
feedResponse.forEach(result -> {
    double requestCharge = result.getRequestCharge();
});
```

### <a name="use-the-nodejs-sdk"></a>Uso del SDK de Node.js

Los objetos devueltos por el [SDK de Node.js](https://www.npmjs.com/package/@azure/cosmos) (consulte [este inicio rápido](create-sql-api-nodejs.md) respecto a su uso) exponen un subobjeto `headers` que asigna todos los encabezados devueltos por la API HTTP subyacente. El cargo de solicitud está disponible en la clave `x-ms-request-charge`.

```javascript
const item = await client
    .database('database')
    .container('container')
    .item('itemId', 'partitionKey')
    .read();
var requestCharge = item.headers['x-ms-request-charge'];

const storedProcedureResult = await client
    .database('database')
    .container('container')
    .storedProcedure('storedProcedureId')
    .execute({
        partitionKey: 'partitionKey'
    });
requestCharge = storedProcedureResult.headers['x-ms-request-charge'];

const query = client.database('database')
    .container('container')
    .items
    .query('SELECT * FROM c', {
        partitionKey: 'partitionKey'
    });
while (query.hasMoreResults()) {
    var result = await query.executeNext();
    requestCharge = result.headers['x-ms-request-charge'];
}
```

### <a name="use-the-python-sdk"></a>Uso del SDK de Python

El objeto `CosmosClient` del [SDK de Python](https://pypi.org/project/azure-cosmos/) (consulte [este inicio rápido](create-sql-api-python.md) respecto a su uso) expone un diccionario `last_response_headers` que asigna todos los encabezados devueltos por la API HTTP subyacente de la última operación ejecutada. El cargo de solicitud está disponible en la clave `x-ms-request-charge`.

```python
response = client.ReadItem('dbs/database/colls/container/docs/itemId', { 'partitionKey': 'partitionKey' })
request_charge = client.last_response_headers['x-ms-request-charge']

response = client.ExecuteStoredProcedure('dbs/database/colls/container/sprocs/storedProcedureId', None, { 'partitionKey': 'partitionKey' })
request_charge = client.last_response_headers['x-ms-request-charge']
```

## <a name="azure-cosmos-dbs-api-for-mongodb"></a>API de Azure Cosmos DB para MongoDB

El cargo de unidad de solicitud se expone mediante un [comando de base de datos](https://docs.mongodb.com/manual/reference/command/) personalizado llamado `getLastRequestStatistics`. Este comando devuelve un documento que contiene el nombre de la última operación ejecutada, el cargo de la solicitud y su duración.

### <a name="use-the-azure-portal"></a>Uso de Azure Portal

Azure Portal permite actualmente buscar el cargo de solicitud de una única consulta.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. [Cree una cuenta de Azure Cosmos DB](create-mongodb-dotnet.md#create-a-database-account) y suminístrele datos, o seleccione una cuenta existente que ya contenga datos.

1. Abra el panel **Data Explorer** y seleccione la colección en la que quiere trabajar.

1. Haga clic en **Nueva consulta**.

1. Escriba una consulta válida y, luego, haga clic en **Ejecutar consulta**.

1. Haga clic en **Query Stats** (Estadísticas de consulta) para mostrar el cargo de solicitud real correspondiente a la solicitud que acaba de ejecutar.

![Captura de pantalla del cargo de solicitud de la consulta de MongoDB en Azure Portal](./media/find-request-unit-charge/portal-mongodb-query.png)

### <a name="use-the-mongodb-net-driver"></a>Uso del controlador de .NET de MongoDB

Cuando se usa el [controlador oficial de .NET de MongoDB](https://docs.mongodb.com/ecosystem/drivers/csharp/) (consulte [este inicio rápido](create-mongodb-dotnet.md) respecto a su uso), se pueden ejecutar los comandos mediante una llamada al método `RunCommand` de un objeto `IMongoDatabase`. Este método requiere una implementación de la clase abstracta `Command<>`.

```csharp
class GetLastRequestStatisticsCommand : Command<Dictionary<string, object>>
{
    public override RenderedCommand<Dictionary<string, object>> Render(IBsonSerializerRegistry serializerRegistry)
    {
        return new RenderedCommand<Dictionary<string, object>>(new BsonDocument("getLastRequestStatistics", 1), serializerRegistry.GetSerializer<Dictionary<string, object>>());
    }
}

Dictionary<string, object> stats = database.RunCommand(new GetLastRequestStatisticsCommand());
double requestCharge = (double)stats["RequestCharge"];
```

### <a name="use-the-mongodb-java-driver"></a>Uso del controlador de .NET de Java

Cuando se usa el [controlador oficial de Java de MongoDB](http://mongodb.github.io/mongo-java-driver/) (consulte [este inicio rápido](create-mongodb-java.md) respecto a su uso), se pueden ejecutar los comandos mediante una llamada al método `runCommand` de un objeto `MongoDatabase`.

```java
Document stats = database.runCommand(new Document("getLastRequestStatistics", 1));
Double requestCharge = stats.getDouble("RequestCharge");
```

### <a name="use-the-mongodb-nodejs-driver"></a>Uso del controlador de Node.js de MongoDB

Cuando se usa el [controlador oficial de Node.js de MongoDB](https://mongodb.github.io/node-mongodb-native/) (consulte [este inicio rápido](create-mongodb-nodejs.md) respecto a su uso), se pueden ejecutar los comandos mediante una llamada al método `command` de un objeto `Db`.

```javascript
db.command({ getLastRequestStatistics: 1 }, function(err, result) {
    assert.equal(err, null);
    const requestCharge = result['RequestCharge'];
});
```

## <a name="cassandra-api"></a>Cassandra API

Al realizar operaciones en Cassandra API de Azure Cosmos DB, el cargo de unidad de solicitud se devuelve en la carga de entrada como un campo llamado `RequestCharge`.

### <a name="use-the-net-sdk"></a>Uso del SDK de .NET

Cuando se usa [.NET SDK](https://www.nuget.org/packages/CassandraCSharpDriver/) (consulte [este inicio rápido](create-cassandra-dotnet.md) respecto a su uso), se puede recuperar la carga de entrada en la propiedad `Info` de un objeto `RowSet`.

```csharp
RowSet rowSet = session.Execute("SELECT table_name FROM system_schema.tables;");
double requestCharge = BitConverter.ToDouble(rowSet.Info.IncomingPayload["RequestCharge"], 0);
```

### <a name="use-the-java-sdk"></a>Uso del SDK de Java

Cuando se usa el [SDK de Java](https://mvnrepository.com/artifact/com.datastax.cassandra/cassandra-driver-core) (consulte [este inicio rápido](create-cassandra-java.md) respecto a su uso), se puede recuperar la carga de entrada mediante la llamada al método `getExecutionInfo()` de un objeto `ResultSet`.

```java
ResultSet resultSet = session.execute("SELECT table_name FROM system_schema.tables;");
Double requestCharge = resultSet.getExecutionInfo().getIncomingPayload().get("RequestCharge").getDouble();
```

## <a name="gremlin-api"></a>API de Gremlin

### <a name="use-drivers-and-sdk"></a>Uso de controladores y SDK

A los encabezados devueltos por la API de Gremlin se les asignan atributos de estado personalizados que actualmente se exponen mediante el SDK de Java y .NET SDK de Gremlin. El cargo de solicitud está disponible en la clave `x-ms-request-charge`.

### <a name="use-the-net-sdk"></a>Uso del SDK de .NET

Cuando se usa el [SDK de Gremlin.NET](https://www.nuget.org/packages/Gremlin.Net/) (consulte [este inicio rápido](create-graph-dotnet.md) sobre su uso), los atributos de estado están disponibles en la propiedad `StatusAttributes` del objeto `ResultSet<>`.

```csharp
ResultSet<dynamic> results = client.SubmitAsync<dynamic>("g.V().count()").Result;
double requestCharge = (double)results.StatusAttributes["x-ms-request-charge"];
```

### <a name="use-the-java-sdk"></a>Uso del SDK de Java

Cuando se usa el [SDK de Java de Gremlin](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver) (consulte [este inicio rápido](create-graph-java.md) respecto a su uso), los atributos de estado se pueden recuperar mediante la llamada al método `statusAttributes()` del objeto `ResultSet`.

```java
ResultSet results = client.submit("g.V().count()");
Double requestCharge = (Double)results.statusAttributes().get().get("x-ms-request-charge");
```

## <a name="table-api"></a>Table API

El único SDK que actualmente devuelve el cargo de unidad de solicitud de las operaciones de tabla es [.NET SDK estándar](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) (consulte [este inicio rápido](create-table-dotnet.md) respecto a su uso). El objeto `TableResult` expone una propiedad `RequestCharge` que se rellena mediante el SDK cuando se usa con Table API de Azure Cosmos DB.

```csharp
CloudTable tableReference = client.GetTableReference("table");
TableResult tableResult = tableReference.Execute(TableOperation.Insert(new DynamicTableEntity("partitionKey", "rowKey")));
if (tableResult.RequestCharge.HasValue) // would be false when using Azure Storage Tables
{
    double requestCharge = tableResult.RequestCharge.Value;
}
```

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para saber cómo optimizar el consumo de unidades de solicitud:

* [Optimización del costo de rendimiento aprovisionado en Azure Cosmos DB](optimize-cost-throughput.md)
* [Optimización de los costos de consulta de Azure Cosmos DB](optimize-cost-queries.md)