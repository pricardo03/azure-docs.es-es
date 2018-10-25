---
title: 'Sintaxis de búsqueda JSON: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre la sintaxis de búsqueda JSON que puede usar en Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 5ece028f89ad9e93840211383db97a5d8a80069a
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900417"
---
# <a name="json-search-syntax"></a>Sintaxis de búsqueda JSON

```javascript
/* Query Object:
   Suppose we have a query path /v0/e0/v1/e1/...
   A query object contains constraints to be applied to graph nodes in a path.
   Constraints are given in <"node identifier", {constraint object}> key-value pairs: 
*/
{
    /* query path can also be set in the query object */
    path: "/v0/e0/v1/e1/.../vn/",
    v0: { /* A Constraint Object */ },
    v1: { /* A Constraint Object */ },
}
```

Los nombres de nodo de una ruta de acceso de consulta (_v0, v1, ..._) actúan como identificadores de nodo a los que se puede hacer referencia en el objeto de consulta; los nombres de borde (_e0, e1, ..._) de la ruta de acceso representan los tipos de los bordes correspondientes. Se puede usar un asterisco _*_ como nombre de nodo o borde (excepto para el nodo de inicio, que debe proporcionarse) para declarar que no hay ninguna restricción en ese elemento. Por ejemplo, una ruta de acceso de consulta `/v0/*/v1/e1/*/` recupera rutas de acceso de los grafos sin restringir el tipo de borde _(v0, v1)_. Mientras tanto, la consulta tampoco tiene restricciones en el destino (el último nodo) de la ruta de acceso.

Cuando una ruta de acceso solo contiene un único nodo, por ejemplo, _v0_, la consulta devolverá simplemente todas las entidades que satisfacen las restricciones. Un objeto de restricción que se aplica al nodo de inicio se llama *objeto de consulta de inicio*, cuya especificación es tal como se indica a continuación.

```javascript
/* Starting Query Object:
   Specifies constraints on the starting node
*/
{
    /* "match" operator searches for entities with the specified properties. 
       All properties defined in the graph schema be queried such as "Name" and "NormalizedTitle".
     */
    "match": { 
        "Name" : "some name",
        ...
    },
    /* "id" operator directly specifies the IDs of the starting nodes. 
       When it is present, the "match" operator is ignored. 
     */
    "id": [ id1, id2, id3... ],
    /* "type" operator limits results to a certain type of entities,
       for example, "Author" or "Paper".
     */
    "type": "Author",
    /* "select" operator pulls properties from the database and 
       returns them to the client.
     */
    "select": ["PaperRank", ...]
}
```

Cuando una ruta de acceso contiene algo más que un nodo de inicio, el procesador de consultas realizará un recorrido de grafo siguiendo el patrón de ruta de acceso especificado. Cuando llegue a un nodo, se desencadenarán las acciones de recorrido especificadas por el usuario, es decir, detenerse en el nodo actual y volver o continuar explorando el grafo. Cuando no se especifica ninguna acción de recorrido, se realizarán las acciones predeterminadas. En un nodo intermedio, la acción predeterminada será continuar la exploración del grafo. En el último nodo de una ruta de acceso, la acción predeterminada será detenerse y volver. Un objeto de restricción que especifica acciones de recorrido se denomina *objeto de acción de recorrido*. Su especificación es como sigue:

```javascript
/* Traversal Action Object:
   Specifies graph traversal actions on a node (except for the starting node).
 */
{
    /* Set the continue condition here. */
    continue: { 
        /* simple property exact match */
        "property_key" : "property_value", 
        /* Advanced query operators */
        /* -- Numerical comparisons */
        "property_key_2" : { "gt" /* or simply ">" */ : 123 },
        /* -- Substring query */
        "property_key_3" : { "substring" : "456" },
        /* -- CellID query */
        "id": [ 111, 222, 333... ],
        /* -- Entity type query */
        "type": "cell_type",
        /* -- Property existance query */
        "has" : "property_key_4",
        /* -- Logical operators */
        /* ---- Note that, by default the operators are combined with AND semantics */
        
        /* -- OR operator */
        "or": {
          /* Query operators... */
        },
        /* -- NOT operator */
        "not": {
            /* Query operators... */
        }
    },
    /* Set the return condition here. */
    return: {
        /* Same operators as the continue object */
    },
    /* The selected properties to return. */
    select: ["property_key_1", ...]
}
```

El cuerpo de POST de una consulta de búsqueda *json* debe contener como mínimo un patrón de *ruta de acceso*. Los objetos de acción de recorrido son opcionales. Estos son dos ejemplos.

```JSON
{
  "path": "/series/ConferenceInstanceIDs/conference/FieldOfStudyIDs/field",
  "series": {
    "type": "ConferenceSeries",
    "FullName": "graph",
    "select": [ "FullName", "ShortName" ]
  },
  "conference": {
    "type": "ConferenceInstance",
    "select": [ "FullName" ]
  },
  "field": {
    "type": "FieldOfStudy",
    "select": [ "Name" ],
    "return": { "Name": { "substring" : "World Wide Web" } }
  }
}
```

```JSON
{
  "path": "/author/PaperIDs/paper",
  "author": {
    "type": "Author",
    "select": [ "DisplayAuthorName" ],
    "match": { "Name": "leslie lamport" }
  },
  "paper": {
    "type": "Paper",
    "select": [ "OriginalTitle", "CitationCount" ],
    "return": { "CitationCount": { "gt": 100 } }
  }
}
```

