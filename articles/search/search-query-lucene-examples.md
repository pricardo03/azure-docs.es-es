---
title: Ejemplos de consulta de Lucene con Azure Search
description: Sintaxis de consulta de Lucene para la búsqueda aproximada, la búsqueda de proximidad, la priorización de términos, la búsqueda de expresiones regulares y la búsqueda con caracteres comodín en un servicio de Azure Search.
author: HeidiSteen
manager: cgronlun
tags: Lucene query analyzer syntax
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 6f7fce7eab697f6517b351d00595cb02110d3641
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61286436"
---
# <a name="query-examples-using-full-lucene-search-syntax-advanced-queries-in-azure-search"></a>Ejemplos de consultas mediante sintaxis de búsqueda de "full" Lucene (consultas avanzadas en Azure Search)

Al construir consultas para Azure Search, puede reemplazar el [analizador de consultas sencillo](query-simple-syntax.md) predeterminado con el más integral [analizador de consultas de Lucene en Azure Search](query-lucene-syntax.md) para formular definiciones de consultas especializadas y avanzadas. 

El analizador de Lucene admite las construcciones de consulta compleja, como consultas centrada en el campo, aproximadas y búsqueda de prefijo con caracteres comodín, búsqueda de proximidad, priorización de términos y búsqueda de expresión regular. La potencia adicional trae consigo requisitos de procesamiento adicional, por lo que debe esperar un tiempo de ejecución un poco más largo. En este artículo, puede ir a través de ejemplos de operaciones de consulta disponibles cuando se usa la sintaxis completa.

