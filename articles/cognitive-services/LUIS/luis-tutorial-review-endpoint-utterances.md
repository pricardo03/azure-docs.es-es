---
title: Revisión de expresiones de punto de conexión
titleSuffix: Azure Cognitive Services
description: Mejore las predicciones de aplicaciones mediante la comprobación o corrección de las expresiones recibidas mediante el punto de conexión HTTP de LUIS de las que LUIS no está seguro. Algunas expresiones puede que se comprueben para la intención y otras puede que necesiten comprobarse para la entidad.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 02/19/2019
ms.author: diberry
ms.openlocfilehash: 25841971a2e7921c89c63032e8fd48bc528263aa
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56878187"
---
# <a name="tutorial-fix-unsure-predictions-by-reviewing-endpoint-utterances"></a>Tutorial: Corrección de predicciones poco seguras mediante la revisión de las expresiones del punto de conexión
En este tutorial, va a mejorar las predicciones de aplicaciones mediante la comprobación o corrección de las expresiones recibidas mediante el punto de conexión HTTPS de LUIS de las que LUIS no está seguro. Algunas expresiones puede que tengan que comprobarse para la intención y otras puede que necesiten comprobarse para la entidad. Debe revisar las expresiones de punto de conexión como una parte convencional del mantenimiento programado de LUIS. 

Este proceso de revisión es otra forma de que LUIS aprenda el dominio de aplicación. LUIS seleccionó las expresiones que aparecen en la lista de revisión. Esta lista:

* Es específica de la aplicación.
* Está pensada para mejorar la precisión de la predicción de la aplicación. 
* Debe revisarse periódicamente. 

Al revisar las expresiones de punto de conexión, debe comprobar o corregir la intención pronosticada de la expresión. También se etiquetan entidades personalizadas que no se predijeron o se predijeron incorrectamente. 

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Importar la aplicación de ejemplo
> * Revisar las expresiones de punto de conexión
> * Actualización de la lista de frases
> * Entrenamiento de la aplicación
> * Publicación de aplicación
> * Consulta del punto de conexión de la aplicación para ver la respuesta JSON de LUIS

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Importar la aplicación de ejemplo

Continúe con la aplicación creada en el último tutorial, denominada **HumanResources**. 

Para ello, siga los pasos que se describen a continuación:

1.  Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-sentiment-HumanResources.json).

1. Importe el archivo JSON en una aplicación nueva.

1. Desde la sección **Manage** (Administrar), en la pestaña **Versions** (Versiones), clone la versión y asígnele el nombre `review`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL.

1. Entrene y publique la nueva aplicación.

1. Use el punto de conexión para agregar las expresiones siguientes. Puede hacer esto con un [script](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/demo-upload-endpoint-utterances/endpoint.js) o desde el punto de conexión en un explorador. Las expresiones que se van a agregar son las siguientes:

   [!code-nodejs[Node.js code showing endpoint utterances to add](~/samples-luis/examples/demo-upload-endpoint-utterances/endpoint.js?range=15-26)]

    Si tiene todas las versiones de la aplicación, mediante la serie de tutoriales, se sorprenderá al ver que la lista **Review endpoint utterances** (Revisar las expresiones de punto de conexión) no cambia, en función de la versión. Existe un único grupo de expresiones para revisar, independientemente de la versión que esté editando activamente o de la versión de la aplicación que se haya publicado en el punto de conexión. 

