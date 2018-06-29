---
title: 'Tutorial para crear una aplicación de LUIS que devuelve el análisis de opiniones: Azure | Microsoft Docs'
description: En este tutorial, aprenderá a agregar el análisis de opiniones a su aplicación de LUIS para que analice expresiones para detectar sentimientos positivos, negativos y neutrales.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: d000637312619fc493e2f7bad8e8edf0d8d0d94b
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265341"
---
# <a name="tutorial-create-app-that-returns-sentiment-along-with-intent-prediction"></a>Tutorial: Creación de una aplicación que devuelve opiniones junto con la predicción de intenciones
En este tutorial va a crear una aplicación que muestra cómo extraer opiniones positivas, negativas y neutrales de expresiones.

<!-- green checkmark -->
> [!div class="checklist"]
> * Descripción de entidades jerárquicas y elementos secundarios aprendidos por el contexto 
> * Creación de una nueva aplicación de LUIS para el dominio de los viajes con la intención Bookflight
> * Adición de la intención _None_ y adición de expresiones de ejemplo
> * Agregar la entidad jerárquica de ubicación con los elementos secundarios de origen y destino
> * Entrenamiento y publicación de la aplicación
> * Consulta del punto de conexión de la aplicación para ver la respuesta JSON de LUIS que incluye elementos secundarios jerárquicos 

Para este artículo, necesita una cuenta de [LUIS][LUIS] gratuita para crear la aplicación de LUIS.

## <a name="sentiment-analysis"></a>análisis de opiniones
El análisis de opiniones es la capacidad para determinar si la expresión de un usuario es positiva, negativa o neutral. 

Las expresiones siguientes muestran ejemplos de opiniones:

|Opinión y puntuación|Expresión|
|:--|--|
|positiva: 0,89 |El plato combinado de sopa y ensalada era estupendo.|
|negativa: 0,07 |No me gustaron los entremeses de la cena.|

El análisis de opiniones es una configuración de aplicación que se aplica a cada expresión. No es necesario que busque las palabras que indican la opinión en una expresión y les asigne etiquetas. LUIS lo hará automáticamente.

## <a name="create-a-new-app"></a>Creación de una nueva aplicación
1. Inicie sesión en el sitio web de [LUIS][LUIS]. Asegúrese de que inicia sesión en la [región][LUIS-regions] en la que necesita publicar los puntos de conexión de LUIS.

