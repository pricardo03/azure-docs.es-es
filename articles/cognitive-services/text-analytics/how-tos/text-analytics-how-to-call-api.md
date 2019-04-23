---
title: Llamada a la API de Text Analytics
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre cómo llamar a la API REST de Text Analytics.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: aahi
ms.openlocfilehash: 720a6c57d4f1a6079f78244559a25018349bd378
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60011269"
---
# <a name="how-to-call-the-text-analytics-rest-api"></a>Cómo llamar a la API REST de Text Analytics

Las llamadas a **Text Analytics API** son llamadas HTTP POST o GET, que se pueden formular en cualquier lenguaje. En este artículo, se usan REST y [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) para demostrar los conceptos clave.

Cada solicitud debe incluir la clave de acceso y un punto de conexión HTTP. El punto de conexión especifica la región seleccionada durante el registro, la dirección URL del servicio y un recurso que se usa en la solicitud: `sentiment`, `keyphrases`, `languages` y `entities`. 

Recuerde que Text Analytics no tiene estado, por lo que no hay ningún recurso de datos para administrar. El texto se carga, se analiza tras la recepción, y los resultados se devuelven inmediatamente a la aplicación que realiza la llamada.

> [!Tip]
> Para las llamadas únicas con el fin de ver cómo funciona la API, puede enviar solicitudes POST desde la **consola de pruebas de API** integrada, disponible en cualquier [página de la documentación de la API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6). No hay ningún programa de instalación y los únicos requisitos son pegar en la solicitud una clave de acceso y los documentos JSON. 

## <a name="prerequisites"></a>Requisitos previos

Debe tener una [cuenta de la API de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con la API de Text Analytics, así como la [clave de acceso y el punto de conexión](text-analytics-how-to-access-key.md) que se generan de forma automática al registrarse en Cognitive Services. 

<a name="json-schema"></a>

## <a name="json-schema-definition"></a>Definición de esquema JSON

La entrada debe ser JSON en texto no estructurado sin formato. XML no se admite. El esquema es sencillo y consta de los elementos descritos en la lista siguiente. 

Actualmente puede enviar los mismos documentos para todas las operaciones de Text Analytics: opiniones, frases clave, detección de idioma e identificación de la entidad. (Es probable que el esquema varíe para cada análisis en el futuro).

| Elemento | Valores válidos | ¿Necesario? | Uso |
|---------|--------------|-----------|-------|
|`id` |El tipo de datos es una cadena, pero en la práctica, los identificadores de documento tienden a ser números enteros. | Obligatorio | El sistema usa los identificadores que proporcione para estructurar la salida. Se generan códigos de idioma, frases clave y puntuaciones de opinión para cada identificador de la solicitud.|
|`text` | Datos no estructurado texto sin formato, hasta 5120 caracteres. | Obligatorio | Para la detección de idioma, el texto se puede expresar en cualquier idioma. Para el análisis de opiniones, la extracción de frases clave y la identificación de entidades, el texto debe estar en un [idioma admitido](../text-analytics-supported-languages.md). |
|`language` | Código [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) de dos caracteres para un [idioma admitido](../text-analytics-supported-languages.md) | Varía | Análisis de opiniones, extracción de frases clave, detección de idioma y vinculación de entidades; opcional para la detección de idioma. No hay ningún error si se excluye, pero sin él se debilita el análisis. El código de idioma se debe corresponder al `text` que proporcione. |

Para obtener más información sobre los límites, vea [Información general de Text Analytics > Límites de datos](../overview.md#data-limits). 

## <a name="set-up-a-request-in-postman"></a>Configuración de una solicitud en Postman

El servicio acepta solicitudes de hasta 1 MB de tamaño. Si usa Postman (u otra herramienta de prueba de API web), configure el punto de conexión para incluir el recurso que quiera usar, y proporcione la clave de acceso en un encabezado de solicitud. Cada operación requiere que anexe el recurso apropiado para el punto de conexión. 

1. En Postman:

   + Elija **Post** como el tipo de solicitud.
   + Pegue el punto de conexión que copió de la página del portal.
   + Anexe un recurso.

   Los puntos de conexión de recursos son los siguientes (la región puede variar):

   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/sentiment`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/keyPhrases`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/languages`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/entities`

2. Establezca los tres encabezados de solicitud:

   + `Ocp-Apim-Subscription-Key`: la clave de acceso obtenida de Azure Portal.
   + `Content-Type`: application/json.
   + `Accept`: application/json.

   La solicitud debe ser similar a la captura de pantalla siguiente, en la que se asume un recurso **/keyPhrases**.

   ![Captura de pantalla de solicitud con el punto de conexión y los encabezados](../media/postman-request-keyphrase-1.png)

4. Haga clic en **Body** (Cuerpo) y elija **raw** (sin formato) para el formato.

   ![Captura de pantalla de la solicitud con la configuración del cuerpo](../media/postman-request-body-raw.png)

5. Pegue algunos documentos JSON en un formato válido para el análisis previsto. Para obtener más información sobre un análisis determinado, vea los temas siguientes:

  + [Detección de idioma](text-analytics-how-to-language-detection.md)  
  + [Extracción de frases clave](text-analytics-how-to-keyword-extraction.md)  
  + [Análisis de opiniones](text-analytics-how-to-sentiment-analysis.md)  
  + [Reconocimiento de entidades](text-analytics-how-to-entity-linking.md)  


6. Haga clic en **Send** (Enviar) para enviar la solicitud. Puede enviar hasta 100 solicitudes por minuto. 

   En Postman, la respuesta se muestra en la ventana siguiente, como un solo documento JSON, con un elemento para cada identificador de documento proporcionado en la solicitud.

## <a name="see-also"></a>Vea también 

 [Introducción a Text Analytics](../overview.md)  
 [Preguntas más frecuentes](../text-analytics-resource-faq.md)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Detección de idioma](text-analytics-how-to-language-detection.md)
