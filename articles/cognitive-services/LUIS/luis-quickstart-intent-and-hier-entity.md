---
title: 'Tutorial para crear una aplicación de LUIS que obtenga datos de ubicación: Azure | Microsoft Docs'
description: En este tutorial, va a aprender a crear una aplicación de LUIS sencilla con intenciones y una entidad jerárquica para extraer datos.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 41f74ff00e4fad751d4a2b7ae96ebb048bbcdfcd
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2018
ms.locfileid: "39492705"
---
# <a name="tutorial-5-add-hierarchical-entity"></a>Tutorial: 5. Incorporación de entidad jerárquica
En este tutorial, creará una aplicación que muestra cómo buscar datos relacionados en función del contexto. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Descripción de entidades jerárquicas y elementos secundarios aprendidos por el contexto 
> * Uso de la aplicación de LUIS en el dominio de recursos humanos (RR. HH.) 
> * Agregar la entidad jerárquica de ubicación con los elementos secundarios de origen y destino
> * Entrenamiento y publicación de la aplicación
> * Consulta del punto de conexión de la aplicación para ver la respuesta JSON de LUIS que incluye elementos secundarios jerárquicos 

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Antes de empezar
Si no tiene la aplicación de Recursos humanos del tutorial de [entidades de lista](luis-quickstart-intent-and-list-entity.md), [importe](luis-how-to-start-new-app.md#import-new-app) el archivo JSON a una nueva aplicación en el sitio web [LUIS](luis-reference-regions.md#luis-website). La aplicación que se va a importar se encuentra en el repositorio de GitHub [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-list-HumanResources.json).

Si desea conservar la aplicación original de Recursos humanos, clone la versión en la página [Configuración](luis-how-to-manage-versions.md#clone-a-version) y llámela `hier`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. 

## <a name="purpose-of-the-app-with-this-entity"></a>Propósito de la aplicación con esta entidad
Esta aplicación determina dónde se debe desplazar a un empleado de la ubicación de origen (edificio y oficina) a la ubicación de destino (edificio y oficina). Utiliza la entidad jerárquica para determinar las ubicaciones dentro de la expresión. 

La entidad jerárquica es una buena opción para este tipo de datos debido a las dos partes de datos:

* Se relacionan entre sí en el contexto de la expresión.
* Utilice una selección de palabras específica para indicar cada ubicación. Ejemplos de estas palabras incluyen: from/to, leaving/headed to, away from/toward.
* Ambas ubicaciones suelen estar en la misma expresión. 

El propósito de la entidad **jerárquica** es encontrar datos relacionados en la expresión en función del contexto. Considere la siguiente expresión:

```JSON
mv Jill Jones from a-2349 to b-1298
```
La expresión tiene dos ubicaciones especificadas, `a-2349` y `b-1298`. Supongamos que la letra corresponde al nombre de un edificio y el número indica la oficina dentro de ese edificio. Tiene sentido que ambos estén agrupados como elementos secundarios de una entidad jerárquica, `Locations` porque ambos datos deben extraerse del enunciado y están relacionados entre sí. 
 
Aunque solo exista un elemento secundario (origen o destino) de una entidad jerárquica, se extraerá. No es preciso encontrar todos los elementos secundarios si solo se va a extraer uno, o varios. 

## <a name="remove-prebuilt-number-entity-from-app"></a>Eliminación de la entidad de número creada previamente de la aplicación
Para ver la expresión completa y marcar los elementos secundarios jerárquicos, quite temporalmente la entidad numérica creada previamente.

1. Asegúrese de que la aplicación de recursos humanos se encuentra en la sección **Build** (Crear) de LUIS. Para cambiar a esta sección, seleccione **Build** (Crear) en la barra de menús superior derecha. 

2. Seleccione **Entities** (Entidades) en el menú izquierdo.

3. Seleccione el botón de puntos suspensivos (***...***) a la derecha de la entidad numérica de la lista. Seleccione **Eliminar**. 

    [ ![Captura de pantalla de la aplicación LUIS en la página de la lista de entidades, con el botón Delete (Eliminar) resaltado para la entidad numérica creada previamente](./media/luis-quickstart-intent-and-hier-entity/hr-delete-number-prebuilt.png)](./media/luis-quickstart-intent-and-hier-entity/hr-delete-number-prebuilt.png#lightbox)


## <a name="add-utterances-to-moveemployee-intent"></a>Adición de expresiones en la intención MoveEmployee

1. Seleccione **Intents** (Intenciones) en el menú izquierdo.

2. Seleccione **MoveEmployee** en la lista de intenciones.

    [ ![Captura de pantalla de la aplicación de LUIS con la intención MoveEmployee resaltada en el menú de la izquierda](./media/luis-quickstart-intent-and-hier-entity/hr-intents-list-moveemployee.png)](./media/luis-quickstart-intent-and-hier-entity/hr-intents-list-moveemployee.png#lightbox)

3. Agregue las siguientes expresiones de ejemplo:

    |Expresiones de ejemplo|
    |--|
    |Move John W. Smith **to** a-2345|
    |Direct Jill Jones **to** b-3499|
    |Organize the move of x23456 **from** hh-2345 **to** e-0234|
    |Begin paperwork to set x12345 **leaving** a-3459 **headed to** f-34567|
    |Displace 425-555-0000 **away from** g-2323 **toward** hh-2345|

    [ ![Captura de pantalla de LUIS con nuevas expresiones en la intención MoveEmployee](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png)](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png#lightbox)

    En el tutorial de [lista de entidades](luis-quickstart-intent-and-list-entity.md), se puede designar a un empleado por nombre, dirección de correo electrónico, extensión telefónica, número de teléfono móvil o número del seguro social federal de Estados Unidos. Estos números de empleado se utilizan en las expresiones. Las expresiones de ejemplo anteriores incluyen diferentes maneras de anotar las ubicaciones de origen y destino, marcadas en negrita. Un par de expresiones solo tienen destinos de forma intencionada. Esto ayuda a LUIS a entender cómo se colocan esas ubicaciones en la expresión cuando no se especifica el origen.     

## <a name="create-a-location-entity"></a>Creación de una entidad de ubicación
LUIS necesita entender lo que es una localización mediante el etiquetado del origen y del destino en las expresiones. Si necesita ver el enunciado en la vista de token (sin formato), seleccione el conmutador en la barra encima de las expresiones etiquetadas como **Entities View** (Vista de entidades). Después de cambiar el modificador, el control tiene la etiqueta **Tokens View** (Vista de tokens).

1. En la expresión, `Displace 425-555-0000 away from g-2323 toward hh-2345`, seleccione la palabra `g-2323`. Aparece un menú desplegable con un cuadro de texto en la parte superior. Escriba el nombre de entidad `Locations` en el cuadro de texto y, a continuación, seleccione **Create new entity** (Crear nueva entidad) en el menú desplegable. 

    [![](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-1.png "Captura de pantalla de creación de una entidad en la página de intención")](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-1.png#lightbox)

2. En la ventana emergente, seleccione el tipo de entidad **Hierarchical** con `Origin` y `Destination` como entidades secundarias. Seleccione **Listo**.

    ![](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-2.png "Captura de pantalla del cuadro de diálogo emergente de creación de entidad para la nueva entidad Location (Ubicación)")

3. La etiqueta de `g-2323` está marcada como `Locations` porque LUIS no sabe si el término era el origen, el destino o ninguno de ellos. Seleccione `g-2323`, después **Locations** y, finalmente, vaya el menú de la derecha y seleccione `Origin`.

    [![](media/luis-quickstart-intent-and-hier-entity/hr-label-entity.png "Captura de pantalla del cuadro de diálogo emergente del etiquetado de la entidad para cambiar los elementos secundarios de la entidad de ubicaciones")](media/luis-quickstart-intent-and-hier-entity/hr-label-entity.png#lightbox)

5. Etiquete las otras ubicaciones en todas las demás expresiones mediante la selección del edificio y de la oficina en la expresión, después Locations, a continuación, se sigue el menú a la derecha y se selecciona `Origin` o `Destination`. Cuando se etiquetan todas las ubicaciones, las expresiones de la **vista de tokens** comienzan a parecer un patrón. 

    [![](media/luis-quickstart-intent-and-hier-entity/hr-entities-labeled.png "Captura de pantalla de la entidad Locations etiquetada en las expresiones")](media/luis-quickstart-intent-and-hier-entity/hr-entities-labeled.png#lightbox)

## <a name="add-prebuilt-number-entity-to-app"></a>Adición de la entidad numérica creada previamente a la aplicación
Agregue la entidad numérica creada previamente a la aplicación.

1. Seleccione **Entities** (Entidades) en el menú de navegación izquierdo.

2. Haga clic en el botón **Manage prebuilt entities** (Administrar entidades creadas previamente).

    [ ![Captura de pantalla de la lista entidades con Manage prebuilt entities (Administrar entidades creadas previamente) resaltado](./media/luis-quickstart-intent-and-hier-entity/hr-manage-prebuilt-button.png)](./media/luis-quickstart-intent-and-hier-entity/hr-manage-prebuilt-button.png#lightbox)

3. Seleccione **number** (número) en la lista de entidades predeterminadas, a continuación, seleccione **Done** (Listo).

    ![Captura de pantalla del cuadro de diálogo de la selección de número en entidades creadas previamente](./media/luis-quickstart-intent-and-hier-entity/hr-add-number-back-ddl.png)

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicación de la aplicación para obtener la dirección URL del punto de conexión

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consulta del punto de conexión con una expresión diferente

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


2. Vaya al final de la dirección URL en la barra de direcciones y escriba `Please relocation jill-jones@mycompany.com from x-2345 to g-23456`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que cualquiera de las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `MoveEmployee` con la entidad jerárquica extraída.

  ```JSON
  {
    "query": "Please relocation jill-jones@mycompany.com from x-2345 to g-23456",
    "topScoringIntent": {
      "intent": "MoveEmployee",
      "score": 0.9966052
    },
    "intents": [
      {
        "intent": "MoveEmployee",
        "score": 0.9966052
      },
      {
        "intent": "Utilities.Stop",
        "score": 0.0325253047
      },
      {
        "intent": "FindForm",
        "score": 0.006137873
      },
      {
        "intent": "GetJobInformation",
        "score": 0.00462633232
      },
      {
        "intent": "Utilities.StartOver",
        "score": 0.00415637763
      },
      {
        "intent": "ApplyForJob",
        "score": 0.00382325822
      },
      {
        "intent": "Utilities.Help",
        "score": 0.00249120337
      },
      {
        "intent": "None",
        "score": 0.00130756292
      },
      {
        "intent": "Utilities.Cancel",
        "score": 0.00119622645
      },
      {
        "intent": "Utilities.Confirm",
        "score": 1.26910036E-05
      }
    ],
    "entities": [
      {
        "entity": "jill - jones @ mycompany . com",
        "type": "Employee",
        "startIndex": 18,
        "endIndex": 41,
        "resolution": {
          "values": [
            "Employee-45612"
          ]
        }
      },
      {
        "entity": "x - 2345",
        "type": "Locations::Origin",
        "startIndex": 48,
        "endIndex": 53,
        "score": 0.8520272
      },
      {
        "entity": "g - 23456",
        "type": "Locations::Destination",
        "startIndex": 58,
        "endIndex": 64,
        "score": 0.974032
      },
      {
        "entity": "-2345",
        "type": "builtin.number",
        "startIndex": 49,
        "endIndex": 53,
        "resolution": {
          "value": "-2345"
        }
      },
      {
        "entity": "-23456",
        "type": "builtin.number",
        "startIndex": 59,
        "endIndex": 64,
        "resolution": {
          "value": "-23456"
        }
      }
    ]
  }
  ```

## <a name="could-you-have-used-a-regular-expression-for-each-location"></a>¿Podría haber usado una expresión regular para cada ubicación?
Sí, cree la expresión regular con los roles de origen y destino y úsela en un patrón.

Las ubicaciones en este ejemplo, como `a-1234`, siguen un formato específico de una o dos letras con un guion y luego una serie de 4 o 5 números. Estos datos pueden describirse como una entidad de expresión regular con una función para cada ubicación. Los roles están disponibles para los patrones. Puede crear patrones basados en estas expresiones y, después, crear una expresión regular para el formato de ubicación y agregarla a los patrones. <!-- Go to this tutorial to see how that is done -->

## <a name="patterns-with-roles"></a>Patrones con roles

[!include[LUIS Compare hierarchical entities to patterns with roles](../../../includes/cognitive-services-luis-hier-roles.md)]

## <a name="what-has-this-luis-app-accomplished"></a>¿Qué ha logrado esta aplicación de LUIS?
Esta aplicación, con solo unas pocas intenciones y una entidad jerárquica, ha identificado una intención de consulta en lenguaje natural y ha devuelto los datos extraídos. 

El bot de chat ahora tiene suficiente información para determinar la acción principal, `MoveEmployee`, y la información de ubicación encontrada en la expresión. 

## <a name="where-is-this-luis-data-used"></a>¿Dónde se utilizan estos datos de LUIS? 
LUIS ha terminado con esta solicitud. La aplicación que realiza la llamada, como un bot de chat, puede tomar el resultado de topScoringIntent y los datos de la entidad para realizar el siguiente paso. LUIS no realiza este trabajo de programación para el bot o la aplicación que realiza la llamada. LUIS solo determina cuál es la intención del usuario. 

## <a name="clean-up-resources"></a>Limpieza de recursos

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Más información sobre cómo agregar una entidad compuesta](luis-tutorial-composite-entity.md) 