---
title: AutoSuggest de ejemplo para agregar los términos de la lista desplegable para un cuadro de búsqueda - Azure Search
description: Agregar entradas de consulta sugeridos mediante la creación de proveedores de sugerencias y formular las solicitudes que invocan las consultas sugeridas en Azure Search.
manager: cgronlun
author: heidisteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: heidist
ms.openlocfilehash: 1db085d61c60d303aaff5c3b0d16998805fcda90
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58373081"
---
# <a name="example-add-autosuggest-for-dropdown-query-selections"></a>Ejemplo: Agregar autosuggest para selecciones de la consulta de lista desplegable

Las entradas de término de búsqueda pueden incluir una lista desplegable de los términos de consulta sugeridos. Ha visto esta capacidad en los motores de búsqueda comercial, y puede implementar una experiencia similar en Azure Search con un [construcción del proveedor de sugerencias](index-add-suggesters.md) y una operación de sugerencias en una solicitud de consulta. En este artículo usa ejemplos para demostrar la formulación de una consulta de autosuggest, con un proveedor de sugerencias que ya ha definido. 

## <a name="rest-api"></a>API DE REST

Puede usar [Postman](search-fiddler.md) o [PowerShell](search-create-index-rest-api.md) y [API de REST](https://docs.microsoft.com/rest/api/searchservice/) probar este ejemplo, pero los resultados se devolverán como documentos JSON. Puede encontrar una experiencia más realista y visual mediante el [código de ejemplo para Autocompletar](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete).

### <a name="1---index-with-a-suggester-construct"></a>1 - el índice con una construcción de proveedor de sugerencias

AutoSuggest comienza con un [construcción del proveedor de sugerencias](index-add-suggesters.md) agregado a un índice, que se compone de campos que contribuyen a los valores de la lista desplegable. Dado el esquema siguiente, las consultas sugeridas se van a definir utilizando los valores de los campos de categoría y hotelName.

Suponiendo que los conjuntos de datos de ejemplo mínimo que se encuentra en las guías de inicio rápido, los nombres de hotel incluyen "Mantener la extravagante" y "Roach motel" y las categorías incluyen "Luxury" y "Presupuesto".

Si ya tiene el índice de hoteles, debe quitar y volver a crearla con el siguiente esquema. Este esquema agrega un proveedor de sugerencias. No olvide volver a cargar también los datos.

```json
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ],
  "suggesters": [
    {
      "name": "sg",
      "searchMode": "analyzingInfixMatching",
      "sourceFields": ["hotelName", "category"]
    }
  ]
}

```

### <a name="2---query-with-suggestions-operator"></a>2: consultar con el operador de sugerencias

Para usar un proveedor de sugerencias e invocar autosuggest, debe enviar un [API de sugerencias](https://docs.microsoft.com/rest/api/searchservice/suggestions) solicitud con GET o POST. En esta solicitud, el servicio de búsqueda busca coincidencias posibles una vez que se reciben los tres primeros caracteres. 

En el encabezado de solicitud, establezca **clave de api** a una clave de administrador o de consulta, y **Content-Type** en application/json. 

```http
GET https://mydemo.search.windows.net/indexes/hotels/docs/suggest?search=fan&&suggesterName=sg
```

Los exámenes de solicitud que se incluyen todos los campos en el proveedor de sugerencias (hotelName y categoría), devolviendo la respuesta siguiente:

```
{
    "@odata.context": "https://mydemo.search.windows.net/indexes('hotels')/$metadata#docs(*)",
    "value": [
        {
            "@search.text": "Fancy Stay",
            "hotelId": "1"
        }
    ]
}
```

Para entregar el resultado esperado, el código de cliente podría representar los resultados como lista desplegable bajo una barra de búsqueda.

## <a name="net-sdk-c"></a>SDK DE .NET (C#)

### <a name="1---index-with-a-suggester-construct"></a>1 - el índice con una construcción de proveedor de sugerencias

En el SDK. NET, use un [clase de proveedor de sugerencias](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet). Proveedor de sugerencias es una colección, pero puede tomar solamente un elemento.

```csharp
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels",
        Fields = FieldBuilder.BuildForType<Hotel>(),
        Suggesters = new List<Suggester>() {new Suggester()
            {
                Name = "sg",
                SourceFields = new string[] { "HotelId", "Category" }
            }}
    };

    serviceClient.Indexes.Create(definition);

}
```

### <a name="2---query-with-suggest-method"></a>2: consultar con sugerir (método)

El [DocumentsOperationsExtensions.Suggest método](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.suggest?view=azure-dotnet) se usa para devolver las cadenas de consulta sugeridos.

```csharp
public ActionResult Suggest(bool highlights, bool fuzzy, string term)
{
    InitSearch();

    // Call suggest API and return results
    SuggestParameters sp = new SuggestParameters()
    {
        UseFuzzyMatching = fuzzy,
        Top = 5
    };

    if (highlights)
    {
        sp.HighlightPreTag = "<b>";
        sp.HighlightPostTag = "</b>";
    }

    DocumentSuggestResult suggestResult = _indexClient.Documents.Suggest(term, "sg",sp);

    // Convert the suggest query results to a list that can be displayed in the client.
    List<string> suggestions = suggestResult.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = suggestions
    };
}
```

## <a name="see-also"></a>Vea también

+ [Explorar la API de REST mediante Postman](search-fiddler.md)
+ [Ejemplo: Autocomplete](search-autocomplete-tutorial.md)
+ [Agregar proveedores de sugerencias para un índice](index-add-suggesters.md)
+ [Agregue una clase de proveedores de sugerencias con .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet)
+ [Sugerencias de llamada con GET o POST (API de REST)](https://docs.microsoft.com/rest/api/searchservice/suggestions)
+ [Llamar a sugerencias mediante SuggestWithHttpMessagesAsync (. NET)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet-preview) o 