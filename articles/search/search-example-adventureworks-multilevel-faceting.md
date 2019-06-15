---
title: 'Ejemplo: Facetas de varios niveles: Azure Search'
description: Obtenga información sobre cómo crear estructuras en facetas para taxonomías de varios niveles mediante la creación de una estructura de navegación anidada que puede incluir en las páginas de aplicaciones.
author: cstone
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: chstone
ms.openlocfilehash: e17a91a35b69102e4e0ac6025559bbc32e71d8fb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65024120"
---
# <a name="example-multi-level-facets-in-azure-search"></a>Ejemplo: Facetas de varios niveles en Azure Search

Los esquemas de Azure Search no admiten explícitamente las categorías de taxonomía de varios niveles, pero es posible aproximarlas si se manipula el contenido antes de la indexación y luego se aplica un control especial a los resultados. 

## <a name="start-with-the-data"></a>Comenzar con los datos

El ejemplo que aparece en este artículo se basa en un ejemplo anterior, [Model the AdventureWorks Inventory database](search-example-adventureworks-modeling.md) (Modelo de la base de datos del inventario de AdventureWorks), para mostrar las facetas de varios niveles de Azure Search.

AdventureWorks tiene una taxonomía simple de dos niveles con una relación principal-secundario. En el caso de las profundidades de la taxonomía de longitud fija de esta estructura, se puede usar una consulta JOIN de SQL sencilla para agrupar la taxonomía:

```T-SQL
SELECT 
  parent.Name Parent, category.Name Category, category.ProductCategoryId
FROM 
  SalesLT.ProductCategory category
LEFT JOIN
  SalesLT.ProductCategory parent
  ON category.ParentProductCategoryId=parent.ProductCategoryId
```

  ![Resultados de la consulta](./media/search-example-adventureworks/prod-query-results.png "Resultados de la consulta")

## <a name="indexing-to-a-collection-field"></a>Indexación en un campo de colección

En el índice que contiene esta estructura, cree un campo **Collection(Edm.String)** en el esquema de Azure Search para almacenar estos datos y asegúrese de que los atributos del campo incluyan searchable, filtrable, facetable y retrievable.

Ahora, al indexar el contenido que hace referencia a una categoría de taxonomía específica, envíe la taxonomía como una matriz que contiene texto de cada nivel de la taxonomía. Por ejemplo, para una entidad con `ProductCategoryId = 5 (Mountain Bikes)`, envíe el campo como `[ "Bikes", "Bikes|Mountain Bikes"]`

Observe la inclusión de la categoría principal "Bikes" (Bicicletas) en el valor "Mountain Bikes" (Bicicletas de montaña) de la categoría secundaria. Cada subcategoría debe insertar su ruta de acceso completa en relación con el elemento raíz. El separador de carácter de barra vertical es arbitrario, pero debe ser coherente y no debe aparecer en el texto de origen. El carácter separador se usará en el código de la aplicación para reconstruir el árbol taxonómico a partir de los resultados de la faceta.

## <a name="construct-the-query"></a>Construcción de la consulta

Al emitir consultas, incluya la especificación de faceta siguiente (donde la taxonomía es el campo de taxonomía facetable): `facet = taxonomy,count:50,sort:value`

El valor del recuento debe ser lo suficientemente alto para devolver todos los valores de taxonomía posibles. Los datos de AdventureWorks contienen 41 valores de taxonomía distintos, por lo que `count:50` es suficiente.

  ![Filtro en facetas](./media/search-example-adventureworks/facet-filter.png "Filtro en facetas")

## <a name="build-the-structure-in-client-code"></a>Creación de la estructura en el código de cliente

En el código de la aplicación cliente, reconstruya el árbol taxonómico mediante la división de cada valor de factura en el carácter de barra vertical.

```javascript
var sum = 0
  , categories = {children:{},length:0}
  , results = getSearchResults();
separator = separator || '|';
field = field || 'taxonomy';
results['@search.facets'][field].forEach(function(d) {
  var node = categories;
  var parts = d.value.split(separator);
  sum += d.count;
  parts.forEach(function(c, i) {
    if (!_(node.children).has(c)) {
      node.children[c] = {};
      node.children[c].count = d.count;
      node.children[c].children = {};
      node.children[c].length = 0;
      node.children[c].filter = parts.slice(0,i+1).join(separator);
      node.length++;
    }
    node = node.children[c];
  });
});
categories.count = sum;
```

El objeto **categories** ahora se puede usar para representar un árbol taxonómico contraíble con recuentos precisos:

  ![filtro en facetas de varios niveles](./media/search-example-adventureworks/multi-level-facet.png "filtro en facetas de varios niveles")

 
Cada vínculo del árbol debe aplicar el filtro relacionado. Por ejemplo:

+ **taxonomy/any**`(x:x eq 'Accessories')` devuelve todos los documentos de la rama Accesorios
+ **taxonomy/any**`(x:x eq 'Accessories|Bike Racks')` devuelve solo los documentos con una categoría de Bike Racks de la rama Accesorios.

Esta técnica escalará para abarcar escenarios más complejos, como árboles taxonómicos más profundos y subcategorías suplicadas que se producen en distintas categorías principales (por ejemplo, `Bike Components|Forks` y `Camping Equipment|Forks`).

> [!TIP]
> El número de facetas devueltas afecta la velocidad de la consulta. Para admitir conjuntos taxonómicos de gran tamaño, considere la posibilidad de agregar un campo **Edm.String** en facetas para contener el valor taxonómico de nivel superior de cada documento. Luego, aplique la misma técnica anterior, pero solo realice la consulta de faceta de colección (filtrada en el campo de taxonomía raíz) cuando el usuario se expanda a un nodo de nivel superior. O bien, si no se requiere una recuperación del 100 %, simplemente disminuya el recuento de facetas a un número razonable y asegúrese de que las entradas de facetas se ordenan por recuento.

## <a name="see-also"></a>Otras referencias

[Ejemplo: Model the AdventureWorks Inventory database for Azure Search](search-example-adventureworks-modeling.md) (Modelo de la base de datos del inventario de AdventureWorks para Azure Search)