---
title: Entidad jerárquica
titleSuffix: Azure Cognitive Services
description: Busque datos relacionados de datos en función del contexto. Por ejemplo, las ubicaciones de origen y destino para un traslado físico de un edificio y oficina a otro edificio y oficina están relacionadas.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 91a9646e88adbfaf6d3c3fc0b06b341c647e773f
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2018
ms.locfileid: "53753700"
---
# <a name="tutorial-extract-contextually-related-data-from-an-utterance"></a>Tutorial: Extracción de datos relacionados contextualmente de una expresión

En este tutorial, busque datos relacionados en función del contexto. Por ejemplo, una ubicación de origen y destino para una transferencia de una ciudad a otra. Ambas partes de datos pueden ser necesarias y están relacionadas entre sí.  

**En este tutorial, aprenderá a:**

> [!div class="checklist"]
> * Crear nueva aplicación
> * Agregar intención 
> * Agregar la entidad jerárquica de ubicación con los elementos secundarios de origen y destino
> * Train
> * Publicar
> * Obtener intenciones y entidades del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="hierarchical-data"></a>Datos jerárquicos

Esta aplicación determina dónde se va a mover un empleado desde la ciudad de origen a la ciudad de destino. Utiliza la entidad jerárquica para determinar las ubicaciones dentro de la expresión. 

La entidad jerárquica es una buena opción para este tipo de datos porque las dos partes de datos, ubicaciones secundarias:

* Son entidades simples.
* Se relacionan entre sí en el contexto de la expresión.
* Use una selección de palabras específica para indicar cada entidad. Ejemplos de estas palabras incluyen: from/to, leaving/headed to, away from/toward.
* Ambas ubicaciones secundarias suelen estar en la misma expresión. 
* Debe estar agrupadas y procesarse por la aplicación cliente como una unidad de información.

## <a name="create-a-new-app"></a>Creación de una nueva aplicación

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="create-an-intent-to-move-employees-between-cities"></a>Creación de una intención para mover los empleados entre ciudades

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. Haga clic en **Create new intent** (Crear intención). 

1. Escriba `MoveEmployeeToCity` en el cuadro de diálogo emergente y seleccione **Done** (Listo). 

    ![Captura de pantalla del cuadro de diálogo Create new intent (Crear nueva intención)](./media/luis-quickstart-intent-and-hier-entity/create-new-intent-move-employee-to-city.png)

