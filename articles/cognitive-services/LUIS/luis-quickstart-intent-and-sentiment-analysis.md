---
title: 'Tutorial para crear una aplicación de LUIS que devuelve el análisis de opiniones: Azure | Microsoft Docs'
description: En este tutorial, aprenderá a agregar el análisis de opiniones a su aplicación de LUIS para que analice expresiones para detectar sentimientos positivos, negativos y neutrales.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: v-geberr
ms.openlocfilehash: ac959989dbe64460025bfba84df7b6f22c3c1c04
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2018
ms.locfileid: "36958436"
---
# <a name="tutorial-create-app-that-returns-sentiment-along-with-intent-prediction"></a>Tutorial: Creación de una aplicación que devuelve opiniones junto con la predicción de intenciones
En este tutorial va a crear una aplicación que muestra cómo extraer opiniones positivas, negativas y neutrales de expresiones.

<!-- green checkmark -->
> [!div class="checklist"]
> * Descripción del análisis de sentimiento
> * Uso de la aplicación de LUIS en el dominio de recursos humanos (RR. HH.) 
> * Incorporación de análisis de sentimiento
> * Entrenamiento y publicación de la aplicación
> * Consulta del punto de conexión de la aplicación para ver la respuesta JSON de LUIS 

Para este artículo, necesita una cuenta de [LUIS][LUIS] gratuita para crear la aplicación de LUIS.

