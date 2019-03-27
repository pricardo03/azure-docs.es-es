---
title: 'Carga de datos en un índice de Azure Search en C# (SDK de .NET): Azure Search'
description: Obtenga información sobre cómo cargar datos en un índice de búsqueda de texto completo en Azure Search con ejemplo de código de C# y el SDK de .NET.
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/20/2019
ms.openlocfilehash: d2d54d1425bbb67a3f5ba1b6081a9f74ff87f4d6
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58286914"
---
# <a name="quickstart-2---load-data-to-an-azure-search-index-using-c"></a>Inicio rápido: 2 - Carga de datos en un índice de Azure Search mediante C#

En este artículo se muestra cómo importar datos en [un índice de Azure Search](search-what-is-an-index.md) mediante C# y [SDK de .NET](https://aka.ms/search-sdk). La inserción de documentos en el índice se logra mediante la realización de estas tareas:

> [!div class="checklist"]
> * Crear un objeto [`SearchIndexClient`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) para conectarse a un índice de búsqueda.
> * Crear un objeto [`IndexBatch`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexbatch?view=azure-dotnet) que contenga los documentos que se van a agregar, modificar o eliminar.
> * Llamar al método `Documents.Index` en `SearchIndexClient` para cargar documentos en un índice.

## <a name="prerequisites"></a>Requisitos previos