## <a name="review-endpoint-utterances"></a>Revisar las expresiones de punto de conexión

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. Seleccione **Review endpoint utterances** (Revisar las expresiones de punto de conexión) en el panel de navegación izquierdo. La lista se filtra para la intención **ApplyForJob**. 

    [![Captura de pantalla del botón Review endpoint utterances (Revisar las expresiones de punto de conexión) en el panel de navegación izquierdo](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-entity-view.png)](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-entity-view.png#lightbox)

1. Alterne la **vista de entidades** para ver las entidades etiquetadas. 
    
    [![Captura de pantalla de Review endpoint utterances (Revisar las expresiones de punto de conexión) con la alternancia de la vista de entidades resaltada](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-token-view.png)](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-token-view.png#lightbox)

    |Expresión|Intención correcta|Entidades que faltan|
    |:--|:--|:--|
    |I'm looking for a job with Natural Language Processing (Estoy buscando un trabajo con procesamiento de lenguaje natural)|GetJobInfo|Trabajo: "Proceso de lenguaje natural"|

    Esta expresión no se encuentra en la intención correcta y tiene una puntuación menor del 50 %. La intención **ApplyForJob** tiene 21 expresiones en comparación con las siete expresiones de **GetJobInformation**. Junto con alinear correctamente la expresión de punto de conexión, se deben agregar más expresiones a la intención **GetJobInformation**. Esto queda como ejercicio para que pueda realizar por su cuenta. Cada intención, excepto para la intención **None** (Ninguno), debe tener aproximadamente el mismo número de expresiones de ejemplo. La intención **None** (Ninguno) debe contener un 10 % de las expresiones totales de la aplicación. 

1. Para la intención `I'm looking for a job with Natual Language Processing`, seleccione la intención correcta, **GetJobInformation** en la columna **Aligned intent** (Intención alineada). 

    [![Captura de pantalla de la alineación de la expresión con la intención en Review endpoint utterances (Revisar las expresiones de punto de conexión)](./media/luis-tutorial-review-endpoint-utterances/align-intent-1.png)](./media/luis-tutorial-review-endpoint-utterances/align-intent-1.png#lightbox)

1. En la misma expresión, la entidad para `Natural Language Processing` es keyPhrase. Debe ser una entidad **Job** en su lugar. Seleccione `Natural Language Processing` y, después, seleccione la entidad **Job** en la lista.

    [![Captura de pantalla del etiquetado de entidad en la expresión en Review endpoint utterances (Revisar las expresiones de punto de conexión)](./media/luis-tutorial-review-endpoint-utterances/label-entity.png)](./media/luis-tutorial-review-endpoint-utterances/label-entity.png#lightbox)

1. En la misma línea, seleccione la marca de verificación en un círculo en la columna **Add to aligned intent** (Agregar a la intención alineada). 

    [![Captura de pantalla de la finalización de la alineación de la expresión con la intención](./media/luis-tutorial-review-endpoint-utterances/align-utterance.png)](./media/luis-tutorial-review-endpoint-utterances/align-utterance.png#lightbox)

    Esta acción mueve la expresión de **Review endpoint utterances** (Revisar las expresiones de punto de conexión) a la intención **GetJobInformation**. La expresión de punto de conexión ahora es una expresión de ejemplo para esa intención. 

1. Revise las expresiones restantes de esta intención, mediante el etiquetado de expresiones y la corrección de la **intención alineada**, si son incorrectas.

1. Cuando todas las expresiones son correctas, active la casilla en cada fila y luego seleccione **Add selected** (Agregar selección) para alinear las expresiones correctamente. 

    [![Captura de pantalla de la finalización de las expresiones restantes en la intención alineada](./media/luis-tutorial-review-endpoint-utterances/finalize-utterance-alignment.png)](./media/luis-tutorial-review-endpoint-utterances/finalize-utterance-alignment.png#lightbox)

1. La lista ya no debe tener esas expresiones. Si aparecen más expresiones, continúe trabajando en la lista, corrija las intenciones y etiquete cualquier entidad que falte, hasta que la lista esté vacía. 

1. Seleccione la siguiente intención en la lista Filter (Filtro) y continúe el proceso de corregir las expresiones y etiquetar entidades. Recuerde que en el último paso de cada intención debe seleccionar la opción **Add to aligned intent** (Agregar a la intención alineada) en la fila de expresiones o activar la casilla por cada intención y seleccionar **Add selected** (Agregar selección) sobre la tabla.

    Continúe hasta que todas las intenciones y entidades de la lista de filtros tengan una lista vacía. Se trata de una aplicación muy pequeña. El proceso de revisión tarda solo unos minutos. 

## <a name="update-phrase-list"></a>Actualización de la lista de frases
Mantenga actualizada la lista de frases con los nombres de trabajo que se acaban de detectar. 

1. Seleccione **Phrase Lists** (Listas de frases) en el panel de navegación izquierdo.

2. Seleccione la lista de frases **Jobs** (Trabajos).

3. Agregue `Natural Language Processing` como un valor y, después, seleccione **Save** (Guardar). 

## <a name="train"></a>Train

LUIS no sabe nada acerca de los cambios hasta que se entrena. 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

Si ha importado a esta aplicación, deberá seleccionar **Sentiment analysis** (Análisis de sentimiento).

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Obtención de intenciones y entidades del punto de conexión

Pruebe con una expresión próxima a la expresión corregida. 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vaya al final de la dirección URL en la dirección y escriba `Are there any natural language processing jobs in my department right now?`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. 

  ```json
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
Algunas de las expresiones de punto de conexión tendrán una puntuación de predicción alta en la lista de revisión. Aun así deberá revisar y comprobar dichas expresiones. Están en la lista porque la siguiente intención más elevada tenía una puntuación demasiado cercana a la puntuación máxima de la intención. Desea una diferencia de aproximadamente el 15 % entre dos de las intenciones con más puntuación.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes
En este tutorial revisó las expresiones enviadas en el punto de conexión, de las que LUIS no estaba seguro. Una vez que comprobadas estas expresiones y trasladadas a las intenciones correctas como expresiones de ejemplo, LUIS mejorará la precisión de la predicción.

> [!div class="nextstepaction"]
> [Más información sobre cómo usar los patrones](luis-tutorial-pattern.md)
