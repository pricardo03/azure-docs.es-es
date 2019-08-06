---
title: 'Roles de patrón: LUIS'
titleSuffix: Azure Cognitive Services
description: Los patrones extraen datos de expresiones de plantillas con el formato correcto. En la expresión de plantilla se usa una entidad sencilla y roles para extraer datos relacionados como las ubicaciones de origen y destino.
ms.custom: seodec18
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 07/16/2019
ms.author: diberry
ms.openlocfilehash: 7586a81eac95a2e4a08b045b3a2826132d9919f7
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560034"
---
# <a name="tutorial-extract-contextually-related-patterns-using-roles"></a>Tutorial: Extracción de patrones relacionados contextualmente mediante roles

En este tutorial, use un patrón para extraer datos de una expresión de plantilla con el formato correcto. En la expresión de plantilla se usa una [entidad sencilla](luis-concept-entity-types.md#simple-entity) y [roles](luis-concept-roles.md) para extraer datos relacionados como las ubicaciones de origen y destino.  Al usar patrones, se necesitan menos expresiones de ejemplo para la intención.


**En este tutorial, aprenderá a:**

> [!div class="checklist"]
> * Importar la aplicación de ejemplo
> * Creación de entidades nuevas
> * Creación de una intención nueva
> * Train
> * Publicar
> * Obtener intenciones y entidades del punto de conexión
> * Crear un patrón con roles
> * Crear una lista de frases de ciudades
> * Obtener intenciones y entidades del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="using-roles-in-patterns"></a>Uso de roles en patrones

El propósito de los roles es extraer las entidades relacionadas contextualmente de una expresión. En la expresión, `Move new employee Robert Williams from Sacramento and San Francisco`, los valores de ciudad de origen y ciudad de destino se relacionan entre sí y usan un lenguaje común para denotar cada ubicación. 


El nombre del empleado nuevo, Billy Patterson, todavía no forma parte de la entidad de la lista **Employee** (Empleado). En primer lugar, se extrae el nombre del empleado nuevo con el fin de enviar el nombre a un sistema externo para crear las credenciales de la empresa. Una vez creadas las credenciales de la empresa, las credenciales del empleado se agregan a la entidad de la lista **Employee** (Empleado).

Es necesario trasladar al empleado nuevo y su familia desde la ciudad actual a una ciudad donde está ubicada la empresa ficticia. Como un empleado nuevo puede provenir de cualquier ciudad, es necesario detectar las ubicaciones. Una lista de conjuntos, como una entidad de lista, no funcionará porque se solo se extraerán las ciudades de la lista.

Los nombres de roles asociados con las ciudades de origen y destino deben ser únicos en todas las entidades. Una manera sencilla de garantizar que los roles son únicos es vincularlos a la entidad contenedora a través de una estrategia de nomenclatura. La entidad **NewEmployeeRelocation** es una entidad simple con dos roles: **NewEmployeeReloOrigin** y **NewEmployeeReloDestination**. "Relo" es la abreviatura de "reubicación".

Dado que la expresión de ejemplo `Move new employee Robert Williams from Sacramento and San Francisco` solo tiene entidades de aprendizaje automático, es importante proporcionar expresiones de ejemplo suficientes a la intención para que se detecten las entidades.  

**Si bien los patrones permiten proporcionar menos expresiones de ejemplo, si no se detectan las entidades, el patrón no coincide.**

Si tiene dificultades con la detección de la entidad simple porque se trata de un nombre como una ciudad, considere la posibilidad de agregar una lista de frases de valores similares. Esto ayuda a la detección del nombre de la ciudad al proporcionar a LUIS una señal adicional sobre ese tipo de palabra o frase. Las listas de frases solo ayudan al patrón al contribuir a la detección de la entidad, algo necesario para que el patrón coincida. 

## <a name="import-example-app"></a>Importar la aplicación de ejemplo
Continúe con la aplicación creada en el último tutorial, denominada **HumanResources**. 

Para ello, siga los pasos que se describen a continuación:

1.  Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-patterns-HumanResources-v2.json).

