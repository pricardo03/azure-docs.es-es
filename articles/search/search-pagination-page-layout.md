---
title: Procedimientos para trabajar con los resultados de búsqueda
titleSuffix: Azure Cognitive Search
description: Estructure y ordene los resultados de búsqueda, obtenga el número de documentos e incorpore la navegación por el contenido a los resultados de búsqueda de Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/24/2020
ms.openlocfilehash: c32e58a43b5409fd9f8ede536167d185270c6a22
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721581"
---
# <a name="how-to-work-with-search-results-in-azure-cognitive-search"></a>Procedimientos para trabajar con los resultados de búsqueda en Azure Cognitive Search
Este artículo contiene instrucciones para implementar los elementos estándar de una página de resultados de búsqueda, como los recuentos totales, la recuperación de documentos, los criterios de ordenación y la funcionalidad de navegación. Las opciones relacionadas con la página que aportan datos o información a los resultados de búsqueda se especifican mediante las solicitudes [Buscar documento](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) que se envían al servicio Azure Cognitive Search. 

En la API REST, las solicitudes incluyen un comando GET, una ruta de acceso y parámetros de consulta que informan al servicio de lo que se está solicitando y de cómo formular la respuesta. En el SDK de .NET, la API equivalente es la [clase DocumentSearchResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.documentsearchresult-1).

Para generar rápidamente una página de búsqueda para el cliente, explore estas opciones:

