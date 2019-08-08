---
title: Tipo de entidad compuesta
titleSuffix: Language Understanding - Azure Cognitive Services
description: Una entidad compuesta consta de otras entidades, como las entidades pregeneradas, expresiones simples y regulares, y entidades de lista. Las entidades independientes forman una entidad completa.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: 55dcc5666e63b8a87ddcb13696991fe02843fbbd
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68480112"
---
# <a name="composite-entity"></a>Entidad compuesta 

Una entidad compuesta consta de otras entidades, como las entidades precompiladas, expresiones simples y regulares, y entidades de lista. Las entidades independientes forman una entidad completa. 

**Esta entidad es la opción ideal cuando los datos:**

* Están relacionados entre sí. 
* Se relacionan entre sí en el contexto de la expresión.
* Usan una variedad de tipos de entidad.
* Deben agruparse y la aplicación cliente debe procesarlos como una unidad de información.
* Tienen una variedad de expresiones de usuario que requieren aprendizaje automático.

![entidad compuesta](./media/luis-concept-entities/composite-entity.png)

## <a name="example-json"></a>Ejemplo de JSON

Considere una entidad compuesta de las entidades `number` y `Location::ToLocation` pregeneradas con la expresión siguiente:

`book 2 tickets to paris`

Fíjese en que `2`, el número, y `paris`, el valor de ToLocation, contienen palabras entre ellos que no forman parte de ninguna de las entidades. El subrayado verde, que se usa en una expresión con etiqueta en el sitio web de [LUIS](luis-reference-regions.md), indica una entidad compuesta.

![Entidad compuesta](./media/luis-concept-data-extraction/composite-entity.png)

Las entidades compuestas se devuelven en una matriz `compositeEntities` y todas las entidades dentro de la composición también se devuelven en la matriz `entities`:

```JSON

"entities": [
    {
    "entity": "2 tickets to cairo",
    "type": "ticketInfo",
    "startIndex": 0,
    "endIndex": 17,
    "score": 0.67200166
    },
    {
    "entity": "2",
    "type": "builtin.number",
    "startIndex": 0,
    "endIndex": 0,
    "resolution": {
        "subtype": "integer",
        "value": "2"
    }
    },
    {
    "entity": "cairo",
    "type": "builtin.geographyV2",
    "startIndex": 13,
    "endIndex": 17
    }
],
"compositeEntities": [
    {
    "parentType": "ticketInfo",
    "value": "2 tickets to cairo",
    "children": [
        {
        "type": "builtin.geographyV2",
        "value": "cairo"
        },
        {
        "type": "builtin.number",
        "value": "2"
        }
    ]
    }
]
```    

|Objeto de datos|Nombre de entidad|Valor|
|--|--|--|
|Entidad creada previamente de número|"builtin.number"|"2"|
|Entidad precompilada: GeographyV2|"Location::ToLocation"|"paris"|

## <a name="next-steps"></a>Pasos siguientes

En este [tutorial](luis-tutorial-composite-entity.md), agregue una **entidad compuesta** para agrupar los datos extraídos de varios tipos en una única entidad contenedora. Mediante la agrupación de los datos, la aplicación cliente puede extraer fácilmente los datos relacionados en diferentes tipos de datos.