2. Importe el archivo JSON en una aplicación nueva.

3. Desde la sección **Manage** (Administrar), en la pestaña **Versions** (Versiones), clone la versión y asígnele el nombre `roles`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL.

## <a name="create-new-entities"></a>Creación de entidades nuevas

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Seleccione **Entities** (Entidades) en el menú de navegación de la izquierda. 

3. Haga clic en **Crear nueva entidad**.

4. En la ventana emergente, escriba `NewEmployee` como una entidad **Simple**.

5. Haga clic en **Crear nueva entidad**.

6. En la ventana emergente, escriba `NewEmployeeRelocation` como una entidad **Simple**.

7. Seleccione **NewEmployeeRelocation** en la lista de entidades. 

8. Escriba el primer rol como `NewEmployeeReloOrigin` y seleccione ENTRAR.

9. Escriba el segundo rol como `NewEmployeeReloDestination` y seleccione ENTRAR.

## <a name="create-new-intent"></a>Creación de una intención nueva
Etiquetar las entidades en estos pasos puede ser más sencillo si se quita la entidad keyPhrase precompilada antes de empezar y luego se agrega después de que haya completado los pasos descritos en esta sección. 

1. Haga clic en **Intents** (Intenciones) en el panel de navegación izquierdo.

2. Haga clic en **Create new intent** (Crear intención). 

3. Escriba `NewEmployeeRelocationProcess` como el nombre de la intención en el cuadro de diálogo emergente.

4. Escriba las expresiones de ejemplo siguientes y etiquete las entidades nuevas. Los valores de entidad y rol aparecen en negrita. No olvide cambiar a la **vista de token** si le resulta más fácil para etiquetar el texto. 

    No se especifica el rol de la entidad al etiquetar la intención. Se hace más adelante cuando se crea el patrón. 

    |Expresión|NewEmployee|NewEmployeeRelocation|
    |--|--|--|
    |Move **Bob Jones** from **Seattle** to **Los Colinas**|Bob Jones|Seattle, Los Colinas|
    |Move **Dave C. Cooper** from **Redmond** to **New York City**|Dave C. Cooper|Redmond, New York City|
    |Move **Jim Paul Smith** from **Toronto** to **West Vancouver**|Jim Paul Smith|Toronto, West Vancouver|
    |Move **J. Benson** from **Boston** to **Staines-upon-Thames**|J. Benson|Boston, Staines-upon-Thames|
    |Move **Travis "Trav" Hinton** from **Castelo Branco** to **Orlando**|Travis "Trav" Hinton|Castelo Branco, Orlando|
    |Move **Trevor Nottington III** from **Aranda de Duero** to **Boise**|Trevor Nottington III|Aranda de Duero, Boise|
    |Move **Dr. Greg Williams** from **Orlando** to **Ellicott City**|Dr. Greg Williams|Orlando, Ellicott City|
    |Move **Robert "Bobby" Gregson** from **Kansas City** to **San Juan Capistrano**|Robert "Bobby" Gregson|Kansas City, San Juan Capistrano|
    |Move **Patti Owens** from **Bellevue** to **Rockford**|Patti Owens|Bellevue, Rockford|
    |Move **Janet Bartlet** from **Tuscan** to **Santa Fe**|Janet Bartlet|Tuscan, Santa Fe|

    El nombre del empleado tiene una variedad de prefijos, recuentos de palabras, sintaxis y sufijos. Esto es importante para que LUIS comprenda las variaciones de un nombre de empleado nuevo. Los nombres de ciudades también tienen una variedad de sintaxis y recuentos de palabras. Esta variedad es importante para enseñar a LUIS cómo estas entidades pueden aparecer en la expresión de un usuario. 
    
    Si ninguna de estas entidades ha tenido el mismo recuento de palabras y ninguna otra variación, se enseñaría a LUIS que esta entidad solo tiene ese recuento de palabras y no hay otras variaciones. LUIS no podría predecir correctamente un conjunto más amplio de variaciones porque no se muestra ninguna. 

    Si quitó la entidad keyPhrase, vuelva a agregarla ahora a la aplicación.

