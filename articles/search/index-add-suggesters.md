---
title: 'Incorporación de consultas de escritura automática a un índice: Azure Search'
description: Para habilitar acciones de escritura automática en Azure Search, puede crear proveedores de sugerencias y formular las solicitudes que invocan los términos de consulta de sugerencias automáticas o de autocompletar.
ms.date: 05/02/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
ms.author: brjohnst
manager: nitinme
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
ms.openlocfilehash: 73cfdb6a4185689a6485f55a4f6bdd1e7e3b14be
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69648852"
---
# <a name="add-suggesters-to-an-index-for-typeahead-in-azure-search"></a>Incorporación de proveedores de sugerencias a un índice de escritura automática en Azure Search

Un **proveedor de sugerencias** es una construcción en un [índice de Azure Search](search-what-is-an-index.md) que admite una experiencia de "búsqueda al escribir". Contiene una lista de campos para los que desea habilitar las entradas de consulta de escrituras automáticas. Dentro de un índice, el mismo proveedor de sugerencias es compatible con uno o con ambas variantes de escritura automática: *Autocompletar* completa el término o frase que esté escribiendo, y *sugerencias* proporciona una breve lista de resultados. 

La siguiente captura de pantalla ilustra ambos tipos de características de escritura automática. En esta página de búsqueda de Xbox, los elementos de Autocompletar llevan a una nueva página de resultados de búsqueda para esa consulta, mientras que las sugerencias son los resultados reales que llevan a una página de juego en particular. Puede limitar la acción de autocompletar para un elemento en una barra de búsqueda o proporcionar una lista como se muestra aquí. Para obtener sugerencias, puede exponer cualquier parte de un documento que mejor describa el resultado.

![Comparación visual de Autocompletar y consultas sugeridas](./media/index-add-suggesters/visual-comparison-suggest-complete.png "Visual comparison of autocomplete and suggested queries")

Para implementar estos comportamientos en Azure Search, hay un componente de consulta e índice. 

+ El componente del índice es un proveedor de sugerencias. Puede usar el portal, las API REST o el SDK de .NET para crear un proveedor de sugerencias. 

+ El componente de consulta es una acción especificada en la solicitud de consulta (una acción de sugerencia o de autocompletar). 

La compatibilidad de búsqueda como al escribir está habilitada por cada campo. Puede implementar ambos comportamientos de escritura automática dentro de la misma solución de búsqueda si desea una experiencia similar a la indicada en la captura de pantalla. El destino de ambas solicitudes son la colección de *documentos* de índice específico y las respuestas se devuelven después de que un usuario haya proporcionado al menos una cadena de entrada de tres caracteres.

## <a name="create-a-suggester"></a>Creación de un proveedor de sugerencias

Aunque un proveedor de sugerencias tiene varias propiedades, es principalmente una colección de campos para los que va a habilitar la experiencia de escritura automática. Por ejemplo, una aplicación de viajes desea habilitar la búsqueda con escritura anticipada de destinos, ciudades y atracciones. Por lo tanto, estos tres campos deberían estar en la colección de campos.

Para crear un proveedor de sugerencias, agregue uno a un esquema de índice. Puede tener un solo proveedor de sugerencias para cada índice (en concreto, un proveedor de sugerencias de la colección de proveedores de sugerencias). 

### <a name="use-the-rest-api"></a>Uso de la API de REST