1. Agregue expresiones de ejemplo a la intención.

    |Expresiones de ejemplo|
    |--|
    |move John W. Smith leaving Seattle headed to Dallas|
    |transfer Jill Jones from Seattle to Cairo|
    |Place John Jackson away from Tampa, coming to Atlanta |
    |move Debra Doughtery to Tulsa from Dallas|
    |mv Jill Jones leaving Cairo headed to Tampa|
    |Shift Alice Anderson to Oakland from Redmond|
    |Carl Chamerlin from San Francisco to Redmond|
    |Transfer Steve Standish from San Diego toward Bellevue |
    |lift Tanner Thompson from Kansas city and shift to Chicago|

    [ ![Captura de pantalla de LUIS con nuevas expresiones en la intención MoveEmployee](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png)](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png#lightbox)

## <a name="create-a-location-entity"></a>Creación de una entidad de ubicación
LUIS necesita entender lo que es una localización mediante el etiquetado del origen y del destino en las expresiones. Si necesita ver el enunciado en la vista de token (sin formato), seleccione el conmutador en la barra encima de las expresiones etiquetadas como **Entities View** (Vista de entidades). Después de cambiar el modificador, el control tiene la etiqueta **Tokens View** (Vista de tokens).

Considere la siguiente expresión:

```json
move John W. Smith leaving Seattle headed to Dallas
```

La expresión tiene dos ubicaciones especificadas, `Seattle` y `Dallas`. Ambas están agrupadas como elementos secundarios de una entidad jerárquica, `Location`, porque ambas partes de datos deben extraerse de la expresión para completar la solicitud en la aplicación cliente y están relacionadas entre sí. 
 
Aunque solo exista un elemento secundario (origen o destino) de una entidad jerárquica, se extraerá. No es preciso encontrar todos los elementos secundarios si solo se va a extraer uno, o varios. 

1. En la expresión, `move John W. Smith leaving Seattle headed to Dallas`, seleccione la palabra `Seattle`. Aparece un menú desplegable con un cuadro de texto en la parte superior. Escriba el nombre de entidad `Location` en el cuadro de texto y, a continuación, seleccione **Create new entity** (Crear nueva entidad) en el menú desplegable. 

    [![Captura de pantalla de la creación de una nueva entidad en la página de intención](media/luis-quickstart-intent-and-hier-entity/create-location-hierarchical-entity-from-example-utterance.png "Screenshot of creating new entity on intent page")](media/luis-quickstart-intent-and-hier-entity/create-location-hierarchical-entity-from-example-utterance.png#lightbox)

1. En la ventana emergente, seleccione el tipo de entidad **Hierarchical** con `Origin` y `Destination` como entidades secundarias. Seleccione **Listo**.

    ![Captura de pantalla del cuadro diálogo emergente de creación de entidad para la nueva entidad de ubicación](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-2.png "Screenshot of entity creation pop-up dialog for new Location entity")

1. La etiqueta de `Seattle` está marcada como `Location` porque LUIS no sabe si el término era el origen, el destino o ninguno de ellos. Seleccione `Seattle`, luego seleccione la **ubicación** y, finalmente, vaya el menú de la derecha y seleccione `Origin`.

    [![Captura de pantalla del cuadro diálogo emergente de etiquetado de entidades para cambiar los elementos secundarios de la entidad Locations](media/luis-quickstart-intent-and-hier-entity/choose-hierarchical-child-entity-from-example-utterance.png "Screenshot of entity labeling pop-up dialog to change locations entity child")](media/luis-quickstart-intent-and-hier-entity/choose-hierarchical-child-entity-from-example-utterance.png#lightbox)

1. Etiquete las demás ubicaciones en todas las otras expresiones. Cuando están marcadas todas las ubicaciones, las expresiones comienzan a parecerse a un patrón. 

    [![Captura de pantalla de la entidad Locations etiquetada en expresiones](media/luis-quickstart-intent-and-hier-entity/all-intents-marked-with-origin-and-destination-location.png "Screenshot of Locations entity labeled in utterances")](media/luis-quickstart-intent-and-hier-entity/all-intents-marked-with-origin-and-destination-location.png#lightbox)

    El subrayado rojo indica que LUIS no está seguro acerca de la entidad. Con entrenamiento se resuelve este problema. 

## <a name="add-example-utterances-to-the-none-intent"></a>Incorporación de expresiones de ejemplo a la intención None 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Entrenamiento de la aplicación para probar los cambios en la intención 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Publicación de la aplicación para que se pueda consultar al modelo entrenado desde el punto de conexión

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Obtención de intención y predicción de entidad desde el punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


1. Vaya al final de la dirección URL en la barra de direcciones y escriba `Please move Carl Chamerlin from Tampa to Portland`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que cualquiera de las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `MoveEmployee` con la entidad jerárquica extraída.

    ```json
    {
      "query": "Please move Carl Chamerlin from Tampa to Portland",
      "topScoringIntent": {
        "intent": "MoveEmployeeToCity",
        "score": 0.979823351
      },
      "intents": [
        {
          "intent": "MoveEmployeeToCity",
          "score": 0.979823351
        },
        {
          "intent": "None",
          "score": 0.0156363435
        }
      ],
      "entities": [
        {
          "entity": "portland",
          "type": "Location::Destination",
          "startIndex": 41,
          "endIndex": 48,
          "score": 0.6044041
        },
        {
          "entity": "tampa",
          "type": "Location::Origin",
          "startIndex": 32,
          "endIndex": 36,
          "score": 0.739491045
        }
      ]
    }
    ```
    
    La intención correcta se predice y la matriz de entidades tiene los valores de origen y destino en la propiedad **entities** correspondiente.
    
## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Información relacionada

* Información de [entidades jerárquicas](luis-concept-entity-types.md)
* [Cómo se realiza el entrenamiento](luis-how-to-train.md)
* [Publicación](luis-how-to-publish-app.md)
* [Prueba de la aplicación en el portal de LUIS](luis-interactive-test.md)
* [Roles frente a entidades jerárquicas](luis-concept-roles.md#roles-versus-hierarchical-entities)
* [Mejora de las predicciones con patrones](luis-concept-patterns.md)

## <a name="next-steps"></a>Pasos siguientes

Este tutorial creó una nueva intención y agregó expresiones de ejemplo para los datos aprendidos contextualmente de las ubicaciones de origen y destino. Una vez que la aplicación está entrenada y publicada, una aplicación cliente puede usar esa información para crear un vale de movimiento con la información relevante.

> [!div class="nextstepaction"] 
> [Más información sobre cómo agregar una entidad compuesta](luis-tutorial-composite-entity.md) 
