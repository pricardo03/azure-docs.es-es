---
title: Patrones
titleSuffix: Azure Cognitive Services
description: Use patrones para aumentar la predicción de intención y entidad a la vez que proporciona menos expresiones de ejemplo. El patrón se proporciona por medio de un ejemplo de expresión de plantilla, que incluye la sintaxis para identificar las entidades y el texto que se puede pasar por alto.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 07/16/2019
ms.author: diberry
ms.openlocfilehash: e559d4a3366c45bd054cbf3a235805e048de3493
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68276046"
---
# <a name="tutorial-add-common-pattern-template-utterance-formats"></a>Tutorial: Incorporación de formatos comunes de expresión de plantilla de patrón

En este tutorial se usan patrones para aumentar la predicción de intención y entidad a la vez que se proporcionan menos expresiones de ejemplo. El patrón se proporciona por medio de un ejemplo de expresión de plantilla, que incluye la sintaxis para identificar las entidades y el texto que se puede pasar por alto. Un patrón es una combinación de coincidencia de expresión y aprendizaje automático.  El ejemplo de expresión de plantilla junto con las expresiones de intención permiten que LUIS comprenda mejor qué expresiones se ajustan a la intención. 

**En este tutorial, aprenderá a:**

> [!div class="checklist"]
> * Importar la aplicación de ejemplo 
> * Crear intención
> * Train
> * Publicar
> * Obtener intenciones y entidades del punto de conexión
> * Crear un patrón
> * Comprobar las mejoras de la predicción del patrón
> * Marcar texto como irrelevante y anidar dentro del patrón
> * Usar el panel de prueba para comprobar la corrección del patrón

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Importar la aplicación de ejemplo

Continúe con la aplicación creada en el último tutorial, denominada **HumanResources**. 

Para ello, siga los pasos que se describen a continuación:

1.  Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-batchtest-HumanResources.json).

2. Importe el archivo JSON en una aplicación nueva.

3. Desde la sección **Manage** (Administrar), en la pestaña **Versions** (Versiones), clone la versión y asígnele el nombre `patterns`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL.

## <a name="create-new-intents-and-their-utterances"></a>Creación de intenciones nuevas y sus expresiones

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. En la página **Intents** (Intenciones), seleccione **Create new intent** (Crear intención). 

3. Escriba `OrgChart-Manager` en el cuadro de diálogo emergente y seleccione **Done** (Listo).

    ![Creación de una ventana emergente de mensaje nuevo](media/luis-tutorial-pattern/hr-create-new-intent-popup.png)

