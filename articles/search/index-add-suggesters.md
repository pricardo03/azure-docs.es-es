---
title: Agregar consultas de escritura anticipada en un índice - Azure Search
description: Habilitar acciones de consulta anticipadas en Azure Search mediante la creación de proveedores de sugerencias y formular las solicitudes que invocan Autocompletar o autosuggested los términos de consulta.
ms.date: 03/22/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 5a46575f6e8a0b05b65dbf49c70bddb570b514b2
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58497440"
---
# <a name="add-suggesters-to-an-index-for-typeahead-in-azure-search"></a>Agregar proveedores de sugerencias para un índice de escritura anticipada en Azure Search

Un **sugerencias** es una construcción en un [índice de Azure Search](search-what-is-an-index.md) que admite una experiencia de "búsqueda como-al escribir". Contiene una lista de campos para el que desea habilitar las entradas de la consulta de escritura anticipada. Hay dos variantes de escritura anticipada: *Autocompletar* se completa el término o frase que vaya escribiendo, *sugerencias* proporciona una breve lista de resultados. 

En esta página de búsqueda de Xbox, los elementos de Autocompletar llevan a una nueva página de resultados de búsqueda para esa consulta, mientras que las sugerencias son los resultados reales que llevan a una página de juego en particular. Puede limitar Autocompletar para un elemento en una barra de búsqueda o proporcionar una lista como se muestra aquí. Para obtener sugerencias, puede exponer cualquier parte de un documento que mejor describe el resultado.

![Comparación visual de Autocompletar y las consultas sugeridas](./media/index-add-suggesters/visual-comparison-suggest-complete.png "comparación Visual de Autocompletar y las consultas sugeridas")

Para implementar estos comportamientos en Azure Search, hay un componente de consulta e índice. 

+ En un índice, agregue un proveedor de sugerencias. Puede usar el portal, API de REST o SDK de .NET para crear un proveedor de sugerencias. 

+ En una consulta, especifique una sugerencia o sutocomplete acción. 

> [!Important]
> Autocompletar está actualmente en versión preliminar, disponible en versión preliminar de las API de REST y SDK de .NET y no se admite para las aplicaciones de producción. 

Compatibilidad de búsqueda como-al escribir está habilitada por campo. Puede implementar ambos comportamientos de escritura anticipada en la misma solución de búsqueda si desea una experiencia similar a la indicada en la captura de pantalla. Destino de ambas solicitudes el *documentos* colección de índice específica y respuestas se devuelven después de que un usuario ha proporcionado al menos una cadena de entrada de tres caracteres.

## <a name="create-a-suggester"></a>Creación de un proveedor de sugerencias

Aunque un proveedor de sugerencias tiene varias propiedades, es principalmente una colección de campos para el que va a habilitar una experiencia de escritura anticipada. Por ejemplo, una aplicación de viajes desea habilitar la búsqueda con escritura anticipada de destinos, ciudades y atracciones. Por lo tanto, los tres campos iría en la colección de campos.

Para crear un proveedor de sugerencias, agregue una a un esquema de índice. Puede tener un proveedor de sugerencias en un índice (en concreto, un proveedor de sugerencias en la colección de proveedores de sugerencias). 

### <a name="use-the-rest-api"></a>Uso de la API de REST

