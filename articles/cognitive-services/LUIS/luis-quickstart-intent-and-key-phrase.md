---
title: 'Tutorial: 8: Extracción de frases clave en LUIS'
titleSuffix: Azure Cognitive Services
description: Use la entidad keyPhrase pregenerada para extraer los temas principales clave de las expresiones. No necesita etiquetar ninguna expresión con entidades pregeneradas. La entidad se detecta automáticamente.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: d6feb24b1e59aee70204d8438f1a4c51f71d1835
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51281819"
---
# <a name="tutorial-8-extract-key-phrases-of-utterance"></a>Tutorial: 8: Extracción de frases clave de expresiones
En este tutorial, va a usar una entidad keyPhrase pregenerada que muestra cómo extraer los temas principales clave de las expresiones. No necesita etiquetar ninguna expresión con entidades pregeneradas. La entidad se detecta automáticamente.

Las expresiones siguientes muestran ejemplos de frases clave:

|Expresión|Valores de la entidad KeyPhrase|
|--|--|
|¿Se ofrecerá el próximo año un nuevo plan médico con una franquicia menor?|"franquicia menor"<br>"nuevo plan médico"<br>"año"|
|¿El plan médico con franquicia alta incluye tratamiento de la visión?|"plan médico con franquicia alta"<br>"tratamiento de la visión"|

La aplicación cliente puede utilizar estos valores, junto con otras entidades extraídas, para decidir el siguiente paso de la conversación.

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Usar la aplicación del tutorial existente
> * Adición de la entidad KeyPhrase 
> * Train
> * Publicar
> * Obtener intenciones y entidades del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Usar una aplicación existente

Continúe con la aplicación creada en el último tutorial, denominada **HumanResources**. 

Si no tiene la aplicación HumanResources del tutorial anterior, siga estos pasos:

1.  Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-simple-HumanResources.json).

2. Importe el archivo JSON en una aplicación nueva.

3. Desde la sección **Manage** (Administrar), en la pestaña **Versions** (Versiones), clone la versión y asígnele el nombre `keyphrase`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL.

## <a name="add-keyphrase-entity"></a>Adición de la entidad KeyPhrase 
Agregue la entidad predefinida keyPhrase para extraer los temas principales de las expresiones.

1. [!INCLUDE[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Seleccione **Entities** (Entidades) en el menú izquierdo.

3. Seleccione **Add prebuilt entity** (Agregar entidad creada previamente).

4. En el cuadro de diálogo emergente, seleccione **keyphrase** y, a continuación, seleccione **Done** (Listo). 

    [ ![Captura de pantalla del cuadro de diálogo emergente con la lista de entidades](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png#lightbox)

5. Seleccione **Intents** (Intenciones) en el menú izquierdo y, después, seleccione la intención **Utilities.Confirm**. La entidad keyPhrase está etiquetada en varias expresiones. 

    [ ![Captura de pantalla de la intención Utilities.Confirm intención con varias entidades keyPhrase etiquetadas en expresiones](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png)](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png#lightbox)

## <a name="train"></a>Train

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Obtención de intenciones y entidades del punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vaya al final de la dirección URL en la dirección y escriba `does form hrf-123456 cover the new dental benefits and medical plan`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. 
    
    ```JSON
    {
      "query": "does form hrf-123456 cover the new dental benefits and medical plan",
      "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.9300641
      },
      "intents": [
        {
          "intent": "FindForm",
          "score": 0.9300641
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0359598845
        },
        {
          "intent": "GetJobInformation",
          "score": 0.0141798034
        },
        {
          "intent": "MoveEmployee",
          "score": 0.0112197418
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.00507669244
        },
        {
          "intent": "None",
          "score": 0.00238501839
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00202810857
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00102957746
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.0008688423
        },
        {
          "intent": "Utilities.Confirm",
          "score": 3.557994E-05
        }
      ],
      "entities": [
        {
          "entity": "hrf-123456",
          "type": "HRF-number",git 
          "startIndex": 10,
          "endIndex": 19
        },
        {
          "entity": "new dental benefits",
          "type": "builtin.keyPhrase",
          "startIndex": 31,
          "endIndex": 49
        },
        {
          "entity": "medical plan",
          "type": "builtin.keyPhrase",
          "startIndex": 55,
          "endIndex": 66
        },
        {
          "entity": "hrf",
          "type": "builtin.keyPhrase",
          "startIndex": 10,
          "endIndex": 12
        },
        {
          "entity": "-123456",
          "type": "builtin.number",
          "startIndex": 13,
          "endIndex": 19,
          "resolution": {
            "value": "-123456"
          }
        }
      ]
    }
    ```

    Al buscar un formulario, el usuario ha proporcionado más información de la necesaria. La información adicional se devuelve como **builtin.keyPhrase**. La aplicación cliente puede utilizar esta información adicional para una pregunta de seguimiento, como "¿Desea para hablar con un representante de recursos humanos acerca de los nuevos beneficios en salud dental" o proporcionar un menú con más opciones, entre las que se incluya, "Más información acerca de los nuevos beneficios en salud dental o del seguro médico".

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se agregó la entidad keyPhrase pregenerada, que proporcionó rápidamente frases clave en las expresiones sin tener que etiquetarlas. 

> [!div class="nextstepaction"]
> [Adición de análisis de opiniones](luis-quickstart-intent-and-sentiment-analysis.md)