## <a name="train"></a>Train

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Obtención de intenciones y entidades del punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Vaya al final de la dirección URL en la dirección y escriba `Move Wayne Berry from Miami to Mount Vernon`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. 

    ```json
    {
      "query": "Move Wayne Berry from Newark to Columbus",
      "topScoringIntent": {
        "intent": "NewEmployeeRelocationProcess",
        "score": 0.514479756
      },
      "intents": [
        {
          "intent": "NewEmployeeRelocationProcess",
          "score": 0.514479756
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.017118983
        },
        {
          "intent": "MoveEmployee",
          "score": 0.009982505
        },
        {
          "intent": "GetJobInformation",
          "score": 0.008637771
        },
        {
          "intent": "ApplyForJob",
          "score": 0.007115978
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.006120186
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.00452428637
        },
        {
          "intent": "None",
          "score": 0.00400899537
        },
        {
          "intent": "OrgChart-Reports",
          "score": 0.00240071164
        },
        {
          "intent": "Utilities.Help",
          "score": 0.001770991
        },
        {
          "intent": "EmployeeFeedback",
          "score": 0.001697356
        },
        {
          "intent": "OrgChart-Manager",
          "score": 0.00168116146
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00163952739
        },
        {
          "intent": "FindForm",
          "score": 0.00112958835
        }
      ],
      "entities": [
        {
          "entity": "wayne berry",
          "type": "NewEmployee",
          "startIndex": 5,
          "endIndex": 15,
          "score": 0.629158735
        },
        {
          "entity": "newark",
          "type": "NewEmployeeRelocation",
          "startIndex": 22,
          "endIndex": 27,
          "score": 0.638941
        }
      ]
    }  
    ```

La puntuación de predicción de la intención es de solo un 50 %. Si la aplicación cliente requiere un número más alto, debe solucionarlo. Las entidades tampoco se predecían.

Se ha extraído una de las ubicaciones pero la otra no. 

Los patrones ayudarán en la puntuación de predicción, pero las entidades se deben predecir correctamente antes de que el patrón coincida con la expresión. 

## <a name="pattern-with-roles"></a>Patrón con roles

1. Seleccione **Build** (Compilación) en el menú superior.

2. Seleccione **Patterns** (Patrones) en el menú de navegación de la izquierda.

3. Seleccione **NewEmployeeRelocationProcess** en la lista desplegable **Select an intent** (Seleccionar una intención). 

4. Escriba el patrón siguiente: `move {NewEmployee} from {NewEmployeeRelocation:NewEmployeeReloOrigin} to {NewEmployeeRelocation:NewEmployeeReloDestination}[.]`

    Si entrena, publica y consulta el punto de conexión, puede que se decepcione al ver que no se encuentran las entidades, por lo que el patrón no coincidió y, por tanto, la predicción no mejoró. Esta es una consecuencia de que no tener expresiones suficientes con entidades etiquetadas. En lugar de agregar más ejemplos, agregue una lista de frases para corregir este problema.

## <a name="cities-phrase-list"></a>Lista de frases de ciudades
Las ciudades, al igual que lo que ocurre con los nombres de personas, son complicadas porque pueden ser cualquier combinación de palabras y signos de puntuación. Las ciudades de la región y del mundo son conocidas, por lo que LUIS necesita una lista de frases de ciudades para empezar el aprendizaje. 

1. Seleccione **Phrase list** (Lista de frases) en la sección **Improve app performance** (Mejorar el rendimiento de las aplicaciones) del menú de la izquierda. 