En la API de REST, puede agregar proveedores de sugerencias a través de [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) o [Actualizar índice](https://docs.microsoft.com/rest/api/searchservice/update-index). 

  ```json
  {
    "name": "hotels",
    "fields": [
      . . .
    ],
    "suggesters": [
      {
        "name": "sg",
        "searchMode": "analyzingInfixMatching",
        "sourceFields": ["hotelName", "category"]
      }
    ],
    "scoringProfiles": [
      . . .
    ]
  }
  ```
Después de crea un proveedor de sugerencias, agregue el [API de sugerencias](https://docs.microsoft.com/rest/api/searchservice/suggestions) o [API de Autocompletar](https://docs.microsoft.com/rest/api/searchservice/autocomplete) en la lógica de consulta para invocar la característica.

### <a name="use-the-net-sdk"></a>Uso del SDK de .NET

En C#, defina un [clase de proveedor de sugerencias](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet). Proveedor de sugerencias es una colección, pero puede tomar solamente un elemento.

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

## <a name="property-reference"></a>Referencia de propiedades

Puntos clave a tener en cuenta acerca de proveedores de sugerencias es que hay un nombre (proveedores de sugerencias se hace referencia por nombre en una solicitud), un searchMode (actualmente, solo uno, "analyzingInfixMatching") y la lista de campos para el que está habilitada la escritura anticipada. 

Las propiedades que definen a un proveedor de sugerencias se incluyen las siguientes:

|Propiedad      |DESCRIPCIÓN      |
|--------------|-----------------|
|`name`        |El nombre del proveedor de sugerencias. Utilice el nombre del proveedor de sugerencias cuando se llama a la [API de REST de sugerencias](https://docs.microsoft.com/rest/api/searchservice/suggestions) o [Autocompletar REST API (versión preliminar)](https://docs.microsoft.com/rest/api/searchservice/autocomplete).|
|`searchMode`  |La estrategia que se usa para buscar las frases candidatas. El único modo que se admite actualmente es `analyzingInfixMatching`, que establece una correspondencia flexible de frases al principio o en medio de las oraciones.|
|`sourceFields`|Una lista de uno o más campos que son el origen del contenido para obtener sugerencias. Solo los campos de tipo `Edm.String` y `Collection(Edm.String)` pueden ser orígenes para obtener sugerencias. Solo se pueden usar los campos que no tienen un analizador de lenguaje personalizado establecido.<p/>Especifique solo los campos que se prestan a una respuesta adecuada y esperada, ya sea una cadena completa en una barra de búsqueda o una lista desplegable.<p/>Un nombre de hotel es una buena candidata porque tiene la precisión. Los campos detallados, como descripciones y comentarios son demasiado densos. De forma similar, los campos repetitivos, como las categorías y etiquetas, son menos eficaces. En los ejemplos, se incluye "categoría" de todos modos para demostrar que puede incluir varios campos. |

## <a name="when-to-create-a-suggester"></a>Cuándo se debe crear un proveedor de sugerencias

Para evitar una regeneración de índice, un proveedor de sugerencias y los campos especificados en `sourceFields` se debe crear al mismo tiempo.

Si agrega un proveedor de sugerencias para un índice existente, donde se incluyen los campos existentes en `sourceFields`, cambia fundamentalmente la definición de campo y se requiere una recompilación. Para obtener más información, consulte [cómo volver a generar un índice de Azure Search](search-howto-reindex.md).

## <a name="how-to-use-a-suggester"></a>Cómo usar un proveedor de sugerencias

Como se indicó anteriormente, puede usar un proveedor de sugerencias para consultas sugeridas, Autocompletar o ambos. 

Se hace referencia a un proveedor de sugerencias en la solicitud junto con la operación. Por ejemplo, en una llamada GET REST, especifique `suggest` o `autocomplete` en la colección de documentos. Para REST, una vez creado un proveedor de sugerencias, use el [API de sugerencias](https://docs.microsoft.com/rest/api/searchservice/suggestions) o [Autocompletar API (versión preliminar)](https://docs.microsoft.com/rest/api/searchservice/autocomplete) en la lógica de consulta.

Para. NET, use [SuggestWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet-preview) o [AutocompleteWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.autocompletewithhttpmessagesasync?view=azure-dotnet-preview&viewFallbackFrom=azure-dotnet).

Para obtener un ejemplo que muestra ambas solicitudes, consulte [ejemplo para agregar Autocompletar y sugerencias en Azure Search](search-autocomplete-tutorial.md).

## <a name="sample-code"></a>Código de ejemplo

El [DotNetHowToAutocomplete](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete) ejemplo contiene dos C# y el código de Java y se muestra la construcción de un proveedor de sugerencias, las consultas sugeridas, Autocompletar y navegación por facetas. 

Usa un espacio aislado de servicio de búsqueda de Azure y un índice cargado previamente por lo que todo lo que debe hacer es presionar F5 para ejecutarlo. Ninguna suscripción o inicio de sesión es necesario.

## <a name="next-steps"></a>Pasos siguientes

Se recomienda el siguiente ejemplo para ver cómo se formulan las solicitudes.

> [!div class="nextstepaction"]
> [Ejemplo de consulta de autocompletado (versión preliminar)](search-autocomplete-tutorial.md) 
