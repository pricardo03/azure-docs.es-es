---
title: 'Método CalcHistogram: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Con el método CalcHistogram se calcula la distribución de los valores de atributo para un conjunto de entidades de documento.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: 1e922f1c02876f09dcf51b192bc08c4f4fccac9e
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55879027"
---
# <a name="calchistogram-method"></a>Método CalcHistogram

La API de REST **calchistogram** se usa para calcular la distribución de valores de atributo para un conjunto de entidades de documento.          


**Punto de conexión de REST:**
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?
``` 
<br>
  
## <a name="request-parameters"></a>Parámetros de solicitud

NOMBRE  |Valor | ¿Necesario?  |DESCRIPCIÓN
-----------|----------|--------|----------
**expr**    |Cadena de texto | Sí  |Una expresión de consulta que especifica las entidades en las que se deben calcular los histogramas.
**model** |Cadena de texto | Sin  |Seleccione el nombre del modelo que quiere consultar.  Actualmente, el valor predeterminado es *latest*.
**attributes** | Cadena de texto | Sin <br>valor predeterminado: | Una lista delimitada por comas que especifica los valores de atributo que se incluyen en la respuesta. Los nombres de atributo distinguen entre mayúsculas y minúsculas.
**count** |Number | Sin <br>Valor predeterminado: 10 |Número de resultados que se van a devolver.
**offset**  |Number | Sin <br>Valor predeterminado: 0 |Índice del primer resultado que se va a devolver.
**timeout**  |Number | Sin <br>Valor predeterminado: 1000 |Tiempo de espera en milisegundos. Se devuelven únicamente las interpretaciones encontradas antes de que haya transcurrido el tiempo de espera.

## <a name="response-json"></a>Respuesta (JSON)

NOMBRE | DESCRIPCIÓN
--------|---------
**expr**  |El parámetro expr de la solicitud.
**num_entities** | Número total de entidades coincidentes.
**histograms** |  Una matriz de histogramas, uno para cada atributo especificado en la solicitud.
**histograms[x].attribute** | Nombre del atributo en el cual se calculó el histograma.
**histograms[x].distinct_values** | Número de valores distintos entre entidades coincidentes para este atributo.
**histograms[x].total_count** | Número total de instancias de valor entre entidades coincidentes para este atributo.
**histograms[x].histogram** | Datos de histograma para este atributo.
**histograms[x].histogram[y].value** |  Un valor para el atributo.
**histograms[x].histogram[y].logprob**  |Probabilidad total de registro natural de entidades coincidentes con este valor de atributo.
**histograms[x].histogram[y].count**  |Número de entidades coincidentes con este valor de atributo.
**aborted** | True si la solicitud ha agotado el tiempo de espera.


#### <a name="example"></a>Ejemplo:
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?expr=And(Composite(AA.AuN=='jaime teevan'),Y>2012)&attributes=Y,F.FN&count=4
```
<br>En este ejemplo, con el fin de generar un histograma del recuento de publicaciones por año de un autor concreto desde 2010, se puede generar en primer lugar la expresión de consulta utilizando la API **interpret** con la cadena de consulta: *papers by jaime teevan after 2012*.

```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/interpret?query=papers by jaime teevan after 2012
```
<br>La expresión de la primera interpretación que se devuelve de la API interpret es *And(Composite(AA.AuN=='jaime teevan'),Y>2012)*.
<br>Este valor de expresión se pasa, a continuación, a la API **calchistogram**. El parámetro *attributes=Y,F.FN* indica que las distribuciones de recuentos de documentos deben ser por año y campo de estudio, p. ej.:
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?expr=And(Composite(AA.AuN=='jaime teevan'),Y>2012)&attributes=Y,F.FN&count=4
```
<br>La respuesta a esta solicitud indica primero que hay 37 documentos que coinciden con la expresión de consulta.  Para el atributo *Year*, hay 3 valores distintos, uno para cada año después de 2012 (es decir, 2013, 2014 y 2015), como se especifica en la consulta.  El recuento total de documentos en los 3 valores distintos es 37.  Para cada *Year*, en el histograma se muestra el valor, la probabilidad total de registro natural y el recuento de entidades coincidentes.     

En el histograma para *Field of Study* se muestra que hay 34 campos de estudio distintos. Como un documento puede estar asociado a varios campos de estudio, el recuento total (53) puede ser mayor que el número de entidades coincidentes.  Aunque haya 34 valores distintos, la respuesta incluye solo los 4 primeros debido al parámetro *count=4*.

```JSON
{
  "expr": "And(Composite(AA.AuN=='jaime teevan'),Y>2012)",
  "num_entities": 37,
  "histograms": [
    {
      "attribute": "Y",
      "distinct_values": 3,
      "total_count": 37,
      "histogram": [
        {
          "value": 2014,
          "logprob": -15.753,
          "count": 15
        },
        {
          "value": 2013,
          "logprob": -15.805,
          "count": 12
        },
        {
          "value": 2015,
          "logprob": -16.035,
          "count": 10
        }
      ]
    },
    {
      "attribute": "F.FN",
      "distinct_values": 34,
      "total_count": 53,
      "histogram": [
        {
          "value": "crowdsourcing",
          "logprob": -15.258,
          "count": 9
        },
        {
          "value": "information retrieval",
          "logprob": -16.002,
          "count": 4
        },
        {
          "value": "personalization",
          "logprob": -16.226,
          "count": 3
        },
        {
          "value": "mobile search",
          "logprob": -17.228,
          "count": 2
        }
      ]
    }
  ]
}
```