4. Agregue expresiones de ejemplo a la intención.

    |Expresiones de ejemplo|
    |--|
    |¿De quién es subordinado John W. Smith?|
    |¿De quién depende John W. Smith?|
    |¿Quién es el gerente de John W. Smith?|
    |¿De quién depende directamente de Jill Jones?|
    |¿Quién es el supervisor de Jill Jones?|

    [![Captura de pantalla de LUIS, agregar nuevas expresiones a una intención](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png "Captura de pantalla de LUIS, agregar nuevas expresiones a una intención")](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png#lightbox)

    No se preocupe si la entidad keyPhrase está etiquetada las expresiones de la intención en lugar de la entidad employee. Ambas se predicen correctamente en el panel de prueba y en el punto de conexión. 

5. Seleccione **Intents** (Intenciones) en el panel de navegación izquierdo.

6. Haga clic en **Create new intent** (Crear intención). 

7. Escriba `OrgChart-Reports` en el cuadro de diálogo emergente y seleccione **Done** (Listo).

8. Agregue expresiones de ejemplo a la intención.

    |Expresiones de ejemplo|
    |--|
    |¿Quiénes son los subordinados de John W. Smith?|
    |¿Quién depende de John W. Smith?|
    |¿De quién es supervisor John W. Smith?|
    |¿Quiénes dependen directamente de Jill Jones?|
    |¿De quién es supervisora Jill Jones?|

## <a name="caution-about-example-utterance-quantity"></a>Precaución sobre la cantidad de expresiones de ejemplo

[!INCLUDE [Too few examples](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]

## <a name="train"></a>Train

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Obtención de intenciones y entidades del punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vaya al final de la dirección URL en la dirección y escriba `Who is the boss of Jill Jones?`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. 

    ```json
    {
        "query": "who is the boss of jill jones?",
        "topScoringIntent": {
            "intent": "OrgChart-Manager",
            "score": 0.353984952
        },
        "intents": [
            {
                "intent": "OrgChart-Manager",
                "score": 0.353984952
            },
            {
                "intent": "OrgChart-Reports",
                "score": 0.214128986
            },
            {
                "intent": "EmployeeFeedback",
                "score": 0.08434003
            },
            {
                "intent": "MoveEmployee",
                "score": 0.019131
            },
            {
                "intent": "GetJobInformation",
                "score": 0.004819009
            },
            {
                "intent": "Utilities.Confirm",
                "score": 0.0043958663
            },
            {
                "intent": "Utilities.StartOver",
                "score": 0.00312064588
            },
            {
                "intent": "Utilities.Cancel",
                "score": 0.002265454
            },
            {
                "intent": "Utilities.Help",
                "score": 0.00133465114
            },
            {
                "intent": "None",
                "score": 0.0011388344
            },
            {
                "intent": "Utilities.Stop",
                "score": 0.00111166481
            },
            {
                "intent": "FindForm",
                "score": 0.0008900076
            },
            {
                "intent": "ApplyForJob",
                "score": 0.0007836131
            }
        ],
        "entities": [
            {
                "entity": "jill jones",
                "type": "Employee",
                "startIndex": 19,
                "endIndex": 28,
                "resolution": {
                    "values": [
                        "Employee-45612"
                    ]
                }
            },
            {
                "entity": "boss of jill jones",
                "type": "builtin.keyPhrase",
                "startIndex": 11,
                "endIndex": 28
            }
        ]
    }
    ```

¿Se realizó correctamente esta consulta? Para este ciclo de entrenamiento, sí. Las puntuaciones de las dos intenciones principales son similares. Como el entrenamiento de LUIS no es exactamente igual cada vez, hay un poco de variación, que estas dos puntuaciones se podrían invertir en el próximo ciclo de entrenamiento. El resultado es que se podría devolver la intención incorrecta. 

Use los patrones para que la puntuación de la intención correcta sea considerablemente más alta en porcentaje y esté más lejos de la puntuación más alta siguiente. 

Deje esta segunda ventana del explorador abierta. Se usará más adelante en el tutorial. 

## <a name="template-utterances"></a>Expresiones de plantilla
Dada la naturaleza del dominio de recursos humanos, hay algunas formas habituales de preguntar por relaciones de empleados en las organizaciones. Por ejemplo:

|Grabaciones de voz|
|--|
|Who does Jill Jones report to? (¿A quién informa Jill Jones?)|
|Who reports to Jill Jones? (¿Quién informa a Jill Jones?)|

Estas expresiones son demasiado cercanas como para determinar la singularidad contextual de cada una de ellas sin proporcionar muchos ejemplos de expresiones. Al agregar un patrón para una intención, LUIS aprende patrones de expresión comunes para una intención sin proporcionar muchos ejemplos de expresiones. 

Los ejemplos de expresiones de plantilla de esta intención incluyen:

|Ejemplos de expresiones de plantilla|significado de sintaxis|
|--|--|
|[¿]Quién depende de {Employee}[?]|intercambiable {Employee}, omitir [?]}|
|[¿]Quién depende de {Employee}[?]|intercambiable {Employee}, omitir [?]}|

La sintaxis `{Employee}` marca la ubicación de la entidad dentro de la expresión de plantilla, así como qué entidad. La sintaxis opcional, `[?]`, marca palabras, o signos de puntuación, que son opcionales. LUIS coincide con la expresión y omite el texto opcional entre corchetes.

Aunque la sintaxis se asemeja a las expresiones regulares, no son expresiones regulares. Solo se admite la sintaxis de llave, `{}`, y corchete, `[]`. Se pueden anidar hasta dos niveles.

Para que un patrón coincida con una expresión, las entidades dentro de la expresión primero tienen que coincidir con las entidades de la expresión de plantilla. Sin embargo, la plantilla no ayuda a predecir las entidades, solo las intenciones. 

**Si bien los patrones permiten proporcionar menos expresiones de ejemplo, si no se detectan las entidades, el patrón no coincide.**

## <a name="add-the-patterns-for-the-orgchart-manager-intent"></a>Agregar los patrones de la intención de OrgChart-Manager

1. Seleccione **Build** (Compilación) en el menú superior.

2. En el panel de navegación izquierdo, debajo de **Improve app performance** (Mejorar el rendimiento de la aplicación), seleccione **Patrones**.

