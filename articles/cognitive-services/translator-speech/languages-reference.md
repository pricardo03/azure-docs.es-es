---
title: Método de idiomas de Translator Speech API
titleSuffix: Azure Cognitive Services
description: Use el método de idiomas de Translator Speech API.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: conceptual
ms.date: 05/18/18
ms.author: v-jansko
ms.openlocfilehash: ac8de247f09ba3b4e38cabb565fe332eba909ccc
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2019
ms.locfileid: "54118139"
---
# <a name="translator-speech-api-languages"></a>Translator Speech API: Lenguajes

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-translator-speech-deprecation-note.md)]

Translator Speech amplía continuamente la lista de idiomas que se admiten en sus servicios. Use esta API para averiguar el conjunto de idiomas disponibles actualmente que se pueden usar con el servicio Translator Speech.

Los ejemplos de código demuestran el uso de la API para obtener los idiomas disponibles del [sitio de GitHub de Microsoft Translator](https://github.com/MicrosoftTranslator).

## <a name="implementation-notes"></a>Notas de implementación

### <a name="get-languages"></a>GET /languages 

Hay disponible un amplio conjunto de idiomas para transcribir voz, traducir el texto transcrito y producir voz sintetizada de la traducción.

Un cliente usa el parámetro de consulta `scope` para definir en qué conjuntos de idiomas está interesado.

* **Conversión de voz en texto:** use el parámetro de consulta `scope=speech` para recuperar el conjunto de idiomas disponibles a fin de transcribir la voz a texto.
* **Traducción de texto:** use el parámetro de consulta `scope=text` para recuperar el conjunto de idiomas disponibles para traducir el texto transcrito.
* **Texto a voz:**  use el parámetro de consulta `scope=tts` para recuperar el conjunto de idiomas y voces disponibles a fin de sintetizar el texto traducido a voz.

Un cliente puede recuperar varios conjuntos simultáneamente mediante la especificación de una lista de opciones separadas por comas. Por ejemplo, `scope=speech,text,tts`.

Una respuesta correcta es un objeto JSON con una propiedad para cada conjunto solicitado.

```
{
    "speech": {
        //... Set of languages supported for speech-to-text
    },
    "text": {
        //... Set of languages supported for text translation
    },
    "tts": {
        //... Set of languages supported for text-to-speech
    }
}
```

Puesto que un cliente puede utilizar el parámetro de consulta `scope` para seleccionar qué conjuntos de idiomas admitidos se deben devolver, una respuesta real solo puede incluir un subconjunto de todas las propiedades que se mostraron anteriormente.

El valor proporcionado con cada propiedad es el siguiente.

### <a name="speech-to-text-speech"></a>Conversión de voz en texto (voz)

El valor asociado a la propiedad de voz a texto, `speech`, es un diccionario de pares (clave-valor). Cada clave identifica un idioma admitido de voz a texto. La clave es el identificador que el cliente pasa a la API. El valor asociado a la clave es un objeto con las siguientes propiedades:

* `name`: nombre para mostrar del idioma.
* `language`: etiqueta de idioma del idioma escrito asociado. Consulte "Traducción de texto" a continuación.
Ejemplo:

```
{
    "speech": {
        "en-US": { name: "English", language: "en" }
    }
}
```

### <a name="text-translation-text"></a>Traducción de texto (texto)

El valor asociado a la propiedad `text` también es un diccionario donde cada clave identifica un idioma admitido para la traducción de texto. El valor asociado a la clave describe el idioma:

* `name`: nombre para mostrar del idioma.
* `dir`: direccionalidad, que es `rtl` para los idiomas de derecha a izquierda o `ltr` para los idiomas de izquierda a derecha.

Ejemplo:

```
{
    "text": {
        "en": { name: "English", dir: "ltr" }
    }
}
```

### <a name="text-to-speech-tts"></a>Texto a voz (tts)

El valor asociado a la propiedad texto a voz, tts, también es un diccionario donde cada clave identifica un idioma admitido. Los atributos de un objeto de voz son:

* `displayName`: nombre para mostrar de la voz.
* `gender`: sexo de la voz (masculina o femenina).
* `locale`: etiqueta de idioma de la voz con subetiqueta de idioma principal y subetiqueta de región.
* `language`: etiqueta de idioma del idioma escrito asociado.
* `languageName`: nombre para mostrar del idioma.
* `regionName`: nombre para mostrar de la región para este idioma.

Ejemplo:

```
{
    "tts": {
        "en-US-Zira": {
            "displayName": "Zira",
            "gender": "female",
            "locale": "en-US",
            "languageName": "English",
            "regionName": "United States",
            "language": "en"
        }
}
```

### <a name="localization"></a>Localización
El servicio devuelve todos los nombres en el idioma del encabezado "Accept-Language", para todos los idiomas admitidos en la traducción de texto.

### <a name="response-class-status-200"></a>Clase de respuesta (estado de 200)
Objeto que describe el conjunto de idiomas admitidos.

Valor de ModelExample: 

Idiomas { voz (objeto, opcional), texto (objeto, opcional), tts (objeto, opcional) }

### <a name="headers"></a>encabezados

|Encabezado|DESCRIPCIÓN|Escriba|
:--|:--|:--|
X-RequestId|Valor generado por el servidor para identificar la solicitud y que se usa para solucionar problemas.|string|

### <a name="parameters"></a>Parámetros

|Parámetro|DESCRIPCIÓN|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|
|api-version    |Versión de la API que el cliente solicitó. Los valores permitidos son: `1.0`.|query|string|
|ámbito  |Conjuntos de idiomas o voces admitidos que se devolverán al cliente. Este parámetro se especifica como una lista de palabras clave separadas por comas. Las palabras clave siguientes están disponibles:<ul><li>`speech`: proporciona el conjunto de idiomas admitidos para la transcripción de voz.</li><li>`tts`: proporciona el conjunto de voces admitidas para la conversión de texto a voz.</li><li>`text`: proporciona el conjunto de idiomas admitidos para traducir texto.</li></ul>Si no se especifica un valor, el valor `scope` se establece en `text` de manera predeterminada.|query|string|
|X-ClientTraceId    |GUID generado por el cliente que se utiliza para realizar un seguimiento de una solicitud. Para facilitar la solución de problemas, los clientes deben proporcionar y registrar un nuevo valor con cada solicitud.|encabezado|string|
|Accept-Language    |Algunos de los campos de la respuesta son nombres de idiomas o regiones. Utilice este parámetro para definir el idioma en que se devuelven los nombres. El idioma se especifica al proporcionar una etiqueta de idioma BCP 47 con formato correcto. Seleccione una etiqueta de la lista de identificadores de idioma que se devuelva con el ámbito `text`. Para los idiomas no admitidos, se proporcionan los nombres en inglés.<br/>Por ejemplo, utilice el valor `fr` para solicitar nombres en francés o utilice el valor `zh-Hant` para solicitar nombres en chino tradicional.|encabezado|string|
    
### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400|Solicitud incorrecta. Compruebe los parámetros de entrada para asegurarse de que son válidos. El objeto de respuesta incluye una descripción más detallada del error.|
|429|Demasiadas solicitudes.|
|500|Se produjo un error. Si el error persiste, notifíquelo junto con el identificador de seguimiento de cliente (X-ClientTraceId) o el identificador de solicitud (X-ID).|
|503|Servidor no disponible temporalmente. Vuelva a intentar realizar la solicitud. Si el error persiste, notifíquelo junto con el identificador de seguimiento de cliente (X-ClientTraceId) o el identificador de solicitud (X-ID).|
