---
title: Ejemplos de consultas simplificadas para Azure Search | Microsoft Docs
description: Ejemplos de consultas simplificadas para búsqueda de texto completo, búsqueda filtrada, búsqueda geográfica, búsqueda por facetas y otras cadenas de consulta que se usan para consultar un índice de Azure Search.
author: HeidiSteen
manager: cgronlun
tags: Simple query analyzer syntax
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: heidist
ms.openlocfilehash: e4bb72eb8ad6f15b0e5bc14e0e07556e76d0477b
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2018
ms.locfileid: "39368603"
---
# <a name="simple-syntax-query-examples-for-building-queries-in-azure-search"></a>Ejemplos de sintaxis de consulta simplificada para la creación de consultas en Azure Search

La [sintaxis de consulta simplificada](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) invoca el analizador de consultas predeterminado para ejecutar consultas de búsqueda de texto completo en un índice de Azure Search. El analizador de consultas simplificadas es rápido y gestiona escenarios comunes en Azure Search, incluida la búsqueda de texto completo, la búsqueda por facetas, la búsqueda filtrada y la búsqueda geográfica. En este artículo, revise ejemplos de operaciones de consulta disponibles cuando se usa la sintaxis simplificada.

La sintaxis de consulta alternativa es [Lucene completa](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), compatible con estructuras de consulta más complejas, como la búsqueda aproximada y la búsqueda con caracteres comodín, por lo que puede tardar más tiempo en procesarse. Para más información y ejemplos que muestran la sintaxis completa, consulte [Ejemplos de consulta de sintaxis de Lucene](search-query-lucene-examples.md).

## <a name="formulate-requests-in-postman"></a>Formulación de solicitudes en Postman

