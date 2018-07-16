---
title: 'Tutorial de uso de una lista de frases para mejorar las predicciones de LUIS: Azure | Microsoft Docs'
description: En este tutorial, agregará una lista de frases a una aplicación de LUIS y verá la mejora de la puntuación.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2017
ms.author: v-geberr
ms.openlocfilehash: feb8acb674fd2dc62b62c26da6a6b42515f30242
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265978"
---
# <a name="tutorial-add-phrase-list-to-improve-predictions"></a>Tutorial: Agregar la lista de frases para mejorar las predicciones
En este tutorial, mejorará la precisión de las puntuaciones de las intenciones e identificará entidades para palabras que tienen el mismo significado (sinónimos) al agregar una [característica de lista de frases](./luis-concept-feature.md) intercambiables.

> [!div class="checklist"]
* Importar una aplicación nueva  
* Consultar un punto de conexión con una expresión conocida 
* Consultar un punto de conexión con una expresión _desconocida_
* Agregar la lista de frases para mejorar la puntuación de la expresión desconocida
* Comprobar que se encuentra la entidad cuando se usa la lista de frases

Para este artículo, necesita una cuenta de [LUIS][LUIS] gratuita para crear la aplicación de LUIS.

## <a name="import-a-new-app"></a>Importar una aplicación nueva
1. Descargue el [ejemplo de la aplicación de LUIS][LuisSampleApp] que está diseñado para este tutorial. Lo usará en el paso siguiente. 

