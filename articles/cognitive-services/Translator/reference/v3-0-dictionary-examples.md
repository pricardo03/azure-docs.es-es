---
title: Método de ejemplos de diccionario de Translator Text API
titlesuffix: Azure Cognitive Services
description: Use el método de ejemplos de diccionario de Translator Text API.
services: cognitive-services
author: v-pawal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: d48349b802023d9a05bf14898440837b7793715d
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2019
ms.locfileid: "59578282"
---
# <a name="translator-text-api-30-dictionary-examples"></a>Translator Text API 3.0: Ejemplos de diccionario

Proporciona ejemplos que muestran cómo se usan los términos del diccionario en el contexto. Esta operación se usa junto con la opción [Búsqueda de diccionario](./v3-0-dictionary-lookup.md).

## <a name="request-url"></a>URL de la solicitud

Envíe una solicitud `POST` a:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0
```

## <a name="request-parameters"></a>Parámetros de solicitud

Los parámetros de solicitud que se pasaron en la cadena de consulta son:

<table width="100%">
  <th width="20%">Parámetro de consulta</th>
  <th>DESCRIPCIÓN</th>
  <tr>
    <td>api-version</td>
    <td>*Parámetro obligatorio*.<br/>Versión de la API que el cliente solicitó. El valor debe ser `3.0`.</td>
  </tr>
  <tr>
    <td>De</td>
    <td>*Parámetro obligatorio*.<br/>Especifica el idioma del texto de entrada. El idioma de origen debe ser uno de los [idiomas admitidos](./v3-0-languages.md) que están incluidos en el ámbito `dictionary`.</td>
  </tr>
  <tr>
    <td>to</td>
    <td>*Parámetro obligatorio*.<br/>Especifica el idioma del texto de salida. El idioma de origen debe ser uno de los [idiomas admitidos](./v3-0-languages.md) que están incluidos en el ámbito `dictionary`.</td>
  </tr>
</table>

Los encabezados de solicitud incluyen lo siguiente:

<table width="100%">
  <th width="20%">encabezados</th>
  <th>DESCRIPCIÓN</th>
  <tr>
    <td>Encabezados de autenticación</td>
    <td><em>Encabezado de solicitud obligatorio</em>.<br/>Consulte las <a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">opciones disponibles para la autenticación</a>.</td>
  </tr>
  <tr>
    <td>Content-Type</td>
    <td>*Encabezado de solicitud obligatorio*.<br/>Especifica el tipo de contenido de la carga. Los valores posibles son: `application/json`.</td>
  </tr>
  <tr>
    <td>Content-Length</td>
    <td>*Encabezado de solicitud obligatorio*.<br/>Longitud del cuerpo de la solicitud.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*Opcional*.<br/>GUID generado por el cliente para identificar de forma única la solicitud. Puede omitir este encabezado si incluye el id. de seguimiento en la cadena de la consulta mediante un parámetro de consulta denominado `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>Cuerpo de la solicitud

El cuerpo de la solicitud es una matriz JSON. Cada elemento de la matriz es un objeto JSON que consta de las siguientes propiedades:

  * `Text`: cadena que especifica el término que se va a buscar. Este debería ser el valor de un campo `normalizedText` que proceda de las traducciones inversas encontradas cuando se realizó una solicitud de una [búsqueda de diccionario](./v3-0-dictionary-lookup.md) anterior. También puede ser el valor del campo `normalizedSource`.

  * `Translation`: cadena que especifica el texto traducido que previamente devolvió la operación [Búsqueda de diccionario](./v3-0-dictionary-lookup.md). Este debería ser el valor del campo `normalizedTarget` en la lista `translations` de la respuesta [Búsqueda de diccionario ](./v3-0-dictionary-lookup.md). El servicio devolverá ejemplos para el par de palabras específico "fuente-objetivo".

Ejemplo:

```json
[
    {"Text":"fly", "Translation":"volar"}
]
```

Se aplican las siguientes limitaciones:

* La matriz puede tener como máximo 10 elementos.
* El valor de texto de un elemento de la matriz no puede superar los 100 caracteres, incluyendo los espacios.

## <a name="response-body"></a>Response body

Una respuesta correcta es una matriz JSON con un resultado para cada cadena en la matriz de entrada. Un objeto del resultado incluye las siguientes propiedades:

  * `normalizedSource`: cadena que proporciona la forma normalizada del término de origen. En general, esto debería ser idéntico al valor del campo `Text` en el índice de la lista de coincidencias del cuerpo de la solicitud.
    
  * `normalizedTarget`: cadena que proporciona la forma normalizada del término de destino. En general, esto debería ser idéntico al valor del campo `Translation` en el índice de la lista de coincidencias del cuerpo de la solicitud.
  
  * `examples`: lista de ejemplos del par (término de origen y de destino). Cada elemento de la lista es un objeto que consta de las siguientes propiedades:

    * `sourcePrefix`: cadena que se va a concatenar _antes_ del valor de `sourceTerm` para formar un ejemplo completo. No agregue un carácter de espacio, ya que ya estará allí en el momento apropiado. Este valor puede ser una cadena vacía.

    * `sourceTerm`: cadena equivalente al término real que se esta buscando. La cadena se agrega con `sourcePrefix` y `sourceSuffix` para formar el ejemplo completo. Su valor está separado, por lo que se puede marcar en una interfaz de usuario poniéndolo en negrita, por ejemplo.

    * `sourceSuffix`: cadena que se va a concatenar _después_ del valor de `sourceTerm` para formar un ejemplo completo. No agregue un carácter de espacio, ya que ya estará allí en el momento apropiado. Este valor puede ser una cadena vacía.

    * `targetPrefix`: cadena similar a `sourcePrefix` que se usa en el destino.

    * `targetTerm`: cadena similar a `sourceTerm` que se usa en el destino.

    * `targetSuffix`: cadena similar a `sourceSuffix` que se usa en el destino.

    > [!NOTE]
    > Si no hay ejemplos en el diccionario, la respuesta es 200 (OK), pero la lista `examples` será una lista vacía.

## <a name="examples"></a>Ejemplos

En este ejemplo se muestra cómo buscar ejemplos de la pareja formada por el término inglés `fly` y su traducción al español `volar`.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly', 'Translation':'volar'}]"
```

---

El cuerpo de la respuesta (abreviado para mayor claridad) es:

```
[
    {
        "normalizedSource":"fly",
        "normalizedTarget":"volar",
        "examples":[
            {
                "sourcePrefix":"They need machines to ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Necesitan máquinas para ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },      
            {
                "sourcePrefix":"That should really ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Eso realmente debe ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },
            //
            // ...list abbreviated for documentation clarity
            //
        ]
    }
]
```
