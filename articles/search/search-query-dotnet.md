---
title: 'Consulta de datos en un índice de Azure Search en C# (SDK de .NET): Azure Search'
description: Ejemplo de código de C# para crear una consulta de búsqueda en Azure Search. Agregue parámetros de búsqueda para filtrar y ordenar los resultados de la búsqueda.
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/20/2019
ms.openlocfilehash: 1194407122123797c2564c96ac452b9582b017a4
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59264954"
---
# <a name="quickstart-3---query-an-azure-search-index-in-c"></a>Inicio rápido: 3 - Consulta de un índice de Azure Search en C#

En este artículo se muestra cómo consultar [un índice de Azure Search](search-what-is-an-index.md) mediante C# y el [SDK de .NET](https://aka.ms/search-sdk). La búsqueda de documentos en el índice se logra mediante la realización de estas tareas:

> [!div class="checklist"]
> * Crear un objeto [`SearchIndexClient`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) para conectarse a un índice de búsqueda con derechos de solo lectura.
> * Crear un objeto [`SearchParameters`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters?view=azure-dotnet) que contenga la definición de la búsqueda o del filtro.
> * Llamar al método `Documents.Search` en `SearchIndexClient` para enviar consultas a un índice.

## <a name="prerequisites"></a>Requisitos previos

[Cargar un índice de Azure Search](search-import-data-dotnet.md) con los datos de ejemplo de los hoteles.

Obtener una clave de consulta usada para tener acceso de solo lectura a los documentos. Hasta ahora, ha usado una clave de API de administrador para poder crear objetos y contenido en un servicio de búsqueda. Pero para poder realizar consultas en las aplicaciones, se recomienda encarecidamente usar una clave de consulta. Para obtener instrucciones, consulte [Creación de una clave de consulta](search-security-api-keys.md#create-query-keys).

## <a name="create-a-client"></a>Creación de un cliente
Cree una instancia de la clase `SearchIndexClient` para que pueda darle una clave de consulta para obtener acceso de solo lectura (en lugar de los derechos de acceso de escritura conferidos el `SearchServiceClient` usado en la lección anterior).

Esta clase tiene varios constructores. El que desea tiene el nombre del servicio de búsqueda, el nombre del índice y un objeto [`SearchCredentials`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchcredentials?view=azure-dotnet) como parámetros. `SearchCredentials` incluye su clave de API.

El código siguiente crea una nueva instancia de `SearchIndexClient` para el índice "hotels" mediante los valores del nombre de servicio de búsqueda y la clave de API que se almacenan en el archivo de configuración de la aplicación (`appsettings.json` en el caso de la [aplicación de ejemplo](https://aka.ms/search-dotnet-howto)):

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

`SearchIndexClient` tiene una propiedad [`Documents`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient.documents?view=azure-dotnet). Esta propiedad proporciona todos los métodos que necesita para consultar los índices de Azure Search.

## <a name="construct-searchparameters"></a>Construcción de SearchParameters
Buscar con el SDK de .NET es tan sencillo como llamar al método `Documents.Search` en su `SearchIndexClient`. Este método usa unos cuantos parámetros, incluido el texto de búsqueda, además de un objeto `SearchParameters` que se puede usar para refinar la consulta.

### <a name="types-of-queries"></a>Tipos de consultas
Los dos [tipos de consultas](search-query-overview.md#types-of-queries) principales que usará son `search` y `filter`. Una consulta `search` realiza búsquedas de uno o más términos en todos los campos *habilitados para búsqueda* del índice. Una consulta `filter` evalúa una expresión booleana en todos los campos *filtrables* de un índice. Puede usar los filtros y las búsquedas conjuntamente o por separado.

Tanto los filtros como las búsquedas se realizan con el método `Documents.Search` . Se puede pasar una consulta de búsqueda en el parámetro `searchText`, mientras que una expresión de filtro se puede pasar en la propiedad `Filter` de la clase `SearchParameters`. Para filtrar sin buscar, solo tiene que pasar `"*"` al parámetro `searchText`. Para buscar sin filtrar, simplemente deje la propiedad `Filter` sin establecer, o no pase ninguna instancia de `SearchParameters`.

### <a name="example-queries"></a>Consultas de ejemplo
El código de ejemplo siguiente muestra algunas formas de consultar el índice "hoteles" definido en [Creación de un índice de Azure Search en C#](search-create-index-dotnet.md#DefineIndex). Tenga en cuenta que los documentos que se devuelven con los resultados de la búsqueda son instancias de la clase `Hotel`, que se definió en [Importación de datos un índice de Azure Search en C#](search-import-data-dotnet.md#construct-indexbatch). El código de ejemplo usa un método `WriteDocuments` para enviar los resultados de búsqueda a la consola. Este método se describe en la sección siguiente.

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
Console.WriteLine("and return the hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take the top two results, and show only hotelName and ");
Console.WriteLine("lastRenovationDate:\n");

parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="handle-search-results"></a>Control de los resultados de la búsqueda
El método `Documents.Search` devuelve un objeto `DocumentSearchResult` que contiene los resultados de la consulta. En el ejemplo de la sección anterior, se usa un método llamado `WriteDocuments` para enviar los resultados de búsqueda a la consola:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Este es el aspecto de los resultados de las consultas de la sección anterior, suponiendo que el índice "hotels" contenga los datos de ejemplo:

```
Search the entire index for the term 'budget' and return only the hotelName field:

Name: Roach Motel

Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close to town hall and the river

Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search the entire index for the term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577
```

El código de ejemplo anterior envía los resultados de búsqueda a la consola. Del mismo modo, necesitará mostrar los resultados de búsqueda en su propia aplicación. Para ver un ejemplo de cómo se representan los resultados de una búsqueda en una aplicación web basada en MVC de ASP.NET, consulte el [proyecto DotNetSample](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) en GitHub.

## <a name="next-steps"></a>Pasos siguientes

Si aún no lo ha hecho, examine el código de ejemplo de [DotNetHowTo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) en GitHub, junto con [Uso de Azure Search desde una aplicación .NET ](search-howto-dotnet-sdk.md) para obtener descripciones detalladas del código de ejemplo. 