## <a name="before-you-begin"></a>Antes de empezar
Si no tiene la aplicación de Recursos humanos del tutorial de [entidades de keyPhrase](luis-quickstart-intent-and-key-phrase.md), [importe](create-new-app.md#import-new-app) el archivo JSON a una nueva aplicación en el sitio web [LUIS](luis-reference-regions.md#luis-website). La aplicación que se va a importar se encuentra en el repositorio de GitHub [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-keyphrase-HumanResources.json).

Si desea conservar la aplicación original de Recursos humanos, clone la versión en la página [Configuración](luis-how-to-manage-versions.md#clone-a-version) y llámela `sentiment`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. 

## <a name="sentiment-analysis"></a>análisis de opiniones
El análisis de opiniones es la capacidad para determinar si la expresión de un usuario es positiva, negativa o neutral. 

Las expresiones siguientes muestran ejemplos de opiniones:

|Opinión|Score|Expresión|
|:--|:--|:--|
|positiva|0,91 |John W. Smith hizo un gran trabajo en la presentación en París.|
|positiva|0.84 |jill-jones@mycompany.com realizó un trabajo fabuloso en el discurso de ventas de Parker.|

El análisis de sentimiento es una configuración de aplicación que se aplica a cada expresión. No es necesario encontrar las palabras que indican opinión en la expresión y etiquetarlas porque el análisis de sentimiento se aplica a toda la expresión. 

## <a name="add-employeefeedback-intent"></a>Adición de la intención EmployeeFeedback 
Agregue una nueva intención para capturar los comentarios de los miembros de la empresa. 

1. Asegúrese de que la aplicación de recursos humanos se encuentra en la sección **Build** (Crear) de LUIS. Para cambiar a esta sección, seleccione **Build** (Crear) en la barra de menús superior derecha. 

    [ ![Captura de pantalla de la aplicación de LUIS con Build (Crear) resaltado en la barra de navegación superior derecha](./media/luis-quickstart-intent-and-sentiment-analysis/hr-first-image.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-first-image.png#lightbox)

2. Seleccione **Create new intent** (Crear nueva intención).

    [ ![Captura de pantalla de la aplicación de LUIS con Build (Crear) resaltado en la barra de navegación superior derecha](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent.png#lightbox)

3. Escriba el nombre de la intención nueva `EmployeeFeedback`.

    ![Cuadro de diálogo Create new intent (Crear nueva intención) con EmployeeFeedback como nombre](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent-ddl.png)

4. Agregue varias expresiones que indiquen que un empleado está haciendo algo bien o un área que necesita mejorar:

    Recuerde que en esta aplicación de Recursos Humanos, los empleados se definen en la entidad de lista, `Employee`, por nombre, correo electrónico, número de extensión telefónica, número de teléfono móvil y su número del seguro social federal de Estados Unidos. 

    |Grabaciones de voz|
    |--|
    |425-555-1212 hizo un buen trabajo al darle la bienvenida a una compañera de trabajo después del permiso de maternidad.|
    |234-56-7891 hizo un gran trabajo al consolar a un compañero de trabajo en momentos de dolor.|
    |jill-jones@mycompany.com no tenía todas las facturas necesarias para los trabajos administrativos.|
    |john.w.smith@mycompany.com entregó los formularios necesarios con un mes de retraso y sin firmas.|
    |x23456 no llegó a la importante reunión de marketing fuera de las instalaciones.|
    |x12345 se perdió la reunión para las revisiones de junio.|
    |Jill Jones fue la estrella de las ventas en Harvard.|
    |John W. Smith hizo un gran trabajo en la presentación en Stanford.|

    [ ![Captura de pantalla de la aplicación de LUIS con expresiones de ejemplo en la intención EmployeeFeedback](./media/luis-quickstart-intent-and-sentiment-analysis/hr-utterance-examples.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-utterance-examples.png#lightbox)

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS
LUIS no sabe nada acerca de la nueva intención ni de sus expresiones de ejemplo hasta que se le entrena. 

1. En la parte superior derecha del sitio web de LUIS, seleccione el botón **Train** (Entrenar).

    ![Captura de pantalla del botón Train (Entrenar) resaltado](./media/luis-quickstart-intent-and-sentiment-analysis/train-button.png)

2. El entrenamiento se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

    ![Captura de pantalla de la barra de notificaciones de entrenamiento correcto ](./media/luis-quickstart-intent-and-sentiment-analysis/hr-trained-inline.png)

## <a name="configure-app-to-include-sentiment-analysis"></a>Configuración de la aplicación para que incluya el análisis de opiniones
Configure el análisis de sentimiento en la página **Publish** (Publicar). 

1. Seleccione **Publish** (Publicar) en el panel de navegación superior derecho.

    ![Captura de pantalla de la página Intents (Intenciones) con el botón Publish (Publicar) expandido ](./media/luis-quickstart-intent-and-sentiment-analysis/hr-publish-button-in-top-nav-highlighted.png)

2. Seleccione **Enable Sentiment Analysis** (Habilitar análisis de sentimiento). Seleccione el espacio de producción y el botón **Publish** (Publicar).

    [![](media/luis-quickstart-intent-and-sentiment-analysis/hr-publish-to-production-expanded.png "Captura de pantalla de la página Publish (Publicar) con el botón de publicación en el espacio de producción resaltado")](media/luis-quickstart-intent-and-sentiment-analysis/hr-publish-to-production-expanded.png#lightbox)

4. La publicación se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

## <a name="query-the-endpoint-with-an-utterance"></a>Consulta del punto de conexión con una expresión

1. En la página **Publish** (Publicar), seleccione el vínculo **endpoint** (Punto de conexión) en la parte inferior de la página. Esta acción abre otra ventana del explorador con la dirección URL del punto de conexión en la barra de direcciones. 

    ![Captura de pantalla de la página Publish (Publicar) con la dirección URL del punto de conexión resaltada](media/luis-quickstart-intent-and-sentiment-analysis/hr-endpoint-url-inline.png)

2. Vaya al final de la dirección URL en la dirección y escriba `Jill Jones work with the media team on the public portal was amazing`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que cualquiera de las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `EmployeeFeedback` con el análisis de sentimiento extraído.

```
{
  "query": "Jill Jones work with the media team on the public portal was amazing",
  "topScoringIntent": {
    "intent": "EmployeeFeedback",
    "score": 0.4983256
  },
  "intents": [
    {
      "intent": "EmployeeFeedback",
      "score": 0.4983256
    },
    {
      "intent": "MoveEmployee",
      "score": 0.06617523
    },
    {
      "intent": "GetJobInformation",
      "score": 0.04631853
    },
    {
      "intent": "ApplyForJob",
      "score": 0.0103248553
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.007531875
    },
    {
      "intent": "FindForm",
      "score": 0.00344597152
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00337914471
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.0026357458
    },
    {
      "intent": "None",
      "score": 0.00214573368
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.00157622492
    },
    {
      "intent": "Utilities.Confirm",
      "score": 7.379545E-05
    }
  ],
  "entities": [
    {
      "entity": "jill jones",
      "type": "Employee",
      "startIndex": 0,
      "endIndex": 9,
      "resolution": {
        "values": [
          "Employee-45612"
        ]
      }
    },
    {
      "entity": "media team",
      "type": "builtin.keyPhrase",
      "startIndex": 25,
      "endIndex": 34
    },
    {
      "entity": "public portal",
      "type": "builtin.keyPhrase",
      "startIndex": 43,
      "endIndex": 55
    },
    {
      "entity": "jill jones",
      "type": "builtin.keyPhrase",
      "startIndex": 0,
      "endIndex": 9
    }
  ],
  "sentimentAnalysis": {
    "label": "positive",
    "score": 0.8694164
  }
}
```

El análisis de sentimiento es positivo con una puntuación de 0,86. 

## <a name="what-has-this-luis-app-accomplished"></a>¿Qué ha logrado esta aplicación de LUIS?
Esta aplicación con el análisis de sentimiento habilitado ha identificado una intención de consulta en lenguaje natural y ha devuelto los datos extraídos incluyendo el sentimiento general como una puntuación. 

Su bot de chat ya tiene ahora suficiente información para determinar el siguiente paso de la conversación. 

## <a name="where-is-this-luis-data-used"></a>¿Dónde se utilizan estos datos de LUIS? 
LUIS ha terminado con esta solicitud. La aplicación que realiza la llamada, como un bot de chat, puede tomar el resultado de topScoringIntent y los datos de opinión de la expresión para realizar el siguiente paso. LUIS no realiza este trabajo de programación para el bot o la aplicación que realiza la llamada. LUIS solo determina cuál es la intención del usuario. 

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no sea necesaria, elimine la aplicación de LUIS. Para ello, seleccione el menú de tres puntos (...) a la derecha del nombre de la aplicación en la lista de aplicaciones y seleccione **Delete** (Eliminar). En el cuadro de diálogo emergente **Delete app?** (¿Eliminar aplicación?), seleccione **Ok** (Aceptar).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Llame a la API del punto de conexión de LUIS con C#](luis-get-started-cs-get-intent.md) 

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
