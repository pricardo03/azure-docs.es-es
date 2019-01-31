---
title: Método evaluate de Knowledge Exploration Service API
titlesuffix: Azure Cognitive Services
description: Aprenda a usar el método evaluate de Knowledge Exploration Service (KES) API.
services: cognitive-services
author: bojunehsu
manager: cgronlun
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: d6ddb76954108c037dc4135393df8622c9dbd101
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55220780"
---
# <a name="evaluate-method"></a>Método evaluate

El método *evaluate* evalúa y devuelve la salida de una expresión de consulta estructurada en función de los datos de índice.

Normalmente, una expresión se obtiene de una respuesta al método interpret.  Pero también puede crear expresiones de consulta por su cuenta (consulte [Expresión de consulta estructurada](Expressions.md)).  

## <a name="request"></a>Solicitud 

`http://<host>/evaluate?expr=<expr>&attributes=<attrs>[&<options>]`   

NOMBRE|Valor|DESCRIPCIÓN
----|----|----
expr       | Cadena de texto | Expresión de consulta estructurada que selecciona un subconjunto de entidades de índice.
attributes | Cadena de texto | Lista delimitada por comas de atributos para incluir en la respuesta.
count      | Número (valor predeterminado= 10) | El número máximo de resultados que se devolverán.
Offset     | Número (valor predeterminado= 0) | Índice del primer resultado que se va a devolver.
orderby |   Cadena de texto | Nombre del atributo que se usa para ordenar los resultados, seguido por un criterio de ordenación opcional (valor predeterminado= asc): "*attrname*[:(asc&#124;desc)]".  Si no se especifica, los resultados se devuelven reduciendo la probabilidad logarítmica natural.
timeout  | Número (valor predeterminado=1000) | Tiempo de espera en milisegundos. Se devuelven únicamente los resultados calculados antes de que haya transcurrido el tiempo de expiración.

Mediante los parámetros *count* y *offset*, puede obtenerse un gran número de resultados incrementalmente sobre varias solicitudes.
  
## <a name="response-json"></a>Respuesta (JSON)
JSONPath|DESCRIPCIÓN
----|----
$.expr | El parámetro *expr* de la solicitud.
$.entities | Matriz de 0 o más entidades de objetos que coinciden con la expresión de consulta estructurada. 
$.aborted | True si la solicitud ha agotado el tiempo de espera.

Cada entidad contiene un valor *logprob* y los valores de los atributos solicitados.

## <a name="example"></a>Ejemplo
En el ejemplo de publicaciones académicas, la siguiente solicitud pasa una expresión de consulta estructurada (posiblemente desde la salida de una solicitud *interpret*) y recupera unos cuantos atributos para las 2 entidades coincidentes principales:

`http://<host>/evaluate?expr=Composite(Author.Name=='jaime teevan')&attributes=Title,Y,Author.Name,Author.Id&count=2`

La respuesta contiene las 2 entidades principales ("count=2") que es más probable que coincidan.  Para cada entidad se devuelven los atributos de título, año, nombre del autor e identificador del autor.  Observe cómo la estructura de los valores de atributo compuestos coincide con la forma en que se especifican en el archivo de datos. 

```json
{
  "expr": "Composite(Author.Name=='jaime teevan')",
  "entities": 
  [
    {
      "logprob": -6.645,
      "Ti": "personalizing search via automated analysis of interests and activities",
      "Y": 2005,
      "Author": [
        {
          "Name": "jaime teevan",
          "Id": 1968481722
        },
        {
          "Name": "susan t dumais",
          "Id": 676500258
        },
        {
          "Name": "eric horvitz",
          "Id": 1470530979
        }
      ]
    },
    {
      "logprob": -6.764,
      "Ti": "the perfect search engine is not enough a study of orienteering behavior in directed search",
      "Y": 2004,
      "Author": [
        {
          "Name": "jaime teevan",
          "Id": 1982462162
        },
        {
          "Name": "christine alvarado",
          "Id": 2163512453
        },
        {
          "Name": "mark s ackerman",
          "Id": 2055132526
        },
        {
          "Name": "david r karger",
          "Id": 2012534293
        }
      ]
    }
  ]
}
```
