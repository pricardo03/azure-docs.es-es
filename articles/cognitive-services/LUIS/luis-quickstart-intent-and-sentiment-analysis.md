---
title: análisis de opiniones
titleSuffix: Azure Cognitive Services
description: En este tutorial va a crear una aplicación que muestra cómo extraer opiniones positivas, negativas y neutrales de expresiones. Las opiniones se determinan a partir de la expresión completa.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: d93c7619bb670a81372ab83359836a78b8956b09
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53098948"
---
# <a name="tutorial-9--extract-sentiment-of-overall-utterance"></a>Tutorial 9:  Extracción de opiniones de expresión global
En este tutorial va a crear una aplicación que muestra cómo extraer opiniones positivas, negativas y neutrales de expresiones. Las opiniones se determinan a partir de la expresión completa.

El análisis de opiniones es la capacidad para determinar si la expresión de un usuario es positiva, negativa o neutral. 

Las expresiones siguientes muestran ejemplos de opiniones:

|Opinión|Score|Expresión|
|:--|:--|:--|
|positiva|0,91 |John W. Smith hizo un gran trabajo en la presentación en París.|
|positiva|0.84 |jill-jones@mycompany.com realizó un trabajo fabuloso en el discurso de ventas de Parker.|

El análisis de opiniones es una configuración de publicación que se aplica a cada expresión. No es necesario encontrar las palabras que indican opinión en la expresión y etiquetarlas porque el análisis de sentimiento se aplica a toda la expresión. 

Dado que es una configuración de publicación, no lo ve en las páginas de entidades o intenciones. Puede verlo en el panel de [prueba interactiva](luis-interactive-test.md#view-sentiment-results) o cuando se prueba en la dirección URL del punto de conexión. 

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Usar la aplicación del tutorial existente 
> * Agregar análisis de opiniones como configuración de publicación
> * Train
> * Publicar
> * Obtener las opiniones de expresión del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Uso de una aplicación existente

Continúe con la aplicación creada en el último tutorial, denominada **HumanResources**. 

Si no tiene la aplicación HumanResources del tutorial anterior, siga estos pasos:

1.  Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-keyphrase-HumanResources.json).

2. Importe el archivo JSON en una aplicación nueva.

3. Desde la sección **Manage** (Administrar), en la pestaña **Versions** (Versiones), clone la versión y asígnele el nombre `sentiment`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL.

## <a name="employeefeedback-intent"></a>Intención EmployeeFeedback 
Agregue una nueva intención para capturar los comentarios de los miembros de la empresa. 

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Haga clic en **Create new intent** (Crear intención).

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

## <a name="train"></a>Train

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="configure-app-to-include-sentiment-analysis"></a>Configuración de la aplicación para que incluya el análisis de opiniones
1. Seleccione **Manage** (Administrar) en la navegación superior derecha y luego seleccione **Publish settings** (Publicar configuración) en el menú izquierdo.

2. Cambie el conmutador **Sentiment Analysis** (Análisis de sentimiento) para habilitar esta configuración. 

    ![Activación del Análisis de sentimiento como configuración de publicación](./media/luis-quickstart-intent-and-sentiment-analysis/turn-on-sentiment-analysis-as-publish-setting.png)

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-sentiment-of-utterance-from-endpoint"></a>Obtener las opiniones de expresión del punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vaya al final de la dirección URL en la dirección y escriba `Jill Jones work with the media team on the public portal was amazing`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que cualquiera de las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `EmployeeFeedback` con el análisis de sentimiento extraído.
    
    ```json
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

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes
Este tutorial agrega el análisis de opiniones como una configuración de publicación para extraer los valores de las opiniones de la expresión en su conjunto.

> [!div class="nextstepaction"] 
> [Revisión de las expresiones de punto de conexión en la aplicación de RR. HH.](luis-tutorial-review-endpoint-utterances.md) 

