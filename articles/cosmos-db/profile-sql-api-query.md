---
title: Obtener métricas de rendimiento y la ejecución de consultas SQL
description: Obtenga información sobre cómo recuperar las métricas de ejecución de consulta SQL y rendimiento de las consultas SQL de Azure Cosmos DB solicitudes de perfil.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: girobins
ms.openlocfilehash: b4017666956d0e01ea19781fb4f1ce2dde15fff5
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481570"
---
# <a name="get-sql-query-execution-metrics-and-analyze-query-performance-using-net-sdk"></a>Obtener métricas de ejecución de consulta SQL y analizar el rendimiento de consulta mediante el SDK de .NET

En este artículo se presenta cómo generar perfiles de rendimiento de las consultas SQL en Azure Cosmos DB. Esta generación de perfiles puede realizarse mediante `QueryMetrics` recuperan desde el SDK de .NET y se detalla aquí. [QueryMetrics](https://msdn.microsoft.com/library/microsoft.azure.documents.querymetrics.aspx) es un objeto fuertemente tipado con información sobre la ejecución de consultas de back-end. Estas métricas se documentan con más detalle en la [ajustar el rendimiento de las consultas](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-metrics) artículo.

## <a name="set-the-feedoptions-parameter"></a>Establezca el parámetro FeedOptions

Todas las sobrecargas [DocumentClient.CreateDocumentQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentquery.aspx) tomar opcional [FeedOptions](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.aspx) parámetro. Esta opción es lo que permite la ejecución de consultas se optimizan y con parámetros. 

Para recopilar las métricas de ejecución de consulta Sql, debe establecer el parámetro [PopulateQueryMetrics](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.populatequerymetrics.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.PopulateQueryMetrics) en el [FeedOptions](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.aspx) a `true`. Establecer `PopulateQueryMetrics` a true hará que para que la `FeedResponse` contendrá la correspondiente `QueryMetrics`. 

## <a name="get-query-metrics-with-asdocumentquery"></a>Obtiene las métricas de consulta con AsDocumentQuery()
Ejemplo de código siguiente muestra cómo recuperar las métricas cuando se usa [AsDocumentQuery()](https://msdn.microsoft.com/library/microsoft.azure.documents.linq.documentqueryable.asdocumentquery.aspx) método:

```csharp
// Initialize this DocumentClient and Collection
DocumentClient documentClient = null;
DocumentCollection collection = null;

// Setting PopulateQueryMetrics to true in the FeedOptions
FeedOptions feedOptions = new FeedOptions
{
    PopulateQueryMetrics = true
};

string query = "SELECT TOP 5 * FROM c";
IDocumentQuery<dynamic> documentQuery = documentClient.CreateDocumentQuery(Collection.SelfLink, query, feedOptions).AsDocumentQuery();

while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    
    // This dictionary maps the partitionId to the QueryMetrics of that query
    IReadOnlyDictionary<string, QueryMetrics> partitionIdToQueryMetrics = feedResponse.QueryMetrics;
    
    // At this point you have QueryMetrics which you can serialize using .ToString()
    foreach (KeyValuePair<string, QueryMetrics> kvp in partitionIdToQueryMetrics)
    {
        string partitionId = kvp.Key;
        QueryMetrics queryMetrics = kvp.Value;
        
        // Do whatever logging you need
        DoSomeLoggingOfQueryMetrics(query, partitionId, queryMetrics);
    }
}
```
## <a name="aggregating-querymetrics"></a>QueryMetrics agregación

En la sección anterior, tenga en cuenta que había varias llamadas a [ExecuteNextAsync](https://msdn.microsoft.com/library/azure/dn850294.aspx) método. Cada llamada devuelve un `FeedResponse` objeto que tiene un diccionario de `QueryMetrics`; una para todas las continuaciones de la consulta. El ejemplo siguiente muestra cómo se agregan estos `QueryMetrics` mediante LINQ:

```csharp
List<QueryMetrics> queryMetricsList = new List<QueryMetrics>();

while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    
    // This dictionary maps the partitionId to the QueryMetrics of that query
    IReadOnlyDictionary<string, QueryMetrics> partitionIdToQueryMetrics = feedResponse.QueryMetrics;
    queryMetricsList.AddRange(partitionIdToQueryMetrics.Values);
}

// Aggregate the QueryMetrics using the + operator overload of the QueryMetrics class.
QueryMetrics aggregatedQueryMetrics = queryMetricsList.Aggregate((curr, acc) => curr + acc);
Console.WriteLine(aggregatedQueryMetrics);
```

## <a name="grouping-query-metrics-by-partition-id"></a>Agrupar las métricas de consulta por Id. de partición

Puede agrupar el `QueryMetrics` mediante el identificador de partición. La agrupación por Id. de partición permite ver si una partición específica está causando problemas de rendimiento en comparación con otros usuarios. El ejemplo siguiente muestra cómo agrupar `QueryMetrics` con LINQ:

```csharp
List<KeyValuePair<string, QueryMetrics>> partitionedQueryMetrics = new List<KeyValuePair<string, QueryMetrics>>();
while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    
    // This dictionary is maps the partitionId to the QueryMetrics of that query
    IReadOnlyDictionary<string, QueryMetrics> partitionIdToQueryMetrics = feedResponse.QueryMetrics;
    partitionedQueryMetrics.AddRange(partitionIdToQueryMetrics.ToList());
}

// Now we are able to group the query metrics by partitionId
IEnumerable<IGrouping<string, KeyValuePair<string, QueryMetrics>>> groupedByQueryMetrics = partitionedQueryMetrics
    .GroupBy(kvp => kvp.Key);

// If we wanted to we could even aggregate the groupedby QueryMetrics
foreach(IGrouping<string, KeyValuePair<string, QueryMetrics>> grouping in groupedByQueryMetrics)
{
    string partitionId = grouping.Key;
    QueryMetrics aggregatedQueryMetricsForPartition = grouping
        .Select(kvp => kvp.Value)
        .Aggregate((curr, acc) => curr + acc);
    DoSomeLoggingOfQueryMetrics(query, partitionId, aggregatedQueryMetricsForPartition);
}
```

## <a name="linq-on-documentquery"></a>LINQ en DocumentQuery

También puede obtener el `FeedResponse` desde una consulta de LINQ mediante la `AsDocumentQuery()` método:

```csharp
IDocumentQuery<Document> linqQuery = client.CreateDocumentQuery(collection.SelfLink, feedOptions)
    .Take(1)
    .Where(document => document.Id == "42")
    .OrderBy(document => document.Timestamp)
    .AsDocumentQuery();
FeedResponse<Document> feedResponse = await linqQuery.ExecuteNextAsync<Document>();
IReadOnlyDictionary<string, QueryMetrics> queryMetrics = feedResponse.QueryMetrics;
```

## <a name="expensive-queries"></a>Consultas costosas

Puede capturar las unidades de solicitud consumidas por cada consulta para investigar consultas costosas o consultas que consumen un alto rendimiento. Puede obtener el cargo de solicitud mediante la [RequestCharge](https://msdn.microsoft.com/library/azure/dn948712.aspx) propiedad `FeedResponse`. Para obtener más información acerca de cómo obtener el cargo de solicitud mediante el portal de Azure y los diferentes SDK, vea [encontrar el cargo de unidad de solicitud](find-request-unit-charge.md) artículo.

```csharp
string query = "SELECT * FROM c";
IDocumentQuery<dynamic> documentQuery = documentClient.CreateDocumentQuery(Collection.SelfLink, query, feedOptions).AsDocumentQuery();

while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    double requestCharge = feedResponse.RequestCharge
    
    // Log the RequestCharge how ever you want.
    DoSomeLogging(requestCharge);
}
```

## <a name="get-the-query-execution-time"></a>Obtener el tiempo de ejecución de consulta

Al calcular el tiempo necesario para ejecutar una consulta del lado cliente, asegúrese de que solo incluyen el tiempo para llamar a la `ExecuteNextAsync` método y no en otras partes del código base. Estas llamadas le ayudarán a calcular el tiempo en que la ejecución de la consulta tardó tal como se muestra en el ejemplo siguiente:

```csharp
string query = "SELECT * FROM c";
IDocumentQuery<dynamic> documentQuery = documentClient.CreateDocumentQuery(Collection.SelfLink, query, feedOptions).AsDocumentQuery();
Stopwatch queryExecutionTimeEndToEndTotal = new Stopwatch();
while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    queryExecutionTimeEndToEndTotal.Start();
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    queryExecutionTimeEndToEndTotal.Stop();
}

// Log the elapsed time
DoSomeLogging(queryExecutionTimeEndToEndTotal.Elapsed);
```

## <a name="scan-queries-commonly-slow-and-expensive"></a>Analizar las consultas (normalmente lento y costosas)

Una consulta de recorrido hace referencia a una consulta que no se ha atendido por el índice, por lo que, muchos documentos se cargan antes de devolver el conjunto de resultados.

A continuación es un ejemplo de una consulta de análisis:

```sql
SELECT VALUE c.description 
FROM   c 
WHERE UPPER(c.description) = "BABYFOOD, DESSERT, FRUIT DESSERT, WITHOUT ASCORBIC ACID, JUNIOR"
```

Filtro de la consulta usa la función de sistema superior, que no se sirve desde el índice. Ejecutar esta consulta en una colección grande genera las siguientes métricas de consulta para la primera continuación:

```
QueryMetrics

Retrieved Document Count                 :          60,951
Retrieved Document Size                  :     399,998,938 bytes
Output Document Count                    :               7
Output Document Size                     :             510 bytes
Index Utilization                        :            0.00 %
Total Query Execution Time               :        4,500.34 milliseconds
  Query Preparation Times
    Query Compilation Time               :            0.09 milliseconds
    Logical Plan Build Time              :            0.05 milliseconds
    Physical Plan Build Time             :            0.04 milliseconds
    Query Optimization Time              :            0.01 milliseconds
  Index Lookup Time                      :            0.01 milliseconds
  Document Load Time                     :        4,177.66 milliseconds
  Runtime Execution Times
    Query Engine Times                   :          322.16 milliseconds
    System Function Execution Time       :           85.74 milliseconds
    User-defined Function Execution Time :            0.00 milliseconds
  Document Write Time                    :            0.01 milliseconds
Client Side Metrics
  Retry Count                            :               0
  Request Charge                         :        4,059.95 RUs
```

Tenga en cuenta los siguientes valores de la salida de las métricas de consulta:

```
Retrieved Document Count                 :          60,951
Retrieved Document Size                  :     399,998,938 bytes
```

Esta consulta carga 60,951 documentos, que suman 399,998,938 bytes. Al cargar este número de bytes da lugar a cargo de alto costo o la solicitud de la unidad. También lleva mucho tiempo para ejecutar la consulta, que es clara con la propiedad de tiempo total transcurrido:

```
Total Query Execution Time               :        4,500.34 milliseconds
```

Lo que significa que la consulta tardó 4.5 segundos en ejecutarse (y esto era solo una continuación).

Para optimizar esta consulta de ejemplo, evite el uso de superior en el filtro. En su lugar, cuando se crean o actualizan, documentos la `c.description` se deben insertar valores en todos los caracteres en mayúsculas. A continuación, se convierte en la consulta: 

```sql
SELECT VALUE c.description 
FROM   c 
WHERE c.description = "BABYFOOD, DESSERT, FRUIT DESSERT, WITHOUT ASCORBIC ACID, JUNIOR"
```

Esta consulta es ahora capaz se sirven desde el índice.

Para más información acerca de cómo ajustar el rendimiento de las consultas, consulte el [ajustar el rendimiento de las consultas](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-metrics) artículo.

## <a id="References"></a>Referencias

- [Especificación de SQL de Azure Cosmos DB](https://go.microsoft.com/fwlink/p/?LinkID=510612)
- [ANSI SQL 2011](https://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
- [JSON](https://json.org/)
- [LINQ](/previous-versions/dotnet/articles/bb308959(v=msdn.10)) 

## <a name="next-steps"></a>Pasos siguientes

- [Optimizar el rendimiento de las consultas](sql-api-query-metrics.md)
- [Introducción a la indexación](index-overview.md)
- [Ejemplos de .NET de Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
