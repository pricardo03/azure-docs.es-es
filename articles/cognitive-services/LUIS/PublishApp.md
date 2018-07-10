---
title: Publicar la aplicación LUIS | Microsoft Docs
description: Después de compilar y probar la aplicación mediante Language Understanding (LUIS), publíquela como servicio web en Azure.
services: cognitive-services
titleSuffix: Azure
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: 12a63e65a739be08d436f8f1b53df566255b1fb1
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321817"
---
# <a name="publish-your-trained-app"></a>Publicar la aplicación entrenada
Cuando termine de compilar y probar la aplicación LUIS, publíquela. Después de publicar la aplicación, la página de publicación muestra todos los [puntos de conexión](luis-glossary.md#endpoint) HTTP asociados. Luego, estos puntos de conexión, por [región](luis-reference-regions.md) y por [clave](Manage-Keys.md), se integran a cualquier aplicación cliente, bot de chat o back-end. 

Siempre puede [probar](interactive-test.md) la aplicación antes de publicarla. 

## <a name="production-and-staging-slots"></a>Espacios de ensayo y producción
Puede publicar la aplicación en el **espacio de ensayo** o en el **espacio de producción**. Si usa dos espacios de publicación, puede tener dos versiones distintas con puntos de conexión publicados o la misma versión en dos puntos de conexión diferentes. 

<!-- TBD: what is the technical difference? log files, endpoint quota? -->

## <a name="settings-configuration-requires-publishing-model"></a>Las opciones de configuración requieren un modelo de publicación
Publique en el punto de conexión después de realizar cambios en las siguientes configuraciones. 

## <a name="external-services-settings"></a>Valores de configuración de servicios externos
Los valores de configuración de servicios externos incluyen el **[Análisis de sentimiento](#enable-sentiment-analysis)** y la **[preparación para la voz](#enable-speech-priming)**.

### <a name="enable-sentiment-analysis"></a>Habilitar el análisis de sentimiento
En **Configuración de servicios externos**, la casilla de verificación **Enable Sentiment Analysis** (Habilitar Análisis de sentimiento) permite que LUIS se integre con [Text Analytics](https://azure.microsoft.com/services/cognitive-services/text-analytics/) para proporcionar un análisis de frases clave y opiniones. No tiene que proporcionar una clave de Text Analytics y no se carga ningún costo de facturación a su cuenta de Azure por este servicio. Una vez seleccionada esta configuración, es persistente. 

Los datos de opinión son una puntuación entre 1 y 0 que indica el valor de opinión positiva (más cercano a 1) o negativa (más cercano a 0) de los datos.

Para obtener más información acerca de la respuesta del punto de conexión JSON con análisis de sentimiento, consulte [Análisis de sentimiento](luis-concept-data-extraction.md#sentiment-analysis).

### <a name="enable-speech-priming"></a>Habilitar la preparación para la voz 
En **Configuración de servicios externos**, la casilla de verificación **Enable Speech Priming** (Habilitar preparación para la voz) le permite tener un solo punto de conexión para obtener una expresión hablada de un bot de chat o una aplicación que llama a LUIS, y recibir la respuesta de predicción de LUIS. La preparación para la voz utiliza [Speech API](../Speech-Service/rest-apis.md) de Cognitive Services. 

![Imagen del diálogo de confirmación de preparación para la voz](./media/luis-how-to-publish-app/speech-prime-modal.png)

Una vez habilitada esta característica, publique la aplicación. Cuando publica la aplicación LUIS, el modelo de aplicación se envía a su propio servicio de voz para prepararlo. La información del modelo **no** se usa fuera de su propio servicio. 

Para completar el uso de la preparación para la voz, necesitará la siguiente información para utilizarla en el [SDK de Voz](../speech-service/speech-sdk-reference.md):
* Una clave de suscripción de LUIS.
* El id. de la aplicación LUIS.
* Un dominio de punto de conexión, conocido como "nombre de host" en el SDK de Voz, como "westus.api.cognitive.microsoft.com", donde el primer subdominio sea la región donde se publica la aplicación.

Para obtener más información, consulte el ejemplo de [conversión de voz en intención](http://aka.ms/speechsdk).

Cuando se elimina la aplicación LUIS o el servicio Voz, se eliminan los datos del modelo. 

## <a name="endpoint-url-settings"></a>Configuración de la dirección URL de punto de conexión
Los valores de configuración del servicio de dirección URL de punto de conexión incluyen el desplazamiento de **[zona horaria](#set-timezone-offset)**, **[todas las puntuaciones de intención pronosticada](#include-all-predicted-intent-scores)** y el **[corrector ortográfico de Bing](#enable-bing-spell-checker)**.

### <a name="set-timezone-offset"></a>Establecer desplazamiento de zona horaria 
Una parte de la opción de espacio es la selección de zona horaria. Esta configuración de zona horaria permite a LUIS [modificar](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) cualquier valor de tiempo de datetimeV2 creado previamente durante la predicción, de manera que los datos de entidad devueltos sean correctos según la zona horaria seleccionada. 

### <a name="include-all-predicted-intent-scores"></a>Incluir todas las puntuaciones de intención pronosticada
La casilla de verificación **Include all predicted intent scores** (Incluir todas las puntuaciones de intención pronosticada) permite que la respuesta a la consulta del punto de conexión incluya la puntuación de predicción para cada intención. 

Esta opción permite que el bot de chat o la aplicación que llama a LUIS tome decisiones mediante programación basadas en las puntuaciones de las intenciones devueltas. Por lo general, las dos primeras intenciones son las más interesantes. Si la mejor puntuación es la intención None, el bot de chat puede hacer una pregunta de seguimiento que elegirá definitivamente entre la intención None y la otra intención con una puntuación alta. 

Las intenciones y sus puntuaciones también se incluyen en los registros de punto de conexión. También puede [exportar](create-new-app.md#export-app) esos registros y analizar los resultados. 

```
{
  "query": "book a flight to Cairo",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.5223427
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.5223427
    },
    {
      "intent": "BookFlight",
      "score": 0.372391433
    }
  ],
  "entities": []
}
```

### <a name="enable-bing-spell-checker"></a>Habilitar el corrector ortográfico de Bing 
En **Endpoint url settings** (Configuración de dirección URL de punto de conexión), la casilla de verificación **Enable Bing spell checker** (Habilitar corrector ortográfico de Bing) permite a LUIS corregir las palabras mal escritas antes de realizar la predicción. Esto requiere que se cree una **[clave de Bing Spell Check](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)**. Una vez creada la clave, se agregan dos parámetros de cadena de consulta a la dirección URL de punto de conexión en la página de publicación. 

Si crea sus propias direcciones URL para la aplicación que llama a LUIS, compruebe el parámetro de cadena de consulta **spellCheck=true** y **bing-spell-check-subscription-key={YOUR_BING_KEY_HERE}**. Reemplace `{YOUR_BING_KEY_HERE}` con su clave del corrector ortográfico de Bing.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```

## <a name="publish-your-trained-app-to-an-http-endpoint"></a>Publicar la aplicación entrenada en un punto de conexión HTTP
Haga clic en el nombre de la aplicación para abrirla desde la página **Mis aplicaciones** y, después, haga clic en **Publicar** en el panel superior. 

![Página de publicación](./media/luis-how-to-publish-app/publish-to-production.png)
 
Cuando la aplicación se publica correctamente, aparece un cuadro de notificación verde en la parte superior del navegador. 

* Elija publicar en **Producción** o **Ensayo** seleccionando la opción en el menú desplegable bajo **Seleccionar la ranura**. 

## <a name="assign-key"></a>Asignar una clave

Si quiere asignar una clave distinta a la clave gratuita Starter_Key que se muestra, haga clic en el botón **Agregar clave**. Esta acción abre un cuadro de diálogo que le permite seleccionar una clave de punto de conexión existente para asignarla a la aplicación. Para obtener más información sobre cómo crear y agregar las claves de punto de conexión a la aplicación LUIS, consulte [Administrar claves](Manage-Keys.md).

Consulte los puntos de conexión y las claves asociadas a otras regiones usando los botones de opción para cambiar entre regiones. Cada fila de la tabla **Resources and Keys** (Recursos y claves) enumera los recursos de Azure asociados con su cuenta y las claves de punto de conexión asociadas con ese recurso.

## <a name="endpoint-url-construction"></a>Creación de la dirección URL de punto de conexión
La dirección URL de punto de conexión corresponde a la región de Azure asociada a la clave del punto de conexión.

Esta tabla refleja cómodamente la configuración de publicación en el punto de conexión de la dirección URL con opciones de ruta y valores de cadena de consulta. Si crea las direcciones URL de los puntos de conexión para su aplicación que llama a LUIS, asegúrese de que estas mismas rutas y valores de cadena de consulta estén establecidos para el punto de conexión utilizado (si quiere que estén establecidos).

La ruta de dirección URL se crea con la región y el id. de la aplicación. Si publica en otras regiones o con otras aplicaciones, puede crear la dirección URL del punto de conexión cambiando los valores de la región y el id. de la aplicación. 

* Seleccione el espacio de producción y presione el botón **Publicar**. Cuando la publicación se complete correctamente, use la dirección URL de punto de conexión que se muestra para acceder a la aplicación LUIS. 

### <a name="optional-query-string-parameters"></a>Parámetros de cadena de consulta opcionales
Los siguientes parámetros de cadena de consulta se pueden utilizar con la dirección URL de punto de conexión:

<!-- TBD: what about speech priming? -->

|Cadena de consulta|Escriba|Valor de ejemplo|Propósito|
|--|--|--|--|
|verbose|boolean|true|Incluir [todas las puntuaciones de intención](#include-all-predicted-intent-scores) para la expresión|
|timezoneOffset|número (unidad en minutos)|60|Establecer un [desplazamiento de zona horaria](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) para las [entidades predeterminadas de datetimeV2](luis-reference-prebuilt-datetimev2.md)|
|spellCheck|boolean|true|[Corregir la ortografía](#enable-bing-spell-checker) de la expresión. Se usa con el parámetro de cadena de consulta bing-spell-check-subscription-key|
|bing-spell-check-subscription-key|Id. de suscripción||Se usa con el parámetro de cadena de consulta spellCheck|
|staging|boolean|false|Seleccionar un punto de conexión de ensayo o de producción|
|log|boolean|true|Agregar la consulta y los resultados al registro|


## <a name="test-your-published-endpoint-in-a-browser"></a>Probar el punto de conexión publicado en un navegador
Para probar el punto de conexión publicado, seleccione la dirección URL en la columna **Punto de conexión**. El navegador predeterminado se abre con la dirección URL generada. Establezca la consulta de prueba como el parámetro "&q" de la dirección URL. Por ejemplo, anexe `&q=Book me a flight to Boston on May 4` a la dirección URL y, después, presione ENTRAR. El navegador muestra la respuesta JSON de su punto de conexión HTTP. 

![Respuesta JSON desde un punto de conexión HTTP publicado](./media/luis-how-to-publish-app/luis-publish-app-json-response.png)

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Administrar claves](./Manage-Keys.md) para agregar claves a la aplicación LUIS y obtener información sobre la asignación de claves en regiones.
* Consulte [Entrenar y probar la aplicación](interactive-test.md) para obtener instrucciones sobre cómo probar la aplicación publicada en la consola de prueba.