3. Seleccione la intención **OrgChart-Manager** y luego escriba las siguientes expresiones de plantilla:

    |Expresiones de plantilla|
    |:--|
    |[¿]De quién es subordinado {Employee}[?]|
    |[¿]Quién depende de {Employee}[?]|
    |[¿]Quién es el administrador de {Employee}[?]|
    |[¿]De quién depende directamente {Employee}[?]|
    |[¿]Quién es el supervisor de {Employee}[?]|
    |[¿]Quién es el jefe de {Employee}[?]|

    Las entidades con roles usan una sintaxis que incluye el nombre del rol y se describen en un [tutorial independiente para los roles](luis-tutorial-pattern-roles.md). 

    Si escribe la expresión de plantilla, LUIS lo ayuda a rellenar la entidad al escribir la llave de apertura, `{`.

    [![Captura de pantalla del ingreso de expresiones de plantilla para la intención](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png)](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png#lightbox)

4. Mientras sigue en la página de patrones, seleccione la intención **OrgChart-Reports** y luego escriba las siguientes expresiones de plantilla:

    |Expresiones de plantilla|
    |:--|
    |[¿]Quiénes son los subordinados de {Employee}[?]|
    |[¿]Quién depende de {Employee}[?]|
    |[¿]De quién es supervisor {Employee}[?]|
    |[¿]Quiénes dependen directamente de {Employee}[?]|
    |[¿]De quién es supervisora {Employee}[?]|
    |[¿]Quién es el jefe de {Employee}[?]|

## <a name="query-endpoint-when-patterns-are-used"></a>Consulta de punto de conexión cuando se usan patrones

Ahora que los patrones se han agregado a la aplicación, entrenar, publique y consulte la aplicación en el punto de conexión en tiempo de ejecución de predicción.

1. Vuelva a entrenar y publicar la aplicación.

1. Cambie las pestañas de explorador de vuelta a la pestaña de la dirección URL del punto de conexión.

1. Vaya al final de la dirección URL en la dirección y escriba `Who is the boss of Jill Jones?` como la expresión. El último parámetro de la cadena de consulta es `q`, la expresión **query**. 

    ```json
    {
        "query": "who is the boss of jill jones?",
        "topScoringIntent": {
            "intent": "OrgChart-Manager",
            "score": 0.9999989
        },
        "intents": [
            {
                "intent": "OrgChart-Manager",
                "score": 0.9999989
            },
            {
                "intent": "OrgChart-Reports",
                "score": 7.616303E-05
            },
            {
                "intent": "EmployeeFeedback",
                "score": 7.84204349E-06
            },
            {
                "intent": "GetJobInformation",
                "score": 1.20674213E-06
            },
            {
                "intent": "MoveEmployee",
                "score": 7.91245157E-07
            },
            {
                "intent": "None",
                "score": 3.875E-09
            },
            {
                "intent": "Utilities.StartOver",
                "score": 1.49E-09
            },
            {
                "intent": "Utilities.Confirm",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Help",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Stop",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Cancel",
                "score": 1.225E-09
            },
            {
                "intent": "FindForm",
                "score": 1.123077E-09
            },
            {
                "intent": "ApplyForJob",
                "score": 5.625E-10
            }
        ],
        "entities": [
            {
                "entity": "jill jones",
                "type": "Employee",
                "startIndex": 19,
                "endIndex": 28,
                "resolution": {
                    "values": [
                        "Employee-45612"
                    ]
                },
                "role": ""
            },
            {
                "entity": "boss of jill jones",
                "type": "builtin.keyPhrase",
                "startIndex": 11,
                "endIndex": 28
            }
        ]
    }
    ```

La predicción de la intención ahora es bastante más confiable.

## <a name="working-with-optional-text-and-prebuilt-entities"></a>Trabajo con texto opcional y entidades creadas previamente

Las expresiones de plantilla del patrón anterior de este tutorial tenían algunos ejemplos de texto opcional, como el uso posesivo de la letra s, `'s` y el uso del signo de interrogación, `?`. Supongamos que necesita permitir fechas actuales y futuras en el texto de la expresión.

Las expresiones de ejemplo son:

|Intención|Expresiones de ejemplo con texto opcional y entidades creadas previamente|
|:--|:--|
|OrgChart-Manager (Organigrama-Administrador)|`Who was Jill Jones manager on March 3?`|
|OrgChart-Manager (Organigrama-Administrador)|`Who is Jill Jones manager now?`|
|OrgChart-Manager (Organigrama-Administrador)|`Who will be Jill Jones manager in a month?`|
|Organigrama-Administrador|`Who will be Jill Jones manager on March 3?`|

Cada uno de estos ejemplos usa un tiempo verbal, `was`, `is`, `will be`, así como una fecha, `March 3`, `now` y `in a month`, que LUIS debe predecir correctamente. Observe que los dos últimos ejemplos usan casi el mismo texto, excepto para `in` y `on`.

Ejemplo de declaraciones de plantilla que permiten esta información opcional: 

|Intención|Expresiones de ejemplo con texto opcional y entidades creadas previamente|
|:--|:--|
|OrgChart-Manager (Organigrama-Administrador)|`who was {Employee}['s] manager [[on]{datetimeV2}?`]|
|OrgChart-Manager (Organigrama-Administrador)|`who is {Employee}['s] manager [[on]{datetimeV2}?]`|


