---
title: Referencia de Microsoft Translator Text API v3.0 | Microsoft Docs
description: Documentación de referencia para Microsoft Translator Text API v3.0.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: cfaa9584e833b137b417d9074fbfcf606eb21388
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380891"
---
#<a name="translator-text-api-v30"></a>Translator Text API v3.0

## <a name="whats-new"></a>Novedades

La versión 3 de Microsoft Translator Text API proporciona una API web moderna basada en JSON. Mejora la facilidad de uso y rendimiento mediante la consolidación de las características existentes en menos operaciones y proporciona nuevas características.

 * Transliteración para convertir texto en un idioma de un script a otro.
 * Traducción a varios idiomas en una sola solicitud.
 * Detección de idioma, traducción y transliteración en una sola solicitud.
 * Diccionario para buscar traducciones alternativas de un término, traducciones inversas y ejemplos que muestren los términos usados en contexto.
 * Resultados de detección de idioma más informativos.

## <a name="base-urls"></a>Direcciones URL base

Text API v3.0 está disponible en la nube siguiente:

| DESCRIPCIÓN | Region | URL base                                        |
|-------------|--------|-------------------------------------------------|
| Azure       | Global | api.cognitive.microsofttranslator.com           |


## <a name="authentication"></a>Autenticación

Suscríbase a Translator Text API en Microsoft Cognitive Services y use la clave de suscripción (disponible en Azure Portal) para realizar la autenticación. 

La manera más sencilla es pasar la clave secreta de Azure al servicio Translator utilizando el encabezado de solicitud `Ocp-Apim-Subscription-Key`.

Una alternativa es usar la clave secreta para obtener un token de autorización del servicio de token. A continuación, debe pasar el token de autorización al servicio Translator mediante el encabezado de solicitud `Authorization`. Para obtener un token de autorización, realice una solicitud `POST` a la dirección URL siguiente:

| Environment     | URL del servicio de autenticación                                |
|-----------------|-----------------------------------------------------------|
| Azure           | `https://api.cognitive.microsoft.com/sts/v1.0/issueToken` |

Estas son algunas solicitudes de ejemplo para obtener un token una vez proporcionada una clave secreta:

```
// Pass secret key using header
curl --header 'Ocp-Apim-Subscription-Key: <your-key>' --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken'
// Pass secret key using query string parameter
curl --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken?Subscription-Key=<your-key>'
```

Una solicitud correcta devuelve el token de acceso codificado como texto sin formato en el cuerpo de respuesta. El token válido se pasa al servicio Translator como un token de portador en la autorización.

```
Authorization: Bearer <Base64-access_token>
```

Un token de autenticación tiene una validez de 10 minutos. El token debe volver a usarse al realizar varias llamadas a las API de Translator. Sin embargo, si el programa realiza las solicitudes a la API de Translator durante un período de tiempo prolongado, el programa debe solicitar un nuevo token de acceso a intervalos regulares (por ejemplo, cada 8 minutos).

En resumen, una solicitud de cliente a la API de Translator incluirá un encabezado de autorización extraído de la tabla siguiente:

<table width="100%">
  <th width="30%">encabezados</th>
  <th>DESCRIPCIÓN</th>
  <tr>
    <td>Ocp-Apim-Subscription-Key</td>
    <td>*Úselo con la suscripción a Cognitive Services si pasa su clave secreta*.<br/>El valor es la clave secreta de Azure para su suscripción a Translator Text API.</td>
  </tr>
  <tr>
    <td>Autorización</td>
    <td>*Úselo con la suscripción a Cognitive Services si pasa un token de autenticación.*<br/>El valor es el token de portador: `Bearer <token>`.</td>
  </tr>
</table> 

## <a name="errors"></a>Errors

Una respuesta de error estándar es un objeto JSON con el par de nombre/valor denominado `error`. El valor también es un objeto JSON con propiedades:

  * `code`: código de error definido por el servidor.

  * `message`: cadena que proporciona una representación legible del error.

Por ejemplo, un cliente con una suscripción de prueba gratuita recibiría el error siguiente una vez agotada la cuota gratuita:

```
{
  "error": {
    "code":403000,
    "message":"The subscription has exceeded its free quota."
    }
}
```
