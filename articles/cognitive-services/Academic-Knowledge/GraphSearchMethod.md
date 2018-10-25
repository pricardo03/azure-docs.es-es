---
title: 'Método de búsqueda de grafo: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Use el método de búsqueda de grafo de Academic Knowledge API para devolver un conjunto de entidades académicas basadas en patrones de grafo concretos.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 83f29106d72f564f894c968102b703ab6bb5d8c2
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902390"
---
# <a name="graph-search-method"></a>Método de búsqueda de grafo

La API de REST de **búsqueda de grafo** se utiliza para devolver un conjunto de entidades académicas basadas en los patrones de grafo dados.  La respuesta es un conjunto de rutas de acceso de grafo que satisfacen las restricciones especificadas por el usuario. Una ruta de acceso de grafo es una secuencia intercalada de nodos y bordes de grafo con el formato _v0, e0, v1, e1, ..., vn_, donde _v0_ es el nodo de inicio de la ruta de acceso.
<br>

**Punto de conexión de REST:**  
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?
```   
<br>

## <a name="request-parameters"></a>Parámetros de solicitud  
NOMBRE     | Valor | ¿Necesario?  | DESCRIPCIÓN
-----------|-----------|---------|--------
**mode**       | Cadena de texto | SÍ | Nombre del modo que quiere usar. El valor es *json* o *lambda*.

El método de búsqueda de grafo debe llamarse a través de una solicitud HTTP POST. La solicitud POST debe incluir el encabezado de tipo de contenido: **application/json**.

##### <a name="json-search"></a>Búsqueda JSON 

Para la búsqueda *json*, el cuerpo de POST es un objeto JSON. El objeto JSON describe un patrón de ruta de acceso con restricciones especificadas por el usuario (consulte la [especificación de objeto JSON](JSONSearchSyntax.md) para la búsqueda *json*).


##### <a name="lambda-search"></a>Búsqueda Lambda

Para la búsqueda *lambda*, el cuerpo de POST es una cadena de texto sin formato. El cuerpo de POST es una cadena de consulta lambda LIKQ, que es una sola instrucción de C# (vea la [especificación de cadena de consulta](LambdaSearchSyntax.md) para la búsqueda *lambda*). 

<br>
## <a name="response-json"></a>Respuesta (JSON)
NOMBRE | DESCRIPCIÓN
-------|-----   
**results** | Una matriz de 0 o más entidades que coinciden con la expresión de la consulta. Cada entidad contiene los valores de los atributos solicitados. Este campo está presente si la solicitud se ha procesado correctamente.
**error** | Códigos de estado HTTP. Este campo está presente si se produce un error en la solicitud.
**message** | Mensaje de error. Este campo está presente si se produce un error en la solicitud.

Si una consulta no se puede procesar en _800 ms_, se devolverá un error de _tiempo de expiración_. 

<br>
#### <a name="example"></a>Ejemplo:

##### <a name="json-search"></a>Búsqueda JSON
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?mode=json
```
<br>
Para la búsqueda *json*, si se quiere obtener los documentos cuyos títulos contienen "graph engine" escritos por "bin shao", se puede especificar la consulta como se indica a continuación.

```JSON
{
  "path": "/paper/AuthorIDs/author",
  "paper": {
    "type": "Paper",
    "NormalizedTitle": "graph engine",
    "select": [
      "OriginalTitle"
    ]
  },
  "author": {
    "return": {
      "type": "Author",
      "Name": "bin shao"
    }
  }
}
```

El resultado de una consulta es una matriz de rutas de acceso de grafo. Una ruta de acceso de grafo es una matriz de objetos de nodo correspondientes a los nodos especificados en la ruta de acceso de la consulta. Estos objetos de nodo tienen al menos una propiedad *CellID*, que representa la id. de la entidad. Se pueden recuperar otras propiedades especificando los nombres de propiedad mediante el operador de selección de un [ *objeto de acción de recorrido*](JSONSearchSyntax.md).

```JSON
{
  "Results": [
    [
      {
        "CellID": 2160459668,
        "OriginalTitle": "Trinity: a distributed graph engine on a memory cloud"
      },
      {
        "CellID": 2093502026
      }
    ],
    [
      {
        "CellID": 2171539317,
        "OriginalTitle": "A distributed graph engine for web scale RDF data"
      },
      {
        "CellID": 2093502026
      }
    ],
    [
      {
        "CellID": 2411554868,
        "OriginalTitle": "A distributed graph engine for web scale RDF data"
      },
      {
        "CellID": 2093502026
      }
    ],
    [
      {
        "CellID": 73304046,
        "OriginalTitle": "The Trinity graph engine"
      },
      {
        "CellID": 2093502026
      }
    ]
  ]
}
 ```

##### <a name="lambda-search"></a>Búsqueda Lambda 

```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?mode=lambda
```
<br>
Para la búsqueda *lambda*, si se quiere obtener las id. de autor de un documento determinado, se puede escribir una consulta como la siguiente.

```
MAG.StartFrom(@"{
    type  : ""Paper"",
    match : {
        NormalizedTitle : ""trinity: a distributed graph engine on a memory cloud""
    }
}").FollowEdge("AuthorIDs").VisitNode(Action.Return)
```

El resultado de una consulta de búsqueda *lambda* también es una matriz de rutas de acceso de grafo:

```JSON
{
  "Results": [
    [
      {
        "CellID": 2160459668
      },
      {
        "CellID": 2142490828
      }
    ],
    [
      {
        "CellID": 2160459668
      },
      {
        "CellID": 2116756368
      }
    ],
    [
      {
        "CellID": 2160459668
      },
      {
        "CellID": 2093502026
      }
    ]
  ]
}
```
 
## <a name="graph-schema"></a>Esquema de grafo

El esquema de grafo es útil para escribir consultas de búsqueda de grafo. Se muestra en la figura siguiente.

![Esquema de Microsoft Academic Graph](./Images/AcademicGraphSchema.png)