El uso de la sintaxis opcional de corchetes, `[]`, facilita la adición de este texto opcional a la expresión de plantilla y se pueden anidar hasta un segundo nivel, `[[]]` e incluir entidades o texto.


**Pregunta: ¿Por qué están todas las letras `w`, la primera letra de cada expresión de plantilla, en minúsculas? ¿No deberían estar en mayúsculas o minúsculas opcionalmente?** La expresión enviada al punto de conexión de consulta por la aplicación cliente se convierte en minúsculas. La expresión de plantilla puede estar en mayúsculas o minúsculas y la expresión del punto de conexión también puede estar en cualquiera de las dos. La comparación siempre se realiza después de la conversión a minúsculas.

**Pregunta: ¿Por qué el número creado previamente no forma parte de la expresión de plantilla si el 3 de marzo se predice como número `3` y fecha `March 3`?** La expresión de plantilla contextualmente usa una fecha, ya sea de modo literal, como en `March 3` o abstracto, como en `in a month`. Una fecha puede contener un número, pero un número es posible que no necesariamente se considere como una fecha. Use siempre la entidad que mejor representa el tipo en el que desea que se devuelvan los resultados JSON de la predicción.  

**Pregunta: ¿Qué sucede con las expresiones de composición incorrecta como `Who will {Employee}['s] manager be on March 3?`?** Los tiempos verbales diferentes gramaticalmente, como este en el que `will` y `be` son independientes, deben ser una nueva expresión de plantilla. La expresión de plantilla existente no producirá coincidencia. Aunque no ha cambiado la intención de la expresión, ha cambiado la colocación de las palabras en la expresión. Este cambio afecta a la predicción de LUIS. También puede usar la sintaxis [group y OR](#use-the-or-operator-and-groups) con los tiempos verbales para combinar estas expresiones. 

**Recuerde: primero se encuentran las entidades y, a continuación, se compara el patrón.**

## <a name="edit-the-existing-pattern-template-utterance"></a>Edición de la expresión de plantilla del patrón existente

1. En el sitio web de LUIS, seleccione **Build** (Crear) en el menú superior y, a continuación, seleccione **Patterns** (Patrones) en el menú izquierdo. 

1. Busque la expresión de plantilla existente, `Who is {Employee}['s] manager[?]`, seleccione los puntos suspensivos (***...*** ) a la derecha y, luego, seleccione **Edit** (Editar). 

1. Cambie la expresión de plantilla a: `who is {Employee}['s] manager [[on]{datetimeV2}?]`

## <a name="add-new-pattern-template-utterances"></a>Adición de nuevas expresiones de plantilla del patrón

1. Mientras sigue en la sección **Patterns** (Patrones) de **Build** (Crear), agregue varias nuevas expresiones de plantilla del patrón. Seleccione **OrgChart-Manager** (Organigrama-Administrador) en el menú desplegable Intent (Intención) y escriba cada una de las siguientes expresiones de plantilla:

    |Intención|Expresiones de ejemplo con texto opcional y entidades creadas previamente|
    |--|--|
    |OrgChart-Manager (Organigrama-Administrador)|`who was {Employee}['s] manager [[on]{datetimeV2}?]`|
    |OrgChart-Manager (Organigrama-Administrador)|`who will be {Employee}['s] manager [[in]{datetimeV2}?]`|
    |OrgChart-Manager (Organigrama-Administrador)|`who will be {Employee}['s] manager [[on]{datetimeV2}?]`|

2. Entrene la aplicación.

3. Seleccione **Test** (Prueba) en la parte superior del panel para abrir el panel de pruebas. 

4. Especifique varias expresiones de prueba para comprobar que el patrón coincide y la puntuación de la intención es considerablemente alta. 

    Después de escribir la primera expresión, seleccione **Inspect** (Inspeccionar) en el resultado para ver todos los resultados de la predicción. Cada expresión debe tener la intención **OrgChart Manager** y debe extraer los valores de las entidades de empleado y datetimeV2.

    |Expresión|
    |--|
    |Who will be Jill Jones manager (¿Quién será el administrador de Jill Jones?)|
    |who will be jill jones's manager (¿Quién será el administrador de Jill Jones?)|
    |Who will be Jill Jones's manager? (¿Quién será el administrador de Jill Jones?)|
    |who will be Jill jones manager on March 3 (¿Quién será el administrador de Jill Jones el 3 de marzo?)|
    |Who will be Jill Jones manager next Month (¿Quién será el administrador de Jill Jones el próximo mes?)|
    |Who will be Jill Jones manager in a month? (¿Quién será el administrador de Jill Jones dentro de un mes?)|

Todas estas expresiones encuentran las entidades, por lo tanto coinciden con el mismo patrón y tienen una puntuación de predicción alta.

## <a name="use-the-or-operator-and-groups"></a>Uso del operador OR y los grupos

Varias de las expresiones de plantilla anteriores son muy parecidas. Use la sintaxis **group** `()` y **OR** `|` para reducir las expresiones de plantilla. 

Los dos siguientes patrones se pueden combinar en un único patrón mediante la sintaxis group `()` y OR `|`.

|Intención|Expresiones de ejemplo con texto opcional y entidades creadas previamente|
|--|--|
|OrgChart-Manager (Organigrama-Administrador)|`who will be {Employee}['s] manager [[in]{datetimeV2}?]`|
|OrgChart-Manager (Organigrama-Administrador)|`who will be {Employee}['s] manager [[on]{datetimeV2}?]`|

La nueva expresión de plantilla será: 

`who ( was | is | will be ) {Employee}['s] manager [([in]|[on]){datetimeV2}?]`. 

Esta expresión usa un elemento **group** alrededor del tiempo verbal necesario y los elementos opcionales `in` y `on` con una barra vertical **or** entre ellos. 

1. En la página **Patterns** (Patrones), seleccione el filtro **OrgChart Manager**. Para limitar la lista, busque `manager`. 

    ![Búsqueda del término "manager" en los patrones de intención de OrgChart-Manager](./media/luis-tutorial-pattern/search-patterns.png)

1. Guarde una versión de la expresión de plantilla (para editarla en el paso siguiente) y elimine las otras variaciones. 

1. Cambie la expresión de plantilla a: 

    `who ( was | is | will be ) {Employee}['s] manager [([in]|[on]){datetimeV2}?]`.

1. Entrene la aplicación.

1. Use el panel de prueba para probar las versiones de la expresión:

    |Expresiones para escribir en el panel de prueba|
    |--|
    |`Who is Jill Jones manager this month`|
    |`Who is Jill Jones manager on July 5th`|
    |`Who was Jill Jones manager last month`|
    |`Who was Jill Jones manager on July 5th`|    
    |`Who will be Jill Jones manager in a month`|
    |`Who will be Jill Jones manager on July 5th`|


## <a name="use-the-utterance-beginning-and-ending-anchors"></a>Uso de los delimitadores inicial y final de la expresión

La sintaxis del patrón proporciona la sintaxis de delimitador de inicio y final de un símbolo de intercalación, `^`. Los delimitadores de inicio y final de expresión se pueden usar juntos para dirigirse a expresiones muy específicas o posiblemente literales, o se pueden usar por separado para dirigirse a intenciones. 

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se agregan dos intenciones de expresiones que fueron difíciles de predecir con alta precisión sin tener muchas expresiones de ejemplo. La incorporación de patrones a ellas ha permitido a LUIS predecir mejor la intención con una puntuación considerablemente más alta. El marcado de entidades y texto irrelevante ha permitido a LUIS aplicar el patrón a una serie más amplia de expresiones.

> [!div class="nextstepaction"]
> [Información sobre cómo usar roles con un patrón](luis-tutorial-pattern-roles.md)
