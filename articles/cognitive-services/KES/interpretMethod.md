---
title: Método interpret de Knowledge Exploration Service API
titlesuffix: Azure Cognitive Services
description: Aprenda a usar el método interpret de Knowledge Exploration Service (KES) API.
services: cognitive-services
author: bojunehsu
manager: cgronlun
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 45badbdbe1a7e1f2028a00d54458db35a4f7d440
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46128018"
---
# <a name="interpret-method"></a>Método interpret

El método *interpret* toma una cadena de consulta de lenguaje natural y devuelve interpretaciones con formato de la intención del usuario, en función de los datos de gramática e índice.  Para proporcionar una experiencia de búsqueda interactiva, se puede llamar a este método con cada carácter especificado por el usuario con el parámetro *complete* establecido en 1 para habilitar las sugerencias de finalización automática.

## <a name="request"></a>Solicitud

`http://<host>/interpret?query=<query>[&<options>]`

NOMBRE|Valor| DESCRIPCIÓN
----|----|----
query    | Cadena de texto | Consulta especificada por el usuario.  Si complete se establece en 1, la consulta se interpretará como un prefijo para generar sugerencias de finalización automática para la consulta.        
complete | 0 (valor predeterminado) o 1 | 1 significa que se generan sugerencias de finalización automática en función de los datos de gramática e índice.         
count    | Número (valor predeterminado= 10) | Número máximo de interpretaciones de retorno.         
Offset   | Número (valor predeterminado= 0) | Índice de la primera interpretación que se debe devolver.  Por ejemplo, *count=2&offset=0* devuelve interpretaciones 0 y 1. *count=2&offset=2* devuelve interpretaciones 2 y 3.       
timeout  | Número (valor predeterminado=1000) | Tiempo de expiración en milisegundos. Solo se devuelven las interpretaciones encontradas antes de que haya transcurrido el tiempo de expiración.

Mediante los parámetros *count* y *offset*, puede obtenerse un gran número de resultados incrementalmente en varias solicitudes.

## <a name="response-json"></a>Respuesta (JSON)

JSONPath     | DESCRIPCIÓN
---------|---------
$.query |El parámetro *query* de la solicitud.
$.interpretations   |Matriz de 0 o más maneras de que la consulta de entrada coincida con la gramática.
$.interpretations[\*].logprob   |Probabilidad relativa de registro de la interpretación (<= 0).  Los valores más altos son más probables.
$.interpretations[\*].parse |Cadena XML que muestra cómo se interpreta cada parte de la consulta.
$.interpretations[\*].rules |Matriz de 1 o más reglas definidas en la gramática invocadas durante la interpretación.
$.interpretations[\*].rules[\*].name    |Nombre de la regla.
$.interpretations[\*].rules[\*].output  |Salida semántica de la regla.
$.interpretations[\*].rules[\*].output.type |Tipo de datos de la salida semántica.
$.interpretations[\*].rules[\*].output.value|Valor de la salida semántica.  
$.aborted | True si la solicitud ha agotado el tiempo de espera.

### <a name="parse-xml"></a>XML de análisis

El XML de análisis anota la consulta (completada) con información acerca de cómo coincide con las reglas de gramática y atributos en el índice.  A continuación, se muestra un ejemplo del dominio de publicaciones académicas:

```xml
<rule name="#GetPapers">
  papers by 
  <attr name="academic#Author.Name" canonical="heungyeung shum">harry shum</attr>
  <rule name="#GetPaperYear">
    written in
    <attr name="academic#Year">2000</attr>
  </rule>
</rule>
```

El elemento `<rule>` delimita el intervalo de la consulta que coincide con la regla especificada por su atributo `name`.  Se pueden anidar cuando el análisis implica referencias de regla en la gramática.

El elemento `<attr>` delimita el intervalo de la consulta que coincide con el atributo de índice especificado por su atributo `name`.  Cuando la coincidencia implica un sinónimo en la consulta de entrada, el atributo `canonical` contendrá el valor canónico que coincida con el sinónimo del índice.

## <a name="example"></a>Ejemplo

En el ejemplo de publicaciones académicas, la siguiente solicitud devuelve hasta 2 sugerencias de finalización automática para la consulta de prefijo "papers by jaime":

`http://<host>/interpret?query=papers by jaime&complete=1&count=2`

La respuesta contiene las dos primeras ("count=2") interpretaciones más probables que completan la consulta parcial "papers by jaime": "papers by jaime teevan" y "papers by jaime green".  El servicio generó finalizaciones de consulta en lugar de considerar solo coincidencias exactas para el autor "jaime", porque la solicitud especificaba "complete=1". Observe que el valor canónico "j l green" coincide con el sinónimo "jamie green", tal como se indica en el análisis.


```json
{
  "query": "papers by jaime",
  "interpretations": [
    {
      "logprob": -5.615,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#Author.Name\">jaime teevan</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(Author.Name=='jaime teevan')"
          }
        }
      ]
    },
    {
      "logprob": -5.849,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#Author.Name\" canonical=\"j l green\">jaime green</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(Author.Name=='j l green')"
          }
        }
      ]
    }
  ]
}
```  

Cuando el tipo de salida semántica es "query", como en este ejemplo, se pueden recuperar los objetos coincidentes pasando *output.value* a la API [*evaluate*](evaluateMethod.md) a través del parámetro *expr*.

`http://<host>/evaluate?expr=Composite(AA.AuN=='jaime teevan')`
  
