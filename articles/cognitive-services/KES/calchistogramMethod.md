---
title: 'Método CalcHistogram: Knowledge Exploration Service API'
titlesuffix: Azure Cognitive Services
description: Aprenda a usar el método calcHistogram en Knowledge Exploration Service (KES) API.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: aaa5b3a85c08f11d821557257de451b8ffc8a3fc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60814180"
---
# <a name="calchistogram-method"></a>Método calcHistogram
El método *calcHistogram* calcula los objetos que coinciden con una expresión de consulta estructurada y estima la distribución de sus valores de atributo.

## <a name="request"></a>Solicitud
`http://<host>/calchistogram?expr=<expr>[&options]` 

NOMBRE|Valor|DESCRIPCIÓN
----|-----|-----------
expr | Cadena de texto | Una expresión de consulta estructurada que especifica las entidades de índice sobre las que se calculan los histogramas.
attributes | Cadena de texto (valor predeterminado= "") | Lista delimitada por comas de atributos para incluir en la respuesta.
count   | Número (valor predeterminado= 10) | Número de resultados que se van a devolver.
Offset  | Número (valor predeterminado= 0) | Índice del primer resultado que se va a devolver.

## <a name="response-json"></a>Respuesta (JSON)
JSONPath | DESCRIPCIÓN
----|----
$.expr | El parámetro *expr* de la solicitud.
$.num_entities | Número total de entidades coincidentes.
$.histograms |  Matriz de histogramas, una para cada atributo solicitado.
$.histograms[\*].attribute | Nombre del atributo sobre el que se calculó el histograma.
$.histograms[\*].distinct_values | Número de valores distintos entre entidades coincidentes para este atributo.
$.histograms[\*].total_count | Número total de instancias de valor entre entidades coincidentes para este atributo.
$.histograms[\*].histogram | Datos de histograma para este atributo.
$.histograms[\*].histogram[\*].value | Valor de atributo.
$.histograms[\*].histogram[\*].logprob  | Probabilidad total de registro natural de entidades coincidentes con este valor de atributo.
$.histograms[\*].histogram[\*].count    | Número de entidades coincidentes con este valor de atributo.
$.aborted | True si la solicitud ha agotado el tiempo de espera.

### <a name="example"></a>Ejemplo
En el ejemplo de publicaciones académicas, se calcula a continuación un histograma de recuentos de publicación por año y por palabra clave de un autor determinado desde 2013:

`http://<host>/calchistogram?expr=And(Composite(Author.Name=='jaime teevan'),Year>=2013)&attributes=Year,Keyword&count=4`

La respuesta indica que hay 37 documentos que coinciden con la expresión de consulta.  Para el atributo *Year*, hay 3 valores distintos, uno por cada año desde 2013.  El recuento total de documentos sobre los 3 valores distintos es 37.  Para cada *Year*, en el histograma se muestra el valor, la probabilidad total de registro natural y el recuento de entidades coincidentes.     

El histograma para *Keyword* indica que hay 34 palabras clave distintas. Como un documento puede estar asociado a varias palabras clave, el recuento total (53) puede ser mayor que el número de entidades coincidentes.  Aunque haya 34 valores distintos, la respuesta incluye solo los 4 primeros debido al parámetro "count=4".

```json
{
  "expr": "And(Composite(Author.Name=='jaime teevan'),Y>=2013)",
  "num_entities": 37,
  "histograms": [
    {
      "attribute": "Y",
      "distinct_values": 3,
      "total_count": 37,
      "histogram": [
        {
          "value": 2014,
          "logprob": -6.894,
          "count": 15
        },
        {
          "value": 2013,
          "logprob": -6.927,
          "count": 12
        },
        {
          "value": 2015,
          "logprob": -7.082,
          "count": 10
        }
      ]
    },
    {
      "attribute": "Keyword",
      "distinct_values": 34,
      "total_count": 53,
      "histogram": [
        {
          "value": "crowdsourcing",
          "logprob": -7.142,
          "count": 9
        },
        {
          "value": "information retrieval",
          "logprob": -7.389,
          "count": 4
        },
        {
          "value": "personalization",
          "logprob": -7.623,
          "count": 3
        },
        {
          "value": "mobile search",
          "logprob": -7.674,
          "count": 2
        }
      ]
    }
  ]
}
``` 