2. Asigne el nombre `Cities` a la lista y agregue lo siguiente `values` para ver la lista:

    |Valores de la lista de frases|
    |--|
    |Seattle|
    |San Diego|
    |Ciudad de Nueva York|
    |Los Ángeles|
    |Portland|
    |Filadelfia|
    |Miami|
    |Dallas|

    No agregue todas las ciudades del mundo ni tampoco de la región. LUIS debe ser capaz de generalizar qué es una ciudad a partir de la lista. Asegúrese de que **These values are interchangeable** (Estos valores son intercambiables) esté seleccionado. Esta configuración significa que las palabras de la lista se tratan como sinónimos. 

3. Entrene y publique la aplicación.

## <a name="get-intent-and-entities-from-endpoint"></a>Obtención de intenciones y entidades del punto de conexión

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Vaya al final de la dirección URL en la dirección y escriba `Move wayne berry from miami to mount vernon`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. 

    ```json
    {
      "query": "Move Wayne Berry from Miami to Mount Vernon",
      "topScoringIntent": {
        "intent": "NewEmployeeRelocationProcess",
        "score": 0.9999999
      },
      "intents": [
        {
          "intent": "NewEmployeeRelocationProcess",
          "score": 0.9999999
        },
        {
          "intent": "Utilities.Confirm",
          "score": 1.49678385E-06
        },
        {
          "intent": "MoveEmployee",
          "score": 8.240291E-07
        },
        {
          "intent": "GetJobInformation",
          "score": 6.3131273E-07
        },
        {
          "intent": "None",
          "score": 4.25E-09
        },
        {
          "intent": "OrgChart-Manager",
          "score": 2.8E-09
        },
        {
          "intent": "OrgChart-Reports",
          "score": 2.8E-09
        },
        {
          "intent": "EmployeeFeedback",
          "score": 1.64E-09
        },
        {
          "intent": "Utilities.StartOver",
          "score": 1.64E-09
        },
        {
          "intent": "Utilities.Help",
          "score": 1.48181822E-09
        },
        {
          "intent": "Utilities.Stop",
          "score": 1.48181822E-09
        },
        {
          "intent": "Utilities.Cancel",
          "score": 1.35E-09
        },
        {
          "intent": "FindForm",
          "score": 1.23846156E-09
        },
        {
          "intent": "ApplyForJob",
          "score": 5.692308E-10
        }
      ],
      "entities": [
        {
          "entity": "wayne berry",
          "type": "builtin.keyPhrase",
          "startIndex": 5,
          "endIndex": 15
        },
        {
          "entity": "miami",
          "type": "builtin.keyPhrase",
          "startIndex": 22,
          "endIndex": 26
        },
        {
          "entity": "wayne berry",
          "type": "NewEmployee",
          "startIndex": 5,
          "endIndex": 15,
          "score": 0.9410646,
          "role": ""
        },
        {
          "entity": "miami",
          "type": "NewEmployeeRelocation",
          "startIndex": 22,
          "endIndex": 26,
          "score": 0.9853915,
          "role": "NewEmployeeReloOrigin"
        },
        {
          "entity": "mount vernon",
          "type": "NewEmployeeRelocation",
          "startIndex": 31,
          "endIndex": 42,
          "score": 0.986044347,
          "role": "NewEmployeeReloDestination"
        }
      ],
      "sentimentAnalysis": {
        "label": "neutral",
        "score": 0.5
      }
   }
    ```

La puntuación de intención ahora es mucho más alta y los nombres de rol forman parte de la respuesta de la entidad.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se ha agregado una entidad con roles y una intención con expresiones de ejemplo. La primera predicción de punto de conexión en la que se usaba la entidad predijo correctamente la intención pero con una puntuación de confianza baja. Solo se ha detectado una de las dos entidades. A continuación, en el tutorial se agregó un patrón en el que se usaban los roles de entidad y una lista de frases para aumentar el valor de los nombres de ciudades en las expresiones. La segunda predicción de punto de conexión devolvió una puntuación de confianza elevada y encontró los dos roles de entidad. 

> [!div class="nextstepaction"]
> [Información sobre los procedimientos recomendados de las aplicaciones de LUIS](luis-concept-best-practices.md)
