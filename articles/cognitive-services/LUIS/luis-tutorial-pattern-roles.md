---
title: 'Tutorial de uso de roles de patrón para mejorar las predicciones de LUIS: Azure | Microsoft Docs'
titleSuffix: Cognitive Services
description: En este tutorial, use roles de patrón para las entidades relacionadas contextualmente para mejorar las predicciones de LUIS.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 07/20/2018
ms.author: diberry
ms.openlocfilehash: 80f2d482a660992011549fe8dc76a09740da6ab1
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39237832"
---
# <a name="tutorial-improve-app-with-pattern-roles"></a>Tutorial: Mejorar la aplicación con roles de patrón

En este tutorial, utilice una entidad simple con los roles combinados con patrones para aumentar la predicción de entidades e intenciones.  Al usar patrones, se necesitan menos expresiones de ejemplo para la intención.

> [!div class="checklist"]
* Descripción de los roles de patrón
* Uso de la entidad simple con roles 
* Creación del patrón para expresiones mediante una entidad simple con roles
* Cómo comprobar las mejoras de la predicción de patrones

Para este artículo, necesita una cuenta gratuita de [LUIS](luis-reference-regions.md) para crear la aplicación.

## <a name="before-you-begin"></a>Antes de empezar
Si no tiene la aplicación de recursos humanos del tutorial sobre [patrones](luis-tutorial-pattern.md), [importe](luis-how-to-start-new-app.md#import-new-app) el archivo JSON a una nueva aplicación en el sitio web de [LUIS](luis-reference-regions.md#luis-website). La aplicación que se va a importar se encuentra en el repositorio de GitHub [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-patterns-HumanResources-v2.json).

Si quiere conservar la aplicación original de Recursos humanos, clone la versión en la página [Configuración](luis-how-to-manage-versions.md#clone-a-version) y llámela `roles`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. 

## <a name="the-purpose-of-roles"></a>El propósito de los roles
El propósito de los roles es extraer las entidades relacionadas contextualmente de una expresión. En la expresión, `Move new employee Robert Williams from Sacramento and San Francisco`, los valores de ciudad de origen y ciudad de destino se relacionan entre sí y usan un lenguaje común para denotar cada ubicación. 

Al usar patrones, se deben detectar todas las entidades en el patrón _antes_ de que el patrón coincida con la expresión. 

Cuando se crea un modelo, el primer paso es seleccionar la intención del patrón. Al seleccionar la intención, si el patrón coincide, la intención correcta siempre se devuelve con una puntuación alta (habitualmente de 99 % a 100 %). 

### <a name="compare-hierarchical-entity-to-simple-entity-with-roles"></a>Comparación de una entidad jerárquica con una entidad simple con roles

En el [tutorial sobre entidades jerárquicas](luis-quickstart-intent-and-hier-entity.md), la intención **MoveEmployee** detectó cuándo se debe trasladar un empleado existente de un edificio y oficina a otro. Las expresiones de ejemplo tenían las ubicaciones de origen y destino, pero no usaban roles. En lugar de eso, el origen y destino eran elementos secundarios de la entidad jerárquica. 

En este tutorial, la aplicación de recursos humanos detecta expresiones sobre cómo trasladar los empleados nuevos de una ciudad a otra. Estos dos tipos de expresiones son similares pero se resuelven mediante distintas funcionalidades de LUIS.

|Tutorial|Expresión de ejemplo|Ubicaciones de origen y destino|
|--|--|--|
|[Jerárquica (sin roles)](luis-quickstart-intent-and-hier-entity.md)|mv Jill Jones from **a-2349** to **b-1298**|a-2349, b-1298|
|Este tutorial (con roles)|Move Billy Patterson from **Yuma** to **Denver**.|Yuma, Denver|

No puede usar la entidad jerárquica en el patrón porque solo los elementos principales jerárquicos se usan en los elementos principales. Para volver a las ubicaciones con nombre de origen y destino, debe usar un patrón.

### <a name="simple-entity-for-new-employee-name"></a>Entidad simple para el nombre del empleado nuevo
El nombre del empleado nuevo, Billy Patterson, todavía no forma parte de la entidad de la lista **Employee** (Empleado). En primer lugar, se extrae el nombre del empleado nuevo con el fin de enviar el nombre a un sistema externo para crear las credenciales de la empresa. Una vez creadas las credenciales de la empresa, las credenciales del empleado se agregan a la entidad de la lista **Employee** (Empleado).

La lista **Employee** (Empleado) se creó en el [tutorial sobre las listas](luis-quickstart-intent-and-list-entity.md).

La entidad **NewEmployee** es una entidad simple sin roles. 

### <a name="simple-entity-with-roles-for-relocation-cities"></a>Entidad simple con roles para ciudades de reubicación
Es necesario trasladar al empleado nuevo y su familia desde la ciudad actual a una ciudad donde está ubicada la empresa ficticia. Como un empleado nuevo puede provenir de cualquier ciudad, es necesario detectar las ubicaciones. Una lista de conjuntos, como una entidad de lista, no funcionará porque se solo se extraerán las ciudades de la lista.

Los nombres de roles asociados con las ciudades de origen y destino deben ser únicos en todas las entidades. Una manera sencilla de garantizar que los roles son únicos es vincularlos a la entidad contenedora a través de una estrategia de nomenclatura. La entidad **NewEmployeeRelocation** es una entidad simple con dos roles: **NewEmployeeReloOrigin** y **NewEmployeeReloDestination**.

### <a name="simple-entities-need-enough-examples-to-be-detected"></a>La detección de entidades simples requiere ejemplos suficientes
Dado que la expresión de ejemplo `Move new employee Robert Williams from Sacramento and San Francisco` solo tiene entidades de aprendizaje automático, es importante proporcionar expresiones de ejemplo suficientes a la intención para que se detecten las entidades.  

**Si bien los patrones permiten proporcionar menos expresiones de ejemplo, si no se detectan las entidades, el patrón no coincide.**

Si tiene dificultades con la detección de la entidad simple porque se trata de un nombre como una ciudad, considere la posibilidad de agregar una lista de frases de valores similares. Esto ayuda a la detección del nombre de la ciudad al proporcionar a LUIS una señal adicional sobre ese tipo de palabra o frase. Las listas de frases solo ayudan al patrón al contribuir a la detección de la entidad, algo necesario para que el patrón coincida. 

## <a name="create-new-entities"></a>Creación de entidades nuevas
1. Seleccione **Build** (Compilación) en el menú superior.

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

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS
La intención y las expresiones nuevas requieren entrenamiento. 

1. En la parte superior derecha del sitio web de LUIS, haga clic en el botón **Entrenar**.

2. El entrenamiento se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicación de la aplicación para obtener la dirección URL del punto de conexión
Para obtener una predicción de LUIS en un bot de chat u otra aplicación, tiene que publicar la aplicación. 

1. En la parte superior derecha del sitio web de LUIS, haga clic en el botón **Publish** (Publicar). 

2. Seleccione el espacio de producción y haga clic en el botón **Publicar**.

3. La publicación se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

## <a name="query-the-endpoint-without-pattern"></a>Consulta del punto de conexión sin patrón
1. En la página **Publish** (Publicar), seleccione el vínculo **endpoint** (Punto de conexión) en la parte inferior de la página. Esta acción abre otra ventana del explorador con la dirección URL del punto de conexión en la barra de direcciones. 

2. Vaya al final de la dirección URL en la dirección y escriba `Move Wayne Berry from Miami to Mount Vernon`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. 

    ```JSON
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

Los patrones ayudarán en la puntuación de predicción, pero las entidades se deben predecir correctamente antes de que el patrón coincida con la expresión. 

## <a name="add-a-pattern-that-uses-roles"></a>Incorporación de un patrón que usa roles
1. Seleccione **Build** (Compilación) en el menú superior.

2. Seleccione **Patterns** (Patrones) en el menú de navegación de la izquierda.

3. Seleccione **NewEmployeeRelocationProcess** en la lista desplegable **Select an intent** (Seleccionar una intención). 

4. Escriba el patrón siguiente: `move {NewEmployee} from {NewEmployeeRelocation:NewEmployeeReloOrigin} to {NewEmployeeRelocation:NewEmployeeReloDestination}[.]`

    Si entrena, publica y consulta el punto de conexión, puede que se decepcione al ver que no se encuentran las entidades, por lo que el patrón no coincidió y, por tanto, la predicción no mejoró. Esta es una consecuencia de que no tener expresiones suficientes con entidades etiquetadas. En lugar de agregar más ejemplos, agregue una lista de frases para corregir este problema.

## <a name="create-a-phrase-list-for-cities"></a>Creación de una lista de frases para las ciudades
Las ciudades, al igual que lo que ocurre con los nombres de personas, son complicadas porque pueden ser cualquier combinación de palabras y signos de puntuación. Pero las ciudades de la región y del mundo son conocidas, por lo que LUIS necesita una lista de frases para las ciudades para empezar el aprendizaje. 

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

    No agregue todas las ciudades del mundo ni tampoco de la región. LUIS debe ser capaz de generalizar qué es una ciudad a partir de la lista. 

    Asegúrese de que **These values are interchangeable** (Estos valores son intercambiables) esté seleccionado. Esta configuración significa que las palabras de la lista se tratan como sinónimos. Esto es exactamente cómo se deben tratar en el patrón.

    Recuerde que [la última vez](luis-quickstart-primary-and-secondary-data.md) que la serie de tutoriales creó una lista de frases también fue para mejorar la detección de entidad para una entidad simple.  

3. Entrene y publique la aplicación.

## <a name="query-endpoint-for-pattern"></a>Consulta de punto de conexión para un patrón
1. En la página **Publish** (Publicar), seleccione el vínculo **endpoint** (Punto de conexión) en la parte inferior de la página. Esta acción abre otra ventana del explorador con la dirección URL del punto de conexión en la barra de direcciones. 

2. Vaya al final de la dirección URL en la dirección y escriba `Move wayne berry from miami to mount vernon`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. 

    ```JSON
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
Cuando ya no sea necesaria, elimine la aplicación de LUIS. Para hacerlo, seleccione los puntos suspensivos (***...***) a la derecha del nombre de la aplicación en la lista de aplicaciones y, después, seleccione **Delete** (Eliminar). En el cuadro de diálogo emergente **Delete app?** (¿Eliminar aplicación?), haga clic en **Ok** (Aceptar).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Información sobre los procedimientos recomendados de las aplicaciones de LUIS](luis-concept-best-practices.md)