Los ejemplos siguientes aprovechan un índice de búsqueda de trabajos de Nueva York que consta de los disponibles según un conjunto de datos proporcionado por la iniciativa [City of New York OpenData](https://nycopendata.socrata.com/). Estos datos no deben considerarse actuales o completos. El índice está en un servicio de espacio aislado proporcionado por Microsoft, lo que significa que no necesita una suscripción de Azure o Azure Search para probar estas consultas.

Lo que necesita es Postman o una herramienta equivalente para emitir la solicitud HTTP en GET. Para más información, consulte [Prueba con clientes REST](search-fiddler.md).

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

Como paso de comprobación, pegue la siguiente solicitud en GET y haga clic en **Enviar**. Los resultados se devuelven como documentos JSON detallados. Puede copiar y pegar esta dirección URL en el primer ejemplo siguiente.

  ```http
  https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&search=*
  ```

La cadena de consulta, **`search=*`**, es una búsqueda sin especificar equivalente a la búsqueda vacía o NULL. No resulta especialmente útil, pero es la búsqueda más sencilla que se puede hacer.

Si lo desea, puede agregar **`$count=true`** a la dirección URL para devolver un recuento de los documentos que coinciden con los criterios de búsqueda. En una cadena de búsqueda vacía, se trata de todos los documentos del índice (2802 en el caso de los trabajos de Nueva York).

## <a name="how-to-invoke-simple-query-parsing"></a>Cómo invocar el análisis de consulta simplificada

Para las consultas interactivas, no tiene que especificar nada: la consulta simplificada es la predeterminada. En el código, si se ha invocado anteriormente **queryType=full** para la sintaxis de consulta completa, podría restablecer el valor predeterminado con **queryType=simple**.

## <a name="example-1-field-scoped-query"></a>Ejemplo 1: Consulta de ámbito de campo

La primera consulta no es específica de un tipo de sintaxis (funciona tanto para la sintaxis simplificada como para la completa), pero empezamos con este ejemplo para introducir un concepto de consulta de referencia que genera una respuesta JSON bastante legible. Por brevedad, la consulta tiene como destino únicamente el campo *business_title* y especifica que se devuelvan solo los puestos de empresa. 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&search=*
```

El parámetro **searchFields** restringe la búsqueda solo al campo de puesto. El parámetro **select** determina qué campos se incluyen en el conjunto de resultados.

La respuesta de esta consulta debe tener un aspecto similar a la siguiente captura de pantalla.

  ![Respuesta de ejemplo de Postman](media/search-query-lucene-examples/postman-sample-results.png)

Es posible que haya observado que la puntuación de búsqueda también se devuelve para cada documento, aunque esta no se especifique. Esto es porque la puntuación de búsqueda son los metadatos, con el valor que indica el orden de clasificación de los resultados. Las puntuaciones uniformes de 1 se producen cuando no hay ninguna clasificación, ya sea debido a que la búsqueda no era de texto completo o porque no hay ningún criterio que aplicar. Para la búsqueda de valores NULL, no hay ningún criterio y las filas que se devuelven están en orden aleatorio. A medida que los criterios de búsqueda se definan más, verá que las clasificaciones evolucionan a valores significativos.

## <a name="example-2-look-up-by-id"></a>Ejemplo 2: Búsqueda por identificador

Este ejemplo es un poco inusual, pero al evaluar comportamientos de búsqueda, es posible que desee inspeccionar todo el contenido de un documento para entender por qué se incluye o se excluye de los resultados. Para devolver un documento completo, utilice una [operación de búsqueda](https://docs.microsoft.com/rest/api/searchservice/lookup-document) para pasar el identificador del documento.

Todos los documentos tienen un identificador único. Para probar la sintaxis de una consulta de búsqueda, en primer lugar devuelva una lista de identificadores de documento para elegir el que desea utilizar. Para NYC Jobs, los identificadores están almacenados en el campo `id`.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=id&$select=id&search=*
 ```

El ejemplo siguiente es una consulta de búsqueda que devuelve un documento específico basado en el `id` "9E1E3AF9-0660-4E00-AF51-9B654925A2D5", que apareció por primera vez en la respuesta anterior. La consulta siguiente devuelve el documento completo, no solo campos seleccionados. 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs/9E1E3AF9-0660-4E00-AF51-9B654925A2D5?api-version=2017-11-11&$count=true&search=*
 ```

## <a name="example-3-search-precision"></a>Ejemplo 3: Precisión de la búsqueda

Las consultas de términos son términos individuales (puede que muchos de ellos) que se evalúan de forma independiente. Las consultas de frases se incluyen entre comillas y se evalúan como una cadena textual. La precisión de la coincidencia se controla mediante operadores y el parámetro searchMode.

Ejemplo 1: **`&search=fire`** devuelve 150 resultados, donde todas las coincidencias contienen la palabra "fire" en algún lugar del documento.

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search=fire
```

Ejemplo 2: **`&search=fire department`** devuelve 2002 resultados. Se devuelven coincidencias para documentos que contienen las palabras "fire" o "department".

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search=fire department
```

Ejemplo 3: **`&search="fire department"`** devuelve 82 resultados. Al incluir la cadena entre comillas se convierte en una búsqueda textual de ambos términos y las coincidencias encontradas en términos con tokens en el índice están formadas por los términos combinados. Esto explica por qué una búsqueda como **`search=+fire +department`** no es equivalente. Ambos términos son obligatorios, pero se buscan de forma independiente. 

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search="fire department"
```

## <a name="example-4-booleans-with-searchmode"></a>Ejemplo 4: Valores booleanos con searchMode

La sintaxis simplificada es compatible con operadores booleanos en forma de caracteres (`+, -, |`). El parámetro searchMode informa de las compensaciones entre precisión y recuperación, donde `searchMode=any` favorece la recuperación (la coincidencia con cualquier criterio permite incluir a un documento en el conjunto de resultados) y `searchMode=all` favorece la precisión (deben cumplirse todos los criterios). El valor predeterminado es `searchMode=any`, lo que puede llevar a confusión si se apila una consulta con varios operadores y se obtienen resultados más amplios, en lugar de más restringidos. Esto sucede especialmente con el operador NOT, donde los resultados incluyen todos los documentos que "no contienen" un término específico.

Con el parámetro searchMode predeterminado (cualquiera), se devuelven 2800 documentos: aquellos que contienen el término de varias partes "fire department", más todos los documentos que no contienen el término "Metrotech Center".

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchMode=any&search="fire department"  -"Metrotech Center"
```
Al cambiar searchMode a `all`, se aplica un efecto acumulativo en los criterios y se devuelve un conjunto de resultados más pequeño (21 documentos) que consta de los documentos que contienen la frase completa "fire department", menos los trabajos con la dirección de Metrotech Center.

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchMode=all&search="fire department"  -"Metrotech Center"
```


## <a name="example-5-structuring-results"></a>Ejemplo 5: Estructuración de resultados

Varios parámetros controlan qué campos se incluyen en la búsqueda de resultados, el número de documentos devueltos en cada lote y el criterio de ordenación. En este ejemplo reaparecen algunos de los ejemplos anteriores, al limitar los resultados a campos específicos mediante la instrucción **$select** y criterios de búsqueda textual, devolviendo 82 coincidencias 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"
```
Al anexarlo al ejemplo anterior, podrá ordenar por título. Esta ordenación funciona porque civil_service_title *se puede ordenar* en el índice.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title
```

La paginación de resultados se implementa mediante el parámetro **$top**, devolviendo en este caso los cinco documentos principales:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title&$top=5&$skip=0
```

Para obtener los cinco siguientes, omita el primer lote:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title&$top=5&$skip=5
```

## <a name="next-steps"></a>Pasos siguientes
Pruebe a especificar las consultas en el código. Los vínculos siguientes explican cómo configurar las consultas de búsqueda para la API REST y .NET mediante la sintaxis simplificada predeterminada.

* [Consultas del índice de Azure Search con el SDK de .NET](search-query-dotnet.md)
* [Realización de una consulta al índice de Azure Search con la API de REST](search-query-rest-api.md)

En los vínculos siguientes, se puede encontrar una referencia de la sintaxis, la arquitectura de las consultas y ejemplos:

+ [Ejemplos de consulta con sintaxis de Lucene para la creación de consultas avanzadas](search-query-lucene-examples.md)
+ [Cómo funciona la búsqueda de texto completo en Azure Search](search-lucene-query-architecture.md)
+ [Sintaxis de consulta simplificada](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Consulta completa de Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)
