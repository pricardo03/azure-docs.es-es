---
title: Aspectos básicos de las consultas en Azure Search | Microsoft Docs
description: Conceptos básicos para la creación de una consulta de búsqueda en Azure Search, utilizando parámetros para filtrar, seleccionar y ordenar resultados.
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/27/2018
ms.openlocfilehash: 4650ad89850f32ae5e83a7ac1cd5eac096b180ed
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366453"
---
# <a name="query-fundamentals-in-azure-search"></a>Aspectos básicos de las consultas en Azure Search

Una definición de consulta en Azure Search es una especificación completa de una solicitud que incluye las siguientes partes: el punto de conexión de la dirección URL del servicio, el índice de destino, la clave de API utilizada para autenticar la solicitud, la versión de API y una cadena de consulta. 

La cadena de consulta consta de los parámetros definidos en [API de documentos de búsqueda](https://docs.microsoft.com/rest/api/searchservice/search-documents). Especialmente importante es el parámetro **search=**, que puede no estar definido (`search=*`), pero suele estar compuesto por términos, frases y operadores de manera similar al ejemplo siguiente:

```
"search": "seattle townhouse +\"lake\""
```

Otros parámetros se usan para dirigir el procesamiento de consultas o para mejorar la respuesta. Los parámetros se utilizan para, por ejemplo, limitar el ámbito de búsqueda a campos específicos, definir un modo de búsqueda para modular el sesgo entre precisión y recuperación y añadir un recuento para llevar el control de los resultados. 

```
{  
    "search": "seattle townhouse* +\"lake\"",  
    "searchFields": "description, city",  
    "searchMode": "all",
    "count": "true", 
    "queryType": "simple" 
 } 
```

Aunque la definición de la consulta es fundamental, el esquema de índice es igualmente importante al especificar operaciones permitidas campo por campo. Durante el desarrollo del índice, los atributos de los campos determinan las operaciones permitidas. Por ejemplo, para que un campo se pueda tener en cuenta en una búsqueda de texto completo y aparezca en los resultados de búsqueda, debe estar marcado como *buscable* y *recuperable*.

En el momento de la consulta, la ejecución se realiza siempre en un índice, que se autentica mediante una clave de API proporcionada en la solicitud. No es posible combinar índices ni crear estructuras de datos personalizadas o temporales como destino de la consulta.  

Los resultados de la consulta se transmiten como documentos JSON en la API REST, aunque si usa API. NET, la serialización está integrada. Puede moldear los resultados mediante la definición de parámetros en la consulta, seleccionando campos específicos para el resultado.

En resumen, la sustancia de la solicitud de consulta especifica el ámbito y las operaciones: qué campos incluir en la búsqueda, qué campos incluir en el conjunto de resultados, si se deben ordenar o filtrar, etc. Una consulta sin especificar se ejecuta en todos los campos de búsqueda como una operación de búsqueda de texto completo, devolviendo un conjunto de resultados sin puntuar en orden aleatorio.

<a name="types-of-queries"></a>

## <a name="types-of-queries-search-and-filter"></a>Tipos de consultas: búsqueda y filtrado

Azure Search ofrece muchas opciones para crear consultas extremadamente eficaces. Los dos tipos principales de consultas que usará son `search` y `filter`. 

+ `search` busca uno o más términos en todos los campos *habilitados para búsqueda* del índice, y funciona tal y como cabría esperar de un motor de búsqueda como Google o Bing. Los ejemplos de la introducción utilizan el parámetro `search`.

+ Las consultas `filter` evalúan una expresión booleana en todos los campos *filtrables* de un índice. A diferencia de `search`, una consulta `filter` busca el contenido exacto de un campo, incluidas las mayúsculas y minúsculas en campos de cadena.

Puede usar las funciones de búsqueda y filtro conjuntamente o por separado. Un filtro independiente, sin una cadena de consulta, resulta útil cuando la expresión de filtro puede definir completamente los documentos de interés. Sin una cadena de consulta, no hay ningún análisis lingüístico ni léxico, ninguna puntuación y ninguna clasificación. Tenga en cuenta que la cadena de búsqueda está vacía.

```
POST /indexes/nycjobs/docs/search?api-version=2017-11-11  
    {  
      "search": "",
      "filter": "salary_frequency eq 'Annual' and salary_range_from gt 90000",
      "count": "true"
    }
```

Si los usa conjuntamente, el filtro se aplica primero a todo el índice y, después, se realiza la búsqueda en los resultados del filtro. Por lo tanto, los filtros pueden ser una técnica útil para mejorar el rendimiento porque reducen el conjunto de documentos en los que se debe procesar la consulta de búsqueda.

La sintaxis de las expresiones de filtro es un subconjunto del [lenguaje de filtro de OData](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search). Para las consultas de búsqueda, puede usar la [sintaxis simplificada](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) o la [sintaxis de consulta de Lucene](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search) que se explica más abajo.


## <a name="choose-a-syntax-simple-or-full"></a>Selección de sintaxis: simplificada o completa

Azure Search se basa en Apache Lucene y le permite elegir entre dos analizadores de consulta para gestionar consultas típicas y especializadas. Las solicitudes de búsqueda típicas se formulan con la [sintaxis de consulta simplificada](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) predeterminada. Esta sintaxis admite varios operadores de búsqueda comunes entre los que se incluyen los operadores de frase, sufijo, precedencia, AND, OR y NOT.

La [sintaxis de consulta de Lucene](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_syntax), que se habilita al agregar **queryType=full** a la solicitud, permite usar el expresivo lenguaje de consulta adoptado ampliamente desarrollado como parte de [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html). Con esta sintaxis de consulta es posible realizar consultas especializadas:

+ [Consultas de ámbito de campo](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fields)
+ [Búsqueda aproximada](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fuzzy)
+ [Búsqueda por proximidad](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_proximity)
+ [Priorización de términos](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_termboost)
+ [Búsqueda de expresiones regulares](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_regex)
+ [Búsqueda con caracteres comodín](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_wildcard)

Los operadores booleanos son básicamente los mismos en ambas sintaxis, con formatos adicionales en la sintaxis de Lucene completa:

+ [Operadores booleanos en sintaxis simplificada](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search#operators-in-simple-search)
+ [Operadores booleanos en sintaxis de Lucene completa](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_boolean)

## <a name="required-and-optional-elements"></a>Elementos obligatorios y opcionales

Cuando envía solicitudes de búsqueda a Azure Search, hay una serie de parámetros que se pueden especificar junto con las palabras reales que se escriben en el cuadro de búsqueda de la aplicación. Estos parámetros de consulta permiten lograr un mayor control de la [experiencia de búsqueda de texto completo](search-lucene-query-architecture.md).

Entre los elementos obligatorios en una solicitud de consulta se incluyen los siguientes componentes:

+ Colección de indexación de documentos y puntos de conexión de servicio, expresada aquí como una dirección URL: `https://<your-service-name>.search.windows.net/indexes/<your-index-name>/docs`.
+ Versión de API (solo REST), expresada como **api-version=**.
+ Clave de API de administración o consulta, expresada como **api-key=**.
+ Cadena de consulta expresada como **search=**, que se puede dejar sin especificar si desea realizar una búsqueda vacía. También puede enviar simplemente una expresión de filtro como **$filter=**.
+ **queryType=**, simplificada o completa, que se puede omitir si desea usar la sintaxis simplificada predeterminada.

Todos los demás parámetros de búsqueda son opcionales.

## <a name="apis-and-tools-for-testing"></a>API y herramientas de pruebas

En la tabla siguiente se enumeran las API y los métodos basados en herramientas para el envío de consultas.

| Metodología | DESCRIPCIÓN |
|-------------|-------------|
| [SearchIndexClient (.NET)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) | Cliente que puede usarse para consultar un índice de Azure Search.  <br/>[Más información.](search-howto-dotnet-sdk.md#core-scenarios)  |
| [Documentos de búsqueda (API REST)](https://docs.microsoft.com/rest/api/searchservice/search-documents) | Métodos GET o POST en un índice, con parámetros de consulta para entradas adicionales.  |
| [Fiddler, Postman u otra herramienta de prueba HTTP](search-fiddler.md) | Explica cómo configurar un encabezado y un cuerpo de solicitud para enviar consultas a Azure Search.  |
| [Explorador de búsqueda de Azure Portal](search-explorer.md) | Proporciona una barra de búsqueda y opciones para la selección del índice y la versión de API. Los resultados se devuelven como documentos JSON. <br/>[Más información.](search-get-started-portal.md#query-index) | 

## <a name="manage-search-results"></a>Administración de los resultados de búsqueda

Los parámetros de la consulta se pueden usar para estructurar el conjunto de resultados de las siguientes maneras:

+ Limitar o procesar por lotes el número de documentos de los resultados (50 de forma predeterminada)
+ Seleccionar campos que incluir en los resultados
+ Definir un criterio de ordenación
+ Añadir resaltado de referencias para centrar la atención sobre términos coincidentes en el cuerpo de los resultados de búsqueda

### <a name="tips-for-unexpected-results"></a>Sugerencias para resultados inesperados

En ocasiones, la sustancia, y no la estructura de los resultados, no es la esperada. Cuando los resultados de la consulta no son los esperados, puede probar estas modificaciones de consulta para ver si mejoran:

+ Cambie `searchMode=any` (predeterminado) a `searchMode=all` para requerir coincidencias en todos los criterios en lugar de en cualquiera de ellos. Esto sucede especialmente cuando se incluyen operadores booleanos en la consulta.

+ Cambie la técnica de la consulta si se necesita análisis léxico o de texto, pero el tipo de consulta impide el procesamiento lingüístico. En la búsqueda de texto completo, el análisis léxico o de texto corrige automáticamente errores de ortografía, formas de palabras con errores de concordancia de número e incluso sustantivos o verbos irregulares. En algunas consultas, como la búsqueda aproximada o la búsqueda con caracteres comodín, el análisis de texto no forma parte de la canalización de análisis de la consulta. En algunos escenarios, se han utilizado expresiones regulares como solución alternativa. 

### <a name="paging-results"></a>Paginación de resultados
Azure Search facilita la implementación de la paginación de los resultados de búsqueda. Mediante el uso de los parámetros `top` y `skip`, puede emitir fácilmente solicitudes de búsqueda que le permiten recibir el conjunto total de resultados de búsqueda dividido en subconjuntos ordenados, sencillos de administrar que permiten habilitar fácilmente prácticas eficaces de interfaz de usuario de búsqueda. Al recibir estos subconjuntos más pequeños de resultados, también puede recibir el número de documentos del conjunto total de resultados de la búsqueda.

Para más información acerca de la paginación de resultados de búsqueda, consulte el artículo [Cómo paginar los resultados de la búsqueda en Azure Search](search-pagination-page-layout.md).

### <a name="ordering-results"></a>Ordenación de los resultados
Al recibir los resultados de una consulta de búsqueda, puede solicitar que Azure Search muestre los resultados ordenados según los valores de un campo específico. De forma predeterminada, Azure Search ordena los resultados de búsqueda basándose en el rango de puntuación de búsqueda de cada documento, que se deriva del valor [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).

Si desea que Azure Search devuelva los resultados ordenados según un valor distinto de la puntuación de búsqueda, puede usar el parámetro `orderby` . Puede especificar el valor del parámetro `orderby` para que incluya nombres de campo y llamadas a la [`geo.distance()` función](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search) para los valores geoespaciales. Cada expresión puede ir seguida de `asc` para indicar que se solicitan los resultados en orden ascendente y de `desc` para indicar que se solicitan los resultados en orden descendente. El valor predeterminado es el orden ascendente.


### <a name="hit-highlighting"></a>Resaltado de referencias
En Azure Search, resaltar la parte exacta de los resultados de búsqueda que coincide con la consulta de búsqueda es fácil mediante el uso de los parámetros `highlight`, `highlightPreTag`, y `highlightPostTag`. Puede especificar qué campos *habilitados para búsquedas* deben tener su texto coincidente resaltado, así como especificar las etiquetas de cadena exactas que se anexarán al comienzo y al final del texto coincidente que devuelve Azure Search.

## <a name="see-also"></a>Otras referencias

+ [Cómo funciona la búsqueda de texto completo en Azure Search (arquitectura de análisis de consultas)](search-lucene-query-architecture.md)
+ [Explorador de búsqueda](search-explorer.md)
+ [Consultas con .NET](search-query-dotnet.md)
+ [Consultas con REST](search-query-rest-api.md)