+ Use el [generador de aplicaciones](search-create-app-portal.md) del portal para crear una página HTML con una barra de búsqueda, navegación por facetas y área de resultados.
+ Siga el tutorial [Creación de la primera aplicación en C#](tutorial-csharp-create-first-app.md) para crear un cliente funcional.

Varios ejemplos de código que puede encontrar aquí incluyen una interfaz de front-end web: [Aplicación de demostración de trabajos de la ciudad de Nueva York](https://azjobsdemo.azurewebsites.net/), [código de ejemplo de JavaScript con un sitio de demostración activo](https://github.com/liamca/azure-search-javascript-samples) y [CognitiveSearchFrontEnd](https://github.com/LuisCabrer/CognitiveSearchFrontEnd).

> [!NOTE]
> Una solicitud válida incluye una serie de elementos, como una dirección URL del servicio y la ruta de acceso, el verbo HTTP, `api-version`, etc. Para mayor brevedad, hemos acortado los ejemplos para resaltar solo la sintaxis que resulta relevante para la paginación. Para más información sobre la sintaxis de las solicitudes, consulte [API REST de Azure Cognitive Search](https://docs.microsoft.com/rest/api/searchservice).
>

## <a name="total-hits-and-page-counts"></a>Total de resultados y recuentos de página

Muestra el número total de resultados devueltos por una consulta y, a continuación, devolver los resultados en bloques más pequeños, es fundamental para casi todas las páginas de búsqueda.

![][1]

En Azure Cognitive Search se usan los parámetros `$count`, `$top` y `$skip` para devolver estos valores. En el ejemplo siguiente, se muestra una solicitud de muestra del total de resultados de un índice llamado "online-catalog", que se devuelve como `@odata.count`:

    GET /indexes/online-catalog/docs?$count=true

Recuperar documentos en grupos de 15 y mostrar también el total de resultados, comenzando por la primera página:

    GET /indexes/online-catalog/docs?search=*&$top=15&$skip=0&$count=true

Paginar resultados requiere `$top` y `$skip`, donde `$top` especifica cuántos elementos se devuelven en un lote y `$skip` especifica cuántos elementos se omiten. En el ejemplo siguiente, cada página muestra los siguientes 15 elementos, indicado por los saltos incrementales en el parámetro `$skip` .

    GET /indexes/online-catalog/docs?search=*&$top=15&$skip=0&$count=true

    GET /indexes/online-catalog/docs?search=*&$top=15&$skip=15&$count=true

    GET /indexes/online-catalog/docs?search=*&$top=15&$skip=30&$count=true

## <a name="layout"></a>Diseño

En una página de resultados de búsqueda, puede ser deseable mostrar una imagen en miniatura, un subconjunto de campos y un vínculo a una página de producto completa.

 ![][2]

En Azure Cognitive Search se usa `$select` y una [solicitud de Search API](https://docs.microsoft.com/rest/api/searchservice/search-documents) para implementar esta experiencia.

Para devolver un subconjunto de campos con un diseño en mosaico:

    GET /indexes/online-catalog/docs?search=*&$select=productName,imageFile,description,price,rating

Las imágenes y los archivos multimedia no se pueden buscar directamente y se deben almacenar en otra plataforma de almacenamiento, como Almacenamiento de blobs de Azure, para reducir los costes. En el índice y los documentos, defina un campo que almacene la dirección URL del contenido externo. Después puede utilizar el campo como referencia de imagen. La dirección URL de la imagen debe estar en el documento.

Para recuperar una página de descripción de producto para un evento **onClick** , use [Buscar documento](https://docs.microsoft.com/rest/api/searchservice/Lookup-Document) para pasar la clave del documento que se va a recuperar. El tipo de datos de la clave es `Edm.String`. En este ejemplo, es *246810*.

    GET /indexes/online-catalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a>Ordenar por relevancia, clasificación o precio

A menudo, el orden predeterminado se basa en la relevancia, pero es habitual poner a disposición de los clientes otros criterios de ordenación para que puedan reorganizar rápidamente los resultados existentes en un orden diferente.

 ![][3]

En Azure Cognitive Search, la ordenación se basa en la expresión `$orderby` en todos los campos indexados como `"Sortable": true.` Las cláusulas `$orderby` son expresiones OData. Para más información sobre la sintaxis, consulte [Sintaxis de expresiones de OData para filtros y cláusulas OrderBy](query-odata-filter-orderby-syntax.md).

La relevancia está estrechamente asociada con perfiles de puntuación. Puede utilizar la puntuación predeterminada, que se basa en el análisis de texto y las estadísticas para ordenar todos los resultados, con las puntuaciones más altas destinadas a documentos con más coincidencias de un término de búsqueda o con coincidencias más importantes.

Los criterios de ordenación alternativos se suelen asociar a eventos **onClick** que llaman a un método que compila el criterio de ordenación. Por ejemplo, con este elemento de página:

 ![][4]

Deberá crear un método que acepte la opción de ordenación seleccionada como entrada y devuelva una lista ordenada de los criterios asociados a esa opción.

 ![][5]

> [!NOTE]
> Aunque la puntuación predeterminada es suficiente para muchos escenarios, se recomienda basar la relevancia en un perfil de puntuación personalizado. Un perfil personalizado de puntuación le ofrece una forma de aumentar los elementos que son más útiles para su negocio. Para más información, consulte [Incorporación de perfiles de puntuación](index-add-scoring-profiles.md).
>

## <a name="hit-highlighting"></a>Resaltado de referencias

Puede aplicar formato a los términos coincidentes de los resultados de búsqueda, lo que facilita la detección de la coincidencia. Se proporcionan instrucciones de resaltado de referencias en la [solicitud de consulta](https://docs.microsoft.com/rest/api/searchservice/search-documents). 

El formato se aplica a las consultas de términos completos. Las consultas sobre términos parciales, como la búsqueda aproximada o la búsqueda con caracteres comodín, que dan como resultado la expansión de la consulta en el motor, no pueden usar el resaltado de referencias.

```http
POST /indexes/hotels/docs/search?api-version=2019-05-06 
    {  
      "search": "something",  
      "highlight": "Description"  
    }
```



## <a name="faceted-navigation"></a>Navegación por facetas

La navegación de búsqueda es habitual en una página de resultados; a menudo se encuentra en un lado o en la parte superior de una página. En Azure Cognitive Search, la navegación por facetas proporciona una búsqueda autodirigida basada en filtros predefinidos. Consulte [Navegación por facetas en Azure Cognitive Search](search-faceted-navigation.md) para más información.

## <a name="filters-at-the-page-level"></a>Filtros en el nivel de página

Si el diseño de la solución incluye páginas de búsqueda dedicadas para determinados tipos de contenido (por ejemplo, una aplicación comercial en línea que enumera los departamentos en la parte superior de la página), puede insertar una [expresión de filtro](search-filters.md) junto con un evento **onClick** para abrir una página en un estado específico aplicado por un filtro previo.

Puede enviar un filtro con o sin expresión de búsqueda. Por ejemplo, la siguiente solicitud filtrará por el nombre de la marca y devolverá solamente los documentos que coincidan con él.

    GET /indexes/online-catalog/docs?$filter=brandname eq 'Microsoft' and category eq 'Games'

Consulte [Búsqueda de documentos (API de Azure Cognitive Search)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) para más información sobre las expresiones `$filter`.

## <a name="see-also"></a>Consulte también

- [API REST de Azure Cognitive Search](https://docs.microsoft.com/rest/api/searchservice)
- [Operaciones de índice](https://docs.microsoft.com/rest/api/searchservice/Index-operations)
- [Operaciones del documento](https://docs.microsoft.com/rest/api/searchservice/Document-operations)
- [Navegación por facetas en Azure Cognitive Search](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png