> [!Note]
> Muchas de las construcciones de consulta especializadas habilitadas mediante la sintaxis de consulta completa de Lucene no son [de análisis de texto](search-lucene-query-architecture.md#stage-2-lexical-analysis), lo que puede ser sorprendente si espera lematización. Solo se realizan análisis léxicos en términos completos (consulta de término o de expresión). Los tipos de consulta con términos incompletos (consulta de prefijo, de carácter comodín, de expresión regular o aproximada) se agregan directamente en el árbol de la consulta, omitiéndose la fase de análisis. La única transformación que se realiza en los términos de consulta incompletos es el establecimiento de minúsculas. 
>

## <a name="formulate-requests-in-postman"></a>Formulación de solicitudes en Postman

Los ejemplos siguientes aprovechan un índice de búsqueda de trabajos de Nueva York que consta de los disponibles según un conjunto de datos proporcionado por la iniciativa [City of New York OpenData](https://opendata.cityofnewyork.us/). Estos datos no deben considerarse actuales o completos. El índice está en un servicio de espacio aislado proporcionado por Microsoft, lo que significa que no necesita una suscripción de Azure o Azure Search para probar estas consultas.

Lo que necesita es Postman o una herramienta equivalente para emitir la solicitud HTTP en GET. Para más información, consulte el artículo sobre la [exploración con clientes REST](search-fiddler.md).

### <a name="set-the-request-header"></a>Establecimiento del encabezado de solicitud

1. En el encabezado de solicitud, establezca **Content-Type** en `application/json`.

2. Agregue un valor de **clave-api** y establézcala en esta cadena: `252044BE3886FE4A8E3BAA4F595114BB`. Se trata de una clave de consulta para el servicio de búsqueda de espacio aislado que hospeda el índice de trabajos de Nueva York.

Después de especificar el encabezado de solicitud, puede volver a usarlo para todas las consultas de este artículo, cambiando solo la cadena **search=**. 

  ![Encabezado de solicitud de Postman](media/search-query-lucene-examples/postman-header.png)

### <a name="set-the-request-url"></a>Establecimiento de la dirección URL de solicitud

La solicitud es un comando GET emparejado con una dirección URL que contiene la cadena de búsqueda y el punto de conexión de Azure Search.

  ![Encabezado de solicitud de Postman](media/search-query-lucene-examples/postman-basic-url-request-elements.png)

La composición de dirección URL tiene los siguientes elementos:

+ **`https://azs-playground.search.windows.net/`** es un servicio de búsqueda de espacio aislado mantenido por el equipo de desarrollo de Azure Search. 
+ **`indexes/nycjobs/`** es el índice de trabajos de Nueva York en la colección de índices de ese servicio. Tanto el nombre del servicio como el índice son necesarios en la solicitud.
+ **`docs`** es la colección de documentos que incluye todo el contenido que permite búsquedas. El valor de clave-api de la consulta proporcionada en el encabezado de solicitud solo funciona en las operaciones de lectura destinadas a la colección de documentos.
+ **`api-version=2017-11-11`** establece la versión de api, que es un parámetro necesario en cada solicitud.
+ **`search=*`** es la cadena de consulta, que en la consulta inicial es NULL, con lo que se devuelven los 50 primeros resultados (de forma predeterminada).

## <a name="send-your-first-query"></a>Envío de la primera consulta

Como paso de comprobación, pegue la siguiente solicitud en GET y haga clic en **Enviar**. Los resultados se devuelven como documentos JSON detallados. Se devuelven documentos completos, lo que permite ver todos los campos y todos los valores.

Pegue esta dirección URL en un cliente REST como paso de validación y ver la estructura del documento.

  ```http
  https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search=*
  ```

La cadena de consulta, **`search=*`**, es una búsqueda sin especificar equivalente a la búsqueda vacía o NULL. Es la búsqueda más sencilla que puede hacer.

Si lo desea, puede agregar **`$count=true`** a la dirección URL para devolver un recuento de los documentos que coinciden con los criterios de búsqueda. En una cadena de búsqueda vacía, se trata de todos los documentos del índice (unos 2800 en el caso de los trabajos de Nueva York).

## <a name="how-to-invoke-full-lucene-parsing"></a>Invocación del análisis completo de Lucene

Si se agrega **queryType = full** para invocar la sintaxis de consulta completa, se reemplaza la sintaxis de consulta simple predeterminada. 

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&queryType=full&search=*
```

Todos los ejemplos de este artículo especifican el parámetro de búsqueda **queryType=full**, que indica que toda la sintaxis se controla mediante el Analizador de consultas de Lucene. 

## <a name="example-1-field-scoped-query"></a>Ejemplo 1: Consulta de ámbito de campo

En este primer ejemplo no es específico de Lucene, pero se llevar con él a introducir el primer concepto fundamental de consulta: contención. En este ejemplo se establece el ámbito de ejecución de consultas y la respuesta a unos pocos campos específicos. Es importante saber cómo estructurar una respuesta JSON legible cuando la herramienta es Postman o el Explorador de búsqueda. 

Por brevedad, la consulta tiene como destino únicamente el campo *business_title* y especifica que se devuelvan solo los puestos de empresa. La sintaxis es **searchFields** para restringir la ejecución de consultas a solo el campo de business_title y **select** para especificar qué campos se incluyen en la respuesta.

### <a name="partial-query-string"></a>Cadena de consulta parcial

```http
&search=*&searchFields=business_title&$select=business_title
```

Esta es la misma consulta con varios campos en una lista delimitada por comas.

```http
search=*&searchFields=business_title, posting_type&$select=business_title, posting_type
```

### <a name="full-url"></a>Dirección URL completa

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&queryType=full&$count=true&search=*&searchFields=business_title&$select=business_title
```

La respuesta de esta consulta debe tener un aspecto similar a la siguiente captura de pantalla.

  ![Respuesta de ejemplo de Postman](media/search-query-lucene-examples/postman-sample-results.png)

Tal vez haya notado la puntuación de búsqueda en la respuesta. Las puntuaciones uniformes de 1 se producen cuando no hay ninguna clasificación, ya sea debido a que la búsqueda no era de texto completo o porque no se ha aplicado ningún criterio. Para la búsqueda de valores null sin ningún criterio, las filas vuelven en orden aleatorio. Al incluir criterios reales, verá que las puntuaciones de búsqueda evolucionan en valores significativos.

## <a name="example-2-intra-field-filtering"></a>Ejemplo 2: Filtrado en el campo

La sintaxis completa de Lucene admite expresiones dentro de un campo. Este ejemplo busca los puestos de empresa con el jefe de término en ellos, pero no "junior".

### <a name="partial-query-string"></a>Cadena de consulta parcial

```http
searchFields=business_title&$select=business_title&search=business_title:senior+NOT+junior
```

Esta es la misma consulta con varios campos.

```http
searchFields=business_title, posting_type&$select=business_title, posting_type&search=business_title:senior+NOT+junior AND posting_type:external
```

### <a name="full-url"></a>Dirección URL completa

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&queryType=full&$count=true&searchFields=business_title&$select=business_title&search=business_title:senior+NOT+junior
```

  ![Respuesta de ejemplo de Postman](media/search-query-lucene-examples/intrafieldfilter.png)

Si especifica una construcción **fieldname:searchterm**, puede definir una operación de consulta clasificada por campos, donde el campo es una sola palabra y el término de búsqueda también es una sola palabra o frase, opcionalmente con operadores booleanos. Estos son algunos ejemplos:

* business_title:(senior NOT junior)
* state:("New York" AND "New Jersey")
* business_title:(senior NOT junior) AND posting_type:external

Asegúrese de colocar varias cadenas entre comillas si desea que las dos cadenas se evalúen como una sola entidad, como en este caso buscando dos ciudades distintas en el campo de ubicación. Además, asegúrese del operador está en mayúsculas como puede ver en NOT y AND.

El campo especificado en **fieldname:searchterm** debe ser un campo que permita búsquedas. Consulte [Creación de un índice (API de REST del servicio Azure Search)](https://docs.microsoft.com/rest/api/searchservice/create-index) para más información sobre cómo se usan los atributos de índice en las definiciones de campo.

## <a name="example-3-fuzzy-search"></a>Ejemplo 3: Búsqueda aproximada

La sintaxis completa de Lucene también admite la búsqueda aproximada, que busca términos que tienen una construcción similar. Para realizar una búsqueda aproximada, agregue la virgulilla, `~`, al final de una sola palabra con un parámetro opcional y un valor entre 0 y 2 para especificar la distancia de edición. Por ejemplo, `blue~` o `blue~1` devolverían blue, blues y glue.

### <a name="partial-query-string"></a>Cadena de consulta parcial

```http
searchFields=business_title&$select=business_title&search=business_title:asosiate~
```

Frases no se admiten directamente, pero puede especificar a una coincidencia aproximada en partes componentes de una frase.

```http
searchFields=business_title&$select=business_title&search=business_title:asosiate~ AND comm~ 
```


### <a name="full-url"></a>Dirección URL completa

Esta consulta busca los trabajos con el término "associate" (mal escrito de forma deliberada):

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&queryType=full&$count=true&searchFields=business_title&$select=business_title&search=business_title:asosiate~
```
  ![Respuesta de búsqueda aproximada](media/search-query-lucene-examples/fuzzysearch.png)


> [!Note]
> Las consultas aproximadas no se [analizan](search-lucene-query-architecture.md#stage-2-lexical-analysis). Los tipos de consulta con términos incompletos (consulta de prefijo, de carácter comodín, de expresión regular o aproximada) se agregan directamente en el árbol de la consulta, omitiéndose la fase de análisis. La única transformación que se realiza en los términos de consulta incompletos es el establecimiento de minúsculas.
>

## <a name="example-4-proximity-search"></a>Ejemplo 4: Búsqueda por proximidad
Las búsquedas de proximidad se utilizan para buscar términos que están cerca entre sí en un documento. Inserte un símbolo "~" de la tilde de la Ñ al final de una frase seguido del número de palabras que crea el límite de proximidad. Por ejemplo, "hotel airport"~5 buscará los términos "hotel" y "airport" dentro de 5 palabras en un documento.

### <a name="partial-query-string"></a>Cadena de consulta parcial

```http
searchFields=business_title&$select=business_title&search=business_title:%22senior%20analyst%22~1
```

### <a name="full-url"></a>Dirección URL completa

En esta consulta, se buscan trabajos con el término "senior analyst" no separado por más de una palabra:

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&queryType=full&$count=true&searchFields=business_title&$select=business_title&search=business_title:%22senior%20analyst%22~1
```
  ![Consulta de proximidad](media/search-query-lucene-examples/proximity-before.png)

Pruebe a quitar las palabras entre el término "senior analyst". Tenga en cuenta que se devuelven ocho documentos para esta consulta en lugar de los diez de la consulta anterior.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&queryType=full&$count=true&searchFields=business_title&$select=business_title&search=business_title:%22senior%20analyst%22~0
```

## <a name="example-5-term-boosting"></a>Ejemplo 5: Priorización de términos
"Priorización de términos" hace referencia a la valoración de un documento superior si contiene el término prioritario con respecto a los documentos que no contienen el término. Para dar prioridad a un término, use el símbolo de intercalación, "^", un símbolo con un factor de prioridad (un número) al final del término que desee buscar. 

### <a name="full-urls"></a>Direcciones URL completas

En esta consulta "anterior", se buscan los trabajos con el término *computer analyst*; observe que no se generan resultados con las palabras *computer* y *analyst*, aunque trabajos *computer* encabezan los resultados.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&queryType=full&$count=true&searchFields=business_title&$select=business_title&search=business_title:computer%20analyst
```
  ![Priorización de términos anterior](media/search-query-lucene-examples/termboostingbefore.png)

En la consulta "posterior", se repite la búsqueda y esta vez se da prioridad a los resultados con el término *analyst* sobre el término *computer*, si ninguna de las palabras existen. 

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&queryType=full&$count=true&searchFields=business_title&$select=business_title&search=business_title:computer%20analyst%5e2
```
Una versión más legible de la consulta anterior es `search=business_title:computer analyst^2`. Para una consulta factible, `^2` se codifica como `%5E2`, que es más difícil de ver.

  ![Priorización de términos posterior](media/search-query-lucene-examples/termboostingafter.png)

La priorización de términos difiere de los perfiles de puntuación en que estos últimos priorizan ciertos campos, en lugar de términos específicos. En el siguiente ejemplo se muestran las diferencias.

Considere la posibilidad de usar un perfil de puntuación que dé prioridad a las coincidencias en un campo determinado, como **genre** en el ejemplo musicstoreindex. La priorización de términos podría utilizarse para dar mayor prioridad a determinados términos de búsqueda frente a otros. Por ejemplo, "rock^2 electronic" dará prioridad a los documentos que contengan los términos de búsqueda del campo **genre** frente a otros campos que permitan búsquedas en el índice. Además, los documentos que contienen el término de búsqueda "rock" tendrán una clasificación mayor que los del término de búsqueda "electronic" como resultado del valor de la priorización de términos (2).

Al establecer el nivel de factor, cuanto más alto sea el factor de prioridad, más apropiado será el término en relación con los demás términos de búsqueda. De forma predeterminada, el factor de prioridad es 1. Aunque el factor de prioridad debe ser positivo, puede ser inferior a 1 (por ejemplo, 0,2).


## <a name="example-6-regex"></a>Ejemplo 6: Regex

Una búsqueda de expresión regular encuentra una coincidencia en función del contenido entre barras diagonales "/", como se documentó en la [clase RegExp](https://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).

### <a name="partial-query-string"></a>Cadena de consulta parcial

```http
searchFields=business_title&$select=business_title&search=business_title:/(Sen|Jun)ior/
```

### <a name="full-url"></a>Dirección URL completa

En esta consulta, busque trabajos con el término jefe o "junior": `search=business_title:/(Sen|Jun)ior/`.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&queryType=full&$count=true&searchFields=business_title&$select=business_title&search=business_title:/(Sen|Jun)ior/
```

  ![Consulta de Regex](media/search-query-lucene-examples/regex.png)

> [!Note]
> Las consultas Regex no se [analizan](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis). La única transformación que se realiza en los términos de consulta incompletos es el establecimiento de minúsculas.
>

## <a name="example-7-wildcard-search"></a>Ejemplo 7: Búsqueda con caracteres comodín
Puede usar la sintaxis generalmente reconocida para búsquedas con caracteres comodín únicas (?) o múltiples (\*). Tenga en cuenta que el Analizador de consultas de Lucene admite el uso de estos símbolos con un único término y no una frase.

### <a name="partial-query-string"></a>Cadena de consulta parcial

```http
searchFields=business_title&$select=business_title&search=business_title:prog*
```

### <a name="full-url"></a>Dirección URL completa

En esta consulta, busque los trabajos que contengan el prefijo "prog" que incluiría los títulos de empresa con los términos "programación" y "programador". ¿No puede utilizar un símbolo * o ? como primer carácter de la búsqueda.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&queryType=full&$count=true&searchFields=business_title&$select=business_title&search=business_title:prog*
```
  ![Consulta de carácter comodín](media/search-query-lucene-examples/wildcard.png)

> [!Note]
> Las consultas con caracteres comodín no se [analizan](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis). La única transformación que se realiza en los términos de consulta incompletos es el establecimiento de minúsculas.
>

## <a name="next-steps"></a>Pasos siguientes
Intente especificar el Analizador de consultas de Lucene en el código. Los vínculos siguientes explican cómo configurar las consultas de búsqueda para la API de REST y .NET. Los vínculos usan la sintaxis simple de forma predeterminada, por lo que deberá aplicar lo aprendido en este artículo para especificar el valor de **queryType**.

* [Consultas del índice de Azure Search con el SDK de .NET](search-query-dotnet.md)
* [Realización de una consulta al índice de Azure Search con la API de REST](search-create-index-rest-api.md)

En los vínculos siguientes, se puede encontrar una referencia de la sintaxis, la arquitectura de las consultas y ejemplos:

+ [Ejemplos de consulta de sintaxis sencilla](search-query-simple-examples.md)
+ [Cómo funciona la búsqueda de texto completo en Azure Search](search-lucene-query-architecture.md)
+ [Sintaxis de consulta simplificada](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Sintaxis de consulta completa de Lucene ](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)