En la API REST, puede agregar proveedores de sugerencias con [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) o con [Update Index](https://docs.microsoft.com/rest/api/searchservice/update-index). 

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
Después de crear un proveedor de sugerencias, agregue la [API Suggestions](https://docs.microsoft.com/rest/api/searchservice/suggestions) o [la API Autocomplete](https://docs.microsoft.com/rest/api/searchservice/autocomplete) a la lógica de consulta para invocar la característica.

### <a name="use-the-net-sdk"></a>Uso del SDK de .NET

En C#, defina un [objeto Suggester](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet). `Suggesters` es una colección, pero puede tomar solamente un elemento. 

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

Algunos puntos clave que hay que tener en cuenta acerca de los proveedores de sugerencias es que hay un nombre (a los proveedores de sugerencias se les hace referencia por su nombre en una solicitud), un searchMode (actualmente, solo uno, "analyzingInfixMatching") y la lista de campos para la que está habilitada la escritura automática. 

Las propiedades que definen a un proveedor de sugerencias se incluyen las siguientes:

|Propiedad      |DESCRIPCIÓN      |
|--------------|-----------------|
|`name`        |El nombre del proveedor de sugerencias. Use el nombre del proveedor de sugerencias al llamar a la [API REST Suggestions](https://docs.microsoft.com/rest/api/searchservice/suggestions) o a la [API REST Autocomplete](https://docs.microsoft.com/rest/api/searchservice/autocomplete).|
|`searchMode`  |La estrategia que se usa para buscar las frases candidatas. El único modo que se admite actualmente es `analyzingInfixMatching`, que establece una correspondencia flexible de frases al principio o en medio de las oraciones.|
|`sourceFields`|Una lista de uno o más campos que son el origen del contenido para obtener sugerencias. Solo los campos de tipo `Edm.String` y `Collection(Edm.String)` pueden ser orígenes para obtener sugerencias. Solo se pueden usar los campos que no tienen un analizador de lenguaje personalizado establecido.<p/>Especifique solo los campos que se prestan a una respuesta adecuada y esperada, ya sea una cadena completa en una barra de búsqueda o una lista desplegable.<p/>Un nombre de hotel es buen candidato porque tiene precisión. Los campos detallados, como las descripciones y los comentarios, son demasiado densos. De forma similar, los campos repetitivos, como las categorías y las etiquetas, son menos eficaces. En los ejemplos, se incluye "categoría" de todos modos para demostrar que puede incluir varios campos. |

#### <a name="analysis-of-sourcefields-in-a-suggester"></a>Análisis de SourceFields en un proveedor de sugerencias

Azure Search analiza el contenido del campo para habilitar las consultas sobre términos individuales. Los proveedores de sugerencias requieren que se indexen los prefijos además de los términos completos, lo cual requiere un análisis adicional de los campos de origen. Las configuraciones de los analizadores personalizados pueden combinar cualquiera de los distintos tokenizadores y filtros, a menudo de formas que harían imposible la generación de los prefijos necesarios para las sugerencias. Por esta razón, **Azure Search impide que los campos con analizadores personalizados se incluyan en un proveedor de sugerencias**.

> [!NOTE] 
>  El enfoque recomendado para solucionar la limitación anterior es usar dos campos independientes para el mismo contenido. Esto permitirá que uno de los campos tenga proveedores de sugerencias y el otro se pueda configurar con una configuración personalizada del analizador.

## <a name="when-to-create-a-suggester"></a>Cuándo crear un proveedor de sugerencias

Para evitar una regeneración de índices, un proveedor de sugerencias y los campos especificados en `sourceFields` se debe crear al mismo tiempo.

Si agrega un proveedor de sugerencias a un índice existente, donde se incluyen los campos existentes en `sourceFields`, cambia fundamentalmente la definición de campo y se requiere una recompilación. Para más información, consulte [Cómo recompilar un índice de Azure Search](search-howto-reindex.md).

## <a name="how-to-use-a-suggester"></a>Cómo usar un proveedor de sugerencias

Como se indicó anteriormente, puede usar un proveedor de sugerencias para consultas sugeridas, autocompletar o ambos. 

A un proveedor de sugerencias se le hace referencia en la solicitud junto con la operación. Por ejemplo, en una llamada GET REST, especifique `suggest` o `autocomplete` en la colección de documentos. Después de crear un proveedor de sugerencias, use la [API Suggestions](https://docs.microsoft.com/rest/api/searchservice/suggestions) o la [API Autocomplete](https://docs.microsoft.com/rest/api/searchservice/autocomplete) en la lógica de consulta.

En . NET, use [SuggestWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet) o [AutocompleteWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.autocompletewithhttpmessagesasync?view=azure-dotnet&viewFallbackFrom=azure-dotnet).

Para obtener un ejemplo que muestra ambas solicitudes, consulte [Ejemplo para agregar Autocompletar y sugerencias en Azure Search](search-autocomplete-tutorial.md).

## <a name="sample-code"></a>Código de ejemplo

El ejemplo [DotNetHowToAutocomplete](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete) contiene tanto código de C# como de Java, y se muestra la construcción de un proveedor de sugerencias, las consultas sugeridas, autocompletar y la navegación por facetas. 

Usa un servicio de espacio aislado de Azure Search y un índice cargado previamente, por lo que todo lo que debe hacer es presionar F5 para ejecutarlo. No se necesita ninguna suscripción o inicio de sesión.

## <a name="next-steps"></a>Pasos siguientes

Se recomienda el siguiente ejemplo para ver cómo se formulan las solicitudes.

> [!div class="nextstepaction"]
> [Ejemplos de sugerencias y de autocompletar](search-autocomplete-tutorial.md) 
