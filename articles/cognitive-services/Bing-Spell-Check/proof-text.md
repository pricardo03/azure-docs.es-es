---
title: ¿Qué es Bing Spell Check API?
titlesuffix: Azure Cognitive Services
description: Bing Spell Check API usa el aprendizaje automático y la traducción automática estadística para el corrector ortográfico contextual.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: overview
ms.date: 05/03/2018
ms.author: nolachar
ms.openlocfilehash: 81c80ab6c8d10d263de96566f5554709a2404a24
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48802600"
---
# <a name="what-is-bing-spell-check-api"></a>¿Qué es Bing Spell Check API?

Bing Spell Check API permite comprobar la gramática y la ortografía en contexto.

¿En qué se diferencian los correctores ortográficos normales y el corrector ortográfico de tercera generación de Bing? Los correctores ortográficos actuales se basan en la comprobación de la ortografía y la gramática en conjuntos de reglas basadas en diccionario que se actualizan actualizado y se amplían periódicamente. En otras palabras, el corrector ortográfico es tan seguro como el diccionario que lo soporta y el personal de la editorial que da soporte al diccionario. Este tipo de corrector ortográfico es como el de Microsoft Word u otros procesadores de texto.

En cambio, Bing ha desarrollado corrector ortográfico basado en web que aprovecha el aprendizaje automático y la traducción automática estadística para entrenar dinámicamente a un algoritmo en constante evolución y altamente contextual. El corrector ortográfico se basa en un gran corpus de documentos y búsquedas web.

Este corrector ortográfico puede controlar cualquier escenario de procesamiento de texto:

- Reconoce el lenguaje coloquial y el lenguaje informal
- Reconoce errores de nombres comunes en contexto
- Corrige problemas de separación de palabras con una única marca
- Puede corregir homófonos en contexto y otros errores difíciles de detectar
- Admite nuevas marcas, entretenimiento digital y expresiones populares en cuanto aparecen
- Palabras que suenan igual, pero cuyo significado y ortografía varían, como "vaca" y "baca".

## <a name="spell-check-modes"></a>Modos de comprobación ortográfica

La API admite dos modos de corrección, `Proof` y `Spell`.  Vea los ejemplos [aquí](https://azure.microsoft.com/en-us/services/cognitive-services/spell-check/).
### <a name="proof---for-documents-scenario"></a>Proof (para el escenario de documentos)
El modo predeterminado es `Proof`. El modo de corrección ortográfica `Proof` es el que proporciona las comprobaciones más completas, agrega la capitalización, la puntuación básica y otras características que ayudan en la creación de documentos. Pero está disponible solo en los mercados de en-US (inglés de Estados Unidos), es-ES (español) y pt-BR (portugués) (Nota: en el caso de español y portugués solo en versión beta). Para los restantes mercados, establezca el parámetro de consulta de modo en Ortografía. 
<br /><br/>**Nota:** si el texto de consulta tiene más de 4096 caracteres, se truncará a 4096 y luego se procesará. 
### <a name="spell----for-web-searchesqueries-scenario"></a>Spell (para el escenario de consultas y búsquedas web)
`Spell` es más agresivo para devolver mejores resultados de búsqueda. El modo `Spell` busca la mayoría de los errores ortográficos, pero no encuentra algunos de los errores gramaticales que `Proof` detecta, por ejemplo, las palabras repetidas o los errores de mayúsculas y minúsculas.
<br /></br>**Nota:** a continuación se indica la longitud de consulta máxima permitida. Si la consulta supera dicho límite, el resultado hace parecer que la consulta no se ha modificado.
<ul><li>130 caracteres para los códigos de idioma en, de, es, fr, pl, pt, sv, ru, nl, nb, tr-tr, lo, zh y ko. </li>
<li>65 caracteres para los restantes</li></ul>

## <a name="market-setting"></a>Configuración del mercado
El mercado debe especificarse en el parámetro de consulta en la dirección URL de solicitud; de lo contrario, el corrector ortográfico tomará el mercado predeterminado en función de la dirección IP.


## <a name="post-vs-get"></a>POST frente a GET

La API admite de HTTP POST o HTTP GET. Lo que se usa depende de la longitud del texto que se va a revisar. Si todas las cadenas tienen menos de 1500 caracteres, usaría una operación GET. Pero si desea admitir cadenas de hasta 10 000 caracteres, usaría POST. La cadena de texto puede incluir cualquier carácter UTF-8 válido.

El ejemplo siguiente muestra una solicitud POST para comprobar la ortografía y la gramática de una cadena de texto. El ejemplo incluye el parámetro de consulta [mode](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#mode) por razones de integridad (se podría se han omitido, ya que el valor predeterminado de `mode` es Proof). El parámetro de consulta [text](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#text) contiene la cadena que deben revisar.
  
```  
POST https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?mode=proof&mkt=en-us HTTP/1.1  
Content-Type: application/x-www-form-urlencoded  
Content-Length: 47  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
 
text=when+its+your+turn+turn,+john,+come+runing  
``` 

Si utiliza HTTP GET, incluiría el parámetro de consulta `text` en la cadena de consulta de la dirección URL
  
A continuación se muestra la respuesta a la solicitud anterior. La respuesta contiene un objeto [SpellCheck](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#spellcheck). 
  
```  
{  
    "_type" : "SpellCheck",  
    "flaggedTokens" : [{  
        "offset" : 5,  
        "token" : "its",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "it's",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 25,  
        "token" : "john",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "John",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 19,  
        "token" : "turn",  
        "type" : "RepeatedToken",  
        "suggestions" : [{  
            "suggestion" : "",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 35,  
        "token" : "runing",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "running",  
            "score" : 1  
        }]  
    }]  
}  
```  
  
El campo [flaggedTokens](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#flaggedtokens) enumera los errores de ortografía y gramática que la API encontró en la cadena [text](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#text). El campo `token` contiene la palabra que se va a reemplazar. Usaría el desplazamiento de base cero en el campo `offset` para buscar el token en la cadena `text`. Luego, reemplazaría la palabra de dicha ubicación por la palabra del campo `suggestion`. 

Si el campo `type` es RepeatedToken, puede reemplazar el token por `suggestion`, pero es probable que también necesite quitar el espacio final.

## <a name="throttling-requests"></a>Solicitudes de limitación

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Pasos siguientes

Para comenzar rápidamente con su primera solicitud, consulte [Making Your First Request](quickstarts/csharp.md) (Realizar su primera solicitud).

Familiarícese con la [referencia de Bing Spell Check API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference). La referencia contiene la lista de puntos de conexión, encabezados y parámetros de consulta que se usan para solicitar los resultados de la búsqueda y las definiciones de los objetos de la respuesta. 

No olvide leer [Requisitos de uso y visualización de Bing](./useanddisplayrequirements.md) para no infringir ninguna de las reglas relativas al uso de los resultados.