[Crear un índice de Azure Search](search-create-index-dotnet.md) y un objeto `SearchServiceClient`, como se muestra en ["Creación de un cliente"](search-create-index-dotnet.md#CreateSearchServiceClient).


## <a name="create-a-client"></a>Creación de un cliente
Para importar datos, se necesita una instancia de la clase `SearchIndexClient`. Existen varios enfoques para la creación de esta clase, entre los que se incluye el uso de la instancia de `SearchServiceClient` que ya se ha creado. 

Como se muestra en el ejemplo siguiente, puede usar la instancia de `SearchServiceClient` instancia y llamar a su método `Indexes.GetClient`. Este fragmento de código obtiene una instancia de `SearchIndexClient` para el índice denominado "hotels" de una instancia de `SearchServiceClient` denominada `serviceClient`.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

`SearchIndexClient` tiene una propiedad `Documents`. Esta propiedad proporciona todos los métodos necesarios para agregar, modificar, eliminar o consultar los documentos del índice.

> [!NOTE]
> En una aplicación de búsqueda típica, las consultas y los índices se controlan por separado. Aunque `Indexes.GetClient` resulta cómodo porque puede reutilizar objetos como `SearchCredentials`, un enfoque más sólido implica la creación de `SearchIndexClient` directamente, con el fin de que pueda pasar una clave de consulta, en lugar de una clave de administrador. Esta práctica está en consonancia con el [principio de privilegios mínimos](https://en.wikipedia.org/wiki/Principle_of_least_privilege) y le ayuda a proteger su aplicación. En el ejercicio siguiente construirá una instancia de `SearchIndexClient`. Para más información acerca de las claves, consulte [Create and manage api-keys for an Azure Search service](search-security-api-keys.md) (Creación y administración de claves de API para un servicio de Azure Search).
> 
> 

<a name="construct-indexbatch"></a>

## <a name="construct-indexbatch"></a>Construcción de IndexBatch

Para importar datos mediante el SDK. de NET, empaquete los datos en un objeto `IndexBatch`. `IndexBatch` encapsula una colección de objetos `IndexAction`, cada uno de los cuales contiene un documento y una propiedad que indica a Azure Search qué acción debe realizar en el documento (upload, merge, delete y mergeOrUpload). Para más información acerca de las acciones de indexación, consulte [Acciones de indexación: upload, merge, mergeOrUpload, delete](search-what-is-data-import.md#indexing-actions).

Si ya conoce las acciones que se van a realizar en los documentos, está listo para construir `IndexBatch`. El ejemplo siguiente muestra cómo crear un lote con otras acciones. El ejemplo usa una clase personalizada denominada `Hotel` que se asigna a un documento del índice "hoteles".

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "1",
                BaseRate = 199.0,
                Description = "Best hotel in town",
                DescriptionFr = "Meilleur hôtel en ville",
                HotelName = "Fancy Stay",
                Category = "Luxury",
                Tags = new[] { "pool", "view", "wifi", "concierge" },
                ParkingIncluded = false,
                SmokingAllowed = false,
                LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                Rating = 5,
                Location = GeographyPoint.Create(47.678581, -122.131577)
            }),
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "2",
                BaseRate = 79.99,
                Description = "Cheapest hotel in town",
                DescriptionFr = "Hôtel le moins cher en ville",
                HotelName = "Roach Motel",
                Category = "Budget",
                Tags = new[] { "motel", "budget" },
                ParkingIncluded = true,
                SmokingAllowed = true,
                LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                Rating = 1,
                Location = GeographyPoint.Create(49.678581, -122.131577)
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close to town hall and the river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

En este caso, se van a usar `Upload`, `MergeOrUpload` y `Delete` como acciones de búsqueda, tal como especifican los métodos a los que se llama en la clase `IndexAction`.

Se supone que este índice "hoteles" de ejemplo ya está relleno con varios documentos. Observe que no tuvimos que especificar todos los campos posibles del documento al utilizar `MergeOrUpload` y que solo especificamos la clave del documento (`HotelId`) al usar `Delete`.

Además, tenga en cuenta que en las solicitudes de indexación individuales solo se puede incluir un máximo de 1000 documentos.

> [!NOTE]
> En este ejemplo, aplicamos acciones diferentes a documentos diferentes. Si deseara realizar las mismas acciones en todos los documentos del lote, en lugar de llamar a `IndexBatch.New`, podría utilizar los restantes métodos estáticos de `IndexBatch`. Por ejemplo, podría crear lotes mediante una llamada a `IndexBatch.Merge`, `IndexBatch.MergeOrUpload` o `IndexBatch.Delete`. Estos métodos toman una colección de documentos (objetos del tipo `Hotel` en este ejemplo), en lugar de objetos `IndexAction`.
> 
> 

## <a name="call-documentsindex"></a>Llamada a Documents.Index
Ahora que tiene un objeto `IndexBatch` inicializado, puede enviarlo al índice, para lo que debe realizar una llamada a `Documents.Index` en el objeto `SearchIndexClient`. En el ejemplo siguiente se muestra cómo llamar a `Index`, así como algunos pasos adicionales que es preciso llevar a cabo:

```csharp
try
{
    indexClient.Documents.Index(batch);
}
catch (IndexBatchException e)
{
    // Sometimes when your Search service is under load, indexing will fail for some of the documents in
    // the batch. Depending on your application, you can take compensating actions like delaying and
    // retrying. For this simple demo, we just log the failed document keys and continue.
    Console.WriteLine(
        "Failed to index some of the documents: {0}",
        String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
}

Console.WriteLine("Waiting for documents to be indexed...\n");
Thread.Sleep(2000);
```

Tenga en cuenta el `try`/`catch` que rodea la llamada al método `Index`. El bloque catch controla un caso de error importante de indexación. Si su servicio Azure Search no logra indizar algunos de los documentos del lote, aparece una `IndexBatchException` producida por `Documents.Index`. Esto puede suceder si indiza documentos mientras el servicio está sobrecargado. **Recomendamos encarecidamente controlar este caso de forma explícita en el código.**  Puede retrasar la indización de los documentos que dieron error y volver a intentarlo; puede crear un registro y continuar, como hace el ejemplo, o puede adoptar otro enfoque según los requisitos de coherencia de datos de la aplicación.

Por último, el código del ejemplo anterior se retrasa dos segundos. La indización ocurre de manera asincrónica en el servicio Azure Search, por lo que la aplicación de ejemplo debe esperar unos momentos para asegurarse de que los documentos estén disponibles para la búsqueda. Retrasos así solo suelen ser necesarios en las pruebas, demostraciones y aplicaciones de ejemplo.

Para más información acerca del procesamiento de documentos, consulte ["Gestión de documentos del SDK de .NET"](search-howto-dotnet-sdk.md#how-dotnet-handles-documents).


## <a name="next-steps"></a>Pasos siguientes
Tras rellenar el índice de Azure Search, el siguiente paso es iniciar la emisión de consultas para buscar documentos. 

> [!div class="nextstepaction"]
> [Consulta de un índice de Azure Search en C#](search-query-dotnet.md)
