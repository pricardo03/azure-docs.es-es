---
title: 'Tutorial 5: Relaciones de elementos primarios y secundarios: entidades jerárquicas de LUIS para datos aprendidos contextualmente'
titleSuffix: Azure Cognitive Services
description: Busque datos relacionados de datos en función del contexto. Por ejemplo, las ubicaciones de origen y destino para un traslado físico de un edificio y oficina a otro edificio y oficina están relacionadas.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: a469bc600715b2e276d6654596da50d75659aadb
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2018
ms.locfileid: "48831031"
---
# <a name="tutorial-5-extract-contextually-related-data"></a>Tutorial 5: Extracción de datos relacionados contextualmente
En este tutorial, busque datos relacionados en función del contexto. Por ejemplo, las ubicaciones de origen y destino para un traslado físico de un edificio y oficina a otro edificio y oficina están relacionadas. Para generar un pedido de trabajo, ambos datos pueden ser necesarios y están relacionados entre sí.  

Esta aplicación determina dónde se debe desplazar a un empleado de la ubicación de origen (edificio y oficina) a la ubicación de destino (edificio y oficina). Utiliza la entidad jerárquica para determinar las ubicaciones dentro de la expresión. El propósito de la entidad **jerárquica** es encontrar datos relacionados en la expresión en función del contexto. 

La entidad jerárquica es una buena opción para este tipo de datos debido a las dos partes de datos:

* Son entidades simples.
* Se relacionan entre sí en el contexto de la expresión.
* Utilice una selección de palabras específica para indicar cada ubicación. Ejemplos de estas palabras incluyen: from/to, leaving/headed to, away from/toward.
* Ambas ubicaciones suelen estar en la misma expresión. 
* Debe estar agrupadas y procesarse por la aplicación cliente como una unidad de información.

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Usar la aplicación del tutorial existente
> * Agregar intención 
> * Agregar la entidad jerárquica de ubicación con los elementos secundarios de origen y destino
> * Train
> * Publicar
> * Obtener intenciones y entidades del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Usar una aplicación existente
Continúe con la aplicación creada en el último tutorial, denominada **HumanResources**. 

Si no tiene la aplicación HumanResources del tutorial anterior, siga estos pasos:

1.  Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-list-HumanResources.json).

2. Importe el archivo JSON en una aplicación nueva.

3. Desde la sección **Manage** (Administrar), en la pestaña **Versions** (Versiones), clone la versión y asígnele el nombre `hier`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL. 

## <a name="remove-prebuilt-number-entity-from-app"></a>Eliminación de la entidad de número creada previamente de la aplicación
Para ver la expresión completa y marcar los elementos secundarios jerárquicos, quite temporalmente la entidad numérica creada previamente.

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Seleccione **Entities** (Entidades) en el menú izquierdo.

3. Active la casilla a la izquierda de la entidad numérica de la lista. Seleccione **Eliminar**. 

## <a name="add-utterances-to-moveemployee-intent"></a>Adición de expresiones en la intención MoveEmployee

1. Seleccione **Intents** (Intenciones) en el menú izquierdo.

2. Seleccione **MoveEmployee** en la lista de intenciones.

3. Agregue las siguientes expresiones de ejemplo:

    |Expresiones de ejemplo|
    |--|
    |Move John W. Smith **to** a-2345|
    |Direct Jill Jones **to** b-3499|
    |Organize the move of x23456 **from** hh-2345 **to** e-0234|
    |Begin paperwork to set x12345 **leaving** a-3459 **headed to** f-34567|
    |Displace 425-555-0000 **away from** g-2323 **toward** hh-2345|

    [ ![Captura de pantalla de LUIS con nuevas expresiones en la intención MoveEmployee](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png)](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png#lightbox)

    En el tutorial de [lista de entidades](luis-quickstart-intent-and-list-entity.md), se designa a un empleado por nombre, dirección de correo electrónico, extensión telefónica, número de teléfono móvil o número del seguro social federal de Estados Unidos. Estos números de empleado se utilizan en las expresiones. Las expresiones de ejemplo anteriores incluyen diferentes maneras de anotar las ubicaciones de origen y destino, marcadas en negrita. Un par de expresiones solo tienen destinos de forma intencionada. Esto ayuda a LUIS a entender cómo se colocan esas ubicaciones en la expresión cuando no se especifica el origen.     

    [!include[Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="create-a-location-entity"></a>Creación de una entidad de ubicación
LUIS necesita entender lo que es una localización mediante el etiquetado del origen y del destino en las expresiones. Si necesita ver el enunciado en la vista de token (sin formato), seleccione el conmutador en la barra encima de las expresiones etiquetadas como **Entities View** (Vista de entidades). Después de cambiar el modificador, el control tiene la etiqueta **Tokens View** (Vista de tokens).

Considere la siguiente expresión:

```JSON
mv Jill Jones from a-2349 to b-1298
```

La expresión tiene dos ubicaciones especificadas, `a-2349` y `b-1298`. Supongamos que la letra corresponde al nombre de un edificio y el número indica la oficina dentro de ese edificio. Tiene sentido que ambos estén agrupados como elementos secundarios de una entidad jerárquica, `Locations` porque ambos datos deben extraerse de la expresión para completar la solicitud en la aplicación cliente y están relacionados entre sí. 
 
Aunque solo exista un elemento secundario (origen o destino) de una entidad jerárquica, se extraerá. No es preciso encontrar todos los elementos secundarios si solo se va a extraer uno, o varios. 

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

3. Seleccione **number** (número) en la lista de entidades predeterminadas, a continuación, seleccione **Done** (Listo).

    ![Captura de pantalla del cuadro de diálogo de la selección de número en entidades creadas previamente](./media/luis-quickstart-intent-and-hier-entity/hr-add-number-back-ddl.png)

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicación de la aplicación para obtener la dirección URL del punto de conexión

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consulta del punto de conexión con una expresión diferente

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


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
    
    La intención correcta se predice y la matriz de entidades tiene los valores de origen y destino en la propiedad **entity** correspondiente.
    

## <a name="could-you-have-used-a-regular-expression-for-each-location"></a>¿Podría haber usado una expresión regular para cada ubicación?
Sí, cree la entidad de expresión regular con los roles de origen y destino y úsela en un patrón.

Las ubicaciones en este ejemplo, como `a-1234`, siguen un formato específico de una o dos letras con un guion y luego una serie de 4 o 5 números. Estos datos pueden describirse como una entidad de expresión regular con una función para cada ubicación. Los roles solo están disponibles para los patrones. Puede crear patrones basados en estas expresiones y, después, crear una expresión regular para el formato de ubicación y agregarla a los patrones. 

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes
Este tutorial creó una nueva intención y agregó expresiones de ejemplo para los datos aprendidos contextualmente de las ubicaciones de origen y destino. Una vez que la aplicación está entrenada y publicada, una aplicación cliente puede usar esa información para crear un vale de movimiento con la información relevante.

> [!div class="nextstepaction"] 
> [Más información sobre cómo agregar una entidad compuesta](luis-tutorial-composite-entity.md) 