2. En el sitio web de [LUIS][LUIS], seleccione **Create new app** (Crear aplicación). 

    [![](media/luis-quickstart-intent-and-sentiment-analysis/app-list.png "Captura de pantalla de la página de lista de aplicaciones")](media/luis-quickstart-intent-and-sentiment-analysis/app-list.png#lightbox)

3. En el cuadro de diálogo **Crear nueva aplicación** asigne el nombre `Restaurant Reservations With Sentiment` a la aplicación y seleccione **Listo**. 

    ![Imagen del cuadro de diálogo Create new app (Crear nueva aplicación)](./media/luis-quickstart-intent-and-sentiment-analysis/create-app-ddl.png)

    Cuando se complete el proceso de creación de la aplicación, LUIS mostrará la lista de intenciones que contiene la intención None.

    [![](media/luis-quickstart-intent-and-sentiment-analysis/intents-list.png "Captura de pantalla de la página de lista de intenciones")](media/luis-quickstart-intent-and-sentiment-analysis/intents-list.png#lightbox)

## <a name="add-a-prebuilt-domain"></a>Incorporación de un dominio creado previamente
Agregue un dominio creado previamente para agregar rápidamente intenciones, entidades y expresiones etiquetadas.

1. Seleccione **Prebuilt Domains** (Dominios creados previamente) en el menú izquierdo.

    [ ![Captura de pantalla del botón Prebuilt Domains](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-button-inline.png)](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-button-expanded.png#lightbox) (Dominios creados previamente)

2. Seleccione **Agregar dominio** para el dominio **RestaurantReservation** creado previamente. Espere hasta que se agregue el dominio.

    [ ![Captura de pantalla de la lista de dominios creados previamente](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-inline.png)](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-expanded.png#lightbox)

3. Seleccione **Intents** (Intenciones) en el panel de navegación izquierdo. Este dominio creado previamente tiene una intención.

    [ ![Captura de pantalla de la lista de dominios creados previamente con Intents (Intenciones) resaltado en el panel de navegación izquierdo](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-domain-added-expanded.png)](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-domain-added-expanded.png#lightbox)

4.  Seleccione la intención **RestaurantReservation.Reserve**. 

    [ ![Captura de pantalla de la lista de intenciones con RestaurantReservation.Reserve resaltado](./media/luis-quickstart-intent-and-sentiment-analysis/select-intent.png)](./media/luis-quickstart-intent-and-sentiment-analysis/select-intent.png#lightbox)

5. Cambie a la **vista Entities** (Entidades) para ver las numerosas expresiones que se proporcionan con entidades etiquetadas de dominio específico.

    [ ![Captura de pantalla de la intención RestaurantReservation.Reserve con la vista Entities (Entidades) cambiando a la vista Token que aparece resaltada](./media/luis-quickstart-intent-and-sentiment-analysis/utterance-list-inline.png)](./media/luis-quickstart-intent-and-sentiment-analysis/utterance-list-expanded.png#lightbox)

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS
LUIS no conoce los cambios en las intenciones y entidades (el modelo) hasta que se le entrena. 

1. En la parte superior derecha del sitio web de LUIS, seleccione el botón **Train** (Entrenar).

    ![Captura de pantalla del botón Train (Entrenar) resaltado](./media/luis-quickstart-intent-and-sentiment-analysis/train-button-expanded.png)

2. El entrenamiento se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

    ![Captura de pantalla de la barra de notificaciones de entrenamiento correcto ](./media/luis-quickstart-intent-and-sentiment-analysis/trained-expanded.png)

## <a name="configure-app-to-include-sentiment-analysis"></a>Configuración de la aplicación para que incluya el análisis de opiniones
El análisis de opiniones está habilitado en la página **Publish** (Publicar). 

1. Seleccione **Publish** (Publicar) en el panel de navegación superior derecho.

    ![Captura de pantalla de la página Intents (Intenciones) con el botón Publish (Publicar) expandido ](./media/luis-quickstart-intent-and-sentiment-analysis/publish-expanded.png)

2. Seleccione **Enable Sentiment Analysis** (Habilitar análisis de sentimiento).

    ![Captura de pantalla de la página Publish (Publicar) con la opción Enable Sentiment Analysis (Habilitar análisis de sentimiento) resaltada ](./media/luis-quickstart-intent-and-sentiment-analysis/enable-sentiment-expanded.png)

3. Seleccione el espacio de producción y el botón **Publish** (Publicar).

    [![](media/luis-quickstart-intent-and-sentiment-analysis/publish-to-production-inline.png "Captura de pantalla de la página Publish (Publicar) con el botón de publicación en el espacio de producción resaltado")](media/luis-quickstart-intent-and-sentiment-analysis/publish-to-production-expanded.png#lightbox)

4. La publicación se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

## <a name="query-the-endpoint-with-an-utterance"></a>Consulta del punto de conexión con una expresión

1. En la página **Publish** (Publicar), seleccione el vínculo **endpoint** (Punto de conexión) en la parte inferior de la página. Esta acción abre otra ventana del explorador con la dirección URL del punto de conexión en la barra de direcciones. 

    ![Captura de pantalla de la página Publish (Publicar) con la dirección URL del punto de conexión resaltada](media/luis-quickstart-intent-and-sentiment-analysis/endpoint-url-inline.png)

2. Vaya al final de la dirección URL en la dirección y escriba `Reserve table for  10 on upper level away from kitchen`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que cualquiera de las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `RestaurantReservation.Reserve` con el análisis de sentimiento extraído.

```
{
  "query": "Reserve table for 10 on upper level away from kitchen",
  "topScoringIntent": {
    "intent": "RestaurantReservation.Reserve",
    "score": 0.9926384
  },
  "intents": [
    {
      "intent": "RestaurantReservation.Reserve",
      "score": 0.9926384
    },
    {
      "intent": "None",
      "score": 0.00961109251
    }
  ],
  "entities": [],
  "sentimentAnalysis": {
    "label": "neutral",
    "score": 0.5
  }
}
```

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
