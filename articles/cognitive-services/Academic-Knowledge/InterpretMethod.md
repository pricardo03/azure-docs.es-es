---
title: 'Método de interpretación: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Utilice el método de interpretación para devolver interpretaciones con formato de cadenas de consulta de usuario basadas en datos de Academic Graph y Academic Grammar en Microsoft Cognitive Services.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: e16a772caa5fba632f8544094e2d8b57ed4ca765
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902576"
---
# <a name="interpret-method"></a>Método de interpretación

La API REST de **interpretación** toma una cadena de consulta de usuario final (es decir, una consulta especificada por un usuario de la aplicación) y devuelve interpretaciones con formato de intención de usuario en función de los datos de Academic Graph y de Academic Grammar.

Para proporcionar una experiencia interactiva, puede llamar a este método varias veces después de cada carácter escrito por el usuario. En ese caso, debe establecer el parámetro **complete** en 1 para habilitar las sugerencias de finalización automática. Si la aplicación no necesita la finalización automática, debe establecer el parámetro **complete** en 0.

**Punto de conexión de REST:**

    https://westus.api.cognitive.microsoft.com/academic/v1.0/interpret?

## <a name="request-parameters"></a>Parámetros de solicitud

NOMBRE     | Valor | ¿Necesario?  | DESCRIPCIÓN
---------|---------|---------|---------
**consulta**    | Cadena de texto | SÍ | Consulta especificada por el usuario.  Si complete se establece en 1, la consulta se interpretará como un prefijo para generar sugerencias de finalización automática para la consulta.        
**model**    | Cadena de texto | Sin   | Nombre del modelo que desea consultar.  Actualmente, el valor predeterminado es *latest* (más reciente).        
**complete** | 0 o 1 | Sin <br>valor predeterminado:0  | 1 significa que las sugerencias de finalización automática se generan según los datos de gráfico y la gramática.         
**count**    | Number | Sin <br>valor predeterminado:10 | Número máximo de interpretaciones de retorno.         
**offset**   | Number | Sin <br>valor predeterminado:0  | Índice de la primera interpretación de retorno. Por ejemplo, *count=2&offset=0* devuelve interpretaciones 0 y 1. *count=2&offset=2* devuelve interpretaciones 2 y 3.       
**timeout**  | Number | Sin <br>valor predeterminado:1000 | Tiempo de espera en milisegundos. Se devuelven únicamente las interpretaciones encontradas antes de que haya transcurrido el tiempo de espera.
<br>
  
## <a name="response-json"></a>Respuesta (JSON)
NOMBRE     | DESCRIPCIÓN
---------|---------
**consulta** |Parámetro *query* de la solicitud.
**interpretations** |Matriz de 0 o varias formas distintas de hacer coincidir las entradas de usuario frente a la gramática.
**interpretations[x].logprob**  |Probabilidad relativa de registro natural de la interpretación. Los valores más altos son los más probables.
**interpretations[x].parse**  |Cadena XML que muestra cómo se interpreta cada parte de la consulta.
**interpretations[x].rules**  |Matriz de 1 o más reglas definidas en la gramática invocadas durante la interpretación. Para Academic Knowledge API, siempre será 1 regla.
**interpretations[x].rules[y].name**  |Nombre de la regla.
**interpretations[x].rules[y].output**  |Resultado de la regla.
**interpretations[x].rules[y].output.type** |El tipo de datos de la salida de la regla.  Para Academic Knowledge API, siempre será "query".
**interpretations[x].rules[y].output.value**  |Resultado de la regla. Para Academic Knowledge API, esto es una cadena de expresión de consulta que se puede pasar a los métodos evaluate y calchistogram.
**aborted** | True si la solicitud ha agotado el tiempo de espera.

<br>
#### <a name="example"></a>Ejemplo:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/interpret?query=papers by jaime&complete=1&count=2
 ```
<br>La respuesta siguiente contiene las dos primeras interpretaciones (debido al parámetro *count=2*) más probables que completan la entrada de usuario parcial *papers by jaime*: *papers by jaime teevan* y *papers by jaime green*.  El servicio ha generado finalizaciones de consulta en lugar de considerar solo las coincidencias exactas para el autor *jaime* porque la solicitud especificaba *complete=1*. Observe que el valor canónico *j l green* coincide con el sinónimo *jamie green*, tal como se indica en el análisis.


```JSON
{
  "query": "papers by jaime",
  "interpretations": [
    {
      "logprob": -12.728,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#AA.AuN\">jaime teevan</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(AA.AuN=='jaime teevan')"
          }
        }
      ]
    },
    {
      "logprob": -12.774,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#AA.AuN\" canonical=\"j l green\">jaime green</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(AA.AuN=='j l green')"
          }
        }
      ]
    }
  ]
}
```  
<br>Para recuperar los resultados de la entidad para una interpretación, use *output.value* desde la API de **interpretación** y páselo a la API de **evaluación** mediante el parámetro*expr*. En este ejemplo, la consulta de la primera interpretación es: 
```
evaluate?expr=Composite(AA.AuN=='jaime teevan')
```
 