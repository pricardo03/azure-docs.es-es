---
title: 'Tutorial para revisar las expresiones de punto de conexión en Language Understanding (LUIS): Azure | Microsoft Docs'
description: En este tutorial, aprenderá a revisar las expresiones de puntos de conexión en el dominio de Recursos humanos (RR. HH.) en LUIS.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 07/03/2018
ms.author: diberry
ms.openlocfilehash: 1f1e3310e0d02983aaecc3f87ba9c116d65b751b
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39237220"
---
# <a name="tutorial-review-endpoint-utterances"></a>Tutorial: Revisión de las expresiones de punto de conexión
En este tutorial, va a mejorar las predicciones de aplicaciones mediante la comprobación o corrección de las expresiones recibidas mediante el punto de conexión HTTP de LUIS. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Descripción de la revisión de las expresiones de punto de conexión 
> * Uso de la aplicación de LUIS para el dominio de Recursos humanos (RR. HH.) 
> * Revisar las expresiones de punto de conexión
> * Entrenamiento y publicación de la aplicación
> * Consulta del punto de conexión de la aplicación para ver la respuesta JSON de LUIS

Para este artículo, necesita una cuenta gratuita de [LUIS](luis-reference-regions.md#luis-website) para crear la aplicación.

## <a name="before-you-begin"></a>Antes de empezar
Si no tiene la aplicación de Recursos Humanos del tutorial de [opinión](luis-quickstart-intent-and-sentiment-analysis.md), importe la aplicación desde el repositorio Github [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-sentiment-HumanResources.json). Si utiliza este tutorial como una nueva aplicación importada, también tendrá que entrenar, publicar y, después, agregar las expresiones al punto de conexión con un [script](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/demo-upload-endpoint-utterances/endpoint.js) o desde el punto de conexión en un explorador. Las expresiones que se van a agregar son las siguientes:

   [!code-nodejs[Node.js code showing endpoint utterances to add](~/samples-luis/examples/demo-upload-endpoint-utterances/endpoint.js?range=15-26)]

Si quiere conservar la aplicación original de Recursos humanos, clone la versión en la página [Configuración](luis-how-to-manage-versions.md#clone-a-version) y llámela `review`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. 

Si tiene todas las versiones de la aplicación, mediante la serie de tutoriales, se sorprenderá al ver que la lista **Review endpoint utterances** (Revisar las expresiones de punto de conexión) no cambia, en función de la versión. Existe un único grupo de expresiones para revisar, independientemente de la versión que esté editando activamente o de la versión de la aplicación que se haya publicado en el punto de conexión. 

## <a name="purpose-of-reviewing-endpoint-utterances"></a>Propósito de la revisión de expresiones de punto de conexión
Este proceso de revisión es otra forma de que LUIS aprenda el dominio de aplicación. LUIS seleccionó los enunciados de la lista de revisión. Esta lista:

* Es específica de la aplicación.
* Está pensada para mejorar la precisión de la predicción de la aplicación. 
* Debe revisarse periódicamente. 

Al revisar las expresiones de punto de conexión, debe comprobar o corregir la intención pronosticada de la expresión. También se etiquetan entidades personalizadas que no se predijeron. 

## <a name="review-endpoint-utterances"></a>Revisar las expresiones de punto de conexión

1. Asegúrese de que la aplicación de recursos humanos se encuentra en la sección **Build** (Crear) de LUIS. Para cambiar a esta sección, seleccione **Build** (Crear) en la barra de menús superior derecha. 

    [ ![Captura de pantalla de una aplicación de LUIS con Build (Crear) resaltado en la barra de navegación superior derecha](./media/luis-tutorial-review-endpoint-utterances/first-image.png)](./media/luis-tutorial-review-endpoint-utterances/first-image.png#lightbox)

1. Seleccione **Review endpoint utterances** (Revisar las expresiones de punto de conexión) en el panel de navegación izquierdo. La lista se filtra para la intención **ApplyForJob**. 

    [ ![Captura de pantalla del botón Review endpoint utterances (Revisar las expresiones de punto de conexión) en el panel de navegación izquierdo](./media/luis-tutorial-review-endpoint-utterances/entities-view-endpoint-utterances.png)](./media/luis-tutorial-review-endpoint-utterances/entities-view-endpoint-utterances.png#lightbox)

2. Alterne la **vista de entidades** para ver las entidades etiquetadas. 
    
    [ ![Captura de pantalla de Review endpoint utterances (Revisar las expresiones de punto de conexión) con la alternancia de la vista de entidades resaltada](./media/luis-tutorial-review-endpoint-utterances/select-entities-view.png)](./media/luis-tutorial-review-endpoint-utterances/select-entities-view.png#lightbox)

    |Expresión|Intención correcta|Entidades que faltan|
    |:--|:--|:--|
    |I'm looking for a job with Natural Language Processing (Estoy buscando un trabajo con procesamiento de lenguaje natural)|GetJobInfo|Trabajo: "Proceso de lenguaje natural"|

    Esta expresión no se encuentra en la intención correcta y tiene una puntuación menor del 50 %. La intención **ApplyForJob** tiene 21 expresiones en comparación con las siete expresiones de **GetJobInformation**. Junto con alinear correctamente la expresión de punto de conexión, se deben agregar más expresiones a la intención **GetJobInformation**. Esto queda como ejercicio para que pueda realizar por su cuenta. Cada intención, excepto para la intención **None** (Ninguno), debe tener aproximadamente el mismo número de expresiones de ejemplo. La intención **None** (Ninguno) debe contener un 10 % de las expresiones totales de la aplicación. 

    Cuando se encuentre en la **vista de tokens**, puede mantener el cursor sobre cualquier texto azul de la expresión para ver el nombre de entidad pronosticada. 

3. Para la intención `I'm looking for a job with Natual Language Processing`, seleccione la intención correcta, **GetJobInformation** en la columna **Aligned intent** (Intención alineada). 

    [ ![Captura de pantalla de la alineación de la expresión con la intención en Review endpoint utterances (Revisar las expresiones de punto de conexión)](./media/luis-tutorial-review-endpoint-utterances/align-intent-1.png)](./media/luis-tutorial-review-endpoint-utterances/align-intent-1.png#lightbox)

4. En la misma expresión, la entidad para `Natural Language Processing` es keyPhrase. Debe ser una entidad **Job** en su lugar. Seleccione `Natural Language Processing` y, después, seleccione la entidad **Job** en la lista.

    [ ![Captura de pantalla del etiquetado de entidad en la expresión en Review endpoint utterances (Revisar las expresiones de punto de conexión)](./media/luis-tutorial-review-endpoint-utterances/label-entity.png)](./media/luis-tutorial-review-endpoint-utterances/label-entity.png#lightbox)

5. En la misma línea, seleccione la marca de verificación en un círculo en la columna **Add to aligned intent** (Agregar a la intención alineada). 

    [ ![Captura de pantalla de la finalización de la alineación de la expresión con la intención](./media/luis-tutorial-review-endpoint-utterances/align-utterance.png)](./media/luis-tutorial-review-endpoint-utterances/align-utterance.png#lightbox)

    Esta acción mueve la expresión de **Review endpoint utterances** (Revisar las expresiones de punto de conexión) a la intención **GetJobInformation**. La expresión de punto de conexión ahora es una expresión de ejemplo para esa intención. 

6. Revise las expresiones restantes de esta intención, mediante el etiquetado de expresiones y la corrección de la **intención alineada**, si son incorrectas.

7. Cuando todas las expresiones son correctas, active la casilla en cada fila y luego seleccione **Add selected** (Agregar selección) para alinear las expresiones correctamente. 

    [ ![Captura de pantalla de la finalización de las expresiones restantes en la intención alineada](./media/luis-tutorial-review-endpoint-utterances/finalize-utterance-alignment.png)](./media/luis-tutorial-review-endpoint-utterances/finalize-utterance-alignment.png#lightbox)

8. La lista ya no debe tener esas expresiones. Si aparecen más expresiones, continúe trabajando en la lista, corrija las intenciones y etiquete cualquier entidad que falte, hasta que esté vacía. Seleccione la siguiente intención en la lista Filter (Filtro) y continúe el proceso de corregir las expresiones y etiquetar entidades. Recuerde que en el último paso de cada intención debe seleccionar la opción **Add to aligned intent** (Agregar a la intención alineada) en la fila de expresiones o activar la casilla por cada intención y seleccionar **Add selected** (Agregar selección) sobre la tabla. 

    Se trata de una aplicación muy pequeña. El proceso de revisión tarda solo unos minutos.

## <a name="add-new-job-name-to-phrase-list"></a>Adición de un nuevo nombre de trabajo a la lista de frases
Mantenga actualizada la lista de frases con los nombres de trabajo que se acaban de detectar. 

1. Seleccione **Phrase Lists** (Listas de frases) en el panel de navegación izquierdo.

2. Seleccione la lista de frases **Jobs** (Trabajos).

3. Agregue `Natural Language Processing` como un valor y, después, seleccione **Save** (Guardar). 

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS
LUIS no sabe nada acerca de los cambios hasta que se entrena. 

1. En la parte superior derecha del sitio web de LUIS, haga clic en el botón **Entrenar**.

2. El entrenamiento se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicación de la aplicación para obtener la dirección URL del punto de conexión
Para obtener el modelo actualizado de la aplicación LUIS en un bot de chat u otra aplicación, tiene que publicar la aplicación. 

1. En la parte superior derecha del sitio web de LUIS, haga clic en el botón **Publish** (Publicar). 

2. Si ha importado a esta aplicación, deberá seleccionar **Sentiment analysis** (Análisis de sentimiento). 

3. Seleccione el espacio de producción y haga clic en el botón **Publicar**.

4. La publicación se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

## <a name="query-the-endpoint-with-an-utterance"></a>Consulta del punto de conexión con una expresión
Pruebe con una expresión próxima a la expresión corregida. 

1. En la página **Publish** (Publicar), seleccione el vínculo **endpoint** (Punto de conexión) en la parte inferior de la página. Esta acción abre otra ventana del explorador con la dirección URL del punto de conexión en la barra de direcciones. 

2. Vaya al final de la dirección URL en la dirección y escriba `Are there any natural language processing jobs in my department right now?`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. 

```JSON
{
  "query": "are there any natural language processing jobs in my department right now?",
  "topScoringIntent": {
    "intent": "GetJobInformation",
    "score": 0.9247605
  },
  "intents": [
    {
      "intent": "GetJobInformation",
      "score": 0.9247605
    },
    {
      "intent": "ApplyForJob",
      "score": 0.129989788
    },
    {
      "intent": "FindForm",
      "score": 0.006438211
    },
    {
      "intent": "EmployeeFeedback",
      "score": 0.00408575451
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00194211153
    },
    {
      "intent": "None",
      "score": 0.00166400627
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00118593348
    },
    {
      "intent": "MoveEmployee",
      "score": 0.0007885918
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.0006373631
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.0005980781
    },
    {
      "intent": "Utilities.Confirm",
      "score": 3.719905E-05
    }
  ],
  "entities": [
    {
      "entity": "right now",
      "type": "builtin.datetimeV2.datetime",
      "startIndex": 64,
      "endIndex": 72,
      "resolution": {
        "values": [
          {
            "timex": "PRESENT_REF",
            "type": "datetime",
            "value": "2018-07-05 15:23:18"
          }
        ]
      }
    },
    {
      "entity": "natural language processing",
      "type": "Job",
      "startIndex": 14,
      "endIndex": 40,
      "score": 0.9869922
    },
    {
      "entity": "natural language processing jobs",
      "type": "builtin.keyPhrase",
      "startIndex": 14,
      "endIndex": 45
    },
    {
      "entity": "department",
      "type": "builtin.keyPhrase",
      "startIndex": 53,
      "endIndex": 62
    }
  ],
  "sentimentAnalysis": {
    "label": "positive",
    "score": 0.8251864
  }
}
}
```

Se puede predecir la intención correcta con una puntuación más alta y la entidad **Job** se detecta como `natural language processing`. 

## <a name="can-reviewing-be-replaced-by-adding-more-utterances"></a>¿Se puede reemplazar la revisión al agregar más expresiones? 
Tal vez se pregunte por qué no agregar más expresiones de ejemplo. ¿Cuál es el propósito de la revisión de las expresiones de punto de conexión? En una aplicación LUIS del mundo real, las expresiones de punto de conexión proceden de usuarios con una elección y disposición de palabras que todavía no han utilizado. Si hubiera usado la misma elección y disposición de palabras, la predicción original tendría un porcentaje mayor. 

## <a name="why-is-the-top-intent-on-the-utterance-list"></a>¿Por qué está la intención principal en la lista de expresiones? 
Algunas de las expresiones de punto de conexión tendrán un porcentaje alto de la lista de revisión. Aun así deberá revisar y comprobar dichas expresiones. Están en la lista porque la siguiente intención más elevada tenía una puntuación demasiado cercana a la puntuación máxima de la intención. 

## <a name="what-has-this-tutorial-accomplished"></a>¿Qué ha logrado este tutorial?
Esta precisión de predicción de la aplicación ha aumentado al revisar las expresiones desde el punto de conexión. 

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no sea necesaria, elimine la aplicación de LUIS. Seleccione **My apps** (Mis aplicaciones) en el menú superior izquierdo. Seleccione los puntos suspensivos **...** a la derecha del nombre de la aplicación en la lista de aplicaciones y, después, seleccione **Delete** (Eliminar). En el cuadro de diálogo emergente **Delete app?** (¿Eliminar aplicación?), haga clic en **Ok** (Aceptar).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre cómo usar los patrones](luis-tutorial-pattern.md)
