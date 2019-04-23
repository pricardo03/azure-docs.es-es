---
title: Indexación en Azure Cosmos DB
description: Comprenda cómo funciona la indexación en Azure Cosmos DB.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: thweiss
ms.openlocfilehash: 3bb8913725acf04f71aba8b4c4350235f2c44dfb
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59996737"
---
# <a name="indexing-in-azure-cosmos-db---overview"></a>Indexación en Azure Cosmos DB: Introducción

Azure Cosmos DB es una base de datos independiente del esquema que le permite realizar una iteración en la aplicación sin tener que tratar con la administración de esquemas o índices. De forma predeterminada, Azure Cosmos DB indexa automáticamente todas las propiedades para todos los elementos de su [contenedor](databases-containers-items.md#azure-cosmos-containers) sin tener que definir ningún esquema ni configurar índices secundarios.

El objetivo de este artículo es explicar cómo Azure Cosmos DB indexa datos y cómo utiliza los índices para mejorar el rendimiento de las consultas. Se recomienda para ir a través de esta sección antes de explorar cómo personalizar [directivas de indexación](index-policy.md).

## <a name="from-items-to-trees"></a>De árboles de artículos

Cada vez que un elemento se almacena en un contenedor, su contenido se puede proyectar como un documento JSON y luego se convierte en una representación de árbol. Eso significa que todas las propiedades de ese elemento se representan como un nodo en un árbol. Se crea un nodo raíz de pseudo como primario de todas las propiedades de primer nivel del elemento. Los nodos hoja contienen los valores escalares reales pertenecientes a un elemento.

Por ejemplo, considere la posibilidad de este elemento:

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

Se podrían estar representado por el árbol de la siguiente:

![El elemento anterior representado como un árbol](./media/index-overview/item-as-tree.png)

Tenga en cuenta cómo se codifican las matrices en el árbol: cada entrada en una matriz Obtiene un nodo intermedio con la etiqueta con el índice de esa entrada dentro de la matriz (0, 1 etcetera.).

## <a name="from-trees-to-property-paths"></a>De árboles a rutas de acceso de propiedad

El motivo por qué Azure Cosmos DB transforma los elementos en árboles es porque permite que las propiedades hacer referencia a sus rutas de acceso dentro de esos árboles. Para obtener la ruta de acceso para una propiedad, podemos recorrer el árbol desde el nodo raíz a esa propiedad y concatenar las etiquetas de cada nodo del recorrido.

Estas son las rutas de acceso para cada propiedad del elemento de ejemplo que se ha descrito anteriormente:

    /locations/0/country: "Germany"
    /locations/0/city: "Berlin"
    /locations/1/country: "France"
    /locations/1/city: "Paris"
    /headquarters/country: "Belgium"
    /headquarters/employees: 250
    /exports/0/city: "Moscow"
    /exports/1/city: "Athens"

Cuando se escribe un elemento, Azure Cosmos DB indexa eficazmente ruta de acceso de cada propiedad y su valor correspondiente.

## <a name="index-kinds"></a>Tipos de índice

Actualmente, Azure Cosmos DB admite dos tipos de índices:

El **intervalo** variante de índice se utiliza para:

- consultas de igualdad: `SELECT * FROM container c WHERE c.property = 'value'`
- las consultas de intervalo: `SELECT * FROM container c WHERE c.property > 'value'` (funciona para `>`, `<`, `>=`, `<=`, `!=`)
- `ORDER BY` consultas: `SELECT * FROM container c ORDER BY c.property`
- `JOIN` consultas: `SELECT child FROM container c JOIN child IN c.properties WHERE child = 'value'`

Los índices de intervalo se pueden usar en valores escalares (cadena o número).

El **espacial** variante de índice se utiliza para:

- consultas de distancia geoespaciales: `SELECT * FROM container c WHERE ST_DISTANCE(c.property, { "type": "Point", "coordinates": [0.0, 10.0] }) < 40`
- geoespacial dentro de consultas: `SELECT * FROM container c WHERE ST_WITHIN(c.property, {"type": "Point", "coordinates": [0.0, 10.0] } })`

Los índices espaciales pueden usarse en el formato correcto [GeoJSON](geospatial.md) objetos. Actualmente se admiten puntos, LineStrings y polígonos.

## <a name="querying-with-indexes"></a>Consultas con índices

Las rutas de acceso extraídos al indizar datos facilitan la tarea buscar el índice al procesar una consulta. Comparando la `WHERE` cláusula de una consulta con la lista de rutas de acceso indizadas, es posible identificar los elementos que coinciden con el predicado de consulta muy rápidamente.

Por ejemplo, considere la siguiente consulta: `SELECT location FROM location IN company.locations WHERE location.country = 'France'`. El predicado de consulta (filtrado de elementos, donde cualquier ubicación tiene "Francia" como su país) coincidiría con la ruta de acceso que se resaltan en rojo a continuación:

![Coincidencia de una ruta de acceso específica dentro de un árbol](./media/index-overview/matching-path.png)

> [!NOTE]
> Un `ORDER BY` cláusula *siempre* necesita un rango de índice y se producirá un error si no tiene la ruta de acceso hace referencia a uno.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información acerca de la indexación en los siguientes artículos:

- [Directiva de indexación](index-policy.md)
- [Cómo administrar la directiva de indexación](how-to-manage-indexing-policy.md)
