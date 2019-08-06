---
title: Indexación en Azure Cosmos DB
description: Comprenda cómo funciona la indexación en Azure Cosmos DB.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: thweiss
ms.openlocfilehash: c8e21ea89f3e23709d636ab8af4716bff76d7217
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479286"
---
# <a name="indexing-in-azure-cosmos-db---overview"></a>Indexación en Azure Cosmos DB: introducción

Azure Cosmos DB es una base de datos independiente del esquema que le permite iterar en la aplicación sin tener que tratar con la administración de esquemas o índices. De forma predeterminada, Azure Cosmos DB indexa automáticamente todas las propiedades de todos los elementos de su [contenedor](databases-containers-items.md#azure-cosmos-containers) sin tener que definir ningún esquema ni configurar índices secundarios.

El objetivo de este artículo es explicar cómo Azure Cosmos DB indexa datos y cómo usa índices para mejorar el rendimiento de las consultas. Se recomienda revisar esta sección antes de explorar cómo personalizar las [directivas de indexación](index-policy.md).

## <a name="from-items-to-trees"></a>De elementos a árboles

Cada vez que un elemento se almacena en un contenedor, su contenido se proyecta como un documento JSON y luego se convierte en una representación de árbol. Eso significa que todas las propiedades de ese elemento se representan como nodos de un árbol. Para todas las propiedades de primer nivel del elemento, se crea un pseudonodo raíz como elemento primario. Los nodos hoja contienen los valores escalares reales pertenecientes a un elemento.

Por ejemplo, considere este elemento:

    {
        "locations": [
            { "country": "Germany", "city": "Berlin" },
            { "country": "France", "city": "Paris" }
        ],
        "headquarters": { "country": "Belgium", "employees": 250 },
        "exports": [
            { "city": "Moscow" },
            { "city": "Athens" }
        ]
    }

Se podría representar mediante el árbol siguiente:

![El elemento anterior representado como un árbol](./media/index-overview/item-as-tree.png)

Observe cómo se codifican las matrices en el árbol: cada entrada de una matriz obtiene un nodo intermedio etiquetado con el índice de esa entrada dentro de la matriz (0, 1, etc.).

## <a name="from-trees-to-property-paths"></a>De árboles a rutas de acceso de propiedades

El motivo por el que Azure Cosmos DB transforma los elementos en árboles es porque permite hacer referencia a las propiedades mediante sus rutas de acceso dentro de esos árboles. Para obtener la ruta de acceso de una propiedad, podemos recorrer el árbol desde el nodo raíz hasta esa propiedad y concatenar las etiquetas de cada nodo recorrido.

Estas son las rutas de acceso de cada propiedad del elemento de ejemplo descrito anteriormente:

    /locations/0/country: "Germany"
    /locations/0/city: "Berlin"
    /locations/1/country: "France"
    /locations/1/city: "Paris"
    /headquarters/country: "Belgium"
    /headquarters/employees: 250
    /exports/0/city: "Moscow"
    /exports/1/city: "Athens"

Cuando se escribe un elemento, Azure Cosmos DB indexa eficazmente la ruta de acceso de cada propiedad y su valor correspondiente.

## <a name="index-kinds"></a>Tipos de índices

Actualmente, Azure Cosmos DB admite tres tipos de índices:

El tipo de índice de **rango** se usa con:

- Consultas de igualdad:

    ```sql
   SELECT * FROM container c WHERE c.property = 'value'
    ```

- Consultas por rango:

   ```sql
   SELECT * FROM container c WHERE c.property > 'value'
   ``` 
  (funciona para `>`, `<`, `>=`, `<=`, `!=`).

- Consultas `ORDER BY`:

   ```sql 
   SELECT * FROM container c ORDER BY c.property
   ```

- Consultas `JOIN`:

   ```sql
   SELECT child FROM container c JOIN child IN c.properties WHERE child = 'value'
   ```

Los índices de rango se pueden usar en valores escalares (cadena o número).

El tipo de índice **espacial** se usa con:

- Consultas de distancia geoespaciales: 

   ```sql
   SELECT * FROM container c WHERE ST_DISTANCE(c.property, { "type": "Point", "coordinates": [0.0, 10.0] }) < 40
   ```

- Información geoespacial dentro de consultas: 

   ```sql
   SELECT * FROM container c WHERE ST_WITHIN(c.property, {"type": "Point", "coordinates": [0.0, 10.0] } })
   ```

Los índices espaciales pueden usarse en objetos [GeoJSON](geospatial.md) con un formato correcto. Actualmente se admiten puntos, LineStrings y polígonos.

El tipo de índice **compuesto** se usa con:

- Consultas `ORDER BY` en varias propiedades: 

   ```sql
   SELECT * FROM container c ORDER BY c.firstName, c.lastName
   ```

## <a name="querying-with-indexes"></a>Consultas con índices

Las rutas de acceso extraídas al indexar datos facilitan la tarea de buscar el índice al procesar una consulta. Al comparar la cláusula `WHERE` de una consulta con la lista de rutas de acceso indexadas, es posible identificar rápidamente los elementos que coinciden con el predicado de consulta.

Por ejemplo, considere la consulta siguiente: `SELECT location FROM location IN company.locations WHERE location.country = 'France'`. El predicado de consulta (filtrado por elementos, donde cualquier ubicación tiene "France" como su país) coincidiría con la ruta de acceso resaltada en rojo a continuación:

![Coincidencia con una ruta de acceso específica dentro de un árbol](./media/index-overview/matching-path.png)

> [!NOTE]
> Una cláusula `ORDER BY` que ordena por una sola propiedad *siempre* necesita un índice de rango y dará error si la ruta de acceso a la que hace referencia no tiene uno. De forma similar, una consulta con varias cláusulas `ORDER BY`*siempre* necesita un índice compuesto.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información acerca de la indexación en los siguientes artículos:

- [Directiva de indexación](index-policy.md)
- [Cómo administrar la directiva de indexación](how-to-manage-indexing-policy.md)
