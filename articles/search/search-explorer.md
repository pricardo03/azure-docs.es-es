---
title: Explorador de búsqueda para consultar datos en Azure Portal - Azure Search
description: Utilice las herramientas de Azure Portal como el explorador de búsqueda para realizar consultas de índices en Azure Search. Escriba los términos de búsqueda o cadenas de búsqueda completa con sintaxis avanzada.
manager: nitinme
author: HeidiSteen
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: e941e487adaac38c4ec3bd61a58b4b0c61f4c80a
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69649965"
---
# <a name="search-explorer-for-querying-data-in-azure-search"></a>Explorador de búsqueda para consultar datos en Azure Search 

En este artículo se muestra cómo consultar un índice existente de Azure Search mediante el **Explorador de búsqueda** de Azure Portal. El Explorador de búsqueda se puede usar para enviar cadenas de consulta de Lucene simples o completas para cualquier índice existente en el servicio. 

   ![Comando del explorador de búsqueda en el portal](./media/search-explorer/search-explorer-cmd2.png "Search explorer command in portal")


Para obtener ayuda para empezar, consulte [Iniciar el explorador de búsqueda](#start-search-explorer).

## <a name="basic-search-strings"></a>Cadenas de búsqueda básica

En los ejemplos siguientes se utiliza el índice realestate de ejemplo integrado. Para obtener ayuda para crear este índice, vea [Quickstart: Import, index, and query in Azure portal](search-get-started-portal.md) (Quickstart: importación, creación de índices y consultas en Azure Portal).

### <a name="example-1---empty-search"></a>Ejemplo 1: búsqueda vacía

Para un echar un primer vistazo al contenido, ejecute una búsqueda vacía haciendo clic en **Buscar** sin especificar ningún término. Una búsqueda vacía resulta útil como primera consulta porque devuelve documentos completos para que pueda revisar la composición del documento. En una búsqueda vacía, no hay ninguna clasificación de búsqueda y los documentos se devuelven en orden arbitrario (`"@search.score": 1` para todos los documentos). De forma predeterminada, en una solicitud de búsqueda se devuelven 50 documentos.

La sintaxis equivalente para una búsqueda vacía es `*` o `search=*`.

   ```Input
   search=*
   ```

   **Resultados**
   
   ![Ejemplo de consulta vacía](./media/search-explorer/search-explorer-example-empty.png "Ejemplo de consulta vacía o incompleta")

### <a name="example-2---free-text-search"></a>Ejemplo 2: búsqueda de texto libre

Las consultas de forma libre, con o sin operadores, resultan útiles para simular consultas definidas por el usuario enviadas desde una aplicación personalizada a Azure Search. Tenga en cuenta que, al proporcionar expresiones o términos de consulta, entra en juego la clasificación de búsqueda. El ejemplo siguiente ilustra una búsqueda de texto libre.

   ```Input
   Seattle apartment "Lake Washington" miele OR thermador appliance
   ```

   **Resultados**

   Puede usar Ctrl-F para buscar términos específicos de interés en los resultados.

   ![Ejemplo de consulta de texto libre](./media/search-explorer/search-explorer-example-freetext.png "Free text query example")

### <a name="example-3---count-of-matching-documents"></a>Ejemplo 3: recuento de documentos coincidentes 

Agregue **$count** para obtener el número de coincidencias encontradas en un índice. En una búsqueda vacía, el recuento corresponde al número total de documentos en el índice. En una búsqueda completa, corresponde al número de documentos que coinciden con la entrada de la consulta.

   ```Input1
   $count=true
   ```
   **Resultados**

   ![Ejemplo de recuento de documentos](./media/search-explorer/search-explorer-example-count.png "Recuento de documentos coincidentes en el índice")

### <a name="example-4---restrict-fields-in-search-results"></a>Example 4: restricción de campos en los resultados de la búsqueda

Agregue **$select** para limitar los resultados a los campos con nombre explícito para obtener una salida más legible en **Explorador de búsqueda**. Para mantener la cadena de búsqueda y **$count = true**, anteponga **&** a los argumentos. 

   ```Input
   search=seattle condo&$select=listingId,beds,baths,description,street,city,price&$count=true
   ```

   **Resultados**

   ![Ejemplo de restricción de campos](./media/search-explorer/search-explorer-example-selectfield.png "Restricción de campos en los resultados de la búsqueda")

### <a name="example-5---return-next-batch-of-results"></a>Ejemplo 5: devolución del lote siguiente de resultados

Azure Search devuelve las primeras 50 coincidencias en función de la clasificación de búsqueda. Para obtener el siguiente conjunto de documentos coincidentes, anexe **$top=100,&$skip=50** para aumentar el conjunto de resultados a 100 documentos (el valor predeterminado es 50, el máximo es 1000), lo que omite los primeros 50 documentos. Recuerde que debe proporcionar criterios de búsqueda, como una expresión o un término de consulta, para obtener los resultados clasificados. Tenga en cuenta que las puntuaciones de búsqueda disminuyen cuanto más profundamente se llega en los resultados de la búsqueda.

   ```Input
   search=seattle condo&$select=listingId,beds,baths,description,street,city,price&$count=true&$top=100&$skip=50
   ```

   **Resultados**

   ![Resultados de la búsqueda por lotes](./media/search-explorer/search-explorer-example-topskip.png "Devolución del siguiente lote de resultados de la búsqueda")

## <a name="filter-expressions-greater-than-less-than-equal-to"></a>Expresiones de filtro (mayor que, menor que, igual a)

Utilice el parámetro **$filter** si quiere especificar criterios precisos en lugar de búsqueda de texto libre. En este ejemplo se buscan más de tres dormitorios:

   ```Input
   search=seattle condo&$filter=beds gt 3&$count=true
   ```
   
   **Resultados**

   ![Expresión de filtro](./media/search-explorer/search-explorer-example-filter.png "Criterios de Filtrar por")

## <a name="order-by-expressions"></a>Expresiones OrderBy

Agregue **$orderby** para ordenar los resultados por otro campo además de la puntuación de búsqueda. Una expresión de ejemplo que puede usar para probar este caso es:

   ```Input
   search=seattle condo&$select=listingId,beds,price&$filter=beds gt 3&$count=true&$orderby=price asc
   ```
   
   **Resultados**

   ![Expresión OrderBy](./media/search-explorer/search-explorer-example-ordery.png "Cambiar el criterio de ordenación")

Ambas expresiones, **$filter** y **$orderby** son construcciones de OData. Para más información, consulte la [sintaxis de filtro de OData](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

<a name="start-search-explorer"></a>

## <a name="how-to-start-search-explorer"></a>Cómo iniciar el Explorador de búsqueda

1. En [Azure Portal](https://portal.azure.com), abra la página del servicio de búsqueda desde el panel o [busque el servicio](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) en la lista.

2. En la página de información general del servicio, haga clic en **Explorador de búsqueda**.

   ![Comando del explorador de búsqueda en el portal](./media/search-explorer/search-explorer-cmd2.png "Search explorer command in portal")

3. Seleccione el índice de la consulta.

   ![Seleccionar el índice de la consulta](./media/search-explorer/search-explorer-changeindex-se2.png "Seleccionar el índice")

4. Si quiere, establezca la versión de API. De forma predeterminada, se selecciona la versión actual de API disponible con carácter general, pero puede elegir una API anterior o versión preliminar si la sintaxis que quiere utilizar es específica de la versión.

5. Una vez seleccionada la versión de la API y el índice, escriba los términos de búsqueda o las expresiones de consulta completas en la barra de búsqueda y haga clic en **Buscar** para ejecutar.

   ![Escriba los términos de búsqueda y haga clic en Buscar](./media/search-explorer/search-explorer-query-string-example.png "Enter search terms and click Search")

Sugerencias para realizar búsquedas en **Explorador de búsqueda**:

+ Los resultados se devuelven como documentos JSON detallados para que pueda ver la construcción y el contenido del documento en su totalidad. Puede usar expresiones de consulta, que se muestran en los ejemplos, para limitar los campos que se devuelven.

+ Los documentos se componen de todos los campos marcados como **Recuperable** en el índice. Para ver los atributos del índice en el portal, haga clic en *realestate-us-sample* en la lista **Índices** de la página de información general de búsqueda.

+ Las consultas de forma libre, similares a las que se pueden escribir en un explorador web comercial, resultan útiles para probar una experiencia de usuario final. Por ejemplo, si partimos del índice realestate de ejemplo integrado, podría escribir "Apartamentos Seattle Lake Washington" y luego usar Ctrl-F para buscar términos dentro de los resultados de búsqueda. 

+ Las expresiones de consulta y de filtro deben articularse en una sintaxis que Azure Search admita. El valor predeterminado es una [sintaxis simple](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search), pero también puede usar la sintaxis de [Lucene completa](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) para realizar consultas más eficaces. Las [expresiones de filtro](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) son una sintaxis de OData.


## <a name="next-steps"></a>Pasos siguientes

Los siguientes recursos proporcionan ejemplos y la información de la sintaxis de consulta adicionales.

 + [Sintaxis de consulta simplificada](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [Sintaxis de consulta de Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [Ejemplos de consultas de Lucene](search-query-lucene-examples.md) 
 + [OData Expression Syntax for Azure Search](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) (Sintaxis de expresiones de OData en Azure Search) 
