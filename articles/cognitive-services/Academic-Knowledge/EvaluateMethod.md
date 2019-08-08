---
title: 'Método de evaluación: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Use el método de evaluación para devolver un conjunto de entidades académicas basadas en una expresión de consulta.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 69e701d6727e5410b71e6cf8fbe20a1cd038ddb0
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705005"
---
# <a name="evaluate-method"></a>Método de evaluación

La API REST de **evaluación** se utiliza para devolver un conjunto de entidades académicas basadas en una expresión de consulta.
<br>

**Punto de conexión de REST:**  
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate? 
```   
<br>

## <a name="request-parameters"></a>Parámetros de solicitud  

NOMBRE     | Valor | ¿Necesario?  | DESCRIPCIÓN
-----------|-----------|---------|--------
**expr**       | Cadena de texto | Sí | Expresión de consulta que especifica qué entidades se deben devolver.
**model**      | Cadena de texto | Sin  | Nombre del modelo que desea consultar.  Actualmente, el valor predeterminado es *latest* (más reciente).        
**attributes** | Cadena de texto | Sin<br>valor predeterminado: Id | Una lista delimitada por comas que especifica los valores de atributo que se incluyen en la respuesta. Los nombres de atributo distinguen entre mayúsculas y minúsculas.
**count**        | Number | Sin<br>Valor predeterminado: 10 | Número de resultados que se van a devolver.
**offset**     | Number |   Sin<br>Valor predeterminado: 0    | Índice del primer resultado que se va a devolver.
**orderby** |   Cadena de texto | Sin<br>valor predeterminado: reducir la probabilidad | Nombre de un atributo que se utiliza para ordenar las entidades. Opcionalmente, se puede especificar el orden ascendente o descendente. El formato es: *nombre:asc* o *nombre:desc*.
  
 <br>

## <a name="response-json"></a>Respuesta (JSON)

NOMBRE | DESCRIPCIÓN
-------|-----   
**expr** |  El parámetro *expr* de la solicitud.
**entities** |  Matriz de 0 o más entidades que coinciden con la expresión de consulta. Cada entidad contiene un valor de probabilidad de logaritmo natural y los valores de otros atributos solicitados.
**aborted** | True si la solicitud ha agotado el tiempo de espera.

<br>

#### <a name="example"></a>Ejemplo:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate?expr=
Composite(AA.AuN=='jaime teevan')&count=2&attributes=Ti,Y,CC,AA.AuN,AA.AuId
```
<br>Normalmente, una expresión se obtiene de una respuesta al método **interpret**.  Pero también puede crear expresiones de consulta por su cuenta (consulte [Sintaxis de expresión de consultas](QueryExpressionSyntax.md)).  
  
Mediante el *recuento* y *desplazamiento* parámetros, puede obtenerse un gran número de resultados sin necesidad de enviar una solicitud única esa respuesta de los resultados en una enorme (y potencialmente lenta).  En este ejemplo, la solicitud ha utilizado la expresión de la primera interpretación de la respuesta de la API de **interpretación** como valor *expr*. El parámetro *count=2* especifica que se han solicitado 2 resultados de entidad. Y el parámetro *attributes=Ti,Y,CC,AA.AuN,AA.AuId* indica que para cada resultado se ha solicitado título, año, recuento de citas, nombre del autor e identificador del autor.  Consulte [Atributos de entidad](EntityAttributes.md) para ver una lista de atributos.
  
```JSON
{
  "expr": "Composite(AA.AuN=='jaime teevan')",
  "entities": 
  [
    {
      "logprob": -15.08,
      "Ti": "personalizing search via automated analysis of interests and activities",
      "Y": 2005,
      "CC": 372,
      "AA": [
        {
          "AuN": "jaime teevan",
          "AuId": 1968481722
        },
        {
          "AuN": "susan t dumais",
          "AuId": 676500258
        },
        {
          "AuN": "eric horvitz",
          "AuId": 1470530979
        }
      ]
    },
    {
      "logprob": -15.389,
      "Ti": "the perfect search engine is not enough a study of orienteering behavior in directed search",
      "Y": 2004,
      "CC": 237,
      "AA": [
        {
          "AuN": "jaime teevan",
          "AuId": 1982462162
        },
        {
          "AuN": "christine alvarado",
          "AuId": 2163512453
        },
        {
          "AuN": "mark s ackerman",
          "AuId": 2055132526
        },
        {
          "AuN": "david r karger",
          "AuId": 2012534293
        }
      ]
    }
  ]
}
 ```
 