2. Como se ha descrito en [Crear una aplicación](Create-new-app.md#import-new-app), importe el archivo que ha descargado en el sitio web de [LUIS][LUIS] como una aplicación nueva. El nombre de la aplicación es "My Phrase List tutorial". Tiene intenciones, entidades y expresiones. 

3. [Entrene](luis-how-to-train.md) la aplicación. Hasta que esté entrenada, no puede [probarla interactivamente](interactive-test.md#interactive-testing) en el sitio web de [LUIS][LUIS]. 

4. En la página [Publicar](PublishApp.md), active la casilla **Include all predicted intent scores** (Incluir todas las puntuaciones de intención pronosticada). Cuando se activa la casilla, se devuelven todas las intenciones. Cuando se desactiva la casilla, se devuelve solo la intención superior. 

5. [Publique](PublishApp.md) la aplicación. Publicar la aplicación permite probarla mediante el punto de conexión HTTPS. 

## <a name="test-a-trained-utterance"></a>Probar una expresión entrenada
Use el punto de conexión publicado para consultar una expresión que la aplicación ya conozca. Dado que LUIS ya conoce la expresión, la puntuación es alta y se detecta la entidad.

1. En el sitio web de [Language Understanding (LUIS)][LUIS], en la página **Publicar** de la nueva aplicación, seleccione la dirección URL del punto de conexión en la sección **Resources and Keys** (Recursos y claves). 

    ![Publicar la dirección URL del punto de conexión](./media/luis-tutorial-interchangeable-phrase-list/luis-publish-url.png)

2. En el explorador, al final de la dirección URL, agregue la siguiente consulta después de `q=`.

    `I want a computer replacement`

    El punto de conexión responde con el siguiente JSON:
    
    ```JSON
    {
      "query": "I want a computer replacement",
      "topScoringIntent": {
        "intent": "GetHardware",
        "score": 0.9735458
      },
      "intents": [
        {
          "intent": "GetHardware",
          "score": 0.9735458
        },
        {
          "intent": "None",
          "score": 0.07053033
        },
        {
          "intent": "Whois",
          "score": 0.03760778
        },
        {
          "intent": "CheckCalendar",
          "score": 0.02285902
        },
        {
          "intent": "CheckInventory",
          "score": 0.0110072717
        }
      ],
      "entities": [
        {
          "entity": "computer",
          "type": "Hardware",
          "startIndex": 9,
          "endIndex": 16,
          "score": 0.8465915
        }
      ]
    }
    ```

    La puntuación de la intención de 0,973 y la puntuación de detección de la entidad de 0,846 son altas porque la aplicación se ha entrenado con esta expresión. La expresión está en la aplicación de LUIS en la página de intención de **GetHardware**. El texto de la expresión, `computer`, se etiqueta como la entidad **Hardware**. 
    
    |Status|Word| Puntuación de intención | Puntuación de entidad |
    |--|--|--|--|
    |Entrenada| want | 0,973 | 0,846 |
    
    
## <a name="test-an-untrained-utterance"></a>Probar una expresión no entrenada
En el explorador, use el mismo punto de conexión publicado para consultar con una expresión que la aplicación aún no conoce:

`I require a computer replacement`

Esta expresión usa un sinónimo de la expresión anterior:

| Palabra entrenada | Sinónimo no entrenado |
|--|--|
| want | require |

La respuesta del punto de conexión es:

```JSON
{
  "query": "I require a computer replacement",
  "topScoringIntent": {
    "intent": "GetHardware",
    "score": 0.840912163
  },
  "intents": [
    {
      "intent": "GetHardware",
      "score": 0.840912163
    },
    {
      "intent": "None",
      "score": 0.0972757638
    },
    {
      "intent": "Whois",
      "score": 0.0448251367
    },
    {
      "intent": "CheckCalendar",
      "score": 0.0291390456
    },
    {
      "intent": "CheckInventory",
      "score": 0.0137837809
    }
  ],
  "entities": []
}
```

| Status | Word | Puntuación de intención | Puntuación de entidad |
|--|--|--|--|
| Entrenada| want | 0,973 | 0,846 |
| No entrenada| require | 0,840 | - |

La puntuación de la intención de la expresión no entrenada es menor que la de la expresión etiquetada porque LUIS sabe que la frase es gramaticalmente la misma. Pero LUIS no sabe que las expresiones tienen el mismo significado. Además, sin la lista de frases, la entidad **Hardware** no se encuentra.

Debe enseñar a LUIS que *want* y *require* significan lo mismo en este dominio de aplicación ya que una palabra puede tener más de un significado. 

## <a name="improve-the-score-of-untrained-utterance-with-phrase-list"></a>Mejorar la puntuación de la expresión no entrenada con la lista de frases 
1. Agregue una característica de [lista de frases](luis-how-to-add-features.md) denominada **want** con el valor de `want` y después presione **Entrar**.

    > [!TIP]
    > Después de cada palabra o frase, presione la tecla **Entrar**. La palabra o frase se agrega al cuadro **Phrase list values** (Valores de lista de frases) mientras el cursor permanece en el cuadro **Valor**. Puede especificar varios valores rápidamente con esta característica.

2. Para ver las palabras que recomienda LUIS, seleccione **Recommend** (Recomendar). 

    ![Recomendar valores](./media/luis-tutorial-interchangeable-phrase-list/recommend.png)

3. Agregue todas las palabras. Si `require` no está en la lista recomendada, debe agregarla como un valor requerido. 

4. Puesto que estas palabras son sinónimos, mantenga la configuración *intercambiable* y después haga clic en **Guardar**.

    ![Valores de lista de frases](./media/luis-tutorial-interchangeable-phrase-list/phrase-list-values.png)

5. En la barra de navegación superior, haga clic en **Entrenar** para entrenar la aplicación, pero no publicarla. Ahora tiene dos modelos. Puede comparar los valores de los dos modelos.

## <a name="compare-the-phrase-list-model-to-the-published-model"></a>Comparar el modelo de lista de frases con el modelo publicado
En esta aplicación, el modelo publicado no se entrena con los sinónimos. Solo el modelo editado actualmente incluye la lista de frases de sinónimos. Para comparar los modelos, use [pruebas interactivas](interactive-test.md#interactive-testing). 

1. Abra el panel **Probar** y escriba la expresión siguiente:

    `I require a computer replacement`

2. Para abrir el panel de inspección, seleccione **Inspeccionar**. 

    ![Seleccionar Inspeccionar](./media/luis-tutorial-interchangeable-phrase-list/inspect-button.png)

3. Para comparar el modelo publicado con el nuevo modelo de lista de frases, haga clic en **Compare with published** (Comparar con el publicado).

    ![Inspeccionar el publicado frente al actual](./media/luis-tutorial-interchangeable-phrase-list/inspect.png)

Después de agregar la lista de frases, se encuentra la precisión aumentada de la expresión y la entidad **Hardware**. 

|Status | Lista de frases| Puntuación de intención | Puntuación de entidad |
|--|--|--|--|
| Publicado | - | 0,84 | - |
| En edición |✔| 0,92 | Entidad Hardware identificada |

> [!TIP]
> * Al usar [pruebas interactivas](interactive-test.md#interactive-testing), puede comparar el modelo publicado con todos los cambios entrenados que se realicen después de la publicación. 
> * Al usar [pruebas de punto de conexión](PublishApp.md#test-your-published-endpoint-in-a-browser), puede ver la respuesta JSON exacta de LUIS. 

## <a name="get-the-entity-score-with-the-endpoint-test"></a>Obtener la puntuación de la entidad con la prueba de punto de conexión
Para ver la puntuación de la entidad, [publique el modelo](PublishApp.md) y consulte el punto de conexión. 

`I require a computer replacement`

```JSON
{
  "query": "I require a computer replacement",
  "topScoringIntent": {
    "intent": "GetHardware",
    "score": 0.916503668
  },
  "intents": [
    {
      "intent": "GetHardware",
      "score": 0.916503668
    },
    {
      "intent": "None",
      "score": 0.136505231
    },
    {
      "intent": "Whois",
      "score": 0.02778677
    },
    {
      "intent": "CheckInventory",
      "score": 0.0144592477
    },
    {
      "intent": "CheckCalendar",
      "score": 0.01401332
    }
  ],
  "entities": [
    {
      "entity": "computer",
      "type": "Hardware",
      "startIndex": 12,
      "endIndex": 19,
      "score": 0.5959917
    }
  ]
}
```

La entidad **Hardware** muestra una puntuación de 0,595 con la lista de frases. Antes de que existiera la lista de frases, no se detectaba la entidad. 

|Status | Lista de frases| Puntuación de intención | Puntuación de entidad |
|--|--|--|--|
| Publicado | - | 0,84 | - |
| En edición |✔| 0,92 | 0,595 |


## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no sea necesaria, elimine la aplicación de LUIS. Para ello, seleccione el menú de tres puntos (...) situado a la derecha del nombre de la aplicación en la lista de aplicaciones y haga clic en **Eliminar**. En el cuadro de diálogo emergente **Delete app?** (¿Eliminar aplicación?), haga clic en **Ok** (Aceptar).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Obtener la predicción de expresiones con consulta de punto de conexión](luis-get-started-cs-get-intent.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
[LuisFeatures]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[LuisSampleApp]: https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/phrase_list/interchangeable/luis-app-before-phrase-list.json
